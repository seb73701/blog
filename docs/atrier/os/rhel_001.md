---
title: Structure de RHEL
description: 
published: true
date: 2023-04-21T11:22:22.534Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:22:17.946Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[<i class="fas fa-chevron-circle-left"></i>  Red Hat Enterprise Linux (RHEL)](rhel.md)

----

# Red Hat - Livre "Red Hat Enterprise Linux CentOS - Mise en production et administration de serveurs"

----

## Structure de RHEL

![Red Hat - Schema structure](../../../img/os/centos_rhel/linux_schema_structure_os.png)

Ce schéma présente les trois parties qui composent l’intégration d’un système d’exploitation avec la machine. Précisément, l’OS est composé des deux parties supérieures, l’espace utilisateur et le noyau :

+ L’**espace utilisateur** (_User Space_) : c’est l’espace mémoire où sont exécutés les processus utilisateurs ; c’est un espace protégé, où un processus ne peut pas interférer avec un autre processus. En résumé, c’est ce qui est visible de l’utilisateur.
+ Le **noyau** (_Kernel_): c’est l’interface entre les logiciels (en _user space_) et le matériel. Il se trouve dans un espace mémoire protégé ; les logiciels ne peuvent accéder au noyau que par l’intermédiaire des appels système (_System Calls_), qui sont des portes d’entrées avec un format particulier.
+ **Matériel** (_Hardware_) : c’est toute l’électronique de la machine qu’une personne veut utiliser par l’intermédiaire des programmes : processeur, mémoire, écran, clavier, souris, carte réseau Ethernet et Wi-Fi, disques durs, lecteur DVD et flash (et leurs contrôleurs), etc.


### L'espace utilisateur

![Red Hat - Schema structure](../../../img/os/centos_rhel/schema_espace_utilisateur.png)

Dans ce schéma, on trouve l'essentiel des différents éléments qui se trouvent en espace utilisateur :

+ **Logiciels utilisateurs** : programmes classiques lancés par l'utilisateur, à partir du shell en ligne de commande ou de l'interface graphique, ils sont l'une des finalités du systèmes d'exploitation. Par exemple le navigateur Internet Firefox en interface graphique, ou l'éditeur de texte `vi` en ligne de commande.
+ **Services** : ce sont des programmes dans l'espaces utilisateur, mais non visibles directement par l'utilisateur : ils sont exécutés de manière permanente en tâche de fond.
 + **Services ou serveurs réseau** : l'autre but d'un OS consiste à offrir des services à des programmes clients se trouvant sur d'autres machines. Les serveurs réseau, comme le serveur web Apache ou le serveur de base de données MariaDB, écoutent des requêtes arrivant sur l'interface réseau.
 + **Service système** : également exécutés en tâche de fond, leur but est de servir le système d'exploitation, par exemple en enregistrant les événements (_syslog_) ou en détectant l'ajout de périphériques (_udev_).
+ **Shells** : ce sont les interpréteurs de commande en mode texte. Ils permettent en autres de lancer les logiciels, mais également d'effectuer la configuration de la machine.
+ **Interface graphique** : c'est la version graphique des shells, permettant de lancer des logiciels.
+ **Configuration et Variables d'environnement** : la configuration de la machine est principalement stockée sous forme de fichiers dans `/etc`. Cependant, la configuration spécifique à chaque utilisateur se trouve plutôt dans des variables stockées en mémoire vive, plus rapidement accessibles que dans des fichiers.
+ **Bibliothèques** (_Librairies_) : ce sont des morceaux de code déjà compilés intégrés à un programme en C. Ces bibliothèques contiennent des fonctions toutes faites, qui sont appelées à l'intérieur d'un programme comme n'importe quelle fonction. L'intérêt est de mutualiser des fonctions qui peuvent être utilisées par plusieurs programmes.
 + **Bibliothèques statiques** (_Static librairies_) : le code d'une bibliothèque statique est intégré au moment de la compilation du programme, et en fait donc partie intégrante. On en parle rarement, car elles sont surtout utilisées par les développeurs, ou lorsqu'on compile soit-même un logiciel à partir du code source.
 + **Bibliothèques partagées** (_Shared Librairies_) : le code d'une bibliothèque partagée est appelée au moment de l'exécution du programme; si cette bibliothèque n'existe plus, le programme ne pourra pas s'exécuter (comme les DLL pour Windows). Ces bibliothèques se trouvent dans `/lib` ou `/lib64` et ont l'extension `.so` (_Shared Object, objet partagé_).
 + **Bibliothèque standard** `glibc` : c'est la version Linux de la "bibliothèque UNIX Standard C" respectant les standards ISO et POSIX. Il s'agit de la bibliothèque qui permet l'utilisation des **appels système** par les programmes en espace utilisateur. Un programme peut faire un appel système sans utiliser `glibc`, mais en utilisant une interruption. Ce cas est cependant plus rare.

### Interface graphique en espace utilisateur

L'interface graphique est composée de trois éléments principaux décrits dans le schéma suivant :

+ **X Window**, serveur d'affichage (_Display Server_) ou systèmede fenêtrage (_Window System_)
+ **Gestionnaire de fenêtre** (_Window Manager_)
+ **Environnement graphique** ou **environnement de bureau** (_Desktop Environment_)

 ![Red Hat](../../../img/os/centos_rhel/rhel_gui_espace_utilisateur.png)

Le **serveur d'affichage** (_Display Server_) ou **système de fenêtrage** (_Window System_) permet aux clients d'interagir avec le matériel (écran, clavier, souris). Le serveur d'affichage n'est pas une interface graphique, mais a le contrôle du matériel à travers le noyau; il est ainsi la porte d'entrée des clients pour afficher quelques chose à l'écran et est composé des éléments suivants :

+ **Protocole X** (_X protocol_) : c'est le protocole de communication réseau (IPv4 ou IPv6) entre les clients X et le serveur X, par l'intermédiaire de requêtes d'affichage.
+ **XLib** (_Librairie X_) : c'est la bibliothèque logicielle exploitée par les clients X pour utiliser le procotole X. Elle offre de fonctions toutes faites aux programmes d'applications graphiques (clients). En général, elle n'est pas utilisée directement, mais sert de base à des bibliothèques graphiques de plus haut niveau comme GTK ou Qt.
+ Le type d'affichage actuel est **X Window**, dont l'implémentation libre s'appelle X.Org.
+ **Wayland** est appelé à remplacer X Window, avec un nouveau serveur d'affichage (Wayland Compositor), un protocole et une bibliothèque logicielle.

Le **Gestionnaire de fenêtres** (_Window Manager_), principal client du serveur X Window, est chargé de l'affichage, de la décoration et du placement des fenêtres. Intermédiaire enre le serveur d'affichage et l'environnement graphique, il joue le rôle de chef d'orchestre et offre les moyens de déplacer, redimensionner, réduire, fermer, décorer les fenêtres utilisées par les autres clients.

On en trouve surtout deux :

+ **Metacity** : c'est le gestionnaire de fenêtre utilisé pour GNOME 2 utilisant ma bibliothèque graphique GTK+.
+ **Mutter** : à partir de GNOME 3, Mutter remplace Metacity et utilise la bibliothèque graphique Clutter.
+ Librairies (_bibliothèques graphiques_) : ensemble de bibliothèques logicielles graphiques, c'est le coeur du Gestionnaire de fenêtre. Exemple : GTK+ (_Gimp Toolkit_). A l'origine développée pour le logiciel de traitement d'image GIMP, cette bibliothèque utilise GLib, qui est un ensemble de bibliothèques de bas niveau pour la partie non graphique de l'interface graphique (ne pas confondre avec glibc).

L'**environnement graphique** ou **environnement de bureau** (_Desktop Environment_) est un gestionnaire de fenêtre enrichi, apportant une expérience unifiée à l'utilisateur. Un ensemble de programmes (par exemple logiciels de traitement de texte et d'images, terminal, navigateur Internet, etc.) est fourni, ainsi que des panneaux, des menus, un gestionnaire de fichiers par défaut, des styles, fonds d'écran, widgets, etc.

On en trouve deux principaux, mais il en existe des multitudes :

+ **GNOME** : environnement de bureau préféré pour RHEL.
 + **GNOME 2** : gestionnaire de fenêtres, utilisé pour RHEL 6.X utilisant Metacity.
 + **GNOME 3** : cette version de GNOME basé sur Mutter se démarque de la philosophie "Bureau" classique, pour faire place à la philisophie "Shell", plutôt orienté tactile et qui n'est pas sans rappeler un mélange des interfaces Windows 8 et Android.
+ **KDE** : environnement de bureau très célèbre concurrent de GNOME utilisant la bibliothèque Qt.

### Le noyau

_(inclure image page 152)_

Le noyau comprends les éléments suivants :

+ **Appels système** (_System Calls ou syscalls_) : il s'agit d'une porte d'entrée vers le noyau pour les programmes se trouvant en espace utilisateur,dont l'objectif est simplement l'utilisation des ressources matérielles (écran, clavier, réseau, mémoire, processeur, etc.). Ce sont des fonctions standards appelées par un programme en espace utilisateur. Elles sont exécutées dans le noyau et le résultat est renvoyé au programme. Ces fonctions sont matérialisées par une bibliothèque logicielle fournie par le système d'exploitation. La bibliothèque est incluse dans le programme, à l'exécution ou à la compilation. Le nombre d'appels système est relativement réduit; sur les noyaux récents en architecture x86 il y en a environ 350-380.
+ **Sous-systèmes principaux** : le noyau est divisé en cinq grandes catégories principales - les sous-systèmes - accessibles grâce aux appels système. Chaque catégorie présente des fonctionnalités, c'est-à-dire une division plus précise des tâches effectuées par le noyau pour un sous-système; il existe bien d'autres fonctionnalités offertes par les sous-systèmes.
 + **Contrôle des processus** : création et destruction des processus, partage du temps CPU par les processus (ordonnanceur ou _scheduler_), communication des processus vers l'extérieur, _concurrency, multitasking_, etc.
 + **Communication interprocessus** (_socket_) : fournir des mécanismes pour la communication entre processus à l'intérieur du système d'exploitation (et non pas vers d'autres machines).
 + **Gestion de la mémoire** : gérer la façon de la mémoire est organisée, et permettre à plusieurs processus indépendants de partager la mémoire de manière sécurisée, ainsi qu'à une mémoire virtuellement plus importante que la mémoire physique disponible (en utilisant des fichiers d'échange, ou swap) : c'est la mémoire virtuelle. La mémoire est gérée par pages; un processus se voit affecter un certain nombre de pages. Sur la plupart des architectures, les pages ont une taille de 4 Ko. Le système garde une trace des pages qui sont utilisées, totalement ou en partie, et de celles qui sont vides.
 + **Gestion des fichiers** : organiser la façon dont les données sont stockées, en présentant un modèle unifié quel que soit ke support physique de stockage : c'est le système de fichiers virtuels VFS (_Virtual File System_). Le VFS propose la mise en cache avec une couche intermédiaire qui optimise l'accès aux données en les gardant en mémoire pendant quelque temps.
 + **Gestion des périphériques** : présenter une interface d'accès commune à une variété de périphériques physiques.
 + **Gestion du réseau** : fournir l'accès à différents standards et fonctionnalités réseau et types d'interfaces physiques.
+ **Implémentation logicielle / matérielle** (couche bas niveau) : c'est le code qui met en oeuvre les fonctionnalités décrites ci-dessus. Il est de deux natures :
 + Code noyau indépendant de l'architecture matérielle : la majeure partie du code du noyau est indépendante du matériel, c'est-à-dire qu'elle est commune à toutes les architectures matérielles supportées par Linux et est donc facilement portable d'une architecture à une autre.
 + Code dépendant de l'architecture : une partie du code est dépendante de l'architecture, ne fonctionnant que sur cette architecture matérielle; il s'agit par exemple du code adapté au jeu d'instructions des processeurs (ARM pour mobile, x86/x64, etc.) fourni par le constructeur (ou kes consortiums) et mis en place sur la carte mère. Cela est appelé plus généralement le _Board Support Package_ (ou BSP).
+ **Modules** : morceaux de code permettant d'ajouter dynamiquement des fonctionnalités au noyau. Les modules sont chargés à la demande par l'utilisateur, contrairement aux patches qui sont des morceaux de code ajoutés au moment de la compilation du noyau. Il existe des modules entre autres pour des pilotes de périphériques, des nouveaux systèmes de fichiers, des pilotes réseaun voire des appels système.