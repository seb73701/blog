---
title: Module 5 - perspectives
description: 
published: true
date: 2023-04-21T10:57:54.191Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:57:47.759Z
---

# Aperçu

## Objectifs d'apprentissage

Cette section comprend les points suivants :

- Implémentation
  + Migration
  + Transition vers le mode production
  + Produit minimum viable (MVP)
  + Vérification Well-Architected
- Modernisation
  + Modernisation avec AWS
  + Architectures modernes : sans serveur, conteneurs, analytique


# Planification de la migration

## Maria planifie la migration

```
video : xxx.mkv
```
*Figure. Maria planifie la migration*

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Mon client est satisfait de la preuve de concept et est prêt à passer à la mise en place et la migration de ses données.
> - Que puis-je faire pour que cette opération se déroule sans accroc et qu'elle soit conforme aux réglementations que dois-je respecter ?
> - Une fois qu'il a migré et qu'il est opérationnel, comment puis-je rester impliquée avec lui et l'aider à continuer à développer ses solutions pour améliorer ses résultats commerciaux ?


## Considérations relatives à la migration

```
video : xxx.mkv
```
*Figure. Considérations relatives à la migration*

## Le processus de migration

> _**Evaluation**_
> - Identification de l'état de préparation
> - Identification des résultats commerciaux potentiels
> _**Préparation et planification**_
> - Analysez son environnement
> - Définition de stratégies de migration
> - Création d'une zone d'atterrissage Well-Architected
> _**Migration et modernisation**_
> - Conception, migration et validation de chaque application
> - Automatique ou manuelle
> - Migration des données
> - Modernisation

Le processus de migration en trois phases est conçu pour aider votre organisation à adopter une migration d'applications. Bien que chaque phase soit un composant courant d'une migration réussie, il ne s'agit pas de phases ponctuelles, mais d'un processus itératif. 

AWS fournit un portefeuille complet d'outils de migration AWS pour simplifier et accélerer chaque étape du processus de migration en trois phases.

Au cours de la phase d'**évaluation**, identifiez l'état de préparation actuel de votre client pour l'exploitation dans le cloud et les résultats commerciaux potentiels de la migration.

Au cours de la phase de **préparation et de planification**, corrigez les écarts de préparation de votre client qui ont été découverts lors de la phase d'évaluation. 

Analysez son environnement, créez une carte des interdépendances et déterminez des stratégies de migration, telles que le réhébergement ou la recréation de plateforme pour créer un plan de migration détaillé avec des priorités pour chaque application. 

A ce stade, vous configurez un environnement AWS multicompte sécurisé et bien architecturé appelé **AWS Landing Zone**.

Dans la phase de **migration**, l'attention passe du niveau du portefeuille à l'application individuelle. Chaque application est conçue, transférée et validée. 

Procédez à la migration automatique ou manuelle des applications depuis différents environnements source, tels que les environnements physiques, virtuels ou basés sur le cloud, vers AWS. 

De plus, vous aurez probablement besoin d'une migration unique d'un volume important de données vers AWS. 

Au cours de la migration, vous itérez pour pouvoir exploiter, gérer et optimiser efficacement les charges de travail dans le cloud. 

Idéalement, vous vous appuyez sur l'expertise fondamentale que vous avez déjà développée. 

Si ce n'est pas le cas, utilisez vos premières migrations d'applications pour développer ces bases afin d'améliorer et de complexifier votre modèle d'exploitation.

## Produit minimum viable (MVP)

Lorsque vous mettez en place une nouvelle solution pour votre client, vous pouvez suivre le concept **produit viable minimum**, aussi appelé **MVP**. 

C'est un excellent moyen d'encourager le processus d'adoption afin que les clients constatent des progrès dès le début, plutôt qu'à la fin d'un long processus.

Qu'est-ce que le **MVP (produit viable minimum)** ?

Un **produit viable minimum** est un produit fonctionnel ou une solution avec suffisamment de fonctions pour satisafaire les exigences des clients lors de leur adoption initiale, mais qui laisse de la place aux commentaires pour le développement futur du produit ou de la solution.

Pendant le cycle de livraison, encouragez vivement les clients à filtrer les idées de nouvelles fonctions et capacités en gardant à l'esprit le **MVP**. 

Essayez d'éviter les solutions "big bang" qui n'ont de valeur qu'à la fin d'un cycle de développement de produit. 

Au lieu de cela, commencez par quelque chose de base et collectez des commentaires pour vous complexifiez.


## MVP et fourniture de résultats

(mettre capture : 2:46)

Le MVP est la différence entre obtenir la valeur ici à la fin du projet et obtenir la valeur à différents stades d'adoption tout au long du projet.

Vous vous posez peut-être des questions sur la différence entre une **preuve de concept** et un **MVP**.

La différence est qu'une POC est une démonstration plus rapide de la solution, alors que le MVP est un niveau de production.

## Stratégies de migration

(mettre capture : 3:05)

Voici un aperçu de certaines des étapes impliquées dans les migrations classiques, en fonction de la méthodologie.

- Le **réhébergement**, ou l'approche "**lift-and-shift**", peut-être effectué entièrement manuellement ou en grande partie automatisé. 
  + La **réalisation manuelle** de cette opération implique une installation, une configuration et un déploiement manuels des composants de l'application. Une fois les applications terminées, elles sont validées sur AWS. Ensuite la production est transférée. Enfin, la production complète est réalisée sur AWS.
  + Si vous utilisez l'**automatisation**, les outils de migration peuvent supprimer toutes ou certaines étapes ci-dessus.
- Lors de la **recréation de plateforme**, ou déplacement, changement et bricolage, la première étape consiste à définir la nouvelle plateforme cible pour l'application. Ensuite, l'infrastructure actuelle peut nécessiter quelques modifications pour se préparer à la migration. Là encore, les outils de migration peuvent être utilisés pour une transition plus homogène.
- La **refactorisation** par nature nécessite de nombreuses étapes supplémentaires. Tout d'abord, l'application doit être repensée pour l'application et l'infrastructure cibles. On développe ensuite le code pour l'architecture cible. Ensuite, une gestion du cycle de vie complet de l'application, ou ALM, ou un cycle de vie de développement logiciel doit être effectué, suivi de l'intégration complète de l'application cible.

## La migration d'AnyCompany : restructuration de la plateforme

(remettre le schema d'architecture simple : 5:55)

La migration de n'importe quelle entreprise est considérée comme une recréation de plateforme. 

La fonction et l'opération de base seront les mêmes, mais certaines modifications sont appropriées pour répondre à certaines spécificités de leurs besoins et objectifs commerciaux.

Voici un bref aperçu de sa migration.

Tout d'abord, l'infrastructure réseau est créée en créant un VPC et des sous-réseaux.

La sécurité des ports est ajoutée via des groupes de sécurité et des ACL réseau.

L'accès au site est assuré par la traduction d'adresses de réseau, ou NAT, et par des passerelles Internet.

La fonction principale de l'application fait l'objet d'une recréation de plateforme.

Le serveur web/d'applications est divisé en différents niveaux :

Un hôte web et un hôte d'application distincts.

Enfin, un troisième et nouvel hôte, l'hôte bastion, est ajouté.

Ils seront soigneusement configurés en tant qu'Amazon Machine Images, qui seront utilisés pour la scalabilité automatique et la répartition de charge.

L'hôte MySQL est en cours de recréation de plateforme en tant qu'Amazon RDS utilisant MySQL.

Ensuite, la répartition de charge et la mise à l'échelle sont assemblées grâce à la création et à la configuration de groupes à scalabilité automatique et d'équilibreurs de charge d'application.

Enfin, les compartiments S3 seont configurés pour être utilisés pour les ressources web statiques et les sauvegardes de base de données.

Une fois tous ces éléments assemblés, les données et le code d'application sont transférés manuellement vers l'instance web et l'instance d'application.

La base de données MySQL peut être transférée vers RDS à l'aide d'**AWS Database Migration Service**.


## Transition vers le mode production

```
video : xxx.mkv
```
*Figure. Transition vers le mode production*

> _**Bonnes pratiques :**_
> - Implication de l'équipe de compte AWS (architecte de solutions ou Technical Account Manager)
> - Exigences réglementaires spécifiques aux clients
> - Niveau d'AWS Support

Lorsque votre client est prêt à passer en production sur AWS, il existe quelques bonnes pratiques pour initier le processus.

Il est fortement recommandé de contacter l'équipe du compte AWS de ce client et de lui indiquer que vous êtes prêt à initier un nouveau projet. 

C'est l'occasion de faire appel à l'architecte de solutions ou au Technical Account Manager pour fournir une assistance technique si nécessaire. 

Si votre client a des exigences réglementaires spécifiques, n'oubliez pas de demander s'il existe une liste de contrôle de mise en place à utiliser pour vous assurer que la solution est conforme. 

Par exemple, dans le cas de la cliente de Maria, elle doit s'interroger sur la conformité PCI. 

Vous pouvez également contacter les équipes de sécurité AWS ou l'architecte de solutions pour obtenir des informations sur la conformité. 

N'oubliez pas que, conformément au modèle de responsabilité partagée d'AWS, le client est responsable de la conformité aux réglémentations spécifiques de sa solution, mais AWS peut fournir des conseils. 

Parfois, cela dépend du niveau **AWS Support** du client. 

Par exemple, si le client opte pour le support d'entreprise, il aura accès à un **Technical Account Manager** qui peut contrôler de manière proactive son environnement et aider à l'optimisation.

## Examen Well-Architected

> - Conseils en matière d'architecture
> - Examen continu
> - Architectures améliorées

Une fois qu'une solution a été mise en place, il est recommandé d'exécuter un examen **Well-Architected** régulier des charges de travail. 

**AWS Well-Architected Tool** vous aide à examiner l'état de vos charges de travail et à les comparer aux bonnes pratiques architecturales AWS les plus récentes. 

Ces tâches peuvent être effectuées par des partenaires APN ou par les clients eux-mêmes.

L'examen fournit des conseils architecturaux, répond à une sérue de questions sur la charge de travail et l'outil fournit un plan d'action avec des instructions étape par étape sur la manière de créer de meilleures charges de travail pour le cloud.

Examiner vos charges de travail de manière cohérente. 

L'outil vous permet de surveiller leur état global et vous aide à comprendre les risques potentiels. 

Utilisez les résultats fournis par l'outil pour identifier les prochaines étapes d'amélioration, prendre des décisions architecturales et intégrer des considérations d'architecture dans le processus de gouvernance de votre entreprise.

Identifier et mettre en place des améliorations. 

Utilisez **AWS Well-Architectured Tool** pour prendre en charge les améliorations continues tout au long du cycle de vie de la charge de travail. 

Cet outil facilite la sauvegarde ponctuelle des jalons et le suivi des modifications apportées à votre charge de travail. 

Vous n'avez qu'à initier un nouveau processus de révision pour vous assurer que votre architecture continue de s'améliorer au fil du temps.

## AWS Well-Architected Tool

L'examen utilise l'outil gratuit **AWS Well-Architected Tool**, disponible dans **AWS Management Console**.

Tout d'abord, identifiez la charge de travail à documenter, puis répondez à une série de questions concernant votre architecture.

Passez en revue vos réponses par rapoorts aux piliers établis par le cadre de **Well-Architected**.

Lorsque vous avez terminé, vous pouvez obtenir des vidéos et de la documentation, un rapport qui résume l'examen de votre charge de travail ainsi que les résultats des examens dans un seul tableau de bord.


# Modernisation

## Modernisation pour stimuler la croissance

```
video : xxx.mkv
```
*Figure. Modernisation pour stimuler la croissance*

> Mise hors service des solutions héritées onéreuses
> Réduction du coût total de possession (TCO), optimisation des coûts
> Gain en agilité à travers l'automatisation
> Libération de ressources pour stimuler l'innovation

Les entreprises cherchant à moderniser la technologie peuvent économiser de l'argent en utilisant AWS à mesure qu'elles créent de nouvelles applications et mettent hors services des solutions héritées. 

Lorsqu'un client planifie sa migration cloud vers AWS, son organisation peut réduire considérablement le coût total de possession (TCO), ce qui libère des ressources pour se concentrer sur la mission principale de l'entreprise et non sur la gestion des serveurs et des bâtiments. 

De plus, l'ampleur de l'hyperscalabilité des services et du niveau d'automatisation dans AWS peut permettre de réaliser des économies progressives et, au fil du temps, une optimisation significative des coûts. 

Le déploiement de solutions d'entreprise dans AWS peut aider vos clients à mettre hors service une infrastructure héritée onéreuse, à réduire leurs coûts, à gagner en agilité grâce à l'automatisation et à libérer des ressources pour favoriser l'innovation, au lieu de se concentrer sur un travail indifférencié.

## Modernisation des architectures

> Conteneurs
> Sans serveur
> Lacs de données et analytique

Quels sont alors les moyens de moderniser les architectures avec AWS ?

Dans cette section, vous découvrirez les conteneurs, le calul sans serveur, ainsi que les lacs de données et l'analytique. 

Il ne s'agit que de quelques-unes des nombreuses solutions proposées par AWS.

Consultez les liens situés dans la section des ressources pour en savoir plus.

## Conteneurs

```
video : xxx.mkv
```
*Figure. Conteneurs*

> _**Conteneurs**_
> - Regroupement du code, des configurations et des dépendances dans un seul objet
> - Partage d'un système d'exploitation
> - Exécution en tant que processus isolés de ressources
> - AWS propose des ressources et des services d'orchestration

Les conteneurs fournissent un moyen unique de joindre le code, les configurations et les dépendances de votre application en un seul objet. 

Les conteneurs partagent un système d'exploitation installé sur le serveur et s'exécutent en tant que processus à ressources isolées, assurant des déploiements rapides, fiablers et cohérents, quel que soit l'environnement. 

AWS Cloud offre des ressources d'infrastructure optimisées pour l'exploitation des conteneurs, ainsi qu'un ensemble de services d'orchestration qui vous facilitent la création et l'exploitation d'applications conteneurisées en production.

> _**Cas d'utilisation**_
> - Microservices
> - Traitement par lot
> - Apprentissage automatique
> - Applications hybrides
> - Migration d'applications vers le cloud
> - Plateforme en tant que service

Quels sont les cas d'utilisation potentiels des conteneurs ?

Les conteneurs assurent l'isolation des processus, ce qui facilite le démontage et l'exécution des applications en tant que composants indépendants appelés microservices. 

Grouper le traitement par lots et les tâches ETL dans des conteneurs pour démarrer rapidement les tâches et les mettre à l'échelle de façon dynamique en fonction de la demande.

Utiliser des conteneurs pour rapidement mettre à l'échelle des modèles d'apprentissage automatique à des fins d'entraînement et d'inférence et les exécuter à proximité de vos sources de données sur n'importe quelle plateforme. 

Les conteneurs permettent à vos clients de normaliser la manière dont le code est déployé, ce qui facilite la création de flux de travail pour les applications qui s'exécutent entre les environnements sur site et les environnements de cloud. 

Les conteneurs facilitent le regroupement d'applications entières et leur déplacement vers le cloud sans avoir besoin de modifier le code. 

De plus, les conteneurs peuvent être utilisés pour créer des plateformes qui n'obligent plus les développeurs à gérer l'infrastructure et à normaliser la façon dont les applications sont déployées et gérées.


## Sans serveur (serverless)

```
video : xxx.mkv
```
*Figure. Sans serveur (serverless)*

> _**Serverless**_
> |Catégorie|Services AWS entièrement gérés pour le serverless|
> |:--------|:------------------------------------------------|
> |Calcul|AWS Lambda<br>Lambda@Edge<br>AWS Fargate|
> |Stockage|Amazon Simple Storage Service<br>Amazon Elastic File System|
> |Magasins de données|Amazon DynamoDB<br>Amazon Aurora Serverless|
> |Proxy API|Amazon API Gateway|
> |Intégration d'applications|Amazon SNS<br>Amazon SQS<br>AWS AppSync<br>Amazon EventBridge|
> |Orchestration|AWS Step Functions|
> |Analytique| Amazon Kinesis<br>Amazon Athena|
> |Outils du développeur|Outils et services divers|

> _**Avantages**_
> - Pas d'approvisionnement, de maintenance et d'administration de serveurs
> - AWS gère la tolérance aux pannes et la disponibilité
> - Accent sur l'innovation de produits

AWS propose un ensemble de services entièrement gérés que vous pouvez utiliser pour concevoir et exécuter des applications sans serveur (serverless).

Les applications sans serveur ne nécessite pas de mettre en service, de maintenir ni d'administrer des serveurs pour les composants dorsaux (backend) comme le calcul, les bases de données, le stockage, le traitement de flux, les files d'attente de messages et bien plus. 

Il n'y a plus besoin de se préoccuper de la tolérance aux pannes ni de la disponibilité de l'application. 

Désormais, AWS gère toutes ces capacités. 

Ainsi, vos clients peuvent se concentrer sur la conception de produits innovants tout en profitant d'une accélération de la mise sur le marché.

## Lacs de données et analytique

```
video : xxx.mkv
```
*Figure. Lacs de données et analytique*

> _**Analytique et lacs de données**_
> - Il peut être difficile d'accéder aux données se trouvant dans plusieurs silos
> - Stockage de données dans un "lac de données" (data lake)
> - Lecture facile des données et obtention d'informations

Au fur et à mesure que l'activité se développe, les données s'accumulent. 

Inévitablement, elles sont placées dans différents silos, ce qui rend leur accès et leur analyse difficiles. 

Le stockage de données à un seul endroit, aussi appelé **lac de données**, peut résoudre ce problème. 

Les **lacs de données** aident les entreprises à optimiser leurs opérations en facilitant la lecture des données et l'obtention d'informations. 

Pour créer vos lacs de données et solutions d'analytique, AWS propose l'ensemble de services le plus complet pour déplacer, stocker et analyser vos données.

Importez vos données depuis les sites en temps réel. 

Stocker en toute sécurité tous les types de données, des gigaoctets aux exaoctets. 

Analysez vos données grâce à la sélection la plus vaste de services d'analyse. 

Estimez les résultats futurs et défijnissez les actions à effectuer pour réagir rapidement.

Les organisations qui génèrent avec succès une valeur opérationnelle à partir de leurs données surprendront leurs homologues. 

La possibilité d'exploiter davantage de données, à partir de plus de sources, en moins de temps, et de permettre aux utilisateurs de collaborer et d'analyser des données de différentes manières conduit à une prise de décision meilleure et plus rapide.

> _**Valeur**_
> - Interactions client améliorées
> - Choix d'innovation en recherche et développement améliorés
> - Amélioration de votre efficacité opérationnelle

Parmi les exemples où les lacs de données ont apporté une valeur ajoutée, citons l'amélioration des interactions avec les clients, l'améliration des choix d'innovation en R&D et l'amélioration de l'efficacité opérationnelle.

## Lacs de données et solutions d'analytique sur AWS

> **Analytique interactive** : Amazon Athena
> **Traitement du Big Data** : Amazon EMR
> **Stockage de données** : Amazon Redshift
> **Analytique en temps réel** : Amazon Kinesis
> **Analytique opérationnelle** : Amazon OpenSearch Service
> **Tableaux de bord et visualisations** : Amazon QuickSight
> **Catalogue et sécurisation** : AWS Lake Formation

AWS propose l'ensemble de services d'analyse de données le plus vaste et le plus économique pour exécuter un lac de données. 

Chaque service d'analytique est spécifiquement conçu pour divers cas d'utilisation d'analytique, comme l'analytique interactive, le traitement big data à l'aide d'Apache Spark et Hadoop, le stockage de données, l'analytique en temps réel, l'analytique opérationnelle, les tableaux de bord, les visualisations, le catalogage et la sécurisation.

- Pour l'**analyse interactive**, **Amazon Athena** facilite l'analyse de données directement dans **S3** et **Glacier** grâce aux requêtes SQL standard. **Athena** fonctionne _sans serveur_ et il n'y a donc pas d'infrastructure à configurer ou à gérer. Les clients peuvent instantanément démarrer des requêtes de données, obtenir des résultats en quelques secondes et ne payer que les requêtes qu'ils exécutent.

- Pour le **traitement Big Data** à l'aide des frameworks Spark et Hadoop, **Amazon EMR** fournit un service géré qui permet de traiter de manière simple, rapide et économique de grandes quantités de données.

- Pour le **stockage de données**, **Amazon Redshift** permet d'exécuter des requêtes analytiques complexes sur des pétaoctets de données structurées et inclut Redshift Spectrum, qui exécute des requêtes SQL directement sur des exaoctets de données structurées ou non dans S3 sans avoir à déplacer des données inutiles.

- Pour l'**analytique en temps réel**, **Amazon Kinesis** simplifie la collecte, le traitement et l'analyse de données de streaming comme les données télémétrique IoT, les journaux d'application et les flux de clics de site Web. Ce service permet aux clients de traiter et d'analyser des données à mesure de leur réception dans le lac de données et de réagir en temps réel au lieu d'attendre que toutes les données soient collectées pour démarrer leur traitement.

- Pour l'**analytique opérationnelle** comme la surveillance d'application, l'analyse de journaux et l'analyse des sites Web, la suite distribuée de recherche et d'analyse open source, **Amazon OpenSearch Service**, permet aux clients de rechercher, d'explorer, de filtrer, d'agréger et de visualiser les données pratiquement en temps réel. **Amazon OpenSearh Service** est le successeur d'**Amazon Elasticsearch Service**. Il fournit des recherches ouvertes, des API faciles à utiliser et des capacités d'analytique en temps réel parallèlement à la disponibilité, la scalabilité et la sécurité nécessaires aux charges de travail de production.

- Pour les **tableaux de bord et les visualisations**, **Amazon QuickSight** propose aux clients un service d'analytique commerciale rapide et optimisé pour le cloud qui simplifie la création de visualisations et de tableaux de bord riches et impressionants auxquels il est possible d'accéder à partir de n'importe quel navigateur ou appareil mobile.

- Pour **cataloguer et sécuriser les données**, **AWS Lake Formation** facilite la création, la sécurisation et la gestion des lacs de données  en simplifiant et en automatisant un grand nombre des étapesmanuelles complexes généralement nécessaires pour créer des lacs de données. **Lake Formation** est pointé vers les sources de données, puis **Lake Formation** explore ces sources de données et transfert les données vers un nouveau lac de données **Amazon S3**.

## Maria a un plan

```
video : xxx.mkv
```
*Figure. Maria a un plan*

> Plan de migration : restructuration de la plateforme
> - Construire l'environnement
>   + Migrer les données
> - Valider la fonctionnalité
> - Exécuter en parallèle avec le site existant
> - Effectuer la transition vers le site AWS
> - Vérification AWS Well-Architected
>   + Coût total de possession (TCO)
>   + Optimisation des coûts
> - Modernisation
> - Analytique
>
> Pour continuer à apprendre...
> - Etudier des cas d'utilisation de clients
> - Exploiter l'expertise interne
> - Formation
>
> Etape suivante :
> - Preuve de concept (POC)
>
> Client :
> - ExampleCorp Gear + AnyCompany Apparel

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Il s'agit d'excellentes informations pour n'importe quelle entreprise.
> - Nous avons un plan de migration solide basé sur la recréation de plateforme de son architecture actuelle.
> - Nous construisons l'environnement et nous procèderons à la migration des données après cela.
> - Une fois mise en place, nous validerons la fonction, exécuterons le site en parallèle avec son site existant, puis nous effectuerons la transition vers le site AWS.



Une fois qu'ils sont opérationnels, je vais suggérer d'exécuter un examen **Well-Architected**. L'un des principaux domaines sera le coût total de possession, ou TCO, afin de s'assurer que toute entreprise bénéficie de la meilleure valeur possible de son architecture.

En apprenant certaines options de modernisation, je suis en mesure d'aider mon nouveau client. Le client prédit une croissance importante pour cette application. Il est nécessaire de commencer à intégrer l'analytique afin d'améliorer l'interaction et l'innonvation du client.

Je pourrais commencer par étudier des cas d'utilisation sur la façon dont les clients de commerce électronique développent leur histoire d'analytique avec AWS.

Je pourrais tirer parti de l'expertise interne en matière de données et d'analytique, tout en développant ma propre expertise grâce à la formation.

Finalement, nosu avons pu créer une POC à l'aide de services de données et d'analytique, afin de les partager avec le client.

Waouh ! Mon premier client.

Je suis impatiente de voir son application réussir et prospérer sur AWS.


# Contrôle des connaissances et ressources

## Contrôle des connaissances

Faites correspondre le terme à gauche à sa définition à droite.

|:--:|:----|
|**A**|Sans serveur|
|**B**|AWS Well-Architected Tool|
|**C**|Conteneurs|
|**D**|Produit viable minimum (MVP)|


|:--:|:----|
|**1**|Solution ou produit fonctionnel disposant de suffisamment de fonctions pour répondre aux exigences|
|**2**|Services fournissant des fonctionnalités sans configurer de composants dorsaux (backend)|
|**3**|Vérification de l'état des charges de travail actuelles et comparaison avec les pratiques architecturales AWS|
|**4**|Packages avec le code d'application, la configuration et les dépendances dans un seul objet|

_**Ma réponse :**_

|:--:|:----|:--:|
|A|2|<span style='color:green'>OK</span>|
|B|3|<span style='color:green'>OK</span>|
|C|4|<span style='color:green'>OK</span>|
|D|1|<span style='color:green'>OK</span>|



_**Solution :**_


|:--:|:----|
|**A**|Services fournissant des fonctionnalités sans configurer de composants dorsaux (backend)|
|**B**|Vérification de l'état des charges de travail actuelles et comparaison avec les pratiques architecturales AWS|
|**C**|Packages avec le code d'application, la configuration et les dépendances dans un seul objet|
|**D**|Solution ou produit fonctionnel disposant de suffisamment de fonctions pour répondre aux exigences|

## Ressources dans cette section

- [Comment procéder à une migration ?](https://aws.amazon.com/cloud-migration/how-to-migrate/)
- [Modernisez vos applications, stimulez la croissance et réduisez le coût total de possession](https://aws.amazon.com/enterprise/modernization/)
- [Sans serveur sur AWS](https://aws.amazon.com/serverless/)
- [Conteneurs chez AWS](https://aws.amazon.com/containers)
- [Analytique sur AWS](https://aws.amazon.com/big-data/datalakes-and-analytics/)
