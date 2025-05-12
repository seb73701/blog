# Partitions et système de fichiers

### LVM (Logical Volume Manager)

**LVM** (_Logical Volume Manager_, gestion de volumes logiques) répond à un besoin de **souplesse dans le partitionnement**.

Le système classique de partitionnement a essayé d'évoluer avec les besoins. Les partitions logiques à l'intérieur d'une partition étendue permettaient d'étendre le nombre de partitions. Il est devenu possible de redimensionner une partition, mais cette opération est coûteuse en temps, parfois risquée, et ne peut pas se faire à chaud (quand le système de fichiers est utilisé).

LVM est, comme son nom l'indique, un type de gestions de volumes logiques, c'est-à-dire que l'on opère plus directement sur les partitions physiques du disque dur. On rajoute entre le disque dur et le système d'exploitation une couche intermédiaire, logicielle, bien plus souple à manipuler.

#### Fonctionnement

Le fonctionnement de LVM consiste à regrouper les disques durs et partitions physiques en un grand "fond commun", que l'on va redistribuer sous forme de nouvelles partitions, non plus physiques, mais logiques cette fois.

+ Les **disques durs et partitions physiques** sont appelés **volumes physiques**.
+ Ce **réservoir commun** dans lequel ils sont réunis est appelé _groupe de volumes_.
+ La **redivision** de ce fond commun en partitions logiques est appelée **volumes logiques**

#### Volumes physiques

En anglais, les _Physical Volumes_ (PV) peuvent être :

+ un disque entier (déconseillé)
+ des partitions d'un disque créées avec `parted`, primaires ou logiques
+ des partitions ou disques durs sur un volume RAID physique (cela est transparent pour LVM)
+ un volume iSCSI ou Fibre Channel qui passe pour une partition aux yeux du système.

Il est ainsi possible de répartir les données sur plusieurs partitions, disques durs, voire même à travers le réseau.

#### Groupes de volumes

En anglais, les _Volume Groups_ (VG) sont la concaténation de volumes physiques (PV).

#### Volumes logiques

Les groupes de volumes sont découpés en volumes logiques (en anglais, _Logical Volumes_, LV).

Les **LV sont vus comme des partitions par le système d'exploitation**. Ils peuvent avoir n'importe quelle taille, et être formatés avec n'importe quel type de système de fichiers.

> **Remarque**
> LVM est indépendant du type de système de fichiers que l'on y met.

![LVM](../../../img/os/centos_rhel/rhel_lvm_arch.png)

### Notions avancées

#### Extensions

##### Extensions physiques ou PE

Les extentions **physiques** (en anglais, _Physical Extents_, PE) sont les plus petites unités de données des volumes physiques.

Ils ont la même taille que les extensions logiques (PE) des volumes logiques (LV) appartenant au même groupe de volumes (VG).

##### Extensions logiques ou LE

Les extensions **logiques** (_Logical Extents_, LE) sont les "morceaux" composant les volumes logiques.

Ce sont en fait des extensions physiques attribuées à des volume logique (LV). Ils ont donc la **même taille que les extensions physiques** des volumes physiques (PV) dont ils sont pris.

##### Allocations des extentions

Lorsque l'on crée un groupe de volumes (VG), on définit la taille des Physical Extents (PE). En général, elle est fixé à 4 Mo.

Lorsque des volumes physiques (PV) sont ajoutés au groupe de volumes (VG), ils sont **découpés en _physical extents_ (PE)**. On obtient un certain nombre de PE pour un volume physique (PV), qui sont ajoutées au réservoir commun d'extensions physiques (PE), c'est-à-dire au groupe de volumes (VG).

Lorsque l'on **crée un volume logique** (LV) dans un groupe de volumes (VG), un **certain nombre de ces PE sont allouées au volume logique** (LV). A ce moment-là, on ne parle plus d'extensions physiques (PE), mais d'extensions logiques (LE).

Exemple :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# vgdisplay -v
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  --- Volume group ---
  VG Name               cl
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <29,00 GiB
  PE Size               4,00 MiB
  Total PE              7423
  Alloc PE / Size       7423 / <29,00 GiB
  Free  PE / Size       0 / 0
  VG UUID               OCOkos-ZN2C-gqsw-kEZB-uaKt-FZlu-O6pW07

  --- Logical volume ---
  LV Path                /dev/cl/swap
  LV Name                swap
  VG Name                cl
  LV UUID                41eZ3J-uCo3-gle2-jpVJ-bK2u-IDss-o7Fky1
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:24 +0100
  LV Status              available
  # open                 2
  LV Size                3,00 GiB
  Current LE             768
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/cl/root
  LV Name                root
  VG Name                cl
  LV UUID                F04yXI-cnuZ-NGZ9-0Pdg-AQoV-SWOG-1jSyH7
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:25 +0100
  LV Status              available
  # open                 1
  LV Size                <26,00 GiB
  Current LE             6655
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0

  --- Physical volumes ---
  PV Name               /dev/sda2
  PV UUID               qMv5rT-hZjS-oUrv-DuVh-BGaC-DCBH-pNvYHa
  PV Status             allocatable
  Total PE / Free PE    7423 / 0

```

Dans cet exemple, nous avons :
+ Un groupe de volumes appelé `cl` (`VG Name`)
 + La taille des extensions physiques de ce groupe de volumes est 4 Mo (`PE Size`)
 + Ce groupe de volumes est composé d'un volume physique (PV) `/dev/sda2` (`PV Name`)
 + Le volume physique `/dev/sda2` possède au total 7423 extensions physiques (PE) (7423 unités de données de 4 Mo = 29692 Mo) dont 0 libre.
 + Le **groupe de volumes** possède donc **au total 7423 extensions physiques** (PE) qu'il peut allouer ensuite aux volumes logiques.
+ Un volume logique appelé `root` composé de **6655 extensions (logiques) parmi les 7423**. Sa taille est donc de 6655 x 4 Mo = 26620 Mo.
+ Un volume logique appelé `swap` composé de **768 extensions (logiques) parmi les 7423**. Sa taille est donc de 768 x 4 Mo = 3072 Mo.

##### Correspondance des extensions

Il y a donc une correspondance (_mapping_) entre le numéro d'extensions physiques et les numéros d'extensions logiques.

Par exemple, les extensions logiques du volume logique `LV-Oracle` sont numérotées de 1 à 125.

Cependant, ces 125 extensions logiques sont réparties sur deux volumes phusiques.

On peut supposer que :
+ Les extensions logiques 1 à 25 correspondent aux extensions physiques 1 à 25 de `/dev/sdb1`.
+ Les extensions logiques 26 à 125 correspondent aux extensions physiques 1 à 100 de `/dev/sdb2`.

Ce type de correspondance est appelé **correspondance linéaire (_linear mapping_)**. Les extensions logiques sont _réparties en série_ sur les volumes physiques.

Il est possible d'avoir une **correspondance par bande (_striped mapping_)**, où les extensions logiques sont _réparties en parallèle_ sur les volumes physiques.

Par exemple :
+ L'extension logique 1 correspond à l'extension physique 1 sur `/dev/sdb1`.
+ L'extension logique 2 correspond à l'extension physique 1 sur `/dev/sdb2`.
+ L'extension logique 3 correspond à l'extension physique 2 sur `/dev/sdb1`.
+ L'extension logique 4 correspond à l'extension physique 2 sur `/dev/sdb2`.
+ Et ainsi de suite jusqu'à l'extension logique 125.

> **Remarque**
> Ce type de correspondance par bande peut améliorer les performances à l'instar du RAID 0. Par exemple si les volumes physiques sont sur deux disques durs différents, un fichier peut être lu ou écrit en parallèle sur les deux disques, permettant en théorie de doubler la vitesse de lecture/écriture. Cela est valable si la taille du volume logique n'est pas augmentée. Si l'on étend le volume logique, les choses peuvent se compliquer.

#### Clichés (snapshots)

LVM permet de réaliser des clichés de volumes logiques, c'est-à-dire de faire une copie d'un volume logique à un moment précis dans le temps.

Lorsqu'un cliché est effectué, un nouveau volume logique snapshot est créé. Cette opération est faite à chaud, même si le volume logique d'origine est en train d'être utilisé.

**Un snapshot n'est pas une copie à l'identique d'un LV**. En fait, le snapshot **ne contient que les différences par rapport à l'original**.

La taille du snapshot est la taille allouée pour enregistrer les différences entre le snapshot à un instant t et le LV principal qui continue d'évoluer.

Ainsi, si des données sont ajoutées sur le LV principal, le snapshot doit noter que ces nouvelles données ne lui appartiennent pas.

A l'inverse, si de la donnée est supprimée sur le LV principal, le snapshot doit conserver ces données de son côté.

Ainsi, le snapshot a une taille souvent plus réduite qu'un LV classique.

Le snapshot peut avoir plusieurs utilisations. Cette technologie permet notamment de faire du _rollback_ (retour arrière), par exemple sur une mise à jour logiciel. Avant de faire la mise à jour, faire un snapshot. Si tout se passe bien, après tests, il suffit de supprimer le snapshot. Sinon, on peut faire un retour arrière sur le snapshot.

Une autre utilisation courante consiste à faire la sauvegarde d'un service avec le minimum de _downtime_ (temps de coupure). Pour certaines bases de données typiquement, pendant une sauvegarde qui peut prendre plusieurs minutes (voire plusieurs dizaines de minutes), on doit empêcher l'écriture sur la base (poser un verrou ou _lock_), ce qui limite considérablement le service.

Avec le snapshot, il suffit de mettre un verrou, puis de prendre une "photo" du volume, ce qui est très rapide. Après un court instant, on enlève le verrou sur la base, qui peut continuer d'évoluer et fournir le service avec un temps de coupure minimum.

### Administration du LVM

#### En bref : trois étapes

Ci-dessous les étapes de création d'un LV: 

1. Déclarer une partition en tant que PV :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
pvcreate /dev/<partition>
```

2. Création d'un VG et ajout du PV à ce nouveau VG

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
vgcreate <nom_VG> /dev/<PV récemment créé>
```

3. Création d'un LV dans le VG précédemment créé :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
lvcreate -L taille -n <nom_LV> <nom_VG>
```

#### Etape 1 : Volumes physiques (PV)

Toutes les commandes relatives aux volumes physiques commencent par `pv`. Pour les voir s'afficher, dans le shell, il faut taper `pv` puis appuyez deux fois sur la touche [Tab] :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# pv
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
pvchange   pvck       pvcreate   pvdisplay  pvmove     pvremove   pvresize   pvs        pvscan
```

##### Création d'un volume physique (PV)

Il faut d'abord déclarer une partition physique ou un disque dur comme étant utilisable par LVM.

Même si cela est possible, il est déconseillé d'utiliser un disque entier comme volume physique.

Bien sûr, les partitions auront été au préalable créées, grâce à l'outil `fdisk` par exemple.

La création d'un volume physique se fait en utilisant la commande :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
pvcreate <partition>
```

On peut avoir des informations sur le volume physique créé avec :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
pvdisplay
```

PAr exemple, soit d'une partition `/dev/sdb1` de 20 Go créé avec `parted` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# pvcreate /dev/sdb1
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  Physical volume "/dev/sdb1" successfully created.
```
<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# pvdisplay
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               cl
  PV Size               <29,00 GiB / not usable 3,00 MiB
  Allocatable           yes (but full)
  PE Size               4,00 MiB
  Total PE              7423
  Free PE               0
  Allocated PE          7423
  PV UUID               qMv5rT-hZjS-oUrv-DuVh-BGaC-DCBH-pNvYHa

  "/dev/sdb1" is a new physical volume of "<20,00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               <20,00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               Pn2JKX-2ru0-Ic8n-04Av-yqNA-HJtB-psyQu3
```

La partition `/dev/sdb1` est maintenant utilisable par LVM.

#### Suppression d'un volume physique

La suppression d'un volume physique se fait avec la commande `pvremove` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
pvremove <volume_physique>
```

Par exemple :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# pvremove /dev/sdb1
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  Labels on physical volume "/dev/sdb1" successfully wiped.
```
<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# pvdisplay
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               cl
  PV Size               <29,00 GiB / not usable 3,00 MiB
  Allocatable           yes (but full)
  PE Size               4,00 MiB
  Total PE              7423
  Free PE               0
  Allocated PE          7423
  PV UUID               qMv5rT-hZjS-oUrv-DuVh-BGaC-DCBH-pNvYHa
```

> **Remarque**
> Le PV ne doit pas être utilisé dans un groupe de volumes.

La commande `pvs` donne des informations pertinentes avec un format réduit.

#### Etape 2 : Groupes de volumes (VG)

##### Création d'un groupe de volumes (VG)

Toutes les commandes concernant les groupes de volumes commencent par `vg` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# vg
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
vgcfgbackup    vgchange       vgconvert      vgdisplay      vgextend       vgimportclone  vgmknodes      vgremove       vgs            vgsplit
vgcfgrestore   vgck           vgcreate       vgexport       vgimport       vgmerge        vgreduce       vgrename       vgscan
```

Il faut créer un groupe de volumes à partir d'au moins un volume physique (PV). Ainsi, avec la commande `vgcreate`, il faut préciser le nom du groupe de volumes, mais également un ou plusieurs volumes physiques :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
vgcreate <nom_du_VG> <volume_physique> [<volume_physique>]
```

Par exemple :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# vgcreate datas /dev/sdb1
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
  Volume group "datas" successfully created
```

```shell
[root@localhost ~]# vgdisplay
  --- Volume group ---
  VG Name               datas
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <20,00 GiB
  PE Size               4,00 MiB
  Total PE              5119
  Alloc PE / Size       0 / 0
  Free  PE / Size       5119 / <20,00 GiB
  VG UUID               UrQrio-FoJV-Avsi-JErr-b8j5-uGzB-NckOpf

  --- Volume group ---
  VG Name               cl
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <29,00 GiB
  PE Size               4,00 MiB
  Total PE              7423
  Alloc PE / Size       7423 / <29,00 GiB
  Free  PE / Size       0 / 0
  VG UUID               OCOkos-ZN2C-gqsw-kEZB-uaKt-FZlu-O6pW07
```

On vient de créé un nouveau VG appelé `datas`, constitué pour l'instant d'un seul PV `/dev/sdb1`.

##### Etendre un groupe de volumes

On peut ajouter de nouveau PV au VG grâce à la commande `vgextend` et à chaud.

```shell
vgextend <nom_du_VG> <volume_physique>
```

Par exemple : 
```shell
[root@localhost ~]# vgextend datas /dev/sdb2
  Volume group "datas" successfully extented
```

##### Informations sur le groupe de volumes

Pour afficher les informations d'un groupe de volumes, on utilise la commande `vgdisplay` :

```shell
[root@localhost ~]# vgdisplay
  --- Volume group ---
  VG Name               datas
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <20,00 GiB
  PE Size               4,00 MiB
  Total PE              5119
  Alloc PE / Size       0 / 0
  Free  PE / Size       5119 / <20,00 GiB
  VG UUID               UrQrio-FoJV-Avsi-JErr-b8j5-uGzB-NckOpf

  --- Volume group ---
  VG Name               cl
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <29,00 GiB
  PE Size               4,00 MiB
  Total PE              7423
  Alloc PE / Size       7423 / <29,00 GiB
  Free  PE / Size       0 / 0
  VG UUID               OCOkos-ZN2C-gqsw-kEZB-uaKt-FZlu-O6pW07
```

Il est également possible d'afficher les volumes physiques participant à un groupe de volumes, avec l'option `-v` :

```shell
[root@localhost ~]# vgdisplay -v
  --- Volume group ---
  VG Name               datas
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <20,00 GiB
  PE Size               4,00 MiB
  Total PE              5119
  Alloc PE / Size       0 / 0
  Free  PE / Size       5119 / <20,00 GiB
  VG UUID               UrQrio-FoJV-Avsi-JErr-b8j5-uGzB-NckOpf

  --- Physical volumes ---
  PV Name               /dev/sdb1
  PV UUID               VHJ741-LdRs-iS7W-tuhX-WKW4-TVhf-2tAuoY
  PV Status             allocatable
  Total PE / Free PE    5119 / 5119

  --- Volume group ---
  VG Name               cl
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <29,00 GiB
  PE Size               4,00 MiB
  Total PE              7423
  Alloc PE / Size       7423 / <29,00 GiB
  Free  PE / Size       0 / 0
  VG UUID               OCOkos-ZN2C-gqsw-kEZB-uaKt-FZlu-O6pW07

  --- Logical volume ---
  LV Path                /dev/cl/swap
  LV Name                swap
  VG Name                cl
  LV UUID                41eZ3J-uCo3-gle2-jpVJ-bK2u-IDss-o7Fky1
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:24 +0100
  LV Status              available
  # open                 2
  LV Size                3,00 GiB
  Current LE             768
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/cl/root
  LV Name                root
  VG Name                cl
  LV UUID                F04yXI-cnuZ-NGZ9-0Pdg-AQoV-SWOG-1jSyH7
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:25 +0100
  LV Status              available
  # open                 1
  LV Size                <26,00 GiB
  Current LE             6655
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0

  --- Physical volumes ---
  PV Name               /dev/sda2
  PV UUID               qMv5rT-hZjS-oUrv-DuVh-BGaC-DCBH-pNvYHa
  PV Status             allocatable
  Total PE / Free PE    7423 / 0
```

La commande `vgs` est également très prisée des administrateursn car **pertinente et rapide à taper** :

```shell
[root@localhost ~]# vgs
  VG    #PV #LV #SN Attr   VSize   VFree
  cl      1   2   0 wz--n- <29,00g      0
  datas   1   0   0 wz--n- <20,00g <20,00g
```

#### Etape 3 : Volumes logiques (LV)

##### Création d'un volume logique : lvcreate

Toutes les **commandes concernant les volumes logiques** commencent par `lv` :

```shell
[root@localhost ~]# lv
lvchange     lvcreate     lvextend     lvmconfig    lvmdump      lvmsadc      lvreduce     lvrename     lvs
lvconvert    lvdisplay    lvm          lvmdiskscan  lvmpolld     lvmsar       lvremove     lvresize     lvscan
```

On peut **divier un groupe de volumes en volumes logiques**, vus comme des partitions par le système.

Pour définir un volume logique dans un groupe de volumes, on utilise la commande `lvcreate` :

```shell
lvcreate -L taille -n <nom_LV> <nom_VG>
```

+ L'option `-L` est la taille du volume logique en mégaoctects. Pour définir une taille en gigaoctects, il est possible de taper **10G** pour 10 gigaoctects.
+ L'option `-n` permet de définir le nom du volume logique. Si cette option n'est pas précisée, le nom par défaut est `lvol1` (s'il s'agit du premier volume logique).
+ Il faut **préciser le nom du groupe de volumes** dans lequel on crée notre volume logique.

Par exemple, on créé un volume logique `dataslv` de 5 Go sur le VG `datas` :

```shell
[root@localhost ~]# lvcreate -L5G -n dataslv datas
  Logical volume "dataslv" created.
```

##### Informations sur le volume logique

Pour afficher les informations sur le volume logique, on utilise la commande `lvdisplay` :

```shell
[root@localhost ~]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/datas/dataslv
  LV Name                dataslv
  VG Name                datas
  LV UUID                wu9xTH-9RvE-uBF1-Rq4Q-dD3v-lAiD-sP6uc3
  LV Write Access        read/write
  LV Creation host, time localhost.localdomain, 2020-11-29 12:20:06 +0100
  LV Status              available
  # open                 0
  LV Size                5,00 GiB
  Current LE             1280
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:2

  --- Logical volume ---
  LV Path                /dev/cl/swap
  LV Name                swap
  VG Name                cl
  LV UUID                41eZ3J-uCo3-gle2-jpVJ-bK2u-IDss-o7Fky1
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:24 +0100
  LV Status              available
  # open                 2
  LV Size                3,00 GiB
  Current LE             768
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/cl/root
  LV Name                root
  VG Name                cl
  LV UUID                F04yXI-cnuZ-NGZ9-0Pdg-AQoV-SWOG-1jSyH7
  LV Write Access        read/write
  LV Creation host, time localhost, 2020-10-29 17:56:25 +0100
  LV Status              available
  # open                 1
  LV Size                <26,00 GiB
  Current LE             6655
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0

```

Notons les éléments suivants :
+ `LV Name` : le volume logique est vu comme un périphérique à part entière dans `/dev/`.
 + Son chemin complet comprend le nom du groupe de volumes dont il est issu.
+ `LV UUID` : comme n'importe quel périphérique du système, le volume logique possède un UUID (_Universal Unique Identifier_, identifiant unique universel).

Il existe la commande `lvs` pour avoir des informations pertinente et rapide :

```shell
[root@localhost ~]# lvs
  LV      VG    Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root    cl    -wi-ao---- <26,00g
  swap    cl    -wi-ao----   3,00g
  dataslv datas -wi-a-----   5,00g
```

##### Notation du LV dans le système

Dans le système, le LV est connu sous deux noms interchangeables :

+ Le VG est un sous-répertoire de `/dev` contenant le LV : `/dev/<vg>/<lv>`
+ Le nom VG+LV est un device dans `/dev/mapper` : `/dev/mapper/<vg>-<lv>`

Exemple :

```shell
[root@localhost ~]# ls -l /dev/mapper/cl-root
lrwxrwxrwx 1 root root 7 29 nov.  11:33 /dev/mapper/cl-root -> ../dm-0
[root@localhost ~]# ls -l /dev/cl/root
lrwxrwxrwx 1 root root 7 29 nov.  11:33 /dev/cl/root -> ../dm-0
```

On peut remarquer que les devices sont en fait des liens symboliques vers un device appelé _device mapper_, qui est la représentation dans le système des blocs de données concaténés provenant des différents volumes physiques.

##### Utilisation du volume logique

Le volume logique ainsi créé doit être utilisé tel quel et considéré comme une partition à part entière. La prochaine étape consiste à le formater, c'est-à-dire à **créer un système de fichiers**.