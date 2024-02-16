# InfluxDB

## 1. Architecture using InfluxDB relay

(voir photo)




## 2. Initialization

### 2.1. Create admin user

```shell
# influx
Connected to http://localhost:8086 version 1.3.1
InfluxDB shell version: 1.3.1
> CREATE USER <username> WITH PASSWORD '<password>' WITH ALL PRIVILEGES
```

### 2.2. Create user

```shell
# influx
Connected to http://localhost:8086 version 1.3.1
InfluxDB shell version: 1.3.1
> CREATE USER <username> WITH PASSWORD '<password>'
```

### 2.3. Create database

```shell
# influx
Connected to http://localhost:8086 version 1.3.1
InfluxDB shell version: 1.3.1
> CREATE DATABASE <name>
```

### 2.4. Grant privileges

```
# influx
Connected to http://localhost:8086 version 1.3.1
InfluxDB shell version: 1.3.1
> GRANT READ ON <database_name> TO <username>
> GRANT WRITE ON <database_name> TO <username>
> GRANT ALL ON <database_name> TO <username>
```

## 3. Retention policy and continuous queries for Telegraf database

Telegraf store all data in a database "telegraf". But, a specific retention policy and continuous queries must be defined in order to :

- Maintain a fair disk usage
- Keep a long retention of information

The idea is to keep 30 days of "raw" data in telegraf database. There is a point every 10 seconds in this database.

Then, we aggregate points every hour and keep that during 1 year.

And then we aggregate points every day and keep that during 10 years.

```shell
# influx
Connected to http://localhost:8086 version 1.6.2
InfluxDB shell version: 1.6.2
> auth
> create database telegraf_hourly
> create database telegraf_daily
> use telegraf
> create retention policy thirty_days on telegraf duration 30d replication 1 default
> create retention policy one_year on telegraf_hourly duration 365d replication 1 default
> create retention policy ten_years on telegraf_daily duration 3650d replication 1 default
> CREATE CONTINUOUS QUERY cq_telegraf_hourly ON telegraf BEGIN SELECT mean(*) INTO telegraf_hourly.one_year.:MEASUREMENT FROM telegraf.autogen./.*/ GROUP BY time(1h), * END
> CREATE CONTINUOUS QUERY cq_telegraf_daily ON telegraf_hourly BEGIN SELECT mean(*) INTO telegraf_daily.ten_years.:MEASUREMENT FROM telegraf_hourly.one_year./.*/ GROUP BY time(1d), * END
```

## 4. Usage

### 4.1. Use database

```shell
> use VRC_STAT
Using database VRC_STAT
```

### 4.2. Display measurements

```shell
> SHOW MEASUREMENTS
name: measurements
name
----
banner
black
bounce
ce
content
dce
dkim
drop
eodata
grey
helo
hspam
legit
lspam
mailfrom
mce
mime
mspam
pce
quarantine
rbl
rcptto
reject
route
size
social
spf
tag
virus
white
```

We can compare measurements to MySQL tables


### 4.3. Show series

```shell
> SELECT * FROM bounce LIMIT 1
name: bounce
time domain full oem stream user
---- ------ ---- --- ------ ----
1507098420000000000 verdi-ingenierie.fr 1 OEM0002 IN VRC012588
```

### 4.4. Display continuous queries

```shell
> SHOW CONTINUOUS QUERIES
name: _internal
name query
---- -----
name: VRC_STAT
name query
---- -----
point5m_full CREATE CONTINUOUS QUERY point5m_full ON VRC_STAT BEGIN SELECT sum(full) AS full INTO AGG5M.point5m.:MEASUREMENT FROM VRC_STAT.all15d./["black"|"white"|"bounce"|"mce"|"dce"|"pce"|"drop"|"hspam"|"mspam"|"lspam"|"legit"|"quarantine"|"reject"|"route"|"social"|"tag"|"virus"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(5m) END
point5m_245 CREATE CONTINUOUS QUERY point5m_245 ON VRC_STAT BEGIN SELECT sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGG5M.point5m.:MEASUREMENT FROM VRC_STAT.all15d./["rcptto"|"mailfrom"|"helo"|"eodata"|"content"|"banner"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(5m) END
point5m_match CREATE CONTINUOUS QUERY point5m_match ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match INTO AGG5M.point5m.:MEASUREMENT FROM VRC_STAT.all15d./["dkim"|"mime"|"rbl"|"size"|"spf"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(5m) END
point1h_full CREATE CONTINUOUS QUERY point1h_full ON VRC_STAT BEGIN SELECT sum(full) AS full INTO AGG1H.point1h.:MEASUREMENT FROM VRC_STAT.all15d./["black"|"white"|"bounce"|"mce"|"dce"|"pce"|"drop"|"hspam"|"mspam"|"lspam"|"legit"|"quarantine"|"reject"|"route"|"social"|"tag"|"virus"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1h) END
point1h_245 CREATE CONTINUOUS QUERY point1h_245 ON VRC_STAT BEGIN SELECT sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGG1H.point1h.:MEASUREMENT FROM VRC_STAT.all15d./["rcptto"|"mailfrom"|"helo"|"eodata"|"content"|"banner"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1h) END
point1h_match CREATE CONTINUOUS QUERY point1h_match ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match INTO AGG1H.point1h.:MEASUREMENT FROM VRC_STAT.all15d./["dkim"|"mime"|"rbl"|"size"|"spf"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1h) END
point1d_full CREATE CONTINUOUS QUERY point1d_full ON VRC_STAT BEGIN SELECT sum(full) AS full INTO AGG1D.point1d.:MEASUREMENT FROM VRC_STAT.all15d./["black"|"white"|"bounce"|"mce"|"dce"|"pce"|"drop"|"hspam"|"mspam"|"lspam"|"legit"|"quarantine"|"reject"|"route"|"social"|"tag"|"virus"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1d) END
point1d_245 CREATE CONTINUOUS QUERY point1d_245 ON VRC_STAT BEGIN SELECT sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGG1D.point1d.:MEASUREMENT FROM VRC_STAT.all15d./["rcptto"|"mailfrom"|"helo"|"eodata"|"content"|"banner"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1d) END
point1d_match CREATE CONTINUOUS QUERY point1d_match ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match INTO AGG1D.point1d.:MEASUREMENT FROM VRC_STAT.all15d./["dkim"|"mime"|"rbl"|"size"|"spf"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1d) END
point1w_full CREATE CONTINUOUS QUERY point1w_full ON VRC_STAT BEGIN SELECT sum(full) AS full INTO AGG1W.point1w.:MEASUREMENT FROM VRC_STAT.all15d./["black"|"white"|"bounce"|"mce"|"dce"|"pce"|"drop"|"hspam"|"mspam"|"lspam"|"legit"|"quarantine"|"reject"|"route"|"social"|"tag"|"virus"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1w) END
point1w_245 CREATE CONTINUOUS QUERY point1w_245 ON VRC_STAT BEGIN SELECT sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGG1W.point1w.:MEASUREMENT FROM VRC_STAT.all15d./["rcptto"|"mailfrom"|"helo"|"eodata"|"content"|"banner"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1w) END
point1w_match CREATE CONTINUOUS QUERY point1w_match ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match INTO AGG1W.point1w.:MEASUREMENT FROM VRC_STAT.all15d./["dkim"|"mime"|"rbl"|"size"|"spf"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(1w) END
point1y_full CREATE CONTINUOUS QUERY point1y_full ON VRC_STAT BEGIN SELECT sum(full) AS full INTO AGG1Y.point1y.:MEASUREMENT FROM VRC_STAT.all15d./["black"|"white"|"bounce"|"mce"|"dce"|"pce"|"drop"|"hspam"|"mspam"|"lspam"|"legit"|"quarantine"|"reject"|"route"|"social"|"tag"|"virus"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(365d) END
point1y_245 CREATE CONTINUOUS QUERY point1y_245 ON VRC_STAT BEGIN SELECT sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGG1Y.point1y.:MEASUREMENT FROM VRC_STAT.all15d./["rcptto"|"mailfrom"|"helo"|"eodata"|"content"|"banner"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(365d) END
point1y_match CREATE CONTINUOUS QUERY point1y_match ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match INTO AGG1Y.point1y.:MEASUREMENT FROM VRC_STAT.all15d./["dkim"|"mime"|"rbl"|"size"|"spf"]/ WHERE time > '2016-01-01 00:00:00' GROUP BY domain, "user", oem, stream, time(365d) END
pointhome CREATE CONTINUOUS QUERY pointhome ON VRC_STAT BEGIN SELECT sum(full) AS full, sum(match) AS match, sum("2XX") AS "2XX", sum("4XX") AS "4XX", sum("5XX") AS "5XX" INTO AGGHOME.pointhome.:MEASUREMENT FROM VRC_STAT.all15d./.*/ WHERE time > '2016-01-01 00:00:00' GROUP BY "user", oem, stream, time(1m) END
name: AGG5M
name query
---- -----
name: AGG1H
name query
---- -----
name: AGG1D
name query
---- -----
name: AGG1W
name query
---- -----
name: AGG1Y
name query
---- -----
name: AGGHOME
name query
---- -----
```

```shell
> SHOW DIAGNOSTICS [1/1913]
name: build
Branch Build Time Commit Version
------ ---------- ------ -------
1.3 a7c3d3c0f7b74f71919047adbc4933460967a576 1.3.6
name: config
bind-address reporting-disabled
------------ ------------------
127.0.0.1:8088 false
name: config-coordinator
log-queries-after max-concurrent-queries max-select-buckets max-select-point max-select-series query-timeout write-timeout
----------------- ---------------------- ------------------ ---------------- ----------------- ------------- -------------
0s 0 0 0 0 0s 10s
name: config-cqs
enabled run-interval
------- ------------
true 1s
name: config-data
cache-max-memory-size cache-snapshot-memory-size cache-snapshot-write-cold-duration compact-full-write-cold-duration dir max-concurrent-compactions max-series-per-database max-values-per-tag wal-dir wal-fsync-delay
--------------------- -------------------------- ---------------------------------- -------------------------------- --- -------------------------- ----------------------- ------------------ ------- ---------------
1073741824 26214400 10m0s 4h0m0s /var/lib/influxdb/data 0 1000000 100000 /var/lib/influxdb/wal 0s
name: config-httpd
bind-address enabled https-enabled max-connection-limit max-row-limit
------------ ------- ------------- -------------------- -------------
0.0.0.0:8086 true false 0 10000
name: config-meta
dir
---
/var/lib/influxdb/meta
name: config-monitor
store-database store-enabled store-interval
-------------- ------------- --------------
_internal true 10s
name: config-precreator
advance-period check-interval enabled
-------------- -------------- -------
30m0s 10m0s true
name: config-retention
check-interval enabled
-------------- -------
30m0s true
name: config-subscriber
enabled http-timeout write-buffer-size write-concurrency
------- ------------ ----------------- -----------------
true 30s 1000 40
name: network
hostname
--------
vcfr1influxdb01p.vadesecure-cloud.local
name: runtime
GOARCH GOMAXPROCS GOOS version
------ ---------- ---- -------
amd64 4 linux go1.8.3
 
name: system
PID currentTime started uptime
--- ----------- ------- ------
104376 2017-10-12T07:46:24.111037293Z 2017-10-05T11:58:06.955647362Z 163h48m17.155390167s
```

### 4.5. Display influxdb statistics

```shell
SHOW STATS
....
name: write
pointReq pointReqLocal req subWriteDrop subWriteOk writeDrop writeError writeOk writeTimeout
-------- ------------- --- ------------ ---------- --------- ---------- ------- ------------
271044800 271044800 92360 0 92360 0 0 92296 0
name: subscriber
createFailures pointsWritten writeFailures
-------------- ------------- -------------
0 0 0
name: cq
queryFail queryOk
--------- -------
0 16059
name: httpd
tags: bind=0.0.0.0:8086
authFail clientError pingReq pointsWrittenDropped pointsWrittenFail pointsWrittenOK queryReq queryReqDurationNs queryRespBytes req reqActive reqDurationNs serverError statusReq writeReq writeReqActive writeReqBytes writeReqDurationNs
-------- ----------- ------- -------------------- ----------------- --------------- -------- ------------------ -------------- --- --------- ------------- ----------- --------- -------- -------------- ------------- ------------------
7 43 16 0 0 113313703 62328 29794499590094 169953818 72241 1 31748027351861 0 0 9890 0 9998645216 1896039863188
```

## 5. Check data consistency

Sometimes you want to ensure that data contains in InfluxDB is coherent. Here are some commands.

### 5.1. Check data consistency

```shell
> precision rfc3339
> -- Analyse content of VRC_STAT database
> use VRC_STAT
> select sum(full) from legit where time >= now() - 24h group by time(1h)
name: legit
time                 sum
----                 ---
2017-12-28T15:00:00Z 35379
2017-12-28T16:00:00Z 65860
2017-12-28T17:00:00Z 37697
2017-12-28T18:00:00Z 23874
2017-12-28T19:00:00Z 19439
2017-12-28T20:00:00Z 18762
2017-12-28T21:00:00Z 17459
2017-12-28T22:00:00Z 15006
2017-12-28T23:00:00Z 15517
2017-12-29T00:00:00Z 13113
2017-12-29T01:00:00Z 12376
2017-12-29T02:00:00Z 14742
2017-12-29T03:00:00Z 28222
2017-12-29T04:00:00Z 12024
2017-12-29T05:00:00Z 15391
2017-12-29T06:00:00Z 22973
2017-12-29T07:00:00Z 52338
2017-12-29T08:00:00Z 73129
2017-12-29T09:00:00Z 80995
2017-12-29T10:00:00Z 82438
2017-12-29T11:00:00Z 51805
2017-12-29T12:00:00Z 40279
2017-12-29T13:00:00Z 61521
2017-12-29T14:00:00Z 62674
2017-12-29T15:00:00Z 34607
 
 
> -- Analyse content of AGG5M database. First and last entry are different, but rest should be the same.
> use AGG5M
> select sum(full) from legit where time >= now() - 24h group by time(1h)
name: legit
time                 sum
----                 ---
2017-12-28T15:00:00Z 31055
2017-12-28T16:00:00Z 65860
2017-12-28T17:00:00Z 37697
2017-12-28T18:00:00Z 23874
2017-12-28T19:00:00Z 19439
2017-12-28T20:00:00Z 18762
2017-12-28T21:00:00Z 17459
2017-12-28T22:00:00Z 15006
2017-12-28T23:00:00Z 15517
2017-12-29T00:00:00Z 13113
2017-12-29T01:00:00Z 12376
2017-12-29T02:00:00Z 14742
2017-12-29T03:00:00Z 28222
2017-12-29T04:00:00Z 12024
2017-12-29T05:00:00Z 15391
2017-12-29T06:00:00Z 22973
2017-12-29T07:00:00Z 52338
2017-12-29T08:00:00Z 73129
2017-12-29T09:00:00Z 80995
2017-12-29T10:00:00Z 82438
2017-12-29T11:00:00Z 51805
2017-12-29T12:00:00Z 40279
2017-12-29T13:00:00Z 61521
2017-12-29T14:00:00Z 62674
2017-12-29T15:00:00Z 35730
> -- Repeat with databases AGG1H and SYSTEMDB.
> -- To check content of database AGG1D, do the same query with "where time >= now() - 7d group by time(1d)"
```

## 6. Replay continuous queries

If consistency between databases is not correct, it means that some continuous queries have not been played correctly. In that case you need to replay them manually. A script has been created for that and is available on Gitlab: https://gitlab.com/devops/misc-scripts#replay-influx-cq

You need to run script script on all InfluxDB servers. If you run it on 1 server only, data will be different between this server and the others.

## 7. Monitoring


We check InfluxDB and InfluxDB relay answer from monitoring server with the following scripts : 

- `/usr/lib/nagios/plugins/check_influx.py`
- `/usr/lib/nagios/plugins/check_influx-relay.py`

> User `monit` is created manually with READ access on _internal database.

The scripts send back some metrics in perfdata format. Only 2 states are implemented **OK** and **CRITICAL**.

`check_influx.py` display metrics about queries and influxdb resources usage.

`check_influx-relay.py` display metrics about writes usage.


### 7.1. Check script usage

```shell
# ./check_influx.py -H influxdb01p -u monit -p <PASSWORD> -P 8086 
OK|queryOK=15963.166667, queryFail=0.000000, HeapReleased=7696358331.733334, HeapAlloc=4993586222.133333, HeapSys=15257642598.400000, HeapIdle=9543669896.533333, HeapInUse=5713972701.866667, HeapObjects=46061825.166667, PauseTotalNs=419729798728.933350, Alloc=4993586222.133333, Frees=536746652077.833313, Sys=16436238224.000000, Lookups=3743039.333333, NumGoroutine=71572.566667, NumGC=16678.200000, Mallocs=536792713903.000000, TotalAlloc=50062709992165.867188
 
# ./check_influx-relay.py -H influxdb01p -u monit -p <PASSWORD> -P 8086 
OK|req=91730.766667, writeOk=91666.766667, writeError=0.000000, writeDrop=0.000000, writeTimeout=0.000000, subWriteOk=91730.766667, subWriteDrop=0.000000, pointReq=267855526.866667, pointReqLocal=267855526.866667
```

## 8. Backup

The backup script, `/usr/local/sbin/influx_backup`. Backup go to `/backup` folder.

Each database is backuped and the metastore which contain internal information about the status of the system, including user information, database/shard metadata, CQs, RPs, and subscriptions.


### 8.1. Backup folder

```shell
# ls /usr/local/sbin/influx_backup base1.influx.tar.gz  base2.influx.tar.gz  base3.influx.tar.gz  base4.influx.tar.gz  base5.influx.tar.gz  base6.influx.tar.gz  meta.00  base7.influx.tar.gz
```

## 9. Restoration

We have a custom script, `influx_restore`, to restore InfluxDB Databases (One or several).

### 9.1. influx_restore --help

```shell
Usage: ./influx_restore [-h] [--all|--database NAME] [--meta] FOLDER
Restore InfluxDB database from backup folder.
NOTE:
The folder given in parameter must contain meta file and databases backup folder or tar archive.
OPTIONS:
-h --help : Show this help message and exit
-a --all-db : Restore all databases from folder
-d --database NAME : Restore only specified database (can be used several times)
-m --meta : Restore meta file
EXEMPLE: ./influx_restore -a /backup
ls /backup
base1.influx.tar.gz base2.influx.tar.gz base3.influx.tar.gz meta.00 SYSTEMDB.influx.tar.gz base4.influx.tar.gz
```

### 9.2. Exemples

```
# Restore Meta Only
influx_restore --meta /backup/
 
# Restore One or Several database
influx_restore -d VRC_STAT -d AGG1M /backup/
 
# Restore everything
influx_restore -a -m /backup/
```

Restart InfluxDB after restoring data. Note that it is necessary in case of continuous queries restoration.


Find more informations here : https://docs.influxdata.com/influxdb/v1.3/administration/backup_and_restore/#restore

### 9.1. Metastore


#### 9.1. Metastore restoration

```shell
influxd restore -metadir /var/lib/influxdb/meta <path-to-backup>
```

### 9.2. Database


#### 9.2. Database restoration

```shell
gzip -d <path-to-backup>
influxd restore -datadir /var/lib/influxdb/data <path-to-backup>
```