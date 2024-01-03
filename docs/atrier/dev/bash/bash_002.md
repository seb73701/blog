---
title: Lire des valeurs depuis un fichier de configuration
description: 
published: true
date: 2023-04-21T11:00:39.768Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:00:34.998Z
---

====== Lire des valeurs depuis un fichier de configuration ======
----

In this article, we tackle the problem of simplifying bash commands. Sound intriguing? Read to learn the answer this developer came up with!

Recently, I was developing a bash script. To make the script flexible and configurable, we decided to maintain some of the values required by the application, in a configuration file. If a change was needed, we only needed to edit the config file and not the script, making script maintenance easier.

To get values from the config file, I used ''grep'' and ''cut'', as follows:

<code>
 schema_name=`grep "schema_name" config.cfg | cut -d '=' -f 2`
 database_url=`grep "database_name" config.cfg | cut -d '=' -f 2`
</code>

While the contents of the config file were:

<code>
schema_name=test

database_url=database.mydomain.com 
</code>

If we need to fetch a lot of values from the config file, this method becomes repetitive and can be error-prone. Hence, I encapsulated the ''grep'' and ''cut'' commands inside a function, making it easier to fetch values, as follows:

<code>
getValueFromConfig() {
    VALUE=`grep ${1} config.sys | cut -d '=' -f 2`
}
</code>

In the defined function, ''${1}'' is the first parameter passed to the function and is used to pass the key, whose value is to be read from the config file.

The way to use this function is

<code>schema_name=`getValueFromConfig "schema_name"`</code>

or

<code>schema_name=$(getValueFromConfig "schema_name")</code>

In fact, it is also possible to further customize the function to parameterize the separator used in the config file, as well as the position of the value that needs to be fetched from the config file.

Source : [[https://dzone.com/articles/bash-snippet-reading-values-from-a-configuration-f]]