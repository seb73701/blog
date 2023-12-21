---
title: AWS Timestream - une base de données pas comme les autres
description: 
published: true
date: 2023-06-17T09:59:14.936Z
tags: 
editor: markdown
dateCreated: 2023-06-17T07:53:36.065Z
---

# AWS Timestream - une base de données pas comme les autres

![site-merch_amazon-timestream_socialmedia_2.png](/assets/img/clouds/tempo/site-merch_amazon-timestream_socialmedia_2.png)

Dans un monde où le nombre d'objets connectés ne fait que croître avec des estimations pouvant aller jusqu'à 24 milliards en 2050, les solutions de timeseries sont un enjeu d’avenir. Cependant, elles restent encore aujourd’hui assez immatures, leur utilisation marginale. Sur [db-ranking](https://db-engines.com/en/ranking) la première timeseries est placée 27ème, ce sont des résultats basés sur la popularité de la solution. Il en existe néanmoins certaines qui se démarquent : [InfluxDB](https://www.influxdata.com/), [TimeScale](https://www.timescale.com/) et, dans une certaine mesure, Prometheus (Prometheus utilise un moteur de timeseries mais son but premier reste de faire de la surveillance). Ces solutions nécessitent d’avoir un bon bagage technique afin de maintenir de bonnes performances pour des grandes volumétries et des taux de disponibilité élevés (il existe la possibilité de passer par leurs offres en SaaS coûteuses).

AWS a annoncé en 2018 à sa conférence annuelle re:Invent, un nouveau service : Amazon Timestream. La promesse semblait incroyable : un nouveau service optimisé pour les données temporelles qui offrent des performances jusqu’à 1000 fois supérieures aux bases de données SQL de l'époque. Un service qui serait, à l’instar de DynamoDB, opéré totalement par AWS, le fameux “serverless”. L’utilisateur final n’aurait qu’à utiliser le service et profiter de cette augmentation brute de performance, tout en ayant des coûts revus à la baisse, et ce jusqu’à les diviser par 10. Et c’est ainsi que lors du 4ème trimestre de 2020, le service annoncé deux ans plus tôt sort finalement de sa version bêta et devient disponible dans 4 des régions AWS :

- USA Est Virginie du Nord (us-east-1),
- USA Est Ohio (us-east-2),
- USA Ouest Oregon (us-west-2),
- Europe Irlande (eu-west-1).

En deux ans, le paysage des technologies timeseries a évolué. [InfluxDB](https://www.influxdata.com/) s’est imposée comme la solution de référence malgré des débuts assez difficiles : une nouvelle version pointe le bout de son nez, promettant d’énormes bouleversements. TimeScale, par le bagage que lui apporte le développement de PostgreSQL, progresse dans sa conquête du marché. Alors, est-ce que dans ce contexte concurrentiel, cette sortie tardive de Timestream va bouleverser l’ordre établi, et permettre à AWS de s’imposer dans le monde des timeseries ?

Cet article n’a pas pour but de déterminer la meilleure base de données timeseries, mais de présenter le fonctionnement de Timestream, comment elle se différencie des autres, ce qu’elle apporte au paysage actuel et de définir quels sont les cas d’utilisation où elle pourrait être pertinente.

Il n’y aura pas de benchmark dans cet article.

## Les Bases de données temporelles
Avant d’aller plus loin, il est important de commencer par comprendre ce qu’est Timestream. Et avant ça, ce qu’est une base de données temporelle ou TSDB.

On appelle base de données temporelles, une base de données dont l’index de chaque valeur est une représentation temporelle, un timestamp. En plus d’être indexée par le temps, la valeur contient la plupart du temps des métadonnées permettant de la qualifier : l’intervalle de mesure, l’adresse mac de l’équipement émetteur, un nom, des caractéristiques, etc. L’ensemble des points dans une telle base est qualifié de série temporelle.

L’avantage et la force de ce type de bases résident dans la possibilité de s’affranchir des difficultés rencontrées par de nombreux développeurs. Ces difficultés sont : la gestion des fuseaux horaires, d’immenses quantités de données et leurs agrégations. Ces trois propriétés sont à l’origine du développement des TSDB.

En effet, quiconque ayant rencontré ces problématiques connaît toute leur complexité. Ce sont des demandes récurrentes mais qui deviennent rapidement complexes, et qui empirent avec le nombre de données croissant.

Prenons un exemple simple : je dispose de serveurs dans des continents différents. Je dois m’assurer de leur état de santé à tout instant pour dépêcher des équipes afin d’opérer aussitôt qu’un événement survienne. En parallèle, une équipe de Data Scientists s’occupe de l’optimisation des coûts sur ces mêmes serveurs. Enfin, des Data Analysts mettent en place des KPI pertinents pour suivre l’évolution du développement du produit. Il existe donc ici trois cas d’utilisation reposant sur un accès à la même donnée, mais sous différents formats :

- L'équipe de maintenance a besoin d’avoir une donnée agrégée à la minute par timezone pour pouvoir reconnaître des indicateurs qui indiqueraient une erreur critique. Et ce, sur les dernières 24 heures.
- Les Data Scientists ont besoin d’avoir accès aux jeux de données brutes complets avec ou sans agrégation selon le modèle qu’ils souhaitent entraîner. Mais avec des fonctionnalités d’aggrégations complexes selon les valeurs physiques qu’ils souhaitent intégrer à leur modèle.
- Enfin, les Data Analysts ont besoin d'agrégations différentes selon les KPIs qu’ils souhaitent créer pouvant aller jusqu’au mois voire même l’année.

Cet exemple très basique permet de mettre en lumière le fait que l’utilisation d’une même donnée peut être très différente selon qui en a besoin. C’est là que les TSDB interviennent : elles permettent de s’affranchir de cette complexité. La complexité n'est plus de gérer ces agrégations mais simplement de requêter la base. Cela ne veut pas dire qu’il n’y a plus besoin de cette couche intermédiaire, mais la difficulté est déportée vers la TSBD et permet aux utilisateurs de la base de se concentrer sur la donnée et non sur la façon de l’obtenir.

TL;DR : Les TSBD ou time series database sont des bases de données permettant de stocker des données ayant un index temporel (timestamp). Un ensemble de données est appelé une série temporelle. Ces bases ont implémenté de nombreuses fonctionnalités optimisées d'agrégation, de gestion de fuseaux horaires, et de fonctions temporelles complexes, permettant la gestion de très grandes quantités de données.

## Le cas Timestream
Nous allons maintenant nous attarder sur la solution Amazon Timestream, décrire ce qu’elle propose et comment elle fonctionne à haut niveau. Et enfin, comment elle s'intègre dans un environnement Data au sein d’AWS et même plus généralement.

### Le modèle Timestream
#### Un système «fully managed serverless»
Comme vu précédemment, Timestream est un système de gestion de base de données orienté timeseries. Il implémente des fonctions d’analyse sur des séries temporelles, comprenant  une fonction d'auto scalabilité, sans gérer les serveurs. En d’autres termes, même si vous souhaitez opérer par vous-même le service, c’est impossible, toute la gestion est faite par AWS.


#### Cycle de vie de la donnée
Souvent, dans le monde de la Data, on utilise le champ lexical de la température. La donnée la plus chaude est accessible en quelques millièmes de secondes alors que la plus froide peut aller jusqu’à plusieurs heures.

Là où Timestream se démarque, c’est qu’en son sein la solution comprend déjà une gestion de la donnée froide et chaude. De facto, sur Timestream lors de l’ingestion, la donnée arrive toujours dans une première zone, la plus chaude disponible, c’est un stockage directement en mémoire, qui propose la latence la plus faible possible. Cette couche, en plus d’être extrêmement rapide,  permet également d’effectuer des optimisations au niveau de la donnée ingérée, notamment la gestion des doublons et l’optimisation pour de futures requêtes. Ces optimisations sont directement gérées par AWS et ne peuvent pas être modifiées, monitorées ou contrôlées par l’utilisateur final, ce qui fait de cette zone, en plus d’être obligatoire, reste une “boite noire”.

Après le passage obligatoire en zone mémoire dont le paramétrage peut être compris entre 1 heure et un an, la donnée est automatiquement transférée vers le second stockage de la base, utilisé pour de l’historisation. Un stockage magnétique, dit stockage froid. Il permet de stocker de très grandes quantités de données à faible coût, mais avec une latence bien plus importante que celle en stockage mémoire.

Comme vous vous en doutez, en théorie, le stockage de la mémoire est le plus intéressant de tous. Mais son coût est dissuasif. Son optimisation est donc primordiale, pour éviter d’avoir des prix excessifs.

Le tableau suivant présente les prix des différents stockages actuels et à venir avec, en comparaison, l’autre géant du stockage en base chez AWS : DynamoDB. Les prix sont basés sur [pricing timestream](https://aws.amazon.com/fr/timestream/pricing/) au 29/12/2020 pour la région us-east-1.

|    |Latence de réponse|Par GB-Month|
|:---|:----------------:|:----------:|
|Stockage mémoire|Chaud (< 1 ms)|$25.92|
|Stockage SSD (à venir)|Tiède (< 30 ms)|$0.3|
|Stockage Magnétique|Froid (< 500 ms)|$0.03|
|Dynamodb (sans DAX ou cache)|Tiède (<30 ms)|$0.25|

tableau est assez explicite : la simplicité et les performances viennent avec un coût élevé, celui-ci n’est pas moindre : le coût du stockage en mémoire est 100 fois plus élevé qu’un stockage sur DynamoDB. Il doit donc être optimisé avec perfection pour éviter les surcoûts. Néanmoins, le stockage magnétique concurrence même S3 en termes de coûts (0.023/Gb-month), ce qui en fait un excellent outil d’historisation. La latence reste cependant très élevée, ce qui peut être un point en défaveur de la solution sur certains cas d’utilisation.

Maintenant que nous avons parlé du stockage, il est temps de voir comment l’intégration se fait sur Timestream.

#### «First writer wins»
«La première écriture l'emporte», telle est la sémantique qu’utilise Timestream pour l’ingestion de données. Comme explicité dans la partie précédente, la donnée ingérée est préparée dans la couche en mémoire, les doublons sont retirés, et des optimisations sont faites. Les premières insertions sont intégrées, les suivantes seront supprimées.

Il est néanmoins possible d’effectuer des actions d’UPSERT (update ou insert si la donnée n’existe pas). Toutefois, étant donné que la donnée insérée devrait être continue pour pouvoir observer son évolution au cours du temps, des updates ne devraient pas être nécessaires.

Afin de continuer dans cette idée que le premier arrivé prend tout, la donnée ingérée l’est, sans passer par une phase de validation de la donnée.

En effet, le schéma de la donnée est créé dynamiquement en fonction de la première insertion. Ce schéma ne peut être défini en amont manuellement ; la donnée doit donc arriver de manière “propre”, afin d’éviter des erreurs d’ingestion dues au type de données.

Afin d’expliciter ce phénomène un exemple sera plus parlant : si la première donnée est un entier et que la suivante est une chaîne de caractères, alors la chaîne de caractère sera rejetée. Il n’est pas possible de caster une chaîne de caractères en entier sans s’assurer de ne pas perdre d’information.

Autre point d’attention, la zone de mémoire vous permet de définir une plage dans laquelle votre donnée doit pouvoir être intégrée de 1 heure à 1 an. **Toute donnée en dehors de cet intervalle ne sera pas intégrée**. Et oui, vous avez bien lu, si vous avez de la donnée historique à migrer, ce n'est pas possible actuellement. Néanmoins, si vous n’avez que quelques mois de données à ingérer il est possible d’augmenter la plage de la donnée en mémoire, faire l’ingestion de données puis la diminuer.

Enfin la lecture est dite cohérente à terme (eventually consistent), c'est-à-dire qu’une lecture directement après une insertion peut ne pas refléter la réalité.

En parlant de lecture, il est temps de s'attarder sur la façon d’interroger la base.

### Interface et requête sur timestream
#### API et JDBC
Comme tous les services, AWS timestream est accompagné d’une API permettant de communiquer avec l’outil. Cette API permet notamment d’envoyer les requêtes de lecture et d’ingestion de données.

La plus intéressante est celle d'écriture qui est optimisée pour de l’ingestion par morceau de 100 éléments. La seule problématique étant le fait que chaque ligne doit contenir exactement une mesure. On ne peut envoyer un morceau de 100 éléments de 100 mesures (cela s’oppose notamment à ce que propose l’API de DynamoDB).

En complément, de l’API, il y a aussi un connecteur JDBC, maintenu par AWS, qui permet donc de connecter de nombreux outils à Timestream, n’étant pas expert du domaine, je n’irai pas plus loin. Vous trouverez ici la documentation liée : [JDBC - Amazon Timestream](https://docs.aws.amazon.com/timestream/latest/developerguide/JDBC.html).

#### Le SQL continue de régner
Pour requêter de la donnée sur Timestream, c’est le langage SQL qui une fois de plus a été choisi comme intermédiaire. Bien entendu c’est ici du simili SQL, la syntaxe de base est bien la même mais de nombreuses options ne sont pas gérées. Tout ce qui est “jointure” n’est pas disponible. De plus, de nouvelles fonctionnalités ont été implémentées afin de gérer la partie temporelle de notre outil. Mais c’est là qu'un choix d’AWS est surprenant : la création de deux modèles pour requêter la base.

#### Deux modèles de requête
Voici donc une autre fonctionnalité de TimeStream qui n’en fait décidément pas un outil comme les autres. Il existe deux modèles pour requêter la donnée.

- Le premier modèle : “flat model”, c’est le modèle par défaut et donne une représentation tabulaire de la donnée. A l’image de ce que l’on pourrait trouver sur une base de données relationnelle. Ce modèle est utilisé lorsqu’aucune opération temporelle n’est demandée.

|time|région|instance_id|measure_name|measure_value: double|
|:---|:-----|:----------|:-----------|:--------------------|
|2019-12-04 19:00:00.000000000|us-east-1|i-1234567890abcdef0|cpu_utilization|35.0|
|2019-12-04 19:00:01.000000000|us-east-1|i-1234567890abcdef0|cpu_utilization|3.2|
|2019-12-04 19:00:02.000000000|us-east-1|i-1234567890abcdef0|cpu_utilization|45.3|

- Le second modèle : “timeseries model”, comme son nom l'indique, est ici pour gérer les données lorsqu’on fait appel à des fonctions temporelles. Dans ce mode il est possible d'effectuer des opérations d’interpolation, intégration, de dérivée, etc. l est obligatoire de passer par ce modèle de données pour TOUTES les opérations temporelles.

|région|instance_id|cpu_utilization|
|us-east-1|i-1234567890abcdef0|[{time: 2019-12-04 19:00:00.000000000, measure_value::double: 35.0},{time: 2019-12-04 19:00:01.000000000, measure_value::double: 38.2},{time: 2019-12-04 19:00:02.000000000, measure_value::double: 45.3}]|

Ce choix rend le SQL comme langage d’interface assez étrange puisqu’en utilisant le modèle timeseries, il est nécessaire de transformer la donnée en timeseries, d’effectuer l’opération qui nous intéresse puis finalement de repasser dans le modèle flat pour récupérer la donnée. Ce qui allonge la taille des requêtes, et nécessite de développer son propre moteur pour générer les requêtes. Un langage propre pour l’outil, ou une API de récupération de données aurait pu être une solution plus adaptée à la manière de ce qui est proposée sur DynamoDB.

#### Example :

Dans l’exemple de code ci-dessous, je présente la requête nécessaire pour effectuer une interpolation linéaire sur des données manquantes, avec un pas de temps de 10 secondes. Comme vous pouvez le constater, il est nécessaire de passer par les deux modèles pour obtenir le résultat désiré. Tout d’abord on effectue l’interpolation avec le mot clé CREATE_TIME_SERIES. Puis on récupère le résultat de la CTE, avec l’instruction UNSET pour obtenir le modèle flat. On est loin de la simplicité d’une requête SQL sur d’autres TSDB.

```sql
WITH interpolated_timeseries AS
(
         SELECT   region,
                  az,
                  vpc,
                  instance_id,
                  interpolate_linear(Create_time_series(time, measure_value::DOUBLE), sequence(ago(6h), now(), 10s)) AS interpolated_cpu_utilization
         FROM     timestreamdb.metrics
         WHERE    measure_name= 'cpu_utilization'
         AND      time >= ago(6h)
         GROUP BY region,
                  az,
                  vpc,
                  instance_id)
SELECT   region,
         az,
         vpc,
         instance_id,
         avg(t.cpu_util)
FROM     interpolated_timeseriescross
JOIN     unnest(interpolated_cpu_utilization) AS t (time, cpu_util)
GROUP BY region,
         az,
         vpc,
         instance_id
```

### Intégration dans un écosystème data
L’une des plus grandes forces de l’outil est son excellent interfaçage avec les solutions existantes d’AWS. Étant un service managé, il rentre parfaitement en combinaison avec d'autres services : Kinesis, Lambda pour l’ingestion ou KMS pour tout ce qui est chiffrement de la donnée (celui-ci est d’ailleurs nécessaire pour la donnée stockée). De plus, il peut également s’interfacer avec AWS IoT Core. Timestream semble être un service visant à devenir de plus en plus présent au sein des architectures événementielles à venir. Quoi qu’il en soit, c’est ce qui semble être poussé par AWS, à voir si avec le temps, la solution sera capable d’évoluer comme l’ont fait ses grandes sœurs. Il existe également une intégration avec SageMaker et Amazon QuickSight. À voir si l’intégration ira plus loin qu’une simple connectivité entre les services.

En dehors d’AWS, l’offre perd en pertinence : le grand outil de visualisation Grafana est complètement opérationnel avec Timestream et c’est une excellente nouvelle. Mais pour le reste il faudra compter sur le driver JDBC et son développement. L’outil n’est vraiment pas prévu pour sortir du contexte d’AWS. Il doit être utilisé en son sein, des connexions avec d’autres outils comme Kafka Connect, Spark sera à la charge des équipes open source.

## La solution miracle de l’IoT pour AWS ?
Miracle non, mais elle peut dans un contexte très particulier, être une solution à envisager.

- Tout d’abord il faut que le client souhaitant utiliser Timestream soit fortement lié avec AWS. Cette solution, encore plus que d’autres entièrement managées (Lambda, Kinesis), crée une forte dépendance, et donc un développement avec le support d’AWS pour profiter au maximum du potentiel de la solution serait bénéfique.
- Ensuite, toutes les équipes seront capables d’utiliser l’outil, mais pour réduire les coûts au minimum une expertise devra être présente. En effet, comme on l’a vu précédemment, les coûts peuvent rapidement décoller surtout avec le stockage. Les écritures quant à elles, si elles sont bien gérées, ne devraient pas poser de soucis mais la gestion de la donnée doit être excellente en amont pour éviter d’avoir à utiliser plus de stockage en mémoire que nécessaire.
- Une grande phase de développement est nécessaire pour intégrer la solution, afin de générer un SQL concis et efficace. Plus les requêtes sont complexes et plus il faudra s’assurer de la robustesse de la solution de génération. Notamment la capacité à jongler entre les deux types de modèles de données.
- Avec seulement un stockage magnétique, le projet souhaitant utiliser Timestream ne doit pas avoir de contrainte de latence trop importante pour les données historiques. Le temps de réponse pouvant être assez long comparé à d’autres solutions.
- Estimer l’impact de la non rejouabilité de la donnée doit être préalable. Et si cette fonctionnalité est un prérequis, il faudra la développer. Notamment pour gérer les longues maintenances ou des pertes de connexion dépassant l’intervalle d’ingestion de la couche chaude.
- Enfin, il est préférable que le projet soit jeune, sans historique. Encore une fois, il n’est pas possible à l’heure actuelle d'ingérer de la donnée ayant plus d’un an. Ce qui fait que le projet ne doit pas avoir besoin de données âgées de plus d’un an, ou que Timestream soit utilisé dans une solution hybride. Mais dans ce cas, d'autres solutions pourraient également répondre au besoin (ex. DynamoDB).


## Conclusion

En conclusion, il est important de garder en tête que Amazon Timestream a été annoncé en 2018. Il a fallu 2 ans de développement supplémentaires pour que la solution soit disponible. Ce qui est un cas exceptionnel au niveau d’AWS, il y a donc fort à parier que la solution n’est pas encore totalement finalisée et que beaucoup d’innovations arriveront au fur et à mesure, afin de rendre le produit plus mature, plus disponible et intéressant pour un plus grand nombre de projets.

Je pense que l’ajout de la gestion d’historique, une amélioration du modèle de données et la mise en place du stockage en SSD pour remplacer celui en mémoire pourraient être des pistes à privilégier.

Ce qui ne fait plus de doute, c’est que les bases de données temporelles sont un sujet d’avenir et que la solution bien qu’imparfaite à l’heure actuelle peut devenir une pierre angulaire du développement d’AWS dans le monde de l’IoT.

Source : https://blog.ippon.fr/