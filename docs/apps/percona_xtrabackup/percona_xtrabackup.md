# PERCONA XTRABACKUP
----

## <i class="fa-solid fa-hashtag"></i> Information

![Logo](../../_media/apps/percona_xtrabackup/percona-logo.svg ':size=250 :no-zoom')


> <i class="fa-solid fa-quote-left"></i> A complete and open source online backup solution for all versions of MySQL.
> 
> Ensure your applications remain fully available — and guarantee the consistency and safety of your data files — with **Percona XtraBackup**.
>
> Designed to make backups a seamless procedure without disrupting the performance of your server in a production environment, **Percona XtraBackup** empowers you to:
>
> - Complete backups quickly and reliably
> - Ensure uninterrupted transaction processing during backups
> - Save on disk space and network bandwidth
> - Automate backup verification
> - Benefit from faster restore time and higher uptime.
> 
> Key features included in **Percona XtraBackup** :
>
> - Create hot InnoDB backups without pausing your database
> - Make incremental backups of MySQL
> - Stream compressed MySQL backups to another server
> - Move tables between MySQL servers on-line
> - Create new MySQL replication replicas easily
> - Backup MySQL without adding load to the server
> - Backup locks are a lightweight alternative to FLUSH TABLES WITH READ LOCK available in Percona Server 5.6+. **Percona XtraBackup** uses them automatically to copy non-InnoDB data to avoid blocking DML queries that modify InnoDB tables.
> - **Percona XtraBackup** performs throttling based on the number of IO operations per second.
> - **Percona XtraBackup** skips secondary index pages and recreates them when a compact backup is prepared.
> - **Percona XtraBackup** can export individual tables even from a full backup, regardless of the InnoDB version.
> - Tables exported with **Percona XtraBackup** can be imported into Percona Server 5.1, 5.5 or 5.6+, or MySQL 5.6+. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://www.percona.com/mysql/software/percona-xtrabackup
- <i class="fa-solid fa-book"></i> **Documentation** : https://docs.percona.com/percona-xtrabackup/innovation-release/
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://docs.percona.com/percona-xtrabackup/innovation-release/release-notes/release-notes.html
- <i class="fa-brands fa-github"></i> **Github** : https://github.com/percona/percona-xtrabackup
- <i class="fab fa-blogger-b"></i> **Blog** : https://www.percona.com/blog/
- <i class="fas fa-comments"></i> **Forum** : https://forums.percona.com/

---

## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [Avoiding the “An optimized (without redo logging) DDL operation has been performed” Error with Percona XtraBackup](https://www.percona.com/blog/2017/08/08/avoiding-the-an-optimized-without-redo-logging-ddloperation-has-been-performed-error-with-percona-xtrabackup/)
- [Comparing Percona XtraBackup to MySQL Enterprise Backup: Part One](https://severalnines.com/database-blog/comparing-percona-xtrabackup-mysql-enterprise-backup-part-one)
- [Comparing S3 Streaming Tools with Percona XtraBackup](https://www.percona.com/blog/2019/11/26/comparing-s3-streaming-tools-with-percona-xtrabackup/)
- [Encrypting Our Backups: Making It to That Finish Line](https://dzone.com/articles/encrypting-our-backups-making-it-to-that-finish-li)
- [How to Restore a Single Table Using Percona Xtrabackup?](https://severalnines.com/database-blog/how-restore-single-table-using-percona-xtrabackup)
- [La sauvegarde de MySQL Server avec Percona Xtrabackup](https://www.dsfc.net/logiciel-libre/mysql-logiciel-libre/sauvegarde-mysql-server-percona-xtrabackup/)
- [MySQL Streaming Xtrabackup To Slave Recovery](https://blog.pythian.com/mysql-streaming-xtrabackup-slave-recovery/)
- [Percona XtraBackup Now Supports Dump of InnoDB Buffer Pool](https://www.percona.com/blog/2019/02/26/percona-xtrabackup-now-supports-dump-of-innodb-buffer-pool/)
- [Percona XtraBackup: Backup and Restore of a Single Table or Database](https://www.percona.com/blog/2020/04/10/percona-xtrabackup-backup-and-restore-of-a-single-table-or-database/)
- [Running XtraBackup on Windows using WSL](http://patg.net/mysql,windows/10,wsl,backups/2017/09/03/xtrabackup-wsl/)
- [Setting World-Writable File Permissions Prior to Preparing the Backup Can Break It](https://www.percona.com/blog/2019/07/01/setting-world-writable-file-permissions-prior-to-preparing-the-backup-can-break-it/)

</details>