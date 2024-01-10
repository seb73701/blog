# PGPOOL
----

## <i class="fa-solid fa-hashtag"></i> Information

![Logo](../../_media/apps/pgpool/pgpool-II_logo.png ':size=250 :no-zoom')


> <i class="fa-solid fa-quote-left"></i> **Pgpool-II** is a middleware that works between PostgreSQL servers and a PostgreSQL database client. It is distributed under a license similar to BSD and MIT. It provides the following features.
>
> - Connection Pooling : **Pgpool-II** saves connections to the PostgreSQL servers, and reuse them whenever a new connection with the same properties (i.e. username, database, protocol version) comes in. It reduces connection overhead, and improves system's overall throughput.
> - Replication : **Pgpool-II** can manage multiple PostgreSQL servers. Using the replication function enables creating a realtime backup on 2 or more physical disks, so that the service can continue without stopping servers in case of a disk failure.
> - Load Balancing : If a database is replicated, executing a SELECT query on any server will return the same result. **Pgpool-II** takes an advantage of the replication feature to reduce the load on each PostgreSQL server by distributing SELECT queries among multiple servers, improving system's overall throughput. At best, performance improves proportionally to the number of PostgreSQL servers. Load balance works best in a situation where there are a lot of users executing many queries at the same time.
> - Limiting Exceeding Connections : There is a limit on the maximum number of concurrent connections with PostgreSQL, and connections are rejected after this many connections. Setting the maximum number of connections, however, increases resource consumption and affect system performance. **pgpool-II** also has a limit on the maximum number of connections, but extra connections will be queued instead of returning an error immediately.
> - Watchdog : Watchdog can coordinate multiple **Pgpool-II**, create a robust cluster system and avoid the single point of failure or split brain. Watchdog can perform lifecheck against other **pgpool-II** nodes, to detect a fault of Pgpoll-II. If active **Pgpool-II** goes down, standby **Pgpool-II** can be promoted to active, and take over Virtual IP.
> - In Memory Query Cache : In memory query cache allows to save a pair of SELECT statement and its result. If an identical SELECTs comes in, **Pgpool-II** returns the value from cache. Since no SQL parsing nor access to PostgreSQL are involved, using in memory cache is extremely fast. On the other hand, it might be slower than the normal path in some cases, because it adds some overhead of storing cache data.
>
> **Pgpool-II** speaks PostgreSQL's backend and frontend protocol, and relays messages between a backend and a frontend. Therefore, a database application (frontend) thinks that **Pgpool-II** is the actual PostgreSQL server, and the server (backend) sees **Pgpool-II** as one of its clients. Because **Pgpool-II** is transparent to both the server and the client, an existing database application can be used with **Pgpool-II** almost without a
>
> **Pgpool-II** talks PostgreSQL's backend and frontend protocol, and relays a connection between them. Therefore, a database application (frontend) thinks that **Pgpool-II** is the actual PostgreSQL server, and the server (backend) sees **Pgpool-II** as one of its clients. Because **Pgpool-II** is transparent to both the server and the client, an existing database application can be used with **Pgpool-II** almost without a change to its sources. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://www.pgpool.net/
- <i class="fa-solid fa-book"></i> **Documentation** : https://www.pgpool.net/mediawiki/index.php/Documentation
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://www.pgpool.net/docs/45/en/html/release-4-5-0.html
- <i class="fa-brands fa-github"></i> **Github** : https://git.postgresql.org/git/pgpool2.git
- <i class="far fa-question-circle"></i> **FAQ** : https://www.pgpool.net/mediawiki/index.php/FAQ

---

## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [A Guide to Pgpool for PostgreSQL - Part One](https://severalnines.com/blog/guide-pgpool-postgresql-part-one)
- [A Guide to Pgpool for PostgreSQL - Part Two](https://severalnines.com/blog/guide-pgpool-postgresql-part-two)
- [Avoiding connection failure while health checking](http://pgsqlpgpool.blogspot.com/2016/06/avoiding-connection-failure-while.html)
- [Avoiding session disconnection while fail over](http://pgsqlpgpool.blogspot.com/2016/07/avoiding-session-disconnection-while.html)
- [Creating a pgpool-II based PostgreSQL Cluster](https://archyslife.blogspot.com/2017/08/creating-pgpool-ii-based-postgresql.html)
- [Detecting "false" primary server of PostgreSQL](http://pgsqlpgpool.blogspot.com/2018/04/detecting-false-primary-server-of.html)
- [Even more fine load balancing control with Pgpool-II 4.0](http://pgsqlpgpool.blogspot.com/2018/06/even-more-fine-load-balancing-control.html)
- [Health check parameters](http://pgsqlpgpool.blogspot.com/2013/09/health-check-parameters.html)
- [More load balancing fine control](http://pgsqlpgpool.blogspot.com/2018/04/more-load-balancing-fine-control.html)
- [Welcome to Pgpool Wiki!](http://pgpool.net/mediawiki/index.php/Main_Page)

</details>