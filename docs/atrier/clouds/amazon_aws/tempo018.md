---
title: Comment utiliser AWS APPSYNC pour générer rapidement un backend pour nos applications MOBILES - WEB via CDK
description: 
published: true
date: 2023-06-21T20:58:25.028Z
tags: 
editor: markdown
dateCreated: 2023-06-21T20:38:33.429Z
---

# Comment utiliser AWS APPSYNC pour générer rapidement un backend pour nos applications MOBILES / WEB via CDK

![appsyncpicture.jpg](/assets/img/clouds/tempo/appsyncpicture.jpg)

Lorsque l’on développe des applications web mobiles, on doit souvent se soucier de la gestion des serveurs backend (gestion des connexions, disponibilité des données, sécurité etc…). Dans un contexte de besoin de productivité et de rapidité des tests/déploiements, il serait idéal de pouvoir déléguer la majeure partie de la gestion du backend à un fournisseur de services Web tels que AWS et ainsi se focaliser sur la logique métier de l’application.

C’est là qu’intervient **AWS AppSync**.

## Un mot sur GraphQL
**GraphQL**, pour Graph Query Language, est une spécification de description de données qui propose une alternative aux services REST traditionnels dans un environnement d’API créé par Facebook en 2012, avant d'être publié comme projet open-source en 2015.

La requête du client définit une structure de données, dont le stockage est éventuellement distribué, et le serveur suit cette structure pour retourner la réponse.

On peut distinguer trois grands principes.

- Premièrement,  l’accès aux données en contexte GraphQL s’effectue à partir d’un point d’entrée unique, qui se termine par convention en `/graphql`. Avec une API REST, typiquement,  les données sont collectées en accédant à plusieurs points de terminaison.
- Deuxièmement, les requêtes GraphQL sont encapsulées dans la méthode HTTP POST quel que soit le type de requête, là où REST utilise l’ensemble des méthodes HTTP (GET, POST, HEAD, etc.) pour préciser la nature de la requête.
- Enfin, le point fort de GraphQL réside dans la possibilité de s'abstraire de toute information superflue. En d'autres mots, en indiquant dans la requête passée à GraphQL les informations souhaitées en réponse, ce dernier est capable de retourner uniquement les informations demandées. Les serveurs REST classiques retournent l'ensemble des informations des objets relatives à une requête, et ce, même si elles ne sont d'aucune utilité pour l'utilisateur.

![image4.png](/assets/img/clouds/tempo/aws/image4.png)

## AppSync
[AWS AppSync](https://aws.amazon.com/appsync/) est un backend GraphQL serverless pour des applications mobile et web. Ce service managé de AWS propose un service complet de gestion d’API GraphQL pouvant se connecter à différentes **datasources** en même temps au sein de leur environnement AWS.

AppSync possède son propre API GATEWAY (endpoint unique d’entrée des requêtes GraphQL), son propre système d’authentification (en passant par une API key, ou encore en s’associant au service AWS Cognito) ainsi que son propre système de **datasources** et **resolvers**.

Ce schéma montre un peu la structure d’une application mobile utilisant AppSync possédant une datasource DynamoDB (base de données NoSQL), et une datasource qui passe par une lambda qui va faire des requêtes HTTP vers une marketPlace quelconque :

![image1.png](/assets/img/clouds/tempo/aws/image1.png)

La requête GraphQL va déterminer vers quelle datasource AppSync se diriger, et appliquer ses resolvers.

On peut donc se servir de AppSync pour gérer notre serveur backend et se concentrer sur le code métier.

On suppose ici que l’on dispose déjà d’un compte AWS configuré.

2 possibilités s’offrent à nous pour déployer une infrastructure AppSync :

- Directement via la console : laborieux et surtout peu évolutif
- Via de l’infra as code : permet de déployer toute l’infra à partir de code de conf. (Terraform étant sans doute le plus connu)

Dans cet article nous allons nous pencher sur la deuxième option et on va pour cela utiliser **AWS CDK** (Cloud Development Kit) : c’est un cadre de développement logiciel open source conçu pour définir les ressources de nos applications cloud à l'aide de langages de programmation courants. Cela nous évite de passer ainsi par des fichiers de conf en YAML, JSON ou autres et d’utiliser des langages de programmation du quotidien.

De plus, CDK permet d’écrire des tests sur l’infrastructure ce qui s’inscrit dans la lignée des bonnes pratiques de programmation.


## Initialisation
Maintenant que nous avons en tête tous les concepts de base, il va s'agir de déployer une instance AppSync sur AWS en utilisant CDK.

Dans cet exemple nous allons utiliser CDK avec TypeScript un langage fortement typé.

Tout d’abord il faut installer nodeJS. On peut utiliser nvm pour cela (https://github.com/nvm-sh/nvm).

```shell
nvm install 16.15.1
```

Ensuite installer TypeScript et ESLint :

```shell
npm install -g --force typescript@">=4.6.3 <4.7.0"
npm install -g --force eslint@">=7.0.0 <8.0.0"
```

Installer la dernière version de AWS CLI (https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html).

Ensuite il faut créer un IAM user avec les permissions nécessaires

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": [
        "iam:*",
        "secretsmanager:*",
        "s3:*",
        "cloudformation:*",
        "cloudwatch:*",
        "dynamodb:*",
        "ec2:*",
        "lambda:*",
        "ram:*",
        "ssm:*",
        "ecr:*",
        "appsync:*",
        "execute-api:*"
      ],
      "Resource": "*"
    }  ]
}
```

Une fois l’user créé il s'agit de récupérer l'access key et le secret access key et des les ajouter à la configuration du CLI :

```shell
$ aws configure 
AWS Access Key ID [None]: <your acces key >
AWS Secret Access Key [None]: <your secret key>
Default region name [None]: < your region>
Default output format [None]: json
```

Idéalement, ajouter ces clés dans nos variables d'environnement

```shell
export CDK_DEFAULT_ACCOUNT=<your aws account id (ex: 123456789)>
export BACKEND_API_AWS_ACCES_KEY_ID=<your aws account acces key (ex: AJOAWYI2CTERW4SVMJAA)>
export BACKEND_API_AWS_REGION=<your default region (ex: eu-west-3)>
export BACKEND_API_AWS_SECRET_ACCES_KEY=<aws account secret key>
```

Ensuite il faut installer CDK. Ce qui donne avec CDK CLI :

```shell
npm install -g aws-cdk
```

Et Bootstrap CDK :

```shell
cdk bootstrap "aws://$CDK_DEFAULT_ACCOUNT/$CDK_DEFAULT_REGION"
```

Voir ici pour plus de détails : https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html

Nous avons tous les prérequis pour commencer à faire de l’infra as code avec CDK et TypeScript.


## CDK & AppSync
Avant de commencer voici quelques commandes qui nous seront utiles dans le projet.

Nous allons créer un  projet avec CDK et TypeScript.

Le fichier `cdk.json` indique à  CDK Toolkit comment exécuter notre application.

**commandes utiles**

- `npm install` Télécharge et installe nos dépendances
- `npm run build` et  compile TypeScript en JavaScript
- `npm run test` lance les tests unitaires avec Jest
- `cdk deploy <nom de la stack>` déploie la stack sur le compte AWS par défaut
- `cdk destroy BackendApiStack` supprime la stack du compte  AWS par défaut
- `cdk synth BackendApiStack` renvoie le template CloudFormation synthétisé

Dans le dossier `/src` de notre projet TypeScript nous allons créer un dossier dédié à l’infrastructure du projet nommé `/infrastructure/cdk`  et un dossier `/runtime` qui sera notre "Main" là où on va utiliser les ressources déclarées dans nos stacks.

Dans le dossier `/runtime` on va créer une classe `Main.ts` :

```typescript
const BackendCdkApp = new BackendApiApp(configuration);
 
export default BackendCdkApp;
```

Les bonnes pratiques CDK préconisent de créer des sous-dossiers pour chaque ressource de la stack et de gérer chaque ressource séparément pour un projet plus propre et lisible. Dans cet exemple, vu que l’on ne va pas détailler certaines ressources, on va se contenter de tout mettre directement dans la stack.

Nous allons créer dans le dossier `/infrastructure` la classe `BackendApiApp.ts` qui va contenir notre stack.

```typescript
import { App } from 'aws-cdk-lib';
import BackendApiStack from '../stack/BackendApiStack';
import Configuration from '../../../shared/configurations/Configuration';
 
export default class BackendApiApp extends App {
  public backendApiStack: BackendApiStack;
 
  public constructor(configuration: Configuration) {
    super();
 
    this.createBackendApiStack(configuration);
  }
 
  private createBackendApiStack(configuration: Configuration): void {
    this.backendApiStack = new BackendApiStack( this, 'BackendApiStack', {
      stackName: 'backend-api-stack',
      env: {
        account: configuration.awsAccount,
        region: configuration.awsRegion,
      },
    });
  }
}
```

La classe `Configuration.ts` est une classe qui va contenir nos variables d’environnement afin de ne pas écrire directement les paramètres des différentes ressources en plus de rendre la configuration facilement évolutive. On pourra mettre cette classe dans un dossier `/shared/configurations/`

```typescript
export default class Configuration {
  // eslint-disable-next-line no-use-before-define
  protected static instance: Configuration;
 
  public application: string;
  public awsAccount: string;
  public awsRegion: string;
  
  
  protected constructor() {
    this.application = process.env.BACKEND_API_APP!;
    this.awsAccount = process.env.BACKEND_API_AWS_ACCOUNT!;
    this.awsRegion = process.env.BACKEND_API_AWS_REGION!;
  }
}
```

Maintenant il ne reste plus qu'à créer la classe qui va contenir nos ressources. On va créer un dossier `infrastructure/cdk/ressource` et créer la classe `BackendApiStack.ts`

```typescript
import { App, Stack, StackProps } from 'aws-cdk-lib';
import Configuration from '../../../shared/configurations/Configuration';
 
export default class BackendApiStack extends Stack {
 
  public constructor(configuration: Configuration, app: App, id: string, props?: StackProps) {
    super(app, id, props);
  }
}
```

Il ne reste plus qu'à créer les ressources voulues. La documentation de CDK donne un échantillon de tout ce qu’il est possible de créer : https://docs.aws.amazon.com/cdk/api/v2/docs/aws-construct-library.html


On va tout d'abord créer un AWS AppSync :

```typescript
import { App, Stack, StackProps } from 'aws-cdk-lib';
import Configuration from '../../../shared/configurations/Configuration';
import * as appsync from '@aws-cdk/aws-appsync-alpha';
 
export default class BackendApiStack extends Stack {
  private appsyncApi: appsync.GraphqlApi;
 
  public constructor(configuration: Configuration, app: App, id: string, props?: StackProps) {
    super(app, id, props);
 
    this.appsyncApi = new appsync.GraphqlApi(stack, 'AppsyncApi', {
      name: `${configuration.application}-appsync-api`,
      schema: appsync.Schema.fromAsset(path.join(__dirname, '../../../../../resources/graphql/schemaPokemons.graphql')),
      authorizationConfig: {
        defaultAuthorization: {
          authorizationType: appsync.AuthorizationType.API_KEY,
        },
      },
      xrayEnabled: true,
    });
  }
}
```

Voilà… c’est aussi simple que cela. Nous avons une stack qui crée une ressource AppsSync à partir d’un schéma GraphQL auquel on a accès via une API key.

On va maintenant ajouter à notre AppSync une datasource.

Pour la suite l’exemple on va supposer que le schéma GraphQL est le suivant :

![image8.png](/assets/img/clouds/tempo/aws/image8.png)

schemaPokemons.graphql

On va pouvoir créer une ressource `dynamodb` et indiquer à AppSync que la query `getPokemons()` doit pointer vers cette base et appliquer son `resolver` :

```typescript
import { App, Stack, StackProps } from 'aws-cdk-lib';
import Configuration from '../../../shared/configurations/Configuration';
import * as appsync from '@aws-cdk/aws-appsync-alpha';
import * as dynamodb from 'aws-cdk-lib/aws-dynamodb';
 
 
export default class BackendApiStack extends Stack {
  private appsyncApi: appsync.GraphqlApi;
  private dynamoDbTable: dynamodb.Table;
 
  public constructor(configuration: Configuration, app: App, id: string, props?: StackProps) {
    super(app, id, props);
 
    this.dynamoDbTable = new dynamodb.Table(stack, 'PokemonTable', {
      partitionKey: {
        name: 'id',
        type: dynamodb.AttributeType.STRING,
      },
      sortKey: {
        name: 'created_at',
        type: dynamodb.AttributeType.STRING,
      },
      billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
      removalPolicy: RemovalPolicy.RETAIN,
    });
 
    this.appsyncApi = new appsync.GraphqlApi(stack, 'AppsyncApi', {
      name: `${configuration.application}-appsync-api`,
      schema: appsync.Schema.fromAsset(path.join(__dirname, '../../../../../resources/graphql/schemaPokemons.graphql')),
      authorizationConfig: {
        defaultAuthorization: {
          authorizationType: appsync.AuthorizationType.API_KEY,
        },
      },
      xrayEnabled: true,
    });
  }
 
  const datasource = this.appsyncApi.addDynamoDbDataSource('dynamoDbDataSource', this.dynamodDb.dynamoDbTable);
  datasource.createResolver({
    typeName: 'Query',
    fieldName: getPokemons,
    requestMappingTemplate: appsync.MappingTemplate.dynamoDbScanTable(),
    responseMappingTemplate: appsync.MappingTemplate.dynamoDbResultList(),
  });
}
```

On vient d’ajouter une datasource pour la query `getPokemon`. Lorsque l’appel GraphQL va être effectué, AppSync va déterminer vers quelle datasource on va se diriger et appliquer un resolver sur ce dernier. `requestMappingTemplate` va mapper la requête pour effectuer un scan sur la base de données DynamoDB et `responseMappingTemplate` va renvoyer la réponse au bon format sous forme de liste de `Pokemon`.
En seulement quelques lignes on a créé un serveur backend pour notre application web / mobile qui va proposer une API permettant de requêter sur des éléments dans une base de données NoSQL.

On peut évidemment pousser la configuration de AppSync. En le situant dans un **VPC** (Virtual Private Cloud) par exemple ou encore en multipliant les datasources ( `lambdas`, `elasticsearch`, `opensearch`) . On peut aussi améliorer l’authentification. Au lieu de passer par une API key on peut utiliser Cognito et s’authentifier via des utilisateurs vérifiés.


On peut maintenant écrire des tests sur nos classes avec **Jest**. L’approche purement **TDD** n’est pas évidente pour les ressources car on ne sait pas à l’avance ce qui va être créé :

```typescript
describe('BackendApiStack', () => {
  describe('BackendApiStack.constructor()', () => {
    const configuration = ConfigurationMock.getInstance();
 
    const app = new App();
    const stack = new Stack(app, 'TestStack', {
      env: { account: configuration.awsAccount, region: configuration.awsRegion },
    });
    
    // THEN
    const template = Template.fromStack(stack);
 
    it('should create the AppSync Api, the dynamoDB  and the DynamoDBDataSource', () => {
      template.hasResourceProperties('AWS::AppSync::GraphQLApi', {
        AuthenticationType: 'AMAZON_COGNITO_USER_POOLS',
        Name: `appsync-api-${configuration.environment}`,
        Tags: [
          {
            Key: 'application',
            Value: 'backend-api',
          },
          {
            Key: 'environment',
            Value: configuration.environment,
          },
          {
            Key: 'version',
            Value: '1.0.0',
          },
        ],
        UserPoolConfig: {
          AwsRegion: configuration.awsRegion,
          DefaultAction: 'ALLOW',
          UserPoolId: configuration.cognitoUserPoolId,
        },
      });
 
      template.hasResourceProperties('AWS::DynamoDB::DataSource', {
        Name: datasourceName,
        Type: 'AWS_LAMBDA',
      });
 
      template.hasResourceProperties('AWS::DynamoDB::Resolver', {
        FieldName: resolverFieldName,
          TypeName: resolverTypeName,
          DataSourceName: datasourceName,
        });
      },
    );
  });
});
```

On peut déployer cette infra avec `cdk deploy` et avec `cdk synth` on peut avoir un récapitulatif de toutes les ressources qui vont être créées. Cela va aussi nous permettre d’écrire les tests associés car nous saurons ce qu’il va être créé dans un premier temps.

Une fois qu’on accepte les changements et qu'on procède au déploiement on peut suivre celui-ci dans le terminal :

![image6.png](/assets/img/clouds/tempo/aws/image6.png)

Et si on regarde dans CloudFormation on peut voir notre stack qui va être créée :

![image2.png](/assets/img/clouds/tempo/aws/image2.png)

On peut aussi vérifier dans l’interface AppSync de la console AWS que notre ressource a bien été créée et configurée comme dans le code CDK :

![image5.png](/assets/img/clouds/tempo/aws/image5.png)

Le schéma à été chargé ainsi que les résolveurs correspondant à la dataSource DynamoDB et associés à la query `getPokemons`.

Dans les paramètres de la console on peut récupérer l'endpoint GraphQL fourni par AppSync ainsi que l’API Key permettant de s'authentifier auprès du service :

![image3.png](/assets/img/clouds/tempo/aws/image3.png)

On peut ainsi se servir d’un outil comme POSTMAN pour tester nos appels GraphQL avant des les intégrer au front de notre application :

![image7.png](/assets/img/clouds/tempo/aws/image7.png)

## Conclusion
En conclusion, **AWS AppSync** nous offre les avantages suivants :

- Faire le lien avec le contrat d’interface GraphQL établi entre le front et le back de l’application
- Automatiser la gestion des datasources et des résolveurs des requêtes GraphQL
- Gérer l’authentification, que ce soit via une API key ou un service d’authentification comme AWS Cognito
- Faire l’abstraction de pas mal de briques d’architecture backend comme la gestion du trafic, la redirection des requêtes etc…
  
source : https://blog.ippon.fr/