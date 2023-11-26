# Execute IcingaDirector manually

```
Hm, alright, then let’s try to run this manually as the icingadirector user:

su - icingadirector -s /bin/bash
icingacli director daemon run --debug --trace
And while you’re at it you could check if the icingadirector user can:

read the config at /etc/icingaweb2/modules/director/config.ini
see the symlinks at /etc/icingaweb2/enabledModules
and change into the directories like so: cd /etc/icingaweb2/enabledModules/director
```