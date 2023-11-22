---
title: Installation Icinga2 (master) sur Rocky 8
description: Application - Icinga
published: true
date: 2023-04-21T10:47:01.256Z
tags: monitoring, supervision, icinga, rocky
editor: markdown
dateCreated: 2023-04-21T10:46:56.697Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis</span>

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Icinga utilise le port 5665, il est donc important d'ouvrir les accès dans les 2 sens (entrée / sortie).
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Les actions ci-dessous devront être exécutées avec le compte utilisateur système `root`.
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Il faut aussi avoir un système d'exploitation à jour.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Ajout du dépôt officiel Icinga</span>

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# rpm --import https://packages.icinga.com/icinga.key
[root@icinga-master01 ~]# wget https://packages.icinga.com/centos/ICINGA-release.repo -O /etc/yum.repos.d/ICINGA-release.repo
```

CentOS 8 a également besoin du référentiel PowerTools pour EPEL :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# dnf install 'dnf-command(config-manager)'
[root@icinga-master01 ~]# dnf config-manager --set-enabled powertools

[root@icinga-master01 ~]# dnf install epel-release
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation Icinga2</span>

> Les commandes ci-dessous doivent être exécutées avec le compte `root`.
{.is-warning}

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# dnf install icinga2
[root@icinga-master01 ~]# systemctl enable icinga2
[root@icinga-master01 ~]# systemctl start icinga2
```



## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>SELinux</span>

Icinga 2 fournit sa propre politique SELinux. Le package exécute la stratégie ciblée qui limite Icinga 2, y compris les fonctionnalités activées et les commandes en cours d'exécution.

> Si **SELINUX** est activé, il faudra obligatoirement installer le package `icinga2-selinux`
{.is-info}

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# dnf install icinga2-selinux
```

On retrouver plus d'informations sur le fonctionnement et les règles à cette adresse : https://icinga.com/docs/icinga2/latest/doc/22-selinux/#selinux


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Le service Systemd</span>

Durant l'installation d'icinga, le fichier **unit** de service Systemd est créé automatiquement. Si le service ne démarre pas, il faut vérifier que la configuration est correcte avec la commande :


<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# icinga2 deamon -C
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
[2022-02-20 17:45:03 +0100] information/cli: Icinga application loader (version: 2.13.2-1)
[2022-02-20 17:45:03 +0100] information/cli: Loading configuration file(s).
[2022-02-20 17:45:03 +0100] information/ConfigItem: Committing config item(s).
[2022-02-20 17:45:03 +0100] information/ApiListener: My API identity: icinga-master01.seb.fr
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 NotificationComponent.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 IdoMysqlConnection.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 CheckerComponent.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 User.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 UserGroup.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 3 ServiceGroups.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 3 TimePeriods.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 11 Services.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 3 Zones.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 ScheduledDowntime.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 2 NotificationCommands.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 2 HostGroups.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 12 Notifications.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 Downtime.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 IcingaApplication.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 Host.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 Endpoint.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 FileLogger.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 2 ApiUsers.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 244 CheckCommands.
[2022-02-20 17:45:03 +0100] information/ConfigItem: Instantiated 1 ApiListener.
[2022-02-20 17:45:03 +0100] information/ScriptGlobal: Dumping variables to file '/var/cache/icinga2/icinga2.vars'
[2022-02-20 17:45:03 +0100] information/cli: Finished validating the configuration file(s).
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation des plugins Nagios</span>

Ces plugins sont nécessaires pour qu'icinga puisse fonctionner immédiatement.

- [Package "nagios_plugins_all"](/supervision/package_nagios_plugins_all)



# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation de l'IDO (Icinga Data Output)</span>

Il est possible d'installer l'un des types d'IDO ci-dessous :
- MySQL : [Installation et configuration de l'IDO (Icinga Data Output) MySQL](/apps/icinga/installation_et_configuration_de_ido_mysql)
- PostgreSQL
- IcingaDB


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Configuration de l'API REST d'Icinga 2</span>

Icinga à besoin d'utilisateurs API pour communiquer via des requêtes HTTP vers d'autres modules ou plugins. Suivre la procédure ci-dessous pour créer les comptes nécessaires :

- [Configuration de l'API REST d'Icinga 2](apps/icinga/configuration_de_l_api_rest_icinga_2)

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Documentation</span>

- https://icinga.com/docs/icinga-2/latest/doc/02-installation/05-CentOS/
- https://icinga.com/docs/icinga-2/latest/doc/22-selinux/

