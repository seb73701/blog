---
title: Réaliser une sauvegarde et la restaurer avec Btrfs
description: 
published: true
date: 2023-04-21T11:15:58.581Z
tags: snapshot, sauvegarde, btrfs
editor: markdown
dateCreated: 2023-04-21T11:15:53.875Z
---

# Réaliser une sauvegarde et la restaurer avec Btrfs

> Disponible depuis maintenant presque 10 ans, Btrfs est un système de fichiers mature qui est poussé par SUSE, il est devenu le système de fichiers par défaut lors de son installation. Il offre des possibilités intéressantes par rapport aux systèmes de fichiers historiques. Démonstration avec une sauvegarde/restauration à l’aide de la fonctionnalité d’instantané (snapshot).
{.is-info}

Le but de cet article n’est pas de présenter en détail le fonctionnement de Btrfs, mais de mettre en pratique l’une de ses fonctionnalités pour réaliser une sauvegarde de ses données et pour les restaurer si nécessaire.

Le tutoriel propose trois approches, une partie pour les sauvegardes locales, sur un disque physique différent, une partie sur les sauvegardes à distance, et une dernière partie qui permet de réaliser les sauvegardes sur d’autres systèmes de fichiers, déjà existants.

> **Pré-requis**
> 
> Les prérequis ne sont pas nombreux.
> 
> Dans les deux premières parties, il est simplement nécessaire d’avoir une partition Btrfs à disposition, ou à défaut, avoir de l’espace pour en créer une.
> 
> Une fois la partition créée, il faut d’abord la formater à l’aide de la commande suivante :
> 
> 
> `$ sudo mkfs.btrfs /dev/vdb1`
> 
> **Attention** : Dans mon cas la partition est /dev/sdb1, il est probable voire certain que ce n’est pas le cas sur votre machine. Vérifiez plusieurs fois que vous formatez la bonne partition avant de valider la commande.
{.is-info}


## 1. Rappel sur les sous-volumes
Les sous-volumes sont une spécificité de Btrfs. Ce n’est en fait qu’une partie du système de fichiers ayant sa propre arborescence de fichiers. Le sous-volume peut-être accédé comme un dossier standard, ou en état monté à l’aide de l’option subvol de la commande mount. Il peut également être déplacé ou renommé comme un dossier et peut contenir un autre sous-volume dans sa sous-arborescence. Pour chaque partition Btrfs, il y a un sous-volume par défaut auquel l’id 5 est assigné et qui se nomme /. Celui-ci ne peut pas être modifié. C’est ce sous-volume qui est monté lorsqu’on ne précise rien.

Dans notre cas, nous allons donc commencer par monter notre nouvelle partition dans /mnt :

```
$ sudo mkdir /mnt/btrfs
$ sudo mount /dev/vdb1 /mnt/btrfs
```

On peut constater qu’effectivement le sous-volume avec l’id 5 est celui monté.

```
$ sudo mount | grep vdb
/dev/vdb1 on /mnt/btrfs type btrfs (rw,relatime,seclabel,space_cache,subvolid=5,subvol=/)
```

Ensuite, nous allons créer un sous-volume et un fichier de test :

```
$ sudo touch /mnt/btrfs/test
$ sudo btrfs subvolume create /mnt/btrfs/sv1
Create subvolume '/mnt/btrfs/sv1'
$ sudo touch /mnt/btrfs/sv1/test_sv1
```

Nous pouvons désormais constater que la partition contient notre fichier de test et ce qui apparaît comme un dossier (en réalité, notre sous-volume).

```
$ ls -lR /mnt/btrfs
/mnt/btrfs:
total 0
drwxr-xr-x. 1 root root 30 Mar 4 21:33 sv1
-rw-r--r--. 1 root root 0 Mar 4 21:23 test

/mnt/btrfs/sv1:
total 0

-rw-r--r--. 1 root root 0 Mar 4 21:33 test_sv1
```

Démontons le sous-volume par défaut et remontons sv1 pour constater que nous voyons uniquement le contenu de celui-ci et non plus le contenu de toute la partition (l’option subvol est utilisée pour cela).

```
$ sudo umount /mnt/btrfs && sudo mount -o subvol=sv1 /dev/vdb1 /mnt/btrfs
$ ls -lR /mnt/btrfs
/mnt/btrfs:
total 0

-rw-r--r--. 1 root root 0 Mar 4 21:33 test_sv1
```

## 2. Rappel sur les instantanés
Un instantané ou « snapshot » est un type de sous-volume Btrfs dont le contenu est initialisé à partir d’un sous-volume existant. Il est possible d’en créer en lecture-écriture ou en lecture seule (ce que nous utiliserons plus tard pour les sauvegardes).

Créons un nouveau sous-volume, nommons-le documents.

```
$ sudo btrfs subvolume create /mnt/btrfs/documents
Create subvolume '/mnt/btrfs/documents'

$ sudo chown ptoret: /mnt/btrfs/documents
```

J’ajoute deux fichiers et je crée un instantané à partir de documents qui s’appellera documents-snap.

```
$ ls -l /mnt/btrfs/documents
total 3170304
-rw-r--r-- 1 ptoret users 3122659328 Mar 16 18:12 Fedora-Server-dvd-x86_64-29-1.2.iso
-rw-r--r-- 1 ptoret users 123731968 Mar 16 18:12 openSUSE-Leap-15.0-NET-x86_64.iso

$ sudo btrfs subvolume snapshot /mnt/btrfs/documents /mnt/btrfs/documents-snap
Create a snapshot of '/mnt/btrfs/documents' in '/mnt/btrfs/documents-snap'
```

Supprimons un des fichiers et ajoutons-en un nouveau dans le sous-volume d’origine et listons le contenu des deux sous-volumes pour comparer.

```
$ ls -lR /mnt/btrfs/documents*
/mnt/btrfs/documents:
total 3049476
-rw-r--r-- 1 ptoret users 3122659328 Mar 16 18:12 Fedora-Server-dvd-x86_64-29-1.2.iso
-rw-r--r-- 1 ptoret users 59 Mar 16 18:19 nouveau_document.txt
/mnt/btrfs/documents-snap:
total 3170304
-rw-r--r-- 1 ptoret users 3122659328 Mar 16 18:12 Fedora-Server-dvd-x86_64-29-1.2.iso
-rw-r--r-- 1 ptoret users 123731968 Mar 16 18:12 openSUSE-Leap-15.0-NET-x86_64.iso
```

L’instantané contient bien le contenu d’origine alors que le répertoire contient lui le résultat des modifications effectuées (nouveau fichier et fichier supprimé).

> **Attention !**
> Un instantané ne fait en aucun cas office de sauvegarde.
{.is-warning}


## 3. La sauvegarde locale
Une fois ces rappels faits, nous pouvons aborder la partie sauvegarde proprement dite. Nous allons utiliser ces deux notions, dans le cadre d’une sauvegarde locale, c’est-à-dire sur un support physiquement différent, mais présent sur la machine, dans notre cas un disque dur. Il est sous-entendu ici que le support physique a été partitionné en Btrfs.

> **Attention !**
> Une notion importante est l’imbrication des sous-volumes : lorsqu’on réalise un instantané sur un sous-volume, le contenu de l’instantané ne prend pas en compte les sous-volumes imbriqués dans le sous-volume d’origine. Attention donc si votre sous-volume en contient d’autres, ils ne seront pas pris en compte dans l’instantané réalisé, vous trouverez uniquement un dossier vide ayant le nom du sous-volume.
{.is-warning}


Tout d’abord, nous devons créer un instantané de type lecture seule.

```
$ sudo btrfs subvolume snapshot -r /mnt/btrfs/documents /mnt/btrfsdocuments-snap-ro
Create a readonly snapshot of '/mnt/btrfs/documents' in '/mnt/btrfs/documents-snap-ro'
```

Nous allons ensuite « envoyer » ce snapshot sur la partition Btrfs de la clef USB (dans notre cas /dev/vdc1). La partition où l’on va sauvegarder (la clef USB) est montée dans /mnt/usb.

```
$ sudo mount /dev/vdc1 /mnt/usb
$ sudo btrfs send /mnt/btrfs/documents-snap-ro | sudo btrfs receive /mnt/usb/
$ ls -l /mnt/usb/documents-snap-ro/
total 3049476
-rw-r--r-- 1 ptoret users 3122659328 Mar 16 18:12 Fedora-Server-dvd-x86_64-29-1.2.iso
-rw-r--r-- 1 ptoret users 59 Mar 16 18:19 nouveau_document.txt
```

La commande btrfs send permet d’envoyer un sous-volume (en lecture seule) et la commande btrfs receive permet de l’écrire. Comme on le voit, il est très facile de copier un sous-volume (et donc tout son contenu à l’identique) d’un support physique à un autre. Le fait d’utiliser un pipe entre les deux commandes permet une très grande souplesse dans l’utilisation. On le verra lors de la sauvegarde à distance et sur un système de fichiers différent.

Il arrive que l’on dispose d’un disque déjà partitionné avec un autre système de fichiers que Btrfs, dans ce cas-là, l’option -f de l’utilitaire btrfs send est prévue pour spécifier un fichier de sortie au lieu d’envoyer sur la sortie standard. Le seul problème est que dans ce cas il n’est pas possible de parcourir le contenu de l’instantané.

```
$ sudo btrfs send -f /mnt/autrefs/documents-snap-ro.btrfs documents-snap-ro
$ ls -l /mnt/autrefs/documents-snap-ro.btrfs
total 3053528
-rw------- 1 root root 3126790515 Mar 16 22:01 documents-snap-ro.btrfs
drwx------ 2 root root 16384 Mar 16 21:59 lost+found
```

## 4. La sauvegarde incrémentale
Lorsqu’on sauvegarde de grosses quantités de données et qu’une sauvegarde a déjà été réalisée, il peut être intéressant d’utiliser la fonctionnalité d’envoi incrémental proposée par btrfs send grâce à l’option -P. Dans mon cas, je rajoute une nouvelle ISO et je décide de sauvegarder à nouveau. En ayant gardé l’instantané précédent, je peux réaliser l’opération suivante, transférer le nouvel instantané (documents-snap-ro2) en référençant documents-snap-ro comme son parent, ainsi ce sera uniquement un différentiel entre ces deux versions qui sera envoyé.

```
$ sudo btrfs subvolume snapshot -r /mnt/btrfs/documents /mnt/btrfs/documents-snap-ro2
$ sudo btrfs send -p /mnt/btrfs/documents-snap-ro /mnt/btrfs/documents-snap-ro2 | sudo btrfs receive /mnt/usb/
```

On pourra ensuite supprimer documents-snap-ro dans /mnt/btrfs (côté disque de sauvegarde on peut le supprimer ou non, selon ce qu’on veut garder), car lors du prochain transfert on créera par exemple documents-snap3 et on prendra documents-snap-ro2 comme référence à passer via l’option -p.

```
$ sudo btrfs subvolume delete -c /mnt/btrfs/documents/snap-ro
$ sudo btrfs subvolume snapshot -r /mnt/btrfs/documents /mnt/btrfs/documents-snap-ro3
$ sudo btrfs send -p /mnt/btrfs/documents-snap-ro2 /mnt/btrfs/documents-snap-ro2 | sudo btrfs receive /mnt/usb/
```

Dans le cas d’une sauvegarde sur un autre système de fichiers on peut également utiliser cette fonctionnalité. Mais je ne rentrerai pas plus dans les détails.

```
$ ls -lh /mnt/vieuxfs/
total 3.1G
-rw------- 1 root root 119M Mar 16 22:30 documents-snap-ro2.btrfs
-rw------- 1 root root 3.0G Mar 16 22:01 documents-snap-ro.btrfs
drwx------ 2 root root 16K Mar 16 21:59 lost+found
```

Comme on peut le voir, il n’est pas si aisé de s’y retrouver entre les instantanés qu’on crée côté système de fichiers à sauvegarder et ceux qui sont sur le support de sauvegarde. Il est intéressant d’utiliser des outils pour automatiser tout cela.

## 5. La sauvegarde à distance
La sauvegarde à distance ne présente pas plus de difficulté, il suffira d’utiliser ssh pour réaliser l’envoi-réception à l’aide des outils Btrfs. Ainsi, si l’on désire envoyer le dernier instantané, on peut utiliser la commande suivante (l’option -p pourra également être utilisée, moyennant l’envoi d’un premier instantané qui servira ensuite de référence). Il arrivera dans /mnt/usb sur la machine distante (sous réserve que /mt/usb abrite bien une partition Btrfs).

```
$ sudo btrfs send /mnt/btrfs/documents-snap-ro | ssh ptoret@192.168.122.102 sudo btrfs receive /mnt/usb/
At subvol /mnt/btrfs/documents-snap-ro
```

## 6. Chiffrer et/ou compresser sa sauvegarde
Le fait d’utiliser un « pipe » UNIX (connu aussi sous le nom de tube) permet de nombreuses choses, comme compresser les sauvegardes sur le support externe par exemple. Un autre avantage est de pouvoir chiffrer celles-ci, surtout dans le cas où on les entreposerait sur un serveur en ligne par exemple. Pour réaliser ceci, il faut une clef publique et utiliser celle-ci de la façon suivante (on chiffre pour Alice qui est le destinataire dans l’exemple, dans les faits, on sera probablement le possesseur de la clef privée aussi, il faut bien pouvoir déchiffrer sa propre sauvegarde).

```
$ sudo btrfs send /mnt/btrfs/documents-snap-ro | gpg -e -r alice@tld > documents-snap-ro.gpg
```

On peut également chiffrer après avoir compressé de la façon suivante :

```
$ sudo btrfs send /mnt/btrfs/documents-snap-ro | pigz - | gpg -e -r alice@tld > documents-snap-ro.gz.gpg
```

Ces deux exemples sont à adapter à vos cas d’usage, mais donne un exemple de ce qu’il est possible de faire, le plus simple est de parcourir les pages de manuels des différents outils pour se donner une idée des possibilités. À noter que selon la taille du sous-volume à compresser, cela peut prendre un temps considérable.

## 7. Restaurer sa sauvegarde
Réaliser des sauvegardes est une bonne chose, savoir accéder à ses données sauvegardées en cas de problème est encore mieux.

On peut imaginer plusieurs cas de figure auxquels je vais essayer de proposer une solution.

### Cas 1 : « j’ai supprimé un fichier et je veux le restaurer depuis la sauvegarde »
Dans le cas où la sauvegarde est sur un système de fichiers Btrfs, elle peut se parcourir comme un dossier, ce qui rend très facile la récupération d’un fichier supprimé.

Ainsi, si je reprends le tout premier exemple, après avoir créé l’instantané documents-snap, j’ai supprimé le fichier openSUSE-Leap-15.0-NET-x86_64.iso. Pour le récupérer, je pourrais simplement le copier à son emplacement initial depuis l’instantané.

```
$ cp /mnt/btrfs/documents-snap-ro/openSUSE-Leap-15.0-NET-x86_64.iso /mnt/btrfs/documents/openSUSE-Leap-15.0-NET-x86_64.iso
```

Dans le cas d’une sauvegarde distante, cela donnerait :

```
$ scp ptoret@192.168.122.102:/mnt/usb/documents-snap-ro/openSUSE-Leap-15.0-NET-x86_64.iso /mnt/btrfs/documents/openSUSE-Leap-15.0-NET-x86_64.iso
```

> **Attention !**
> Il n’est pas possible de restaurer un fichier de façon si simple lorsque la sauvegarde est réalisée sur un système de fichiers autre que Btrfs. Il faudrait alors restaurer tout le sous-volume comme dans le cas 2 et une fois cela fait, revenir au cas 1 pour récupérer le fichier manquant.
{.is-warning}


### Cas 2 : « je veux restaurer tout mon sous-volume »
Ici, on peut imaginer le cas d’un disque qui est tombé en panne ou autre ne laissant plus aucune trace du sous-volume d’origine.

Dans ce cas-là, il faut 2 étapes.

Récupérer le sous-volume sur le système de fichiers local.

Dans le cas où la sauvegarde est à distance :

```
$ ssh ptoret@192.168.122.102 sudo btrfs send /mnt/usb/documents-snap-ro | sudo btrfs receive /mnt/btrfs/
At subvol /mnt/usb/documents-snap-ro
```

Dans le cas où la sauvegarde est locale :

```
$ sudo btrfs send /mnt/usb/documents-snap-ro | sudo btrfs receive /mnt/btrfs

At subvol /mnt/btrfs/documents-snap-ro
```

Repasser le sous-volume en lecture/écriture.

```
$ sudo btrfs property set -tsubvol /mnt/btrfs/documents-snap-ro ro false
```

Enfin, il suffit de renommer le sous-volume :

```
$ sudo mv /mnt/btrfs/documents-snap-ro /mnt/btrfs/documents
```

> **Attention !**
> Suivre cet article devrait vous permettre d’avoir une bonne idée de ce qu’il est possible de faire, mais il est important de tester sa propre procédure de restauration au calme, juste pour être sûr de pouvoir le faire le jour où un problème survient (on se retrouve souvent un peu paniqué dans ces moments-là).
{.is-warning}


## Aller plus loin
La gestion des instantanés que ce soit en local ou à distance peut vite devenir complexe, il est possible d’écrire des scripts pour automatiser, à vrai dire un simple cron peut suffire, mais il existe également de nombreux outils qui peuvent s’occuper de cela pour vous.

Il est possible d’obtenir un fonctionnement type « time-machine » [1]. Une solution générique a d’ailleurs été proposée sous le nom d’autosnap [2], mais elle n’est pas encore disponible dans les distributions.

Actuellement, l’outil le plus connu est sans doute Snapper [3] qui est développé par SUSE (mais utilisable sur de nombreuses distributions), il mériterait un article à lui seul, car il est très complet et possède de nombreux plugins. Il est néanmoins nécessaire d’adopter un agencement particulier des instantanés pour que ce dernier fonctionne.

Pour les sauvegardes, un outil relativement répandu est btrbk [4]. Il permet de mettre en place assez facilement une sauvegarde dans le cadre d’un PC portable et d’un disque dur externe par exemple.

## Références
[1] https://btrfs.wiki.kernel.org/index.php/UseCases#How_can_I_use_btrfs_for_backups.2Ftime-machine.3F
[2] https://btrfs.wiki.kernel.org/index.php/Autosnap
[3] https://github.com/openSUSE/snapper
[4] https://github.com/digint/btrbk



source : https://connect.ed-diamond.com/Linux-Pratique/lp-114/realiser-une-sauvegarde-et-la-restaurer-avec-btrfs 