---
title: Enable Gzip Compression on Apache Shared Hosting
description: 
published: true
date: 2023-07-03T13:54:57.938Z
tags: 
editor: markdown
dateCreated: 2023-07-03T13:54:57.938Z
---

# Enable Gzip Compression on Apache Shared Hosting

I was working on tuning the performance of a site that happened to be hosted on a shared hosting provider - Dreamhost in this case. One of the simplest things you can do to improve performance is enable Gzip compression for HTTP requests. This is supported in all modern browsers and provides a quick win by reducing the size of HTTP responses and, therefore, improving response times. The instructions on how to enable this will vary based on your web server and the level of control you have.

![yslow-compress-a.png](../images/apps/apache_http_server/yslow-compress-a.png)

Dreamhost’s shared hosting platform is based on Apache 2. Since they don’t provide the necessary privileges to update the server configuration files directly, you can use configuration directives in an [.htaccess](http://httpd.apache.org/docs/2.2/howto/htaccess.html) file to enable compression. In Apache, compression is provided by the [mod_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html) module and a typical configuration could look something like:

```
#Gzip
<ifmodule mod_deflate.c>
AddOutputFilterByType DEFLATE text/text text/html text/plain text/xml text/css application/x-javascript application/javascript text/javascript
</ifmodule>
#End Gzip
```

Of course, you can change the list of content types to best fit your site. Typically, you would add this to the .htaccess file in your web root directory (or create one if it does not already exist).

Now run Google PageSpeed Insights or YSlow and see your improved performance scores!

Source : https://ryaneschinger.com/