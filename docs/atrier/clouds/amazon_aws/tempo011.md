---
title: Amazon MSK : sécuriser l’accès à l’API Kafka
description: 
published: true
date: 2023-06-11T12:15:51.133Z
tags: 
editor: markdown
dateCreated: 2023-06-11T12:15:51.133Z
---

# Amazon MSK : sécuriser l’accès à l’API Kafka

![blog_msk_picture.jpg](/assets/img/clouds/tempo/blog_msk_picture.jpg)

Amazon Managed Streaming for Apache Kafka (MSK) est un service managé proposé par AWS permettant à ses clients de déployer un cluster Kafka sans avoir de connaissances poussées dans l’administration de cette technologie. Apache Kafka est un projet open source qui facilite la mise en place d’applications de streaming de données en temps réel. C’est un composant qu’on retrouve dans de nombreuses architectures aujourd’hui.

Amazon MSK gère la configuration, le déploiement ainsi que la maintenance continue des clusters Apache Kafka. Cela permet facilement d’avoir une architecture hautement disponible et résiliente car les agents sont répartis dans plusieurs zones de disponibilité (datacenters séparés de plusieurs kilomètres).

MSK s’intègre également avec d’autres produits AWS, notamment :

- Cloudwatch pour les métriques
- Cloudwatch Logs, S3 et Kinesis Firehose pour les logs
- Lambda pour la consommation/production de messages

Plusieurs types d’utilisateurs et d’applications doivent être en mesure d’interagir avec le cluster :

- Des applications qui produisent des messages dans des topics (“catégories” dans lesquelles sont publiés et stockés les messages)
- Des applications qui consomment ces messages
- Des administrateurs ou des applications (Infra-as-code par exemple) qui configurent le cluster, les topics, …

Nous allons voir dans cet article quels sont les moyens qu’AWS met à notre disposition pour appliquer le principe de “moindre privilège”, c’est-à-dire autoriser seulement les actions nécessaires au bon fonctionnement des applications et permettant aux administrateurs de réaliser uniquement les actions qu’ils ont à faire.

Dans un premier temps, il est important de différencier l’API MSK et l’API Apache Kafka.

## API MSK
L’API AWS MSK permet notamment de créer le cluster et de gérer ses paramètres de sécurité, de monitoring et de connectivité. Les paramètres du serveur Kafka peuvent également être édités via cette API.

Toutes ces actions peuvent être réalisées via la CLI, le SDK, les outils d’infra-as-code, ou la console AWS.

Exemple pour la création du cluster MSK :

```
aws kafka create-cluster \
    --cluster-name "service-finance-msk-cluster" \
    --broker-node-group-info file://brokernodegroupinfo.json \
    --kafka-version "2.6.2" \
    --number-of-broker-nodes 3
```

Comme pour toutes les API AWS, l’attribution de permission se fait via des stratégies AWS IAM (Identity and Access Management).

Par exemple, associée à son rôle, la stratégie suivante permet à un ingénieur cloud de lire toutes les informations exposées par MSK (essentiellement des informations de configuration et des informations d’opérations telles que les maintenances des clusters)

```yaml
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "kafka:Describe*",
                "kafka:List*",
                "kafka:Get*",
                "ec2:DescribeNetworkInterfaces",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeSubnets",
                "ec2:DescribeVpcs",
                "kms:DescribeKey"
            ],
            "Effect": "Allow",
            "Resource": "*"
        }
    ]
}
```

Elle correspond au contenu de la stratégie managée par AWS `AmazonMSKReadOnlyAccess`.

Il n’est cependant pas possible de créer des topics ou bien de publier des messages via l’API MSK. C’est à cela que sert l’API Kafka et cet article va faire un focus sur la sécurité de celle-ci dans la cadre d’un cluster MSK.

## API Kafka
Il existe aujourd’hui 4 modes d’authentification sur l’API Kafka dans le cadre d’un cluster MSK

- Accès non authentifié
- Authentification du client TLS via AWS Certificate Manager (mutual TLS)
- Stratégie basé sur des certificats clients
- Authentification SASL/SCRAM via AWS Secret Manager
- Stratégie basé sur des couples noms d’utilisateur/mots de passe
- Authentification basée sur les rôles IAM
- Stratégie basé sur IAM (service AWS qui gère les autorisations)

Il est possible d’activer un ou plusieurs de ces modes sur un nouveau cluster ou un cluster existant. Cependant, il est important de noter que cela nécessite que le chiffrement TLS soit activé entre les clients et les agents (sauf pour l’accès non authentifié). A l’heure où j’écris cet article, il n’est pas possible d’activer ce chiffrement après la création du cluster.

En fonction des modes d'authentification, le port de connexion à utiliser sur les brockers sera différent. Par exemple, la ressource Terraform `aws_msk_cluster` fournit les outputs suivants qui permettent de se connecter au cluster :

`bootstrap_brokers` → Communication en clair entre client et brocker

`bootstrap_brokers_sasl_scram` →  Authentification SASL/SCRAM

`bootstrap_brokers_tls` → Communication TLS (avec authentification via mutual TLS ou non)

bootstrap_brokers_sasl_iam → Authentification IAM

La commande `aws kafka get-bootstrap-brokers --cluster-arn <value>` donne les mêmes informations.

## Accès non authentifié
L’activation de l’accès non authentifié permet à tous les clients ayant un accès réseau au cluster de s’y connecter. Bien qu’une configuration réseau correcte (cluster dans un sous-réseau privé, sans accès public, avec des groupes de sécurité correctement paramétrés) puisse suffire dans certains cas d’utilisation, il est préférable de mettre en place un mécanisme d’authentification.

## Authentification du client TLS via AWS Certificate Manager (mutual TLS)
Le TLS mutuel (mTLS) est un mécanisme d’authentification bi-directionnel qui permet de s’assurer que le trafic entre un client et un serveur puisse être digne de confiance, dans les deux sens. C’est un mécanisme standard utilisé par les serveurs pour authentifier un client.

Dans un premier temps, il faut que le chiffrement TLS soit mis en place.

Chaque serveur a son certificat et sa clé privée. Les clients utilisent le certificat pour authentifier le serveur, c'est-à-dire que son autorité de certification doit être présente dans le truststore.

Pour paramétrer le SSL (one-way) coté client, il faut lui transmettre le truststore :

```ini
security.protocol=SSL
ssl.truststore.location=/tmp/truststore.jks
ssl.truststore.password=<truststore-password>
```

Ce paramétrage n’a rien de spécifique à l’authentification mTLS, il est également présent avec les autres modes d’authentification que nous allons voir dans les prochaines parties. Dans le cas de MSK, le serveur utilise un certificat dont l’autorité de certification (CA) provient d’Amazon. Or le truststore de la JVM fait confiance à cette CA par défaut, donc il n’est pas obligatoire de fournir un truststore au client.

Dans le cadre de l’authentification du client via mTLS, chaque client doit également avoir son certificat et sa clé privée. Le serveur contrôle le certificat et valide la connexion.

Pour paramétrer ce certificat côté client, il faut lui transmettre un keystore (qui contient la clé privée et le certificat signé):

```ini
ssl.keystore.location=/tmp/keystore.jks
ssl.keystore.password=<keystore-password>
ssl.key.password=<key-password>
```

Sur AWS, avec MSK, la gestion des certificats client est simplifiée avec une autorité de certification privée (PCA) d'AWS Certificate Manager. Comme son nom l’indique, c’est un service qui permet de créer des hiérarchies d'autorité de certification privée, dont des autorités de certification racine et subordonnées, sans les coûts d'investissement et de maintenance liés à l'exploitation d'une autorité de certification sur site.

Il est cependant important de noter qu’une PCA ACM coûte 400$ par mois.

Pour activer l’authentification client via mTLS et configurer la PCA au niveau de MSK, c’est très simple : il suffit de lui indiquer l’ARN de la PCA :

```yaml
resource "aws_msk_cluster" "main" {
    // [ … ]
  client_authentication {
   tls {
     certificate_authority_arns = [
     	"arn:aws:acm-pca:eu-west-1:${local.accountId}:certificate-authority/${local.certificate_authority_id}"     
     ]
   }
  }
}
```

Il faut ensuite créer le keystore pour le client à l’aide de la PCA. Pour cela il est possible de suivre les instructions fournies par AWS : https://docs.aws.amazon.com/msk/latest/developerguide/msk-authentication.html#msk-authentication-client

Cette procédure se décompose en 5 étapes :

- Création de la clé privée et du keystore (à l’aide de `keytool`)
- Création d’une certificate request (à l’aide de `keytool`)
- Génération du certificat via la PCA (commande: `aws acm-pca issue-certificate`)
- Récupération du certificat et de la chaîne de certificat (via `aws acm-pca get-certificate`) et combinaison des deux.
- Ajout du certificat au keystore

Le keystore est configuré au niveau du client comme décrit précédemment.

Pour associer des permissions à un utilisateur, il faut définir des Access Control List (ACL). Ce sont des objets propres à Kafka et ce n’est pas obligatoire d’en définir (si la propriété  `allow.everyone.if.no.acl.found` est définie à `true`).

Dans le cas d’une authentification avec mTLS, l’identifiant de l’utilisateur est le DN (X.500 Distinguished Name) qui a été défini dans la création de la clé privée et du certificat. Il prend la forme suivante :

```
CN=writeuser,OU=Unknown,O=Unknown,L=Unknown,ST=Unknown,C=Unknown
```

Note : il est possible d’appliquer des règles de transformation sur le principal.

L’ajout d’ACL est identique qu’on soit sur MSK ou non. Cela peut se faire via la CLI Kafka :

```
bin/kafka-acls.sh --bootstrap-server $brokerssaslscram \
  --add --allow-principal "User:CN=writeuser,OU=Unknown,O=Unknown,L=Unknown,ST=Unknown,C=Unknown" \
  --operation Read --topic test group=* 
  --command-config /tmp/client.properties
```

C’est possible aussi via le provider communautaire terraform :

```yaml
resource "kafka_acl" "test" {
  resource_name       = "test"
  resource_type       = "Topic"
  acl_principal       = "User:CN=writeuser,OU=Unknown,O=Unknown,L=Unknown,ST=Unknown,C=Unknown"
  acl_host            = "*"
  acl_operation       = "Read"
  acl_permission_type = "Allow"
}
```

Pour plus d’informations sur les ACL Kafka, vous pouvez vous référez à la documentation : https://docs.confluent.io/platform/current/kafka/authorization.html

## Authentification SASL/SCRAM via AWS Secret Manager
Salted Challenge Response Authentication Mechanism (SCRAM), aussi connu sous le nom SASL/SCRAM est un protocole SASL (cadre d'authentification et d'autorisation standardisé par l'IETF) qui permet de faire de l'authentification basée sur un nom d’utilisateur et un mot de passe entre un client et un serveur.

Dans la version open source de Kafka, il est possible de créer des utilisateurs via cette commande :

```
bin/kafka-configs 
  --bootstrap-server localhost:9092 --alter \
  --add-config 'SCRAM-SHA-256=[iterations=8192,password=alice-secret],SCRAM-SHA-512=[password=alice-secret]' \
  --entity-type users --entity-name alice
```

Cependant depuis Septembre 2020, AWS propose une version qui utilise Secret Manager pour créer des utilisateurs. Cela rend leur gestion plus facile.

AWS Secrets Manager est un service qui permet, comme son nom l’indique, de stocker des informations sensibles telles que des clés d’API et des mots de passe. Une rotation des secrets peut également être mise en place et des fonctionnalités d’audit sont directement intégrées (via AWS Cloudtrail).

Pour créer un utilisateur, il faut :

- Créer un secret dont le nom commence par **AmazonMSK_** avec comme contenu:

```json
{
  "username": "alice",
  "password": "alice-secret"
}
```

Note : il faut chiffrer le secret avec une clé KMS personnalisée (et non pas avec la clé par défaut)

- Il faut ensuite associer le secret avec le cluster MSK. Cela peut être fait via la console ou via la CLI avec la commande :

```
aws kafka batch-associate-scram-secret --cluster-arn <value> --secret-arn-list <value>
```

ou bien avec Terraform :

```yaml
resource "aws_msk_cluster" "main" {
    // [ … ]
  client_authentication {
   sasl {
     scram = true
   }
  }
}

data "aws_secretsmanager_secret" "admin" {
 name = "AmazonMSK_SASL_ADMIN_CREDENTIALS"
}

resource "aws_msk_scram_secret_association" "admin" {
 cluster_arn     = aws_msk_cluster.main.arn
 secret_arn_list = [data.aws_secretsmanager_secret.admin.arn]
}
```

Pour s’y connecter, il faut fournir les informations de connexion aux producteurs et consommateurs de message. Par exemple pour un producteur en Java :

```java
private static String getScramAuthString() {
    return String.format("org.apache.kafka.common.security.scram.ScramLoginModule required \nusername=\"%s\"\npassword=\"%s\";", username, password);
}

Properties props = new Properties();
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, serverBootstrap);
// Other properties
if (useScram()) {
  props.put("security.protocol", "SASL_SSL");
  props.put("sasl.mechanism", "SCRAM-SHA-512");
  props.put("sasl.jaas.config", getScramAuthString());
}
producer = new KafkaProducer<>(props);
```

Pour associer des permissions à un utilisateur, il faut configurer les ACL Kafka de la même manière qu’avec la méthode mutual TLS.

Le principal à définir dans l’ACL sera le username présent dans le secret (ici alice).

## Authentification basée sur les rôles IAM
Depuis mai 2021, il est désormais possible d’activer l’authentification basée sur les rôles IAM sur les clusters nouveaux ou existant (à condition que le chiffrement TLS entre les clients et les nœuds soit activé).

Cette fonctionnalité permet de gérer l’authentification via un rôle/utilisateur IAM que possèdent déjà de nombreuses charges de travail s’exécutant sur AWS.

Elle permet également de se passer des ACL Kafka (qui n’ont plus d’effet) car c’est avec les stratégies IAM que sont définis les droits aux ressources Kafka.

Par exemple, si un service doit produire uniquement sur le topic user-event, alors la stratégie IAM suivante est suffisante (dans l’hypothèse où arn:aws:kafka:eu-west-1:1234567890:cluster/my-cluster/uuid est l’ARN du cluster MSK) :

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Producer",
            "Action": [
                "kafka-cluster:Connect",
                "kafka-cluster:DescribeTopic",
                "kafka-cluster:WriteData"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:kafka:eu-west-1:1234567890:cluster/my-cluster/uuid",
                "arn:aws:kafka:eu-west-1:1234567890:topic/my-cluster/uuid/
user-event"
            ]
        }
    ]
}
```

Note : pour produire via des transactions et/ou de manière idempotent, cette stratégie doit être complétée (cf https://docs.aws.amazon.com/msk/latest/developerguide/iam-access-control.html#iam-access-control-use-cases) .

Pour consommer sur le topic user-event, à partir du groupe my-app, en appliquant le principe du moindre privilège, il faut appliquer cette stratégie :

```yaml
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Consumer",
            "Action": [
                "kafka-cluster:Connect",
                "kafka-cluster:DescribeTopic",
                "kafka-cluster:ReadData",
                "kafka-cluster:DescribeGroup",
                "kafka-cluster:AlterGroup"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:kafka:eu-west-1:1234567890:cluster/my-cluster/uuid"
                "arn:aws:kafka:eu-west-1:1234567890:topic/my-cluster/uuid/
user-event",
                "arn:aws:kafka:eu-west-1:1234567890:group/my-cluster/uuid/
my-app",
            ]
        }
    ]
}
```

Comme pour la méthode précédente, l’authentification se base sur le protocole SASL mais utilise un mécanisme créé par AWS appelé AWS_MSK_IAM.

AWS propose une librairie Java pour faciliter la mise en place de ce mode d’authentification. Cependant, on peut résumer le déroulement du mécanisme comme ceci :

- Le client génère un payload qui contient les informations relatives à l’action qu’il veut entreprendre.
- Il est ensuite signé avec la méthode AWS SigV4 (processus permettant d'ajouter des informations d'authentification à AWS à une requête). Le payload final ressemble à ça :

```yaml
{
    "version" : "2020_10_22",
    "host" : "<broker address>",
    "user-agent": "<user agent string from the client>",
    "action": "kafka-cluster:Connect",
    "x-amz-algorithm" : "<algorithm>",
    "x-amz-credential" : "<clientAWSAccessKeyID>/<date in yyyyMMdd format>/<region>/kafka-cluster/aws4_request",
    "x-amz-date" : "<timestamp in yyyyMMdd'T'HHmmss'Z' format>",
    "x-amz-security-token" : "<clientAWSSessionToken if any>",
    "x-amz-signedheaders" : "host",
    "x-amz-expires" : "<expiration in seconds>",
    "x-amz-signature" : "<AWS SigV4 signature computed by the client>"
}
```

- Le code exécuté par le nœud Kafka (légèrement modifié par AWS par rapport à la version OpenSource) recalcule la signature pour valider l’intégrité de la requête et accepte la demande d'authentification SASL si elle est valide.

Mais comme expliqué précédemment, il n’est pas nécessaire de ré-implémenter tout le protocole dans le cadre d’un client Kafka Java. Une librairie peut le faire à votre place : https://github.com/aws/aws-msk-iam-auth. Elle peut être soit inclue via Maven (méthode recommandée, voir ci-dessous), soit inclue via son Jar dans le classpath.

```xml
<dependency>
    <groupId>software.amazon.msk</groupId>
    <artifactId>aws-msk-iam-auth</artifactId>
    <version>1.1.1</version>
</dependency>
```

Il faut ensuite fournir les informations de connexion aux producteurs et consommateurs de messages. On suppose ici que la configuration du projet vers AWS est faite et que le rôle ou utilisateur associé a les permissions IAM pour se connecter au cluster.

Pour un producteur en Java, il faut compléter la configuration comme ceci :

```java
configuration.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,getIamKafkaBootstrapServers());
configuration.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
configuration.put("sasl.mechanism", "AWS_MSK_IAM");
configuration.put("sasl.jaas.config", "software.amazon.msk.auth.iam.IAMLoginModule required;");
configuration.put("sasl.client.callback.handler.class", "software.amazon.msk.auth.iam.IAMClientCallbackHandler");
```

A l’heure de l’écriture de cet article, la protocole IAM Auth pour MSK n’est pas disponible dans les librairies Kafka dans un langage autre que Java (notamment dans la librairie utilisée par le provider communautaire Terraform Kafka).

Il est également important de savoir que lorsqu’on utilise l’authentification IAM sur MSK avec des nœuds de la famille `kafka.t3.small` (la plus petite taille), le cluster peut accepter des nouvelles connexions TCP à hauteur de une par seconde par broker (20 pour les autres types de nœuds).

C’est une limitation qui peut être facilement atteinte lors du démarrage de services qui ouvre plusieurs connexions. Dans ce cas, AWS recommande de mettre le paramètre `reconnect.backoff.ms` à 1000 mais dans certains cas il sera nécessaire d’utiliser des gabarits de nœuds supérieur (`kafka.m5.large` par exemple)

## Conclusion
AWS propose plusieurs possibilités pour simplifier l’authentification dans sa solution de Kafka managé, MSK.

Le TLS bi-directionel (mTLS) se base des certificats clients générés par une autorité de certificat privée. Dans le cas d’Amazon MSK, cette autorité doit être une PCA ACM. C’est une méthode reconnue et fréquemment utilisée. Cependant la création du keystore implique une complexité opérationnelle plus importante que les autres méthodes. Le prix d’une PCA ACM (400$ /mois) peut également être un frein à l’adoption.

Le mécanisme SASL/SCRAM permet de mettre en place facilement une authentification Username/Password basée sur AWS Secret Manager.

Le dernier mécanisme est l’authentification basée sur AWS IAM. Cela permet de se passer de la gestion de certificats ou de secrets à condition que le client ait déjà un accès AWS configuré. De plus, ce mécanisme permet de se passer des ACL Kafka car la partie autorisation est déléguée à IAM et à ses stratégies. Cependant c’est un mécanisme qui est plus jeune, moins utilisé et, en dehors de Java, pas présent dans les librairies Kafka.

source : https://blog.ippon.fr/