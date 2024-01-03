---
title: Curl (Commande)
description: 
published: true
date: 2023-04-21T11:16:20.227Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:16.177Z
---

## Liste des User-Agent

+ [User agent (Wikipedia)](https://fr.wikipedia.org/wiki/User_agent)
+ [List of User Agents (whatismybrowser.com)](https://developers.whatismybrowser.com/useragents/explore/)

## Exemples

### Utiliser un user-agent "-A"

```
curl -I -s -X GET -A "User-Agent: Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.110 Safari/537.36" https//www.google.com
```

### Utiliser la décompression (gzip,etc) "--compressed"

```
curl --compressed -I -s -k -X GET https://www.google.com
```