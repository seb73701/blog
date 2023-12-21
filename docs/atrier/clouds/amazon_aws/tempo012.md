---
title: Automatisation de la création d'une AMI CentOS avec un partitionnement en LVM
description: 
published: true
date: 2023-06-11T14:03:29.481Z
tags: 
editor: markdown
dateCreated: 2023-06-11T14:03:29.481Z
---

# Automatisation de la création d'une AMI CentOS avec un partitionnement en LVM

## Contexte
Chez notre client, nous avions besoin d’avoir une AMI (Amazon Machine Image) personnalisée avec différentes partitions afin d’être conforme aux règles CIS (Center for Internet Security) recommandées. Ces règles permettent à la fois de séparer logiquement les fichiers liés aux applications des éléments tels que les journaux, mais aussi de limiter l'exécution de fichiers (comme les scripts shell par exemple) dans des répertoires accessibles à tous (tels que /tmp et /var).

Nous avions pour cela décidé d’utiliser la solution de partitionnement de volume Linux nommée LVM (Logical Volume Manager) qui existe depuis longtemps et qui offre un certain nombre de fonctionnalités avancées. Elle fonctionne sur une couche séparée afin de créer un schéma de partitionnement virtuel par-dessus la disposition réelle du (ou des) disque(s).

Dans un premier temps, nous verrons pourquoi la solution LVM a été choisie. Puis dans un second temps, nous verrons comment créer une AMI personnalisée avec une configuration LVM sur la base d’une AMI Centos. Finalement, nous verrons comment automatiser cette procédure de création d’AMI avec l’outil Packer.

## Pourquoi utiliser LVM ?
La capacité de stockage traditionnelle est basée sur la capacité des disques individuels. LVM utilise un concept différent. L'espace de stockage est géré en mettant en commun la capacité des disques disponibles. Avec le stockage traditionnel, trois disques de 100 Go sont traités individuellement. Avec LVM, ces trois mêmes disques sont considérés comme une capacité de stockage agrégée de 300 Go.

Cela se fait en désignant les disques de stockage en tant que volumes physiques (PV). Les PV sont ensuite ajoutés à un ou plusieurs groupes de volumes (VG). Les VG sont découpés en un ou plusieurs volumes logiques (LV), qui sont ensuite traités comme des partitions traditionnelles.

Voici un schéma montrant les différentes couches de l’architecture LVM :

![lvm_v4--1--1.png](/assets/img/clouds/tempo/lvm_v4--1--1.png)

LVM offre une plus grande flexibilité par rapport aux partitions traditionnelles, en effet, avec des configurations LVM, on a la possibilité de faire évoluer la capacité de stockage facilement et rapidement. Et la capacité peut être aussi facilement réallouée.

## Création de l’AMI personnalisée

### Lancement d’une nouvelle instance avec un volume EBS supplémentaire
Tout d’abord, nous allons lancer une instance EC2 et dans notre cas, nous attachons un volume EBS SSD de 20 Go en plus du volume racine. Le volume EBS porte le nom de périphérique suivant : "/dev/sdf".

### Création du volume physique sur le volume EBS attaché
Nous allons maintenant utiliser l’outil “sgdisk” pour la création et la manipulation de tables de partition en ligne de commande :

```
sgdisk -og /dev/xvdf
```

Dans un premier temps, nous allons effacer toutes les données de partition et convertir le disque MBR (Master Boot Record), par défaut, en un disque GPT (GUID Partition Table).

Les disques partitionnés Master Boot Record sont remplacés par la nouvelle norme GUID Partition Table, mais MBR est encore largement utilisé comme format par défaut. Le format GPT pour les tables de partition présente de nombreux avantages par rapport au format MBR. Outre la prise en charge de disques d'une taille nettement supérieure, elle permet un démarrage plus rapide et plus stable. C’est donc pour cela que nous allons partir sur un disque GPT.

Dans une configuration GPT, il est nécessaire d’avoir une partition "BIOS boot" et la taille recommandée pour celle-ci est de 1 Mo :

```
sgdisk -n 128:1MiB:2MiB -c 128:"BIOS Boot Partition" -t 128:ef02 /dev/xvdf
```

Mais nous avons aussi besoin d’une partition Linux "/boot" de 500 Mo pour pouvoir installer Grub, le logiciel qui permet de charger le système d’exploitation :

```
sgdisk -n 1:3MiB:503MiB -c 1:"Linux" -t 1:8300 /dev/xvdf
```

Avant de pouvoir créer les différentes partitions LVM, nous allons devoir créer la partition principale “Linux LVM” :

```
sgdisk -n 2:504MiB:0 -c 2:"Linux LVM" -t 2:8e00 /dev/xvdf
```

La commande lsblk nous permet de confirmer la création de ces différentes partitions.

Une fois cette partition principale créée, nous allons pouvoir créer dans celle-ci, le volume physique :

```
pvcreate /dev/xvdf2
```

### Création du groupe de volume et ajout du volume physique dans le groupe de volume

Ensuite nous allons créer un groupe de volume pour ajouter le nouveau volume physique :

```
vgcreate centos /dev/xvdf2
```

Nous pouvons voir les détails de ce nouveau groupe de volume grâce à la commande vgdisplay.

### Création des volumes logiques et montage des répertoires sur le LVM
Une fois que le groupe de volumes LVM est créé, il est temps de créer des volumes logiques :

```
lvcreate -L 8G -n root centos
lvcreate -L 4G -n var centos
lvcreate -L 3G -n tmp centos
lvcreate -L 2G -n home centos
lvcreate -L 2G -n opt centos
```

Pour voir les détails des volumes logiques, nous pouvons utiliser la commande lvdisplay.

### Formatage et montage des systèmes de fichiers
Maintenant que les volumes logiques sont créés, nous pouvons les formater en utilisant n'importe quel système de fichiers comme ext4, XFS, et plus encore. Si vous utilisez le système de fichiers xfs :

```
mkfs.xfs /dev/xvdf1
mkfs.xfs /dev/centos/root
mkfs.xfs /dev/centos/var
mkfs.xfs /dev/centos/tmp
mkfs.xfs /dev/centos/home
mkfs.xfs /dev/centos/opt
```

Une fois les volumes logiques formatés, vous devez créer les répertoires de montage avant de monter les différents volumes logiques sur ces répertoires :

```
mkdir /newroot
mount /dev/centos/root /newroot/

mkdir /newroot/boot
mount /dev/xvdf1 /newroot/boot/

mkdir /newroot/var
mount /dev/centos/var /newroot/var/

mkdir /newroot/tmp
mount /dev/centos/tmp /newroot/tmp/

mkdir /newroot/home
mount /dev/centos/home /newroot/home/

mkdir /newroot/opt
mount /dev/centos/opt /newroot/opt/
```

Nous allons ensuite créer un montage "bind" pour le système de fichiers racine (/) existant. Cela nous permet de copier les données sans copier les fichiers dont nous n'avons pas besoin, comme les systèmes de fichiers tmpfs et /dev.

```
mkdir /oldroot
mount -o bind / /oldroot
```

Puis, nous utiliserons rsync pour copier les données du système de fichiers existant vers le nouveau, mais d'autres outils devraient fonctionner tout aussi bien, tant qu'ils préservent la propriété et les autorisations. Cette étape peut prendre un certain temps selon la quantité de données à copier.

```
rsync -avxHAX --exclude={"/home/centos/.ssh/authorized_keys","/var/log","/lost+found","/tmp/*"} /oldroot/ /newroot/
```

Nous allons maintenant devoir modifier les options de montage dans le fichier /etc/fstab afin que les nouveaux montages persistent après le redémarrage.

Tout d'abord, nous devons connaître l'UUID du nouveau système de fichiers /boot sur /dev/xvdf1.

```
boot_id=`blkid -s UUID -o value /dev/xvdf1`
```

Ensuite, nous pourrons supprimer le fichier /etc/fstab existant contenant la liste des partitions qui seront montées automatiquement au démarrage puis créer le nouveau :

```
rm -f /newroot/etc/fstab
echo "/dev/mapper/centos-root / xfs defaults 0 0" >> /newroot/etc/fstab
echo "UUID=${boot_id} /boot xfs defaults 1 2" >> /newroot/etc/fstab
echo "/dev/mapper/centos-var /var xfs defaults 0 0" >> /newroot/etc/fstab
echo "/dev/mapper/centos-tmp /tmp xfs defaults 0 0" >> /newroot/etc/fstab
echo "/dev/mapper/centos-home /home xfs defaults 0 0" >> /newroot/etc/fstab
echo "/dev/mapper/centos-opt /opt xfs defaults 0 0" >> /newroot/etc/fstab
```

Nous devons maintenant mettre à jour le fichier par défaut de Grub dans /newroot/etc/default/grub en ajoutant la partie lvm :

```
sed -i 's/^GRUB_CMDLINE_LINUX.*/GRUB_CMDLINE_LINUX="crashkernel=auto ipv6.disable=1 console=ttyS0,115200 console=tty0 rd.lvm.lv=centos\/root"/' /newroot/etc/default/grub
```

### Modification des paramètres de démarrage
Ensuite, nous devons créer un nouveau fichier grub.cfg, un nouveau initramfs (système de fichiers virtuel initial), et installer grub sur le nouveau disque. Tout cela sera fait sur le nouveau disque et pour ce faire, nous allons utiliser la commande chroot qui nous permettra de changer d’environnement en changeant le répertoire racine vers le nouvel emplacement.

Les commandes que nous allons exécuter nécessitent l'accès à /dev, /sys, et /proc, mais ceux-ci n'existent pas en chroot, nous devons donc les lier au point de montage du nouveau volume racine (/newroot) :

```
mount -o bind /dev /newroot/dev
mount -o bind /sys /newroot/sys
mount -o bind /proc /newroot/proc
```

Nous allons maintenant chrooter vers le nouveau volume :

```
chroot /newroot/
```

Ensuite il va falloir activer /tmp afin de pouvoir l’utiliser, étant désactivé par défaut :

```
systemctl unmask tmp.mount
```

Puis nous allons créer le nouveau fichier grub.cfg :

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

En plus de cela, nous allons créer le nouveau fichier initramfs qui contiendra les modules pour LVM. Il est important d’utiliser la bonne version du kernel pour cela.

La commande "uname -r" nous permettra de récupérer le numéro de version du système d'exploitation :

```
dracut -f -v "/boot/initramfs-$(uname -r).img"
```

Et enfin, nous devons installer Grub sur le secteur de démarrage de /dev/xvdf. Nous avons besoin de l'option --recheck pour qu’il soit associé au bon périphérique :

```
grub2-install --recheck /dev/xvdf
```

Une fois avoir fait toutes ces étapes, nous pouvons arrêter l'instance et créer un instantané du volume attaché. Ensuite, nous pouvons créer l’AMI à partir de cet instantané nouvellement créé.

En lançant une nouvelle instance avec cette AMI, vous verrez que le système a été migré vers LVM avec les différentes partitions.

## Automatisation de la création d’AMI personnalisée avec Packer
### Qu'est-ce que Packer ?
Packer est un outil open-source conçu par Hashicorp qui automatise la création de tout type d'images machine pour de multiples plateformes.                                        

Il ne remplace pas les outils de gestion de la configuration comme Ansible mais il peut fonctionner avec ces outils pour installer des applications tout en créant des images. Un fichier de configuration json (template) est utilisé pour créer une image de machine.

Packer utilise les concepts de builders pour faire tourner une instance et des provisioners afin de configurer des applications ou des services. Une fois la configuration terminée, il arrête l'instance et crée un instantané à partir de celui-ci.

### Composants du template Packer

- **Variables**
Les variables utilisateur sont utilisées pour paramétrer le template Packer. Ces variables peuvent être transmises par des lignes de commande, des variables d'environnement, Vault ou des fichiers. Cette section variable est une association clé-valeur avec le nom de la variable assigné à une valeur par défaut. Ces variables peuvent être remplacées pendant l'exécution en utilisant l'option -var.

```
"variables":{
    "region": "eu-west-1",
    "vpc_id": "vpc-0c6d8478a3a1cdb91",
    "subnet_id": "subnet-0ef24da70dbc926e8",
    "source_ami": "ami-0b850cf02cc00fdc8",
    "ssh_username": "centos"
 }
```

- **Builders**
Ces builders sont responsables de la création d'une instance et de la génération d'images de machine à partir de celle-ci. Le builder contient des informations telles que les clés d'accès dont le builder a besoin pour se connecter à une plate-forme comme AWS. Nous avons plusieurs types de "builder" comme "amazon-ebs" ou "amazon-ebssurrogate".

Le builder "amazon-ebs" permet de créer des AMI utilisant des EBS comme volume de stockage, mais ne permet pas d’utiliser un second volume de stockage attaché à l’instance comme nous l’aurions besoin dans notre cas. Nous allons pour cela utiliser le builder "amazon-ebssurrogate". Ce builder permet de créer des AMI en exécutant une instance source avec un volume attaché, en provisionnant le volume attaché de manière à ce qu'il puisse être utilisé comme volume racine pour l'AMI, puis en faisant un snapshot et en créant l'AMI à partir de ce volume.

```json
"builders": [{
  "type": "amazon-ebssurrogate",
  "ami_virtualization_type": "hvm",
  "region": "{{user `region`}}",
  "instance_type": "t2.micro",
  "ena_support": true,
  "vpc_id": "{{user `vpc_id`}}",
  "subnet_id": "{{user `subnet_id`}}",
  "ssh_username": "{{user `ssh_username`}}",
  "source_ami": "{{user `source_ami`}}",
  "ami_name": "centos-7",
  "ami_description": "Centos 7 LVM",
  "launch_block_device_mappings": [{
     "delete_on_termination": true,
     "device_name": "/dev/xvdf",
     "volume_type": "gp2",
     "volume_size": "20"
    }
   ],
  "ami_root_device": {
    "source_device_name": "/dev/xvdf",
    "device_name": "/dev/xvda",
    "delete_on_termination": true,
    "volume_size": "20",
    "volume_type": "gp2"
   }
 }]
```

On lance une instance avec un deuxième volume de 20G grâce au paramètre "launch_block_device_mappings” en plus du volume root de 20G.

- **Provisioners**
La section Provisioners contient une liste de provisioners que Packer utilise pour installer et configurer les applications dans l'instance en cours d'exécution avant de créer une image de machine. Il prend en charge différents types de gestion de la configuration comme Ansible, scripts shell, etc.

```json
"provisioners": [
    {
    "type": "ansible",
    "playbook_file": "ansible/playbook.yml"
    }
   ]
```

Dans notre cas nous faisons toute la configuration via Ansible et voici à quoi ressemble le playbook :

```
- name: "init-build-scripts: adding ansible folder"
  copy:
    src: ../ansible/
    dest: /tmp/ansible/

- name: "init-build-scripts: start paritionning script"
  command: sudo -E bash scripts/create_partitions.sh
  args:
    chdir: /tmp

- name: "init-build-scripts: start chroot script"
  command: sudo -E chroot /newroot/ ./tmp/chroot.sh
  args:
    chdir: /tmp
```

Tout d’abord, on copie notre dossier ansible où devront se trouver nos scripts shell contenant toutes les commandes vu précédemment sur la machine distante puis on lance le script permettant la création des différentes partitions et enfin, on lance le script “chroot” pour configurer le nouveau volume correctement et notamment les paramètres de démarrage.

### Exécution du template Packer
Avant de pouvoir lancer le build, nous devons dans un premier temps vérifier que le fichier packer est valide en utilisant la commande validate :

```
packer validate packer.json
```

Suite à la vérification du fichier packer, nous pouvons lancer le build de l’AMI :

```
packer build packer.json
```

Une fois que le fichier packer est exécuté avec succès, le nom de l'AMI nouvellement générée est affiché à la fin.

## Conclusion
Voilà ! Vous savez maintenant comment créer une AMI personnalisée avec différentes partitions grâce à l’utilisation de LVM, mais aussi comment automatiser cette procédure avec Packer afin de vous simplifier la vie.

Bien qu'il soit facile d'exécuter Packer sur votre poste de travail pour créer des AMIs, son exécution prend généralement pas mal de temps. C'est pourquoi il est préférable d'utiliser Packer depuis un serveur d'automatisation tel que Jenkins, sur une machine séparée. Par exemple, chez notre client, le Packer est lancé à partir d’un agent Jenkins dans un cluster EKS (Elastic Kubernetes Service).

Nous créons une AMI de base pour ensuite créer des AMIs applicatives à partir de celle-ci. Par la suite, nous lançons des instances EC2 à partir de ces AMIs applicatives contenant les applications principales et toutes leurs dépendances. Cela nous permet de créer une infrastructure immuable, en effet, plus aucune modification manuelle n’est effectuée, une fois les instances lancées.

Et lorsque nous voulons apporter des modifications à l’application ou que nous voulons appliquer des patchs de sécurité, nous avons juste à builder et déployer une nouvelle version de l’AMI applicative.

Source : https://blog.ippon.fr/