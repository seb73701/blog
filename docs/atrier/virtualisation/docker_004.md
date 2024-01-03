---
title: Optimiser la taille de vos images Docker pour gagner en efficacité
description: 
published: true
date: 2023-04-21T11:18:08.322Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:18:04.306Z
---

# Optimiser la taille de vos images Docker pour gagner en efficacité

>Cet article pratique a pour but d’expliquer comment sont construites les images Docker et l’impact de leurs tailles. Je présenterai ensuite plusieurs techniques pour optimiser leurs tailles sur quelques exemples.

## 1. Les images Docker
### 1.1 Explication des principes
Les images de conteneurs utilisent un système de fichiers particulier (union-capable file system) comme OverlayFS, qui permet d’utiliser plusieurs couches dans le système de fichiers, un peu comme un oignon, ou comme vous pouvez le voir sur la figure 1 avec l’empilement des différentes couches.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lphs-052/opti_figure_01.png">

Fig. 1 : Représentation du système de fichiers d’un conteneur.


La création d’une image passe par l’utilisation d’un fichier appelé Dockerfile. Il s’agit d’un simple fichier texte qui décrit la façon de construire l’image. L’utilisation d’un fichier texte permet de rendre les images facilement compréhensibles et automatisables. Les images sont toujours créées à partir d’une image source (qui peut être vide) sur laquelle on ajoute des configurations (fichiers, variable d’environnement, etc.).

Lors de l’exécution d’un conteneur, les fichiers de l’image vont être utilisés, en lecture seule, et le Docker Engine va créer une nouvelle couche du système de fichiers qui sera, elle, en lecture/écriture. Le système de fichiers va alors utiliser le principe de Copy on Write (CoW), tel qu’illustré sur la figure 2.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lphs-052/opti_figure_02.png">

Fig. 2 : Explication du Copy on Write.

Cette dernière couche du système de fichiers est considérée comme éphémère, car elle ne sera pas sauvegardée en cas d’arrêt/redémarrage du conteneur.

### 1.2 Bénéfices de réduire la taille des images Docker
La taille des images Docker a plusieurs impacts :

- Lors du téléchargement des images, en plus de la latence et la bande passante disponible, le temps de téléchargement est directement lié à la taille des images.
- Une fois téléchargées, il faut stocker les images, que ce soit dans une registry ou dans le cache du nœud Docker sur lequel on exécute l’image. Le nombre d’images que l’on pourra stocker dépendra de leur taille.
- Lors de l’exécution et en particulier dans un cluster orchestré par Docker Swarm, Kubernetes ou d’autres solutions, lors d’un incident, un conteneur doit être redémarré sur un nouveau nœud. Le temps de redémarrage va dépendre directement du téléchargement et du chargement en mémoire de l’image qui sont également liés à la taille de l‘image. Ce temps a donc un impact sur le temps de retour à la situation nominale en cas d’incident.
- Lors des phases de développement et de tests de l’application, le temps de construction et d’exécution des conteneurs est un temps pendant lequel le développeur ne peut pas tester les résultats de ses modifications et est en attente. Il faut donc essayer de le limiter au maximum pour améliorer l’efficacité des développeurs.
- Enfin, d’un point de vue sécurité, la surface potentielle d’attaque sur le conteneur est d’autant plus réduite qu’il ne contient que le minimum d’outils et de librairies à l’intérieur de l’image de conteneurs.

Toutes ces raisons devraient vous pousser à essayer d’optimiser au mieux vos images de conteneurs.

### 1.3 Prérequis pour les exercices pratiques
Pour pouvoir reproduire les exercices pratiques de la suite de cet article, il faut que vous disposiez d’un ordinateur avec une architecture processeur compatible (comme x86_64).

Il faut ensuite disposer de l’outil Docker.

Si vous utilisez un système d’exploitation macOS ou Windows 10, je vous conseille d’installer Docker Desktop : https://www.docker.com/products/docker-desktop.

Si vous êtes sous Linux, vous devrez installer Docker Engine : https://docs.docker.com/engine/install/.

Suivez les étapes d’installation et faites un test avec la commande docker version dans un terminal. Ce test devrait normalement être concluant maintenant et vous devriez voir un résultat similaire à l’exemple ci-dessous :

```
$ docker version
Client:
Cloud integration: 1.0.17
Version:           20.10.7
API version:       1.41
Go version:        go1.16.4
Git commit:        f0df350
Built:             Wed Jun 2 11:56:22 2021
OS/Arch:           darwin/amd64
Context:           default
Experimental:      true
 
Server: Docker Engine - Community
Engine:
  Version:          20.10.7
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       b0f5bc3
  Built:            Wed Jun 2 11:54:58 2021
  OS/Arch:          linux/amd64
  Experimental:     false
containerd:
  Version:          1.4.6
  GitCommit:        d71fcd7d8303cbf684402823e425e9dd2e99285d
runc:
  Version:          1.0.0-rc95
  GitCommit:        b9ee9c6314599f1b4a7f497e1f1f856fe433d3b7
docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

### 1.4 Premiers pas sur quelques images
Pour un premier test, on va utiliser une première image publique « toute faite ». Pour cela, on va la télécharger avec la commande suivante :

```
$ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
b4d181a07f80: Pull complete
edb81c9bc1f5: Pull complete
b21fed559b9f: Pull complete
03e6a2452751: Pull complete
b82f7f888feb: Pull complete
5430e98eba64: Pull complete
Digest: sha256:47ae43cdfc7064d28800bc42e79a429540c7c80168e8c8952778c0d5af1c09db
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

On peut déjà voir sur cette simple commande que l’image de base nginx contient déjà 6 couches pour une image fournissant un simple serveur web.

On peut analyser sa taille avec la commande suivante :

```
$ docker image ls nginx
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
nginx        latest    4f380adfc10f   6 days ago   133MB
```

Cette image a une taille de 133 MB, soit beaucoup plus petite que la taille d’une machine virtuelle, mais encore assez grosse pour un conteneur.

Si l’on souhaite analyser plus en détail, il faudrait connaître la taille de chaque couche et son contenu.

Pour cela, on peut utiliser la commande suivante :

```
$ docker image history nginx
IMAGE          CREATED      CREATED BY                                      SIZE      COMMENT
4f380adfc10f   6 days ago   /bin/sh -c #(nop) CMD ["nginx" "-g" "daemon…   0B        
<missing>      6 days ago   /bin/sh -c #(nop) STOPSIGNAL SIGQUIT           0B        
<missing>      6 days ago   /bin/sh -c #(nop) EXPOSE 80                    0B        
<missing>      6 days ago   /bin/sh -c #(nop) ENTRYPOINT ["/docker-entr…   0B        
<missing>      6 days ago   /bin/sh -c #(nop) COPY file:09a214a3e07c919a…   4.61kB    
<missing>      6 days ago   /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB    
<missing>      6 days ago   /bin/sh -c #(nop) COPY file:0b866ff3fc1ef5b0…   1.96kB    
<missing>      6 days ago   /bin/sh -c #(nop) COPY file:65504f71f5855ca0…   1.2kB     
<missing>      6 days ago   /bin/sh -c set -x     && addgroup --sys…   63.9MB    
<missing>      6 days ago   /bin/sh -c #(nop) ENV PKG_RELEASE=1~buster     0B        
<missing>      6 days ago   /bin/sh -c #(nop) ENV NJS_VERSION=0.5.3        0B        
<missing>      6 days ago   /bin/sh -c #(nop) ENV NGINX_VERSION=1.21.0     0B        
<missing>      6 days ago   /bin/sh -c #(nop) LABEL maintainer=NGINX Do…   0B        
<missing>      6 days ago   /bin/sh -c #(nop) CMD ["bash"]                 0B        
<missing>      6 days ago   /bin/sh -c #(nop) ADD file:4903a19c327 …   69.3MB    
```

On peut voir ici les commandes issues du Dockerfile ayant permis de construire cette image nginx.

On identifie les commandes ayant un impact sur la taille de l’image. Si on commence par le bas, on voit d’abord l’ajout d’un ensemble de fichiers pour 69.3 MB. Ceci correspond à l’image de base utilisée dans le conteneur avec un ensemble de fichiers système.

Ensuite, la deuxième couche ayant un impact est celle commençant par la commande /bin/sh -c set -x && addgroup pour une taille de 63.9 MB. Pour avoir le détail de cette commande, on peut utiliser la commande suivante :

```
$ docker image history nginx --no-trunc | grep set
<missing>                                                                 6 days ago   /bin/sh -c set -x     && addgroup --system --gid 101 nginx     && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false --uid 101 nginx     && apt-get update     && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates     &&     NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62;     found='';     for server in         ha.pool.sks-keyservers.net         hkp://keyserver.ubuntu.com:80         hkp://p80.pool.sks-keyservers.net:80         pgp.mit.edu     ; do         echo "Fetching GPG key $NGINX_GPGKEY from $server";         apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break;     done;     test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1;     apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/*     && dpkgArch="$(dpkg --print-architecture)"     && nginxPackages="         nginx=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-xslt=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-geoip=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-image-filter=${NGINX_VERSION}-${PKG_RELEASE}         nginx-module-njs=${NGINX_VERSION}+${NJS_VERSION}-${PKG_RELEASE}     "     && case "$dpkgArch" in         amd64|i386|arm64)             echo "deb https://nginx.org/packages/mainline/debian/ buster nginx" >> /etc/apt/sources.list.d/nginx.list             && apt-get update             ;;         *)             echo "deb-src https://nginx.org/packages/mainline/debian/ buster nginx" >> /etc/apt/sources.list.d/nginx.list                         && tempDir="$(mktemp -d)"             && chmod 777 "$tempDir"                         && savedAptMark="$(apt-mark showmanual)"                         && apt-get update             && apt-get build-dep -y $nginxPackages             && (                 cd "$tempDir"                 && DEB_BUILD_OPTIONS="nocheck parallel=$(nproc)"                     apt-get source --compile $nginxPackages             )                         && apt-mark showmanual | xargs apt-mark auto > /dev/null             && { [ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; }                         && ls -lAFh "$tempDir"             && ( cd "$tempDir" && dpkg-scanpackages . > Packages )             && grep '^Package: ' "$tempDir/Packages"             && echo "deb [ trusted=yes ] file://$tempDir ./" > /etc/apt/sources.list.d/temp.list             && apt-get -o Acquire::GzipIndexes=false update             ;;     esac         && apt-get install --no-install-recommends --no-install-suggests -y                         $nginxPackages                         gettext-base                         curl     && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx.list         && if [ -n "$tempDir" ]; then         apt-get purge -y --auto-remove         && rm -rf "$tempDir" /etc/apt/sources.list.d/temp.list;     fi     && ln -sf /dev/stdout /var/log/nginx/access.log     && ln -sf /dev/stderr /var/log/nginx/error.log     && mkdir /docker-entrypoint.d   63.9MB
```

Ici, on peut observer la phase d’installation du package nginx et ses dépendances avec le gestionnaire de package apt sur l’image de base précédemment utilisée.

Pour résumer ce premier test, on peut voir que les images que l’on utilise sans les construire peuvent être plus ou moins optimisées et qu’il est intéressant de comprendre le processus utilisé pour les construire et les outils inclus dans ces images.

On va essayer avec un deuxième exemple sur une première application Java et une image de conteneur que l’on va construire.

Voici le code de notre application :

```
$ cat Hello.java
public class Hello {
  public static void main(String[] args) {
    System.out.println("Hello Linux Pratique !");
  }
}
```

Voici le Dockerfile :

```
$ cat Dockerfile
FROM openjdk
ADD Hello.java /app/Hello.java
WORKDIR /app
RUN javac Hello.java
ENTRYPOINT java Hello
```

On peut ensuite construire l’image et regarder sa taille :

```
$ docker build -t hello_java .
[+] Building 37.3s (10/10) FINISHED                                                                                                     
=> [internal] load build definition from Dockerfile                                            0.3s
=> => transferring dockerfile: 144B                                                            0.0s
=> [internal] load .dockerignore                                                               0.4s
=> => transferring context: 2B                                                                 0.0s
=> [internal] load metadata for docker.io/library/openjdk:latest                               5.5s
=> [auth] library/openjdk:pull token for registry-1.docker.io                                  0.0s
=> [internal] load build context                                                               0.4s
=> => transferring context: 157B                                                               0.0s
=> [1/4] FROM docker.io/library/openjdk@sha256:05eee0694a2ecfc3e94d29d420bd8703fa9dcc64755962e267fd5dfc22f23664     24.5s
=> => resolve docker.io/library/openjdk@sha256:05eee0694a2ecfc3e94d29d420bd8703fa9dcc64755962e267fd5dfc22f23664     0.2s
=> => sha256:05eee0694a2ecfc3e94d29d420bd8703fa9dcc64755962e267fd5dfc22f23664 1.04kB / 1.04kB       0.0s
=> => sha256:27ba2cda208b1ee9069476d6f6a0c8a9ddfa7353b271b0b1d3dae8c75cd4a3f2 954B / 954B           0.0s
=> => sha256:de085dce79ff05081b11406a4ce9b53bb624c565b7d0f888b3d5434803dfc454 4.45kB / 4.45kB       0.0s
=> => sha256:5a581c13a8b96af42c06955acd88fdb3aff53edcbdd33faa7bca9854fd8bfbaf 42.18MB / 42.18MB     3.9s
=> => sha256:66727af51578917387f3fdc9ad6ed2d9fe890d4ad0f4523afc868b58b2e832e1 184.80MB / 184.80MB  12.7s
=> => sha256:26cd02acd9c2b40dba18c3fe68882bcb5ebda0caeb5e550965d9ac81e3b55fbf 13.40MB / 13.40MB     2.0s
=> => extracting sha256:5a581c13a8b96af42c06955acd88fdb3aff53edcbdd33faa7bca9854fd8bfbaf       5.3s
=> => extracting sha256:26cd02acd9c2b40dba18c3fe68882bcb5ebda0caeb5e550965d9ac81e3b55fbf       1.6s
=> => extracting sha256:66727af51578917387f3fdc9ad6ed2d9fe890d4ad0f4523afc868b58b2e832e1       8.9s
=> [2/4] ADD Hello.java /app/Hello.java                                                        1.6s
=> [3/4] WORKDIR /app                                                                          0.8s
=> [4/4] RUN javac Hello.java                                                                  2.2s
=> exporting to image                                                                          1.3s
=> => exporting layers                                                                         1.1s
=> => writing image sha256:b5975f3b084e2ee908748e5dbe897ef7716af8bf6f8eb2caf4ecf02e138fcad8    0.0s
=> => naming to docker.io/library/hello_java                                                   0.0s
 
$ docker image ls hello_java
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
hello_java   latest    458bafb150f0   8 minutes ago   467MB
 
$ docker run -t hello_java
Hello Linux Pratique !
```

On obtient une image de 467 MB pour afficher un « hello world ».

## 2. Optimisation des images
### 2.1 Bien choisir son image de base
Nous allons reprendre le premier exemple utilisé ci-dessus et modifier l’image de base utilisée.

L’image nginx préconstruite utilisée dans cet exemple utilisait en image de base, une image Debian 10 minimaliste.

Voyons ce que l’on obtient si on remplace cette image de base par une image Alpine https://alpinelinux.org/. Pour cela, nul besoin de reconstruire soi-même l’image, voilà ce que l’on peut obtenir simplement en modifiant le tag utilisé pour télécharger l’image :

```
$ docker pull nginx:alpine
alpine: Pulling from library/nginx
540db60ca938: Pull complete
0ae30075c5da: Pull complete
9da81141e74e: Pull complete
b2e41dd2ded0: Pull complete
7f40e809fb2d: Pull complete
758848c48411: Pull complete
Digest: sha256:cc8c413c74aba9fef9dae7f3da736725136bad1e3f24fbc93788aea1944f51c4
Status: Downloaded newer image for nginx:alpine
docker.io/library/nginx:alpine
 
$ docker image ls nginx:alpine
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        alpine    a6eb2a334a9f   4 weeks ago   22.6MB
```

Ce simple ajout d’un tag sur l’image téléchargée nous a permis de passer de 133 MB à 22.6 MB pour le même serveur web. L’image de base a en fait été remplacée par Alpine à la place de Debian, les librairies utilisées ne sont pas forcément les mêmes, par exemple musl au lieu de glibc.

Ce changement sur les librairies peut provoquer des comportements différents dans des situations très spécifiques, mais la plupart du temps, il sera invisible d’un point de vue du service.

Le deuxième changement est sur les outils disponibles à l’intérieur du conteneur lorsqu’on a besoin de s’y connecter pour investiguer sur un éventuel problème lors du démarrage ou de l’exécution du conteneur. Cependant, il existe d’autres moyens pour investiguer sans nécessairement avoir à se connecter dans le conteneur, car cela peut impacter son fonctionnement et son intégrité. En effet, un conteneur doit normalement fonctionner sans aucune intervention manuelle au cours de son exécution, car il doit pouvoir être déclenché par un orchestrateur comme Docker Swarm ou Kubernetes.

### 2.2 Optimiser son process de build
Nous allons maintenant reprendre le deuxième exemple utilisé ci-dessus, sur la même application Java, et modifier la construction de notre application.

Pour cela, nous allons à la fois utiliser une image de base optimisée (Alpine) et une possibilité de Docker qui est le Multi-stage builds. Cela permet de compiler son code dans une image et d’en extraire l’exécutable et les librairies pour créer une image finale allégée sans les outils de compilation.

Ici, dans le cas de Java, nous allons, dans une première image de conteneurs, utiliser l’outil jlink pour créer un Java Runtime Environment (JRE) minimaliste. Nous allons ensuite compiler notre application. Enfin, il nous suffit d’exporter dans notre image de base optimisée (Alpine), le JRE minimaliste et le bytecode de notre application Java (le fichier .class) pour pouvoir l’exécuter, en utilisant le moins de ressources possible.

Voici ce que cela donne :

```yaml
$ cat Dockerfile                   
FROM alpine:latest as packager
ADD Hello.java /app/Hello.java
WORKDIR /app
RUN apk --no-cache add openjdk11-jdk openjdk11-jmods
ENV JAVA_MINIMAL="/opt/java-minimal"
# build minimal JRE
RUN /usr/lib/jvm/java-11-openjdk/bin/jlink \
    --verbose \
    --add-modules \
        java.base \
    --compress 2 --strip-debug --no-header-files --no-man-pages \
    --output "$JAVA_MINIMAL"
# build our application
RUN /usr/lib/jvm/java-11-openjdk/bin/javac Hello.java
 
FROM alpine:latest
ENV JAVA_HOME=/opt/java-minimal
ENV PATH="$PATH:$JAVA_HOME/bin"
COPY --from=packager "$JAVA_HOME" "$JAVA_HOME"
COPY --from=packager /app/Hello.class /app/Hello.class
WORKDIR /app
ENTRYPOINT ["java","Hello"]
 
$ docker build -t hello_java_opti .    
[+] Building 32.3s (14/14) FINISHED                                                                                                     
=> [internal] load build definition from Dockerfile                                            0.8s
=> => transferring dockerfile: 731B                                                            0.0s
=> [internal] load .dockerignore                                                               0.9s
=> => transferring context: 2B                                                                 0.0s
=> [internal] load metadata for docker.io/library/alpine:latest                                3.8s
=> [internal] load build context                                                               0.7s
=> => transferring context: 157B                                                               0.0s
=> [packager 1/6] FROM docker.io/library/alpine:latest@sha256:234cb88d3020898631af0ccbbcca9a66ae7306ecd30c9720690858c1b007d2a0    2.0s
=> => resolve docker.io/library/alpine:latest@sha256:234cb88d3020898631af0ccbbcca9a66ae7306ecd30c9720690858c1b007d2a0             0.2s
=> => sha256:234cb88d3020898631af0ccbbcca9a66ae7306ecd30c9720690858c1b007d2a0 1.64kB / 1.64kB  0.0s
=> => sha256:1775bebec23e1f3ce486989bfc9ff3c4e951690df84aa9f926497d82f2ffca9d 528B / 528B      0.0s
=> => sha256:d4ff818577bc193b309b355b02ebc9220427090057b54a59e73b79bdfe139b83 1.47kB / 1.47kB  0.0s
=> => sha256:5843afab387455b37944e709ee8c78d7520df80f8d01cf7f861aae63beeddb6b 2.81MB / 2.81MB  0.6s
=> => extracting sha256:5843afab387455b37944e709ee8c78d7520df80f8d01cf7f861aae63beeddb6b       0.2s
=> [packager 2/6] ADD Hello.java /app/Hello.java                                               0.7s
=> [packager 3/6] WORKDIR /app                                                                 0.8s
=> [packager 4/6] RUN apk --no-cache add openjdk11-jdk openjdk11-jmods                         7.6s
=> [packager 5/6] RUN /usr/lib/jvm/java-11-openjdk/bin/jlink     --verbose     --add-modules         java.base     --compress 2   8.3s
=> [packager 6/6] RUN /usr/lib/jvm/java-11-openjdk/bin/javac Hello.java                        2.3s
=> [stage-1 2/4] COPY --from=packager /opt/java-minimal /opt/java-minimal                      1.1s
=> [stage-1 3/4] COPY --from=packager /app/Hello.class /app/Hello.class                        0.9s
=> [stage-1 4/4] WORKDIR /app                                                                  0.7s
=> exporting to image                                                                          1.5s
=> => exporting layers                                                                         1.4s
=> => writing image sha256:26928303796d3c6e971f270dd62dd3b961a7404dea9fa09c04c898b60f5a7f24    0.0s
=> => naming to docker.io/library/hello_java_opti                                              0.1s
 
$ docker image ls             
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
hello_java        latest    b5975f3b084e   4 minutes ago    467MB
hello_java_opti   latest    26928303796d   22 minutes ago   40.2MB
$ docker run -t hello_java_opti    
Hello Linux Pratique !
```

Cette méthode nous a permis de passer d’une image de 467 MB à une image de 40.2 MB pour la même application Java de type « hello world », grâce à la modification de l’image de base et l’optimisation du process de build notamment grâce au multi-stage build et l’utilisation d’un JRE minimaliste.

### 2.3 Optimisation à l’extrême
Pour certains langages, on peut pousser ce processus à l’extrême. En effet, on peut compiler certains langages comme C ou Go en un exécutable auto porteur, qui n’a donc pas besoin de prérequis au-delà du noyau pour fonctionner. Il n’est donc pas nécessaire d’avoir certaines librairies comme un prérequis à l’exécution. Du coup, dans un format conteneur, on peut utiliser une image de base spécifique appelée scratch, qui ne contient aucun fichier.

Voici un exemple sur une application en C :

```c
$ cat hello.c
#include <stdio.h>
int main () {
  printf("Hello Linux Pratique !");
  return 0;
 
$ cat Dockerfile
FROM gcc as builder
ADD hello.c .
RUN gcc -static -Os hello.c -o hello
RUN strip --strip-all hello
FROM scratch
COPY --from=builder hello hello
CMD ["./hello"]
 
$ docker build -t hello_c .            
[+] Building 57.7s (10/10) FINISHED                                                                               
=> [internal] load build definition from Dockerfile                                                0.7s
=> => transferring dockerfile: 204B                                                                0.0s
=> [internal] load .dockerignore                                                                   0.8s
=> => transferring context: 2B                                                                     0.0s
=> [internal] load metadata for docker.io/library/gcc:latest                                       1.7s
=> [internal] load build context                                                                   0.7s
=> => transferring context: 118B                                                                   0.0s
=> [builder 1/4] FROM docker.io/library/gcc@sha256:a4804bac884c4341a84aa09e4e62acda526a195d85a423b8eaf58a            45.9s
=> => resolve docker.io/library/gcc@sha256:a4804bac884c4341a84aa09e4e62acda526a195d85a423b8eaf58a42802717b    0.2s
=> => sha256:a4804bac884c4341a84aa09e4e62acda526a195d85a423b8eaf58a42802717bb 1.43kB / 1.43kB      0.0s
=> => sha256:a72364918434e6f02899102ee451e1036a402243702e52b37d385f1eb2f8cd9f 2.22kB / 2.22kB      0.0s
=> => sha256:34fcef66284504b4b4497ae9c46644c9dda8fd7ecf23de2806dd8641539ad44d 9.40kB / 9.40kB      0.0s
=> => sha256:d28ba3bddf26336a2dff9ce3319ecd166971168469860f171fa659f62cb3ff6d 54.90MB / 54.90MB    5.3s
=> => sha256:bf411e1e4e47abd58ba697cdc3f8f769d452520d81804d6260ac2edb014fd41d 5.15MB / 5.15MB      1.2s
=> => sha256:ecab85705b51fb188c0567dca75cc18291fe81258917cbae6ed4d271ae1e1e1d 10.87MB / 10.87MB    1.7s
=> => sha256:d458df13ef560766d7247fad9531a5df62c87a39c070e22fc435608be267e463 54.57MB / 54.57MB    6.8s
=> => sha256:97953ded330d5d6a4f7e7f82423165481164c9cdadf4205485d343b53c0cf98a 196.42MB / 196.42MB  17.7s
=> => extracting sha256:d28ba3bddf26336a2dff9ce3319ecd166971168469860f171fa659f62cb3ff6d            4.6s
=> => sha256:fb3d00b829e6516af54a885c209d2774f29daf8a9c1a160d6d55e1ee5a916ceb 15.41kB / 15.41kB     5.7s
=> => sha256:4910eb151521343cc039bceb7b73ec4f5a46558f5cf2f95b8dfbce8f4c42ef2a 127.21MB / 127.21MB  16.3s
=> => sha256:795ac78071c76340a92d2968facb833d42ba704087d168cbecfb81e8543353bf 10.13kB / 10.13kB     7.2s
=> => sha256:9dc71acfb689cfae53b95400f0d1afae464abbd0b20f3e14f43e5ac815899cc1 1.88kB / 1.88kB       7.6s
=> => extracting sha256:bf411e1e4e47abd58ba697cdc3f8f769d452520d81804d6260ac2edb014fd41d            0.4s
=> => extracting sha256:ecab85705b51fb188c0567dca75cc18291fe81258917cbae6ed4d271ae1e1e1d            0.5s
=> => extracting sha256:d458df13ef560766d7247fad9531a5df62c87a39c070e22fc435608be267e463            4.9s
=> => extracting sha256:97953ded330d5d6a4f7e7f82423165481164c9cdadf4205485d343b53c0cf98a           10.4s
=> => extracting sha256:fb3d00b829e6516af54a885c209d2774f29daf8a9c1a160d6d55e1ee5a916ceb            0.0s
=> => extracting sha256:4910eb151521343cc039bceb7b73ec4f5a46558f5cf2f95b8dfbce8f4c42ef2a            8.8s
=> => extracting sha256:795ac78071c76340a92d2968facb833d42ba704087d168cbecfb81e8543353bf            0.0s
=> => extracting sha256:9dc71acfb689cfae53b95400f0d1afae464abbd0b20f3e14f43e5ac815899cc1            0.0s
=> [builder 2/4] ADD hello.c .                                                                      3.3s
=> [builder 3/4] RUN gcc -static -Os hello.c -o hello                                               1.5s
=> [builder 4/4] RUN strip --strip-all hello                                                        1.7s
=> [stage-1 1/1] COPY --from=builder hello hello                                                    0.5s
=> exporting to image                                                                               0.6s
=> => exporting layers                                                                              0.6s
=> => writing image sha256:bd277d6e4ef5ded4f247ebe4471a92c79351e4e8b6e080989d3539e0834c50eb         0.0s
=> => naming to docker.io/library/hello_c                                                           0.0s
 
$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
hello_c      latest    bd277d6e4ef5   40 seconds ago   707kB
 
$ docker run -t hello_c
Hello Linux Pratique !                                                                                          
```

Une fois l’application Java recodée en C, et en utilisant le multi-stage build et l’image de base scratch, on arrive à descendre à 707 kB pour notre image conteneur.

Ceci est la forme la plus optimisée, à l’extrême. Cependant, elle ne contient aucun outil pour aider à comprendre ce qu’il se passe si l’image de conteneur ne réagit pas comme cela est attendu. Il faut donc avoir d’autres moyens d’analyse, mais cela dépasse le cadre de cet article.

## Conclusion
Cet article vous a guidé à travers la découverte ou la redécouverte de la technologie Docker et en particulier des images Docker et de leurs optimisations avec quelques exemples simples pour comprendre les différentes approches et les options disponibles.

J’espère que vous avez apprécié la démarche progressive de l’apprentissage.

Dans cet article, je suis resté concentré sur les méthodes de construction d’images de conteneurs incluses dans Docker. Il existe également de nombreux autres outils permettant de construire ses images Docker sans utiliser la commande docker build comme par exemple : Jib, Kaniko, Bazel, Buildah, BuildKit, img, pouch…

Si vous souhaitez étendre ce sujet, je vous conseille de jeter un œil sur les Cloud Native Buildpacks https://buildpacks.io/ : ils permettent de créer une image de conteneurs sans avoir à écrire un Dockerfile, tout en intégrant de l’automatisation et des bonnes pratiques pour optimiser et sécuriser les images obtenues. Cet outil est même supporté dans des langages comme Spring Boot : https://spring.io/blog/2020/08/14/creating-efficient-docker-images-with-spring-boot-2-3.

Enfin, si vous voulez aller encore plus loin dans l’optimisation, je vous conseille cette série d’articles de blog par Jérôme Petazzoni, intitulée « Chérie, j'ai rétréci Docker » en 3 parties : https://enix.io/fr/blog/cherie-j-ai-retreci-docker-part1/.
  
source : https://connect.ed-diamond.com/linux-pratique/lphs-052/optimiser-la-taille-de-vos-images-docker-pour-gagner-en-efficacite