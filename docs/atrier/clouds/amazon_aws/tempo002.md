---
title: Autoriser des accès temporaires à une base de données RDS
description: 
published: true
date: 2023-04-21T10:58:27.283Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:58:22.091Z
---

# Autoriser des accès temporaires à une base de données RDS


## Introduction
Les opérations manuelles ne sont jamais recommandées en production. Cependant, il arrive parfois pour un besoin très précis de devoir accéder à la base.

Pour des raisons de partage de responsabilité, il se trouve très souvent que les développeurs n’aient pas accès aux identifiants des bases de données de production, et ne peuvent donc pas s’y authentifier. En cas de problème et de phase de troubleshooting, il est essentiel d'avoir un moyen d'accéder à la base. Une procédure exceptionnelle et temporaire doit être mise en place pour donner cet accès. D’un point de vue sécurité, il est important de respecter le principe de moindre privilège. On ne souhaite donc pas autoriser l’accès à l’ensemble des développeurs. Idéalement, on souhaite aussi éviter qu’un utilisateur soit configuré de manière permanente sur la base et que certaines personnes y aient accès. La solution idéale serait d’avoir un mécanisme nous permettant de demander un accès temporaire, et que cet accès soit révoqué automatiquement.


C’est exactement le besoin que nous avons eu chez un client. Pour y répondre, nous avons mis en place une solution basée sur Systems Manager Automation, que je vais vous présenter dans cet article. L’ensemble de l’infrastructure pour cet article est relative à AWS et aux bases de données RDS.


## Utiliser des identifiants IAM pour s’authentifier sur la base RDS

Afin de pouvoir s’affranchir d’identifiants mysql ou postgre pour les développeurs, il faut, dans un premier temps, activer un plugin sur la base RDS.  Ce plugin peut être activé manuellement dans la console AWS. Mais comme toujours avec le cloud, les opérations manuelles ne sont pas reproductibles.
Avec Terraform, par exemple, nous avons la possibilité d’activer ce plugin sur la base :

```
resource "aws_db_instance" "my_db" { 
  ... // Other configuration for your RDS 
  iam_database_authentication_enabled = true 
}
```

Une fois configuré, il faut également créer un utilisateur dans la base qui a le plugin d’authentification activé. La documentation d’AWS nous donne ces requêtes :

Pour Mysql
```sql
CREATE USER jane_doe IDENTIFIED WITH AWSAuthenticationPlugin AS 'RDS'; 
```

Pour PostgreSQL
```sql
CREATE USER iamuser WITH LOGIN; 
GRANT rds_iam TO iamuser;
```


On peut reproduire cette requête dans Terraform avec un provider mysql. Voici ce que cela donne (en omettant l’initialisation du plugin) avec ce provider :

```
resource "mysql_user" "iam" {
  user               = "iam"
  host               = "%"
  auth_plugin        = "AWSAuthenticationPlugin"
}

resource "mysql_grant" "iam" {
  user       = mysql_user.iam.user
  host       = mysql_user.iam.host
  database   = var.database
  privileges = ["SELECT", "INSERT", "DELETE", "UPDATE"]
 }
```


Je recommande de créer deux utilisateurs : un avec des accès de lecture / écriture, et un avec des accès en lecture seule.

L’ensemble de ces opérations est défini dans la [documentation officielle d'AWS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.html).

Une fois cela fait, il faudra ajouter une policy IAM aux utilisateurs (ou rôles/groupes selon les cas) qui nous autorise à nous authentifier avec ce plugin sur la base. C’est cette partie là que nous allons exécuter et révoquer automatiquement avec Systems Manager Automation.

## Le chef d’orchestre, Systems Manager Automation
Chez notre client, c’est la solution que nous avons choisie. [Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) est un service qui nous permet d’exécuter des opérations appelées “runbooks”. Ces runbooks sont très adaptés à la configuration de machines EC2 mais peuvent aussi interagir avec d’autres services d’AWS.

Cette solution n’est pas la seule que l’on peut mettre en place chez AWS. On pourrait tout à fait utiliser un AWS Step Functions ou simplement des lambdas directement.

Alors, pourquoi a-t-on choisi cette solution ?
Et bien premièrement, pourquoi pas ? Nous ne connaissions pas ce service et nous avons souhaité l’explorer. Ensuite, on peut définir un formulaire d’entrée pour paramétrer notre runbook ! Systems Manager nous propose également une [action de validation manuelle](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-approve.html) qui nous intéressait (nous en reparlerons dans la suite de cet article).  En plus de quelques actions intégrées directement, on peut aussi exécuter du Python.


Ce service s’intègre également très bien avec les autres. On peut par exemple exécuter une lambda qui va utiliser des API externes non disponibles dans Systems Manager Automation, ou [appeler l’API d’AWS directement](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-executeAwsApi.html).

## Un exemple de runbook
Avant d’expliquer ce que l’on a mis en place comme runbook, je vais d’abord donner un peu de contexte sur l’environnement dans lequel celui-ci à été déployé.

La base de données étant dans un sous-réseau privé, nous avons aussi utilisé une connexion VPN mise en place bien avant ce runbook pour accéder aux bases.

Chez le client, l’authentification sur AWS est faite grâce à un Identity Provider configuré au niveau d’AWS en utilisant SAML. Les identifiants de l’Identity Provider  nous permettent d’assumer un rôle sur AWS, qui est le même pour tous les utilisateurs du même statut (administrateurs, tech, techlead etc..). Nous ne souhaitions pas ajouter la policy IAM sur un rôle qui va être assumé par plusieurs personnes. Nous avons donc mis en place un groupe au niveau de l’Identity Provider qui permet d’assumer un nouveau rôle. Pour autoriser un utilisateur, nous devons d’abord faire un appel à notre Identity Provider pour ajouter l’utilisateur temporairement à un groupe qui nous permettra d’assumer le rôle. Nous ajoutons dans un deuxième temps la policy sur le rôle.

Afin de simplifier cet article, je vais présenter une version légère de notre runbook. On va supposer à partir de maintenant que la base de données est accessible au niveau du réseau, et que l’authentification sur AWS se fait avec des utilisateurs IAM.

Voici donc un schéma qui résume les étapes du runbook:

![systems-manager-acce-s-temporaire-rds.excalidraw.png](/assets/img/clouds/tempo/systems-manager-acce-s-temporaire-rds.excalidraw.png)

Nous allons expliquer les différentes parties de ce runbook, cependant aucune implémentation de code ne sera fournie dans cet article..
Avant de lancer le runbook, l’utilisateur doit remplir un formulaire avec certaines valeurs.

On demande d’abord le nom de l’utilisateur IAM pour lequel on souhaite faire l’authentification. Dans notre cas réel, nous demandions l’email associé sur l’Identity Provider, mais cela revient au même ici.

Nous demandons ensuite évidemment le nom de la base de données, appelé “Database Identifier” sur la console.
On demande ensuite de déterminer une durée des accès. On a imposé un choix parmi une liste de valeurs prédéfinies pour éviter les abus.
Enfin, on demande si l’utilisateur souhaite des accès en lecture seule ou également en écriture.

Ensuite, on a mis en place [l’approbation manuelle](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-approve.html) de Systems Manager Automation. Cela nous permet de confirmer la demande avant de donner les accès afin de limiter qui peut accéder à la base. Sans cela, nous n’aurions pas résolu notre problème initial ! En paramétrant cette approbation, on peut lui spécifier un topic SNS sur lequel envoyer une notification.  Attention, ici, il faut penser à autoriser le runbook à envoyer une notification sur SNS. Un ARN de rôle peut être spécifié à la création du runbook.


Nous avons alors développé une Lambda très simple qui permet d’envoyer un message sur Slack. Le message contient les informations demandées au début afin que l’approbation se fasse en toute connaissance de cause, ainsi que des liens pour accepter ou rejeter la demande.

Systems Manager attend alors qu’une réponse soit donnée avant de continuer. Si la réponse est positive, alors la suite du runbook s’exécute. L’étape d’après est d’ajouter une policy sur notre utilisateur IAM. On calcule dynamiquement cette policy en se basant sur l’[exemple fourni par la documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.IAMPolicy.html).

Une fois cela fait, l’utilisateur peut se connecter à la base de données en suivant les [étapes définies dans la documentation d’AWS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.IAMDBAuth.Connecting.AWSCLI.html).

Finalement, la [fonctionnalité de délai](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-action-sleep.html) de Systems Manager nous permet d’attendre le temps imparti avant de supprimer la policy, et donc de révoquer les accès. Attention cependant : une fois la policy supprimée, cela ne garantit pas nécessairement que la session mysql sera interrompue. Cela empêche juste de démarrer une nouvelle session.

## En conclusion
Nous avons un workflow contenu dans Systems Manager (et quelques lambdas) qui nous permet d’autoriser des accès temporaires à une base de données. Nous avons gagné en sécurité et nous avons répondu au besoin initial. Aucune manipulation d'identifiants n’a lieu pour l’utilisateur.

En bonus, nous pouvons facilement auditer les différentes demandes d’accès ! Il est très simple de savoir qui a demandé quel accès, sur quelle base, pendant combien de temps et quel jour. On peut aussi voir qui à valider la demande. La page Systems Manager Automation de la console AWS nous expose toutes ces informations. Nous pouvons également voir ces infos sur les messages Slack envoyés à chaque requête.

Bien entendu, la solution présentée dans cet article n’est pas totalement complète pour être utilisée en production. Il y a des éléments à adapter à votre situation, et il y a des aspects de sécurité à prendre en compte. Les bases de données doivent être dans des réseau privés, et donc l’accès à cette base doit être sécurisé, par exemple par un VPN. Il est tout à fait envisageable d’ajouter une étape dans le workflow qui autorise temporairement sur les groupes de sécurité l'adresse IP de l’utilisateur.

Source = https://blog.ippon.fr/tag/devops/