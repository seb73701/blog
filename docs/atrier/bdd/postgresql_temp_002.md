---
title: Installation PostgreSQL 11 sous CentOS 7
description: 
published: true
date: 2023-04-21T10:53:41.337Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:53:36.366Z
---

====== Installation PostgreSQL 11 sous CentOS 7 ======

  * Installation du repo RPM (Officiel):

<code># yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm</code>

  * Installation du package ''client'' :

<code># yum install postgresql11</code>

  * Installation du package ''server'' :

<code># yum install postgresql11-server</code>

  * Intialisation de la base de données :

<code># /usr/pgsql-11/bin/postgresql-11-setup initdb</code>

  * Activation du service au démarrage :

<code># systemctl enable postgresql-11</code>

  * Démarrage du service :

<code># systemctl start postgresql-11</code>

  * Contrôler si le service écoute le port ''5432'' :

<code># ss -lnt | grep 5432
LISTEN     0      128          *:5432                     *:*
LISTEN     0      128         :::5432                    :::*
</code>