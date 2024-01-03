---
title: Découvrez la virtualisation avec VirtualBox
description: 
published: true
date: 2023-04-21T11:20:39.217Z
tags: virtualisation, virtualbox, machine virtuelle
editor: markdown
dateCreated: 2023-04-21T11:20:34.496Z
---

# Découvrez la virtualisation avec VirtualBox

> VirtualBox est un logiciel libre de virtualisation proposé par Oracle. Son point fort est son accessibilité aussi bien au niveau des systèmes sur lesquels il est disponible que dans sa prise en main.

Aujourd’hui, lorsqu’il s’agit d’installer plusieurs systèmes sur la même machine physique, deux choix s’offrent à nous : la virtualisation complète ou le confinement de processus (conteneurs). Dans cet article, il va être question de virtualisation complète via le logiciel VirtualBox.

## 1. La virtualisation (in a nutshell)
La virtualisation est la capacité d’un système d’exploitation à faire tourner plusieurs noyaux en parallèle de façon isolée sur le même matériel. Le système d’exploitation accueillant les machines virtuelles est appelé système hôte. Les systèmes virtualisés sont appelés systèmes invités ou machines virtuelles (souvent abrégés VM pour Virtual Machine). Les systèmes invités peuvent être totalement différents les uns des autres. Par exemple, un système hôte Linux peut héberger des invités BSD, Windows et Linux. Le composant du système hôte réalisant la virtualisation se nomme l’hyperviseur.

L’hyperviseur est implémenté dans le noyau du système d’exploitation. Il peut résider dans le noyau standard d’un système sous forme de module. On parle alors d’hyperviseur hébergé de type 2. Sinon, il peut être localisé directement sur un matériel spécialisé embarqué sur la carte mère. On parle parle alors d’hyperviseur natif de type 1 (ou bare metal). Un invité peut avoir conscience ou pas d’être virtualisé (donc géré par un hyperviseur). S’il n’en pas conscience alors on parle de virtualisation complète. L’hyperviseur présente à l’invité un environnement matériel virtualisé dédié. Le système invité s’installe puis s’exécute sur le matériel présenté par l’hyperviseur comme s’il était réel. L’invité n’y voit que du feu. Pour gagner en performances (en perdant au niveau de l’isolation), les hyperviseurs peuvent pratiquer la para-virtualisation. Dans ce cas, le noyau de l’invité intègre une portion de code permettant d’accéder directement au matériel de l’hôte. L’invité a donc conscience d’être virtualisé.

Avec l’évolution des hyperviseurs, les frontières entre virtualisation et para-virtualisation (voire même confinement) sont de plus en plus floues. Des hyperviseurs autrefois dédiés à la virtualisation complète intègrent des éléments de para-virtualisation pour les invités le supportant, les CPUs peuvent être partiellement émulés (essentiellement le ring 0) pour gagner en performances, etc. Dans cet article, nous allons étudier VirtualBox en mode virtualisation complète.

## 2. Lancement de notre première VM
Nous allons nous baser sur la dernière version de VirtualBox disponible pour Ubuntu 18.04. Pour l’obtenir, il faut ajouter le dépôt multiverse et procéder à l’installation du package :

```
$ sudo add-apt-repository multiverse && sudo apt-get update
$ sudo apt install virtualbox
```

Commençons par récupérer une ISO type CentOS sur le site officiel qui nous fera office de support d’installation. Cette machine virtuelle nous servira pour illustrer l’article sur LXD. La manipulation va donc consister à :

1. créer la VM ;
1. faire en sorte de pouvoir y accéder confortablement pour la post-configuration ;
1. la cloner ;
1. faire en sorte que la VM et son clone puissent communiquer.

Commençons par cliquer sur l’icône Nouvelle pour créer une nouvelle VM. VirtualBox nous demande de donner un nom à la VM (nous allons l’appeler lxd), un répertoire où stocker cette VM, une famille de système (ici Linux) et enfin une version précise de cette famille (ici, Red Hat 64 bits). En cliquant sur Suivant, on arrive sur la quantité de mémoire à allouer à la VM. On peut laisser la valeur par défaut à 1 Go. Ensuite, on attache un disque dur à la machine, nous choisissons donc de créer un disque dur virtuel maintenant puis on choisit le format VDI. Nous allons sélectionner un disque dynamiquement alloué, c’est-à-dire que la place est occupée au fur et à mesure que la VM se remplit (par opposition à la taille fixe, qui alloue tout l’espace au départ). Nous finissons par choisir sa taille et la machine est prête à être lancée.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/var/diamond/84311/new_vm.png">

*Fig. 1 : Création d’une VM.*

On peut cliquer sur Démarrer pour lancer la VM. VirtualBox nous informe alors que c’est le premier démarrage de la VM, donc il propose de lui indiquer un disque de démarrage. Nous allons lui passer l’ISO de CentOS précédemment téléchargée. On procède à l’installation du système comme sur une machine normale. Passons à la configuration réseau de la VM.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/var/diamond/84311/capture_guest.png">

*Fig. 2 : La VM CentOS en exécution sur un hôte Ubuntu.*

## 3. Configuration réseau de VirtualBox
Par défaut, VirtualBox attache une seule interface réseau à la VM. On notera que dans la section Réseau du menu Configuration de la VM il est possible d’en ajouter jusqu’à quatre. Pour chacune de ces cartes, on peut appliquer une configuration réseau différente. Les configurations qui vont nous intéresser sont NAT et Réseau interne.

### 3.1 La configuration NAT
L’interface fournie par défaut dans la VM est configurée en NAT. La configuration en mode NAT (Network Address Translation) autorise la VM à sortir avec l’adresse de l’hôte. Bien qu’elles soient sur le même sous-réseau IP, les interfaces de NAT des différentes VM sont isolées les unes des autres par l’hyperviseur. Si on veut que les machines puissent se voir, il faut utiliser la configuration réseau NAT. L’hôte fait office de serveur DHCP, passerelle par défaut et cache DNS pour les interfaces réseaux des invités configurées en NAT. Cependant, dans cette configuration, il est impossible de se connecter (par exemple en SSH) sur la VM. Nous allons donc configurer une règle de redirection sur cette interface. Cette règle dira que les connexions TCP arrivant sur le port 20022 de l’interface de rebouclage (127.0.0.1) de l’hôte seront redirigées sur le port TCP 22 de l’interface en NAT de l’invité. On peut faire ça de deux manières : en ligne de commandes ou en mode graphique.

Pour le mode graphique, il faut dérouler le menu Avancé dans l’onglet de configuration de l’Interface réseau et cliquer sur Redirection de ports. Il faut ensuite donner les informations suivantes :

- un nom (SSH_Connect) ;
- un protocole (TCP) ;
- IP hôte (127.0.0.1) ;
- Port hôte (20022) ;
- IP invité (on laisse vide, car comme on est en DHCP l’assignation est dynamique) ;
- Port invité (22).

Une fois la règle validée, on teste :

```
$ ssh root@127.0.0.1 -p 20022
root@127.0.0.1's password:
Last login: Tue Mar 5 12:04:25 2019
[root@lxd ~]#
```

<img src="https://connect.ed-diamond.com/sites/default/files/articles/var/diamond/84311/net.png">

*Fig. 3 : Configuration de la redirection de ports.*

On aurait pu tout faire en ligne de commandes avec VBoxManage :

```
$ VBoxManage modifyvm "lxd" --natpf1 "SSH_Connect,tcp,127.0.0.1,20022,,22"
```

Le seul argument en plus est --natpf1 qui permet de cibler l’interface sur laquelle attacher la règle de redirection : natpf1 pour la première interface, natpf2 pour la seconde, etc. La commande VboxManage permet plein d’autres choses en ligne de commandes. Parfois il est même nécessaire de l’utiliser pour certaines configurations très particulières non interfacées en graphique.

### 3.2 La configuration réseau interne
La configuration réseau interne crée une sorte de commutateur virtuel qui permet de connecter les interfaces des VM domiciliées dans le même réseau interne. Sur notre VM, nous allons déclarer un réseau interne pour qu’elle puisse communiquer avec son clone que nous allons créer par la suite. La première chose à faire est d’éteindre la VM. Ensuite, il faut retourner dans la rubrique Réseau du menu Configuration de la VM. Cliquons sur l’onglet Interface 2 pour trouver la checkbox Activer l’interface réseau. On trouve un menu déroulant Mode d’accès réseau avec toutes les configurations possibles. Parmi ces options, nous allons choisir Réseau interne. Le dernier paramètre à renseigner est le nom du réseau interne. Ce nom doit être le même pour toutes les VM connectées par ce réseau interne. Nous l’appellerons LXD_net.

### 3.3 Les autres configurations
On trouve d’autres configurations réseau dans le menu déroulant. Nous allons très rapidement les évoquer. L’accès par pont a été détaillé dans l‘article sur LXD, mais en gros cette configuration expose votre VM sur le réseau au même plan que l’hôte. Cette configuration est souvent utilisée pour exposer la VM sur le monde extérieur. Enfin, la configuration Réseau privé hôte rend l’interface uniquement accessible par l’hôte. Pour clore le chapitre sur le réseau et VirtualBox, il faut savoir que le mode promiscuous est bloqué par défaut sur les interfaces. Ce mode est utilisé par les gens qui font de la capture réseau, donc si vous installez une sonde réseau en VM, il faut penser à autoriser ce mode en allant dans la section Avancé de la configuration et sélectionner Mode de promiscuité à autoriser toutes les VM.

## 4. Clonage
Nous allons maintenant cloner la VM. Pour ce faire, il suffit de faire un clic droit sur la VM et choisir l’option Cloner. Lorsque l’on fait un clone, il est important d’avoir plusieurs choses à l’esprit. La plupart du temps, il faut penser à réinitialiser les adresses MAC donc bien choisir Générer de nouvelles adresses MAC pour toutes les interfaces réseau pour l’attribut Politique d’adresse MAC. Pour bien comprendre quel type de clone on fait, il faut aller voir dans le Mode expert ce qui est sélectionné.

Par défaut, le type de clone est intégral. Cela veut dire que le stockage est dupliqué et que chaque machine va vivre sa vie de son côté. Pour comprendre le reste, il est nécessaire d’amener (succinctement) la notion de snapshot. Un snapshot (ou cliché instantané) est une photo du stockage de la machine à un instant t. Ensuite, lors d’une modification du système de fichiers, le bloc de donnée d’origine est sauvé dans l’espace réservé au snapshot avant d’être modifié. L’objectif est de pouvoir revenir au moment de la création du snapshot. Un clone lié est donc créé au-dessus d’un snapshot du système initial. L’avantage est un gain de place conséquent si le système n’est pas amené à beaucoup diverger de son parent. L’inconvénient est que cela crée une relation de dépendance forte entre le parent et le clone. Le dernier paramètre permet de copier les snapshots du parent sur le clone s’il y en a en cours.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/var/diamond/84311/clone.png">

*Fig. 4 : Configuration de la redirection de ports.*

À la fin de l’opération, nous obtenons une seconde machine virtuelle configurée à l’identique de la première y compris pour les aspects réseau. Ainsi, le clone est connecté à son parent via le réseau interne LXD_net. Le clone et la machine d’origine peuvent communiquer via le réseau LXD_net pour peu que la configuration IP de leurs interfaces soit correcte.

## 5. Les autres ressources (CPU et RAM)
Nous avons passé pas mal de temps sur le réseau et le stockage, mais il reste deux ressources importantes dont il faut discuter : le CPU et la mémoire vive.

Dans une VM, il est tout à fait possible de fixer le nombre de CPU à utiliser. Cependant, ce nombre ne peut excéder le nombre de cœurs (physiques, on ne parle pas des hyperthreads) de l’hôte. La raison en est très simple. Contrairement à des systèmes comme QEMU qui émulent un processeur complet, VirtualBox n’émule que le ring 0. Tout le reste tourne sur le vrai fil d’exécution du cœur, donc on ne peut pas excéder le nombre de cœurs de la machine. Cela limite aussi l’architecture processeur à celle de l’hôte. Pour simuler des processeurs logiques complets, il faut regarder du côté de QEMU.

Pour la mémoire, il est important de noter que l’addition de la quantité allouée par VM est supérieure à l’utilisation réelle visualisée sur l’hôte. Cela est dû au fait que VirtualBox intègre un mécanisme de déduplication des pages mémoires. Si plusieurs invités instancient la même page mémoire, elle n’est projetée en mémoire vive qu’une seule fois. Si les invités sont homogènes, le gain d’occupation de la mémoire vive est considérable.

## 6. Les « additions invités »
Les additions invités sont des compléments logiciels à installer sur l’invité. Ils permettent d’améliorer à la fois l’expérience utilisateur et les performances des systèmes virtualisés. L’augmentation des performances est rendue possible par l’installation d’un module noyau dans la VM qui va collaborer avec l’hyperviseur. Ainsi, avec l’ajout des additions invités, VirtualBox se rapproche de la para-virtualisation. Nous allons discuter de l’installation et détailler une de ces fonctions : le partage de fichiers.

### 6.1 Installation
Pour faire cela, c’est très simple. Un fois la VM démarrée, il faut aller dans le menu Périphériques et cliquer sur Insérer l’image CD des additions invités. On le monte dans la VM comme un CD traditionnel :

```
[root@lxd ~]# mount /dev/cdrom /mnt
```

Il faut installer quelques dépendances. Les additions invités doivent compiler un module noyau, il nous faut donc un compilateur (gcc) et les sources du noyau :

```
[root@lxd ~]# yum update
[root@lxd ~]# yum install gcc kernel-devel kernel-headers dkms make bzip2 perl
```

Enfin, on peut lancer l’installation des additions invités :

```
[root@lxd ~]# /mnt/VBoxLinuxAdditions.run --nox11
```

Ces additions apportent à la fois des améliorations au niveau de l’ergonomie (redimensionnement automatique de la fenêtre de la VM, copier-coller unidirectionnel ou bidirectionnel entre l’invité et l’hôte, drag and drop, partage de fichiers, etc.) et des performances (accélération graphique et ballooning mémoire). Le paramètre --nox11 spécifie que nous installons les additions invités sur une VM dénuée d’interface graphique.

### 6.2 Partage de fichiers
Avec les additions invités, il est très simple de partager des fichiers entre l’hôte et les invités. Pour configurer un partage, il faut aller dans la configuration de l’invité et choisir la rubrique Dossier partagés. En cliquant sur l’icône avec le plus, on peut ajouter un nouveau partage. Il faut lui spécifier le chemin du dossier, c’est-à-dire l’emplacement du dossier à partager sur l’hôte et un point de montage sur l’invité (le répertoire doit évidemment être créé au préalable). Ensuite, on peut choisir de placer le partage en lecture seule et/ou de le monter automatiquement au lancement de l’invité. Enfin, on peut aussi choisir de le rendre permanent pour que le partage survive au redémarrage de VirtualBox.

## Conclusion
Dans cet article, nous avons vu quelques éléments théoriques sur la virtualisation appliqués au logiciel VirtualBox. Nous avons ensuite étudié quelques fonctions clés de VirtualBox pour vous permettre de vous lancer rapidement et mettre en place vos VM. Le seul point qu’il vous manque dans cet article pour réellement être opérationnel sur la virtualisation en production est la compréhension du mode réseau accès par pont, mais nous avons détaillé celui-ci dans l’article sur LXD.


source : https://connect.ed-diamond.com/Linux-Pratique/lphs-045/decouvrez-la-virtualisation-avec-virtualbox