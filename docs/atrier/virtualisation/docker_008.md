---
title: Docker 2
description: 
published: true
date: 2023-06-10T06:45:20.217Z
tags: 
editor: markdown
dateCreated: 2023-06-10T06:45:20.217Z
---

# Docker

## Installation de Docker

### Ubuntu / Debian

Follow the steps below and install Docker on Ubuntu 20.04 using the method of your choice

> Important: Make sure to remove any older Docker installations before installing a new one. Removing previous Docker versions doesn't delete the images, containers, volumes, or networks you have created. Run the following command to uninstall previous versions:

```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

**Step 1: Update the Package Repository**

Run the following command to update the system's package repository and ensure the latest prerequisite packages are installed:

```
$ sudo apt update
```

**Step 2: Install Prerequisite Packages**

The apt package manager requires a few prerequisite packages on the system to use packages over HTTPS. Run the following command to allow Ubuntu to access the Docker repositories over HTTPS:

```
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

The command above:

- Allows `apt` to transfer files and data over https.
- Allows the system to check security certificates.
- Installs `curl`, a data-transfer utility.
- Adds scripts for software management.

**Step 3: Add GPG Key**

A GPG key verifies the authenticity of a software package. Add the Docker repository GPG key to your system by running:

```
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

The output should state OK, verifying the authenticity.

**Step 4: Add Docker Repository**

Run the following command to add the Docker repository to apt sources:

```
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```

The command adds the official Docker repository and updates the package database with the latest Docker packages.

**Step 5: Specify Installation Source**

Execute the `apt-cache` command to ensure the Docker installation source is the Docker repository, not the Ubuntu repository. The `apt-cache` command queries the package cache of the apt package manager for the Docker packages we have previously added.

Run the following command:

```
$ sudo apt-cache policy docker-ce
```

The output states which version is the latest in the added source repository.

**Step 6: Install Docker**

Install Docker by running:

```
$ sudo apt install docker-ce -y
```

Wait for the installation process to complete.

**Step 7: Check Docker Status**

Check if Docker is installed, the daemon started, and the process is enabled to start on boot. Run the following command:

```
$ sudo systemctl status docker
```

**How to Use Docker on Ubuntu**

All Docker information, including the syntax, options, and commands, is available by running the docker command in the terminal:

```
$ sudo docker
```

Start using Docker by downloading Docker images, creating containers, and managing Docker volumes.

> Important: Docker commands can only be run with the sudo prefix on Ubuntu.


## Hello World

Démarrer un conteneur Hello-World

```
$ sudo docker run hello-world
```


## Commandes

|Action|Commande|
|------|--------|
|Télécharger une image sans la lancer|`$ sudo docker pull hello-world`|
|Lister les conteneurs en cours|`$ sudo docker ps`|
|Lister tous les conteneurs|`$ sudo docker ps -a`|
|Lister les images|`$ sudo docker images`|
|Rechercher une image Docker|`$ sudo docker search --stars=5 nginx`|
|Obtenir la liste des commandes du client Docker|`$ sudo docker help`|
|Obtenir de l'aide sur les options d'une commmande|`$ sudo docker help nom_de_la_commande`|
|Connexion à un compte Docker Hub|`$ sudo docker login`|
|Déconnexion du compte Docker Hub|`$ sudo docker logout`|
|Récupération d'une image dans sa dernière version|`$ sudo docker pull ubuntu:latest`|
|Afficher les images Docker présentes sur la machine|`$ sudo docker images`|
|Lancement sans option d'un conteneur|`$ sudo docker run ubuntu`|
|Lister les conteneurs actifs|`$ sudo docker ps`|
|Lister tous les conteneurs, y compris les inactifs|`$ sudo docker ps -a`|
|Supprimer un conteneur arrêté|`$ sudo docker rm loving_lovelace`|
|Lancer un conteneur en mode interactif|`$ sudo docker run -i -t ubuntu`|
|Trouver la liste des modifications d'un conteneur par rapport à son image de lancement|`$ sudo docker diff [identifiant du conteneur]`|
|Persister l'état d'un conteneur en une nouvelle image|`$ sudo docker commit [identifiant ou non du conteneur] [nom de l'image]`|
|Supprimer une image|`$ sudo docker rmi [nom de l'image]`|
|Supprimer tous les conteneurs|`$ sudo docker rm ``docker ps -a -q`` `|
|Supprimer plusieurs images|`$ sudo docker rmi [image 1] [image 2] ... [image n]`|
||``|
||``|
||``|
||``|
||``|