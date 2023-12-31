---
title: temp1
description: 
published: true
date: 2023-04-21T11:07:58.059Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:07:54.065Z
---

---
title: Get-Date : Manipuler la date en PowerShell
description: 
published: true
date: 2023-02-03T10:14:19.857Z
tags: 
editor: markdown
dateCreated: 2021-02-17T12:38:44.024Z
---

---
title: Get-Date : Manipuler la date en PowerShell
description: 
published: true
date: 2023-01-27T18:07:05.886Z
tags: 
editor: markdown
dateCreated: 2021-02-17T12:38:44.024Z
---

---
title: Get-Date : Manipuler la date en PowerShell
description: 
published: true
date: 2021-02-17T12:38:44.024Z
tags: 
editor: markdown
dateCreated: 2021-02-17T12:38:44.024Z
---

====== Get-Date : Manipuler la date en PowerShell ======

===== I. Présentation =====
PowerShell dispose d'un commandlet officiel pour récupérer la date et l'heure sur une machine : ''Get-Date''. Derrière ce nom de commande facile à retenir, se cache un outil indispensable pour nos scripts PowerShell et on s'en servira notamment pour : alimenter un fichier de log avec la date et l'heure des différents événements, comparer la date actuelle avec la date d'un objet (exemple fichier), etc.

===== II. Récupérer la date sous différents formats avec PowerShell =====

Comment récupérer la date sous différentes formes afin d'adapter la valeur retournée par ''Get-Date''.

<code>Get-Date</code>

Le format de sortie par défaut est le suivant : samedi 9 mars 2019 13:57:13

Avec le paramètre ''-Format'', nous allons pouvoir le personnaliser en jouant avec les lettres ''m'', ''d'', ''y'', ''h'', ''s'', ''f'' où la casse est importante et impacte le résultat retourné.

Voici quelques exemples :

  * Obtenir la date sous la forme "**AnnéeMoisJour**"
<code>Get-Date -Format "yyyyMMdd"</code>
<color grey>//Sortie ://</color>
<code>20190310</code>
 

  * Obtenir la date sous la forme "**Jour/Mois/Année**"
<code>Get-Date -Format "dd/MM/yyyy"</code>
<color grey>//Sortie ://</color>
<code>10/03/2019</code>
 

  * Obtenir **l'heure sans les secondes** :
<code>Get-Date -Format "HH:mm"</code>
<color grey>//Sortie ://</color>
<code>08:36</code>
 

  * Obtenir **le mois en toute lettre** :
<code>Get-Date -Format "MMMM"</code>
<color grey>//Sortie ://</color>
<code>mars</code>
 

  * Obtenir la date et l'heure séparées par un underscore sous la forme "**AnnéeMoisJour_HeureMinute**"
<code>Get-Date -Format "yyyyMMdd_HHmm"</code>
<color grey>//Sortie ://</color>
<code>20190310_0822</code>
 

  * Pour obtenir seulement **l'heure avec les secondes** :
<code>Get-Date -DisplayHint Time</code>
<color grey>//Sortie ://</color>
<code>13:58:58</code>
 

  * Ce qui est valable aussi pour **la date (dans son format par défaut)** :
<code>Get-Date -DisplayHint Date</code>
<color grey>//Sortie ://</color>
<code>samedi 9 mars 2019</code>
 

  * Récupérer une date spécifique
Pour récupérer de façon statique une date et la stocker dans une variable pour hériter du bon type de valeur (''DateTime''), on peut directement faire cela :

<code>$DateHeureStatique = (Get-Date -Year 2019 -Month 04 -Day 16 -Hour 00 -Minute 00)</code>
<color grey>//Sortie ://</color>
<code>mardi 16 avril 2019 00:00:48</code>

===== III. Calculer une date avec "AddDays" =====

Dans un script, par exemple qui cible l'Active Directory, on peut chercher à obtenir la liste des comptes utilisateurs non utilisés ces 90 derniers jours. Pour cela, il va falloir obtenir une variable contenant la date d'il y a 90 jours, de façon glissante puisque cette date va changer tous les jours.

Pour calculer une date, nous allons utiliser la méthode ''AddDays()'' qui s'applique directement à ''Get-Date'' et qui attend un nombre comme valeur. Si le nombre est positif, le nombre de jours sera ajouté à la date du jour, s'il est négatif, il sera soustrait et la date calculée sera retournée.

Pour soustraire 90 jours à la date du jour, cela nous donne :

<code>(Get-Date).AddDays(-90)</code>

Par exemple :
<code>
Write-Output "Date du jour : $(Get-Date)"
Write-Output "Date il y a 90 jours : $((Get-Date).AddDays(-90))"
Write-Output "Date dans 90 jours : $((Get-Date).AddDays(90))"
</code>
<color grey>//Sortie ://</color>
<code>
Date du jour : 05/05/2019 09:03:40
Date il y a 90 jours : 02/04/2019 09:03:40
Date dans 90 jours : 08/03/2019 09:03:40
</code>

Votre date de référence étant calculée, vous allez pouvoir comparer facilement cette date avec une autre ! Une condition dans votre script avec ''-lt'' ou ''-gt'' sera très efficace pour savoir si une date est "**plus grande**" qu'une autre ou pas.

<WRAP center round info 80%>Si vous avez besoin d'additionner ou soustraire des heures, des minutes ou des secondes, c'est possible ! Il suffit d'utiliser la méthode adaptée : ''AddHours()'', ''AddMinutes()'', ''AddSeconds()'' ou encore ''AddMonths()'' pour les mois.</WRAP>


===== IV. Convertir la date en timestamp =====

L'horodatage (timestamp) est une valeur numérique qui a pour objectif de représenter la date et l'heure selon un format bien précis. Dans certains cas, vous pourriez avoir besoin de comparer deux timestamps, si vous avez un timestamp d'un côté et la date/heure dans un autre format de l'autre, ça pose problème.

Il est alors nécessaire de convertir la chaîne "date - heure" en valeur timestamp. Pour y parvenir, nous allons utiliser la méthode ToFileTime() issue de .NET Framework.

Ce qui nous donne :

<code>(Get-Date).ToFileTime()</code>
<color grey>//Sortie ://</color>
<code>131975864278102003</code>
La valeur retournée est alors un horodatage.

===== V. Récupérer d'autres informations liées au calendrier =====

  * Comment savoir quel jour de l'année on est grâce à PowerShell ? 

<code>(Get-Date).DayOfYear</code>
<color grey>//Sortie ://</color>
<code>70</code>

  * On peut obtenir la même information mais pour avoir le numéro du mois en cours :

<code>(Get-Date).Month</code>
<color grey>//Sortie ://</color>
<code>3</code>

Il est aussi possible d'obtenir des détails sur le jour en cours : combien est-ce qu'il y a eu de secondes écoulées aujourd'hui, et même de millisecondes ? Via cette commande c'est facile :

<code>(Get-Date).TimeOfDay</code>
<color grey>//Sortie ://</color>
<code>
Days : 0
Hours : 8
Minutes : 17
Seconds : 12
Milliseconds : 644
Ticks : 298326447959
TotalDays : 0,345285240693287
TotalHours : 8,28684577663889
TotalMinutes : 497,210746598333
TotalSeconds : 29832,6447959
TotalMilliseconds : 29832644,7959
</code>
