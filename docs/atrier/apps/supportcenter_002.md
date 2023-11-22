---
title: Liste de requêtes utiles
description: 
published: true
date: 2023-04-21T10:49:44.065Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:49:40.799Z
---

====== Liste de requêtes utiles ======
----

==== Requêtes reçues par date par priorité ====

<code sql>
SELECT pd.PRIORITYNAME "Priorité",wo.WORKORDERID "ID de la requête",longtodate(wo.CREATEDTIME) "Heure de création" FROM WorkOrder wo LEFT JOIN WorkOrderStates wos ON wo.WORKORDERID=wos.WORKORDERID LEFT JOIN PriorityDefinition pd ON wos.PRIORITYID=pd.PRIORITYID WHERE  ((((longtodate(wo.CREATEDTIME) >= 1523829600000) AND ((longtodate(wo.CREATEDTIME) != 0) AND (longtodate(wo.CREATEDTIME) IS NOT NULL))) AND ((longtodate(wo.CREATEDTIME) <= 1524434399000) AND (((longtodate(wo.CREATEDTIME) != 0) AND (longtodate(wo.CREATEDTIME) IS NOT NULL)) AND (longtodate(wo.CREATEDTIME) != -1)))) AND (wo.DEPARTMENTID = 1))  AND wo.ISPARENT='1'   ORDER BY 3 NULLS FIRST 
</code>

==== Tickets ouverts par semaine ====

<code sql>
SELECT cd.CATEGORYNAME "Catégorie",pd.PRIORITYNAME "Priorité",pd.PRIORITYNAME "Priorité" FROM WorkOrder wo LEFT JOIN WorkOrderStates wos ON wo.WORKORDERID=wos.WORKORDERID LEFT JOIN CategoryDefinition cd ON wos.CATEGORYID=cd.CATEGORYID LEFT JOIN PriorityDefinition pd ON wos.PRIORITYID=pd.PRIORITYID WHERE  ((((wo.CREATEDTIME >= 1523224800000) AND ((wo.CREATEDTIME != 0) AND (wo.CREATEDTIME IS NOT NULL))) AND ((wo.CREATEDTIME <= 1523829599000) AND (((wo.CREATEDTIME != 0) AND (wo.CREATEDTIME IS NOT NULL)) AND (wo.CREATEDTIME != -1)))) AND (wo.DEPARTMENTID = 1))  AND wo.ISPARENT='1' 
</code>

{{:applications:supportcenter:supportcenter_report_ticket_per_week.png|}}


==== MailFetching - THREAD STATE ====
<code sql>
SELECT paramvalue FROM scpglobalconfig WHERE category='MailFetching' and parameter like '%THREAD%';
>> STOPPED
</code>

==== MailFetching - LAST FETCHED TIME ====
<code sql>
SELECT paramvalue FROM scpglobalconfig WHERE category='MailFetching' and parameter like '%LAST%';
>> 1589749513212
</code>
(à convertir)

==== MailFetching - MAIL COUNT ====
<code sql>
SELECT paramvalue FROM scpglobalconfig WHERE category='MailFetching' and parameter like '%COUNT%';
>> 0
</code>

==== Step to increase fetching mail count ====
Execute the below query :

<code sql>select * from scpglobalconfig where category='MailFetching' and parameter='ProcessMails';</code>

if there is no row available then execute below query alone.

<code sql>INSERT INTO scpglobalconfig values('MailFetching','ProcessMails','50');</code>

if entry is there while executing the first query then update its value to 50 by executing below query

<code sql>UPDATE scpglobalconfig set paramvalue='50' where category='MailFetching' and parameter='ProcessMails';</code>