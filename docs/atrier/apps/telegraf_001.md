---
title: Configuration de Telegraf
description: 
published: true
date: 2023-04-21T10:49:50.092Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:49:45.937Z
---

====== Configuration de Telegraf ======
----

Par défaut le fichier de configuration de **Telegraf** ce situe dans le chemin suivant : **''/etc/telegraf/telegraf.conf''**

===== Création et édition du fichier de configuration =====

Il est important de modifier le fichier de configuration avant de démarrer le service afin de spécifier les entrées / sorties que l'on désire.

On peut créer une configuration en ligne de commande en spécifiant l'entrée désirée via l'argument ''-input-filter'' et définir le type de sortie avec l'argument ''-output-filter''.

Dans l'exemple ci-dessous, on créé un fichier de configuration appelé ''telegraf.conf'' avec 2 entrées: la première sera la lecture des métriques a propos de l'utilisation du CPU (cpu) et l'autre avec la lecture de l'utilisation mémoire (mem). La sortie est redirigé vers une base de type InfluxDB.

<code>telegraf -sample-config -input-filter cpu:mem -output-filter influxdb > telegraf.conf</code>

==== Démarrer le service ====

  * Sur un linux avec SysInit) :

<code>service telegraf start</code>

  * Sur un Linux avec SystemD :

<code>systemctl start telegraf</code>

===== Résultat =====

Dès que telegraf est démarré et fonctionnel, il va commencé à collecter des données et les écrire vers la sortie désirée.

Il sera alors possible de faire des requêtes vers l'outil désigné en sortie.