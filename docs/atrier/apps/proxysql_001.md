---
title: Installation ProxySQL
description: 
published: true
date: 2023-04-21T10:48:50.429Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:46.668Z
---

# Installation sur un Cluster Percona XtraDB Cluster

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-08-22</span>

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

```
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
```

  * Démarrage du service :
<code>systemctl start proxysql.service</code>