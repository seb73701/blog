---
title: Vault, vos secrets vous remercieront
description: 
published: true
date: 2023-04-21T10:50:48.844Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:50:44.293Z
---

# Vault, vos secrets vous remercieront

Cet article a été ecrit en collaboration avec Tanguy LECLERC, Vault étant le sujet autour duquel notre stage a gravité.

Introduction
La question des identifiants, des clés API et des mots de passe est un enjeu majeur lors du développement d’une application, d'un service ou d'infrastructure. En effet, les développeurs et les pipelines CI/CD ont besoin de ces moyens d’accès afin de pouvoir délivrer leur travail.

Malheureusement, ce sujet est parfois traité avec “les moyens du bord”, en les stockant en variable d’environnement ou bien en les renseignant manuellement. Ces solutions sont, d’un point de vue sécurité, des failles permettant à des personnes malveillantes d’en profiter pour ainsi récupérer des secrets.

Nous nous sommes donc posé la question : Comment peut-on obtenir une gestion des secrets centralisée, avec un accès granulaire, garantissant une sécurité maximale, et ce le plus simplement possible ?

En cherchant, nous nous sommes rendu compte qu’une solution sur le marché répondait à nos besoins : Hashicorp Vault. La solution semble avoir grandi en maturité et en adoption durant ces dernières années, si vous n'avez encore rien lu à son sujet, vous avez bien fait d'arriver ici.

Vault est un logiciel développé par Hashicorp, la maison mère de Terraform.
Disponible en version open-source et Enterprise, Vault possède une documentation très bien fournie ainsi qu’une section Learn permettant de se familiariser avec l’outil.

Vault, c’est quoi ?
Vault est un outil conçu dans le but de permettre l’accès de manière sécurisée aux secrets d’un projet et de les transmettre au sein d’une organisation. Un secret peut désigner une API key, des clefs d’accès ou de chiffrement, n’importe quel objet sur lequel on veut garder un contrôle.

Le Vault permet d’accéder à ces secrets à travers une API, reliée à un stockage en backend (ici Consul) tout en ayant un contrôle, un accès à des logs détaillés et la capacité de contrôler les agissements des différents membres de l’organisation : une traçabilité.

Un des avantages de Vault est le fait qu’il est intégrable à la fois sur tout cloud privé et public. Ainsi nous ne sommes pas dans l’obligation de nous limiter à un seul fournisseur de service et pouvons donc nous adapter à notre cas d’usage.

Dans cet article, nous nous intéresserons notamment à la partie open source que propose la solution en essayant d'aller plus loin que le simple POC.

Dans tout ça, où sont stockés ces secrets ? C’est ce que nous allons voir maintenant avec Consul notre backend.

Consul, c’est quoi ?
Également développé par Hashicorp, Consul est un système de gestion d’applications distribuées sur de multiples plateformes et providers cloud. Il permet le stockage de secrets via Vault, autorise le monitoring et gère les configurations des différentes applications qu’il orchestre.

Le choix de Consul s’est fait car il présente certains avantages utiles dans notre cas d’utilisation. En effet il intègre directement le service discovery couplé à des health checks.

Ainsi nos différents serveurs Vault et Consul peuvent se gérer automatiquement et éviter de diriger les requêtes vers des serveurs malades, rendant le système résilient face à une quelconque panne.

De plus, Consul lorsqu’il est utilisé avec Vault permet le chiffrement de toutes les données en transit via TLS grâce à un certificat présent sur chaque instance du réseau.

Fonctionnalités disponibles

Vault fonctionne par module, chacun permettant de gérer des aspects différents tels que les types de secrets, d’authentification, de stockage ainsi que les journaux.
Nous allons ici traiter de ces 4 aspects avec des exemples concrets.

Gestion des secrets

Vault propose une multitude de secrets engines (une quinzaine), composants qui stockent, génèrent ou chiffrent des données.

Ils permettent une grande flexibilité s’adaptant à beaucoup d' use-cases différents. Certains vont servir simplement à stocker et lire des données (key-value store, redis), d’autres vont interagir avec des services externes tel AWS ou GCP afin de générer des credentials dynamique tandis que d’autres proposent de l’encryption as a service ou de la génération de certificats.

Afin de pouvoir utiliser ces secrets engines, il faut les activer via un chemin dans le vault. Lorsqu’une requête sera effectuée, le routeur va automatiquement les diriger vers le bon service.

La mise en place d’un secrets engines est très simple comme on peut le voir à travers la commande suivante :

```
$ vault secrets enable -version=1 kv
```

kv est un moteur de secrets (secrets engine) permettant un stockage clé valeur des secrets.

On peut ensuite écrire un secret dans cet engine, de la façon suivante :

```
$ vault kv put kv/my-secret my-value=s3cr3t

Success! Data written to: kv/my-secret
```

Afin de pouvoir utiliser ces différents secrets engines, il faut leur associer une policy qui va définir les actions réalisables par l’utilisateur.

Pour cela, on précise dans un fichier JSON ou HCL le path du secret ainsi que les capabilities que l’on accorde à ce dernier comme on peut le voir sur la capture d’écran suivante :
```
path "secret/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}
```

Ces policies sont personnalisables avec un contrôle fin sur les permissions, comme démontré ici dans la documentation. On peut également noter que c’est le principe du least access qui fait foi, permettant de renforcer encore plus la sécurité.

De plus, les policies ne s’appliquent pas seulement aux secrets engines, mais également aux commandes de Vault, et aux méthodes d’authentification. On peut donc personnaliser le Vault afin de limiter au maximum les opérations réalisables en son sein.

Un aspect important concerne le TTL (time to live), qui correspond à la durée pendant laquelle le token d’accès généré par Vault sera valide.

Ainsi, si ce token fuite, il ne sera utilisable que durant le temps de sa création et son moment d’expiration, qui se fait automatiquement. C’est pour cela qu’il est important de spécifier à chaque fois un TTL suffisamment bas.

Pour révoquer un secret manuellement un simple appel API par un utilisateur avec les bons droits suffira.

Gestion de l’authentification
Stocker des secrets, c’est bien, mais y accéder c’est mieux !  Pour cela, Vault propose de nombreuses méthodes d’authentification. Ces dernières sont responsables de l’attribution de l’identité et des policies liées aux utilisateurs.

Les méthodes d’authentification sont à définir selon l’utilisation qui sera faite de Vault. Par exemple via des token JWT pour l’utilisation dans des pipelines ou bien user/password pour un accès via CLI.

La mise en place d’une nouvelle méthode d’authentification se réalise selon des étapes similaires à la mise en place du secret engine KV, comme précisé par la suite.

On va dans un premier temps enable la méthode (ici user/password) :

```
$ vault auth enable userpass
```

Pour ensuite écrire la configuration avec la policy associée  :
```
$ vault write auth/userpass/users/mitchellh \
	password=foo \
    policies=admins
```

Une fois ces étapes réalisées, un utilisateur disposant de l’username (ici mitchellh) et du password (ici foo) pourra se connecter au Vault et récupérer les secrets dont il a l’accès, selon la policy admins.

Stockage
Vault supporte un peu moins d’une vingtaine de solutions de stockage tel que S3, Consul ou DynamoDB par exemple. Peu importe le choix que vous faites, Vault chiffrera automatiquement les données.

Seuls 3 modules de stockage sont supportés directement par Hashicorp : Consul, filsesystem, in-memory (non recommandé pour la production). Il faut aussi prendre en compte que seuls certains choix de stockage peuvent être mis en production en High Availability.

Audit
Le module d’audit de Vault est assez simple, il peut être configuré pour écrire les logs dans un fichier, dans les syslogs de la machine ou sur un socket. Il est conseillé d’avoir plus d’un service de log activé car si il n’y en a qu’un et qu’il est bloqué, le Vault ne traitera aucune requête tant qu’il ne peut écrire ses logs.

$ vault audit enable file file_path=/var/log/vault_audit.log
Ces logs créent un fichier au format JSON où sera inscrit toutes les requêtes reçues par le Vault ainsi que les actions prises par ce dernier.

Il n’y a pas d’options de rotations de logs natives a Vault, il faudra configurer cela vous même. Si vous utilisez le système de fichier, il est nécessaire d’envoyer un signal “hang up” au service vault après chaque rotation de logs.

Pour ce qui est de Consul tout est natif, dans la configuration on peut choisir le temps de rotation de logs, leur taille maximum, le nombre de fichiers que l’on garde, du niveau de logs que l’on veut (entre erreurs, warnings etc..), tout est configurable dans le fichier de configuration.

```
enable_syslog = true
log_level = "WARN"
log_file = "/consul/log/"
log_rotate_duration = "2h"
log_rotate_max_files = -1
```

Ces logs sont les logs du service consul, on y voit les warnings et erreurs qu’il peut se produire entre les serveurs et les clients.

Exemples d’utilisation de Vault
Module Gitlab d’authentification (JWT)
Nous avons décidé de créer un Vault pour un usage précis : ne pas garder de credentials AWS dans notre pipeline Gitlab. Afin de permettre à notre pipeline de s’identifier auprès de Vault, nous utilisons une authentification JWT (JSON Web Token).

Afin de mettre en place cette authentification JWT, nous avons réalisé 4 étapes.

La première étape consiste à activer JWT, grâce à la commande suivante :
```
$ vault auth enable jwt
```

Ensuite, on définit la policy qui va nous permettre de définir quels sont les droits de lecture/ecriture/update sur les différents moteurs de secrets. Sachant que nous allons utiliser le moteur aws, nous pouvons lui accorder les droits de lecture et de création.
```
$ vault policy write myproject - <<EOF
path "aws/*" {
  capabilities = [ "read", "create" ]
}
EOF
```

On peut maintenant passer à la création de rôle on retrouve plusieurs paramètres clés :
```
 $ vault write auth/jwt/role/myproject - <<EOF
     {
     "role_type": "jwt",
     "policies": ["myproject"],
     "token_explicit_max_ttl": 60,
     "user_claim": "user_email",
     "bound_claims": {
     "project_id": "3064",
     "ref": "master",
     "ref_type": "branch"
     }
     }
     EOF
```
role_type -> le type d’authentification souhaité
policies -> la policy associé
token_explicit_max_tll -> la durée de validité du token renvoyé par Vault, ici égale à 60 secondes, permettant de réduire le risque de compromission du vault
bound_claims -> une entité où l’on peut préciser une branche, un id de projet ou un tag, ce qui nous permet de granuler encore plus l’accès
Finalement, on peut finir la configuration JWT comme indiqué sur la capture suivante.
```
$ vault write auth/jwt/config \
    jwks_url="https://gitlab.ippon.fr/-/jwks" \
    bound_issuer="gitlab.ippon.fr"
```
bound_issuer -> le domaine dont provient la requête
jwks_url -> l’endpoint utilisé pour s’authentifier au Vault via le projet
Ainsi lorsque notre pipeline Gitlab CI va effectuer une requête à notre serveur Vault, il va être identifié via l’url envoyant la requête accompagné des différents paramètres précisés lors de la configuration. Il sera ainsi autorisé à accéder aux secrets définis dans la policy précédemment créée.
Dans notre cas d’usage, on va donc pouvoir récuperer des acces key et secret key temporaires afin de permettre à notre pipeline de pouvoir s’authentifier auprès d’AWS et ainsi réaliser

Module AWS de création de secrets
Dans notre cas d’usage, nous avons utilisé le service AWS qui nous permet de générer des clés d’accès, liées à un rôle IAM.

Il existe différentes méthodes d’authentification :

STS AssumeRole
STS Federation Token
IAM User
Pour notre use-case, nous avons utilisé le STS Federation Token, car les autres méthodes ne nous permettent pas de générer un session token, utile lors du déploiement via Terraform.

Concernant la configuration, il faut dans un premier temps renseigner nos credentials AWS à Vault afin que ce dernier puisse générer des credentials IAM
```
$ vault write aws/config/root \
    access_key=AKIAJWVN5Z4FOFT7NLNA \
    secret_key=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 \
    region=us-east-1
```
Il faut par la suite créer une policy AWS, qui nous octroie les droits IAM nécessaires selon notre use-case.
```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": [
      "ec2:*",
      "sts:GetFederationToken"
    ],
    "Resource": "*"
  }
}
```
Ensuite, nous créons un rôle en précisant le nom du rôle, le type de credentials que nous souhaitons ainsi que la policy AWS créée précédemment. A noter qu’il est également possible de préciser un ARN de policy à la place d’un policy document.
```
$ vault write aws/roles/ec2_admin \
    credential_type=federation_token \
    policy_document=@policy.json
```
Finalement, on peut générer les credentials et les lire en utilisant la commande suivante, et utiliser ces access_key, secret_access_key et security_token pour n’importe quelle de nos applications qui en a besoin. On précise également un TTL pour des questions de sécurité
```
$ vault write aws/sts/ec2_admin ttl=60m
Key             Value
lease_id        aws/sts/ec2_admin/31d771a6-fb39-f46b-fdc5-945109106422
lease_duration  60m0s
lease_renewable false
access_key      ASIAJYYYY2AA5K4WIXXX
secret_key      HSs0DYYYYYY9W81DXtI0K7X84H+OVZXK5BXXXX
security_token  EwasAKwQyZUtZaCjVNDiXXXXXXXXgUgBBVUUbSyujLjsw6jYzboO...
```

Architecture de notre solution
Networking

<img src="https://lh3.googleusercontent.com/LvifrgIY0JlqzL6RzHosT5QP-m3mGRfstfmKJelchdmfQlTbxY74SmxrarM5y6pfgpOG5D9IKuv2SK4Rq6XBFqyZVKqyengMcDHwYIYULOjCShGyAJgn0ZqS4CbugQVnXOK0NyIP">

L’architecture réseau de notre solution est très importante pour garantir son intégrité. En effet nos serveurs consul ne doivent pas être accessible depuis internet étant donné la criticité des données qui y sont stockées. De plus, nous devons balancer le trafic entre plusieurs zones de disponibilités (infrastructure physique géographiquement éloignées) pour garantir son bon fonctionnement au cas où une des zones tombe.

C’est pour cela que nous avons choisi de configurer notre architecture avec :

1 VPC avec son internet gateway
3 subnets publics pouvant accueillir chacun un serveur Vault ainsi qu’une NAT gateway
3 subnets privés accueillant chacun un serveur consul
Vault
Dans le déploiement d’un Vault en HA, il est important de définir un Vault principal tandis que les autres seront en StandBy. Dans notre cas, nous avons choisi d’utiliser 2 Vault pouvant monter à 3 si nécessaire.

Le Vault principal est celui qui traitera toutes les requêtes, celles allant sur le Vault en standby seront redirigées vers le principal. Ce comportement est différent dans la version entreprise ou les Vault en StandBy peuvent prendre en charge les opérations de lecture.

Les serveurs Vault ont besoin d’un client Consul pour communiquer avec les serveurs Consul. Il est donc important d’ouvrir les bons ports entre les security groups.

<img src="https://lh5.googleusercontent.com/nd55IpWjgFFICeWz2WcBrq8-1UdT89cl1RIp182hn5gWo-rWYzqkDcJm0M9hRO9N-4xy3EZpFDZj5Yo8mX58bqsn3ALBjop-BJpfYwxVb7EmOBBRFYuTb5FDlcAmjw8ut_r3d1nr">

En plus de ces ports, Vault a besoin des ports :

8200 le port de l’API pour recevoir les requêtes, ouvert depuis l’ELB et utilisant du TLS pour augmenter la sécurité.
8201 ouvert entre les serveurs Vault pour communiquer entre eux
Consul
Les serveurs consuls sont eux dans des subnet privés derrière un ELB et un Auto Scaling Group, contrairement aux serveurs Vault, chaque serveur est actif.

Les ports à ouvrir sont ceux que l'on peut voir sur la figure n° , il est important de les ouvrir aussi avec le security group Vault afin de communiquer avec les clients Consul.

Audit et sauvegarde
Logs Vault
Pour les logs Vault nous avons choisis la solution fichier comme vu précédemment. Pour la rotation des logs, Vault dispose d’un système intégré que l’on paramètre comme montré ci-dessous.

On effectue ensuite un cron job qui va récupérer les logs de la journée, préciser la date de création puis les synchroniser avec un bucket S3.

<img src="https://lh3.googleusercontent.com/Sp7PJxeT3LQgsHHf6wGERiy9EUU6Eji7SgnHAbuhJZpyXoiP0fzc5Zl7x3NXfq5Rn8_386FmEsLsB-K-qDVd4vvx-KFGZxCHmXvdaZbwO7G_8S-dO_phBmaoGRDusAIde5J3e-Fm">

Logs Consul
Pour les logs Consul, nous avons choisis une rotation de 2 heures puis un cronjob s’effectue et viens ajouter l’adresse IP du serveur consul devant le fichier puis synchronise le tout avec un autre bucket S3.

Sauvegarde
Notre stack, malgré son architecture haute disponibilité, n’est pas à l’abri d’avoir une défaillance grave. Etant donné que les secrets sont des données sensibles et essentielles, il est important d’avoir une sauvegarde qui s’effectue régulièrement pour se protéger.
Pour cela, Consul propose d’effectuer des snapshots qui enregistrent toutes les données qui y sont stockées. Afin de restaurer ce snapshot, nous devrons utiliser le certificat d'autorité généré lors du lancement de la stack, sans ça il nous est impossible de retrouver les données (qui sont évidemment toutes chiffrées).
On effectue quotidiennement un snapshot grâce à un cron job qui est par la suite envoyé sur un bucket S3 chiffré.

Pour conclure
Vault est une technologie permettant la gestion de secrets et la création de secrets dynamiques, et liée à Consul elle permet un déploiement HA sécurisé, résilient et simplifié. Avec cet article nous avons montré comment mettre en place une stack AWS permettant la création de token utilisateurs AWS pour la sécurisation d’une pipeline.

Grâce à ce Vault, nous pouvons donc récupérer des tokens AWS nouvellement créer à travers notre pipeline, ce qui permet de ne pas laisser de clés en variable d’environnement.

Ce n’est qu’une partie de ce que peut faire Vault et nous vous invitons à penser à Vault lorsque vous rencontrez la gestion de secrets dans vos projets, il y a sûrement une solution pour votre cas d’usage comme vous pouvez le voir sur le schéma ci-dessous.


source = https://blog.ippon.fr/2021/06/22/vault-vos-secrets-vous-remercieront/