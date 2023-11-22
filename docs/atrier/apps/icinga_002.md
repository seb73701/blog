---
title: Installation et configuration de l'IDO (Icinga Data Output) MySQL
description: Application - Icinga
published: true
date: 2023-04-21T10:51:50.865Z
tags: monitoring, supervision, icinga, mysql, ido
editor: markdown
dateCreated: 2023-04-21T10:46:50.724Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis</span>

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Icinga installé

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation de l'IDO (Icinga Data Output) MySQL</span>
  
L'option IDO (Icinga Data Output) stocke toutes les configurations et les informations de statut dans une base de données.

Avant de configurer l'IDO, il faut installer MySQL.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de MySQL/MariaDB</span>

Voici la procédure d'installation de MariaDB : [Installer MariaDB Server sur Rocky 8](/bdd/mariadb/installer_mariadb_server_sur_rocky_8)

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de l'option "IDO MySQL"</span>

Il est nécessaire d'installer le package `icinga2-ido-mysql`

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install icinga2-ido-mysql
```

Création de la base de données `icinga` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```sql
mysql -u root -p

CREATE DATABASE icinga;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
quit
```

Après avoir créé la base de données, il faut importer le schéma :

```shell
mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql
```


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Activation de l'option "IDO MySQL"</span>

Lorsque le package est installé, la configuration de l'IDO se trouve dans le fichier `/etc/icinga2/features-available/ido-mysql.conf` dans lequel on peut configurer les identifiants de connexion.

Tous les attributs sont expliqués dans l'objet [IdoMysqlConnection](https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#objecttype-idomysqlconnection)

Décommentez les lignes puis mettre à jour les informations. 

<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/icinga2/features-enabled/ido-mysql.conf*</span>  
  
```js
/**
 * The IdoMysqlConnection type implements MySQL support
 * for DB IDO.
 */

object IdoMysqlConnection "ido-mysql" {
  user = "icinga"
  password = "icinga"
  host = "localhost"
  database = "icinga"
}
```

L'activation se fait par la commande :

```shell
icinga2 feature enable ido-mysql
Module 'ido-mysql' was enabled.
Make sure to restart Icinga 2 for these changes to take effect.
```

Rédémarrage d'Icinga 2 :

```shell
systemctl restart icinga2
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Documentation</span>
- https://icinga.com/docs/icinga-2/latest/doc/02-installation/05-CentOS/#ido-with-mysql