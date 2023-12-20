---
title: PostgreSQL - Exporter un table dans un fichier CSV
description: 
published: true
date: 2023-04-21T10:53:33.865Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:53:27.735Z
---

====== PostgreSQL - Exporter un table dans un fichier CSV ======
----

Commande :

<code>COPY %NOM_TABLE% TO '%CHEMIN_FICHIER_CSV%' DELIMITER ',' CSV HEADER;</code>

  * **%NOM_TABLE%** = Nom de la table qui doit être exporté
  * **%CHEMIN_FICHIER_CSV%** = Chemin et nom du fichier avec l'extension ".csv"
  * **DELIMITER ','** = Définit le caractère qui va délimiter les colonnes (champs)
  * **HEADER** = Affiche le nom des colonnes dans l'entête du fichier CSV

Exemple :

<code>COPY director_datafield TO '/tmp/director_datafield.csv' DELIMITER ',' CSV HEADER;</code>


===== Extraire certains champs =====

Exemple : 

<code>COPY persons(first_name,last_name,email) TO 'C:\tmp\persons_partial_db.csv' DELIMITER ',' CSV HEADER;</code>

===== Extraire qu'un champ =====

<code>COPY persons(email) TO 'C:\tmp\persons_email_db.csv' DELIMITER ',' CSV;</code>

===== \Copy =====

Export data from a table to CSV file using the \copy command
In case you have the access to a remote PostgreSQL database server, but you don’t have sufficient privileges to write to a file on it, you can use the PostgreSQL built-in command \copy.

The \copy command basically runs the COPY statement above. However, instead of server writing the CSV file, psql writes the CSV file, transfers data from the server to your local file system. To use \copy command, you just need to have sufficient privileges to your local machine. It does not require PostgreSQL superuser privileges.

For example, if you want to export all data of the persons table into persons_client.csv file, you can execute the \copy command from the psql client as follows:

1
\copy (SELECT * FROM persons) to 'C:\tmp\persons_client.csv' with csv

Source = http://www.postgresqltutorial.com/export-postgresql-table-to-csv-file/

====== Cluster PostgreSQL avec PgPool-II (Haute Disponibilité) ======
----

Source : [[https://www.along.party/postgresql-ha%E6%96%B9%E6%A1%88pgpool-ii.html|ici]]

OS: CentOS 7

Master : node1 (IP 192.168.52.101)
Slave : node2 (IP 192.168.52.102)

VIP：192.168.52.100

(master/standby)
Désactivation de SELINUX :
<code># vim /etc/sysconfig/selinux</code>
<code>SELINUX=disabled</code>

Modification du fichier ''/etc/hosts'' :

<code>
# vim /etc/hosts
192.168.52.101   pgpool-node01
192.168.52.102   pgpool-node02
</code>

Configuration de l'installation de l'environnement de réplication de flux postgreSQL 

Configuration Connexion SSH :
<code>
# su – postgres
$ ssh-keygen
$ ssh-copy-id postgres@192.168.52.101
$ ssh postgres@192.168.52.101
$ ssh-copy-id postgres@192.168.52.102
$ ssh postgres@192.168.52.102
</code>

Installation des dépendances :
<code># yum -y install readline-devel zlib zlib-devel openssl openssl-devel pam-devel libxml2-devel libxslt-devel python-devel tcl-devel gcc perl-ExtUtils-Embed</code>

Installation de postgresql

(master)
Configuration du fichier ''/var/lib/pgsql/9.6/data/postgresql.conf'' :

<code>
$ cd /var/lib/pgsql/9.6/data/
$ vim postgresql.conf

listen_addresses = '*'
port = 5432
max_connections = 500
wal_level = hot_standby
archive_mode = on
archive_command = '/bin/date'
max_wal_senders = 6
hot_standby = on
</code>

(master)
Editer le fichier ''/var/lib/pgsql/9.6/data/pg_hba.conf''

<code>
$ cd /var/lib/pgsql/9.6/data/
$ vim pg_hba.conf

host        all             all             192.168.52.101/32   trust
host        all             all             192.168.52.102/32   trust
host        replication     replica         192.168.52.102/32   trust
host        replication     replica         192.168.52.101/32   trust
</code>

(master)
Edition du fichier ''/var/lib/pgsql/9.6/data/recovery.done'' (résynchronisation suite primary -> standby)
<code>
$ vim /var/lib/pgsql/9.6/data/recovery.done

standby_mode = on
recovery_target_timeline = ‘latest’
primary_conninfo = ’host=192.168.52.102 port=5432 user=replica password=replica’
trigger_file = ‘/var/lib/pgsql/9.6/data/trigger_file’
</code>

(master)
Redémarrage du service PostgreSQL :
<code>$ pg_ctl -D /var/lib/pgsql/9.6/data/ -l /var/lib/pgsql/9.6/data/logfile restart</code>

(master)
Création du compte utilisateur pour la réplication :
<code>
$ psql -U postgres
postgres# CREATE ROLE replica login replication encrypted password 'replica';
</code>

(standby)
Synchronisation des données PostgreSQL :
<code>
# su – postgres
$ pg_basebackup -F p --progress -D /var/lib/pgsql/9.6/data/ -h 192.168.52.101 -p 5432 -U replica --password
</code>

(standby)
Configuration du serveur en Standby :
<code>
$ mv /var/lib/pgsql/9.6/data/recovery.done /var/lib/pgsql/9.6/data/recovery.conf
$ vim /var/lib/pgsql/9.6/data/recovery.conf

standby_mode = on
recovery_target_timeline =’latest’
primary_conninfo = ‘host=192.168.52.101 port=5432 user=replica’
trigger_file = ‘/var/lib/pgsql/9.6/data/trigger_file’
</code>

(standby)
Redémarrage du service PostgreSQL :
<code>$ pg_ctl –D /var/lib/pgsql/9.6/data/ -l /var/lib/pgsql/9.6/data/logfile start</code>

Vérifier le fonctionnement :

<code># ps –ef | grep postgres</code>

{{:sgdbr:postgresql:1503926542030_8_thumb-2.png|}}

(master)
Vérifier le fonctionnement :

<code># ps –ef | grep postgres</code>

{{:sgdbr:postgresql:1503926549739_9_thumb-1.png|}}

Test

(master)
<code>
postgres =# create table test( id int, name varchar(10));
postgres =# create table test( id int, name varchar(10));
postgres=# select table_name from information_schema.tables where table_schema = 'public';
table_name 
------------
 test
(1 row)  
</code>

(standby)
<code>
postgres=# select table_name from information_schema.tables where table_schema = 'public';
 table_name 
------------
 test
(1 row)  
</code>

Les nouvelles données ajoutées ont été transférées et la session de secours est en lecture seule.

(master/standby)
Installation de PgPool

(master)
Installer la fonction pgpool-regclass, pgpool-recovery
<code>
[postgres@node1 ~]$cd /usr/local/src/pgsql/pgpool-II-3.4.3/src/sql/pgpool-regclass
[postgres@node1 pgpool-regclass]$make
[postgres@node1 pgpool-regclass]$make install
[postgres@node1 pgpool-regclass]$psql -f pgpool-regclass.sql template1
[postgres@node1 pgpool-regclass]$cd ../pgpool-recovery
[postgres@node1 pgpool-recovery]$make
[postgres@node1 pgpool-recovery]$make install
[postgres@node1 pgpool-recovery]$psql -f pgpool-recovery.sql template1
</code>

Après l'installation, le répertoire ''/opt/pgsql/share/extension/'' doit avoir les fichiers suivants.
<code>
pgpool_recovery--1.1.sql
pgpool_recovery.control
pgpool-recovery.sql
pgpool_regclass--1.0.sql
pgpool_regclass.control
pgpool-regclass.sql
</code>

Remarque: le noeud de secours n'a pas besoin de créer pgpool_regclass, pgpool_recovery)

Voir les nouvelles fonctions ajoutées :
<code>
[postgres@node1 ~]$ psql -U postgres template1
template1=# \df
</code>

{{:sgdbr:postgresql:1_thumb-1.png|}}

Configurez ifconfig, arping execute permission (master / standby)

<code>
# vim /etc/sudoers

postgres        ALL=(ALL)       NOPASSWD: /sbin/ifconfig
postgres        ALL=(ALL)       NOPASSWD: /sbin/ifup
postgres        ALL=(ALL)       NOPASSWD: /sbin/ifdown
postgres        ALL=(ALL)       NOPASSWD: /sbin/arping
</code>

Droits d'exécution :
<code>
# chmod u+s /sbin/ifconfig
# chmod u+s /usr/sbin/arping
</code>

Sortie des logs :

(master/standby)

<code>
# vim /etc/rsyslog.conf
local0.* /var/log/pgpool/pgpool.log
</code>

Redémarrage du service : 

<code># /etc/init.d/rsyslog restart</code>

Modification du fichier de conf de PgPool :

<code>
# vim /etc/pgpool-II-96/pgpool.conf
log_destination='syslog'
</code>

Configuration du fichier de PgPool ''/etc/pgpool-II-96/pgpool.conf''

<code>
[postgres@node1 ~]$ cd /etc/pgpool-II-96/
[postgres@node1 etc]$ cp pgpool.conf.sample pgpool.conf
[postgres@node1 etc]$ vim pgpool.conf
</code>

Exemple de configuration :
<code ini>
# CONNECTIONS
# - pgpool Connection Settings -
listen_addresses = '*'
port = 9999
socket_dir = '/etc/pgpool-II-96'
# - pgpool Communication Manager Connection Settings –
pcp_listen_addresses = '*'
pcp_port = 9898
pcp_socket_dir = '/etc/pgpool-II-96'
# - Backend Connection Settings -
backend_hostname0 = 'node1'
backend_port0 = 5432
backend_weight0 = 1
backend_data_directory0 = '/var/lib/pgsql/9.6/data'
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_hostname1 = 'node2'
backend_port1 = 5432
backend_weight1 = 1
backend_data_directory1 = '/var/lib/pgsql/9.6/data'
backend_flag1 = 'ALLOW_TO_FAILOVER'
# - Authentication -
enable_pool_hba = on
pool_passwd = 'pool_passwd'
authentication_timeout = 60
# LOGS
# - Where to log -
log_destination = 'syslog'
# - What to log -
print_timestamp = on
log_connections = on
log_hostname = on
log_statement = on
log_per_node_statement = off
log_standby_delay = 'none'
# FILE LOCATIONS
pid_file_name = '/var/run/pgpool-II-96/pgpool.pid'
logdir = '/var/log/pgpool'
# CONNECTION POOLING
# CONNECTION POOLING
connection_cache = on
reset_query_list = 'ABORT; DISCARD ALL'
# REPLICATION MODE
replication_mode = off
replicate_select = off
insert_lock = on
lobj_lock_table = ''
# LOAD BALANCING MODE
load_balance_mode = on
ignore_leading_white_space = on
white_function_list = ''
black_function_list = 'nextval,setval,nextval,setval'
# MASTER/SLAVE MODE
master_slave_mode = on
master_slave_sub_mode = 'stream'
# - Streaming -
sr_check_period = 5
sr_check_user = 'replica'
sr_check_password = 'replica'
delay_threshold = 16000
# - Special commands -
follow_master_command = ''
parallel_mode = off
pgpool2_hostname = ''
system_db_hostname  = 'localhost'
system_db_port = 5432
system_db_dbname = 'pgpool'
system_db_schema = 'pgpool_catalog'
system_db_user = 'pgpool'
system_db_password = ''
# HEALTH CHECK
health_check_period = 5
health_check_timeout = 20
health_check_user = 'replica'
health_check_password = 'replcia'
health_check_max_retries = 3
health_check_retry_delay = 1
# FAILOVER AND FAILBACK
failover_command = '/etc/pgpool-II-96/failover_stream.sh  %d %H /var/lib/pgsql/9.6/data/trigger_file'  
failback_command = ''
fail_over_on_backend_error = on
search_primary_node_timeout = 10
# ONLINE RECOVERY
recovery_user = 'nobody'
recovery_password = ''
recovery_1st_stage_command = ''
recovery_2nd_stage_command = ''
recovery_timeout = 90
client_idle_limit_in_recovery = 0
# WATCHDOG
use_watchdog = on
# -Connection to up stream servers -
trusted_servers = ''
ping_path = '/bin'
# - Watchdog communication Settings -
wd_hostname = 'node1'
wd_port = 9000
wd_authkey = ''
# - Virtual IP control Setting –
delegate_IP = '192.168.1.233' 
ifconfig_path = '/sbin'  
if_up_cmd = 'ifconfig eth0:0 inet $_IP_$ netmask 255.255.255.0'
if_down_cmd = 'ifconfig eth0:0 down'
arping_path = '/usr/sbin'           # arping command path
arping_cmd = 'arping -U $_IP_$ -w 1'
# - Behaivor on escalation Setting -
clear_memqcache_on_escalation = on
wd_escalation_command = ''
# - Lifecheck Setting -
wd_lifecheck_method = 'heartbeat'
wd_interval = 6
# -- heartbeat mode --
wd_heartbeat_port = 9694
wd_heartbeat_keepalive = 2
wd_heartbeat_deadtime = 30
heartbeat_destination0 = 'node2'
heartbeat_destination_port0 = 9694 
heartbeat_device0 = 'eth0' 
# -- query mode --
wd_life_point = 3
wd_lifecheck_query = 'SELECT 1'
wd_lifecheck_dbname = 'template1'
wd_lifecheck_user = 'nobody'
wd_lifecheck_password = ''
# - Other pgpool Connection Settings -
other_pgpool_hostname0 = 'node2'
other_pgpool_port0 = 9999
other_wd_port0 = 9000
</code>

Configuration du fichier ''pcp.conf'' :
<code>
$ cd /etc/pgpool-II-96/
$ cp pcp.conf.sample pcp.conf
</code>

On génère un hash MD5 du mot de passe du compte "postgres" via la commande :

<code>
$pg_md5 -u postgres -p 
password: postgres
e8a48653851e28c69d0506508fb27fc5
</code>

On récupère la valeur retournée et on l'insère dans le fichier ''pcp.conf'' :

<code># vim /etc/pgpool-II-96/pcp.conf</code>

Le fichier devrait contenir ceci :

<code>
# USERID:MD5PASSWD
postgres:e8a48653851e28c69d0506508fb27fc5
</code>

Configuration du fichier ''pool_hba''


<code>
[postgres@node1 ~]$ cd /etc/pgpool-II-96/etc
    [postgres@node1 etc]$cp pool_hba.conf.sample pool_hba.conf
</code>

<code>
[postgres@node1 etc]$ vim pool_hba.conf
    host    all             all           192.168.52.102/32        trust
    host    all             all           192.168.52.101/32        trust
</code>


Configuration du failover :

<code>$touch /etc/pgpool-II-96/failover_stream.sh</code>

<code bash>
#! /bin/sh
# Failover command for streaming replication.
# This script assumes that DB node 0 is primary, and 1 is standby.
#
# If standby goes down, do nothing. If primary goes down, create a
# trigger file so that standby takes over primary node.
#
# Arguments: $1: failed node id. $2: new master hostname. $3: path to
# trigger file.
failed_node=$1
new_master=$2
trigger_file=$3
`# Do nothing if standby goes down.
    #if [ $failed_node = 1 ]; then`
#        exit 0;
#fi
/usr/bin/ssh -T $new_master /bin/touch $trigger_file
exit 0;
</code>

Droits d'exécution :
<code># chmod 775 /etc/pgpool-II-96/failover_stream.sh<code>

(standby)
Configuration du fichier ''pgpool.conf'' :

<code>
[postgres@node2 ~]$ cd /etc/pgpool-II-96/etc
[postgres@node2 etc]$cp pgpool.conf.sample pgpool.conf
[postgres@node2 etc]$vim pgpool.conf
</code>

<code>
# CONNECTIONS
listen_addresses = '*'
port = 9999
socket_dir = '/etc/pgpool-II-96'
# - pgpool Communication Manager Connection Settings –
pcp_listen_addresses = '*'
pcp_port = 9898
pcp_socket_dir = '/etc/pgpool-II-96'
# - Backend Connection Settings -
backend_hostname0 = 'node1'
backend_port0 = 5432
backend_weight0 = 1
backend_flag0 = 'ALLOW_TO_FAILOVER'
backend_hostname1 = 'node2'
backend_port1 = 5432
backend_weight1 = 1
backend_flag1 = 'ALLOW_TO_FAILOVER'
# - Authentication -
enable_pool_hba = on
pool_passwd = 'pool_passwd'
authentication_timeout = 60
# LOGS
# - What to log -
log_destination = 'syslog'
print_timestamp = on
log_connections = on
log_hostname = on
log_statement = on
log_per_node_statement = off
log_standby_delay = 'none'
# FILE LOCATIONS
pid_file_name = '/var/run/pgpool-II-96/pgpool.pid'
logdir = '/var/log/pgpool'
# CONNECTION POOLING
connection_cache = on
reset_query_list = 'ABORT; DISCARD ALL' 
# LOAD BALANCING MODE
load_balance_mode = on
ignore_leading_white_space = on
white_function_list = ''
black_function_list = ' nextval,setval,nextval,setval'
# MASTER/SLAVE MODE
master_slave_mode = on
master_slave_sub_mode = 'stream'
# - Streaming -
sr_check_period = 0
sr_check_user = 'replica'
sr_check_password = 'replica'
delay_threshold = 16000
# - Special commands -
follow_master_command = ''
parallel_mode = off
pgpool2_hostname = ''
system_db_hostname  = 'localhost'
system_db_port = 5432
system_db_dbname = 'pgpool'
system_db_schema = 'pgpool_catalog'
system_db_user = 'pgpool'
system_db_password = ''
# HEALTH CHECK
health_check_period = 0
health_check_timeout = 20
health_check_user = 'nobody'
health_check_password = ''
health_check_max_retries = 0
health_check_retry_delay = 1
# FAILOVER AND FAILBACK
failover_command = '/etc/pgpool-II-96/failover_stream.sh %d %H /var/lib/pgsql/9.6/data/trigger_file'
failback_command = ''
fail_over_on_backend_error = on
search_primary_node_timeout = 10
# ONLINE RECOVERY
recovery_user = 'nobody'
recovery_password = ''
recovery_1st_stage_command = ''
recovery_2nd_stage_command = ''
recovery_timeout = 90
client_idle_limit_in_recovery = 0
# WATCHDOG
# - Enabling -
use_watchdog = on
# -Connection to up stream servers -
trusted_servers = ''
ping_path = '/bin'
# - Watchdog communication Settings -
wd_hostname = 'node2 '
wd_port = 9000
wd_authkey = ''
# - Virtual IP control Setting -
delegate_IP = '192.168.1.233 '
ifconfig_path = '/sbin'
if_up_cmd = 'ifconfig eth0:0 inet $_IP_$ netmask 255.255.255.0'
if_down_cmd = 'ifconfig eth0:0 down'
arping_path = '/usr/sbin'           # arping command path
arping_cmd = 'arping -U $_IP_$ -w 1'
# -- heartbeat mode --
wd_heartbeat_port = 9694
wd_heartbeat_keepalive = 2
wd_heartbeat_deadtime = 30
heartbeat_destination0 = 'node1'
heartbeat_destination_port0 = 9694 
heartbeat_device0 = 'eth0'
# - Other pgpool Connection Settings -
other_pgpool_hostname0 = 'node1'
other_pgpool_port0 = 9999
other_wd_port0 = 9000
</code>

Configuration du fichier ''pcp.conf'' :
<code>
$ cd /etc/pgpool-II-96/
$ cp pcp.conf.sample pcp.conf
</code>

On génère un hash MD5 du mot de passe du compte "postgres" via la commande :

<code>
$pg_md5 -u postgres -p 
password: postgres
e8a48653851e28c69d0506508fb27fc5
</code>

On récupère la valeur retournée et on l'insère dans le fichier ''pcp.conf'' :

<code># vim /etc/pgpool-II-96/pcp.conf</code>

Le fichier devrait contenir ceci :

<code>
# USERID:MD5PASSWD
postgres:e8a48653851e28c69d0506508fb27fc5
</code>

Configuration du fichier ''pool_hba''


<code>
[postgres@node2 ~]$ cd /etc/pgpool-II-96/
    [postgres@node2 etc]$cp pool_hba.conf.sample pool_hba.conf
</code>

<code>
[postgres@node2 etc]$ vim pool_hba.conf
    host    all             all           192.168.52.102/32        trust
    host    all             all           192.168.52.101/32        trust
</code>

Configuration du failover :

<code>$touch /etc/pgpool-II-96/failover_stream.sh</code>

<code bash>
#! /bin/sh
# Failover command for streaming replication.
# This script assumes that DB node 0 is primary, and 1 is standby.
#
# If standby goes down, do nothing. If primary goes down, create a
# trigger file so that standby takes over primary node.
#
# Arguments: $1: failed node id. $2: new master hostname. $3: path to
# trigger file.
failed_node=$1
new_master=$2
trigger_file=$3
`# Do nothing if standby goes down.
    #if [ $failed_node = 1 ]; then`
#        exit 0;
#fi
/usr/bin/ssh -T $new_master /bin/touch $trigger_file
exit 0;
</code>

Droits d'exécution :
<code># chmod 775 /etc/pgpool-II-96/failover_stream.sh</code>

Vérification

Contrôle des process : 
(master)
<code>ps -ef | grep postgres</code>

{{:sgdbr:postgresql:4_thumb-1.png|}}

(slave)
{{:sgdbr:postgresql:5_thumb-1.png|}}

Acces à la base de données via pgpool :

<code>[postgres@node1 ~]$ psql -h 192.168.52.100 -p 9999 -U postgres -d postgres</code>

{{:sgdbr:postgresql:6_thumb-1.png|}}

Afficher le status des noeuds PgPool :

<code>postgres=# show pool_nodes;</code>

{{:sgdbr:postgresql:7_thumb-1.png|}}

Informations sur le statut :

<code>
0 - This state is only used during the initialization. PCP will never display it.
1 - Node is up. No connections yet.
2 - Node is up. Connections are pooled.
3 - Node is down.
</code>

Afficher l'IP Virtuelle :

(master)
{{:sgdbr:postgresql:8_thumb-1.png|}}

Test de basculement

Arrêt de PostgreSQL sur node1 :

{{:sgdbr:postgresql:9_thumb-1.png|}}

Afficher le nœud maître pgsql actuel

Vous pouvez voir que le nœud maître pgsql est sur node01 et que pgsql est désactivé pour node01.
Node1 ferme la base de données

<code>
$ pg_ctl -m fast stop
waiting for server to shut down....LOG:  received fast shutdown request
LOG:  aborting any active transactions
FATAL:  terminating connection due to administrator command
LOG:  autovacuum launcher shutting down
LOG:  shutting down
LOG:  database system is shut down
</code>

{{:sgdbr:postgresql:11_thumb-1.png|}}

Test sur le node2

<code>
[root@db2 ~]# pg_controldata | grep cluster
Database cluster state:               in production
[root@db2 ~]# ll /var/lib/pgsql/9.6/data/recovery.done 
-rw-r--r--. 1 postgres postgres 189 Aug 28 15:55 /var/lib/pgsql/9.6/data/recovery.done
[root@db2 ~]#
</code>

Remarque: Vous pouvez voir que le noeud node02 a terminé de passer du rôle de **standby** en **primaire** et que le fichier ''/var/lib/pgsql/9.6/data/recovery.conf'' devient ''recovery.done''.

Afficher le statut de PgPool :

<code>
-bash-4.1$ psql -h 192.168.52.100 -p 9999
psql (9.3.4)
Type "help" for help.
postgres=# show pool_nodes;
 node_id |   hostname    | port | status | lb_weight |  role   
---------+---------------+------+--------+-----------+---------
 0       | pgpool-node01 | 5432 | 3      | 0.500000  | standby
 1       | pgpool-node02 | 5432 | 2      | 0.500000  | primary
(2 rows)
</code>

Remarque: ''node02'' a été converti en rôle **primaire**. L'état ''node01'' est **3**, indiquant l'état **standby**.

Démarrer node2 en mode standby :

<code>
-bash-4.1$ cd /var/lib/pgsql/9.6/data/
-bash-4.1$ mv recovery.done recovery.conf
-bash-4.1$ pg_ctl start
server starting
-bash-4.1$ LOG:  database system was shut down at 2017-08-28 16:25:23 CST
LOG:  entering standby mode
LOG:  consistent recovery state reached at 0/13000090
LOG:  record with zero length at 0/13000090
LOG:  database system is ready to accept read only connections
LOG:  fetching timeline history file for timeline 6 from primary server
LOG:  started streaming WAL from primary at 0/13000000 on timeline 5
LOG:  replication terminated by primary server
DETAIL:  End of WAL reached on timeline 5 at 0/13000090.
LOG:  new target timeline is 6
LOG:  restarted WAL streaming at 0/13000000 on timeline 6
LOG:  redo starts at 0/13000090
</code>

Rattachement du node1 :

<code>
-bash-4.1$ pcp_attach_node -d 5 pgpool-node01 9898 postgres postgres 0
DEBUG: send: tos="R", len=46
DEBUG: recv: tos="r", len=21, data=AuthenticationOK
DEBUG: send: tos="D", len=6
DEBUG: recv: tos="c", len=20, data=CommandComplete
DEBUG: send: tos="X", len=4
</code>

DEBUG

Utilisation de ''journalctl -d''

====== Installation de PostgreSQL version 9.6 ======
----

===== Prérequis =====
  * Installation d'un OS CentOS 7.x
    * Désactivation de SELINUX = <code>vim /etc/sysconfig/selinux</code>
    * Arrêt de firewalld = <code>systemctl stop firewalld</code>
    * Désactivation de firewalld = <code>systemctl disable firewalld</code>
    * Mise à jour de l'OS => <code>yum update -y</code>
    * Reboot du serveur => <code>reboot</code>
    * Installation de packages standards => <code>yum update -y vim perl</code>
    * Installation de VMWare-Tools => <code>yum update -y open-vm-tools</code>
    * Installation du repo EPEL => <code>yum install -y epel-release.noarch</code>
    * Installation du repo PostgreSQL => <code>yum install -y https://download.postgresql.org/pub/repos/yum/9.6/redhat/rhel-7-x86_64/pgdg-centos96-9.6-3.noarch.rpm</code>

===== Installation de PostgreSQL 9.6 =====

  * Installation des packages :

<code>yum install -y postgresql96 postgresql96-server postgresql96-contrib</code>

  * Initialisation de la base de données :

<code>/usr/pgsql-9.6/bin/postgresql96-setup initdb</code>

  * Démarrage du service :

<code>systemctl start postgresql-96.service</code>
