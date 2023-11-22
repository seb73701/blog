---
title: installation_d_icingaweb_2_sous_centos_8
description: 
published: true
date: 2023-04-21T10:47:14.624Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:10.644Z
---



# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Icinga 2 avec le backend IDO (MySQL ou PostgreSQL)
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Serveur Web (Apache ou Nginx)
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>PHP version >= 5.6.0
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Module Icinga PHP Librairy (ipl) (>= 0.6)
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Module Icinga PHP Thirdparty (>= 0.10)
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Les modules PHP suivants doivent être installés : `cURL`, `gettext`, `intl`, `mbstring`, `OpenSSL` et `xml`.
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Il faudra installer la librairie PHP LDAP si on utilise une authentification basé sur Active Directory ou LDAP.
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>La librairie PHP MySQL ou PostgreSQL en fonction de l'IDO.


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation d'Icinga Web 2

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Configuration du repo

Si Icinga 2 a était installé correctement, le repo est déjà enregistré. Dans le cas contraire, exécuter la commande suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
dnf install https://packages.icinga.com/epel/icinga-rpm-release-8-latest.noarch.rpm
```

> Les packages pour RHEL/CentOS dépendent d'autres packages qui sont distribués dans le cadre du référentiel EPEL.
>
> CentOS 8 a également besoin du référentiel `PowerTools` pour **EPEL** : sachez que le nom du référentiel `powertools` est passé en minuscule avec CentOS 8.3.2011 et versions ultérieures. Si vous utilisez CentOS 8.2.2044 ou une version antérieure, veuillez changer le nom en `PowerTools` dans la commande suivante.
{.is-warning}

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
dnf install 'dnf-command(config-manager)'
dnf config-manager --set-enabled powertools

dnf install epel-release
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation d'Icinga Web 2

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
dnf install icingaweb2 icingacli
```

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>SELinux

Si SELinux est activé, il faudra installer le package `icingaweb2-selinux`.

  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
dnf install icingaweb2-selinux
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation du serveur Web
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
yum install httpd

systemctl start httpd.service
systemctl enable httpd.service
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de PHP-FPM
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
dnf module list php
dnf module enable php:7.4 -y
dnf clean all
dnf info php-fpm.x86_64
dnf install php-fpm.x86_64
```

Modifier le port d'écouter (par défaut : socket) en éditant le fichier de configuration `/etc/php-fpm.d/www.conf`
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
vim /etc/php-fpm.d/www.conf
```

Editer aussi le fichier : `/etc/httpd/conf.d/php.conf` et modifier en fonction du socket ou du port.
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
vim /etc/httpd/conf.d/php.conf
``` 

Ensuite redémarrer le service Apache (httpd)
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
[root@icinga_master01 ~]# systemctl restart httpd
```
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
[root@icinga_master01 ~]# systemctl status php-fpm
● php-fpm.service - The PHP FastCGI Process Manager
   Loaded: loaded (/usr/lib/systemd/system/php-fpm.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
```
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
systemctl enable php-fpm.service

Created symlink /etc/systemd/system/multi-user.target.wants/php-fpm.service → /usr/lib/systemd/system/php-fpm.service.

systemctl start php-fpm.service
systemctl status php-fpm

● php-fpm.service - The PHP FastCGI Process Manager
   Loaded: loaded (/usr/lib/systemd/system/php-fpm.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2021-06-08 14:17:11 CEST; 4s ago
 Main PID: 5213 (php-fpm)
   Status: "Ready to handle connections"
    Tasks: 6 (limit: 24932)
   Memory: 8.9M
   CGroup: /system.slice/php-fpm.service
           ├─5213 php-fpm: master process (/etc/php-fpm.conf)
           ├─5214 php-fpm: pool www
           ├─5215 php-fpm: pool www
           ├─5216 php-fpm: pool www
           ├─5217 php-fpm: pool www
           └─5218 php-fpm: pool www

juin 08 14:17:11 icinga_master01 systemd[1]: Starting The PHP FastCGI Process Manager...
juin 08 14:17:11 icinga_master01 systemd[1]: Started The PHP FastCGI Process Manager.
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Génération du token

Pour générer le token, il faut utiliser la commande `icingacli` :
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
icingacli setup token create
```

Pour retrouver le dernier token généré, il suffit de lancer la commande :
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
icingacli setup token show
``` 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Démarrage du Wizard d'installation

Pour poursuivre l'installation, il faut lancer le navigateur sur l'url `http://domaine.com/icingaweb2/setup`.

On arrive donc sur cette page :

![icingaweb2_wizard_01.png](/assets/img/apps/icingaweb/icingaweb2_wizard_01.png)

Saisir le token généré précédemment et cliquer sur le bouton "**Next**".

Maintenant, il faut sélectionner les modules que l'on souhaite intégré à Icinga Web 2.

![icingaweb2_wizard_02.png](/assets/img/apps/icingaweb/icingaweb2_wizard_02.png)

Par défaut, seul "**Monitoring**" est activé car indispensable et cliquez sur le bouton "**Next**".

On arrive ensuite, sur la page des prérequis d'Icinga Web 2. Comme on peut le voir ci-dessous, si un élément est manquant ou mal installé, il sera surligné en jaune.

Dans notre exemple, on peut voir que le module PHP "Imagick" n'est pas installé. Il faut donc effectuer l'installation de ce dernier.

![icingaweb2_wizard_03.png](/assets/img/apps/icingaweb/icingaweb2_wizard_03.png)
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
yum install php74-php-pecl-imagick.x86_64
```

ou 
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```
yum install ImageMagick ImageMagick-devel ImageMagick-perl
```

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```
[root@icinga_master01 httpd]# echo extension=/opt/remi/php74/root/usr/lib64/php/modules/imagick.so >> /etc/php.ini

[root@icinga_master01 httpd]# systemctl restart php-fpm.service

[root@icinga_master01 httpd]# systemctl restart httpd.service

[root@icinga_master01 httpd]# php -i | grep -i ima

Protocols => dict, file, ftp, ftps, gopher, http, https, imap, imaps, ldap, ldaps, pop3, pop3s, rtsp, scp, sftp, smb, smbs, smtp, smtps, telnet, tftp
imagick
imagick module => enabled
imagick module version => 3.4.4
imagick classes => Imagick, ImagickDraw, ImagickPixel, ImagickPixelIterator, ImagickKernel
Imagick compiled with ImageMagick version => ImageMagick 6.9.12-1 Q16 x86_64 2021-02-21 https://imagemagick.org
Imagick using ImageMagick library version => ImageMagick 6.9.12-14 Q16 x86_64 2021-05-31 https://imagemagick.org
ImageMagick copyright => (C) 1999-2021 ImageMagick Studio LLC
ImageMagick release date => 2021-05-31
ImageMagick number of supported formats:  => 244
ImageMagick supported formats => 3FR, 3G2, 3GP, AAI, AI, APNG, ART, ARW, AVI, AVS, BGR, BGRA, BGRO, BIE, BMP, BMP2, BMP3, BRF, CAL, CALS, CANVAS, CAPTION, CIN, CIP, CLIP, CMYK, CMYKA, CR2, CR3, CRW, CUR, CUT, DATA, DCM, DCR, DCX, DDS, DFONT, DNG, DOT, DPX, DXT1, DXT5, EPDF, EPI, EPS, EPS2, EPS3, EPSF, EPSI, EPT, EPT2, EPT3, ERF, EXR, FAX, FILE, FITS, FRACTAL, FTP, FTS, G3, G4, GIF, GIF87, GRADIENT, GRAY, GRAYA, GROUP4, GV, H, HALD, HDR, HISTOGRAM, HRZ, HTM, HTML, HTTP, HTTPS, ICB, ICO, ICON, IIQ, INFO, INLINE, IPL, ISOBRL, ISOBRL6, J2C, J2K, JBG, JBIG, JNG, JNX, JP2, JPC, JPE, JPEG, JPG, JPM, JPS, JPT, JSON, K25, KDC, LABEL, M2V, M4V, MAC, MAGICK, MAP, MASK, MAT, MATTE, MEF, MIFF, MKV, MNG, MONO, MOV, MP4, MPC, MPG, MRW, MSL, MSVG, MTV, MVG, NEF, NRW, NULL, ORF, OTB, OTF, PAL, PALM, PAM, PANGO, PATTERN, PBM, PCD, PCDS, PCL, PCT, PCX, PDB, PDF, PDFA, PEF, PES, PFA, PFB, PFM, PGM, PGX, PICON, PICT, PIX, PJPEG, PLASMA, PNG, PNG00, PNG24, PNG32, PNG48, PNG64, PNG8, PNM, POCKETMOD, PPM, PREVIEW, PS, PS2, PS3, PSB, PSD, PTIF, PWP, RADIAL-GRADIENT, RAF, RAS, RAW, RGB, RGBA, RGBO, RGF, RLA, RLE, RMF, RW2, SCR, SCT, SFW, SGI, SHTML, SIX, SIXEL, SPARSE-COLOR, SR2, SRF, STEGANO, SUN, SVG, SVGZ, TEXT, TGA, THUMBNAIL, TIFF, TIFF64, TILE, TIM, TTC, TTF, TXT, UBRL, UBRL6, UIL, UYVY, VDA, VICAR, VID, VIDEO, VIFF, VIPS, VST, WBMP, WEBM, WEBP, WMF, WMV, WMZ, WPG, X, X3F, XBM, XC, XCF, XPM, XPS, XV, XWD, YCbCr, YCbCrA, YUV
imagick.locale_fix => 0 => 0
imagick.progress_monitor => 0 => 0
imagick.set_single_thread => 1 => 1
imagick.shutdown_sleep_count => 10 => 10
imagick.skip_version_check => 0 => 0

```


Après avoir installer le package "imagick", il faut restart le service PHP-FPM :
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
```shell
systemctl restart php-fpm.service
```

Puis, cliquer sur le bouton "Refresh" afin de recharger la page et valider que tous les prérequis sont installés.

![icingaweb2_wizard_04.png](/assets/img/apps/icingaweb/icingaweb2_wizard_04.png)

Cliquer sur le bouton "**Next**".

Maintenant, on doit choisir la méthode d'authentification (Database, LDAP ou external) :

![icingaweb2_wizard_05.png](/assets/img/apps/icingaweb/icingaweb2_wizard_05.png)

Cliquer sur le bouton "**Next**".

Sur la page "**Database Resource**", saisir les informations qui permettrons de stocké les informations des utilisateurs et des groupes.

![icingaweb2_wizard_06.png](/assets/img/apps/icingaweb/icingaweb2_wizard_06.png)

Pour vérifier que les informations soient cohérentes, il faut cliquer sur le bouton "Validate Configuration".

En cas de succès :

![icingaweb2_wizard_07.png](/assets/img/apps/icingaweb/icingaweb2_wizard_07.png)

Puis, on valide en cliquant sur le bouton "**Next**".

Si la base n'est pas créé, il sera demandé un compte utilisateur avec privilèges sur l'étape suivante :

![icingaweb2_wizard_08.png](/assets/img/apps/icingaweb/icingaweb2_wizard_08.png)

Et valider en cliquant sur le bouton "**Next**".

Si le compte n'a pas les droits nécessaires, il y aura un avertissement :

![icingaweb2_wizard_09.png](/assets/img/apps/icingaweb/icingaweb2_wizard_09.png)

Sinon, on arrive sur la page suivante :

![icingaweb2_wizard_10.png](/assets/img/apps/icingaweb/icingaweb2_wizard_10.png)

Il suffit de saisir le nom du backend puis de cliquer sur "**Next**".

Maintenant, il faut créer un compte administrateur :

![icingaweb2_wizard_11.png](/assets/img/apps/icingaweb/icingaweb2_wizard_11.png)

Compléter les informations et cliquer sur le bouton "**Next**"

Sur l'écran "Application Configuration", sélectionner les options nécessaires :

![icingaweb2_wizard_12.png](/assets/img/apps/icingaweb/icingaweb2_wizard_12.png)

Et valider en cliquant sur le bouton "**Next**".

On arrive ensuite sur la page récapitulative :

![icingaweb2_wizard_13.png](/assets/img/apps/icingaweb/icingaweb2_wizard_13.png)

Vérifier les informations et quand c'est bon, cliquer sur le bouton "**Next**"

On arrive maintenant sur la configuration de la partie supervision :

![icingaweb2_wizard_14.png](/assets/img/apps/icingaweb/icingaweb2_wizard_14.png)

Cliquer sur le bouton "**Next**".

![icingaweb2_wizard_15.png](/assets/img/apps/icingaweb/icingaweb2_wizard_15.png)

Contrôler que les informations correspondent bien et valider sur le bouton "**Next**".

![icingaweb2_wizard_16.png](/assets/img/apps/icingaweb/icingaweb2_wizard_16.png)

![icingaweb2_wizard_17.png](/assets/img/apps/icingaweb/icingaweb2_wizard_17.png)

![icingaweb2_wizard_18.png](/assets/img/apps/icingaweb/icingaweb2_wizard_18.png)

![icingaweb2_wizard_19.png](/assets/img/apps/icingaweb/icingaweb2_wizard_19.png)

![icingaweb2_wizard_20.png](/assets/img/apps/icingaweb/icingaweb2_wizard_20.png)

![icingaweb2_wizard_21.png](/assets/img/apps/icingaweb/icingaweb2_wizard_21.png)

![icingaweb2_wizard_22.png](/assets/img/apps/icingaweb/icingaweb2_wizard_22.png)

![icingaweb2_wizard_23.png](/assets/img/apps/icingaweb/icingaweb2_wizard_23.png)


