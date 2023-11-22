---
title: Plugin > Input > Apache
description: 
published: true
date: 2023-04-21T10:50:09.399Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:50:05.230Z
---

====== Plugin > Input > Apache ======
----

Lien : [[https://github.com/influxdata/telegraf/blob/master/plugins/inputs/apache]]

Voici la liste des indicateurs disponibles avec le plugin :

^  Nom variable  ^  Informations  ^
|apache|Nom de l'input|
|host=localhost.localdomain|Hostname|
|port=80|Port de l'instance (Tag)|
|server=localhost|Hostname (Tag)|
|BusyWorkers=1|Le nombre de processus servant les requêtes|
|BytesPerReq=553.396|Les valeurs moyennes du nombre d'octets par requête|
|BytesPerSec=54.5961|Les valeurs moyennes du nombre d'octets servis par seconde|
|CPULoad=0.00167926|Le pourcentage CPU instantané par l'ensemble des processus|
|IdleWorkers=5|Le nombre de processus inactifs|
|ReqPerSec=0.0986566|Les valeurs moyennes du nombre de requêtes par seconde|
|TotalAccesses=235|Le nombre total d'accès effectués|
|TotalkBytes=127|Le nombre total d'octets servis|
|Uptime=2382|Le moment où le serveur a été démarré/redémarré et le temps écoulé depuis|
|scboard_closing=0||
|scboard_dnslookup=0||
|scboard_finishing=0||
|scboard_idle_cleanup=0||
|scboard_keepalive=0||
|scboard_logging=0||
|scboard_open=250||
|scboard_reading=0||
|scboard_sending=1||
|scboard_starting=0||
|scboard_waiting=5 1538324649000000000||

===== Module mod_status =====

Pour permettre à Telegraf de récupérer les informations d'Apache, il faut que le module ''mod_status'' soit activé dans la configuration d'Apache.

Voici comment vérifier que le module est chargé (ici actif):

<code>apachectl -M | grep status
module_status (shared)
</code>

Si ce n'est pas le cas, veuillez insérer les lignes suivantes dans le fichier de configuration d'Apache :

<code>[root@localhost ~]# vim /etc/httpd/conf/httpd.conf</code>

<code>
<Location "/server-status">
    SetHandler server-status
    Require ip 127.0.0.1
    Require local
</Location>
</code>

On vérifie que la configuration est correcte :

<code>[root@localhost ~]# apachectl -t
Syntax OK
</code>

On redémarre le service Apache :

<code>systemctl restart httpd</code>

On vérifie que le service est bien redémarré :

<code>[root@localhost ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: active (running) since jeu. 2018-09-27 15:40:36 CEST; 8s ago
     Docs: man:httpd(8)
           man:apachectl(8)
  Process: 2379 ExecStop=/bin/kill -WINCH ${MAINPID} (code=exited, status=0/SUCCESS)
 Main PID: 2384 (httpd)
   Status: "Processing requests..."
   CGroup: /system.slice/httpd.service
           ├─2384 /usr/sbin/httpd -DFOREGROUND
           ├─2385 /usr/sbin/httpd -DFOREGROUND
           ├─2386 /usr/sbin/httpd -DFOREGROUND
           ├─2387 /usr/sbin/httpd -DFOREGROUND
           ├─2388 /usr/sbin/httpd -DFOREGROUND
           └─2389 /usr/sbin/httpd -DFOREGROUND

sept. 27 15:40:36 localhost.localdomain systemd[1]: Starting The Apache HTTP Server...
sept. 27 15:40:36 localhost.localdomain systemd[1]: Started The Apache HTTP Server.
</code>

On vérifie que l'on a accès à la page en local :

<code>curl 127.0.0.1/server-status</code>

Ce qui donne :

{{:applications:telegraf:server-status.png|}}

Une autre commande permet d'avoir une vue plus compréhensible :

<code>curl 127.0.0.1/server-status?auto
Total Accesses: 4
Total kBytes: 10
Uptime: 634
ReqPerSec: .00630915
BytesPerSec: 16.1514
BytesPerReq: 2560
BusyWorkers: 1
IdleWorkers: 4
Scoreboard: __W__...........................................................................................................................................................................................................................................................
</code>

Il est possible d'ajouter plus d'informations sur cette dernière vue en ajoutant ''ExtentedStatus On'' dans le fichier de configuration ''httpd.conf'' (activé par défaut sur Apache 2.4).

Editer le fichier ''/etc/telegraf/telegraf.d/inputs.apache.conf'' :

<code>vim /etc/telegraf/telegraf.d/inputs.apache.conf</code>

Modifier le fichier à vos besoins :

<code ini>
# # Read Apache status information (mod_status)
[[inputs.apache]]
#   ## An array of URLs to gather from, must be directed at the machine
#   ## readable version of the mod_status page including the auto query string.
#   ## Default is "http://localhost/server-status?auto".
 urls = ["http://127.0.0.1/server-status?auto"]
#
#   ## Credentials for basic HTTP authentication.
#   # username = "myuser"
#   # password = "mypassword"
#
#   ## Maximum time to receive response.
 response_timeout = "5s"
#
#   ## Optional TLS Config
#   # tls_ca = "/etc/telegraf/ca.pem"
#   # tls_cert = "/etc/telegraf/cert.pem"
#   # tls_key = "/etc/telegraf/key.pem"
#   ## Use TLS but skip chain & host verification
#   # insecure_skip_verify = false
</code>

Redémarrer ensuite le service Telegraf :

<code>systemctl restart telegraf.service</code>

Pour tester la configuration, on va exécuter la commande suivante :

<code># telegraf --test --config-directory /etc/telegraf/telegraf.d/</code>


Voici le résultat :

<code>
apache,host=localhost.localdomain,port=80,server=localhost BusyWorkers=1,BytesPerReq=553.396,BytesPerSec=54.5961,CPULoad=0.00167926,IdleWorkers=5,ReqPerSec=0.0986566,TotalAccesses=235,TotalkBytes=127,Uptime=2382,scboard_closing=0,scboard_dnslookup=0,scboard_finishing=0,scboard_idle_cleanup=0,scboard_keepalive=0,scboard_logging=0,scboard_open=250,scboard_reading=0,scboard_sending=1,scboard_starting=0,scboard_waiting=5 1538324649000000000
</code>
