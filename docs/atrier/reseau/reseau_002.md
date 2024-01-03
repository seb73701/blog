---
title: tempo001
description: 
published: true
date: 2023-04-21T10:38:49.930Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:38:45.587Z
---

---
title: Authentification de l’accès à votre réseau : mise en place d’un portail captif et d’un service 802.1X
description: 
published: true
date: 2023-02-03T10:40:45.259Z
tags: 
editor: markdown
dateCreated: 2022-12-21T09:33:48.932Z
---

---
title: Authentification de l’accès à votre réseau : mise en place d’un portail captif et d’un service 802.1X
description: 
published: true
date: 2023-01-27T17:51:12.379Z
tags: 
editor: markdown
dateCreated: 2022-12-21T09:33:48.932Z
---

---
title: Authentification de l’accès à votre réseau : mise en place d’un portail captif et d’un service 802.1X
description: 
published: true
date: 2022-12-21T09:33:48.932Z
tags: 
editor: markdown
dateCreated: 2022-12-21T09:33:48.932Z
---

# Authentification de l’accès à votre réseau : mise en place d’un portail captif et d’un service 802.1X

> Un portail captif est un élément de contrôle mandataire régulant l’accès au réseau pour les clients équipés d’un navigateur web. Dans cet article, nous allons voir comment intégrer cet élément dans une infrastructure pour authentifier les clients connectés en Wi-Fi avant leur accès effectif au réseau extérieur. Nous allons également présenter le 802.1X qui vise à authentifier les utilisateurs avant tout accès au réseau.

Les solutions de portails captifs fonctionnent toutes sur le même principe :

1. L’utilisateur tente d’aller sur Internet ;
1. Il est redirigé sur une page d’authentification ;
1. Il entre son identifiant et son mot de passe ;
1. Un ensemble de règles de filtrage est joué sur le portail captif pour règlementer son accès au réseau externe ;
1. Il sort sur le réseau conformément aux règles générées.

Dans cet article, nous allons aborder la mise en place d’un tel portail à l’aide de la solution pfSense sur une machine dédiée. Nous allons également présenter une solution alternative d’authentification des utilisateurs avant leur accès au réseau sous la forme d’un service 802.1X embarqué directement sur la borne Wi-fi grâce à OpenWRT.

# 1. pfSense
Le logiciel pfSense est une solution d’UTM (Unified Threat Management) libre. Un UTM est un boîtier regroupant de multiples fonctionnalités de sécurité comme le filtrage réseau, la détection/prévention  d’intrusions, un service de VPN, proxy web ou encore un portail captif. C’est sur cette dernière fonctionnalité que nous allons nous attarder. Commençons par présenter l’environnement de test.

## 1.1 Environnement de test
Pour faire nos tests, dédions une machine physique à pfSense. Cette machine dispose de deux interfaces réseaux. La première est reliée à un commutateur sur lequel la borne Wi-fi est raccordée (partie gauche de la figure 1) tandis que la seconde est connectée au réseau extérieur (qui a accès à Internet). La première notion à clarifier pour l’installation et l’exploitation de pfSense est celle de LAN/WAN.

Un LAN (Local Area Network) est stricto sensu un domaine de broadcast Ethernet. En pratique, cela concerne les machines connectées au même équipement de commutation (switch physique, borne Wi-fi etc.). Ces équipements de commutations peuvent être empilés (c’est-à-dire raccordés les uns aux autres) pour accroître le nombre de ports disponibles sur le LAN (ou, comme dans notre exemple, ouvrir un réseau filaire au sans fil).

Le WAN (Wide Area Network) est un réseau dit étendu. L’idée est que le WAN connecte un ou plusieurs LAN à Internet. Attention, cette définition est extrêmement grossière, car le WAN peut être un backbone de routeurs connectant une multinationale au restant du monde ou une simple interface estampillée « WAN » sur un routeur (ou une box) connectée au réseau téléphonique (ou fibre). Ces deux définitions sont sujettes à interprétation, car fortement dépendantes du contexte.

Pour nous, le LAN est le réseau 192.168.0.0/24 avec la borne occupant l’adresse 192.168.0.1 et la patte LAN du pfSense en 192.168.0.2. Le service DHCP est activé sur cette patte. Le réseau extérieur est en 192.168.1.0/24. On y retrouve la patte WAN du pfSense en 192.168.1.99 et le routeur connecté à Internet en 192.168.1.254. Ce routeur étant une box internet, il fait aussi office de serveur DNS.


<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/test_pfsense1.png">

Fig. 1 : Environnement de test pfSense.

## 1.2 Configuration de la borne
Commençons par configurer la borne. Il est intéressant de constater qu’elle dispose elle-même d’une interface estampillée WAN et d’une autre LAN. Ça raccroche avec ce que je mentionnais plus haut à savoir que ces deux notions sont assez floues (relevant quasiment de la tradition orale). En effet, le WAN et une simple interface réseau RJ45 et le LAN un bridge de quatre interfaces physiques RJ45 et des deux interfaces Wi-fi pour constituer un petit commutateur Ethernet embarqué directement sur la borne unifiant les connexions filaires et sans fils. Le point d’accès (AP) Wi-fi utilisé est un TP-LINK Archer C7. La caractéristique intéressante est qu’il embarque un logiciel d’exploitation grandement basé sur la distribution Linux libre OpenWRT dédiée aux points d’accès Wi-fi, nous y reviendrons par la suite.

Dans notre exemple, seule l’interface LAN de la borne est configurée. Cette interface est connectée à notre commutateur sur lequel la patte LAN du pfSense est également présente. En effet, le concept de portail captif ne se limite pas aux réseaux sans fil. Il y a deux étapes nécessaires au bon fonctionnement de la borne :

- donner une adresse à l’interface LAN pour gérer la borne via l’interface web (ou en SSH), ici il s’agit de la 192.168.0.1 ;
- déclarer un SSID (Service Set IDentifier).

Le SSID est le nom du réseau sans fil qui est affiché par les systèmes des utilisateurs en recherche de connexion. Optionnellement, il est possible d’ajouter une clé pour s’attacher au réseau Wi-fi (option conseillée tant que notre pfSense n’est pas opérationnel). On notera dans la capture de la figure 2 qu’une interface WAN est configurée, mais l’adresse est bidon, l’interface n’est même pas branchée (elle est marquée d’une croix).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/borne1.png">

Fig. 2 : Configuration de la borne.

## 1.3 Configuration de pfSense
L’installation de pfSense est extrêmement bien décrite dans cette documentation [1]. Le seul point méritant une explication est la notion de VLAN [2]. Nous n’allons pas les utiliser, mais ils sont pris en charge par pfSense. Un VLAN est un moyen de segmenter les domaines de broadcast sur un équipement physique de commutation en vue de les faire cohabiter avec une certaine étanchéité. Une fois ces étapes accomplies, il faut configurer les interfaces LAN et WAN. C’est exactement le même principe que pour la borne Wi-fi.

L’interface LAN est la passerelle par défaut des machines situées soit sur le réseau filaire du commutateur, soit sur le réseau sans fil. Au lancement de pfSense, il faut vérifier que l’interface physique raccordée au réseau accueillant les clients sans fil est bien positionnée en LAN et que celle connectée au monde extérieur est en WAN. Si ce n’est pas le cas, il faut choisir l’option numéro 1 : Assign Interfaces. Le programme de configuration va ensuite vous demander d’entrer le nom des interfaces à assigner au WAN puis au LAN. Ensuite, il faut leur affecter des adresses IP en choisissent l’option numéro 2 : Set Interface(s) IP address. On choisit l’interface à configurer. Commençons par le LAN. Nous lui affectons l’IP 192.168.0.2. Pour le masque (bit count), nous choisissons 24. Et nous n’entrons pas de passerelle par défaut. On ne configure pas non plus l’IPv6. Il faut ensuite activer le DHCP sur l’interface. La dernière question n’est pas très explicite : « Do you want to revert to HTTP as the webConfigurator protocol ? ». Par défaut, pfSense configure un accès HTTPS à l’interface d’administration. Nous allons rester en HTTP pour ne pas alourdir cet article avec la gestion des certificats. Nous allons donc répondre oui. À l’issue de ces manipulations, l’interface d’administration est accessible sur http://192.168.0.1.

Il reste l’interface WAN à configurer. On retourne sur l’option numéro 2 utilisée précédemment et cette fois-ci il faut choisir l’interface WAN. La première question est de savoir si on veut configurer l’interface en client d’un serveur DHCP. La réponse est non, nous allons opter pour une configuration statique. Nous entrons ensuite l’IP de l’interface 192.168.1.99 et 24 pour le masque de sous-réseau. Cette fois-ci, nous allons fixer une adresse de passerelle qui est celle du routeur connecté à Internet : 192.168.1.254. Enfin, comme précédemment, nous n’activons rien de relatif à IPv6. Nous pouvons maintenant nous connecter sur l’interface d’administration avec le compte par défaut « admin » et le mot de passe « pfsense » et nous arrivons sur l’écran d’accueil de pfSense (figure 3).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/accueil1.png">

Fig. 3 : Page d’accueil de pfSense.

Une fois connecté à l’interface d’administration, il faut fixer quelques paramètres sur le service DHCP. Dans la section Services, il faut aller sur DHCP Server. Les paramètres qui vont nous intéresser sont Subnet, Subnet mask et Range. Ces trois paramètres devraient être initialisés à des valeurs correctes pour nos manipulations, mais dans le doute, Subnet est à 192.168.0.0, Subnet mask à 24 et Range de 100 à 199. Les deux derniers paramètres à gérer sont DNS servers et Gateway, soit le serveur DNS à servir aux clients et la passerelle par défaut. Pour le segment LAN, la passerelle est l’interface de la machine pfSense donc 192.168.0.2 et le serveur DNS est le cache hébergé sur le routeur de sortie, donc 192.168.1.254. Avec ces paramètres, les clients connectés à la borne devraient pouvoir sortir sur le réseau externe.

## 1.4 Configuration du portail captif
Avant d’entrer dans le vif de la configuration du portail, il convient de suivre le périple des paquets émis par une machine domiciliée sur le réseau sans fil derrière le pfSense. Lorsque la machine émet le paquet, si son adresse de destination n’est pas sur le réseau LAN, alors le paquet est envoyé à la passerelle par défaut. Cette passerelle est l’interface LAN du pfSense. Une fois le paquet arrivé sur la passerelle, il est confronté aux règles de filtrage définies sur l’interface LAN. Si le paquet est autorisé, alors le noyau transfère le paquet de l’interface LAN au WAN, car le passage de paquet d’une interface à l’autre est autorisé par la variable du noyau net.inet.ip.forwarding positionnée à 1. À ce niveau-là, l’adresse source du paquet est natée avec celle de l’interface WAN. En conséquence, vus du réseau externe (WAN) tous les paquets issus du réseau sans fil arrivent avec la même IP source, ce qui peut être un souci majeur dans le processus de monitoring réseau. Il peut donc être nécessaire d’instrumenter la passerelle pfSense dans un contexte professionnel [3]. Une fois naté, le paquet est envoyé vers la passerelle par défaut de la machine pfSense qui n’est autre que le routeur de sortie du réseau WAN donc Internet. Enfin, le paquet sort.

Le portail captif intercepte donc le trafic avant qu’il ne sorte de l’interface. La première décision à prendre est de savoir où placer ce portail. Nous allons le mettre au plus près des clients sans fil, c’est-à-dire sur l’interface LAN du pfSense. Pour accéder à la configuration du portail, il faut sélectionner l’option Captive Portaldu menu Services. Ensuite, on clique sur Add pour ajouter une instance de portail. Pour pfSense, un portail sert une zone, il faut donc la nommer. Nous l’appellerons Wifi. Il faut cliquer sur Save & Continue pour enregistrer l’instance de portail dans la configuration de pfSense. Un écran demandant d’activer le portail arrive, il faut cocher l’option et cliquer sur Save. Enfin, la configuration du portail s’affiche. Ici, on ne va fixer que deux paramètres pour avoir une instance minimale fonctionnelle :

1. l’interface sur laquelle le portail va s’attacher ;
1. la localisation de la base des comptes autorisés à utiliser le portail.

Pour attacher le portail à l’interface, il faut choir LAN pour l’attribut Interfaces (figure 4).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/enable_portail1.png">

Fig. 4 : Attacher le portail à l’interface LAN.

Toujours dans le souci de créer un portail minimal, nous allons adosser les utilisateurs du portail à une base locale créée sur le pfSense. Pour cela, il faut sélectionner l’option Use an Authentication backend pour l’attribut Authentication Method de la section Authentication.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/auth_portail1.png">

Fig. 5 : Adosser une base de compte locale.

La dernière étape est de mettre le serveur DNS en liste blanche. En effet, pour qu’un navigateur émette une requête HTTP à intercepter par le portail, il est nécessaire qu’il puisse faire une résolution DNS au préalable pour localiser la machine à qui adresser cette requête. Cela se passe dans la section Allowed IP Addresses de la configuration du portail. On doit entrer l’adresse du serveur DNS pour l’attribut IP Address, soit 192.168.1.254 et le sens de communication pour l’attribut Direction, soit Both (figure 6). Le portail est maintenant configuré et fonctionnel. Il ne reste plus qu’a créer les utilisateurs.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/whitelist_portail1.png">

Fig. 6 : Autorisation du serveur DNS.

Si vous êtes dans un environnement assez chargé, il vous faudrait également considérer les deux attributs suivants dans la configuration du portail : Idle timeout et Hard timeout. Le premier donne un délai en minutes au-delà duquel un utilisateur inactif est déconnecté. Le second donne un délai de déconnexion indépendamment de toute activité.

## 1.5 Création des utilisateurs
Nous allons ajouter des utilisateurs pour le portail. L’idée est de créer un groupe avec tous les utilisateurs autorisés à utiliser le portail. Commençons par créer le groupe. Cela se passe dans System puis User Manager puis Groups. Nous créons un groupe nommé Portail (Group Name), dans le Scope Local (c’est-à-dire local à notre pfSense). La partie la plus importante est la section Assigned Privileges où il faut cliquer sur Add et rechercher le privilège « User – Services : Captive Portal Login » (figure 7).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/groupe1.png">

Fig. 7 : Création du groupe d’utilisateurs du portail.

Puis créons un utilisateur nommé nico. Cela se passe dans System puis User Manager puis Users. Les deux paramètres importants sont le Username (identifiant) et Password (mot de passe). Dès cet écran, il est possible de positionner l’utilisateur directement dans le groupe portail (figure 8).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/utilisateur1.png">

Fig. 8 : Création d’un utilisateur.

Lorsque l’utilisateur va essayer d’accéder à un site web, il va tomber sur une page lui demandant de s’authentifier avant d’accéder au réseau (figure 9). Une fois votre identifiant et votre mot de passe entrés, vous pouvez sortir sur Internet. Vos traces sont désormais authentifiées par le portail.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/captive1.png">

Fig. 9 : Page d’authentification du portail.

## 1.6 Résolution des problèmes
Si toutefois vous ne tombez pas sur la page directement il peut y avoir plusieurs raisons. La première est que vous n’avez peut-être pas autorisé les requêtes DNS. Pour vérifier, vous pouvez ouvrir un terminal et taper la commande suivante :

```
ngreneche@DESKTOP-RE130IB:~$ host www.google.fr 192.168.1.254
www.google.fr has address 216.58.204.99
www.google.fr has IPv6 address 2a00:1450:4007:80a::2003
```

Où nous demandons explicitement au serveur 192.168.1.254 de résoudre https://www.google.fr/. Si cela fonctionne, peut-être qu’une notification est apparue sur votre navigateur type « Ce réseau nécessite que vous vous connectiez à un compte pour utiliser Internet ».Dans ce cas, il faut penser à la valider et vous allez arriver sur le portail. Si les problèmes persistent, vous pouvez aller voir ce qui s'affiche dans les logs du portail captif. Ils se trouvent dans Status > System Logs > Captive Portal Auth. Il peut également se produire que le portail affiche une page blanche (ce n'est pas forcément lié a pfSense, ça arrive aussi avec des portails commerciaux). Dans ce cas, c'est souvent que la déconnexion de l'utilisateur s'est mal passée. Il faut procéder à une déconnexion manuelle (et/ou réduire le Idle Timeout). Vous pouvez visualiser les utilisateurs connectés dans la section Status > Captive Portal > Wifi (c'est le nom de notre zone).

Côté client, une variable qui peut aussi influer sur le comportement du portail est l'activation ou non de la détection automatique de portail captif. Le système va essayer d’accéder à une URL qu'il connaît bien et s’il récupère la page attendue il conclut qu'il n'a pas besoin de passer par un portail pour s'authentifier. Dans le cas contraire, il ouvre la fenêtre de connexion.

## 2. OpenWRT
OpenWRT est une distribution Linux adaptée aux équipements type routeurs, point d'accès Wi-fi, etc. Dans notre article, nous allons remplacer le système d'exploitation du routeur par une instance d'OpenWRT pour proposer une authentification 802.X à nos utilisateurs. La différence avec la partie d'avant sur pfSense est que l'utilisateur devra entrer un identifiant et un mot de passe pour obtenir la connexion physique au réseau. En effet, dans le cas précédent, l'utilisateur avait une connexion réseau, car il devait être capable d'accéder au portail pour s'authentifier ainsi qu’au DNS.

### 2.1 Environnement de test
Notre nouvel environnement est composé d'un réseau sans fil en 192.168.0.0/24 et du même réseau local proposant l'accès externe en 192.168.1.0/24. La différence est que cette fois-ci c'est la borne elle-même qui va porter les interfaces LAN et WAN. L'interface LAN en 192.168.0.1 servira le réseau sans fil tandis que l'interface WAN en 192.168.1.99 connectera notre routeur Wi-fi au monde extérieur. Le service DHCP sera embarqué directement sur la borne sur l'interface LAN pour configurer automatiquement les clients sans fil.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/test_openwrt1.png">

Fig. 10 : Environnement de test OpenWRT.

### 2.2 Configuration de la borne
Pour configurer la borne, il faut commencer par remplacer le firmware de base par celui d'OpenWRT. Avant tout, il faut contrôler la version d'OpenWRT à appliquer sur son matériel selon la matrice de compatibilité suivante [4]. Une fois le firmware téléchargé [5], il faut le charger sur l'équipement. Pour notre borne TP-Link, cela se passe dans Outils Systèmes > Mise à jour logiciel. On y trouve une rubrique Mise à jour manuelle (figure 11) dans laquelle on peut charger l'image téléchargée d'OpenWRT.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/firmware_openwrt1.png">

Fig. 11 : Chargement du firmwareOpenWRT.

Une fois la mise à jour validée, le système est remplacé et il redémarre sous OpenWRT. On peut se brancher en filaire pour accéder à l'interface web de configuration sur http://192.168.1.1 avec comme identifiant root, sans mot de passe. Il ne faut évidemment pas connecter l'interface WAN, car elle serait sur le même réseau que LAN ce qui poserait de gros soucis ! Une fois dans l'interface, commençons par fixer un mot de passe à root. On voit qu'une fois connecté, nous avons une bannière nous invitant à changer le mot de passe root (figure 12). En plus, nous pouvons même y activer un accès SSH. Les paramètres par défaut du service SSH sont bons (port 22, autoriser la connexion root et l’authentification par mot de passe). Pensez bien à sauvegarder et appliquer votre configuration à chaque étape.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/accueil_openwrt1.png">
	
Fig. 12 : Accueil OpenWRT.

Les deux choses à faire sont de configurer les interfaces LAN et WAN. Petite astuce, le changement de d'adresse de LAN via l'interface ne fonctionne pas forcément bien sur notre version d'OpenWRT. Nous allons donc mettre à profit le serveur SSH fraîchement installé et modifier l'adresse en ligne de commandes, cela se passe dans la section config interface ‘lan’ du fichier /.etc/config/network :

```
ngreneche@DESKTOP-RE130IB:~$ ssh root@192.168.0.1
root@OpenWrt:~# cat /,etc/config/network
config interface 'lan'
        option type 'bridge'
        option ifname 'eth0.1'
        option proto 'static'
        option ipaddr '192.168.0.1'
        option netmask '255.255.255.0'
        option ip6assign '60'
```

Une fois le paramètre option ipadddr modifié de 192.168.1.1 vers 192.168.0.1, redémarrez OpenWRT :

```
root@OpenWrt:~# reboot
```

Pour en finir avec le LAN, il faut bien vérifier que le DHCP est activé. Retournez dans l’interface web d’OpenWRT et cliquez sur Network puis Interfaces et enfin, sur Edit de l'interface LAN. Vérifiez que le paramètre Ignore interface de l’onglet DHCP Server soit bien décoché. Faites également un tour dans la section Advanced Settings du serveur DHCP pour ajouter les options suivantes : « 6.192.168.1.254 » et « 3.192.168.0.1 ». En effet, pour DHCP chaque option supplémentaire passée au client correspond à un numéro qui préfixe l’adresse IP. Ainsi, nous avons l’option 6 qui définit le DNS et l’option 3 la passerelle par défaut à passer au client obtenant le bail. Dans certaines versions d’OpenWRT, les champs sont présentés explicitement.

Pour l'interface WAN, nous pouvons y aller sans soucis par l'interface web. À l'instar de pfSense, il faut juste lui définir une IP, un masque, une route par défaut et un serveur DNS. Cliquez sur Network puis Interfaces et enfin sur Edit de l'interface WAN. Vous allez tomber sur l'écran présenté en figure 13. Vous pouvez maintenant raccorder le WAN au commutateur du réseau 192.168.1.0/24.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lp-114/84340/wan-openwrt1.png">

Fig. 13 : Configuration WAN d'OpenWRT.

Vous pouvez y appliquer une configuration en adresse statique (Protocol = Static Address) avec l'IP 192.168.1.99 (IPv4 address), le masque à 255.255.255.0 (IPv4 netmask), la passerelle par défaut (IPv4 gateway) et le DNS (Use custom DNS servers) à 192.168.1.254. Comme d'habitude, on applique et on sauve. À partir de ce moment, les clients connectés en filaire à la borne TP-Link devraient pouvoir se connecter au monde extérieur. On notera que nous n'avons pour l'instant défini aucun réseau sans fil. C'est volontaire !

### 2.3 Installation et configuration de FreeRadius
Pour faire très très simple, Radius et un protocole d’authentification populaire dans le monde des appliances réseau, car il est simple, donc peu gourmand en ressources. Nous allons installer l'implémentation FreeRadius sur notre borne OpenWRT. Pour ce faire, ouvrons une session SSH :

```
ngreneche@DESKTOP-RE130IB:~$ ssh root@192.168.0.1
```

Et utilisons le gestionnaire de paquets d'OpenWRT opkg :

```
root@OpenWrt:~# opkg update

root@OpenWrt:~# opkg install freeradius3 freeradius3-common freeradius3-democerts freeradius3-mod-always freeradius3-mod-attr-filter freeradius3-mod-chap freeradius3-mod-detail freeradius3-mod-digest freeradius3-mod-eap freeradius3-mod-eap-gtc freeradius3-mod-eap-leap freeradius3-mod-eap-md5 freeradius3-mod-eap-mschapv2 freeradius3-mod-eap-peap freeradius3-mod-eap-tls freeradius3-mod-eap-ttls freeradius3-mod-exec freeradius3-mod-expiration freeradius3-mod-expr freeradius3-mod-files freeradius3-mod-logintime freeradius3-mod-mschap freeradius3-mod-pap freeradius3-mod-preprocess freeradius3-mod-radutmp freeradius3-mod-realm freeradius3-mod-unix freeradius3-utils
```

L'update permet de mettre à jour l'index des paquets et install de les installer. On voit qu'il y en a un paquet à installer ! En fait, cela s'explique par le fonctionnement de Radius. En général, on a un serveur Radius central adossé au système d’authentification (OpenLDAP, Active Directory, etc.) et des petits systèmes satellites autour qui interrogent ce système central. Typiquement, les systèmes embarquant OpenWRT sont plutôt destinés à être des satellites. Donc FreeRadius a été saucissonné en petits modules pour satisfaire les besoins des satellites de façon unitaire.

Examinons maintenant la configuration du serveur FreeRadius :

```
root@OpenWrt:~# cat /etc/freeradius3/clients.conf | egrep -v "^\s*(#|$)"

client localhost {
     ipaddr = 127.0.0.1
     proto = *
     secret = testing123
     require_message_authenticator = no   
 nas_type      = other
     limit {
             max_connections = 16
             lifetime = 0
             idle_timeout = 30
     }
}
```

Cette directive client définit les sources autorisées à tenter une authentification. Ici, on n’accepte qu’une source locale (localhost). Cette directive crée un point d'entrée destiné aux satellites pour réaliser les authentifications qu'on leur demande. Si on examine le point d'entrée nommé localhost, on constate qu'il y a deux attributs particulièrement significatifs : ipaddr et secret. ipaddr spécifie l'adresse sur laquelle FreeRadius attend les authentifications et secret est un mot de passe qui doit être renseigné dans la configuration du satellite pour qu’il puisse s’adresser au serveur FreeRadius.

Dans notre cas, c’est la borne OpenWRT elle-même qui va faire office de serveur FreeRadius. Il va donc falloir :

1. créer un utilisateur ;
1. configurer l’accès au serveur FreeRadius local.

Pour créer l’utilisateur, nous allons utiliser la méthode la plus simple, à savoir déclarer un identifiant et un mot de passe en clair dans un fichier plat. Cela se passe dans le fichier /etc/freeradius3/mods-config/files/authorize.

```
root@OpenWrt:~# cat /etc/freeradius3/mods-config/files/authorize

nico Cleartext-Password := "Deftones"
```

Nous avons créé un utilisateur nico qui utilise la méthode d’authentification mot de passe en clair (Cleartest-Password) avec comme mot de passe Deftones.

Pour l’accès au FreeRadius, la philosophie des réseaux sans fil est la suivante : une méthode d’authentification est attachée à un SSID. Pour prendre un exemple concret, lorsque vous êtes abonné chez Free, vous avez deux SSID : un personnel pour votre box et un autre nommé freewifi. Le premier s’appuie sur un mécanisme PSK (Pre-Shared Key), c’est-à-dire que vous devez entrer une longue phrase qui fait office de secret entre vous et la box. Pour le freewifi, vous avez un identifiant et un mot de passe. Nous allons donc créer notre SSID s’authentifiant en 802.1X sur notre serveur FreeRadius :

```
root@OpenWrt:~# cat /etc/config/wireless

config wifi-iface
        option device 'radio1'
        option mode 'ap'
        option ssid 'OpenWrt_8021X'
        option network 'lan'
        option encryption 'wpa2'
        option server '127.0.0.1'
        option key 'testing123'
```

Les trois attributs les plus significatifs sont ssid qui définit le nom de notre nouveau SSID, server qui pointe sur le serveur RADIUS à interroger (ici, le local 127.0.0.1) et key qui est le mot de passe associé. Vous retrouvez bien les paramètres de la configuration du serveur. Les clients voulant s’attacher à ce réseau Wi-Fi vont se voir demander un identifiant et un mot de passe au moment de se connecter au SSID OpenWrt_8021X.

## Conclusion
Dans cet article, nous avons abordé le portail captif qui est un moyen simple d’authentifier les utilisateurs avant leur sortie sur Internet. Son utilisation nécessite juste la présence d’un navigateur web sur la machine. Cependant, pour accéder au portail la machine doit disposer d’un minimum de connectivité (ne serait-ce qu’un accès au serveur DNS). En complément, et j’insiste sur le mot complément, car les deux méthodes ne s’opposent pas, nous pouvons aussi utiliser le 802.1X. Comme l’authentification est déportée sur l’équipement de commutation, c’est donc une connaissance d’un secret qui conditionne l’accès physique au réseau. De plus, nous n’avons fait qu’effleurer les possibilités du 802.1X qui sert de base à des mécanismes comme l’assignation dynamique dans des VLANs ou encore des contrôles de conformité sur la machine avant tout accès au réseau, etc. Nous avons la possibilité de proposer deux SSID : un relié au portail captif et l’autre au réseau 802.1X.

## Références
[1] https://www.pc2s.fr/pfsense-installation-et-configuration

[2] https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-198/Les-reseaux-logiques-VLANs

[3] https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-175/Introduction-au-Network-Security-Monitoring-NSM

[4] https://openwrt.org/_media/toh_dump_tab_separated_csv.csv.gz

[5] http://downloads.openwrt.org/releases/18.06.2/targets/ar71xx/generic/openwrt-18.06.2-ar71xx-generic-archer-c7-v5-squashfs-factory.bin

source : https://connect.ed-diamond.com/Linux-Pratique/lp-114/authentification-de-l-acces-a-votre-reseau-mise-en-place-d-un-portail-captif-et-d-un-service-802.1x