---
title: Red Hat - Aide
description: 
published: true
date: 2023-04-21T11:14:17.067Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:14:12.147Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[<i class="fas fa-chevron-circle-left"></i>  Red Hat Enterprise Linux (RHEL)](rhel.md)

----

# Red Hat - Aide

## Aide propre aux commandes

Il n'est pas possible de connaître par coeur tous les paramètres et arguments d'une commande. Linux propose heureusement au moins deux mécanismes pour connaître ceux qui sont supportés par une commande. La plupart du temps, le paramètre `--help` affiche l'aide incluse directement au sein du programme appelé. Parfois l'aide apportée est suffisante pour trouver ce qu'on recherche. C'est le cas avec la commande `date` dotn la sortie est volontairement tronquée ici car elle prendrait deux pages.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ date --help
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
Utilisation : date [OPTION]... [+FORMAT]
         ou : date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]
Afficher la date actuelle au FORMAT indiqué ou initialiser la date système.

Les arguments obligatoires pour les options longues le sont aussi pour les
options courtes.
  -d, --date=CHAÎNE          afficher la date décrite par CHAÎNE, pas « now » (maintenant)
      --debug                annoter la date analysée
                                                           et émettre des avertissements sur stderr au sujet
                              des utilisations douteuses
  -f, --file=FICHIER_DATES   comme --date mais pour chaque ligne de FICHIER_DATES
  -I[FMT], --iso-8601[=FMT]  afficher la date et l'heure au format
                               ISO 8601. FMT est « date » pour la date seule
                               (par défaut), « hours », « minutes », « seconds » ou
                               « ns » pour indiquer la précision des date et heure.
                               Exemple : 2006-08-14T02:34:56-06:00
  -R, --rfc-email            afficher la date et l'heure au format RFC 5322.
                               Exemple : Mon, 07 Aug 2006 12:34:56 -0600
...
```

Il peut cependant qu'il arrive que l'aide soit trop concise ou manque d'explication, ou bien soit totalement absente. Dans ce cas `--help` est considéré comme un paramètre invalide et on risque d'obtenir un message d'erreur et/ou une ligne d'informations.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ sestatus --help
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
sestatus: invalid option -- '-'

Usage: sestatus [OPTION]

  -v  Verbose check of process and file contexts.
  -b  Display current state of booleans.

Without options, show SELinux status.
```

----

## Aide interne au shell

Les commandes internes n'acceptent pas de paramètre `--help` mais pour ces commandes l'interpréteur de commandes propose une commande `help`. Utilisée seule elle fournit la liste des commandes internes. Si on lui passe comme paramètre le nom d'une commande interne, l'aide de celle-ci est affichée. C'est ainsi que l'on peut apprendre que `pwd` admet deux paramères optionnels.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ help pwd
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```
pwd: pwd [-LP]
    Affiche le nom du répertoire de travail courant.

    Options :
      -L        affiche la valeur de $PWD s'il nomme le répertoire de travail
                courant
      -P        affiche le répertoire physique, sans aucun lien symbolique

    Par défaut, « pwd » se comporte comme si « -L » était spécifié.

    Code de retour :
    Renvoie 0 à moins qu'une option non valable ne soit donnée ou que le
    répertoire courant ne peut pas être lu.
```

----

## Manuel en ligne

### Accès

Quand les deux mécanismes d'aide précédents se révèlent être insuffisants, il est très probable que l'aide recherchée se situe au sein du manuel Unix. Ce manuel est standard sur tous les Unix dont Linux, et quel que soit le shell puisqu'il s'agit d'une commande externe.

Le manuel est accessible depuis la commande `man`. On peut faire un essai simple avec la commande `date` : `$ man date`.

Le mode d'emploi de la commande en paramètre de `man` est affiché.



### Structure d'une page

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ man date
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```
DATE(1)                                                                                                      User Commands                                                                                                      DATE(1)

NAME
       date - print or set the system date and time

SYNOPSIS
       date [OPTION]... [+FORMAT]
       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

DESCRIPTION
       Display the current time in the given FORMAT, or set the system date.

       Mandatory arguments to long options are mandatory for short options too.

       -d, --date=STRING
              display time described by STRING, not 'now'

       --debug
              annotate the parsed date, and warn about questionable usage to stderr

       -f, --file=DATEFILE
              like --date; once for each line of DATEFILE

       -I[FMT], --iso-8601[=FMT]
              output date/time in ISO 8601 format.  FMT='date' for date only (the default), 'hours', 'minutes', 'seconds', or 'ns' for date and time to the indicated precision.  Example: 2006-08-14T02:34:56-06:00

       -R, --rfc-email
              output date and time in RFC 5322 format.  Example: Mon, 14 Aug 2006 02:34:56 -0600

       --rfc-3339=FMT
              output date/time in RFC 3339 format.  FMT='date', 'seconds', or 'ns' for date and time to the indicated precision.  Example: 2006-08-14 02:34:56-06:00

       -r, --reference=FILE
              display the last modification time of FILE

       -s, --set=STRING
              set time described by STRING

       -u, --utc, --universal
              print or set Coordinated Universal Time (UTC)

       --help display this help and exit

       --version
              output version information and exit

       FORMAT controls the output.  Interpreted sequences are:

       %%     a literal %

       %a     locale's abbreviated weekday name (e.g., Sun)

       %A     locale's full weekday name (e.g., Sunday)

       %b     locale's abbreviated month name (e.g., Jan)

       %B     locale's full month name (e.g., January)

       %c     locale's date and time (e.g., Thu Mar  3 23:05:25 2005)

       %C     century; like %Y, except omit last two digits (e.g., 20)

       %d     day of month (e.g., 01)

       %D     date; same as %m/%d/%y

       %e     day of month, space padded; same as %_d

       %F     full date; same as %Y-%m-%d

       %g     last two digits of year of ISO week number (see %G)

       %G     year of ISO week number (see %V); normally useful only with %V

       %h     same as %b

       %H     hour (00..23)

       %I     hour (01..12)

       %j     day of year (001..366)

       %k     hour, space padded ( 0..23); same as %_H

       %l     hour, space padded ( 1..12); same as %_I

       %m     month (01..12)

       %M     minute (00..59)

       %n     a newline

       %N     nanoseconds (000000000..999999999)

       %p     locale's equivalent of either AM or PM; blank if not known

       %P     like %p, but lower case

       %q     quarter of year (1..4)

       %r     locale's 12-hour clock time (e.g., 11:11:04 PM)

       %R     24-hour hour and minute; same as %H:%M

       %s     seconds since 1970-01-01 00:00:00 UTC

       %S     second (00..60)

       %t     a tab

       %T     time; same as %H:%M:%S

       %u     day of week (1..7); 1 is Monday

       %U     week number of year, with Sunday as first day of week (00..53)

       %V     ISO week number, with Monday as first day of week (01..53)

       %w     day of week (0..6); 0 is Sunday

       %W     week number of year, with Monday as first day of week (00..53)

       %x     locale's date representation (e.g., 12/31/99)

       %X     locale's time representation (e.g., 23:13:48)

       %y     last two digits of year (00..99)

       %Y     year

       %z     +hhmm numeric time zone (e.g., -0400)

       %:z    +hh:mm numeric time zone (e.g., -04:00)

       %::z   +hh:mm:ss numeric time zone (e.g., -04:00:00)

       %:::z  numeric time zone with : to necessary precision (e.g., -04, +05:30)

       %Z     alphabetic time zone abbreviation (e.g., EDT)

       By default, date pads numeric fields with zeroes.  The following optional flags may follow '%':

       -      (hyphen) do not pad the field

       _      (underscore) pad with spaces

       0      (zero) pad with zeros

       ^      use upper case if possible

       #      use opposite case if possible

       After  any flags comes an optional field width, as a decimal number; then an optional modifier, which is either E to use the locale's alternate representations if available, or O to use the locale's alternate numeric symbols
       if available.

EXAMPLES
       Convert seconds since the epoch (1970-01-01 UTC) to a date

              $ date --date='@2147483647'

       Show the time on the west coast of the US (use tzselect(1) to find TZ)

              $ TZ='America/Los_Angeles' date

       Show the local time for 9AM next Friday on the west coast of the US

              $ date --date='TZ="America/Los_Angeles" 09:00 next Fri'

DATE STRING
       The --date=STRING is a mostly free format human readable date string such as "Sun, 29 Feb 2004 16:21:42 -0800" or "2004-02-29 16:21:42" or even "next Thursday".  A date string may contain items indicating calendar date, time
       of day, time zone, day of week, relative time, relative date, and numbers.  An empty string indicates the beginning of the day.  The date string format is more complex than is easily documented here but is fully described in
       the info documentation.

ENVIRONMENT
       TZ     Specifies the timezone, unless overridden by command line parameters.  If neither is specified, the setting from /etc/localtime is used.

AUTHOR
       Written by David MacKenzie.

REPORTING BUGS
       GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
       Report date translation bugs to <https://translationproject.org/team/>

COPYRIGHT
       Copyright © 2018 Free Software Foundation, Inc.  License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>.
       This is free software: you are free to change and redistribute it.  There is NO WARRANTY, to the extent permitted by law.

SEE ALSO
       Full documentation at: <https://www.gnu.org/software/coreutils/date>
       or available locally via: info '(coreutils) date invocation'

GNU coreutils 8.30                                                                                             July 2018                                                                                                        DATE(1)

```

Une page du manuel est composée de plusieurs sections dont celles-ci, sachant qu'elles ne sont pas forcément toutes présentes :

+ **Nom** : nom et rôle de la commande
+ **Synopsis** : syntaxe générale, paramètres et arguments acceptés.
+ **Description** : mode d'emploi détaillé du fonctionnement de la commande et des arguments principaux.
+ **Options** : description détaillée de chaque paramètre possible, généralement sous forme de liste.
+ **Exemples** : le manuel peut fournir des exemples de concepts d'utilisation de la commande.
+ **Environnement** : le fonctionnement de la commande peut réagir différemment si des variables du shell sont positionnées à certaines valeurs.
+ **Conformité** : la commandeest conforme à des recommandations ou normes (par exemple POSIX).
+ **Bogues** : la commande peut parfois rencontrer des dysfonctionnement dans des cas ponctuels qui peuvent être énumérés à cet endroit.
+ **Diagnostics/retour** : la commande, selon son résultat, peut retourner des codes d'erreurs significatifs dont la valeur permet de déterminer le type de problème (fichier en argument absent, etc.).
+ **Voir aussi** : liste des commandes liées au programme qui peuvent intéresser l'utilisateur.

### Navigation

+ La barre d'espace `[Espace]` défile une page complète.
+ La touche `[Entrée]` défile ligne par ligne.
+ Les touches `[Haut]` et `[Bas]` défilent d'une ligne vers le haut ou le vers le bas.
+ Les touches `[Pageup]` et `[Pagedown]` défilent d'une demi-page vers le haut ou vers le bas.
+ Les touches `[Début]` et `[Fin]` font exactement ce qu'on attend d'elles.
+ La touche `/` permet une recherche. `/toto` recherche **toto**. Dans ce cas la touche `n` cherche l'occurence suivante, tandis que `[Shift] n` (**N**) recherche la précédente.
+ La touche `Q` quitte l'aide et revient au shell.


### Les sections

Le manuel Linux ne fait pas que référencer les commandes classiques. C'est un manuel bien plus complet que ça. Les commandes simples, celles d'administration, les fichiers de configuration, les périphériques, les appels systèmes, les fonctions de programmation de divers langages, et bien d'autres choses encore, peuvent y être référencés. C'est pourquoi le manuel est composé de plusieurs sections distinctes.

|Section|Contenu|
|-------|-------|
|1|Instructions exécutables ou commandes du shell|
|2|Appels système (API du noyau...)|
|3|Appels des bibliothèques (fonction C...)|
|4|Fichiers spéciaux (contenu de `/dev` comme `sd`, `hd`, `pts`, etc.)|
|5|Format des fichiers (`/etc/password`, `/etc/hosts`, etc.)|
|6|Les jeux, éconimiseurs d'écran, gadgets, etc.|
|7|Divers, commandes non standard, ne trouvant pas place ailleurs|
|8|Commandes d'administration du système Linux|
|9|Sous-programmes du noyau (souvent vide)|

Il arrive parfois que l'appel au manuel pour une commande ne retourne pas la page du manuel concernée. C'est que `man` recherche par défaut la première occurrence dans l'ordre des sections.

Si on recherche sur le format du fichier des mots de passe, on tombe tout d'abord sur l'aide de la commande `passwd`. Voir l'en-tête de la page. Le numéro de la section est indiqué juste après le nom de la commande entre parenthèses. La commande `man` a trouvé une occurrence de `passwd` dans la section 1 et affiche la page du manuel associée.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ man passwd
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```
PASSWD(1)                                                                                                    User utilities                                                                                                   PASSWD(1)

NAME
       passwd - update user's authentication tokens
```

## Mise à jour la base de "Man"

On peut mettre à jour la base de données de `man` via la commande `mandb`. Cette commande recherche 

```
[root@localhost ~]# mandb
Traitement des pages du manuel sous /usr/share/man…
Mise à jour du cache d'index pour le chemin « /usr/share/man/man1 ». Attendez…mandb: attention : /usr/share/man/man1/ex.1.gz est un lien symbolique flottant
mandb: attention : /usr/share/man/man1/rview.1.gz est un lien symbolique flottant
mandb: attention : /usr/share/man/man1/view.1.gz est un lien symbolique flottant
Mise à jour du cache d'index pour le chemin « /usr/share/man/mann ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man…
Recherche des fichiers préformatés sans sources sous /var/cache/man…
Traitement des pages du manuel sous /usr/share/man/ru…
Mise à jour du cache d'index pour le chemin « /usr/share/man/ru/man3 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/ru…
Recherche des fichiers préformatés sans sources sous /var/cache/man/ru…
Traitement des pages du manuel sous /usr/share/man/fr…
Mise à jour du cache d'index pour le chemin « /usr/share/man/fr/man7 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/fr…
Recherche des fichiers préformatés sans sources sous /var/cache/man/fr…
Traitement des pages du manuel sous /usr/share/man/ja…
Mise à jour du cache d'index pour le chemin « /usr/share/man/ja/man7 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/ja…
Recherche des fichiers préformatés sans sources sous /var/cache/man/ja…
Traitement des pages du manuel sous /usr/share/man/ko…
Mise à jour du cache d'index pour le chemin « /usr/share/man/ko/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/ko…
Recherche des fichiers préformatés sans sources sous /var/cache/man/ko…
Traitement des pages du manuel sous /usr/share/man/pl…
Mise à jour du cache d'index pour le chemin « /usr/share/man/pl/man5 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/pl…
Recherche des fichiers préformatés sans sources sous /var/cache/man/pl…
Traitement des pages du manuel sous /usr/share/man/sk…
Mise à jour du cache d'index pour le chemin « /usr/share/man/sk/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/sk…
Recherche des fichiers préformatés sans sources sous /var/cache/man/sk…
Traitement des pages du manuel sous /usr/share/man/cs…
Mise à jour du cache d'index pour le chemin « /usr/share/man/cs/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/cs…
Recherche des fichiers préformatés sans sources sous /var/cache/man/cs…
Traitement des pages du manuel sous /usr/share/man/da…
Mise à jour du cache d'index pour le chemin « /usr/share/man/da/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/da…
Recherche des fichiers préformatés sans sources sous /var/cache/man/da…
Traitement des pages du manuel sous /usr/share/man/de…
Mise à jour du cache d'index pour le chemin « /usr/share/man/de/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/de…
Recherche des fichiers préformatés sans sources sous /var/cache/man/de…
Traitement des pages du manuel sous /usr/share/man/hu…
Mise à jour du cache d'index pour le chemin « /usr/share/man/hu/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/hu…
Recherche des fichiers préformatés sans sources sous /var/cache/man/hu…
Traitement des pages du manuel sous /usr/share/man/id…
Mise à jour du cache d'index pour le chemin « /usr/share/man/id/man5 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/id…
Recherche des fichiers préformatés sans sources sous /var/cache/man/id…
Traitement des pages du manuel sous /usr/share/man/it…
Mise à jour du cache d'index pour le chemin « /usr/share/man/it/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/it…
Recherche des fichiers préformatés sans sources sous /var/cache/man/it…
Traitement des pages du manuel sous /usr/share/man/pt_BR…
Mise à jour du cache d'index pour le chemin « /usr/share/man/pt_BR/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/pt_BR…
Recherche des fichiers préformatés sans sources sous /var/cache/man/pt_BR…
Traitement des pages du manuel sous /usr/share/man/sv…
Mise à jour du cache d'index pour le chemin « /usr/share/man/sv/man7 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/sv…
Recherche des fichiers préformatés sans sources sous /var/cache/man/sv…
Traitement des pages du manuel sous /usr/share/man/tr…
Mise à jour du cache d'index pour le chemin « /usr/share/man/tr/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/tr…
Recherche des fichiers préformatés sans sources sous /var/cache/man/tr…
Traitement des pages du manuel sous /usr/share/man/zh_CN…
Mise à jour du cache d'index pour le chemin « /usr/share/man/zh_CN/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/zh_CN…
Recherche des fichiers préformatés sans sources sous /var/cache/man/zh_CN…
Traitement des pages du manuel sous /usr/share/man/zh_TW…
Mise à jour du cache d'index pour le chemin « /usr/share/man/zh_TW/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/zh_TW…
Recherche des fichiers préformatés sans sources sous /var/cache/man/zh_TW…
Traitement des pages du manuel sous /usr/share/man/es…
Mise à jour du cache d'index pour le chemin « /usr/share/man/es/man1 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/es…
Recherche des fichiers préformatés sans sources sous /var/cache/man/es…
Traitement des pages du manuel sous /usr/share/man/uk…
Mise à jour du cache d'index pour le chemin « /usr/share/man/uk/man1 ». Attendez…mandb: attention : /usr/share/man/uk/man1/dos2unix.1.gz : la recherche de whatis sur dos2unix(1) a échoué
mandb: attention : /usr/share/man/uk/man1/dos2unix.1.gz : la recherche de whatis sur mac2unix(1) a échoué
mandb: attention : /usr/share/man/uk/man1/dos2unix.1.gz : la recherche de whatis sur unix2dos(1) a échoué
mandb: attention : /usr/share/man/uk/man1/dos2unix.1.gz : la recherche de whatis sur unix2mac(1) a échoué
Mise à jour du cache d'index pour le chemin « /usr/share/man/uk/man7 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/uk…
Recherche des fichiers préformatés sans sources sous /var/cache/man/uk…
Traitement des pages du manuel sous /usr/share/man/nl…
Mise à jour du cache d'index pour le chemin « /usr/share/man/nl/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/nl…
Recherche des fichiers préformatés sans sources sous /var/cache/man/nl…
Traitement des pages du manuel sous /usr/share/man/ca…
Mise à jour du cache d'index pour le chemin « /usr/share/man/ca/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/ca…
Recherche des fichiers préformatés sans sources sous /var/cache/man/ca…
Traitement des pages du manuel sous /usr/share/man/pt…
Mise à jour du cache d'index pour le chemin « /usr/share/man/pt/man8 ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/share/man/pt…
Recherche des fichiers préformatés sans sources sous /var/cache/man/pt…
Traitement des pages du manuel sous /usr/local/share/man…
Mise à jour du cache d'index pour le chemin « /usr/local/share/man/mann ». Attendez…terminé.
Recherche des fichiers préformatés sans sources sous /usr/local/share/man…
Recherche des fichiers préformatés sans sources sous /var/cache/man/local…
111 sous-répertoires de manuel contiennent de nouvelles pages.
5670 pages de manuel ont été ajoutées.
Une page sans source a été ajoutée.
Une entrée inutile de la base a été supprimée.
```