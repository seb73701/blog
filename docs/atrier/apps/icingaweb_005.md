---
title: Liste des autorisations dans IcingaWeb2
description: 
published: true
date: 2023-04-21T10:47:47.125Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:42.688Z
---

====== Liste des autorisations dans IcingaWeb2 ======
----

===== Description =====

Cette procédure décrit les différents droits disponibles dans IcingaWeb2.

La liste des utilisateurs est basé sur le LDAP. 
La liste des groupes est aussi basé sur le LDAP. Un utilisateur peut appartenir à un groupe ou plusieurs groupes.

On peut affecter un rôle à un ou plusieurs groupe(s).
On peut aussi affecter un rôle à un ou plusieurs utilisateur(s).

Le rôle "**Administrators**" est réservé au propriétaire de l'actif et à son backup. En aucun cas, il faut rattaché un autre utilisateur dans ce rôle sans avoir eu une dérogation de la RSSI C&S.

Il existe 2 types de compte :
  * Compte LDAP (identifiants LDAP)
  * Compte Local (identifiants custom)


La configuration des rôles se fait par le menu "**Configuration > Authentification**"
===== Droits =====

  * Allow everything (*)
  * Allow to share navigation items (application/share/navigation)
  * Allow to adjust in the preferences whether to show stacktraces (application/stacktraces)
  * Allow to view the application log (application/log)
  * Grant admin permissions, e.g. manage announcements (admin)
  * Allow config access (config/*)
  * Allow access to module director (module/director)
  * Allow to access the director API (director/api)
  * Allow to access the full audit log (director/audit)
  * Allow to show configuration (could contain sensitive information) (director/showconfig)
  * Allow to show the full executed SQL queries in some places (director/showsql)
  * Allow to deploy configuration (director/deploy)
  * Allow to configure hosts (director/hosts)
  * Allow to configure services (director/services)
  * Allow to configure service sets (director/servicesets)
  * Allow to define Service Set Apply Rules (director/service_set/apply)
  * Allow to configure users (director/users)
  * Allow to configure notifications (director/notifications)
  * Allow to inspect objects through the Icinga 2 API (could contain sensitive information) (director/inspect)
  * Allow unrestricted access to Icinga Director (director/*)
  * Allow access to module doc (module/doc)
  * Allow access to module monitoring (module/monitoring)
  * Allow all commands (monitoring/command/*)
  * Allow scheduling host and service checks (monitoring/command/schedule-check)
  * Allow acknowledging host and service problems (monitoring/command/acknowledge-problem)
  * Allow removing problem acknowledgements (monitoring/command/remove-acknowledgement)
  * Allow adding and deleting host and service comments (monitoring/command/comment/*)
  * Allow commenting on hosts and services (monitoring/command/comment/add)
  * Allow deleting host and service comments (monitoring/command/comment/delete)
  * Allow scheduling and deleting host and service downtimes (monitoring/command/downtime/*)
  * Allow scheduling host and service downtimes (monitoring/command/downtime/schedule)
  * Allow deleting host and service downtimes (monitoring/command/downtime/delete)
  * Allow processing host and service check results (monitoring/command/process-check-result)
  * Allow processing commands for toggling features on an instance-wide basis (monitoring/command/feature/instance)
  * Allow processing commands for toggling features on host and service objects (monitoring/command/feature/object/*)
  * Allow processing commands for toggling active checks on host and service objects (monitoring/command/feature/object/active-checks)
  * Allow processing commands for toggling passive checks on host and service objects (monitoring/command/feature/object/passive-checks)
  * Allow processing commands for toggling notifications on host and service objects (monitoring/command/feature/object/notifications)
  * Allow processing commands for toggling event handlers on host and service objects (monitoring/command/feature/object/event-handler)
  * Allow processing commands for toggling flap detection on host and service objects (monitoring/command/feature/object/flap-detection)
  * Allow sending custom notifications for hosts and services (monitoring/command/send-custom-notification)
  * Allow access to module setup (module/setup)
  * Allow access to module translation (module/translation)

===== Rôles =====

==== Administrators ====

^User(s)| |
^Group(s)||

^Droits|Allow everything (*)|
==== Developpeurs ====

^User(s)| |
^Group(s)| |

^ Droits |Allow to share navigation items (application/share/navigation)|
^:::|Allow to adjust in the preferences whether to show stacktraces (application/stacktraces)|
^:::|Allow to view the application log (application/log)|
^:::|Grant admin permissions, e.g. manage announcements (admin)|
^:::|Allow access to module director (module/director)|
^:::|Allow to access the director API (director/api)|
^:::|Allow to access the full audit log (director/audit)|
^:::|Allow to show configuration (could contain sensitive information) (director/showconfig)|
^:::|Allow to show the full executed SQL queries in some places (director/showsql)|
^:::|Allow to deploy configuration (director/deploy)|
^:::|Allow to configure hosts (director/hosts)|
^:::|Allow to configure services (director/services)|
^:::|Allow to configure service sets (director/servicesets)|
^:::|Allow to define Service Set Apply Rules (director/service_set/apply)|
^:::|Allow to configure users (director/users)|
^:::|Allow to configure notifications (director/notifications)|
^:::|Allow to inspect objects through the Icinga 2 API (could contain sensitive information) (director/inspect)|
^:::|Allow unrestricted access to Icinga Director (director/*)|
^:::|Allow access to module doc (module/doc)|
^:::|Allow access to module monitoring (module/monitoring)|
^:::|Allow all commands (monitoring/command/*)|
^:::|Allow scheduling host and service checks (monitoring/command/schedule-check)|
^:::|Allow acknowledging host and service problems (monitoring/command/acknowledge-problem)|
^:::|Allow removing problem acknowledgements (monitoring/command/remove-acknowledgement)|
^:::|Allow adding and deleting host and service comments (monitoring/command/comment/*)|
^:::|Allow commenting on hosts and services (monitoring/command/comment/add)|
^:::|Allow deleting host and service comments (monitoring/command/comment/delete)|
^:::|Allow scheduling and deleting host and service downtimes (monitoring/command/downtime/*)|
^:::|Allow scheduling host and service downtimes (monitoring/command/downtime/schedule)|
^:::|Allow deleting host and service downtimes (monitoring/command/downtime/delete)|
^:::|Allow processing host and service check results (monitoring/command/process-check-result)|
^:::|Allow processing commands for toggling features on an instance-wide basis (monitoring/command/feature/instance)|
^:::|Allow processing commands for toggling features on host and service objects (monitoring/command/feature/object/*)|
^:::|Allow processing commands for toggling active checks on host and service objects (monitoring/command/feature/object/active-checks)|
^:::|Allow processing commands for toggling passive checks on host and service objects (monitoring/command/feature/object/passive-checks)|
^:::|Allow processing commands for toggling notifications on host and service objects (monitoring/command/feature/object/notifications)|
^:::|Allow processing commands for toggling event handlers on host and service objects (monitoring/command/feature/object/event-handler)|
^:::|Allow processing commands for toggling flap detection on host and service objects (monitoring/command/feature/object/flap-detection)|
^:::|Allow sending custom notifications for hosts and services (monitoring/command/send-custom-notification)|
==== Techniciens ====

^User(s)| |
^Group(s)| |

^ Droits |Allow to view the application log (application/log)|
^:::|Grant admin permissions, e.g. manage announcements (admin)|
^:::|Allow access to module monitoring (module/monitoring)|
^:::|Allow all commands (monitoring/command/*)|
^:::|Allow scheduling host and service checks (monitoring/command/schedule-check)|
^:::|Allow acknowledging host and service problems (monitoring/command/acknowledge-problem)|
^:::|Allow removing problem acknowledgements (monitoring/command/remove-acknowledgement)|
^:::|Allow adding and deleting host and service comments (monitoring/command/comment/*)|
^:::|Allow commenting on hosts and services (monitoring/command/comment/add)|
^:::|Allow deleting host and service comments (monitoring/command/comment/delete)|
^:::|Allow scheduling and deleting host and service downtimes (monitoring/command/downtime/*)|
^:::|Allow scheduling host and service downtimes (monitoring/command/downtime/schedule)|
^:::|Allow deleting host and service downtimes (monitoring/command/downtime/delete)|
^:::|Allow processing host and service check results (monitoring/command/process-check-result)|
^:::|Allow processing commands for toggling features on an instance-wide basis (monitoring/command/feature/instance)|
^:::|Allow processing commands for toggling features on host and service objects (monitoring/command/feature/object/*)|
^:::|Allow processing commands for toggling active checks on host and service objects (monitoring/command/feature/object/active-checks)|
^:::|Allow processing commands for toggling passive checks on host and service objects (monitoring/command/feature/object/passive-checks)|
^:::|Allow processing commands for toggling notifications on host and service objects (monitoring/command/feature/object/notifications)|
^:::|Allow processing commands for toggling event handlers on host and service objects (monitoring/command/feature/object/event-handler)|
^:::|Allow processing commands for toggling flap detection on host and service objects (monitoring/command/feature/object/flap-detection)|
^:::|Allow sending custom notifications for hosts and services (monitoring/command/send-custom-notification)|

==== Viewer ====

^User(s)| |
^Group(s)|(*)|


^  Droits  |Allow access to module monitoring (module/monitoring)|
^:::|Allow all commands (monitoring/command/*)|