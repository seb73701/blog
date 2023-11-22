---
title: Planifier l'exécution d'un plugin Redmine dans la Crontab
description: 
published: true
date: 2023-04-21T10:49:21.726Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:49:17.617Z
---

====== Planifier l'exécution d'un plugin Redmine dans la Crontab ======
----
===== Explication =====

Pour exécuter un plugin redmine depuis la crontab, il faut passer par un script bash sinon cela ne fonctionne pas.

===== Exemple =====

Ici un exemple avec le plugin ''periodictask'' ([[http://www.redmine.org/plugins/redmine_periodic_task]]):

<code>[root@server]# cat /opt/redmine-3.4.2/lib/tasks/periodictask.sh</code>
<code>
#! /bin/bash
PATH=$PATH:/usr/local/bin

log_file="/var/log/redmine_periodictask.log"
echo "[$(date +"%d-%m-%y %T")] ===== Launch =====" >> $log_file
cd /opt/redmine-3.4.2/lib/tasks/
bash -lc "/usr/local/rvm/gems/ruby-2.0.0-p247/bin/bundle exec rake redmine:check_periodictasks RAILS_ENV=\"production\" >> $log_file"

</code>

  * Crontab :

<code>[root@server]# vim /etc/crontab</code>

<code>* * * * * root sh /opt/redmine-3.4.2/lib/tasks/periodictask.sh</code>