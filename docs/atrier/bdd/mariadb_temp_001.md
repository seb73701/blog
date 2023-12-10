---
title: Installer MariaDB Server sur Rocky 8
description: Base de données
published: true
date: 2023-04-21T10:51:48.378Z
tags: bdd, mariadb
editor: markdown
dateCreated: 2023-04-21T10:51:44.356Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis</span>

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Les actions ci-dessous devront être exécutées avec le compte utilisateur système `root`.
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Il faut aussi avoir un système d'exploitation à jour.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation de MariaDB Server</span>

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell  
dnf install mariadb-server mariadb
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Activation du service au démarrage</span>

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell  
systemctl enable mariadb
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Démarrage du service mariadb (server)</span>

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell  
systemctl start mariadb
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Configuration de mariadb (server)</span>

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell  
mysql_secure_installation
```

```
Example:
localhost:# mysql_secure_installation 


NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!


In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.


Enter current password for root (enter for none): 
OK, successfully used password, moving on...


Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.


You already have a root password set, so you can safely answer 'n'.


Change the root password? [Y/n] n
 ... skipping.


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.


Remove anonymous users? [Y/n] y
 ... Success!


Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.


Disallow root login remotely? [Y/n] y
 ... Success!


By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.


Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!


Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.


Reload privilege tables now? [Y/n] y
 ... Success!


Cleaning up...


All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.


Thanks for using MariaDB!
```

Source : https://mariadb.com/kb/en/mysql_secure_installation/
