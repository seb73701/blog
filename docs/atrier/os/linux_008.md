---
title: Gérez votre stockage avec Stratis
description: 
published: true
date: 2023-04-21T11:16:26.472Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:22.025Z
---

# Gérez votre stockage avec Stratis

> Quand on parle de gestion du stockage local à un administrateur système Linux, il est généralement question de LVM et de filesystem XFS ou ext4. À un niveau plus évolué, on trouvera ZFS qui n'est pas libre ou bien Btrfs qui tarde à tenir ses promesses. Stratis a pour vocation de proposer quelque chose de neuf avec l’existant !

Lors de la sortie de la RHEL8, Red Hat a annoncé Stratis qui permet une gestion moderne et avancée du stockage proche de ce qu'offre ZFS. Cependant à la différence des filesystems traditionnels opérant avec un module kernel et une CLI, Stratis se base sur les briques existantes, device-mapper et XFS. Ces briques étant matures et éprouvées depuis des années et Stratis opérant en espace utilisateur, cela a permis de nettement accélérer le développement.

Par gestion moderne du stockage, concrètement j'entends par là :

- une gestion basée sur des pools ;
- les snapshots de filesystem ;
- le thin provisionning ;
- la gestion de cache FS ;
- l'utilisation d'une CLI homogène pour l'ensemble des opérations.


## 1. Installation
Stratis ayant été développé pour RHEL8 et placé sous licence libre MPL 2.0, il est naturellement disponible pour l'ensemble des dérivés de celle-ci, CentOS, Oracle Linux et pour ma part, Rocky Linux ainsi que pour Fedora.

Pour l'installation, c'est très simple, s'agissant d'un démon userland, une simple installation de paquet et l'activation d'un service systemd suffisent.

```
[root@rocky ~]# dnf install stratisd stratis-cli
[root@rocky ~]# systemctl start stratisd.service
[root@rocky ~]# systemctl status stratisd.service
```

C’est tout, il n’y a pas de module kernel ou de fichier de configuration à modifier. Toutes les interactions se font au travers de la CLI.

## 2. Création d'un pool
Les disques gérés par Stratis doivent être tout d'abord ajoutés à un pool. Ces disques ne doivent être rattachés à aucune configuration de stockage et n'avoir aucune table de partition.

Dans cet exemple, j'ai ajouté quatre disques vierges, vérifions avec lsblk :

```
[root@rocky ~]# lsblk -p
NAME                    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
/dev/sda                  8:0    0   32G 0 disk
├─/dev/sda1               8:1    0    1G 0 part /boot
└─/dev/sda2               8:2    0   31G 0 part
  ├─/dev/mapper/rl-root 253:0    0   29G 0 lvm  /
  └─/dev/mapper/rl-swap 253:1    0  2.1G 0 lvm [SWAP]
/dev/sdb                  8:16   0   32G 0 disk
/dev/sdc                  8:32   0   32G 0 disk
/dev/sdd                  8:48   0   32G 0 disk
/dev/sde                  8:64   0   32G 0 disk
/dev/sr0                 11:0    1  1.9G 0 rom
```

Dans le cas où des données sont présentes sur les disques, il est possible de faire le nettoyage avec la commande wipefs et bien entendu après avoir préalablement vérifié que les données présentes ne sont plus nécessaires.

Créons donc un pool avec deux disques. Il est tout à fait possible de créer un pool à un seul disque. La redondance n’est d’ailleurs pas encore gérée par Stratis.

```
[root@rocky ~]# stratis pool create stratispool /dev/sdb /dev/sdc
```

Vérifions que le pool a bien été créé :

```
[root@rocky ~]# stratis pool list
Name                          Total Physical   Properties
stratispool   64 GiB / 46.45 MiB / 63.95 GiB      ~Ca,~Cr
```

Les properties affichées signifient que le pool n'a pas de cache (~Ca) et n'est pas chiffré (~Cr).

Vérifions ensuite nos disques. En principe, ils sont désormais gérés par Stratis et sont identifiés comme tel :

```
[root@rocky ~]# stratis blockdev list
Pool Name     Device Node   Physical Size   Tier
stratispool   /dev/sdb             32 GiB   Data
stratispool   /dev/sdc             32 GiB   Data
```

Effectivement, les deux disques ajoutés au pool sont désormais rattachés au pool stratispool. Aucun des autres disques qui n'ont pas été ajoutés à un pool ne sont listés. Ils ne sont en effet pas gérés par Stratis. Les commandes classiques s’appliquent donc pour ces derniers.

Naturellement, l'ensemble des périphériques bloc d'un même pool doivent être de même type afin d'avoir une consistance sur les I/O. Un même pool ne doit pas être constitué de périphériques bloc SSD et SATA 7200 par exemple.

## 3. Création d'un système de fichiers
Nous avons créé précédemment un pool de stockage, nous pouvons désormais créer un filesystem s'appuyant sur ce pool. Le choix du type de FS est imposé, il s'agit forcément de XFS.

```
[root@rocky ~]# stratis fs create stratispool data
```

Vérifions :

```
[root@rocky ~]# stratis fs list
Pool Name     Name   Used      Created             Device                          UUID
stratispool   data   546 MiB   Sep 12 2021 05:21   /dev/stratis/stratispool/data   9f1f993f780c40f795ed28fca79c3b18
```

Vous avez dû immédiatement remarquer quelque chose. En effet, à aucun moment nous n'avons précisé de taille sur notre système de fichiers. Avec Stratis, la règle est l'utilisation du thin provisionning. C’est-à-dire que les blocs ne sont pas réservés sur les périphériques sous-jacents, mais alloués à la volée. Lorsque le FS est proche d'une saturation, une extension de celui-ci est alors réalisée.

C'est pour cette raison qu'il est indispensable de ne jamais manipuler un filesystem en dehors de Stratis. D'ailleurs lsblk affiche les différentes couches utilisées en interne :

```
[root@rocky ~]# lsblk /dev/sdb /dev/sdc
NAME                                                                                        MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sdb                                                                                           8:16   0   32G 0 disk
└─stratis-1-private-c5537a12176a482e8df8af23ebdce463-physical-originsub                     253:2    0   64G 0 stratis
  ├─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-thinmeta                        253:3    0   64M 0 stratis
  │ └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-thinpool-pool                      253:6    0 63.9G 0 stratis
  │   └─stratis-1-c5537a12176a482e8df8af23ebdce463-thin-fs-9f1f993f780c40f795ed28fca79c3b18 253:7    0    1T 0 stratis
  ├─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-thindata                        253:4    0 63.9G 0 stratis
  │ └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-thinpool-pool                      253:6    0 63.9G 0 stratis
  │   └─stratis-1-c5537a12176a482e8df8af23ebdce463-thin-fs-9f1f993f780c40f795ed28fca79c3b18 253:7    0    1T 0 stratis
  └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-mdv                             253:5    0   16M 0 stratis
sdc                                                                                           8:32   0   32G 0 disk
└─stratis-1-private-c5537a12176a482e8df8af23ebdce463-physical-originsub                     253:2    0   64G 0 stratis
  ├─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-thinmeta                        253:3    0   64M 0 stratis
  │ └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-thinpool-pool                      253:6    0 63.9G 0 stratis
  │   └─stratis-1-c5537a12176a482e8df8af23ebdce463-thin-fs-9f1f993f780c40f795ed28fca79c3b18 253:7    0    1T 0 stratis
  ├─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-thindata                        253:4    0 63.9G 0 stratis
  │ └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-thinpool-pool                      253:6    0 63.9G 0 stratis
  │   └─stratis-1-c5537a12176a482e8df8af23ebdce463-thin-fs-9f1f993f780c40f795ed28fca79c3b18 253:7    0    1T 0 stratis
  └─stratis-1-private-c5537a12176a482e8df8af23ebdce463-flex-mdv                             253:5    0   16M 0 stratis
```

## 4. Montage d'un système de fichiers
Le montage d'un système de fichiers dans l'arborescence est somme toute classique et se déroule exactement de la même façon qu'avec n'importe quel autre système de fichiers.

```
[root@rocky ~]# mkdir /mnt/data
[root@rocky ~]# mount /dev/stratis/stratispool/data /mnt/data
[root@rocky ~]# df -h /mnt/data
Filesystem                                                                                       Size Used Avail Use% M
/dev/mapper/stratis-1-c5537a12176a482e8df8af23ebdce463-thin-fs-9f1f993f780c40f795ed28fca79c3b18 1.0T 7.2G 1017G   1% /
```

Pour rendre permanent le montage, je recommande néanmoins de récupérer l'UUID du périphérique et d'utiliser celui-ci pour configurer le fichier /etc/fstab.

Pour le récupérer, encore une fois la commande lsblk est là pour nous aider :

```
root@rocky ~]# lsblk -o uuid /dev/stratis/stratispool/data
UUID
9f1f993f-780c-40f7-95ed-28fca79c3b18
[root@rocky ~]# echo "UUID=9f1f993f-780c-40f7-95ed-28fca79c3b18 /mnt/data xfs defaults 0 0" >> /etc/fstab
```

## 5. Étendre le pool
Avec la croissance des données, il arrivera forcément un moment où l'extension du pool sera nécessaire.

Pour cela, il suffit d'ajouter un disque au pool :

```
[root@rocky ~]# stratis pool add-data stratispool /dev/sdd
```

Et contrôler que le pool a bien été étendu de la volumétrie souhaitée :

```
[root@rocky ~]# stratis pool list
Name                           Total Physical   Properties
stratispool   96 GiB / 599.68 MiB / 95.41 GiB      ~Ca,~Cr
```

Le pool est étendu, il n’y a toujours pas d’action à prévoir côté système de fichiers. Stratis se chargera de redimensionner le FS en fonction de l’espace disponible sur le pool. Note : il n’est pas possible de définir une taille maximale pour un système de fichiers au sein d’un pool. Dans ce cas, il est nécessaire de créer un pool dédié.

## 6. Snapshots
Un snapshot est une copie extraite à un instant donné d'un système de fichiers. Ceux-ci peuvent être pris à chaud, sans démontage du filesystem sur lequel un instantané doit être pris.

Un snapshot Stratis peut être ensuite monté en lecture/écriture et a ainsi une existence totalement indépendante du FS d'origine. Un snapshot Stratis ne fonctionne donc pas tout à fait comme un snapshot de machine virtuelle.

Pour créer un snapshot de notre précédent FS, il suffit de :

```
[root@rocky ~]# stratis fs snapshot stratispool data data_snap
```

Comme je le disais, un snapshot étant un FS indépendant du FS original, pour lister les snapshots, il faut en pratique prendre la liste des FS :

```
[root@rocky ~]# stratis fs list
Pool Name     Name        Used      Created             Device                               UUID
stratispool   data_snap   546 MiB   Sep 12 2021 09:33   /dev/stratis/stratispool/data_snap   eb3ca4299bb34d8993260d7af087c21e
stratispool   data        546 MiB   Sep 12 2021 09:32   /dev/stratis/stratispool/data        5a9c2077ec6a4793a361587938045154
```

Le snapshot peut ensuite être monté comme n'importe quel filesystem :

```
[root@rocky ~]# mkdir /mnt/data_snap
[root@rocky ~]# mount /dev/stratis/stratispool/data_snap /mnt/data_snap/
```

En ce qui concerne le retour arrière sur snapshot, il n'existe pas de procédure automatique. Vous l'aurez deviné, il faut donc arrêter les processus qui écrivent sur le système de fichiers, le démonter et monter le snapshot à la place.

Pour faire propre, je vous recommande de renommer le snapshot afin de ne pas garder un nom incohérent avec l'environnement de production puis de supprimer le système de fichiers inutile.

## 7. Cache
Stratis a la capacité de proposer un cache disque. Cela n’est utile en pratique que dans le cas où plusieurs technologies disques sont présentes. Par exemple, votre stockage nominal est constitué de disques SAS et vous disposez de disques flash NVME. Stratis permet de configurer un cache sur le stockage flash afin d'augmenter sensiblement les performances du système de fichiers.

Pour ajouter un disque comme cache à un volume Stratis, on passe par la commande init-cache :

```
[root@rocky ~]# stratis pool init-cache stratispool /dev/sde
```

L'ajout du cache au pool modifie ainsi les propriétés du pool. La propriété Ca perd ainsi le caractère ~ qui constitue une négation au niveau de l'affichage :

```
[root@rocky ~]# stratis pool list
Name                           Total Physical   Properties
stratispool   96 GiB / 599.68 MiB / 95.41 GiB       Ca,~Cr
```

En affichant la liste des périphériques bloc gérés par Stratis, on trouve désormais nos trois disques initiaux flaggés comme disques de données, et le disque que l'on vient d'ajouter comme disque de cache :

```
[root@rocky ~]# stratis blockdev list
Pool Name     Device Node   Physical Size    Tier
stratispool   /dev/sdb             32 GiB    Data
stratispool   /dev/sdc             32 GiB    Data
stratispool   /dev/sdd             32 GiB    Data
stratispool   /dev/sde             32 GiB   Cache
```


## 8. Nettoyage
Notre environnement de test étant terminé, procédons au nettoyage. Premièrement, on démonte les deux FS. Il faudra également si nécessaire retirer les entrées associées dans le fichier /etc/fstab :

```
[root@rocky ~]# umount /mnt/data_snap
[root@rocky ~]# umount /mnt/data
```

On peut ensuite supprimer les systèmes de fichiers :

```
[root@rocky ~]# stratis fs destroy stratispool data_snap
[root@rocky ~]# stratis fs destroy stratispool data
```

Et enfin, on peut supprimer le pool :

```
[root@rocky ~]# stratis pool destroy stratispool
```

On peut vérifier la suppression du pool en contrôlant que les disques ne sont plus attachés à aucun pool :

```
[root@rocky ~]# stratis blockdev list
Pool Name   Device Node   Physical Size   Tier
```

## 9. Chiffrement
Stratis supporte le chiffrement de pool à la création. Ce qui signifie donc qu’il n’est pas possible de chiffrer un volume a posteriori. S’agissant d’un chiffrement au niveau du pool, tous les FS basés sur ce pool se retrouvent donc automatiquement chiffrés.

En premier lieu, il faut générer une clé de chiffrement. Voici comment en générer une depuis l’entrée standard et vérifier qu’elle soit bien connue de Stratis :

```
[root@rocky ~]# stratis key set --capture-key poolkey
Enter key data followed by the return key:
[root@rocky ~]# stratis key list
Key Description
poolkey
```

Il est désormais possible de créer le pool chiffré avec cette clé et un FS sur ce pool :

```
[root@rocky ~]# stratis pool create --key-desc poolkey poolcrypt /dev/sdd
[root@rocky ~]# stratis pool list
Name                           Total Physical   Properties
poolcrypt   31.98 GiB / 39.24 MiB / 31.95 GiB      ~Ca, Cr
[root@rocky ~]# stratis filesystem create poolcrypt data
[root@rocky ~]# stratis fs list
Pool Name   Name   Used      Created             Device                        UUID
poolcrypt   data   546 MiB   Sep 12 2021 12:54   /dev/stratis/poolcrypt/data   60eae29bf1fd48b6ad25fd2eeba7ce2f
```

La gestion du chiffrement des pools a cependant un gros défaut, elle se fait en mémoire. En effet, après un redémarrage du système d’exploitation, la clé n’est pas connue de manière persistante par Stratis et doit à nouveau lui être fournie. Par conséquent, les volumes ne seront pas réactivés au démarrage. Pour ceci, une des solutions est de gérer le cas avec systemd après avoir placé la clé dans un fichier protégé par les bons droits d’accès :

```
[root@rocky ~]# echo "secret" > /root/poolkey
[root@rocky ~]# chmod 0400 /root/poolkey && chown root:root /root/poolkey
```

Il reste à définir un unit systemd qui chargera la clé dans Stratis, qui activera le pool et qui montera le filesystem :

```ini
[Unit]
Description = Montage de /data
After = stratisd.service
 
[Service]
ExecStartPre=stratis key set --keyfile-path /root/poolkey poolkey && stratis pool unlock keyring
ExecStart=mount /dev/stratis/poolcrypt/data /data
RemainAfterExit=yes
 
[Install]
WantedBy = multi-user.target
Enfin, il ne reste plus qu’à activer notre nouvel unit :

[root@rocky ~]# systemctl enable stratis-data
Created symlink /etc/systemd/system/multi-user.target.wants/stratis-data.service → /etc/systemd/system/stratis-data.service.
```


## Conclusion
Stratis est assez jeune et est encore en Technological Preview chez Red Hat. Néanmoins, sa conception simple et son interface en ligne de commandes simple et cohérente sont appréciables.

Bien entendu, l'ensemble des opérations d'exploitation du stockage dont un sysadmin a besoin au quotidien sont pour l’instant très loin de toutes être gérées, mais l'outil est très prometteur et répond à un vrai besoin de simplification.

## Références
[1] Site officiel de Stratis : https://stratis-storage.github.io

[2] La conception de Stratis expliquée : https://stratis-storage.github.io/StratisSoftwareDesign.pdf


source : https://connect.ed-diamond.com/linux-pratique/lp-128/gerez-votre-stockage-avec-stratis