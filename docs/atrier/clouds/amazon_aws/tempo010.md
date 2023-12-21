---
title: Accélérez votre développement d’applications serverless avec SAM
description: 
published: true
date: 2023-06-09T19:35:14.936Z
tags: cloud, aws, devops, sam, serverless, infra-as-code
editor: markdown
dateCreated: 2023-06-09T19:06:40.470Z
---

# Accélérez votre développement d’applications serverless avec SAM

![sam.jpeg](/assets/img/clouds/tempo/sam.jpeg)

## C’est qui SAM ?
SAM dans l'écosystème AWS, ce n’est pas celui qui conduit lorsque vous avez un peu trop bu.

SAM (Serverless Application Model) est un ensemble d’outils mis à votre disposition par AWS afin de faciliter la création et le déploiement d’applications serverless.

Deux principaux groupes de fonctionnalités sont présents:

- Description des ressources AWS à déployer avec un fichier en YAML ou JSON (infrastructure as code).
- Interaction avec les ressources décrites précédemment (déploiement, exécution en local, publication de paquet…) grâce à l’interface en lignes de commandes SAM CLI.

Ces deux groupes sont détaillés dans les parties suivantes:

## La description de ressource CloudFormation avec SAM

### Qu’est ce que CloudFormation ?
Avant de parler de la principale fonctionnalité de SAM, il est nécessaire de présenter le service d’infrastructure-as-code d’AWS: CloudFormation.

Ce service permet de gérer l’infrastructure déployée sur AWS avec des fichiers au format YAML ou JSON appelés “fichiers template”.

Voici un exemple très simple d’un fichier de template créant un bucket S3:

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: 'AWS CloudFormation Sample Template:
 Sample template which will create an s3 bucket'
Parameters:
 BucketName:
   Type: String
   Description: 'The name of the S3 Bucket to create, make this unique'
Resources:
 S3BUCKET:
   Type: 'AWS::S3::Bucket'
   Properties:
     BucketName: !Ref BucketName
Outputs:
 Bucket:
   Description: S3 Bucket Name
   Value: !Ref S3BUCKET
```

Ici, 3 principales sections sont présentes : **Parameters**, **Resources** et **Outputs**.

Plusieurs autres sections peuvent être également présentes dans ce fichier de template. Le but de cet article n’étant pas de présenter en détail CloudFormation, je vous invite à consulter la documentation AWS pour plus d’informations: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html

La partie Resources est la partie la plus importante du template et est d’ailleurs la seule obligatoire.
Cette partie permet de définir les ressources AWS qui seront créées lors du déploiement du template.
Ces différentes ressources peuvent être retrouvées dans la documentation suivante: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html

Le service CloudFormation étant un service AWS classique, nous pouvons interagir avec via le CLI AWS ou la console de gestion AWS (interface web), et l’on peut exécuter des actions telles que:

- Lister les stacks déployées;
- Décrire chaque stack pour récupérer :
  - L’état de la stack ;
  - La liste des ressources AWS créées associées à la stack ;
  - L’historique des modifications réalisées par AWS lors du changement des ressources (création, modification et suppression) ;
  - etc

Une stack, au sens CloudFormation, est un ensemble de ressources AWS regroupées dans une seule unité. Ainsi lors de la création de cette stack, l’ensemble des ressources décrites seront créées. Si une ressource échoue lors de la création, tout le déploiement est annulé et aucune ressource ne sera créée.
Notons que ce comportement (rollback de la stack en cas d’erreur) peut être surchargé (cf [documentation AWS](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stack-failure-options.html)).

Ceci est un aperçu des différentes fonctionnalités offertes par CloudFormation afin d’introduire les extensions de SAM.

Beaucoup de points n’ont pas été abordés dans cette description, on peut par exemple noter : les fonctions de manipulation de chaînes de caractères (ex: Fn::Sub, Fn::GetAtt, etc.), la gestion des Stacks et des StackSets, la détection et la gestion des drifts, le designer, etc.

### SAM : une surcouche de CloudFormation

SAM simplifie la définition des ressources Cloudformation dans un contexte serverless (la définition des ressources est plus compacte car adaptée au contexte). Notons également que les différentes fonctionnalités offertes par CloudFormation restent disponibles avec SAM.

Le template CloudFormation est légèrement modifié avec l’utilisation de SAM, on y retrouve les sections suivantes:

```yaml
Transform: AWS::Serverless-2016-10-31

Globals:
set of globals

Description:
String

Metadata:
template metadata

Parameters:
set of parameters

Mappings:
set of mappings

Conditions:
set of conditions

Resources:
set of resources

Outputs:
set of outputs
```

Les principales différences par rapport au template CloudFormation sont :

- L’ajout de la déclaration **Transform** qui permet à CloudFormation d’identifier que le fichier de template peut contenir des descriptions SAM et que celles-ci doivent être transformées. Ces transformations sont liées aux ressources SAM, plus de détails sont présents dans la suite de l’article.

- La section **Globals** permet la factorisation des propriétés (ex : il est possible de définir ici le runtime des fonctions Lambda. Toutes les ressources de ce type déclarées plus tard hériteront de cette propriété)

Notons également que les différentes ressources CloudFormation sont entièrement utilisables dans un template SAM.

Voici un exemple de template utilisant les deux nouvelles propriétés:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31

Globals:
 Function:
   Timeout: 5
   MemorySize: 256
   Runtime: python3.9

Resources:
 EndpointFct:
   Type: AWS::Serverless::Function
   Properties:
     Handler: app.lambda_handler
     CodeUri: lambda1
     FunctionName: "lambda1Function"
     Environment:
       Variables:
         ENV_VAR: "value"

 GenerateDataFct:
   Type: AWS::Serverless::Function
   Properties:
     Handler: app.lambda_handler
     CodeUri: lambda2
     FunctionName: "lambda2Function"
     Environment:
       Variables:
         ENV_VAR2: "value2"
```

Ici, les deux fonctions Lambda, auront les propriétés **Timeout**, **MemorySize** et **Runtime** communes.

Il est également possible de surcharger les propriétés définies dans la partie **Globals**. Par exemple, la fonction EndpointFct, peut surcharger la propriété Timeout afin de définir une valeur particulière.
Néanmoins, il n’est pas possible de supprimer une propriété commune. Pour plus de précisions sur cette section, je vous invite à consulter la [documentation AWS](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-specification-template-anatomy-globals.html).

### Les ressources SAM
Comme vous avez pu le remarquer sur l’exemple précédent, les fonctions Lambda sont définies avec la ressource `AWS::Serverless::Function`.

Cette ressource fait partie d’un ensemble de ressources mis à disposition par SAM.

Nous pouvons y retrouver les ressources suivantes:

- [`AWS::Serverless::Api`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-api.html)
- [`AWS::Serverless::Application`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-application.html)
- [`AWS::Serverless::Function`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-function.html)
- [`AWS::Serverless::HttpApi`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-httpapi.html)
- [`AWS::Serverless::LayerVersion`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-layerversion.html)
- [`AWS::Serverless::SimpleTable`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-simpletable.html)
- [`AWS::Serverless::StateMachine`](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-resource-statemachine.html)

Notons que ces ressources sont liées à des services serverless d’AWS.

Ces ressources permettant la simplification du fichier de template d’infrastructure-as-code.
En effet, la ressource SAM abstrait une certaine complexité liées aux ressources CloudFormation: pour plusieurs ressources CloudFormation seulement une est nécessaire avec SAM.
En plus de cette réduction de complexité, l’utilisation des ces ressources simplifie la mise en place de certaines bonnes pratiques d’AWS tel que l’utilisation du [least-privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) pour les droits IAM.

Prenons un exemple pour illustrer l'intérêt de ces ressources. Nous souhaitons créer un backend via l’utilisation d’une API REST et d’une base de données NoSQL.
Pour cela, la création d’une API Gateway, d’une base de données DynamoDB et d’une fonction Lambda est nécessaire.

Avec SAM, nous avons besoin de seulement deux ressources !

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Serverless patterns - Amazon API Gateway to AWS Lambda to Amazon DynamoDB
Resources:
 LambdaFunction:
   Type: AWS::Serverless::Function
   Description: 'Lambda function inserts data into DynamoDB table'
   Properties:
     FunctionName: WeatherFunction
     Handler: index.lambda_handler
     Runtime: python3.9
     CodeUri: src/
     Policies:
       DynamoDBCrudPolicy:
         TableName: !Ref DynamoDBTable
     Events:
       ApiEvent:
         Type: Api
         Properties:
           Path: /
           Method: POST
 DynamoDBTable:
   Type: AWS::Serverless::SimpleTable
   Properties:
     TableName: WeatherData
Outputs:
 EndpointUrl:
   Description: 'HTTP REST endpoint URL'
   Value: !Sub 'https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod'
```

Je ne vous copie pas le template CloudFormation associé, car celui-ci serait inutilement long. Mais voici les ressources AWS créées lors du déploiement de cette infra :

![image3-2.png](/assets/img/clouds/tempo/image3-2.png)

Pour rappel, les ressources SAM sont transformées par CloudFormation lors du déploiement. Vous pouvez par ailleurs retrouver sur la console AWS dans le service CloudFormation le template utilisé et le template transformé :

![image1-2.png](/assets/img/clouds/tempo/image1-2.png)

Remarque : Avant de déployer vos ressources via CloudFormation, il est possible de valider le template SAM via la commande : `sam validate`

### Ne réinventez pas la roue avec `serverlessland.com`

Afin de faciliter l’utilisation du serverless, AWS met à disposition un site permettant de trouver différentes informations : les nouveautés, des articles pour l’apprentissage, des snippets et également des exemples de template SAM : https://serverlessland.com/patterns?framework=SAM

N’hésitez pas à y faire un tour !

## Le CLI
Maintenant que nous avons appris comment simplifier votre infrastructure-as-code grâce à SAM, nous avons dorénavant besoin de construire notre application, de la tester et de la déployer.

Ici aussi, SAM a prévu de nous (r)accompagner pour ces étapes ! :)

Note : Ici le terme “application” est utilisé pour représenter une stack cloudFormation où des services AWS sont utilisés conjointement afin de répondre aux besoins. Par exemple : une lambda avec une Api Gateway et une base de données NoSQL.
Notons également que ces différents services sont serverless.

### L’initialisation
Pour la construction d’une application, avant toutes choses, nous avons besoin d’initier un projet.
Pour cela SAM propose la commande : `sam init`

Voici un exemple pour la création d’une lambda en Python avec l’ajout d’une API REST via l’api gateway :

```
sam init --runtime python3.7 --dependency-manager pip --app-template hello-world --name sam-app
```

Certains bootstrap peuvent également être utilisés avec cette commande.
C’est par exemple le cas avec le très bon framework AWS Lambda Powertools:

```
sam init --location https://github.com/aws-samples/cookiecutter-aws-sam-python
```

### Test local
Avant de déployer son application nous souhaitons la tester afin de valider que le comportement correspond à celui attendu.

Avant d’aller plus loin, les artefacts des fonctions doivent être générés. Le build de l’application doit donc être effectué via la commande sam build avant d’utiliser les fonctionnalités suivantes. Cette étape sera détaillée dans une prochaine section.

SAM propose différentes commandes pour tester notre stack avant de la déployer dans AWS:

- `sam local invoke`
Permet d’invoquer une fonction dans un environnement similaire à celui des lambdas. Il est possible d’ajouter en argument un mock de l’event qui sera reçu par la lambda et également de définir des variables d’environnement. Docker doit être installé pour pouvoir utiliser cette commande.
Notons également qu’il est possible de définir un port de debug afin d’utiliser des debugger tel que [ptvsd](https://pypi.org/project/ptvsd/) ou [delve](https://github.com/go-delve/delve).

- `sam local start-api`
Permet d’invoquer une fonction au travers d’une API Gateway locale et ainsi de tester des requêtes et réponses HTTP.
Afin de lier la fonction à l’API Gateway, SAM se réfère au template et trouve les fonctions ayant des événements **HttpAPi** ou **Api**.
Tout comme l’invocation simple d’une fonction, il est possible de définir un port de debug.

- `sam local start-lambda`
Permet d’invoquer une fonction au travers un endpoint créé localement (similaire à celui créé par AWS lors du déploiement d’une lambda).
La commande permet de démarrer un endpoint (par ex: http://127.0.0.1:3001) qui simule le service lambda d’AWS.

- `sam local generate-event`
De manière générale, les lambdas sont invoquées par le déclenchement d’un événement que ce soit, par exemple ,un appel depuis l’API Gateway, un fichier déposé dans un bucket s3 ou d’un message envoyé dans une file sqs.
Cette commande SAM nous permet de générer des événements de test pour ces différents services.

Bien que SAM offre différentes possibilités de tester ses lambdas localement, nous ne les utilisons que très peu dans nos projets.
Nous nous sommes plutôt orientés vers l’utilisation de [moto](https://github.com/spulec/moto) et [mockito](https://github.com/kaste/mockito-python) afin de tester nos lambdas.
Moto est une librairie python permettant de mocker facilement les services AWS (tel que dynamodb, s3, sqs, etc) et mockito est une librairie de mock tout comme son homologue Java.
Ces librairies peuvent aider à l’écriture de tests unitaires, cependant l’élément le plus important est d’avoir un code testable avant tout.

### Construction
Une fois l’application implémentée, nous souhaitons la construire (build) afin d’avoir un packaging déployable et testable.

Pour ce faire, SAM propose deux fonctionnalités:

- `sam build`
- `sam package`

Bien que ces commandes soient relativement proches (elles permettent la création d'artefact) leur utilisation est différente.

`sam build`
Cette commande permet de générer des artefacts pour les lambdas et tout le nécessaire pour déployer notre stack dans AWS.

La liste des ressources ayant besoin d’avoir une construction des artefacts sont les lambda (`AWS::Serverless::Function` ou `AWS::Lambda::Function`) et les layers.

Par défaut, SAM prend en charge la génération des artefacts pour les ressources `AWS::Serverless::Function` et `AWS::Lambda::Function` tandis qu’une métadonnée doit être ajoutée dans le template SAM pour la construction des layers (cf doc AWS [ici](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/building-layers.html)).

Lors de la génération des artefacts, `sam build` gère la gestion des dépendances utilisées lors du runtime de la lambda. Ceci nativement pour les langage suivants: Python 3.6 à 3.9 (Pip), Nodejs 12.x à 16.x (NPM), Ruby 2.7 (Bundler), Java 8 et 11 (Maven ou Gradle), Dotnet ou Go.
Afin de gérer ces dépendances, SAM s’appuie sur un fichier de manifest (par ex: requirements.txt pour Python ou package.json pour nodejs).
Remarque : Ce fichier de manifest peut être surchargé par une option de la commande sam build.

Notons également que les runtimes définis ci-dessus sont gérés nativement par SAM, mais il n’est pas exclu d’utiliser son propre runtime. Pour plus d’informations, une documentation est disponible [ici](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/building-custom-runtimes.html).

`sam build` permet également deux modes de construction des artefacts (à utiliser suivant l’usage): zip ou image Docker.
En effet des limitations sont présentes avec le service lambda d’AWS: la taille maximum d’une archive doit être de 250 MB dézippée.
Une image Docker, quant à elle, ne doit pas être supérieure à 10 GB.

Autre point intéressant avec sam build: l’utilisation de l’option “`--use-container`”. Lors de l’ajout de cette option, SAM va télécharger une image Docker correspondant au runtime de la lambda à construire. Cette option permet alors de générer des artefacts identiques à l’environnement AWS cible.

Voir la [documentation officielle](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-build.html) pour plus de détails.

`sam package`
Contrairement à `sam build`, `sam package` ne gère pas la génération des artefacts avec la gestion des dépendances.
`sam package` se résume à la création d’un package en se basant sur le type du package et de la localisation pour une ressource donnée. Par exemple, pour la ressource `AWS::Serverless::Function`, si le package est une archive zip (propriété PackageType) un package sera créé à partir du répertoire défini par la propriété “codeUri”.

Cette commande est à utiliser après `sam build`, en effet `sam build` permet la construction des artefacts dans un dossier `.aws-sam` et sam package permet la création des archives à partir de ce répertoire et upload ceux-ci dans un bucket S3.
Pour les lambda ayant un package de type image Docker, `sam package` permet l’upload de l’image locale dans ECR.

Notons également que l’étape d’upload peut avoir des impacts sur le fichier de template. Les références des artefacts locaux sont remplacées par ceux uploadé sur S3.

Par exemple, la template suivante (`template.yaml`):

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'

Description: Lambda Hello world

Resources:
 HelloWorldFct:
   Type: 'AWS::Serverless::Function'
   Properties:
     Handler: src/app.lambda_handler
     Runtime: python3.8
     CodeUri: .
     Description: Hello world
     MemorySize: 512
     Timeout: 30
```

est remplacé par (package.yaml) après upload:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Lambda Hello world
Resources:
 HelloWorldFct:
   Type: AWS::Serverless::Function
   Properties:
     Handler: src/app.lambda_handler
     Runtime: python3.8
     CodeUri: s3://bucket-exemple/6080bf6e6ef05db380d65443d3add25d
     Description: Hello world
     MemorySize: 512
     Timeout: 30
   Metadata:
     SamResourceId: HelloWorldFct
```

Voir la [documentation officielle](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-cli-command-reference-sam-package.html) pour plus de détails.

### Déploiement
Dès lors que la construction de nos artefacts, et si nécessaire l’upload, est faite, l’étape de déploiement est à effectuer.
Pour cela une commande est à utiliser: `sam deploy`

Cette commande permet de créer une stack cloudformation, si celle-ci n’est pas existante, et de déployer les différentes ressources décrites dans le template SAM.

Notons qu’il est possible d’utiliser cette commande de plusieurs façons:

- Soit après la commande `sam package` (qui est en charge de l’upload des artefacts sur AWS).
Par exemple avec: `sam deploy --template-file packaged.yaml --stack-name sam-app --capabilities CAPABILITY_IAM`

- Soit directement. Dans ce cas, la commande `sam package` ne sera pas utilisée: l’upload et la génération du template sera effectué par la commande `deploy`.
Le déploiement utilisera un fichier de configuration (par défaut samconfig.toml) permettant de définir différentes propriétés tels que le nom de la stack, le bucket s3 contenant les artefacts, la nécessité de confirmer les deltas avant déploiements, etc.
Afin de créer ce fichier de configuration, il est possible d’utiliser la commande `sam deploy --guided`.

### Monitoring
À partir du moment où l’application est déployée, les opérations de monitoring sont essentielles.

SAM possède deux fonctionnalités nous aidant pour ce besoin: `sam logs` et `sam traces`

`sam logs` permet de récupérer les logs cloudwatch liée à une lambda.
Quant à lui, `sam traces` récupère les traces X-Ray (Pour plus d’information sur ce service, je vous invite à suivre [la documentation suivante](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html)).

Voici un exemple de commande pour récupérer les logs de manière continue pour une stack donnée: `sam logs --stack-name MyStack --tail`

### Nettoyage des ressources déployées
Dans le cadre où nous souhaitons supprimer l’application déployée ainsi que toutes les ressources AWS construires, la commande a utiliser est: `sam delete`

Cette commande permet la destruction de la stack CloudFormation et par conséquent les différentes actions de suppression associé aux ressources AWS construites.

### Mise en place d’une CI/CD
Dorénavant que l’application est testée, packagée et déployée, il nous manque l’automatisation de ces tâches au travers la création d’une chaîne CI/CD.

La commande `sam pipeline` permet de nous aider par rapport à ce besoin.
Cette commande, au travers d’un wizard, créée différents fichiers aidant pour la mise en place d’une chaîne CI/CD. Cette CI/CD peut-être de différents types (avec le template AWS QuickStart):

- Jenkins
- Gitlab CI/CD
- GitHub Actions
- Bitbucket Pipelines
- AWS CodePipeline

Deux commandes sont distincts:

- `sam pipeline bootstrap`
Cette commande permet de créer les ressources AWS nécessaires pour la mise en place de la chaîne CI/CD tel que la création d’un user et de rôles IAM, la création d’un bucket pour les artefacts.

- `sam pipeline init`
Cette commande permet d’initialiser les fichiers de configuration pour la création de la chaîne CI/CD en fonction des outils souhaités (Jenkins, Gitlab CI/CD, etc.). Les ressources nécessaires à la chaîne CI/CD doivent être créés préalablement (soit manuellement soit via le bootstrap)

(A noter qu’il est possible de combiner ces deux commandes en une seule via `sam pipeline init --bootstrap`).

Par défaut, les pipelines construits peuvent dissocier les comptes AWS de staging et de production.

Voici un exemple de pipeline avec le template AWS Quickstart avec CodePipeline:

![pipeline.png](/assets/img/clouds/tempo/pipeline.png)

Ici, 6 steps sont présentes: le checkout des sources, la mise à jour du pipeline, l’exécution des tests unitaires, la génération des archives et l’upload de ceux-ci pour le compte de staging et prod (grâce aux commande build et package vus précédemment), le déploiement et l'exécution des tests d’intégration sur l’environnement de staging et finalement avec une validation manuelle le déploiement en production.

Un autre exemple de pipeline avec Gitlab CI/CD est disponible ici

### Accélérer le feedback des déploiements
Une autre fonctionnalité intéressante de SAM est `sam sync`.
Cette commande permet d'accélérer le déploiement des ressources AWS.

Cette accélération peut-être de deux types:

- L’utilisation des API AWS à la place de la mise à jour via CloudFormation
- La combinaison des commandes vues précédemment pour builder, générer le package et déployer.

Par exemple, pour le premier cas, la commande `sam sync --stack-name <stack_name> --code`, gère uniquement le delta au niveau du code et utilisera les API AWS pour mettre à jour les éléments cibles (lambda, layers, step-function ou API Gateway).

Pour le second cas, la commande `sam sync --stack-name <stack_name> --watch va build`, générer le package et déployer.

Notons également la présence de l’option “`--watch`”. Cette option va permettre de surveiller les modifications réalisées dans le workspace afin d'appliquer les modifications réalisées.

Ces modifications peuvent être de deux types: infrastructure ou code.
Les modifications d’infrastructure (`template.yaml`) provoquera la mise à jour de la stack cloudformation tandis que la modification de code passera directement par les API AWS.

### Publier son application SAM
Si vous souhaitez partager votre application, vous pouvez la publier dans le service Serverless Application Repository (SAR) de votre compte AWS.
Il vous sera possible de définir la visibilité de votre application en public afin de la partager aux autres utilisateurs d’AWS.

Afin de réaliser ces actions, la commande `sam publish` sera utile.

Pour plus d’informations sur le sujet, je vous invite à consulter la documentation https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-publishing-applications.html

## Quelques limitations
SAM, pour la partie construction, se repose sur certaines conventions.
Par exemple, pour python, seulement l’utilisation de pip comme gestionnaire de dépendance est possible nativement. Ce qui engendre des développements spécifiques pour des projets utilisant d’autres outils (ex: poetry).
Cette difficulté peut également être présente pour des langages non pris en charge actuellement.

Une autre difficulté que l’on a pu rencontrer est l’intégration de SAM avec d’autres outils. Dans notre cas, il n’était pas possible de déployer une application SAM dans LocalStack car le CLI SAM ne permettait pas de spécifier l’endpoint de LocalStack nativement.

## Pour aller plus loin
[CDK est maintenant compatible avec SAM](https://docs.aws.amazon.com/fr_fr/serverless-application-model/latest/developerguide/serverless-cdk-getting-started.html), ce qui permet de “remplacer” le template YAML par une stack CDK. Il est donc possible d’utiliser son langage de programmation préféré pour développer son infrastructure et profiter de toutes les commandes offertes par le CLI SAM.

## Conclusion
Lors de ce tour d’horizon des fonctionnalités SAM, nous avons pu voir les nombreuses fonctionnalités de l'outil.
Ainsi, si vous souhaitez mettre en œuvre une application serverless simple, SAM permet un bootstrap très facile et un time to market très faible. Nous pouvons également noter la grande variété des commandes permettant d’aider pour toutes les étapes de l’application (du développement jusqu’au monitoring) ainsi qu’une grande intégration à l'écosystème AWS. Ces nombreuses fonctionnalités permettent de réduire le boucle de feedback lors de l’étape de développement, accélérant ainsi cette dernière.
Néanmoins, l’utilisation de SAM reste très fortement liée à des applications serverless. Pour des projets plus complexes ou utilisant des services non serverless l’utilisation du CDK et de CloudFormation est à privilégier.

Source : https://blog.ippon.fr/