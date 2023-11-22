---
title: Erreurs de dépendances à l'installation du plugin nagios-plugins-all
description: 
published: true
date: 2023-06-10T06:54:25.705Z
tags: 
editor: markdown
dateCreated: 2023-06-10T06:54:25.705Z
---

# Erreurs de dépendances à l'installation du plugin nagios-plugins-all

## Description

Cette procédure décrit comment résoudre les problèmes de dépendances lors de l'installation du package "nagios-plugins-all".

## Prérequis

- Accès au serveur en SSH (client / infra)
- Récupérer le nom du package / librairies posant problème

## Rôle et Criticité

> **Attention, lors de l'installation de package(s), certains peuvent supprimé(s) ou mises à jour de certaine(s) dépendance(s). Merci d'être très vigilant et de bien lire avant de valider**
{.is-warning}

## Erreur : libmysqlclient.so.18(libmysqlclient_18)

Si vous rencontrer ce type de message sur un environnement CentOS 7 et avec MySQL Community d'installé en version 5.7.x :

```shell
Removing mariadb-libs.x86_64 1:5.5.35-3.el7 - u due to obsoletes from installed mysql-community-libs-5.7.29-1.el7.x86_64
--> Restarting Dependency Resolution with new changes.
--> Running transaction check
---> Package mariadb-libs.x86_64 1:5.5.35-3.el7 will be installed
--> Processing Dependency: libmysqlclient.so.18()(64bit) for package: nagios-plugins-mysql-2.3.3-2.el7.x86_64
--> Processing Dependency: libmysqlclient.so.18(libmysqlclient_18)(64bit) for package: nagios-plugins-mysql-2.3.3-2.el7.x86_64
--> Finished Dependency Resolution
Error: Package: nagios-plugins-mysql-2.3.3-2.el7.x86_64 (rhel7-epel-x86_64)
           Requires: libmysqlclient.so.18()(64bit)
Error: Package: nagios-plugins-mysql-2.3.3-2.el7.x86_64 (rhel7-epel-x86_64)
           Requires: libmysqlclient.so.18(libmysqlclient_18)(64bit)
You could try using --skip-broken to work around the problem
You could try running: rpm -Va --nofiles --nodigest
```

Cela signifie que la dépendance vers la librairie `libmysqlclient.so.18()` n'a pas pu être résolu. Cette librairie se trouve dans le package `mysql-community-libs-compat.x86_64` qui se trouve sur le repo **MySQL Community 5.7 RHEL7**.

 

La root cause étant :

- Le repo "MySQL Community 5.7 RHEL7" est soit désactivé, soit pas configuré
- Vérification de la présence du repo "MySQL Community 5.7 RHEL7" et si il est actif :

```
yum repolist all
```

ou

```
rhn-channel -l
```

Si il n'est pas activé, alors éditer le fichier repo qui se trouve normalement dans `/etc/yum.repos.d/custom`.

- Effectuer à nouveau l'installation du package "nagios-plugins-all" :

```
yum install nagios-plugins-all
```

Et normalement, vous ne devriez plus être coincé.