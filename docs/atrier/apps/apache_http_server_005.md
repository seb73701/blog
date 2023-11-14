# Récupérer la liste des urls avec un compteur sur les hits

```shell
[root@server  /var/log/httpd/01_wiki]$ cat ssl_ccess_log | grep "ajax.php" | awk -F\" '{print $4}' | sed '/^$/d' | sed 's/\?.*//g' | sort | uniq -c | sort -rn
```

Résultat :

```
498 https://wiki.home.net/start
290 https://wiki.home.net/
267 https://wiki.home.net/process/global/utilisation_de_la_plateforme_resiliation
```