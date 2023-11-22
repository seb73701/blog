---
title: Mise à jour du module Director
description: 
published: true
date: 2023-04-21T10:48:00.553Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:56.040Z
---

====== Mise à jour du module Director ======
----

Procédure : [[https://icinga.com/docs/director/latest/doc/05-Upgrading/]]

  * Faire un backup de sa BDD (contrôler les logs)
  * Faire une sauvegarde du dossier :

<code>cp -a /usr/share/icingaweb2/modules/director/ /usr/share/icingaweb2/modules/director_`date +%Y%m%d`/</code>

  * Désactiver le module ''Director'' depuis l'interface IcingaWeb2 depuis le menu "**Configuration > Modules**"
  * Renommer le dossier director actuel :

<code>mv /usr/share/icingaweb2/modules/director/ /usr/share/icingaweb2/modules/director_old/</code>

  * Création du directory :

<code>mkdir -p /usr/share/icingaweb2/modules/director/</code>

  * Installation du module :
<code>
ICINGAWEB_MODULES=/usr/share/icingaweb2/modules
DIRECTOR_GIT=https://github.com/Icinga/icingaweb2-module-director.git
git clone $DIRECTOR_GIT $ICINGAWEB_MODULES/director
</code>

  * Activer le module depuis l'interface d'IcingaWeb2 depuis le menu "**Configuration > Modules**"
  * Aller sur l'onglet "**Configuration**" et cliquer sur le bouton "**Apply schema migrations**"

{{:supervision:icingaweb2:icinga2_director_update_schema.png?nolink|}}

  * Contrôler les différents éléments constituants le paramétrage mais aussi les objets.

