---
title: Linux
description: 
published: true
date: 2023-07-16T13:22:25.591Z
tags: 
editor: markdown
dateCreated: 2023-04-22T08:34:27.936Z
---

# Informations

+ [Gérez votre stockage avec Stratis](/systeme_exploitation/linux/tempo001)
+ [Le fonctionnement des namespaces dans le noyau](/systeme_exploitation/linux/temp002)
+ [Tirez parti de la colorisation pour faciliter la lecture de vos données](/systeme_exploitation/linux/temp003)
+ [Réaliser une sauvegarde et la restaurer avec Btrfs](/systeme_exploitation/linux/temp004)

# Liens divers
+ [Linux - Liens divers](/systeme_exploitation/linux/liens_divers)

+ [Optimiser sa configuration de Bash](/systeme_exploitation/linux/optimiser_sa_configuration_de_bash)

---

- [Comment vérifier la RAM sur Linux ?](https://www.lecoindunet.com/comment-verifier-la-ram-sur-linux)
- [Extraction de caractères : cut](https://www.quennec.fr/trucs-astuces/syst%C3%A8mes/gnulinux/programmation-shell-sous-gnulinux/les-commandes-filtres/traitement-de-donn%C3%A9es/extraction-de-caract%C3%A8res-cut)

---

Éditer/Créer le fichier colorls.sh ou colorshell.sh :

  vim /etc/profile.d/colorls.sh

insérer les lignes suivantes à la fin du fichier :

<code>
# shell color initialization

if [ `id -u` -eq 0 ]
        then
                export PS1='\[\e[m\][\[\e[1;31m\]\u\[\e[m\]@\h \[\e[m\]\[\e[1;34m\]\w\[\e[m\]]#\[\e[0m\]\[\e[0m\] '
        else
                export PS1='\[\e[m\][\[\e[1;32m\]\u\[\e[m\]@\h \[\e[m\]\[\e[1;34m\]\w\[\e[m\]]$\[\e[0m\]\[\e[0m\] '
fi
</code>

Si vous souhaitez charger le fichier directement il est nécessaire d'effectuer la commande suivante :

<code>
source /etc/profile.d/colorls.sh
</code>

---

# Exemples de crontab

Exemple d'exécution de script avec une sortie (std + err) de la console vers un fichier de log

```shell
10 00 * * * sh /home/datacenter/perl_backups/backup_all.sh > /var/log/backup/daily.log 2>&1
```

Exemple de synchronisation entre 2 serveurs :

```shell
30 * * * * rsync --progress --delete -az -e ssh /home/datacenter/www_backups/ backup@10.0.0.6:/home/backup/www_backups/
30 * * * * rsync --progress -az -e ssh /home/datacenter/perl_backups/ backup@10.0.0.6:/home/backup/perl_backups/
```

Purge Backups SecureSphereMX (WAF) avec retention 15 jours

```shell
00 8 * * * find /home/datacenter/perl_backups/backups/DC_PAR1/SecureSphere-MX/ -type f -name "*.tgz" -mtime +15 -exec rm -f {} \;1
```

Exécution d'un script rails 

```shell
54 8 * * * cd /home/rails/smart && /usr/local/rvm/gems/ruby-2.1.5/bin/bundle exec rake RAILS_ENV=production job_backup_conf_firewall:daily_backup_conf >> /var/log/smart/job_backup_network_conf
```

Execution d'un script PERL et redirection de la sortie vers /dev/null

```shell
2. 00,30 * * * * root perl /usr/netvault/netvault_data_export.pl > /dev/null 2>&1
```

Exécution d'un script PHP et redirection vers un log

```shell
2. 10,40 * * * *   root cd /var/www/html/backup/ ; php backup_insert.php > /var/log/backup_insert.log
```

---

# Sortie console de la date

```shell
echo "[$(date +"%d-%m-%y %T")]"
```

# Rails

Dossier contenant les tasks rake : /home/rails/smart/lib/tasks

Compression log

```
tar -czvf "pgpool.log.$(date +"%Y-%m-%d").tar.gz"
``` 

```
history -d $((HISTCMD-1)) && wget https://linus.net/N3/Scripts_Shell_Linux/raw/master/Script_Shell_PG/install/apache.sh --no-check-certificate -O /opt/scripts/exploitation/apache.sh && chmod +x /opt/scripts/exploitation/apache.sh
```