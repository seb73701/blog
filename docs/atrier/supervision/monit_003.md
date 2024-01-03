---
title: temp2
description: 
published: true
date: 2023-04-21T10:41:21.623Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:41:17.103Z
---

---
title: Supervision : Firewall
description: 
published: true
date: 2023-02-03T10:15:54.627Z
tags: 
editor: markdown
dateCreated: 2021-02-17T13:36:14.362Z
---

---
title: Supervision : Firewall
description: 
published: true
date: 2023-01-27T17:52:57.551Z
tags: 
editor: markdown
dateCreated: 2021-02-17T13:36:14.362Z
---

---
title: Supervision : Firewall
description: 
published: true
date: 2021-02-17T13:36:14.362Z
tags: 
editor: markdown
dateCreated: 2021-02-17T13:36:14.362Z
---

====== Supervision : Firewall ======
----

===== Fortinet - Fortigate =====

==== FortiOS 5.x ====

[[http://help.fortinet.com/fos60hlp/60/Content/FortiOS/fortigate-system-administration/Monitoring/SNMP.htm?Highlight=mib%20snmp#Fortinet]]
[[http://help.fortinet.com/fos60hlp/60/Content/FortiOS/fortigate-firewall/IPv6/IPv6%20Features/New_Fortinet_FortiGate_IPv6_MIB.htm?Highlight=mib%20snmp]]
==== General ====

^  MIB file name or RFC  ^  Description  ^
|**FORTINET-CORE-MIB.mib**|The Fortinet MIB includes all system configuration information and trap information that is common to all Fortinet products.\\ Your SNMP manager requires this information to monitor FortiGate unit configuration settings and receive traps from the FortiGate SNMP agent.|
|**FORTINET-FORTIGATE-MIB.mib**|The FortiGate MIB includes all system configuration information and trap information that is specific to FortiGate units.\\ Your SNMP manager requires this information to monitor FortiGate configuration settings and receive traps from the FortiGate SNMP agent. FortiManager systems require this MIB to monitor FortiGate units.|
|**RFC-1213 (MIB II)**|The FortiGate SNMP agent supports MIB II groups with these exceptions.\\  * No support for the EGP group from MIB II (RFC 1213, section 3.11 and 6.10).\\  * Protocol statistics returned for MIB II groups (IP/ICMP/TCP/UDP/etc.) do not accurately capture all FortiGate traffic activity. More accurate information can be obtained from the information reported by the Fortinet MIB.|
|**RFC-2665 (Ethernet-like MIB)**|The FortiGate SNMP agent supports Ethernet-like MIB information. FortiGate SNMP does not support for the dot3Tests and dot3Errors groups.|


=== Cluster HA ===

Source : [[http://docs-legacy.fortinet.com/fos50hlp/50/index.html#page/FortiOS%25205.0%2520Help/HA_maintenance.085.19.html]]


^  MIB field  ^  OID  ^  Description  ^
|fgHaSystemMode|.1.3.6.1.4.1.12356.101.13.1.1.0|HA mode (standalone, a-a, or a-p)|
|fgHaGroupId|.1.3.6.1.4.1.12356.101.13.1.2.0|The HA priority of the cluster unit. Default 128.|
|fgHaPriority|.1.3.6.1.4.1.12356.101.13.1.3.0|The HA priority of the cluster unit. Default 128.|
|fgHaOverride|.1.3.6.1.4.1.12356.101.13.1.4.0|Whether HA override is disabled or enabled for the cluster unit.|
|fgHaAutoSync|.1.3.6.1.4.1.12356.101.13.1.5.0|Whether automatic HA synchronization is disabled or enabled.|
|fgHaSchedule|.1.3.6.1.4.1.12356.101.13.1.6.0|The HA load balancing schedule. Set to none unless operating in a-p mode.|
|fgHaGroupName|.1.3.6.1.4.1.12356.101.13.1.7.0|The HA group name.|
|fgHaStatsIndex|.1.3.6.1.4.1.12356.101.13.2.1.1.1.1|The cluster index of the cluster unit. 1 for the primary unit, 2 to x for the subordinate units.|
|fgHaStatsSerial|.1.3.6.1.4.1.12356.101.13.2.1.1.2.1|The serial number of the cluster unit.|
|fgHaStatsCpuUsage|.1.3.6.1.4.1.12356.101.13.2.1.1.3.1|The cluster unit’s current CPU usage.|
|fgHaStatsMemUsage|.1.3.6.1.4.1.12356.101.13.2.1.1.4.1|The cluster unit’s current Memory usage.|
|fgHaStatsNetUsage|.1.3.6.1.4.1.12356.101.13.2.1.1.5.1|The cluster unit’s current Network bandwidth usage.|
|fgHaStatsSesCount|.1.3.6.1.4.1.12356.101.13.2.1.1.6.1|The cluster unit’s current session count.|
|fgHaStatsPktCount|.1.3.6.1.4.1.12356.101.13.2.1.1.7.1|The cluster unit’s current packet count.|
|fgHaStatsByteCount|.1.3.6.1.4.1.12356.101.13.2.1.1.8.1|The cluster unit’s current byte count.|
|fgHaStatsIdsCount|.1.3.6.1.4.1.12356.101.13.2.1.1.9.1|The number of attacks reported by the IPS for the cluster unit.|
|fgHaStatsAvCount|.1.3.6.1.4.1.12356.101.13.2.1.1.10.1|The number of viruses reported by the antivirus system for the cluster unit.|
|fgHaStatsHostname|.1.3.6.1.4.1.12356.101.13.2.1.1.11.1|The hostname of the cluster unit.|