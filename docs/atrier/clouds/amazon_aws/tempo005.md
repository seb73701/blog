---
title: Introduction à la sécurité des environnements AWS
description: 
published: true
date: 2023-04-21T10:58:51.721Z
tags: cloud, aws, sécurité
editor: markdown
dateCreated: 2023-04-21T10:58:46.533Z
---

# Introduction à la sécurité des environnements AWS

> AWS est un vaste écosystème qui intègre un grand nombre de services en tout genre, traitant aussi bien de stockage, de calcul et de réseau que d’IoT, d’IA et de robotique.Nous allons nous intéresser à quelques éléments d’introduction à la sécurité des environnements AWS, en nous fixant pour objectif de disposer de toutes les connaissances nécessaires à la compréhension de la suite du dossier, qui traitera des techniques d’intrusion, d’élévation de privilèges et de post-exploitation au sein d’un environnement AWS, ainsi que des problématiques de détection et de réponse à incident.

## 1. En quoi les environnements AWS posent-ils problème ?
AWS ne se réduit pas à un service de cloud computing mettant à disposition des machines virtuelles : les infrastructures que l’on y trouve sont bien plus complexes, et contiennent souvent des ressources sensibles. Il n’est pas rare, en effet, de voir des entreprises y déporter une partie significative de leur système d’information, allant parfois jusqu’à mettre en place une association entre leur réseau local et leur infrastructure cloud, notamment en fédérant les identités de leur Active Directory avec celles de leur compte AWS.

Pour être à la hauteur de ces enjeux, Amazon propose un nombre conséquent de services de sécurité, permettant de gérer les clés de chiffrement, de stocker les secrets, de filtrer les flux réseau, de détecter les attaques ou encore de contrôler les accès.

Bien que cette multitude de services garantisse, en théorie, un bon niveau de sécurité, elle présente un inconvénient majeur : sa grande complexité. De fait, les concepts de sécurité sont parfois très pointus, la configuration des mécanismes impliqués souvent exigeante et la maîtrise d’un environnement en perpétuelle évolution toujours difficile.

Par conséquent, dans la pratique, des défauts de configuration apparaissent régulièrement, créant ainsi les conditions de possibilité pour un attaquant de s’introduire dans l’environnement, ou d’y compromettre des ressources.

La criticité des environnements AWS, qui nous amène à nous y intéresser, tient donc d’une part de la nature sensible des ressources qu’ils hébergent, et d’autre part de la difficulté d’y maintenir un bon niveau de sécurité à mesure qu’ils évoluent.

## 2. La gestion des identités et des accès
Parmi tous les services de sécurité proposés par AWS, le gestionnaire d’identités et d’accès (IAM) est sans doute le plus important, du fait de sa dimension structurelle et de l’étendue de son champ d’application.

Nous aurons l’occasion de le rencontrer à de nombreuses reprises, prenons donc le temps d’en comprendre les grands principes.

### 2.1 Les entités
Le service IAM distingue deux typologies d’entité: les identités et les ressources.

#### 2.1.1 Les identités
Les identités correspondent soit à un utilisateur – c’est-à-dire à une personne physique, soit à un groupe d’utilisateurs, soit à un rôle.

La notion de rôle est spécifique à AWS et doit être parfaitement comprise avant d’aborder les prochains sujets.

Un rôle est un ensemble de permissions « flottantes », que l’on peut accorder de manière temporaire à une identité ou à une ressource. En cela, un rôle peut être représenté par une casquette, indicatrice d’une fonction, qui peut être portée par les uns ou par les autres, sans que la fonction qu’elle représente ne soit liée de manière persistante à celui qui la porte. Ainsi, plutôt que d’ajouter un utilisateur ponctuel à un groupe de projet, on préférera lui attribuer un rôle lui donnant temporairement un accès limité à l’environnement de travail. Par ailleurs, dans la pratique, les rôles sont régulièrement utilisés pour mutualiser des droits associés à des ressources, celles-ci ne pouvant être ajoutées à un groupe.

Nous n’irons pas plus loin sur le fonctionnement des rôles pour le moment, mais aurons l’occasion d’apporter quelques précisions sur leurs modalités d’approbation dans l’article « Compromission des services exposés d’AWS ».

#### 2.1.2 Les ressources
Une ressource est, selon les termes utilisés dans la documentation AWS, « une entité que l’on peut utiliser ». Cela concerne les espaces de stockage, les machines virtuelles, les clés de chiffrement, les modèles de déploiement, et bien d’autres choses encore.

D’une certaine manière, les ressources peuvent également être vues comme des sujets, dans la mesure où elles peuvent utiliser d’autres ressources. Par exemple, une machine virtuelle peut utiliser un espace de stockage ou une clé de chiffrement et, en cela, doit être désignée explicitement comme exécutante légitime des actions qu’elle réalise, au même titre qu’une identité.

### 2.2 Les politiques de sécurité
Tous les accès, qu’ils émanent indifféremment d’une identité ou d’une ressource, sont réglementés par des politiques de sécurité, également appelées stratégies de sécurité.

Une politique peut être attachée à un utilisateur, à un groupe d’utilisateurs, à un rôle (définissant ainsi la fonction qu’il représente), ou à une ressource.

Elle prend la forme d’un document JSON, constitué de la manière suivante :

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Accès stratégies Administrateurs",
            "Effect": "Allow",
            "Action": [
                "iam:ListGroupPolicies",
            ],
            "Resource": "arn:aws:iam::1234:group/Administrateurs"
        }
    ]
}
```

Cette politique signifie que les entités auxquelles elle est attachée ont le droit (Allow) de lister les politiques de sécurité liées à un groupe (iam:ListGroupPolicies), et plus précisément au groupe « Administrateurs » du compte AWS identifié par le numéro 1234.

> L’ARN, pour Amazon Ressource Name, est une typologie d’identification des éléments d’un environnement AWS, qu’il s’agisse indifféremment d’une identité IAM, d’une ressource, d’une politique de sécurité, ou encore d’une clé de chiffrement.
> 
> **La forme générique inclut notamment le service concerné (ici IAM), l’identifiant du compte AWS (1234), le type de l’entité visée (groupe) et le nom de cette entité (Administrateurs).**
{.is-info}


## 3. Utiliser AWS
Il existe trois vecteurs d’administration d’un environnement AWS : l’application web dite « console », la bibliothèque de programmation boto3, et l’outil en ligne de commandes awscli. Les trois interagissent avec une API commune, qui agrège toutes les fonctionnalités existantes.

Seul l’accès à la console nécessite un couple (nom d’utilisateur, mot de passe) : les appels effectués depuis la ligne de commandes [1] utilisent, pour les utilisateurs (nous verrons plus tard qu’il n’en va pas de même pour les rôles), un couple de clés d’API (Access Key ID, Secret Access Key ID). Ces clés sont générées depuis la console, et doivent ensuite être configurées sur l’environnement local.

Nous supposerons tout au long du dossier que nous utilisons un environnement Linux.

Commençons par installer awscli, en utilisant l’utilitaire pip :

```
$ pip3 install awscli
```

Vérifions que l’outil est effectivement installé :

```
$ aws --version
aws-cli/1.16.118 Python/3.7.3rc1 Linux/4.19.0-debian-amd64 botocore/1.12.108
```

Tout semble bon ! À présent, nous allons configurer les clés d’API que nous avons générées depuis le service IAM de notre environnement AWS :

```
$ aws configure     
AWS Access Key ID []: AKIAI[...]XBLLXKSQ
AWS Secret Access Key []: oDe6L[...]MTpEfZlVz
```

Désormais, tous les appels à l’API que nous réaliserons au moyen de la commande aws seront effectués avec les permissions accordées à notre utilisateur.

Les commandes suivent le modèle suivant :

aws $service $appel $arguments

$service correspondant au service concerné par notre requête, $appel le nom de la méthode d’API utilisée, et $arguments aux arguments de la méthode.

Nous pouvons par exemple lister nos groupes, en interrogeant le service iam avec l’appel list-groups :

```json
$ aws iam list-groups
{
    "Groups": [
        {
            "Path": "/",
            "GroupName": "Production",
            "GroupId": "AGPAJS[...]XP6PGEE",
            "Arn": "arn:aws:iam::1234:group/Production",
            [..]
        },
        {
            "Path": "/",
            "GroupName": "Admin_SiteA",
            "GroupId": "AGPA[...]BN2O",
            "Arn": "arn:aws:iam::1234:group/Admin_SiteA",
            "CreateDate": "2019-03-06T15:09:59Z"
        }
 ]
}
```

Le résultat nous indique que nous appartenons aux groupes « Production » et « Admin_SiteA ».

La liste de tous les appels à l’API, ainsi que leur documentation, est référencée dans le document AWS CLI Command Reference [1].

## Conclusion
AWS propose de nombreux services de sécurité, mais ce nombre ainsi que leur complexité rend leur bonne utilisation difficile. Maintenant que nous avons en introduit les bases, je vous propose de découvrir quelques techniques d’intrusion dans l’article 2 « Compromission des services exposés d’AWS »du présent dossier.

## Référence
[1] https://docs.aws.amazon.com/cli/latest/reference/



source : https://connect.ed-diamond.com/MISC/misc-105/introduction-a-la-securite-des-environnements-aws