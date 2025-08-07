# LS

----

## <i class="fa-solid fa-hashtag"></i> Information

`ls` - Afficher le contenu de répertoires

Des alias sont fréquemment positionnés au sein des distributions courantes dans le fichier `~/.bashrc`.
C’est le cas de l’alias `ll` :

```shell
alias ll='ls -l --color=auto'
```

## MAN

- [ArchiLinux](https://man.archlinux.org/man/ls.1.fr)
- [Debian](https://manpages.debian.org/bullseye/manpages-fr/ls.1.fr.html)
- [Rocky](https://docs.rockylinux.org/fr/books/admin_guide/03-commands/#la-commande-ls)
- [Ubuntu](https://manpages.ubuntu.com/manpages/questing/fr/man1/ls.1.html)

----

## EXEMPLES

### Description des colonnes générées par l'exécution de la commande `ls -lia`

<details>

```shell
$ ls -lia /home
78489 drwx------ 4 rockstar rockstar 4096 25 oct. 08:10 rockstar
```

|Valeur|Information|
|:-----|:----------|
|`78489`|Numéro d’inode.|
|`drwx------`|Type de fichier (`d`) et droits (`rwx------`).|
|`4`|Nombre de sous-répertoires (`.` et `..` inclus). Pour un fichier, il représente le nombre de liens physiques et 1 représente lui-même.|
|`rockstar`|Propriété d'utilisateur.|
|`rockstar`|Appartenance de groupe.|
|`4096`|Pour les fichiers, il affiche la taille du fichier. Pour les répertoires, il montre la valeur fixe de 4096 octets occupée par le nom du fichier. Pour calculer la taille totale d'un répertoire, utilisez `du -sh rockstar/`|
|`25 oct. 08:10`|Date de dernière modification.|
|`rockstar`|Nom du fichier (ou du répertoire).|

</details>

----

### Lister les fichiers de `/etc` par ordre de dernière modification

<details>

```shell
$ ls -ltr /etc
total 1332
-rw-r--r--.  1 root root    662 29 may   2021 logrotate.conf
-rw-r--r--.  1 root root    272 17 may.   2021 mailcap
-rw-------.  1 root root    122 12 may.  2021 securetty
...
-rw-r--r--.  2 root root     85 18 may.  17:04 resolv.conf
-rw-r--r--.  1 root root     44 18 may.  17:04 adjtime
-rw-r--r--.  1 root root    283 18 may.  17:05 mtab
```

</details>

----

### Lister les fichiers de `/var` plus gros qu’un méga-octet mais moins qu’un giga-octets

<details>

L'exemple ici utilise des commandes avancées `grep` avec des expressions régulières.

```shell
$ ls -lhR /var/ | grep ^\- | grep -E "[1-9]*\.[0-9]*M" 
...
-rw-r--r--. 1 apache apache 1.2M 10 may.  13:02 XB RiyazBdIt.ttf
-rw-r--r--. 1 apache apache 1.2M 10 may.  1 apache apache 1,2M 10 may. 1 apache apache 1.1M 10 may.  1 apache apache 1.2M 10 may.
```

</details>

----

### Afficher les droits sur un dossier

<details>

Pour connaître les droits sur un dossier, dans notre exemple `/etc`, la commande suivante ne conviendrait pas :

```shell
$ ls -l /etc
total 1332
-rw-r--r--.  1 root root     44 18 nov.  17:04 adjtime
-rw-r--r--.  1 root root   1512 12 janv.  2010 aliases
-rw-r--r--.  1 root root  12288 17 nov.  17:41 aliases.db
drwxr-xr-x.  2 root root   4096 17 nov.  17:48 alternatives
...
```

La commande ci-dessus affichera par défaut le contenu du dossier. Pour le dossier lui-même, vous pouvez utiliser l'option `-d`.

```shell
ls -ld /etc
drwxr-xr-x. 69 root root 4096 18 nov.  17:05 /etc
```

</details>

----

### Trier par taille de fichier, la plus grande en premier

<details>

```shell
ls -lhS
```

</details>

----

### format heure/date avec `-l`

<details>

```shell
ls -l --time-style="+%Y-%m-%d %m-%d %H:%M" /
total 12378
dr-xr-xr-x. 2 root root 4096 2014-11-23 11-23 03:13 bin
dr-xr-xr-x. 5 root root 1024 2014-11-23 11-23 05:29 boot
```

</details>

---

### Ajouter le trailing slash à la fin des dossiers

<details>

Par défaut, la commande `ls` n’affiche pas le dernier slash d’un dossier. Dans certains cas, comme pour des scripts par exemple, il est utile de les afficher :

```shell
$ ls -dF /etc
/etc/
```

</details>

---

### Masquer certaines extensions

<details>

```shell
ls /etc --hide=*.conf
```

</details>

----

### Afficher les inodes

<details>

```shell
ls -i
```

</details>

----

### Afficher les propriétaires et les groupes en valeur numérique (par leur id, ici 0 et 0)

<details>

```shell
$ ls -ln /boot |head
total 126840
-rw-r--r-- 1 0 0   206361 nov.  11  2019 config-4.19.0-6-amd64
-rw-r--r-- 1 0 0   234587 déc.  31 16:19 config-5.9.0-0.bpo.5-amd64
drwx------ 3 0 0     4096 janv.  1  1970 efi
drwxr-xr-x 6 0 0     1024 janv. 26 11:33 grub
drwxr-xr-x 2 0 0     1024 févr. 11  2020 hd-media
-rw-r--r-- 1 0 0 45555288 août  29 18:37 initrd.img-4.19.0-6-amd64
-rw-r--r-- 1 0 0 51654794 févr.  1 10:24 initrd.img-5.9.0-0.bpo.5-amd64
-r-------- 1 0 0 16777216 mai   31  2019 nuc_buster_luksheader_BU
-r-------- 1 0 0  1052672 mai   31  2019 nuc_data_luksheader_BU
```

</details>

----

### Ne pas afficher les propriétaires (g) et/ou les groupes (G)

<details>

```shell
$ ls -lgG /boot |head
total 126840
-rw-r--r-- 1   206361 nov.  11  2019 config-4.19.0-6-amd64
-rw-r--r-- 1   234587 déc.  31 16:19 config-5.9.0-0.bpo.5-amd64
drwx------ 3     4096 janv.  1  1970 efi
drwxr-xr-x 6     1024 janv. 26 11:33 grub
drwxr-xr-x 2     1024 févr. 11  2020 hd-media
-rw-r--r-- 1 45555288 août  29 18:37 initrd.img-4.19.0-6-amd64
-rw-r--r-- 1 51654794 févr.  1 10:24 initrd.img-5.9.0-0.bpo.5-amd64
-r-------- 1 16777216 mai   31  2019 nuc_buster_luksheader_BU
-r-------- 1  1052672 mai   31  2019 nuc_data_luksheader_BU
```

</details>

----

### Afficher les tailles en format adopté à l'humain (b=octets, K=2\*\*10, M=2\*\*20,G=2\*\*30)

<details>

```shell
$ ls -lh /boot |head
total 124M
-rw-r--r-- 1 root root 202K nov.  11  2019 config-4.19.0-6-amd64
-rw-r--r-- 1 root root 230K déc.  31 16:19 config-5.9.0-0.bpo.5-amd64
drwx------ 3 root root 4,0K janv.  1  1970 efi
drwxr-xr-x 6 root root 1,0K janv. 26 11:33 grub
drwxr-xr-x 2 root root 1,0K févr. 11  2020 hd-media
-rw-r--r-- 1 root root  44M août  29 18:37 initrd.img-4.19.0-6-amd64
-rw-r--r-- 1 root root  50M févr.  1 10:24 initrd.img-5.9.0-0.bpo.5-amd64
-r-------- 1 root root  16M mai   31  2019 nuc_buster_luksheader_BU
-r-------- 1 root root 1,1M mai   31  2019 nuc_data_luksheader_BU
```

</details>

----

### Afficher les tailles selon une unité fixe, puissance de 2 (K=2\*\*10, M=2\*\*20,G=2\*\*30)

<details>

```shell
$ ls -l --block=M /boot |head
total 124M
-rw-r--r-- 1 root root  1M nov.  11  2019 config-4.19.0-6-amd64
-rw-r--r-- 1 root root  1M déc.  31 16:19 config-5.9.0-0.bpo.5-amd64
drwx------ 3 root root  1M janv.  1  1970 efi
drwxr-xr-x 6 root root  1M janv. 26 11:33 grub
drwxr-xr-x 2 root root  1M févr. 11  2020 hd-media
-rw-r--r-- 1 root root 44M août  29 18:37 initrd.img-4.19.0-6-amd64
-rw-r--r-- 1 root root 50M févr.  1 10:24 initrd.img-5.9.0-0.bpo.5-amd64
-r-------- 1 root root 16M mai   31  2019 nuc_buster_luksheader_BU
-r-------- 1 root root  2M mai   31  2019 nuc_data_luksheader_BU
```

</details>

----

### Afficher les tailles selon une unité fixe, puissance de 10 (KB,MB,GB)

<details>

```shell
$ ls -l --block=MB /boot |head
total 130MB
-rw-r--r-- 1 root root  1MB nov.  11  2019 config-4.19.0-6-amd64
-rw-r--r-- 1 root root  1MB déc.  31 16:19 config-5.9.0-0.bpo.5-amd64
drwx------ 3 root root  1MB janv.  1  1970 efi
drwxr-xr-x 6 root root  1MB janv. 26 11:33 grub
drwxr-xr-x 2 root root  1MB févr. 11  2020 hd-media
-rw-r--r-- 1 root root 46MB août  29 18:37 initrd.img-4.19.0-6-amd64
-rw-r--r-- 1 root root 52MB févr.  1 10:24 initrd.img-5.9.0-0.bpo.5-amd64
-r-------- 1 root root 17MB mai   31  2019 nuc_buster_luksheader_BU
-r-------- 1 root root  2MB mai   31  2019 nuc_data_luksheader_BU
```

</details>
