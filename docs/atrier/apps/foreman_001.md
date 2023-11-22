---
title: Listing Hosts
description: 
published: true
date: 2023-04-21T10:46:24.130Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:46:20.702Z
---

<code>
Comme vu ensemble, la requête api pour les « hosts » est trop lourde pour « TheForeman ».
En effet, elle récupère de très nombreuses informations comme le réseau, les disques, confi vm vmware …
En passant directement par les facts, on limite la remontée d’information au stricte nécessaire.

Voici comment faire :
1/ Récupère, dans un premier temps, tous les hôtes grâces aux facts :
                Uri  GET /api/fact_values 
                Filter  facts.operatingsystem ~ CentOS or facts.operatingsystem ~ Debian

2/ Puis, récupérer tous les hôtes ayant le fact « update_security_status » : 
                Uri  GET /api/fact_values 
                Filter   facts.update_security_status != null

3/ Et enfin, croiser les deux tableaux pour obtenir le résultat

</code>