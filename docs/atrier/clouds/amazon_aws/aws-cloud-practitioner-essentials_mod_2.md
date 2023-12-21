---
title: Module 2 - calcul dans le cloud
description: 
published: true
date: 2023-04-21T10:55:52.649Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:55:45.101Z
---

# Module 2 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Décrire les avantages d’**Amazon EC2** à un niveau de base
- Identifier les différents types d’**instances Amazon EC2**
- Faire la distinction entre les diverses options de facturation pour **Amazon EC2**
- Résumer les avantages d’**Amazon EC2 Auto Scaling**
- Résumer les avantages d’**Elastic Load Balancing**
- Donner un exemple d’utilisation d’**Elastic Load Balancing**
- Résumer les différences entre **Amazon Simple Notification Service (Amazon SNS)** et **Amazon Simple Queue Service (Amazon SQS)**
- Résumer les options de calcul AWS supplémentaires

```
video : xxx.mkv
```
*Figure. Introduction to Amazon EC2*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Dans cette vidéo, nous allons aborder en profondeur le service **Amazon Elastic Compute Cloud** ou **EC2**. Souvenez-vous de notre café, les employés sont une métaphore du modèle client/serveur où un client envoie une demande au serveur, le serveur travaille, puis envoie une réponse. Cet exemple est pour le café. Mais la même idée s’applique à d’autres entreprises. Votre entreprise, que ce soit dans le secteur de la santé, de la fabrication, de l’assurance, ou de la diffusion de contenu vidéo à des millions d’utilisateurs dans le monde, utilise également ce modèle pour livrer des produits, des ressources, ou des données à vos utilisateurs finaux. Et vous allez avoir besoin de serveurs pour alimenter votre entreprise et vos applications. Vous avez besoin d’une capacité brute de calcul pour héberger vos applications et offrir la puissance de calcul requise par votre entreprise. Lorsque vous travaillez avec AWS, ces serveurs sont virtuels. Et le serveur que vous utilisez pour accéder aux serveurs virtuels s’appelle **EC2**. 
> 
> L’utilisation d’**EC2** pour le calcul est très flexible, rentable et rapide par rapport à l’utilisation de vos propres serveurs sur site dans un centre de données vous appartenant. Le temps et l’argent nécessaires pour être opérationnels avec des ressources sur site sont assez élevés. Lorsque vous possédez une flotte de serveurs physiques, vous devez d'abord effectuer un tas de recherches pour savoir quels types de serveurs acheter et de combien vous aurez besoin. Ensuite, vous achetez ce matériel à l'avance. Vous attendez plusieurs semaines ou mois que le fournisseur vous livre ces serveurs. Puis, vous les emmenez dans un centre de données que vous possédez ou louer pour les installer, les empiler et tous les brancher. Ensuite, vous vous assurez qu'ils sont sécurisés et mis sous tension puis ils sont prêts à être utilisés. C'est seulement là que vous pouvez héberger vos applications sur ces serveurs. Le pire, c'est qu'une fois achetés, vous les avez sur les bras que vous les utilisiez ou non. 
> 
> Avec **EC2**, il est beaucoup plus simple de se lancer. AWS s'est déjà chargée de la partie la plus compliquée. AWS a déjà construit et sécurisé les centres de données, AWS a déjà acheté les serveurs, les a mis en rack et les a empilés, et ils sont déjà en ligne, prêts à être utilisés. AWS traite en permanence une quantité massive de capacité de calcul. Et vous pouvez utiliser n’importe quelle portion de cette capacité quand vous en avez besoin. Il suffit de demander les **instances EC2** que vous souhaitez et elles seront lancées et démarrées, prêtes à être utilisées en quelques minutes. Une fois terminé, vous pouvez facilement arrêter ou résilier les **instances EC2**. Vous n’êtes pas enfermé ou coincé avec des serveurs dont vous n’avez pas besoin ou ne voulez pas. Votre utilisation des **EC2** peut varier considérablement au fil du temps. Vous ne payez que ce que vous utilisez. Avec **EC2**, vous ne payez en effet que les instances en cours d’exécution, et non les instances en arrêt ou résiliées. 
> 
> **EC2** s’exécute sur des machines hôtes physiques gérées par AWS avec la technologie de virtualisation. Lorsque vous lancez une **instance EC2**, vous ne prenez pas nécessairement un hôte entier pour vous. Au lieu de cela, vous partagez l’hôte avec plusieurs autres instances. On les appelle également machines virtuelles. Un hyperviseur qui s’exécute sur la machine hôte permet le partage des ressources physiques sous-jacentes entre les machines virtuelles. Cette idée de partager le matériel sous-jacent s’appelle la multilocation. L’hyperviseur est chargé de coordonner cette multilocation et il est géré par AWS. L’hyperviseur isole les machines virtuelles les unes des autres et elles partagent des ressources de l’hôte. Cela signifie que les **instances EC2** sont sécurisées. Même si elles partagent des ressources, une **instance EC2** ne connaît pas les autres **instances EC2** également présentes sur cet hôte. Elles sont sécurisées et séparées les unes des autres. 
> 
> Heureusement, ce n'est pas quelque chose que vous devez configurer vous-même. Il est important de connaître le concept de multilocation et de bien en connaître le fonctionnement. **EC2** vous offre beaucoup de flexibilité et de contrôle. Non seulement vous pouvez lancer de nouveaux serveurs ou les mettre hors ligne à volonté, mais vous disposez aussi de flexibilité et de contrôle pour configurer ces instances. 
> 
> Lorsque vous mettez en service une **instance EC2**, vous pouvez choisir le système d’exploitation basé sur Windows ou Linux. Vous pouvez mettre en service des milliers d'**instances EC2** à la demande. Vous disposez de divers systèmes d’exploitation et configurations pour soutenir les différentes applications de votre entreprise. 
> 
> En plus du système d’exploitation, vous configurez également le logiciel que vous souhaitez exécuter sur l’instance. Qu’il s’agisse de vos propres applications commerciales, d’applications web simples, ou complexes, de bases de données ou de logiciels tiers comme les packages logiciels d’entreprise, vous avez le contrôle complet sur ce qui se passe sur cette instance. Les **instances EC2** sont également redimensionnables. Vous pouvez commencer par une **instance Small**, vous rendre compte que l’application que vous exécutez commence à atteindre la limite de ce serveur, et vous pouvez donner plus de mémoire à cette instance, et plus de CPU. C'est ce que nous appelons la mise à l'échelle verticale d'une instance. 
> 
> En substance, vous pouvez créer des instances plus grandes ou plus petites chaque fois que vous en avez besoin. Vous contrôlez également la partie mise en réseau d'**EC2**. Ainsi, c’est à vous de décider quels types de demandes vont sur le serveur, et si elles sont accessibles de manière publique ou privée. 
> 
> Nous aborderons cette partie plus en détail plus tard dans le cours. Les machines virtuelles ne sont pas une invention récente. Mais la facilité de mise en service des **instances EC2** permet aux programmeurs et aux entreprises d’innover plus rapidement. AWS vient de rendre les choses beaucoup plus simples et plus rentables pour accéder aux serveurs par le biais de ce modèle **Compute as a Service (CaaS)**. Il y a beaucoup d'autres choses à apprendre sur **EC2**. Nous avons parlé de la virtualisation et des types de logiciels que vous pouvez exécuter sur une **instance EC2**. Mais **EC2** vous permet de configurer bien plus encore.
</div>
  
## Amazon Elastic Compute Cloud (Amazon EC2)

[Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2/) fournit une capacité de calcul sécurisée et redimensionnable dans le cloud en tant qu’instances Amazon EC2. 

Imaginez que vous êtes responsable de l’architecture des ressources de votre entreprise et que vous avez besoin de prendre en charge de nouveaux sites web. Avec les ressources sur site traditionnelles, vous devez effectuer les opérations suivantes :
 
- Faire des dépenses initiales pour acheter du matériel
- Attendre que les serveurs vous soient livrés
- Installer les serveurs dans votre centre de données physique
- Faire toutes les configurations nécessaires

En comparaison, avec une instance Amazon EC2, vous pouvez utiliser un serveur virtuel pour exécuter des applications dans AWS Cloud.

- Vous pouvez mettre en service et lancer une instance Amazon EC2 en quelques minutes.
- Vous pouvez cesser de l’utiliser lorsque vous avez terminé d’exécuter une charge de travail.
- Vous payez uniquement le temps de calcul que vous utilisez lorsqu’une instance est en cours d’exécution, et non lorsqu’elle est arrêtée ou résiliée.
- Vous pouvez économiser sur ces coûts en payant uniquement la capacité de serveur dont vous avez besoin ou que vous souhaitez.

## Fonctionnement d’Amazon EC2

Pour en savoir plus, sélectionnez chaque marqueur.

![fonctionnement_ec2.jpg](/assets/img/clouds/tempo/fonctionnement_ec2.jpg =750x)

#### Tabs {.tabset}
##### Lancement

Tout d'abord, lancer une instance.
Commencez par sélectionner un modèle avec des configurations de base pour votre instance. Ces configurations comprennent le système d'exploitation, le serveur d'applications ou les applications.
Sélectionnez également le type d'instance, qui correspond à la configuration matérielle spécifique de votre instance.

Lorsque vous vous apprêtez à lancer une instance, vous spécifiez des paramètres de sécurité pour contrôler le trafic réseau pouvant entrer ou sortir de votre instance.
Plus tard dans ce cours,  nous explorerons plus en détail les fonctions de sécurité d'**Amazon EC2**.

##### Connexion

Connectez-vous à l'instance. Vous pouvez vous connecter à l'instance de plusieurs manières.
Vos programmes et applications disposent de plusieurs méthodes différentes pour se connecter directement à l'instance et échanger des données. Les utilisateurs peuvent également se connecter à l'instance en se connectant et en accédant au bureau de l'ordinateur.

##### Utilisation

Une fois que vous vous êtes connectés à l'instance, vous pouvez commencer à l'utiliser. Vous pouvez exécuter des commandes pour installer un logiciel, ajouter du stockage, copier et organiser des fichiers, etc.

# Types d'instances Amazon EC2

```
video : xxx.mkv
```
*Figure. A little more about Amazon EC2*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Maintenant que nous avons étudié les **instances EC2** et le rôle crucial qu’elles jouent dans AWS, parlons des différents types d’**instances EC2** disponibles. En reprenant notre analogie du café, vous vous souvenez que les **instances EC2** sont comme nos employés et qu’elles répondent aux demandes des clients. Si nous souhaitons que le café serve de nombreux clients, nous allons probablement avoir besoin de beaucoup d’employés, n’est-ce pas ? Et nous n'avons pas uniquement besoin de caissiers. Nous avons aussi besoin de quelqu’un chargé des boissons, de quelqu’un en charge de la nourriture, et peut-être de quelqu’un qui s’occupe du latte art dont raffolent nos clients. Comme dans toute entreprise, une grande variété de tâches doit être exécutée, faisant souvent appels à des compétences différentes. 
> 
> Si nous voulons que notre entreprise fonctionne aussi efficacement que possible, il est important de s’assurer que les compétences de chaque employé correspondent à son poste. Comme notre café qui a différents types d’employés, AWS dispose de différents types d’**instances EC2** que vous pouvez lancer et déployer dans votre environnement AWS. 
> 
> Chaque type d’instance est regroupé en famille d’instance qui est optimisée pour certains types de tâches. Ces types d’instances correspondent à différentes combinaisons en termes de capacités de processeur, de mémoire, de stockage, et de mise en réseau. Vous pouvez ainsi choisir un ensemble de ressources parfaitement adapté à vos applications. Les différentes familles d’**instance de EC2** sont les suivantes : **polyvalentes (General Purpose)**, **calcul optimisé (Compute optimized)**, **mémoire optimisée (Memory optimized)**, **calcul accéléré (Accelerated computing)**, et **stockage optimisé (Storage optimized)**. 
> 
> Les **instances à usage général** assurent l’équilibre entre les ressources informatiques, la mémoire et les ressources réseau, et elles peuvent être utilisées pour un certain nombre de charges de travail diverses comme le service web ou les référentiels de code. 
> 
> Les **instances de calcul optimisées** sont idéales pour les tâches gourmandes en calcul comme les serveurs de jeux, le calcul haute performance (HPC), et même la modélisation scientifique. 
> 
> De même, les **instances de mémoire optimisées** sont adaptées aux tâches gourmandes en mémoire. Le calcul accéléré convient au calcul du nombre flottant, au traitement graphique, ou au filtrage du schéma de données, car ils utilisent des accélérateurs matériels. 
> 
> Enfin, le **stockage optimisé** est adapté à... à votre avis ? Les charges de travail qui nécessitent des performances élevées pour les données stockées localement. 
> 
> Pour en revenir à notre café, notre caissier est une **instance EC2** optimisée en mémoire, les baristas sont des instances optimisées en calcul, et notre employé consacré au latte art est un type d’instance de calcul accéléré. Et maintenant vous connaissez tous les types d’**instance EC2**.
</div>

## Types d’instances Amazon EC2

[Les types d’instances Amazon EC2](https://aws.amazon.com/ec2/instance-types/) sont optimisés pour différentes tâches. Lorsque vous sélectionnez un type d’instance, tenez compte des besoins spécifiques de vos charges de travail et applications. Cela peut inclure des exigences pour les capacités de calcul, de mémoire ou de stockage.

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Instances à usage général" value="true">

**Les instances à usage général** fournissent un équilibre entre les ressources de calcul, de mémoire et de réseaux. Vous pouvez les utiliser pour une variété de charges de travail, telles que :

- les serveurs d’applications
- les serveurs de jeux
- les serveurs backend pour les applications d’entreprise
- les bases de données petites et moyennes

Supposons que vous ayez une application dans laquelle les besoins en ressources pour le calcul, la mémoire et la mise en réseau sont à peu près équivalents. Vous pouvez envisager de l’exécuter sur une instance à usage général, car l’application ne nécessite aucune optimisation dans une seule zone de ressources.
</div>

<div class="collapseContent" display="Instances de calcul optimisées" value="true">

**Les instances de calcul optimisées** sont idéales pour les applications de calcul qui utilisent des processeurs hautes performances. Comme les instances à usage général, vous pouvez utiliser des instances de calcul optimisées pour des charges de travail telles que des serveurs web, d’applications et de jeux.

Cependant, la différence est que les applications de calcul optimisées sont idéales pour les serveurs web hautes performances, les serveurs d’applications exigeantes en calcul et les serveurs de jeux dédiés. Vous pouvez également utiliser des instances de calcul optimisées pour les charges de travail de traitement par lots qui nécessitent le traitement de nombreuses transactions dans un seul groupe.
</div>
  
<div class="collapseContent" display="Instances de mémoire optimisée" value="true">

**Les instances optimisées en mémoire** sont conçues pour fournir des performances rapides pour les charges de travail qui traitent de grands jeux de données en mémoire. En informatique, la mémoire est une zone de stockage temporaire. Elle contient toutes les données et instructions dont une unité centrale de traitement (CPU) a besoin pour effectuer des actions. Avant qu’un programme ou une application ne puisse s’exécuter, ils sont stockées dans la mémoire. Ce processus de préchargement donne au CPU un accès direct au programme informatique.

Supposons que vous ayez une charge de travail qui nécessite le préchargement de grandes quantités de données avant l’exécution d’une application. Ce scénario peut être une base de données hautes performances ou une charge de travail impliquant le traitement en temps réel d’une grande quantité de données non structurées. Dans ces types de cas d’utilisation, envisagez d’utiliser une instance optimisée en mémoire. Les instances optimisées en mémoire vous permettent d’exécuter des charges de travail avec des besoins de mémoire élevés et de bénéficier de performances exceptionnelles.
</div>

<div class="collapseContent" display="Instances de calcul accéléré" value="true">

**Les instances de calcul accéléré** utilisent des accélérateurs matériels, ou coprocesseurs, pour exécuter certaines fonctions plus efficacement que ce qui est possible dans les logiciels s’exécutant sur des CPU. Les exemples de ces fonctions incluent les calculs de nombres à virgule flottante, le traitement de graphiques et l’appariement des schémas de données.

En informatique, un accélérateur matériel est un composant qui peut accélérer le traitement des données. Les instances de calcul accéléré sont idéales pour les charges de travail telles que les applications graphiques, le streaming de jeux et le streaming d’applications.
</div>

<div class="collapseContent" display="Instances optimisées pour le stockage" value="true">

**Les instances optimisées pour le stockage** sont conçues pour les charges de travail qui nécessitent un accès élevé en lecture et en écriture séquentielle à de grands jeux de données en stockage local. Les exemples de charges de travail adaptées aux instances optimisées pour le stockage comprennent les systèmes de fichiers distribués, les applications de stockage de données, ainsi que les systèmes de traitement transactionnel en ligne (OLTP) à fréquence élevée.

En calcul, le terme opérations d’entrée/de sortie par seconde (IOPS) est une métrique qui mesure les performances d’un périphérique de stockage. Elle indique le nombre d’opérations d’entrée ou de sortie différentes qu’un périphérique peut effectuer en une seconde. Les instances optimisées pour le stockage sont conçues pour fournir des dizaines de milliers d’IOPS aléatoires à faible latence aux applications. 

Vous pouvez considérer les opérations d’entrée comme des données placées dans un système, telles que des enregistrements saisis dans une base de données. Une opération de sortie consiste en des données générées par un serveur. Un exemple de sortie peut être l’analytique effectuée sur les enregistrements d’une base de données. Si vous avez une application qui a un besoin élevé d’IOPS par seconde, une instance optimisée pour le stockage peut fournir de meilleures performances par rapport aux autres types d’instances non optimisés pour ce type de cas d’utilisation.
</div>

## Contrôle des connaissances

Faites correspondre chaque description à un type d’instance Amazon EC2.

|:--:|:----|
|**A**|Equilibre les ressources de calcul, de mémoire et de mise en réseau|
|**B**|Propose des processeurs hautes performances|
|**C**|Convient aux applications de stockage de données|
|**D**|Idéal pour les bases de données hautes performances|


|:--:|:----|
|**1**|Optimisée en mémoire|
|**2**|Optimisée pour le stockage|
|**3**|A usage général|
|**4**|Optimisée pour le calcul|


<div class="collapseContent" display="Solution" value="true">

A = A usage général
B = Optimisée pour le calcul
C = Optimisée pour le stockage
D = Optimisée en mémoire

</div>
  
  
# Tarification Amazon EC2

```
video : xxx.mkv
```
*Figure. Amazon EC2 pricing*

<div class="collapseContent" display="Transcription" value="true">
  
> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Nous avons abordé les types d'**instance EC2**, mais vous vous demandez probablement combien cela va vous coûter ? N'ayez pas d'inquiétude. Pour **EC2**, nous avons plusieurs options de facturation disponibles. 
> 
> La première, et la plus connue, est l’option de **paiement à la demande**. Ce qui signifie que vous payez uniquement pour la durée d’exécution de votre instance. Cela peut être par heure ou par seconde, en fonction du type d’instance et du système d’exploitation que vous choisissez d’exécuter. Aucun engagement à long terme ni paiement à l’avance ne sont requis. Ce type de tarification est généralement utilisé lorsque vous débutez et que vous souhaitez lancer les serveurs pour tester les charges de travail et faire des essais. Vous n’avez pas besoin de contrat préalable ni de communiquer avec AWS pour utiliser la tarification à la demande. Vous pouvez également utiliser cette option pour obtenir une idée de votre utilisation moyenne, ce qui nous conduit à notre option de tarification suivante : **Savings Plan**. 
> 
> **Savings Plan** offre des prix réduits pour l’utilisation d’**EC2** si vous vous engagez à utiliser en continu une certaine quantité mesurée en dollar par heure pour une durée d’un à trois ans. Ce modèle de tarification flexible contribue ainsi à économiser jusqu’à 72 % sur votre utilisation des services de calcul AWS. Cela peut faire baisser les prix de votre utilisation **EC2**, quelle que soit la famille d’instance, la taille, le système d’exploitation, la location, ou la **région AWS**. Cela s’applique également à l’utilisation d’**AWS Fargate** et **AWS Lambda**, qui sont des options de calcul sans serveur et que nous aborderons plus tard dans ce cours. 
> 
> Comme autre option, nous avons les **instances réservées**. Ces dernières sont adaptées aux charges de travail constantes ou celles ayant une utilisation prévisible et elle vous garantit jusqu’à 75 % de réduction par rapport à la tarification à la demande. Vous avez droit à une réduction si vous vous engagez à une utilisation de un à trois ans, et vous pouvez choisir entre trois options de paiement : paiement total anticipé, où vous payez l'intégralité au moment de l'engagement ; des frais initiaux partiels, où vous payez pour une portion lorsque vous vous engagez ; et aucun frais initial, où vous ne versez rien du tout au départ.
> 
> L’option suivante, les **instances Spot**, vous permet d’enchérir sur les capacités de calcul **Amazon EC2** non utilisées pour économiser jusqu’à 90 % du prix des instances à la demande. Le problème est que AWS peut récupérer l’instance dès qu’elle en a besoin, et vous disposez de deux minutes pour terminer votre travail et enregistrer l’état. Vous pouvez toujours reprendre plus tard si nécessaire. Ainsi, lorsque vous choisissez des **instances Spot**, assurez-vous que vous pouvez vous permettre d’interrompre vos charges de travail. Les charges de travail par lot sont un bon exemple. 
> 
> Et enfin, nous avons **les hôtes dédiés**, qui sont des hôtes physiques dédiés à votre utilisation **EC2**. Ils sont généralement utilisés afin de respecter certaines exigences de conformité et personne ne sera autorisé à partager la location de cet hôte.
</div>

## Tarification Amazon EC2

Avec Amazon EC2, vous payez uniquement le temps de calcul que vous utilisez. Amazon EC2 offre une variété d’options de tarification pour différents cas d’utilisation. Par exemple, si votre cas d’utilisation peut supporter les interruptions, vous pouvez faire des économies avec les instances Spot. Vous pouvez également réaliser des économies en vous engageant tôt et en verrouillant un niveau d’utilisation minimal avec les instances réservées.

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="A la demande" value="true">

**Les instances à la demande** sont idéales pour les charges de travail irrégulières à court terme qui ne peuvent être interrompues. Aucun coût initial ou contrat minimum ne s’applique. Les instances s’exécutent en continu jusqu’à ce que vous les arrêtiez, et vous payez uniquement le temps de calcul que vous utilisez.

Les exemples de cas d’utilisation pour les instances à la demande incluent le développement et les tests d’applications, ainsi que l’exécution d’applications ayant des schémas d’utilisation imprévisibles. Les instances à la demande ne sont pas recommandées pour les charges de travail qui durent un an ou plus, car ces charges de travail peuvent bénéficier d’économies plus importantes grâce aux instances réservées.
</div>

<div class="collapseContent" display="Amazon EC2 Savings Plans" value="true">

AWS propose des Savings Plans pour plusieurs services de calcul, y compris Amazon EC2. **Les Savings Plans Amazon EC2** vous permettent de réduire vos coûts de calcul en vous engageant à utiliser le calcul de manière régulière pour une période de 1 an ou de 3 ans. Cet engagement se traduit par des économies allant jusqu’à 72 % par rapport aux coûts à la demande.

Toute utilisation jusqu’à l’engagement est facturée au taux réduit du **Savings Plan** (par exemple, 10 USD l’heure). Toute utilisation au-delà de l’engagement est facturée aux tarifs à la demande réguliers.

Ultérieurement dans ce cours, vous étudierez **AWS Cost Explorer**, un outil qui vous permet de visualiser, de comprendre et de gérer vos coûts et votre utilisation AWS au fil du temps. Si vous réfléchissez aux options des **Savings Plans**, **AWS Cost Explorer** peut analyser votre utilisation d’**Amazon EC2** au cours des 7, 30 ou 60 derniers jours. **AWS Cost Explorer** fournit également des recommandations personnalisées pour les **Savings Plans**. Ces recommandations évaluent le montant que vous pourriez économiser sur vos coûts mensuels **Amazon EC2**, en fonction de l’utilisation précédente d’**Amazon EC2** et du montant de l’engagement horaire d’un **Savings Plan** d’un an ou de trois ans.
</div>

<div class="collapseContent" display="Instances réservées" value="true">

**Les instances réservées** constituent une réduction de facturation appliquée à l’utilisation des **instances à la demande** dans votre compte. Vous pouvez acheter des **instances réservées standard** et des **instances réservées convertibles** pour une période d’un ou de trois ans, et des **instances réservées programmées** pour une période d’un an. Vous réalisez des économies plus importantes grâce à l’option de 3 ans.

A la fin d’une période d’**instance réservée**, vous pouvez continuer à utiliser l’**instance Amazon EC2** sans interruption. Toutefois, des tarifs à la demande vous sont facturés jusqu’à ce que vous effectuez l’une des opérations suivantes :

- Résilier l’instance.
- Acheter une nouvelle instance réservée qui correspond aux attributs de l’instance (type d’instance, région, location et plateforme).
</div>

<div class="collapseContent" display="Instances Spot" value="true">

**Les instances Spot** sont idéales pour les charges de travail dont les heures de début et de fin sont flexibles, ou qui peuvent supporter les interruptions. Les **instances Spot** sont des capacités de calcul **Amazon EC2** inutilisées et vous offrent des économies de coûts allant jusqu’à 90 % des prix à la demande.

Supposons que vous ayez une tâche de traitement en arrière-plan qui peut démarrer et s’arrêter au besoin (tel que la tâche de traitement de données pour une enquête client). Vous souhaitez démarrer et arrêter la tâche de traitement sans affecter l’ensemble des opérations de votre entreprise. Si vous effectuez une demande Spot et que la capacité **Amazon EC2** est disponible, votre **instance Spot** est lancée. Toutefois, si vous effectuez une demande **Spot** et que la capacité **Amazon EC2** n’est pas disponible, la demande ne réussit pas tant que la capacité n’est pas redevenue disponible. La capacité indisponible peut retarder le lancement de votre tâche de traitement en arrière-plan.

Après avoir lancé une **instance Spot**, si la capacité n’est plus disponible ou si la demande d’**instances Spot** augmente, votre instance peut être interrompue. Cela peut ne pas poser de problème pour votre tâche de traitement en arrière-plan. Cependant, dans l’exemple précédent de développement et de test d’applications, vous voudrez probablement éviter des interruptions inattendues. Dans ce cas, choisissez un type d’**instance EC2** différent et idéal pour ces tâches.
</div>

<div class="collapseContent" display="Hôtes dédiés" value="true">

**Les hôtes dédiés** sont des serveurs physiques avec des capacités d’instance **Amazon EC2 dédiées** à votre utilisation. 

Vous pouvez utiliser vos licences logicielles par socket, par nœud ou par machine virtuelle existantes pour vous aider à maintenir la conformité des licences. Vous pouvez acheter des réservations d’hôtes dédiés à la demande et d’hôtes dédiés. De toutes les options **Amazon EC2** couvertes, les hôtes dédiés sont les plus chers.
</div>

## Contrôle des connaissances

**Quelle est la différence entre les Savings Plans Amazon EC2 et les instances Spot ?**

Sélectionnez chaque carte-mémoire pour la retourner.

![card_saving_plans_01.png](/assets/img/clouds/tempo/card_saving_plans_01.png)

![card_saving_plans_02.png](/assets/img/clouds/tempo/card_saving_plans_02.png)

![card_spot_01.png](/assets/img/clouds/tempo/card_spot_01.png)

![card_spot_02.png](/assets/img/clouds/tempo/card_spot_02.png)

# Mise à l’échelle Amazon EC2

```
video : xxx.mkv
```
*Figure. Scaling Amazon EC2 - Part 1*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Nous avons maintenant une bonne idée des bases de EC2 et de la manière dont ce service peut être utile pour tous les besoins de calcul, comme faire du café. Métaphoriquement, j'entends. Le café représente ce que votre instance produit. Le prochain élément que nous allons aborder est l’un des autres avantages majeurs d’AWS, la scalabilité et l’élasticité, ou comment la capacité peut augmenter ou diminuer, en fonction des besoins de l’entreprise. 
> 
> Voici le dilemme du centre de données sur site. Si votre entreprise est comme 99 % des entreprises dans le monde, la charge de travail liée aux clients varie dans le temps : peut-être sur une période de 24 heures seulement, ou vous êtes peut-être occupé à des saisons spécifiques, et certaines semaines sont plutôt tranquilles. Si vous construisez un centre de données, la question est, quelle quantité de matériel est-il nécessaire d’acheter ? Si vous achetez pour le montant moyen, l’utilisation moyenne, vous ne gaspillerez pas d’argent en moyenne. Mais quand les pics de travail arrivent, vous ne disposerez pas du matériel pour répondre à vos clients, en particulier aux moments cruciaux qui produisent généralement la majorité de vos résultats. 
> 
> Cependant, si vous optez pour la charge maximale supérieure, vos clients seront satisfaits, mais la plupart du temps au cours de l’année, vos ressources seront inutilisées. Ce qui signifie que votre moyenne d'utilisation est très faible, Et j’ai vu des centres de données dont le taux d’utilisation moyenne est inférieur à 10 %, juste par peur de manquer le pic de demande. Alors, comment résoudre ce problème sur site. En vérité, il n'y a pas de solution. Voici en quoi AWS change complètement la donne. 
> 
> Et si vous pouviez ajuster exactement votre charge de travail à la demande, chaque jour, à chaque heure de la journée ? Vous avez maintenant des clients satisfaits, parce qu’ils obtiennent toujours le service qu’ils souhaitent. Et cela rend heureux votre directeur financier parce que le RSI (retour sur investissement) obtenu correspond aux objectifs de la société. 
> 
> Voici comment cela fonctionne. Morgan est derrière le comptoir, Il prend les commandes et nous avons un système découplé. Morgan ne s’occupe pas de tout, et nous avons donc besoin de quelqu’un qui se charge des boissons. Rudy semble pouvoir s'en occuper. Alors le premier problème que je veux résoudre est l’idée de se préparer au pire. Une fantastique citation de Werner Vogels dit : « Tout échoue tout le temps, alors planifiez l’échec et rien n’échoue » En d’autres termes, nous nous demandons ce qui se passerait si l’on perdait notre instance de prise de commandes. Eh bien, nous serions hors-jeu jusqu’à ce que nous ayons une autre personne pour travailler, ou plutôt une autre instance en cours d’exécution. 
> 
> Voici en quoi AWS rend les choses très simples. En utilisant la même méthode de programmation qui nous a permis de créer la Morgan d’origine, nous pouvons créer une seconde Morgan. En l’absence de l’un d’eux, nous disposons d’un autre élément déjà en place qui prend les commandes. Les clients ne perdent jamais l'accès au service. N'oubliez pas le backend. Faisons de même avec nos instances de traitement. Nous obtenons alors notre capacité d'exploitation moyenne. Nous disposons à présent d’un système hautement disponible, avec aucun point de défaillance. Et aussi longtemps que le nombre des clients de la file ne change pas, tout va bien. Mais vous savez que cela va finir par changer, n'est-ce pas ? Jetons donc un coup d’œil à ce qu’il se passe en cas de pic de clients, d’augmentation de la demande.
</div>
  

## Mise à l’échelle

**La mise à l’échelle** implique de commencer uniquement avec les ressources dont vous avez besoin et de concevoir votre architecture pour répondre automatiquement à l’évolution de la demande avec une mise à l’échelle ascendante ou descendante. Par conséquent, vous ne payez que les ressources que vous utilisez. Vous n’avez pas à vous soucier d’un manque de capacité de calcul pour répondre aux besoins de vos clients.

Si vous souhaitez que le processus de mise à l’échelle soit automatique, quel service AWS utiliseriez-vous ? Le service AWS qui fournit cette fonctionnalité pour les instances Amazon EC2 est **Amazon EC2 Auto Scaling**.

## Amazon EC2 Auto Scaling

![aws_ec2_auto_scaling_01.png](/assets/img/clouds/tempo/aws_ec2_auto_scaling_01.png)

Si vous avez essayé d’accéder à un site web qui ne se chargeait pas et qui expirait souvent, il est possible que le site web ait reçu plus de demandes qu’il ne pouvait traiter. Cette situation est similaire au fait d’attendre dans une longue file d’attente d’un café, où il n’y a qu’un seul barista présent pour prendre les commandes des clients.

![autoscaling_01.png](/assets/img/clouds/tempo/autoscaling_01.png =500x)

**Amazon EC2 Auto Scaling** vous permet d’ajouter ou de supprimer automatiquement des **instances Amazon EC2** en réponse à l’évolution de la demande d’applications. En mettant automatiquement à l’échelle vos instances en fonction des besoins, vous pouvez conserver une meilleure disponibilité des applications.

Avec **Amazon EC2 Auto Scaling**, vous pouvez utiliser deux approches : la **scalabilité dynamique** et la **scalabilité prédictive**.

- La **scalabilité dynamique** répond à l’évolution de la demande. 
- La **scalabilité prédictive** planifie automatiquement le bon nombre d’**instances Amazon EC2** en fonction de la demande prévue.

> Pour mettre à l’échelle plus rapidement, vous pouvez utiliser la scalabilité dynamique et la scalabilité prédictive ensemble.
{.is-info}

```
video : xxx.mkv
```
*Figure. Scaling Amazon EC2 - Part 2*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Il existe deux manières pour faire face à des demandes croissantes. Vous pouvez effectuer une mise à échelle ascendante ou horizontale. Une mise à échelle ascendante signifie ajouter de la puissance aux machines en cours d’exécution, ce qui peut être utile dans certains cas mais réfléchissez-y. Lorsque vous avez une augmentation du nombre de clients, une instance plus importante de Morgan ne peut pas vraiment prendre la commande d’un client plus rapidement, car cela dépend davantage du client que de Morgan. 
> 
> « Je voudrais un expresso. Attendez, est-ce que c'est du café bio ? Est-ce que vous faites du latte au soja ? En fait, je ne sais pas. Est-ce que vous auriez simplement du thé ? »
> 
> Ce qu'il nous faut en réalité, c'est plus de Morgan. Des clients ? Oh, non ! On dirait que les instances de traitement sont sur le point d’être surchargées. Nous allons les mettre à l'échelle aussi. 
> 
> La question évidente est évidente. Comment est-ce possible qu’il y ait plus d’instances de prise de commande que d’instances de préparation de commande ? 
> 
> Dans ce cas, la quantité de travail que vous êtes capable d'effectuer reste supérieure à ce que peuvent vous transmettre les machines de commande. Vous n'avez pas de file d'attente. Donc, aucune raison d'ajouter plus d'instances de travail. C'est l'un des grands avantages du découplage du système. Vous pouvez obtenir exactement le niveau de puissance requis pour chaque partie du processus, plutôt que d’allouer excessivement pour résoudre un problème distinct. Bien, on dirait que nous venons de résoudre cette vague. 
> 
> Vous voyez désormais en quoi AWS fait vraiment la différence pour votre entreprise. Tous ces travailleurs supplémentaires qui sont assis et inactifs, si vous n’en avez pas besoin, vous les renvoyez chez eux ou vous arrêtez les instances. **Amazon EC2 Auto Scaling**. Ajoute des instances en fonction de la demande puis désactive des instances quand elles ne sont plus nécessaires. Cela signifie qu’à chaque minute de la journée, vous disposez toujours du nombre correct d’instances. Des clients heureux. Un directeur financier heureux. Une architecture heureuse.
</div>

## Exemple : Amazon EC2 Auto Scaling

Dans le cloud, la puissance de calcul est une ressource programmable, ainsi vous pouvez adopter une approche plus souple pour la mise à l’échelle. En ajoutant **Amazon EC2 Auto Scaling** à une application, vous pouvez ajouter de nouvelles instances à l’application si nécessaire et les résilier lorsqu’elles ne sont plus nécessaires.

Supposons que vous vous apprêtiez à lancer une application sur des instances Amazon EC2. Lorsque vous configurez la taille de votre groupe Auto Scaling, vous pouvez fixer le nombre minimal d’instances Amazon EC2 à un. Cela signifie qu’en tout temps, il doit y avoir au moins une instance Amazon EC2 en cours d’exécution.

![autoscaling_02.png](/assets/img/clouds/tempo/autoscaling_02.png =750x)

Lorsque vous créez un **groupe Auto Scaling**, vous pouvez définir le nombre minimal d’**instances Amazon EC2**. La capacité minimale correspond au nombre d’instances **Amazon EC2** qui se lancent immédiatement après la création du groupe Auto Scaling. Dans cet exemple, la capacité minimale du **groupe Auto Scaling** est d’une **instance Amazon EC2**.

Ensuite, vous pouvez fixer la **capacité souhaitée** à deux instances Amazon EC2, même si votre application a besoin d’au moins une seule instance Amazon EC2 pour s’exécuter.

> Si vous ne spécifiez pas le nombre souhaité d’instances Amazon EC2 dans un groupe Auto Scaling, la capacité souhaitée correspond par défaut à votre capacité minimale.
{.is-info}

La troisième configuration que vous pouvez définir dans un groupe Auto Scaling est la **capacité maximale**. Par exemple, vous pouvez configurer le groupe Auto Scaling pour sa mise à l’échelle ascendante en réponse à une demande accrue, mais uniquement jusqu’à un maximum de quatre instances Amazon EC2.

Étant donné qu’Amazon EC2 Auto Scaling utilise des instances Amazon EC2, vous payez uniquement les instances que vous utilisez, lorsque vous les utilisez. Vous disposez désormais d’une architecture rentable qui fournit la meilleure expérience client possible tout en réduisant les dépenses.

# Diriger le trafic avec Elastic Load Balancing

```
video : xxx.mkv
```
*Figure. Directing traffic with Elastic Load Balancing*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Nous avons donc résolu le problème de mise à l'échelle avec Amazon EC2 Auto Scaling. Mais maintenant, nous avons un léger problème de trafic, n'est-ce pas ? Jetons un coup d'œil à la situation. Lorsque les clients entrent dans le café, ils ont actuellement trois options de caisse pour faire leur demande. Curieusement, la plupart d’entre eux font la queue dans une seule file, provoquant une répartition inégale des clients par file. Même si d'autres caissiers attendent de prendre des commandes, debout et inoccupés. Les clients entrent et ne savent pas où passer leur commande. Ajouter un hôte pourrait être très utile dans cette situation. 
> 
> Un hôte est posté à l’entrée et lorsque des clients entrent dans le café, il leur indique la file où se placer pour passer commande. L’hôte contrôle le caissier qui prend les commandes, et compte le nombre de personnes servies par le caissier dans chaque file. Ensuite, il dirigera les nouveaux clients vers la caissier dont la file est la plus courte, celle qui a le moins d’attente, ce qui permet aux files d’être égales entre caissiers et les clients peuvent être servis de la manière la plus efficace possible. 
> 
> La même idée s'applique à votre environnement AWS. Lorsque vous avez plusieurs instances EC2 en train d’exécuter le même programme, pour le même objectif, et une demande arrive, comment cette dernière sait-elle vers quelle instance EC2 se diriger ? Comment s’assurer de la répartition égale de la charge de travail entre instances EC2 ? Pour que ce ne soit pas une seule instance qui est traitée tandis que les autres restes inactives. Vous avez besoin d'un moyen d'acheminer les demandes aux instances pour traiter cette demande. La répartition des charges vous permet de résoudre ce problème. 
> 
> L’équilibreur de charge est une application qui prend les demandes et les dirige vers les instances à traiter. Il existe de nombreux équilibreurs de charge en vente libre qui fonctionnent très bien sur AWS. Et si vous avez un modèle préféré qui fait déjà exactement ce que vous souhaitez, n’hésitez pas à continuer à l’utiliser. Dans ce cas, votre équipe opérationnelle est chargée d’installer l’application, de la gérer, de la mettre à échelle, de gérer les défaillances et la disponibilité. C'est faisable. Il est probable que vous ayez seulement besoin de répartir correctement le trafic dans un système haute performance, rentable, hautement disponible, évolutif automatiquement que vous pouvez configurer et ne plus y penser. 
> 
> Présentation d'Elastic Load Balancing. **Elastic Load Balancing (ELB)** est l’un des principaux services gérés dont nous allons parler dans ce cours. Il est conçu pour gérer les charges lourdes indifférenciées de répartition de charge. Pour illustrer ce point, je dois zoomer légèrement par ici. Tout d’abord, **Elastic Load Balancing** est une structure régionale et nous vous expliquerons de quoi il retourne dans des vidéos ultérieures. Pour vous, l’élément clé à savoir est qu’il fonctionne au niveau régional plutôt que sur des instances EC2 individuelles, et le service est donc automatiquement hautement disponible sans requérir d’effort supplémentaire de votre part. 
> 
> **ELB** est automatiquement évolutif. Alors que le trafic augmente, **ELB** est conçu pour traiter le débit supplémentaire sans augmenter le prix horaire. Lorsque votre flotte EC2 effectue une mise à échelle horizontale automatique, dès qu’une instance est mise en ligne, le service de mise à échelle automatique prévient le service **Elastic Load Balancing** qu’il est prêt à traiter le trafic, puis il s’exécute. Une fois que la flotte est mise à l’échelle, **ELB** arrête d’abord tout le nouveau trafic, et attend que les demandes actuelles soient terminées, et vidées. Ensuite, le moteur de mise à échelle automatique peut résilier les instances sans perturbation pour les clients existant. 
> 
> **ELB** n'est pas uniquement utilisé pour le trafic externe. Examinons le niveau de commande, et comment il communique avec le niveau de production. En ce moment même, chaque instance frontend est consciente de la présence de chaque instance backend. Si la nouvelle instance backend est mise en ligne, dans l’architecture actuelle, elle doit informer chaque nouvelle instance frontend qu’elle peut maintenant accepter le trafic. C'est déjà compliqué avec seulement une demi-douzaine d'instances, alors imaginez que vous ayez potentiellement des centaines d’instances sur les deux niveaux. Chaque changement de niveau étant constamment basé sur la demande, le simple fait de les garder efficacement en réseau est impossible. 
> 
> Le chaos du trafic backend se résout également avec un **ELB**. L'**ELB** étant régional, il s'agit d'une URL unique que chaque instance frontend utilise. Ensuite, l’**ELB** dirige le trafic vers le backend qui a le moins de demandes en attente. Si le backend se met à l’échelle, dès que la nouvelle instance est prête, elle communique simplement à l’**ELB** qu’elle peut accepter du trafic, et s’exécute. Le frontend ne sait pas et n’a pas besoin de savoir le nombre d’instances backend en cours d’exécution. Il s'agit d'une véritable architecture découplée. 
> 
> Et l’**ELB** peut faire bien d’autres choses, que vous découvrirez plus tard, et c’est suffisant pour l’instant. La clé est de choisir le bon outil pour la tâche correspondante, et c’est l’une des raisons pour lesquelles AWS propose de si nombreux services différents. Par exemple, la communication backend. Elle peut être gérée de différentes manières et **ELB** n’en est qu’une. La prochaine fois, nous aborderons d’autres services qui pourraient fonctionner encore mieux pour certaines architectures.
</div>
  
## Elastic Load Balancing

**[Elastic Load Balancing](https://aws.amazon.com/fr/elasticloadbalancing/)** est le service AWS qui distribue automatiquement le trafic d’applications entrant sur plusieurs ressources, telles que les instances Amazon EC2. 

#### Tabs {.tabset}
##### Application Load Balancer

![aws_elb_application_01.png](/assets/img/clouds/tempo/aws_elb_application_01.png)

##### Gateway Load Balancer

![aws_elb_gateway_01.png](/assets/img/clouds/tempo/aws_elb_gateway_01.png)

##### Network Load Balancer

![aws_elb_netowrk_01.png](/assets/img/clouds/tempo/aws_elb_netowrk_01.png)

#### 

Un équilibreur de charge agit comme un point de contact unique pour tout le trafic web entrant de votre groupe Auto Scaling. Cela signifie que lorsque vous ajoutez ou supprimez des instances Amazon EC2 en réponse à la quantité de trafic entrant, ces demandes acheminent d’abord vers l’équilibreur de charge. Ensuite, les demandes se répartissent sur plusieurs ressources qui les géreront. Par exemple, si vous disposez de plusieurs instances Amazon EC2, Elastic Load Balancing répartit la charge de travail entre les différentes instances de sorte qu’aucune instance n’ait à en supporter la majeure partie. 

Bien qu’Elastic Load Balancing et Amazon EC2 Auto Scaling soient des services distincts, ils travaillent ensemble pour garantir que les applications exécutées dans Amazon EC2 puissent fournir des performances et une disponibilité élevées.

## Exemple : Elastic Load Balancing

**Période de faible demande**

Voici un exemple du fonctionnement d’Elastic Load Balancing. Supposons que plusieurs clients sont arrivés au café et qu’ils sont prêts à passer leurs commandes. 

![elb_01.png](/assets/img/clouds/tempo/elb_01.png =500x){.align-left}

Si plusieurs caisses sont ouvertes, cela correspond à la demande des clients qui doivent être servis. Le café est moins susceptible d’avoir des caisses ouvertes en l’absence de clients. Dans cet exemple, vous pouvez considérer les caisses comme des instances Amazon EC2.



**Période de forte demande**

Tout au long de la journée, à mesure que le nombre de clients augmente, le café ouvre davantage de caisses pour les accueillir. Dans le diagramme, ceci est représenté par le groupe Auto Scaling.

![elb_02.png](/assets/img/clouds/tempo/elb_02.png =500x)

De plus, un employé de café dirige les clients vers la caisse la plus appropriée afin que le nombre de demandes puisse être réparti uniformément entre les caisses ouvertes. Vous pouvez penser à cet employé de café comme à un équilibreur de charge.


# Messagerie et mise en file d’attente

```
video : xxx.mkv
```
*Figure. Messaging and queueing*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Parlons de la messagerie et de la mise en file d'attente. Dans le café, les caissiers prennent les commandes auprès des clients et les baristas les préparent. Le caissier prend actuellement la commande, l’inscrit sur un papier, et transmet cette commande au barista. Le barista prend ensuite le papier et prépare la commande. Quand la commande suivante arrive, le processus se répète. Cela fonctionne très bien tant que le caissier et le barista sont synchronisés. Mais que ce passerait-il si le caissier prenait une commande et la transmettait au barista et ce dernier était absent ou occupé avec une autre commande ? Et bien ce caissier est coincé jusqu’à ce que le barista soit prêt à prendre la commande. Et à un certain moment, la commande sera probablement abandonnée pour que le caissier puisse servir le client suivant. 
> 
> Vous pouvez voir en quoi ce processus est défectueux car, dès que le caissier ou le barista ne sont plus synchronisés, le processus se dégrade, provoquant des ralentissements dans la réception des commandes qui arrivent et les commandes complètes ne sont plus remplies. Un bien meilleur processus consisterait à introduire une sorte de tampon ou de file d'attente dans le système. Au lieu de transmettre directement la commande au barista, le caissier placerait les commandes dans un élément tampon, comme un tableau de commandes. 
> 
> Cette idée de placer des messages dans un élément tampon est appelée messagerie et mise en file d’attente. Comme notre caissier envoie les commandes au barista, les applications s’envoient des messages pour communiquer. Si les applications communiquent directement comme notre caissier et le barista auparavant, on appelle cela le fait d’être étroitement couplé. 
> 
> Un trait caractéristique d’**une architecture étroitement couplée** est que si un seul composant tombe en panne ou change, cela provoque des problèmes pour d’autres composants ou même le système complet. Par exemple, si nous avons l’application A qui envoie des messages directement à l’application B, si l’application B est en panne et ne peut pas accepter ces messages, l’application A commence également à produire des erreurs. Il s'agit d'une architecture étroitement couplée. 
> 
> **L'architecture faiblement couplée** est plus fiable. Il s’agit d’une architecture où, si un composant tombe en panne, il est isolé et ne crée pas de pannes en cascades dans tout le système. Si l’application est codée pour utiliser une architecture plus faiblement couplée, elle pourrait se présenter comme ceci. 
> 
> Comme notre caissier et notre barista, nous avons introduit un élément tampon entre les deux. Dans ce cas précis, nous avons introduit une file d'attente de messages. Les messages sont placés dans la file d’attente par l’application A puis traités par l’application B. Si l'application B tombe en panne l’application A ne subit aucune perturbation. Les messages envoyés peuvent toujours être envoyés dans la file d’attente et il y resteront jusqu’à leur traitement final. 
> 
> Il s'agit d'une architecture faiblement couplée. C'est ce que nous nous efforçons de réaliser avec les architectures sur AWS. Cela me conduit à vous présenter deux services AWS pouvant être utiles dans cette situation. **Amazon Simple Queue Service (SQS)** et **Amazon Simple Notification Service (SNS)**. Avant d’aborder en profondeur ces deux services, je vais commander un café à emporter sur notre site de café. C’est fait. D'accord, c'est fantastique. Je devrais recevoir un message lorsque la commande sera prête. 
> 
> Tout d'abord, parlons d'**Amazon SQS**. **SQS** vous permet d’envoyer, de stocker et de recevoir des messages entre composants logiciels quel que soit leur volume. Le tout sans perdre de messages ni avoir besoin que d'autres services soient disponibles. Représentez-vous ces messages comme nos commandes de café et le tableau de commande comme la **file d’attente SQS**. Les messages contiennent le nom du client, le café commandé et l’heure de la prise de commande. Les données contenues dans un message s’appellent la charge utile, et elles sont protégées jusqu’à leur livraison. **SQS queues** est l’endroit où les messages sont placés jusqu’à ce qu’ils soient traités. Et AWS gère l’infrastructure sous-jacente pour que vous puissiez héberger ces files d’attente. Elles se mettent à l’échelle automatiquement, elles sont fiables, et faciles à configurer et à utiliser. 
> 
> **Amazon SNS** est similaire car il est utilisé pour envoyer des messages aux services, mais il peut également envoyer des notifications aux utilisateurs finaux. Il applique une méthode différente appelée **publication/abonnement** ou **modèle pub/sub**. Cela signifie que vous pouvez créer ce qu’on appelle **une rubrique SNS** qui est simplement **un canal dédié aux transmissions de messages**. Vous configurez alors les abonnés de cette rubrique puis vous publiez finalement les messages à destination de ces abonnés. Dans la pratique, cela signifie que vous pouvez envoyer un message à une rubrique qui va ensuite se déployer à tous les abonnés simultanément. Ces abonnés peuvent également être des points de terminaison comme **les files SQS**, les fonctions AWS Lambda, et les web hooks HTTPS ou HTTP. 
> 
> De plus, **SNS** peut être utilisé pour diffuser des notifications aux utilisateurs finaux à l'aide du push mobile, des SMS, et des e-mails. Si nous appliquons cela à notre café, nous pourrions envoyer une notification lorsque la commande d’un client est prête. Cela pourrait être un simple SMS pour les informer qu'ils peuvent la récupérer ou même un push mobile. 
> 
> D’ailleurs, il semblerait que mon téléphone vient juste de recevoir un message. On dirait que ma commande est prête. À bientôt.
</div>
  
## Applications monolithiques et microservices


Les applications sont faites de plusieurs composants. Les composants communiquent entre eux pour transmettre des données, traiter des demandes et maintenir l’application en cours d’exécution. 

Supposons que vous ayez une application dont les composants sont étroitement couplés. Ces composants peuvent inclure des bases de données, des serveurs, l’interface utilisateur, la logique métier, etc. Ce type d’architecture peut être considéré comme une **application monolithique**. 

![sns_01.png](/assets/img/clouds/tempo/sns_01.png =500x)

Dans cette approche de l’architecture d’application, si un seul composant échoue, d’autres composants échouent, et l’application entière est susceptible d’échouer.

> Pour maintenir la disponibilité de l’application en cas de panne d’un seul composant, vous pouvez concevoir votre application à l’aide d’une **approche de microservices**.
{.is-info}

Dans une approche de microservices, les composants d’application sont faiblement couplés. Dans ce cas, si un seul composant tombe en panne, les autres composants continuent de fonctionner, car ils communiquent entre eux. Le couplage faible empêche l’ensemble de l’application de tomber en panne. 

![sns_02.png](/assets/img/clouds/tempo/sns_02.png =500x)

Lors de la conception d’applications sur AWS, vous pouvez adopter une approche de microservices avec des services et des composants qui remplissent différentes fonctions. Deux services facilitent l’intégration des applications : **Amazon Simple Notification Service (Amazon SNS)** et **Amazon Simple Queue Service (Amazon SQS)**.

## Amazon Simple Notification Service (Amazon SNS)

**[Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/fr/sns/)** est un service de publication/abonnement. À l’aide des **rubriques Amazon SNS**, un éditeur publie des messages à l’intention des abonnés. Cela est semblable à l’exemple du café ; le caissier transmet les commandes de café au barista qui prépare les boissons.

![aws_sns_01.png](/assets/img/clouds/tempo/aws_sns_01.png)

Dans **Amazon SNS**, les abonnés peuvent être des serveurs web, des adresses e-mail, des fonctions **AWS Lambda** ou plusieurs autres options. 

> Dans la prochaine leçon, vous en apprendrez plus sur AWS Lambda.
{.is-info}

![sns_03.png](/assets/img/clouds/tempo/sns_03.png =500x)

### Tabs {.tabset}
#### Etape 1

Publication des mises à jour d’une seule rubrique

![sns_04.png](/assets/img/clouds/tempo/sns_04.png)

Supposons que le café dispose d’une lettre d’informations unique qui comprend les mises à jour de tous les domaines de son activité. Il comprend des rubriques telles que les coupons, les anecdotes sur le café et les nouveaux produits. Toutes ces rubriques sont regroupées, car il s’agit d’une lettre d’informations unique. Tous les clients qui s’abonnent au bulletin d’informations reçoivent des mises à jour sur les coupons, les anecdotes sur le café et les nouveaux produits.

Après un certain temps, certains clients expriment leur souhait de recevoir des lettres d’informations distinctes, uniquement pour les rubriques spécifiques qui les intéressent. Les propriétaires du café décident d’essayer cette approche.

#### Etape 2

Publication des mises à jour de plusieurs rubriques

![sns_05.png](/assets/img/clouds/tempo/sns_05.png)

Maintenant, au lieu d’avoir une seule lettre d’informations pour toutes les rubriques, le café l’a divisée en trois bulletins distincts. Chaque bulletin d’informations est consacré à une rubrique spécifique : coupons, anecdotes de café et nouveaux produits.

Les abonnés recevront désormais des mises à jour immédiates uniquement pour les rubriques spécifiques auxquelles ils ont souscrit.

Il est possible de s’abonner à une seule rubrique ou à plusieurs rubriques. Par exemple, le premier client s’abonne uniquement à la rubrique des coupons et le deuxième s’abonne uniquement à la rubrique des anecdotes sur le café. Le troisième client s’abonne à la fois aux anecdotes sur le café et aux nouveaux produits.

Bien que cet exemple du café implique des abonnés qui sont des personnes, dans **Amazon SNS**, les abonnés peuvent être des serveurs web, des adresses e-mail, des fonctions AWS Lambda ou plusieurs autres options.

## Amazon Simple Queue Service (Amazon SQS)

**[Amazon Simple Queue Service (Amazon SQS)](https://aws.amazon.com/fr/sqs/)** est un service de mise en file d’attente de messages. 

![aws_sqs_01.png](/assets/img/clouds/tempo/aws_sqs_01.png)

Grâce à Amazon SQS, vous pouvez envoyer, stocker et recevoir des messages entre les composants logiciels, sans perdre de messages ni avoir besoin que d’autres services soient disponibles. Dans Amazon SQS, une application envoie des messages dans une file d’attente. Un utilisateur ou un service extrait un message de la file d’attente, le traite, puis le supprime de la file d’attente.

![sqs_01.png](/assets/img/clouds/tempo/sqs_01.png =500x)

### Tabs {.tabset}
#### Etape 1

Exemple : exécution d’une commande

![sqs_02.png](/assets/img/clouds/tempo/sqs_02.png)

Supposons que le café a un processus de commande dans lequel un caissier prend les commandes et un barista prépare les commandes. Pensez au caissier et au barista comme deux composants distincts d’une application. 

Tout d’abord, le caissier prend la commande et l’écrit sur un morceau de papier. Ensuite, le caissier transmet le papier au barista. Enfin, le barista prépare la boisson et la donne au client.

Lorsque la commande suivante arrive, le processus se répète. Ce processus se déroule bien tant que le caissier et le barista sont coordonnés.

Que se passerait-il si le caissier prenait une commande et allait la donner au barista, mais que le barista était en pause ou occupé avec une autre commande ? Le caissier devrait attendre que le barista soit prêt à accepter la commande. Cela entraînerait des retards dans le processus de commande et obligerait les clients à attendre plus longtemps pour recevoir leurs commandes.

Le café devenant de plus en plus populaire et la queue progressant plus lentement, les propriétaires remarquent que le processus de commande actuel est long et inefficace. Ils décident d’essayer une approche différente qui utilise une file d’attente.

#### Etape 2

Exemple : commandes dans une file d’attente

![sqs_03.jpg](/assets/img/clouds/tempo/sqs_03.jpg)

Rappelons que le caissier et le barista sont deux composants distincts d’une application. Un service de mise en file d’attente de messages tel qu’Amazon SQS active les messages entre les compléments d’application découplés.

Dans cet exemple, la première étape du processus reste la même qu’avant : un client passe une commande auprès du caissier. 

Le caissier met la commande dans une file d’attente. Vous pouvez considérer cela comme un panneau de commande qui sert de tampon entre le caissier et le barista. Même si le barista est en pause ou occupé avec une autre commande, le caissier peut continuer à placer de nouvelles commandes dans la file d’attente. 

Ensuite, le barista vérifie la file d’attente et récupère la commande.

Il prépare la boisson et la donne au client. 

Le barista supprime ensuite la commande terminée de la file d’attente. 

Pendant que le barista prépare la boisson, le caissier peut continuer à prendre de nouvelles commandes et à les ajouter à la file d’attente.

Pour les applications découplées et les microservices, Amazon SQS vous permet d’envoyer, de stocker et de récupérer des messages entre les composants. 

Cette approche découplée permet aux composants distincts de fonctionner de manière plus efficace et indépendante. 

### 

**Quel service AWS est le meilleur choix pour publier des messages aux abonnés ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Amazon Simple Queue Service (Amazon SQS)</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Amazon EC2 Auto Scaling</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Amazon Simple Notification Service (Amazon SNS)</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Elastic Load Balancing</label><br>
</form>



<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Amazon Simple Notification Service (Amazon SNS)**.

Amazon SNS est un service de publication/d’abonnement. À l’aide des rubriques Amazon SNS, un éditeur publie des messages à l’intention des abonnés.

Les autres réponses sont incorrectes, car :

- Amazon Simple Queue Service (Amazon SQS) est un service de mise en file d’attente de messages. Il n’utilise pas le modèle d’abonnement aux messages et de rubrique pris en charge par Amazon SNS.
- Amazon EC2 Auto Scaling vous permet d’ajouter ou de supprimer automatiquement des instances Amazon EC2 en réponse à l’évolution de la demande des applications.
- Elastic Load Balancing est le service AWS qui distribue automatiquement le trafic d’application entrant sur plusieurs ressources, telles que les instances Amazon EC2.

En savoir plus :

- [Amazon SNS](https://aws.amazon.com/sns)
</div>
  
  
# Services de calcul supplémentaires

```
video : xxx.mkv
```
*Figure. Additional compute services*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Les instances EC2 sont des machines virtuelles que vous pouvez mettre en service avec un minimum de friction afin qu'elles soient opérationnelles sur AWS. EC2 est idéal pour une grande variété de cas d’utilisation comme l’exécution de serveurs web de base ou l’exécution de clusters de calcul hautes performances et toute une série d’autres cas. 
> 
> Cela dit, même si EC2 est incroyablement flexible, fiable et évolutif, selon votre cas d’utilisation, vous envisagez probablement d’autres solutions pour votre capacité de calcul EC2 nécessite que vous configuriez et gériez votre flotte d’instances au fil du temps. Lorsque vous utilisez EC2, vous êtes responsable de l’application de correctifs à vos instances lorsque de nouveaux packages de logiciels sortent, de la configuration de la mise à échelle de ces instances et de vous assurer que vous avez réalisé l’architecture de vos solutions pour être hébergées de la manière la plus disponible possible. Cela représente toujours moins de gestion qu’avec des hébergements sur site. Cependant, les processus de gestion doivent quand même être mis en place. 
> 
> Vous vous demandez peut-être quels autres services de calcul sont proposés par AWS et qui sont plus pratiques du point de vue de la gestion. C'est ici que le terme sans serveur entre en jeu. AWS offre plusieurs options de calcul sans serveur. Sans serveur signifie que vous ne pouvez pas vraiment voir ou accéder à l’infrastructure sous-jacente ou aux instances qui hébergent votre solution. Au lieu de cela, toute la gestion de l’environnement sous-jacent du point de vue de la gestion de l’approvisionnement, de la mise à l’échelle, de la tolérance aux pannes et de la maintenance, est prise en charge pour vous. Tout ce que vous avez à faire est de vous concentrer sur votre application et le reste est pris en charge. 
> 
> AWS Lambda fait partie des options de calcul sans serveur. Lambda est un service qui vous permet de télécharger votre code dans ce que nous appelons une fonction Lambda. Configurez un déclencheur et à partir de là, le service attend le déclencheur. Lorsque le déclencheur est détecté, le code est automatiquement exécuté dans un environnement géré, et vous n'avez pas trop à vous soucier de celui-ci car il est automatiquement évolutif hautement disponible, et toute la maintenance de l'environnement lui-même est assurée par AWS. Si vous disposez d'un ou de 1 000 déclencheurs entrants, Lambda fera évoluer votre fonction pour répondre à la demande. Lambda est conçu pour exécuter le code sous 15 minutes et il n’est donc pas adapté aux longs processus d’exécution comme le deep learning. Il est plus adapté pour un traitement rapide tel qu’un backend web, le traitement des demandes ou un service de traitement du rapport de coût backend où chaque appel prend moins de 15 minutes. 
> 
> Dans le cas où vous ne seriez pas encore prêt pour le sans serveur ou si vous avez besoin d’accéder à l’environnement sous-jacent, mais vous cherchez quand même l’efficacité et la portabilité, vous devriez vous intéresser aux services de conteneurs AWS comme Amazon Elastic Container Service, également appelé ECS. Ou Amazon Elastic Kubernetes Service, aussi connu comme EKS. 
> 
> Ces deux services sont des outils d’orchestration de conteneurs, mais avant d’aller plus loin, un conteneur dans ce cas est un conteneur Docker. Docker est une plateforme largement utilisée qui utilise la virtualisation de niveau du système d’exploitation pour fournir des logiciels dans des conteneurs. Un conteneur est un package pour votre code dans lequel vous emballez votre application, ses dépendances ainsi que toutes les configurations à exécuter. Ces conteneurs s’exécutent sur les instances EC2 et de manière séparée les uns des autres comme pour le fonctionnement des machines virtuelle. Mais dans ce cas, l'hôte est une instance EC2. Lorsque vous utilisez des conteneurs Docker sur AWS, vous avez besoin de processus pour démarrer, arrêter, redémarrer et contrôler les conteneurs exécutés non seulement sur une instance EC2 mais sur un groupe d’instances que nous appelons un cluster. 
> 
> Ce processus s’appelle l’orchestration de conteneurs et il se trouve que c’est une opération vraiment difficile à effectuer par soi-même. Des outils d'orchestration ont été créés pour vous aider à gérer vos conteneurs. ECS est conçu pour vous aider à exécuter vos applications conteneurisées à grande échelle sans avoir à gérer votre propre logiciel d’orchestration de conteneurs. EKS sert à peu près à la même chose, et possède des fonctions différentes. 
> 
> Amazon ECS et Amazon EKS peuvent tous deux fonctionner sur EC2. Si vous ne pensez pas du tout utiliser EC2 pour héberger vos conteneurs parce que vous n’avez pas besoin d’accéder au système d’exploitation sous-jacent ou vous ne souhaitez pas gérer ces instances EC2, vous pouvez utiliser la plateforme de calcul AWS Fargate. Fargate est une plateforme de calcul sans serveur pour ECS ou EKS. Il s'agit d'un outil de haut niveau qui peut être déroutant alors nous allons éclaircir un peu son fonctionnement. 
> 
> Si vous essayez d’héberger des applications classiques et vous souhaitez un accès complet à un système d’exploitation sous-jacent comme Linux ou Windows, vous souhaiterez utiliser EC2. Si vous cherchez à héberger des fonctions à courte exécution, orientées service ou des applications basées sur les événements et vous ne souhaitez pas gérer du tout l’environnement sous-jacent, intéressez-vous au service sans serveur AWS Lambda. Si vous souhaitez exécuter des charges de travail basées sur les conteneurs Docker sur AWS, vous devez tout d’abord choisir votre outil d’orchestration. Souhaitez-vous utiliser Amazon ECS ou Amazon EKS ? Après avoir choisi votre outil, vous devez choisir votre plateforme. Souhaitez-vous exécuter vos conteneurs sur des instances EC2 que vous gérez ou dans un environnement sans serveur comme AWS Fargate qui est géré pour vous ? 
> 
> Ce ne sont que quelques-unes des options de calcul disponibles avec AWS et il ne s’agit même pas d’une liste complète. Reportez-vous aux remarques pour plus de renseignements sur les services de calcul AWS et d’autres services que nous n’avons pas pu aborder dans cette vidéo.
</div>
  
## Calcul sans serveur

Plus tôt dans ce module, vous avez découvert Amazon EC2, un service qui vous permet d’exécuter des serveurs virtuels dans le cloud. Si vous avez des applications que vous souhaitez exécuter dans Amazon EC2, vous devez effectuer les opérations suivantes :

1. Mettre en service des instances (serveurs virtuels)
1. Télécharger votre code
1. Continuer à gérer les instances pendant l'exécution de votre application.

Le terme « sans serveur » signifie que votre code s’exécute sur des serveurs, mais vous n’avez pas besoin de mettre en service ou de gérer ces serveurs. Avec le calcul sans serveur, vous pouvez vous concentrer davantage sur l’innovation de produits et fonctions plutôt que sur la maintenance des serveurs.

![serverless_01.png](/assets/img/clouds/tempo/serverless_01.png =500x)

Un autre avantage du calcul sans serveur est la flexibilité de mettre automatiquement à l’échelle les applications sans serveur. Le calcul sans serveur peut ajuster la capacité des applications en modifiant les unités de consommation, telles que le débit et la mémoire. 

**AWS Lambda** est le service AWS de calcul sans serveur.

## AWS Lambda

**[AWS Lambda](https://aws.amazon.com/lambda)** est un service qui vous permet d’exécuter du code sans devoir mettre en service ou gérer des serveurs. 

#### tabs {.tabset}
##### Traitement des fichiers

![aws_lambda_trait_fichiers_01.png](/assets/img/clouds/tempo/aws_lambda_trait_fichiers_01.png)

##### Traitement des flux

![aws_lambda_trait_flux_01.png](/assets/img/clouds/tempo/aws_lambda_trait_flux_01.png)

##### Application Web

![aws_labmda_app_web_01.png](/assets/img/clouds/tempo/aws_labmda_app_web_01.png)

##### Backends IoT

![aws_lambda_backends_iot_01.png](/assets/img/clouds/tempo/aws_lambda_backends_iot_01.png)

##### Backends Mobiles

![aws_lambda_backends_mobiles.png](/assets/img/clouds/tempo/aws_lambda_backends_mobiles.png)


### 
Lorsque vous utilisez AWS Lambda, vous ne payez que le temps de calcul que vous consommez. Les frais s’appliquent uniquement lorsque votre code est en cours d’exécution. Vous pouvez également exécuter le code pour quasiment n’importe quel type d’application ou de service backend, sans aucune administration. 

Par exemple, une simple fonction Lambda peut impliquer le redimensionnement automatique des images téléchargées dans le AWS Cloud. Dans ce cas, la fonction se déclenche lors du téléchargement d’une nouvelle image. 

### Fonctionnement d’AWS Lambda

![serverless_02.png](/assets/img/clouds/tempo/serverless_02.png)

1. Vous téléchargez votre code sur Lambda.
1. Vous définissez votre code pour qu’il se déclenche à partir d’une source d’événement, telle que des services AWS, des applications mobiles ou des points de terminaison HTTP.
1. Lambda exécute votre code uniquement lorsqu’il est déclenché.
1. Vous payez uniquement le temps de calcul utilisé. Dans l’exemple précédent de redimensionnement des images, vous ne payez que le temps de calcul utilisé lors du téléchargement de nouvelles images. Le téléchargement des images déclenche Lambda, qui exécute le code de la fonction de redimensionnement de l’image.

> Dans AWS, vous pouvez également concevoir et exécuter des applications **conteneurisées**.
{.is-info}

## Conteneurs

**Les conteneurs** vous fournissent un moyen standard de rassembler le code et les dépendances de votre application dans un seul objet. Vous pouvez également utiliser des conteneurs pour des processus et des flux de travail dans lesquels il existe des exigences essentielles en matière de sécurité, de fiabilité et de scalabilité.

![serverless_03.png](/assets/img/clouds/tempo/serverless_03.png =500x)

#### Tabs {.tabset}
##### Etape 1

###### Un hôte avec plusieurs conteneurs

![serverless_04.png](/assets/img/clouds/tempo/serverless_04.png)

Supposons que le développeur d’applications d’une entreprise possède sur son ordinateur un environnement différent de celui des ordinateurs utilisés par le personnel des opérations informatiques. Le développeur veut s’assurer que l’environnement de l’application reste cohérent, quel que soit le déploiement, de sorte qu’il utilise une approche conteneurisée. Cela permet de réduire le temps consacré au débogage des applications et au diagnostic des différences dans les environnements de calcul.

##### Etape 2

###### Des dizaines d’hôtes avec des centaines de conteneurs

![serverless_05.png](/assets/img/clouds/tempo/serverless_05.png)

Lors de l’exécution d’applications conteneurisées, il est important de tenir compte de la scalabilité. Supposons qu’au lieu d’un seul hôte avec plusieurs conteneurs, vous devez gérer des dizaines d’hôtes avec des centaines de conteneurs. Alternativement, vous devez gérer éventuellement des centaines d’hôtes avec des milliers de conteneurs. À grande échelle, imaginez combien de temps il vous faudra pour surveiller l’utilisation de la mémoire, la sécurité, la journalisation, etc.

##### Résumé
Les services d’orchestration de conteneurs vous aident à déployer, gérer et mettre à l’échelle vos applications conteneurisées. Ensuite, vous découvrirez deux services qui fournissent l’orchestration de conteneurs : **Amazon Elastic Container Service** et **Amazon Elastic Kubernetes Service**.

## Amazon Elastic Container Service (Amazon ECS)

[Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs/) est un système de gestion de conteneurs hautement évolutif et à hautes performances qui vous permet d’exécuter et de mettre à l’échelle des applications conteneurisées sur AWS. 

![aws_ecs_01.png](/assets/img/clouds/tempo/aws_ecs_01.png)

Amazon ECS prend en charge les conteneurs Docker. [Docker](https://www.docker.com/) est une plateforme logicielle qui vous permet de concevoir, de tester et de déployer des applications rapidement. AWS prend en charge l’utilisation de Docker Community Edition en open source et Docker Enterprise Edition en version abonnement. Avec Amazon ECS, vous pouvez utiliser les appels d’API pour lancer et arrêter des applications compatibles avec Docker.

## Amazon Elastic Kubernetes Service (Amazon EKS)

[Amazon Elastic Kubernetes Service (Amazon EKS)](https://aws.amazon.com/eks/) est un service entièrement géré que vous pouvez utiliser pour exécuter Kubernetes sur AWS. 

#### tabs {.tabset}
##### Exécuter Amazon EKS dans le Cloud

![aws_eks_cloud_01.png](/assets/img/clouds/tempo/aws_eks_cloud_01.png)

##### Exécuter Amazon EKS sur AWS Outposts

![aws_eks_outposts_01.png](/assets/img/clouds/tempo/aws_eks_outposts_01.png)

##### Exécuter Amazon EKS Anywhere

![aws_eks_anywhere_01.png](/assets/img/clouds/tempo/aws_eks_anywhere_01.png)

##### Déployer des applications avec vos propres outils

![aws_eks_deploy_apps_01.png](/assets/img/clouds/tempo/aws_eks_deploy_apps_01.png)

### 

[Kubernetes](https://kubernetes.io/) est un logiciel open source qui vous permet de déployer et de gérer des applications conteneurisées à grande échelle. Une grande communauté de bénévoles maintient Kubernetes, et AWS collabore activement avec la communauté Kubernetes. À mesure que de nouvelles fonctions sont disponibles pour les applications Kubernetes, vous pouvez facilement appliquer ces mises à jour à vos applications gérées par Amazon EKS.

## AWS Fargate

[AWS Fargate](https://aws.amazon.com/fargate/) est un moteur de calcul sans serveur pour les conteneurs. Il fonctionne avec Amazon ECS et Amazon EKS. 

![aws_fargate_01.png](/assets/img/clouds/tempo/aws_fargate_01.png)

Lorsque vous utilisez AWS Fargate, vous n’avez pas besoin de mettre en service ou de gérer des serveurs. AWS Fargate gère votre infrastructure de serveurs à votre place. Vous pouvez vous concentrer davantage sur l’innovation et le développement de vos applications, et vous ne payez que les ressources nécessaires à l’exécution de vos conteneurs.

> Pour en savoir plus sur les services et solutions supplémentaires, consultez [Calcul sur AWS](https://aws.amazon.com/products/compute).
{.is-info}

# Module 2 : résumé

Dans le module 2, vous avez découvert les concepts suivants :

- Types d’instances Amazon EC2 et options de tarification
- Amazon EC2 Auto Scaling
- Elastic Load Balancing
- Services AWS pour messagerie, conteneurs et calcul sans serveur

```
video : xxx.mkv
```
*Figure. Module Summary - Compute in the cloud*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Bonjour à tous. J'espère que jusqu'à présent, vous avez apprécié ce cours. Nous ferons des points comme celui-ci après chaque rubrique importante pour vérifier ce que vous aurez appris. 
> 
> Donc tout d’abord, qu'est-ce que le cloud computing et qu'est-ce qu'offre AWS ? Nous définissons le cloud computing comme la mise à disposition de ressources informatiques à la demande via Internet, avec un paiement à l’utilisation. Cela signifie que vous pouvez faire des demandes de ressources informatiques comme le calcul, la mise en réseau, le stockage, l’analytique ou d’autres types de ressources, et elles sont mises à votre disposition à la demande. Vous ne payez pas la ressource à l'avance. Vous la mettez simplement en service et vous payez à la fin du mois. 
> 
> AWS propose des services et de nombreuses catégories que vous utilisez ensemble pour créer vos solutions. Nous n’avons abordé que quelques services jusqu’à présent, vous en savez plus sur Amazon EC2. Avec EC2, vous pouvez mettre en service de façon dynamique et arrêter les serveurs virtuels appelés instances EC2. Lorsque vous lancez une instance EC2, vous choisissez la famille d’instances. La famille d’instance détermine le matériel sur lequel l’instance s’exécute. 
> 
> Et vous avez des instances qui sont construites pour votre objectif spécifique. Les catégories d'instances sont les suivantes : polyvalentes, optimisées pour le calcul, optimisées pour la mémoire, calcul accéléré et optimisées pour le stockage. 
> 
> Vous pouvez mettre vos instances EC2 à l’échelle soit verticalement en redimensionnant l’instance, soit horizontalement en lançant de nouvelles instances et en les ajoutant au groupe. Vous pouvez configurer une mise à l’échelle horizontale automatique, en utilisant Amazon EC2 Auto Scaling. 
> 
> Après avoir mis vos instances EC2 à l’échelle horizontalement, vous avez besoin de quelque chose pour distribuer le trafic entrant entre ces instances. C'est là qu'Elastic Load Balancer entre en jeu. 
> 
> Les instances EC2 ont différents modèles de tarification : à la demande, qui est le plus flexible et n’implique pas de contrat, le prix spot qui vous permet d’utiliser la capacité inutilisée à un prix réduit, Savings Plans ou instances réservées, qui vous permettent de signer un contrat avec AWS pour bénéficier de prix réduits lorsque vous vous engagez à un certain niveau d’utilisation, et Savings Plans qui s’applique à AWS Lambda, et AWS Fargate, ainsi que les instances EC2. 
> 
> Nous avons également couvert le thème des services de messagerie, Amazon Simple Queue Service (SQS), qui vous permet de découpler les composants d'un système. Les messages restent dans la file d’attente jusqu’à ce qu’ils soient traités ou supprimés. Amazon Simple Notification Service (SNS), sert à envoyer les messages comme les e-mails, les SMS, les notifications push ou même les requêtes HTTP. Après publication d’un message, il est envoyé à tous les abonnés. 
> 
> Vous avez également appris que AWS propose différents types de services de calcul en plus des serveurs virtuels comme EC2. Il propose des services de conteneur, tels qu'Amazon Elastic Container Service (ECS) ou Amazon Elastic Service Kubernetes (EKS). Les deux sont des outils d'orchestration de conteneurs. Vous pouvez utiliser ces outils avec des instances EC2 mais si vous ne souhaitez pas le gérer, vous n’y êtes pas obligé. 
> 
> Vous pouvez utiliser AWS Fargate qui vous aide à exécuter vos conteneurs en plus d’une plateforme de calcul sans serveur. Ensuite, vous avez AWS Lambda, qui vous permet simplement de télécharger votre code, et de le configurer pour qu’il s’exécute en fonction des déclencheurs. Et vous ne payez que lorsque le code est en cours d’exécution. Pas de conteneurs, pas de machines virtuelles, simplement un code et une configuration. J'espère avoir fait un bon résumé. On se revoit au prochain cours.
</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été explorés dans le module 2, consultez ces ressources.

- [Calcul sur AWS](https://aws.amazon.com/products/compute)
- [Blog sur le calcul AWS](https://aws.amazon.com/blogs/compute/)
- [Services de calcul AWS](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/compute-services.html)
- [Tutoriels pratiques : calcul](https://aws.amazon.com/getting-started/hands-on/?awsf.getting-started-category=category%23compute&awsf.getting-started-content-type=content-type%23hands-on)
- [Découverte approfondie d’une catégorie : sans serveur](https://aws.amazon.com/getting-started/deep-dive-serverless/)
- [Témoignages de clients AWS : sans serveur](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23serverless)

# Module 2 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Vous souhaitez utiliser une instance Amazon EC2 pour une charge de travail de traitement par lots. Quel serait le meilleur type d’instance Amazon EC2 à utiliser ?**


<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">A usage général</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Optimisée en mémoire</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Optimisée pour le calcul</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">Optimisée pour le stockage</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Optimisée pour le calcul**.



Les autres réponses sont incorrectes, car :

- Les instances à usage général assurent un équilibre entre les ressources de calcul, de mémoire et de réseau. Cette famille d’instances ne serait pas le meilleur choix pour l’application dans ce scénario. Les instances de calcul optimisées sont plus adaptées aux charges de travail de traitement par lots que les instances à usage général.
- Les instances optimisées en mémoire sont plus adaptées aux charges de travail qui traitent des jeux de données volumineux en mémoire, telles que les bases de données hautes performances.
- Les instances optimisées pour le stockage sont conçues pour les charges de travail qui nécessitent un accès en lecture et en écriture séquentielle élevé aux jeux de données volumineux sur le stockage local. La question ne précise pas la taille des données qui seront traitées. Le traitement par lots implique le traitement des données par groupes. Une instance de calcul optimisée est idéale pour ce type de charge de travail, qui bénéficierait d’un processeur hautes performances.
En savoir plus :

- [Types d’instances Amazon EC2](https://aws.amazon.com/ec2/instance-types/)

</div>  

**Quelles sont les options de durée de contrat pour les instances réservées Amazon EC2 ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q3r1" name="question3" value="1">
  <label for="q3r1">1 an</label><br>
  <input type="checkbox" id="q3r2" name="question3" value="2">
  <label for="q3r2">2 ans</label><br>
  <input type="checkbox" id="q3r3" name="question3" value="3">
  <label for="q3r3">3 ans</label><br>
  <input type="checkbox" id="q3r4" name="question3" value="4">
  <label for="q3r4">4 ans</label><br>
  <input type="checkbox" id="q3r5" name="question3" value="5">
  <label for="q3r5">5 ans</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **1 an**
- **3 ans**

Les instances réservées requièrent un engagement de 1 an ou de 3 ans. L’option de 3 ans offre une réduction plus importante.

En savoir plus :

- [Instances réservées Amazon EC2](https://aws.amazon.com/ec2/pricing/reserved-instances/)
</div>
  
  
**Vous avez une charge de travail qui durera au total 6 mois et peut résister aux interruptions. Quelle serait l’option d’achat Amazon EC2 la plus rentable ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Instance réservée</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">Instance Spot</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">Instance dédiée</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Instance à la demande</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Instance Spot**.

Les autres réponses sont incorrectes, car :

- Les instances réservées nécessitent une durée de contrat de 1 an ou de 3 ans. La charge de travail dans ce scénario ne sera exécutée que pendant 6 mois.
- Les instances dédiées s’exécutent dans un cloud privé virtuel (VPC) sur du matériel dédié à un seul client. Elles ont un coût plus élevé que les autres options de réponse, qui s’exécutent sur du matériel partagé.
- Les instances à la demande satisfont aux exigences relatives à l’exécution pendant seulement 6 mois et à la résistance aux interruptions. Toutefois, une instance Spot serait le meilleur choix, car elle ne nécessite pas de durée de contrat minimale, est capable de résister aux interruptions et coûte moins cher qu’une instance à la demande.

En savoir plus :

- [Tarification Amazon EC2](https://aws.amazon.com/ec2/pricing/)
</div>
  
  
**Quel processus est un exemple d’Elastic Load Balancing ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Veiller à ce qu’aucune instance Amazon EC2 n’ait à supporter seule la charge de travail complète</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Suppression des instances Amazon EC2 inutiles lorsque la demande est faible</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Ajout d’une deuxième instance Amazon EC2 lors d’une vente très populaire d’une boutique en ligne</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Ajustement automatique du nombre d’instances Amazon EC2 pour répondre à la demande</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Veiller à ce qu’aucune instance Amazon EC2 n’ait à supporter seule la charge de travail complète**.

Elastic Load Balancing est le service AWS qui distribue automatiquement le trafic d’application entrant sur plusieurs ressources, telles que les instances Amazon EC2. Cela permet de s’assurer qu’aucune ressource n’est surexploitée.

Les autres options de réponse sont toutes des exemples d’Auto Scaling.

En savoir plus :

- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing)
- [Auto Scaling Amazon EC2](https://aws.amazon.com/ec2/autoscaling)
</div>
  
  
**Vous souhaitez déployer et gérer des applications conteneurisées. Quel service devriez-vous utiliser ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">AWS Lambda</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Amazon Simple Notification Service (Amazon SNS)</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">Amazon Simple Queue Service (Amazon SQS)</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">Amazon Elastic Kubernetes Service (Amazon EKS)</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Elastic Kubernetes Service (Amazon EKS)**.

Amazon EKS est un service Kubernetes entièrement géré. Kubernetes est un logiciel open source qui vous permet de déployer et de gérer des applications conteneurisées à grande échelle.

Les autres réponses sont incorrectes, car :

- AWS Lambda est un service qui vous permet d’exécuter du code sans allouer ou gérer les serveurs.
- Amazon Simple Queue Service (Amazon SQS) est un service qui vous permet d’envoyer, de stocker et de recevoir des messages entre les composants logiciels via une file d’attente.
- Amazon Simple Notification Service (Amazon SNS) est un service de publication/d’abonnement. À l’aide des rubriques Amazon SNS, un éditeur publie des messages à l’intention des abonnés.

En savoir plus :

- [Amazon EKS](https://aws.amazon.com/eks)
</div>
  
  
Le module suivant examine l’infrastructure mondiale AWS.