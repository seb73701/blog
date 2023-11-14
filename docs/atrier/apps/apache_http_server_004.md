# HTACCESS - Bloquer des IP et des Proxies

```ini
#On bloque les ip par plage de certains pays
RewriteCond %{REMOTE_ADDR} ^41\.[0-9]+\.[0-9]+\.[0-9]+ [OR]
RewriteCond %{REMOTE_ADDR} ^81\.[0-9]+\.[0-9]+\.[0-9]+ [OR]
RewriteCond %{REMOTE_ADDR} ^196\.[0-9]+\.[0-9]+\.[0-9]+
RewriteRule .* - [F]
#on bloque les proxy
RewriteCond %{HTTP:VIA}                 !^$ [OR]
RewriteCond %{HTTP:FORWARDED}           !^$ [OR]
RewriteCond %{HTTP:USERAGENT_VIA}       !^$ [OR]
RewriteCond %{HTTP:X_FORWARDED_FOR}     !^$ [OR]
RewriteCond %{HTTP:PROXY_CONNECTION}    !^$ [OR]
RewriteCond %{HTTP:XPROXY_CONNECTION}   !^$ [OR]
RewriteCond %{HTTP:HTTP_PC_REMOTE_ADDR} !^$ [OR]
RewriteCond %{HTTP:HTTP_CLIENT_IP}      !^$
RewriteRule ^(.*)$ - [F]
```