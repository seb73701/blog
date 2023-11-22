---
title: Requêtes Icinga pour Dashboard Grafana
description: 
published: true
date: 2023-04-21T10:48:06.792Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:02.567Z
---

====== Requêtes Icinga pour Dashboard Grafana ======

  * Version de l'agent sur le master actif :

<code sql>SELECT program_version FROM icinga_programstatus;</code>

Date et Heure de la dernière mise à jour :

<code sql>SELECT status_update_time FROM icinga_programstatus;</code>

Nom du MASTER actif :
<code sql>SELECT endpoint_name FROM icinga_programstatus;</code>

Uptime du démarrage d'Icinga :
<code sql>SELECT program_start_time FROM icinga_programstatus;</code>

Date et Heure de la dernière commande check :
<code sql>SELECT last_command_check FROM icinga_programstatus;</code>


Nb de services au total (OK-KO-WARNING-maintenance-pending) actifs
<code sql>SELECT count(*) FROM icinga_objects WHERE objecttype_id=2 AND is_active=1</code>

Nb de hosts au total (tous les status confonfus) actifs
<code sql>SELECT count(*) FROM icinga_objects WHERE objecttype_id=1 AND is_active=1</code>

Get all Host Status in icinga Hostgroup by name

<code sql>SELECT * FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE object_id IN 
(SELECT host_object_id FROM icinga_hostgroup_members WHERE hostgroup_id = 
(SELECT hostgroup_id FROM icinga_hostgroups WHERE hostgroup_object_id = 
(select object_id FROM icinga_objects WHERE objecttype_id = 3 AND name1 LIKE 'GROUPNAME')))</code>


Nb de host OK :
<code sql>SELECT count(*) FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE is_active=1 and o.objecttype_id=1 and s.current_state=0</code>

Nb de host KO (KO+maintenance+pending): 
<code sql>SELECT count(*) FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE is_active=1 and o.objecttype_id=1 and s.current_state=1</code>


NB de host KO en pending :
<code sql>SELECT * FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE is_active=1 and o.objecttype_id=1 and s.current_state=1 and has_been_checked=0</code>

Nb de hosts en KO (schedule downtime/acknoledge)
<code sql>SELECT count(*) FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE is_active=1 and o.objecttype_id=1 and s.current_state=1 and has_been_checked=1 and scheduled_downtime_depth=1</code>

Nb de hosts en KO :
<code sql>SELECT count(*) FROM icinga_objects o LEFT JOIN icinga_hoststatus s ON  s.host_object_id = o.object_id  WHERE is_active=1 and o.objecttype_id=1 and s.current_state=1 and has_been_checked=1 and scheduled_downtime_depth=0</code>
