---
title: Déployer un backend TypeScript full-serverless avec SAM
description: 
published: true
date: 2023-07-03T13:43:35.531Z
tags: 
editor: markdown
dateCreated: 2023-07-03T13:35:27.395Z
---

# Déployer un backend TypeScript full-serverless avec SAM (1/3)

## Partie 1 - Objectifs, structure du repo et processus de build
Cette série de trois articles a pour objectif de présenter l’usage d’un **blueprint** pour déployer un **backend full-serverless** développé avec **typescript**. Cet article, le premier de la série, aura pour but d’expliquer pourquoi utiliser ce blueprint, ainsi que les besoins qui nous ont poussé à le développer. Il abordera également en détail la structure de l’application, plus particulièrement le processus de build et de packaging du code avant un déploiement sur AWS.

### Pourquoi utiliser un blueprint pour mes projets full-serverless ?
Plusieurs besoins nous ont poussés à réaliser ces blueprints. Le nombre de use-cases serverless étant de plus en plus important, le besoin principal que nous avons identifié est de pouvoir démarrer rapidement un projet full-serverless ou tout du moins de proposer un point de départ pour démarrer ce type de projet. Cela implique de pouvoir créer et déployer facilement une architecture full serverless en production. De plus, certains use-cases étant assez récurrents, il nous semblait pertinent de proposer une solution clé en main nécessitant uniquement de faire du développement spécifique à l’application souhaitée.

Un autre point important que nous souhaitions aborder est la mise en place de best practices. Pour les architectures récurrentes évoquées précédemment, notre but est de permettre que tout le monde ait le même socle de départ respectant les best-practices d’AWS, pour éviter d’avoir de nombreuses architectures différentes pour mettre en place un service similaire. Cela implique de mettre en place des best practices au niveau de la sécurité de l’architecture pour chacun des use-cases, et d’optimiser et minimiser l’espace occupé sur le service Lambda par l’application déployée. Cela passe par éviter les duplications de code, et le packager correctement.

Le dernier besoin auquel nous souhaitions répondre est celui de l’environnement de développement. Nous souhaitions créer un environnement de développement adapté à un langage choisi. Ici, nous avons opté pour un développement en TypeScript. L’idée est de pouvoir profiter des avantages de TypeScript, plus adapté à un projet de grande envergure que le JavaScript afin d’éviter des erreurs courantes notamment grâce à l’inférence de type. Le développeur pourrait ainsi créer son code en TypeScript, et le déployer facilement sur AWS Lambda, qui ne supporte pas nativement ce langage.

## Structure de l’architecture et de l’environnement de développement
### Architecture mise en place
L’architecture que nous allons mettre en place dans cette série d’articles pour illustrer ces blueprints est assez classique et consiste en une API sécurisée, cachée derrière un CloudFront, pouvant déclencher des fonctions Lambda pouvant, elles, lire ou écrire dans une table DynamoDB. Voici un schéma de l’architecture finale vers laquelle nous souhaitons nous diriger :


- Amazon Cognito nous servirait d’identity provider, et permettrait au client de s’authentifier auprès du CloudFront
- Nous nous servirons du CDN CloudFront pour les nombreux avantages qu’il présente (notamment en termes de sécurité). La distribution CloudFront ajouterait un header ayant pour valeur le secret stocké dans Secrets Manager à la requête originale du client afin d’être acceptée par le WAF.
- Le secret manager permet lui de stocker la valeur d’un secret et de la modifier périodiquement de façon sécurisée grâce à une fonction lambda de rotation.
- Le WAF permet quant à lui de vérifier qu’une requête est bien issue du CloudFront en vérifiant que le header “customisé” a bien été ajouté à la requête avec la valeur adéquate. Il permet ainsi d’autoriser l’appel à l’API ou non, en renvoyant un code 403 forbidden.
- Nous nous servirons également de la fonctionnalité de layers proposée par AWS Lambda afin de gérer plus efficacement le code partagé entre les différentes fonctions.

Plus de détails sur ces ressources et leur fonctionnement seront apportés dans les paragraphes et articles suivants.

Notons que l’architecture finale que nous souhaitons mettre en place présente également un WAF placé devant l’API, un Secrets Manager, un Cognito ainsi que d’un CloudFront. Ces éléments seront abordés plus en détail dans les articles suivants, leur mise en place faisant partie des best-practices d’AWS en termes de sécurité quant à la création d’un tel backend.

### Structure de l’environnement de développement
Intéressons nous maintenant à la partie applicative : les fonctions Lambda. Cette partie de l’architecture sera modifiée selon le besoin utilisateur.  Cependant nous avons mis en place une structure de code qui nous semble intuitive pour le développement en TypeScript ainsi qu’un processus de build/deploy simple à mettre en place dans un environnement de production.


Comme expliqué précédemment, nous souhaitons pouvoir utiliser TypeScript dans notre environnement de développement bien que celui-ci ne soit pas directement pris en charge par l’environnement AWS Lambda.

L’idée étant que le développeur puisse écrire ses fonctions en TypeScript, en pouvant réutiliser du code partagé entre ses fonctions, qu’avec une simple commande il puisse builder son projet, le packager, puis le déployer.

Nous avons donc dû créer une structure de projet assez particulière pour pouvoir supporter ces contraintes. Nous avons donc choisi de diviser en plusieurs parties le projet.

#### Dépendances liées au développement

Dans cette structure, toutes les dépendances liées au build de l’application sont définies dans le **dossier build**.

Dans le template que nous proposons de base, comme nous n’utilisons que typescript et webpack comme outils de développement, le dossier build ne contiendra que le fichier de configuration de typescript et un package.json listant les dépendances nécessaires au build de l’application. L'intérêt d'avoir centralisé les dépendances du build dans son propre package est d’éviter d’avoir à répéter la même configuration partout et de n’avoir qu’une seule déclaration des versions des dépendances, cela dans un souci de cohérence et de maintenance du code. Il faut cependant bien noter que les dépendances définies dans ce dossier ne seront pas incluses dans la stack finale déployée sur AWS, celles-ci n’étant pas nécessaires au runtime.

#### Le code partagé

Un problème récurrent lors du développement d’un projet full serverless est la réutilisation du code. Comme le travail est réalisé au niveau de la fonction, cela peut paraitre idiot d’avoir à répéter la déclaration d’un même morceau de code dans chacune des fonctions. Cela rend le code moins propre et plus difficilement maintenable. C’est pour cela que nous avons créé ce dossier.

Ainsi, le code partagé entre plusieurs fonctions est défini dans le **dossier shared**. Le code présent dans ce dossier sera déployé sous forme de layer sur AWS afin de prendre le moins de place possible et d’éviter que le code soit dupliqué sur le service AWS Lambda. Ainsi, le code ne sera ni dupliqué dans l’environnement local du développeur, ni dans l’environnement distant une fois la stack déployée. Dans le package.json de ce dossier on retrouve les dépendances nécessaires au développement et à l’exécution des différentes fonctions de notre application.


Pour ce qui est de la structure même de ce dossier, nous avons choisi de diviser le code source en plusieurs fichiers. Un par type de dépendance, par exemple nous avons un fichier logger.ts qui va définir un logger pino, et un fichier aws-sdk.ts qui va lui exporter les objets liés au aws-sdk utilisés dans différentes fonctions (un client dynamoDB pour nos fonctions par exemple) pour éviter d’avoir à les re-déclarer dans chacune d’elles. Ce dossier contient également un fichier index.ts qui va lui exporter tout le code défini dans les autres fichiers, ceci afin de ne pas avoir à modifier le package.json à chaque fois que du code est ajouté.

#### Le code des fonctions

Le code source de toutes les fonctions est défini dans le dossier function. Ce code source, une fois le développeur satisfait, va être packagé avec webpack pour créer des fichiers JS les plus petits et optimisés possibles avant de les déployer. Pour le packaging des fonctions, nous avons choisi de nous servir d’un unique package.json afin de pouvoir gérer plus efficacement les dépendances (i.e. ne pas avoir à modifier 20 fichiers pour changer une simple dépendance), pour la cohérence du code et l’accélération du build des fonctions.


Afin d’illustrer le tout, nous avons mis en place 2 fonctions très simples:

- click: cette fonction va ajouter (ou mettre à jour) un item compteur qui a pour clé la date du jour dans la table dynamoDB. Elle a vocation à illustrer une opération en écriture dans la table.
- get-total-clicks: cette fonction va retourner le nombre de “click” effectué le jour même, cette fonction a donc vocation à illustrer une opération en lecture dans la table DynamoDB.

#### Le dossier layers

Ce dossier n’a normalement jamais besoin d’être modifié. Il contient uniquement le package.json nécessaire à la création du layer Lambda, celui-ci reprenant uniquement le code du dossier shared pour créer le layer contenant l’ensemble du code partagé.

Nous reviendrons plus en détail sur le fonctionnement et la mise en place des layers dans l’article suivant à propos du framework SAM.

#### Au final, comment build le projet et le packager?

Donc pour résumer, cela va permettre au développeur de coder avec TypeScript en local et de profiter de tous ses avantages, notamment l’inférence de type. Il peut mettre à jour les dépendances et build son projet dans les différents dossiers en se servant de la commande custom **yarn build**, qui va donc installer les dépendances dans les différents dossiers et transpiler les fichiers TypeScript en JS.

Une fois que le développeur a fini de travailler sur son code, d’une simple commande **yarn bundle**, il peut packager tout le code grâce à webpack, dont le fichier de configuration webpack.config.ts est le suivant.


Ce fichier, écrit en TypeScript, indique à webpack quels fichiers aller chercher dans l'arborescence afin de les packager. On va notamment lui indiquer de chercher uniquement dans les dossiers finissant par “-function” et de packager le fichier “/src/app.ts” (point d’entrée des fonctions) de ces dossiers. Ce fichier de configuration définit également l’output de la commande et où se trouveront les fichiers bundle.js qui seront utilisés par SAM pour build les fonctions Lambda. Enfin, nous avons utilisé une librairie complémentaire, **webpack-node-externals**, qui permet, par la simple utilisation de la fonction nodeExternals(), de spécifier à webpack de ne pas packager le dossier node_modules. Cette opération permet de rendre les fichiers de sortie beaucoup plus légers et de fortement accélérer le processus.

Dans l’article suivant, nous vous expliquerons le fonctionnement de l’outil SAM, comment nous l’utilisons dans ce projet, et comment s’en servir pour déployer l’architecture décrite ci-dessus sur AWS.

---

# Déployer un backend TypeScript full-serverless avec SAM (2/3)

## Partie 2 - Intégration au template Serverless Application Model (SAM)
Dans le premier article nous avons expliqué en détail la structure du code mise en place pour ce blueprint ainsi que le processus de build à suivre pour obtenir des fonctions lisibles par Lambda. Nous allons maintenant revenir pas à pas sur la création des différentes ressources de l’architecture grâce à SAM ainsi que sur le déploiement de l’application Serverless dans AWS.

## SAM, c’est quoi ?
Serverless Application Model, ou SAM, est un framework open-source d’AWS destiné à gérer le développement, le debug et le déploiement d'applications Serverless via Lambda. Il est composé de deux outils : une spécification de template et une interface en ligne de commande.

La spécification, basée sur celle de CloudFormation, offre une syntaxe simple et efficace pour définir les fonctions, API, évènements et autres ressources qui constitueront votre application Serverless. Les Lambda par exemple sont définies par une ressource “AWS::Serverless::Function” comme présenté dans le code ci-dessous.


Au moment du build, SAM va réaliser la traduction de cette syntaxe en syntaxe CloudFormation et va implicitement définir les rôles, permissions et aliases liés à la fonction Lambda. SAM propose également une grand nombre de policies gérées par AWS et prêtes à être utilisées (ici DynamoDBReadPolicy) ce qui simplifie la déclaration de Lambdas. D’autres ressources liées à la partie applicative possèdent une spécification en SAM (table DynamoDB, API Gateway, …) mais la plupart des ressources orientées infra sont définies comme en syntaxe CloudFormation. Certaines propriétés peuvent également être factorisées dans une section **Globals** située au début du template, ce qui évite de nombreuses répétitions de ces propriétés.


Le principal avantage de SAM est que le template fait office d’entité centralisée (tout dans une seule stack), versionable et déployable de votre application Serverless. Mais le framework propose également d’autres fonctionnalités intéressantes comme l’intégration facile de best practices : tracing avec X-Ray, déploiements avec CodeDeploy, …

La spécification de SAM nécessite que chaque propriété CodeUri des fonctions Lambda pointe sur un dossier contenant un fichier package.json. Seulement, comme explicité dans l’article 1, notre structure projet ne possède qu’un seul **package.json**, commun à toutes les fonctions et situé à la racine. Pour pouvoir tout de même build le projet, la solution trouvée a été de mettre un package.json vide dans chaque dossier de fonction, en attendant une meilleure gestion de ce cas d’usage par SAM.


La propriété Handler des fonctions Lambda quant à elle est définie dans la section Globals et pointe vers le fichier bundle.js généré par webpack.

Le deuxième composant de SAM est sa puissante interface en ligne de commande. Les commandes proposées par la CLI permettent de vérifier la validité du template, build/deploy l’application Serverless sur AWS mais également d’invoquer les Lambdas en local, ce qui facilite le debug. Ces commandes pourront également être intégrées à des pipelines de CI/CD pour automatiser le build et le déploiement.

## Le template “TypeScript backend”

L’architecture que nous avons choisi de mettre en place pour illustrer ce projet nécessite la définition de multiples ressources dans un template SAM. La partie concernant la sécurisation de l’API sera abordée dans l’article suivant, nous nous attarderons ici sur le cœur de l’application : Lambda, API et CloudFront.

Nous avons brièvement évoqué le fonctionnement du Lambda Layer dans l’article précédent : il s’agit d’une ressource Lambda contenant les dépendances partagées par les différentes fonctions d’une application. Sa déclaration dans le template est assez simple et pointe vers le dossier “layers” de notre arborescence.


Le package.json contenu dans le dossier pointé par ContentURI contient un script de build qui, grâce à la commande rsync, vise à exclure les dossiers src et node_modules lors de la constitution du dossier dist du layer. En effet, SAM effectuant une commande “npm install” plus tard dans le processus de build, il n’est pas nécessaire d’inclure node_modules. L’utilisation de la commande rsync sur le répertoire shared est nécessaire car à l’heure actuelle, SAM ne supporte pas les chemins locaux passés en dépendance.


Une fois le Layer déclaré, on peut le référencer dans nos fonctions Lambda et celle-ci aura ainsi accès à toutes les dépendances qu’il contient. Les autres propriétés essentielles des fonctions Lambda sont le chemin vers le code packagé par webpack, sa Policy associée (gérée par AWS ou bien personnalisée) et bien sûr l’évènement qui va déclencher la Lambda, ici un appel à l’API.


On peut noter sur cette capture d’écran la référence à un paramètre **ApiStageName**. Tout comme CloudFormation, les templates SAM sont paramétrables, ce qui permet par exemple d’utiliser facilement le même template pour plusieurs environnements (dev, production, …). Ces paramètres sont entrés soit lors d’un prompt via la commande “sam deploy --guided”, soit directement dans la commande via l’option --parameter-overrides.

L’API Gateway est définie succinctement dans le template car les spécifications de chacun de ses endpoints, des fonctions associées, des réponses HTTP et de la sécurité de l’API se situent dans le fichier **swagger.yaml** référencé par la propriété DefinitionBody. La présence de ce fichier de spécification OpenAPI est une best practice car il permet non seulement d’alléger le template SAM mais également utilisé par API Gateway pour générer une documentation versionnée de l’API.

Nous avons choisi un endpoint de type régional pour notre API, ce qui signifie que le endpoint sera créé et hosté sur la région dans laquelle il a été créé. Dans ce cas là, il est recommandé de positionner un CloudFront devant l’API, ce qui va permettre de distribuer l’API de manière globale via des caches et de pouvoir mettre en place un contrôle d’accès sur lequel nous reviendrons en détail dans l’article suivant. Une distribution CloudFront est donc déclarée dans le template SAM avec comme unique origine l’API Gateway.

## Comment déployer cette application Serverless ?
Une fois le code transpilé et packagé grâce à webpack et le template écrit en spécification SAM, il est très simple de déployer notre application Serverless sur AWS. Pour cela on va utiliser deux commandes de la SAM CLI :

- **sam build** : Cette commande va préparer l’application au déploiement en allant dans un premier temps lire le fichier swagger.yaml pour construire l’API. Elle va ensuite packager le Layer puis les fonctions Lambda en se servant du code préparé par webpack, pour enfin traduire le template SAM en template CloudFormation. Cette traduction comprend notamment la transformation des ressources Serverless en différentes ressources CloudFormation inhérentes aux Lambda par exemple. Toutes ces ressources sont placées dans un dossier .aws-sam, prêtes pour le déploiement.
- **sam deploy** : Une fois le build effectué, la dernière étape est le déploiement. Cette commande peut être utilisée dans un terminal avec l’option --guided, qui lance un prompt permettant à l’utilisateur d’entrer ses paramètres, la région ou le nom de la stack CloudFormation. Ces valeurs peuvent bien sûr être passées grâce à des options afin d’automatiser le processus dans des pipelines CI/CD. La configuration de déploiement peut être sauvegardée dans un fichier samconfig.toml pour faciliter les futurs déploiements.

Il est important de rappeler que nous avons choisi d’implémenter des fonctions Lambda en relation avec une table DynamoDB pour démontrer les avantages d’un tel projet, mais la partie applicative peut être complètement modifiée selon le besoin utilisateur : URL pré-signées S3, log de messages SNS, …

Nous avons choisi SAM pour réaliser ce blueprint au détriment d'autres frameworks comme Serverless Framework ou bien d’outils comme Terraform car SAM est natif à AWS. Nous trouvons que sa syntaxe simplifiée pour les ressources Serverless et son intégration de best practices est idéale pour une application comme celle-ci. Toutefois, Serverless Framework possède de nombreux plugins offrant ces avantages et aurait également pu être un bon choix pour réaliser ce template.

Dans l’article suivant, nous aborderons le besoin de sécuriser cette application, les outils choisis pour mettre en place cette sécurité et comment le template SAM est affecté par ces changements.

---

# Déployer un backend TypeScript full-serverless avec SAM (3/3)

## Partie 3 - Sécurité s’il vous plaît
Cette série de trois articles a pour objectif de présenter l’usage de **blueprints** pour déployer un **backend full-serverless** développé avec **typescript**. Cet article, le dernier de la série, aura pour but d’expliquer les best practices liées à la sécurité d’un tel backend avec AWS. Ces best practices étant mises en place grâce à ce blueprint.

## Pourquoi sécuriser ses ressources publiques?
Un des aspects les plus importants de ce blueprint est la sécurité : en effet, l’architecture contient un point d’entrée public qui est l’API, il est donc nécessaire de le sécuriser.

Cette sécurité est mise en place à 2 niveaux : un contrôle d’accès utilisateur grâce à Cognito et un pare-feu WAF qui bloque les requêtes venant d’une source autre que le CloudFront.

Pour rappel, ceci est l’architecture finale que nous allons mettre en place :


Cet article abordera donc l’utilisation de Amazon Cognito, ainsi que du Secrets Manager et du WAF. Il est important de comprendre pourquoi il est essentiel de mettre ces ressources en place.

### Pourquoi se servir de Cognito avec son API?
Amazon Cognito est un service simple de synchronisation des données et de l'identité des utilisateurs qui nous aide à gérer et à synchroniser en toute sécurité les données des applications pour nos utilisateurs sur leurs appareils mobiles.

Il nous permet de créer des identités uniques pour vos utilisateurs par l'intermédiaire de plusieurs fournisseurs de connexion publics (Amazon, Facebook et Google) et de prendre en charge les invités non authentifiés. Grâce à Cognito nous pouvons sauvegarder les données des applications localement sur les appareils des utilisateurs, ce qui permet aux applications de fonctionner même lorsque les appareils sont hors ligne.

Nous nous servirons ici de Cognito comme d’un authentificateur auprès de l’API pour autoriser (ou non) certains utilisateurs à accéder à certaines ressources de l’API.

### Pourquoi mettre un CloudFront devant son API?
Outre les nombreux avantages de se servir d’une distribution CloudFront qui sont listés ici, nous parlerons des avantages liés à la sécurité qu’apportent CloudFront à notre infrastructure.

#### Protection contre les attaques des couches réseau et application

Amazon CloudFront, AWS Shield, AWS Web Application Firewall (WAF) et Amazon Route 53 fonctionnent conjointement en toute transparence afin de créer un périmètre de sécurité flexible et à couches contre les différents types d'attaques, dont les attaques DDoS contre les couches réseau et application. Tous ces services sont corésidents à la périphérie d'AWS et fournissent un périmètre de sécurité extensible, fiable et aux performances élevées pour nos applications et contenus. Avec CloudFront comme «porte d'entrée» de notre application et infrastructure, nous retirons la surface d'attaque principale de nos contenus, données, codes et infrastructures critiques.

#### Chiffrements SSL/TLS et HTTPS

Avec Amazon CloudFront, nous pouvons diffuser notre contenu, les API ou les applications via SSL/TLS et les fonctionnalités SSL avancées sont activées automatiquement. Nous pouvons utiliser AWS Certificate Manager (ACM) afin de créer facilement un certificat SSL personnalisé et de déployer notre distribution CloudFront gratuitement. ACM gère automatiquement le renouvellement des certificats, en éliminant les frais généraux et les coûts associés à un processus de renouvellement manuel. De plus, CloudFront fournit un certain nombre d'optimisations et de capacités avancées SSL, comme les connexions HTTPS avec semi pont/pont complet, le stapling OCSP, les tickets de session, la confidentialité persistante parfaite, les impositions de protocole TLS et le chiffrement au niveau du champ.

#### Contrôle d'accès

Avec Amazon CloudFront, nous pouvons limiter l'accès à nos contenus via un certain nombre de capacités. Avec les URL signées et les cookies signés, nous pouvons prendre en charge l'authentification des jetons afin de limiter l'accès aux visionneuses authentifiées uniquement. La capacité de restriction géographique nous permet d'empêcher les utilisateurs situés dans des emplacements géographiques spécifiques d'accéder à des contenus que vous diffusez via CloudFront.

#### Conformité

L'infrastructure et les processus CloudFront sont tous conformes à PCI-DSS Niveau 1, HIPAA et ISO 9001, ISO 27001, SOC (1, 2 et 3) pour diffuser en toute sécurité nos données les plus sensibles.

Seulement, c’est bien beau de placer une distribution CloudFront devant son API, mais si celle-ci peut être contournée, cela perd tout de suite de son intérêt. Nous allons donc ici mettre en place une architecture permettant d’avoir accès à notre API uniquement via notre distribution CloudFront et bloquer les accès directs à l’API aux petits malins essayant de contourner la distribution.

## Cognito
Dans un premier lieu parlons de l’identification via Cognito : Ce service AWS utilise le protocole OAuth2 qui est le protocole standard d’autorisation pour les API. Le fonctionnement mis en place ici est simple : on veut pouvoir authentifier à la fois des utilisateurs mais aussi des applications client. On va donc définir un UserPool classique et deux UserPoolClients qui vont lui être associés.

- Le premier, UsernameClient va permettre à un utlisateur de s’enregistrer via son email et un mot de passe. Il pourra ensuite récupérer un token d’authentification.
- Le second, ApiClient va permettre l'identification server-to-server, c'est-à-dire qu’avec l’ID du client et son secret on va pouvoir directement récupérer un token.

Nous avons choisi d’implémenter les deux pour montrer que c’était possible et donner une façon de le faire, bien évidemment certains projets ne nécessitent pas les deux systèmes d’authentification. Il faudra adapter cette partie en fonction du cas d’usage.

Une fois le token récupéré, on va pouvoir envoyer notre requête à CloudFront en étant sûr qu’on est bien identifiés grâce au header Authorization. Ce qui nous amène à la deuxième partie, comment forcer le passage par la distribution CloudFront devant notre API avec WAF et Secrets Manager.

### Mise en place de Cognito avec SAM
La mise en place de la sécurité Cognito implique un certain nombre de ressources à mettre en place dans le template SAM:

**Cognito User Pool**: le User Pool n’est ni plus ni moins qu’un répertoire d’utilisateurs pour notre application, nous choisissons ici des paramètres très basiques, à noter que ceux-ci peuvent être bien plus personnalisés (voir [Documentation](https://docs.aws.amazon.com/fr_fr/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpool.html))


**Cognito User Pool Domain**: Cette ressource nous permet de configurer l'adresse de nos pages web d'inscription et de connexion. Encore une fois, pour de plus amples informations nous vous invitons à lire la [documentation](https://docs.aws.amazon.com/fr_fr/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpooldomain.html)


**Cognito User Pool Clients**: Une application est une entité au sein d'un groupe d'utilisateurs qui a la permission d'appeler des opérations API non authentifiées (opérations qui n'ont pas d'utilisateur authentifié). Il s'agit par exemple d'opérations d'enregistrement, de connexion et de gestion des mots de passe oubliés. Encore une fois, ici nous en avons défini 2, un pour l’identification côté client et l’autre pour l’identification côté serveur.

Plus d’informations pour la personnalisation de ces ressources [ici](https://docs.aws.amazon.com/fr_fr/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpoolclient.html).



**Cognito Resource Server**: C’est un serveur de ressources dont l'accès est protégé. Il traite les demandes authentifiées provenant d'une application qui possède un token. Cette ressource est nécessaire pour créer un authorizer dans l’API. Plus d’informations sur cette ressource [ici](https://docs.aws.amazon.com/fr_fr/AWSCloudFormation/latest/UserGuide/aws-resource-cognito-userpoolresourceserver.html).


L’inclusion du Cognito pour l’API inclut également des modifications sur la déclaration de l’API dans le template, il faut en effet rajouter un authorizer dans la déclaration de celle-ci:


## WAF et Secret Manager
### Secrets Manager
AWS Secrets Manager est le service AWS fournissant un coffre-fort sécurisé dans lequel est stocké les secrets nécessaires pour accéder à nos applications, services ou ressources informatiques. Le service nous permet de faire pivoter, de gérer et de récupérer facilement les informations d'identification des bases de données, les clés API et d'autres secrets tout au long de leur cycle de vie. Les utilisateurs et les applications récupèrent les secrets en appelant les API de Secrets Manager, ce qui élimine la nécessité de coder en dur les informations sensibles en “plain text”.

### WAF
Nous placerons devant l’API, un Web Application Firewall (WAF) Régional permettant de mettre en place une Web Access Control List (Web ACL).

Une liste de contrôle d'accès web nous permet d’avoir un contrôle précis sur les demandes web auxquelles notre API Amazon API Gateway va répondre. Nous pouvons autoriser ou bloquer les types de requêtes suivants :

- Provenant d'une adresse IP ou d'une plage d'adresses IP ;
- Provenant d'un pays ou de pays spécifiques
- Contenant une chaîne spécifiée ou correspondant à un modèle d'expression régulière (regex) d'un ensemble particulier de demandes
- dépassant une longueur spécifiée ;
- contenant un code SQL malveillant (appelé injection SQL) ;
- contenant des scripts malveillants (appelés scripts inter-site).

Ici, nous nous servirons des règles mises en place par la Web ACL pour vérifier si la valeur du header X-Origin-Verify existe et est bien égale à la valeur stockée dans Secret Manager, au sein d’AWS, donc inaccessible au grand public.

Si cela est bien le cas, cela signifie que la requête auprès de l’API vient bien de la distribution CloudFront, et la requête ne sera donc pas interdite par le WAF.

### Rotation du secret
Bien que le service Secrets Manager offre une rotation de secret intégrée pour certains services (Amazon RDS, Redshift, DocumentDB…), ce n’est pas le cas pour notre use case, nous devons donc définir nous même la procédure de rotation, ainsi que l’intervalle auquel nous souhaitons modifier ce secret.

Pour cela nous devons coder une fonction lambda (dite de rotation) qui a pour rôle de modifier la valeur du secret dans Secrets Manager et dans les services utilisés, ici la Web ACL et la distribution CloudFront.

Nous ne rentrerons pas dans les détails du code, il faut juste retenir que cette fonction va créer un nouveau secret “en attente”, le tester auprès des différents services, si les tests sont validés, va changer le statut du secret de “en attente” à “actuel” et supprimer l’ancien secret. Vous êtes libres d’aller voir le code de la fonction, et plus d’informations sont disponibles sur la [documentation officielle d’AWS](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets-lambda-function-overview.html).

Notons également que nous avons choisi de conserver l’autorisation sur le précédent secret dans les règles de la Web ACL en raison du temps nécessaire à la mise à jour de la distribution CloudFront, qui peut parfois prendre plusieurs heures. Ceci afin de ne pas refuser des requêtes pouvant être légitimes.

### Mise en place des ressources avec SAM
WAF Rule Group: C’est dans le WAF Rule Group que nous définissons une règle “OR” qui va autoriser 2 secrets. C’est grâce à ce statement que nous pouvons toujours accepter le précédent secret mais également l’actuel afin de ne pas rejeter des requêtes légitimes au vu du temps que prend CloudFront pour se mettre à jour.


Il faut également ajouter le custom header “X-Origin-Verify” au CloudFront, et qu’il référence la même valeur que celle attendue par le WAF :


Finalement, la ressource WebACLAssociation, définie dans le template permet, elle, de placer notre WebACL devant notre API afin de la protéger. Comme vous pouvez le voir, la déclaration de cette association est très simple :


Pour la rotation du secret, la Lambda doit tout de même être définie dans le template SAM, avec un certain nombre de variables d’environnement ainsi que les politiques d’accès aux ressources qui conviennent :


De plus il faudra donner la permission au Secrets Manager d’invoquer la fonction ainsi que créer un rotation schedule permettant de spécifier la fréquence à laquelle nous souhaitons que la valeur du secret soit modifiée:


Pour les autres ressources liées au WAF et au Secrets Manager, elles sont toutes présentes dans le fichier template.yaml à la racine du projet.

## Déployez votre application full serverless
Vous cherchez à déployer une application serverless sécurisée et développée en TypeScript ? Après avoir lu ces trois articles et en utilisant le blueprint, cela devrait maintenant être un jeu d’enfant car l’architecture globale de l’application ne devrait pas être sujette à changement. En effet, seule la partie applicative, dépendant du besoin utilisateur, peut être adaptée et modifiée sans perturber le fonctionnement global de l’application. Il vous suffira de modifier le template SAM expliqué dans l’article 2 et d’ajouter vos fonctions et dépendances associées aux dossiers correspondants de l’arborescence (voir article 1).

La sécurisation de l’API peut elle aussi être personnalisée selon vos besoins, notamment via le choix de client Cognito ou bien par la modification de la période de rotation du secret utilisé par le WAF. Une fois toutes vos ressources mises en place, vous n’avez plus qu’à suivre le processus simple de build/deploy pour obtenir une application entièrement serverless déployée sur le cloud AWS.

## Coûts
Il faut cependant noter les coûts qu’impliquent la mise en place de toutes ces ressources. Bien que le côté serverless de cette architecture permette de réduire les coûts, il nous semble important de détailler le mode de facturation du WAF et du Secret Manager. Sur notre plateforme de démonstration, le coût global mensuel était inférieur à $5.

### WAF
Les frais d’implémentation d’un WAF sont détaillés par AWS dans leur [documentation](https://aws.amazon.com/fr/waf/pricing/#:~:text=Remarque%203%20%3A%20le%20prix%20d,associ%C3%A9%20%C3%A0%20votre%20ACL%20web.). Ce qu’il est important de retenir est le tableau suivant:

|Type de ressources|Prix|
|:----------------:|:--:|
|ACL Web|5,00 USD par mois (au prorata horaire)|
|Règle|1,00 USD par mois (au prorata horaire)|
|Demande|0,60 USD par tranche de 1 million de demandes|

Pour notre exemple qui inclut 1 ACL Web, 1 Rule Group (avec une seule règle), le WAF coûterait 6 USD par mois, auquel viennent s’ajouter les frais liés à la demande.

Nous pouvons cependant noter que si un WAF est déjà utilisé dans une architecture, l’implémentation de la règle de vérification du header n’implique qu’un coût de 1 USD par mois.

### Secrets Manager

Pour ce qui est de la tarification d’un Secrets Manager, elle est décrite ici. Bien que les coûts impliqués soient minimes, il nous semblait important de les détailler. Secrets Manager facture 0,40 USD par secret stocké par mois, et 0,05 USD par tranche de 10 000 appels vers l’API Secrets Manager. L’architecture ici mise en place a pour seul appel vers l’API la rotation de secret. Si celle-ci est effectuée 1 fois par jour, le coût total du Secrets Manager pour cette solution devrait tourner autour de 0,40 USD par mois.

source : https://blog.ippon.fr/