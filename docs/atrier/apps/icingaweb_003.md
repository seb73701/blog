---
title: Personnalisation logo du Dashboard
description: 
published: true
date: 2023-04-21T10:47:34.319Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:31.051Z
---

======  Personnalisation logo du Dashboard ======
----

Voici comment changer le logo du Dashboard d'Icinga Web 2 par un nouveau logo.

{{:supervision:icingaweb2:supervision:icingaweb2:personnalisation_logo_dashboard-20171117-001558.png?nolink}}

Le logo est déclaré dans une feuille de style qui se trouve dans le fichier : ''/usr/share/icingaweb2/public/css/icinga/layout.less''.

Voici les blocs à modifier (j'ai commenté les lignes originales représentées par ''/* ... */'' et rajouté la propriétés ''background-image: url(...);'' pointant vers un logo différent) :

<code>
#header-logo {
  /*background-image: url('../img/icinga-logo.svg');*/
  background-image: url('https://www.domain.fr/sites/www.domain.fr/files/logo-domain.png');
  ...
}
</code>


<code>
.ie8 {
  #header-logo {
    /*background-image: url('../img/icinga-logo.png');*/
    background-image: url('https://www.domain.fr/sites/www.domain.fr/files/logo-domain.png');
  }
 ...
}

</code>

Voici le résultat final :

{{:supervision:icingaweb2:supervision:icingaweb2:personnalisation_logo_dashboard-20171117-001404.png?nolink}}

===== Personnalisation des icones "Réseaux sociaux" =====

Fichier : ''/usr/share/icingaweb2/application/views/scripts/authentication/login.phtml''

