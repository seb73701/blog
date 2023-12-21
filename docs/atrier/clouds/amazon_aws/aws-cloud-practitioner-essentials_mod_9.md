---
title: Module 9 - Migration et innovation
description: 
published: true
date: 2023-04-21T10:56:57.527Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:51.253Z
---

# Module 9 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Comprendre la migration et l’innovation dans le Cloud AWS
- Résumer le Framework d’adoption du Cloud AWS (AWS CAF) 
- Résumer les six facteurs clés d’une stratégie de migration dans le cloud
- Décrire les avantages des solutions de migration de données AWS, telles qu’AWS Snowcone, AWS Snowball et AWS Snowmobile
- Résumer le large éventail de solutions innovantes offert par AWS

```
video : xxx.mkv
```
*Figure. Module introduction - Migration and innovation*

<div class="collapseContent" display="Transcription" value="true">


> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Nous savons maintenant comment démarrer sur AWS Cloud mais qu’en est-il si vous avez des déploiements existants dans des environnements sur site ou vous avez commencé votre transition vers le cloud sans AWS ? Nous adorerions vous aider, d’autant plus que la transition peut vous conférer des économies réelles. 
> 
> Cela nous amène à la migration et à l'innovation. Nous allons parler de toutes les options disponibles comme les outils de migration, le framework d’adoption du cloud AWS, et même Snow Family, qui se compose de périphériques physiques permettant de migrer des données dans et hors d’AWS. 
> 
> Nous n'avons pas fini ; restez bien assis pour découvrir plus d’informations sur la migration d’un environnement sur site ou du cloud à AWS. Nous parlerons même des 6 « R » de la migration.
</div>


# Framework d’adoption du Cloud AWS (AWS CAF)

```
video : xxx.mkv
```
*Figure. AWS Cloud Adoption Framework*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> La migration vers le cloud est un processus. Il ne suffit pas de claquer des doigts pour que tout soit hébergé par magie dans AWS. La migration d'applications vers AWS représente une charge de travail importante et une migration vers le cloud réussie requiert de l’expertise. 
> 
> Heureusement, beaucoup de personnes ont déjà, avec succès, migré vers le cloud dans le passé et vous n’êtes pas les premiers à avoir des solutions dans AWS. Ainsi, la majorité des connaissances inhérentes à l’hébergement sur AWS ont été capturées et partagées. 
> 
> Cela étant dit, le poste que vous occupez au sein de votre organisation aura une incidence sur ce que vous devez savoir, et sur la manière dont vous pouvez aider à une migration. Si vous êtes développeur, votre rôle et votre point de vue seront très différents d'un architecte cloud, d'un analyste d’entreprise ou d'un analyste financier. Différents types de personnes apportent des perspectives différentes en ce qui concerne une migration. Il est conseillé de s’approprier de ces différentes perspectives et de vous assurer que tout le monde a la même orientation. 
> 
> Vous devez également vous assurer d'avoir recours aux personnes appropriées pour prendre en charge votre migration et les RH devront donc embaucher des personnes en conséquence pour procédure à votre migration. Cela peut faire beaucoup à suivre, et quelqu'un de peu expérimenté dans la cloud pourrait ne pas penser à toutes les différentes personnes qui ont besoin d'être impliquées. 
> 
> L'équipe d’AWS Professional Services a créé le framework d’adoption du cloud AWS pour vous aider à gérer ce processus par le biais de conseils. Le Framework d’adoption du cloud fournit des conseils à votre entreprise pour permettre une migration en douceur vers AWS. 
> 
> Le framework organise ses conseils selon six domaines en fonction des différents types de personnes que vous devrez impliquer dans votre migration. Chaque perspective porte sur des responsabilités distinctes, reposant sur différents groupes. Les différentes perspectives Entreprise, Personnes et Gouvernance se concentrent sur les capacités commerciales, tandis que les perspectives Plateforme, Sécurité et Opérations sont axées sur les capacités techniques. Une personne qui est un analyste d’entreprise ou financier serait couverte par la perspective Entreprise, les RH par la perspective Personnes et un architecte cloud par la perspective Plateforme. 
> 
> Chaque perspective est utilisée pour découvrir des lacunes dans vos compétences et vos processus, qui sont ensuite enregistrés en tant qu'entrées. Les entrées sont ensuite utilisées comme base pour la création d'un plan d'action Framework d’adoption du Cloud AWS que vous pouvez ensuite utiliser pour guider la gestion du changement de votre organisation lorsque vous migrez vers le cloud. Un plan d'action adapté à votre organisation peut vous aider à rester sur la bonne voie. 
> 
> La migration vers le cloud peut être compliquée, mais vous n'êtes pas seul pour y arriver. Il existe des dizaines de ressources pour vous aider à démarrer et le framework d’adoption du cloud en est un bon exemple.
</div>

## Six perspectives de base du Framework d’adoption du cloud

Au plus haut niveau, le **[Framework d’adoption du Cloud AWS (AWS CAF)](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)** comporte des conseils concernant six zones d’intérêt, appelées **perspectives**. Chaque perspective porte sur des responsabilités distinctes. Le processus de planification aide les personnes appropriées au sein de l’organisation à se préparer aux changements à venir.

En règle générale, les perspectives **Business**, **Personnes** et **Gouvernance** se concentrent sur les capacités de l’entreprise, tandis que les perspectives **Plateforme**, **Sécurité** et **Opérations** se concentrent sur les capacités techniques.

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Perspective Entreprise" value="true">

La **perspective Entreprise** veille à créer une harmonie entre l’informatique et les besoins de l’entreprise ainsi qu’à créer un lien entre les investissements informatiques et les principaux résultats opérationnels.

Utilisez la perspective Entreprise afin de créer une analyse de rentabilisation solide aux fins de l’adoption du cloud et d’établir un ordre de priorité pour les initiatives d’adoption du cloud. Veillez à ce que vos stratégies et objectifs opérationnels s’harmonisent avec vos stratégies et objectifs informatiques.

La perspective Entreprise inclue les rôles habituels suivants : 

- Gestionnaires opérationnels
- Directeurs financiers
- Propriétaires de budget
- Parties prenantes stratégiques
</div>

<div class="collapseContent" display="Perspective Personnes" value="true">

La **perspective Personnes** prend en charge le développement d’une stratégie de gestion des modifications à l’échelle de l’organisation pour assurer la réussite de l’adoption du cloud.

Utilisez la perspective Personnes pour évaluer les structures et les rôles organisationnels ainsi que les nouvelles exigences en matière de compétences et de processus, et identifier les failles. Cela permet d’établir un ordre de priorité pour la formation, les effectifs et les changements organisationnels.

La perspective Personnes inclue les rôles habituels suivants : 

- Ressources humaines
- Effectifs
- Gestionnaires de personnes
</div>

<div class="collapseContent" display="Perspective Gouvernance" value="true">

La **perspective Gouvernance** met l’accent sur les compétences et les processus permettant d’harmoniser la stratégie informatique avec la stratégie opérationnelle. Cela vous permet d’optimiser la valeur opérationnelle et de réduire les risques.

Utilisez la perspective Gouvernance pour comprendre comment mettre à jour les compétences du personnel et les processus nécessaires à la gouvernance opérationnelle dans le cloud. Gérez et mesurez les investissements liés au cloud pour évaluer les résultats opérationnels.

La perspective Gouvernance inclue les rôles habituels suivants : 

- Chief Information Officer (CIO)
- Responsables de programmes
- Architectes d’entreprise
- Analystes d’entreprise
- Gestionnaires de portefeuille
</div>

<div class="collapseContent" display="Perspective Plateforme " value="true">

La **perspective Plateforme** inclut des principes et des modèles pour l’implémentation de nouvelles solutions dans le cloud et la migration des charges de travail sur site vers le cloud.

Utilisez une variété de modèles d’architecture pour comprendre et communiquer la structure des systèmes informatiques et leurs relations. Décrivez en détail l’architecture de l’environnement à l’état cible.

La perspective Plateforme inclue les rôles habituels suivants : 

- Chief Technology Officer (CTO)
- Gestionnaires informatiques
- Architectes de solutions
</div>

<div class="collapseContent" display="Perspective Sécurité" value="true">

La **perspective Sécurité** garantit que l’organisation respecte les objectifs de sécurité en matière de visibilité, de capacité d’audit, de contrôle et d’agilité.

Utilisez AWS CAF pour structurer la sélection et l’implémentation des contrôles de sécurité qui répondent aux besoins de l’organisation.

La perspective Sécurité inclue les rôles habituels suivants : 

- Responsable de la sécurité de l’information (CISO)
- Gestionnaires de la sécurité informatique
- Analystes de sécurité informatique
</div>

<div class="collapseContent" display="Perspective Opérations" value="true">

La **perspective Opérations** vous permet d’activer, d’exécuter, d’utiliser, d’exploiter et de récupérer des charges de travail informatiques au niveau convenu avec les parties prenantes de votre entreprise.

Définissez la manière dont les activités quotidiennes, trimestrielles et annuelles doivent être menées dans l’entreprise. Soutenez les opérations de l’entreprise tout en étant en adéquation avec celles-ci. L’AWS CAF aide ces parties prenantes à définir les procédures opérationnelles actuelles et à identifier les modifications de processus et la formation nécessaires pour assurer la réussite de l’adoption du cloud.

La perspective Opérations inclue les rôles habituels suivants : 

- Gestionnaires des opérations informatiques
- Gestionnaires du support informatique
</div>

## Contrôle des connaissances

**Quelle perspective du Framework d’adoption du Cloud AWS vous permet de concevoir, d’implémenter et d’optimiser votre infrastructure AWS en fonction de vos perspectives et objectifs opérationnels ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Perspective Entreprise</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Perspective Plateforme</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Perspective Opérations</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Perspective Personnes</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Perspective Plateforme**.

La perspective Plateforme du Framework d’adoption du Cloud AWS comprend également des principes pour l’implémentation de nouvelles solutions et la migration des charges de travail sur site vers le cloud.

Les autres réponses sont incorrectes, car :

- La perspective Entreprise vous aide à passer d’un modèle qui sépare les stratégies opérationnelles et informatiques à un modèle opérationnel qui intègre la stratégie informatique.
- La perspective Opérations met l’accent sur l’exploitation et la récupération des charges de travail informatiques afin de répondre aux besoins des parties prenantes de votre entreprise.
- La perspective Personnes aide les employés des ressources humaines (RH) à préparer leurs équipes à l’adoption du cloud en mettant à jour les processus organisationnels et les compétences du personnel afin d’y inclure des compétences basées sur le cloud.

En savoir plus :

- [Livre blanc : une présentation du Framework d’adoption du Cloud AWS](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
</div>

# Stratégies de migration

```
video : xxx.mkv
```
*Figure. The 6 R's of migration*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Le moment de migrer depuis votre déploiement sur site vers AWS est enfin venu. Comme Morgan l'a dit juste avant, ce serait génial de simplement claquer des doigts pour que tous les éléments de votre centre de données sur site existant apparaissent instantanément et par magie sur AWS optimisées et efficientes. Mais cela ne fonctionne pas comme ça. 
> 
> Chaque application (ou groupe d'applications, si elles sont étroitement couplées) dispose de six options possibles pour une migration d’entreprise. On appelle ça les six « R ». Une fois la phase de découverte terminée et que vous savez exactement ce que vous avez dans votre environnement existant, décidez laquelle des six R pourrait être la plus adaptée selon des facteurs tels que le temps, le coût, la priorité et la criticité. 
> 
> La première option est le réhébergement. C'est également connu sous le nom de « lift and shift ». Cette option est simple, car vous n’apportez aucun changement. Du moins, pas au début. Il suffit de déplacer les applications telles qu’elles sont vers AWS. Il est possible que vous n'obteniez pas tous les avantages possibles, mais certaines entreprises ont constaté que même sans aucune optimisation, elles pouvaient économiser jusqu’à 30 % de leurs coûts totaux juste en se réhébergeant. De plus, il est plus facile d'optimiser les applications ultérieurement, une fois qu’elles sont déjà hébergées sur le cloud, car les compétences de votre organisation sont meilleures. La partie difficile, soit la migration, est déjà terminée. 
> 
> La deuxième option est le remaniement, ou « lift, tinker, and shift ». C'est essentiellement un « lift and shift », mais au lieu d’une migration directe il y a quelques optimisations de cloud à faire. Vous ne touchez à aucun code de base lors du processus. Aucun travail de développement n'est requis. Par exemple, vous pouvez remanier votre base de données MySQL existante sur RDS MySQL, sans aucun changement de code. Ou vous pouvez même envisager une mise à niveau vers Amazon Aurora. Cela confère d'importants avantages à votre équipe d'administrateurs de bases de données ainsi que des performances accrues sans aucun changement de code. 
> 
> Avant d'ajouter plus d'options de migration, deux des six R ne se retrouvent pas sur AWS. L'option 3, la mise hors service. Certains éléments de votre portefeuille informatique professionnel ne sont tout simplement plus nécessaires. De 10 à 20 % des portefeuilles d'applications des sociétés comprennent des applications qui ne sont plus utilisées ou ont déjà des remplacements actifs et fonctionnels. Utiliser le plan de migration AWS pour arrêter ces applications contribue à réduire significativement les coûts et les efforts de votre équipe. Parfois, il faut simplement éteindre les lumières. 
> 
> La quatrième option, la rétention. Certaines applications sont sur le point de devenir obsolètes, mais pas dans l’immédiat. Elles doivent encore être exécutées, ne les désactivez pas pendant encore trois ou huit mois. Ces applications pourraient être migrées vers AWS, mais pourquoi ? Vous ne devez migrer que ce qui est logique et nécessaire pour votre entreprise. Au fil du temps, ces applications peuvent être rendues obsolètes dans leur emplacement et finalement retirées. Revenons aux éléments que l'on déplace vers AWS. 
> 
> La cinquième option, le rachat. C'est courant pour les entreprises de chercher à se détacher de leurs fournisseurs de logiciels et prendre un nouveau départ avec la migration. Par exemple, mettre fin à un contrat avec un ancien fournisseur CRM pour collaborer avec un nouveau fournisseur. Ou enfin résilier votre licence auprès d'un fournisseur de bases de données obsolète en faveur d’offres de bases de données natives cloud. C'est intéressant, mais n'oubliez pas que vous devez gérer un nouveau package logiciel et certains sont faciles à implémenter, d’autres prennent du temps. Les dépenses initiales totales augmentent donc mais les avantages potentiels peuvent être considérables. 
> 
> La sixième option, la refactorisation. Ici, vous écrivez un nouveau code. Elle est motivée par un besoin important des entreprises d'ajouter des fonctions, ou d’augmenter les performances, ce qui peut être difficile sur un environnement existant mais à présent est à votre portée. Modifier votre architecture de manière radicale peut être bénéfique pour votre entreprise mais les coûts initiaux sont très élevés en matière de planification et d’effort humain. 
> 
> Voici donc six possibilités de migration vers AWS. Après que vous avez établi l'inventaire de tous vos composants, faire le bon choix pour chaque partie est essentiel au succès de votre migration.
</div>

## 6 stratégies de migration

Lors de la migration d’applications vers le cloud, les six [stratégies de migration](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/) les plus courantes que vous pouvez implémenter sont :

- Réhébergement
- Remaniement
- Refactorisation/réorganisation
- Rachat
- Retenue
- Retrait

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.


<div class="collapseContent" display="Réhébergement" value="true">

**Le réhébergement**, également connu sous le nom de « **lift-and-shift** », implique le déplacement d’applications sans modifications. 

Dans le scénario d’une migration existante importante, dans lequel l’entreprise cherche à implémenter sa migration et à se mettre rapidement à l’échelle afin de répondre à une analyse de rentabilisation, la majorité des applications sont réhébergées.
</div>

<div class="collapseContent" display="Remaniement" value="true">

**Le remaniement**, également connu sous le nom de « **lift, tinker, and shift** », implique de réaliser quelques optimisations de cloud pour obtenir un avantage tangible. L’optimisation est assurée sans modifier l’architecture de base de l’application.
</div>

<div class="collapseContent" display="Refactorisation / réorganisation" value="true">

**La refactorisation** (également connue sous le nom de **réorganisation**) consiste à réimaginer l’architecture et le développement d’une application à l’aide de fonctions natives cloud. La refactorisation est motivée par un besoin important des entreprises d’ajouter des fonctions, de mettre à l’échelle ou de gonfler les performances ; des éléments difficiles à obtenir avec l’environnement existant de l’application.
</div>

<div class="collapseContent" display="Rachat" value="true">

**Le rachat** consiste à passer d’une licence traditionnelle à un modèle de logiciel en tant que service. 

Par exemple, une entreprise peut choisir d’implémenter la stratégie de rachat en migrant d’un système de gestion des relations clients (CRM) vers Salesforce.com.
</div>

<div class="collapseContent" display="Retenue" value="true">
  
**La retenue** consiste à conserver les applications essentielles pour l’entreprise dans l’environnement source. Cela peut inclure des applications qui requièrent une refactorisation majeure avant de pouvoir être migrées, ou des tâches qui peuvent être reportées à une date ultérieure.
</div>

<div class="collapseContent" display="Retrait" value="true">

**Le retrait** est le processus qui permet de supprimer les applications qui ne sont plus nécessaires.
</div>

## Contrôle des connaissances

**Quelle stratégie de migration implique de passer à un produit différent ?**

<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">Refactorisation</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Retrait</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Remaniement</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">Rachat</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Rachat**.

Le rachat consiste à remplacer une application existante par une version basée sur le cloud, par exemple un logiciel trouvé sur AWS Marketplace.

Les autres réponses sont incorrectes, car :

- La refactorisation consiste à modifier l’architecture et le développement d’une application, généralement à l’aide de fonctions natives cloud.
- Le retrait consiste à supprimer une application qui n’est plus utilisée ou qui peut être désactivée.
- Le remaniement consiste à optimiser, de façon sélective, les aspects d’une application pour obtenir des avantages dans le cloud sans modifier l’architecture de base de l’application. Il est également connu sous le nom de « lift, tinker, and shift ».

En savoir plus :

- [6 stratégies de migration d’applications vers le cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
</div>

# La AWS Snow Family

```
video : xxx.mkv
```
*Figure. AWS Snow Family*

<div class="collapseContent" display="Transcription" value="true">
  

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Certains de nos clients doivent déplacer des données sur AWS et la plupart d'entre eux aimeraient le faire de manière efficace et opportune. Habituellement il suffit de copier les données requises par Internet ou mieux encore, en passant par une ligne Direct Connect. Cependant, avec les limites de la bande passante, cela peut prendre des jours, des semaines, voire des mois. 
> 
> Par exemple, une connexion réseau dédiée d'un gigaoctet par seconde déplace en théorie un pétaoctet de données en environ 100 jours. Dans le monde réel c’est probablement plus long et plus cher. Avec ce retour client et afin de traiter ce problème, nous vous présentons la gamme de périphériques AWS Snow. 
> 
> Le premier périphérique qui est aussi notre nouvelle offre, est AWS Snowcone, et c’est un périphérique qui conserve jusqu’à huit téraoctets de données et contient le calcul en périphérie. Les options de calcul en périphérie sont les instances Amazon EC2 et AWS IoT Greengrass. Pour en obtenir un, vous effectuez une commande via AWS Management Console, nous vous l’envoyons, vous le branchez, copiez vos données, puis vous nous le renvoyez Nous copions ensuite les données sur votre compte AWS, généralement votre compartiment S3, et voilà, vos données sont disponibles. Les clients utilisent généralement ces périphériques pour transférer des téraoctets d’informations telles que des données analytiques, des bibliothèques vidéo, des collections d’images, des sauvegardes, et même des données de remplacement de bande. Qui ne veut pas sauvegarder une collection de téraoctets d’images de chats sur le cloud AWS ? 
> 
> Et si huit téraoctets ne suffisent pas ? Bonne nouvelle, nous proposons un produit qui satisfait à cette exigence, appelé Snowball Edge. Il est disponible en deux versions : Snowball Edge Compute Optimized et Snowball Edge Storage Optimized. Mieux encore, ils s'intègrent dans des racks de serveur existants et peuvent être regroupés en cluster pour des besoins de calcul plus importants. Lorsque vous les avez branchés dans votre infrastructure vous pouvez même exécuter des fonctions AWS Lambda, des AMI compatibles avec Amazon EC2 ou même AWS IoT Greengrass pour traiter en toute facilité les données dans l’immédiat. Les clients transfèrent habituellement ces données vers des emplacements distants, où il est difficile d’avoir beaucoup de puissance de calcul. Les cas d’utilisation incluent la capture des flux depuis des périphériques IoT, la compression d’images, le transcodage vidéo, et même la transmission industrielle. 
> 
> Le dernier périphérique est le plus grand de notre flotte, AWS Snowmobile, comme son nom l’indique, c’est un conteneur de près de 14 mètres de long tiré par un semi-remorque, c’est juste énorme ! Il héberge 100 pétaoctets et il est parfait pour les grandes migrations et même les arrêts de centres de données. Nous conduisons le semi-remorque à votre emplacement désigné. Nous le branchons et il devient un périphérique de stockage connecté au réseau d’une capacité de jusqu’à 100 pétaoctets. Il est inviolable et étanche, sa température est contrôlée. Il a même une fonction d’extinction des incendies et le suivi GPS. Pouvez-vous croire qu'il dispose aussi d’une vidéosurveillance 24h sur 24 et 7 j sur 7 avec une équipe de sécurité dédiée et un véhicule de sécurisé d’escorte pendant le transport ? C'est sérieux. 
> 
> Sachez que tous les périphériques de la gamme Snow sont conçus pour être sécurisés et inviolables sur site ou en transit. Cela signifie que le matériel et le logiciel sont signés de façon chiffrée, et toutes les données stockées sont automatiquement sécurisées par un chiffrement à 256 bits détenues et gérée par vous, le client. Vous pouvez même utiliser AWS Key Management Service pour générer et gérer les clés. 
> 
> Comme d'habitude, ça a été un plaisir de vous expliquer un autre service AWS. J'espère que vous avez pris du plaisir à découvrir la gamme AWS Snow.
</div>

## Eléments de la gamme AWS Snow

La **[gamme AWS Snow](https://aws.amazon.com/snow)** est un ensemble de périphériques physiques qui permettent de transporter physiquement des données, jusqu’à plusieurs exaoctets, vers et hors d’AWS. 

La gamme AWS Snow se compose de **AWS Snowcone**, de **AWS Snowball** et de **AWS Snowmobile**. 

![aws_snow_01.jpg](/assets/img/clouds/tempo/aws_snow_01.jpg)

Ces périphériques offrent différents points de capacité, et la plupart d’entre eux comprennent des fonctionnalités de calcul intégrées. AWS détient et gère les périphériques de la gamme Snow et intègre des fonctionnalités de sécurité, de contrôle, de gestion du stockage et de calcul AWS.  

Pour en savoir plus sur chaque catégorie, sélectionnez chaque onglet.

### Tabs {.tabset}

#### AWS Snowcone

**[AWS Snowcone](https://aws.amazon.com/snowcone)** est un petit appareil de transfert de données et de calcul en périphérie robuste et sécurisé.

![aws_snowcone_01.png](/assets/img/clouds/tempo/aws_snowcone_01.png)


Il dispose de 2 CPU, de 4 Go de mémoire et de 8 To de stockage utilisable.

#### AWS Snowball

**[AWS Snowball](https://aws.amazon.com/snowball/)** fournit deux types de périphériques :

![aws_snowball_01.png](/assets/img/clouds/tempo/aws_snowball_01.png)
![aws_snowball_02.png](/assets/img/clouds/tempo/aws_snowball_02.png)

- **les périphériques Snowball Edge Storage Optimized** conviennent aux migrations de données à grande échelle et aux flux de transfert récurrents, ainsi qu’aux besoins de calcul locaux qui exigent des capacités accrues. 
  + Stockage : 80 To de capacité de disque dur (HDD) pour les volumes par bloc et le stockage d’objets compatibles Amazon S3, et 1 To de capacité de disque SSD SATA pour les volumes par bloc. 
  + Calcul : 40 vCPU, et 80 Gio de mémoire pour prendre en charge les instances sbe1 Amazon EC2 (équivalentes aux C5).
- **Snowball Edge Compute Optimized** offre de puissantes ressources de calcul pour les cas d’utilisation tels que le machine learning, l’analyse Full Motion Video, l’analytique ou les piles de calcul locales. 
  + Stockage : 42 To de capacité HDD utilisable pour le stockage d’objets compatibles Amazon S3 ou les volumes par bloc compatibles Amazon EBS, et 7,68 To de capacité SSD NVMe utilisable pour les volumes par bloc compatibles Amazon EBS. 
  + Calcul : 52 vCPU, 208 Gio de mémoire et 1 GPU NVIDIA Tesla V100 en option. Les périphériques exécutent des instances sbe-c et sbe-g Amazon EC2, qui sont équivalentes aux instances C5, M5a, G3 et P3.

#### AWS Snowmobile

**[AWS Snowmobile](https://aws.amazon.com/snowmobile)** est un service de transfert de données d’une capacité de plusieurs exaoctets qui permet de déplacer de très grandes quantités de données vers AWS.

![aws_snowmobile_01.png](/assets/img/clouds/tempo/aws_snowmobile_01.png)

Vous pouvez transférer jusqu’à 100 pétaoctets de données par le biais de Snowmobile, un conteneur robuste de près de 14 mètres de long tiré par un semi-remorque.

## Contrôle des connaissances

**Quelle est la capacité de stockage de Snowball Edge Storage Optimized ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">40 To</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">60 To</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">80 To</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">100 To</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **80 To**.

Snowball Edge Storage Optimized est un périphérique qui vous permet de transférer de grandes quantités de données vers et hors d’AWS. Il offre 80 To de stockage HDD utilisable.

En savoir plus :

- [AWS Snow Family](https://aws.amazon.com/snow/)
</div>

# Innovation avec AWS

```
video : xxx.mkv
```
*Figure. Innovation with AWS*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Vous pouvez faire tellement plus d'actions sur AWS que celles dont nous avons le temps de parler. Par exemple, pour la migration vers AWS, nous n’avons même pas parlé de l’exécution de VMware sur AWS. La même infrastructure basée sur VMware que vous utilisez sur site peut dans de nombreux cas, simplement être déposée sur AWS, via VMware Cloud sur AWS. Ce n'est qu'un des nombreux concepts qui font d'AWS un endroit où les générateurs créent et innovent au rythme de leurs idées. 
> 
> Pour le machine learning et l’intelligence artificielle, AWS propose la gamme de services d’IA et de machine learning la plus large pour votre entreprise. Vous pouvez choisir parmi des services d'IA pré-entraînés pour l’aide visuelle par ordinateur, les recommandations linguistiques et les prévisions. Amazon SageMaker : créez, entraînez et déployez rapidement des modèles de machine learning à l’échelle. 
> 
> Ou créez des modèles personnalisés prenant en charge tous les frameworks open source courants. Nos capacités se basent sur la plateforme cloud la plus complète, optimisée pour le machine learning avec le calcul haute performance sans compromis sur la sécurité et l’analytique. Des outils comme Amazon SageMaker et Amazon Augmented AI, ou Amazon A2I, fournissent une plateforme de machine learning que toute entreprise peut exploiter sans avoir besoin d’expertise du niveau d’un doctorat en interne. Ou peut-être des solutions d’IA prêtes à l’emploi comme Amazon Lex, le cœur d'Alexa… 
> 
> [Alexa] Bonjour. Comment puis-je vous aider ? 
> 
> - … pour créer des chatbots interactifs. 
> 
> Qu'en est-il d'Amazon Textract ? Il vous permet d'extraire du texte et des données à partir de documents afin que votre entreprise puisse les utiliser au lieu d’être simplement inclus dans un référentiel. 
> 
> Voulez-vous littéralement mettre le machine learning dans les mains de vos développeurs ? Pourquoi n'essayez-vous pas AWS DeepRacer ? Il permet à vos développeurs d'expérimenter avec l’apprentissage par renforcement. C'est l'une des plus récentes branches des algorithmes de machine learning, qui permet de s’amuser dans un environnement de course. 
> 
> AWS propose de toutes nouvelles technologies pour des choses comme l'Internet des objets, ce qui permet aux dispositifs connectés de communiquer partout dans le monde. 
> 
> En parlant de communication dans le monde entier, avez-vous déjà voulu avoir votre propre satellite ? Lancer votre propre satellite coûte trop cher ? Pourquoi ne pas simplement utiliser AWS Ground Station et payer uniquement le temps de satellite dont vous avez réellement besoin ? 
> 
> Je pourrais parler de toutes ces nouvelles technologies pendant des jours. Vraiment, pendant des jours. AWS Training and Certification propose déjà des cours et formations sur bon nombre de ces technologies et, tous les mois, AWS semble lancer quelque chose d’encore mieux.
</div>

## Innover avec les services AWS

Lorsque vous étudiez comment utiliser les services AWS, il est important de vous concentrer sur les résultats souhaités. Vous êtes bien équipé pour favoriser l’innovation dans le cloud si vous êtes en mesure de clairement exprimer les conditions suivantes : 

- L’état actuel
- L’état souhaité
- Les problèmes que vous essayez de résoudre

Envisagez certains chemins à explorer à l’avenir au fur et à mesure de votre transition vers le cloud. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.


<div class="collapseContent" display="Applications sans serveur" value="true">

Avec AWS, le terme **sans serveur** fait référence aux applications pour lesquelles vous ne devez pas mettre en service, maintenir ou gérer de serveurs. Vous ne devez pas vous soucier de la tolérance aux pannes ou de la disponibilité. AWS gère ces fonctionnalités pour vous.

AWS Lambda est un exemple de service que vous pouvez utiliser pour exécuter des applications sans serveur. Si vous concevez votre architecture pour déclencher des fonctions Lambda et exécuter votre code, vous pouvez contourner la gestion d’une flotte de serveurs.

Élaborer votre architecture avec des applications sans serveur permet à vos développeurs de se concentrer sur leur produit principal, plutôt que de gérer et d’exploiter des serveurs.
</div>


<div class="collapseContent" display="Intelligence Artificielle" value="true">

AWS propose une variété de services propulsés par l’**intelligence artificielle (IA)**. 

Par exemple, vous pouvez effectuer les tâches suivantes :

- Convertir un discours en texte avec Amazon Transcribe.
- Découvrir les schémas dans du texte avec Amazon Comprehend.
- Identifier les activités en ligne potentiellement frauduleuses avec Amazon Fraud Detector.
- Créer des chatbots à communication vocale et textuelle avec Amazon Lex.
</div>

<div class="collapseContent" display="Machine Learning" value="true">

Le développement du **machine learning (ML)** traditionnel est complexe, coûteux, chronophage et sujet aux erreurs. AWS propose Amazon SageMaker afin de faciliter le processus et de vous donner les moyens de concevoir, de former et de déployer rapidement des modèles ML.

Vous pouvez utiliser le ML pour analyser des données, résoudre des problèmes complexes et estimer les résultats avant qu’ils se produisent.
</div>

## Contrôle des connaissances

**Quel service vous permet de créer, de former et de déployer rapidement des modèles de machine learning ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Amazon Textract</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">Amazon Lex</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">AWS DeepRacer</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Amazon SageMaker</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon SageMaker**.

Avec Amazon SageMaker, vous pouvez commencer rapidement et facilement à travailler sur des projets de machine learning. Vous ne devez pas rassembler manuellement des outils et des flux de travail distincts.

Les autres réponses sont incorrectes, car :

- Amazon Textract est un service de machine learning qui extrait automatiquement le texte et les données des documents numérisés.
- Amazon Lex est un service qui vous permet de créer des interfaces de conversation reposant sur la voix et le texte.
- AWS DeepRacer est une voiture de course autonome à l’échelle 1/18 que vous pouvez utiliser pour tester des modèles d’apprentissage par renforcement.

En savoir plus :

- [Amazon SageMaker](https://aws.amazon.com/sagemaker)
</div>

# Module 9 : résumé

Dans le module 9, vous avez découvert les concepts suivants :

- Le Framework d’adoption du Cloud AWS
- Les six stratégies de migration
- La AWS Snow Family
- Innovation avec les services AWS

```
video : xxx.mkv
```
*Figure. Module summary - Migration and innovation*


<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Ces dernières vidéos, vous ont permis de découvrir la migration vers AWS, ainsi que certains services innovants et intéressants que vous pouvez utiliser pour votre migration ou vos améliorations avec AWS. 
> 
> Vous avez découvert le Framework d'adoption du Cloud AWS. Le Framework d'adoption du cloud vous donne des conseils sur les personnes impliquées dans une migration cloud, leur rôle et les éléments auxquels elles doivent s’intéresser. Il y a les perspectives Entreprise, Personnes et Gouvernance pour la planification non technique, et les perspectives Plateforme, Sécurité et Opérations pour la planification technique. 
> 
> Nous avons également parlé des six « R » de la migration. Les R représentent différentes stratégies de déplacement de solutions vers le cloud. Réhéberger, remanier, racheter, refactoriser, mettre hors service et retenir. 
> 
> On s'est aussi posé la question par rapport au déplacement de quantités massives de données dans AWS, sans passer par le réseau. Vous avez alors découvert AWS Snowball et AWS Snowmobile, qui vous permettent de remplir un périphérique physique avec vos données et de le renvoyer à AWS qui s’occupe d’en charger le contenu. C'est utile pour contourner tout potentiel problème de débit, et c’est aussi plus sécurisé que d’utiliser une connexion Internet haut débit.
</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été explorés au cours du module 9, consultez ces ressources.

- [Migration et transfert sur AWS](https://aws.amazon.com/products/migration-and-transfer)
- [Un processus de migration de masse vers le cloud](https://aws.amazon.com/blogs/enterprise-strategy/214-2/)
- [6 stratégies de migration d’applications vers le cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
- [Framework d’adoption du Cloud AWS](https://aws.amazon.com/professional-services/CAF/)
- [Fondamentaux d’AWS : concepts de base](https://aws.amazon.com/getting-started/fundamentals-core-concepts/)
- [Blog sur les stratégies d’entreprise pour le Cloud AWS](https://aws.amazon.com/blogs/enterprise-strategy/)
- [Modernisation avec le blog AWS](https://aws.amazon.com/blogs/modernizing-with-aws/)
- [Témoignages de clients AWS : migration des centres de données](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23datacenter-migration)

# Module 9 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelle perspective du Framework d’adoption du Cloud AWS vous permet de structurer la sélection et l’implémentation des autorisations ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Perspective Gouvernance</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Perspective Sécurité</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Perspective Opérations</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Perspective Entreprise</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Perspective Sécurité**.

La perspective Sécurité du Framework d’adoption du Cloud AWS vous permet également d’identifier les domaines de non-conformité et de planifier des initiatives de sécurité continue.

Les autres réponses sont incorrectes, car :

- La perspective Gouvernance vous aide à identifier et à implémenter les bonnes pratiques en matière de gouvernance informatique ainsi qu’à prendre en charge les processus métier grâce à la technologie.
- La perspective Opérations met l’accent sur l’exploitation et la récupération des charges de travail informatiques afin de répondre aux besoins des parties prenantes de votre entreprise.
- La perspective Entreprise vous permet de passer d’un modèle qui sépare les stratégies informatiques et opérationnelles à un modèle opérationnel qui intègre la stratégie informatique.

En savoir plus :

- [Livre blanc : une présentation du Framework d’adoption du Cloud AWS](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
</div>

**Quelles stratégies sont incluses dans les six stratégies de migration des applications ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q6r1" name="question6" value="1">
  <label for="q6r1">Revisite</label><br>
  <input type="checkbox" id="q6r2" name="question6" value="2">
  <label for="q6r2">Retenue</label><br>
  <input type="checkbox" id="q6r3" name="question6" value="3">
  <label for="q6r3">Souvenir</label><br>
  <input type="checkbox" id="q6r4" name="question6" value="4">
  <label for="q6r4">Redéveloppement</label><br>
  <input type="checkbox" id="q6r5" name="question6" value="5">
  <label for="q6r5">Réhébergement</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Retenue**
- **Réhébergement**

Les stratégies de migration d’applications sont le réhébergement, le remaniement, la refactorisation/réorganisation, le rachat, la retenue et le retrait.

En savoir plus :

- [6 stratégies de migration d’applications vers le cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
</div>

**Quelle est la capacité de stockage d’AWS Snowmobile ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">40 Po</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">60 Po</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">80 Po</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">100 Po</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **100 Po**.

AWS Snowmobile est un service qui permet de transférer jusqu’à 100 Po de données vers AWS. Chaque Snowmobile est un conteneur de près de 14 mètres de long tiré par un semi-remorque.

En savoir plus :

- [AWS Snow Family](https://aws.amazon.com/snow)
</div>

**Quelle affirmation décrit le mieux Amazon Lex ?**

<form>
  <input type="radio" id="q8r1" name="question8" value="1">
  <label for="q8r1">Un service qui vous permet de créer des interfaces de conversation reposant sur la voix et le texte</label><br>
  <input type="radio" id="q8r2" name="question8" value="2">
  <label for="q8r2">Un service de machine learning qui extrait automatiquement du texte et des données depuis des documents numérisés</label><br>
  <input type="radio" id="q8r3" name="question8" value="3">
  <label for="q8r3">Un service de base de données documentaire compatible avec les charges de travail MongoDB</label><br>
  <input type="radio" id="q8r4" name="question8" value="4">
  <label for="q8r4">Un service qui vous permet d’identifier les activités en ligne potentiellement frauduleuses</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Lex**.

Dans Amazon Lex, vous pouvez rapidement créer, tester et déployer des chatbots conversationnels pour vos applications.

Les autres réponses sont incorrectes, car :

- Un service de machine learning qui extrait automatiquement du texte et des données du document numérisé décrit Amazon Textract.
- Un service de base de données documentaire compatible avec les charges de travail MongoDB décrit Amazon DocumentDB.
- Un service qui vous permet d’identifier les activités en ligne potentiellement frauduleuses décrit Amazon Fraud Detector.

En savoir plus :

- [Amazon Lex](https://aws.amazon.com/lex)
</div>

Le module suivant porte sur le framework AWS Well-Architected et les avantages de AWS Cloud.