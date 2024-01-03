---
title: CentOS
description: 
published: true
date: 2023-04-21T11:14:05.334Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:41:54.145Z
---

![centos-logo-light.png](/assets/img/systeme_exploitation/centos/centos-logo-light.png =300x)

# Informations

+ *Site Officiel (en) : https://www.centos.org/*
+ *Documentation (en) : https://wiki.centos.org/Documentation*
+ *Forum (en) : https://www.centos.org/forums/*
+ *Changelog (en) : https://wiki.centos.org/Manuals/ReleaseNotes*

---

+ *Site Officiel (en) : https://www.redhat.com/*
+ *Site Officiel (fr) : https://www.redhat.com/fr*
+ *Documentation (en) : https://access.redhat.com/documentation/*
+ *Documentation (fr) : https://access.redhat.com/documentation/fr-fr/red_hat_enterprise_linux/8/*
+ *Changelog (en) : https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/8.2_release_notes/index*
+ *Portail Client (en) : https://access.redhat.com/*

# Liens divers
+ [CentOS - Liens divers](/systeme_exploitation/centos/liens_divers)


Fichier Kickstart CentOS 8 :

```ini
[root@icinga_master01 html]# cat ks.cfg
#version=RHEL8

# System bootloader configuration
bootloader --location=mbr --boot-drive=sda
autopart --type=lvm
zerombr
# Partition clearing information
clearpart --all --drives=sda
ignoredisk --only-use=sda


#ignoredisk --only-use=sda
#autopart --type=lvm
# Partition clearing information
#clearpart --none --initlabel
# Use graphical install
#graphical
text
# Use CDROM installation media
cdrom
# Keyboard layouts
keyboard --vckeymap=fr-oss --xlayouts='fr (oss)'
# System language
lang fr_FR.UTF-8

# Network information
network  --bootproto=static --device=enp0s3 --gateway=10.0.2.1 --ip=10.0.2.30 --nameserver=8.8.8.8,8.8.4.4 --netmask=255.255.255.0 --noipv6 --activate
network  --hostname=localhost.localdomain
repo --name="Minimal" --baseurl=file:///run/install/repo/Minimal
# Root password
rootpw --iscrypted $6$7u4i9888RfWTUVJB$Fns2NQLmxhq1jDY5C6TlHbPomJFv0hui8VgPhPweFt7uXdLXda3pUcUT03LWJrkUwXBsR51dOxlx3/MYYL22S/
# Run the Setup Agent on first boot
firstboot --enable
# Do not configure the X Window System
skipx
# System services
services --disabled="chronyd"
# System timezone
timezone Europe/Paris --isUtc --nontp

%packages
@^server-product-environment

%end

%addon com_redhat_kdump --disable --reserve-mb='auto'

%end

%anaconda
pwpolicy root --minlen=6 --minquality=1 --notstrict --nochanges --notempty
pwpolicy user --minlen=6 --minquality=1 --notstrict --nochanges --emptyok
pwpolicy luks --minlen=6 --minquality=1 --notstrict --nochanges --notempty
%end

```