---
title: Docker
description: 
published: true
date: 2023-06-08T20:32:21.945Z
tags: docker
editor: markdown
dateCreated: 2023-06-08T20:32:21.945Z
---

# Docker

## Installation Docker CE
---
Source : https://docs.docker.com/engine/install/centos/
https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user
https://docs.docker.com/engine/security/rootless/

### Désinstallation des anciennes versions :
---

```shell
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-engine
```

Images, containers, volumes, and networks stored in `/var/lib/docker/` aren’t automatically removed when you uninstall Docker.

---
### Install using the repository
---
#### Set up the repository

```shell
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

**1. Install Docker Engine**

```
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

If prompted to accept the GPG key, verify that the fingerprint matches `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`, and if so, accept it.

**2. Start Docker**

```
systemctl start docker
```

Verify that Docker Engine installation is successful by running the hello-world image.

```
docker run hello-world
```

This command downloads a test image and runs it in a container. When the container runs, it prints a confirmation message and exits.

```
docker
```

lister les contaiers présent sur le serveur :

```
docker ps
```

[root@rocky8 ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES


pour télécharger une image en local (ici l'image alpine dans sa dernière version): 

```
docker pull alpine
```

```
[root@rocky8 ~]# docker pull alpine
Using default tag: latest
latest: Pulling from library/alpine
63b65145d645: Pull complete
Digest: sha256:ff6bdca1701f3a8a67e328815ff2346b0e4067d32ec36b7992c1fdc001dc8517
Status: Downloaded newer image for alpine:latest
docker.io/library/alpine:latest
```


pour lancer un conteneur : 

```
docker run alpine:latest
```

