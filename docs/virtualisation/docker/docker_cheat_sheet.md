# Docker Cheat Sheet

## Créer et gérer des images

Lance la création de l'image Docker

```shell
docker build -t <image>
```

Affiche l'ensemble des images locales

```shell
docker images
```

Supprimer une image locale `<image>`

```shell
docker rmi <image>
```

Renomme une image locale `<src>` avec un nouveau nom et un nouveau tag `<dest>`

```shell
docker tag <src> <dest>
```

Récupère des images depuis un registry Docker

```shell
docker pull
```

Pousse des images Docker vers un registry

```shell
docker push
```

## Lancer et gérer des conteneurs

Lance un conteneur en arrière-plan

```shell
docker run -it -d <image>
```

Affiche l'ensemble des conteneurs

```shell
docker ps
```

Arrête un conteneur

```shell
docker stop <container>
```

Force la destruction d'un conteneur

```shell
docker rm -f <container>
```

Affiche les logs du conteneur

```shell
docker logs -f <container>
```

Permet de lancer un prompt BASH dans le conteneur cible

```shell
docker exec -ti <container> bash
```

## Débugueur

Donne les informations système de Docker (versions, espace consommé, etc...)

```shell
docker info
```

Afficher les logs d'un conteneur

```shell
docker logs <container>
```

Donne toutes les informations techniques d'un conteneur

```shell
docker inspect <container>
```

Stoppe tous les conteneurs actifs

```shell
docker stop $(docker ps -aq)
```

Supprime tous les conteneurs stoppés

```shell
docker rm $(docker ps -aq)
```

Purge les données non utilisées par Docker

```shell
docker system prune
```

## Dockerfile

Définit l'image source `<src>` à utiliser comme base
  
```dockerfile
FROM <src>
```

Exécute des commandes qui permettent de modifier votre image Docker

```dockerfile
RUN <cmd>
```

Copie un fichier, dossier ou une url `<src>` vers le chemin cible `<dest>`

```dockerfile
ADD <src> <dest>
```

Similaire à la commande `ADD`, mais ne supporte pas les url

```dockerfile
COPY <src> <dest>
```

Définit des variables d'environnement disponibles lors du build ainsi que lors de l'utilisation du conteneur

```dockerfile
ENV <key>=<value>
```

Définit les points de montages disponibles entre l'host et le conteneur

```dockerfile
VOLUME <dest>
```

Définit les ports entre l'host et le conteneur

```dockerfile
EXPOSE <port>
```

Définit le répertoire de travail du conteneur

```dockerfile
WORKDIR <dest>
```

Définit un préfixe pour chaque commande utilisées lors d'un `docker run`

```dockerfile
ENTRYPOINT <cmd>
```

Définit l'utilisateur qui doit faire tourner l'ensemble des processus du conteneur

```dockerfile
USER <user>
```

Définit la commande à utiliser par défaut lors de l'exécution d'un conteneur

```dockerfile
CMD <cmd>
```
