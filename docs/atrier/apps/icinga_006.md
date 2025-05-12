# Package "nagios_plugins_all"

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-08-22</span>

----


## Informations

+ *Site Officiel (en) : https://nagios-plugins.org/*
+ *Documentation (en) : https://www.monitoring-plugins.org/doc/man/index.html*
+ *Documentation (en) : http://nagios-plugins.org/doc/man/index.html*
+ *Github (en) : https://github.com/nagios-plugins/nagios-plugins*

## Liste des plugins "check_xxx"

Le package contient un certains nombre de plugins qui permet de superviser des serveurs, des équipements réseaux, des applications etc.

Certains plugins s'installe en fonction de l'OS. Par exemple, le plugin `check_apt` ne s'installe pas sur un système CentOS.

Il contient ces plugins :

| Commande            | Nom du script    | Langage   | Documentation | Description |
| ------------------- | ---------------- | :-------: | :-----------: | ----------- |
| **<span style="color:brown;">check_apt*</span>**       | `check_apt`      |           |[lien](http://nagios-plugins.org/doc/man/check_apt.html)|This plugin checks for software updates on systems that use package management systems based on the apt-get(8) command found in Debian GNU/Linux|
| **check_breeze**    | `check_breeze`   |Perl|[lien](http://nagios-plugins.org/doc/man/check_breeze.html)|This plugin reports the signal strength of a Breezecom wireless equipment|
| **check_by_ssh**|`check_by_ssh`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_by_ssh.html)|This plugin uses SSH to execute commands on a remote host|
| **check_clamd**|`check_clamd` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_clamd.html)|This plugin tests CLAMD connections with the specified host (or unix socket).|
| **check_cluster**|`check_cluster`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_cluster.html)|Host/Service Cluster Plugin for Nagios 2|
| **check_dhcp**|`check_dhcp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_dhcp.html)|This plugin tests the availability of DHCP servers on a network.|
| **check_dig**|`check_dig`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_dig.html)|This plugin test the DNS service on the specified host using dig|
| **check_disk**|`check_disk`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_disk.html)|This plugin checks the amount of used disk space on a mounted file system and generates an alert if free space is less than one of the threshold values|
| **check_disk_smb**|`check_disk_smb`|Perl|[lien](http://nagios-plugins.org/doc/man/check_disk_smb.html)|Perl Check SMB Disk plugin for Nagios|
| **check_dns**|`check_dns`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_dns.html)|This plugin uses the nslookup program to obtain the IP address for the given host/domain query. An optional DNS server to use may be specified. If no DNS server is specified, the default server(s) specified in /etc/resolv.conf will be used.|
| **check_dummy**|`check_dummy`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_dummy.html)|This plugin will simply return the state corresponding to the numeric value of the <state> argument with optional text|
| **check_file_age**|`check_file_age`|Perl|[lien](http://nagios-plugins.org/doc/man/check_file_age.html)|This plugin checks file age|
| **check_flexlm**|`check_flexlm`|Perl|[lien](http://nagios-plugins.org/doc/man/check_flexlm.html)|Check available flexlm license managers|
| **check_fping**|`check_fping`|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_fping.html)|This plugin will use the `fping` command to ping the specified host for a fast check. Note that it is necessary to set the suid flag on `fping`.|
| **check_ftp**|`check_ftp` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ftp.html)|This plugin tests FTP connections with the specified host (or unix socket).|
| **check_hpjd**|`check_hpjd`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_hpjd.html)|This plugin tests the STATUS of an HP printer with a JetDirect card. Net-snmp must be installed on the computer running the plugin.|
| **check_http**|`check_http`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_http.html)|This plugin tests the HTTP service on the specified host. It can test normal (http) and secure (https) servers, follow redirects, search for strings and regular expressions, check connection times, and report on certificate expiration times.|
| **check_icmp**|`check_icmp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_icmp.html)|Contrairement à check_ping, check_icmp permet de vérifier plusieurs hôtes une seule fois avec plus de personnalisation et il génère également des données de performance. Mais la principale différence est que check_ping exécute la commande système ping et analyse sa sortie alors que le plugin check_icmp utilise ICMP lui-même et sa performance est meilleure. Ce plugin nécessite d’activer le bit suid (chmod 755) pour fonctionner correctement avec le compte utilisateur nagios ou en utilisant la commande sudo.|
| **check_ide_smart**|`check_ide_smart`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ide_smart.html)|This plugin checks a local hard drive with the (Linux specific) SMART interface|
| **<span style="color:brown;">check_ifoperstatus*</span>**|`check_ifoperstatus`||[lien](https://www.monitoring-plugins.org/doc/man/check_ifoperstatus.html)|check_ifoperstatus plugin for monitoring operational status of a particular network interface on the target host|
| **<span style="color:brown;">check_ifstatus*</span>**|`check_ifstatus`||[lien](https://www.monitoring-plugins.org/doc/man/check_ifstatus.html)|check_ifstatus plugin for monitoring operational status of each network interface on the target host|
| **check_imap**|`check_imap` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_imap.html)|This plugin tests IMAP connections with the specified host (or unix socket).|
| **check_ircd**|`check_ircd`|Perl|[lien](http://nagios-plugins.org/doc/man/check_ircd.html)|Perl Check IRCD plugin for Nagios|
| **check_jabber**|`check_jabber` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_jabber.html)|This plugin tests JABBER connections with the specified host (or unix socket).|
| **check_ldap**|`check_ldap`|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_ldap.html)|Check LDAP|
| **check_ldaps**|`check_ldaps` (`check_ldap`)|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_ldaps.html)|Check LDAPS|
| **check_load**|`check_load`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_load.html)|This plugin tests the current system load average.|
| **check_log**|`check_log`|Shell Script|[lien](http://nagios-plugins.org/doc/man/check_log.html)|Log file pattern detector plugin for Nagios|
| **check_mailq**|`check_mailq`|Perl|[lien](http://nagios-plugins.org/doc/man/check_mailq.html)|Checks the number of messages in the mail queue (supports multiple sendmail queues, qmail) Feedback/patches to support non-sendmail mailqueue welcome|
| **check_mrtg**|`check_mrtg`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_mrtg.html)|This plugin will check either the average or maximum value of one of the two variables recorded in an MRTG log file.|
| **check_mrtgtraf**|`check_mrtgtraf`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_mrtgtraf.html)|This plugin will check the incoming/outgoing transfer rates of a router, switch, etc recorded in an MRTG log.  If the newest log entry is older than <expire_minutes>, a WARNING status is returned. If either the incoming or outgoing rates exceed the <icl> or <ocl> thresholds (in Bytes/sec), a CRITICAL status results.  If either of the rates exceed the <iwl> or <owl> thresholds (in Bytes/sec), a WARNING status results.|
| **check_mysql**|`check_mysql`|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_mysql.html)|This program tests connections to a MySQL server|
| **check_mysql_query**|`check_mysql_query`|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_mysql_query.html)|This program checks a query result against threshold levels|
| **check_nagios**|`check_nagios`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_nagios.html)|This plugin checks the status of the Nagios process on the local machine. The plugin will check to make sure the Nagios status log is no older than the number of minutes specified by the expires option. It also checks the process table for a process matching the command argument.|
| **check_nntp**|`check_nntp` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_nntp.html)|This plugin tests NNTP connections with the specified host (or unix socket).|
| **check_nntps**|`check_nntps (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_nntps.html)|This plugin tests NNTPS connections with the specified host (or unix socket).|
| **check_nscp_api**|`check_nscp_api`|C/C++|[lien](https://docs.nsclient.org/api/)|NSClient++ API|
| **check_nt**|`check_nt`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_nt.html)|This plugin collects data from the NSClient service running on a Windows NT/2000/XP/2003 server.|
| **check_ntp**|`check_ntp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ntp.html)|This plugin checks the selected ntp server. <span style="color:red;">WARNING: `check_ntp` is deprecated. Please use `check_ntp_peer` or `check_ntp_time` instead.</span>|
| **check_ntp_peer**|`check_ntp_peer`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ntp_peer.html)|This plugin checks the selected ntp server|
| **check_ntp_time**|`check_ntp_time`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ntp_time.html)|This plugin checks the clock offset with the ntp server|
| **check_nwstat**|`check_nwstat`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_nwstat.html)|This plugin attempts to contact the MRTGEXT NLM running on a Novell server to gather the requested system information.|
| **check_oracle**|`check_oracle`|Shell Script|[lien](http://nagios-plugins.org/doc/man/check_oracle.html)|Check Oracle status|
| **check_overcr**|`check_overcr`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_overcr.html)|This plugin attempts to contact the Over-CR collector daemon running on the remote UNIX server in order to gather the requested system information.|
| **check_pgsql**|`check_pgsql`|C/C++|[lien](https://www.monitoring-plugins.org/doc/man/check_pgsql.html)|Test whether a PostgreSQL Database is accepting connections.|
| **check_ping**|`check_ping`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ping.html)|Use ping to check connection statistics for a remote host.|
| **check_pop**|`check_pop` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_pop.html)|This plugin tests POP connections with the specified host (or unix socket).|
| **check_procs**|`check_procs`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_procs.html)|Checks all processes and generates WARNING or CRITICAL states if the specified metric is outside the required threshold ranges. The metric defaults to number of processes.  Search filters can be applied to limit the processes to check.|
| **check_real**|`check_real`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_real.html)|This plugin tests the REAL service on the specified host.|
| **check_rpc**|`check_rpc`|Perl|[lien](http://nagios-plugins.org/doc/man/check_rpc.html)|Check if a `rpc` service is registered and running using `rpcinfo -H host -C rpc_command`|
| **check_sensors**|`check_sensors`|Shell Script|[lien](http://nagios-plugins.org/doc/man/check_sensors.html)|This plugin checks hardware status using the lm_sensors package.|
| **check_simap**|`check_simap` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_simap.html)|This plugin tests SIMAP connections with the specified host (or unix socket).|
| **check_smtp**|`check_smtp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_smtp.html)|This plugin will attempt to open an SMTP connection with the host.|
| **check_snmp**|`check_snmp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_snmp.html)|Check status of remote machines and obtain system information via SNMP|
| **check_spop**|`check_spop` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_spop.html)|This plugin tests SPOP connections with the specified host (or unix socket).|
| **check_ssh**|`check_ssh`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ssh.html)|Try to connect to an SSH server at specified server and port|
| **check_ssl_validity**|`check_ssl_validity`|Perl|[lien](http://nagios-plugins.org/doc/man/check_ssl_validity.html)|check for valid SSL certificate|
| **check_ssmtp**|`check_ssmtp` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ssmtp.html)|This plugin tests SSMTP connections with the specified host (or unix socket).|
| **check_swap**|`check_swap`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_swap.html)|Check swap space on local machine.|
| **check_tcp**|`check_tcp`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_tcp.html)|This plugin tests TCP connections with the specified host (or unix socket).|
| **check_time**|`check_time`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_time.html)|This plugin will check the time on the specified host.|
| **check_udp**|`check_udp` (`check_tcp`)|C/C++|[lien](http://nagios-plugins.org/doc/man/check_udp.html)|This plugin tests UDP connections with the specified host (or unix socket).|
| **check_ups**|`check_ups`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_ups.html)|This plugin tests the UPS service on the specified host. Network UPS Tools from www.networkupstools.org must be running for this plugin to work.|
| **<span style="color:brown;">check_uptime*</span>**|`check_uptime`|???|[lien](http://nagios-plugins.org/doc/man/check_uptime.html)|This plugin checks the system uptime and alerts if more than the threshold. Threshold unit of measurement specified with "-u".|
| **check_users**|`check_users`|C/C++|[lien](http://nagios-plugins.org/doc/man/check_users.html)|This plugin checks the number of users currently logged in on the local system and generates an error if the number exceeds the thresholds specified.|
| **check_wave**|`check_wave`|Perl|[lien](http://nagios-plugins.org/doc/man/check_wave.html)|???|
| **negate**|`negate`|C/C++|[lien](http://nagios-plugins.org/doc/man/negate.html)|Negates the status of a plugin (returns OK for CRITICAL and vice-versa). Additional switches can be used to control which state becomes what.|
| **urlize**|`urlize`|C/C++|[lien](http://nagios-plugins.org/doc/man/urlize.html)|This plugin wraps the text output of another command (plugin) in HTML <A> tags, thus displaying the child plugin's output as a clickable link in the Nagios status screen.  This plugin returns the status of the invoked plugin.|

**<span style="color:brown;">check_xxx*</span>** = s'installe en fonction de l'OS. 

----

## Installation du package "nagios_plugins_all"


<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
dnf install nagios-plugins-all
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
[root@icinga-l-master01 ~ ]#  dnf install nagios-plugins-all
Dernière vérification de l’expiration des métadonnées effectuée il y a 0:05:19 le dim. 23 août 2020 08:52:38 CEST.
Dépendances résolues.
=============================================================================================================================================================================================================
 Paquet                                                  Architecture                       Version                                                             Dépôt                                  Taille
=============================================================================================================================================================================================================
Installation:
 nagios-plugins-all                                      x86_64                             2.3.3-4.el8                                                         epel                                    24 k
Installation des dépendances:
 bind-libs                                               x86_64                             32:9.11.13-5.el8_2                                                  AppStream                              171 k
 bind-libs-lite                                          x86_64                             32:9.11.13-5.el8_2                                                  AppStream                              1.2 M
 bind-license                                            noarch                             32:9.11.13-5.el8_2                                                  AppStream                              100 k
 bind-utils                                              x86_64                             32:9.11.13-5.el8_2                                                  AppStream                              443 k
 esmtp                                                   x86_64                             1.2-15.el8                                                          epel                                    57 k
 fping                                                   x86_64                             4.2-2.el8                                                           epel                                    43 k
 libesmtp                                                x86_64                             1.0.6-18.el8                                                        epel                                    70 k
 liblockfile                                             x86_64                             1.14-1.el8                                                          AppStream                               32 k
 libmaxminddb                                            x86_64                             1.2.0-7.el8                                                         AppStream                               25 k
 libpq                                                   x86_64                             12.1-3.el8                                                          AppStream                              195 k
 lm_sensors                                              x86_64                             3.4.0-21.20180522git70f7e08.el8                                     BaseOS                                 151 k
 lm_sensors-libs                                         x86_64                             3.4.0-21.20180522git70f7e08.el8                                     BaseOS                                  59 k
 mariadb-connector-c-config                              noarch                             3.0.7-1.el8                                                         AppStream                               13 k
 mysql-common                                            x86_64                             8.0.17-3.module_el8.0.0+181+899d6349                                AppStream                              143 k
 mysql-libs                                              x86_64                             8.0.17-3.module_el8.0.0+181+899d6349                                AppStream                              1.2 M
 nagios-common                                           x86_64                             4.4.5-1.el8                                                         epel                                    30 k
 nagios-plugins                                          x86_64                             2.3.3-4.el8                                                         epel                                   118 k
 nagios-plugins-breeze                                   x86_64                             2.3.3-4.el8                                                         epel                                    25 k
 nagios-plugins-by_ssh                                   x86_64                             2.3.3-4.el8                                                         epel                                    48 k
 nagios-plugins-cluster                                  x86_64                             2.3.3-4.el8                                                         epel                                    42 k
 nagios-plugins-dhcp                                     x86_64                             2.3.3-4.el8                                                         epel                                    48 k
 nagios-plugins-dig                                      x86_64                             2.3.3-4.el8                                                         epel                                    47 k
 nagios-plugins-disk                                     x86_64                             2.3.3-4.el8                                                         epel                                    61 k
 nagios-plugins-disk_smb                                 x86_64                             2.3.3-4.el8                                                         epel                                    28 k
 nagios-plugins-dns                                      x86_64                             2.3.3-4.el8                                                         epel                                    50 k
 nagios-plugins-dummy                                    x86_64                             2.3.3-4.el8                                                         epel                                    37 k
 nagios-plugins-file_age                                 x86_64                             2.3.3-4.el8                                                         epel                                    26 k
 nagios-plugins-flexlm                                   x86_64                             2.3.3-4.el8                                                         epel                                    26 k
 nagios-plugins-fping                                    x86_64                             2.3.3-4.el8                                                         epel                                    48 k
 nagios-plugins-hpjd                                     x86_64                             2.3.3-4.el8                                                         epel                                    46 k
 nagios-plugins-http                                     x86_64                             2.3.3-4.el8                                                         epel                                    64 k
 nagios-plugins-icmp                                     x86_64                             2.3.3-4.el8                                                         epel                                    55 k
 nagios-plugins-ide_smart                                x86_64                             2.3.3-4.el8                                                         epel                                    43 k
 nagios-plugins-ircd                                     x86_64                             2.3.3-4.el8                                                         epel                                    26 k
 nagios-plugins-ldap                                     x86_64                             2.3.3-4.el8                                                         epel                                    49 k
 nagios-plugins-load                                     x86_64                             2.3.3-4.el8                                                         epel                                    46 k
 nagios-plugins-log                                      x86_64                             2.3.3-4.el8                                                         epel                                    26 k
 nagios-plugins-mailq                                    x86_64                             2.3.3-4.el8                                                         epel                                    29 k
 nagios-plugins-mrtg                                     x86_64                             2.3.3-4.el8                                                         epel                                    43 k
 nagios-plugins-mrtgtraf                                 x86_64                             2.3.3-4.el8                                                         epel                                    43 k
 nagios-plugins-mysql                                    x86_64                             2.3.3-4.el8                                                         epel                                    56 k
 nagios-plugins-nagios                                   x86_64                             2.3.3-4.el8                                                         epel                                    44 k
 nagios-plugins-nt                                       x86_64                             2.3.3-4.el8                                                         epel                                    49 k
 nagios-plugins-ntp                                      x86_64                             2.3.3-4.el8                                                         epel                                    70 k
 nagios-plugins-nwstat                                   x86_64                             2.3.3-4.el8                                                         epel                                    51 k
 nagios-plugins-oracle                                   x86_64                             2.3.3-4.el8                                                         epel                                    26 k
 nagios-plugins-overcr                                   x86_64                             2.3.3-4.el8                                                         epel                                    46 k
 nagios-plugins-perl                                     x86_64                             2.3.3-4.el8                                                         epel                                    25 k
 nagios-plugins-pgsql                                    x86_64                             2.3.3-4.el8                                                         epel                                    47 k
 nagios-plugins-ping                                     x86_64                             2.3.3-4.el8                                                         epel                                    49 k
 nagios-plugins-procs                                    x86_64                             2.3.3-4.el8                                                         epel                                    49 k
 nagios-plugins-real                                     x86_64                             2.3.3-4.el8                                                         epel                                    45 k
 nagios-plugins-rpc                                      x86_64                             2.3.3-4.el8                                                         epel                                    27 k
 nagios-plugins-sensors                                  x86_64                             2.3.3-4.el8                                                         epel                                    24 k
 nagios-plugins-smtp                                     x86_64                             2.3.3-4.el8                                                         epel                                    52 k
 nagios-plugins-snmp                                     x86_64                             2.3.3-4.el8                                                         epel                                    53 k
 nagios-plugins-ssh                                      x86_64                             2.3.3-4.el8                                                         epel                                    44 k
 nagios-plugins-ssl_validity                             x86_64                             2.3.3-4.el8                                                         epel                                    28 k
 nagios-plugins-swap                                     x86_64                             2.3.3-4.el8                                                         epel                                    43 k
 nagios-plugins-tcp                                      x86_64                             2.3.3-4.el8                                                         epel                                    52 k
 nagios-plugins-time                                     x86_64                             2.3.3-4.el8                                                         epel                                    45 k
 nagios-plugins-ups                                      x86_64                             2.3.3-4.el8                                                         epel                                    47 k
 nagios-plugins-users                                    x86_64                             2.3.3-4.el8                                                         epel                                    41 k
 nagios-plugins-wave                                     x86_64                             2.3.3-4.el8                                                         epel                                    25 k
 net-snmp-libs                                           x86_64                             1:5.8-14.el8_2.1                                                    BaseOS                                 821 k
 net-snmp-utils                                          x86_64                             1:5.8-14.el8_2.1                                                    AppStream                              237 k
 perl-Carp                                               noarch                             1.42-396.el8                                                        BaseOS                                  30 k
 perl-Compress-Raw-Bzip2                                 x86_64                             2.081-1.el8                                                         AppStream                               40 k
 perl-Compress-Raw-Zlib                                  x86_64                             2.081-1.el8                                                         AppStream                               68 k
 perl-Convert-ASN1                                       noarch                             0.27-17.el8                                                         AppStream                               60 k
 perl-Crypt-X509                                         noarch                             0.51-19.el8                                                         epel                                    26 k
 perl-Data-Dump                                          noarch                             1.23-7.el8                                                          AppStream                               36 k
 perl-Data-Dumper                                        x86_64                             2.167-399.el8                                                       BaseOS                                  58 k
 perl-Digest                                             noarch                             1.17-395.el8                                                        AppStream                               27 k
 perl-Digest-HMAC                                        noarch                             1.03-17.el8                                                         AppStream                               20 k
 perl-Digest-MD5                                         x86_64                             2.55-396.el8                                                        AppStream                               37 k
 perl-Digest-SHA                                         x86_64                             1:6.02-1.el8                                                        AppStream                               66 k
 perl-Encode                                             x86_64                             4:2.97-3.el8                                                        BaseOS                                 1.5 M
 perl-Encode-Locale                                      noarch                             1.05-9.el8                                                          AppStream                               21 k
 perl-Errno                                              x86_64                             1.28-416.el8                                                        BaseOS                                  76 k
 perl-Exporter                                           noarch                             5.72-396.el8                                                        BaseOS                                  34 k
 perl-File-Listing                                       noarch                             6.04-17.el8                                                         AppStream                               18 k
 perl-File-Path                                          noarch                             2.15-2.el8                                                          BaseOS                                  38 k
 perl-File-Temp                                          noarch                             0.230.600-1.el8                                                     BaseOS                                  63 k
 perl-Filter                                             x86_64                             2:1.58-2.el8                                                        AppStream                               82 k
 perl-Getopt-Long                                        noarch                             1:2.50-4.el8                                                        BaseOS                                  63 k
 perl-HTML-Parser                                        x86_64                             3.72-14.el8                                                         AppStream                              119 k
 perl-HTML-Tagset                                        noarch                             3.20-33.el8                                                         AppStream                               24 k
 perl-HTTP-Cookies                                       noarch                             6.04-2.el8                                                          AppStream                               39 k
 perl-HTTP-Date                                          noarch                             6.02-18.el8                                                         AppStream                               19 k
 perl-HTTP-Message                                       noarch                             6.18-1.el8                                                          AppStream                              100 k
 perl-HTTP-Negotiate                                     noarch                             6.01-19.el8                                                         AppStream                               22 k
 perl-HTTP-Tiny                                          noarch                             0.074-1.el8                                                         BaseOS                                  58 k
 perl-IO                                                 x86_64                             1.38-416.el8                                                        BaseOS                                 141 k
 perl-IO-Compress                                        noarch                             2.081-1.el8                                                         AppStream                              258 k
 perl-IO-HTML                                            noarch                             1.001-10.el8                                                        AppStream                               28 k
 perl-IO-Socket-IP                                       noarch                             0.39-5.el8                                                          AppStream                               47 k
 perl-IO-Socket-SSL                                      noarch                             2.066-4.el8                                                         AppStream                              297 k
 perl-Import-Into                                        noarch                             1.002005-7.el8                                                      PowerTools                              20 k
 perl-LWP-MediaTypes                                     noarch                             6.02-14.el8                                                         AppStream                               29 k
 perl-MIME-Base64                                        x86_64                             3.15-396.el8                                                        BaseOS                                  31 k
 perl-Module-Runtime                                     noarch                             0.016-2.el8                                                         AppStream                               26 k
 perl-NTLM                                               noarch                             1.09-17.el8                                                         AppStream                               24 k
 perl-Net-HTTP                                           noarch                             6.17-2.el8                                                          AppStream                               43 k
 perl-Net-SSLeay                                         x86_64                             1.88-1.el8                                                          AppStream                              379 k
 perl-PathTools                                          x86_64                             3.74-1.el8                                                          BaseOS                                  90 k
 perl-PerlIO-utf8_strict                                 x86_64                             0.007-5.el8                                                         PowerTools                              28 k
 perl-Pod-Escapes                                        noarch                             1:1.07-395.el8                                                      BaseOS                                  20 k
 perl-Pod-Perldoc                                        noarch                             3.28-396.el8                                                        BaseOS                                  86 k
 perl-Pod-Simple                                         noarch                             1:3.35-395.el8                                                      BaseOS                                 213 k
 perl-Pod-Usage                                          noarch                             4:1.69-395.el8                                                      BaseOS                                  34 k
 perl-Scalar-List-Utils                                  x86_64                             3:1.49-2.el8                                                        BaseOS                                  68 k
 perl-Socket                                             x86_64                             4:2.027-3.el8                                                       BaseOS                                  59 k
 perl-Storable                                           x86_64                             1:3.11-3.el8                                                        BaseOS                                  98 k
 perl-Term-ANSIColor                                     noarch                             4.06-396.el8                                                        BaseOS                                  46 k
 perl-Term-Cap                                           noarch                             1.17-395.el8                                                        BaseOS                                  23 k
 perl-Text-Glob                                          noarch                             0.11-4.el8                                                          AppStream                               17 k
 perl-Text-ParseWords                                    noarch                             3.30-395.el8                                                        BaseOS                                  18 k
 perl-Text-Tabs+Wrap                                     noarch                             2013.0523-395.el8                                                   BaseOS                                  24 k
 perl-Time-Local                                         noarch                             1:1.280-1.el8                                                       BaseOS                                  34 k
 perl-TimeDate                                           noarch                             1:2.30-13.el8                                                       AppStream                               53 k
 perl-Try-Tiny                                           noarch                             0.30-2.el8                                                          AppStream                               44 k
 perl-URI                                                noarch                             1.73-3.el8                                                          AppStream                              116 k
 perl-Unicode-Normalize                                  x86_64                             1.25-396.el8                                                        BaseOS                                  82 k
 perl-WWW-RobotRules                                     noarch                             6.02-18.el8                                                         AppStream                               22 k
 perl-constant                                           noarch                             1.33-396.el8                                                        BaseOS                                  25 k
 perl-encoding                                           x86_64                             4:2.22-3.el8                                                        AppStream                               68 k
 perl-interpreter                                        x86_64                             4:5.26.3-416.el8                                                    BaseOS                                 6.3 M
 perl-libnet                                             noarch                             3.11-3.el8                                                          AppStream                              121 k
 perl-libs                                               x86_64                             4:5.26.3-416.el8                                                    BaseOS                                 1.6 M
 perl-libwww-perl                                        noarch                             6.34-1.el8                                                          AppStream                              212 k
 perl-macros                                             x86_64                             4:5.26.3-416.el8                                                    BaseOS                                  72 k
 perl-open                                               noarch                             1.11-416.el8                                                        AppStream                               77 k
 perl-parent                                             noarch                             1:0.237-1.el8                                                       BaseOS                                  20 k
 perl-podlators                                          noarch                             4.11-1.el8                                                          BaseOS                                 118 k
 perl-threads                                            x86_64                             1:2.21-2.el8                                                        BaseOS                                  61 k
 perl-threads-shared                                     x86_64                             1.58-2.el8                                                          BaseOS                                  48 k
 perl-utf8-all                                           noarch                             0.024-7.el8                                                         epel                                    28 k
 python3-bind                                            noarch                             32:9.11.13-5.el8_2                                                  AppStream                              148 k
 python3-ply                                             noarch                             3.9-8.el8                                                           BaseOS                                 108 k
 rpcbind                                                 x86_64                             1.2.5-7.el8                                                         BaseOS                                  70 k
 samba-client                                            x86_64                             4.11.2-13.el8                                                       BaseOS                                 658 k
Installation des dépendances faibles:
 geolite2-city                                           noarch                             20180605-1.el8                                                      AppStream                               19 M
 geolite2-country                                        noarch                             20180605-1.el8                                                      AppStream                              1.0 M
 perl-Mozilla-CA                                         noarch                             20160104-7.el8                                                      AppStream                               15 k
Activation des flux de modules:
 mysql                                                                                      8.0

Résumé de la transaction
=============================================================================================================================================================================================================
Installer  146 Paquets

Taille totale des téléchargements : 42 M
Taille des paquets installés : 119 M
```
