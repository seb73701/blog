---
title: Comment installer le serveur Web Apache sur CentOS 8
description: Application
published: true
date: 2023-04-21T10:46:18.555Z
tags: app, apache, httpd, centos
editor: markdown
dateCreated: 2023-04-21T10:46:14.278Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Aucun

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation d'Apache


Installer le package Apache `httpd` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
dnf install httpd
```


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Réglage du pare-feu

Ouvrir les ports 80 et 443 :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
```
 
Recharger le pare-feu pour appliquer ces nouvelles règles :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
firewall-cmd --reload
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Vérification de votre serveur Web

Apache ne démarre pas automatiquement sur CentOS. Par conséquent, il faut démarrer le processus Apache manuellement :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
systemctl start httpd
```
 
Utiliser la commande suivante pour vérifier si le service s'exécute :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl status httpd
```
 
Le statut deviendra `active` une fois que le service sera en cours d'exécution :

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>  
  
```shell
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disa>
   Active: active (running) since Thu 2020-04-23 22:25:33 UTC; 11s ago
     Docs: man:httpd.service(8)
 Main PID: 14219 (httpd)
   Status: "Running, listening on: port 80"
    Tasks: 213 (limit: 5059)
   Memory: 24.9M
   CGroup: /system.slice/httpd.service
           ├─14219 /usr/sbin/httpd -DFOREGROUND
           ├─14220 /usr/sbin/httpd -DFOREGROUND
           ├─14221 /usr/sbin/httpd -DFOREGROUND
           ├─14222 /usr/sbin/httpd -DFOREGROUND
           └─14223 /usr/sbin/httpd -DFOREGROUND

```

Ouvrir la page d'accueil par défaut d'Apache pour confirmer que le logiciel fonctionne correctement grâce à l'adresse IP du serveur :

```
http://your_server_ip
```

Si l'on connait pas l'adresse IP du serveur, il est possible d'exécuter la commande :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
ip addr show enp0s3 | grep -w inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

> **`"enp0s3"`** étant le nom de l'interface réseau.  
{.is-info}

  
Ce qui donne :

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span> 

```
10.0.2.3
```

La page web Apache suivante doit apparaître par défaut CentOS 8 :

![apache_http_server_centos8.png](../images/apps/apache_http_server/apache_http_server_centos8.png ':size=x500')

Cette page indique qu'Apache fonctionne correctement. Elle contient également des informations de base sur les fichiers et les emplacements de répertoires importants.

