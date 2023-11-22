---
title: Présentation ProxySQL
description: 
published: true
date: 2023-04-21T10:48:56.470Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:52.475Z
---

ProxySQL est un proxy dédié à MySQL. Il est performant avec une architecture multicore et il est conçu pour recevoir des centaines de milliers de connexions simultanées, multiplexées vers potentiellement des centaines de serveurs.

ProxySQL permet :
  * **Mise en cache des requêtes** (met en cache les requêtes mais aussi les résultats pour une période configurable).
  * **Configuration avancée sans coupure** (//Le système de configuration de ProxySQL est inspiré des routeurs. Vous pouvez tout configurer de manière dynamique, conserver la configuration et la modifier, le tout sans coupure//).
  * **Prise en charge de topologie avancée** (//Proxy en cascade pour plus de disponibilité et de flexibilité. Prise en charge des topologies MySQL complexes, impliquant la réplication et le basculement ou la mise en miroir de requêtes. Tout cela a été exécuté sans effort par ProxySQL.//)
  * **Routage des requêtes** (//Prenons un cas avancé, où différentes classes de requêtes doivent être routées vers différents clusters MySQL, avec différentes configurations. Résolvez-le avec le concept de groupe hôte de ProxySQL. Basé sur un moteur de correspondance avancé, il est capable de router les requêtes de manière transparente vers le cluster de destination afin de les exécuter de manière plus efficace.//)
  * **Proxy de couche d'application** (//ProxySQL ne transfère pas le trafic aveuglément. Il comprend le protocole MySQL et agit en conséquence. C'est pourquoi il peut facilement servir à des cas d'utilisation avancés, tels que des transactions difficiles ou à la génération de statistiques détaillées en temps réel sur la charge de travail.//)
  * **Pare-feu**  (//En cas de requêtes fautives qui posent des problèmes à la base de données (injection SQL ou récupération inefficace d'informations via SELECT * sans WHERE, par exemple), ProxySQL agit en tant que portier entre l'application et la base de données, permettant ainsi aux administrateurs de base de réagir rapidement.//)
  * **Prend en charge le basculement** (//Bien que ProxySQL n'offre pas de fonctionnalité de prise en charge de basculement, il collabore de manière fluide avec les outils existants qui l'activent. Il surveille la santé des serveurs avec lesquels il communique et peut les éviter temporairement en fonction de taux d'erreur configurables.//)

## Fonctionnalités

  * **LICENSE**
    * OpenSource
  * **LOAD BALANCING** 
    * Application Layer balancer
    * Weighted Balance
  * **TOPOLOGIES**
    * MySQL Master/Slave Replication
    * MySQL Group Replication (//external script//)
    * MySQL NDB Cluster (//external script//)
    * Galera Replication (//external script//)
    * Custom membership (//external script//)
  * **PROTOCOLS**
    * MySQL native protocol
    * MySQL compression
    * Backend SSL encryption
  * **CONFIGURATION**
    * Local config file
    * Local reconfiguration
    * Remote reconfiguration
    * Zero downtime reconfiguration
    * Online upgrade (SO_REUSEPORT)
  * **CONNECTION MANAGEMENT**
    * Connection Pooling
    * Backend connections limit
    * Frontend connections limit
    * Per user connections limit
    * Connection Multiplexing
    * Timeout in millisecond
  * **PERSISTENT CONNECTION**
    * Transaction tracking
    * User tracking
    * Schema tracking
    * Charset tracking
    * Session variables tracking
  * **ROUTING**
    * Queries read/write split (//regex//)
    * Port based routing
    * Query based routing (//regex//)
    * Hint based routing
  * **SHARDING**
    * Schema based sharding
    * User based sharding
    * Query based sharding (//regex//)
    * Hint based sharding
  * **QUERIES MANAGEMENT**
    * Rewrite
    * Recursive rewrite
    * Timeout
    * Retry on failure
  * **THROTTLING**
    * Query Throttling
  * **SECURITY**
    * Firewall
    * Data Masking
  * **MIRRORING**
    * Query mirroring
    * Query rewrite
    * Mirror statistics
  * **QUERY CACHING**
    * Caching by query
    * Caching by user
    * Caching by schema
    * Caching by hint
  * **PREPARED STATEMENTS**
    * General PS support
    * PS metadata caching
    * PS reuse
  * **LOGGING**
    * All queries logging
  * **EXPORT METRICS**
    * Locally
    * Remotely
    * Queryable (SQL)
    * Export to monitoring/trending tools (//graphite/prometheus/others//)
    * Queries response time 
    * Queries digests