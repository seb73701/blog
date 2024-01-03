---
title: Mettre à jour la liste des CPAN Mirror Network
description: 
published: true
date: 2023-04-21T11:06:51.206Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:06:46.449Z
---

# Mettre à jour la liste des CPAN Mirror Network


Lancer les commandes suivantes :

<code># perl -MCPAN -e shell</code>

<code>cpan[0]> install DBI</code>

Si l'on obtient des erreurs de ce type :

<code>
HTTP request sent, awaiting response... 404 Not Found
2018-06-06 10:40:11 ERROR 404: Not Found.

Please check, if the URLs I found in your configuration file
(http://www.perl.org/CPAN/MIRRORED.BY) are valid. The urllist can be
edited. E.g. with 'o conf urllist push ftp://myurl/'
</code>

Avant de modifier la liste, vous pouvez trouver celle-ci sur cette page [[https://www.cpan.org/SITES.html]].

On peut aussi voir l'état des miroirs sur cette page : [[http://mirrors.cpan.org/]]

Il faut alors modifier la liste des miroirs avec les commandes suivantes :

<code>
cpan[1]> o conf urllist push http://ftp.igh.cnrs.fr/pub/CPAN/
cpan[2]> o conf commit
</code>

Voilà c'est tout !!!!

Maintenant, on peut installer "**DBI**" :

<code>cpan[3]> install DBI</code>