---
title: Augmenter l'espace disque LVM d'une VM VMware sous CentOS 7
description: 
published: true
date: 2023-04-21T11:16:07.342Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:00.720Z
---

====== Augmenter l'espace disque LVM d'une VM VMware sous CentOS 7 ======
  * Augmenter l'espace disque alloué sur vSphere (vCenter)
  * Se connecter au serveur en SSH
  * Vérifier si l'espace disque est détecté automatiquement

<code>fdisk -l
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p

Disk /dev/sda: 73.0 GB, 73014444032 bytes, 142606336 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a1b6e

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   142606335    70253568   8e  Linux LVM

Command (m for help): q
</code>

Ici on a une VM taillé à 100Go et ''fdisk'' qui remonte que 73.0 GB. Il faut donc refaire un scan pour récupérer la taille réelle.

<code>echo 1 > /sys/class/scsi_device/%index_device%/device/rescan</code>

__Exemple :__
<code> echo 1 >/sys/class/scsi_device/0\:0\:0\:0/device/rescan</code>

On vérifie maintenant :

<code>fdisk -l

Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a1b6e

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   142606335    70253568   8e  Linux LVM

Disk /dev/mapper/rootvg-rootlv: 69.8 GB, 69784829952 bytes, 136298496 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rootvg-swaplv: 2147 MB, 2147483648 bytes, 4194304 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
</code>

Comme on peut le voir, la taille est bien de 100Go.

On va créé une nouvelle partition :

<code>cfdisk</code>

<code>
                                                                                                   cfdisk (util-linux 2.23.2)

                                                                                                      Disk Drive: /dev/sda
                                                                                               Size: 107374182400 bytes, 107.3 GB
                                                                                     Heads: 255   Sectors per Track: 63   Cylinders: 13054

           Name                             Flags                            Part Type                    FS Type                                       [Label]                                 Size (MB)
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                              Pri/Log                     Free Space                                                                                 1.05                      *
           sda1                             Boot                              Primary                     ext4                                                                                    1073.75                      *
           sda2                                                               Primary                     LVM2_member                                                                            71939.66                      *
                                                                              Pri/Log                     Free Space                                                                             34359.74                      *













































              [   Help   ]  [   New    ]  [  Print   ]  [   Quit   ]  [  Units   ]  [  Write   ]


                                                                                              Create new partition from free space

</code>
avec les fleches <Haut><Bas>, se diriger vers la ligne "Free Space"
avec les fleches <Gauche><Droite>, se diriger vers le menu "[   New    ]"
<code>
     [Primary]  [Logical]  [Cancel ]


                                                                                                 Create a new primary partition
</code>
Sélectionner "[Logical]"
Valider la taille proposée :
<code>
           Size (in MB): 34359.74
</code>
On obtient :
<code>
                                                                                                   cfdisk (util-linux 2.23.2)

                                                                                                      Disk Drive: /dev/sda
                                                                                               Size: 107374182400 bytes, 107.3 GB
                                                                                     Heads: 255   Sectors per Track: 63   Cylinders: 13054

           Name                             Flags                            Part Type                    FS Type                                       [Label]                                 Size (MB)
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                              Primary                     Free Space                                                                                 1.05                      *
           sda1                             Boot                              Primary                     ext4                                                                                    1073.75                      *
           sda2                                                               Primary                     LVM2_member                                                                            71939.66                      *
           sda5                                                               Logical                     Linux                                                                                  34359.74                      *













































              [ Bootable ]  [  Delete  ]  [   Help   ]  [ Maximize ]  [  Print   ]  [   Quit   ]  [   Type   ]  [  Units   ]  [  Write   ]


                                                                                         Toggle bootable flag of the current partition

</code>
Notre ligne "sda5" est créée, on va lui définir un type en sélectionnant "[   Type   ]"
<code>
              [ Bootable ]  [  Delete  ]  [   Help   ]  [ Maximize ]  [  Print   ]  [   Quit   ]  [   Type   ]  [  Units   ]  [  Write   ]


                                                                                    Change the filesystem type (DOS, Linux, OS/2 and so on)

</code>
On va lui définir un code "8e" :

<code>
                                                                                                   cfdisk (util-linux 2.23.2)

                                                                                                      Disk Drive: /dev/sda
                                                                                               Size: 107374182400 bytes, 107.3 GB
                                                                                     Heads: 255   Sectors per Track: 63   Cylinders: 13054

           Name                             Flags                            Part Type                    FS Type                                       [Label]                                 Size (MB)
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                              Primary                     Free Space                                                                                 1.05                      *
           sda1                             Boot                              Primary                     ext4                                                                                    1073.75                      *
           sda2                                                               Primary                     LVM2_member                                                                            71939.66                      *
           sda5                                                               Logical                     Linux                                                                                  34359.74                      *

 01 FAT12                 0F W95 Ext'd (LBA)       39 Plan 9                55 EZ-Drive              84 OS/2 hidden C: drive  A8 Darwin UFS            DA Non-FS data           F2 DOS secondary
 02 XENIX root            10 OPUS                  3C PartitionMagic recov  56 Golden Bow            85 Linux extended        A9 NetBSD                DB CP/M / CTOS / ...     FB VMware VMFS
 03 XENIX usr             11 Hidden FAT12          40 Venix 80286           5C Priam Edisk           86 NTFS volume set       AB Darwin boot           DE Dell Utility          FC VMware VMKCORE
 04 FAT16 <32M            12 Compaq diagnostics    41 PPC PReP Boot         61 SpeedStor             87 NTFS volume set       AF HFS / HFS+            DF BootIt                FD Linux raid autodetec
 05 Extended              14 Hidden FAT16 <32M     42 SFS                   63 GNU HURD or SysV      88 Linux plaintext       B7 BSDI fs               E1 DOS access            FE LANstep
 06 FAT16                 16 Hidden FAT16          4D QNX4.x                64 Novell Netware 286    8E Linux LVM             B8 BSDI swap             E3 DOS R/O               FF BBT
 07 HPFS/NTFS/exFAT       17 Hidden HPFS/NTFS      4E QNX4.x 2nd part       65 Novell Netware 386    93 Amoeba                BB Boot Wizard hidden    E4 SpeedStor
 08 AIX                   18 AST SmartSleep        4F QNX4.x 3rd part       70 DiskSecure Multi-Boo  94 Amoeba BBT            BE Solaris boot          EB BeOS fs
 09 AIX bootable          1B Hidden W95 FAT32      50 OnTrack DM            75 PC/IX                 9F BSD/OS                BF Solaris               EE GPT
 0A OS/2 Boot Manager     1C Hidden W95 FAT32 (LB  51 OnTrack DM6 Aux1      80 Old Minix             A0 IBM Thinkpad hiberna  C1 DRDOS/sec (FAT-12)    EF EFI (FAT-12/16/32)
 0B W95 FAT32             1E Hidden W95 FAT16 (LB  52 CP/M                  81 Minix / old Linux     A5 FreeBSD               C4 DRDOS/sec (FAT-16 <   F0 Linux/PA-RISC boot
 0C W95 FAT32 (LBA)       24 NEC DOS               53 OnTrack DM6 Aux3      82 Linux swap / Solaris  A6 OpenBSD               C6 DRDOS/sec (FAT-16)    F1 SpeedStor
 0E W95 FAT16 (LBA)       27 Hidden NTFS WinRE     54 OnTrackDM6            83 Linux                 A7 NeXTSTEP              C7 Syrinx                F4 SpeedStor































              Enter filesystem type: 8e




</code>

On obtient ceci :

<code>
                                                                                                   cfdisk (util-linux 2.23.2)

                                                                                                      Disk Drive: /dev/sda
                                                                                               Size: 107374182400 bytes, 107.3 GB
                                                                                     Heads: 255   Sectors per Track: 63   Cylinders: 13054

           Name                             Flags                            Part Type                    FS Type                                       [Label]                                 Size (MB)
 ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                                              Primary                     Free Space                                                                                 1.05                      *
           sda1                             Boot                              Primary                     ext4                                                                                    1073.75                      *
           sda2                                                               Primary                     LVM2_member                                                                            71939.66                      *
           sda5                                                               Logical                     Linux LVM                                                                              34359.74                      *



</code>

On va maintenant écricre les informations dans la table des partitions :

<code>
              [ Bootable ]  [  Delete  ]  [   Help   ]  [ Maximize ]  [  Print   ]  [   Quit   ]  [   Type   ]  [  Units   ]  [  Write   ]


                                                                                    Write partition table to disk (this might destroy data)

</code>
On valide la confirmation :

<code>
              Are you sure you want to write the partition table to disk? (yes or no):

                                                                                         Warning!!  This may destroy data on your disk!


</code>
On quitte l'utilitaire :
<code>
              [ Bootable ]  [  Delete  ]  [   Help   ]  [ Maximize ]  [  Print   ]  [   Quit   ]  [   Type   ]  [  Units   ]  [  Write   ]


                                                                                          Quit program without writing partition table

</code>

On vérifie ensuite avec la commande ''fdisk'' :

<code>
fdisk -l

Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a1b6e

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   142606335    70253568   8e  Linux LVM
/dev/sda3       142606336   209715199    33554432    5  Extended
/dev/sda5       142606399   209715199    33554400+  8e  Linux LVM

Disk /dev/mapper/rootvg-rootlv: 69.8 GB, 69784829952 bytes, 136298496 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/rootvg-swaplv: 2147 MB, 2147483648 bytes, 4194304 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
</code>

<code>
[root@myserver  ~]#  pvcreate /dev/sda5
  Device /dev/sda5 not found.
  
</code>
<code>[root@myserver  ~]# partprobe</code>
<code>
[root@myserver  ~]#  pvcreate /dev/sda5
  Physical volume "/dev/sda5" successfully created.
</code>
<code>
[root@myserver  ~]# vgextend rootvg /dev/sda5
  Volume group "rootvg" successfully extended
</code>
<code>
[root@myserver  ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree
  /dev/sda2  rootvg lvm2 a--  <67.00g   4.00m
  /dev/sda5  rootvg lvm2 a--  <32.00g <32.00g
</code>
<code>
[root@myserver  ~]# lvs
  LV     VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  rootlv rootvg -wi-ao---- 64.99g
  swaplv rootvg -wi-ao----  2.00g
</code>
<code>
[root@myserver  ~]# lvextend -l+100%FREE /dev/mapper/rootvg-rootlv
  Size of logical volume rootvg/rootlv changed from 64.99 GiB (16638 extents) to 96.99 GiB (24830 extents).
  Logical volume rootvg/rootlv successfully resized.
</code>
<code>
[root@myserver  ~]# resize2fs /dev/mapper/rootvg-rootlv
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/mapper/rootvg-rootlv is mounted on /; on-line resizing required
old_desc_blocks = 9, new_desc_blocks = 13
The filesystem on /dev/mapper/rootvg-rootlv is now 25425920 blocks long.
</code>
<code>
[root@myserver  ~]# df -h
Filesystem                 Size  Used Avail Use% Mounted on
/dev/mapper/rootvg-rootlv   96G   63G   29G  69% /
devtmpfs                   904M     0  904M   0% /dev
tmpfs                      915M     0  915M   0% /dev/shm
tmpfs                      915M  1.3M  914M   1% /run
tmpfs                      915M     0  915M   0% /sys/fs/cgroup
/dev/sda1                  976M  249M  661M  28% /boot
tmpfs                      183M     0  183M   0% /run/user/0

</code>