---
title: Plugin > Input > MySQL
description: 
published: true
date: 2023-04-21T10:50:02.864Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:49:58.979Z
---

====== Plugin > Input > MySQL ======

Lien : [[https://github.com/influxdata/telegraf/tree/release-1.7/plugins/inputs/mysql]]

Voici la liste des indicateurs disponibles avec le plugin :

^ Métrique Telegraf  ^  Infos  ^
|mysql_aborted_clients|    |
|mysql_aborted_connects|    |
|mysql_binlog_cache_disk_use|    |
|mysql_binlog_cache_use|    |
|mysql_binlog_stmt_cache_disk_use|    |
|mysql_binlog_stmt_cache_use|    |
|mysql_bytes_received|    |
|mysql_bytes_sent|    |
|mysql_com_admin_commands|    |
|mysql_com_alter_db|    |
|mysql_com_alter_db_upgrade|    |
|mysql_com_alter_event|    |
|mysql_com_alter_function|    |
|mysql_com_alter_procedure|    |
|mysql_com_alter_server|    |
|mysql_com_alter_table|    |
|mysql_com_alter_tablespace|    |
|mysql_com_analyze|    |
|mysql_com_assign_to_keycache|    |
|mysql_com_begin|    |
|mysql_com_binlog|    |
|mysql_com_call_procedure|    |
|mysql_com_change_db|    |
|mysql_com_change_master|    |
|mysql_com_check|    |
|mysql_com_checksum|    |
|mysql_com_commit|    |
|mysql_com_create_db|    |
|mysql_com_create_event|    |
|mysql_com_create_function|    |
|mysql_com_create_index|    |
|mysql_com_create_procedure|    |
|mysql_com_create_server|    |
|mysql_com_create_table|    |
|mysql_com_create_trigger|    |
|mysql_com_create_udf|    |
|mysql_com_create_user|    |
|mysql_com_create_view|    |
|mysql_com_dealloc_sql|    |
|mysql_com_delete|    |
|mysql_com_delete_multi|    |
|mysql_com_do|    |
|mysql_com_drop_db|    |
|mysql_com_drop_event|    |
|mysql_com_drop_function|    |
|mysql_com_drop_index|    |
|mysql_com_drop_procedure|    |
|mysql_com_drop_server|    |
|mysql_com_drop_table|    |
|mysql_com_drop_trigger|    |
|mysql_com_drop_user|    |
|mysql_com_drop_view|    |
|mysql_com_empty_query|    |
|mysql_com_execute_sql|    |
|mysql_com_flush|    |
|mysql_com_grant|    |
|mysql_com_ha_close|    |
|mysql_com_ha_open|    |
|mysql_com_ha_read|    |
|mysql_com_help|    |
|mysql_com_insert|    |
|mysql_com_insert_select|    |
|mysql_com_install_plugin|    |
|mysql_com_kill|    |
|mysql_com_load|    |
|mysql_com_lock_tables|    |
|mysql_com_optimize|    |
|mysql_com_preload_keys|    |
|mysql_com_prepare_sql|    |
|mysql_com_purge|    |
|mysql_com_purge_before_date|    |
|mysql_com_release_savepoint|    |
|mysql_com_rename_table|    |
|mysql_com_rename_user|    |
|mysql_com_repair|    |
|mysql_com_replace|    |
|mysql_com_replace_select|    |
|mysql_com_reset|    |
|mysql_com_resignal|    |
|mysql_com_revoke|    |
|mysql_com_revoke_all|    |
|mysql_com_rollback|    |
|mysql_com_rollback_to_savepoint|    |
|mysql_com_savepoint|    |
|mysql_com_select|    |
|mysql_com_set_option|    |
|mysql_com_show_authors|    |
|mysql_com_show_binlog_events|    |
|mysql_com_show_binlogs|    |
|mysql_com_show_charsets|    |
|mysql_com_show_collations|    |
|mysql_com_show_contributors|    |
|mysql_com_show_create_db|    |
|mysql_com_show_create_event|    |
|mysql_com_show_create_func|    |
|mysql_com_show_create_proc|    |
|mysql_com_show_create_table|    |
|mysql_com_show_create_trigger|    |
|mysql_com_show_databases|    |
|mysql_com_show_engine_logs|    |
|mysql_com_show_engine_mutex|    |
|mysql_com_show_engine_status|    |
|mysql_com_show_errors|    |
|mysql_com_show_events|    |
|mysql_com_show_fields|    |
|mysql_com_show_function_status|    |
|mysql_com_show_grants|    |
|mysql_com_show_keys|    |
|mysql_com_show_master_status|    |
|mysql_com_show_open_tables|    |
|mysql_com_show_plugins|    |
|mysql_com_show_privileges|    |
|mysql_com_show_procedure_status|    |
|mysql_com_show_processlist|    |
|mysql_com_show_profile|    |
|mysql_com_show_profiles|    |
|mysql_com_show_relaylog_events|    |
|mysql_com_show_slave_hosts|    |
|mysql_com_show_slave_status|    |
|mysql_com_show_status|    |
|mysql_com_show_storage_engines|    |
|mysql_com_show_table_status|    |
|mysql_com_show_tables|    |
|mysql_com_show_triggers|    |
|mysql_com_show_variables|    |
|mysql_com_show_warnings|    |
|mysql_com_signal|    |
|mysql_com_slave_start|    |
|mysql_com_slave_stop|    |
|mysql_com_stmt_close|    |
|mysql_com_stmt_execute|    |
|mysql_com_stmt_fetch|    |
|mysql_com_stmt_prepare|    |
|mysql_com_stmt_reprepare|    |
|mysql_com_stmt_reset|    |
|mysql_com_stmt_send_long_data|    |
|mysql_com_truncate|    |
|mysql_com_uninstall_plugin|    |
|mysql_com_unlock_tables|    |
|mysql_com_update|    |
|mysql_com_update_multi|    |
|mysql_com_xa_commit|    |
|mysql_com_xa_end|    |
|mysql_com_xa_prepare|    |
|mysql_com_xa_recover|    |
|mysql_com_xa_rollback|    |
|mysql_com_xa_start|    |
|mysql_compression|    |
|mysql_connections|    |
|mysql_created_tmp_disk_tables|    |
|mysql_created_tmp_files|    |
|mysql_created_tmp_tables|    |
|mysql_delayed_errors|    |
|mysql_delayed_insert_threads|    |
|mysql_delayed_writes|    |
|mysql_flush_commands|    |
|mysql_handler_commit|    |
|mysql_handler_delete|    |
|mysql_handler_discover|    |
|mysql_handler_prepare|    |
|mysql_handler_read_first|    |
|mysql_handler_read_key|    |
|mysql_handler_read_last|    |
|mysql_handler_read_next|    |
|mysql_handler_read_prev|    |
|mysql_handler_read_rnd|    |
|mysql_handler_read_rnd_next|    |
|mysql_handler_rollback|    |
|mysql_handler_savepoint|    |
|mysql_handler_savepoint_rollback|    |
|mysql_handler_update|    |
|mysql_handler_write|    |
|mysql_innodb_buffer_pool_bytes_data|    |
|mysql_innodb_buffer_pool_bytes_dirty|    |
|mysql_innodb_buffer_pool_pages_data|    |
|mysql_innodb_buffer_pool_pages_dirty|    |
|mysql_innodb_buffer_pool_pages_flushed|    |
|mysql_innodb_buffer_pool_pages_free|    |
|mysql_innodb_buffer_pool_pages_misc|    |
|mysql_innodb_buffer_pool_pages_total|    |
|mysql_innodb_buffer_pool_read_ahead|    |
|mysql_innodb_buffer_pool_read_ahead_evicted|    |
|mysql_innodb_buffer_pool_read_ahead_rnd|    |
|mysql_innodb_buffer_pool_read_requests|    |
|mysql_innodb_buffer_pool_reads|    |
|mysql_innodb_buffer_pool_wait_free|    |
|mysql_innodb_buffer_pool_write_requests|    |
|mysql_innodb_data_fsyncs|    |
|mysql_innodb_data_pending_fsyncs|    |
|mysql_innodb_data_pending_reads|    |
|mysql_innodb_data_pending_writes|    |
|mysql_innodb_data_read|    |
|mysql_innodb_data_reads|    |
|mysql_innodb_data_writes|    |
|mysql_innodb_data_written|    |
|mysql_innodb_dblwr_pages_written|    |
|mysql_innodb_dblwr_writes|    |
|mysql_innodb_have_atomic_builtins|    |
|mysql_innodb_log_waits|    |
|mysql_innodb_log_write_requests|    |


Avant d'activer le plugin, il faut créer un utilisateur avec des droits limités mais qu'il puisse récupérer les informations sur la BDD.

Pour cela, il faut :

  * Se connecter au serveur client en SSH
  * Se connecter à MySQL en local avec le compte ''root''

<code>mysql -u root -p</code>

  * Créer un utilisateur ''telegraf'' avec des droits restreins

<code>
mysql> CREATE USER 'telegraf'@'localhost' IDENTIFIED BY 'password';
mysql> REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'telegraf'@'localhost';
mysql> GRANT REPLICATION CLIENT ON *.* TO 'telegraf'@'localhost';
mysql> GRANT SHOW VIEW ON *.* TO 'telegraf'@'localhost';
mysql> FLUSH PRIVILEGES;
</code>

ou 

<code>
 the telegraf.conf file I have the following line to connect to the database server.

servers = ["telegraf:password@unix(/tmp/mysql.sock)/information_schema"]
To create the telegraf MySQL user and grant it the minimal level of privileges, I ran the following:

CREATE USER 'telegraf'@'localhost' IDENTIFIED WITH mysql_native_password AS '***';
GRANT PROCESS ON *.* TO 'telegraf'@'localhost' REQUIRE NONE WITH 
  MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 
  MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0;

GRANT REPLICATION CLIENT ON *.* TO 'telegraf'@'localhost';
GRANT SELECT ON `cluster\_replication`.* TO 'telegraf'@'localhost';
GRANT SELECT ON `information\_schema`.* TO 'telegraf'@'localhost';
GRANT SELECT ON `performance\_schema`.* TO 'telegraf'@'localhost';
I seem to be getting all the metrics that I expect, so I think this is working. I think this is a minimal set of privileges because it is only SELECT on various databases and the ability to read process variables. I’m no expert, but this seems fairly safe.
</code>

  * Tester l'accès de l'utilisateur

<code>
mysql> SELECT * FROM information_schema.USER_PRIVILEGES;
mysql> SHOW GLOBAL STATUS;
mysql> SHOW SLAVE STATUS;
mysql> SHOW VARIABLES;
mysql> SHOW DATABASES;
mysql> SELECT TABLE_NAME, AVG_ROW_LENGTH, DATA_LENGTH, MAX_DATA_LENGTH, INDEX_LENGTH, DATA_FREE, TABLE_SCHEMA FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE = 'BASE TABLE' AND MAX_DATA_LENGTH > 0 ORDER BY TABLE_SCHEMA ASC;
</code>

  * Activer le plugin comme ci-dessous :

<code>
# # Read metrics from one or many mysql servers
 [[inputs.mysql]]
#   ## specify servers via a url matching:
#   ##  [username[:password]@][protocol[(address)]]/[?tls=[true|false|skip-verify|custom]]
#   ##  see https://github.com/go-sql-driver/mysql#dsn-data-source-name
#   ##  e.g.
#   ##    servers = ["user:passwd@tcp(127.0.0.1:3306)/?tls=false"]
#   ##    servers = ["user@tcp(127.0.0.1:3306)/?tls=false"]
#   #
#   ## If no servers are specified, then localhost is used as the host.
   servers = ["telegraf:password@tcp(127.0.0.1:3306)/?tls=false"]
#
#   ## Selects the metric output format.
#   ##
#   ## This option exists to maintain backwards compatibility, if you have
#   ## existing metrics do not set or change this value until you are ready to
#   ## migrate to the new format.
#   ##
#   ## If you do not have existing metrics from this plugin set to the latest
#   ## version.
#   ##
#   ## Telegraf >=1.6: metric_version = 2
#   ##           <1.6: metric_version = 1 (or unset)
   metric_version = 2
#
#   ## the limits for metrics form perf_events_statements
   perf_events_statements_digest_text_limit  = 120
   perf_events_statements_limit              = 250
   perf_events_statements_time_limit         = 86400
#   #
#   ## if the list is empty, then metrics are gathered from all databasee tables
   table_schema_databases                    = []
#   #
#   ## gather metrics from INFORMATION_SCHEMA.TABLES for databases provided above list
   gather_table_schema                       = false
#   #
#   ## gather thread state counts from INFORMATION_SCHEMA.PROCESSLIST
   gather_process_list                       = true
#   #
#   ## gather thread state counts from INFORMATION_SCHEMA.USER_STATISTICS
   gather_user_statistics                    = true
#   #
#   ## gather auto_increment columns and max values from information schema
   gather_info_schema_auto_inc               = true
#   #
#   ## gather metrics from INFORMATION_SCHEMA.INNODB_METRICS
   gather_innodb_metrics                     = true
#   #
#   ## gather metrics from SHOW SLAVE STATUS command output
   gather_slave_status                       = true
#   #
#   ## gather metrics from SHOW BINARY LOGS command output
#   gather_binary_logs                        = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_TABLE
   gather_table_io_waits                     = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.TABLE_LOCK_WAITS
   gather_table_lock_waits                   = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.TABLE_IO_WAITS_SUMMARY_BY_INDEX_USAGE
   gather_index_io_waits                     = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.EVENT_WAITS
   gather_event_waits                        = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.FILE_SUMMARY_BY_EVENT_NAME
   gather_file_events_stats                  = false
#   #
#   ## gather metrics from PERFORMANCE_SCHEMA.EVENTS_STATEMENTS_SUMMARY_BY_DIGEST
   gather_perf_events_statements             = false
#   #
#   ## Some queries we may want to run less often (such as SHOW GLOBAL VARIABLES)
   interval_slow                   = "30m"
#
#   ## Optional TLS Config (will be used if tls=custom parameter specified in server uri)
#   # tls_ca = "/etc/telegraf/ca.pem"
#   # tls_cert = "/etc/telegraf/cert.pem"
#   # tls_key = "/etc/telegraf/key.pem"
#   ## Use TLS but skip chain & host verification
#   # insecure_skip_verify = false

</code>