---
title: Comment installer Apache 2.4 (httpd) sous CentOS 7
description: Application
published: true
date: 2023-04-21T10:46:12.085Z
tags: app, apache, httpd, centos
editor: markdown
dateCreated: 2023-04-21T10:46:07.946Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Aucun

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installer Apache

Installer le package Apache `httpd` :


<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
yum install httpd
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Vérifier que le serveur Web est fonctionnel

Apache ne démarre pas automatiquement sur CentOS une fois l'installation terminée. Il faut démarrer le processus Apache manuellement :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl start httpd
```

Vérifier que le service est en cours d'exécution avec la commande suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl status httpd
```

Le statut `"active"` est présent lorsque le service est en cours d'exécution :

<span style="color:darkseagreen;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Redirecting to /bin/systemctl status httpd.service
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2019-02-20 01:29:08 UTC; 5s ago
     Docs: man:httpd(8)
           man:apachectl(8)
 Main PID: 1290 (httpd)
   Status: "Processing requests..."
   CGroup: /system.slice/httpd.service
           ├─1290 /usr/sbin/httpd -DFOREGROUND
           ├─1291 /usr/sbin/httpd -DFOREGROUND
           ├─1292 /usr/sbin/httpd -DFOREGROUND
           ├─1293 /usr/sbin/httpd -DFOREGROUND
           ├─1294 /usr/sbin/httpd -DFOREGROUND
           └─1295 /usr/sbin/httpd -DFOREGROUND
```

Ouvrir votre navigateur et saisir dans la barre d'adresse : `http://your_server_ip`

La page Web par défaut de CentOS 7 Apache doit apparaître comme ci-dessous :


![apache_http_server_centos8.png](/assets/img/apps/apache_http_server/apache_http_server_centos8.png =x500)

Cette page indique que le serveur web Apache fonctionne correctement. Il contient également des informations de base sur les fichiers importants et les emplacements de répertoires.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Activer Apache au démarrage du système

Par défaut, le service Apache `httpd` n'est pas démarrer automatiquement au démarrage du serveur. Pour y remédier, il faut exécuter la commande suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl enable httpd
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Created symlink from /etc/systemd/system/multi-user.target.wants/httpd.service to /usr/lib/systemd/system/httpd.service.
```


