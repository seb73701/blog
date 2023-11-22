---
title: IcingaWeb2 - IDO via ProxySQL
description: 
published: true
date: 2023-04-21T10:47:54.194Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:49.024Z
---

====== IcingaWeb2 - IDO via ProxySQL ======
----
===== Installation de Percona XtraDB Cluster =====
Prérequis :

  * Ouverture firewall entre myserver-l-bdd0[1-3] sur les ports 3306/4444/4567/4568
  * SELinux en permissive 
<code>
setenforce 0
# ou
vi /etc/selinux/config
</code>

  * Installation du repo publique Percona
<code>yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm</code>

  * Déplacement du fichier repo dans le dossier custom de ''Yum'' :
<code>mv /etc/yum.repos.d/percona-original-release.repo /etc/yum.repos.d/custom/</code>

  * Installation du Package :
<code>yum install -y Percona-XtraDB-Cluster-57</code>

  * Démarrage du service :
<code>systemctl start mysql</code>

  * Récupération du mot de passe temporaire :
<code>grep 'temporary password' /var/log/mysqld.log</code>

  * Connexion à MySQL :
<code>mysql -u root -p</code>

  * Changement du mot de passe du compte ''root'' :
<code>
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'rootPass';
Query OK, 0 rows affected (0.00 sec)

mysql> exit
Bye
</code>

  * Démarrage du service MySQL :
<code>systemctl start mysql</code>

  * Edition du fichier de configuration du Cluster :
<code>vim /etc/percona-xtradb-cluster.conf.d/wsrep.cnf</code>

<code ini>
[mysqld]
# Path to Galera library
wsrep_provider=/usr/lib64/galera3/libgalera_smm.so

# Cluster connection URL contains IPs of nodes
#If no IP is found, this implies that a new cluster needs to be created,
#in order to do that you need to bootstrap this node
wsrep_cluster_address=gcomm://10.30.0.1,10.30.0.2,10.30.0.3

# In order for Galera to work correctly binlog format should be ROW
binlog_format=ROW

# MyISAM storage engine has only experimental support
default_storage_engine=InnoDB

# Slave thread to use
#wsrep_slave_threads= 8

#wsrep_log_conflicts

# This changes how InnoDB autoincrement locks are managed and is a requirement for Galera
innodb_autoinc_lock_mode=2

# Node IP address
wsrep_node_address=10.30.0.1
# Cluster name
wsrep_cluster_name=cluster-test

#If wsrep_node_name is not specified,  then system hostname will be used
wsrep_node_name=myserver-l-bdd01

#pxc_strict_mode allowed values: DISABLED,PERMISSIVE,ENFORCING,MASTER
pxc_strict_mode=ENFORCING

# SST method
wsrep_sst_method=xtrabackup-v2

#Authentication for SST method
#wsrep_sst_auth="sstuser:s3cretPass"
</code>

  * Installer sur **myserver-l-bdd02** et **myserver-l-bdd03**
  * Configurer le fichier ''/etc/percona-xtradb-cluster.conf.d/wsrep.cnf'' :

Sur **myserver-l-bdd02** :
<code ini>
# Node IP address
wsrep_node_address=10.30.0.2
#If wsrep_node_name is not specified,  then system hostname will be used
wsrep_node_name=myserver-l-bdd02
</code>

Sur **myserver-l-bdd03** :
<code ini>
# Node IP address
wsrep_node_address=10.30.0.3
#If wsrep_node_name is not specified,  then system hostname will be used
wsrep_node_name=myserver-l-bdd03
</code>

Sur **myserver-l-bdd01** :

  * Démarrage de MySQL en mode ''Bootstap'' :
<code>systemctl start mysql@bootstrap.service</code>

  * Connexion à la console MySQL :
<code>mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.27-30-57-log Percona XtraDB Cluster (GPL), Release rel30, Revision 64987d4, WSREP version 31.39, wsrep_31.39

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
</code>

  * Contrôle des variables :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+--------------------------------------+
| Variable_name                    | Value                                |
+----------------------------------+--------------------------------------+
| wsrep_local_state_uuid           | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d |
| wsrep_protocol_version           | 9                                    |
| wsrep_last_applied               | 1                                    |
| wsrep_last_committed             | 1                                    |
| wsrep_replicated                 | 0                                    |
| wsrep_replicated_bytes           | 0                                    |
| wsrep_repl_keys                  | 0                                    |
| wsrep_repl_keys_bytes            | 0                                    |
| wsrep_repl_data_bytes            | 0                                    |
| wsrep_repl_other_bytes           | 0                                    |
| wsrep_received                   | 2                                    |
| wsrep_received_bytes             | 151                                  |
| wsrep_local_commits              | 0                                    |
| wsrep_local_cert_failures        | 0                                    |
| wsrep_local_replays              | 0                                    |
| wsrep_local_send_queue           | 0                                    |
| wsrep_local_send_queue_max       | 1                                    |
| wsrep_local_send_queue_min       | 0                                    |
| wsrep_local_send_queue_avg       | 0.000000                             |
| wsrep_local_recv_queue           | 0                                    |
| wsrep_local_recv_queue_max       | 2                                    |
| wsrep_local_recv_queue_min       | 0                                    |
| wsrep_local_recv_queue_avg       | 0.500000                             |
| wsrep_local_cached_downto        | 0                                    |
| wsrep_flow_control_paused_ns     | 0                                    |
| wsrep_flow_control_paused        | 0.000000                             |
| wsrep_flow_control_sent          | 0                                    |
| wsrep_flow_control_recv          | 0                                    |
| wsrep_flow_control_interval      | [ 100, 100 ]                         |
| wsrep_flow_control_interval_low  | 100                                  |
| wsrep_flow_control_interval_high | 100                                  |
| wsrep_flow_control_status        | OFF                                  |
| wsrep_cert_deps_distance         | 0.000000                             |
| wsrep_apply_oooe                 | 0.000000                             |
| wsrep_apply_oool                 | 0.000000                             |
| wsrep_apply_window               | 0.000000                             |
| wsrep_commit_oooe                | 0.000000                             |
| wsrep_commit_oool                | 0.000000                             |
| wsrep_commit_window              | 0.000000                             |
| wsrep_local_state                | 4                                    |
| wsrep_local_state_comment        | Synced                               |
| wsrep_cert_index_size            | 0                                    |
| wsrep_cert_bucket_count          | 22                                   |
| wsrep_gcache_pool_size           | 1320                                 |
| wsrep_causal_reads               | 0                                    |
| wsrep_cert_interval              | 0.000000                             |
| wsrep_open_transactions          | 0                                    |
| wsrep_open_connections           | 0                                    |
| wsrep_ist_receive_status         |                                      |
| wsrep_ist_receive_seqno_start    | 0                                    |
| wsrep_ist_receive_seqno_current  | 0                                    |
| wsrep_ist_receive_seqno_end      | 0                                    |
| wsrep_incoming_addresses         | 10.30.0.1:3306                      |
| wsrep_cluster_weight             | 1                                    |
| wsrep_desync_count               | 0                                    |
| wsrep_evs_delayed                |                                      |
| wsrep_evs_evict_list             |                                      |
| wsrep_evs_repl_latency           | 0/0/0/0/0                            |
| wsrep_evs_state                  | OPERATIONAL                          |
| wsrep_gcomm_uuid                 | c16715cf-0208-11ea-a624-861076477205 |
| wsrep_cluster_conf_id            | 1                                    |
| wsrep_cluster_size               | 1                                    |
| wsrep_cluster_state_uuid         | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d |
| wsrep_cluster_status             | Primary                              |
| wsrep_connected                  | ON                                   |
| wsrep_local_bf_aborts            | 0                                    |
| wsrep_local_index                | 0                                    |
| wsrep_provider_name              | Galera                               |
| wsrep_provider_vendor            | Codership Oy <info@codership.com>    |
| wsrep_provider_version           | 3.39(rb3295e6)                       |
| wsrep_ready                      | ON                                   |
+----------------------------------+--------------------------------------+
71 rows in set (0.00 sec)

mysql>
</code>
  * Création d'un utilisateur dédiée pour la réplication/backup :

<code sql>
mysql@myserver-l-bdd01> CREATE USER 'sstuser'@'localhost' IDENTIFIED BY 'passw0rd';
mysql@myserver-l-bdd01> GRANT RELOAD, LOCK TABLES, PROCESS, REPLICATION CLIENT ON *.* TO 'sstuser'@'localhost';
mysql@myserver-l-bdd01> FLUSH PRIVILEGES;
</code>

  * Ajouter les identifiants pour le compte ''sstuser'' sur les 3 serveurs :
<code>vi /etc/percona-xtradb-cluster.conf.d/wsrep.cnf</code>

  * Redémarrage du service "mysql" (toujours resté en mode bootstrap pour la configuration) :
<code>systemctl restart  mysql@bootstrap.service</code>

Sur **myserver-l-bdd02** :
  * Démarrage du service ''mysql'' :
<code>systemctl start mysql</code>

  * Connexion à la console :
<code>
mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.27-30-57-log Percona XtraDB Cluster (GPL), Release rel30, Revision 64987d4, WSREP version 31.39, wsrep_31.39

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
</code>

  * Contrôle des variables :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+-------------------------------------------------+
| Variable_name                    | Value                                           |
+----------------------------------+-------------------------------------------------+
| wsrep_local_state_uuid           | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d            |
| wsrep_protocol_version           | 9                                               |
| wsrep_last_applied               | 4                                               |
| wsrep_last_committed             | 4                                               |
| wsrep_replicated                 | 0                                               |
| wsrep_replicated_bytes           | 0                                               |
| wsrep_repl_keys                  | 0                                               |
| wsrep_repl_keys_bytes            | 0                                               |
| wsrep_repl_data_bytes            | 0                                               |
| wsrep_repl_other_bytes           | 0                                               |
| wsrep_received                   | 4                                               |
| wsrep_received_bytes             | 539                                             |
| wsrep_local_commits              | 0                                               |
| wsrep_local_cert_failures        | 0                                               |
| wsrep_local_replays              | 0                                               |
| wsrep_local_send_queue           | 0                                               |
| wsrep_local_send_queue_max       | 1                                               |
| wsrep_local_send_queue_min       | 0                                               |
| wsrep_local_send_queue_avg       | 0.000000                                        |
| wsrep_local_recv_queue           | 0                                               |
| wsrep_local_recv_queue_max       | 1                                               |
| wsrep_local_recv_queue_min       | 0                                               |
| wsrep_local_recv_queue_avg       | 0.000000                                        |
| wsrep_local_cached_downto        | 0                                               |
| wsrep_flow_control_paused_ns     | 0                                               |
| wsrep_flow_control_paused        | 0.000000                                        |
| wsrep_flow_control_sent          | 0                                               |
| wsrep_flow_control_recv          | 0                                               |
| wsrep_flow_control_interval      | [ 173, 173 ]                                    |
| wsrep_flow_control_interval_low  | 173                                             |
| wsrep_flow_control_interval_high | 173                                             |
| wsrep_flow_control_status        | OFF                                             |
| wsrep_cert_deps_distance         | 0.000000                                        |
| wsrep_apply_oooe                 | 0.000000                                        |
| wsrep_apply_oool                 | 0.000000                                        |
| wsrep_apply_window               | 0.000000                                        |
| wsrep_commit_oooe                | 0.000000                                        |
| wsrep_commit_oool                | 0.000000                                        |
| wsrep_commit_window              | 0.000000                                        |
| wsrep_local_state                | 4                                               |
| wsrep_local_state_comment        | Synced                                          |
| wsrep_cert_index_size            | 0                                               |
| wsrep_cert_bucket_count          | 22                                              |
| wsrep_gcache_pool_size           | 1592                                            |
| wsrep_causal_reads               | 0                                               |
| wsrep_cert_interval              | 0.000000                                        |
| wsrep_open_transactions          | 0                                               |
| wsrep_open_connections           | 0                                               |
| wsrep_ist_receive_status         |                                                 |
| wsrep_ist_receive_seqno_start    | 0                                               |
| wsrep_ist_receive_seqno_current  | 0                                               |
| wsrep_ist_receive_seqno_end      | 0                                               |
| wsrep_incoming_addresses         | 10.30.0.1:3306,10.30.0.2:3306,10.30.0.3:3306 |
| wsrep_cluster_weight             | 3                                               |
| wsrep_desync_count               | 0                                               |
| wsrep_evs_delayed                |                                                 |
| wsrep_evs_evict_list             |                                                 |
| wsrep_evs_repl_latency           | 0/0/0/0/0                                       |
| wsrep_evs_state                  | OPERATIONAL                                     |
| wsrep_gcomm_uuid                 | b759d1a0-020b-11ea-a8de-cfda76160d07            |
| wsrep_cluster_conf_id            | 3                                               |
| wsrep_cluster_size               | 3                                               |
| wsrep_cluster_state_uuid         | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d            |
| wsrep_cluster_status             | Primary                                         |
| wsrep_connected                  | ON                                              |
| wsrep_local_bf_aborts            | 0                                               |
| wsrep_local_index                | 1                                               |
| wsrep_provider_name              | Galera                                          |
| wsrep_provider_vendor            | Codership Oy <info@codership.com>               |
| wsrep_provider_version           | 3.39(rb3295e6)                                  |
| wsrep_ready                      | ON                                              |
+----------------------------------+-------------------------------------------------+
71 rows in set (0.01 sec)

mysql>
</code>

Sur **myserver-l-bdd03** :
  * Démarrage du service ''mysql'' :
<code>systemctl start mysql</code>

  * Connexion à la console :
<code>
mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.27-30-57-log Percona XtraDB Cluster (GPL), Release rel30, Revision 64987d4, WSREP version 31.39, wsrep_31.39

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
</code>

  * Contrôle des variables :

<code sql>
mysql> show status like 'wsrep%';
+----------------------------------+-------------------------------------------------+
| Variable_name                    | Value                                           |
+----------------------------------+-------------------------------------------------+
| wsrep_local_state_uuid           | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d            |
| wsrep_protocol_version           | 9                                               |
| wsrep_last_applied               | 4                                               |
| wsrep_last_committed             | 4                                               |
| wsrep_replicated                 | 0                                               |
| wsrep_replicated_bytes           | 0                                               |
| wsrep_repl_keys                  | 0                                               |
| wsrep_repl_keys_bytes            | 0                                               |
| wsrep_repl_data_bytes            | 0                                               |
| wsrep_repl_other_bytes           | 0                                               |
| wsrep_received                   | 3                                               |
| wsrep_received_bytes             | 317                                             |
| wsrep_local_commits              | 0                                               |
| wsrep_local_cert_failures        | 0                                               |
| wsrep_local_replays              | 0                                               |
| wsrep_local_send_queue           | 0                                               |
| wsrep_local_send_queue_max       | 1                                               |
| wsrep_local_send_queue_min       | 0                                               |
| wsrep_local_send_queue_avg       | 0.000000                                        |
| wsrep_local_recv_queue           | 0                                               |
| wsrep_local_recv_queue_max       | 1                                               |
| wsrep_local_recv_queue_min       | 0                                               |
| wsrep_local_recv_queue_avg       | 0.000000                                        |
| wsrep_local_cached_downto        | 0                                               |
| wsrep_flow_control_paused_ns     | 0                                               |
| wsrep_flow_control_paused        | 0.000000                                        |
| wsrep_flow_control_sent          | 0                                               |
| wsrep_flow_control_recv          | 0                                               |
| wsrep_flow_control_interval      | [ 173, 173 ]                                    |
| wsrep_flow_control_interval_low  | 173                                             |
| wsrep_flow_control_interval_high | 173                                             |
| wsrep_flow_control_status        | OFF                                             |
| wsrep_cert_deps_distance         | 0.000000                                        |
| wsrep_apply_oooe                 | 0.000000                                        |
| wsrep_apply_oool                 | 0.000000                                        |
| wsrep_apply_window               | 0.000000                                        |
| wsrep_commit_oooe                | 0.000000                                        |
| wsrep_commit_oool                | 0.000000                                        |
| wsrep_commit_window              | 0.000000                                        |
| wsrep_local_state                | 4                                               |
| wsrep_local_state_comment        | Synced                                          |
| wsrep_cert_index_size            | 0                                               |
| wsrep_cert_bucket_count          | 22                                              |
| wsrep_gcache_pool_size           | 1456                                            |
| wsrep_causal_reads               | 0                                               |
| wsrep_cert_interval              | 0.000000                                        |
| wsrep_open_transactions          | 0                                               |
| wsrep_open_connections           | 0                                               |
| wsrep_ist_receive_status         |                                                 |
| wsrep_ist_receive_seqno_start    | 0                                               |
| wsrep_ist_receive_seqno_current  | 0                                               |
| wsrep_ist_receive_seqno_end      | 0                                               |
| wsrep_incoming_addresses         | 10.30.0.1:3306,10.30.0.2:3306,10.30.0.3:3306 |
| wsrep_cluster_weight             | 3                                               |
| wsrep_desync_count               | 0                                               |
| wsrep_evs_delayed                |                                                 |
| wsrep_evs_evict_list             |                                                 |
| wsrep_evs_repl_latency           | 0/0/0/0/0                                       |
| wsrep_evs_state                  | OPERATIONAL                                     |
| wsrep_gcomm_uuid                 | e55bfae9-020b-11ea-b471-e2a1941d94bb            |
| wsrep_cluster_conf_id            | 3                                               |
| wsrep_cluster_size               | 3                                               |
| wsrep_cluster_state_uuid         | 92d40cd1-0204-11ea-8ee5-9e4763eaa74d            |
| wsrep_cluster_status             | Primary                                         |
| wsrep_connected                  | ON                                              |
| wsrep_local_bf_aborts            | 0                                               |
| wsrep_local_index                | 2                                               |
| wsrep_provider_name              | Galera                                          |
| wsrep_provider_vendor            | Codership Oy <info@codership.com>               |
| wsrep_provider_version           | 3.39(rb3295e6)                                  |
| wsrep_ready                      | ON                                              |
+----------------------------------+-------------------------------------------------+
71 rows in set (0.01 sec)

mysql>
</code>

==== Vérification de la réplication ====

Sur **myserver-l-bdd02** :
<code sql>
mysql> CREATE DATABASE percona;
Query OK, 1 row affected (0.01 sec)
</code>

Sur **myserver-l-bdd03** :
  * Connexion à la base ''percona'' et création d'une table ''example'' :
<code sql>
mysql> USE percona;
Database changed
mysql> CREATE TABLE example (node_id INT PRIMARY KEY, node_name VARCHAR(30));
Query OK, 0 rows affected (0.11 sec)
</code>

Sur **myserver-l-bdd01** :
  * Redémarrage du service ''mysql'' (en mode normal) :
<code>systemctl stop mysql@bootstrap.service</code>
<code>systemctl start mysql</code>

  * Connexion à la console :
<code>
mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.27-30-57-log Percona XtraDB Cluster (GPL), Release rel30, Revision 64987d4, WSREP version 31.39, wsrep_31.39

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
</code>

  * Insertion d'un enregistrement dans la base :

<code sql>
mysql> INSERT INTO percona.example VALUES (1, 'percona1');
Query OK, 1 row affected (0.02 sec)

mysql>
</code>

Sur **myserver-l-bdd02** :
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


===== Installation de ProxySQL =====
Procédure : [[https://wiki.pictime-groupe.com/logiciels/applications/proxysql/installation/installation_et_configuration_std_25441150919_n2_n3_mes_outils_dt]]

Sur **myserver-l-master01** :
  * Installation du repo Percona :
<code>yum install -y https://repo.percona.com/yum/percona-release-latest.noarch.rpm</code>

  * Déplacement du fichier repo dans le dossier ''custom'' de ''Yum'' :
<code>mv /etc/yum.repos.d/percona-original-release.repo /etc/yum.repos.d/custom/</code>

  * Installation de ProxySQL :
<code>yum install -y proxysql2</code>

  * Activation de ProxySQL au démarrage :
<code>systemctl enable proxysql</code>

Sur **myserver-l-bdd01** :
  * Création d'un compte utilisateur ''proxysqladmin'' :
<code sql>
mysql> GRANT ALL PRIVILEGES ON *.* to 'proxysqladmin'@'10.30.0.%' identified by 'password';
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)
</code>

Sur **myserver-l-master01** :
  * Configuration du fichier template ''/etc/proxysql-admin.cnf'' :
<code>vim /etc/proxysql-admin.cnf</code>

<code ini>
# proxysql admin interface credentials.
export PROXYSQL_DATADIR='/var/lib/proxysql'
export PROXYSQL_USERNAME='admin'
export PROXYSQL_PASSWORD='admin'
export PROXYSQL_HOSTNAME='localhost'
export PROXYSQL_PORT='6032'

# PXC admin credentials for connecting to pxc-cluster-node.
export CLUSTER_USERNAME='proxysqladmin'
export CLUSTER_PASSWORD='pasSwOrd'
export CLUSTER_HOSTNAME='10.30.0.1'
export CLUSTER_PORT='3306'

# proxysql monitoring user. proxysql admin script will create this user in pxc to monitor pxc-nodes.
export MONITOR_USERNAME='monitor'
export MONITOR_PASSWORD='monitor'

# Application user to connect to pxc-node through proxysql
export CLUSTER_APP_USERNAME='app-user'
export CLUSTER_APP_PASSWORD='pAsSwOrd'

# ProxySQL hostgroup IDs
export WRITER_HOSTGROUP_ID='10'
export READER_HOSTGROUP_ID='11'
export BACKUP_WRITER_HOSTGROUP_ID='12'
export OFFLINE_HOSTGROUP_ID='13'

# ProxySQL read/write configuration mode.
export MODE="loadbal"

# max_connections default (used only when INSERTing a new mysql_servers entry)
export MAX_CONNECTIONS="1000"

# Determines the maximum number of writesets a node can have queued
# before the node is SHUNNED to avoid stale reads.
#export MAX_TRANSACTIONS_BEHIND=100

# Connections to the backend servers (from ProxySQL) will use SSL
export USE_SSL="no"

# Determines if a node should be added to the reader hostgroup if it has
# been promoted to the writer hostgroup.
# If set to 'yes', then all writers (including backup-writers) are added to
# the read hostgroup.
# If set to 'no', then none of the writers (including backup-writers) are added.
# If set to 'backup', then only the backup-writers will be added to
# the read hostgroup.
export WRITERS_ARE_READERS="yes"
</code>

  * Démarrage du service :
<code>systemctl start proxysql.service</code>