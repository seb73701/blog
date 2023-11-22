---
title: Redémarrer une instance Redmine
description: 
published: true
date: 2023-04-21T10:49:26.864Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:49:23.381Z
---

====== Redémarrer une instance Redmine ======
----
===== Explication =====


Pour redémarrer l'instance "redmine", il faut :

  * Se positionner dans le dossier redmine :

<code>[root@server]# cd /var/www/html/redmine/</code>

  * Créer le fichier ''tmp/restart.txt'' :

<code>[root@server]# touch tmp/restart.txt</code>

<bootnote warning>Le chemin est bien **''tmp/''** et non **''/tmp/''**.</bootnote>

  * Appeler une page du redmine (project / issue) qui détectera le fichier et fera un redémarrage sans indisponibilité de l'url.