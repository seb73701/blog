---
title: Module 6 - sécurité
description: 
published: true
date: 2023-04-21T10:56:30.190Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:22.965Z
---

# Module 6 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Expliquer les avantages du modèle de responsabilité partagée.
- Décrire l’authentification multifacteur (MFA).
- Faire la distinction entre les niveaux de sécurité AWS Identity et Access Management (IAM).
- Expliquer les principaux avantages d’AWS Organizations.
- Décrire les bases des stratégies de sécurité.
- Résumer les avantages de la conformité avec AWS.
- Expliquer les bases des services de sécurité AWS supplémentaires.

```
video : xxx.mkv
```
*Figure. Module introduction - security*


<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Il semble que nous soyons en train d’aller plus loin sur notre compte AWS. Les choses se passent bien. Et je veux discuter avec vous des mesures de sécurité que nous avons mises en place. Par cela, j'entends que nous devons décrire les différents mécanismes de sécurité que nous proposons sur AWS Cloud comme le modèle de responsabilité partagée. 
> 
> Avec le modèle de responsabilité partagée, AWS contrôle la sécurité du cloud et les clients contrôlent la sécurité dans le cloud. Nous, en tant qu'AWS, contrôlons les centres de données, la sécurité de nos services, et toutes les couches de cette section. Ensuite viennent les charges de travail que les clients AWS exécutent dans le cloud et qu’ils ont la responsabilité de sécuriser. C'est quelque chose que nous partageons avec les clients pour assurer la sécurité dans le cloud.
> 
> Jetons un œil aux différents autres services de sécurité, les mécanismes, et fonctions offerts par AWS dans ce module. Restez attentifs.
</div>

# Modèle de responsabilité partagée

```
video : xxx.mkv
```
*Figure. The AWS shared responsability model*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Quand il s'agit de sécuriser votre entreprise sur AWS, il est important de se poser la question suivante : En fin de compte, qui est responsable de la sécurité ? C’est A : Vous, le client ? ou B : AWS ? Et la bonne réponse est : Oui. Les deux Les deux sont finalement responsables de s’assurer que vous êtes en sécurité. 
> 
> Si vous êtes expert en sécurité et vous me regardez en ce moment, vous êtes probablement en train de secouer la tête en disant qu’il est impossible d’avoir deux entités différentes, responsables ultimes d’un seul objet. Il ne s’agit pas de sécurité, c’est tout simplement impossible. Chez AWS, nous sommes tout à fait d’accord. Mais pour nous, nous ne regardons pas votre environnement en tant qu'objet unique. Nous le considérons au contraire comme un ensemble de parties qui s’appuient les unes sur les autres. AWS est responsable de la sécurité de certains objets. Responsable à 100 % de ces derniers. Vous êtes responsable à 100 % de la sécurité des autres. C'est ce qu'on appelle un modèle de responsabilité partagée. 
> 
> C’est comme sécuriser une maison. Le constructeur a bâti la maison avec quatre murs et une porte. Il est de sa responsabilité de s'assurer que les murs et les portes sont solides. C’est votre responsabilité, le propriétaire de la maison, de fermer et de verrouiller les portes. 
> 
> C'est tout aussi simple sur AWS. Prenez EC2, par exemple. EC2 se trouve dans un bâtiment physique, un centre de données qui doit être sécurisé. Il dispose d'un réseau et d'un hyperviseur qui prennent en charge vos instances et leurs systèmes d’exploitation individuels. En plus de ce système d'exploitation, vous avez votre application, ce qui prend en charge vos données. Donc, pour EC2 et tous les services proposés par AWS, il existe une pile similaire de parties placées les unes sur les autres. AWS est responsable à 100 % de certaines parties, et vous l’êtes des autres. 
> 
> Donc, commençons par la couche physique. C'est du fer et du béton, des clôtures et des gardes de sécurité. Quelqu'un doit posséder le béton. Quelqu'un doit faire recruter du personnel pour le périmètre physique, 24h sur 24 et 7 jours sur 7. C’est AWS. Au-dessus de la couche physique, nous avons notre réseau et notre hyperviseur. Je ne vais pas entrer dans les détails sur la manière dont ils sont tous sécurisés, mais fondamentalement, nous avons réinventé ces technologies pour les rendre plus rapides, plus fortes et inviolables. 
> 
> Mais vous n'avez pas à nous croire sur parole. Nous avons de nombreux auditeurs tiers qui ont vérifié le code et la façon dont nous construisons notre infrastructure et qui peuvent fournir la bonne documentation requise pour vos structures de conformité en matière de sécurité. En plus de tout cela, sur EC2, vous pouvez maintenant choisir le système d’exploitation sur lequel vous souhaitez exécuter. 
> 
> C'est la ligne de division magique qui sépare notre responsabilité. La responsabilité d'AWS et votre responsabilité. C'est votre système d'exploitation. Vous en êtes à 100 % responsable. AWS n'a pas de porte dérobée dans votre système. La clé de chiffrement est uniquement en votre possession pour vous connecter à la racine de ce système d’exploitation ou pour créer des comptes utilisateur ici. Ce n’est pas différent d’une entreprise de construction qui garderait des doubles de votre clé de porte d’entrée, AWS ne peut pas entrer dans votre système d’exploitation. Et voici un indice. Si quelqu'un d'AWS appelle et vous demande votre clé de système d'exploitation, ce n’est pas AWS. 
> 
> Cela signifie que votre équipe d’opérations est 100 % responsable de maintenir le système d'exploitation corrigé. Si AWS découvre qu’il y a de nouvelles vulnérabilités dans votre version de Windows, par exemple, nous pouvons certainement en informer le propriétaire de votre compte, mais ne pouvons pas déployer de correctif. C'est une très bonne chose pour votre sécurité. Cela signifie que personne ne peut déployer quoi que ce soit qui pourrait endommager votre système sans que votre équipe en soit chargée. En plus de ce système d’exploitation, vous pouvez exécuter n'importe quelle application de votre choix. Vous les possédez. Vous les entretenez. 
> 
> Ce qui nous amène à la partie la plus importante de la pile : vos données. Données. C'est toujours dans votre domaine de contrôle. Et parfois, vous voudrez peut-être avoir vos données ouvertes à la vue de tous, comme des images sur un site de vente au détail. Mais pour des comptes en banque ou des soins de santé, pas tellement. AWS fournit un ensemble d'outils nécessaire pour que les données soient ouvertes à certaines personnes autorisées, à tous, à seulement une personne dans des conditions spécifiques, ou peut même les verrouiller pour que personne ne puisse y accéder. De plus, vous avez la possibilité d'avoir un chiffrement omniprésent. De cette manière, même si vous avez laissé votre porte d’entrée ouverte par accident, on ne pourrait voir qu’un contenu chiffré illisible. 
> 
> Le modèle de responsabilité partagée AWS consiste à s’assurer que les deux côtés comprennent exactement quelles sont les tâches qui nous incombent. Pour simplifier, AWS est responsable de la sécurité du cloud et vous êtes responsable de la sécurité du cloud. Ensemble, vous avez un environnement fiable.
</div>

## Le modèle de responsabilité partagée AWS

Tout au long de ce cours, vous avez découvert plusieurs ressources que vous pouvez créer dans AWS Cloud. Ces ressources incluent les instances Amazon EC2, les compartiments Amazon S3 et les bases de données Amazon RDS. Qui est responsable de la sécurité de ces ressources : vous (le client) ou AWS ?

La réponse est les deux. En effet, vous ne traitez pas votre environnement AWS comme un seul objet. Au contraire, vous traitez l’environnement comme un ensemble de pièces qui reposent les unes sur les autres. AWS est responsable de certaines pièces de votre environnement, et vous (le client) êtes responsable d’autres pièces. Ce concept est connu sous le nom de **[modèle de responsabilité partagée](https://aws.amazon.com/compliance/shared-responsibility-model/)**.

Ce modèle fait la distinction entre les responsabilités du client (communément appelées « sécurité dans le cloud ») et les responsabilités d’AWS (communément appelées « sécurité du cloud »).

![aws_model_resp_partage.png](/assets/img/clouds/tempo/aws_model_resp_partage.png)

Ce modèle est semblable à la répartition des responsabilités entre le propriétaire d’une maison et le constructeur. Le constructeur (AWS) est responsable de la construction de votre maison et de sa solidité. En tant que propriétaire (le client), il est de votre responsabilité de sécuriser tout ce qui se trouve dans la maison en vous assurant que les portes sont fermées et verrouillées. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Clients - sécurité dans le cloud" value="true">

Les clients sont responsables de la sécurité de tout ce qu’ils créent et placent dans AWS Cloud.

Lorsque vous utilisez les services AWS, en tant que client, vous gardez un contrôle absolu sur votre contenu. Vous êtes responsable de la gestion des exigences de sécurité pour votre contenu, y compris le contenu que vous choisissez de stocker sur AWS, les services AWS que vous utilisez et les personnes qui ont accès à ce contenu. Vous contrôlez également la manière dont les droits d’accès sont accordés, gérés et révoqués.

Les mesures de sécurité que vous prenez dépendront de facteurs tels que les services que vous utilisez, la complexité de vos systèmes et les besoins opérationnels et de sécurité spécifiques de votre entreprise. Les étapes comprennent la sélection, la configuration et l’application de correctifs aux systèmes d’exploitation qui s’exécuteront sur les instances Amazon EC2, la configuration de groupes de sécurité et la gestion de comptes d’utilisateurs.
</div>

<div class="collapseContent" display="AWS - sécurité du cloud" value="true">

AWS est responsable de la sécurité du cloud.

AWS exploite, gère et contrôle les composants à toutes les couches de l’infrastructure. Cela inclut des domaines tels que le système d’exploitation hôte, la couche de virtualisation et même la sécurité physique des centres de données à partir desquels les services fonctionnent. 

AWS se charge de protéger l’infrastructure mondiale sur laquelle s’exécutent tous les services proposés dans AWS Cloud. Cette infrastructure inclut les régions AWS, les zones de disponibilité et les emplacements périphériques.

AWS gère la sécurité du cloud, en particulier l’infrastructure physique qui héberge vos ressources, notamment :

- Sécurité physique des centres de données
- Infrastructure matérielle et logicielle
- Infrastructure réseau
- Infrastructure de virtualisation

Bien que vous ne puissiez pas visiter les centres de données AWS pour voir directement cette protection, AWS fournit plusieurs rapports d’auditeurs tiers. Ces auditeurs ont vérifié sa conformité à plusieurs normes et règlements en matière de sécurité informatique.
</div>

## Contrôle des connaissances

**Quelles sont les tâches qui incombent aux clients ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q1r1" name="question1" value="1">
  <label for="q1r1">Maintien de l’infrastructure réseau</label><br>
  <input type="checkbox" id="q1r2" name="question1" value="2">
  <label for="q1r2">Application de correctifs logiciels sur les instances Amazon EC2</label><br>
  <input type="checkbox" id="q1r3" name="question1" value="3">
  <label for="q1r3">Implémentation de contrôles de sécurité physique dans les centres de données</label><br>
  <input type="checkbox" id="q1r4" name="question1" value="4">
  <label for="q1r4">Définition des autorisations pour les objets Amazon S3</label><br>
  <input type="checkbox" id="q1r5" name="question1" value="5">
  <label for="q1r5">Gestion des serveurs qui exécutent des instances Amazon EC2</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- Application de correctifs logiciels sur les instances Amazon EC2
- Définition des autorisations pour les objets Amazon S3

Les trois autres réponses sont des tâches qui relèvent de la responsabilité d’AWS.

En savoir plus :

- [Modèle de responsabilité partagée AWS](https://aws.amazon.com/compliance/shared-responsibility-model/)
</div>

# Autorisations et accès utilisateur

```
video : xxx.mkv
```
*Figure. User permissions and access*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Dans le café, chaque employé a une identité. Ils viennent au travail le matin et ils se connectent au système pour pointer, utiliser les registres et gérer les systèmes, faire tourner le café au quotidien. Nous avons les caisses enregistreuses, les ordinateurs qui nous aident à exécuter l’ensemble de l’opération. Chaque personne a un accès unique à ces systèmes en fonction de leur poste. 
> 
> Imaginons que Rudy est à la caisse et prend les commandes et Blaine est à l’arrière, si on vérifie les niveaux d’inventaire sur l’ordinateur, ils ont deux connexions différentes et deux ensembles d’autorisations différents. Rudy peut utiliser la caisse enregistreuse, mais s’il se connecte au système de stocks, il ne pourrait pas le gérer. 
> 
> Vous voudrez étendre la portée des autorisations de vos utilisateurs dans AWS de manière similaire. Lorsque vous créez un compte AWS, on vous donne ce qu’on appelle l’utilisateur du compte racine. L’utilisateur racine est propriétaire du compte AWS et il est autorisé à faire ce qu’il souhaite dans ce compte. C'est comme être le propriétaire du café. 
> 
> Dans cette situation, disons que je suis le propriétaire du café. Je peux entrer dans le magasin, utiliser les identifiants pour utiliser la caisse, gérer le système d’inventaire ou tout autre système du café. Je ne peux pas être limité. Comme utilisateur racine AWS, vous pouvez accéder et contrôler n'importe quelle ressource dans le compte. Vous pouvez lancer des bases de données, des instances EC2, des services de Blockchain ou faire littéralement de que vous souhaitez. Comme cet utilisateur est si puissant, nous recommandons que, dès que vous créez un compte AWS, et vous vous connectez à votre utilisateur racine, vous activez authentification multifacteur (MFA), pour vous assurer d’avoir non seulement besoin de l’e-mail et du mot de passe, mais aussi d'un jeton aléatoire pour vous connecter. 
> 
> C'est génial. Mais même avec la MFA activée, il ne faut pas utiliser l’utilisateur racine pour tout. En tant que propriétaire du café, je ne donne pas mon niveau d'accès à tous les employés. Rudy à la caisse enregistreuse ne peut pas accéder au système de stocks, vous vous souvenez ? Vous contrôlez l'accès de manière détaillé à l’aide du service AWS, AWS Identity and Access Management (IAM). 
> 
> Dans IAM, vous pouvez créer des utilisateurs IAM. Lorsque vous créez un utilisateur IAM, par défaut, il n'a pas d'autorisations. L’utilisateur ne peut même pas se connecter au compte AWS au départ, il n'a absolument aucune autorisation. Il ne peut pas lancer d'instance EC2. Il ne peut pas créer de compartiment S3. Rien. Vous devez donner explicitement l’autorisation à l’utilisateur pour faire quoi que ce soit sur ce compte. Rappelez-vous, par défaut, toutes les actions sont refusées. Vous devez autoriser explicitement toute action effectuée par un utilisateur. Vous donnez uniquement accès aux personnes pour ce dont ils ont besoin et rien d'autre. Cette idée est appelée principe du moindre privilège. 
> 
> La façon dont vous accordez ou refusez l'autorisation sert à associer ce qu’on appelle une politique IAM à un utilisateur IAM. Une politique IAM est un document JSON qui décrit les appels d'API qu'un utilisateur peut ou ne peut pas faire. Regardons cet exemple rapide. Dans cet exemple, vous pouvez voir que nous avons une déclaration d'autorisation qui a l’effet « Autoriser » l’action comme s3:Listbucket. Et la ressource est un ID unique pour un compartiment S3. Donc, si j'attache cette politique à un utilisateur et cet utilisateur peut voir le compartiment « coffee_shop_reports » mais réaliser aucune autre action sur ce compte. Il n'y a que deux options potentielles pour l’effet de toute politique. Autoriser ou refuser. Pour une action, vous pouvez répertorier n'importe quel appel d'API AWS et pour une ressource, vous devez répertorier la ressource AWS correspondant à cet appel d’API spécifique. En tant qu’entreprise, vous n'auriez pas besoin d'écrire ces politiques, mais elles sont utilisées partout sur vos comptes AWS. 
> 
> Une façon de faciliter la gestion de vos utilisateurs et de leurs autorisations est de les organiser en groupes IAM. Les groupes sont des regroupements d'utilisateurs. Vous pouvez attacher une politique à un groupe, et tous les utilisateurs de ce groupe se voient octroyer les autorisations. Si vous avez plusieurs caissiers dans le café, au lieu de leur accorder un accès individuel à tous à la caisse : vous pouvez accorder l'accès à tous les caissiers puis ajouter chaque caissier individuel dans ce groupe. Même idée avec les groupes dans IAM. 
> 
> Jusqu'à présent avec IAM, vous avez l'utilisateur racine, qui peut tout faire. Vous avez des utilisateurs qui peuvent être organisés en groupes. Et vous avez aussi les politiques qui sont des documents qui décrivent les autorisations que vous pouvez alors attacher aux utilisateurs ou aux groupes. Il y a une autre identité majeure dans IAM, qui s’appelle un rôle. 
> 
> Pour comprendre le concept de rôle, pensons au café. Comme nous le savons, Blaine travaille dans la boutique et selon le personnel présent dans le café, il peut s’occuper de la caisse ou du système de stocks ou il pourrait faire le nettoyage en fin de journée sans accès à aucun système. J'ai, en tant que propriétaire, l'autorité pour attribuer ces différents rôles à Blaine. Ses responsabilités et l'accès sont variables et changent selon le jour. Juste parce qu'il travaillait sur le suivi des stocks dans le système hier, ne signifie pas qu’il doit le faire tout le temps. Son rôle au travail change et est de nature temporaire. Le même type d'idée existe dans AWS. Vous pouvez créer des identités dans AWS qui sont appelées rôles. 
> 
> Les rôles sont associés à des autorisations qui permettent ou refusent des actions spécifiques. Et ces rôles peuvent être assumés pour des périodes de temps temporaires. Il est similaire à un utilisateur, mais n'a ni nom d'utilisateur ni mot de passe. À la place, il est une identité que vous pouvez endosser pour obtenir un accès temporaire aux autorisations. Vous utilisez des rôles pour accorder temporairement l'accès aux ressources AWS, à des utilisateurs, des identités extérieures, des applications, et même d’autres services AWS. Lorsqu'une identité assume un rôle, elle abandonne toutes les autorisations précédentes qu’elle possède et assume les autorisations de ce rôle. 
> 
> Vous pouvez éviter de créer des utilisateurs IAM pour chaque personne de votre organisation en fédérant les utilisateurs dans votre compte. Cela signifie qu’ils pourraient utiliser leurs informations d'identification habituelles pour se connecter à AWS en mappant leurs identités d’entreprise aux rôles IAM. Authentification et autorisation AWS IAM en tant que service.
</div>

## AWS Identity and Access Management (IAM)

**[AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/)** vous permet de gérer, de façon sécurisée, l’accès aux services et ressources AWS.   

![aws_iam_01.png](/assets/img/clouds/tempo/aws_iam_01.png)

IAM vous offre la flexibilité nécessaire pour configurer l’accès en fonction des besoins opérationnels et de sécurité spécifiques de votre entreprise. Pour ce faire, combinez les fonctions IAM, qui sont expliquées en détail dans cette leçon :

- Utilisateurs, groupes et rôles IAM
- Stratégies IAM
- Authentification multifacteur

Vous découvrirez également les bonnes pratiques pour chacune de ces fonctions.

## Utilisateur racine d’un compte AWS

Lorsque vous créez un compte AWS pour la première fois, commencez par une identité connue sous le nom **[d’utilisateur racine](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)**. 

L’utilisateur racine est accessible après connexion à l’aide de l’adresse e-mail et du mot de passe utilisés pour la création du compte AWS. Un utilisateur racine est semblable au propriétaire du café. Il dispose d’un accès complet à tous les services et ressources AWS du compte.

![aws_security_01.png](/assets/img/clouds/tempo/aws_security_01.png =750x)

> **Bonne pratique** :
> 
> n’utilisez **pas** l’utilisateur racine pour les tâches quotidiennes. 
> 
> Utilisez plutôt l’utilisateur racine pour créer votre premier utilisateur IAM et lui attribuer des autorisations pour créer d’autres utilisateurs.
> 
> Ensuite, continuez à créer d’autres utilisateurs IAM et accédez à ces identités pour effectuer des tâches régulières dans AWS. N’utilisez l’utilisateur racine que lorsque vous devez effectuer un nombre limité de tâches qui ne sont disponibles que pour l’utilisateur racine. Parmi ces tâches, mentionnons la modification de l’adresse e-mail de votre utilisateur racine et la modification de votre formule de support AWS.
{.is-info}

## Utilisateurs IAM

Un **utilisateur racine** est une identité que vous créez dans AWS. Il représente la personne ou l’application qui interagit avec les services et ressources AWS. Il se compose d’un nom et d’informations d’identification.

Par défaut, lorsque vous créez un nouvel utilisateur IAM dans AWS, aucune autorisation ne lui est associée. Pour permettre à l’utilisateur IAM d’effectuer des actions spécifiques dans AWS, comme lancer une instance Amazon EC2 ou créer un compartiment Amazon S3, vous devez accorder à l’utilisateur IAM les autorisations nécessaires.

> **Bonne pratique** :
> 
> nous vous conseillons de créer des utilisateurs IAM individuels pour chaque personne ayant besoin d’accéder à AWS.  
> 
> Même si plusieurs employés ont besoin du même niveau d’accès, vous devez créer des utilisateurs IAM individuels pour chacun d’eux. Cela offre une sécurité supplémentaire en permettant à chaque utilisateur IAM d’avoir un ensemble unique d’informations d’identification de sécurité.
{.is-info}

## Politiques IAM

Une **politique IAM** est un document qui autorise ou refuse des autorisations aux services et ressources AWS.  

Les politiques IAM vous permettent de personnaliser les niveaux d’accès des utilisateurs aux ressources. Par exemple, vous pouvez autoriser les utilisateurs à accéder à tous les compartiments Amazon S3 de votre compte AWS, ou uniquement à un compartiment spécifique.

> **Bonne pratique** :
> 
> suivez le principe de sécurité du **moindre privilège** lors de l’octroi des autorisations. 
> 
> En suivant ce principe, vous empêchez les utilisateurs ou les rôles d’avoir plus d’autorisations que nécessaire pour effectuer leurs tâches. 
> 
> Par exemple, si un employé n’a besoin d’accéder qu’à un compartiment spécifique, spécifiez le compartiment dans la politique IAM. Faites cela au lieu d’accorder à l’employé l’accès à tous les compartiments de votre compte AWS.
{.is-info}

## Exemple : politique IAM

Voici un exemple de fonctionnement des politiques IAM. Supposons que le propriétaire du café ait créé un utilisateur IAM pour un caissier qui vient d’être engagé. Le caissier doit avoir accès aux reçus conservés dans un compartiment Amazon S3 possédant l’ID : AWSDOC-EXAMPLE-BUCKET.

![aws_security_02.png](/assets/img/clouds/tempo/aws_security_02.png =300x){.align-left}

*Cet exemple de politique IAM autorise l’accès aux objets dans le compartiment Amazon S3 possédant l’ID : AWSDOC-EXAMPLE-BUCKET.*

Dans cet exemple, la politique IAM autorise une action spécifique dans Amazon S3 : ListObject. La politique mentionne également un ID de compartiment spécifique : AWSDOC-EXAMPLE-BUCKET. Lorsque le propriétaire associe cette politique à l’utilisateur IAM du caissier, cela permet au caissier d’afficher tous les objets du compartiment AWSDOC-EXAMPLE-BUCKET. 

Si le propriétaire souhaite que le caissier puisse accéder à d’autres services et effectuer d’autres actions dans AWS, il doit joindre des politiques supplémentaires pour spécifier ces services et actions.

Maintenant, supposons que le café ait engagé plusieurs caissiers. Au lieu d’attribuer des autorisations à chaque utilisateur IAM individuel, le propriétaire place les utilisateurs dans un **[groupe IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)**.

## Groupes IAM

Un groupe IAM est un ensemble d’utilisateurs IAM. Lorsque vous attribuez une politique IAM à un groupe, tous les utilisateurs du groupe bénéficient des autorisations spécifiées par la politique.

Voici un exemple de la façon dont cela pourrait fonctionner pour ce café. Au lieu d’attribuer des autorisations aux caissiers une à la fois, le propriétaire peut créer un groupe IAM « Caissiers ». Le propriétaire peut ensuite ajouter des utilisateurs IAM au groupe, puis joindre des autorisations au niveau du groupe.

![aws_security_03.png](/assets/img/clouds/tempo/aws_security_03.png =500x)

L’attribution de politiques IAM au niveau du groupe facilite également l’ajustement des autorisations lorsqu’un employé est transféré vers une autre tâche. Par exemple, si un caissier devient un spécialiste du stock, le propriétaire du café le retire du groupe IAM « Caissiers » et l’ajoute au groupe IAM « Spécialistes du stock ». Cela garantit que les employés disposent uniquement des autorisations requises pour leur rôle actuel.

Que se passe-t-il si un employé du café n’a pas changé d’emploi de façon permanente, mais qu’il occupe différents postes de travail tout au long de la journée ? Cet employé peut obtenir l’accès dont il a besoin par le biais des **[rôles IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)**.

## Rôles IAM

Dans le café, un employé occupe différents postes de travail tout au long de la journée. Selon le personnel présent dans le café, cet employé peut accomplir plusieurs tâches : travailler à la caisse enregistreuse, mettre à jour le système de stock, traiter les commandes en ligne, etc. 

Lorsque l’employé doit effectuer une autre tâche, il abandonne son accès à un poste de travail pour accéder au poste de travail suivant. L’employé peut facilement basculer d’un poste de travail à l’autre, mais il ne peut avoir accès qu’à un seul poste de travail à un moment précis. Ce même concept existe dans AWS avec les rôles IAM.

Un rôle IAM est une identité que vous pouvez endosser pour obtenir un accès temporaire aux autorisations.  

Avant qu’un utilisateur, une application ou un service IAM puisse endosser un rôle IAM, il doit se voir accorder des autorisations pour basculer vers ce rôle. Lorsqu’une personne endosse un rôle IAM, cette personne abandonne toutes les autorisations précédentes qu’elle possédait sous un rôle antérieur et endosse les autorisations du nouveau rôle.

> **Bonne pratique** :
> 
> les rôles IAM conviennent parfaitement lorsque l’accès aux services ou aux ressources doit être accordé temporairement, plutôt qu’à long terme.
{.is-info}

## Exemple : rôles IAM
Pour consulter un exemple de la façon dont les rôles IAM peuvent être utilisés dans le café, sélectionnez COMMENCER.

#### tabs {.tabset}
##### Etape 1

Icône d’autorisation qui comprend deux coches et un « X »

![aws_security_04.png](/assets/img/clouds/tempo/aws_security_04.png =150x)

Tout d’abord, le propriétaire accorde à l’employé les autorisations de basculer vers un rôle pour chaque poste de travail dans le café.

##### Etape 2

L’icône utilisateur IAM connecté au rôle « Caissier » (représenté par une icône de casque de chantier)

![aws_security_05.png](/assets/img/clouds/tempo/aws_security_05.png =x300)

L’employé commence sa journée en endossant le rôle de « caissier ». Cela lui permet d’accéder à la caisse enregistreuse.

Icône utilisateur IAM connecté au rôle « Inventaire » et qui n’est plus connecté au rôle « Caisse »

![aws_security_06.png](/assets/img/clouds/tempo/aws_security_06.png =300x)

Plus tard dans la journée, l’employé doit mettre à jour le système de stock. Il endosse le rôle « Stock ». 

Cela autorise l’accès de l’employé au système de stock et révoque son accès au système de caisse enregistreuse.

## Authentification multifacteur

Vous êtes-vous déjà connecté à un site web qui vous demandait de fournir plusieurs informations pour vérifier votre identité ? Vous pourriez avoir besoin de fournir votre mot de passe, puis une deuxième forme d’authentification, par exemple un code aléatoire envoyé à votre téléphone. Il s’agit d’un exemple de **[l’authentification multifacteur](https://aws.amazon.com/iam/features/mfa/)**.

Dans IAM, l’authentification multifacteur (MFA, Multi-Factor Authentication) fournit une couche supplémentaire de sécurité pour votre compte AWS.

## Fonctionnement de l’authentification multifacteur (MFA)
Pour passer en revue les étapes liées à l’authentification multifacteur, sélectionnez COMMENCER.

#### tabs {.tabset}
##### Etape 1

Exemple de zones de texte de formulaire pour saisir le nom d’utilisateur et le mot de passe

![aws_security_07.png](/assets/img/clouds/tempo/aws_security_07.png =300x)

Tout d’abord, lorsqu’un utilisateur se connecte à un site web AWS, il saisit son ID utilisateur et son mot de passe IAM.

##### Etape 2

Illustration d’un jeton matériel

![aws_security_08.png](/assets/img/clouds/tempo/aws_security_08.png =200x)

Ensuite, l’utilisateur est invité à fournir une réponse d’authentification à partir de son dispositif AWS MFA. Il peut s’agir d’une clé de sécurité matérielle, d’un périphérique matériel ou d’une application MFA sur un appareil comme un smartphone.


Icône « ordinateur » avec une icône de pouce vers le haut sur l’écran

![aws_security_09.png](/assets/img/clouds/tempo/aws_security_09.png =200x)

Lorsque l’utilisateur s’est authentifié, il peut accéder aux services ou ressources AWS demandés.

Vous pouvez activer la MFA pour l’utilisateur racine et les utilisateurs IAM. Il est recommandé d’activer MFA pour l’utilisateur racine et tous les utilisateurs IAM de votre compte. Ainsi, vous protégez votre compte AWS contre tout accès non autorisé.

# AWS Organizations

```
video : xxx.mkv
```
*Figure. AWS Organizations*

<div class="collapseContent" display="Transcription" value="true">


> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Avec vos premiers pas dans le cloud AWS, vous commencerez probablement avec un compte AWS où vous mettrez tout. La plupart des gens commencent de cette façon, mais à mesure que votre entreprise grandit ou même commence sa transition vers le cloud, il est important d’avoir une séparation des tâches. Par exemple, vous voulez que vos développeurs aient accès aux ressources de développement, que votre personnel comptable puisse accéder aux informations de facturation, ou même avoir des unités commerciales séparées afin qu’elles puissent expérimenter avec les services AWS sans s’impacter mutuellement. Vous commencez donc à ajouter plus de cartes pour chaque personne, selon les besoins d’intégration de chacun. Et en un rien de temps, vous vous retrouvez avec un méli-mélo de compte AWS, pas aussi agréable que vous pourriez l’imaginer. 
> 
> Par exemple, vous aurez à garder une trace du compte A, F et G, ou peut-être que le compte B a les mauvaises autorisations et le compte C a les informations de facturation et de conformité. Une façon d'installer l'ordre et de faire appliquer qui est autorisé pour effectuer certaines fonctions dans quel compte consiste à utiliser un service AWS appelé AWS Organizations. 
> 
> La façon la plus simple d'imaginer Organizations est comme un emplacement central pour gérer plusieurs comptes AWS. Vous pouvez gérer le contrôle de facturation, l’accès, la conformité, la sécurité, et partager les ressources entre vos comptes AWS. Nous allons décrire certaines des principales caractéristiques d’AWS Organizations, d’accord ? 
> 
> La première est la gestion centralisée de tous les comptes AWS. Pensez à tous ces comptes AWS, nous avions : A, B, C, F et G. Maintenant, vous pouvez les combiner dans une organisation qui vous permet de gérer les comptes centraux. Nous avons trouvé les comptes D et E dans le processus. Ensuite, la facturation consolidée pour tous les comptes membres. Cela signifie que vous pouvez utiliser le compte principal de votre organisation pour consolider et payer tous les comptes membres. Un autre avantage de la facturation consolidée : les remises sur quantité. De l'argent liquide. 
> 
> La fonctionnalité suivante vous permet d’implémenter des groupements hiérarchiques de vos comptes pour répondre aux besoins de sécurité, conformité ou budget. Cela signifie que vous pouvez regrouper les comptes en unités d’organisation (OU), similaires à des unités organisationnelles (BU). Par exemple, si vous avez des comptes qui ne peuvent accéder qu'aux services AWS qui répondent à certaines exigences réglementaires, vous pouvez placer ces comptes dans une unité d'organisation, ou si vous avez des comptes qui relèvent de l’unité d’organisation du développeur, vous pouvez les regrouper en conséquence. 
> 
> L’une des dernières fonctionnalités principales que nous aborderons, c’est que vous avez le contrôle des services AWS et les actions API auxquelles chaque compte peut accéder en tant qu’administrateur du compte principal d’une organisation. Vous pouvez utiliser ce qu’on appelle les stratégies de contrôle des services, (SCP) pour indiquer le nombre maximum d’autorisations pour les comptes membres de l’organisation. Les SCP vous permettent d'imposer des restrictions sur les services AWS et les ressources et sur les actions API individuelles auxquelles les utilisateurs et les rôles de chaque compte membre peuvent accéder.
</div>

## AWS Organizations

Supposons que votre entreprise dispose de plusieurs comptes AWS. Vous pouvez utiliser **[AWS Organizations](https://aws.amazon.com/organizations)** pour consolider et gérer plusieurs comptes AWS au sein d’un emplacement central.

![aws_organizations_01.png](/assets/img/clouds/tempo/aws_organizations_01.png)

Lorsque vous créez une organisation, AWS Organizations crée automatiquement une racine, qui est le conteneur parent de tous les comptes de votre organisation. 

Dans AWS Organizations, vous pouvez contrôler de manière centralisée les autorisations pour les comptes de votre organisation à l’aide de **[stratégies de contrôle des services (SCP)](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)**. Les SCP vous permettent d’imposer des restrictions sur les services AWS, les ressources et les actions d’API individuelles auxquelles les utilisateurs et les rôles de chaque compte peuvent accéder.

> La facturation consolidée est une autre fonction d’AWS Organizations. Vous en apprendrez plus sur la facturation consolidée dans un prochain module.
{.is-info}

## Unités organisationnelles

Dans AWS Organizations, vous pouvez regrouper des comptes en unités organisationnelles (OU) pour faciliter la gestion des comptes ayant des exigences commerciales ou de sécurité similaires. Lorsque vous appliquez une stratégie à une unité organisationnelle, tous les comptes de l’unité organisationnelle héritent automatiquement des autorisations spécifiées dans la stratégie.  

En organisant les comptes distincts en UO, vous pouvez isoler plus facilement les charges de travail ou les applications possédant des exigences de sécurité spécifiques. Par exemple, si votre entreprise dispose de comptes qui ne peuvent accéder qu’aux services AWS qui répondent à certaines exigences réglementaires, vous pouvez placer ces comptes dans une unité organisationnelle. Ensuite, vous pouvez joindre une stratégie à l’unité organisationnelle qui bloque l’accès à tous les autres services AWS qui ne répondent pas aux exigences réglementaires.

## Exemple : AWS Organizations
Pour consulter un exemple de la façon dont une entreprise peut utiliser AWS Organizations, sélectionnez COMMENCER.

#### tabs {.tabset}
##### Etape 1

Diagramme organisationnel d’un compte racine et de quatre comptes AWS : Finance, Informatique, RH et Juridique

![aws_organizations_02.png](/assets/img/clouds/tempo/aws_organizations_02.png =500x)

Imaginez que votre entreprise dispose de comptes AWS distincts pour les services Finance, Informatique (IT), Ressources humaines (RH) et Juridique. Vous décidez de consolider ces comptes dans une seule organisation afin de pouvoir les administrer à partir d’un emplacement central. Lorsque vous créez l’organisation, cela établit la racine.

Lors de la conception de votre organisation, tenez compte des besoins commerciaux, de sécurité et réglementaires de chaque service. Vous utiliserez ces informations pour décider des services a regrouper dans les unités d’organisation.

##### Etape 2

Diagramme montrant les comptes des services Finance et Informatique placés directement sous la racine

![aws_organizations_03.png](/assets/img/clouds/tempo/aws_organizations_03.png =500x)

Les services financiers et informatiques ont des exigences qui ne se chevauchent pas avec celles d’un autre service. Vous importez ces comptes dans votre organisation pour profiter d’avantages tels que la facturation consolidée, mais vous ne les placez pas dans une unité organisationnelle.

##### Etape 3

Diagramme montrant une unité organisationnelle placée sous la racine. L’unité organisationnelle contient les comptes HR et juridiques.

![aws_organizations_04.png](/assets/img/clouds/tempo/aws_organizations_04.png =500x)

Les services RH et juridiques doivent accéder aux mêmes services et ressources AWS. Vous devez donc les placer ensemble dans une même unité organisationnelle. En les plaçant dans une même unité organisationnelle, vous pouvez associer les stratégies qui s’appliquent aux comptes AWS des services RH et juridiques.

Même si vous avez placé ces comptes dans des UO, vous pouvez continuer à fournir l’accès aux utilisateurs, aux groupes et aux rôles via IAM.

En regroupant vos comptes en UO, vous pouvez leur donner plus facilement accès aux services et aux ressources dont ils ont besoin. Vous les empêchez également d’accéder aux services ou ressources dont ils n’ont pas besoin.

## Contrôle des connaissances

**Vous configurez des stratégies de contrôle des services (SCP) dans AWS Organizations. À quelles identités et ressources les SCP peuvent-elles être appliquées ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q1r1" name="question1" value="1">
  <label for="q1r1">Les utilisateurs IAM</label><br>
  <input type="checkbox" id="q1r2" name="question1" value="2">
  <label for="q1r2">Les groupes IAM</label><br>
  <input type="checkbox" id="q1r3" name="question1" value="3">
  <label for="q1r3">Un compte de membre individuel</label><br>
  <input type="checkbox" id="q1r4" name="question1" value="4">
  <label for="q1r4">Les rôles IAM</label><br>
  <input type="checkbox" id="q1r5" name="question1" value="5">
  <label for="q1r5">Une unité d’organisation (OU)</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Un compte de membre individuel**
- **Une unité organisationnelle (OU)**

Dans AWS Organizations, vous pouvez appliquer des stratégies de contrôle des services (SCP) à la racine de l’organisation, à un compte de membre individuel ou à une OU. Une SCP affecte tous les utilisateurs, groupes et rôles IAM au sein d’un compte, y compris l’utilisateur racine du compte AWS.

Vous pouvez appliquer des politiques IAM aux utilisateurs, groupes ou rôles IAM. Vous ne pouvez pas appliquer une politique IAM à l’utilisateur racine du compte AWS.

En savoir plus :

- [AWS Organizations](https://aws.amazon.com/organizations)
- [Stratégies de contrôle des services](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)
- [Attacher des SCP](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps_attach.html)
</div>

# Conformité

```
video : xxx.mkv
```
*Figure. Compliance*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Il existe des normes spécifiques à chaque secteur d’activités que vous devez appliquer, et vous ferez l’objet d’un audit ou d’une inspection pour vérifier que vous avez respecté ces normes. Par exemple, pour un café, les services d’hygiène viendront vérifier que tout est conforme aux règles et à aux normes sanitaires. De même, vous pourriez être audité pour les taxes, pour contrôler que vous avez exécuté le back-office correctement et avez appliqué la loi. Vous vous appuyez sur la documentation, les rapports et les inspections pour passer les audits et les vérifications de conformité à mesure qu’ils surviennent. 
> 
> Vous devrez concevoir une façon similaire de respecter la conformité et l’audit dans AWS. Selon les types de solutions que vous hébergez sur AWS, vous devez vous assurer que vous êtes conforme aux normes et réglementations spécialement imposées à votre entreprise. Si vous exécutez un logiciel qui traite de données sur les consommateurs dans l'UE, vous devez vérifiez que vous respectez le RGPD, ou si vous exécutez des applications de soins de santé aux États-Unis, vous devez concevoir vos architectures pour répondre aux exigences de conformité HIPAA. 
> 
> Quels que soient vos besoins de conformité, vous aurez besoin de certains outils pour pouvoir recueillir des documents, des dossiers et inspecter votre environnement AWS pour vérifier si vous répondez aux réglementations de conformité applicables. La première chose à noter est qu’AWS a déjà construit une infrastructure de centre de données et réseaux selon les bonnes pratiques du secteur pour la sécurité, et en tant que client AWS, vous héritez de toutes les bonnes pratiques en matière de politiques AWS, d’architecture et de processus opérationnels.
> 
> AWS est conforme à une liste de programmes d'assurance disponible en ligne. Cela signifie que certains aspects de votre conformité sont déjà remplis, et vous pouvez vous concentrer sur la conformité au sein de vos propres architectures que vous construisez sur AWS. Ce qu’il faut également savoir, en ce qui concerne la conformité et AWS, c’est que la région à partir de laquelle vous choisissez d’opérer, peut vous aider à répondre aux réglementations de conformité. Si vous ne pouvez stocker légalement des données que dans leur pays d’origine, vous pouvez choisir une région qui a du sens pour vous et AWS ne répliquera pas automatiquement les données entre régions. 
> 
> Vous devriez également être conscient du fait que vous êtes propriétaire de vos données dans AWS. Comme démontré dans modèle de responsabilité partagée AWS, vous avez un contrôle total sur les données que vous stockez dans AWS. Vous pouvez employer plusieurs mécanismes de chiffrement différents pour garantir la sécurité de vos données, et qui varient d’un service à l’autre. Donc, si vous avez besoin de normes spécifiques pour le stockage des données, vous pouvez concevoir un moyen d’atteindre ces exigences en le construisant vous-même sur AWS ou en utilisant les fonctionnalités qui existent déjà dans de nombreux services. Pour beaucoup de services, l'activation de la protection des données est un paramètre de configuration sur la ressource. 
> 
> AWS offre également plusieurs livres blancs et documents que vous pouvez télécharger et utiliser pour les rapports de conformité. Puisque vous n'exécutez pas le centre de données vous-même, vous pouvez essentiellement demander à AWS de vous fournir une documentation qui prouve l’application des bonnes pratiques de sécurité et conformité. 
> 
> Vous pouvez accéder à ces documents via un service appelé AWS Artifact. Avec AWS Artifact, vous pouvez obtenir un accès aux rapports de conformité effectués par des tiers qui ont validé une large gamme de normes de conformité. Consultez le centre de conformité AWS pour trouver toutes les informations sur la conformité au même endroit. Il vous dirigera vers les services d'activation de conformité ainsi que la documentation comme le livre blanc AWS risque et sécurité, que vous devriez lire pour vous assurer que vous comprenez la sécurité et la conformité avec AWS. 
> 
> Pour savoir si vous êtes conforme dans AWS, n’oubliez pas que nous suivons une responsabilité partagée. La plateforme sous-jacente est sécurisée et AWS peut fournir la documentation sur les types d’exigences de conformité qu’ils rencontrent, via des services tels qu’AWS Artifact et les livres blancs. Mais, au-delà de cela, ce que vous construisez sur AWS dépend de vous. Vous contrôlez l'architecture de vos applications et les solutions que vous construisez, et elles doivent être construites avec la conformité, la sécurité et le modèle de responsabilité partagée à l’esprit.
</div>

## AWS Artifact

Selon le secteur d’activité de votre entreprise, vous devrez peut-être respecter des normes spécifiques. Un audit ou une inspection permettra de garantir le respect de ces normes par l’entreprise.

**[AWS Artifact](https://aws.amazon.com/artifact)** est un service qui fournit un accès à la demande aux rapports de sécurité et de conformité AWS et à certains contrats en ligne. AWS Artifact comprend deux sections principales : AWS Artifact Agreements et AWS Artifact Reports.

![aws_organizations_05.png](/assets/img/clouds/tempo/aws_organizations_05.png)

Pour en savoir plus, sélectionnez le symbole + en regard de chaque section.

<div class="collapseContent" display="AWS Artifact Agreements" value="true">

Supposons que votre entreprise doive signer un accord avec AWS concernant votre utilisation de certains types d’informations dans l’ensemble des services AWS. Vous pouvez le faire par le biais d’**AWS Artifact Agreements**. 

Dans AWS Artifact Agreements, vous pouvez consulter, accepter et gérer des contrats pour un compte individuel et pour tous vos comptes dans AWS Organizations. Différents types d’accords sont proposés pour répondre aux besoins des clients soumis à des réglementations spécifiques, comme le Health Insurance Portability and Accountability Act (HIPAA).
</div>

<div class="collapseContent" display="AWS Artifact Reports" value="true">

Supposons ensuite qu’un membre de l’équipe de développement de votre entreprise conçoive une application et qu’il ait besoin de plus d’informations sur sa responsabilité de se conformer à certaines normes réglementaires. Vous pouvez lui conseiller d’accéder à ces informations dans **AWS Artifact Reports**.

AWS Artifact Reports fournit des rapports de conformité d’auditeurs tiers. Ces auditeurs ont testé et vérifié qu’AWS est conforme à diverses normes et réglementations de sécurité mondiales, régionales et spécifiques à l’industrie. AWS Artifact Reports reste à jour avec les derniers rapports publiés. Vous pouvez fournir les artefacts d’audit AWS à vos auditeurs ou régulateurs comme preuves des contrôles de sécurité AWS.
</div>

Voici quelques-uns des rapports et règlements de conformité que vous pouvez trouver dans AWS Artifact. Chaque rapport comprend une description de son contenu et de la période de rapport pour laquelle le document est valide.

![aws_organizations_06.jpg](/assets/img/clouds/tempo/aws_organizations_06.jpg)

## Centre de conformité client

Le **[centre de conformité client](https://aws.amazon.com/compliance/customer-center/)** comprend des ressources pour vous aider à en savoir plus sur la conformité AWS. 

Le centre de conformité client inclut des témoignages de clients inhérents à la conformité qui vous permettent de découvrir comment les entreprises des secteurs réglementés ont résolu divers défis en matière de conformité, de gouvernance et d’audit.

Vous pouvez également accéder aux livres blancs et à la documentation sur la conformité portant sur des sujets tels que :

- Les réponses AWS aux principales questions de conformité
- Une présentation des risques et de la conformité AWS
- Une liste de contrôle de la sécurité d’audit

En outre, le centre de conformité client inclut un chemin d’apprentissage de l’auditeur. Ce chemin d’apprentissage est conçu pour les personnes exerçant des fonctions d’audit, de conformité et juridiques qui souhaitent en savoir plus sur la façon dont leurs opérations internes peuvent démontrer la conformité à l’aide d’AWS Cloud.

## Contrôle des connaissances

**Quelles tâches pouvez-vous effectuer dans AWS Artifact ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q2r1" name="question2" value="1">
  <label for="q2r1">Accéder aux rapports de conformité AWS à la demande.</label><br>
  <input type="checkbox" id="q2r2" name="question2" value="2">
  <label for="q2r2">Consolider et gérer plusieurs comptes AWS au sein d’un emplacement central.</label><br>
  <input type="checkbox" id="q2r3" name="question2" value="3">
  <label for="q2r3">Créer des utilisateurs pour permettre aux personnes et aux applications d’interagir avec les services et ressources AWS.</label><br>
  <input type="checkbox" id="q2r4" name="question2" value="4">
  <label for="q2r4">Définir les autorisations pour les comptes en configurant des stratégies de contrôle des services (SCP).</label><br>
  <input type="checkbox" id="q2r5" name="question2" value="5">
  <label for="q2r5">Consulter, accepter et gérer des accords avec AWS.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Accéder aux rapports de conformité AWS à la demande.**
- **Consulter, accepter et gérer des accords avec AWS.**
  
Les autres réponses sont incorrectes, car :

- Consolider et gérer plusieurs comptes AWS au sein d’un emplacement central est une tâche qui peut être effectuée dans AWS Organizations.
- Créer des utilisateurs pour permettre aux personnes et aux applications d’interagir avec les services et ressources AWS est une tâche qui peut être effectuée dans AWS Identity and Access Management (IAM).
- Définir les autorisations pour les comptes en configurant des stratégies de contrôle des services (SCP) est une tâche qui peut être effectuée dans AWS Organizations.

En savoir plus :

- [AWS Artifact](https://aws.amazon.com/artifact)
</div>

# Attaques par déni de service

```
video : xxx.mkv
```
*Figure. Denial-of-service attacks*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> D-D-o-S, DDoS, soit le déni de service distribué. Il s'agit d'une attaque contre l'infrastructure de votre entreprise, et vous en avez déjà entendu parler. Votre équipe de sécurité a certainement élaboré un plan pour ce type d'attaque qui, vous le savez, a dévasté de nombreuses entreprises. En quoi consiste exactement cette attaque, et plus important encore, comment pouvez-vous vous en défendre ? 
> 
> Pour être honnête, je devrais en parler pendant 14 heures afin que vous compreniez chaque aspect, mais vous devez au moins avoir connaissance des notions fondamentales concernant l’exécution de ces attaques, et la façon dont AWS peut automatiquement défendre votre infrastructure de ces attaques paralysantes. Nous n’avons pas beaucoup de temps pour en parler, le compte à rebours commence maintenant. L’objectif d’une attaque DDoS est d’entraver le fonctionnement de votre application en surchargeant le système au point qu’il ne peut plus fonctionner. 
> 
> En temps normal, votre application reçoit des requêtes des clients et renvoie des résultats. Dans une attaque DDoS, la personne malveillante tente de surcharger la capacité de votre application, essentiellement afin de refuser tout accès à vos services. Toutefois, il n'y a aucune chance que l'attaque soit aboutie si celle-ci est menée par une seule et unique machine. Ainsi, l’aspect distribué repose sur le fait que l’attaque exploite d’autres machines sur Internet afin d’attaquer votre infrastructure à son insu. La personne malveillante crée une armée de robots-zombies qui attaque absurdement votre entreprise. La clé d’une bonne attaque, et j’emploie le terme « bonne » alors que je devrais dire « puissante ». C’est vraiment un mal chaotique, mais la clé est que la personne responsable de l’attaque exécute la plus petite quantité de travail nécessaire et que la victime ciblée reçoive, à la suite de l’attaque, une charge de travail insupportable à traiter. 
> 
> Voici quelques exemples d'attaques spécifiques qui fonctionnent parfaitement. Le flux UDP. sur les composants utiles d'Internet, comme le service de météo national. Tout le monde peut envoyer une requête au service de météo, pour demander quelle es la météo, et en retour, la flotte de machines du service météo renvoie une quantité considérable de données de télémesure météorologique, des prévisions, des mises à jour, beaucoup de choses. L'attaque est simple. La personne malveillante envoie une simple requête : « Quelle est la météo ? » Mais elle indique une fausse adresse de retour sur la demande, votre adresse de retour. Le service météorologique inonde maintenant avec plaisir votre serveur avec des mégaoctets de prévisions de pluie, et votre système pourrait être paralysé, en triant les informations qu’il n’a jamais demandées. Il s’agit d’un exemple parmi une demi-douzaine d’attaques en force de faible niveau, toutes conçues pour épuiser votre réseau. 
> 
> Certaines attaques sont plus sophistiquées, comme les attaques au niveau HTTP, qui ressemblent à des clients normaux demandant des choses normales, comme des recherches de produits complexes à maintes et maintes reprises, appelant tous d’une armée de robots zombifiés. Ils demandent tant d’attention que les autres clients ne peuvent pas entrer. 
> 
> Ils essaient même d'horribles tours, comme l’attaque Slowloris. Mm-hmm. Imaginez-vous faire la queue dans un café, et le client devant vous prend sept minutes à commander ce qu’il commande, et vous ne pouvez pas commander avant qu’il ne s’en aille et vous laisse commander. L'attaque Slowloris, c’est exactement la même chose. Au lieu d’utiliser une connexion normale, la personne malveillante passe une commande et fait semblant que sa connexion est terriblement lente. Vous avez compris la scène. Pendant ce temps, vos serveurs de production attendent que le client termine sa demande et ils peuvent renvoyer le résultat. Mais tant qu'ils n'ont pas reçu la totalité du paquet, ils ne peuvent pas passer au suivant, au client suivant. Il suffit de quelques attaqueurs Slowloris pour épuiser la capacité totale de votre frontend sans presque aucun effort. Je pourrais encore parler pendant des heures de l’architecture malveillante élégamment conçue de ces attaques, mais le temps est compté, et il est temps de mettre fin à ces attaques Voici la solution, que vous connaissez déjà. 
> 
> Tout ce dont nous avons parlé depuis le début de ce cours est non seulement une bonne architecture mais cela permet également de résoudre presque tous les vecteurs des attaques DDoS sans coût ni effort supplémentaire. Parlons d'abord des attaques réseau de faible niveau comme les flux UDP. La solution ? Les groupes de sécurité. Les groupes de sécurité laissent uniquement entrer le trafic de requêtes approprié. Le protocole utilisé par les bulletins météo est totalement différent de celui utilisé par vos clients. Vous ne devez pas parler au serveur. De plus, les groupes de sécurité fonctionnent au niveau du réseau AWS, et non au niveau de l’instance EC2, comme le ferait un pare-feu de système d’exploitation. 
> 
> Des attaques telles que les inondations UDP ou des attaques de réflexion sont simplement balayées par la mise à l’échelle de l’ensemble de la capacité des régions AWS, et non la capacité de votre EC2 individuelle. Notre taille constitue ici un avantage considérable pour votre protection. Je ne dirai pas qu'il est impossible de surcharger AWS, mais l’ampleur et les efforts nécessaires, coûteraient trop d’argent à ces personnes malveillantes. Des attaques Slowloris ? Il y a notre Elastic Load Balancer. L'ELB gère en premier lieu le trafic de requêtes HTTP, et il attend de recevoir la totalité du message, peu importe le temps que cela peut prendre, avant de l’envoyer au serveur web frontend. Bien sûr, vous pouvez essayer de le surcharger, mais souvenez-vous que l’ELB est évolutif et qu’il fonctionne au niveau des régions. 
> 
> Pour surcharger l'ELB, vous devez surcharger l’ensemble de la région AWS. Ce n'est pas impossible en théorie, mais cela coûterait beaucoup trop cher. Pour les attaques les plus sophistiquées et pointues, AWS fournit des outils de défense spécialisés appelés AWS Shield avec AWS WAF. AWS WAF utilise un pare-feu d’applications web pour filtrer le trafic entrant pour les signatures des personnes malveillantes. Ses capacités de machine learning sont vastes, et il peut reconnaître de nouvelles menaces à mesure qu’elles évoluent et protéger de manière proactive votre système contre la longue liste des vecteurs destructeurs. 
> 
> On arrive à la fin, le temps est presque écoulé. La conclusion est qu'un système bien conçu est déjà protégé contre la plupart des attaques. Avec AWS Shield Advanced, AWS peut devenir votre partenaire contre les attaques DDoS. Oh, oh.
</div>

Les clients peuvent appeler le café pour passer leurs commandes. Après avoir répondu à chaque appel et pris les commandes, le caissier les transmet au barista. 
 
Cependant, supposons qu’un plaisantin appelle plusieurs fois pour passer des commandes, mais qu’il ne vienne jamais récupérer ses boissons. Le caissier n’est donc pas disponible pour prendre les appels d’autres clients. Le café peut tenter de stopper ces fausses demandes en bloquant le numéro de téléphone utilisé par le plaisantin. 

Dans ce scénario, les actions du plaisantin sont semblables à une **attaque par déni de service**.

  
## Attaques par déni de service

Une **attaque par déni de service (DoS)** est une tentative délibérée de rendre un site web ou une application inaccessible aux utilisateurs.

![aws_ddos_01.png](/assets/img/clouds/tempo/aws_ddos_01.png =300x)

Par exemple, un attaquant peut inonder un site web ou une application avec un trafic réseau excessif jusqu’à ce que le site web ou l’application ciblé(e) devienne surchargé(e) et ne soit plus en mesure de répondre. Si le site web ou l’application devient indisponible, le service est refusé aux utilisateurs qui tentent de faire des demandes légitimes.

## Attaques par déni de service distribué (DDoS)

Maintenant, supposons que le plaisantin ait fait appel à des amis. 

Le plaisantin et ses amis appellent à plusieurs reprises le café pour passer des commandes, bien qu’ils n’aient pas l’intention d’aller les chercher. Ces demandes proviennent de différents numéros de téléphone et il est impossible pour le café de tous les bloquer. De plus, les clients ont de plus en plus de mal à passer leurs appels en raison de l’afflux d’appels. Ce type d’attaque est semblable à une **attaque par déni de service distribué**.

![aws_ddos_02.png](/assets/img/clouds/tempo/aws_ddos_02.png =500x)

Dans une attaque par déni de service distribué (DDoS), plusieurs sources sont utilisées pour lancer une attaque visant à rendre un site web ou une application indisponible. Cela peut provenir d’un groupe d’attaquants, ou même d’un seul attaquant. L’attaquant seul peut utiliser plusieurs ordinateurs infectés (également appelés « bots ») pour envoyer un trafic excessif vers un site web ou une application.

Pour réduire l’effet des attaques DoS et DDoS sur vos applications, vous pouvez utiliser **[AWS Shield](https://aws.amazon.com/shield)**.

## AWS Shield

AWS Shield est un service qui protège les applications contre les attaques par déni de service distribué (DDoS). AWS Shield offre deux niveaux de protection : Standard et Avancé.

![aws_ddos_03.png](/assets/img/clouds/tempo/aws_ddos_03.png)

Pour en savoir plus, sélectionnez le symbole + en regard de chaque service.

<div class="collapseContent" display="AWS Shield Standard" value="true">

**AWS Shield Standard** protège automatiquement tous les clients AWS, sans aucuns frais. Ce service protège vos ressources AWS contre les types d’attaques DDoS les plus courants et les plus fréquents.

A mesure que le trafic réseau entre dans vos applications, AWS Shield Standard utilise différentes techniques d’analyse pour détecter le trafic malveillant en temps réel et l’atténuer automatiquement.
</div>

<div class="collapseContent" display="AWS Shield Advanced" value="true">

**AWS Shield Advanced** est un service payant qui fournit des diagnostics détaillés des attaques et permet de détecter et d’atténuer les attaques DDoS sophistiquées. 

Ce service s’intègre également à d’autres services tels qu’Amazon CloudFront, Amazon Route 53 et Elastic Load Balancing. En outre, vous pouvez intégrer AWS Shield à AWS WAF en rédigeant des règles personnalisées pour atténuer les attaques par déni de service distribué complexes.
</div>

# Services de sécurité supplémentaires

```
video : xxx.mkv
```
*Figure. Additional security services*

<div class="collapseContent" display="Trasncription" value="true">


> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Avec toutes les allées et venus dans votre café, vous voulez accroître la sécurité de vos grains de café, de vos équipements, et même de l’argent dans la caisse. En ce qui concerne vos grains de café, cela peut être lorsqu’ils se trouvent dans votre salle de réserve ou même lorsque vous les transportez entre boutiques. Après tout, nous ne voulons pas que des visiteurs indésirables aient accès à nos grains de café, ou qu’ils s’emparent de nos précieux équipements. 
> 
> Pour commencer, parlons de la façon dont vous pouvez sécuriser vos grains de café, ou vos données que ce soit au repos, ou en transit. Pour vos grains, la façon simple de les sécuriser serait de verrouiller la porte en partant le soir. Il s'agit de la notion de chiffrement, qui sécurise un message ou des données de sorte que seules les parties autorisées puissent y accéder. sont donc moins susceptibles de pouvoir accéder au message. Ou ne peuvent pas y accéder du tout. Pensez à l'exemple de la clé et de la porte. Si vous avez la clé, vous pouvez déverrouiller la porte. Sinon, vous ne pouvez pas déverrouiller cette porte. 
> 
> Chez AWS, ce processus se présente sous deux formes. Le chiffrement au repos et le chiffrement en transit. Au repos signifie que vos données sont inactives. Celles-ci sont simplement stockées et ne sont pas déplacées. Par exemple, le chiffrement au repos côté serveur est activé sur toutes les données des tables DynamoDB. Cela permet d'éviter les accès non autorisés. Le chiffrement au repos de DynamoDB s’intègre également à AWS KMS, ou Key Management Service, pour gérer la clé de chiffrement utilisée pour chiffre vos tables. Il s'agit de la clé de votre porte, vous vous souvenez ? Sans elle, vous ne pouvez pas accéder à vos données. Veillez donc à la garder en sécurité. 
> 
> De même, le terme « en transit » signifie que les données se déplacent entre, disons A et B. qui accès au service. Voire un autre service AWS. Par exemple, imaginons que nous avons une instance Redshift est en cours d'exécution. Nous voulons la connecter à un client SQL. Nous utilisons le protocole SSL, ou les connexions SSL pour chiffrer les données et nous pouvons utiliser les certificats du service pour valider, et autoriser un client. Cela signifie que les données sont protégées lors du transfert entre Redshift, et notre client. Cette fonction existe pour de nombreux autres services AWS, comme SQS, S3, RDS et bien d’autres. 
> 
> En parlant d'autres services, le prochain service dont nous allons parler s’appelle Amazon Inspector. Inspector contribue à améliorer la sécurité, et la conformité de vos applications AWS déployées, en réalisant une évaluation de sécurité automatisée de votre infrastructure. Plus précisément, il permet de vérifier les manquements par rapport aux bonnes pratiques de sécurité, l’exposition aux instances EC2, les vulnérabilités, etc. Le service se compose de trois éléments : un élément d’accessibilité de configuration réseau, un agent Amazon, qui peut être installé sur une instance EC2, et un service d’évaluation de la sécurité qui rassemble le tout. Pour l’utiliser, configurez les options d’Inspector, exécutez le service, et une liste des potentiels problèmes de sécurité s’affiche. Les résultats s’affichent dans la console Amazon Inspector, et ils sont présentés avec une description détaillée de la question de sécurité, et une recommandation sur la manière de la résoudre. En outre, vous pouvez récupérer les résultats par le biais d'une API. Cela permet d’appliquer les bonnes pratiques en matière de correction pour résoudre les problèmes. 
> 
> Une autre dimension de service est notre offre de détection des menaces, appelé Amazon GuardDuty. Il analyse les flux continus de métadonnées, générés à partir de votre compte, et les activités du réseau détectées sur les événements AWS CloudTrail, les journaux de flux Amazon VPC, et les journaux DNS. Ce service utilise des renseignements sur les menaces tels que les adresses IP malveillantes connues, la détection d'anomalies et le machine learning pour identifier les menaces avec plus de précision. Le meilleur de toute cela est qu’il s'exécute indépendamment de vos autres services AWS. Son utilisation n’affecte donc pas les performances ou la disponibilité de votre infrastructure et vos charges de travail existantes.
> 
> Il y a de nombreux autres services de sécurité comme Advanced Shield et Security Hub. Consultez la section des ressources pour en savoir plus. Merci de nous avoir suivis.
</div>

## AWS Key Management Service (AWS KMS)

Le café dispose de nombreux articles, tels que des machines à café, des pâtisseries, de l’argent dans les caisses enregistreuses, etc. Vous pouvez considérer ces éléments comme des données. Les propriétaires du cafés veulent s’assurer que tous ces articles sont en sécurité, qu’ils se trouvent dans la salle de stockage ou soient en cours de transport entre les magasins. 

De la même manière, vous devez vous assurer que les données de vos applications sont en sécurité pendant leur stockage **(chiffrement au repos)** et pendant leur transmission **(chiffrement en transit)**.

**[AWS Key Management Service (AWS KMS)](https://aws.amazon.com/kms)** vous permet d’effectuer des opérations de chiffrement à l’aide de **clés de chiffrement**. Une clé de chiffrement est une chaîne aléatoire de chiffres utilisée pour verrouiller (chiffrer) et déverrouiller (déchiffrer) les données. Vous pouvez utiliser AWS KMS pour créer, gérer et utiliser des clés de chiffrement. Vous pouvez également contrôler l’utilisation des clés dans un large éventail de services et dans vos applications.

![aws_kms_01.png](/assets/img/clouds/tempo/aws_kms_01.png)

Le diagramme montre les principales fonctions d'AWS Key Management Service et les possibilités d'intégration à d'autres services AWS. Trois sections s'affichent.

La première section est intitulée « AWS KMS », comporte une illustration de l'icône de l'architecture AWS KMS et le texte « Create and control the cryptographic keys that protect your data » (Créez et contrôlez des clés cryptographiques qui protègent vos données). Un encadré, intitulé « Create a KMS key » (Créer une clé KMS) est situé à droite et comporte le texte « Your KMS keys never leave our FIPS 140-validated HSMs unencrypted, and you control who can access and use them » (Vos clés KMS ne quittent jamais nos HSM validés FIPS 140 sans être chiffrées et vous contrôlez qui peut y accéder et les utiliser). Un autre en-tête, intitulé « Cryptographic operations » (Opérations cryptographiques), se trouve à droite et comporte l'explication suivante : « Use your KMS keys in cryptographic operations » (Utilisez vos clés KMS dans le cadre d'opérations cryptographiques). Cette première section répertorie également les fonctions d'AWS KMS. Ces fonctions sont les suivantes : « Encrypt and decrypt data » (Chiffrer et déchiffrer les données), « Sign and verify » (Signer et vérifier), « Generate and export data keys » (Générer et exporter des clés de données) et « Generate and verify MACs » (Générer et vérifier les MAC).

En dessous de la première section, se trouve le titre « KMS key monitoring » (Surveillance des clés KMS), accompagné de l'explication suivante : « Automate monitoring to receive event alerts and audit who used which keys, on which resources, and when » (Automatiser la surveillance pour recevoir des alertes sur les événements et vérifier qui a utilisé quelles clés, sur quelles ressources et à quel moment). Deux services sont répertoriés, AWS CloudTrail et Amazon CloudWatch, et accompagnés de leur icône d'architecture respective.

La troisième et dernière section se trouve à droite des sections précédemment mentionnées. Elle est intitulée « AWS KMS integrated services » (Services intégrés AWS KMS) et comporte le texte suivant : « AWS services perform envelope encryption using data keys protected by your KMS keys to encrypt data at rest »(Les services AWS effectuent le chiffrement d'enveloppe à l'aide de clés de données protégées par vos clés KMS afin de chiffrer les données au repos). Les services répertoriés sont Amazon EC2, Amazon S3, Amazon EBS et d'autres services AWS.

Avec AWS KMS, vous pouvez choisir les niveaux de contrôle d’accès spécifiques dont vous avez besoin pour vos clés. Par exemple, vous pouvez spécifier quels utilisateurs et rôles IAM sont capables de gérer les clés. Vous pouvez également désactiver temporairement les clés afin que personne ne puisse les utiliser. Vos clés ne quittent jamais AWS KMS et vous les contrôlez toujours.

## AWS WAF

**[AWS WAF](https://aws.amazon.com/waf)** est un pare-feu d’application web qui vous permet de surveiller les requêtes réseau qui entrent dans vos applications web. 

![aws_waf_01.png](/assets/img/clouds/tempo/aws_waf_01.png)

AWS WAF travaille en collaboration avec Amazon CloudFront et un équilibreur de charge d’application. Rappelez-vous des listes de contrôle d’accès réseau qui ont été abordées dans un module antérieur. AWS WAF fonctionne de la même manière pour bloquer ou autoriser le trafic. Toutefois, ce service a recours à **[une liste de contrôle d’accès (ACL) web](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html)** pour protéger vos ressources AWS. 

Voici un exemple de la manière dont vous pouvez utiliser AWS WAF pour autoriser et bloquer des demandes spécifiques.

![aws_waf_02.png](/assets/img/clouds/tempo/aws_waf_02.png =300x)

Supposons que votre application ait reçu des requêtes réseau malveillantes de plusieurs adresses IP. Vous souhaitez empêcher ces demandes de continuer à accéder à votre application, mais vous voulez également vous assurer que les utilisateurs légitimes peuvent toujours y accéder. Vous configurez la liste ACL web pour autoriser toutes les demandes, sauf celles provenant des adresses IP que vous avez spécifiées.

Lorsqu’une requête entre dans AWS WAF, ce dernier vérifie et consulte la liste des règles que vous avez configurées dans la liste ACL web. Si une demande ne provient pas de l’une des adresses IP bloquées, elle autorise l’accès à l’application.

![aws_waf_03.png](/assets/img/clouds/tempo/aws_waf_03.png =300x)

Toutefois, si une demande provient de l’une des adresses IP bloquées que vous avez spécifiées dans la liste ACL web, l’accès est refusé.

## Amazon Inspector

Supposons que les développeurs du café conçoivent et testent une nouvelle application de commande. Ils veulent s’assurer qu’ils développent l’application conformément aux bonnes pratiques en matière de sécurité. Cependant, ils doivent développer plusieurs autres applications et ne peuvent donc pas passer beaucoup de temps à effectuer des évaluations manuelles. Pour effectuer des évaluations de sécurité automatisées, ils décident d’utiliser **[Amazon Inspector](https://aws.amazon.com/fr/inspector/)**.

Amazon Inspector contribue à améliorer la sécurité et la conformité des applications en exécutant des évaluations de sécurité automatisées. Ce service vérifie les applications pour détecter les failles de sécurité et les écarts par rapport aux bonnes pratiques en matière de sécurité, telles que l’accès libre aux instances Amazon EC2 et les installations de versions de logiciels vulnérables. 

![aws_inspector_01.png](/assets/img/clouds/tempo/aws_inspector_01.png)

Une fois qu’Amazon Inspector a effectué une évaluation, une liste des résultats de sécurité vous est fournie. Cette liste établit les priorités par niveau de gravité, y compris une description détaillée de chaque problème de sécurité et une recommandation sur la façon de le résoudre. Toutefois, AWS ne garantit pas que l’application des recommandations fournies résoudra chaque problème de sécurité potentiel. Dans le cadre du modèle de responsabilité partagée, les clients sont responsables de la sécurité de leurs applications, processus et outils qui s’exécutent sur les services AWS.

## Amazon GuardDuty

**[Amazon GuardDuty](https://aws.amazon.com/guardduty)** est un service qui assure une détection intelligente des menaces pour votre infrastructure et vos ressources AWS. Ce service détecte les menaces grâce à un contrôle en continu des activités de réseau et des comportements de compte au sein de votre environnement AWS.

![aws_guardduty_01.png](/assets/img/clouds/tempo/aws_guardduty_01.png)

![aws_guardduty_02.png](/assets/img/clouds/tempo/aws_guardduty_02.png =750x)

Une fois que vous avez activé GuardDuty pour votre compte AWS, GuardDuty commence à surveiller l’activité de votre réseau et de votre compte. Vous n’avez pas besoin de déployer ou de gérer un logiciel de sécurité supplémentaire. GuardDuty analyse ensuite en permanence les données provenant de plusieurs sources AWS, y compris les journaux de flux VPC et les journaux DNS. 

Si GuardDuty détecte des menaces, vous pouvez consulter les conclusions détaillées les concernant depuis AWS Management Console. Les conclusions reprennent notamment les étapes recommandées pour résoudre ces menaces. Vous pouvez également configurer les fonctions AWS Lambda pour qu’elles prennent automatiquement des mesures de résolution en réponse aux conclusions de sécurité de GuardDuty.

# Module 6 : résumé

Dans le module 6, vous avez découvert les concepts suivants :

- Le modèle de responsabilité partagée
- Fonctions d’AWS Identity and Access Management
- Méthodes de gestion de plusieurs comptes dans AWS Organizations
- Ressources de conformité AWS
- Services AWS pour la sécurité et le chiffrement des applications

```
video : xxx.mkv
```
*Figure. Module summary - Security*

<div class="collapseContent" display="Trasncription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Bien. Il est temps de résumer ce que nous venons d'aborder. Tout d’abord, AWS suit un modèle de responsabilité partagée. AWS est responsable de la sécurité « du » cloud, tandis que vous êtes responsable de la sécurité « dans » le cloud. 
> 
> Avec IAM, il y a des utilisateurs, des groupes, des rôles et des stratégies. Les utilisateurs se connectent avec un identifiant et un mot de passe, et par défaut ils n’ont pas d’autorisations. Les groupes regroupent plusieurs utilisateurs et les rôles sont des identités que vous pouvez endosser pour accéder à des informations d’identification temporaires et à des autorisations selon un délai configurable. Afin d'attribuer des autorisations à une identité, vous devez créer des stratégies qui permettent explicitement ou refusent une action spécifique dans AWS. Avec IAM, il y a aussi la fédération d'identité. Si vous disposez d'une base d'identités d'entreprise existante, vous pouvez fédérer ces utilisateurs auprès d’AWS, en utilisant l’accès basé sur les rôles, ce qui permet à vos utilisateurs de disposer d’une seule session pour se connecter à vos systèmes d’entreprise et à AWS. Un dernier point à retenir concernant IAM est que vous devez vous assurer d’activer l’authentification multifacteur pour les utilisateurs, mais surtout pour votre utilisateur racine qui dispose, par défaut, de toutes les autorisations et qui ne peut pas être restreint. 
> 
> Puis, nous avons discuté d'AWS Organizations. Avec AWS, vous avez certainement plusieurs comptes. Les comptes sont couramment utilisés pour isoler les charges de travail, les environnements, les équipes ou les applications. AWS Organizations vous permet de gérer plusieurs comptes de façon hiérarchique. Nous avons ensuite abordé la conformité. AWS a recours à des auditeurs tiers pour prouver qu'il respecte une grande variété de programmes de conformité. Vous pouvez utiliser le centre de conformité AWS pour en savoir plus sur la conformité et AWS Artifact pour accéder aux documents de conformité. Les exigences de conformité varient d’une application à l’autre et selon les domaines d’opérations. 
> 
> Nous avons ensuite parlé des attaques par déni de service distribué, ou attaques DDoS, et comment les combattre avec AWS et des outils comme ELB, les groupes de sécurité, AWS Shield, et AWS WAF. Nous avons aussi parlé du chiffrement. Dans AWS, vous êtes le propriétaire de vos données, et vous êtes responsable de la sécurité. Cela signifie que vous devez envisager le chiffrement, en transit et au repos. 
> 
> Il y a beaucoup de considérations lorsqu’il est question de sécurité dans AWS. La sécurité est la priorité absolue d'AWS, et continuera de l’être. Assurez-vous de lire la documentation sur la sécurisation de vos ressource AWS, car cela varie d’un service à l’autre. Utilisez le principe de moindre privilège pour définir les autorisations des utilisateurs et des rôles dans IAM, chiffrez vos données à chaque couche, en transit et au repos. Et assurez-vous d'utiliser les services AWS pour protéger votre environnement.
  
</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts abordés dans le module 6, consultez ces ressources.

- [Sécurité, identité et conformité sur AWS](https://aws.amazon.com/products/security)
- [Livre blanc : introduction à la sécurité AWS](https://docs.aws.amazon.com/whitepapers/latest/introduction-aws-security/welcome.html)
- [Livre blanc : Amazon Web Services – Présentation des processus de sécurité](https://docs.aws.amazon.com/whitepapers/latest/aws-overview-security-processes/aws-overview-security-processes.pdf)
- [Blog sur la sécurité AWS](https://aws.amazon.com/blogs/security/)
- [Conformité AWS](https://aws.amazon.com/compliance)
- [Témoignages de clients AWS : sécurité, identité et conformité](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23security-identity-compliance)

# Module 6 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelle affirmation décrit le mieux une politique IAM ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Un processus d’authentification qui fournit une couche supplémentaire de protection pour votre compte AWS</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">Un document qui accorde ou refuse des autorisations aux services et ressources AWS</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">Une identité que vous pouvez endosser pour obtenir un accès temporaire aux autorisations</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Une identité établie lorsque vous créez un compte AWS pour la première fois</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Un document qui accorde ou refuse des autorisations aux services et ressources AWS**.

Les politiques IAM vous offrent la flexibilité nécessaire pour personnaliser les niveaux d’accès des utilisateurs aux ressources. Par exemple, vous pouvez autoriser les utilisateurs à accéder à tous les compartiments Amazon S3 de votre compte AWS ou à un compartiment spécifique.

Les autres réponses sont incorrectes, car :

- L’authentification multifacteur (MFA, Multi-Factor Authentication) est un processus d’authentification qui fournit une couche supplémentaire de protection pour votre compte AWS.
- Un rôle IAM est une identité que vous pouvez endosser pour obtenir un accès temporaire aux autorisations.
- L’identité de l’utilisateur racine est l’identité qui est établie lorsque vous créez un compte AWS pour la première fois.

En savoir plus :

- [AWS IAM : politiques et autorisations](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)
</div>

**Un employé a besoin d’un accès temporaire pour créer plusieurs compartiments Amazon S3. Quelle option serait le meilleur choix pour cette tâche ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Utilisateur racine d’un compte AWS</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">Groupe IAM</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">Rôle IAM</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Stratégie de contrôle de services (SCP)</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Rôle IAM**.

Un rôle IAM est une identité que vous pouvez endosser pour obtenir un accès temporaire aux autorisations. Lorsqu’une personne endosse un rôle IAM, elle abandonne toutes les autorisations dont elle disposait sous un rôle précédent et endosse les autorisations liées à ce nouveau rôle. Les rôles IAM conviennent parfaitement lorsque l’accès aux services ou aux ressources doit être accordé temporairement, plutôt qu’à long terme.

Les autres réponses sont incorrectes, car :

- L’utilisateur racine du compte AWS est établi lorsque vous créez un compte AWS pour la première fois. Une bonne pratique consiste à ne pas se servir de l’utilisateur racine pour les tâches quotidiennes.
- Même si vous pouvez attacher des politiques IAM à un groupe IAM, ce ne serait pas le meilleur choix dans ce cas, car l’employé n’a besoin que d’autorisations temporaires.
- Les stratégies de contrôle des services (SCP) vous permettent de contrôler, de manière centralisée, les autorisations des comptes de votre organisation. Une SCP n’est pas le meilleur choix pour accorder des autorisations temporaires à un employé individuel.

En savoir plus :

- [Rôles IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
</div>

**Quelle affirmation décrit le mieux le principe du moindre privilège ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Ajouter un utilisateur IAM dans au moins un groupe IAM</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Vérifier les autorisations d’un paquet par rapport à une liste de contrôle d’accès</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Octroyer uniquement les autorisations nécessaires à l’exécution de tâches spécifiques</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Exécuter une attaque par déni de service provenant d’au moins un périphérique</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Octroyer uniquement les autorisations nécessaires à l’exécution de tâches spécifiques**.

Lorsque vous octroyez des autorisations en suivant le principe du moindre privilège, vous empêchez les utilisateurs ou les rôles d’avoir plus d’autorisations que nécessaire pour effectuer des tâches spécifiques. Par exemple, les caissiers du café doivent avoir accès au système de caisse enregistreuse. Il est recommandé d’accorder aux utilisateurs et aux rôles IAM un ensemble minimal d’autorisations, puis d’accorder des autorisations supplémentaires si nécessaire.

En savoir plus :

- [Bonnes pratiques en matière de sécurité dans IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
  
</div>

**Quel service aide à protéger vos applications contre les attaques par déni de service distribué (DDoS) ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">Amazon GuardDuty</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Amazon Inspector</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">AWS Artifact</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">AWS Shield</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Shield**.

À mesure que le trafic réseau entre dans vos applications, AWS Shield utilise plusieurs techniques d’analyse pour détecter les potentielles attaques DDoS en temps réel et les atténue automatiquement.

Les autres réponses sont incorrectes, car :

- Amazon GuardDuty est un service qui assure une détection intelligente des menaces pour votre infrastructure et vos ressources AWS. Il identifie les menaces en surveillant en continu l’activité du réseau et le comportement des comptes au sein de votre environnement AWS.
- Amazon Inspector vérifie les applications pour détecter les failles de sécurité et les manquements aux bonnes pratiques en matière de sécurité, comme l’accès ouvert aux instances Amazon EC2 et les installations de versions logicielles vulnérables.
- AWS Artifact est un service qui fournit un accès à la demande aux rapports de sécurité et de conformité AWS et à certains contrats en ligne.

En savoir plus :

- [AWS Shield](https://aws.amazon.com/shield/)
</div>

**Quelle tâche AWS Key Management Service (AWS KMS) peut-il effectuer ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">Configurer la Multi-Factor Authentication (MFA).</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">Mettre à jour le mot de passe de l’utilisateur racine du compte AWS.</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">Créer des clés de chiffrement.</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">Attribuer des autorisations aux utilisateurs et aux groupes.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Créer des clés de chiffrement**.

AWS Key Management Service (AWS KMS) vous permet d’effectuer des opérations de chiffrement à l’aide de clés de chiffrement. Une clé de chiffrement est une chaîne aléatoire de chiffres utilisée pour verrouiller (chiffrer) et déverrouiller (déchiffrer) les données. Vous pouvez utiliser AWS KMS pour créer, gérer et utiliser des clés de chiffrement. Vous pouvez également contrôler l’utilisation des clés dans un large éventail de services et dans vos applications.

Les autres réponses sont incorrectes, car :

- Vous pouvez configurer l’authentification multifacteur (MFA) dans AWS Identity and Access Management (IAM).
- Vous pouvez mettre à jour le mot de passe de l’utilisateur racine du compte AWS dans AWS Management Console.
- Vous pouvez attribuer des autorisations aux utilisateurs et aux groupes dans AWS Identity and Access Management (IAM).

En savoir plus :

- [AWS KMS](https://aws.amazon.com/kms/)
</div>

Le module suivant aborde les outils AWS de surveillance et d’analytique.