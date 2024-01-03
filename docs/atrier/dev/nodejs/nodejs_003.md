---
title: Comment mettre en place un projet AWS lambda en Node.js
description: 
published: true
date: 2023-06-21T20:33:37.607Z
tags: 
editor: markdown
dateCreated: 2023-06-21T20:07:36.972Z
---

# Comment mettre en place un projet AWS lambda en Node.js

![nodejs_plus_lambda-2.jpg](/assets/img/developpement/tempo/nodejs_plus_lambda-2.jpg)

AWS Lambda est un service de calcul sans serveur proposé par Amazon. Il permet d’effectuer des traitements par déclenchement d’évènements et permet aussi de s’abstraire du besoin de gérer l’allocation et la gestion des serveurs. J’ai pu remarquer cependant que la plupart des guides et tutoriaux traitant du sujet ne montraient pas comment mettre en place la structure d’un tel projet. Je vous propose donc dans cet article de vous montrer pas à pas comment mettre en place un projet AWS Lambda en Node.js (ou même un projet Node.js en général) complet. On se concentrera plus particulièrement sur l’organisation du code et les divers outils que je pense indispensables.

## De bons outils pour bien travailler
Le service AWS Lambda supporte différents runtime (environnements d’exécutions) mais nous allons ici nous concentrer sur le développement d’une lambda en Node.js et mettre en place les outils suivants :

- [Typescript](https://www.typescriptlang.org/) pour avoir du typage statique et ainsi aider à réduire les erreurs durant le développement
- [Webpack](https://webpack.js.org/) pour générer la distribution de l’application
- [Eslint](https://eslint.org/) + [Prettier](https://prettier.io/) pour l’analyse statique du code et le formatage
- [Jest](https://jestjs.io/fr/) comme framework de test
- [Localstack](https://github.com/localstack/localstack) pour émuler les services AWS lors des tests car dans la plupart des cas les lambdas sont utilisées pour communiquer avec d’autres services AWS
- [Sonar](https://www.sonarqube.org/) pour vérifier la qualité de code
- [Gitlab CI](https://docs.gitlab.com/ee/ci/) pour l’intégration continue

## Initialiser le projet
Nous allons développer une application sur la dernière version disponible de Node.js sur AWS Lambda qui est à ce jour la version 14.

Je vous conseille donc d’installer Node.js 14 avec nvm, cela vous permettra de gérer facilement les versions et de passer d’une version à l’autre très simplement.

Plutôt que d’utiliser npm en tant que package manager (gestionnaire de paquets), je vous propose dans cet article Yarn. Vous pouvez l’installer en lançant la commande suivante :

```shell
npm install -g yarn
```

Nous allons initialiser notre projet en générant un `package.json`, il s’agit d’un fichier JSON contenant les métadonnées du projet et permet la gestion des dépendances, des scripts et du versioning :

```shell
cd lambda-project
yarn init -y # Libre à vous de ne pas ajouter l'option -y pour définir certaines valeurs via le CLI
```

Pour l’instant, nous allons juste créer un fichier dans le chemin `src/handler.ts` avec le contenu suivant :

```typescript
export function handler() {
 return 'Hello World!';
}
```

Vous devez vous retrouver à la fin de cette étape avec l’arborescence suivante :

```
.
├── src
    └── handler.ts
└── package.json
```

## Installer TypeScript
Après avoir initialisé le projet, nous allons mettre en place TypeScript. Je ne vais pas m’attarder sur les nombreux avantages qu’il procure par rapport à JavaScript mais celui-ci propose du typage statique qui permet de réduire le nombre d’erreurs (mais pas toutes) lors du développement d’applications et l’intégration de divers plugins dans les IDE aident grandement à travailler plus efficacement pour les développeurs.

Pour installer TypeScript, exécutez simplement ces commandes :

```shell
yarn add --dev typescript @tsconfig/node14
touch tsconfig.json # fichier contenant la configuration de Typescript 
```

Dans le fichier `tsconfig.json`, vous pouvez ajouter ces lignes :

```json
{
  "extends": "@tsconfig/node14/tsconfig.json", // 1
  "compilerOptions": {},                       // 2
  "include": ["src"],                          // 3
  "exclude": ["node_modules"]                  // 4
}
```

Les différentes lignes permettent :

1. de charger une configuration existante, ici nous utilisons la configuration recommandée pour notre version de Node.js.
1. de choisir les [options de compilation](https://www.typescriptlang.org/tsconfig) telles que le dossier de sortie de la transpilation `outDir` ou encore `noImplicitAny` pour autoriser ou non l’utilisation du type `any` dans le code.
1. d’inclure le dossier `src` dans le programme.
1. d’ignorer le dossier  `node_modules`.

Finalement pour transpiler votre code, il suffit simplement de lancer la commande :

```shell
npx tsc
```

Le fichier généré se trouvera dans `src/handler.js` ce qui n’est pas idéal. Vous pouvez ajouter dans les `compilerOptions` le chemin de sortie pour ne pas mélanger votre code source et le build (distribution). Dans notre cas, cela ne va pas nous poser de problème car nous allons utiliser Webpack pour générer notre distribution.

A la fin de cette étape vous devez vous retrouver avec cette arborescence au sein de votre projet :

```shell
.
├── node_modules/*
├── src
    └── handler.ts
├── tsconfig.json
└── package.json
```

## Générer la distribution avec Webpack
Webpack est un outil puissant et complet qui va nous permettre ici de builder et de packager  notre Lambda. Nous allons l’utiliser avec `zip-webpack-plugin` qui est un plugin permettant d’archiver la distribution.

Vous pouvez les installer en exécutant cette commande :

```shell
yarn add --dev webpack webpack-cli zip-webpack-plugin ts-loader
```

Webpack peut être configuré de différentes manières, ici nous allons juste créer le fichier `webpack.config.js` et y insérer ce code :

```javascript
/* eslint-disable @typescript-eslint/no-var-requires */
const path = require("path");
const ZipPlugin = require("zip-webpack-plugin");

module.exports = {
  entry: "./src/handler.ts",                        // 1
  target: "node",                                   // 2
  devtool: "source-map",                            // 3
  module: {
	rules: [
  	{
    	test: /\.tsx?$/,
    	use: "ts-loader",                             // 4
    	exclude: /node_modules/,
  	},
	],
  },
  resolve: {
	extensions: [".tsx", ".ts", ".js"],
  },
  output: {
	filename: "lambda.js",                        // 5
	libraryTarget: "commonjs",                    // 6
	path: path.resolve(__dirname, "dist"),        // 7
  },
  plugins: [
	new ZipPlugin({
  	filename: "lambda.zip",                       // 8
	}),
  ],
};
```

Les différentes lignes permettent d’indiquer :

1. à Webpack le point d’entrée du projet pour construire la distribution.
1. à Webpack quel est l'environnement cible.
1. quel type de source map on souhaite ou aucun.
1. quel loader utiliser pour la transformation du code source, ici, tous les fichiers finissant par `.ts` sont transformés par `ts-loader`.
1. le fichier de sortie.
1. le format du module de sortie. Il est à noter que Webpack n’expose pas les exports par défaut. Si vous ne spécifiez pas ce champ, il est possible que le fichier de sortie soit vide.
1. le dossier de sortie.
1. que nous souhaitons utiliser un plugin. Ici, nous utilisons un plugin permettant d’archiver le contenu du dossier de sortie.

Vous pouvez ensuite ajouter ces lignes dans le fichier `package.json` :

```json
...
"scripts": {
	"build": "webpack --mode=production",
	"build:watch": "webpack --mode=development --watch"
}
...
```

La distribution peut maintenant être générée en lançant simplement la commande :

```shell
yarn build
```

La distribution sera générée et archivée dans le dossier `dist`.

Il est aussi possible de la générer automatiquement à chaque modification de votre code en lançant la commande suivante :

```shell
yarn build:watch
```

Cette commande est très utile dans le cas où vous développez une librairie qui est utilisée dans une autre application et que vous souhaitez tester directement les changements. Vous pouvez lier des paquets avec [yarn link](https://classic.yarnpkg.com/en/docs/cli/link).

A la fin de cette étape vous devez vous retrouver avec cette arborescence au sein de votre projet :

```shell
.
├── node_modules/*
├── dist
    ├── lambda.js
    ├── lambda.js.map
    └── lambda.zip
├── src
    └── handler.ts
├── package.json
├── tsconfig.json
└── webpack.config.js
```

## Petit mot sur les source maps
En passant par Webpack le code source est à la fois combiné en seul fichier et minifié. Dans cette situation lorsqu’une erreur apparaît pendant l’exécution de la lambda ou tout autre application, il est très difficile de débugger.

Prenons cet exemple :

```typescript
export function handler() {}

let error;
(error as any).undefined;
```


Si l’on génère la distribution et exécute le code, on obtient cette erreur presque illisible :

```shell
/home/lcso/workspace/lambda-project/dist/lambda.js:1
(()=>{"use strict";var e={};(()=>{var r=e;Object.defineProperty(r,"__esModule",{value:!0}),r.handler=void 0,r.handler=function(){},(void 0).undefined})();var r=exports;for(var d in e)r[d]=e[d];e.__esModule&&Object.defineProperty(r,"__esModule",{value:!0})})();
                                                                                                                                        	^

TypeError: Cannot read properties of undefined (reading 'undefined')
	at /home/lcso/workspace/lambda-project/dist/lambda.js:1:141
	at /home/lcso/workspace/lambda-project/dist/lambda.js:1:152
	at Object.<anonymous> (/home/lcso/workspace/lambda-project/dist/lambda.js:1:258)
	at Module._compile (node:internal/modules/cjs/loader:1103:14)
	at Object.Module._extensions..js (node:internal/modules/cjs/loader:1155:10)
	at Module.load (node:internal/modules/cjs/loader:981:32)
	at Function.Module._load (node:internal/modules/cjs/loader:822:12)
	at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:77:12)
	at node:internal/main/run_main_module:17:47
```

C’est pour cela qu’il est possible de générer un fichier qui fait le mapping entre le code de production et le code source, il s’agit du source map. Pour l’activer, rien de plus simple que de l’ajouter dans les options de Node.js. Son activation se fait par la commande `export NODE_OPTIONS=--enable-source-maps`.

Nous obtenons ensuite des retours beaucoup plus simples à comprendre.

```typescript
webpack://lambda-project/src/handler.ts:7
error.undefined;
  	^

TypeError: Cannot read properties of undefined (reading 'undefined')
	at undefined (webpack://lambda-project/src/handler.ts:7:7)
	at undefined (webpack://lambda-project/src/handler.ts:7:7)
	at Object.<anonymous> (webpack://lambda-project/src/handler.ts:7:7)
	at Module._compile (node:internal/modules/cjs/loader:1103:14)
	at Object.Module._extensions..js (node:internal/modules/cjs/loader:1155:10)
	at Module.load (node:internal/modules/cjs/loader:981:32)
	at Function.Module._load (node:internal/modules/cjs/loader:822:12)
	at Function.executeUserEntryPoint [as runMain] (node:internal/modules/run_main:77:12)
	at node:internal/main/run_main_module:17:47
```

## Avoir un code au propre
Dans cette partie, nous allons mettre en place ESlint pour effectuer une analyse statique et trouver d'éventuels problèmes dans notre code qui ne respecte pas certaines règles et aussi ajouter Prettier. Prettier est un formateur de code qui permet de forcer un style d’écriture et garder une certaine consistance dans la forme du code.

### ESlint
Pour installer ESlint, vous pouvez exécuter ces commandes :

```shell
yarn add eslint @typescript-eslint/eslint-plugin @typescript-eslint/parser --dev
yarn run eslint --init
```

Une fois la configuration faite via le CLI, vous devez vous retrouver avec le fichier `.eslintrc.json` (si vous avez choisi une configuration via JSON) :

```json
{
  "env": {
	"commonjs": true,
	"es2021": true,
	"node": true
  },
  "extends": [
	"eslint:recommended",
	"plugin:@typescript-eslint/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
	"ecmaVersion": "latest"
  },
  "plugins": [
	"@typescript-eslint"
  ],
  "rules": {}
}
```

Vous pouvez ensuite ajouter les scripts suivants dans le `package.json` :

```json
...
"scripts": {
	"lint": "eslint src/**/*.ts",
	"lint:fix": "eslint src/**/*.ts --fix"
}
...
```

Il suffit simplement de lancer la commande `yarn lint` ou `yarn lint --fix` pour lancer ESlint.

### Prettier
Pour installer Prettier, vous pouvez exécuter ces commandes :

```shell
yarn add prettier eslint-config-prettier eslint-plugin-prettier --dev
```

Les paquets suivants permettent :

- `eslint-config-prettier` : de désactiver les règles qui peuvent entrer en conflit entre ESlint et Prettier
- `eslint-plugin-prettier` : de lancer Prettier en tant que règle ESlint

La configuration de Prettier se fait ensuite dans le fichier `.prettierrc` à la racine du projet. Vous pouvez l’initialiser avec ce contenu :

```json
{
  "semi": true,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 120,
  "tabWidth": 2
}
```

Il suffit simplement de lancer la commande `yarn lint --fix` pour formater avec Prettier.

Vous devez avoir à la fin de cette étape une arborescence comme ceci :

```shell
.
├── node_modules/*
├── dist
    ├── lambda.js
    ├── lambda.js.map
    └── lambda.zip
├── src
    └── handler.ts
├── .eslintrc.json
├── .prettierrc
├── package.json
├── tsconfig.json
└── webpack.config.js
```

## Automatisation
Maintenant que vous avez ESlint et Prettier mis en place, il serait dommage d’oublier de lancer ces outils avant de commiter. Je vous propose ici d’automatiser cela pour appliquer ces outils lors du pré-commit avec Lint-staged et Husky.

### Lint staged
Au fur et à mesure que le projet grossit, le nombre de fichiers augmente. Exécuter ESlint sur tout votre projet n’est pas forcément judicieux. Lint-staged permet d'exécuter des Linters sur les fichiers qui sont dans le “git stage”, ce qui va nous permettre d’appliquer uniquement ESlint sur les modifications que nous souhaitons commit.

Pour l’installer, il suffit de lancer la commande :

```shell
yarn add lint-staged --dev
```

Vous pouvez ensuite configurer lint-staged dans `package.json` pour exécuter ESlint sur les fichiers TypeScript et Prettier sur les autres fichiers comme ceci :

```json
...
"scripts": { ... },
...
"lint-staged": {
    "*.ts": "eslint --fix",
    "*.{js,json,css,scss}": "prettier --write"
}
...
```

### Husky
Husky est un outil permettant d’utiliser très facilement les [Git Hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks). Nous allons nous en servir pour lancer Lint-staged avant chaque commit.

Pour l’installer, il suffit de lancer la commande :

```shell
yarn add husky --dev
```

Ajoutez le script suivant dans le `package.json` :

```json
...
"scripts": {
    "prepare": "husky install"
}
...
```

Cela permettra d’installer Husky lors de yarn install. Vous pouvez ensuite exécuter la commande :

```shell
yarn prepare
```

Vous pouvez enfin ajouter lint-staged en action de pré-commit par cette commande :

```shell
npx husky add .husky/pre-commit "npx --no lint-staged"
```

Maintenant si vous essayez de commit vos changements, Lint-staged sera lancé et vous aurez un résultat comme celui-ci :

```shell
✔ Preparing lint-staged...
✔ Running tasks for staged files...
✔ Applying modifications from tasks...
✔ Cleaning up temporary files...
```

A la fin de cette étape vous devez vous retrouver avec cette arborescence au sein de votre projet :

```shell
.
├── .husky/*
├── node_modules/*
├── dist
    ├── lambda.js
    ├── lambda.js.map
    └── lambda.zip
├── src
    └── handler.ts
├── .eslintrc.json
├── package.json
├── tsconfig.json
└── webpack.config.js
```

## Mettre en place les tests
On ne le dira jamais assez, les tests sont indispensables pour un code fiable. Il existe plusieurs frameworks de tests : Mocha, Jasmine ou encore Jest pour ne citer qu’eux. Dans cet article nous allons opter pour ce dernier.

### Installer Jest
Vous pouvez créer le fichier `src/handler.spec.ts` avec ce contenu pour exemple :

```typescript
import { handler } from './handler';

describe('#handler', () => {
  it('should return Hello World!', () => {
	const result = handler();
	expect(result).toEqual('Hello World!');
  });
});
```

Pour installer Jest, exécutez la commande suivante :

```shell
yarn add jest @types/jest jest-sonar-reporter ts-jest --dev
```

Voici une description des paquets installés :

- `jest` est le framework de test.
- `@types/jest` contient les définitions TypeScript de Jest.
- `jest-sonar-reporter` permet de convertir la sortie de Jest vers le format générique des tests de Sonar.
- `ts-jest` permet d’utiliser Jest pour tester du code écrit en TypeScript.

Dans le fichier `jest.config.js`, vous pouvez ajouter ces lignes :

```javascript
module.exports = {
  testEnvironment: 'node',                                            #1
  testTimeout: 30000,                                                 #2
  roots: ['<rootDir>/src'],                                           #3
  transform: {
	'^.+\\.tsx?$': 'ts-jest',                                         #4
  },
  testRegex: '(//.*|(\\.|/)(test|spec))\\.tsx?$',                     #5
  moduleFileExtensions: ['ts', 'tsx', 'js', 'jsx', 'json', 'node'],   #6
  collectCoverage: true,                                              #7
  coverageDirectory: 'reports',                                       #8
  coverageReporters: ['lcov', 'text'],                                #9
  testResultsProcessor: 'jest-sonar-reporter',                       #10
};
```

Les différentes lignes permettent d’indiquer :

1. l’environnement de tests à utiliser.
1. le délai maximal d’un test.
1. le répertoire où trouver les tests à exécuter.
1. quel transformateur utiliser pour quel type de fichier car TypeScript n’est pas supporté nativement par Node.js.
1. quel format le nom du fichier doit avoir pour être exécuté.
1. les différentes extensions de fichiers que vos modules utilisent.
1. si l’on souhaite collecter les données de couverture de tests.
1. le dossier où ajouter les rapports de couvertures de tests.
1. les différents rapporteurs à utiliser ; nous allons ici prendre `lcov` et `text` (celui-ci permet d’afficher le rapport de couverture dans le terminal à la fin des tests).
1. si l'on souhaite utiliser un rapporteur personnalisé, dans notre cas nous prenons `jest-sonar-reporter`.

Ajoutez ensuite dans le package.json le script suivant :

```json
...
"scripts": {
    "test": "jest"
}
...
```

Il suffit simplement de lancer la commande `yarn test` pour lancer les tests.

```shell
yarn run v1.22.18
$ jest
 PASS  src/handler.spec.ts
  #handler
	✓ should return Hello World ! (1 ms)

------------|---------|----------|---------|---------|-------------------
File        | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
------------|---------|----------|---------|---------|-------------------
All files   |     100 |      100 |     100 |     100 |              	 
 handler.ts |     100 |      100 |     100 |     100 |              	 
------------|---------|----------|---------|---------|-------------------
Test Suites: 1 passed, 1 total
Tests:   	1 passed, 1 total
Snapshots:   0 total
Time:    	1.287 s
Ran all test suites.
Done in 1.78s.
```

### Écrire les tests
Nous avons maintenant Jest, mais comment tester ? Je vais montrer dans cette partie comment tester par un exemple. Je ne vais pas m’attarder sur la syntaxe car la documentation est là pour ça, mais plutôt montrer comment utiliser les Mocks.

Nous allons modifier notre code pour tester avec un exemple. Nous allons développer une lambda qui lorsqu’elle est appelée va récupérer le contenu de la requête et générer un lien vers un fichier S3 avec le contenu de la requête en tant que métadonnées.

Vous pouvez installer les librairies suivantes :

```shell
yarn add aws-sdk uuid --save
yarn add @types/aws-sdk @types/aws-lambda @types/uuid --dev
```

Voici un exemple de code décrivant le comportement expliqué précédemment :

`src/handler.ts`

```typescript
import { S3 } from 'aws-sdk';
import { ALBEvent, ALBResult } from 'aws-lambda';
import { S3Service } from './services/s3-services';

const S3_CLIENT = new S3({ endpoint: process.env.S3_ENDPOINT, s3ForcePathStyle: !!process.env.S3_FORCE_PATH_STYLE });
const S3_SERVICE = new S3Service(S3_CLIENT);

export function handler(event: ALBEvent): ALBResult {
  const metadata = JSON.parse(event.body as string);
  const presignedUrl = S3_SERVICE.generatePresignedUrl(metadata, process.env.BUCKET_NAME as string);

  return {
    statusCode: 200,
    body: JSON.stringify({
      presignedUrl,
    }),
  };
}
```

`src/services/s3-services.ts`

```typescript
import { S3 } from 'aws-sdk';
import { v4 as uuidv4 } from 'uuid';

export class S3Service {
  private s3Client: S3;

  constructor(s3Client: S3) {
    this.s3Client = s3Client;
  }

  generatePresignedUrl(metadata: unknown, bucketName: string): string {
    const params = {
      Bucket: bucketName,
      Key: `bucket/path/${uuidv4()}.txt`,
      Metadata: metadata,
    };

    return this.s3Client.getSignedUrl('putObject', params);
  }
}
```

Testons la classe `S3Service` et plus particulièrement la fonction `generatePresignedUrl`. Cette fonction est relativement simple, elle génère un identifiant unique (uuid) et appelle la librairie AWS pour récupérer l’URL. Il est intéressant de voir que nous devons mocker la librairie du client S3 et le générateur d’identifiant (uuid). Nous allons voir comment le faire de deux manières différentes.

Voici ce que vous pouvez obtenir comme test dans `src/handler.spec.ts` :

```typescript
/* eslint-disable @typescript-eslint/no-empty-function */
import { S3 } from 'aws-sdk';
import { S3Service } from './s3-services';
import * as uuid from 'uuid';

jest.mock('uuid'); // 1

const s3ClientMock = {
  getSignedUrl: () => {}, // 2
} as unknown as S3;

describe('S3 service unit tests', () => {
  const s3Service = new S3Service(s3ClientMock);

  it('should call presigned url with correct payload and metadata', () => {
    const presignedUrl =
      'https://bucket.s3.region.amazonaws.com/myfile.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=random-aws-credential-to-identify-the-signer&X-Amz-Date=timestamp-of-generation-of-url&X-Amz-Expires=validity-from-generation-timestamp&X-Amz-Signature=6ffca338-f5b2-48ad-89ec-4ae462cb46dc&X-Amz-SignedHeaders=host';
    jest.spyOn(s3ClientMock, 'getSignedUrl').mockReturnValue(presignedUrl); // 3
    jest.spyOn(uuid, 'v4').mockReturnValue('11aa4c90-fb02-445d-b898-da7f4bbaadd3'); // 3

    const result = s3Service.generatePresignedUrl(
      { request_id: '123126af-2ce6-41f2', author: 'Pikachu' },
      'local-bucket',
    );

    expect(s3ClientMock.getSignedUrl).toHaveBeenCalledWith('putObject', {
      Bucket: 'local-bucket',
      Key: 'bucket/path/11aa4c90-fb02-445d-b898-da7f4bbaadd3.txt',
      Metadata: {
        request_id: '123126af-2ce6-41f2',
        author: 'Pikachu',
      },
    }); // 4

    expect(result).toEqual(presignedUrl); // 4
  });
});
```

Pas de panique, je vais vous expliquer les points importants.

L’un des moyens de Mocker avec Jest (// 1) est d’appeler `jest.mock('uuid')`. Cela permet dans notre cas de simuler toute la librairie uuid. Le second moyen de Mocker, dans le cas où il s’agit d’une dépendance à injecter, est de créer un objet (// 2) qui imite la signature (mais vide d’implémentation) de la classe à mocker et de l’injecter dans votre service.

Cela permet par la suite d’utiliser `jest.spyOn('object', 'methodName')` pour créer une fonction simulée sur la fonction de l’objet et de mocker (// 3) le résultat de l’appel.

Vous pouvez ensuite vérifier (// 4) avec `expect` les résultats attendus ou que les mocks ont bien été appelés avec les paramètres attendus.

Vous pouvez bien sûr [consulter la documentation](https://jestjs.io/fr/docs/mock-functions) de Jest pour avoir un guide un peu plus complet sur ce qu'offrent les Mock Jest et même le framework de test en général.

## Localstack
Dans notre application, nous faisons appel au service AWS S3. Pour pouvoir lancer des tests d’intégration en local ou en intégration continue, nous allons utiliser [Localstack](https://github.com/localstack/localstack). Il s’agit d’un émulateur permettant notamment de simuler localement les services AWS. Il existe déjà un [article](https://blog.ippon.fr/2019/10/12/tester-avec-localstack/) traitant bien du sujet. Nous allons voir ici comment l’intégrer dans notre projet.

Nous allons changer le contenu du fichier `src/handler.ts` avec un test d’intégration qui doit effectuer un appel vers S3 comme ceci :

```typescript
process.env.S3_ENDPOINT = process.env.S3_ENDPOINT ?? 'http://localhost:4566'; // On indique ici l'url vers le serveur localstack
process.env.BUCKET_NAME = 'local-bucket';
process.env.S3_FORCE_PATH_STYLE = 'true';
process.env.AWS_ACCESS_KEY_ID = 'test';
process.env.AWS_SECRET_ACCESS_KEY = 'test';

import { handler } from './handler';
import { S3 } from 'aws-sdk';
import { ALBEvent } from 'aws-lambda';

describe('Handler test integration', () => {
  let s3Client: S3;

  beforeAll(async () => {
    s3Client = new S3({ endpoint: process.env.S3_ENDPOINT, s3ForcePathStyle: !!process.env.S3_FORCE_PATH_STYLE });
    await s3Client
      .createBucket({
        Bucket: 'local-bucket',
      })
      .promise();
  });

  it('should generate return presigned url', () => {
    const result = handler({ body: JSON.stringify({ author: 'Tripod Studio', title: 'Lost Ark' }) } as ALBEvent);
    expect(result.statusCode).toEqual(200);
    const body = JSON.parse(result.body as string);
    expect(body.presignedUrl).toContain(`${process.env.S3_ENDPOINT}/local-bucket/bucket/path`);
    expect(body.presignedUrl).toContain('x-amz-meta-title=Lost%20Ark');
    expect(body.presignedUrl).toContain('x-amz-meta-author=Tripod%20Studio');
  });
});
```

Ce test va d’abord initialiser le contexte en créant un bucket S3, ensuite il lancera simplement un test pour appeler notre handler (appel S3 pour générer une URL) et vérifier que le retour contient bien cette URL pré-signée S3.

Si vous n’avez pas encore [Docker](https://www.docker.com/), je vous conseille de l’installer. Nous allons créer un fichier `docker-compose.yml` avec cette configuration :

```yaml
version: '3.7'

services:
  localstack:
    hostname: lambda-project-localstack
    image: localstack/localstack:0.14.2
    ports:
      - '4566:4566'
    environment:
      - SERVICES=s3
```

Vous pouvez ensuite ajouter dans le `package.json` les scripts suivants :

```json
...
"scripts": {
    "localstack:start": "cd tests/localstack && docker-compose -p lm-upload-localstack up -d",
    "localstack:stop": "cd tests/localstack && docker-compose -p lm-upload-localstack down"
}
...
```

Vous pouvez maintenant exécuter les tests (`yarn test`) en vous assurant d'avoir lancé localstack auparavant avec `yarn localstack:start`.

A la fin de cette étape, vous devez vous retrouver avec cette arborescence au sein de votre projet :

```shell
.
├── .husky/*
├── localstack
    └── docker-compose.yml
├── node_modules/*
├── dist
    ├── lambda.js
    ├── lambda.js.map
    └── lambda.zip
├── reports
    ├── lcov-report/*
    ├── lcov.info
    └── test-report.xml
├── src
    ├── services
        ├── s3-services.spec.ts
        └── s3-services.ts
    ├── handler.ts
    └── handler.spec.ts
├── .eslintrc.json
├── .prettierrc
├── jest.config.js
├── package.json
├── tsconfig.json
└── webpack.config.js
```

## Analyser son code avec Sonar
Sonar est un outil d’analyse sur la qualité du code en continu, il permet notamment d’identifier la duplication de code, d'éventuels bugs dans une certaine mesure, de vérifier le respect des règles de programmation, etc. Nous allons voir dans cette partie comment le mettre en place dans une CI (Intégration continue) dans Gitlab.

### Configuration
La configuration se fait via le fichier `sonar-project.properties` :

```shell
sonar.host.url=https://sonarcloud.io                    #1
sonar.organization=lingchun-so                          #2
sonar.projectKey=lingchun.so_lambda-project             #3
sonar.sources=src                                       #4
sonar.tests=src                                         #5
sonar.test.inclusions=**/*.spec.ts                      #6
sonar.sourceEncoding=UTF-8                              #7
sonar.exclusions=node_modules/*,**/*.js                 #8
sonar.coverage.exclusions=src/**/*.spec.ts,node_modules/*,reports/lcov-report/*                                                 #9
sonar.qualitygate.wait=true                             #10
sonar.javascript.lcov.reportPaths=reports/lcov.info     #11
sonar.testExecutionReportPaths=reports/test-report.xml  #12
```

Les différentes lignes permettent d’indiquer :

1. l’URL du serveur Sonar.
1. l’organisation.
1. la clé du projet.
1. où trouver le code source.
1. où trouver les tests.
1. quels pattern sont considérés comme des tests.
1. quel est l’encodage des fichiers sources.
1. les fichiers que l’on souhaite exclure de l’analyse Sonar.
1. les fichiers que l'on souhaite exclure des couvertures.
1. à Sonar d’attendre jusqu’à que le statut de la Quality Gate soit disponible.
1. le chemin vers les rapports de tests produit ici par Jest.
1. le chemin vers le rapport d’exécution de tests.

### Intégration dans Gitlab CI
Dans cette partie nous allons mettre en place une simple intégration continue en deux phases, les tests et l’analyse du code source.

Vous pouvez créer le fichier `.gitlab-ci.yml` et ajouter le contenu suivant :

```yaml
stages:
  - test
  - 'code analysis'

build:
  stage: test
  image: node:14-alpine
  services:
    - name: localstack/localstack:0.14.2   # lance un container avec localstack dans le job
      alias: localstack                    # nom d'url du serveur localstack
  variables:
    SERVICES: s3
    S3_ENDPOINT: http://localstack:4566 
  script:
    - yarn
    - yarn test
  artifacts:
    paths:
      - reports # permet notamment de garder des fichiers entre "stage"

sonar:
  stage: 'code analysis'
  image:
    name: sonarsource/sonar-scanner-cli:latest
    entrypoint: ['']
  script:
    - sonar-scanner
```

Il faut aussi ajouter en tant que variable d’environnement Gitlab un token Sonar pour s’authentifier : `SONAR_TOKEN`.

Si vous faites un commit et envoyez les changements sur votre répertoire distant, la CI devrait se lancer. Après avoir lancé les tests, Sonar sera lancé et enverra les résultats vers le serveur Sonar que vous lui aurez indiqué.

A la fin de cette étape vous devez vous retrouver avec cette arborescence au sein de votre projet :

```shell
.
├── .husky/*
├── localstack
    └── docker-compose.yml
├── node_modules/*
├── dist
    ├── lambda.js
    ├── lambda.js.map
    └── lambda.zip
├── reports
    ├── lcov-report/*
    ├── lcov.info
    └── test-report.xml
├── src
    ├── services
        ├── s3-services.spec.ts
        └── s3-services.ts
    ├── handler.ts
    └── handler.spec.ts
├── .eslintrc.json
├── .gitlab-ci.yml
├── .prettierrc
├── jest.config.js
├── package.json
├── tsconfig.json
├── sonar-project.properties
└── webpack.config.js
```

## Le mot de la fin
Ainsi s’achève ce guide, vous êtes paré pour bien travailler sur votre lambda ! Nous avons pu voir dans cet article comment mettre en place la structure d’un projet de A à Z. Il n’est finalement pas spécifique pour les lambda AWS car il peut très bien être utilisé pour tout autre type de projet Node.js.

Nous n’avons pas non plus traité comment déployer une lambda dans cet article car il y a beaucoup de moyens de le faire. Ce n’est pas le but ici et de très bons guides existent déjà comme [celui-ci](https://blog.ippon.fr/2021/04/26/deployer-un-backend-typescript-full-serverless-avec-sam-1-3/) sur comment déployer un backend TypeScript full-serverless avec SAM. Je n’ai pas non plus abordé le sujet des binaires mais il est relativement simple de mettre en place dans la CI l'envoi de la distribution archivée (ZIP) dans un dépôt binaire, on pourrait le déposer sur un S3 par exemple.

Vous pourrez retrouver le code source [ici](https://gitlab.com/lingchun.so/lambda-project) et les résultats de Sonar [ici](https://sonarcloud.io/project/overview?id=lingchun.so_lambda-project).
  
source : https://blog.ippon.fr/