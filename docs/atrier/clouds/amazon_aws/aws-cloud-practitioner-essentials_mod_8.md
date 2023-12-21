---
title: Module 8 - Tarification et support
description: 
published: true
date: 2023-04-21T10:56:48.502Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:42.000Z
---

# Module 8 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Décrire les modèles de support et de tarification AWS
- Décrire l’offre gratuite d’AWS
- Décrire les principaux avantages d’AWS Organizations et de la facturation consolidée
- Expliquer les avantages de AWS Budgets
- Expliquer les avantages d’AWS Cost Explorer
- Expliquer les principaux avantages de la calculatrice de prix AWS
- Faire la distinction entre les diverses formules AWS Support
- Décrire les avantages d’AWS Marketplace

```
video : xxx.mkv
```
*Figure. Module introduction - Pricing and support*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Maintenant que Morgan est propriétaire du café, associés à cette responsabilité supplémentaire. Des coûts comme le bail, les tables, le coût des employés, bien sûr. Qu'en est-il des taxes ? Et l'électricité, le chauffage et la climatisation ? Et des jardiniers qui embellissent l’extérieur ? La question la plus importante est : combien cela va-t-il coûter le mois prochain de maintenir la boutique ouverte ? Il y a beaucoup d'éléments changeants. 
> 
> Nous pourrions certainement estimer les coûts mensuels pour évaluer les coûts du mois suivant, en sachant que la saison de grande activité arrive. Afin de connaître le montant exact, nous devons connaître le coût de chaque élément ainsi que le nombre de ces éléments que nous prévoyons d’utiliser le mois suivant. Puis, il faut mettre ces chiffres dans un tableau et calculer la prévision. Terminé. 
> 
> Mais nous pourrions aussi nous poser des questions sur les coûts qu’engendrerait le déménagement dans une autre ville, et sur la différence des coûts en abordant la tarification et le support. Je ne veux pas gâcher le plaisir, mais sachez qu'AWS fournit de nombreux outils gratuits pour vous aider à planifier et analyser vos budgets pour vos environnements AWS. Allons-y.
</div>

# Offre gratuite d’AWS

```
video : xxx.mkv
```
*Figure. AWS Free tier*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Vous venez de créer un tout nouveau compte AWS et vous souhaitez commencer, mais vous avez peur d'épuiser votre limite de crédit. Ne vous inquiétez pas. Si vous voulez essayer certains services AWS, vous pouvez le faire par le biais de votre offre gratuite. 
> 
> Selon le produit que vous cherchez, l’offre gratuite propose trois façons différentes d’essayer un service. D'abord, l'offre Toujours gratuit. Cela signifie qu'un service est disponible pour tous les clients AWS et que l’offre gratuite n’expire pas. Ensuite, l'offre 12 mois gratuits. Cela signifie que vous disposez de 12 mois pour essayer un service à compter de votre date d'inscription initiale à AWS, soit la date à laquelle vous avez créé votre compte AWS. Enfin, l'offre Essais. Certains services offrent un essai gratuit à court terme, puis expirent après la fin de cette période. 
> 
> Assez simple, n'est-ce pas ? Illustrons-les avec quelques exemples. Prenons AWS Lambda, notre option de calcul sans serveur. À compter de mars 2020, il permet un million d'appels gratuits par mois. Cela signifie que si vous restez sous la limite d'un million d'appels, c’est toujours gratuit. Cette offre gratuite n'expire jamais. Un autre exemple est S3, notre service de stockage d'objets. Il est gratuit pendant 12 mois jusqu'à 5 Go de stockage. Après cette limite, vous êtes facturé. C'est parfait pour essayer un site web statique sur S3. Le dernier exemple est Lightsail, qui vous permet de déployer des piles d’applications prêtes à l’emploi. Nous offrons un essai d'un mois avec jusqu'à 750 heures d'utilisation. Cette belle offre est suffisante pour déployer et tester un blog WordPress. Vous voulez peut-être communiquer aux autres vos recettes de smoothies. 
> 
> J'en reviens au sujet principal, d’autres services font partie de l’offre gratuite, SageMaker, Comprehend Medical, DynamoDB, SNS, Cognito, et bien d’autres. Si vous voulez obtenir la liste complète d'une soixantaine de services, consultez nos section de ressources. Bonne découverte de l'offre gratuite.
</div>

## Offre gratuite d’AWS

L’[offre gratuite d’AWS](https://aws.amazon.com/free/) vous permet de commencer à utiliser certains services sans vous soucier des coûts pour la période spécifiée. 

Trois types d’offres sont disponibles : 

- Toujours gratuit
- 12 mois gratuits
- Essais

Pour chaque offre gratuite, assurez-vous de consulter les détails spécifiques concernant les types de ressources exactement inclus. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Toujours gratuit" value="true">

Ces offres n’expirent pas et sont disponibles pour tous les clients AWS.

Par exemple, AWS Lambda assure 1 million de requêtes gratuites et jusqu’à 3,2 millions de secondes de temps de calcul par mois. Amazon DynamoDB offre 25 Go de stockage gratuit par mois.
</div>

<div class="collapseContent" display="12 mois gratuit" value="true">

Ces offres sont gratuites pendant 12 mois à compter de la date à laquelle vous vous êtes inscrit à AWS.

Les exemples incluent des quantités spécifiques de stockage standard Amazon S3, des seuils pour les heures de temps de calcul Amazon EC2 par mois et des quantités de transfert de données Amazon CloudFront.
</div>

<div class="collapseContent" display="Essais" value="true">

Les offres d’essai gratuit à court terme débutent à la date d’activation d’un service particulier. La durée de chaque essai peut varier en fonction du nombre de jours ou du volume d’utilisation du service.

Par exemple, Amazon Inspector propose un essai gratuit de 90 jours. Amazon Lightsail (un service qui vous permet d’exécuter des serveurs privés virtuels) offre 750 heures d’utilisation gratuites sur une période de 30 jours.
</div>

## Contrôle des connaissances

**L’offre gratuite d’AWS inclut des offres qui sont disponibles pour les nouveaux clients AWS pendant une certaine période après leur date d’inscription à AWS. Quelle est la durée de cette période ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">3 mois</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">6 mois</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">9 mois</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">12 mois</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **12 mois**.

L’offre gratuite AWS se compose de trois types d’offres qui permettent aux clients d’utiliser les services AWS sans frais : Toujours gratuit, 12 mois gratuits et Essais.

Pendant 12 mois, à compter de votre première inscription à un compte AWS, vous pouvez profiter des offres de la catégorie 12 mois gratuits. Les exemples d’offres de cette catégorie incluent des quantités spécifiques de stockage standard Amazon S3, des seuils pour les heures de temps de calcul Amazon EC2 par mois et des quantités de transfert de données Amazon CloudFront.

En savoir plus :

- [Offre gratuite d’AWS](https://aws.amazon.com/free)
</div>

# Concepts de tarification d’AWS

## Fonctionnement de la tarification AWS

AWS offre une gamme de services de cloud computing avec une tarification à l’utilisation. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Paiement à l'utilisation" value="true">

Pour chaque service, vous payez la quantité de ressources que vous utilisez réellement, sans contrats à long terme ou l’octroi de licences complexes.
</div>

<div class="collapseContent" display="Réduction en cas de réservation" value="true">

Certains services offrent des options de réservation qui assurent une remise importante par rapport à la tarification des instances à la demande.

Par exemple, supposons que votre entreprise utilise des instances Amazon EC2 pour une charge de travail qui doit s’exécuter en continu. Vous pouvez choisir d’exécuter cette charge de travail sur Amazon EC2 Instance Savings Plans, car ce programme vous permet d’économiser jusqu’à 72 % sur une capacité équivalente d’instances à la demande.
</div>

<div class="collapseContent" display="Des remises quantitatives en cas d'utilisation impoirtante" value="true">

Certains services offrent une tarification progressive, de sorte que le coût unitaire diminue de manière incrémentielle avec une utilisation accrue.

Par exemple, plus vous utilisez d’espace de stockage Amazon S3, moins vous payez par Go.
</div>

## Calculatrice de prix AWS

La **[Calculatrice de prix AWS](https://calculator.aws/#/)** vous permet d’explorer les services AWS et de créer une estimation du coût de vos cas d’utilisation sur AWS. Vous pouvez organiser vos estimations AWS par groupes que vous définissez. Un groupe peut refléter l’organisation de votre entreprise, par exemple en fournissant des estimations par centre de coûts.

Lorsque vous avez créé une estimation, vous pouvez l’enregistrer et générer un lien pour la partager avec d’autres personnes.

![aws_calculatrice_01.png](/assets/img/clouds/tempo/aws_calculatrice_01.png)

Supposons que votre entreprise soit intéressée par Amazon EC2. Cependant, vous n’avez pas encore déterminé la région AWS ou le type d’instance qui serait le plus rentable pour votre cas d’utilisation. Dans la Calculatrice de prix AWS, vous pouvez saisir des détails tels que le type de système d’exploitation dont vous avez besoin, la mémoire requise et les exigences d’entrée/de sortie (I/O). En utilisant la Calculatrice de prix AWS, vous pouvez consulter une comparaison estimée des différents types d’instances EC2 dans les régions AWS.

![aws_calculatrice.jpg](/assets/img/clouds/tempo/aws_calculatrice.jpg)

## Exemples de tarification AWS

Cette section présente quelques exemples de [tarification des services AWS](https://aws.amazon.com/lambda/pricing/). 

### AWS Lambda

Pour en savoir plus sur la **[tarification AWS Lambda](https://aws.amazon.com/lambda/pricing/)**, sélectionnez chaque onglet.

#### Tabs {.tabset}

##### Tarification AWS Labmda

Pour AWS Lambda, vous payez selon le nombre de requêtes pour vos fonctions et selon le temps nécessaire à leur exécution.

AWS Lambda garantit 1 million de requêtes gratuites et jusqu’à 3,2 millions de secondes de temps de calcul par mois.

Vous pouvez faire des économies sur AWS Lambda en vous inscrivant à un Compute Savings Plan. Un Compute Savings Plan offre des coûts de calcul moindres en échange d’un engagement à utiliser un volume constant sur une période d’un an ou de trois ans. C’est un exemple de **prix minoré en cas de réservation**.

##### Exemple de tarification

Si vous avez utilisé AWS Lambda dans plusieurs régions AWS, vous pouvez consulter les frais détaillés par région sur votre facture. 

Dans cet exemple, l’ensemble de l’utilisation d’AWS Lambda a eu lieu dans la région Virginie du Nord. La facture répertorie les frais distincts pour le nombre de requêtes de fonctions et leur durée. 

Le nombre de requêtes et leur durée totale dans cet exemple sont tous deux inférieurs aux seuils de l’offre gratuite d’AWS. Ainsi, le propriétaire du compte ne devra pas payer l’utilisation d’AWS Lambda pour ce mois-ci.


![aws_calculatrice_02.png](/assets/img/clouds/tempo/aws_calculatrice_02.png)

## Amazon EC2

Pour en savoir plus sur la **[tarification Amazon EC2](https://aws.amazon.com/ec2/pricing/)**, sélectionnez chaque onglet.

#### Tabs {.tabset}

##### Tarification Amazon EC2

Avec Amazon EC2, vous payez uniquement le temps de calcul que vous utilisez pendant l’exécution de vos instances.

Pour certaines charges de travail, vous pouvez considérablement réduire les coûts Amazon EC2 en utilisant les instances Spot. Par exemple, supposons que vous exécutiez une tâche de traitement par lots capable de résister aux interruptions. L’utilisation d’une instance Spot vous permettrait de réaliser jusqu’à 90 % d’économies de coûts tout en respectant les exigences de disponibilité de votre charge de travail.

Vous pouvez réaliser des économies supplémentaires sur Amazon EC2 en envisageant d’utiliser les Savings Plans et les instances réservées.


##### Exemple de tarification

Les frais de service dans cet exemple incluent les détails pour les éléments suivants :

- Chaque type d’instance Amazon EC2 utilisé
- La quantité d’espace de stockage Amazon EBS allouée
- La durée d’utilisation d’Elastic Load Balancing

Dans cet exemple, tous les volumes d’utilisation ne sont pas inférieurs aux seuils de l’offre gratuite d’AWS. Dès lors, le propriétaire du compte devra payer son utilisation d’Amazon EC2 pour ce mois-ci.

![aws_calculatrice_03.png](/assets/img/clouds/tempo/aws_calculatrice_03.png)

## Amazon S3

Pour en savoir plus sur la **[tarification Amazon S3](https://aws.amazon.com/s3/pricing/)**, sélectionnez chaque onglet.

#### Tabs {.tabset}

##### Tarification Amazon S3

Pour la tarification Amazon S3, tenez compte des composants de coûts suivants :

- **Stockage** : vous payez uniquement le stockage que vous utilisez. Vous êtes facturé selon le tarif de stockage d’objets dans vos compartiments Amazon S3 en fonction de la taille de vos objets, de leur classe de stockage et de la durée de stockage de chaque objet au cours du mois.
- **Demandes et extractions de données** : vous payez les demandes formulées pour vos objets et compartiments Amazon S3. Par exemple, supposons que vous stockiez des fichiers photo dans des compartiments Amazon S3 et que vous les hébergiez sur un site web. Chaque fois qu’un visiteur effectue une requête pour le site web qui contient ces fichiers photo, cela est pris en compte dans les requêtes que vous devez payer.
- **Transfert de données** : le transfert de données entre différents compartiments Amazon S3 ou depuis Amazon S3 vers d’autres services au sein de la même région AWS n’engendre aucun coût. Toutefois, vous payez les données que vous transférez vers et hors d’Amazon S3, à quelques exceptions près. Le transfert de données vers Amazon S3 à partir d’Internet ou vers Amazon CloudFront n’engendre aucun coût. En outre, le transfert de données vers une instance Amazon EC2 dans la même région AWS que le compartiment Amazon S3 n’engendre aucun coût.
- **Gestion et réplication** : vous payez les fonctions de gestion de stockage que vous avez activées sur les compartiments Amazon S3 de votre compte. Ces fonctionnalités incluent l’inventaire Amazon S3, l’analytique et l’identification d’objets.

##### Exemple de tarification

Le compte AWS de cet exemple a utilisé Amazon S3 dans deux régions : Virginie du Nord et Ohio. Pour chaque région, les frais détaillés sont basés sur les facteurs suivants :

- Le nombre de demandes d’ajout ou de copie d’objets dans un compartiment
- Le nombre de demandes de récupération d’objets depuis un compartiment
- La quantité d’espace de stockage utilisée

L’ensemble de l’utilisation d’Amazon S3 dans cet exemple est inférieure aux limites de l’offre gratuite d’AWS. Ainsi, le propriétaire du compte ne devra pas payer une utilisation d’Amazon S3 au cours de ce mois-ci.

![aws_calculatrice_04.png](/assets/img/clouds/tempo/aws_calculatrice_04.png)

# Tableau de bord de facturation

```
video : xxx.mkv
```
*Figure. Billing dashboard*

<div class="collapseContent" display="Transcription" value="true">
  
> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Salut les aficionados d'AWS. Êtes-vous, comme moi, curieux de découvrir les informations de facturation de votre compte AWS ? Procédons à une démonstration afin que vous sachiez où aller. 
> 
> Comme vous pouvez le voir, je suis déjà connecté. Nous allons chercher « facturation » et cliquer sur l’option qui s’affiche. Et boum, vous obtenez déjà des informations utiles. À droite se trouvent vos dépenses pour le mois actuel avec les services les plus utilisés. Plus bas, à gauche, il s'agit des montants du mois précédent, actuels et prévus. Plus bas sont répertoriés les services de l'offre gratuite par utilisation. Vous voyez même les pourcentages de votre utilisation pour le mois actuel. Pas mal, hein ? 
> 
> En remontant, vous avez accès aux autres outils de facturation comme Cost Explorer, Budgets, entre autres. Vous avez aussi accès à votre facture même. Cliquons sur Factures. Nous pouvons voir les services que nous payons, et si nous les développons, nous pouvons consulter les coûts par région. Pour les services mondiaux, c'est aussi indiqué. Par exemple, si je clique sur Route 53, vous voyez qu’il s’agit d’un service mondial dont le détail est le suivant. Voilà, vous disposez d'un moyen simple de vérifier ce que vous dépensez pour chaque service dans AWS. Merci de nous avoir rejoints.
</div>

Utilisez le **[tableau de bord AWS Billing & Cost Management](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/billing-what-is.html)** pour payer votre facture AWS, contrôler votre utilisation, ainsi qu’analyser et contrôler vos coûts.

Comparez votre solde mensuel actuel avec celui du mois précédent et consultez une prévision du mois suivant en fonction de l’utilisation actuelle.
- Affichez les dépenses mensuelles par service.
- Affichez l’utilisation de l’offre gratuite par service.
- Accédez à Cost Explorer et créez des budgets
- Achetez et gérez des Savings Plans.
- Publiez des [AWS Cost and Usage Reports (rapports de coût et d’utilisation AWS)](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html).


# Facturation consolidée

```
video : xxx.mkv
```
*Figure. Consolidated billing*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Lorsque vous possédez plusieurs cafés de la ville, comme nous l’avons précédemment mentionné chaque café aura ses propres dépenses et ses propres bénéfices. À la fin de la journée, ces cafés appartiennent tous à la même entité. Dès lors, l'argent circule depuis et vers un compte principal, si quelqu’un est appelé pour réparer les machines à café lorsqu’elles sont en panne, la personne en charge des réparations facturera l’organisation et non le café individuel. 
> 
> Appliquons cette idée à AWS. Il est plus que probable qu’une entreprise dispose de plusieurs comptes AWS et comme vous l’avez vu, plusieurs comptes dans AWS peuvent être gérés avec AWS Organizations. Une des fantastiques fonctions qu'offre AWS Organizations s’appelle la facturation consolidée. À la fin de chaque mois, au lieu de devoir payer une facture AWS pour chaque compte, vous pouvez regrouper ces factures en une seule facture pour le propriétaire de l’organisation. Il est donc plus facile de garder une trace de vos factures. Vous n'avez pas 100 factures lorsque vous avez 100 comptes AWS. C’est comme l’exemple de notre café. Si mon réparateur s’est rendu dans 11 cafés, pour réparer les machines à café, il ne donnera qu’une seule facture pour le cycle de facturation. La gestion est plus facile pour l'entreprise. 
> 
> C'est la même idée pour la fonction de facturation consolidée pour AWS Organizations. Vous pouvez toujours consulter votre facture AWS de manière détaillée, afin de connaître les dépenses de chaque compte, mais tout est centralisé pour faciliter la visualisation. Cette fonction offre d'autres avantages. L'un d'eux est que l'utilisation des ressources AWS est regroupée au niveau de l’organisation. AWS offre la tarification selon la quantité. Chaque compte individuel profitera d’une petite quantité d’utilisation, mais vous pouvez bénéficier de la tarification avec remise sur quantité grâce au regroupement entre l’ensemble des comptes de l’organisation. En outre, si vous disposez d'un Savings Plan, ou si vous utilisez des instances réservées pour EC2, vous pouvez les partager entre vos comptes AWS au sein de l’organisation. La cerise sur le gâteau ? La fonction est gratuite et facile à utiliser. Cela simplifie le processus de facturation, vous permet de partager les économies entre les comptes et ne vous coûte pas plus cher. Super !
</div>

## Facturation consolidée

Dans un module précédent, vous avez découvert AWS Organizations, un service qui vous permet de gérer plusieurs comptes AWS à partir d’un emplacement centralisé. AWS Organizations propose également l’option de **[facturation consolidée](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html)**. 

La fonction de facturation consolidée d’AWS Organizations vous permet de recevoir une facture unique pour tous les comptes AWS de votre organisation. En utilisant la facturation consolidée, vous pouvez facilement suivre les coûts combinés de tous les comptes liés de votre organisation. Le nombre maximal par défaut de comptes autorisés pour une organisation est de 4, mais vous pouvez contacter AWS Support pour augmenter votre quota, si nécessaire.

Sur votre facture mensuelle, vous pouvez consulter les frais détaillés engagés par chaque compte. Cela garantit une meilleure transparence pour les comptes de votre organisation, tout en conservant l’aspect pratique d’une facture mensuelle unique.

Un autre avantage que confère la facturation consolidée est la possibilité de partager la tarification avec remises en bloc, les Savings Plans et les instances réservées entre les comptes de votre organisation. Par exemple, il est possible qu’un compte ne dispose pas d’une utilisation mensuelle suffisante pour bénéficier de la tarification avec remises. Toutefois, lorsque plusieurs comptes sont combinés, leur utilisation groupée peut entraîner un avantage applicable à l’ensemble des comptes de l’organisation.

### Exemple : facturation consolidée

Afin de consulter un exemple de facturation consolidée, sélectionnez COMMENCER. 

#### tabs {.tabset}
##### Etape 1

Diagramme de trois comptes AWS gérés par le biais d’une seule organisation

![aws_facturation_01.png](/assets/img/clouds/tempo/aws_facturation_01.png)

Supposons que vous supervisiez la facturation AWS de votre entreprise. 

Votre entreprise a trois comptes AWS, qui sont utilisés pour des services distincts. Au lieu de payer la facture mensuelle de chaque site séparément, vous décidez de créer une organisation et d’y ajouter les trois comptes. 

Vous gérez l’organisation par le biais du compte principal.

##### Etape 2

Exemple de frais de plusieurs comptes dans une facture consolidée unique

![aws_facturation_02.png](/assets/img/clouds/tempo/aws_facturation_02.png)

Chaque mois, AWS débite votre compte souscripteur principal pour tous les comptes liés dans une facture consolidée. Avec le compte principal, vous pouvez également obtenir un rapport détaillé des coûts pour chaque compte lié. 

La facture consolidée mensuelle comprend également les coûts d’utilisation engagés par le compte principal. Il ne comprend pas de frais supplémentaires liés au fait de disposer d’un compte principal. 

La facture consolidée indique les coûts associés aux actions du compte principal (telles que le stockage de fichiers dans Amazon S3 ou l’exécution d’instances Amazon EC2).

##### Etape 3

Exemple de trois comptes AWS distincts avec des volumes distincts d’utilisation d’Amazon S3

![aws_facturation_03.png](/assets/img/clouds/tempo/aws_facturation_03.png)

La facturation consolidée vous permet également de partager les remises de tarification sur volume entre les comptes. 

Certains services AWS, tels qu’Amazon S3, offrent des remises de prix sur volume qui garantissent des prix moindres au fur et à mesure que vous utilisez le service. Dans Amazon S3, si les clients transfèrent 10 To de données au cours d’un mois, ils paieront un prix de transfert par Go moindre pour les 40 prochains To de données transférées. 

Dans cet exemple, trois comptes AWS distincts ont transféré différentes quantités de données dans Amazon S3 au cours du mois en cours : 

- Le compte 1 a transféré 2 To de données.
- Le compte 2 a transféré 5 To de données.
- Le compte 3 a transféré 7 To de données.

Étant donné qu’aucun compte n’a dépassé le seuil de 10 To, aucun d’entre eux n’est éligible au prix de transfert par Go moindre pour les 40 prochains To de données transférées.

##### Combinaison

Exemple de combinaison de l’utilisation d’Amazon S3 pour trois comptes liés dans une organisation

![aws_facturation_04.png](/assets/img/clouds/tempo/aws_facturation_04.png)

Supposons maintenant que ces trois comptes distincts soient regroupés en tant que comptes liés au sein d’une organisation AWS unique et utilisent la facturation consolidée.

Lorsque l’utilisation d’Amazon S3 pour les trois comptes liés est combinée (2 + 5 + 7), cela donne lieu à un volume de transfert de données combiné de 14 To. Ce volume combiné dépasse le seuil de 10 To. 

Grâce à la facturation consolidée, AWS combine l’utilisation de tous les comptes pour déterminer les niveaux de tarification en fonction du volume à appliquer, ce qui permet de réduire vos coûts globaux autant que possible. AWS alloue ensuite à chaque compte lié une partie de la remise sur volume globale en fonction de l’utilisation du compte. 

Dans cet exemple, le compte 3 recevrait une plus grande partie de la remise sur volume globale, car il a transféré plus de données (7 To) que le compte 1 (2 To) et le compte 2 (5 To).


# AWS Budgets

```
video : xxx.mkv
```
*Figure. AWS Budgets*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> À mesure de vos déploiements AWS, vous voulez probablement vous assurer de ne pas dépenser un montant non prévu au budget. Comme pour la plupart des entreprises, vous souhaitez suivre les coûts et vous assurer de ne pas dépasser vos limites. Je vous présente AWS Budgets. 
> 
> Pensez à un budget pour vos dépenses personnelles et vous comprendrez le service AWS Budgets. Il vous permet de définir des budgets personnalisés pour une variété de scénarios comme le coût et l’utilisation. Vous recevez une alerte lorsque vos coûts ou votre utilisation dépassent ou vont dépasser le montant prévu dans votre budget. 
> 
> Imaginons un budget de mille dollars et je souhaite être averti lorsque j’atteins 80 % de ce montant. Avec Budgets, vous pouvez le faire. Vous pouvez donc être averti si vous allez dépasser le montant prévu dans votre budget destiné aux ressources. 
> 
> Je vais illustrer l'idée avec une démonstration. Pour commencer, accédez à la section de facturation de votre console AWS, cliquez sur Budgets, puis Create Budget (Créer un budget). Choisissez un type de budget. Je choisis ici les coûts. Attribuez un nom à votre budget. Saisissez un montant. Insérons mille dollars, comme mon exemple précédent, puis cliquez sur Configure Alert (Configurer l’alerte). Ensuite, nous définissons un seuil d'alerte. Insérons 80 %, ce qui signifie que nous recevrons une alerte si nous avons atteint 80 % de notre montant prévu au budget. Ajoutons maintenant une adresse e-mail. N'oubliez pas de la flouter, les gars. J'ai déjà assez de courriers indésirables. Puis, cliquez sur Confirm Budget (Confirmer le budget) et Create (Créer). 
> 
> Voilà, notre premier budget est prêt. On ne peut pas faire plus simple. On peut dire qu'on a utilisé notre temps à bon escient dans cette démonstration.
</div>

## AWS Budgets

Dans **[AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets)**, vous pouvez créer des budgets pour planifier l’utilisation de vos services, les coûts des services et les réservations d’instances.

![aws_budgets_01.png](/assets/img/clouds/tempo/aws_budgets_01.png)

Les informations dans les AWS Budgets sont mises à jour trois fois par jour. Cela vous permet de déterminer avec précision votre utilisation par rapport aux montants prévus au budget ou aux limites de l’offre gratuite d’AWS.

Dans AWS Budgets, vous pouvez également définir des alertes personnalisées lorsque votre utilisation dépasse (ou va dépasser) le montant prévu au budget.

## Exemple : AWS Budgets

Supposons que vous ayez défini un budget pour Amazon EC2. Vous souhaitez vous assurer que l’utilisation d’Amazon EC2 par votre entreprise ne dépasse pas 200 USD pour le mois. 

Dans AWS Budgets, vous pouvez définir un budget personnalisé pour vous avertir lorsque votre utilisation a atteint la moitié de ce montant (100 USD). Ce paramètre vous permet de recevoir une alerte et de gérer votre utilisation ultérieure d’Amazon EC2.

Pour en savoir plus, sélectionnez chaque marqueur.

![aws_budgets_02.png](/assets/img/clouds/tempo/aws_budgets_02.png)

![aws_budgets_03.png](/assets/img/clouds/tempo/aws_budgets_03.png)

![aws_budgets_04.png](/assets/img/clouds/tempo/aws_budgets_04.png)


# AWS Cost Explorer

```
video : xxx.mkv
```
*Figure. AWS Cost Explorer*


<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Comme nous l'avons précédemment dit, AWS offre un modèle de coût variable, et vous ne payez que ce que vous utilisez. Vous n'avez pas un montant facturé fixe à la fin de chaque mois. Au lieu de cela, cela fluctue en fonction des ressources que vous utilisez et votre manière de les utiliser. Compte tenu de ce modèle de coût, il est vraiment important que vous puissiez explorer le contenu de votre facture et observer la façon dont vous dépensez. 
> 
> AWS dispose d'un service appelé AWS Cost Explorer qui est un service basé sur une console qui vous permet de voir et d’analyser la façon dont vous dépensez sur AWS. Il vous indique les services pour lesquels vous dépensez le plus d’argent et fournit 12 mois de données historiques, pour contrôler vos dépenses dans le temps. Ainsi, si vous observez un pic de dépenses pour, par exemple, EC2 d’octobre à décembre, vous pouvez utiliser ces données pour en comprendre les raisons. 
> 
> Jetons un coup d'œil à AWS Cost Explorer dans mon propre compte AWS. Vous voyez que je suis déjà connecté à la console, et je vais saisir Cost Explorer dans la barre de recherche. J'accède alors au tableau de bord de gestion des coûts. et je clique sur Cost Explorer. Cela m'affiche les coûts associés à mon compte des six derniers mois et l’attribut sélectionné est le service. Je peux changer le délai pour afficher huit mois et modifier les données pour qu’elles soient regroupées selon différents attributs. Je termine et je sélectionne l’attribut de région. Il y a également d'autres attributs. 
> 
> Un attribut de regroupement important est la balise. De nombreuses ressources dans AWS peuvent être balisées. Les balises sont essentiellement des paires clé-valeur définies par l'utilisateur. Vous pouvez donc baliser une instance EC2 avec un nom de projet spécifique, ou une base de données avec le même nom de projet, puis accéder à AWS Cost Explorer, filtrer par balise, et voir toutes les dépenses associées à cette balise. Cost Explorer permet également de créer des rapports personnalisés. 
> 
> Je vais maintenant créer un rapport sur les coûts quotidiens du mois de janvier pour cette année. Je vais filtrer par service. Vous pouvez voir que le coût des services est généralement le même chaque jour, sauf que mon utilisation d’EC2 est plus élevée certains jours que d’autres. Je peux ensuite sauvegarder ce rapport et y revenir au besoin. 
> 
> Comme vous le voyez, Cost Explorer vous fournit quelques rapports par défaut puissants, mais vous pouvez aussi créer vos propres rapports personnalisés. Cela vous permet d'identifier les facteurs de coûts et prendre des mesures pour réduire les dépenses. L'optimisation des coûts est une priorité que vous devez suivre de près et vous pouvez utiliser Cost Explorer pour vous orienter vers la bonne direction.
</div>

## AWS Cost Explorer

**[AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)** est un outil qui vous permet de visualiser, de comprendre et de gérer vos coûts et votre utilisation AWS au fil du temps.

![aws_cost_explorer.png](/assets/img/clouds/tempo/aws_cost_explorer.png)

AWS Cost Explorer inclut un rapport par défaut sur les coûts et l’utilisation de vos cinq services AWS les plus générateurs de coûts. Vous pouvez appliquer des filtres et des groupes personnalisés afin d’analyser vos données. Par exemple, vous pouvez afficher l’utilisation des ressources d’un point de vue horaire.

## Exemple : AWS Cost Explorer

![aws_cost_explorer_02.png](/assets/img/clouds/tempo/aws_cost_explorer_02.png)

Cet exemple de tableau de bord AWS Cost Explorer affiche les coûts mensuels pour les instances Amazon EC2 sur une période de 6 mois. La barre de chaque mois sépare les coûts des différents types d’instances Amazon EC2 (tels que t2.micro ou m3.large). 

L’analyse de vos coûts AWS au fil du temps vous permet de prendre des décisions éclairées concernant les futurs coûts et la planification de vos budgets.

# Programmes AWS Support

```
video : xxx.mkv
```
*Figure. AWS Support plans*


<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Un des grands avantages d'AWS est que, quelle que soit la taille de votre entreprise, vous n’êtes jamais seul. Des start-ups aux grandes entreprises, du secteur privé au secteur public, toutes les entreprises peuvent bénéficier d’options de support conçues pour répondre à leurs besoins spécifiques. 
> 
> Pour commencer, chaque client bénéficie automatiquement du support basic AWS, sans aucun frais. Tout client peut accéder aux fonctions de support telles que l’accès au service clientèle 24 heures sur 24 et 7 jours sur 7, la documentation, les livres blancs, les forums d’assistance, AWS Trusted Advisor, et AWS Personal Health Dashboard. Il s'agit d'une vue personnalisée de la santé des services AWS et des alertes générées lorsque vos ressources pourraient être affectées. Ces fonctions sont gratuites pour tout le monde, mais lorsque vous commencez à déplacer des charges de travail stratégiques dans AWS, nous proposons des niveaux de support plus élevées afin de correspondre à vos niveaux de besoins. 
> 
> Le niveau suivant est celui des développeurs, qui inclut tous les éléments du support basic. Vous pouvez envoyer un e-mail directement au service clientèle, qui répondra dans les 24 heures à toutes les questions que vous pouvez avoir. Et dans les 12 heures, en cas de défaillances de vos systèmes. C'est un excellent niveau pour les entreprises qui essayent AWS ou configurent des tests ou des preuves de concept. 
> 
> Lorsque vous commencez à utiliser des charges de travail de production, nous constatons que les clients sont plus prospères en allant vers le niveau professionnel. Outre l'ensemble des avantages des niveaux précédents, Trusted Advisor offre la suite complète de vérifications pour votre compte. Vous pouvez directement contacter notre équipe de support par téléphone, dont la SLA de réponse est de quatre heures. En cas de défaillance de votre système de production, et un SLA d’une heure en cas de panne de vos systèmes de production. En outre, dans le cadre du niveau Professionnel, vous pouvez accéder à l’infrastructure Event Management où, moyennant des frais supplémentaires, nous pouvons vous aider à planifier d’importants événements comme de nouveaux lancements ou des campagnes publicitaires mondiales. 
> 
> Enfin, pour les entreprises exécutant des charges de travail stratégiques, nous recommandons le support niveau entreprise. Outre les avantages des niveaux précédents, il inclut un SLA de 15 minutes pour les charges de travail professionnelles critiques, un Technical Account Manager (TAM). Ce dernier coordonne l'accès aux programmes et d’autres experts AWS le cas échéant. 
> 
> Parlons un peu plus spécifiquement du travail d'un TAM. Les TAM font partie de l'équipe de support concierge inclut dans le niveau entreprise. Ils sont non seulement spécialisés dans la surveillance proactive de votre environnement et contribuent aux optimisations mais ils assurent également l’Infrastructure Event Management, les évaluations Well-Architected, et les évaluations des opérations. 
> 
> Vous vous demandez ce qu'est une évaluation Well-Architected. Pour faire simple, les TAM travaillent avec les clients pour vérifier les architectures en utilisant le framework Well-Architected. Les vérifications des architectures reposent sur les cinq piliers du framework Well-Architected : l’excellence opérationnelle, la sécurité, la fiabilité, l’efficacité des performances, et l’optimisation des coûts. Comme vous pouvez le voir, un TAM ne fait pas que gérer les tickets de problèmes. AWS Support s'intéresse au client de manière globale, pas uniquement en cas de problèmes, mais comment pouvons-nous aider le client à réussir ? 
> 
> C'est la mission d'AWS Support. Pour en savoir plus sur AWS Support, notamment la tarification, visitez https://aws.amazon.com/fr/premiumsupport/.
</div>

## AWS Support

AWS propose quatre **[formules de support](https://aws.amazon.com/premiumsupport/plans/)** différentes afin de vous aider à résoudre les problèmes, à réduire les coûts et à utiliser efficacement les services AWS. 

Vous pouvez choisir parmi les formules de support suivantes afin de répondre aux besoins de votre entreprise : 

- Basic
- Développeur
- Business
- Entreprise

## Support Basic

**Le support basic** est gratuit pour tous les clients AWS. Il comprend l’accès aux livres blancs, à la documentation et aux communautés de support. Avec le Support basic, vous pouvez également contacter AWS pour des questions de facturation et accroître les limites de services.

Avec le Support basic, vous avez accès à une sélection limitée de vérifications AWS Trusted Advisor. En outre, vous pouvez utiliser l’**AWS Personal Health Dashboard**, un outil qui fournit des alertes et des conseils de correction liés aux événements AWS susceptibles d’affecter vos opérations. 

Si votre entreprise a besoin d’un support plus important que le support Basic, vous pouvez envisager d’acheter un support Développeur, Business ou Entreprise.

## Support Développeur, Business et Entreprise

Les formules de support Développeur, Business et Entreprise incluent tous les avantages du support Basic, ainsi que la possibilité d’ouvrir un nombre illimité de cas de support technique. Ces trois formules de support présentent une tarification au mois et ne requièrent aucun contrat à long terme.

Les informations contenues dans ce cours ne mettent en évidence qu’une sélection de détails pour chaque formule de support. Une présentation complète des éléments inclus dans chaque formule de support, notamment la tarification de chaque formule, est disponible sur le site [AWS Support](https://aws.amazon.com/premiumsupport/plans/).

En règle générale, pour la tarification, la formule Développeur présente le coût le plus bas, la formule Entreprise présente le coût le plus élevé, et la formule Business se situe entre les deux. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Support Développeur" value="true">

Les clients de la formule de **support Développeur** ont accès à des fonctions telles que :

- Conseils concernant les bonnes pratiques
- Outils de diagnostic client
- Support d’architecture par blocs de construction, qui comprend des conseils sur la façon d’utiliser ensemble les offres, les fonctions et les services AWS.

Par exemple, supposons que votre entreprise soit en train d’explorer les services AWS. Vous avez entendu parler de divers services AWS. Toutefois, vous ne savez pas comment les utiliser ensemble afin de créer des applications capables de répondre aux besoins de votre entreprise. Dans ce scénario, le support d’architecture par blocs de construction, inclus dans la formule de support Développeur, pourrait vous aider à identifier les possibilités de combinaison des services et des fonctions spécifiques.
</div>

<div class="collapseContent" display="Support Business" value="true">

Les clients disposant d’une formule de **support Professionnel** ont accès à des fonctions supplémentaires, notamment : 

- Conseils selon les cas d’utilisation pour identifier les offres, les fonctions et les services AWS qui répondent le mieux à vos besoins spécifiques
- Toutes les vérifications AWS Trusted Advisor
- Support limité pour les logiciels tiers, tels que les systèmes d’exploitation courants et les composants de piles d’applications

Supposons que votre entreprise dispose de la formule de support Business et souhaite installer un système d’exploitation tiers classique sur vos instances Amazon EC2. Vous pouvez contacter AWS Support pour obtenir de l’aide afin d’installer, de configurer et de résoudre les problèmes du système d’exploitation. Pour des rubriques plus avancées telles que l’optimisation des performances, l’utilisation de scripts personnalisés ou la résolution de problèmes de sécurité, vous devrez peut-être contacter directement le fournisseur de logiciel tiers.
</div>

<div class="collapseContent" display="Support Entreprise" value="true">

Outre l’ensemble des fonctions incluses dans les formules de support Basic, Développeur et Business, les clients disposant d’une formule de **support Entreprise** ont accès à des fonctions telles que :

- Conseils concernant l’architecture des applications : une relation consultative permettant de prendre en charge les cas d’utilisation et les applications spécifiques de votre entreprise
- Gestion des événements d’infrastructure : un engagement à court terme auprès d’AWS Support qui aide votre entreprise à mieux comprendre vos cas d’utilisation. Cela fournit également à votre entreprise des conseils en matière d’architecture et de mise à l’échelle.
- Un Technical Account Manager
</div>

## Technical Account Manager (TAM)

La formule de support Entreprise inclut l’accès à un **Technical Account Manager (TAM)**.

Si votre entreprise dispose d’une formule de support Entreprise, le TAM est votre principal point de contact chez AWS. Il fournit des conseils et des évaluations d’architectures et assure une communication continue avec votre entreprise pour la planification, le déploiement et l’optimisation de vos applications. 

Votre TAM apporte son expertise sur l’ensemble des services AWS. Il vous aide à concevoir des solutions qui utilisent conjointement et efficacement plusieurs services grâce à une approche intégrée.

Par exemple, supposons que vous soyez intéressé par le développement d’une application qui utilise conjointement plusieurs services AWS. Votre TAM pourrait fournir des informations sur la meilleure façon d’utiliser conjointement ces services. En outre, il tient compte des besoins spécifiques que votre entreprise espère satisfaire par le biais de la nouvelle application.

## Contrôle des connaissances

**Quelle formule de support inclut l’ensemble des vérifications AWS Trusted Advisor au coût le plus bas ?**

<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">Basic</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Développeur</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Business</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">Entreprise</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Business**.

Seuls les formules de support Business et Enterprise incluent l’ensemble des vérifications AWS Trusted Advisor. Parmi ces deux formules de support, la formule de support Business a un coût moindre.

En savoir plus :

- [Comparer les formules AWS Support](https://aws.amazon.com/premiumsupport/plans/)
</div>


# AWS Marketplace

```
video : xxx.mkv
```
*Figure. AWS Marketplace*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> AWS offre également un support considérable aux entreprises via AWS Marketplace. AWS Marketplace est un catalogue numérique organisé qui vous permet de trouver, déployer et gérer les logiciels tiers s’exécutant dans votre architecture AWS tout en proposant une série d’options de paiement vous permettant d’accélérer l’innovation tout en déployant rapidement et en toute sécurité une large gamme de solutions en réduisant également votre coût total de possession.
> 
> Pour que vous compreniez mieux, je vais expliquer ce qu’apporte AWS Marketplace et comment il peut vous aider à améliorer votre vitesse et votre agilité avec AWS. AWS Marketplace aide les clients, car, au lieu de devoir créer, installer et maintenir les infrastructures de base nécessaires à l’exécution de ces applications tierces, les clients ont l’option du déploiement en un clic pour acquérir et utiliser des produits provenant de milliers de vendeurs de logiciels juste au moment où ils en ont besoin. 
> 
> Comment vous le savez, certains fournisseurs tiers ont des contrats annuels pour les centres de données sur site. Vous vous demandez peut-être si c'est la même chose avec AWS. Chaque fournisseur peut choisir les options disponibles, mais presque tous les fournisseurs sur Marketplace vous autoriseront à utiliser n’importe quelle licence annuelle en votre possession et à l’attribuer au déploiement d’AWS. Plus important encore, la plupart des fournisseurs sur Marketplace proposent également des options de paiement à l’utilisation à la demande. Ces plans de tarification flexibles vous offrent plus d'options pour payer le logiciel en fonction de votre utilisation sans gaspiller de licences inutilisées qui pèsent sur vos bilans. 
> 
> Nombre d'entre eux offrent même des essais gratuits ou des plans de démarrage rapide pour vous permettre d’essayer et d’en savoir plus sur leurs offres. Car ils savent, et AWS le sait aussi, que la meilleure façon de savoir si quelque chose fonctionne pour vous est de simplement l’exécuter. Même pour les grandes entreprises qui négocient des tarifs personnalisés et des fournisseurs à Marketplace offre un certain nombre de fonctionnalités centrées sur l’entreprise comme la tarification et les conditions personnalisées pour gérer des contrats de licence personnalisés. Il offre aussi un marketplace privé, où vous pouvez créer un catalogue personnalisé de solutions logicielles préapprouvées qui répondent aux normes juridiques ou de sécurité spécifiques. Il s'intègre à vos systèmes d'approvisionnement et fournit une gamme d’outils de gestion des coûts juste pour ne citer que quelques avantages. Pour en savoir plus sur AWS Marketplace, visitez aws.amazon.com/marketplace.
</div>


## AWS Marketplace

**[AWS Marketplace](https://aws.amazon.com/marketplace)** est un catalogue numérique qui comprend des milliers de listes de logiciels provenant de fournisseurs de logiciels indépendants. Vous pouvez utiliser AWS Marketplace pour trouver, tester et acheter des logiciels qui s’exécutent sur AWS. 

Pour chaque liste sur AWS Marketplace, vous pouvez accéder à des informations détaillées sur les options de tarification, le support disponible et les avis d’autres clients AWS.

Vous pouvez également explorer les solutions logicielles par secteur d’activité et par cas d’utilisation. Supposons par exemple que votre entreprise appartienne au secteur des soins de santé. Sur AWS Marketplace, vous pouvez consulter les cas d’utilisation que le logiciel permet de résoudre, tels que l’implémentation de solutions pour protéger les dossiers des patients ou l’utilisation de modèles de machine learning pour analyser les antécédents médicaux d’un patient et prévoir les possibles risques pour la santé.

### Catégories AWS Marketplace

![aws_marketplace_01.png](/assets/img/clouds/tempo/aws_marketplace_01.png)

Icônes représentant chaque catégorie AWS Marketplace : applications métier, données et analytique, DevOps, logiciels d’infrastructure, Internet des objets (IoT), machine learning, migration et sécurité
AWS Marketplace propose des produits dans plusieurs catégories, telles que les produits d’infrastructure, les applications métier, les produits de données et les DevOps.

Dans chaque catégorie, vous pouvez affiner votre recherche en parcourant les listes de produits dans les sous-catégories. Par exemple, les sous-catégories de la catégorie DevOps incluent des domaines tels que le développement d’applications, la surveillance et les tests.

# Module 8 : résumé

Dans le module 8, vous avez découvert les concepts suivants :

- Trois types d’offres inclus dans l’offre gratuite d’AWS : 12 mois gratuits, Toujours gratuit et Essais
- Avantages de la facturation consolidée dans AWS Organizations
- Outils de planification, d’estimation et de révision des coûts AWS
- Différences entre les quatre formules AWS Support : Basic, Développeur, Business et Entreprise
- Comment découvrir des logiciels sur AWS Marketplace

```
video : xxx.mkv
```
*Figure. Module summary - Pricing and support*


<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Vous avez découvert beaucoup de choses sur la tarification et le support, probablement beaucoup plus que vous ne le pensiez. Nous avons parlé du paiement à l'utilisation pour les ressources du cloud AWS. Nous avons expliqué la différence entre les coûts sur site et sur le cloud et nous avons même montré comment commencer en douceur avec l’offre gratuite pour la plupart des services AWS. 
> 
> Nous avons parlé de l'utilisation d'AWS Organizations et de l’offre de facturation consolidée de plusieurs comptes AWS. Nous avons parlé de AWS Budgets et de Cost Explorer. Nous avons même expliqué la facturation d’AWS Console pour que vous vous familiarisiez avec elle avant d’aborder les différents modèles de support proposés par AWS. Ils sont très pratiques si vous cherchez de l'aide dans votre transition vers le Cloud. 
> 
> En parlant d’aide, nous vous avons présenté le vaste écosystème de partenaires AWS pour vous aider à trouver des partenaires pour collaborer sur vos charges de travail. Enfin, nous avons parlé d'AWS Marketplace, qui est intéressant si vous cherchez des services accessibles en un seul clic.
</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts explorés dans le module 8, consultez ces ressources.

- [Tarification AWS](https://aws.amazon.com/pricing)
- [Offre gratuite d’AWS](https://aws.amazon.com/free)
- [Gestion des coûts AWS](https://aws.amazon.com/aws-cost-management/)
- [Livre blanc : fonctionnement de la tarification AWS](https://d1.awsstatic.com/whitepapers/aws_pricing_overview.pdf)
- [Livre blanc : introduction à l’optimisation des coûts WS](https://d1.awsstatic.com/whitepapers/introduction-to-aws-cloud-economics-final.pdf)
- [AWS Support](https://aws.amazon.com/premiumsupport)
- [Centre de connaissances AWS](https://aws.amazon.com/premiumsupport/knowledge-center/)

# Module 8 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelle action pouvez-vous effectuer avec la facturation consolidée ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Vérifier les coûts que votre utilisation prévisionnelle d’AWS entraînera d’ici la fin du mois</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">Créer une estimation du coût de vos cas d’utilisation sur AWS</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">Combiner l’utilisation des comptes pour bénéficier de remises de tarification sur volume</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Visualiser et gérer vos coûts et votre utilisation AWS au fil du temps</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Combiner l’utilisation des comptes pour bénéficier de remises de tarification sur volume**.

Les autres réponses sont incorrectes, car :

- Vérifier les coûts que votre utilisation prévisionnelle d’AWS entraînera d’ici la fin du mois : vous pouvez effectuer cette action dans AWS Budgets.
- Créer une estimation du coût de vos cas d’utilisation sur AWS : vous pouvez effectuer cette action dans la Calculatrice de prix AWS.
- Visualiser et gérer vos coûts et votre utilisation AWS au fil du temps : vous pouvez effectuer cette action dans AWS Cost Explorer.

En savoir plus :

- [Facturation consolidée pour AWS Organizations](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html)
</div>

**Quel outil de tarification est utilisé pour visualiser, comprendre et gérer vos coûts et votre utilisation AWS au fil du temps ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Calculatrice de prix AWS</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">AWS Budgets</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">AWS Cost Explorer</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Offre gratuite d’AWS</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Cost Explorer**.

AWS Cost Explorer inclut un rapport par défaut sur les coûts et l’utilisation de vos cinq services AWS générant le plus de coûts. Vous pouvez appliquer des filtres et des groupes personnalisés afin d’analyser vos données. Par exemple, vous pouvez afficher l’utilisation des ressources d’un point de vue horaire.

Les autres réponses sont incorrectes, car :

- La Calculatrice de prix AWS vous permet de créer une estimation du coût de vos cas d’utilisation sur AWS.
- AWS Budgets vous permet de créer des budgets pour planifier l’utilisation de vos services, les coûts de services et les réservations d’instances. Dans AWS Budgets, vous pouvez également définir des alertes personnalisées lorsque votre utilisation dépasse (ou va dépasser) le montant prévu au budget.
- L’offre gratuite d’AWS est un programme qui se compose de trois types d’offres qui permettent aux clients d’utiliser les services AWS sans engendrer de coûts : Toujours gratuit, 12 mois gratuits et Essais.

En savoir plus :

- [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
</div>

**Quel outil de tarification vous permet de recevoir des alertes lorsque l’utilisation de votre service dépasse un seuil que vous avez défini ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Tableau de bord de facturation dans AWS Management Console</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">AWS Budgets</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Offre gratuite d’AWS</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">AWS Cost Explorer</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Budgets**.

Dans AWS Budgets, vous pouvez définir des alertes personnalisées qui vous avertiront lorsque l’utilisation de votre service dépasse (ou va dépasser) le montant que vous avez budgété.

Votre budget peut être basé sur les coûts ou l’utilisation. Par exemple, vous pouvez définir une alerte qui vous avertira lorsque vos coûts s’élèvent à 100,00 USD dans Amazon EC2 ou que vous atteignez 500 000 requêtes dans AWS Lambda.

Les autres réponses sont incorrectes, car :

- Depuis le tableau de bord de la facturation dans l’AWS Management Console, vous pouvez afficher les détails de votre facture AWS, tels que les coûts de services par région, les dépenses mensuelles, etc. Toutefois, vous ne pouvez pas définir d’alertes à partir du tableau de bord de la facturation.
- L’offre gratuite d’AWS est un programme qui se compose de trois types d’offres qui permettent aux clients d’utiliser les services AWS sans engendrer de coûts : Toujours gratuit, 12 mois gratuits et Essais.
- AWS Cost Explorer est un outil qui vous permet de visualiser, de comprendre et de gérer vos coûts et votre utilisation AWS au fil du temps.

En savoir plus :

- [AWS Budgets](https://aws.amazon.com/aws-cost-management/aws-budgets)
</div>

**Votre entreprise souhaite bénéficier du support d’un responsable technique de compte AWS (TAM). Quelle formule de support devriez-vous choisir ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">Développeur</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Entreprise</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">Basic</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">Business</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Entreprise**.

Le Technical Account Manager (TAM) n’est disponible que pour les clients AWS disposant d’une formule de support Enterprise. Un TAM fournit des conseils et des évaluations d’architectures et assure une communication continue avec votre entreprise pour la planification, le déploiement et l’optimisation de vos applications.

En savoir plus :

- [Comparer les formules AWS Support](https://aws.amazon.com/premiumsupport/plans/)
</div>

**Quel service ou quelle ressource permet de trouver des logiciels tiers qui s’exécutent sur AWS ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">AWS Marketplace</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">Offre gratuite d’AWS</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">AWS Support</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">Tableau de bord de facturation dans AWS Management Console</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Marketplace**.

AWS Marketplace est un catalogue numérique qui comprend des milliers de listes de logiciels provenant de fournisseurs de logiciels indépendants. Vous pouvez utiliser AWS Marketplace pour trouver, tester et acheter des logiciels qui s’exécutent sur AWS.

Les autres réponses sont incorrectes, car :

- L’offre gratuite d’AWS se compose d’offres qui permettent aux clients d’utiliser les services AWS sans frais. Ces offres sont liées aux services AWS, et non aux logiciels tiers qui peuvent être utilisés sur AWS.
- AWS Support est une ressource qui peut répondre à des questions concernant les bonnes pratiques, vous aider à résoudre les problèmes, à identifier les moyens d’optimiser votre utilisation des services AWS, etc.
- Vous pouvez utiliser le tableau de bord de la facturation dans AWS Management Console pour afficher des détails tels que les coûts de services par région, les principaux services utilisés par votre compte et les coûts de facturation prévisionnels. Depuis le tableau de bord de facturation, vous pouvez également accéder à d’autres outils de facturation AWS, tels qu’AWS Cost Explorer, AWS Budgets et AWS Budgets Reports.

En savoir plus :

- [AWS Marketplace](https://aws.amazon.com/marketplace)
</div>

Le module suivant étudie la migration et l’innovation dans AWS Cloud.