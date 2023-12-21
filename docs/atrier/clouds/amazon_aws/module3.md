---
title: Module 3 - des services aux solutions
description: 
published: true
date: 2023-04-21T10:57:35.735Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:57:28.404Z
---

# Aperçu

## Objectifs d'apprentissage

À la fin de ce module, les apprenants seront en mesure d'effectuer les actions suivantes :

- adapter les solutions AWS pour relever les défis des clients ;
- adapter les bonnes pratiques en matière d'architecture et d'optimisation des coûts AWS.

Cette section comprend les points suivants :

- Description de la demande client
- Présentation d'une solution AWS initiale
- Aller plus loin :
  + Approches de migration
  + Bonnes pratiques en matière d'architecture
  + Cadre AWS Well-Architected
  + Cas d'utilisation

# Présentation de la conception de solutions

## Solutions AWS

```
video : 01_presentation_de_la_conception_de_solutions.mkv
```
*Figure. Solution AWS*

Dans le module précédent, vous avez découvert les avantages d'AWS Cloud et certains des principaux services qui aident les clients à tirer parti de ces avantages.

Dans ce module, vous découvrirez comment combiner ces services pour résoudre les défis commerciaux.

Voici quelques exemples de solutions possibles avec AWS.

- **Applications d'entreprise** : créez un ensemble mûr de services spécialement conçu pour les exigences spéciques des grandes organisations en matière de sécurité, de confirmité, de confidentialité et de gouvernance.

- **Stockage** : des options durables et rentables pour la sauvegarde, la reprise après sinistre et l'archivage de donnée à l'échelle de pétaoctets.

- **Calcul sans serveur** : créez et exécutez des applications et des services sans vous soucier des serveurs.

- **Charges de travail Windows** : une capacité de calcul flexible et évolutive pour les applications Microsoft, pour gérer et sécuriser facilement vos charges de travail Windows.

- **Conteneurs** : regroupez et déployez des applications légères et proposez un environnement logiciel cohérent et mobile pour les applications qui pourront s'exécuter et se mettre à l'échelle facilement et de n'importe où.

- **Apprentissage automatique (Machine Learning)** : créez où que vous soyez à l'aide de plateformes et de services complets, ainsi qu'avec l'assistance la plus large pour les frameworks d'apprentissage automatique.

- **Internet des objets (IoT)** : un système de dispositifs omniprésents reliant le monde physique au cloud.

- **Analytique et lacs de données** : stockez, catégorisez et analysez en toute sécurité toutes vos données dans un référentiel centralisé.


Une **solution AWS** peut combiner plusieurs services et fonctionnalités nécessaires pour résoudre un défi commercial.

Voici un exemple de diagramme d'architecture illustrant comment une conception de solution peut se présenter.

![schema_infrastructure.png](/assets/img/clouds/tempo/schema_infrastructure.png =750x)
*Figure. Diagramme d'architecture*

Un diagramme de ce type est conçu pour représenter tous les services inclus, ainsi que l'infrastructure qui les entoure.

Observez les éléments principaux de ce diagramme et nous reverrons les détails à la fin de ce module.

Chaque icône représente un service ou une fonction AWS. 

Par exemple, ces icônes représentent plusieurs **instances EC2** (*Bastion*, *Proxy Web*, *Serveur d'applications*).

Celles-ci représentent des instances de base de données **Amazon RDS** (*MySQL Primary* et *Réplica MySQL*), et **Amazon S3** (*Compartiment Amazon S3*).

Le diagramme indique aussi quelles sont les ressources AWS hébergées dans telle ou telle **zone de disponibilité** (*Zone de disponibilité 1* et *Zone de disponibilité 2*) et quelles sont les ressources prises en charge par **Auto Scaling** (*Bastion*, *Proxy Web*, *Serveur d'applications*) et **Elastic Load Balancer**.

Apprendre à lire et à créer des diagrammes tels que celui-ci est un aspect clé de la conception de la solution.

Après avoir découvert ce qui intègre une conception de solution solide, nous allosn revoir ce diagramme et le présenter en plus de détails.

# Relever les défis des clients

## Maria rencontre son client

```
video : 02_maria_rencontre_son_client.mkv
```
*Figure. Maria rencontre son client*

> **Réunion avec le client**
> - Lee - AnyCompany Apparel
> - Acquisition de ExampleCorp
> - Site e-commerce lent
> - Créé par un sous-traitant
> - Pas assez de ressources
> - Non conforme à la norme PCI
> - Dépend du sous-traitant
> - Préfère moins de pratique

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Bonjour, je suis Maria.

> ![lee.png](/assets/img/lee.png =64x)
> - Bonjour Maria, c'est Lee d'AnyCompagny Apparel. 
> - Je suis ravi de vous rencontrer.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Bonjour Lee, ravie de vous rencontrer également ! Merci d'avoir appelé. 
> - Allons-y. 
> - Pourriez-vous prendre une minute pour m'expliquer votre solution ?

> ![lee.png](/assets/img/lee.png =64x)
> - Bien sûr ! 
> - AnyCompany Apparel est une importante boutique de vêtements en ligne. 
> - Nous existons depuis 8 ans maintenant et avons connu une croissance constante depuis nos débuts. 
> - Il y a environ 2 mois, nous avons acquis une petite entreprise, ExampleCorp Gear. 
> - Ils fabriquent des t-shirts et des sweats à capuche personnalisés. 
> - Leurs clients peuvent concevoir leurs propres créations, ou utiliser le service d'artiste du site pour se faire aider dans la conception de leur vêtement. 
> - Bon nombre de leurs clients sont des musiciens ou des troupes de théâtre. 
> - Je suis le directeur informatique régional et j'ai donc absorbé leur infrastructure. 
> - Leur site est soigné et nous prévoyons de le conserver tel qu'il est pour le moment, mais nous devons le rendre beaucoup plus robuste. 

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - C'est une excellente ligne de travail. 
> - Quels types de problèmes rencontrez-vous ?

> ![lee.png](/assets/img/lee.png =64x)
> - Comme je l'ai dit, la fonctionnalité est bonne, mais depuis quelque temps, les performances sont médiocres. 
> - A certaines heures, le site est très lent pour répondre aux utilisateurs.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Pourquoi, selon vous ?

> ![lee.png](/assets/img/lee.png =64x)
> - Eh bien, un sous-traitant a conçu l'application et l'infrastructure, et il a fait un excellent travail, mais elle fonctionne en interne sur seulement deux ou trois serveurs. 
> - C'était plus que suffisant pour démarrer le site, mais maintenant ce n'est plus le cas.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Je vois. Pourquoi ne pas simplement ajouter d'autres ressources ?

> ![lee.png](/assets/img/lee.png =64x)
> - En fait, ils se penchaient sur la question lorsque nous sommes arrivés. 
> - J'ai effectué un audit du site, puis j'ai également remarqué d'autres choses. 
> - Par exemple, le système de cartes de crédit d'AnyCompany Apparel est conforme à la norme **PCI**. 
> - Bien que leur infrastructure soit assez bien sécurisée, elle n'est pas encore conforme et il faut donc travailler là-dessus également pour y parvenir. 
> - De plus, maintenant que mon équipe est impliquée, il est absurde de faire appel à un sous-traitant.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Cela semble logique. Y-a-t-il d'autres problèmes ?

> ![lee.png](/assets/img/lee.png =64x)
> - Oui, nous n'avons pas acquis de personnel informatique dans le cadre de l'acquisition. 
> - Donc nous espérons aussi qu'une fois que tout sera en place, il s'agira d'un environnement quasi-automatique, du moins au quotidien. 
> - Cela nous permettra de concentrer notre énergie sur la mise en place de nouvelles fonctionnalités.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Je comprends tout à fait. 
> - Si cela ne vous dérange pas, pourriez-vous m'envoyer des détails sur les applications en question ? 
> - Je pourrai alors commencer à travailler sur une solution sur mesure pour vous. 
> - Je vous contacterai lorsque j'aurai quelque chose à partager et nous pourrons alors planifier une réunion. 

> ![lee.png](/assets/img/lee.png =64x)
> - Super ! 
> - Merci Maria, j'ai hâte d'avoir de vos nouvelles. Au revoir.

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Au revoir.

## Examen plus en détail de la configuration actuelle d'AnyCompany

```
video : 03_examen_plus_en_detail_de_la_configuration_actuelle_d_anycompany.mkv
```
*Figure. Examen plus en détail de la configuration actuelle d'AnyCompany*

## Configuration actuelle d'AnyCompany

L'architecture actuelle qu'AnyCompany Apparel cible pour la migration est illustrée ici.

![schema_architecture_app.png](/assets/img/clouds/tempo/schema_architecture_app.png =750x)
*Figure. Configuration actuelle d'AnyCompany*

Il s'agit d'une application Web d'e-commerce monolithique et développée en Java. 

Il s'agit d'une solution à deux niveaux, ce qui signifie un niveau Web et de base de données distincts.

L'application et le serveur Web frontal sont déployés sur une paire de serveurs Web/d'application à charge équilibrée. 

Les serveurs Web/d'application exécutent un serveur Web Apache sur une distribution Ubuntu Linux. 

Ils s'exécutent dans un sous-réseau public auquel les clients accèdent via un pare-feu avec les ports 80 et 443 ouverts.

La base de données est MySQL et s'exécute sur un serveur unique sans redondance. 

Il se trouve dans un sous-réseau privé derrière un pare-feu qui autorise uniquement le trafic sur le port 3306 à partir du serveur Web/d'application via une règle d'adresse IP codée en dur.

La base de données est sauvegardée via une sauvegarde hebdomadaire complète et une sauvegarde incrémentielle nocturne sur bande à l'aide d'un logiciel de sauvegarde sur bande commercial.

## **Maria cherche à concevoir une solution**

```
video : 04_maria_cherche_a_concevoir_une_solution.mkv
```
*Figure. Maria cherche a concevoir une solution*

> **Ce que j'ai pour le moment**
> - Architecture actuelle du client
> - Enjeux relatifs au client
> - Idées de services et de fonctions

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - J'ai vu l'architecture actuelle du client et je comprends parfaitement ses enjeux. 
> - J'ai une bonne idée des services et fonctionnalités qui répondraient à ces défis, mais je dois faire quelques recherches supplémentaires. 
> - Je voudrais présenter au client quelque chose d'incroyable qui couvrirait toutes les bases : **sécurité**, **disponibilité**, **optimisation des coûts**.
> - Quelles sont les approches courantes de ce type de migration ? 
> - Quelles sont les bonnes pratiques ? 
> - Existe-t-il des études de cas concrets sur lesquels je peux m'appuyer ?
> - Je souhaiterais également voir si je peux partager d'autres témoignages client afin de renforcer la confiance du client.

# Stratégies de migration

```
video : 05_strategies_de_migration.mkv
```
*Figure. Stratégies de migration*

## Les sept "R"
> - **Réhéberger**
> - **Restructurer la plateforme**
> - **Relocaliser**
> - **Refactoriser**
> - **Retirer**
> - **Retenir**
> - **Racheter**

Les sept approches courantes de la migration d'architectures vers le cloud sont le **réhébergement**, la **récréation de plateforme**, la **relocalisation**, la **refactorisation**, le **retrait**, la **rétention** et le **rachat**.

Les _quatre premières_ impliquent la migration de charges de travail vers AWS Cloud. _Les autres_ représentent d'autres possibilités de traitement des architectures sur site actuelles.

Les prochaines diapositives les aborderont en détail.

## Réhébergement : approche "lift-and-shift"

> _**Centre de données interne**_
> 
> - **Serveur d'applications** --> Migration vers AWS Cloud --> **Instance EC2**
> - **Serveur d'applications** --> Migration vers AWS Cloud --> **Instance EC2**
> - **Serveur de base de données** --> Migration vers AWS Cloud --> **Instance EC2**
> - **Stockage** --> Migration vers AWS Cloud --> **Compartiment S3**
> - Recréation du réseau sur site, hébergé uniquement sur AWS
> - Automatisation avec des outils tels que **AWS Application Migration Service**
> - Optimisation et réarchitecture simplifiée des applications après la migration

Pour les clients commençant avec une application hébergée sur site, l'approche la plus courante consiste à réhéberger : une approche **"lift-and-shift"** d'une application vers le cloud.

Considérez cela comme un transfert à données constantes de ressources d'application d'un centre de données sur site vers AWS Cloud.

On procède à la migration des serveurs vers des **instances EC2**.

Les configurations réseau sont recréées à l'aide de **VPC**, de **sous-réseaux**, de **groupes de sécurité**, de passerelles Internet et VPN, etc. 
Il s'agit d'une recréation fonctionnelle du réseau sur site, hébergée uniquement sur AWS. 

Le client bénéficie du modèle de paiement à l'utilisation et peut rapidement déployer plus de ressources en fonction de ses besoins.

Des économies significatives peuvent être réalisées, même en exécutant des serveurs de test et de rapports uniquement à certaines heures.

Le réhébergement peut être automatisé à l'aide d'outils tels que **AWS Application Migration Service**.

Certains clients préfèrent le faire manuellement lorsqu'ils apprennent à appliquer leurs systèmes existants à la nouvelle plateforme cloud.

Un autre avantage de cette approche est qu'il est plus facile d'optimiser et de réorganiser les applications après leur migration vers le cloud.

## Relocalisation : approche "lift-and-shift au niveau de l'hyperviseur

> _**Centre de données interne**_
> 
> Infrastructure VMware
>
> **AWS Cloud**
> VMWare Cloud sur AWS
>
> - Migration spécifique à VMware Cloud sur AWS
> - Exemple :
>   + Migration de l'hyperviseur hébergeant une base de données sur VMware Cloud sur AWS

Le relocalisation est identique au réhébergement, mais elle se déroule au niveau de l'hyperviseur.

Cette migration déplace l'infrastructure vers le cloud sans avoir à acheter de nouveau matériel, à réécrire les applications ou à modifier les opérations existantes. Cependant, ce scénario est spécifique à VMware Cloud sur AWS.

VMware Cloud sur AWS permet la compatibilité avec les machines virtuelles et la portabilité des charges de travail entre un environnement sur site et AWS.

Les technologies de la VMware Cloud Foundation peuvent être utilisées à partir des centres de données sur site lors de la migration d'infrastructure vers VMware Cloud sur AWS.

La migration d'un hyperviseur hébergeant une base de données Oracle vers VMware Cloud sur AWS est un exemple de relocalisation.

## Recréation de plateforme : "lift, tinker and shift"

> _**Centre de données interne**_
>
> **Serveur d'applications** --> AWS Cloud --> **Instance EC2**
> **Serveur d'applications** --> AWS Cloud --> **Instance EC2**
> **Serveur de base de données** --> AWS Cloud --> **Amazon RDS**
> **Stockage** --> AWS Cloud --> **Compartiment S3**
> - Conservation de l'architecture de base
> - Optimisation ciblée du Cloud AWS
> - Exemples :
>   + Migration de bases de données vers **Amazon RDS**
>   + Migration d'applications vers **Amazon Elastic Beanstalk**

La recréation de plateforme est similaire à un réhébergement, car l'architecture de base de l'application n'est pas modifiée. 

Il s'agit d'optimiser le cloud ciblé, comme passer à une solution gérée telle que **RDS** ou procéder à la migration de votre application vers une plateforme entièrement gérée comme **Amazon Elastic Beanstalk**. 

Cela peut également impliquer l'optimisation de l'infrastructure à l'aide de bonnes pratiques d'architecture, pour passer d'une application fonctionnelle à une application sécurisée, résiliente et rentable.

## Refactorisation : moderniser

> _**Centre de données interne**_
> 
> **Serveur d'applications** --> AWS Cloud --> **Instance EC2**
> **Serveur d'applications** --> AWS Cloud --> **Instance EC2**
> **Serveur de base de données** --> AWS Cloud --> **Amazon RDS**
> **Stockage** --> AWS Cloud --> **Compartiment S3**
> - Repenser l'architecture et le développement de l'application
> - Utilisation des fonctionnalités natives cloud

La refactorisation implique de repenser l'architecture et le développement de l'application, généralement à l'aide de fonctionnalités natives cloud.

Cette stratégie est motivée par un besoin important des entreprises d'ajouter des fonctions, de mettre à l'échelle ou de gonfler les performances; des éléments difficiles à obtenir avec l'environnement existant de l'application. 

On peut citer la modification d'une structure de base de données, d'une base de données hébergée sur **EC2** ou d'une base de données **RDS** standard vers **Amazon Aurora** ou la modification d'une architecture d'application monolithique vers une conception plus axée sur les services afin de tirer parti des technologies modernes telles que les conteneurs ou les applications sans serveur comme **AWS Lambda**.

Vous en apprendrez plus sur la modernisation des applications plus tard dans ce cours.

## Autres stratégies

> **Retirer**
> - Arrêt des applications inutiles
> - Réduction des dépenses, de la gestion et de la sécurité
>
> **Rétention/réexamen**
> - Maintien de certaines applications sur site
>
> **Racheter**
> - Déplacement de flux de travail vers des logiciels en tant que service (SaaS)

Voici quelques stratégies supplémentaires à prendre en compte lors de l'évaluation d'une migration.

Souvent, un aperçu détaillé de l'architecture d'un client révèle que des applications ne sont plus utiles. 

Certaines d'entre elles peuvent être simplement arrêtées, ce qui rend la quantité d'architecture pour la migration, la maintenance et sécurisation.

Parfois, le client n'est pas prêt pour la migration de certaines applications pour diverses raisons, techniques ou financières, par exemple, une application récemment mise à niveau.

Les clients ne doivent procéder à la migration que de ce qui est logique pour leur entreprise.

Il existe de nombreuses plateformes applicatives basées sur le cloud. 

Les clients peuvent choisir le logiciel en tant que services, ou SaaS, lorsqu'ils souhaitent passer à un mode sans maintenance et le produit offre une connectivité moderne aux données, au traitement cloud et à d'autres applications métier.

Salesforce est un succès, car il peut offrir un CRM fondamentalement meilleur que les applications sur site.

## La stratégie de migration de Maria

```
video : 06_la_strategie_de_migration_de_maria.mkv
```
*Figure. La stratégie de migration de Maria*

> - Restructuration de la plateforme
> - Migration de fonctionnalité
> - Optimisation de composants

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> Etant donné la taille et la compléxité de la charge de travail, je commencerais par restructurer la plateforme, procéderais à la migration certaines fonctionnalités en l'état et envisagerais d'optimiser d'autres composants de l'architecture, tels que l'échange par **Amazon RDS** pour les bases de données principales afin d'améliorer leur résilience. 
> Je vais devoir effectuer des recherches complémentaires pour en savoir plus.

# Bonnes pratiques en matière d'architecture

```
video : 07_strategies_de_migration.mkv
```
*Figure. Stratégie de migration*

## Bonnes pratiques d'architecture cloud

> 1. Concevez en pensant aux défaillances et rien ne sera défaillant
> 1. Intégrez la sécurité dans chaque couche
> 1. Exploitez plusieurs options de stockage
> 1. Implémentez l'élasticité
> 1. Pensez en parallèle
> 1. Le couplage faible vous libère
> 1. Ne craignez pas les contraintes 

Voici quelques principes clés à prendre en compte lors de la conception d'une solution qui suit les bonnes pratiques d'architecture.

### 1. Concevez en pensant aux défaillances et rien ne sera défaillant

Concevez en pensant aux défaillances et rien ne sera défaillant. 

Partez du principe que tout peut défaillir et concevez votre architecture en commençant par la fin.

> - Evitez les points uniques de défaillance
> - Instances multiples
> - Plusieurs zones de disponibilité
> - Séparez un serveur unique en applications à plusieurs niveaux
> - Pour **Amazon RDS**, utilisez la fonction **Multi-AZ**

Concevez l'architecture afin qu'il n'y ait pas de points uniques de défaillance. 

Utilisez plusieurs instances plutôt qu'une. 

Exploitez plusieurs zones de disponibilité pour vos applications afin d'offrir une excellente disponibilité.

Séparez un serveur unique en applications à plusieurs niveaux, où chaque serveur exécute une seule fonction, comme un hôte Web et un serveur de base de données distincts.

Si vous utilisez **Amazon RDS**, utilisez la fonctionnalité **Multi-AZ**.

### 2. Intégrez la sécurité dans chaque couche

Intégrez la sécurité dans chaque couche.

> - Chiffrez les données au repos et en transit
> - Appliquez le principe du moindre privilège dans **IAM**
> - Implémentez des **groupes de sécurité** et des **listes de contrôles d'accès réseau (NACL)**
> - Envisagez des fonctionnalités et services de sécurité avancés


Activez le chiffrement des données pour les données au repos et en transit.

Appliquez le principe du moindre privilège dans **IAM**, de sorte qu'aucun individu ou service n'ait plus de privilèges qu'il n'en a réellement besoin.

Créez un pare-feu robuste en implémentant les **groupes de sécurité** et les **listes de contrôle d'accès réseau** pour vos entités.

Pensez également à utiliser des fonctionnalités et des services de sécurité avancés, tels qu'**Amazon Inspector**, **Amazon GuardDuty** et **AWS Shield**.

### 3. Exploitez plusieurs options de stockage

> - Migrez des ressources Web statiques vers **Amazon S3**
> - Utilisez **Amazon CloudFront** pour être présent dans le monde entier
> - Stockez l'état d'une session dans **DynamoDB**
> - Utilisez **ElastiCache** entre les hôtes et les bases de données

Exploitez plusieurs options de stockage. 

En ce qui concerne le stockage, il n'y a pas de solution universelle. 

Par exemple, au lieu d'héberger tout votre contenu Web à l'aide des **volumes ESB** des hôtes Web, envisagez la migration des ressources Web statiques vers **Amazon S3**. 

Cela permettra également d'utiliser **Amazon CloudFront**, afin de permettre une mise à l'échelle globale de votre contenu Web statique.

Vous pouvez également transférer l'état de session Web de déchargement de votre hôte vers une base de données NoSQL distincte, telle qu'**Amazon DynamoDB**. 

Cela vous permettra de réaliser une mise à l'échelle ascendante et descendante sans perdre les informations de session lorsque cette mise à l'échelle horizontale se produit.

Cela rend également votre niveau Web "**stateless**" pour faciliter la mise à l'échelle.

Enfin, vous pouvez utiliser **ElastiCache** pour stocker les résultats de requête de base de données courants, ce qui réduit la charge au niveau de la base de données.

### 4. Implémentez l'élasticité

> - Implémentez des stratégies **Auto Scaling**
> - Architecturez la résilience pour redémarrer le relancer.
> - Utilisez des services gérés comme **Amazon S3** et **Amazon DynamoDB**

Implémentez des politiques de mise à l'échelle automatique en gardant à l'esprit vos modèles d'accès spécifiques aux applications.

Architecturez la résilience pour redémarrer le relancer. 

Lorsqu'une instance est lancée, elle doit demander "Qui suis-je et quel est mon rôle ?".

Exploitez des services gérés hautement évolutifs tels que **S3** et **DynamoDB**.

### 5. Pensez en parallèle

> - Procédez à une mise à l'échelle horizontale, pas verticale
> - Découplez le calcul de la session/l'état
> - Utilisez **Elastic Load Balancing**
> - Ajustez votre infrastructure


Procéder à une mise à l'échelle horizontale et pas verticale implique d'jouoter plus de ressources de calcul à votre application, plutôt que d'ajouter plus de puissance à vos ressources de calcul.

Découplez le calcul des données d'état et de session pour faciliter la mise à l'échelle et la disponibilité.

Utilisez des **Elastic Load Balancers** pour répartir la charge.

Ajustez votre infrastructure en fonction de votre charge de travail pour obtenir le meilleur équilibre entre les coûts et les performances.

### 6. Le couplage faible vous libère

> - Au lieu d'un seul flux de travail ordonné, utilisez plusieurs files d'attente.
> - Utilisez **Amazon Simple Queue Service** et **Amazon Simple Notification Service** (**SQS** et **SNS**)
> - Exploitez les services existants

Lorsque les services sont faiblement couplés, ils peuvent être mis à l'échelle et être tolérants aux pannes indépendamment les uns des autres. 

Plus le couplage des services est libre, plus ils peuvent être mis à l'échelle.

Au lieu d'un seul flux de travail ordonné, utilisez plusieurs files d'attente.

AWS dispose d'un certain nombre de services capables de résoudre les principaux domaines de fonctionnalités de votre application.

Profitez des services prédéfinis plutôt que d'écrire vous-même de nombreux services.

Ils peuvent vous aider dans différents domaines comme les e-mails, les files d'attente, le transcodage, la recherche, les bases de données et la surveillance et les métriques.

### 7. Ne craignez pas les contraintes 

> - Repensez les contraintes traditionnelles
> - Besoin de plus de RAM ?
> - De meilleures IOPS pour les bases de données ?
> - Répondre à une défaillance ?

Repensez les contraintes architecturales traditionnelles.

Si vous avez besoin de plus de RAM, une solution traditionnelle consiste à installer plus de RAM dans le serveur d'applications.

Pensez plutôt à répartir la charge sur un certain nombre d'instances de base de données.

Si vous avez besoin de plus d'opérations d'entrée/sortie par seconde, ou IOPS, pour une base de données, une solution traditionnelle consiste à retravailler minutieusement un schéma pour augmenter les IOPS.

Là encore, envisagez la mise à l'échelle horizontale en répartissant la charge.

Envisagez de créer un réplica en lecture pour votre base de données et de modifier votre application pour séparer la lecture des bases de données des écritures.

Imaginez que le matériel est défectueux ou qu'une configuration a été corrompue.

Plutôt que de perdre des ressources et du temps précieux pour diagnostiquer les problèmes et remplacer des composants, privilégiez une approche **"rip-and-replace"** (mort et remplacer). 

Il suffit de mettre hors service l'ensemble du composant et de mettre en place un remplacement entièrement fonctionnel.

## Cadre AWS Well-Architected

```
video : 08_cadre_aws_well-architected.mkv
```
*Figure. Cadre AWS Well-Architected*

## Qu'est-ce que le cadre AWS Well-Architected ?

> - Un framework permettant de s'assurer que les infrastructures sont :
>   + Sécurisées
>   + Très performantes
>   + Résilientes
>   + Efficaces
>   + Durables
>
> - Pratiques développées via la révision d'architectures de clients sur AWS
> - Approche systématique pour l'évaluation/l'implémentation d'architectures
> - **Well-Architected Tool** dans la console

Le cadre **AWS Well-Architected** constitue une ressource essentielle pour vous aider à concevoir des solutions conformes aux bonnes pratiques.

Il est conçu pour aider vos clients à créer l'insfrastructure la plus sécurisée, performante, résiliente, efficace et durable possible dans le cloud.

Il a été élaboré en examinant les architectures de milliers de clients sur AWS.

Le framework fournit une approche cohérente pour évaluer les architectures et mettre en oeuvre des conceptions grâce à un ensemble de questions au travers de six pilliers avec des principes de conception qui évoluent en fonction de vos besoins au fil du temps.

De plus, un outil **Well-Architectured Tool** est disponible dans la console AWS. 

Il aide les clients à consulter l'état de vos charges de travail et les comparer aux bonnes pratiques architecturales AWS les plus récentes.

Ce point sera abordé plus en détail ultérieurement dans ce cours.

## Les six piliers

> - Excellence opérationnelle
> - Sécurité
> - Fiabilité
> - Efficacité des performances
> - Optimisation des coûts
> - Durabilité


Le framework est divisé en six domaines d'étude appelés les piliers du cadre **Well-Architected**.

- Le pilier **Excellence opérationnelle** est axé sur l'exécution et la surveillance des systèmes afin de créer une valeur opérationnelle et de perfectionner en permanence les processus et les procédures. 
Les points clés abordent la gestion et l'automatisation des changements, la réactivité aux événements et la définition de normes pour gérer les opérations quotidiennes avec succès.

- Le pilier **Sécurité** est axé sur la protection des informations et des systèmes. 
Les points clés comprennent la confidentialité et l'intégrité des données, l'identification et la gestion des utilisateurs et de leurs autorisations via la gestion des privilèges, et la mise en place de contrôles pour détecter les événements de sécurité.

- Le pilier **Fiabilité** est axé sur la capacité à prévenir les défaillances et à assurer une récupération rapide afin de satisfaire à la demande des clients et de l'entreprise. 
Les points clés abordent des éléments fondamentaux relatifs à la configuration, aux exigences inter-projet, à la planification de la reprise et à la gestion des changements.

- Le pilier **Efficacité des performances** est axé sur l'utilisation efficace des ressources informatiques. 
Les points clés abordent la sélection des type des tailles de ressources appropriés en fonction des exigences de la charge de travail, la surveillance des performances et la prise de décision éclairées pour assurer l'efficacité de l'entreprise au fil de l'évolution de ses besoins.

- Le pilier **Optimisation des coûts** s'attache à l'élimination des coûts superflus. 
Les points clés abordent la compréhension et le contrôle des dépenses, la sélection des types de ressources les plus appropriés et en quantités adéquates, l'analyse des dépenses sur la durée et l'adéquation avec les besoins de l'entreprise, pour éviter les dépenses superflues.

- Le pilier **Développement durable (Durabilité)** met l'accent sur la réduction des impacts sur l'environnement de l'exécution des charges de travail dans le cloud. 
Les points clés incluent un modèle de responsabilité partagée pour plus de durabilité, l'analyse des répercussions et l'optimisation de l'utilisation afin de limiter les ressources nécessaires et de réduire l'impact en aval.

## Exemple Well-Architected : optimisation des coûts

![exemple_well-architected_optimisation_des_couts.png](/assets/img/clouds/tempo/exemple_well-architected_optimisation_des_couts.png =750x)
*Figure. Exemple Well-Architected : optimisation des coûts*

L'une des nombreuses différences entre un environnement sur site et un environnement sur AWS est que sur AWS, les clients ne paient que pour ce qu'ils utilisent. 

L'optimisation des coûts consiste également à s'assurer que les clients ne paient que ce dont ils ont besoin, en exécutant la quantité exacte de ressources nécessaires pour mener à bien leurs activités. 

AWS a identifié quatre bonnes pratiques phares en matière d'optimisation des coûts :

- **Dimensionnement judicieux des instances**
  
  Le dimensionnement judicieux consiste à examiner les ressources déployées et à identifier les éventuelles opportunités de réduction. Par exemple, si une instance d'application sous-utilise constamment sa RAM et son CPU, passer à une instance plus petite peut vous permettre de réaliser des économies significatives tout en conservant les mêmes performances.

- **Augmentation de l'élasticité des applications**

  Par exemple, le recours à la scalabilité automatique permet d'assurer qu'un nombre approprié d'instances est disponible pour gérer la charge de travail d'une application. Effectuez une mise à l'échelle ascendante en cas de forte de demande, et une mise à l'échelle descendante en cas de faible demande.

- **Sélection du bon modèle de tarification**

  Un exemple consiste en l'utilisation d'instances réservées pour les charges de travail qui doivent s'exécuter en permanence ou presque, comme les environnements de production. Cela peut avoir des répercussions significatives sur les économies par rapport aux charges à la demande, dans certains cas jusqu'à 75%.

- **Optimisation du stockage**

  Par exemple, la classe de stockage **S3 Intelligent-Tiering** est conçue pour optimiser les coûts en transférant automatiquement les données vers le niveau de stockage le plus économique.
  
## Le framework d'adoption du Cloud AWS (AWS CAF)

```
video : 09_le_framework_d_adoption_du_cloud_aws_aws_caf_.mkv
```
*Figure. Le framework d'adoption du cloud AWS (AWS CAF)*

> **Réussir et accélérer l'adoption du cloud**
> - La migration vers le cloud est un processus
> - La réussite de la migration vers le cloud requiert de l'expertise.
> - Exploiter les différentes perspectives.
> - S'assurer d'avoir recours aux personnes appropriées.
> - L'équipe **AWS Professional Services** a crée l'**AWS CAF**.
> - **AWS CAF** permet une migration sans heurts vers AWS

La migration vers le cloud est un processus. Il ne suffit pas de claquer des doigts pour que tout soit hébergé par magie dans AWS.

La migration d'applications nécessite des efforts et une migration dans le cloud réussie nécessite une expertise.

Heureusement, de nombreuses personnes ont réussi leur migration vers AWS, et beaucoup de connaissances sur le processus ont été emmagasinées et partagées.

Différents types de personnes apportent des perspectives différentes en ce qui concerne une migration.

Il vous faut exploiter ces différentes perspectives et vous assurer que tout le monde à la même orientation.

Vous devez également vous assurer d'avoir les bons talents pour soutenir votre migration.

Les RH devront donc embaucher en conséquence pour procéder à la migration. 

Cela peut faire beaucoup à suivre, et quelqu'un de peu expérimenté dans le cloud pourrait ne pas penser à toutes les différentes personnes qu'il est nécessaire d'impliquer.

L'équipe **AWS Professional Services** a crée le framework d'adaption d'AWS Cloud, ou **AWS CAF**, pour vous aider à gérer ce processus. 

**AWS CAF** conseille les organisations pour permettre une migration sans heurt vers AWS. 

> _**Capacités de l'entreprise**_
> - Entreprise
> - Personnes
> - Gouvernance
>
> _**Capacités techniques**_
> - Plateforme
> - Sécurité
> - Opérations

Au plus haut niveau, **AWS CAF** organise les conseils en six domaines d'intérêt, appelés **"perspectives"**.

Chaque perspective porte sur des responsabilités distinctes. 

Le processus de planification aide les bonnes personnes de l'organisation à se préparer aux changements à venir.

Leurs perspectives sont regroupées par capacités.

Les perspectives "**Entreprise**", "**Personnes**" et "**Gouvernance**" forment les **capacités commerciales**.

Les perspectives "**Plateforme**", "**Sécurité**" et "**Opérations**" constituent les **capacités techniques**.

  > _**Perspective Entreprise**_
  > - Veiller à ce que l'informatique s'adapte aux besoins de l'entreprise
  > - Créer une solide étude de cas pour l'adaption du cloud
  > - Veiller à ce que les stratégies de l'entreprise s'adaptent aux stratégies informatiques
  > - Les rôles courants comprennent :
  >   + Responsables d'entreprises
  >   + Directeurs financiers
  >   + Responsables de budget
  >   + Parties prenantes stratégiques


- La perspective **"Entreprise"** garantit que l'informatique s'harmonise avec les besoins opérationnels et que les investissements informatiques sont liés aux principaux résultats de l'entreprise. 
Utilisez la perspective **"Entreprise"** pour créer une solide étude de cas pour l'adaption du cloud et prioriser les initiatives d'adoption du cloud. 
Veilez à ce que vos stratégies et vos objectifs opérationnels correspondent à vos stratégies et objectifs informatiques. 
Certains rôles courants de la perspective **"Entreprise"** comprennent les gestionnaires opérationnels, les directeurs financiers, les responsables du budget et les parties prenantes de la stratégie.

  > _**Perspective Personnes**_
  > - Soutenir la stratégie de gestion des modifications
  > - Evaluer les structures et les rôles organisationnels
  > - Evaluer les nouvelles exigences en matière de compétences et de processus
  > - Identifier les lacunes
  > - Prioriser la formation
  > - Rôles courants :
  >   + Ressources humaines
  >   + Personnel
  >   + Managers de personnes

- La persperctive **"Personnes"** aide au développement d'une stratégie de gestion du changement à l'échelle de l'entreprise pour une adoption du cloud réussie. 
Utilisez la perspective **"Personnes"** pour évaluer les structures et rôles organisationnels, les nouvelles exigences en matière de compétence et de processus et identifier des lacunes. 
Cela aide à prioriser la formation, les effectifs et les changements organisationnels. 
La perspective **"Personnes"** comprend des rôles tels que les ressources humaines, le personnel et les gestionnaires d'équipe.


  > _**Perspective Gouvernance**_
  > - Focalisation sur les compétences et les processus
  > - S'assurer que les valeurs opérationnelles sont maximisées, et les riisques minimisés
  > - Actualiser les compétences du personnel et les processus
  > - Mesurer les invetissements dans le cloud afin d'évaluer les résultats opérationnels
  > - Rôles courants
  >   + Chieff Information officer (CIO)
  >   + Responsable de programme
  >   + Enterprise architects
  >   + Analystes métier
  >   + Gestionnaires de portefeuilles

- La perspective **"Gouvernance"** met l'accent sur les compétences et les processus permettant d'harmoniser la stratégie informatique avec la stratégie opérationnelle. 
Cela permet de s'assurer que les valeurs opérationnelles sont maximisées et les risques minimisés. 
Utilisez la perspective "Gouvernance" pour comprendre comment mettre à niveau les compétences du personnel et les processus nécessaires à la gouvernance opérationnelle dans le cloud. 
Gérez et mesurez les investissements dans le cloud afin d'évaluer les résultats opérationnels. Quelques rôles courants dans la perspective "Gouvernance" sont les Chief information officers (CIO), les responsables de programmes, les Enterprise Architects, les analystes d'entreprise et les gestionnaires de portefeuille.

  > _**Perspective Plateforme**_
  > - Implémenter de nouvelles solutions dans le cloud.
  > - Migrer les charges de travail sur site vers le cloud.
  > - Comprendre et communiquer la structure des systèmes informatiques et de leurs relations.
  > - Décrire en détail de l'architecture de l'environnement d'état cible.
  > - Rôles courants :
  >   + Chief Technology Officer (CTO)
  >   + Responsables informatiques
  >   + Architectes de solutions

- La perspective **"Plateforme"** comprennent des principes et des modèles pour la mise en oeuvre des nouvelles solutions dans le cloud et pour la migration de charges de travail sur site vers le cloud. 
Utilisez une variété de modèles architecturaux pour comprendre et communiquer la structure des systèmes informatiques et leurs relations. 
Décrivez en détail l'architecture de l'environnement d'état cible. 
La perspective **"Plateforme"** inclut des rôles tels que les Chief technology officiers (CTO), les responsables informatiques, et les architectes de solutions.

  > _**Perspective Sécurité**_
  > - Répondre aux objectifs de sécurité en matière de visibilité, de capacité d'audit, de contrôle et d'agilité.
  > - Structurer la sélection et l'implémentation de contrôles de sécurité
  > - Rôles courants :
  >   + Chief Information Security Officer (CISO)
  >   + Responsables de la sécurité informatique
  >   + Analystes de la sécurité informatique

- La perspective **"Sécurité"** garantit que l'organisation respecte les objectifs de sécurité en matière de visibilité, de capacité d'audit, du contrôle et d'agilité. 
Utilisez **AWS CAF** pour structurer la sélection et la mise en oeuvre des contrôles de sécurité qui répondent aux besoins de l'organisation. 
La perspective **"Sécurité"** inclut des rôles tels que les Chief Information security officiers (CISO), les responsables de la sécurité informatique et les analystes de la sécurité informatique.

	> _**Perspective Opérations**_
	> - Activer, exécuter, utiliser, exploiter et récupérer les charges de travail informatiques.
  > - Définir le déroulement des activités.
  > - Soutenir les opérations de l'entreprise tout en étant en adéquation avec celles-ci
  > - Définir les procédures opérationnelles actuelles
  > - Rôles courants :
  >   + Responsables des opérations informatiques
  >   + Responsables du support informatique

- La perspective **"Opérations"** aide à activer, exécuter, utiliser, exploiter et récupérer les charges de travail informatiques au niveau convenu avec les parties prenantes opérationnelles. 
Définissez la façon dont les activités quotidiennes, trimestrielles et annuelles sont menées. 
Soutenez les opérations de l'entreprise tout en étant en adéquation avec celles-ci. 
**AWS CAF** aide les parties prenantes à définir les procédures opérationnelles actuelles et à identifier les changements de processus et la formation nécessaires à la mise en oeuvre réussie de l'adoption du cloud. 
La perspective **"Opérations"** inclut des rôles tels que les responsables des opérations informatiques et les responsables du support informatique.

  > _**Plan d'action**_
  > - Repérer les lacunes au niveau des compétences et des processus
  > - S'appuyer sur les entrées pour créer le Plan d'action **AWS CAF**.
  > - Guider la gestion des modifications d'une organisation
  > - Rester sur la bonne voie pour obtenir les résultats souhaités

Chaque perspective révèle des lacunes dans vos compétences et vos processus, qui sont ensuite enregistrées en tant qu'entrées. 

Les entrées sont ensuite utilisées comme base pour la création d'un plan d'action pour le framework d'adoption d'AWS Cloud. 

Le plan d'action sert à guider la gestion des modifications d'une organisation lors de son parcours vers le cloud. 
Un plan d'action adapté à une organisation peut aider à la maintenir sur la bonne voie pour obtenir les résultats souhaités. 

La migration vers le cloud peut être compliquée, mais vous n'êtes pas seul pour y arriver. 

Vous avez accès à de nombreuses ressources pour vous aider à vous lancer. 
Et le cadre d'adoption d'AWS Cloud est un endroit utile pour vos recherches.

# Autres ressources de conception de solutions

## Etude de cas

```
video : 10_etudes_de_cas.mkv
```
*Figure. Etudes de cas*

## Avantages des cas d'utilisation des clients


> _**Sur le papier et en réalité**_
> 
> Découvrez les avantages liés à l'utilisation de cas d'utilisation concrets. 
> En tant que professionnels de l'informatique, il peut être simple de créer quelque chose sur le papier qui suit certaines règles pour produire des résultats prévisibles. 
> Les environnements de production offrent souvent des remaniements uniques nécessitant des modification mineures des architectures de modèle.

Vous informer des solutions créées autour de ces aspects uniques peut vous aider à affiner une conception et vous adapter à ces différences. 

Lorsqu'il s'agit de présenter une solution, les cas d'utilisation ont une place importante. 
Quelle que soit la solidité de la conception et de l'infrastructure sous-jacente, une migration vers le cloud représente souvent un changement opérationnel majeur pour un client. 
Il lui faudra souvent plus que la présentation de dessins architecturaux et de contrats de niveau de service pour dissiper ses craintes. 
Il est souvent moins convaincant de présenter **99,99%** que de présenter les témoignages réels d'un client similaire sur la façon dont ces **99,99%** ont résolu un problème commercial ou directement lié à une valeur opérationnelle supplémentaire.

**AWS Customer Success** (https://aws.amazon.com/solutions/case-studies) est une ressource clé afin d'inspirer la conception de solutions et d'accroître la confiance auprès des clients. 

Il présente des cas de clients de divers types, tailles et domaines de solutions. Ouvrez le site web illustré ici et faites défiler l'écran jusqu'à l'endroit où vous pouvez rechercher un témoignage de client. 

Par exemple, dans le cas de Maria, elle peut vouloir examiner des clients similaires au sien. 

- Effacez donc tous les filtres, puis développez la liste des cas d'utilisation pour affiner la liste des technologies. 
- Développez les secteurs pour voir des études de cas dans des secteurs similaires. 
- Sélectionnez la vente au détail. 
- Voici un cas d'utilisation (abof) spécifique dans le secteur de la mode. 
- Sélectionnez donc celui-ci. 
- En haut, vous trouverez la citation d'un client expliquant comment AWS a eu un impact positif sur son activité. 
- Faites défiler l'écran vers le bas pour trouver les détails architecturaux de cette implémentation.


## Autres ressources

```
video : 11_autres_ressources.mkv
```
*Figure. Autres ressources*

## Solutions AWS et AWS Solution Space

> - Définition d'une **solution AWS** : implémentations de référence techniques et approuvées conçues pour aider les clients à résoudre leurs problèmes courants et à accélérer leurs créations - https://aws.amazon.com/solutions
> - **AWS Solution Space** aide les clients qui ont besoin d'aide pour déployer une solution AWS en mettant en avant les solutions des partenaires AWS du programme de compétences. https://aws.amazon/solutionspace
>
> Les **solutions AWS** ont été créées à l'aide des services d'AWS. Elles sont conçues pour aider les clients à résoudre des problèmes courants et créer plus vite.

Toutes les **solutions AWS** sont approuvées par des architectes AWS et sont conçues pour être efficaces, fiables, sécurisées et rentables.

Chaque **solution AWS** comporte une architecture détailllée, un guide de déploiement et des instructions pour un déploiement manuel ou automatisé. Le cas échéant, les **solutions AWS** intègrent entièrement les offres des partenaires APN pour garantir la solution la plus complète au client.

**AWS Solution Space** fournit aux partenaires AWS du programme de compétences un moyen de démontrer leur expertise. Ces solutions partenaires incluent des fichiers de solution, des **Quick Starts AWS** et des offres de conseil **Solution Space**.

Les **Quick Starts** AWS (https://aws.amazon.com/quickstart) sont une autre ressource conçue pour vous aider à affiner et à accélerer une solution conçue. 

Les déploiements **Quick Starts** sont conçus par les architectes et partenaires de solution AWS pour aider les clients à déployer des technologies connues sur AWS en vous appuyant sur les bonnes pratiques d'AWS pour en assurer la sécurité et la haute disponibilité.

Ces accélérateurs permettent de passer de centaines de procédures manuelles à seulement quelques étapes pour que vous puissiez créer rapidement votre environnement de production et commencer à l'utiliser immédiatement. 

Chaque **Quick Start** comprend des modèles **AWS CloudFormation** qui automatisent le déploiement ainsi qu'un guide qui décrit l'architecture et indique les différentes étapes de déploiement à suivre.

Connectez-vous au site web illustré ici. 

Notez les différents cas d'utilisation disponibles sur la gauche.

Maria souhaite peut-être effectuer des recherches sur la **conformité PCI**. 

Sélectionnez donc le cas d'utilisation **Sécurité, identité et conformité** et recherchez **PCI**. 

Voici un **Quick Start** spécifique pour la **conformité PCI DSS**. 

Cliquez dessus. 

Il existe une grande quantité d'informations sur ce **Quick Start**, y compris un guide de déploiement. 

Faites défiler l'écran vers le bas pour trouver des détails d'architecture supplémentaires sur ce déploiement.


# Solution proposée

## Maria a une solution

```
video : 12_maria_a_une_solution.mkv
```

> ![ichat-icon.png](/assets/img/ichat-icon.png =48x)
> - Prête à proposer une solution !
>   + Sécurisée
>   + Rentable
>   + Evolutive
>   + Conforme
>
> - Je viens de terminer mes recherches. 
> - J'ai travaillé avec l'un de mes collègues, qui est un consultant Professional Services, et nous avons mis au point une solution pour notre client. 
> - Elle répond directement à ses défis commerciaux, tout en étant sécurisée, rentable, évolutive et conforme.

## Solution AnyCompany proposée

```
video : 13_solution_anycompany_proposee.mkv
```
*Figure. Solution AnyCompany proposée*

## Architecture actuelle d'AnyCompany

![solution_anycompany_proposee.png](/assets/img/clouds/tempo/solution_anycompany_proposee.png =750x)
*Figure. Architecture actuelle d'AnyCompany*

> _**Solution à deux niveaux :**_
> - Web
> - Base de données
>
> _**Accès :**_
> - Niveau Web : clients sur les ports Web
> - clients sur les ports Web niveau Web sur les ports de base de données
>
> Sauvegardes de bade de données nocturnes sur bande

![nouvelle_solution_anycompany_proposee.png](/assets/img/clouds/tempo/nouvelle_solution_anycompany_proposee.png =750x)
*Figure. Présentation de la nouvelle architecture*

Voici une autre présentation de l'architecture actuelle d'AnyCompany. Il s'agit d'une solution à deux niveaux avec un niveau exécutant à la fois l'application web et l'application de commerce électronique sur deux serveurs à charge équilibrée et un niveau de serveur de base de données unique.

Le niveau web autorise l'accès aux clients sur les ports web, et le niveau de base de données se trouve dans un sous-réseau privé accessible depuis le niveau web via les ports de base de données.

La base de données effectue des sauvegardes nocturnes sur bande.

La solution proposée prendra en charge les bonnes pratiques AWS conforme au cadre **Well-Architected** et sera sécurisée, résiliente et hautement disponible. 

Elle offre les caractéristiques suivantes :

L'**application à deux niveaux** d'origine a évolué vers une **application à trois niveaux** et la fonctionnalité de base de données est en cours de migration vers **Amazon RDS** à l'aide du moteur **MySQL**. 

Cela réduit considérablement les frais généraux de gestion de la base de données, et offre un basculement et une sauvegarde automatique sur **S3**. 

Des hôtes **bastion** ont également été ajoutés. 

Un hôte **bastion** est un hôte dédié à la gestion des hôtes de production. Il deviendra le point unique de gestion de toutes les instances.

Pour résoudre les problèmes de mise à l'échelle, les niveaux web et application utilise **Elastic Load Balancing** et la scalabilité automatique.

Les instances seront ajoutées et supprimées en fonction de la charge à chaque moment donné.

De plus, **S3** sera déployé pour héberger du contenu web statique, qui peut être diffusé via **Amazon CloudFront** pour une expérience client plus robuste sur plusieurs emplacements géographiques.

> _**Sécurité**_
> 
> - Seuls les hôtes Web dans le sous-réseau public
> - **Groupes de sécurité** :
>   + le Web autorise uniquement le trafic Internet sur 80/443
>   + Les hôtes d'application autorisent uniquement le proxy web sur le port d'application
>   + **RDS** autorise uniquement l'hôte d'application sur 3306
>   + **Bastion** autorise uniquement AnyCompany sur **mgmt**
> - **Liste de contrôle d'accès réseau** en tant que couche supplémentaire
> - Les **compartiments S3** ont des fonctions de sécurité activées
> - Des politiques **IAM standard** avec des groupes associés et des rôles configurés ainsi que les principes du moindre privilège
> - Surveillance et la journalisation

La disponibilité des applications a été obtenue non seulement par le biais de la scalabilité automatique, mais aussi par le fait que chaque niveau utilise **deux zones de disponibilité**. 

Cela inclut la présence d'une réplica MySQL. 

Une autre préoccupation majeure concerne la sécurité. 

Cette préoccupation a été résolue de plusieurs manières.

Dans l'application, seuls les hôtes web se trouvent dans un sous-réseaux public. 

L'application et les **instances RDS** se trouvent dans des sous-réseaux privés.

Les **groupes de sécurité** sont configurés avec les règles suivantes :

- Les hôtes web autorisent le trafic Internet sur les ports 80 et 443 pour les requêtes web.
- Les hôtes d'application autorisent uniquement les connexions de trafic de port d'application à partir d'hôtes web.
- Les **instances RDS** autorisent uniquement les connexions de base de données depuis le port 3306 à partir des hôtes d'application.
- Les hôtes **bastion** autorisent les connexions de gestion uniquement à partir du réseau interne d'AnyCompany.
- Et tous les autres hôtes autorisent uniquement les connexions de gestion depuis les autres hôtes **bastions**.

Les **ACL réseau** filtreront le trafic vers les sous-réseaux, comme une couche supplémentaire de sécurité réseau.

Des **compartiments S3** seront implémentés avec les fonctions de sécurité activées et des politiques **IAM standard** avec des groupes associés et des rôles configurés ainsi que les principes du moindre privilège.

En outre, la surveillance et la journalisation, les alertes et les notifications pour les événements critiques seront configurées.


# Contrôle des connaissances et ressources

## Contrôle des connaissances

Faites correspondre le terme à gauche à sa définition à droite.

|:--:|:----|
|**A**|Mise à l'échelle horizontale|
|**B**|Dimensionnement judicieux des instances|
|**C**|Recréation de plateforme|
|**D**|Cadre Well-Architected|


|:--:|:----|
|**1**|Migration d'une application sur site vers le cloud, tout en optimisant le cloud ciblé|
|**2**|Ajout de ressources de calcul supplémentaires à une application, plutôt que d'ajouter plus de puissance aux ressources de calcul|
|**3**|Examen des ressources déployées, recherche de possibilités de réduire la taille des types d'instances|
|**4**|Ressource critique pour vous aider à concevoir des solutions conformes aux bonnes pratiques|


_**Solution :**_
- **Mise à l'échelle horizontale** = Ajout de ressources de calcul supplémentaires à une application, plutôt que d'ajouter plus de puissance aux ressources de calcul
- **Dimensionnement judicieux des instances** = Examen des ressources déployées, recherche de possibilités de réduire la taille des types d'instances
- **Recréation de plateforme** = Migration d'une application sur site vers le cloud, tout en optimisant le cloud ciblé
- **Cadre Well-Architected** = Ressource critique pour vous aider à concevoir des solutions conformes aux bonnes pratiques

## Ressources dans cette section

- [AWS Application Migration Service](https://aws.amazon.com/application-migration-service/)
- [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)
- [Centre d'optimisation des coûts du Cloud](https://aws.amazon.com/economics/)
- [Framework d'adoption du Cloud AWS](https://aws.amazon.com/professional-services/CAF/)
- [Aperçu du framework d'adoption du Cloud AWS](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
- [Témoignages de réussite de clients](https://aws.amazon.com/solutions/case-studies)
- [Bibliothèque de solutions AWS](https://aws.amazon.com/solutions/)
- [Offres de conseil en matière de solutions AWS](https://aws.amazon.com/solutionspace)
- [AWS Quick Starts](https://aws.amazon.com/quickstart)

