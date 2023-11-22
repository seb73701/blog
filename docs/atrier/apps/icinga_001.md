---
title: Configuration de l'API REST d'Icinga 2
description: Application - Icinga
published: true
date: 2023-04-21T10:46:49.076Z
tags: monitoring, supervision, api, icinga, icingaweb
editor: markdown
dateCreated: 2023-04-21T10:46:44.765Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis</span>

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir icinga installé

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Configuration de l'API REST</span>
  
Icinga Web 2 et d'autres interfaces Web nécessitent l'[API REST](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-setup) pour envoyer des actions (replanifier les checks, etc.) et interroger le détail sur un objet par exemple.

Il faut exécuter la commande CLI `icinga2 api setup` pour activer la [fonction `api`](https://icinga.com/docs/icinga-2/latest/doc/11-cli-commands/#enable-features) et configurer des certificats ainsi qu'un nouvel utilisateur API "**root**" avec un mot de passe généré automatiquement dans le fichier de configuration `/etc/icinga2/conf.d/api-users.conf` :


<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```
[root@icinga-master01 ~]# icinga2 api setup
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
information/cli: Generating new CA.
information/base: Writing private key to '/var/lib/icinga2/ca//ca.key'.
information/base: Writing X509 certificate to '/var/lib/icinga2/ca//ca.crt'.
information/cli: Generating new CSR in '/var/lib/icinga2/certs//icinga-master01.seb.fr.csr'.
information/base: Writing private key to '/var/lib/icinga2/certs//icinga-master01.seb.fr.key'.
information/base: Writing certificate signing request to '/var/lib/icinga2/certs//icinga-master01.seb.fr.csr'.
information/cli: Signing CSR with CA and writing certificate to '/var/lib/icinga2/certs//icinga-master01.seb.fr.crt'.
information/pki: Writing certificate to file '/var/lib/icinga2/certs//icinga-master01.seb.fr.crt'.
information/cli: Copying CA certificate to '/var/lib/icinga2/certs//ca.crt'.
information/cli: Adding new ApiUser 'root' in '/etc/icinga2/conf.d/api-users.conf'.
information/cli: Reading '/etc/icinga2/icinga2.conf'.
information/cli: Enabling the 'api' feature.
Enabling feature api. Make sure to restart Icinga 2 for these changes to take effect.
information/cli: Updating 'NodeName' constant in '/etc/icinga2/constants.conf'.
information/cli: Created backup file '/etc/icinga2/constants.conf.orig'.
information/cli: Updating 'ZoneName' constant in '/etc/icinga2/constants.conf'.
information/cli: Backup file '/etc/icinga2/constants.conf.orig' already exists. Skipping backup.
Done.

Now restart your Icinga 2 daemon to finish the installation!

```  
  
  
Le setup ajoute le compte ci-dessous dans le fichier :


<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/icinga2/conf.d/api-users.conf*</span>
  
```js
/**
 * The ApiUser objects are used for authentication against the API.
 */
object ApiUser "root" {
  password = "ae84bf5f1235057f"
  // client_cn = ""

  permissions = [ "*" ]
}
```

Editer le fichier `api-users.conf` et ajouter à un nouvel objet [ApiUser](https://icinga.com/docs/icinga-2/latest/doc/09-object-types/#apiuser). Spécifier les attributs de [permissions](https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/#icinga2-api-permissions) avec les autorisations minimales requises par Icinga Web 2.


<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```
[root@icinga-master01 ~]# vim /etc/icinga2/conf.d/api-users.conf
```

<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/icinga2/conf.d/api-users.conf*</span>

```json
object ApiUser "icingaweb2" {
  password = "Wijsn8Z9eRs5E25d"
  permissions = [ "status/query", "actions/*", "objects/modify/*", "objects/query/*" ]
}
```

Il faut ensuite redémarrer le service `icinga2` pour prendre en compte les changements :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>     
  
```
[root@icinga-master01 ~]# systemctl restart icinga2
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Documentation</span>
- https://icinga.com/docs/icinga-2/latest/doc/02-installation/05-CentOS/#set-up-icinga-2-rest-api
- https://icinga.com/docs/icinga-2/latest/doc/12-icinga2-api/