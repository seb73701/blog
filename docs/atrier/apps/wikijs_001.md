---
title: Comment créer un service pour démarrer Wiki.js au démarrage du serveur
description: Application
published: true
date: 2023-04-21T10:50:55.326Z
tags: app, wiki.js
editor: markdown
dateCreated: 2023-04-21T10:50:51.060Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir installé Wiki.js (image docker) grâce à la documentation officielle : https://docs.requarks.io/install/docker

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Création du service

Créer le fichier `"wikijs.service"` qui contiendra les informations du service.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
vim /etc/systemd/system/wikijs.service
```

Ajouter le contenu ci-dessous dans le fichier :


<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/systemd/system/wikijs.service*</span>
  
```ini
#########################
#    WIKIJS             #
#    SERVICE            #	
##########################

[Unit]
Description=Docker Compose WIKIJS Service
Requires=docker.service
After=docker.service network.target

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/wikijs
ExecStart=/usr/local/bin/docker-compose up -d
ExecStop=/usr/local/bin/docker-compose down
TimeoutStartSec=0

[Install]
WantedBy=multi-user.target
```


> **NOTE** : Il faut renseigner la variable `"WorkingDirectory="` avec le chemin du fichier `"docker-compose.yml"` de notre instance Wiki.js
{.is-info}


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Test du service

Démarrer le service `"wikijs.service"` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl start wikijs.service
```

Vérifier le statut du service `"wikijs.service"` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl status wikijs.service
```

Activer le service `"wikijs.service"` au démarrage du serveur :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl enable wikijs.service
```