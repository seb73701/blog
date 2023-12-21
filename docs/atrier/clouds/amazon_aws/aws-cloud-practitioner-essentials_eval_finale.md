---
title: Evaluation Finale
description: 
published: true
date: 2023-04-21T10:55:17.787Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:55:11.225Z
---

# Evaluation finale

Testez vos connaissances à l’égard de certains concepts clés de ce cours en répondant aux questions de cette évaluation finale.


Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour en savoir plus.

COMMENCER LE QUIZ

Question : **01/30**

**Quel service vous permet de consolider et de gérer plusieurs comptes AWS à partir d’un emplacement centralisé ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">AWS Key Management Service (AWS KMS)</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">AWS Organizations</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">AWS Identity and Access Management (IAM)</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">AWS Artifact</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Organizations**.

Dans AWS Organizations, vous pouvez contrôler de manière centralisée les autorisations pour les comptes de votre organisation à l’aide de stratégies de contrôle des services (SCP). En outre, vous pouvez utiliser la fonction de facturation consolidée d’AWS Organizations pour combiner l’utilisation et recevoir une facture unique pour plusieurs comptes AWS.

Les autres réponses sont incorrectes, car :

- AWS Identity and Access Management (IAM) est un service qui permet de gérer l’accès aux services et aux ressources AWS.  
- AWS Artifact est un service qui vous permet d’accéder aux rapports de conformité et de sécurité AWS et à certains contrats en ligne.
- AWS Key Management Service (AWS KMS) vous permet de créer, de gérer et d’utiliser des clés de chiffrement.

En savoir plus :

- [AWS Organizations](https://aws.amazon.com/organizations)
</div>

Question : **02/30**

**Quelles formules de support incluent l’accès à l’ensemble des vérifications AWS Trusted Advisor ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q2r1" name="question2" value="1">
  <label for="q2r1">Développeur</label><br>
  <input type="checkbox" id="q2r2" name="question2" value="2">
  <label for="q2r2">Entreprise</label><br>
  <input type="checkbox" id="q2r3" name="question2" value="3">
  <label for="q2r3">Offre gratuite d’AWS</label><br>
  <input type="checkbox" id="q2r4" name="question2" value="4">
  <label for="q2r4">Business</label><br>
  <input type="checkbox" id="q2r5" name="question2" value="5">
  <label for="q2r5">Basic</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Entreprise**
- **Business**

Les autres réponses sont incorrectes, car :

- Les formules de support Basic et Développeur offrent un accès à une sélection limitée de vérifications AWS Trusted Advisor.
- L’offre gratuite d’AWS ne constitue pas une formule de support. Il s’agit d’un programme qui se compose de trois types d’offres qui permettent aux clients d’utiliser les services AWS sans frais : Toujours gratuit, 12 mois gratuits et Essais.

En savoir plus :

- [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)
</div>

Question : **03/30**

**Quel service vous permet de consulter les détails des activités des utilisateurs et des appels d’API inhérents à votre environnement AWS ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Amazon CloudWatch</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">Amazon Inspector</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">AWS CloudTrail</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">AWS Trusted Advisor</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS CloudTrail**.

Avec CloudTrail, vous pouvez afficher un historique complet de l’activité des utilisateurs et des appels d’API pour vos applications et ressources. 

Les événements sont généralement mis à jour dans CloudTrail dans les 15 minutes suivant les appels d’API. Vous pouvez filtrer les événements en spécifiant l’heure et la date auxquelles un appel d’API s’est produit, l’utilisateur qui a demandé l’action, le type de ressource impliquée dans l’appel d’API, etc.

Les autres réponses sont incorrectes, car :

- Amazon CloudWatch est un service qui fournit des données que vous pouvez utiliser pour surveiller vos applications, optimiser l’utilisation des ressources et répondre aux changements de performances à l’échelle du système.
- Amazon Inspector est un service qui vérifie les applications pour détecter les failles de sécurité et les manquements aux bonnes pratiques en matière de sécurité.
- AWS Trusted Advisor est un outil en ligne qui inspecte votre environnement AWS et fournit des conseils en temps réel conformément aux bonnes pratiques AWS.

En savoir plus :

- [AWS CloudTrail](https://aws.amazon.com/cloudtrail)
</div>

Question ; **04/30**

**Dans la classe de stockage S3 Intelligent-Tiering, Amazon S3 déplace les objets entre un niveau d’accès fréquent et un niveau d’accès peu fréquent. Quelles classes de stockage sont utilisées pour ces niveaux ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q4r1" name="question4" value="1">
  <label for="q4r1">S3 Glacier</label><br>
  <input type="checkbox" id="q4r2" name="question4" value="2">
  <label for="q4r2">S3 standard – Accès peu fréquent</label><br>
  <input type="checkbox" id="q4r3" name="question4" value="3">
  <label for="q4r3">S3 Standard</label><br>
  <input type="checkbox" id="q4r4" name="question4" value="4">
  <label for="q4r4">S3 unizone – Accès peu fréquent</label><br>
  <input type="checkbox" id="q4r5" name="question4" value="5">
  <label for="q4r5">S3 Glacier Deep Archive</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **S3 standard**
- **S3 standard – Accès peu fréquent**

Dans la classe de stockage S3 Intelligent-Tiering, Amazon S3 contrôle les modèles d’accès des objets. Si vous n’avez pas accédé à un objet pendant 30 jours consécutifs, Amazon S3 le déplace automatiquement vers le niveau d’accès peu fréquent, S3 standard – Accès peu fréquent. Si vous accédez à un objet du niveau d’accès peu fréquent, Amazon S3 le déplace automatiquement vers le niveau d’accès fréquent, S3 Standard.


En savoir plus :

- [Classes de stockage Amazon S3](https://aws.amazon.com/s3/storage-classes/)
</div>

Question : **05/30**

**Vous souhaitez envoyer et recevoir des messages entre les composants d’applications distribuées. Quel service devriez-vous utiliser ?**    

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">AWS Snowball</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Amazon Simple Queue Service (Amazon SQS)</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Amazon Route 53</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Amazon ElastiCache</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Simple Queue Service (Amazon SQS)**.

Amazon SQS est un service de mise en file d’attente de messages. Avec Amazon SQS, vous pouvez envoyer, stocker et recevoir des messages entre les composants logiciels, quelle que soit la taille du volume, sans perdre de messages ni avoir besoin que d’autres services soient disponibles. 

Dans Amazon SQS, une application envoie des messages dans une file d’attente. Un utilisateur ou un service extrait un message de la file d’attente, le traite, puis le supprime de la file d’attente.

Les autres réponses sont incorrectes, car : 

- AWS Snowball est un périphérique qui vous permet de transférer de grandes quantités de données vers et hors d’AWS.
- Amazon ElastiCache est un service qui ajoute des couches de mise en cache au-dessus de vos bases de données afin d’améliorer les temps de lecture des demandes courantes.
- Amazon Route 53 est un service web de DNS. Il offre aux développeurs et aux entreprises un moyen fiable d’acheminer les utilisateurs finaux vers des applications Internet hébergées dans AWS. En outre, vous pouvez transférer des registres DNS pour des noms de domaine existants actuellement gérés par d’autres bureaux d’enregistrement de domaines ou enregistrer de nouveaux noms de domaine directement dans Amazon Route 53.

En savoir plus :

- [Amazon SQS](https://aws.amazon.com/sqs)
</div>

Question : **06/30**

**Quel composant du cloud privé virtuel (VPC) contrôle le trafic entrant et sortant pour les instances Amazon EC2 ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">Liste de contrôle d’accès réseau</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Passerelle Internet</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">Groupe de sécurité</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">Sous-réseau</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Groupe de sécurité**.

Un groupe de sécurité est un pare-feu virtuel qui contrôle le trafic entrant et sortant d’une instance Amazon EC2. 

Par défaut, un groupe de sécurité refuse tout le trafic entrant et autorise tout le trafic sortant. Vous pouvez ajouter des règles personnalisées afin de configurer quel trafic doit être autorisé ou refusé.

Les autres réponses sont incorrectes, car :

- Un sous-réseau est une section d’un VPC dans laquelle vous pouvez regrouper des ressources en fonction des besoins de sécurité ou opérationnels.
- Une liste de contrôle d’accès (ACL) réseau est un pare-feu virtuel qui contrôle le trafic entrant et sortant au niveau du sous-réseau.
- Une passerelle Internet est une connexion entre un VPC et Internet. Elle permet au trafic public à partir d’Internet d’accéder à un VPC.

En savoir plus :

- [Groupes de sécurité pour votre VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
</div>

Question : **07/30**

**Vous souhaitez stocker des données dans un volume attaché à une instance Amazon EC2. Quel service devriez-vous utiliser ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">AWS Lambda</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">Amazon Simple Storage Service (Amazon S3)</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">Amazon ElastiCache</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">Amazon Elastic Block Store (Amazon EBS)</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Amazon Elastic Block Store (Amazon EBS)**.

Amazon EBS fournit des volumes de stockage au niveau bloc que vous pouvez utiliser avec les instances Amazon EC2. Si vous arrêtez ou résiliez une instance Amazon EC2, toutes les données du volume EBS attaché restent disponibles.

Les autres réponses sont incorrectes, car :

- Amazon Simple Storage Service (Amazon S3) est un service qui fournit un stockage au niveau des objets. Amazon S3 stocke les données sous forme d’objets dans des compartiments.
- AWS Lambda est un service qui vous permet d’exécuter du code sans mettre en service ni gérer de serveurs.
- Amazon ElastiCache est un service qui ajoute des couches de mise en cache au-dessus de vos bases de données afin d’améliorer les temps de lecture des requêtes courantes.

En savoir plus :

- [Amazon EBS](https://aws.amazon.com/ebs)
</div>

Question : **08/30**

**Vous exécutez une instance Amazon EC2 et souhaitez stocker des données dans une ressource attachée. Vos données sont temporaires et ne seront pas conservées à long terme. Quelle ressource devriez-vous utiliser ?**

<form>
  <input type="radio" id="q8r1" name="question8" value="1">
  <label for="q8r1">Sous-réseau</label><br>
  <input type="radio" id="q8r2" name="question8" value="2">
  <label for="q8r2">Volume Amazon Elastic Block Store (Amazon EBS)</label><br>
  <input type="radio" id="q8r3" name="question8" value="3">
  <label for="q8r3">Stockage d’instance</label><br>
  <input type="radio" id="q8r4" name="question8" value="4">
  <label for="q8r4">Compartiment Amazon S3</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Stockage d’instances**.

Les stockages d’instances sont idéaux pour les données temporaires qui n’ont pas besoin d’être conservées à long terme.

Lorsqu’une instance Amazon EC2 est arrêtée ou résiliée, toutes les données qui ont été écrites dans le stockage d’instances attaché sont supprimées.

Les autres réponses sont incorrectes, car :

- Les volumes Amazon EBS sont idéaux pour les données qui doivent être conservées. Lorsqu’une instance Amazon EC2 est arrêtée ou résiliée, toutes les données du volume EBS attaché sont toujours disponibles.
- Les compartiments Amazon S3 ne peuvent pas être attachés aux instances Amazon EC2.
- Un sous-réseau est une section d’un cloud privé virtuel (VPC) dans laquelle vous pouvez regrouper des ressources en fonction des besoins de sécurité ou opérationnels.

En savoir plus :

- [Stockage d’instances Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
</div>

Question : **09/30**

**Quel outil permet d’automatiser les actions pour les services et applications AWS à l’aide de scripts ?** 

<form>
  <input type="radio" id="q9r1" name="question9" value="1">
  <label for="q9r1">Interface de ligne de commande AWS</label><br>
  <input type="radio" id="q9r2" name="question9" value="2">
  <label for="q9r2">Amazon Redshift</label><br>
  <input type="radio" id="q9r3" name="question9" value="3">
  <label for="q9r3">Amazon QLDB</label><br>
  <input type="radio" id="q9r4" name="question9" value="4">
  <label for="q9r4">AWS Snowball</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **AWS Command Line Interface**.

AWS Command Line Interface (AWS CLI) vous permet de contrôler plusieurs services AWS directement à partir de la ligne de commande au sein d’un seul outil. Par exemple, vous pouvez utiliser des commandes pour démarrer une instance Amazon EC2, pour connecter une instance Amazon EC2 à un groupe Auto Scaling spécifique, etc. L’AWS CLI est disponible pour les utilisateurs sous Windows, macOS et Linux.

Les autres réponses sont incorrectes, car :

- Amazon Redshift est un service de stockage de données que vous pouvez utiliser aux fins de l’analytique des Big Data. Ce service permet de collecter des données provenant de nombreuses sources et vous aide à comprendre les relations et les tendances entre vos données. 
- Amazon Quantum Ledger Database (Amazon QLDB) est un service de base de données de registre. Vous pouvez utiliser Amazon QLDB pour consulter un historique complet de toutes les modifications apportées aux données de votre application.
- AWS Snowball est un périphérique qui vous permet de transférer de grandes quantités de données vers et hors d’AWS.

En savoir plus :

- [AWS Command Line Interface](https://aws.amazon.com/cli/)
</div>

Question : **10/30**

**Vous souhaitez stocker des données dans une base de données clé-valeur. Quel service devriez-vous utiliser ?**

<form>
  <input type="radio" id="q10r1" name="question10" value="1">
  <label for="q10r1">Amazon Aurora</label><br>
  <input type="radio" id="q10r2" name="question10" value="2">
  <label for="q10r2">Amazon DynamoDB</label><br>
  <input type="radio" id="q10r3" name="question10" value="3">
  <label for="q10r3">Amazon RDS</label><br>
  <input type="radio" id="q10r4" name="question10" value="4">
  <label for="q10r4">Amazon DocumentDB</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Amazon DynamoDB**.

Amazon DynamoDB est un service de base de données clé-valeur. Une base de données clé-valeur peut inclure des paires de données, telles que « Nom : Jean Dupont », « Adresse : 123, Avenue X » et « Ville : XYZ ».

Dans une base de données clé-valeur, vous pouvez ajouter ou supprimer des attributs des éléments de la table à tout moment. En outre, tous les éléments de la table ne doivent pas avoir les mêmes attributs.  

Les autres réponses sont incorrectes, car :

- Amazon Relational Database Service (Amazon RDS) et Amazon Aurora utilisent le langage de requête structuré (SQL) pour stocker et interroger les données. Il ne s’agit pas de bases de données clé-valeur.
- Amazon DocumentDB est un service de base de données documentaire compatible avec les charges de travail MongoDB.

En savoir plus :

- [Amazon DynamoDB](https://aws.amazon.com/dynamodb)
</div>

Question : **11/30**

**Quelle affirmation est VRAIE en ce qui concerne AWS Lambda ?**

<form>
  <input type="radio" id="q11r1" name="question11" value="1">
  <label for="q11r1">Vous payez uniquement le temps de calcul pendant lequel votre code est en cours d’exécution.</label><br>
  <input type="radio" id="q11r2" name="question11" value="2">
  <label for="q11r2">La première étape pour utiliser AWS Lambda consiste à mettre en service un serveur.</label><br>
  <input type="radio" id="q11r3" name="question11" value="3">
  <label for="q11r3">Pour utiliser AWS Lambda, vous devez configurer les serveurs qui exécutent votre code.</label><br>
  <input type="radio" id="q11r4" name="question11" value="4">
  <label for="q11r4">Avant d’utiliser AWS Lambda, vous devez prépayer une estimation de votre temps de calcul.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Vous payez uniquement le temps de calcul pendant lequel votre code est en cours d’exécution**.

AWS Lambda est un service qui vous permet d’exécuter du code sans devoir mettre en service ou gérer des serveurs. 

Lorsque vous utilisez AWS Lambda, vous ne payez que le temps de calcul que vous consommez. Vous êtes facturé uniquement lorsque votre code est en cours d’exécution. Avec AWS Lambda, vous pouvez exécuter du code pour pratiquement n’importe quel type d’application ou service backend, sans aucune tâche administrative. 

En savoir plus :

- [AWS Lambda](https://aws.amazon.com/lambda)
</div>

Question : **12/30**

**Quel outil vous permet de visualiser, de comprendre et de gérer vos coûts et votre utilisation d’AWS au fil du temps ?**

<form>
  <input type="radio" id="q12r1" name="question12" value="1">
  <label for="q12r1">AWS Budgets</label><br>
  <input type="radio" id="q12r2" name="question12" value="2">
  <label for="q12r2">AWS Cost Explorer</label><br>
  <input type="radio" id="q12r3" name="question12" value="3">
  <label for="q12r3">AWS Artifact</label><br>
  <input type="radio" id="q12r4" name="question12" value="4">
  <label for="q12r4">Calculatrice de prix AWS</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **AWS Cost Explorer**.

Avec AWS Cost Explorer, vous pouvez rapidement créer des rapports personnalisés pour analyser vos données d’utilisation et de coûts AWS.

Les autres réponses sont incorrectes, car :

- AWS Budgets vous permet de définir des alertes personnalisées qui vous avertiront lorsque l’utilisation de votre service dépasse (ou va dépasser) le montant que vous avez prévu au budget.
- La Calculatrice de prix AWS vous permet d’explorer les services AWS et de créer une estimation du coût de vos cas d’utilisation sur AWS. Dans la Calculatrice de prix AWS, vous pouvez saisir les détails de vos besoins en cloud computing, puis recevoir une estimation détaillée qui peut être exportée et partagée. 
- AWS Artifact est un service qui vous permet d’accéder aux rapports de conformité et de sécurité AWS et à certains contrats en ligne.

En savoir plus :

- [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
</div>

Question : **13/30**

**Quel service est utilisé pour transférer jusqu’à 80 Po de données vers AWS ?**

<form>
  <input type="radio" id="q13r1" name="question13" value="1">
  <label for="q13r1">Amazon CloudFront</label><br>
  <input type="radio" id="q13r2" name="question13" value="2">
  <label for="q13r2">Amazon Neptune</label><br>
  <input type="radio" id="q13r3" name="question13" value="3">
  <label for="q13r3">AWS Snowmobile</label><br>
  <input type="radio" id="q13r4" name="question13" value="4">
  <label for="q13r4">AWS DeepRacer</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **AWS Snowmobile**.

AWS Snowmobile est un service qui permet de transférer jusqu’à 80 Po de données vers AWS. Chaque Snowmobile est un conteneur de près de 14 mètres de long tiré par un semi-remorque. Il peut transférer jusqu’à 80 Po de données.

Les autres réponses sont incorrectes, car :

- Amazon Neptune est un service de base de données orientée graphe. Vous pouvez utiliser Amazon Neptune pour concevoir et exécuter des applications qui fonctionnent avec des jeux de données hautement connectés, tels que les moteurs de recommandation, la détection des fraudes et les graphiques de connaissances.
- Amazon CloudFront est un service de diffusion de contenu.
- AWS DeepRacer est une voiture de course autonome à l’échelle 1/18 que vous pouvez utiliser pour tester des modèles d’apprentissage par renforcement.

En savoir plus :

- [AWS Snow Family](https://aws.amazon.com/snow)
</div>

Question : **14/30**

**Quelles sont les tâches qui incombent à AWS ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q14r1" name="question14" value="1">
  <label for="q14r1">Configuration des périphériques d’infrastructures AWS</label><br>
  <input type="checkbox" id="q14r2" name="question14" value="2">
  <label for="q14r2">Formation des employés de l’entreprise concernant l’utilisation des services AWS</label><br>
  <input type="checkbox" id="q14r3" name="question14" value="3">
  <label for="q14r3">Conservation d’une infrastructure de virtualisation</label><br>
  <input type="checkbox" id="q14r4" name="question14" value="4">
  <label for="q14r4">Création d’utilisateurs et de groupes IAM</label><br>
  <input type="checkbox" id="q14r5" name="question14" value="5">
  <label for="q14r5">Configuration des groupes de sécurité sur les instances Amazon EC2</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
Les deux bonnes réponses sont les suivantes :

- **Conservation d’une infrastructure de virtualisation**
- **Configuration des périphériques d’infrastructures AWS**

Les trois autres options de réponse sont des tâches qui incombent aux clients.

En savoir plus :

- [Modèle de responsabilité partagée AWS](https://aws.amazon.com/compliance/shared-responsibility-model/)
</div>

Question : **15/30**

**Quelle affirmation décrit le mieux Amazon GuardDuty ?**

<form>
  <input type="radio" id="q15r1" name="question15" value="1">
  <label for="q15r1">Un service qui vérifie les applications pour détecter les failles de sécurité et les manquements aux bonnes pratiques de sécurité</label><br>
  <input type="radio" id="q15r2" name="question15" value="2">
  <label for="q15r2">Un service qui vous permet de surveiller les requêtes réseau qui entrent dans vos applications web</label><br>
  <input type="radio" id="q15r3" name="question15" value="3">
  <label for="q15r3">Un service qui permet de protéger vos applications contre les attaques par déni de service distribué (DDoS)</label><br>
  <input type="radio" id="q15r4" name="question15" value="4">
  <label for="q15r4">Un service qui assure une détection intelligente des menaces pour votre infrastructure et vos ressources AWS</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Un service qui assure une détection intelligente des menaces pour votre infrastructure et vos ressources AWS**.

AWS GuardDuty détecte les menaces grâce à un contrôle en continu des activités de réseau et des comportements des comptes au sein de votre environnement AWS.

Les autres réponses sont incorrectes, car :

- Un service qui permet de protéger vos applications contre les attaques par déni de service distribué (DDoS) : cette option de réponse décrit AWS Shield.
- Un service qui vérifie les applications pour détecter les failles de sécurité et les manquements aux bonnes pratiques de sécurité : cette option de réponse décrit Amazon Inspector.
- Un service qui vous permet de surveiller les requêtes de réseau qui entrent dans vos applications web : cette option de réponse décrit AWS WAF.

En savoir plus :

- [Amazon GuardDuty](https://aws.amazon.com/guardduty)
</div>

Question : **16/30**

**Quel pilier du framework AWS Well-Architected met l’accent sur l’utilisation des ressources informatiques de manière à répondre aux exigences du système ?**

<form>
  <input type="radio" id="q16r1" name="question16" value="1">
  <label for="q16r1">Fiabilité</label><br>
  <input type="radio" id="q16r2" name="question16" value="2">
  <label for="q16r2">Excellence opérationnelle</label><br>
  <input type="radio" id="q16r3" name="question16" value="3">
  <label for="q16r3">Efficacité des performances</label><br>
  <input type="radio" id="q16r4" name="question16" value="4">
  <label for="q16r4">Sécurité</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Efficacité des performances**.

Le pilier Efficacité des performances met l’accent sur l’utilisation efficace des ressources de calcul pour répondre aux exigences du système et maintenir l’efficacité au fur et à mesure que la demande change et que les technologies évoluent.

Les autres réponses sont incorrectes, car :

- Le pilier Excellence opérationnelle comprend la capacité à exécuter efficacement des charges de travail, à obtenir des informations sur leurs opérations et à améliorer continuellement les processus de prise en charge pour assurer une valeur opérationnelle. 
- Le pilier Sécurité est axé sur la protection des données, des systèmes et des ressources. Il met également l’accent sur l’utilisation des technologies cloud pour améliorer la sécurité de vos charges de travail.
- Le pilier Fiabilité met l’accent sur la capacité d’une charge de travail à exécuter de manière cohérente et appropriée les fonctions prévues.

En savoir plus :

- [AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
</div>

Question : **17/30**

**Quelle affirmation décrit le mieux Elastic Load Balancing ?**

<form>
  <input type="radio" id="q17r1" name="question17" value="1">
  <label for="q17r1">Un service qui distribue le trafic entrant sur plusieurs cibles, telles que les instances Amazon EC2</label><br>
  <input type="radio" id="q17r2" name="question17" value="2">
  <label for="q17r2">Un service qui vous permet de configurer, de gérer et de mettre à l’échelle un environnement distribué en mémoire ou en cache dans le cloud</label><br>
  <input type="radio" id="q17r3" name="question17" value="3">
  <label for="q17r3">Un service qui fournit des données que vous pouvez utiliser pour surveiller vos applications, optimiser l’utilisation des ressources et répondre aux changements de performances à l’échelle du système</label><br>
  <input type="radio" id="q17r4" name="question17" value="4">
  <label for="q17r4">Un service qui surveille vos applications et ajoute ou supprime automatiquement la capacité de vos groupes de ressources en réponse à l’évolution de la demande</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Un service qui distribue le trafic entrant sur plusieurs cibles, telles que les instances Amazon EC2**.

Un équilibreur de charge agit comme un point de contact unique pour tout le trafic web entrant de votre groupe Auto Scaling. Cela signifie que lorsque des instances Amazon EC2 sont ajoutées ou supprimées en réponse à la quantité de trafic entrant, ces demandes sont acheminées vers l’équilibreur de charge avant d’être réparties sur plusieurs ressources qui les géreront.

Les autres réponses sont incorrectes, car :

- Un service qui surveille vos applications et ajoute ou supprime automatiquement la capacité de vos groupes de ressources en réponse à l’évolution de la demande : cette option de réponse décrit AWS Auto Scaling.
- Un service qui fournit des données que vous pouvez utiliser pour surveiller vos applications, optimiser l’utilisation des ressources et répondre aux changements de performances à l’échelle du système : cette option de réponse décrit Amazon CloudWatch. Bien qu’Elastic Load Balancing optimise l’utilisation des ressources en répartissant le trafic entrant entre les ressources disponibles, il ne s’agit pas de la meilleure option de réponse, car Elastic Load Balancing n’assure pas toutes les autres fonctions répertoriées.
- Un service qui vous permet de configurer, de gérer et de mettre à l’échelle un environnement distribué en mémoire ou en cache dans le cloud : cette option de réponse décrit Amazon ElastiCache.

En savoir plus :

- [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing)
</div>

Question : **18/30**

**Quel service permet d’exécuter des applications conteneurisées sur AWS ?**

<form>
  <input type="radio" id="q18r1" name="question18" value="1">
  <label for="q18r1">Amazon SageMaker</label><br>
  <input type="radio" id="q18r2" name="question18" value="2">
  <label for="q18r2">Amazon Elastic Kubernetes Service (Amazon EKS)</label><br>
  <input type="radio" id="q18r3" name="question18" value="3">
  <label for="q18r3">Amazon Aurora</label><br>
  <input type="radio" id="q18r4" name="question18" value="4">
  <label for="q18r4">Amazon Redshift</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

  La bonne réponse est **Amazon Elastic Kubernetes Service (Amazon EKS)**.

Amazon EKS est un service entièrement géré que vous pouvez utiliser pour exécuter Kubernetes sur AWS. Kubernetes est un logiciel open source qui vous permet de déployer et de gérer des applications conteneurisées à grande échelle.

Les conteneurs vous fournissent un moyen standard de regrouper le code et les dépendances de votre application dans un seul objet. Les conteneurs sont fréquemment utilisés pour des processus et des flux de travail dans lesquels il existe des exigences essentielles en matière de sécurité, de fiabilité et de capacité de mise à l’échelle.

Les autres réponses sont incorrectes, car :

- Amazon SageMaker est un service qui vous permet de créer, de former et de déployer rapidement des modèles de machine learning.
- Amazon Aurora est une base de données relationnelle d’entreprise. 
- Amazon Redshift est un service de stockage de données que vous pouvez utiliser pour l’analytique des Big Data.

En savoir plus :

- [Amazon EKS](https://aws.amazon.com/eks)
</div>

Question : **19/30**

**Vous souhaitez qu’Amazon S3 contrôle les modèles d’accès de vos objets. Quelle classe de stockage devriez-vous utiliser ?** 

<form>
  <input type="radio" id="q19r1" name="question19" value="1">
  <label for="q19r1">S3 standard – Accès peu fréquent</label><br>
  <input type="radio" id="q19r2" name="question19" value="2">
  <label for="q19r2">S3 Glacier</label><br>
  <input type="radio" id="q19r3" name="question19" value="3">
  <label for="q19r3">S3 Intelligent-Tiering</label><br>
  <input type="radio" id="q19r4" name="question19" value="4">
  <label for="q19r4">S3 unizone – Accès peu fréquent</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **S3 Intelligent-Tiering**.

Dans la classe de stockage S3 Intelligent-Tiering, Amazon S3 surveille les modèles d’accès des objets. Si vous n’avez pas accédé à un objet pendant 30 jours consécutifs, Amazon S3 le déplace automatiquement vers le niveau d’accès peu fréquent, S3 standard – Accès peu fréquent. Si vous accédez à un objet du niveau d’accès peu fréquent, Amazon S3 le déplace automatiquement vers le niveau d’accès fréquent, S3 Standard.

Les autres réponses sont incorrectes, car :

- S3 Glacier est une classe de stockage économique idéale pour l’archivage des données. Vous pouvez récupérer des objets stockés dans la classe de stockage S3 Glacier en quelques minutes ou en quelques heures.
- La classe de stockage S3 standard – Accès peu fréquent est idéale pour les données rarement consultées, mais nécessitant une haute disponibilité en cas de besoin. S3 Standard et S3 Standard – Accès peu fréquent stockent les données dans un minimum de trois zones de disponibilité. S3 standard – Accès peu fréquent offre le même niveau de disponibilité que S3 Standard, mais à un prix de stockage moindre. 
- La classe de stockage S3 unizone – Accès peu fréquent est idéale pour les données rarement consultées qui ne nécessitent pas une haute disponibilité.

En savoir plus :

- [Classes de stockage Amazon S3](https://aws.amazon.com/s3/storage-classes/)
</div>

Question : **20/30**

**Quelle action pouvez-vous effectuer dans Amazon CloudFront ?**

<form>
  <input type="radio" id="q20r1" name="question20" value="1">
  <label for="q20r1">Diffuser du contenu aux clients par le biais d’un réseau mondial d’emplacements périphériques.</label><br>
  <input type="radio" id="q20r2" name="question20" value="2">
  <label for="q20r2">Mettre en service une section isolée de AWS Cloud pour lancer des ressources dans un réseau virtuel que vous définissez.</label><br>
  <input type="radio" id="q20r3" name="question20" value="3">
  <label for="q20r3">Exécuter une infrastructure selon une approche de cloud hybride.</label><br>
  <input type="radio" id="q20r4" name="question20" value="4">
  <label for="q20r4">Mettre en service des ressources à l’aide de langages de programmation ou d’un fichier texte.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Diffuser du contenu aux clients par le biais d’un réseau mondial d’emplacements périphériques**.

Amazon CloudFront est un service de diffusion de contenu. 

Il utilise un réseau d’emplacements périphériques pour mettre en cache le contenu et le fournir à des clients du monde entier. Lorsque le contenu est mis en cache, il est stocké localement sous forme de copie. Ce contenu peut inclure des fichiers vidéo, des photos et des pages web, entre autres.

Les autres réponses sont incorrectes, car :

- Exécuter une infrastructure selon une approche de cloud hybride : cette action peut être effectuée avec AWS Outposts. 
- Mettre en service des ressources à l’aide de langages de programmation ou d’un fichier texte : cette action peut être effectuée dans AWS CloudFormation.
- Mettre en service une section isolée de AWS Cloud pour lancer des ressources dans un réseau virtuel que vous définissez : cette action peut être effectuée dans Amazon Virtual Private Cloud (Amazon VPC).

En savoir plus :

- [Amazon CloudFront](https://aws.amazon.com/cloudfront)
</div>

Question : **21/30**

**Quelle affirmation décrit le mieux AWS Marketplace ?**

<form>
  <input type="radio" id="q21r1" name="question21" value="1">
  <label for="q21r1">Un outil en ligne qui inspecte votre environnement AWS et fournit des conseils en temps réel conformément aux bonnes pratiques AWS</label><br>
  <input type="radio" id="q21r2" name="question21" value="2">
  <label for="q21r2">Un catalogue numérique qui inclut des milliers de listes de logiciels de fournisseurs indépendants de logiciels</label><br>
  <input type="radio" id="q21r3" name="question21" value="3">
  <label for="q21r3">Une ressource qui fournit des conseils et des évaluations d’architectures et assure une communication continue avec votre entreprise pour la planification, le déploiement et l’optimisation de vos applications</label><br>
  <input type="radio" id="q21r4" name="question21" value="4">
  <label for="q21r4">Une ressource qui peut répondre à des questions concernant les bonnes pratiques et aider à résoudre les problèmes</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Un catalogue numérique qui inclut des milliers de listes de fournisseurs indépendants de logiciels**.

Vous pouvez utiliser AWS Marketplace pour trouver, tester et acheter des logiciels qui s’exécutent sur AWS.

Les autres réponses sont incorrectes, car :

- Une ressource qui peut répondre à des questions concernant les bonnes pratiques et aider à résoudre les problèmes : cette option de réponse décrit AWS Support.
- Une ressource qui fournit des conseils et des évaluations d’architectures et assure une communication continue avec votre entreprise pour la planification, le déploiement et l’optimisation de vos applications : cette option de réponse décrit un Technical Account Manager (TAM). 
- Un outil en ligne qui inspecte votre environnement AWS et fournit des conseils en temps réel conformément aux bonnes pratiques AWS : cette option de réponse décrit AWS Trusted Advisor.

En savoir plus :

- [AWS Marketplace](https://aws.amazon.com/marketplace)
</div>

Question : **22/30**

**Quelles actions pouvez-vous effectuer dans Amazon Route 53 ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q22r1" name="question22" value="1">
  <label for="q22r1">Relier les requêtes des utilisateurs à l’infrastructure dans AWS et en dehors d’AWS</label><br>
  <input type="checkbox" id="q22r2" name="question22" value="2">
  <label for="q22r2">Gérer les enregistrements DNS pour les noms de domaine</label><br>
  <input type="checkbox" id="q22r3" name="question22" value="3">
  <label for="q22r3">Surveiller vos applications et répondre aux changements de performances à l’échelle du système</label><br>
  <input type="checkbox" id="q22r4" name="question22" value="4">
  <label for="q22r4">Automatiser le déploiement des charges de travail dans votre environnement AWS.</label><br>
  <input type="checkbox" id="q22r5" name="question22" value="5">
  <label for="q22r5">Accéder aux rapports de conformité et de sécurité AWS et à certains contrats en ligne</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Relier les demandes des utilisateurs à l’infrastructure dans AWS et en dehors d’AWS.**
- **Gérer les registres DNS pour les noms de domaine.**

Amazon Route 53 est un service web de DNS. Il offre aux développeurs et aux entreprises un moyen fiable d’acheminer les utilisateurs finaux vers des applications Internet hébergées dans AWS. 

En outre, vous pouvez transférer des registres DNS pour des noms de domaine existants, actuellement gérés par d’autres bureaux d’enregistrement de domaines, ou enregistrer de nouveaux noms de domaine directement dans Amazon Route 53.

Les autres réponses sont incorrectes, car :

- Surveiller vos applications et répondre aux changements de performances à l’échelle du système : ces actions peuvent être effectuées dans Amazon CloudWatch.
- Accéder aux rapports de conformité et de sécurité AWS et à certains contrats en ligne : cette action peut être effectuée dans AWS Artifact.
- Automatiser le déploiement des charges de travail dans votre environnement AWS : cette action peut être effectuée avec AWS Quick Starts.

En savoir plus :

- [Amazon Route 53](https://aws.amazon.com/route53)
</div>

Question : **23/30**

**Quel composant ou service vous permet d’établir une connexion privée dédiée entre votre centre de données et votre cloud privé virtuel (VPC) ?**

<form>
  <input type="radio" id="q23r1" name="question23" value="1">
  <label for="q23r1">AWS Direct Connect</label><br>
  <input type="radio" id="q23r2" name="question23" value="2">
  <label for="q23r2">Passerelle Internet</label><br>
  <input type="radio" id="q23r3" name="question23" value="3">
  <label for="q23r3">Passerelle de réseau privé virtuel</label><br>
  <input type="radio" id="q23r4" name="question23" value="4">
  <label for="q23r4">Amazon CloudFront</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Direct Connect**.

AWS Direct Connect est un service qui vous permet d’établir une connexion privée dédiée entre votre centre de données et votre VPC.

La connexion privée fournie par AWS Direct Connect vous aide à réduire les coûts de réseau et à augmenter la quantité de bande passante pouvant circuler sur votre réseau.

Les autres réponses sont incorrectes, car :

- Amazon CloudFront est un service de diffusion de contenu. Il utilise un réseau d’emplacements périphériques pour mettre en cache le contenu et le fournir à des clients du monde entier.
- Une passerelle réseau privé virtuel vous permet d’établir une connexion de réseau privé virtuel (VPN) entre votre VPC et un réseau privé, tel qu’un centre de données sur site ou un réseau interne d’entreprise. Une passerelle réseau privé virtuel autorise uniquement le trafic vers le VPC s’il provient d’un réseau approuvé.
- Une passerelle Internet est une connexion entre un VPC et Internet. Elle permet au trafic public à partir d’Internet d’accéder à un VPC.

En savoir plus :

- [AWS Direct Connect](https://aws.amazon.com/directconnect)
</div>

Question : **24/30**

**Quel service permet de déployer et de mettre à l’échelle rapidement des applications sur AWS ?**

<form>
  <input type="radio" id="q24r1" name="question24" value="1">
  <label for="q24r1">AWS Elastic Beanstalk</label><br>
  <input type="radio" id="q24r2" name="question24" value="2">
  <label for="q24r2">AWS Snowball</label><br>
  <input type="radio" id="q24r3" name="question24" value="3">
  <label for="q24r3">Amazon CloudFront</label><br>
  <input type="radio" id="q24r4" name="question24" value="4">
  <label for="q24r4">AWS Outposts</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Elastic Beanstalk**.

Vous téléchargez votre application et Elastic Beanstalk effectue automatiquement les étapes du déploiement que sont la mise en service des capacités, l’équilibrage des charges, la scalabilité automatique et la surveillance de la santé des applications.

Les autres réponses sont incorrectes, car :

- AWS Outposts est un service qui vous permet d’exécuter une infrastructure selon une approche de cloud hybride.
- Amazon CloudFront est un service de diffusion de contenu. 
- AWS Snowball est un périphérique qui vous permet de transférer de grandes quantités de données vers et hors d’AWS.

En savoir plus :

- [AWS Quick Starts](https://aws.amazon.com/quickstart)
</div>

Question : **25/30**

**Quelle catégorie AWS Trusted Advisor comprend les vérifications des limites de vos services et des instances surexploitées ?**

<form>
  <input type="radio" id="q25r1" name="question25" value="1">
  <label for="q25r1">Optimisation des coûts</label><br>
  <input type="radio" id="q25r2" name="question25" value="2">
  <label for="q25r2">Sécurité</label><br>
  <input type="radio" id="q25r3" name="question25" value="3">
  <label for="q25r3">Tolérance aux pannes</label><br>
  <input type="radio" id="q25r4" name="question25" value="4">
  <label for="q25r4">Performances</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Performances**.

Dans cette catégorie, AWS Trusted Advisor contribue également à améliorer les performances de vos services en formulant des recommandations sur la façon de tirer parti du débit mis en service.

Les autres réponses sont incorrectes, car :

- La catégorie Sécurité comprend des vérifications qui vous permettent d’examiner vos autorisations et d’identifier les fonctions de sécurité AWS à activer.
- La catégorie Optimisation des coûts comprend des vérifications pour les ressources inutilisées ou inactives qui pourraient être éliminées et générer des économies de coûts.
- La catégorie Tolérance aux pannes comprend des vérifications qui vous permettent d’améliorer la disponibilité et la redondance de vos applications.

En savoir plus :

- [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)
</div>

Question : **26/30**

**Quelle stratégie de migration consiste à modifier l’architecture et le développement d’une application, généralement à l’aide de fonctions natives cloud ?**

<form>
  <input type="radio" id="q26r1" name="question26" value="1">
  <label for="q26r1">Refactorisation</label><br>
  <input type="radio" id="q26r2" name="question26" value="2">
  <label for="q26r2">Réhébergement</label><br>
  <input type="radio" id="q26r3" name="question26" value="3">
  <label for="q26r3">Rachat</label><br>
  <input type="radio" id="q26r4" name="question26" value="4">
  <label for="q26r4">Remaniement</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Refactorisation**.

Les autres réponses sont incorrectes, car :

- Le rachat consiste à remplacer une application existante par une version basée sur le cloud, telle qu’un logiciel sur AWS Marketplace.
- Le réhébergement consiste à déplacer une application vers le cloud en apportant peu de modifications, voire aucune, à l’application elle-même. Il est également connu sous le nom de « lift and shift ».
- Le remaniement consiste à optimiser de manière sélective les aspects d’une application pour bénéficier d’avantages dans le cloud sans modifier l’architecture de base de l’application. Il est également connu sous le nom de « lift, tinker, and shift ».

En savoir plus :

- [6 stratégies de migration d’applications vers le cloud](https://aws.amazon.com/blogs/enterprise-strategy/6-strategies-for-migrating-applications-to-the-cloud/)
</div>

Question : **27/30**

**Quelle option de calcul permet de réduire les coûts lorsque vous vous engagez à utiliser le calcul de manière régulière pour une période d’un an ou de trois ans ?**

<form>
  <input type="radio" id="q27r1" name="question27" value="1">
  <label for="q27r1">Savings Plans</label><br>
  <input type="radio" id="q27r2" name="question27" value="2">
  <label for="q27r2">Hôtes dédiés</label><br>
  <input type="radio" id="q27r3" name="question27" value="3">
  <label for="q27r3">Instances Spot</label><br>
  <input type="radio" id="q27r4" name="question27" value="4">
  <label for="q27r4">Instances réservées</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

  
La bonne réponse est **Savings Plans**.

Les Savings Plans Amazon EC2 vous permettent de réduire vos coûts de calcul en vous engageant à utiliser le calcul de manière régulière pour une période de 1 an ou de 3 ans. Cela permet d’économiser jusqu’à 72 % par rapport aux coûts des instances à la demande. Toute utilisation incluse dans l’engagement est facturée selon le tarif réduit du Savings Plan (par exemple, 10 USD l’heure). Toute utilisation au-delà de l’engagement est facturée aux tarifs habituels des instances à la demande.

Les autres réponses sont incorrectes, car :

- Les instances réservées constituent une remise de facturation appliquée à l’utilisation des instances à la demande de votre compte. Vous pouvez acheter des instances réservées standard et convertibles pour une période d’un an ou de trois ans, et des instances réservées planifiées pour une période d’un an. Contrairement aux Savings Plans, les instances réservées ne requièrent pas un engagement à utiliser le calcul de manière régulière pendant la durée du contrat.
- Les instances Spot sont idéales pour les charges de travail dont les heures de début et de fin sont flexibles ou qui peuvent résister aux interruptions. Les instances Spot tirent parti de la capacité de calcul EC2 inutilisée et vous offrent des économies de coûts jusqu’à 90 % sur les prix des instances à la demande.
- Les hôtes dédiéssont des serveurs physiques avec une capacité d’instance EC2 entièrement dédiée à votre utilisation. 
- Vous pouvez utiliser vos licences logicielles par socket, par nœud ou par machine virtuelle existantes pour vous aider à maintenir la conformité des licences. Vous pouvez acheter des hôtes dédiés à la demande ou réservés. Parmi toutes les options Amazon EC2 abordées dans le cadre de ce cours, les hôtes dédiés sont les plus chers.

En savoir plus :

- [Savings Plans](https://aws.amazon.com/savingsplans/)
</div>

Question : **28/30**

**Parmi les déclarations suivantes, laquelle décrit le mieux le concept de zones de disponibilité ?**

<form>
  <input type="radio" id="q28r1" name="question28" value="1">
  <label for="q28r1">Un emplacement géographique distinct avec plusieurs emplacements isolés les uns des autres</label><br>
  <input type="radio" id="q28r2" name="question28" value="2">
  <label for="q28r2">Une partie entièrement isolée de l’infrastructure mondiale d’AWS</label><br>
  <input type="radio" id="q28r3" name="question28" value="3">
  <label for="q28r3">Le serveur à partir duquel Amazon CloudFront obtient vos fichiers</label><br>
  <input type="radio" id="q28r4" name="question28" value="4">
  <label for="q28r4">Un site qu’Amazon CloudFront utilise pour mettre en cache des copies de contenu pour assurer une diffusion plus rapide aux utilisateurs, quel que soit l’emplacement</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Une partie entièrement isolée de l’infrastructure mondiale d’AWS**.

Une zone de disponibilité est un centre de données unique ou un groupe de centres de données au sein d’une région. 

Les zones de disponibilité sont situées à des dizaines de kilomètres les unes des autres. Cela les aide à fournir une interconnectivité pour prendre en charge les services et les applications qui s’exécutent dans une région.

Les autres réponses sont incorrectes, car :

- Un emplacement géographique distinct avec plusieurs emplacements isolés les uns des autres : cette option de réponse décrit une région.
- Le serveur à partir duquel Amazon CloudFront obtient vos fichiers : cette option de réponse décrit une origine.
- Un site qu’Amazon CloudFront utilise pour mettre en cache des copies de contenu pour assurer une diffusion plus rapide aux utilisateurs, quel que soit l’emplacement : cette option de réponse décrit un emplacement périphérique.

En savoir plus :

- [Infrastructure mondiale d’AWS](https://aws.amazon.com/about-aws/global-infrastructure)
- [Régions et zones de disponibilité](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/)
</div>

Question : **29/30**

**Quelle perspective du framework d’adoption de AWS Cloud met l’accent sur la récupération des charges de travail informatiques afin de répondre aux exigences des parties prenantes de votre entreprise ?**

<form>
  <input type="radio" id="q29r1" name="question29" value="1">
  <label for="q29r1">Perspective Entreprise</label><br>
  <input type="radio" id="q29r2" name="question29" value="2">
  <label for="q29r2">Perspective Opérations</label><br>
  <input type="radio" id="q29r3" name="question29" value="3">
  <label for="q29r3">Perspective Gouvernance</label><br>
  <input type="radio" id="q29r4" name="question29" value="4">
  <label for="q29r4">Perspective Personnes</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Perspective Opérations**.

La perspective Opérations du framework d’adoption du cloud AWS (AWS CAF) inclut également des principes d’exploitation dans le cloud en utilisant des bonnes pratiques en termes d’agilité.

Les autres réponses sont incorrectes, car :

- La perspective Entreprise vous aide à passer d’un modèle qui sépare les stratégies opérationnelles et informatiques à un modèle opérationnel qui intègre la stratégie informatique.
- La perspective Personnes aide les employés des ressources humaines (RH) à préparer leurs équipes à l’adoption du cloud en mettant à jour les processus organisationnels et les compétences du personnel afin d’y inclure des compétences basées sur le cloud.
- La perspective Gouvernance vous aide à comprendre comment mettre à jour les compétences du personnel et les processus organisationnels nécessaires à la gouvernance opérationnelle dans le cloud.

En savoir plus :

- [Livre blanc : Présentation du Framework d’adoption du Cloud AWS](https://d1.awsstatic.com/whitepapers/aws_cloud_adoption_framework.pdf)
</div>

Question : **30/30**

**Quel service vous permet de créer les flux de travail nécessaires à la vérification humaine des prédictions de machine learning ?**

<form>
  <input type="radio" id="q30r1" name="question30" value="1">
  <label for="q30r1">Amazon Aurora</label><br>
  <input type="radio" id="q30r2" name="question30" value="2">
  <label for="q30r2">Amazon Augmented AI</label><br>
  <input type="radio" id="q30r3" name="question30" value="3">
  <label for="q30r3">Amazon Lex</label><br>
  <input type="radio" id="q30r4" name="question30" value="4">
  <label for="q30r4">Amazon Textract</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Augmented AI**.

Amazon Augmented AI (Amazon A2I) fournit des flux de travail intégrés de vérification humaine pour les cas d’utilisation courants de machine learning, tels que la modération de contenu et l’extraction de texte à partir de documents. Avec Amazon A2I, vous pouvez également créer vos propres flux de travail pour les modèles de machine learning reposant sur Amazon SageMaker ou sur tout autre outil.

Les autres réponses sont incorrectes, car :

- Amazon Textract est un service de machine learning qui extrait automatiquement le texte et les données des documents numérisés.
- Amazon Lex est un service qui vous permet de créer des interfaces de conversation reposant sur la voix et le texte.
- Amazon Aurora est une base de données relationnelle d’entreprise.

En savoir plus :

- [Amazon Augmented AI](https://aws.amazon.com/augmented-ai)
</div>


Résultats du quiz : 73% = échec (seuil à 80%)