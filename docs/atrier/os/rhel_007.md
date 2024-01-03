---
title: Démarrage Système
description: 
published: true
date: 2023-04-21T11:14:52.142Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:14:47.544Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[<i class="fas fa-chevron-circle-left"></i>  Red Hat Enterprise Linux (RHEL)](rhel.md)

----

# Red Hat - Livre "Red Hat Enterprise Linux CentOS - Mise en production et administration de serveurs"

----

## Démarrage Système

### La séquence de démarrage

Sur Linux, il est bien de connaître les différentes étapes de démarrage, depuis le moment où on appui sur le bouton ON de la machine, jusqu'à l'apparition d'une invite de login, ou connexion.

Pourquoi connaître la séquence de démarrage ?

+ Pour obtenir une configuration fine du système
+ Adapter le système au besoin d'utilisation
+ Améliorer les performances
+ Renforcer la sécurité
+ Pour le dépannage

De l'appui sur le bouton ON jusqu'à l'invite de connexion :

+ **BIOS** : le BIOS de la machine effectue une vérification physique de la machine et lance le boot loader (chargeur de démarrage) qui se trouve dans le MBR (_Master Boot Record_) du premier disque dur.
+ **Boot loader niveau 1** : le boot loader de niveau 1 lance un boot loader de niveau 2 qui se trouve sur la partition `/boot/`.
+ **Boot loader niveau 2** : le boot loader de niveau 2 monte l'image `initrd` et y charge les modules nécessaires. Le noyau monte la partition racine `/` en lecture seule.
+ **Noyau** : le noyau lance ensuite le premier programme `/sbin/init`
+ **Espace utilisateur**, `Init` : `/sbin/init` ou `systemd` charge le reste du système : services, environnement utilisateur et montage des partitions.
+ **Login** : finalement, `/sbin/init` ou `systemd` présente l'invite de connexion à l'utilisateur.

### BIOS

Le BIOS (_Basic Input/Output System_, ou système élémentaire d'entrée/sortie) est un programme se trouvant dans une mémoire en lecture seule, dont le principal objectif est de fournir un accès aux différents éléments physiques de la machine. Il peut être utilisé par le système d'exploitation, mais on le connaît surtout pour son utilisation au démarrage de la machine.

Lorsqu'on appui sur le bouton ON du serveur, le BIOS effectue un POST (_Power-On Seft Test_, autotest d'allumage). La mémoire est vérifiée, les disques et aux autres composants sont déclarés, des diagnostics sont effectués.

Si tout va bien, le BIOS lance alors le système d'exploitation sur le **périphérique d'amorçage**, c'est-à-dire le périphérique qui va lancer le système d'exploitation. En général, il cherche d'abord sur le lecteur de disquette et de CD-ROM, puis sur le disque dur.

Précisement, sur le périphérique d'amorçage, l'"amorce" que l'on recherche pour le lancement du système d'exploitation s'appelle un **bootloader** (ou chargeur de démarrage).

Le bootloader installé et utilisé par Red Hat Enterprise Linux s'appelle GRUB.

### UEFI

L'UEFI, de plus en plus répandu sur les machines récentes, se pose en successeur du BIOS par l'apport de fonctionnalités, comme une interface haute résolution, le support intégré du réseau et le multiboot, c'est-à-dire la possibilité d'installer plusieurs systèmes d'exploitation et d'en faire la sélection à partir de l'UEFI et non à partir du chargeur de démarrage.

Mais la fonctionnalité principale de l'UEFI est le `secure boot`, dont l'objectif est de déjouer l'installation d'un rootkit par un pirate, en installant un noyau contenant des failles de sécurité à la place d'un noyau légitime.

Le secure boot (démarrage sécurisé) demande ainsi le lancement d'un noyau signé avec une clé privée autorisée. L'UEFI contient un jeu de clés publiques officielles correspondantes. Lorsque cette fonctionnalité est activée, un noyau non signé ne peut être exécuté.

Il est bien entendu que seuls les éditeurs officiels de systèmes d'exploitation possèdent cette clé privée. Ainsi, tous les noyaux officiels fournis par Red Hat sont signés, et on a rien à faire sinon qu'à activer le secure boot si l'on souhaite.

> Le secure boot concerne aussi le chargement de modules signés dans le noyau.


### Chargeur de démarrage GRUB

Beaucoup de bootloaders ne sont en général qu'un morceau de code se trouvant au début du disque dur d'amorçage, servant à lancer directement le système d'exploitation unique installé sur le système.

GRUB permet de sélectionner au démarrage un des systèmes d'exploitation présents sur le disque dur. S'il s'agit de Linux, il permet également de choisir le noyau à démarrer, voir même de lui passer des options.

Par exemple, GRUB pourrait présenter les choix suivants : 

+ _Red Hat Linux_
+ _Red Hat Linux en mode maintenance (option passée au noyau)_
+ _Red Hat Linux SMP (choix d'un autre noyau avec support multiprocesseur)_
+ _Microsoft Windows 10 (choix d'un autre système d'exploitation)_

Ainsi, GRUB (_GRand Unified Bootloader_, chargeur de démarrage unifié) ne se limite pas à un système d'exploitation en particulier, même s'il est le plus souvent installé par des systèmes Linux comme Red Hat Enterprise Linux.

GRUB présente beaucoup d'autres avantages, parmi lesquels la possibilité de pouvoir modifier les options de démarrage du système.

Il existe deux versions de GRUB :

+ **GRUB 0.9X** : première version de GRUB, installé sur RHEL5/6, appelée également GRUB Legacy (GRUB héritage).
+ **GRUB 2** : réécriture, à partir de RHEL 7.

### GRUB Legacy (RHEL 5/6)

#### Level 1 et Master Boot Record

Le MBR, ou Zone d'Amorce est le premier secteur du disque dur d'une taille de 512 octects. Il contient principalement le bootloader de niveau 1 de GRUB et la table de partitions du disque.

Le MBR ayant une taille trop réduite pour contenir un bootloader "entier", il est nécessaire de charger un autre programme contenant plus de fonctionnalités. C'est pourquoi GRUB est séparé en deux niveaux.

Le bootloader de niveau 1 indique ainsi à quel endroit trouver le bootloader de niveau 2. Ce bootloader est alors exécuté.

#### Level 2

C'est le bootloader qui contient toutes les fonctionnalités de GRUB et présente une interface. Ce bootloader a donc une taille plus importante et se trouve sur une partition appelée `/boot/`.

> **Remarque**
> GRUB Legacy ne sait lire que les systèmes de fichiers `ext2`, `ext3` et `ext4`. C'est pourquoi la partition `/boot/` ne peut pas être formatée en `NTFS` par exemple (système de fichiers de Windows) ou en LVM.

> **Remarque**
> Il existe dans certains cas exceptionnels un bootloader de niveau 1.5 pour des raisons imposées par le matériel.

En fonction de la configuration, le bootloader niveau 2 lance le système d'exploitation. Il s'agit bien souvent d'un système d'exploitation Windows iy de type UNIX/Linux.
Dans le cas d'un système d'exploitation Linux, le bootloader niveau 2 lance le noyau du système d'exploitation se trouvant dans `/boot/`.

Pour résumer, au début du périphérique d'amorçage (le disque dur) se trouve une zone appelée MBR contenant le chargeur de démarrage de niveau 1. Ce dernier lance un charge de démarrage de niveau 2 situéà un autre endroit sur le disque. C'est lui qui ensuite lance le système d'exploitation (un noyau Linux par exemple).

> **Remarque**
> GRUB Legacy étant maintenant obsolète.

### GRUB 2 (RHEL 7/8)

La version 2 de GRUB a été réécrite à partir de zéro, en ajoutant de nombreuses modifications. Ainsi, GRUB version 2 n'hérite pas du fonctionnement du GRUB d'origine, qu'on appelle rétroactivement GRUB Legacy (GRUB héritage), et par ricochet, GRUB version 2 devient simplement GRUB.

Entre autres modifications importantes, GRUB peut maintenant lire des partitions LVM et RAID. De plus, le fichier de configuration, dont la syntaxe est différente, inclut un langage de script permettant des tests de conditions et des boucles.

Le lancement de GRUB se fait en trois étapes :

+ **Stage 1** : c'est le morceau de code appelé `boot.img` lancé par la machine pour le démarrage de l'OS. Il est stocké sur le secteur 0 de 512 octets où se trouve également la table de partition du disque. Son travail est simplement de lancer le **Stage 1.5**.
+ **Stage 1.5** : c'est le code appelé `core.img` (d'environ 20-25 Ko) stocké sur les secteurs 1 à 62 du disque, avant le début de la première partition. Son travail est de charger des pilotes supportant de multiples systèmes de fichiers, puis de lancer le **Stage 2** contenu dans l'un de ces systèmes de fichiers.
+ **Stage 2** : c'est la dernière partie du bootloader chargée d'afficher un menu pour l'utilisateur et de lancer les OS. Le stage 2 est contenu dans le répertoire `/boot/grub`.

#### Les interfaces

Au démarrage du système, GRUB présente une interface de démarrage, ou interface Menu. Cette interface donne en fait accès à deux autres interfaces.

##### Interface Menu

C'est l'interface par défaut affichée au démarrage de la machine. Le choix est donné parmi une liste de systèmes d'exploitation, ou de versions de Linux avec différents noyaux. On se déplace avec les flèches du clavier et on appui sur [Entrée] pour lancer le système qu'on a choisit.

![Red Hat](../../../img/os/centos_rhel/grub2_menu_os.png)

Ce menu donne également accès aux deux autres interfaces:
+ On appui sur la touche [e] pour accéder à l'éditeur d'interface
+ On appui sur la touche [c] pour accéder à la ligne de commande.

##### Editeur d'interface

Ce menu permet de modifier la configuration pour une des entrées de l'interface Menu en appuyant sur la touche [e].

Chaque entrée de l'interface Menu peut être modifiée grâce à un fichier de configuration `/boot/grub2/grub.cfg` accessible une fois Linux lancé. Cependant, avec l'éditeur d'interface, on peut modifier une entrée dynamiquement, au démarrage, sans avoir à éditer ce fichier.

![Red Hat](../../../img/os/centos_rhel/grub2_menu_os_edit.png)


Voici le contenu du fichier de configuration de GRUB `/boot/grub2/grub.cng`

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# cat /boot/grub2/grub.cfg
```
<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
#
# DO NOT EDIT THIS FILE
#
# It is automatically generated by grub2-mkconfig using templates
# from /etc/grub.d and settings from /etc/default/grub
#

### BEGIN /etc/grub.d/00_header ###
set pager=1

if [ -f ${config_directory}/grubenv ]; then
  load_env -f ${config_directory}/grubenv
elif [ -s $prefix/grubenv ]; then
  load_env
fi
if [ "${next_entry}" ] ; then
   set default="${next_entry}"
   set next_entry=
   save_env next_entry
   set boot_once=true
else
   set default="${saved_entry}"
fi

if [ x"${feature_menuentry_id}" = xy ]; then
  menuentry_id_option="--id"
else
  menuentry_id_option=""
fi

export menuentry_id_option

if [ "${prev_saved_entry}" ]; then
  set saved_entry="${prev_saved_entry}"
  save_env saved_entry
  set prev_saved_entry=
  save_env prev_saved_entry
  set boot_once=true
fi

function savedefault {
  if [ -z "${boot_once}" ]; then
    saved_entry="${chosen}"
    save_env saved_entry
  fi
}

function load_video {
  if [ x$feature_all_video_module = xy ]; then
    insmod all_video
  else
    insmod efi_gop
    insmod efi_uga
    insmod ieee1275_fb
    insmod vbe
    insmod vga
    insmod video_bochs
    insmod video_cirrus
  fi
}

terminal_output console
if [ x$feature_timeout_style = xy ] ; then
  set timeout_style=menu
  set timeout=5
# Fallback normal timeout code in case the timeout_style feature is
# unavailable.
else
  set timeout=5
fi
### END /etc/grub.d/00_header ###

### BEGIN /etc/grub.d/00_tuned ###
set tuned_params=""
set tuned_initrd=""
### END /etc/grub.d/00_tuned ###

### BEGIN /etc/grub.d/01_menu_auto_hide ###
if [ "${boot_success}" = "1" -o "${boot_indeterminate}" = "1" ]; then
  set last_boot_ok=1
else
  set last_boot_ok=0
fi

# Reset boot_indeterminate after a successful boot
if [ "${boot_success}" = "1" ] ; then
  set boot_indeterminate=0
# Avoid boot_indeterminate causing the menu to be hidden more then once
elif [ "${boot_indeterminate}" = "1" ]; then
  set boot_indeterminate=2
fi
set boot_success=0
save_env boot_success boot_indeterminate

if [ x$feature_timeout_style = xy ] ; then
  if [ "${menu_show_once}" ]; then
    unset menu_show_once
    save_env menu_show_once
    set timeout_style=menu
    set timeout=60
  elif [ "${menu_auto_hide}" -a "${last_boot_ok}" = "1" ]; then
    set orig_timeout_style=${timeout_style}
    set orig_timeout=${timeout}
    if [ "${fastboot}" = "1" ]; then
      # timeout_style=menu + timeout=0 avoids the countdown code keypress check
      set timeout_style=menu
      set timeout=0
    else
      set timeout_style=hidden
      set timeout=1
    fi
  fi
fi
### END /etc/grub.d/01_menu_auto_hide ###

### BEGIN /etc/grub.d/01_users ###
if [ -f ${prefix}/user.cfg ]; then
  source ${prefix}/user.cfg
  if [ -n "${GRUB2_PASSWORD}" ]; then
    set superusers="root"
    export superusers
    password_pbkdf2 root ${GRUB2_PASSWORD}
  fi
fi
### END /etc/grub.d/01_users ###

### BEGIN /etc/grub.d/10_linux ###
insmod part_msdos
insmod ext2
set root='hd0,msdos1'
if [ x$feature_platform_search_hint = xy ]; then
  search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 --hint='hd0,msdos1'  1a03c597-e892-4ba0-a4fd-7897d99591c1
else
  search --no-floppy --fs-uuid --set=root 1a03c597-e892-4ba0-a4fd-7897d99591c1
fi
insmod part_msdos
insmod ext2
set boot='hd0,msdos1'
if [ x$feature_platform_search_hint = xy ]; then
  search --no-floppy --fs-uuid --set=boot --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 --hint='hd0,msdos1'  1a03c597-e892-4ba0-a4fd-7897d99591c1
else
  search --no-floppy --fs-uuid --set=boot 1a03c597-e892-4ba0-a4fd-7897d99591c1
fi

# This section was generated by a script. Do not modify the generated file - all changes
# will be lost the next time file is regenerated. Instead edit the BootLoaderSpec files.
#
# The blscfg command parses the BootLoaderSpec files stored in /boot/loader/entries and
# populates the boot menu. Please refer to the Boot Loader Specification documentation
# for the files format: https://www.freedesktop.org/wiki/Specifications/BootLoaderSpec/.

set default_kernelopts="root=/dev/mapper/cl-root ro resume=/dev/mapper/cl-swap rd.lvm.lv=cl/root rd.lvm.lv=cl/swap "

insmod blscfg
blscfg
### END /etc/grub.d/10_linux ###

### BEGIN /etc/grub.d/20_linux_xen ###
### END /etc/grub.d/20_linux_xen ###

### BEGIN /etc/grub.d/20_ppc_terminfo ###
### END /etc/grub.d/20_ppc_terminfo ###

### BEGIN /etc/grub.d/30_os-prober ###
### END /etc/grub.d/30_os-prober ###

### BEGIN /etc/grub.d/30_uefi-firmware ###
### END /etc/grub.d/30_uefi-firmware ###

### BEGIN /etc/grub.d/40_custom ###
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
### END /etc/grub.d/40_custom ###

### BEGIN /etc/grub.d/41_custom ###
if [ -f  ${config_directory}/custom.cfg ]; then
  source ${config_directory}/custom.cfg
elif [ -z "${config_directory}" -a -f  $prefix/custom.cfg ]; then
  source $prefix/custom.cfg;
fi
### END /etc/grub.d/41_custom ###
```

L'éditeur d'interface est très utile pour tester plusieurs types de démarrage pour un système d'exploitation, ou pour corriger une mauvaise configuration qui empêcherait un OS de démarrer.

+ Pour l'édition, il faut utiliser les flèches et on édite directement le texte désiré.
+ Pour lancer l'entrée avec la modification, on appui sur les touches [Ctrl][X].
+ Sinon, on appui sur la touche [Echap] pour abandonner les modifications et retourner à l'interface du Menu.

> **Remarque**
> Attention, les modifications faites par l'éditeur d'interface ne sont pas sauvegardées dans le fichier de configuration de GRUB `/boot/grub2/grub.cfg`.

##### Ligne de commande

La ligne de commande permet de faire à la main ce que fait le fichier de configuration de GRUB, par exemple démarrer un système d'exploitation ou changer l'image de démarrage. Cela est pratique pour le dépannage par exemple, où une configuration corrompue de GRUB qui empêcherait le système de démarrer.

#### Fichier de configuration

Le fichier de configuration de GRUB est `/boot/grub2/grub.cfg`. C'est le fichier utilisé pour l'affichage du menu de démarrage et la sélection du noyau avec les options de démarrage.

Cependant, ce fichier n'est **pas modifiable directement**, contrairement à GRUB Legacy. On peut le modifier manuellement (car c'est un fichier texte facile à éditer), mais on prendrait le risque de perdre ces modifications à chaque mise à jour de GRUB.

Le fichier `grub.cfg` est construit à partir de scripts de création et d'un fichier de configuration dans `/etc` :

+ `/etc/default/grub` : paramètres généraux de GRUB
+ `/etc/grub.d/` : scripts de création de menu GRUB, dont le fichier `40_custom` contenant des entrées personnalisées du menu.

Une fois ces fichiers modifiées selon notre besoin, **il faut générer un nouveau fichier de configuration** avec la commande `grub2-mkconfig`, en n'oubliant pas de préciser l'emplacement du fichier destination :

```shell
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Pour GRUB sur UEFI, le fichier de configuration se trouve dans `/boot/efi`. Il faut dans ce cas taper la commande suivante pour générer un nouveau fichier de configuration :

```shell
grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg
```

> **Remarque**
> Il est important de recréer le menu (c'est-à-dire le fichier `grub.cfg`) après chaque modification, sinon les changements seront pas pris en compte.

##### Ajouter un nouveau noyau

Pour ajouter un nouveau noyau, c'est très simple :

+ Installez le nouveau noyau avec `yum`. Le nouveau noyau s'installe en tant que `vmlinux-*` dans `/boot`.
+ Exécutez la commande de création de configuration ci-dessus, qui détectera automatiquement le nouveau noyau et l'ajoutera au menu.

##### Changer les paramètres généraux de GRUB 2

Le fichier `/etc/default/grub` contient les paramètres du comportement général de GRUB. Parmi les variables intéressantes, dont la plupart modifient toutes les entrées du menu, on trouve :

+ `GRUB_TIMEOUT=<secondes>` : délai d'attente avant de démarrer le choix par défaut.
+ `GRUB_DISTRIBUTOR=` : contient le nom du système d'exploitation. C'est un champ dynamique, généré à partir du fichier `/etc/system-release` créé par Red Hat.
+ `GRUB_CMDLINE_LINUX="<arguments passés au noyau>"` : contient, entre guillemets, la liste des arguments passés au noyau au démarrage. C'est par exemple à cet endroit qu'on changera la cible de systemd pour démarrer en mode graphique ou texte.
+ `GRUB_DEFAULT=<numéro>` : contient le numéro de l'entrée à démarrer par défaut, en partant de 0. Il pointe souvent vers la première entrée dans le menu (l'entrée 0 donc).

Il est possible de mettre le mot-clé `saved` pour que GRUB se rappelle de l'entrée choisie au prochain redémarrage (en conjonction avec la variable `GRUB_SAVEDEFAULT=true`). La valeur est alors sauvegardée dans `/boot/grub2/grubenv` (appelé "Environment Block"). Il est aussi possible de positionner cette valeur avec la commande `grub2-set-default` ou `grub2-reboot` (la valeur est positionnée simplement pour le prochain redémarrage, par exemple pour un test).

##### Ajouter une entrée manuellement

Les entrées du menu GRUB sont ajoutés automatiquement en fonction des noyaux détectés dans `/boot` et des paramètres généraux.

Pour ajouter une entrée manuellement, avec tous les paramètres désirés, il faut l'insérer dans le fichier `/etc/grub.d/40_custom`.

Ce fichier doit contenir une ou plusieurs entrées commençant par `menuentry`, suivi d'une section commençant par `{` et finissant par `}`. Ces entrées doivent être positionnées après les remarques et commandes du début (en général à partir de la ligne 6).

```shell
#!/bin/sh
exec tail -n +3 $0
# This file provides an easy way to add custom menu entries.  Simply type the
# menu entries you want to add after this comment.  Be careful not to change
# the 'exec tail' line above.
```

> **Remarque**
> On peut bien sûr créer une entrée de toutes pièces dans `40_custom`, mais l'idéal est de copier et de modifier une entrée existante du fichier `/boot/grub2/grub.cfg`.

##### Configurer l'authentification

La fonctionnalité la plus nécessaire de GRUB est sans doute l'authentification. Il est important de mettre en oeuvre la protection contre un accès illicite au menu GRUB.

L'authentification peut se faire :
+ Sur l'édition d'une entrée du menu GRUB.
+ Sur le simple lancement d'une des entrées pour un OS.

Pour cela, il faut créér le fichier `/etc/grub.d/01_users` s'il n'existe pas, avec les droits nécessaires :

```shell
touch /etc/grub.d/01_users
chmod 755 /etc/grub.d/01_users
```

> **Remarque**
> Ce fichier est normalement créé à l'installation si l'authentification de GRUB a été choisie à ce moment là.

Exemple :

```shell
#!/bin/sh -e
cat << EOF
if [ -f \${prefix}/user.cfg ]; then
  source \${prefix}/user.cfg
  if [ -n "\${GRUB2_PASSWORD}" ]; then
    set superusers="root"
    export superusers
    password_pbkdf2 root \${GRUB2_PASSWORD}
  fi
fi
EOF
```

Pour limiter l'**édition** des entrées à un ou plusieurs utilisateurs (les super-utilisateurs), ajouter les lignes suivantes dans `/etc/grub.d/01_users` :

```shell
#!/bin/sh -e
## Declare users and passwords
cat <<EOF
set superusers="<super-utilisateur>"
password <super-utilisateur> <mot de passe>
EOF
```

> **Remarque**
> Précisément, le ou les super-utilisateurs sont autorisés à utiliser la ligne de commande GRUB, à éditer ou à démarrer une entrée.

Si l'on souhaite limiter le **lancement** d'une entrée à un ou plusieurs utilisateurs, il faut ajouter ces lignes :

```shell
#!/bin/sh -e
## Declare users and passwords
cat <<EOF
set superusers="<super-utilisateur>"
password <super-utilisateur> <mot de passe>
password <super-utilisateur> <mot de passe>
EOF
```

Par exemple, les utilisateurs `eunice` et `macy` sont super-utilisateurs. Quant à `rico`, c'est un utilisateur normal :

```shell
#!/bin/sh -e
## Declare users and passwords
cat <<EOF
set superusers="<eunice macy>"
password eunice kawabunga
password macy kungfu
password rico yahanajessica
EOF
```

Il faut ensuite éditer l'entrée dont on désire limiter le lancement **directement dans le fichierr** `/boot/grub2/grub.cfg`. Ajoutez l'option `--users` à l'entrée nécessaire commençant par `menuentry`.

Par exemple, on remplace :

```shell
menuentry 'Red Hat Enterprise Linux Server, with Linux 3.10' {
```

Par :

```shell
menuentry 'Red Hat Enterprise Linux Server, with Linux 3.10' --users "rico" {
```

Ici, seul l'utilisateur `rico` (avec les super-utilisateurs) sera autorisé à **lancer** cette entrée dans le menu. Sans cela, le démarrage ne sera pas effectué et la machine bloquera au menu GRUB.

Puisque l'on a édité le fichier `grub.cfg` directement, il ne faut pas lancer la commande `grub2-mkconfig`, auquel cas nos modifications seront écrasées.
L'alternative consiste à copier cette entrée modifiée du fichier `grub.cfg` dans le fichier `40_custom`. Ainsi, même en cas de recompilation, l'entrée sera recréée.

##### Authentification avec mot de passe hashé

Bien sûr, le mot de passe en clair dans `01_users` doit absolument être évité. Il peut être remplacé par un hash de mot de passe au format PBKDF2 généré en ligne de commande avec la commande `grub2-mkpasswd-pbkdf2`.

Par exemple, générons un mot de passe hashé :

```shell
[root@localhost ~]# grub2-mkpasswd-pbkdf2
Entrez le mot de passe :
Entrez de nouveau le mot de passe :
Le hachage PBKDF2 du mot de passe est grub.pbkdf2.sha512.10000.3CAD760A95611DE4A5BB6B9657FBFEB38CF37901B7E6213868FA341005B57F6F39BFB41534B51083513B6C291ECBF7028A467C92E3EA89AC2A50009C2EFB0FCD.3D3C7506277441403B0DFA791195BE99138B5F76C77D4391869A414353557442666610DC99B81E23F5F366F45D2F314CDD8ED02CBBCFB523419D892BEB8AF0A0
```

Ensuite, on copie ce hash dans le fichier `01_users` pour l'utilisateur `rico` (tout est sur une seule ligne) :

```shell
password_pbkdf2 rico grub.pbkdf2.sha512.10000.3CAD760A95611DE4A5BB6B9657FBFEB38CF37901B7E6213868FA341005B57F6F39BFB41534B51083513B6C291ECBF7028A467C92E3EA89AC2A50009C2EFB0FCD.3D3C7506277441403B0DFA791195BE99138B5F76C77D4391869A414353557442666610DC99B81E23F5F366F45D2F314CDD8ED02CBBCFB523419D892BEB8AF0A0
```

> **Remarque**
> Il faut noté que le mot-clé `password` est remplacé par `password_pbkdf2`.

##### Réinstallation de GRUB

Il peut être nécessaire de réinstaller GRUB si les différents stages ont été modifiés (par exemple si un autre OS a installé son propre chargeur de démarrage) ou si la configuration a été corrompue.

On a le choix :
+ Simplement réinstaller les stages 1 et 1.5 avec la configuration existante de GRUB
+ Revenir à la configuration par défaut de GRUB

Pour **réinstaller les stages de GRUB sur le disque dur** (au cas où ils auraient été effacés), utilisez la commande suivante :

```shell
grub2-install /dev/<disque>
```

Cette commande réinstallera le stage 1 dans le MBR, le stage 1.5 dans les bons secteurs. Si le stage 2 (`/boot/grub2`) n'existe pas, il sera recrée.

Par exemple, pour réinstaller les stages de GRUB sur le premier disque :

```shell
grub2-install /dev/sda
Installing for i386-pc platform
Installation terminée, sans erreur.
``` 

> **Remarque**
> Notons que la configuration `/boot/grub2/grub.cfg` (stage 2) est indépendante des stages 1 et 1.5.

**Réinitialiser la configuration** de GRUB consiste à :
+ Supprimer les différents fichiers de configurations
+ Demander à `yum` de réinstaller le paquet
+ Relancer la compilation de `grub.cfg`
+ Si nécessaire, réinstaller les stages sur le disque.

Pour cela, tapez les commandes suivantes :

```shell
rm /etc/grub.d/*
rm /etc/sysconfig/grub
yum reinstall grub2-tools
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Ainsi une nouvelle configuration par défaut sera créée à la réinstallation du paquet `grub2-tools`. Ensuite, une nouvelle configuration minimale en fonction des noyaux disponibles sera créée par `grub2-mkconfig`.

> **Remarque**
> Il n'est pas nécessaire de réinstaller les stages 1 et 1.5 avec `grub2-install` si c'est seulement la configuration du menu qui pose problème.


### Lancement du noyau et du processus père

#### Rôle du noyau (kernel)

Une fois le système d'exploitation choisi, s'il s'agit d'un système Linux, le chargeur de démarrage lance le noyau, qui prendl la suite des opérations de démarrage.

Comme son nom l'indique, le noyau est l'élément essentiel du système d'exploitation. Son rôle est de faire l'interface entre le matériel, la machine, et le logiciel, c'est-à-dire les composants logiciels du système d'exploitation comme le shell (pour le contrôle de la machine) et les différents programmes exécutés.

En tant qu'interface, le noyau fournit une couche d'abstraction aux logiciels.
En effet, les logiciels ne savent pas gérer directement la mémoire, le processeur et les supports de stockage. C'est au noyau de fournir des branchements "universels" aux logiciels pour accéder aux ressources physiques. Bien sûr, grâce au noyau, les logiciels peuvent également communiquer entre eux.

Ainsi, le noyau est un ensemble de mécanismes gérant :
+ des fonctions
 + la gestion des systèmes des fichiers
 + la gestion des processus (des logiciels)
 + les fonctions réseau : mise en oeuvre de IPv4 et IPv6, pare-feu, etc.
 + etc.
 + ces fonctions peuvent faire partie intégrante du noyau, ou être ajoutées dynamiquement en chargeant des modules d'extension.
+ des accès aux composants matériels par l'intermédiaire de pilotes :
 + supports de stockage (disques durs IDE/SCSI, USB, etc.)
 + interfaces réseau
 + carte son
 + etc.

Les pilotes standards font partie intégrante du noyau (par exemple le support de l'IDE), ou peuvent être chargés.

Le charge de démarrage place également une image `initramfs` en mémoire. L'image en question est un système de fichiers virtuels, c'est-à-dire qu'elle contient des répertoires et fichiers qui sont accédés par le noyau comme s'ils étaient sur un disque dur. Le noyau accède à l'image `initramfs` (_Initial RAM Filesystem_), système de fichiers RAM initial) par le chemin `/sysroot/`.

L'image `initramfs`contient des pilotes et des modules qui sont nécessaires au noyau pour le démarrage du système. Comme ces pilotes et modules ne sont pas standard, on préfère ne pas les inclure de manière permanente et statique au noyau, qui peut ainsi les charger au besoin et garder une taille réduite. De fait, la taille d'un noyau ne dépasse pas quelques mégaoctects.

Une fois chargé, le noyau effectue les tâches d'initialisation du matériel pour préparer la suite du démarrage :

+ Initialise la mémoire, les composants matériels, le processeur et les périphériques de stockage.
+ Décompresse dans `/sysroot/` l'image `initramfs` chargée en mémoire par le chargeur de démarrage.
+ Charge à partir de `/sysroot/` les pilotes et modules nécessaires.
+ Monte la partition racine en lecture seule dans `/` pour pouvoir accéder au reste des fichiers nécessaires au démarrage.
+ Lance le processus `/sbin/init` sur la partition racine `/` qui prend le relais pour le démarrage du système.

