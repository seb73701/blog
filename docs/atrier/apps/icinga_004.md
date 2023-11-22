---
title: installation_icinga2_sur_centos_8
description: 
published: true
date: 2023-04-21T10:47:08.339Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:03.955Z
---

---
title: Installation Icinga2 sur CentOS 8
description: Application : Icinga 2
published: true
date: 2023-02-03T10:13:15.119Z
tags: beta, icinga, monitoring, supervision, nagios
editor: markdown
dateCreated: 2021-02-17T06:29:42.823Z
---

---
title: Installation Icinga2 sur CentOS 8
description: Application : Icinga 2
published: true
date: 2023-01-27T17:56:44.043Z
tags: beta, icinga, monitoring, supervision, nagios
editor: markdown
dateCreated: 2021-02-17T06:29:42.823Z
---

---
title: Installation Icinga2 sur CentOS 8
description: Application : Icinga 2
published: true
date: 2021-08-16T12:06:27.241Z
tags: beta, icinga, monitoring, supervision, nagios
editor: markdown
dateCreated: 2021-02-17T06:29:42.823Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Icinga utilise le port **5665**, il est donc important d'ouvrir les accès dans les 2 sens (entrée / sortie).
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Les actions ci-dessous devront être exécutées avec le compte utilisateur système **''root''**.
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Il faut aussi avoir un système d'exploitation à jour.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation du repo Icinga

L'installation du repo passe par la commande suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install https://packages.icinga.com/epel/icinga-rpm-release-8-latest.noarch.rpm
```

Il faut aussi installer le repo "**EPEL**" car Icinga2 installera des dépendances à partir de celui-ci. De plus, sur CentOS 8, il est nécessaire d'installer aussi le repo "**PowerTools**".

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install 'dnf-command(config-manager)'
dnf config-manager --set-enabled powertools
dnf install epel-release
```


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation Icinga2

Il faut installer le package :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install icinga2
```

Maintenant, il faut activer et démarrer le service :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
  
```shell
systemctl enable icinga2
systemctl start icinga2
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation du package "nagios-plugins-all"

- [Package "nagios_plugins_all"](/supervision/package_nagios_plugins_all)

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Le service Icinga2

Le service Icinga2 est géré par le service "**Systemd**" sous CentOS 8. Il permet d"effectuer les actions suivantes avec la commande `systemctl`.

| Commande            | Description      |
| ------------------- | ---------------- |
| **`start`**         |Démarre le daemon Icinga2.|
| **`stop`**          |Arrête le daemon Icinga2.|
| **`restart`**       |C'est un raccourci qui va arrêter (**`stop`**) le deamon Icinga2 puis le démarrer (**`start`**).
| **`reload`**        |L'action de rechargement envoie le signal `HUP` à Icinga 2 qui le fait redémarrer. Contrairement à l'action de redémarrage, le rechargement n'attend pas que Icinga 2 ait redémarré.|
| **`status`**        |Permet de vérifier si Icinga2 est démarré.|
| **`enable`**        |Permet d'activer le service au démarrage du système.|


Il peut y avoir des d'erreur lors du redémarrage du service :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
systemctl enable icinga2

systemctl restart icinga2
Job for icinga2.service failed. See 'systemctl status icinga2.service' and 'journalctl -xn' for details.
```

Dans la plupart des cas, il s'agît d'un problème de configuration. Pour vérifier, il suffit d'exécuter la commande :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
  
```shell
icinga2 daemon -C
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>SELinux

Icinga 2 fournit sa propre politique SELinux. `icinga2-selinuxest` un package de règles pour Red Hat Enterprise Linux 7 et ses dérivés. Le package exécute la stratégie ciblée qui limite Icinga 2, y compris les fonctionnalités activées et les commandes en cours d'exécution.


<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install icinga2-selinux
```

On retrouver plus d'informations sur le fonctionnement et les règles à cette adresse : https://icinga.com/docs/icinga2/latest/doc/22-selinux/#selinux.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation du package de coloration syntaxique Icinga2 pour Vim

Icinga fournit un fichier de modèle pour la coloration syntaxique pour Vim. Pour l'installer, il suffit d'exécuter la commande suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install vim-icinga2
```

S'assurer que la coloration syntaxique est activé :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
vim ~/.vimrc
```

Si l'option ci-dessous n'est pas présente, alors l'ajouter :

<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*~/.vimrc ou /root/.vimrc*</span>

```
syntax on
```


### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Template coloration syntaxique pour Vim "icinga2.vim"

Le fichier source est disponible à cette adresse : https://github.com/Icinga/icinga2/blob/master/tools/syntax/vim/syntax/icinga2.vim


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Préparation pour Icinga Web 2

Icinga 2 peut être utilisé avec Icinga Web 2 et une variété de modules.

La fonction DB IDO (Database Icinga Data Output) pour Icinga 2 s'occupe d'exporter toutes les informations de configuration et d'état dans une base de données.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Configuration de DB IDO MySQL

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation d'un serveur de base de données MySQL

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
dnf install mariadb-server mariadb
systemctl enable mariadb
systemctl start mariadb
mysql_secure_installation
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>  
  
```

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

Set root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

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

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation du module IDO MySQL ¶

La prochaine étape consiste à installer le package "icinga2-ido-mysql" depuis le gestionnaire de paquets du système.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
dnf install icinga2-ido-mysql
```

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Configuration de la base de données MySQL

#### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Configuration de la base de données pour Icinga 2

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
mysql -u root -p
```

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>    
  
```sql
CREATE DATABASE icinga;
GRANT SELECT, INSERT, UPDATE, DELETE, DROP, CREATE VIEW, INDEX, EXECUTE ON icinga.* TO 'icinga'@'localhost' IDENTIFIED BY 'icinga';
quit
```

Après avoir créé la base de données, on importe le schéma IDO Icinga 2 à l'aide de la commande suivante. Saisir le mot de passe "root" quand le prompt le demande.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>    
  
```shell
mysql -u root -p icinga < /usr/share/icinga2-ido-mysql/schema/mysql.sql
```

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Activer le module IDO MySQL


Le paquet fournit un nouveau fichier de configuration qui est installé dans `/etc/icinga2/features-available/ido-mysql.conf`. On peut mettre à jour les informations d'identification de la base de données dans ce fichier.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>    
  
```shell
vim /etc/icinga2/features-enabled/ido-mysql.conf
```

Décommentez les lignes puis mettre à jour les informations. 

Exemple : 

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

Il faut redémarrer le service `icinga2` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```
systemctl restart icinga2
```

> Tous les attributs disponibles sont expliqués dans la documentation en ligne sur l'objet [`IdoMysqlConnection`](https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#idomysqlconnection).
{.is-info}

On peut activer la fonctionnalité `ido-mysql` à l'aide de la commande `icinga2` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span> 
  
```
icinga2 feature enable ido-mysql
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Module 'ido-mysql' was enabled.
Make sure to restart Icinga 2 for these changes to take effect.
Restart Icinga 2.
```

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span> 

```shell
systemctl restart icinga2
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Serveur Web

La meilleure façon d'installer Icinga Web 2 est d'utiliser Apache comme serveur Web en combinaison avec PHP-FPM.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
yum install httpd
systemctl enable httpd
systemctl start httpd
```

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Règles Firewall


Activez le port **80 (http)**. La meilleure pratique consiste à activer uniquement le port **443 (https)** et à utiliser des certificats TLS.

#### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>firewall-cmd

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
firewall-cmd --add-service=http
firewall-cmd --permanent --add-service=http
```

#### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>iptables

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
service iptables save
```

### <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Configuration de l'API REST Icinga 2

Icinga Web 2 et d'autres interfaces Web nécessitent l'[API REST](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-setup) pour envoyer des actions (replanifier la vérification, etc.) et interroger les détails de l'objet.

Vous pouvez exécuter la commande CLI `icinga2 api setup` pour activer la [fonction `api`](https://icinga.com/docs/icinga-2/latest/doc/11-cli-commands/#enable-features) et configurer des certificats ainsi qu'un nouvel utilisateur API "**root**" avec un mot de passe généré automatiquement dans le fichier de configuration `/etc/icinga2/conf.d/api-users.conf` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
icinga2 api setup
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>
```
information/cli: Generating new CA.
information/base: Writing private key to '/var/lib/icinga2/ca//ca.key'.
information/base: Writing X509 certificate to '/var/lib/icinga2/ca//ca.crt'.
information/cli: Generating new CSR in '/var/lib/icinga2/certs//icinga_master01.csr'.
information/base: Writing private key to '/var/lib/icinga2/certs//icinga_master01.key'.
information/base: Writing certificate signing request to '/var/lib/icinga2/certs//icinga_master01.csr'.
information/cli: Signing CSR with CA and writing certificate to '/var/lib/icinga2/certs//icinga_master01.crt'.
information/pki: Writing certificate to file '/var/lib/icinga2/certs//icinga_master01.crt'.
information/cli: Copying CA certificate to '/var/lib/icinga2/certs//ca.crt'.
information/cli: Adding new ApiUser 'root' in '/etc/icinga2/conf.d/api-users.conf'.
information/cli: Reading '/etc/icinga2/icinga2.conf'.
information/cli: Enabling the 'api' feature.
Enabling feature api. Make sure to restart Icinga 2 for these changes to take effect.
information/cli: Updating 'NodeName' constant in '/etc/icinga2/constants.conf'.
information/cli: Created backup file '/etc/icinga2/constants.conf.orig'.
information/cli: Updating 'ZoneName' constant in '/etc/icinga2/constants.conf'.
information/cli: Backup file '/etc/icinga2/constants.conf.orig' already exists. Skipping backup.
Done.

Now restart your Icinga 2 daemon to finish the installation!
```

Edit the api-users.conf file and add a new ApiUser object. Specify the permissions attribute with minimal permissions required by Icinga Web 2.

Editer le fichier `api-users.conf` et ajouter à un nouvel objet [ApiUser](https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#apiuser) Spécifier les attributs de [permissions](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-permissions) avec les autorisations minimales requises par Icinga Web 2.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
vim /etc/icinga2/conf.d/api-users.conf
```
<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/icinga2/conf.d/api-users.conf*</span>
```json
object ApiUser "icingaweb2" {
  password = "Wijsn8Z9eRs5E25d"
  permissions = [ "status/query", "actions/*", "objects/modify/*", "objects/query/*" ]
}

Restart Icinga 2 to activate the configuration.
```

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>     
  
```shell
systemctl restart icinga2
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Sources

- https://icinga.com/docs/icinga-2/latest/doc/02-installation/