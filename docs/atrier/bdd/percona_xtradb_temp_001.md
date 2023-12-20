---
title: Installation Percona XtraDB Cluster
description: 
published: true
date: 2023-04-21T10:53:05.930Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:52:59.546Z
---

====== Installation Percona XtraDB Cluster ======

===== Information =====
Ce cluster sera composé de 3 noeuds :

^  Serveur  ^  @IP  ^
|vm-bdd01|10.0.2.10|
|vm-bdd02|10.0.2.11|
|vm-bdd03|10.0.2.12|
|vm-master01|10.0.2.15|
|vm-master02|10.0.2.16|

===== Prérequis =====
  * Avoir un accès avec le compte ''root''
  * Ouverture des ports :
    * 3306
    * 4444
    * 4567
    * 4568
  * Avoir SELinux en mode "**permissive**"
<code>
sed -i s/^SELINUX=.*$/SELINUX=permissive/ /etc/selinux/config
setenforce 0
</code>

===== Installation du repositorie Percona =====

<code>yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm</code>

===== Installation du Package Percona-XtraDB-Cluster-57 =====

Sur les serveurs **vm-bdd01** / **vm-bdd02** / **vm-bdd03** :

<code>yum install -y Percona-XtraDB-Cluster-57</code>

Sur le serveur **vm-bdd01** :
  * Démarrage du service ''mysql'' :
<code>systemctl start mysql</code>

  * Récupération du mot de passe temporaire :

<code>grep 'temporary password' /var/log/mysqld.log</code>

  * Connexion à la console de MySQL avec le compte ''root'' et le mot de passe temporaire :

<code>mysql -u root -p</code>

  * Changement du mot de passe pour le compte ''root'' :

<code sql>
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'rootPass';
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
</code>

  * Arrêt du service ''mysql'' :

<code>systemctl stop mysql</code>

===== Configuration du cluster =====

Sur le serveur **vm-bdd01** :

Modification du fichier de configuration ''/etc/percona-xtradb-cluster.conf.d/wsrep.cnf'' :

<code>vim /etc/percona-xtradb-cluster.conf.d/wsrep.cnf</code>

Par défaut :

<code ini>
[mysqld]
# Path to Galera library
wsrep_provider=/usr/lib64/galera3/libgalera_smm.so

# Cluster connection URL contains IPs of nodes
#If no IP is found, this implies that a new cluster needs to be created,
#in order to do that you need to bootstrap this node
wsrep_cluster_address=gcomm://

# In order for Galera to work correctly binlog format should be ROW
binlog_format=ROW

# MyISAM storage engine has only experimental support
default_storage_engine=InnoDB

# Slave thread to use
wsrep_slave_threads= 8

wsrep_log_conflicts

# This changes how InnoDB autoincrement locks are managed and is a requirement for Galera
innodb_autoinc_lock_mode=2

# Node IP address
#wsrep_node_address=192.168.70.63
# Cluster name
wsrep_cluster_name=pxc-cluster

#If wsrep_node_name is not specified,  then system hostname will be used
wsrep_node_name=pxc-cluster-node-1

#pxc_strict_mode allowed values: DISABLED,PERMISSIVE,ENFORCING,MASTER
pxc_strict_mode=ENFORCING

# SST method
wsrep_sst_method=xtrabackup-v2

#Authentication for SST method
#wsrep_sst_auth="sstuser:s3cretPass"
</code>

  * Adapter le fichier de configuration en fonction de l'architecture :
<code ini>
[mysqld]
# Path to Galera library
wsrep_provider=/usr/lib64/galera3/libgalera_smm.so

# Cluster connection URL contains IPs of nodes
#If no IP is found, this implies that a new cluster needs to be created,
#in order to do that you need to bootstrap this node
wsrep_cluster_address=gcomm://10.0.2.10,10.0.2.11,10.0.2.12

# In order for Galera to work correctly binlog format should be ROW
binlog_format=ROW

# MyISAM storage engine has only experimental support
default_storage_engine=InnoDB

# Slave thread to use
wsrep_slave_threads= 8

wsrep_log_conflicts

# This changes how InnoDB autoincrement locks are managed and is a requirement for Galera
innodb_autoinc_lock_mode=2

# Node IP address
wsrep_node_address=10.0.2.10
# Cluster name
wsrep_cluster_name=cluster-mysql

#If wsrep_node_name is not specified,  then system hostname will be used
wsrep_node_name=vm-bdd01

#pxc_strict_mode allowed values: DISABLED,PERMISSIVE,ENFORCING,MASTER
pxc_strict_mode=ENFORCING

# SST method
wsrep_sst_method=xtrabackup-v2

#Authentication for SST method
wsrep_sst_auth="sstuser:passw0rd"
</code>

Sur le serveur **vm-bdd02** :
  * Copier le fichier de configuration et adapter les lignes suivantes :

<code ini>
wsrep_node_name=vm-bdd02
wsrep_node_address=10.0.2.11
</code>

Sur le serveur **vm-bdd03** :
  * Copier le fichier de configuration et adapter les lignes suivantes :

<code ini>
wsrep_node_name=vm-bdd03
wsrep_node_address=10.0.2.12
</code>

===== Amorçage du premier noeud (Bootstrapping) =====
Sur le serveur **vm-bdd01** :

  * On démarre le service ''mysql'' en mode ''bootstrapping'' :

<code>systemctl start mysql@bootstrap.service</code>

  * Se connecter à la console MySQL :

<code>
[root@vm-bdd01 ~]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.7.27-30-57-log Percona XtraDB Cluster (GPL), Release rel30, Revision 64987d4, WSREP version 31.39, wsrep_31.39

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
</code>

  * Contrôler que le cluster est bien initialisé :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+--------------------------------------+
| Variable_name                    | Value                                |
+----------------------------------+--------------------------------------+
| wsrep_local_state_uuid           | d022b8f0-0653-11ea-b8d9-3fc95b26f2f1 |
| ...                              | ...                                  |
| wsrep_local_state                | 4                                    |
| wsrep_local_state_comment        | Synced                               |
| ...                              | ...                                  |
| wsrep_cluster_size               | 1                                    |
| wsrep_cluster_status             | Primary                              |
| wsrep_connected                  | ON                                   |
| ...                              | ...                                  |
| wsrep_ready                      | ON                                   |
+----------------------------------+--------------------------------------+
71 rows in set (0.00 sec)

mysql>

</code>

La précédente sortie affiche que le cluster est constitué d'un node (''wsrep_cluster_size=1''), qu'il est le composant primaire (''wsrep_cluster_status=Primary'') avec un état synchronisé (''wsrep_local_state_comment=Synced'') et qu'il est prêt pour la réplication.
Avant d'ajouter d'autres nodes au nouveau cluster, il faut créer l'utilisateur for SST et lui affecter les droits nécessaire. Les identifiants doivent être les mêmes dans le fichier de configuration ''/etc/percona-xtradb-cluster.conf.d/wsrep.cnf''.

  * Ajouter l'utilisateur ''sstuser'' :

<code sql>
mysql> CREATE USER 'sstuser'@'localhost' IDENTIFIED BY 'passw0rd';
Query OK, 0 rows affected (0.00 sec)

mysql> GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT ON *.* TO 'sstuser'@'localhost';
Query OK, 0 rows affected (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.01 sec)
</code>

===== Ajout des nodes au cluster =====
Les nouveaux nœuds correctement configurés sont provisionnés automatiquement. Lorsque vous démarrez un nœud avec l'adresse déclaré dans la variable ''wsrep_cluster_address'', il rejoint automatiquement le cluster et se synchronise avec ce dernier.
<WRAP center round important 60%>
Ne pas démarrer plusieurs noeuds en même cas, car cela va entraîner une forte charge 
</WRAP>

Sur le serveur **vm-bdd02** :

  * Démarrer le service ''mysql'' :

<code>systemctl start mysql</code>

Après que le service soit démarrer, il va recevoir le SST automatiquement.

  * Se connecter à la console MySQL :

<code>mysql -u root -p</code>

  * Vérifier le statut :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+--------------------------------------------------+
| Variable_name                    | Value                                            |
+----------------------------------+--------------------------------------------------+
| wsrep_local_state_uuid           | d022b8f0-0653-11ea-b8d9-3fc95b26f2f1             |
| wsrep_local_state                | 4                                                |
| wsrep_local_state_comment        | Synced                                           |
| wsrep_cluster_size               | 2                                                |
| wsrep_cluster_status             | Primary                                          |
| wsrep_connected                  | ON                                               |
| wsrep_ready                      | ON                                               |
+----------------------------------+--------------------------------------------------+
71 rows in set (0.00 sec)

mysql>

</code>

Sur le serveur **vm-bdd03** :

  * Démarrer le service ''mysql'' :

<code>systemctl start mysql</code>

  * Se connecter à la console MySQL :

<code>mysql -u root -p</code>

  * Vérifier le statut :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+-------------------------------------------------+
| Variable_name                    | Value                                           |
+----------------------------------+-------------------------------------------------+
| wsrep_local_state_uuid           | d022b8f0-0653-11ea-b8d9-3fc95b26f2f1            |
| wsrep_local_state                | 4                                               |
| wsrep_local_state_comment        | Synced                                          |
| wsrep_cluster_size               | 3                                               |
| wsrep_cluster_status             | Primary                                         |
| wsrep_connected                  | ON                                              |
| wsrep_ready                      | ON                                              |
+----------------------------------+-------------------------------------------------+
71 rows in set (0.01 sec)

mysql>

</code>

===== Vérification de la synchronisation =====
Sur **vm-bdd02** :
<code sql>
mysql> CREATE DATABASE percona;
Query OK, 1 row affected (0.01 sec)
</code>

Sur **vm-bdd03** :
  * Connexion à la base ''percona'' et création d'une table ''example'' :
<code sql>
mysql> USE percona;
Database changed
mysql> CREATE TABLE example (node_id INT PRIMARY KEY, node_name VARCHAR(30));
Query OK, 0 rows affected (0.11 sec)
</code>

Sur **vm-bdd01** :
  * Redémarrage du service ''mysql'' (en mode normal) :
<code>systemctl stop mysql@bootstrap.service</code>
<code>systemctl start mysql</code>

  * Connexion à la console :
<code>mysql -u root -p</code>

  * Insertion d'un enregistrement dans la base :

<code sql>
mysql> INSERT INTO percona.example VALUES (1, 'percona1');
Query OK, 1 row affected (0.02 sec)

mysql>
</code>

Sur **vm-bdd02** :
  * Contrôle que l'enregistrement est accessible :
<code sql>
mysql> SELECT * FROM percona.example;
+---------+-----------+
| node_id | node_name |
+---------+-----------+
|       1 | percona1  |
+---------+-----------+
1 row in set (0.00 sec)
</code>

  * Activer au démarrage le service ''mysql'' (à faire sur les 3 serveurs) :
<code>systemctl is-enabled mysql</code>

===== Installation de ProxySQL2 (Percona) =====

Sur serveur **vm-master01** et **vm-master02** :

  * Installation du repo Percona :

<code>yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm</code>

  * Installation du Package Percona ''Percona-XtraDB-Cluster-client-57.x86_64'' et ''proxysql2''

<code>yum install -y Percona-XtraDB-Cluster-client-57.x86_64 proxysql2</code>

<WRAP center round important 80%>
Attention\\
Ne pas démarrer ProxySQL, avec les identifiants par défaut en production.\\
Avant de démarrer le service ''proxysql'', il faut modifier les variables contenant les identifiants admin dans le fichier ''/etc/proxysql.cnf''.
</WRAP>

  * Editer le fichier ''/etc/proxysql.cnf'' et changer la valeur du paramètre ''admin_credentials''

<code>vim /etc/proxysql.cnf</code>

==== Configuration automatique (proxysql-admin tool) ====

Le package ''proxysql2'' de Percona inclus l'outil ''proxysql-admin'' pour configurer les noeuds Percona XtrDB Cluster avec ProxySQL.
Avant d'utiliser l'outil ''proxysql-admin'', s'assurer que ProxySQL et que les noeuds Percona XtraDB Cluster qu'on veut ajouter sont démarrés. 

<WRAP center round important 80%>Important\\ L'outil ''proxysql-admin'' doit être utilisé que pour la configuration initiale de ProxySQL.</WRAP>

Voici les options disponibles avec l'outil ''proxysql-admin'' :

<code>
Usage: proxysql-admin [ options ]
Options:

--config-file=<config-file>        Read login credentials from a configuration file
                                   (command line options override any configuration file values)

--writer-hg=<number>               The hostgroup that all traffic will be sent to
                                   by default. Nodes that have 'read-only=0' in MySQL
                                   will be assigned to this hostgroup.
--backup-writer-hg=<number>        If the cluster has multiple nodes with 'read-only=0'
                                   and max_writers set, then additional nodes (in excess
                                   of max_writers), will be assigned to this hostgroup.
--reader-hg=<number>               The hostgroup that read traffic should be sent to.
                                   Nodes with 'read-only=0' in MySQL will be assigned
                                   to this hostgroup.
--offline-hg=<number>              Nodes that are determined to be OFFLINE will
                                   assigned to this hostgroup.

--proxysql-datadir=<datadir>       Specify the proxysql data directory location
--proxysql-username=<user_name>    ProxySQL service username
--proxysql-password[=<password>]   ProxySQL service password
--proxysql-port=<port_num>         ProxySQL service port number
--proxysql-hostname=<host_name>    ProxySQL service hostname

--cluster-username=<user_name>     Percona XtraDB Cluster node username
--cluster-password[=<password>]    Percona XtraDB Cluster node password
--cluster-port=<port_num>          Percona XtraDB Cluster node port number
--cluster-hostname=<host_name>     Percona XtraDB Cluster node hostname

--cluster-app-username=<user_name> Percona XtraDB Cluster node application username
--cluster-app-password[=<password>] Percona XtraDB Cluster node application passwrod
--without-cluster-app-user         Configure Percona XtraDB Cluster without application user

--monitor-username=<user_name>     Username for monitoring Percona XtraDB Cluster nodes through ProxySQL
--monitor-password[=<password>]    Password for monitoring Percona XtraDB Cluster nodes through ProxySQL
--use-existing-monitor-password    Do not prompt for a new monitor password if one is provided.

--node-check-interval=<NUMBER>     The interval at which the proxy should connect
                                   to the backend servers in order to monitor the
                                   Galera staus of a node (in milliseconds).
                                   (default: 5000)
--mode=[loadbal|singlewrite]       ProxySQL read/write configuration mode
                                   currently supporting: 'loadbal' and 'singlewrite'
                                   (default: 'singlewrite')
--write-node=<IPADDRESS>:<PORT>    Specifies the node that is to be used for
                                   writes for singlewrite mode.  If left unspecified,
                                   the cluster node is then used as the write node.
                                   This only applies when 'mode=singlewrite' is used.
--max-connections=<NUMBER>         Value for max_connections in the mysql_servers table.
                                   This is the maximum number of connections that
                                   ProxySQL will open to the backend servers.
                                   (default: 1000)
--max-transactions-behind=<NUMBER> Determines the maximum number of writesets a node
                                   can have queued before the node is SHUNNED to avoid
                                   stale reads.
                                   (default: 100)
--use-ssl=[yes|no]                 If set to 'yes', then connections between ProxySQL
                                   and the backend servers will use SSL.
                                   (default: no)
--writers-are-readers=[yes|no|backup]
                                   If set to 'yes', then all writers (backup-writers also)
                                   are added to the reader hostgroup.
                                   If set to 'no', then none of the writers (backup-writers also)
                                   will be added to the reader hostgroup.
                                   If set to 'backup', then only the backup-writers
                                   will be added to the reader hostgroup.
                                   (default: backup)
--remove-all-servers               When used with --update-cluster, this will remove all
                                   servers belonging to the current cluster before
                                   updating the list.
--debug                            Enables additional debug logging.
--help                             Dispalys this help text.

These options are the possible operations for proxysql-admin.
One of the options below must be provided.
--adduser                          Adds the Percona XtraDB Cluster application user to the ProxySQL database
--disable, -d                      Remove any Percona XtraDB Cluster configurations from ProxySQL
--enable, -e                       Auto-configure Percona XtraDB Cluster nodes into ProxySQL
--update-cluster                   Updates the cluster membership, adds new cluster nodes
                                   to the configuration.
--update-mysql-version             Updates the `mysql-server_version` variable in ProxySQL with the version
                                   from a node in the cluster.
--quick-demo                       Setup a quick demo with no authentication
--syncusers                        Sync user accounts currently configured in MySQL to ProxySQL
                                   May be used with --enable.
                                   (deletes ProxySQL users not in MySQL)
--sync-multi-cluster-users         Sync user accounts currently configured in MySQL to ProxySQL
                                   May be used with --enable.
                                   (doesn't delete ProxySQL users not in MySQL)
--add-query-rule                   Create query rules for synced mysql user. This is applicable only
                                   for singlewrite mode and works only with --syncusers
                                   and --sync-multi-cluster-users options
--is-enabled                       Checks if the current configuration is enabled in ProxySQL.
--status                           Returns a status report on the current configuration.
                                   If "--writer-hg=<NUM>" is specified, than the
                                   data corresponding to the galera cluster with that
                                   writer hostgroup is displayed. Otherwise, information
                                   for all clusters will be displayed.
--force                            This option will skip existing configuration checks in mysql_servers,
                                   mysql_users and mysql_galera_hostgroups tables. This option will only
                                   work with ``proxysql-admin --enable``.
--disable-updates                  Disable admin updates for ProxySQL cluster for the
                                   current operation. The default is to not change the
                                   admin variable settings.  If this option is specifed,
                                   these options will be set to false.
                                   (default: updates are not disabled)
--version, -v                      Prints the version info
</code>

==== Préparation du fichier de configuration ====

Pour initialiser la configuration de ProxySQL, on édite le fichier ''/etc/proxysql-admin.cnf''.

Voici le modèle par défaut :

<code ini>
# proxysql admin interface credentials.
export PROXYSQL_DATADIR='/var/lib/proxysql'
export PROXYSQL_USERNAME='admin'
export PROXYSQL_PASSWORD='admin'
export PROXYSQL_HOSTNAME='localhost'
export PROXYSQL_PORT='6032'

# PXC admin credentials for connecting to pxc-cluster-node.
export CLUSTER_USERNAME='admin'
export CLUSTER_PASSWORD='admin'
export CLUSTER_HOSTNAME='localhost'
export CLUSTER_PORT='3306'

# proxysql monitoring user. proxysql admin script will create this user in pxc to monitor pxc-nodes.
export MONITOR_USERNAME='monitor'
export MONITOR_PASSWORD='monit0r'

# Application user to connect to pxc-node through proxysql
export CLUSTER_APP_USERNAME='proxysql_user'
export CLUSTER_APP_PASSWORD='passw0rd'

# ProxySQL hostgroup IDs
export WRITER_HOSTGROUP_ID='10'
export READER_HOSTGROUP_ID='11'
export BACKUP_WRITER_HOSTGROUP_ID='12'
export OFFLINE_HOSTGROUP_ID='13'

# ProxySQL read/write configuration mode.
export MODE="singlewrite"

# max_connections default (used only when INSERTing a new mysql_servers entry)
export MAX_CONNECTIONS="1000"

# Determines the maximum number of writesets a node can have queued
# before the node is SHUNNED to avoid stale reads.
export MAX_TRANSACTIONS_BEHIND=100

# Connections to the backend servers (from ProxySQL) will use SSL
export USE_SSL="no"

# Determines if a node should be added to the reader hostgroup if it has
# been promoted to the writer hostgroup.
# If set to 'yes', then all writers (including backup-writers) are added to
# the read hostgroup.
# If set to 'no', then none of the writers (including backup-writers) are added.
# If set to 'backup', then only the backup-writers will be added to
# the read hostgroup.
export WRITERS_ARE_READERS="backup"

</code>

Voici le fichier de configuration pour notre architecture :

<code ini>
# proxysql admin interface credentials.
export PROXYSQL_DATADIR='/var/lib/proxysql'
export PROXYSQL_USERNAME='proxyadmin'
export PROXYSQL_PASSWORD='proxyadmin'
export PROXYSQL_HOSTNAME='localhost'
export PROXYSQL_PORT='6032'

# PXC admin credentials for connecting to pxc-cluster-node.
export CLUSTER_USERNAME='admin'
export CLUSTER_PASSWORD='admin'
export CLUSTER_HOSTNAME='localhost'
export CLUSTER_PORT='3306'

# proxysql monitoring user. proxysql admin script will create this user in pxc to monitor pxc-nodes.
export MONITOR_USERNAME='monitor'
export MONITOR_PASSWORD='monit0r'

# Application user to connect to pxc-node through proxysql
export CLUSTER_APP_USERNAME='proxysql_user'
export CLUSTER_APP_PASSWORD='passw0rd'

# ProxySQL hostgroup IDs
export WRITER_HOSTGROUP_ID='10'
export READER_HOSTGROUP_ID='11'
export BACKUP_WRITER_HOSTGROUP_ID='12'
export OFFLINE_HOSTGROUP_ID='13'

# ProxySQL read/write configuration mode.
export MODE="singlewrite"

# max_connections default (used only when INSERTing a new mysql_servers entry)
export MAX_CONNECTIONS="1000"

# Determines the maximum number of writesets a node can have queued
# before the node is SHUNNED to avoid stale reads.
export MAX_TRANSACTIONS_BEHIND=100

# Connections to the backend servers (from ProxySQL) will use SSL
export USE_SSL="no"

# Determines if a node should be added to the reader hostgroup if it has
# been promoted to the writer hostgroup.
# If set to 'yes', then all writers (including backup-writers) are added to
# the read hostgroup.
# If set to 'no', then none of the writers (including backup-writers) are added.
# If set to 'backup', then only the backup-writers will be added to
# the read hostgroup.
export WRITERS_ARE_READERS="backup"
</code>
