---
title: Rancher Desktop - une alternative à Docker Desktop
description: 
published: true
date: 2023-04-21T10:45:22.512Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:45:18.525Z
---

# Rancher Desktop, une alternative à Docker Desktop ?


## Introduction
Le 31 Août 2021, Docker annonçait un changement au niveau des conditions d’utilisation de son application Docker Desktop. Pour rappel, Docker Desktop est une application pour les environnements Mac et Windows fournissant un ensemble de services permettant de créer, faire tourner des applications conteneurisées sur son poste. Le changement ? L'éditeur introduit désormais une souscription mensuelle rendant l’outil payant pour les grandes entreprises ayant plus de 250 employés ou ayant un chiffre d’affaires supérieur à 10 millions de dollars. L’application reste cependant gratuite pour les usages personnels et open source.

Suite à cette annonce, une question se pose. Des alternatives existent-elles à Docker Desktop ? La réponse ? Oui il y en a quelques unes ! On peut citer notamment Podman, Colima, Minikube ou encore Rancher Desktop. Aujourd’hui, je vous propose de découvrir Rancher Desktop !

## Présentation
Rancher Desktop est une application open source pour Mac, Windows et Linux développée en TypeScript avec le framework Electron. L’application permet d’avoir un environnement Kubernetes en local et de le configurer facilement. Pour rappel, Kubernetes est une plateforme d’orchestration de conteneurs. Vous avez la possibilité de choisir la version de votre choix et de naviguer facilement d’une version à une autre.

En outre, elle permet également la gestion de conteneurs et d’images. Vous pouvez build, pull, push des images et lancer des conteneurs avec le container runtime de votre choix : dockerd ou containerd. De plus, Rancher Desktop est compatible avec Dockerfile et Docker Compose.

Dans cet article, je me concentrerai seulement sur la partie gestion de conteneurs. Il sera plus intéressant d’évoquer la partie Kubernetes dans un autre article.

<img src="https://blog.ippon.fr/content/images/2022/07/rancher_desktop.png">

Rancher Desktop fonctionne en utilisant une machine virtuelle sur laquelle vous aurez une distribution Kubernetes et un container runtime pour créer et faire fonctionner vos conteneurs. La machine virtuelle est gérée par Lima/QEMU sur MacOs et Linux et WSL2 pour Windows. L’application embarque l’outil de ligne de commande kubectl et le package manager Helm pour gérer les clusters Kubernetes. Elle intègre également les outils de ligne de commande nerdctl et Docker CLI pour communiquer avec le container runtime choisi (containerd/dockerd).

L’application fournit également une interface graphique permettant de changer facilement votre version de Kubernetes, le container runtime, les ressources de la VM, les options de port forwarding et la gestion de vos images.

<img src="https://blog.ippon.fr/content/images/2022/07/kubernetis_settings.png">

## Installation
Voyons maintenant comment installer Rancher Desktop sur votre poste.

### Sous macOS
Si vous êtes sur un Mac, il n’y pas de manipulations spécifiques à faire. Assurez-vous juste d’avoir :

- La version macOS Catalina 10.15 ou supérieure
- Le processeur Apple Silicon(M1) ou Intel avec VT-x

### Sous Windows
Si vous êtes sur Windows, quelques manipulations peuvent être nécessaires. Assurez-vous d’avoir :

- La version Windows 10 build 1909 supérieure ou Windows 11
- WSL2 (Windows Subsystem for Linux) activé. Pour plus de détail, dirigez vous sur ce lien https://docs.microsoft.com/fr-fr/windows/wsl/install
- L’option de virtualisation activée au niveau du BIOS. En fonction du processeur, AMD-V ou Intel VT-x

### Sous Linux
Si vous êtes sur Linux assurez vous d’avoir :

- Une distribution pouvant installer des AppImages ou paquets debian, rpm
- L’option de virtualisation activée au niveau du BIOS. En fonction du processeur, AMD-V ou Intel VT-x
- Les droits de lecture et d'écriture sur `/dev/kvm`. Pour plus de détails, dirigez vous sur ce lien https://docs.rancherdesktop.io/getting-started/installation#linux

Si les prérequis ont bien été réalisés, dirigez-vous sur le site officiel de Rancher Desktop  https://rancherdesktop.io pour télécharger le fichier d’installation adapté à votre environnement préféré (Mac, Windows).

## Configuration
Si vous avez déjà Docker Desktop sur votre poste, assurez-vous de le stopper. Rancher Desktop et Docker Desktop ne peuvent pas fonctionner en même temps. Après l’installation, lorsque vous lancez Rancher desktop pour la première fois, Kubernetes est activé par défaut et containerd choisi en tant que container runtime.

<img src="https://blog.ippon.fr/content/images/2022/07/rancher_desktop_start_config.png">

Dans cet article nous nous concentrerons seulement sur la partie conteneur, vous pouvez donc décocher l’activation de Kubernetes. Cela permettra de réduire la consommation de ressource par Rancher Desktop. Choisissez également dockerd au lieu de containerd. Nous verrons plus en détail par la suite la différence entre les deux. Ces options ne sont pas définitives, vous pourrez les changer à tout moment.

Si l’installation s’est bien passée, vous devriez avoir l’interface ci-dessous :

<img src="https://blog.ippon.fr/content/images/2022/07/rancher_desktop_start.png">

Après quelques minutes d’initialisation (le chargement peut être assez long la première fois), vous êtes prêts pour lancer vos premiers conteneurs !

## Utilisation
Nous avons vu précédemment que nous avions le choix entre containerd et dockerd lors de la configuration mais quelle est la différence entre les deux ? Pour résumer rapidement, ce sont des containers runtime, ils gèrent les images, le cycle de vie complet des conteneurs mais aussi la partie stockage et réseau.

Containerd, originellement dans le projet Docker, en a été extrait pour devenir un projet open source réutilisable par d’autres projets tels que Docker lui-même et Kubernetes. Pour lancer vos conteneurs, il est utilisé avec nerdctl, un outil de ligne de commande équivalent à docker.

Dockerd quant à lui, basé sur le projet Moby, intègre l’API et la CLI Docker vous permettant d’utiliser les commandes Docker. Il a été ajouté récemment à Rancher Desktop pour combler les commandes docker non couvertes par nerdctl.

Regardons à présent comment lancer un conteneur avec Rancher Desktop en fonction du container runtime choisi. Vérifiez si Rancher Desktop est lancé et mettez-vous sur un terminal.

### Dockerd
En choisissant dockerd, lors de la configuration, il n’y a pas de changement au niveau des commandes Docker. Vous pouvez build, pull des images et lancer des conteneurs comme vous le faisiez sur Docker Desktop.

Essayons de builder une image nginx à partir de ce Dockerfile :

```yaml
FROM nginx:latest

```
puis lancer la commande:

```
docker build -t mon-image .
```

Lançons maintenant notre conteneur avec notre image en l’exposant sur le port 8080 :

```
docker run -d -p 8080:80 mon-image
```

En naviguant sur http://localhost:8080, vous devriez tomber sur la page d’accueil de Nginx :

<img src="https://blog.ippon.fr/content/images/2022/07/welcome_nginx.png">

Pas de changement également avec Docker Compose. Essayez de lancer les conteneurs décrits par ce fichier `docker-compose.yml` :

```yaml
version: "3.9" 
services:
  db:
    container_name: my_db_container
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: my_db
    ports:
      - "3307:3306"
  web:
    container_name: my_nginx_container
    image: nginx
```

avec la commande :

```
docker compose up -d
```

En vérifiant avec la commande `docker ps`, vous devriez avoir maintenant vos deux conteneurs mysql et nginx en cours d’exécution :

<img src="https://blog.ippon.fr/content/images/2022/07/docker_ps.png">

Et voilà ! Vous avez lancé vos premiers conteneurs avec Rancher Desktop ! Voyons maintenant comment cela se passe avec containerd.

### Containerd
Containerd était de base, le seul container runtime proposé par Rancher Desktop avant l’ajout récent de dockerd. Il s’utilise avec nerdctl, un outil de ligne de commande qui reprend la structure des commandes docker. La plupart des commandes docker sont couvertes et il supporte dockerfile et docker compose.

Essayons de faire la même chose que dans la partie précédente avec la commande `nerdctl`. Builder votre image avec le même Dockerfile :

```
nerdctl build -t mon-image .
```

Lançons le conteneur :

```
nerdctl run -d -p 8000:80 mon-image
```

Vous devriez avoir également la page d’accueil de Nginx en naviguant sur http://localhost:8080

Pour Docker Compose, `nerdctl` a un équivalent qui est `nerdctl-compose`. Lancez la commande suivante avec le même fichier `docker-compose.yml` :
```
nerdctl compose up -d
```

En vérifiant avec la commande `nerdctl ps`, vous devriez voir vos deux conteneurs lancés !

L’utilisation avec Docker Compose reste néanmoins perfectible selon la configuration du fichier `docker-compose.yml`. Certaines options peuvent ne pas fonctionner. À vérifier selon votre configuration.

## Conclusion
Comme nous avons pu le voir, Rancher Desktop est une très bonne alternative à Docker Desktop. L’ensemble des fonctionnalités basiques sont couvertes, nous avons pu construire nos images et lancer sans problème nos conteneurs. La mise en route a été très simple. Il nous a juste suffi de lancer l’application et d’attendre l’initialisation avant de lancer nos commandes. Même si l’utilisation cible était destinée à Kubernetes, il est devenu un très bon outil pour juste lancer des conteneurs. Au niveau des défauts, on peut lui reprocher le manque d’un onglet de visualisation de logs sur l’interface graphique. Une fonctionnalité très pratique présente sur Docker Desktop, qui je l’espère sera ajoutée dans les prochaines versions. Rancher Desktop a en tout cas de l’avenir devant lui, avec une communauté très active, c’est un projet à suivre.

source : https://blog.ippon.fr/2022/07/04/rancher-desktop-une-alternative-a-docker-desktop/