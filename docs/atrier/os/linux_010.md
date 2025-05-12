# Red Hat - Livre "Red Hat Enterprise Linux CentOS - Mise en production et administration de serveurs"

----

## Aide-mémoire des principales commandes

### 1. Démarrage

#### 1.1 GRUB2

+ Paramètres généraux : `/etc/default/grub`
+ Construction du fichier de configuration : `grub2-mkconfig -o /boot/grub2/grub.cfg`
+ Entrées manuelles : `/etc/grub.d/`
+ Réinstallation de GRUB sur un disque : `grub2-install /dev/<disque>`
+ Réinitialisation complète de GRUB :

```shell
rm /etc/grub.d/*
rm /etc/sysconfig/grub
yum reinstall grub2-tools
grub2-mkconfig -o /boot/grub2/grub.cfg
```

#### 1.2 Noyau et modules
+ Dans `/boot/`
+ Afficher paramètres noyau : `systcl -a`
+ Changer paramètres noyau : `sysctl <paramètre>=<valeur>`
+ Configuration des paramètres : `/etc/sysctl.conf` et `/etc/sysctl.d/`
+ Modules
 + Lister : `lsmod`
 + Infos : `modinfo <module>`
 + Charger : `modprobe <module> [paramètre=valeur]`
 + Décharger : `modprobe -r <module>`
 + Fichiers de configuration : `/etc/modprobe.d/*.conf` et `/etc/modules-load.d/*.conf`

#### 1.3 Démarrage et services

##### Systemd

+ Liste les unités : `systemd`
+ Statut : `systemctl status <unité>`
+ Activer et démarrer un service : `systemctl enable --now <nom du service>`
+ Démarrer / arrêter service : `systemctl start <nom du service>.service` et `systemctl stop <nom du service>.service`
+ Activer / désactiver service au démarrage : `systemctl enable <nom du service>.service` et `systemctl disable <nom du service>.service`
+ Démarrer un service et l'activer au démarrage : `systemctl enable --now <nom du service>.service` et `systemctl disable --now <nom du service>.service`
+ Logs pour le service `journalctl` : `_SYSTEMD_UNIT=<nom du service>.service`

##### Eteindre l'OS
+ Arrêter : `shutdown -h now` ou `halt`
+ Redémarrer : `shutdown -r now` ou `reboot`
+ Redémarrer à 07h05 : `shutdown -r 07:05`
+ Eteindre dans 10 minutes : `shutdown +10`
+ Annuler shutdown : `shutdown -c`

### 2. Installation de logiciels

#### 2.1 Abonnement

+ Déclarer système sur Portail : `subscription-manager register`
+ Attacher le système à un abonnement : `subscription-manager attach --auto`

```shell
subscription-manager unregistrer
subscription-manager list
subscription-manager facts
subscription-manager repos
```

#### 2.2 YUM/DNF (dépôts de paquets)

##### Liste des dépôts

`yum repolist`
`yum repolist all`

##### Chercher et installer des paquets

`yum list`
`yum list installed`
`yum list available`
`yum list all`
`yum grouplist`
`yum search <mot>`

##### Infos sur un paquet ou un fichier

`yum info <paquet>`
`yum provides <fichier>`

##### Installer/désinstaller un paquet ou un groupe

`yum install <paquet>`
`yum groupinstall "<groupe>"` ou `yum install @"<groupe>"`
`yum remove <paquet>`
`yum groupremove "<groupe>"`

##### Mise à jour de paquet

`yum check-update`
`yum update <paquet>`
`yum update`

##### Plugins

`yum info yum`

##### Sécurité

`yum install yum-plugin-security`
`yum check-update --security`
`yum update --security`
`yum upate-minimal --security`

##### Réinitialiser

`yum clean all`

##### Applications Streams (RHEL8/DNF)

###### Afficher la liste des modules

`yum module list`

###### Installer un module

`yum module install <nom du module>`
`yum module install <nom du module>:<version>/<profil>`

#### 2.3 RPM (paquets indépendants)

##### Installation et mise à jour/désinstallation

`rpm -Uvh <paquet>`
`rpm -e <bom du logiciel>`

##### Infos sur un paquet

`rpm -q <programme>`
`rpm -qi <programme>`

+ Lister des fichiers : `rpm -ql <paquet>`
+ Etat des fichiers : `rpm -qs <paquet>`
+ Documentations contenues : `rpm -qd <paquet>`
+ Fichiers de configuration : `rpm -qc <paquet>`

#### 2.4 Compilation à partir des sources

+ Installation compilateur C : `yum install gcc`
+ Pré-test de compilation : `./configure`
+ Compilation : `make`
+ Installation : `make install`

### 3. Partitions et systèmes de fichiers

#### 3.1 Partitionnement

+ Informations sur le disque : `parted -l <disque>`
+ Création de partitions : `parted <disque>`

Exemple : `parted /dev/sdb`

+ Commandes :
`help` : aide
`mktable msdos` / `mktable gpt` : définir le type de partitionnement
`mktable PART-TYPE [FS-TYPE] DEBUT FIN` : créer une nouvelle partition
`print` : afficher la table de partitions
`rm <numéro>` : supprimer une partition

#### 3.2 Logical Volume Manager

##### Jargon

+ PV (volumes physiques) : disques, partitions
+ VG (groupes de volumes) : concaténation de PV
+ LV (volumes logiques) : partitions logiques créées dans le VG

##### Physical Volume (PV)

+ Infos PV : `pvdisplay`
+ Déclarer un PV/Supprimer : 

`pvcreate <partition>`

Exemple : `pvcreate /dev/sdb5`

`pvremove <partition>`

Exemple : `pvremove /dev/sdb5`

+ Déplacer données vers autre PV : `pvmove <PV source> <PV destination>`

##### Volume Group (VG)

+ Informations VG : `vgdisplay`
+ Création : `vgcreate <nom du VG> <PV> [<autre PV> <etc.>]`

Exemple : 

`vgcreate Database /dev/sdb1`

+ Ajout PV au VG : `vgextend <nom du VG> <PV>`
+ Supprimer un VG : `vgremove <VG>`
+ Enlever un PV d'un VG : `vgreduce <VG> <PV>`

##### Logical Volume (LV)

+ Informations LV : `lvdisplay`
+ Créer un LV dans un VG : `lvcreate -L <taille> -n <nom LV> <VG>`

Exemple : 

`lvcreate -L 500 -n LV-Oracle Database`

+ Supprimer : `lvremove <LV>` (démonter d'abord)
+ Etendre : `lvextend -L<nouvelle taille> <LV>`
 + L800 (taille fixe 800 Mo) ou -L4G (taille fixe 4 Go) ou -L+300 (taille +300 Mo)
+ Réduire : `lvreduce -L<nouvelle taille> <LV>`
+ Créer un snapshot : `lvcreate -L <taille du snapshot> -s -n <nom du snapshot> <LV source>`
+ Supprimer un snapshot : `lvremote /dev/<VG source>/<nom du snapshot>`
+ Retour arrière sur un snapshot : `lvconvert --merge /dev/<VG source>/<nom du snapshot>`

#### 3.3 Stratis

##### Jargon

+ **blockdev** : équivalent du PV, disques, partitions
+ **pool** : équivalent du VG : contacténation de blockdevs
+ **filesystem** : équivalent du LV : partitions logiques créées dans les pools

##### Etapes

1) Déclaration de **blockdevs** et création d'un **pool**

`stratis_pool create <nom du pool> </dev/disque> </dev/disque>`

2) Création d'un **filesystem**

`stratis filesystem create <nom du pool> <nom du filesystem>`

3) Montage du **filesystem**

`mount </dev/stratis/filesystem> <point de montage>`

##### Affichage

`stratis blockdev list`
`stratis pool list`
`stratis filesystem list`
`stratis filesystem snapshot <pool> <filesystem source> <nom du snapshot> <nom du snapshot>`

#### 3.4 RAID logiciel

+ RAID-0 en bandes (performances)

`mdadm --create --verbose /dev/md0 --level=stripe --raid-devices=2 /dev/<disque1> /dev/<disque2>`

+ RAID-1 en miroir (fiabilité)

`mdadm --create --verbose /dev/md0 --level=mirror --raid-devices=2 /dev/<disque1> /dev/<disque2>`

+ RAID-5/6 (fiabilité+perfs)

`mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/sdb1 /dev/sdc1 /dev/sdd1 --spare-device=1 /dev/sde1`

+ Surveillance

`mdadm --monitor --mail-noc@acme.com --delay=300 /dev/md0 &`

#### 3.5 Système de fichiers

+ Liens symboliques/physiques
 + Physique : `ln <fichier_originel> <nouveau fichier>`
 + Symbolique : `ln -s <fichier_pointé> <nouveau fichier>`
+ Quel type de FS ?

`df -TH` ou `file -sL <partition>`

+ ext4
 + Création : `mkfs.ext4 /dev/<partition>`
 + Modification paramètres : `tune2fs`
 + Modification étiquette : `tune2fs -L <etiquette> /dev/<partition>`
 + Modification taille : `resize2fs`
 + Vérification FS : `e2fsck`
 + Informations FS : `dump2fs`
 + Structure interne : `debugfs`

+ XFS
 + Création : `mkfs.xfs /dev/<partition>`
 + Réparation : `xfs_repair <partition>`
 + Augmentation taille max : `xfs_growfs -d <partition>`

+ Montage

`mount </dev/chemin FS> <répertoire point_de_montage> [-o<options>] [-t <type_de_FS>]`

 + Montage auto dans `/etc/fstab`
`<périphérique> <point_de_montage> <type_de_FS> <options> <activation_dump> <priorité_fsck>`
 + Remonter en lecture/écriture : `mount -o remount,rw <système_de_fichiers>`
+ Informations
 + Utilisation des FS : `df -h`
 + Taille des répertoires : `du --max-depth=1 -h 2>/dev/null`

#### 3.6 Quotas

+ 1. Monter partition avec options `usrquota,grpquota`
+ 2. Analyser disque : `quotacheck -cug <point_de_montage>`
+ 3. Créer base de quotas utilisateurs : `quotacheck -avug`
+ 4. Assigner quotas à un utilisateur : `edquota <utilisateur>` ou en ligne : `setquota -u <utilisateur> <limite_blocs_souples> <limite_blocs_stricte> <limite_inode_souple> <limite_inode_stricte> -a <FS>`
+ 5. Vérifier quotas : `quota <utilisateur>`
+ 6. Activer quotas : `quotaon`

### 4. Utilisateurs et groupes

#### 4.1 Utilisateurs

+ Ajout : `useradd <nom_d'utilisateur>`
 + `-c '<informations>'`
 + `-d <répertoire_utilisateur>`
 + `-e <date>`
 + `-s <chemin_du_shell>`
 + `-r (compte système)`
+ Modification : `usermod <options> <utilisateur>`
 + `-a -G <groupes> (ajout à des groupes)`
 + `-L (verrouiller)`
 + `-U (déverrouiller)`
 + `-m -d <nouveau_repertoire_utilisateur>`
+ Suppression : `userdel <utilisateur>`
+ Mot de passe : `passwd <nom_d'utilisateur>`
+ Changement temporisateurs : `chage` (voir schéma)
+ Changement première connexion : `chage -d 0 <utilisateur>`

##### Variables d'environnement
+ Afficher : `set`
+ Définir : `export <variable>=<valeur>`
+ Stockage : `/etc/profile` ou `~/.bash_profile`

##### Alias

+ Définir alias : `<alias>='<commande>'`
+ Stockage : `/etc/bashrc` ou `~./bashrc`

#### 4.2 Fichiers des utilisateurs

+ `/etc/passwd`

`<username>:x:UID:GID:<informations>:<repertoire>:<shell>`

+ `/etc/shadow`

`<username>:<hash mot de passe>:DATE:MIN_DAYS:MAX_DAYS:WARNDAYS:INACTIVE:EXPDATE`

+ `/etc/login.defs`

 + `PASS_MAX_DAYS`
 + `PASS_MIN_DAYS`
 + `PASS_WARN_AGE`

+ `/etc/default/useradd`
 + `INACTIVE`

#### 4.3 Droits

+ Sur fichiers
 + En lecture : fichier peut être lu.
 + En écriture : fichier peut être écrit.
 + En exécution : fichier peut être exécuté.

+ Sur répertoires
 + En lecture : lister le contenu du répertoire.
 + En écriture : modifier le contenu du répertoire.
 + En exécution seule : traverser le répertoire.

+ Droits spéciaux
 + SetUID/SetGID `u+s`,`g+s` sur exécutables : prendre droits du propriétaire.
 + Stickybit `+t` sur répertoire : suppression impossible des fichiers des autres utilisateurs
 + SetGID `g+s` sur répertoires : même GID pour répertoires enfants que pour répertoires parents.

+ Afficher : `ls -l`
+ Changement de propriétaire : `chown -v <utilisateur_propriétaire>:<groupe_propriétaire> <fichier_ou_répertoire>`
+ Changement de droits : `chmod <droits> <fichier_ou_repertoire>`
 + Exemple : `chmod ug+rw,o-rw fichier`
 + Exemple : `chmod 755 fichier`
+ Droits par défaut : `umask`
 + Fichier : `/etc/profile`

#### 4.4 File ACL

+ Lire droits FACL : `getfacl <fichier>`
+ Modifier droits FACL : `setfacl -m u:<utilisateur>:<droits>,g:<groupe>:<droits> <fichier>`
 + Par exemple : `setfacl -m u:rico:rwx,u:thbault:rx /home/ITprojets/`

#### 4.5 Attributs étendus

+ Changer : `chattr <attribut> <fichier>`
 + `-R` : récursif
+ Lister : `lsattr <fichier>`

#### 4.6 sudo

+ Etat : `getenforce`
+ Changer état : `setenforce 0` (permissif) ou `/etc/selinux/config`

`SELINUX=disabled`

+ Voir contextes de sécurité fichiers : `ls -Z`; processus : `ps -efZ`
+ Rétablir bon contexte de sécurité : `restorecon <chemin/fichier>`
 + `-R` : récursif
+ Voir et comprendre les messages d'alerte SELinux : `sealert -a /var/log/audit/audit.log`
 + Avec résumé : `sealert -a /var/log/audit/audit.log | audit2allow`

### 5 Réseau

#### 5.1 Vérification

##### Hostname

`hostname`

##### Adresse IP/Masque

`ifconfig`
`ip address`

##### Passerelle par défaut

`route`
`ip route`

##### Adresse serveur DNS

`/etc/resolv.conf`

#### 5.2 Configuration

##### Hostname

`nmcli general hostname <nom machine>`

##### Profils ("connections")

+ Lister : `nmcli connection`
+ Liste pour une interface : `nmcli -f CONNECTIONS device show <interface>`
+ Détails : `nmcli connection show id <profil>`
+ Nouveau manuel : `nmcli connection add con-name <profil> ifname <interface> type ethernet ip 4 <adresse/masque> gw4 <passerelle> ip6 <adresse>`
+ Nouveau DHCP : `nmcli connection mod <profil> ipv4.dns "<serveurs DNS>"`
+ Nouveau DHCP IPv6 : `nmcli connection mod <profil> ipv6.dns "<serveurs DNS IPv6>"`
+ Modifier : `nmcli connection modify <profil> <paramètre> <valeur>`
+ Activer/désactiver : `nmcli connection up <profil>` ou `nmcli connection down <profil>`

##### Interfaces ("devices")

+ Liste : `nmcli device`
+ Détails actuels : `nmcli device show`
+ Activer/désactiver : `nmcli device connect <interface>` ou `nmcli device disconnect <interface>`
+ `/etc/sysconfig/network-scripts/ifcfg-<profil>`

#### 5.3 Diagnostic

`ping <adresse_IP_machine>`
`traceroute <adresse_IP_machine>`

+ Ports ouverts :

`netstat -anp --inet --inet6`
`ss -tlunp`

+ Détection d'OS : `nmap -O <adresse_IP_machine>`
+ Ports et service ouverts : `nmap -sV <adresse_IP_machine>`
+ Analyseur de paquets : `tcpdump`
+ Forgeur de paquet : `ncat`
+ Forgeur de paquet : `hping3`

#### 5.4 Routes

##### 5.4.1 Temporaire

`ip route add <reseau/masque> via <passerelle>`

Exemple : `ip route add 172.17.0.0/16 via 192.196.25.30`

##### 5.4.2 Permanent

`nmcli c modify <profil> +ipv4.routes "<reseau/masque> <passerelle>"`

#### 5.5 Pare-feu

##### firewalld

+ Afficher configuration
 + Zones actives (où sont les interfaces)

`firewall-cmd --get-active-zone`

 + Lister configuration par défaut

`firewall-cmd --get-zones`
`firewall-cmd --get-services`
`firewall-cmd --get-icmptypes`

 + Configuration détaillée pour une zone

`firewall-cmd --list-all --zone=<zone>`

 + Configuration détaillée pour tout

`firewall-cmd --list-all-zones`

+ Modifier zone par défaut

`firewall-cmd --set-default-zone=<zone>`

+ Mettre ou charger zone d'une interface

`firewall-cmd --zone=<zone> --add-interface=<interface>`
`firewall-cmd --zone=<zone> --remove-interface=<interface>`
`firewall-cmd --zone=<zone> --change-interface=<interface>`

+ Ajout/suppression autorisation service

`firewall-cmd --zone=<zone> --add-service=<service>`
`firewall-cmd --zone=<zone> --remove-service=<service>`

+ Ajout/suppression interdiction ICMP

`firewall-cmd --zone=<zone> --add-icmp-block=<icmptype>`
`firewall-cmd --zone=<zone> --remove-icmp-block=<icmptype>`

+ Ports et services non prédéfinis

`firewall-cmd --zone=<zone> --add-port=<port>/<protocol>`
`firewall-cmd --zone=<zone> --remove-port=<port>/<protocol>`

+ Modification définitive
 + Ajouter option `--permanent`

+ Redémarrer pare-feu
`firewall-cmd --reload`

+ Mode panique

`firewall-cmd --panic-on`
`firewall-cmd --panic-off`
`firewall-cmd --query-panic`

#### 5.6 SSH

+ Connection : `ssh login@machine` ou `ssh machine -l login`
+ Connection graphique : `-X`
+ Lancement de commande : `ssh utilisateur@machine '<commande>'`
+ Génération de clés utilisateurs
 + `ssh-keygen`
 + Clé privée : `~/.ssh/id_dsa`
 + Clé publique : `~/.ssh/id_dsa.pub`
 + Clé publique sur compte utilisateur du serveur dans `~/.ssh/authorized_keys`
+ Copie SCP
 + Local vers distant : `scp utilisateur@machine:</chemin/vers/fichier_distant>` 
 + Distant vers local : `scp utilisateur@machine:</chemin/vers/fichier_distant> </chemin/vers/fichier_local>`