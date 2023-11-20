---
title: Choisir la bonne architecture pour son cluster elasticsearch
description: 
published: true
date: 2023-06-21T04:07:35.608Z
tags: 
editor: markdown
dateCreated: 2023-06-21T04:07:35.608Z
---

# Choisir la bonne architecture pour son cluster elasticsearch

Dans cet article, nous allons décrire les différentes architectures elasticsearch disponibles afin que vous puissiez choisir la meilleure solution en fonction de vos besoins et de votre niveau de maturité dans l’utilisation de la suite ELK.

Nous donnerons l’exemple d’une implémentation dans le cloud AWS avec des instances EC2.

Tout au long de la lecture de cet article, n’oubliez pas qu’il n’existe pas d’architecture standard, les exemples que nous allons décrire représentent les tendances majeures en termes de design, mais doivent toujours être adaptées en fonction de vos spécificités.

## Quelques notions de base
L’objectif n’est pas d’expliquer en détail l’implémentation technique d’un cluster elasticsearch. Cependant, pour faciliter la compréhension, il nous a semblé nécessaire d’effectuer un rappel sur quelques notions importantes auxquelles nous allons faire référence dans les prochaines sections.

Un cluster elasticsearch est constitué d’un ensemble de **nœuds**. Un nœud peut endosser plusieurs **rôles** qui déterminent les tâches qu’il pourra réaliser dans le cluster.

La liste exhaustive des rôles peut évoluer au fil des versions, arrêtons-nous sur celle-ci :

- master
- data
- data_content
- data_hot
- data_warm
- data_cold
- data_frozen
- ingest
- ml
- remote_cluster_client
- transform

Les rôles d'un nœud sont spécifiés par le paramètre `node.roles` du fichier de configuration `elasticsearch.yml`.

Un nœud qui possède le rôle master dans sa liste est un nœud **master-eligible**, ce qui signifie qu’il peut être promu **master** suite à une élection à laquelle participent tous les nœuds qui possèdent le rôle master.

Ainsi, dans un cluster, un seul nœud peut être le master à un instant donné.

Le rôle master permet d’effectuer les tâches de gestion du cluster, la création et la suppression d'index, l'allocation des shards entre les nœuds ainsi que l'ajout et la suppression des nœuds du cluster.

Les rôles `data*` permettent aux nœuds de stocker les **shards** qui composent les **index** du cluster.

Si le paramètre `node.roles` est omis, le nœud endosse tous les rôles définis dans la liste précédente. Dans ce cas, l’architecture est dite **homogène** puisqu’il n’existe pas de nœud spécialisé dans le cluster.

Il s’agit de la première architecture que nous allons étudier.

## Architecture Homogène
Lors de la mise en place d’une première plateforme de logs, il est courant d’adopter cette architecture simple dans laquelle chaque nœud peut endosser les mêmes rôles dans le cluster, y compris le rôle master.

En principe, on retrouve cette architecture lorsque le cluster ne comporte pas plus de 3 nœuds.

Un seul nœud est élu master et chaque nœud contient des données.

Pour implémenter cette solution dans le cloud AWS, il est possible de déployer 3 instances EC2 sur les 3 Availability Zones (AZ) d’une région afin de garantir la résilience du cluster :

![untitled.png](/assets/img/apps/elasticsearch/untitled.png)

## Nœuds Master dédiés
À mesure que la volumétrie des données augmente, il peut être intéressant de réaliser une première amélioration sur notre cluster de départ en séparant les nœuds qui peuvent être élus master des nœuds qui traitent la donnée.

Il s’agit de l’architecture homogène avec **nœuds master dédiés**.

Cette solution garantit plus de stabilité en réduisant le risque d’un arrêt inopiné du processus elasticsearch sur le nœud master suite à un débordement mémoire ou à une surcharge du CPU puisque ce nœud ne traite pas la donnée et se restreint uniquement aux tâches de gestion du cluster.

Dès lors, il est possible de rajouter si besoin de nouveaux nœuds data sans perdre en performance sur le cluster.

![untitled(1).png](/assets/img/apps/elasticsearch/untitled(1).png)

## Définir le bon nombre de nœuds master dédiés
Afin de garantir la résilience du cluster, il est nécessaire de mettre en place plusieurs nœuds master-eligible dédiés.

Lorsque le cluster élit un nouveau nœud pour endosser le rôle master, tous les nœuds qui possèdent ce rôle participent à ce vote.

Cependant, il n’est pas nécessaire d’obtenir tous les votes des nœuds avec le rôle master pour terminer l’élection, **plus de la moitié suffit**.

Ce système permet de pouvoir élire un nouveau nœud master, même si un ou plusieurs nœuds sont défaillants ou injoignables.

Ainsi, pour qu’une élection puisse se conclure, le nombre de votes reçus doit être supérieur ou égal au quorum qu’il est possible de calculer avec la formule suivante :

```
Quorum = ⌊nombre de nœuds avec le rôle master / 2⌋ + 1
```

La méthode d’élection n’ayant plus de secret pour vous, essayons de déterminer le nombre optimal de nœuds master dédiés pour notre cluster.

Bien évidemment, il ne serait pas possible d’avoir **un seul** nœud master-eligible puisque dans le cas où ce nœud serait indisponible le cluster n’aurait plus de master.

**Deux** nœuds master-eligible n’est pas une option possible, puisque si le premier nœud n’est pas joignable, le deuxième nœud n’aurait pas le quorum nécessaire pour être élu master.

Avec **trois** nœuds, en cas de défaillance sur le nœud master, il reste 2 autres nœuds qui possèdent le quorum nécessaire (`Quorum = ⌊3 / 2⌋ + 1 = 2`) pour élire un nouveau master. Cette configuration est donc fonctionnelle.

Intuitivement **quatre** nœuds paraît être une option plus robuste, cependant nous obtenons :

```
Quorum = ⌊4 / 2⌋ + 1 = 3
```

Par conséquent, il n’est possible de perdre qu'un seul nœud (puisque si on perdait 2 nœuds, il resterait 2 autres nœuds, ce qui serait insuffisant pour atteindre un quorum de 3).
Cette option ne permet donc pas de perdre plus de nœuds que l’option précédente avec 3 nœuds master-eligible.

De plus, un autre désavantage lié à l’architecture des AZ AWS doit être pris en considération.

Effectivement, déployer quatre nœuds dans une même région implique d’avoir une AZ avec 2 nœuds. En cas de défaillance sur cette AZ, il ne resterait que deux nœuds fonctionnels sur les AZ non impactées et donc le quorum ne serait pas atteint.

Enfin, il nous reste la solution à cinq nœuds qui permet de perdre 2 nœuds master-eligible simultanément (`Quorum = ⌊5 / 2⌋ + 1 = 3`).

Cependant, il est nécessaire de payer le coût de 4 instances inactives, et de ce fait cette solution est souvent considérée comme étant excessive.

Ainsi, pour la mise en place d'un cluster à haute disponibilité, il est recommandé d'avoir **3 nœuds avec le rôle master**.

### Les nœuds voting-only pour optimiser les coûts
Les nœuds maîtres dédiés offrent de nombreux avantages, cependant il est nécessaire de souligner le surcoût qui incombe à leur implémentation.

Effectivement, ces nœuds spécialisés consomment des licences (même lorsqu’ils sont inactifs) ainsi que des ressources cloud (instance EC2 et disques EBS notamment).

Pour éviter de devoir inclure dans le cluster trop de nœuds master-eligible inactifs, il est possible d'en remplacer une partie par des **nœuds voting-only**.

Un nœud voting-only participe à l'élection (il est donc inclus dans le calcul du quorum) cependant il ne peut pas être élu master.

L’architecture elastic ayant évolué à ce sujet, pour des raisons de compatibilités, un nœud voting-only doit impérativement posséder le rôle voting-only **ET** le rôle master (même si celui-ci ne sera jamais élu master).

Ce qui nous donne la configuration suivante :

```yaml
node.roles: [ master, voting_only, ... ]
```

Un nœud voting-only peut être un nœud data ce qui permet de remplacer un nœud master dédié sans diminuer le nombre de votants :

```yaml
node.roles: [ master, voting_only, data ... ]
```

Si nous reprenons nos 3 nœuds master-eligible dédiés, il serait possible d’en supprimer un et de compter sur un nœud data pour conclure les élections entre les 2 nœuds master restants.

## Architecture Hot-Warm
Lorsque la valeur de la donnée diminue avec le temps, il peut être judicieux de mettre en place une architecture hot-warm pour optimiser les coûts et les performances du cluster.

On retrouve ce cas d’usage par exemple pour les logs systèmes et applicatifs. Le plus souvent, ce sont les données récentes qui sont les plus demandées par les utilisateurs, pour pouvoir investiguer un problème sur l’infrastructure ou pour poursuivre un développement logiciel, tandis que les données de plus d’une semaine, en revanche, ont très peu de chances d’être analysées.

### Les différents types de nœuds de données
Dans une architecture hot-warm, les nœuds data sont divisés en deux types :

**Les nœuds hot** qui contiennent les index les plus récents. L'ingestion des données s'effectue au niveau de ces nœuds qui traitent la majorité des requêtes de recherche.

Cela implique un matériel performant dû à un usage intensif du processeur et à un nombre élevé d'E/S. Les disques SSD sont donc à privilégier.

**Les nœuds warm** permettent le stockage à long terme des données, pour des index le plus souvent en lecture seule.

Dès lors, le matériel nécessaire est moins gourmand en ressources et des disques HDD peuvent suffire.

Pour définir le profil hardware de vos nœuds hot et warm, vous pouvez vous référer à la [documentation elasticsearch](https://www.elastic.co/guide/en/cloud/current/ec-default-provider-configurations.html).

Pour les nœuds hot le profil **highio** est conseillé tandis que pour les nœuds warm il est possible d’implémenter le profil **highstorage** qui permet de réduire les coûts du stockage.

Dans tous les cas, la RAM sera déterminée à partir d’un ratio sur la taille des disques.

Concernant le fichier `elasticsearch.yml`, depuis la version 7.10, il n'est plus nécessaire d'utiliser des attributs hot / warm, la définition s’effectue uniquement via les rôles :

```yaml
# Hot Node
node.roles: [ data_hot ]

# Warm Node
node.roles: [ data_warm ]
```

### La transition des index
Une fois que les index des nœuds hot dépassent la période de rétention prévue et qu'ils ne sont plus alimentés, ils sont déplacés vers les nœuds warm.

Une **Lifecycle Policy** permet de définir les modalités de transition d’un index d'un nœud hot vers un nœud warm.

Il est possible de spécifier jusqu’à cinq **phases** :

- hot
- warm
- cold
- frozen
- delete

Lorsqu’un index change de phase, il est automatiquement redirigé vers un nœud data qui possède le rôle correspondant dans son attribut `node.roles`.

L’attribut `min_age` permet de contrôler à partir de combien de temps un index peut entrer dans une phase.

Attention toutefois, l’âge d’un index dépend du rollover :

- Si le rollover est activé pour un index, son âge est calculé à partir de la date où cet index a été substitué.
- S’il n’y a pas de rollover, l’âge de l’index correspond à sa date de création.

Dans l’exemple suivant, un rollover est effectué 7 jours après la création de l’index, c’est à partir de ce moment que l’on décompte à nouveau 7 jours supplémentaires avant de rediriger l’index vers les nœuds warm où il sera supprimé au bout de 30 jours (toujours calculés à partir de la date du rollover).

Dans cette policy, nous avons également défini une priorité qui diffère entre les phases hot et warm.

Lors des opérations d’allocation, les shards des index avec une priorité élevée sont alloués en premier.

Dans notre cas, la priorité est de 50 pour les index hot et de 25 pour les index warm.

```json
PUT _ilm/policy/hot-warm-delete
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_age": "7d"
          },
          "set_priority": {
            "priority": 50
          }
        }
      },
      "warm": {
        "min_age": "7d",
        "actions": {
          "set_priority": {
            "priority": 25
          }
        }
      },
      "delete": {
        "min_age": "30d",
        "actions": {
          "delete": {}
        }
      }
    }
  }
}
```

Un index template permet de lier un index pattern à notre lifecycle policy :

```json
PUT /_template/my-template
{
  "index_patterns": [
    "my-index*"
  ],
  "settings": {
    "index": {
      "lifecycle": {
        "name": "hot-warm-delete",
        "rollover_alias": "my-index"
      },
      "number_of_shards": 1,
      "number_of_replicas": 1
    }
  }
}
```

Enfin, il ne nous reste plus qu’à créer l’index avec l’alias associé :

```json
PUT my-index-000001
{
  "aliases": {
    "my-index": {
      "is_write_index": true
    }
  }
}
```

### Comment exposer le cluster ?
Le cluster elasticsearch doit être joignable par logstash pour indexer les données via le plugin logstash-output-elasticsearch :

```ruby
output {
    elasticsearch {
        hosts => "URL_LB:9200"
        index => "my-index"
    }
}
```

De plus, Kibana doit également s’y connecter pour pouvoir visualiser les données.

Si vous souhaitez exposer votre cluster via un Load Balancer, il est conseillé de cibler uniquement les nœuds hot.

Effectivement, la documentation logstash pour le plugin logstash-output-elasticsearch déconseille d’envoyer des requêtes bulk vers les nœuds master dédiés et concernant Kibana, dans la mesure où les nœuds warm ont généralement moins de CPU/RAM, ils sont plus susceptibles d'échouer ou de rencontrer des problèmes de performance si par exemple une requête d'agrégation coûteuse leur est directement adressée.

In fine, l’architecture hot-warm suivante résume les différents points que nous avons pu étudier précédemment :

![untitled(2).png](/assets/img/apps/elasticsearch/untitled(2).png)

## Quelques bonnes pratiques
### Shard allocation awareness
Si vous avez plus de nœuds data que d’AZ, il est impératif de paramétrer le cluster de manière à ce qu’il prenne en compte cette spécificité, on parle de **shard allocation awareness**.

L’objectif est d’éviter le scénario suivant :

![untitled(3).png](/assets/img/apps/elasticsearch/untitled(3).png)

Dans cet exemple volontairement simplifié, seul un sous-ensemble du cluster sur 2 AZ est représenté.

Nous considérons un index constitué d’un seul shard primaire, situé sur le nœud 1 et de son réplica, situé sur le nœud 2.

Dans le cas où l’AZ eu-west-1a rencontrerait une défaillance, les nœuds 1 et 2 seraient impactés, ce qui signifie que le shard primaire et le shard réplica seraient perdus. Les autres nœuds restants ne pourraient donc pas récupérer les données de cet index.

Pour éviter ce problème, la répartition suivante devrait être adoptée :

![untitled(4).png](/assets/img/apps/elasticsearch/untitled(4).png)

Si nous reprenons le même scénario catastrophe, à savoir la perte de l’AZ eu-west-1a, le nœud 3 toujours disponible pourrait transformer son shard réplica en shard primaire tandis qu’un autre nœud pourrait reconstruire un réplica. L’index ne serait donc pas impacté.

Pour activer cette fonctionnalité, il faut associer à chaque nœud un identifiant correspondant à son AZ et indiquer que cet attribut est à prendre en considération lors des opérations d’allocation :

```yaml
# elasticsearch.yml

# On définit un attribut "az" qui a pour valeur le nom de l’AZ de l’instance
# Exemple dans le cas où l'instance est créée dans l'AZ eu-west-1a

node.attr.az: eu-west-1a

cluster.routing.allocation.awareness.attributes: az
```

### Taguer ses ressources
Si vous optez pour une architecture avec des nœuds spécialisés, n’oubliez pas d’ajouter un tag pour repérer ces spécificités.

Cela permet de faciliter la découverte des nœuds master lorsqu’un nouveau nœud entre dans le cluster.

Le plugin [EC2 Discovery](https://www.elastic.co/guide/en/elasticsearch/plugins/master/discovery-ec2-usage.html) permet d’ajouter cette logique dans le fichier `elasticsearch.yml`.

## Votre aventure ne fait que commencer
Dans cet article, nous avons présenté les différentes architectures disponibles, de la simple architecture homogène à l’architecture hot-warm avec nœuds master dédiés.

À titre d’exemple, nous avons récemment mis en place pour un client, un cluster hot-warm de 8 nœuds qui comporte 2 nœuds master dédiés, 2 nœuds hot (r5.2xlarge avec un disque EBS io2 de 2 To) et 4 nœuds warm dont un voting-only (r5.xlarge avec un disque EBS st1 de 5 To) pour une volumétrie de logs d’un peu moins de 1 To par semaine.

Les données sont conservées sur les nœuds hot pendant 2 semaines puis sont déplacées sur les nœuds warm pour des périodes de rétention de 1 ou 3 mois en fonction de la volumétrie des index.

Avec ces grands principes en tête, vous devriez avoir suffisamment d’éléments pour pouvoir prendre les bonnes décisions pour la création ou l’évolution de votre cluster.

D’autres optimisations sont encore possibles, notamment l’intégration de nœuds spécialisés pour le machine learning, voire la création de nœuds supplémentaires pour la gestion des phases cold et frozen.

source : https://blog.ippon.fr/