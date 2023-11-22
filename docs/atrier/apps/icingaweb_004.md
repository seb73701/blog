---
title: Personnalisation logo de la page "Connexion"
description: 
published: true
date: 2023-04-21T10:47:40.819Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:47:36.508Z
---

======  Personnalisation logo de la page "Connexion" ======
----

Voici comment changer le logo de la page "Connexion" d'Icinga Web 2 par un nouveau logo.

{{:supervision:icingaweb2:supervision:icingaweb2:personalisation_logo-20171117-000437.png?nolink}}

Le logo est déclaré dans une feuille de style qui se trouve dans le fichier : ''/usr/share/icingaweb2/public/css/icinga/layout.less''.

Voici les blocs à modifier (j'ai commenté les lignes originales représentées par ''/* ... */'' et rajouté la propriétés ''background-image: url(...);'' pointant vers un logo différent) :

<code>
#icinga-logo {
  /*background-image: url('../img/icinga-logo-big.svg');*/
  background-image: url('https://www.domain.fr/sites/www.domain.fr/files/logo-domain.png');
  ...
}

</code>

<code>
.ie8 {
  ...
  #icinga-logo {
    /*background-image: url('../img/icinga-logo-big.png');*/
    background-image: url('https://www.domain.fr/sites/www.domain.fr/files/logo-domain.png');
  }
}

</code>

Voici le résultat final :

{{:supervision:icingaweb2:supervision:icingaweb2:personalisation_logo-20171117-000236.png?nolink}}

===== Changement de l'image de fond =====

Par défaut, la page de connexion ressemble a ceci :

{{:supervision:icingaweb2:img_defaut_icingaweb2.png?500|}}

Pour personnaliser, il faut ouvrir le fichier : ''/usr/share/icingaweb2/public/css/icinga/login.less''

<code>vim /usr/share/icingaweb2/public/css/icinga/login.less</code>

et mettre ce code :
<code>
  /*background-image: url(../img/icingaweb2-background.jpg);*/
  background-image: url(../img/img.jpg);
</code>

Le résultat final est : 

{{:supervision:icingaweb2:img_cs_icingaweb2.png?500|}}