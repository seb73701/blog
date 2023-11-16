---
title: Eviter le chaos dans un environnement Kafkaïen
description: 
published: true
date: 2023-07-03T21:18:13.689Z
tags: 
editor: markdown
dateCreated: 2023-07-03T21:18:13.689Z
---

# Eviter le chaos dans un environnement Kafkaïen

Non ne partez pas ! Je vous promets qu’à la fin de cet article vous serez libre.  

Si vous comprenez comment fonctionne Apache Kafka, vous serez libre de vos choix et éviterez ainsi le chaos.

La légende raconte qu’Apache Kafka est nommé ainsi par Jay Kreps (à ce moment-là CEO de LinkedIn) car c’est un “système optimisé pour l’écriture”, Jay Kreps est également un fervent lecteur de Franz Kafka. Nous pourrions approuver cette légende sans se poser de questions si nous ne connaissions pas l’écrivain.

![crumb-kafka.jpg](../images/apps/apache_kafka/crumb-kafka.jpg)

Franz Kafka est un auteur moderne réputé pour ses textes caractérisés par une atmosphère cauchemardesque et sinistre. Ses œuvres sont vues comme le symbole de l'homme déraciné des temps modernes. Franz Kafka a averti sa première fiancée, Felice Bauer, que si elle voulait partager le reste de sa vie avec lui, ce serait "une vie monastique, côte à côte, avec un homme agité, mélancolique, silencieux, insatisfait et maladif".

Peut-être Jay Kreps, supposait-il quelque chose en nommant la plateforme de streaming distribuée de cette manière ?

Néanmoins, Franz Kafka, tout au long de sa vie, a su passionner les lecteurs et il est reconnu aujourd’hui comme un écrivain majeur du XXème siècle. Apache Kafka s’est immiscé au milieu des frameworks/plateformes Big Data et est devenu incontournable aujourd’hui lorsque vous souhaitez gérer des Gigaoctets de données par seconde. Voyons quelques concepts et quelques manières d’améliorer votre (futur) bus de messages préféré.

Si vous ne connaissez pas encore Apache Kafka, je vous redirige vers les articles existants du blog Ippon, vous trouverez de très bonnes explications sur son fonctionnement !

Gardez en tête que ce n’est “rien de plus” qu’un commit log distribué sur plusieurs machines, tout cela grâce au principe de partitionnement.

Nous allons voir plus en détails comment configurer nos producteurs de messages et nos topics pour éviter le chaos dans notre architecture distribuée.

## Réplication
Au sein d’un cluster Kafka se trouvent plusieurs brokers (en fait des serveurs) chargés de répartir uniformément le nombre de partitions pour chaque topic, qui, eux, constituent une séquence ordonnée et immuable de messages.

Pour le bon fonctionnement de Kafka (et c’est toute la différence entre un système de queueing et de publish-subscribe), le système va garder chaque message sur le disque et lui appliquer une rétention réglée par défaut sur 168 heures soit 7 jours grâce à la propriété `log.retention.hours = 168`. Autrement dit, la donnée est persistée dans les disques de chaque broker et permet un très haut débit en termes d'écriture et de lecture.

À noter, qu'ici, Kafka utilise l'écriture sur disque et non en RAM pour des raisons de coûts des composants (imaginez stocker des terra-octets en RAM...), mais également car les écritures sont optimisées grâce au [principe de Zero-Copy](https://en.wikipedia.org/wiki/Zero-copy) et grâce à l'écriture séquentielle ([sequential IO](https://docs.confluent.io/platform/current/kafka/design.html)).

Maintenant, imaginez votre cluster Kafka en production, il est superbe ! Vous avez mis 3 brokers, un topic avec un facteur de réplication de 3 (à renseigner lors de la création du topic), rien ne peut vous arriver...

En fait si, imaginez que vous écriviez dans un topic, et que le broker qui héberge la partition leader (≠ followers) tombe juste après avoir reçu votre acquittement (ACK) : la donnée, peut ne pas avoir été encore répliquée sur les autres brokers.

Dans un environnement à haut débit, critique et temps réel (banking/trading etc...), cela peut être problématique de perdre cette précieuse donnée !


Du coup, dans ce cas-là, on aimerait plutôt recevoir un non-acquittement (NACK) de la part du système non ? Histoire d'avoir un système cohérent et pouvoir gérer au cas par cas ces problèmes.

## Acquittement
Kafka est une plateforme optimisée pour la faible latence et le haut débit, par défaut, l'acquittement (ACK) est configuré pour n'être envoyé que lorsque le leader reçoit et traite la donnée (comme le cas ci-dessus) grâce à la propriété `acks = 1`

Si vous décidez, par excès de folie, de mettre la propriété `acks = 0`, aucune garantie ne peut être faite que le broker a reçu le message. Si un problème survient, les messages, une fois envoyés, ne seront pas réémis. Ce paramètre offre une latence plus faible et un débit plus élevé au prix d'un risque beaucoup plus élevé de perte de message.

Afin d'assurer la durabilité de la donnée au sein de notre cluster, il est préférable de configurer la propriété comme suit acks=all. Ainsi, une fois que le leader reçoit les acquittements de tous les autres brokers, indiquant qu'ils ont répliqué le message, il renverra un acquittement au producteur. Cela garantit que l'enregistrement ne sera pas perdu tant qu'au moins un réplica synchronisé reste actif.

> 👨‍💻 Reprenons notre cluster en production, j'envoie un message dans mon topic, ma donnée va être répliquée sur les 3 brokers, mais il ne m'enverra un acquittement qu'une fois la donnée répliquée partout ! Ça devrait couvrir notre cas ci-dessus ?



Oui, mais ça ne suffit pas...

Il existe une notion d'ISR (in-sync replica) qui représente tous les réplicas d'une partition qui sont "synchronisés" avec le leader. Au minimum, l'ISR comprendra le réplica leader et tous les réplicas followers supplémentaires qui sont également considérés comme synchronisés. Les followers répliquent les données du leader en envoyant des demandes de synchronisations périodiques, par défaut l'ISR est positionné sur 1 grâce à la propriété `min.insync.replicas=1` et ceci toujours dans un but d'avoir un système à faible latence et à haut débit, car en augmentant le nombre d'ISR, chacun des brokers va devoir se synchroniser pour vérifier la réplication du message.

Dans notre cas, nous appliquerons une valeur de 2 à la propriété `min.insync.replicas` ce qui veut dire que si 1 réplica tombe parmi les 3, grâce à notre replication factor de 3 (chaque broker héberge une copie de chaque partition du topic) et grâce à `acks=all`, on pourra enfin recevoir un acquittement (ACK) qui validera les règles minimales de haute disponibilité que nous nous sommes données.

Si, par malchance, 2 de vos brokers tombent pendant l'envoi de votre message dans le topic, alors un non-acquittement (NACK) accompagné d'une exception `NotEnoughReplicas`, vous indiquera que votre message n'a pas pu être délivré et répliqué sur nos brokers.

> 👨‍💻 Bon là j'ai un super système haute disponibilité, faible latence et haut débit ! On part en prod ?

Pas si vite...

## Stratégie de retry
Dans la plateforme Kafka, par défaut, il existe un système de retry qui peut vous être très très utile !

Avant la version 2.1, le système existait mais était désactivé par défaut. Depuis, cette version, le système de retry est configuré sur la valeur `Integer.MAX_VALUE` (2147483647) via la propriété `retries=2147483647` dans votre producteur et est donc activé par défaut.

Une erreur "retryable" est une erreur transitoire :

- de type TimeoutException,
- de sockets,
- Réélection d'un leader pour le topic.

Dans les pannes évoquées ci-dessus, il vous sera utile d'utiliser la stratégie de retry (à condition qu'elle soit activée via la propriété ci-dessus...) afin de ne pas perdre votre donnée.

> 👨‍💻 Je suis en 2.1, donc je n'ai rien à faire, ils sont forts les développeurs de Kafka, ils ont pensé à tout !

En effet, ce levier va nous permettre de garantir la durabilité de la donnée dans notre pipeline.

Mais il y a un mais... (encore ?????)

L'ordre n'est plus garanti et l'unicité non plus. 😩

## Idempotence
Imaginez cette fois-ci que vous envoyiez un message dans votre topic, tout se passe bien, il est répliqué... Mais lorsque le broker va vouloir envoyer l'acquittement (ACK), un Timeout a lieu. Votre donnée est dans le cluster, mais le producteur, ayant reçu un non-acquittement, va réessayer d'envoyer le message. Et cette fois-ci, ça fonctionne.

On se retrouve ainsi avec notre donnée en double dans notre cluster 🤯


Pour corriger ceci, nous allons pouvoir activer la propriété `enable.idempotence=true` dans le producteur.

Le producteur va s'assurer qu'exactement une seule copie du message existe dans le topic (en injectant un UUID dans le message, qui permet au producteur de vérifier s'il a déjà été traité ou pas, que ça soit sur les partitions leaders ou followers).

> 👨‍💻 On peut partir en prod plus sereinement !

## Conclusion
Kafka est une plateforme avec des concepts clés et une très grande flexibilité de configuration.

Retenez bien que la plateforme est conçue, par défaut, pour avoir une latence faible et un haut débit de production/consommation.

Tous les systèmes distribués doivent faire des compromis entre garantir la cohérence, la disponibilité et la tolérance au partitionnement réseau ([théorème CAP](http://en.wikipedia.org/wiki/CAP_theorem)) :

Cohérence : chaque lecture reçoit l'écriture la plus récente ou une erreur,
Disponibilité : chaque lecture reçoit une réponse sans erreur, même s'il ne s'agit pas de l'écriture la plus récente,
Tolérance au partitionnement réseau : le système continue de fonctionner même si une partie du réseau tombe en panne.
D'après les [ingénieurs de LinkedIn](https://engineering.linkedin.com/kafka/intra-cluster-replication-apache-kafka) à l'origine du projet Kafka, la plateforme a été conçue dans le but de garantir la haute disponibilité et la cohérence des données, plutôt que la tolérance au partitionnement réseau en présumant qu'ils arrivent rarement au sein d'un datacenter.

Grâce à la configuration fine de Kafka, vous allez être en capacité de pouvoir déployer des clusters hautes-performances comme le font [Criteo en Europe](https://fr.slideshare.net/RicardoPaiva17/how-criteo-is-managing-one-of-the-largest-kafka-infrastructure-in-europe) ou [LinkedIn aux USA](https://engineering.linkedin.com/blog/2019/apache-kafka-trillion-messages).

N'hésitez pas à consulter la [riche documentation](https://kafka.apache.org/documentation/) d'Apache Kafka si vous souhaitez aller encore plus loin. 😎

source : https://blog.ippon.fr/