---
title: Le réseau dans Docker
description: 
published: true
date: 2023-04-21T11:18:14.278Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:18:10.348Z
---

# Le réseau dans Docker
> Cet article pratique a pour but d’expliquer les aspects réseaux dans Docker. J’aborderai la connectivité des conteneurs, mais également la répartition de charge et l’auto-découverte des services, au travers de quelques exemples simples.

## 1. Prérequis pour les exercices pratiques
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

## 2. Réseau pour un conteneur
### 2.1 Connectivité
Par défaut, lors de l’installation de Docker, il y a 3 réseaux différents préconfigurés.

On peut le vérifier avec la commande suivante :
```
$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
482781d4611a   bridge    bridge    local
1e18b65d843d   host      host      local
e23e38b91afe   none      null      local
```


### 2.1.1 Réseau bridge
Lorsqu’on crée un conteneur sans préciser le réseau utilisé, par défaut, c’est le réseau bridge qui est utilisé. On peut le vérifier avec les commandes suivantes :

```
$ docker run -d --name=nginx nginx:alpine
f22924b298e832e391ae89ed5c92b5f3ce0947e1d743dc65a2bea6f534e3a58d
$ docker inspect nginx | grep -A 1 Networks
            "Networks": {
                "bridge": {
...
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
```

Ce réseau bridge permet de fournir un réseau, 172.17.0.0/16 par défaut, sur lequel seront connectés les conteneurs, ainsi qu’une passerelle par défaut, 172.17.0.1, gérée par l’ordinateur sur lequel est installé Docker pour accéder au reste du réseau et éventuellement à Internet.

Si on démarre un deuxième conteneur, on peut voir l’adresse IP de ce deuxième conteneur et voir la connectivité avec ce premier conteneur.

```
$ docker run -it --name=alpine alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
Digest: sha256:234cb88d3020898631af0ccbbcca9a66ae7306ecd30c9720690858c1b007d2a0
Status: Downloaded newer image for alpine:latest
/ # ip a show eth0
130: eth0@if131: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # ping 172.17.0.2 -c 2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.118 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.206 ms
--- 172.17.0.2 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.118/0.162/0.206 ms
/ # wget -O- 172.17.0.2
Connecting to 172.17.0.2 (172.17.0.2:80)
writing to stdout
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
 
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
 
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
-                    100% |********************************|   612 0:00:00 ETA
written to stdout
```

Ici, on a donc réussi à démarrer un deuxième conteneur sur le même réseau et à valider la connectivité, d’abord en faisant un ping puis en faisant une requête HTTP sur le premier conteneur qui était un serveur web nginx.

On peut aussi valider l’accès Internet depuis ce conteneur, avec la commande suivante :

```
/ # wget -O- wttr.in/Moon
Connecting to wttr.in (5.9.243.187:80)
writing to stdout
                        ------.    
                         . .   `--.    
                                 . `-.    
                           @@@@@      `-.    
                           @@@@@@   .    \    
                           @@@@@@       . \.    
                           @@@@@@@   O      \    
                            @@@@@@@@     @@@ \    
                            @@@@@@@@@@ o @@@@|    
                            @@@@@@@@@@    @@ \     New Moon +
                             @@@@@@@@ @@@@    |     6 10:20:58
                              @@   . @@@@@@@ |     First Quarter -
                               @@@   @@@@@@@ |     0 22:32:55
                             . @@@@   @@@@ o /    
                                @@   .       |    
                            O    .     o   . /    
                               .    .       /    
                           .      .   .-. /'    
                                     `-' /    
                              O   .   .-'    
                              .    .-'    
                               .--'    
                        ------'    
 
 
Follow @igor_chubin for wttr.in updates
-                    100% |********************************| 16766 0:00:00 ETA
written to stdout
```

Ici, l’accès Internet de mon ordinateur, sur lequel j’ai installé Docker, est utilisé. La technique utilisée est en fait du NAT (Network Address Translation) pour réutiliser l’adresse IP de mon ordinateur.

### 2.1.2 Réseau none
Si on essaye maintenant de lancer un conteneur avec le paramètre --net=none, on obtient un conteneur avec aucune interface réseau, donc aucune adresse IP et donc aucune connectivité avec les autres conteneurs ou le réseau externe, comme testé ci-dessous :

```
$ docker run -it --name=alpine_none --net=none alpine
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: tunl0@NONE: <NOARP> mtu 1480 qdisc noop state DOWN qlen 1000
    link/ipip 0.0.0.0 brd 0.0.0.0
3: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN qlen 1000
    link/tunnel6 00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00 brd 00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00
/ # ping 8.8.8.8 -c 2
PING 8.8.8.8 (8.8.8.8): 56 data bytes
ping: sendto: Network unreachable
/ # wget -O- www.google.fr
wget: bad address 'www.google.fr'
```

L’utilisation de ce réseau est, en général, pour les conteneurs n’ayant pas besoin d’accès réseau ou pour tester un nouveau conteneur sans risquer une propagation via le réseau.

### 2.1.3 Réseau host
Si on essaye maintenant de lancer un conteneur avec le paramètre --net=host, on obtient un conteneur avec une interface réseau qui a, sur Linux, la même adresse IP que l’ordinateur sur lequel Docker a été installé. Sous macOS ou Windows avec Docker Desktop, c’est l’adresse IP de la mini VM Linux qui porte la partie Docker.

>Attention 
>Si vous souhaitez vous connecter à la mini VM fournie par Docker Desktop, vous pouvez utiliser la commande suivante :
>
>```
>$ docker run -it --rm --privileged --pid=host justincormack/nsenter1
>```

Du coup, on peut tester la connectivité en créant, par exemple, un serveur web sur un conteneur et en y accédant depuis l’ordinateur portant le Docker Engine, comme testé ci-dessous :

```
$ docker run -it --name=alpine_host --net=host alpine
/ # ip a show eth0
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 65513 qdisc pfifo_fast state UP qlen 1000
    link/ether 02:50:00:00:00:01 brd ff:ff:ff:ff:ff:ff
    inet 192.168.65.3/24 brd 192.168.65.255 scope global dynamic noprefixroute eth0
       valid_lft 5636sec preferred_lft 4196sec
    inet6 fe80::50:ff:fe00:1/64 scope link
       valid_lft forever preferred_lft forever
/ # { echo -e 'HTTP/1.1 200 OK\r\n'; echo "Ca marche"; } | nc -v -l -p 80
listening on [::]:80 ...
```

On peut maintenant voir ce pseudo serveur web, démarré depuis le conteneur, depuis l’ordinateur portant le Docker Engine, comme testé ci-dessous :

```
/ # netstat -puant | grep 80
tcp        0      0 :::80                   :::*                    LISTEN      10469/nc
/ # wget -O- localhost
Connecting to localhost ([::1]:80)
writing to stdout
Ca marche
-                    100% |********************************|    10 0:00:00 ETA
written to stdout

```
On voit le process lancé par la commande nc dans le conteneur, visible depuis l’ordinateur portant le Docker Engine, et la connectivité en utilisant localhost, comme si la commande avait été lancée depuis le même ordinateur.

L’utilisation de ce réseau est surtout pour les conteneurs avec des commandes bas niveau, nécessitant d’avoir accès à la même pile réseau que l’ordinateur sur lequel il s’exécute.

### 2.1.4 Réseau personnalisé
On peut aussi créer ses propres réseaux personnalisés pour isoler des conteneurs entre eux, comme dans l’exemple ci-dessus :

```
$ docker network create lp    
3f2c5d36e8c5a3b645e3873b49781c6f5688c5de73bd103960d4cfc9be253721
$ docker run -it --name=alpine_custom --net=lp alpine
/ # ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2): 56 data bytes
^C
--- 172.17.0.2 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss
/ # wget -O- 172.17.0.2 -T 1
Connecting to 172.17.0.2 (172.17.0.2:80)
wget: download timed out
/ # ip a show eth0
137: eth0@if138: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever
/ # wget -O- wttr.in
Connecting to wttr.in (5.9.243.187:80)
writing to stdout
Weather report: Paris, France
 
      \   /     Sunny
       .-.      27 °C          
    ― (   ) ―   ← 15 km/h      
       `-’      10 km          
      /   \     0.0 mm         
```

Dans cet exemple, on peut voir que lorsqu’on crée un réseau avec la commande docker network create lp et qu’on y attache un conteneur au lancement, avec l’argument --net=lp, on constate que le nouveau conteneur n’a pas de connectivité vers les conteneurs attachés aux autres réseaux, en effet il est dans un autre réseau indépendant, ici le réseau 172.18.0.0/16. Il a cependant accès à Internet.

### 2.2 Accès depuis l’extérieur
Nous avons vu dans le paragraphe précédent comment accéder à un serveur web nginx conteneurisé depuis d’autres conteneurs. Maintenant nous allons voir comment accéder à ce même serveur depuis l’extérieur.

Pour cela, nous allons utiliser du PAT (Port Address Translation). En effet, nous allons exposer le port 80 du conteneur au reste du réseau en le faisant correspondre à un port sur l’ordinateur exécutant le Docker Engine.

Nous allons d’abord nettoyer notre environnement en arrêtant et supprimant tous les conteneurs précédemment exécutés, puis nous allons exécuter notre conteneur nginx avec le paramètre --publish=80.

```
$ docker rm -f $(docker ps -aq)
d820314267fe
904e0836a625
3c8fb02d8a64
 
$ docker run -d --name nginx --publish=80 nginx:alpine
2b3a18f409a3934e9305f58508d156a45e0da25fb6826469e8dfc1e375a3375e
 
$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                   NAMES
2b3a18f409a3   nginx:alpine             "/docker-entrypoint.…"   54 seconds ago   Up 51 seconds   0.0.0.0:63397->80/tcp   nginx
 
$ lsof -i :63397 -s TCP:LISTEN +c0
COMMAND             PID    USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
com.docker.backend 1387 gmorini   72u IPv6 0x48b94c974cd9a19f      0t0 TCP *:63397 (LISTEN)
 
$ curl localhost:63397
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
 
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
 
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Nous voyons dans cet exemple que le port 80 de notre conteneur a été transformé en port 63397 sur notre ordinateur, et que l’on peut y accéder directement au travers de la commande curl ou d’un navigateur.

On peut aussi préciser le port qui doit être utilisé avec la même commande en préfixant le port du conteneur par le port local sur l’ordinateur et en les séparant par un :, comme sur l’exemple ci-dessous :

```
$ docker run -d --name nginx --publish=8080:80 nginx:alpine
3fec39d5ed2f24ac0c120506cce0bb9158f9e706d0443d2d9aaba0175017e90e
 
$ docker ps                                                
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                      NAMES
3fec39d5ed2f   nginx:alpine   "/docker-entrypoint.…"   6 seconds ago   Up 3 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx
 
$ curl localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>
 
<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>
 
<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```


## 3. Connectivité entre conteneurs sur plusieurs nœuds
Pour augmenter la redondance de l’infrastructure de conteneurs, on utilise souvent plusieurs machines avec un Docker Engine et on forme un cluster (Docker Swarm) entre ces machines, pour plus de détails vous pouvez lire un de mes précédents articles sur le sujet [1].

### 3.1 Architecture cluster
Pour les prochains exemples, j’ai initialisé un cluster Docker Swarm en utilisant le site web « Play with Docker » [PWD].

```
$ docker node ls
ID                            HOSTNAME   STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
sflug8irk677qtaqqkruov53e *   manager1   Ready     Active         Reachable        20.10.0
xg08b6nntulndkabfxyvfpacb     manager2   Ready     Active         Reachable        20.10.0
zgee9vho9g7mk9bi1291jqobh     manager3   Ready     Active         Leader           20.10.0
l03i0i9vqld3kpxje0rbk6nl3     worker1    Ready     Active                          20.10.0
nkcw5i9zx669jc0zdzau74rt6     worker2    Ready     Active                          20.10.0
```

On voit donc les 3 managers et les 2 workers en statut Ready et Active.

Si on regarde maintenant les réseaux disponibles sur ce cluster, on retrouve les 3 réseaux décrits précédemment, ainsi que deux réseaux additionnels :

```
$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
83fbce476528   bridge            bridge    local
b2b5da16525a   docker_gwbridge   bridge    local
9a2e0b31a52f   host              host      local
ry5hrqxbevps   ingress           overlay   swarm
c7fda93d61e3   none              null      local
```

On voit un quatrième réseau, nommé docker_gwbridge ainsi qu’un cinquième nommé ingress qui lui utilise un driver overlay et n’a pas un scope local comme les précédents, mais un scope appelé swarm.

Le réseau nommé docker_gwbridge permet de connecter le Docker Engine aux autres Docker Engine sur les autres machines participant au cluster Docker Swarm.

Nous allons détailler le fonctionnement des réseaux overlay dans le prochain paragraphe.

### 3.2 Réseau overlay
Les réseaux overlay sont des réseaux étendus entre les différentes machines formant le cluster Docker Swarm pour prolonger un réseau/adressage entre plusieurs conteneurs ne s’exécutant pas forcément sur la même machine.

Vous trouverez un schéma représentant un réseau overlay dans la figure 1.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lphs-052/reseau_figure_01.png">

Fig. 1 : Schéma représentant un réseau overlay dans un cluster Docker Swarm de 2 machines.

De la même façon que pour les réseaux bridge personnalisés, on peut créer des réseaux overlay personnalisés. Ces réseaux vont fournir une connectivité entre les connecteurs connectés au même réseau et une connectivité vers l’extérieur.

```
$ docker network create -d overlay lp_net
d35gaoe66fqq8xustiiexkb87
$ docker network ls
NETWORK ID     NAME              DRIVER    SCOPE
83fbce476528   bridge            bridge    local
b2b5da16525a   docker_gwbridge   bridge    local
9a2e0b31a52f   host              host      local
ry5hrqxbevps   ingress           overlay   swarm
d35gaoe66fqq   lp_net            overlay   swarm
c7fda93d61e3   none              null      local
$ docker service create -d --publish=8080 --replicas=5 --name=web –network=lp_net stefanscherer/whoami
wolwpsa0g1d5bxxgpcarcdosd
$ docker service ls
ID             NAME      MODE         REPLICAS   IMAGE                         PORTS
wolwpsa0g1d5   web       replicated   5/5        stefanscherer/whoami:latest   *:30005→8080/tcp
$ docker service ps web
ID             NAME      IMAGE                         NODE       DESIRED STATE   CURRENT STATE                ERROR     PORTS
nlgffdkfbo2l   web.1     stefanscherer/whoami:latest   manager3   Running         Running 23 seconds ago             
m3kcl5u4zjcu   web.2     stefanscherer/whoami:latest   manager1   Running         Running 23 seconds ago             
lg5bgstu144e   web.3     stefanscherer/whoami:latest   worker2    Running         Running 23 seconds ago             
5pxfel3ro5dk   web.4     stefanscherer/whoami:latest   worker1    Running         Running 23 seconds ago             
utj92gl8k7tx   web.5     stefanscherer/whoami:latest   manager2   Running         Running 23 seconds ago             
$ docker ps
CONTAINER ID   IMAGE                         COMMAND   CREATED         STATUS           PORTS      NAMES
4c356f093240   stefanscherer/whoami:latest   "/http"   52 seconds ago   Up 48 seconds   8080/tcp   web.2.m3kcl5u4zjculpxoa68ccfynb
$ docker inspect web.2.m3kcl5u4zjculpxoa68ccfynb
...
            "Networks": {
                "ingress": {
                    "IPAMConfig": {
                        "IPv4Address": "10.0.0.39"
                    },
...
                },
                "lp_net": {
                    "IPAMConfig": {
                        "IPv4Address": "10.0.1.37"
                    },
...
```
Ici, on a lancé un service Docker Swarm avec l’image de conteneur stefanscherer/whoami, qui lance un serveur web qui écoute sur le port 8080 et renvoie des informations techniques comme l’ID du conteneur qui répond à la requête.

Vous trouverez un schéma représentant un exemple de déploiement d’un service sur un réseau personnalisé overlay avec l’exposition du port 8080 dans la figure 2.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lphs-052/reseau_figure_02.png">

Fig. 2 : Schéma représentant un déploiement d’un service sur un réseau overlay dans un cluster Docker Swarm de 3 machines.

Dans le déploiement réalisé ci-dessus, il y a plusieurs instances du service web, donc plusieurs conteneurs répartis sur plusieurs machines. Nous allons détailler le fonctionnement de la répartition de charge dans le prochain paragraphe.

### 3.3 Répartition de charge
Les services dans Docker Swarm permettent de répartir la charge entre plusieurs conteneurs fournissant le même service, c’est-à-dire la même image de conteneurs.

Sur le déploiement précédent, on peut faire plusieurs requêtes successives sur le service depuis un des nœuds du cluster.
```
$ curl localhost:30005
I'm 0928279b798e running on linux/amd64
...
$ curl localhost:30005
I'm 4d266b15d06e running on linux/amd64
...
```
On peut voir en effet que les requêtes arrivent sur différents conteneurs identifiés ici par leurs ID.

```
$ NET=`docker network ls | grep lp_net |awk '{print $1}'`
$ apk -U add ipvsadm
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/community/x86_64/APKINDEX.tar.gz
OK: 406 MiB in 154 packages
[manager1] (local) root@192.168.0.16 ~
$ nsenter --net=/var/run/docker/netns/lb_${NET:0:9} ipvsadm -L
IP Virtual Server version 1.2.1 (size=4096)
Prot LocalAddress:Port Scheduler Flags
  -> RemoteAddress:Port           Forward Weight ActiveConn InActConn
FWM 265 rr
  -> web.1.nlgffdkfbo2lrx961e5edx Masq    1      0          0         
  -> web.2.m3kcl5u4zjculpxoa68ccf Masq    1      0          0         
  -> web.3.lg5bgstu144etqvf9jm4t5 Masq    1      0          0         
  -> web.4.5pxfel3ro5dkorn10iau3d Masq    1      0          0         
  -> web.5.utj92gl8k7txc0vuu1umec Masq    1      0          0                  
```

En récupérant les 9 premiers caractères de l’ID du réseau overlay et en y préfixant « lb_ », on peut trouver le nom du namespace réseau utilisé pour cette répartition de charge. Ensuite, en utilisant la commande ipvsadm, on peut visualiser la table de répartition de charge pour notre service et voir qu’il s’agit d’un « round-robin » entre les 5 conteneurs, ou réplicas, du service, c’est-à-dire qu’il envoie une requête à chaque réplica dans un ordre séquentiel.

Vous trouverez un schéma représentant un exemple de répartition de charge lors de l’accès à ce service dans la figure 3.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lphs-052/reseau_figure_03.png">

Fig. 3 : Schéma de répartition de charge sur un service.

### 3.4 Auto-découverte de services (Service discovery)

À l’intérieur d’un cluster Docker Swarm, un autre des avantages est l’auto-découverte des services.

Cette découverte se fait en association avec le réseau overlay utilisé.

```
$ docker service create -t --network=lp_net guismo/mon_alpine
7tdf044lan702uk8t1p0hupor
overall progress: 1 out of 1 tasks
1/1: running   [==================================================>]
$ docker exec -it peaceful_jennings.1.lpx5147zaf282gm1ioho2bpzj bash
/ # curl web:8080
I'm 4c356f093240 running on linux/amd64
…
/ # ping web -c 2
PING web (10.0.1.35): 56 data bytes
64 bytes from 10.0.1.35: seq=0 ttl=64 time=0.095 ms
64 bytes from 10.0.1.35: seq=1 ttl=64 time=0.115 ms
ash-5.1# nslookup 10.0.1.37
37.1.0.10.in-addr.arpa name = web.2.m3kcl5u4zjculpxoa68ccfynb.lp_net.
 
Authoritative answers can be found from:
bash-5.1# ping web.2.m3kcl5u4zjculpxoa68ccfynb.lp_net -c 2
PING web.2.m3kcl5u4zjculpxoa68ccfynb.lp_net (10.0.1.37): 56 data bytes
64 bytes from 10.0.1.37: seq=0 ttl=64 time=0.270 ms
64 bytes from 10.0.1.37: seq=1 ttl=64 time=0.196 ms
 
--- web.2.m3kcl5u4zjculpxoa68ccfynb.lp_net ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.196/0.233/0.270 ms
```

Dans l’exemple ci-dessus, on lance un service de test basé sur une image alpine pour se connecter au service web précédemment lancé. On peut tester l’accès en utilisant une commande curl, mais cette fois ni sur l’adresse IP, ni depuis l’extérieur en passant par le port sur la machine, mais en utilisant simplement le nom du service, ici « web ».

On peut également utiliser le nom pour chacun des réplicas en utilisant le nom long du conteneur, au format « nom du service ».« numéro du réplica ».« id de la task ».

Ces informations peuvent être trouvées ci-dessous.

```
$ docker service ps web
ID             NAME      IMAGE                         NODE       DESIRED STATE   CURRENT STATE         ERROR     PORTS
nlgffdkfbo2l   web.1     stefanscherer/whoami:latest   manager3   Running         Running 2 hours ago             
m3kcl5u4zjcu   web.2     stefanscherer/whoami:latest   manager1   Running         Running 2 hours ago             
lg5bgstu144e   web.3     stefanscherer/whoami:latest   worker2    Running         Running 2 hours ago             
5pxfel3ro5dk   web.4     stefanscherer/whoami:latest   worker1    Running         Running 2 hours ago             
utj92gl8k7tx   web.5     stefanscherer/whoami:latest   manager2   Running         Running 2 hours ago             
$ docker ps
CONTAINER ID   IMAGE                         COMMAND   CREATED       STATUS       PORTS      NAMES
4c356f093240   stefanscherer/whoami:latest   "/http"   2 hours ago   Up 2 hours   8080/tcp   web.2.m3kcl5u4zjculpxoa68ccfynb
```

Cette auto-découverte de service est très utile pour pouvoir préconfigurer les connexions entre services, car le nom du service est prévisible, contrairement à l’adresse IP. Par exemple, lors de la connexion à une base de données, on peut personnaliser la configuration dans son application, car on sait que le hostname de la base sera le nom du service que l’on peut anticiper.

## Conclusion
Cet article vous a guidé à travers la découverte du réseau dans la technologie Docker, à travers des exemples aussi bien sur un nœud Docker individuel qu’à l’intérieur d’un cluster Docker Swarm.

J’espère que vous avez apprécié la démarche progressive de l’apprentissage.

On a vu dans cet article les avantages d’une solution comme Docker, qui simplifie la création et la gestion de réseaux, la répartition de charge sur plusieurs machines et l’auto-découverte de services au travers de l’auto-configuration du DNS.

Vous pourrez maintenant reproduire en conteneurs vos environnements les plus complexes avec segmentation réseau, juste avec quelques lignes de commandes, ou plus simplement dans un fichier Docker Compose que je n’ai pas pu aborder dans cet article, mais que je vous invite à creuser si vous souhaitez aller plus loin, avec cet article [2] par exemple.

## Références
- [PWD] Play with Docker labs : https://labs.play-with-docker.com
- [1] G. Morini, « Cluster et orchestration de conteneurs avec Docker Swarm », Linux Pratique Hors-Série n°47, janvier-février 2020 : https://connect.ed-diamond.com/Linux-Pratique/LPHS-047/Cluster-et-orchestration-de-conteneurs-avec-Docker-Swarm
- [2] Docker et ses networks : https://w3blog.fr/2016/09/20/docker-et-ses-networks/

source : https://connect.ed-diamond.com/linux-pratique/lphs-052/le-reseau-dans-docker

