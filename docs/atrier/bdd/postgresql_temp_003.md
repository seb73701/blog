---
title: DevSecOps - Sécurisez, Auditez, Automatisez vos bases de données PostgreSQL
description: 
published: true
date: 2023-06-17T06:46:27.338Z
tags: 
editor: markdown
dateCreated: 2023-06-10T07:10:54.261Z
---

# DevSecOps - Sécurisez, Auditez, Automatisez vos bases de données PostgreSQL

Malgré le “boom” autour des technologies de la data, les bases de données relationnelles restent la technologie la plus répandue dans les systèmes informatiques des entreprises. Passé le cap de l’hégémonie d’Oracle, **PostgreSQL**, en tant que base de données relationnelle open source, s’est imposée sur la dernière décennie.

Grâce à “l’infrastructure As Code”, le déploiement des bases de données PostgreSQL est devenu simple, automatisable et intégrable dans les pipelines CI/CD. Toutefois, d’après mon expérience, c’est à partir de ce moment que les vrais problèmes commencent, tout du moins, du point de vue de la sécurité.

Avant de rentrer dans le dur, quelques mots sur la démarche. J’ai récemment publié sur la terraform registry un module de mon cru : [jparnaudeau/database-admin](https://registry.terraform.io/modules/jparnaudeau/database-admin/postgresql/latest).
Vous pourrez retrouver l’intégralité du code parcouru dans ces articles en consultant l’exemple [full-rds-example](https://registry.terraform.io/modules/jparnaudeau/database-admin/postgresql/latest/examples/full-rds-example) disponible dans le module.

Ce 1er article couvre l’aspect théorique de la sécurité autour d'une base PostgreSQL :  

- L'encryption at Rest
- L'encryption at Transit
- La gestion des Secrets
- La gestion des rôles et des permissions
- Le principe du "Least Privilege"


Les 3 articles suivants décriront pas à pas l’application de ces principes :

- Comment créer proprement les **rôles**, les **permissions** et les **users**,
- Comment appliquer le principe de **Least Privilege**,
- Comment déployer un **système d’auditabilité**, parfois nécessaire pour les entreprises exerçant dans des activités soumises à des réglementations (domaine médical par exemple) ou à des contraintes liées à la régulation des marchés (domaine financier ou assurance).

A titre de démonstration, on couplera ce système d'auditabilité avec un outil de “SOC” (Security Operation Center), en l’occurrence [Elasticsearch](https://www.elastic.co/fr/) dans notre cas. J’ai choisi le cloud provider AWS pour me fournir l'infrastructure et une instance de base. Néanmoins, le module fournit un “docker-compose” qui vous permettra de réaliser vos tests directement depuis une image PostgreSQL. Le module est indépendant de l’infrastructure sous-jacente.

Effectuons un tour d’horizon de “ce qu’il faudrait faire” en terme de sécurité.

## Encryption at Rest

On commencera par sécuriser le stockage des données. Cela peut se faire de différentes manières :

- en chiffrant le système de fichiers sur lequel les données de la base seront conservées. Ce sera au niveau de l’OS qu’il faudra gérer la clé privée vous permettant de chiffrer/déchiffrer.
- en utilisant les fonctionnalités de [chiffrement](https://www.postgresql.org/docs/current/encryption-options.html) (disponibles au moins depuis la version 8) offertes par le moteur PostgreSQL.
- en utilisant une solution tiers comme [CipherTrust Data System Security](https://cpl.thalesgroup.com/fr/encryption/database-security/postgresql-database-encryption) de chez Thales.

## Encryption at Transit
On continuera par sécuriser l’accès à la base de données PostgreSQL, en établissant une connexion sécurisée TLS. Quitte à exécuter des requêtes pour récupérer des données sensibles, autant que celles-ci soient faites à travers une connexion chiffrée. Le moteur PostgreSQL supporte bon nombre de mécanisme avancé pour l'authentification des clients (cf doc officielle [ici](https://www.postgresql.org/docs/14/auth-methods.html)).

Si vous souhaitez approfondir ces aspects, vous trouverez de nombreux articles sur Internet. Celui-ci est une très bonne synthèse :

https://www.cybertec-postgresql.com/en/setting-up-ssl-authentication-for-postgresql/

Maintenant qu’on a sécurisé “la maison”, établi un protocole permettant de gérer les identités des personnes pouvant y entrer, reste à gérer proprement les permissions de ces personnes.



## Principes de Sécurité à appliquer pour la gestion des utilisateurs
Ces principes sont expliqués dans l’article suivant :

https://aws.amazon.com/blogs/database/managing-postgresql-users-and-roles/

Illustrons cela avec un schéma :

<img src="https://blog.ippon.fr/content/images/2022/02/Diagram-Relations.png">

Lors de l’installation d’un serveur PostgreSQL, un utilisateur `postgres` est créé par défaut. Il est possible de lui donner un autre nom. Quoiqu’il en soit, c’est un `super-user`, avec les droits les plus élevés. Cet utilisateur sera conservé par l’équipe ops et ne doit pas être connu des équipes devops affectées aux développements des produits/applications.

La première étape est de créer un **rôle “admin”** qui possédera des droits élevés d’administration d’une instance de base. Cet utilisateur sera utilisé dans les tâches d’administration et sera en charge de :

- créer la database PostgreSQL (“create database”). Il sera owner de la base.
- créer éventuellement des schémas (si vous ne souhaitez pas utiliser le schéma “public” par défaut).
- créer tous les objets type tables, fonctions, procédures que vous définirez.
- créer des rôles qui serviront dans les différents composants de votre application.

Le schéma montre la création de 2 rôles pour une application :

- un rôle “write” avec l’attribution des permissions permettant de faire des select/insert/update/delete sur les tables de la database.
- un rôle “read” permettant uniquement les “select” sur les tables de la database.


Notez également que j’emploie le mot “rôle” et pas “user”. Dans le modèle PostgreSQL, les users sont des rôles avec la propriété `login = true`. Les rôles peuvent hériter d’autres rôles, récupérant les permissions associées à chaque rôle. Même s’il existe la commande `create user`, il s’agit d’un alias pour la création d’un rôle. Utilisez `\du+` pour lister l’ensemble de vos rôles et users de votre base.

Pour terminer, on crée 2 users héritant l’un du rôle “write”, l’autre du rôle “read”. Ces 2 users seront utilisés dans le cadre d’un composant, par exemple, en charge de faire du reporting de la base, ne nécessitant que les droits “read”, et l’autre pour un composant backend nécessitant des droits d’écriture sur les tables.

> Remarques :
> Certaines applications (par exemple confluence d’Atlassian) génèrent à la volée des schémas et des tables. Il faudra donc leur donner des droits équivalents à celui de l’utilisateur admin, mais sans le droit de création de rôle. Selon vos besoins, vous pourrez rencontrer différents cas :
>
> - le cas où une application a besoin d’accéder à 2 databases différentes (mais créées au sein de la même instance PostgreSQL). Dans ce cas, créer des rôles pour chaque database et créer un user qui héritera des rôles. Par exemple, le rôle “write” de la database1 et le rôle “read” de la database2.
> - De même, vous pourrez rencontrer le cas où 2 applications différentes auront besoin d’accéder à la même database. Dans ce cas, un rôle par application vous permettra de cloisonner ces applications à leur périmètre respectif.
Pour ce qui concerne le "troubleshooting", l’analyse post-mortem, ou tout simplement une mise en production nécessitant des opérations effectuées manuellement, il est acceptable de créer des users avec les permissions nécessaires et suffisantes (mais pas plus). Par contre, ces users utilisés par des “humains” devront avoir une date d’expiration.

## Least Privilege

A l’instar de ce qui se fait dans la gestion des permissions dans le cloud, les rôles que vous définissez devraient respecter le principe du “moindre accès”. Par exemple, si votre database contient 4 tables, vous pouvez créer un rôle ayant la permission “write” sur 2 tables, la permission “read” sur une 3eme table. Sans déclaration explicite, ce rôle ne permettra ni le read ni le write sur la 4eme table.

Donner les permissions “write” sur toutes les tables devrait être **l’exception et pas la règle**.

Chaque composant d’une application devrait avoir un rôle dédié avec des permissions minimales et suffisantes. Ainsi, si une faille de sécurité était découverte par un attaquant, le fait de minimiser les permissions permet de contenir la surface d’attaque.

Avoir un module permettant de simplifier la gestion des permissions permettra aux devops / équipe en charge des bases, d’assurer plus facilement l’application du “least privilege”, ou tout du moins n’auront plus d’excuses pour ne pas le mettre en œuvre !!

A noter : pour permettre une gestion fine des droits sur les tables dans le schéma public, il sera nécessaire de révoquer les privilèges par défaut. Vous trouverez une explication détaillée dans le Readme du module, ici. Quoiqu’il en soit, le module révoque par défaut ces privilèges, vous pouvez donc utiliser le schéma public sans crainte.

## Gestion des Secrets

La gestion des secrets pourrait faire l’objet d’un article en soi. Énonçons rapidement les avantages des “coffres-forts numériques”, tels que **Vault** d’Hashicorp, **AWS SecretsManager** pour AWS ou **Azure Key Vault** pour Azure :

- d’abord une seule source de vérité : c’est dans le coffre-fort que sont stockés les mots de passe, les clés ssh etc … Cela permet d’homogénéiser votre SI, d’éviter la duplication des mots de passe, gagner en cohérence.
- Avec l’API que propose le coffre-fort, il sera possible de récupérer la valeur du mot de passe en référençant son emplacement dans le coffre-fort. Cela évite d’avoir à mettre les mots de passe en clair dans les fichiers de configuration de vos composants techniques ou de les stocker de manière chiffrée dans votre outil de gestion de configuration. C’est parfaitement intégré dans la gestion de la configuration de vos clusters kubernetes, ECS, etc... Pour les stacks Java plus traditionnelles, il existe également des “wrappers” au-dessus des drivers JDBC permettant de récupérer la valeur du mot de passe à la volée. Il existe l’équivalent pour le domaine .Net.
- Il est possible de mettre en œuvre la rotation des mots de passe. Autant il est simple, côté coffre-fort, d’effectuer cette rotation, autant il est plus compliqué de propager une modification de mot de passe au sein des backends sans provoquer d’interruption de services. Néanmoins, notons qu’à partir du moment où la connexion est établie, le changement de mot de passe n’aura pas d’influence tant que la connexion persiste.

Quelques pointeurs si vous souhaitez approfondir le sujet :

- https://medium.com/4th-coffee/on-devops-11-secret-management-an-introduction-to-secret-manager-and-best-practice-da74c6f03aa7
- https://github.com/aws/aws-secretsmanager-jdbc
- https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets_strategies.html
- https://nimblegecko.com/how-to-securely-store-and-retrieve-passwords-in-dot-net-core-apps-with-azure-key-vault/

Quelques mots sur la gestion des secrets dans Terraform : C’est un peu le talon d’achille de cet outil. C’est assez frustrant mais quelque soit la façon dont vous décidez de passer un mot de passe à une ressource, ces secrets se retrouveront toujours en clair dans le tfstate de votre composant terraform !

Il s'agit d'une [issue](https://github.com/hashicorp/terraform/issues/516) ouverte depuis plus de 6 ans maintenant, sans qu'aucune vraie solution ne soit prévue. Pour mitiger cette problématique, vous devrez penser à :

- Stocker le “terraform state” dans un backend qui prend en charge le chiffrement. Terraform supporte bon nombre de backends faisant cela, tels que S3, GCS et Azure Blob Storage.
- Contrôler strictement qui peut accéder à votre backend Terraform avec une politique stricte de permissions. Par exemple, limiter uniquement à votre serveur CI que vous utilisez pour le déploiement en production).

Si vous souhaitez approfondir le sujet, je vous conseille cet excellent article de chez gruntwork :  
https://blog.gruntwork.io/a-comprehensive-guide-to-managing-secrets-in-your-terraform-code-1d586955ace1

Avant de clore ce chapitre, revenons à notre problématique initiale : le module terraform crée des users en spécifiant leur mot de passe. Il faut donc générer un mot de passe aléatoire, l’affecter au user et le stocker dans un endroit sécurisé.

Dans la conception du module, n’ayant pas d’a priori sur l’endroit où le mot de passe doit être stocké, un système de **post-processing playbook** est disponible. Ce playbook exécute un script (python, shell, ..) que vous devrez écrire. Ce script devra faire 3 choses : premièrement, générer un mot de passe aléatoire, deuxièmement mettre à jour le mot de passe du user dans la base PostgreSQL et enfin, stocker le mot de passe dans un endroit sécurisé.

Cela a 2 avantages :

- d’une part, la génération du mot de passe dans un script permet de ne pas le rendre visible dans le tfstate.
- d’autre part, le stockage du mot de passe peut se faire dans n’importe quel endroit puisque c’est vous qui scriptez l’endroit où vous voulez les mettre.

---

Pour des raisons de simplicité, le code construit, dans un seul composant terraform, l’ensemble des ressources nécessaires pour la démonstration. De même, la base de données et l’instance Elasticsearch seront exposées publiquement mais avec un accès restreint. Dans la réalité, vous décomposerez votre architecture en plusieurs composants (le VPC, la base RDS, la configuration de votre base PostgreSQL, l’outil de SOC) et vous déploierez vos bases dans un réseau privé. Cela étant dit, nous pouvons commencer.

Afin d’illustrer tous ces principes de sécurité, prenons le cas d’une “fake application” de gestion d’un panier, implémentée avec le modèle de données suivant :

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/02/FakeApplication.png">

L’application possédera 3 composants :

- un composant **web** permettant la gestion des clients et de leurs paniers,
- un composant **backoffice** permettant de mettre à jour les produits en vente sur le site,
- et enfin, un **batch** permettant de mettre à jour une table de statistiques permettant de comptabiliser le montant total vendu par produit. Le traitement sera exécuté par une invraisemblable procédure stockée !

En terme de permissions “nécessaires et suffisantes”, cela donne le tableau suivant :

|Composant|CUSTOMER|BASKET|PRODUCT|STATS|
|:--------|:-------|:-----|:------|:----|
|Web|Write Operations|Write Operations|Read Operations|N/A|
|BackOffice|Read Operations|Read Operations|Write Operations|Read Operations|
|Batch(Stats)|Read Operations|Read Operations|Read Operations|Write Operations|

Vous pouvez retrouver l’ensemble du code source [ici](https://github.com/jparnaudeau/terraform-postgresql-database-admin/tree/master/examples/full-rds-example).

Notez que j’utilise l’excellente librairie `direnv` pour positionner un certain nombre de variables d’environnement. Voici mon fichier `.envrc` :

```sh
export PGPASSWORD="My-Strong-Password"
export AWS_PROFILE=ippon-sandbox
export TF_VAR_rds_root_password="${PGPASSWORD}"
```

- `PGPASSWORD` : variable d’environnement native de PostgreSQL pour récupérer le mot de passe. C’est comme cela que le provider PostgreSQL fonctionne.
- `AWS_PROFILE` : Le profil AWS contenant les éléments d’authentification de votre compte AWS.
- `TF_VAR_rds_root_password` : valorisation de la variable terraform `rds_root_password`

## Construction de notre “playground”
Comme dit en préambule, on va construire une infrastructure sur AWS. Pour cela, on doit construire en premier lieu un **VPC**. Vous pouvez retrouver le code dans le fichier `vpc.tf`.

Notez qu’en plus du vpc, on déclare un `security_group` nous permettant de joindre la base de données sur son adresse IP publique, port `5432`, et qui limite l’accès à notre adresse IP de sortie.

## Construction de l’instance RDS
Continuons par déployer notre base RDS, en utilisant les modules officiels de la registry terraform :

```yaml
######################################
# Deploy RDS Instance
######################################
module "rds" {
   source  = "terraform-aws-modules/rds/aws"
   version = "3.5.0"

   identifier = local.name

   engine                = "postgres"
   engine_version        = var.rds_engine_version
   family                = var.rds_family
   major_engine_version  = var.rds_major_engine_version
   instance_class        = var.rds_instance_class

   allocated_storage     = var.rds_allocated_storage
   max_allocated_storage = var.rds_max_allocated_storage
   storage_encrypted     = var.rds_storage_encrypted

   name     = var.inputs["db_name"]
   username = var.rds_superuser_name
   password = var.rds_root_password
   port     = 5432

   multi_az               = true
   # because we want reach the database from our local workstation, 
   # we need to deploy our RDS in the public subnets
   # DO NOT DO THAT IN PRODUCTION
   # to reduce the attack surface, limit the access of the RDS Instance 
   # to our personal IP addresses
   publicly_accessible    = true
   subnet_ids             = module.vpc.public_subnets
   vpc_security_group_ids = [module.security_group.security_group_id]

   maintenance_window              = "Mon:00:00-Mon:03:00"
   backup_window                   = "03:00-06:00"
   enabled_cloudwatch_logs_exports = ["postgresql", "upgrade"]

   backup_retention_period = 0
   skip_final_snapshot     = true
   deletion_protection     = false

   create_db_parameter_group = false
   parameter_group_name      = aws_db_parameter_group.postgres.id

   create_db_option_group = false
   create_db_subnet_group = false
   db_subnet_group_name   = aws_db_subnet_group.main_db_subnet_group.id

   tags = local.tags
}
```

le fichier `terraform.tfvars` nous fournit les paramètres :

```yaml
# rds settings
rds_name                  = "myfullrdsexample"
rds_engine_version        = "13.5"
rds_major_engine_version  = "13"
rds_family                = "postgres13"
rds_instance_class        = "db.t3.micro"
rds_allocated_storage     = 10
rds_max_allocated_storage = 20
allowed_ip_addresses      = ["X.X.X.X/32"]
rds_superuser_name        = "root"
```

Notez qu’on renomme le superuser en `root` et non `postgres` (valeur par défaut dans le moteur PostgreSQL).


## Construction de la database PostgreSQL
Quelques mots sur 3 attributs relatifs à la ressource terraform `aws_db_instance` :

```yaml
identifier = var.rds_name
name       = var.inputs["db_name"]
username   = var.rds_superuser_name
```

- `identifier` : c’est le nom du moteur postgresql. C’est une enveloppe représentant l’installation d’un serveur postgresql. Il faut le voir comme le nom de la machine sur laquelle est installée le serveur postgresql. Ce n’est pas l’objet “database” au sens postgresql. C’est ce que vous verrez dans le dashboard de synthèse du service RDS dans la console AWS.
- `name` :  c’est le nom de la database au sens postgresql que l’api va nous créer. C’est un attribut optionnel. Si vous ne le spécifiez pas, c’est la database `postgres` qui sera créée.
- `username` : Il s'agit du libellé de l’utilisateur “super-user” associée à votre instance RDS. Si vous ne le spécifiez pas, ce sera le user `postgres`.

Dans notre démonstration, lorsque terraform va nous déployer notre instance RDS, nous aurons donc :

- une instance RDS `myfullrdsexample`
- une database `mydatabase` à l’intérieur de cette instance
- un user `root` super-user relatif à toutes les databases déployées dans l’instance RDS.


## Construction de notre user “admin”
Appelons notre module pour générer les rôles et permissions associées à notre futur utilisateur `admin` :

```yaml
########################################
# Initialize the database and the objects
# (roles & grants), the default privileges
########################################
module "initdb" {
   source  = "jparnaudeau/database-admin/postgresql//create-database"
   version = "2.0.0"
   
   depends_on = [module.rds]
   
   # set the provider
   providers = {
      postgresql = postgresql.pgadm
   }
   
   # targetted rds
   pgadmin_user = var.rds_superuser_name
   dbhost       = module.rds.db_instance_address
   dbport       = var.dbport
   
   # input parameters for creating database & objects inside database
   create_database = false
   inputs          = var.inputs
   
   # because the superuser is not "postgres", need to set it in the module
   default_superusers_list = [var.rds_superuser_name]
}
```

Remarque : on passe l’attribut `create_database` à `false` car l’API RDS nous a créé la database pour nous.

Spécifions ensuite ceci dans le fichier `terraform.tfvars` :

```yaml
# database and objects creation
inputs = {
   # parameters used for creating database
   db_schema_name = "public"
   db_name        = "mydatabase" # should be the same as var.rds_name. if not, a new database will be created
   db_admin       = "app_admin_role" # owner of the database
   
   # install extensions if needed
   extensions = ["pgaudit"]

   # CREATE ROLES
   db_roles = [
      { id = "admin", role = "app_admin_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE", "CREATE"], createrole = true },
],

   #  GRANT PERMISSIONS ON ROLES
   db_grants = [
   # define grants for app_admin_role :
   # - access to all objects on database
      { object_type = "database", privileges = ["CREATE", "CONNECT", "TEMPORARY"], objects = [], role = "app_admin_role", owner_role = "root", grant_option = true },
      { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_admin_role", owner_role = "root", grant_option = true },
   ],

   # CREATE USERS
   db_users = [
      { name = "admin", inherit = true, login = true, membership = ["app_admin_role"], validity = "infinity", connection_limit = -1, createrole = true },
   ]
}

# Refresh or not refresh passwords
refresh_passwords = ["all"]
```

Quelques mots sur la structure d’un `grant` :

- `object_type` : la liste des objets supportés par le provider est disponible [ici](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/postgresql_grant#object_type).
- `privileges` : la liste des privilèges est disponible [ici](https://registry.terraform.io/providers/cyrilgdn/postgresql/latest/docs/resources/postgresql_grant#privileges).
- `objects` : la liste éventuelle des objets sur lesquels le grant s’applique. Liste vide signifie `all`.
- `role` : le rôle sur lequel on souhaite ajouter les permissions.
- `owner_role` : le rôle à partir duquel le grant est exécuté. Le rôle spécifié doit avoir les permissions suffisantes pour réaliser le grant en question. Ici, parce qu’on est en train de créer l’utilisateur “admin” (il n’existe donc pas encore), on utilise notre super-user “root”.
- `grant_option` : à `true`, cela signifie qu’on délègue, au rôle qu’on est en train de créer, le droit d’affecter ses privilèges à d’autres “rôles”.


La création des users se fait en utilisant le second sous-module `create-users`:

```yaml
#########################################
# Create the users inside the database
#########################################
# AWS Region
data "aws_region" "current" {}


module "create_users" {
   source  = "jparnaudeau/database-admin/postgresql//create-users"
   version = "2.0.0"

   # need that all objects, managed inside the module "initdb", are created
   depends_on = [module.initdb]

   # set the provider
   providers = {
      postgresql = postgresql.pgadm
   }

   # targetted rds
   pgadmin_user = var.rds_superuser_name
   dbhost       = module.rds.db_instance_address
   dbport       = var.dbport
   
   # input parameters for creating users inside database
   db_users = var.inputs["db_users"]

   # set passwords
   passwords = { for user in var.inputs["db_users"] : user.name => random_password.passwords[user.name].result }

   # set postprocessing playbook
   postprocessing_playbook_params = {
      enable  = true
      db_name = var.inputs["db_name"]
      extra_envs = {
         REGION   = data.aws_region.current.name
         RDS_NAME = var.rds_name
      }
      refresh_passwords = var.refresh_passwords
      shell_name        = "./gen-password-in-secretsmanager.py"
   }
}
```

Concernant l’attribut passwords, nous passons des `random_password`. On pourrait penser que c’est dangereux car le password correspondant sera stocké en clair dans le `remote_state` ? Oui mais non.

Ici, on exécute le script `gen-password-in-secretsmanager.py`. Celui-ci va générer un mot de passe aléatoire, écraser le mot de passe de l’utilisateur créé en base et le stocker dans AWS SecretsManager. Le mot de passe contenu dans le `remote_state` n’est donc plus l’actuel mot de passe de l’utilisateur.

Le système de **postprocessing_playbook** injecte automatiquement des variables d’environnement accessibles dans le script :

- Les variables d’environnement natives PostgreSQL : `PGHOST`, `PGPORT`, `PGUSER`, `PGDATABASE`.
- Une variable d’environnement `DBUSER` représentant le user pour lequel on souhaite lui affecter un nouveau mot de passe.
- Toutes les variables que vous aurez définies dans `extra_envs`.

Concernant **la rotation des mots de passe**  :

- en laissant `refresh_passwords = ["all"]`, cela indique au post-processing playbook de s’exécuter pour tous les utilisateurs définis. Cela signifie qu’à chaque fois que l’apply est exécuté, les mots de passe des utilisateurs seront redéfinis.
- Pour suspendre la rotation des mots de passe, vous pouvez mettre `refresh_passwords = [""]`.
- Si vous souhaitez rafraîchir uniquement le mot de passe d'un user, par exemple  “redfang”, il suffira alors de mettre `refresh_passwords = ["redfang"]`.

Pour compléter le tout, notons que pour simplifier l’écriture du script, nous créons via terraform des entrées dans **AWS SecretsManager**. Là aussi, le password qui est utilisé lors de la création de ces secrets n’a pas d’importance. Il sera écrasé lors de l’exécution du post-processing plakbook. Cela évite de gérer dans le playbook le cas où le secret n’existe pas.

A l’apply, si tout se passe bien, on dispose donc d’un utilisateur `admin` ayant tous les droits sur la database `mydatabase` et dont le mot de passe est récupérable depuis AWS SecretsManager.

```yaml
Apply complete! Resources: 44 added, 0 changed, 0 destroyed.
Outputs:
affected_schema = "public"
created_roles = [
   "app_admin_role",
]
db_users = {
   "admin" = {
      "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -W"
      "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-admin-PFxvvF"
      "secret_name" = "secret-kv-myfullrdsexample-admin"
   }
}
```

Tentons de nous connecter :

```
psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -W
Password: <find password in AWS SecretsManager in secret-kv-myfullrdsexample-admin>
psql (13.5 (Ubuntu 13.5-2.pgdg20.04+1))
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.
mydatabase=> \l
List of databases
Name    |  Owner   | Encoding |   Collate   |    Ctype    |     Access privileges
------------+----------+----------+-------------+-------------+----------------------------
mydatabase | root     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =Tc/root                   +
           |          |          |             |             | root=CTc/root              +
           |          |          |             |             | app_admin_role=C*T*c*/root +
postgres   | root     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | 
rdsadmin   | rdsadmin | UTF8     | en_US.UTF-8 | en_US.UTF-8 | rdsadmin=CTc/rdsadmin      +
           |          |          |             |             | rdstopmgr=Tc/rdsadmin            
template0  | rdsadmin | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/rdsadmin                +
           |          |          |             |             | rdsadmin=CTc/rdsadmin
template1  | root     | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/root                    +
           |          |          |             |             |root=CTc/root
(5 rows)
```

On vient de terminer les étapes de configuration de notre user `admin`. A partir de maintenant, ce sera ce user qui sera utilisé pour les tâches quotidiennes de maintenance de notre base.

---

Continuons en créant le modèle de données de notre "fake application", ainsi que la procédure stockée.

```sql
psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -f create-tables.sql -W
Password: <find password in AWS SecretsManager in secret-kv-myfullrdsexample-admin>
CREATE TABLE
INSERT 0 1
CREATE TABLE
CREATE INDEX
CREATE TABLE
CREATE TABLE


psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -f create-procedure-statistiques.sql -W
CREATE PROCEDURE

psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -f create-tables.sql -W
Password:

mydatabase=> \dt
         List of relations
 Schema |   Name   | Type  | Owner
--------+----------+-------+-------
 public | basket   | table | admin
 public | customer | table | admin
 public | product  | table | admin
 public | stats    | table | admin
(4 rows)
```

Pour créer des rôles ayant des permissions sur des objets en base, il est nécessaire que ces objets existent. C’est pour cela, qu’on crée d’abord le user “admin”, ensuite le modèle de données et enfin les différents users qui seront utilisés dans les composants de la “fake application”.


## Construction de nos users applicatifs avec limitation de leurs permissions
Complétons notre fichier `terraform.tfvars` :

La partie “role” :

```yaml
#  CREATE ROLES
   db_roles = [
      { id = "admin", role = "app_admin_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE", "CREATE"], createrole = true },
      { id = "readonly", role = "app_readonly_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE"], createrole = false },
      { id = "web", role = "app_writeweb_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE"], createrole = false },
      { id = "backoffice", role = "app_writebo_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE"], createrole = false },
      { id = "batch", role = "app_writebatch_role", inherit = true, login = false, validity = "infinity", privileges = ["USAGE"], createrole = false },
   ],
```

Appliquons les permissions strictement nécessaires pour chacun de ces rôles :

```yaml
  db_grants = [
   # define grants for app_admin_role :
   # - access to all objects on database
       { object_type = "database", privileges = ["CREATE", "CONNECT", "TEMPORARY"], objects = [], role = "app_admin_role", owner_role = "root", grant_option = true },
       { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_admin_role", owner_role = "root", grant_option = true },

   # define grants for app_readonly_role
   # - access to 'SELECT' on all tables
   # - access to 'SELECT' on all sequences
       { object_type = "database", privileges = ["CONNECT"], objects = [], role = "app_readonly_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_readonly_role", owner_role = "app_admin_role", grant_option = true },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER"], objects = [], role = "app_readonly_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "sequence", privileges = ["SELECT", "USAGE"], objects = [], role = "app_readonly_role", owner_role = "app_admin_role", grant_option = false },

   # define grants for app_writeweb_role
   # - access in Read/Write on tables "customer" & "basket"
   # - access in Read on table "Product"
       { object_type = "database", privileges = ["CONNECT"], objects = [], role = "app_writeweb_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_writeweb_role", owner_role = "app_admin_role", grant_option = true },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER", "INSERT", "UPDATE", "DELETE"], objects = ["customer", "basket"], role = "app_writeweb_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER"], objects = ["product"], role = "app_writeweb_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "sequence", privileges = ["SELECT", "USAGE"], objects = [], role = "app_writeweb_role", owner_role = "app_admin_role", grant_option = false },

   # define grants for app_writebo_role
   # - access in Read/Write on table "product"
   # - access in Read on table "customer" , "basket" & "stats"
       { object_type = "database", privileges = ["CONNECT"], objects = [], role = "app_writebo_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_writebo_role", owner_role = "app_admin_role", grant_option = true },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER", "INSERT", "UPDATE", "DELETE"], objects = ["product"], role = "app_writebo_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER"], objects = ["customer", "basket", "stats"], role = "app_writebo_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "sequence", privileges = ["SELECT", "USAGE"], objects = [], role = "app_writebo_role", owner_role = "app_admin_role", grant_option = false },

   # define grants for app_writebatch_role
   # - access in Read/Write on table "stats"
   # - access in Read on table "customer", "basket", "product"
   # - execute functions
       { object_type = "database", privileges = ["CONNECT"], objects = [], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "type", privileges = ["USAGE"], objects = [], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = true },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER", "INSERT", "UPDATE", "DELETE"], objects = ["stats"], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "table", privileges = ["SELECT", "REFERENCES", "TRIGGER"], objects = ["customer", "basket", "product"], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "sequence", privileges = ["SELECT", "USAGE"], objects = [], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = false },
       { object_type = "function", privileges = ["EXECUTE"], objects = [], role = "app_writebatch_role", owner_role = "app_admin_role", grant_option = false },

  ],
```

Notez que pour tous ces rôles, l’attribut `owner_role` a la valeur `app_admin_role` (et non le super-user) et que l’attribut `grant_option` vaut `false`.

Enfin, créons les users correspondant à chacun de ces rôles :

```yaml
   db_users = [
      { name = "admin", inherit = true, login = true, membership = ["app_admin_role"], validity = "infinity", connection_limit = -1, createrole = true },
      { name = "web", inherit = true, login = true, membership = ["app_writeweb_role"], validity = "infinity", connection_limit = -1, createrole = false },
      { name = "backoffice", inherit = true, login = true, membership = ["app_writebo_role"], validity = "infinity", connection_limit = -1, createrole = false },
      { name = "batch", inherit = true, login = true, membership = ["app_writebatch_role"], validity = "infinity", connection_limit = -1, createrole = false },
      { name = "lemmy", inherit = true, login = true, membership = ["app_readonly_role"], validity = "2022-07-31 00:00:00+00", connection_limit = -1, createrole = false },
   ]
```

L’attribut `createrole = false` empêche l’utilisateur de créer un rôle.

Un utilisateur `lemmy`, ayant les droits “read” sur toutes les tables, a été créé avec une date d’expiration.


Et on a terminé !! Après l’apply, voici une partie des outputs :

```yaml
affected_schema = "public"
created_roles = [
   "app_admin_role",
   "app_readonly_role",
   "app_writeweb_role",
   "app_writebo_role",
   "app_writebatch_role",
]
db_users = {
   "admin" = {
         "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U admin -d mydatabase -W"
         "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-admin-PFxvvF"
         "secret_name" = "secret-kv-myfullrdsexample-admin"
   }
   "backoffice" = {
         "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U backoffice -d mydatabase -W"
         "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-backoffice-cvPvLW"
         "secret_name" = "secret-kv-myfullrdsexample-backoffice"
   }
   "batch" = {
         "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U batch -d mydatabase -W"
         "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-batch-jrtS2V"
         "secret_name" = "secret-kv-myfullrdsexample-batch"
   }
   "lemmy" = {
         "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U lemmy -d mydatabase -W"
         "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-lemmy-g0ARmI"
         "secret_name" = "secret-kv-myfullrdsexample-lemmy"
   }
   "web" = {
            "connect_command" = "psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U web -d mydatabase -W"
            "secret_arn" = "arn:aws:secretsmanager:eu-west-3:444444444444:secret:secret-kv-myfullrdsexample-web-PvmPYu"
            "secret_name" = "secret-kv-myfullrdsexample-web"
   }
}
```

Voici le résultat des tests :

- user “backoffice” :

|Operation|Customer|Basket|Product|Stats|
|:--------|:-------|:-----|:------|:----|
|Read|OK|OK|OK|OK|
|Write|Permission Denied|Permission Denied|OK|Permission Denied|


- user “web” :

|Operation|Customer|Basket|Product|Stats|
|:--------|:-------|:-----|:------|:----|
|Read|OK|OK|OK|Permission Denied|
|Write|OK|OK|Permission Denied|Permission Denied|


- user “batch” :

|Operation|Customer|Basket|Product|Stats|
|:--------|:-------|:-----|:------|:----|
|Read|OK|OK|OK|OK|
|Write|Permission Denied|Permission Denied|Permission Denied|OK|

C’est donc exactement ce que l’on souhaitait.

Testons quelques instructions SQL d’élévation de privilèges depuis le user `web`:

```sql
psql -h myfullrdsexample.pandemonium.eu-west-3.rds.amazonaws.com -p 5432 -U web -d mydatabase -W
Password:
psql (13.5 (Ubuntu 13.5-2.pgdg20.04+1))
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
mydatabase=> CREATE DATABASE exfiltration;
ERROR:  permission denied to create database

mydatabase=> CREATE SCHEMA exfiltration;
ERROR:  permission denied for database mydatabase

mydatabase=> CALL feed_stats(1);
ERROR:  permission denied for table stats
CONTEXT:  SQL statement "insert into stats (product,value) values (infos.ProductLabel,infos.totalAmount)"
PL/pgSQL function feed_stats(integer) line 18 at SQL statement

mydatabase=> CREATE ROLE dream SUPERUSER LOGIN PASSWORD 'theater';
ERROR:  must be superuser to create superusers

mydatabase=> CREATE ROLE suicidal CREATEDB LOGIN PASSWORD 'tendencies';
ERROR:  permission denied to create role

mydatabase=> CREATE ROLE marilyn LOGIN PASSWORD 'manson';
ERROR:  permission denied to create role

mydatabase=> CREATE ROLE slayer;
ERROR:  permission denied to create role
```

Ce qui est plutôt rassurant.

## Conclusion
Dans cet exemple, si j’avais dû gérer manuellement la création des rôles, des permissions et des users, cela m’aurait pris un certain temps pour adapter les scripts SQL que j’ai amoncelés durant mes différentes expériences. D’autre part, j'aurais dû reporter les mots de passe dans AWS SecretsManager.

Enfin, ici j’ai créé 5 users. Si je devais le faire pour 20, voire plus, comme c’est possible dans certaines organisations, cela prendrait encore plus de temps.

Si on doit rajouter la gestion d’un process pour supprimer un user, ou invalider son accès, ou mettre à jour son mot de passe, ou étendre sa date d’expiration, c’est encore d’autres scripts qu’il faut maintenir.

Avec ce module, j’ai maintenant une méthode simple permettant d’appliquer tous les aspects de sécurité présentés dans le 1er article. Ce module s’inscrit parfaitement dans la mouvance “devops” en apportant aux développeurs ou aux ops, un moyen de sécuriser leurs bases de données.

Enfin, le système de post-processing playbook mis en œuvre permet d’adapter ce module à chaque contexte client, et par la même occasion, de stocker à un seul endroit le mot de passe, dans un endroit sécurisé de son choix. Bien que l’utilisation des `null_resource` de Terraform soit à proscrire, en écrivant des scripts shell ou python, vous ne prendrez pas un grand risque dans l’interopérabilité de l’exécution du code Terraform.

---

Dans les 3 précédents articles, nous avons pu voir :

- article #1 : La théorie relative à la sécurisation des bases de données PostgreSQL
- article #2 : L'application de ces concepts sur un exemple concret avec construction de notre base dans le cloud et création d'un user admin
- article #3 : Le déploiement des rôles, permissions, users pour répondre aux besoins fonctionnels de notre application, ainsi que la gestion et la rotation des mots de passe.

Aujourd'hui, attachons nous à décrire une fonctionnalité, pas toujours nécessaire, l'auditabilité de vos bases de données.

## Fonction avancée : SOC et Auditabilité
Quelques mots concernant la mise en œuvre d’un système d’auditabilité dans un moteur PostgreSQL. Cette fonctionnalité est apportée par l’installation d’une **pg_extension** : `pgaudit`.

En spécifiant dans le fichier postgresql.conf, la directive `log_statement = all`, cela vous permettra de logguer une partie des requêtes exécutées sur la base. Cela est acceptable pour la surveillance et d'autres utilisations mais ne fournit pas le niveau de détail généralement requis pour un audit, notamment de pouvoir suivre les requêtes exécutées dans les fonctions ou les procédures stockées. `pgAudit` répond à cette problématique.

Mon module prend en charge l’installation des pg_extensions, tout du moins sur une base déployée sur AWS. Je n’ai malheureusement pas eu le loisir de le tester sur un moteur de base de données installé manuellement.

A noter qu’il est possible de déployer une pg_extension a posteriori, une fois une instance de base de données existante. Il faudra faire un arrêt-relance de la base pour que celle-ci soit active.

Dans le cadre d’une instance RDS, AWS a packagé la possibilité d’installer différentes pg_extensions. Pour obtenir la liste, exécuter la requête suivante :

```sql
select * from pg_available_extensions order by name;
```

J’en cite 2 que j’ai pu mettre en oeuvre :

- `pg_stat_statements` pour la mise en œuvre de statistiques sur les tables.
- `aws_s3` pour pouvoir faire des dumps de vos données directement sur s3.

### Installation & Configuration
Pour activer notre extension `pgaudit`, dans le fichier `terraform.tfvars` :

```ini
# install extensions if needed
extensions = ["pgaudit"]
```

Pour que l’extension fonctionne, nous devons passer un certain nombre de paramètres. Cela est fait par l’intermédiaire de la ressource `aws_db_parameter_group`.

Ce “groupe de paramètres” rassemble 2 types de paramètres que vous pouvez appliquer sur une instance RDS : les paramètres de type `immediate` et ceux de type `pending_reboot`.

Les paramètres liés aux pg_extensions sont du second type. Le fichier `terraform.tfvars` fournit :

```yaml
parameter_group_params = {
   immediate = {
      autovacuum         = 1
      client_encoding    = "utf8"
      log_connections    = "1"
      log_disconnections = "1"
      log_statement      = "all"
   }
   pending-reboot = {
      shared_preload_libraries     = "pgaudit",
      track_activity_query_size    = "2048",
      "pgaudit.log"                = "ALL",
      "pgaudit.log_level"          = "info",
      "pgaudit.log_statement_once" = "1"
   }
}
```

Se référer à https://github.com/pgaudit/pgaudit#settings pour le détail de la configuration. Le détail de la procédure est disponible dans la documentation officielle AWS ici :

https://aws.amazon.com/premiumsupport/knowledge-center/rds-postgresql-pgaudit/?nc1=h_ls

Les paramètres `pending_reboot`, comme leur type l’indique, ont besoin d’un arrêt relance de la base afin qu’ils soient pris en compte. Dans notre cas, comme il s’agit de la création, ceux-ci seront pris en compte au démarrage de la base.


Lorsque la base RDS a été déployée, l’extension est active et fonctionne. On peut déjà ausculter les logs qui sont envoyés. D’abord depuis la console RDS : Onglet `Logs & Events`, trier les fichiers par la colonne `Last Written`, sélectionner le dernier log et cliquer sur le bouton `watch`.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/02/RDSWatchLogs.PNG">

Vous pouvez également le faire directement depuis l’API RDS, comme le montre le script suivant :

```shell
#!/bin/bash
export INSTANCE_IDENTIFIER=`terraform output|grep db_instance_id|awk -F '=' '{print $2}'|sed 's/^ *//g'|sed 's/"//g'`

LOGFILE=$(aws rds describe-db-log-files --db-instance-identifier ${INSTANCE_IDENTIFIER} --query 'DescribeDBLogFiles[-1].[LogFileName]' --output text)

echo "LOGFILE = $LOGFILE"

aws rds download-db-log-file-portion \
--db-instance-identifier ${INSTANCE_IDENTIFIER} \
--starting-token 0 \
--log-file-name "${LOGFILE}" \
--output text | grep AUDIT
```

Le `grep AUDIT` tronque les requêtes qui comportent plusieurs lignes.

### Déploiement Elasticsearch & Indexation des logs
Pour simuler un outil de SOC, qui nous permettra d’indexer nos logs, revenons sur le fichier `terraform.tfvars` et passons la variable `create_elasticsearch` à `true` :

```
################################################
# Elasticsearch
################################################
create_elasticsearch = true
es_instance_type     = "t3.small.elasticsearch"
es_instance_count    = 1
es_ebs_volume_size   = 10
```

Comme pour l’instance RDS, le domaine elasticSearch est créé de manière à ce qu’il soit publiquement exposé. Afin de limiter l’exposition, une `domain policy` est appliquée dans laquelle on restreint les adresses IP source à notre adresse IP de sortie personnelle.

Pour créer le cluster, on utilise le module de la registry écrit par “cloudposse” :

```yaml
########################################
# Retrieve infos on AWS STS Caller
########################################
data "aws_caller_identity" "current" {}

###########################################
# Deploy an Elasticsearch Cluster
###########################################
module "elasticsearch" {
   source  = "cloudposse/elasticsearch/aws"
   version = "0.35.0"

   # create or not all related resources inside the module
   enabled = var.create_elasticsearch

   # naming
   namespace = "soc"
   stage     = var.environment
   name      = "es"

   # config
   vpc_enabled                    = false
   zone_awareness_enabled         = false
   elasticsearch_version          = "7.4"
   instance_type                  = var.es_instance_type
   instance_count                 = var.es_instance_count
   ebs_volume_size                = var.es_ebs_volume_size
   iam_role_arns                  = [aws_iam_role.lambda-role.arn]
   iam_actions                    = ["es:ESHttpGet", "es:ESHttpPut", "es:ESHttpPost"]
   encrypt_at_rest_enabled        = "true"
   kibana_subdomain_name          = "kibana-soc"
   create_iam_service_linked_role = false
   allowed_cidr_blocks            = var.allowed_ip_addresses
   advanced_options = {
      "rest.action.multi.allow_explicit_index" = "true"
   }
}
```


Une fois que le domaine elasticSearch est déployé, il ne reste plus qu’à souscrire le cloudwatch log group contenant les streams des logs de la base RDS (dont ceux générés par pgaudit) sur l’endpoint de l’elasticsearch.

Il est possible de le faire depuis la console. Suivez ce blog pour le faire “proprement” :

https://aws.amazon.com/blogs/database/analyze-postgresql-logs-with-amazon-elasticsearch-service/

Pour le faire de manière programmatique, j’ai publié un autre module sur la registry qui encapsule les ressources nécessaires :

```yaml
###########################################
# Deploy a subscription filter on RDS CloudWatch Logs
# to stream logs on an Elasticsearch domain endpoint
###########################################
module "stream2es" {
   source  = "jparnaudeau/cloudwatch-subscription-elasticsearch/aws"
   version = "1.0.0"

   for_each = var.create_elasticsearch ? toset(["1"]) : []

   # global variables
   region      = var.region
   environment = var.environment
   tags        = local.tags

   # other variables
   function_name           = local.lambda_function_name
   rds_name                = var.rds_name
   rds_cloudwatch_log_name = format("/aws/rds/instance/%s/postgresql",var.rds_name)
   es_domain_endpoint      = try(module.elasticsearch.domain_endpoint, "")
   source_account_id       = data.aws_caller_identity.current.account_id
   lambda_role_arn         = aws_iam_role.lambda-role.arn
}
```

Ce module est en charge de :

- déployer une fonction lambda qui va streamer les logs d’un cloudwatch log group. Ici, en l'occurrence, les logs de la base RDS. Le code de la lambda est le code source officiel fourni par AWS. 2 modifications sont apportées : d’une part, le passage de l’endpoint du domaine Elasticsearch en tant que variable d’environnement de la lambda, au lieu de le “hardcoder” comme cela est fait quand vous créez la souscription depuis la console, et d’autre part, le fait de suivre les bonnes pratiques en ayant un index elasticSearch par jour.
- déployer la souscription de la fonction lambda sur le cloudwatch log group. Un `filter pattern` est appliqué : `[date, time, misc, message]`.

Une fois l’apply terminé, vous devriez pouvoir afficher l’instance kibana (fourni dans les outputs) et voir en quasi temps réel les logs de la base. Si on génère un peu d’activité, vous devriez voir quelque chose comme ceci :

![elk1.png](/assets/img/bdd/tempo/elk1.png)

On peut filtrer les logs d’audit pour sortir toutes les requêtes exécutées sur la table customer par exemple : `message:*AUDIT* and message:*customer*` :

![kibana2.jpg](/assets/img/bdd/tempo/kibana2.jpg)

On peut également filtrer les logs d’audit pour sortir toutes les requêtes exécutées par un user donné :  `message:*AUDIT* and misc:*batch*`    

![kibana3.jpg](/assets/img/bdd/tempo/kibana3.jpg)

Notez que l’on voit toutes les requêtes exécutées, même celles exécutées à travers la procédure stockée, ce qui n’est pas le cas avec le système de logging par défaut.

## Conclusion  
A travers les fonctionnalités couvertes par mon module terraform, on a vu qu’il est possible facilement de mettre en œuvre une vraie gestion des users dans les bases PostgreSQL. Avec les primitives offertes par le provider PostgreSQL, on a démontré dans cette suite d’articles comment mettre en œuvre les bonnes pratiques de sécurité, voir même comment déployer un système d’auditabilité.

Bien évidemment, il reste encore du travail. D’abord, sur la rétention des logs dans votre outil de SOC. D’autre part, c’est une réponse technique à une problématique. Mais il y a un coût derrière. Mais l’un ne va pas sans l’autre généralement. Notez également que l’activation de l’extension `pgAudit` génère des logs en plus, qui doivent être pris en compte lorsque vous dimensionnerez la taille du stockage de votre base. L’aspect performance pouvant être également un aspect qui influencera le niveau de verbosité souhaité.

L’ensemble des éléments décrits dans cet article sont disponibles dans l’exemple “full-rds-exampe” dont je vous redonne le lien :

https://registry.terraform.io/modules/jparnaudeau/database-admin/postgresql/latest/examples/full-rds-example

La partie la plus “pénible” du travail consiste à écrire exhaustivement la liste des rôles et des permissions. Vous pouvez néanmoins vous appuyer sur la structure présente dans les fichiers terraform.tfvars de l’ exemple.

N’hésitez pas à me contacter jparnaudeau@ippon.fr si vous souhaitez obtenir plus d’informations.

N’hésitez pas non plus à créer des issues sur mon repo github si vous voyez des améliorations ou des bugs à corriger.

Pour ceux qui seraient intéressés par la fonctionnalité de mapping des users sur des rôles IAM dans AWS, je vous redirige vers l'excellent article de mon collègue Mathieu Regnard [ici](https://blog.ippon.fr/2022/01/05/autoriser-des-acces-temporaires-a-une-base-de-donnees-rds/).

Source : https://blog.ippon.fr/
