---
title: Urls IcingaWeb (Dashboards)
description: 
published: true
date: 2023-04-21T10:48:13.424Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:09.084Z
---

====== Urls IcingaWeb (Dashboards) ======
----

/etc/icingaweb2/enabledModules/monitoring/configuration.php



  * Hosts = monitoring/list/hosts?sort=host_severity&limit=10
  * Services = monitoring/list/services?sort=service_severity&limit=10
  * Hostgroups = monitoring/list/hostgroups?limit=10
  * Servicegroups = monitoring/list/servicegroups?limit=10



===== Problems =====
  * Host Problems = monitoring/list/hosts?host_problem=1&sort=host_severity
  * Service Problems = monitoring/list/services?service_problem=1&sort=service_severity&dir=desc
  * Service Grid = monitoring/list/servicegrid?problems
  * Current Downtimes = monitoring/list/downtimes?downtime_is_in_effect=1

===== Overview =====
  * Tactical Overview = monitoring/tactical
  * Hosts = monitoring/list/hosts
  * Services = monitoring/list/services
  * Servicegroups = monitoring/list/servicegroups
  * Hostgroups = monitoring/list/hostgroups
  * Contacts = monitoring/list/contacts
  * Contactgroups = monitoring/list/contactgroups
  * Comments = monitoring/list/comments?comment_type=comment|comment_type=ack
  * Downtimes = monitoring/list/downtimes

===== History =====
  * Event Grid = monitoring/list/eventgrid
  * Event Overview = monitoring/list/eventhistory?timestamp>=-7%20days
  * Notifications = monitoring/list/notifications?notification_timestamp>=-7%20days
  * Timeline = monitoring/timeline

===== System =====
  * Monitoring Health = monitoring/health/info


===== Dashboard - Current Incidents =====
  * Service Problems = monitoring/list/services?service_problem=1&limit=10&sort=service_severity
  * Recently Recovered Services = monitoring/list/services?service_state=0&limit=10&sort=service_last_state_change&dir=desc
  * Host Problems = monitoring/list/hosts?host_problem=1&sort=host_severity


===== Dashboard - Overdue =====
  * Late Host Check Results = monitoring/list/hosts?host_next_update<now
  * Late Service Check Results = monitoring/list/services?service_next_update<now
  * Acknowledgements Active For At Least Three Days = monitoring/list/comments?comment_type=Ack&comment_timestamp<-3 days&sort=comment_timestamp&dir=asc
  * Downtimes Active For More Than Three Days = monitoring/list/downtimes?downtime_is_in_effect=1&downtime_scheduled_start<-3%20days&sort=downtime_start&dir=asc

===== Dashboard - Overview =====
  * Service Grid = monitoring/list/servicegrid?limit=15,18
  * Service Groups = monitoring/list/servicegroups
  * Host Groups = monitoring/list/hostgroups

===== Dashboard - Muted =====
  * Disabled Service Notifications = monitoring/list/services?service_notifications_enabled=0&limit=10
  * Disabled Host Notifications = monitoring/list/hosts?host_notifications_enabled=0&limit=10
  * Disabled Service Checks = monitoring/list/services?service_active_checks_enabled=0&limit=10
  * Disabled Host Checks = monitoring/list/hosts?host_active_checks_enabled=0&limit=10
  * Acknowledged Problem Services = monitoring/list/services?service_acknowledgement_type=2&service_problem=1&sort=service_state&limit=10
  * Acknowledged Problem Hosts = monitoring/list/hosts?host_acknowledgement_type=2&host_problem=1&sort=host_severity&limit=10
  * 

===== Activity Stream =====
  * Recent Events = monitoring/list/eventhistory?timestamp>=-3%20days&sort=timestamp&dir=desc&limit=8
  * Recent Hard State Changes = monitoring/list/eventhistory?timestamp>=-3%20days&type=hard_state&sort=timestamp&dir=desc&limit=8
  * Recent Notifications = monitoring/list/eventhistory?timestamp>=-3%20days&type=notify&sort=timestamp&dir=desc&limit=8
  * Downtimes Recently Started = monitoring/list/eventhistory?timestamp>=-3%20days&type=dt_start&sort=timestamp&dir=desc&limit=8
  * Downtimes Recently Ended = monitoring/list/eventhistory?timestamp>=-3%20days&type=dt_end&sort=timestamp&dir=desc&limit=8


===== Stats =====
  * Check Stats = monitoring/health/stats
  * Process Information = monitoring/health/info


<code>
Info :
CSS =
/*
 * CSS
 */
$this->provideCssFile('service-grid.less');
$this->provideCssFile('tables.less');
</code>

===== showFullscreen (Hides the left menu and optimizes the layout for full screen resolution.) =====

Examples for showFullscreen:

http://localhost/icingaweb2/dashboard?showFullscreen 

http://localhost/icingaweb2/monitoring/list/services?service_problem=1&sort=service_severity&showFullscreen

===== showCompact (Provides a compact view. Hides the title and upper menu. This is helpful to embed a dashboard item into an external iframe) =====

Examples for showCompact:

http://localhost/icingaweb2/dashboard?showCompact&showFullscreen

http://localhost/icingaweb2/monitoring/list/services?service_problem=1&sort=service_severity&showCompact

===== Format (Selected views can be exported as JSON or CSV. This also is available in the upper menu. You can also export the SQL queries for manual analysis) =====

Examples for format:

http://localhost/icingaweb2/monitoring/list/services?format=json

http://localhost/icingaweb2/monitoring/list/services?service_problem=1&sort=service_severity&dir=desc&format=csv


===== Autre urls custom =====
  * [[https://localhost/monitoring/list/hosts?addColumns=host_check_source,_host_os&_host_status=build&host_problem=1&modifyFilter=1]]
  * [[https://localhost/monitoring/list/hosts?addColumns=host_check_timeperiod]]