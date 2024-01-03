---
title: Package "nagios_plugins_all"
description: 
published: true
date: 2023-04-21T10:41:10.085Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:41:04.885Z
---

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

  
## Erreur lors de l'installation du package "nagios-plugins-all" sur CentOS 8
  
Si vous rencontrez ce type de message : 
  
```shell
Error:
Problem: package nagios-plugins-all-2.3.3-4.el8.x86_64 requires nagios-plugins-disk_smb, but none of the providers can be installed
  - package nagios-plugins-disk_smb-2.3.3-4.el8.x86_64 requires perl(utf8::all), but none of the providers can be installed
  - conflicting requests
  - nothing provides perl(Import::Into) needed by perl-utf8-all-0.024-7.el8.noarch
  - nothing provides perl(PerlIO::utf8_strict) needed by perl-utf8-all-0.024-7.el8.noarch
(try to add '--skip-broken' to skip uninstallable packages or '--nobest' to use not only best candidate packages)
```

Le problème vient du fait que le repo "**PowerTools**" (http://mirror.centos.org/centos/8/PowerTools/x86_64/os/Packages/) n'est pas actif et comme il est obligatoire pour les dépendances ci-dessous.

Le package "**perl-utf8-all**" depend des packages suivants :
- **perl-Import-Into**
- **perl-PerlIO-utf8_strict**


Mais, par défaut le repo **[PowerTools]** (`/etc/yum.repos.d/CentOS-Linux-PowerTools.repo`)  est désactiver "`enabled=0`".
  
Il faut donc l'activer pour corriger le problème pour cela, on va éditer le fichier du repo :
  
```shell
vim /etc/yum.repos.d/CentOS-Linux-PowerTools.repo
```

Mettre `1` sur `enabled=` :
  
```ini
# CentOS-Linux-PowerTools.repo
#
# The mirrorlist system uses the connecting IP address of the client and the
# update status of each mirror to pick current mirrors that are geographically
# close to the client.  You should use this for CentOS updates unless you are
# manually picking other mirrors.
#
# If the mirrorlist does not work for you, you can try the commented out
# baseurl line instead.

[powertools]
name=CentOS Linux $releasever - PowerTools
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=PowerTools&infra=$infra
#baseurl=http://mirror.centos.org/$contentdir/$releasever/PowerTools/$basearch/os/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
``` 
  
Il suffit ensuite de relancer l'installation (voir commande ci-dessus).