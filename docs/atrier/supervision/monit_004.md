---
title: Basez votre supervision sur des logs de qualité avec Rsyslog
description: 
published: true
date: 2023-04-21T10:41:28.234Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:41:23.689Z
---

# Basez votre supervision sur des logs de qualité avec Rsyslog

> Les événements systèmes (aussi nommés logs dans la suite de l’article) sont des éléments déterminants pour la supervision du bon fonctionnement du système d’exploitation. Leur intérêt est souvent sous-coté aussi bien du point de vue maintenance du système que de sa sécurité. Cet article a pour ambition de poser les bases d’une bonne gestion des logs.

Les logs sont primordiaux pour l’administrateur dans la mesure où ils constituent les traces de l’activité du système. Ces logs sont générés par les processus en cours d’exécution sur le système. Ils sont normés par la RFC 5424 (Syslog). Il en existe cependant une plus historique, la 3164 (BSD Syslog). La différence essentielle entre les deux est que les en-têtes des messages sont plus complets dans la RFC 5424. Ces logs peuvent aussi constituer des preuves lors d’une procédure judiciaire. Par exemple, ceux initiés depuis un proxy web type Squid imputent les URL visitées à l’utilisateur. La justice peut s’appuyer sur ces preuves pour démontrer l’accès à du contenu illicite. Ces événements très sensibles peuvent être envoyés vers des serveurs de logs avec de grosses capacités de stockage et un accès filtré pour servir de séquestre. La qualité de la chaîne d’acquisition des logs conditionne leur éventuelle recevabilité.

## 1. Introduction aux concepts de Syslog
Les messages Syslog sont initiés par les processus en exécution sur le système. Ils comportent un certain nombre d’en-têtes définis par les deux RFC sus-nommées. Dans la RFC 3164, on retrouve la PRIORITY, un timestamp, le nom de la machine (HOSTNAME) où l’évènement est survenu et enfin le message propre à l’application (MSG). La PRIORITY est une valeur numérique déduite de deux paramètres (eux aussi numériques : FACILITY et SEVERITY). Le premier donne la famille du log (mail, FTP, NTP, UUCP, kernel, etc.). Attention, il s’agit de catégories généralistes. Par exemple, derrière la catégorie « mail », un serveur Postfix ou Qmail peut se cacher. Chaque famille est identifiée par une valeur normée par la RFC. La seconde est une indication sur le niveau de criticité du log (alert, erreur, notice, debug, etc.). Là encore, chaque niveau correspond à une valeur numérique. Pour calculer l’attribut PRIORITY, on procède de la façon suivante : FACILITY multiplié par 8 + SEVERITY. La RFC 5424 étend ces attributs en ajoutant le nom de l’application à l’origine du log (APP-NAME), le PID du processus (PROCID), le type du message (MSGID) et des données structurées (STRUCTURED-DATA). Les données structurées sont des couples clés/valeurs. Elles offrent la possibilité d’ajouter des métadonnées au message facilement exploitables par un analyseur syntaxique.

Chaque log émis par une application passe par 3 étapes lors de la réception : sélection, routage et stockage. La sélection consiste à se baser sur les en-têtes syslog pour choisir si le service Syslog va traiter ou non le message. Par exemple, on peut choisir de ne traiter que les logs ayant une SEVERITY supérieure à « notice » et ignorer les autres. Le routage définit si le log est à destination de l’instance Syslog locale à la machine et/ou à une autre instance localisée sur le réseau. En effet, les messages Syslog peuvent très bien transiter par le réseau pour être mis en sûreté sur un serveur distant dédié à cet usage de séquestre. Enfin, le périple s’achève avec le stockage du message sur le système de fichiers (traditionnellement dans /var/log). Vous avez maintenant les éléments théoriques pour appréhender la configuration de votre serveur Syslog. Dans cet article, nous allons travailler avec Rsyslog qui est l’implémentation par défaut du protocole Syslog sous Debian. Nous allons commencer par travailler en local sur la machine.

## 2. Rsyslog en local
Dans cette section, nous allons examiner la configuration de Rsyslog telle qu’elle est fournie par défaut sous Debian. Les messages syslog seront donc sélectionnés, routés et stockés sur la même machine. Notre machine est une Debian Buster nommée logclient.

### 2.1 Configuration locale de Rsyslog
Commençons par examiner un log typique. Je me connecte à la machine de logclient en SSH. Il y a donc un serveur sshd sur la machine et vraisemblablement les connexions sont tracées. Les logs de connexions sont stockés dans le fichier /var/log/auth.log. Voyons ce qu’il y a dedans :

```
root@logclient:~# grep sshd /var/log/auth.log
Nov 30 09:36:45 logclient sshd[437]: Server listening on 0.0.0.0 port 22.
Nov 30 09:37:59 logclient sshd[439]: Accepted password for root from 10.0.2.2 port 58494 ssh2
```

J’ai sélectionné deux messages. Le premier nous indique que le serveur sshd a démarré, qu’il a obtenu le PID 437 et qu’il écoute sur toutes les adresses de la machine. Vérifions :

```
root@logclient:~# ps -auxww | grep sshd
root       437 0.0 1.3 15852 6740 ?        Ss   09:36   0:00 /usr/sbin/sshd -D
root       439 0.0 1.5 16632 7888 ?        Ss   09:37   0:00 sshd: root@pts/0
```

Nous retrouvons bien le processus /usr/sbin/sshd avec le PID 437. Nous avons un second processus, correspondant au shell de ma session SSH. Ce shell possède le PID 439, ce qui est raccord avec la seconde ligne de log présentée plus haut. Ce log correspond en effet à la validation du couple identifiant/mot de passe utilisé pour ouvrir ce shell distant. Voyons maintenant comment rsyslogd, le démon derrière le service de log, alimente ce fichier. Cela se passe dans /,etc/rsyslog.conf (je me base sur le squelette par défaut fourni par le mainteneur du paquet Debian) :

```
root@logclient:~# cat /etc/rsyslog.conf
$FileOwner root
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
 
auth,authpriv.*                 /var/log/auth.log
```

Les quatre premières lignes sont des paramètres utilisés par rsyslogd lorsqu’il doit créer un fichier de log tel que auth.log. Il va positionner l’utilisateur propriétaire à root ($FileOwner) et le groupe à adm ($FileGroup). Le fichier va posséder les permissions lecture/écriture pour le propriétaire (c’est-à-dire root, car rsyslogd tourne avec cette identité) et lecture pour le groupe ($FileCreateMode 0640). Au cas où il s’agit d’un répertoire à créer, tous les droits sont donnés au propriétaire, les autres doivent se contenter des droits de lecture et de traversée ($DirCreateMode 0755). Enfin, le cinquième paramètre $Umask positionne le umask du processus rsyslogd pour tout ce qui est fichier temporaire. La valeur 0022 équivaut à tous les droits pour le propriétaire et lecture pour le reste du monde.

La dernière ligne est très simple. Le processus rsyslogd sélectionne les messages ayant les FACILITY authpriv ou auth. La différence entre les deux est que le premier concerne les messages d’autorisation non-système pouvant potentiellement contenir des mots de passe en clair (par exemple, un démon trop bavard sur les logs d’authentification, typiquement dans une phase de debug de l’authentification). Le second concerne des informations bien plus génériques sur les processus d’authentification et d’autorisation du système. Pour ces deux FACILITY, rsyslogd prend toutes les SEVERITY (*). Il n’y a aucune indication de routage, donc c’est cette instance de rsyslogd qui va stocker le message et il le fait dans /var/log/auth.log.

Voyons un second exemple avec une SEVERITY positionnée :

```
mail.info                       -/var/log/mail.info
mail.warn                       -/var/log/mail.warn
mail.err                        /var/log/mail.err
```

Ici, il y a trois SEVERITY (info, warn et err) pour la FACILITY mail, journalisées chacune dans leur propre fichier (mail.info, mail.warn et mail.err). Nous observons également un signe ‘-’ précédent l’emplacement du fichier de logs. Ce signe précise à rsyslogd de ne pas initier un appel système sync() à chaque écriture d’un log dans le fichier. C’est risqué en cas de crash de la machine, mais les performances sont accrues.

Le lecteur attentif aura peut-être relevé quelque chose d’étrange. À aucun moment dans les deux événements journalisés (démarrage du service sshd et connexion root), la PRIORITY n’apparaît. Cela est du au fait que le template par défaut pour le stockage des logs exclut de garder cette information. Le template est configuré de la façon suivante (toujours dans rsyslog.conf) :

```
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
```

Nous allons en définir un autre nommé priority et le définir par défaut :

```
$template priority,"%syslogfacility-text%:%syslogseverity-text%:%timegenerated%:%HOSTNAME%:%msg%\n"
$ActionFileDefaultTemplate priority
```

Nous définissons donc un nouveau template nommé priority (ligne 1) avec comme information à stocker : FACILITY et SEVERITY en mode texte lisible et non sous forme d’entier (%syslogfacility-text% et%syslogseverity-text%), le timestamp de génération du log (%timegenerated%), la machine où l’évènement a eu lieu (%HOSTNAME%) et enfin le message (%msg%). Puis, nous le positionnons comme template par défaut (ligne 2). Voyons le résultat en redémarrant le serveur rsyslogd puis le service sshd afin de générer un nouveau log :

```
root@logclient:~# systemctl restart rsyslog.service
root@logclient:~# systemctl restart sshd.service
root@logclient:~# cat /var/log/auth.log
auth:info:Dec 1 17:45:14:logclient: Server listening on 0.0.0.0 port 22.
```

Un nouveau log affichant le couple FACILITY auth et SEVERITY info a été généré au redémarrage du processus sshd. Le nom du service ainsi que le PID ont disparu (ils n’ont pas été définis dans le template). Pour conclure sur la configuration locale du service Rsyslog, j’attire l’attention du lecteur sur le fait que modifier le template n’est pas sans incidence sur le fonctionnement du système dans la mesure où il existe quantité d’outils qui analysent les fichiers de logs et qui s’attendent à les traiter dans un certain format. Du coup si on modifie le format de stockage des logs, l’outil ne verra plus rien.

### 2.2 Rotation des logs
Un point essentiel qui va de pair avec le stockage est la notion de rotation des logs. Nous avons vu que les logs sont ajoutés les uns à la suite des autres dans un ou plusieurs fichiers. Or, ces fichiers ne peuvent pas croître à l’infini sous peine de remplir l’espace disponible. C’est ici qu’intervient le principe de rotation. La rotation c’est le fait de prendre un fichier plein, faire quelque chose avec et mettre un nouveau fichier vide à disposition. La rotation peut être déclenchée sur des bases de périodicité (tous les jours, toutes les semaines, etc.) ou de taille de fichier (si supérieur à X Ko, Mo). L’idée est de définir une politique de gestion de l’archivage pour chaque fichier. Le composant s’occupant de cela se nomme logrotate. Voyons comment cela se configure pour notre fichier auth.log dans /,etc/logrotate.d/rsyslog :

```
root@logclient:~# cat /,etc/logrotate.d/rsyslog
/var/log/kern.log
/var/log/auth.log
/var/log/user.log
{
        rotate 4
        weekly
        missingok
        notifempty
        compress
        delaycompress
        sharedscripts
        postrotate
                /usr/lib/rsyslog/rsyslog-rotate
        endscript
}
```

D’après ce fichier, nous voyons que la même politique de rotation s’applique a kern.log, auth.log et user.log. Elle indique que nous gardons 4 fichiers d’archive (rotate 4). En conséquence, à la cinquième rotation le fichier le plus ancien est détruit. Cette rotation a lieu toutes les semaines (weekly). Ainsi, nous avons quatre semaines d’archives sur quatre fichiers différents. La rotation n’aura pas lieu si le fichier est vide (notifempty). Les archives sont compressées (compress). Le paramètre suivant delaycompress est utilisé lorsque des programmes ne peuvent pas être directement notifiés de fermer leur(s) fichier(s) de log pour en utiliser un/des nouveau(x). Ce paramètre est un peu hors sujet, car il aborde les problèmes de rafraîchissement de descripteurs de fichiers, ce qui est une notion méritant un article à elle seule. Enfin, le script rsyslog-rotate est invoqué afin que le démon rsyslogd rafraîchisse ses références (ou descripteurs pour les lecteurs pointus) vers les différents fichiers de logs à l’issue de la rotation.

## 3. Centralisation des logs
Cette section est le second gros morceau de l’article. La centralisation va consister à envoyer certains logs de logclient vers une autre machine nommée logserveur afin de les mettre en lieu sûr. Dans un premier temps, nous allons configurer l’envoi simple de logs via UDP puis TCP et regarder comment les ordonner côté serveur.

### 3.1 Envoi simple en UDP
Cette fonctionnalité de centralisation intervient au moment du routage du message Syslog. En effet, au lieu de définir un fichier cible, nous pouvons spécifier une machine vers laquelle relayer ce message. Évidemment, de l’autre côté, la machine doit accepter les messages entrants. C’est-à-dire qu’un service rsyslogd (ou autre) doit être à l’écoute (traditionnellement, sur le port 514 UDP). Voyons un peu les services en écoute sur nos deux machines :

```
root@logclient:~# netstat -laputn | grep 514
root@logserveur:~# netstat -laputn | grep 514
```

Rien n’écoute donc sur le port 514 UDP. Il va falloir configurer la machine logserveur pour qu’elle accepte les logs provenant de logclient. Allons sur logserveur faire les modifications nécessaires dans le fichier rsyslog.conf :

```
root@logserveur:~# cat /,etc/rsyslog.conf
module(load="imudp")
input(type="imudp" port="514")
```

Il faut juste ajouter (ou décommenter) ces deux directives qui 1) active l’écoute en UDP et 2) précise le port. On relance rsyslogd et on revérifie les services en écoute :

```
root@logserveur:~# systemctl restart rsyslog.service
root@logserveur:~# netstat -laputn | grep 514
udp        0      0 0.0.0.0:514             0.0.0.0:*                           642/rsyslogd
udp6       0      0 :::514                  :::*                                642/rsyslogd
```

Nous avons bien un service UDP en écoute sur le port 514. La machine est prête à recevoir les logs. Configurons logclient pour lui envoyer les informations contenues dans auth.log. Éditons le fichier rsyslog.conf de logclient :

```
root@logclient:~# cat /etc/rsyslog.conf
auth,authpriv.*                 /var/log/auth.log
auth,authpriv.*                 @logserveur.test.local
```

À la dernière ligne, nous passons comme cible le paramètre @logserveur.test.local qui précise que ces messages doivent également (eh oui, la ligne précédente n’a pas été effacée) être relayés vers le serveur logserveur.test.local en UDP (signe ‘@’). Redémarrons sshd sur logclient :

```
root@logclient:~# systemctl restart sshd.service
```

Et voyons le résultat sur logserveur :

```
root@logserveur:~# grep logclient /var/log/auth.log
Dec 1 19:09:53 logclient sshd[1265]: Server listening on 0.0.0.0 port 22.
Dec 1 19:09:53 logclient sshd[1265]: Server listening on :: port 22.
```

Les messages sont donc bien envoyés de logclient vers logserveur.

### 3.2 Envoi simple en TCP
L’envoi de messages en mode connecté (TCP) ajoute la fiabilité par rapport au mode non connecté (UDP). Dans ce cas, pourquoi faire de l’UDP ? La réponse est simple, les implémentations du protocole Syslog sont multiples et certaines vont supporter le chiffrement, d’autre non, certaines TCP, d’autre non. Rsyslog a pris le parti d’essayer d’être compatible avec tout le monde. Nous allons donc activer TCP en plus d’UDP sur logserveur et passer logclient en TCP. Ajoutons donc le support de TCP sur logserveur (on remplace juste imudp par imtcp) :

```
root@logserveur:~# cat /,etc/rsyslog.conf
module(load="imtcp")
input(type="imtcp" port="514")
```

On redémarre et on vérifie que rsyslogd écoute bien sur le port 514 TCP :

```
root@logserveur:~# systemctl restart rsyslog.service
root@logserveur:~# netstat -laputn | grep 514
tcp        0      0 0.0.0.0:514             0.0.0.0:*               LISTEN      480/rsyslogd
tcp6       0      0 :::514                  :::*                    LISTEN      480/rsyslogd
udp        0      0 0.0.0.0:514             0.0.0.0:*                           480/rsyslogd
udp6       0      0 :::514                  :::*                                480/rsyslogd
```

Tout semble parfait. Allons ensuite sur le client. La manipulation va consister a ajouter un ‘@’ devant celui existant pour passer en TCP dans le rsyslog.conf :

```
root@logclient:~# cat /etc/rsyslog.conf
auth,authpriv.*                 /var/log/auth.log
auth,authpriv.*                 @@logserveur.test.local
```

Je me déconnecte et me reconnecte afin de produire un log :

```
root@logserveur:~# grep logclient /var/log/auth.log
Dec 1 19:31:13 logclient sshd[1281]: Accepted password for root from 10.0.2.2 port 62671 ssh2
Dec 1 19:31:13 logclient sshd[1281]: pam_unix(sshd:session): session opened for user root by (uid=0)
```

Nous avons donc une réception TCP fonctionnelle. Le dernier point est d’envoyer les logs provenant de logclient dans un endroit à part pour ne pas polluer les logs locaux de logserveur.

### 3.3 Organiser ses logs distants
Notre objectif va être d’envoyer les logs d’authentification de logclient dans un répertoire dédié sur logserveur. Nous allons donc modifier le fichier rsyslog.conf de logserveur et ajouter une règle qui envoie les messages distants (c’est-à-dire dont l’origine n’est pas 127.0.0.1) dans un fichier personnalisé :

```
root@logserveur:~# cat /etc/rsyslog.conf
template (name="remoteauth" type="string" string="/var/log/%HOSTNAME%/auth.log")
if ($fromhost-ip != "127.0.0.1" and ($syslogfacility-text == "auth" or $syslogfacility-text == "authpriv")) then {
        action(type="omfile" dynaFile="remoteauth")
        stop
}
```

Nous commençons par créer un template qui va définir la forme du fichier de destination de nos logs distants (ligne 1). Le template se nomme remoteauth (name) , il est de type chaîne de caractère (type="string") et instancie le gabarit de fichier `/var/log/<NOM DE LA MACHINE>/auth.log` (string="/var/log/%HOSTNAME%/auth.log"). Dans notre cas, les logs iront dans le fichier /var/log/logclient/auth.log. Nous créons ensuite une règle pour sélectionner certains messages à envoyer dans ce fichier (ligne 2). Nous prenons les messages ne venant pas de 127.0.0.1, donc distants ($fromhost-ip != "127.0.0.1"). Pour ces messages, nous sélectionnons ceux qui ont la FACILITY auth ou authpriv ($syslogfacility-text == "auth" or $syslogfacility-text == "authpriv"). Pour cette sélection, nous invoquons la fonction action d’écrire les messages (type="omfile") dans le fichier défini par notre template (dynaFile="remoteauth"). Après cette action, nous stoppons le traitement du message (stop). Redémarrons le service sshd sur le client :

```
root@logclient:~# systemctl restart sshd.service
```

Et vérifions le contenu du fichier /var/log/logclient/auth.log :

```
root@logserveur:~# cat /var/log/logclient/auth.log
Dec 1 20:58:20 logclient sshd[473]: Server listening on 0.0.0.0 port 22.
Dec 1 20:58:20 logclient sshd[473]: Server listening on :: port 22.
```

Les messages sont bien routés vers ce fichier sans se mélanger avec ceux de logserveur (qui sont directement dans /var/log). À ce niveau de l’article, le seul moyen que le lecteur a de limiter les accès à son serveur Rsyslog est d’utiliser un filtrage réseau sur les ports 514 UDP et TCP. Nous allons voir comment ajouter de la sécurité dans tout ça en introduisant la fonctionnalité de transmission TLS proposée par rsyslogd.

## 4. Sécurisation avec TLS
Le démon rsyslogd peut s’appuyer sur le protocole TLS (Transport Layer Security) pour authentifier les deux extrémités de la communication et chiffrer leurs échanges. Pour ce faire, chaque machine doit se doter de certificats. Dans notre exemple, c’est la machine logserveur qui va distribuer les certificats. Les seules choses à savoir sur les certificats sont que :

1. Chaque machine génère une biclé (publique et privée) ;
1. Ce qui est chiffré avec la clé publique se déchiffre avec la clé privée ;
1. Ce qui est chiffré avec la clé privée se déchiffre avec la clé publique ;
1. La clé privée ne sort jamais de la poche de celui qui a généré la biclé ;
1. Un certificat est un moyen de distribuer une clé publique en y adjoignant un nom de machine.


### 4.1 Mise en place de l’autorité de certification
Pour passer les communications de rsyslogd en TLS, chaque machine doit disposer d’un certificat signé par une entité commune : l’autorité de certification ou CA (Certificate Autority). Nous allons la placer sur la machine logserveur. Installons les outils pour générer facilement les biclés et les certificats :

```
root@logserveur:~# apt-get install gnutls-bin
```

Commençons par générer la clé privée de l’autorité de certification (--generate-privkey). Nous la nommerons ca-key.pem (--outfile ca-key.pem). Cette clé sera utilisée par la CA pour signer les certificats contenant les clés publiques des différentes machines (ici, logclient et logserveur). Nous faisons ces manipulations dans le répertoire /opt/CA :

```
root@logserveur:/opt/CA# certtool --generate-privkey --outfile ca-key.pem --sec-param High
Generating a 3072 bit RSA private key…
```

Générons ensuite une clé publique (--outfile ca.pem) associée à cette clé privée (--load-privkey ca-key.pem) que nous encapsulons dans un certificat auto-signé (--generate-self-signed) par la CA. Je n’ai gardé que les sorties où il faut entrer une valeur qui n’est pas celle par défaut :

```
root@logserveur:/opt/CA# certtool --generate-self-signed --load-privkey ca-key.pem --outfile ca.pem
Generating a self signed certificate...
Please enter the details of the certificate's distinguished name. Just press enter to ignore a field.
Common name: logserveur.test.local
[...]
The certificate will expire in (days): 3650
 
Extensions.
Does the certificate belong to an authority? (y/N): y
[...]
Will the certificate be used to sign other certificates? (y/N): y
[...]
Is the above information ok? (y/N): y
 
Signing certificate…
```

Nous lui passons le Common Name qui est égal au nom complet (FQDN) de la machine faisant office de CA (ici, logserveur.test.local). Nous disons ensuite que le certificat sera valide 3650 jours, qu’il appartient à une autorité de certification et qu’il servira de certificat racine à distribuer sur toutes les machines régies par cette CA. On valide et on se retrouve avec le certificat racine ca.pem.

### 4.2 Génération du certificat de logserveur
Restons sur logerveur pour générer son certificat signé par la CA. Comme précédemment, nous commençons par générer une clé privée, ici il s’agit du fichier logserveur.pem :

```
root@logserveur:/opt/CA# certtool --generate-privkey --outfile logserveur.pem --sec-param High
Generating a 3072 bit RSA private key…
```

Ensuite, nous créons une demande de signature de certificat (--generate-request), également nommée CSR (Certificate Signing Request), à soumettre à la CA. Cette demande va générer une clé publique associée à la clé privée précédente logserveur.pem (--load-privkey logserveur.pem). La demande de certificat est ensuite stockée dans le fichier logserveur.csr (--outfile logserveur.csr) :

```
root@logserveur:/opt/CA# certtool --generate-request --load-privkey logserveur.pem --outfile logserveur.csr
Generating a PKCS #10 certificate request...
Common name: logserveur.test.local
[...]
Enter a dnsName of the subject of the certificate: logserveur.test.local
[...]
Is this a TLS web client certificate? (y/N): y
Is this a TLS web server certificate? (y/N): y
```

On donne le nom complet de la machine pour le Common Name et pour le dnsName. Enfin, on précise que le certificat servira à faire du TLS (mais pas forcément pour un serveur web, vous l’aurez deviné). La dernière étape est de faire signer cette CSR à la CA.

Nous allons donc générer un certificat (--generate-certificate) à partir de la CSR logserveur.csr (--load-request logserveur.csr). Nous attacherons ce certificat au certificat racine ca.pem (--load-ca-certificate ca.pem). Enfin, nous le signons avec la clé privée de la CA (--load-ca-privkey ca-key.pem). Ce certificat se nommera logserveur.crt (--outfile logserveur.crt) :

```
root@logserveur:/opt/CA# certtool --generate-certificate --load-request logserveur.csr --outfile logserveur.crt --load-ca-certificate ca.pem --load-ca-privkey ca-key.pem
Generating a signed certificate...
[...]
Activation/Expiration time.
The certificate will expire in (days): 365
 
 
Extensions.
Does the certificate belong to an authority? (y/N): n
Is this a TLS web client certificate? (y/N): y
Is this a TLS web server certificate? (y/N): y
Enter a dnsName of the subject of the certificate: logserveur.test.local
[...]
Is the above information ok? (y/N): y
 
Signing certificate…
```

On lui donne une durée de 365 jours et nous précisons qu’il n’appartient pas à une CA. Pour le reste, on confirme ce qui était dans la CSR. On déplace la clé et le certificat dans un répertoire /,etc/rsyslog-tls/ :

```
root@logclient:~# mv logserveur.* /,etc/rsyslog-tls/
```

À la fin de ces manipulations, nous disposons d’un certificat et d’une clé privée sur logserveur. Le certificat est signé par la CA. Ainsi, toute machine disposant du certificat racine de la CA peut vérifier que le certificat a bien été délivré par elle (propriété 2 et 3 ci-dessus). En conséquence, le fichier ca.pem devra être déployé sur toutes les machines pour qu’elles puissent vérifier que le certificat présenté à l’autre extrémité de la connexion TLS ait bien été validé par la CA.

### 4.3 Génération du certificat sur le client
Les actions sur logclient seront les mêmes que pour logserveur et se passent dans le répertoire /,etc/rsyslog-tls. Il faudra cependant remplacer logserveur.test.local par logclient.test.local pour les CN et dnsName. La seule chose en plus est que nous devons envoyer la CSR de logclient sur la CA (donc sur logserveur) et récupérer le certificat signé. Je ne vais donc donner que les commandes les unes après les autres :

```
root@logclient:/etc/rsyslog-tls# certtool --generate-privkey --outfile logclient.pem --sec-param High
root@logclient:/etc/rsyslog-tls# certtool --generate-request --load-privkey logclient.pem --outfile logclient.csr
```

On envoie la CSR générée sur logclient à logserveur et on la signe :

```
root@logclient:/etc/rsyslog-tls# scp logclient.csr root@logserveur:/opt/CA/
root@logserveur:/opt/CA# certtool --generate-certificate --load-request logclient.csr --outfile logclient.crt --load-ca-certificate ca.pem --load-ca-privkey ca-key.pem
```

On renvoie le certificat signé et le certificat racine sur logclient :

```
root@logserveur:/opt/CA# scp logclient.crt root@logclient:/etc/rsyslog-tls/
root@logserveur:/opt/CA# scp ca.pem root@logclient:/etc/rsyslog-tls/
```

Et nous sommes parés ! Le dernier point est de configurer rsyslogd.

### 4.4 Configuration de rsyslogd en TLS
Pour le client et le serveur, nous devrons installer l’extension GNU TLS de rsyslogd :

```
root@logclient:~# apt-get install rsyslog-gnutls
root@logserveur:~# apt-get install rsyslog-gnutls
```

Commençons par la configuration du serveur (la plus compliquée). Le principe est d’initialiser les paramètres TLS au moment du chargement du module de communication TCP (le TLS fonctionne forcément en TCP, en UDP c’est du DTLS).

```
root@logserveur:/opt/CA# cat /etc/rsyslogd.conf
module(load="imtcp"
    StreamDriver.Name="gtls"
    StreamDriver.Mode="1"
    StreamDriver.Authmode="anon"
)
$DefaultNetstreamDriver gtls
$DefaultNetstreamDriverCAFile /opt/CA/ca.pem
$DefaultNetstreamDriverCertFile /etc/rsyslog-tls/logserveur.crt
$DefaultNetstreamDriverKeyFile /etc/rsyslog-tls/logserveur.pem
input(type="imtcp" port="6514")
```

La directive de chargement du module TCP a été quelque peu modifiée pour préciser que nous allons créer une socket TLS (StreamDriver.Name="gtls"), forcer le mode TLS et éviter les communications en clair (StreamDriver.Mode="1") et nous précisons enfin que l’authentification en mode anonyme est autorisée. C’est-à-dire que toute machine possédant un certificat signé par la CA peut envoyer des logs à logserveur (StreamDriver.Authmode="anon"). Évidemment, cela est modifiable pour filtrer les machines par rapport à leur CN présenté dans leur certificat en utilisant la combinaison StreamDriver.Authmode="x509/name" et StreamDriver.PermittedPeer=["host1", "host2"] (mais l’article est déjà suffisamment long !). Le paramètre suivant nous indique que les flux vont être chiffrés en TLS par défaut ($DefaultNetstreamDriver gtls). Le triplé de lignes suivant sert à localiser 1) le certificat racine (ca.pem) 2) le certificat de la machine (logserveur.crt) et 3) sa clé privée (logserveur.pem). Enfin, la dernière ligne place le processus en écoute sur le port TLS configuré précédemment.

Il y a une chose à savoir, l’ordre que je vous ai donné n’est pas le fruit du hasard. Il faut présenter les choses en suivant cette séquence sinon cela ne fonctionne pas. D’après ce que j’ai pu voir, pas mal de documentations fausses sur Internet se recopient les unes les autres.

Le lecteur attentif aura remarqué que nous avons changé de port pour le TLS et que nous utilisons le 6514. La machine est prête à recevoir les logs, passons au client. Je vous donne le fichier de configuration du client :

```
root@logclient:/# cat /etc/rsyslogd.conf
$DefaultNetstreamDriver gtls
$DefaultNetstreamDriverCAFile /etc/rsyslog-tls/ca.pem
$DefaultNetstreamDriverCertFile /etc/rsyslog-tls/logclient.crt
$DefaultNetstreamDriverKeyFile /etc/rsyslog-tls/logclient.pem
$ActionSendStreamDriverAuthMode anon
$ActionSendStreamDriverMode 1
auth,authpriv.*                 @@logserveur.test.local:6514
```

Rien de bien nouveau sous le soleil à part pour la dernière ligne où nous modifions le port en 6514 pour refléter l’utilisation de la socket TLS côté logserveur. On redémarre rsyslog des deux côtés :

```
root@logclient:~# systemctl restart rsyslog.service
root@logserveur:~# systemctl restart rsyslog.service
```

Générons un log côté logclient en redémarrant le serveur sshd :

```
root@logclient:~# systemctl restart sshd.service
```

Voyons côté logserveur :

```
root@logserveur:~# cat /var/log/logclient/auth.log
Dec 3 00:45:36 logclient sshd[479]: Received signal 15; terminating.
Dec 3 00:45:36 logclient sshd[567]: Server listening on 0.0.0.0 port 22.
Dec 3 00:45:36 logclient sshd[567]: Server listening on :: port 22.
```

Et hop ! On peut vérifier que tout est bien chiffré côté logserveur avec un tcpdump :

```
root@logserveur:~# tcpdump -i enp0s8 tcp port 6514 -X -s 0 -nn
```

Et là vous devriez voir passer un tas de trucs incompréhensibles (chiffrés quoi).

## Conclusion
En conclusion, on peut remarquer que d’après l’horodatage des logs j’étais un peu en retard pour la remise de cet article ^^. Plus sérieusement, cette remarque n’est pas qu’humoristique. Pour que les logs soient de qualité, ils doivent pouvoir s’appuyer sur une infrastructure proposant des services de base tels que la synchronisation de temps (pour avoir un horodatage cohérent sur tout votre réseau), un service DNS bien renseigné (pour les résolutions directes et inverses) ou encore des systèmes d’authentification centralisés (pour garantir l’imputabilité des actions à un utilisateur précis et pas à un compte générique). La qualité de ces logs est d’autant plus importante que différents outils s’appuient dessus pour corréler les nombreux événements à analyser.


  
source : https://connect.ed-diamond.com/Linux-Pratique/lphs-047/basez-votre-supervision-sur-des-logs-de-qualite-avec-rsyslog