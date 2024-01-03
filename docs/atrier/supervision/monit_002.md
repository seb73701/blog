---
title: Supervision - Elasticsearch
description: 
published: true
date: 2023-04-21T10:41:15.537Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:41:12.166Z
---

====== Supervision - Elasticsearch ======
----
{{:supervision:elasticsearch:elasticsearch-dashboard-final2.png?300&nolink|}}
----

===== Mise en supervision =====

Simple mais efficace.

Un check simple basé sur la commande ''curl'' d’état d'elasticsearch :

<code>
[root@server]# curl http://172.23.4.5:9200/_cat/health
1509719614 15:33:34 elasticsearch_prod green 2 2 5772 2886 0 0 0 0 - 100.0%
</code>

Avec ''check_http'' :

<code>
[root@server]# ./check_http -I SRV-BDD-01 -u /_cat/health?h=st -p 9200 -r 'red' --invert-regex
HTTP OK: HTTP/1.0 200 OK - 86 octets en 0,004 secondes de temps de réponse |time=0,004206s;;;0,000000 size=86B;;;0
</code>

<code>
[root@server]# ./check_http -I SRV-BDD-01 -u /_cat/health?h=st -p 9200 -r 'green'
HTTP OK: HTTP/1.0 200 OK - expression trouvée - 86 octets en 0,020 secondes de temps de réponse |time=0,019805s;;;0,000000 size=86B;;;0
</code>