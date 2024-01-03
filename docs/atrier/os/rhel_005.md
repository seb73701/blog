---
title: Red Hat - Livre "Red Hat Enterprise Linux CentOS - Mise en production et administration de serveurs"
description: 
published: true
date: 2023-04-21T11:14:37.864Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:14:32.294Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[<i class="fas fa-chevron-circle-left"></i>  Red Hat Enterprise Linux (RHEL)](rhel.md)

----

# Red Hat - Livre "Red Hat Enterprise Linux CentOS - Mise en production et administration de serveurs"

----
## SHELL

Le shell (enveloppe, ou coquille) est l'interface de contrôle du système d'exploitation. Il s'agit d'une interface en mode texte. On parle alors de la **ligne de commande**, ou de **l'interface en ligne de commande** (en anglais : **CLI**, _Command-Line Interface_).

Comme son nom l'indique, le shell entoure le Kernel (noyau). Sans entrer dans le détail, le noyau est le coeur du système d'exploitation. Il est en relation directe avec les différents éléments matériels de la machine, qu'il contrôle. Le shell est un interpréteur de commandes : il interprète les ordres de l'utilisateur, qu'il transforme en langage compréhensible par le noyau. C'est ainsi grâce au shell, que l'utilisateur contrôle la machine. L'ensemble, associée à divers programmes, forme le système d'exploitation.

Il existe plusieurs variétés de shell :

+ `sh` : le **Bourne Shell**, un des premiers shells largement utilisé.
+ `bash` : **Bourne Again Shell**, le `bash` est compatible avec le `sh` et en étend les possibilités. C'est le shell le plus utilisé dans le monde Linux, celui qui est installé en standard.
+ `csh` : **C-Shell**, un shell utilisant une syntaxe proche du langage C.
+ `ksh` : **Korn Shell**, shell disponible sous Unix, Linux et Windows. Ce shell est compatible avec le **Bourne Shell** (`sh`) et inclut des fonctionnalités du **C-Shell** (`csh`).

Les avantages du shell en mode texte :
+ Les commandes en ligne sont complètes : en mode texte, il est possible d'ajouter un nombre impressionnant d'options, qui adaptent la commande à n'importe quel besoin. Elles sont renseignées dans des pages d'aide (`man`), bien fournies et formatées.
+ Les commandes en ligne sont universelles : pour expliquer comment réaliser une opération, il suffit de donner la commande à taper.
+ Les commandes en ligne sont combinables entre elles et automatisables.
+ Certaines opérations ne sont pas possibles qu'en ligne de commande.
+ A l'utilisation, les commandes en ligne s'avèrent bien souvent plus rapides.
+ La ligne de commande utilise un peu de ressources et peut être accédée à partir d'équipements peu puissants.

----
### Bash

Le bash est donc un interpréteur de commande. Il permet également de faire des scripts, c'est-à-dire de courts programmes contenus dans un fichier et lus par le shell, sans intervention de l'utilisateur.

Un shell bash peut être lancé de plusieurs manières, qui sont utilisées autant les unes que les autres :

+ **En mode console ou terminal** : on est **physiquement devant la machine**. Lorsque le système n'a pas d'interface graphique, il propose au démarrage une invite de connexion, demandant d'entrer un nom d'utilisateur est un mot de passe, après quoi le shell bash est lancé. Ce mode d'utilisation est appelé **tty** (_teletype_, ou téléscripteur).

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
Red Hat Enterprise Linux Server release 6.0 (Santiago)
Kernel 2.6.32-71.el6.i686 on an i686

login as : jdoe
jdoe@192.168.0.37's password:
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
Last login: Wed Dec  1 04:03:37 2010 from 192.168.0.2
[jdoe@cobb ~]$
```

> **Remarque**
> Il est possible de jongler entre six terminaux en mode texte, même si l'interface graphique est lancée. Tapez simultanément sur la touche [Ctrl] et les touches [F2] à [F6] pour aller sur les consoles 2 à 6. Tapez [Ctrl][F1] ou [F7] pour aller sur l'interface graphique.

+ **Dans une interface graphique** : on est déjà connecté avec notre nom d'utilisateur et notre mot de passe. Il suffit d'ouvrir alors un terminal, qui contiendra le shell bash dans une fenêtre.

> **Remarque**
> Le terminal graphique se trouve dans le menu **Applications**, sous-menu **Outils système**.

+ **En ligne de commande à distance** : en utilisant le protocole de communication sécurisé SSH (_Secure Shell_, Shell sécurisé), on peut ouvrir une interface en ligne de commande sur un ordinateur distant. On parle alors de terminal virtuel, ou **vty** (_virtual teletype_, ou téléscripteur virtuel).

Dans l'exemple suivant, on se trouve sur la machine `eames` (en utilisateur `root`), après quoi on se connecte en ssh à la machine `cobb` (en utilisateur `jdoe`) :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@eames ~]# ssh jdoe@cobb
jdoe@cobb's password:
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
Last login: Tue Dec  7 21:19:37 2010 from 192.168.0.1
[jdoe@cobb ~]$
```

----
#### Syntaxe

La ligne de commande s'articule comme une phrase, avec différents types de mots séparés par des espaces :

```shell
commande option(s) cible(s)
```

> **Remarque**
> Le shell est sensible à la casse. Sauf cas exceptionnel, la plupart des commandes sont en minuscules.

Le premier mot est **la commande**. On pourrait dire que c'est le "verbe" de notre ligne de commande. C'est le **programme qui va effectuer une certaine action**, par exemple, afficher le contenu d'un fichier.

**La cible, c'est l'objet sur lequel on va effectuer l'action**. Par exemple, un nom de fichier, dont on va afficher le contenu.

Par exemple, avec la commande `cat`, on affiche le contenu du fichier verlaine :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[jdoe@cobb ~]$ cat verlaine
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
Les sanglots longs
Des violons
De l'automne
Blessent mon coeur
D'une langueur
Monotone.
```

Une commande peut traiter une ou plusieurs cibles.

Par exemple, on affiche maintenant le contenu de deux fichiers :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[jdoe@cobb ~]$ cat verlaine apollinaire
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
Les sanglots longs
Des violons
De l'automne
Blessent mon coeur
D'une langueur
Monotone.
Sous le pont Mirabeau coule la Seine
Et nos amours
Faut-il qu'il m'en souvienne
La joie venait toujours après la peine
```

**Les options sont comme des adverbes : elles modifient la façon dont le programme opère**. On peut dire que le résultat est le même, mais le traitement est effectué d'une façon différente en fonction des options.

Une option est souvent un caractère, précédé d'un tiret.

Par exemple, l'option `-n` avec la commande `cat` affiche les numéros de lignes :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[jdoe@cobb ~]$ cat -n verlaine
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
1   Les sanglots longs
2   Des violons
3   De l'automne
4   Blessent mon coeur
5   D'une langueur
6   Monotone.
```

Ici, le résultat est le même que pour la première commande, mais présenté d'une manière différente.

----
#### Remarques concernant les options

+ Il est possible de mettre plusieurs options à la suite, en précédant l'ensemble des lettres d'un seul tiret. Par exemple, les options `-E` et `-n` peuvent également s'écrire `-En`.
+ Une option peut être un mot complet. Dans ce cas, elle est précédée de **deux** tirets. Par exemple, l'option `--version` donne la version d'un programme. Attention ! A ne pas confondre avec `-version` (avec un seul tiret). C'est en fait la concaténation des options `-v`, `-e`, `-r`, `-s`, `-i`, `-o`, `-n` (si elles existent).

Il existe des centaines de commandes, chacune pouvant posséder parfois des dizaines d'options, vous permettant d'effectuer n'importe quelle tâche.

----
#### Types de commandes

Il existe plusieurs types de commandes :

+ Les alias : ce soint des mots remplaçant des groupes de mots. Par exemple, `ll` remplace `ls -l -color=auto`.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[jdoe@cobb ~]$ alias
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias which='(alias; declare -f) | /usr/bin/which --tty-only --read-alias --read-functions --show-tilde --show-dot'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'

```

+ Les commandes internes : des commandes qui font en fait partie du shell, comme par exemple les commandes `cd`, `echo`, `kill`, `pwd`. Ce ne sont pas des programmes séparés. Lorsque l'on appel ce type de commandes, on fait appel en réalité au shell bash. Pour avoir une liste des commandes internes, on tape dans un shell `man bash` et on se positionne à la section `COMMANDES INTERNES`.
+ Les commandes externes : des programmes externes stockés quelque part sur le disque dur.

## Aide en ligne

Les commandes du shell sont nombreuses et compliquées. Elles disposent de nombreuses options qu'ik est difficile de mémoriser. C'est pourquoi la plupart des commandes disposent d'une aide en ligne, un "manuel". On entend souvent parler des _pages de manuel_, des pages _man_ ou des _manpages_.

Pour afficher la page de `man` d'une commande, il suffit de taper :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man commande
```
<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man ifconfig
```
Le résultat affiche un manuel organisé dans un format bien particulier. On trouvera en général au moins les sections suivantes :

+ **Nom** : les différents moyens d'appeler cette commande, ainsi qu'une brève description.
+ **Synoptis** : la syntaxe d'utilisation de la commande.
+ **Description** : une description détaillée de ce que fait la commande, avec souvent force détails sur son fonctionnement, son utilisation, etc.
+ **Options** : un descriptif des options disponibles pour cette commande.
+ **See also** (_voir aussi_) : d'autres commandes en rapport. Les chiffrers entre paranthèses désignent la catégorie de la commande.
+ D'autres sections existent en fonction de la page de manuel.

> **Remarque**
> Dans les synopsis de commandes que bien souvent les options et parfois les cibles sont **entre crochets** :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man ls
```
<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```
LS(1)                                                                                        User Commands                                                                                        LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...
```

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man mv
```
<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```
MV(1)                                                                                        User Commands                                                                                        MV(1)

NAME
       mv - move (rename) files

SYNOPSIS
       mv [OPTION]... [-T] SOURCE DEST
       mv [OPTION]... SOURCE... DIRECTORY
       mv [OPTION]... -t DIRECTORY SOURCE...
```

Cela signifie que les options ou les cibles sont **optionnelles**.

### Utiliser les pages de man

+ Pour se déplacer dans une page de manuel, il faut utiliser les touches [Flèche en haut] et [Flèche en bas], ou les touches [Page Up] et [Page Down].
+ Pour aller à la fin de la page en appuyant sur G (G majuscule, ou touche [Shift] + touche G).
+ Pour aller au début de la page de `man`, il faut appuyer sur "g".
+ Pour quitter la page de `man`, pressez la touche "Q".

### Sections de manpages

Les commandes sont organisées en plusieurs catégories numérotées, appelées sections :

1. Commandes utilisateur
2. Appels système
3. Fonctions de bibliothèque
4. Fichiers spéciaux
5. Format de fichier
6. Jeux
7. Divers
8. Administration système
9. Interface du noyau Linux

Il est possible d'avoir un descriptif de chaque section en tapant :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man 8 intro
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>


```
INTRO(8)                                                                               Linux Programmer's Manual                                                                               INTRO(8)

NAME
       intro - introduction to administration and privileged commands

DESCRIPTION
       Section 8 of the manual describes commands which either can be or are used only by the superuser, like system-administration commands, daemons, and hardware-related commands.

       As with the commands described in Section 1, the commands described in this section terminate with an exit status that indicates whether the command succeeded or failed.  See intro(1) for more
       information.

NOTES
   Authors and copyright conditions
       Look at the header of the manual page source for the author(s) and copyright conditions.  Note that these can be different from page to page!

COLOPHON
       This page is part of release 4.15 of the Linux man-pages project.  A description of the project, information about reporting bugs, and the  latest  version  of  this  page,  can  be  found  at
       https://www.kernel.org/doc/man-pages/.

Linux                                                                                          2007-10-23                                                                                      INTRO(8)
 Manual page intro(8) line 1/20 (END) (press h for help or q to quit)

```

Certaines commandes sont classées dans plusieurs catégories à la fois. Leur fonctionnement ne sera pas le même en fonction du contexte. Elles disposent alors de plusieurs pages de `man`, qu'on peut accéder en précisant le numéro de section.

Par exemple, `crontab` est autant un format de fichier (section 5) qu'une commande utilisateur (section 1). Il est possible de voir le manuel pour les deux sections :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man 1 crontab
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>


```
CRONTAB(1)                                                                                   User Commands                                                                                   CRONTAB(1)

NAME
       crontab - maintains crontab files for individual users

SYNOPSIS
       crontab [-u user] file
       crontab [-u user] [-l | -r | -e] [-i] [-s]
       crontab -n [ hostname ]
       crontab -c
       crontab -V

DESCRIPTION
       Crontab  is  the  program used to install a crontab table file, remove or list the existing tables used to serve the cron(8) daemon.  Each user can have their own crontab, and though these are
       files in /var/spool/, they are not intended to be edited directly.  For SELinux in MLS mode, you can define more crontabs for each range.  For more information, see selinux(8).

       In this version of Cron it is possible to use a network-mounted shared /var/spool/cron across a cluster of hosts and specify that only one of the hosts should run the crontab jobs in the  par‐
       ticular  directory  at  any  one time.  You may also use crontab(1) from any of these hosts to edit the same shared set of crontab files, and to set and query which host should run the crontab
       jobs.

       Running cron jobs can be allowed or disallowed for different users.  For this purpose, use the cron.allow and cron.deny files.  If the cron.allow file exists, a user must be listed in it to be
       allowed  to  use  cron  If the cron.allow file does not exist but the cron.deny file does exist, then a user must not be listed in the cron.deny file in order to use cron.  If neither of these
       files exists, only the super user is allowed to use cron.  Another way to restrict access to cron is to use PAM authentication in /etc/security/access.conf to set up users, which  are  allowed
       or disallowed to use crontab or modify system cron jobs in the /etc/cron.d/ directory.

       The temporary directory can be set in an environment variable.  If it is not set by the user, the /tmp directory is used.
...
```

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man 5 crontab
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>


```
CRONTAB(5)                                                                                    File Formats                                                                                   CRONTAB(5)

NAME
       crontab - files used to schedule the execution of programs

DESCRIPTION
       A crontab file contains instructions for the cron(8) daemon in the following simplified manner: "run this command at this time on this date".  Each user can define their own crontab.  Commands
       defined in any given crontab are executed under the user who owns that particular crontab.  Uucp and News usually have their own crontabs, eliminating the need for explicitly running su(1)  as
       part of a cron command.

       Blank  lines, leading spaces, and tabs are ignored.  Lines whose first non-white space character is a pound-sign (#) are comments, and are not processed.  Note that comments are not allowed on
       the same line as cron commands, since they are considered a part of the command.  Similarly, comments are not allowed on the same line as environment variable settings.

       An active line in a crontab is either an environment setting or a cron command.  An environment setting is of the form:

          name = value

       where the white spaces around the equal-sign (=) are optional, and any subsequent non-leading white spaces in value is a part of the value assigned to name.  The value string may be placed  in
       quotes (single or double, but matching) to preserve leading or trailing white spaces.

       Several environment variables are set up automatically by the cron(8) daemon.  SHELL is set to /bin/sh, and LOGNAME and HOME are set from the /etc/passwd line of the crontab´s owner.  HOME and
       SHELL can be overridden by settings in the crontab; LOGNAME can not.

       (Note: the LOGNAME variable is sometimes called USER on BSD systems and is also automatically set).

       In addition to LOGNAME, HOME, and SHELL, cron(8) looks at the MAILTO variable if a mail needs to be send as a result of running any commands in that particular crontab.  If MAILTO  is  defined
       (and non-empty), mail is sent to the specified address.  If MAILTO is defined but empty (MAILTO=""), no mail is sent.  Otherwise, mail is sent to the owner of the crontab.  This option is use‐
       ful if you decide to use /bin/mail instead of /usr/lib/sendmail as your mailer.  Note that /bin/mail does not provide aliasing and UUCP usually does not read its mail.  If MAILFROM is  defined
       (and non-empty), it is used as the envelope sender address, otherwise, ``root'' is used.

       By  default,  cron sends a mail using the 'Content-Type:' header of 'text/plain' with the 'charset=' parameter set to the 'charmap/codeset' of the locale in which crond(8) is started up, i.e.,
       either the default system locale, if no LC_* environment variables are set, or the locale specified by the LC_* environment variables (see locale(7)).  Different  character  encodings  can  be
       used for mailing cron job outputs by setting the CONTENT_TYPE and CONTENT_TRANSFER_ENCODING variables in a crontab to the correct values of the mail headers of those names.

       The  CRON_TZ  variable specifies the time zone specific for the cron table.  The user should enter a time according to the specified time zone into the table.  The time used for writing into a
       log file is taken from the local time zone, where the daemon is running.
...
```

> **Remarque**
> Pour rechercher une commande ave `man`, il faut utiliser l'option `-k` ou `--apropos`). Cette option vous permet de rechercher une chaîne de caractère dans les sections **apropos** des manpages, c'est-à-dire les noms et descriptions de la commande.

Par exemple, pour rechercher les commandes faisant du ftp :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
man -k ftp
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>


```
ftpusers (5)         - list of users that may not log in via the FTP daemon
pam_ftp (8)          - PAM module for anonymous access module
sftp (1)             - secure file transfer program
sftp-server (8)      - SFTP server subsystem
```

## Se déplacer dans l'arborescence (ou changer de répertoire)

La commande `cd` (_change directory_)

+ `cd` = aller dans le dossier "home" de l'utilisateur
+ `cd /` = aller à la racine
+ `cd /home/` = chemin absolu
+ `cd home/` = chemin relatif


Les répertoires spéciaux :

+ répertoire courant = `.` (point)
+ répertoire supérieur = `..`
+ répertoire utilisateur = `~` (tilde) ou `cd`
+ répertoire précédent = `-` (répertoire avant changement) 

## Dans quel répertoire suis-je ?

La commande `pwd`

## Afficher le contenu d'un répertoire

La commande `ls` (_list_)

Options de `ls` :
+ `-l` = liste détaillée
+ `-h` = (_human-readable_ - lisible pour un humain), vision des tailles de fichiers converties en `ko`, `Mo`, etc.
+ `a` = affiche les fichiers cachés (nom de fichier commençant par un `.`)
+ `F` = affiche les répertoires avec un `/` et les fichiers exécutables par un `*`
+ `t` = classe par date de modifications

## Créer un répertoire

La commande `mkdir`

Options de `mkdir` :

+ `-p` = parent (créé l'arborescence si elle n'existe pas)

## Supprimer un répertoire

La commande `rmdir` permet de supprimer un répertoire **vide**.


<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
rmdir /home/toto/titi/
```

Si le répertoire n'est pas vide, un message d'erreur indiquera qu'il n'est pas possible de le supprimé.

Dans ce cas, il faudra utiliser la commande `rm -/path/du/répertoire/ -fr`

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
rm -/home/toto/titi/ -fr
```

Les paramètres `-fr` positionné à la fin est une sécurité pour éviter une suppression accidentelle lors de la saisie.

Par exemple si on appuit sur la touche [Entrée] au moment de la saisie de la commande `rm -fr /`. Ce qui va supprimer toute l'arborescence du serveur et sans confirmation.

## Supprimer des fichiers

La commande `rm`

Options de `rm` :

+ `-f` (force) = demande pas de confirmation lors de la suppression
+ `-r` (resursif) = supprime un répertoire ainsi que tout ce qui se trouve dedans (sous-dossiers)


<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
# supprime un répertoire ainsi que tous les fichiers qui s'y trouvent et de façon récursive
rm -/home/toto/titi/ -fr

# suppression d'un fichier
rm -/home/toto/titi/file.txt -f
```

## Copier et déplacer des fichiers

La commande pour copier est `cp` (_copy_)

La commande pour déplacer est `mv` (_move_)

Copie du fichier `/etc/hosts` vers `/root` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
cp /etc/hosts /root
```

## Copier et déplacer des répertoires

Pour copier ou déplacer un répertoire, il faut utiliser l'option `-R` (récursif).

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
cp -R /home /root
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
[root@localhost ~]# cp -R /home /root

[root@localhost ~]# ll -R /home/
/home/:
total 0
drwx------. 2 casimir casimir 83 29 oct.  18:46 casimir

/home/casimir:
total 0

[root@localhost ~]# ll -R /root/
/root/:
total 4
-rw-------. 1 root root 1326 29 oct.  18:03 anaconda-ks.cfg
drwxr-xr-x  3 root root   21 15 nov.  10:45 home

/root/home:
total 0
drwx------ 2 root root 83 15 nov.  10:45 casimir

/root/home/casimir:
total 0
```

Le dossier `/etc/home` est copier ainsi que tout ce qui se trouve à l'intérieur vers le dossier `/root` ce qui donne `/root/home/*`. On peut voir que les droits et propriétaires ont changés pour affectés les droits de l'utilisateur actuel `root`.

Pour garder les droits et propriétaires, il faut ajouter l'argument `-p` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
cp -pR /home /root
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
[root@localhost ~]# ll -R /root/home/
/root/home/:
total 0
drwx------ 2 casimir casimir 83 29 oct.  18:46 casimir

/root/home/casimir:
total 0
```

## Déplacement d'un dossier

Lors d'un déplacement, les droits et les propriétaires restent les mêmes.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
mv /home/toto /root/
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
[root@localhost ~]# ll /root/
total 4
-rw-------. 1 root root 1326 29 oct.  18:03 anaconda-ks.cfg
drwxr-xr-x  3 root root   21 29 oct.  18:03 home
drwxr-xr-x  2 root root    6 15 nov.  10:53 toto
```

## Renommer un fichier ou un répertoire

On veut renommer le dossier `toto` qui se trouve dans `/home/` en `titi` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
[root@localhost ~]# ll /home/
total 0
drwx------. 2 casimir casimir 83 29 oct.  18:46 casimir
drwxr-xr-x  2 root    root     6 15 nov.  10:53 toto

mv /home/toto /home/titi
```

```shell
[root@localhost home]# ll /home/
total 0
drwx------. 2 casimir casimir 83 29 oct.  18:46 casimir
drwxr-xr-x  2 root    root     6 15 nov.  10:53 titi
```

## Renommer en copiant un fichier

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
cp /home/toto/bonjour /root/bonsoir
```

## Rechercher des fichiers

### Métacaractères

+ `*` (étoile) : correspond à n'importe quel caractère (voire aucun)
+ `?` : correspond à un caractère unique
+ `[abc]` : correspond aux caractères `a`,`b` ou `c`
+ `[!abc]` : correspond à n'importe quel caractère unique sauf `a`,`b` ou `c`
+ `[a-e]` : correspond à tous les caractères de `a` à `e`

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
ls *clients          # clients dommagesclients lefichierclients
ls *[1-9]*           # t5c
ls a?c               # abc agc ahc
ls a*c               # abc acourte agc ahc alongc
ls [a-c]bc           # abc bbc cbc 
```

### Recherche classique

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
find <chemin> "<motif>"
```

Options de `find` :

+ `<chemin>` : c'est le répertoire par lequel la recherche va commancer puis examiner tous les sous-répertories. Par défaut c'est le dossier courant.
+ `<motif>` : cherche les fichiers/répertoires correspondant au motif. Les motifs les plus courant sont :
 + `-name <texte>` : recherche par le nom. Attention à la casse. On préférra `-iname`. 
 + `-iname <texte>` : même chose que `-name` mais ignore la casse. 
 + `-type <lettre_de_type>` : recherche par type de fichiers indiqué par une lettre. Les types de fichiers sont affichés par exemple par la commande `ls -l` 
 + `-user <nom_utilisateur>` ou `-uid <uid_utilisateur>` : permet de rechercher les fichiers par leur utilisateur ou propriétaire.
 + `-group <nom_groupe>` ou `-gid <gid_groupe>` : permet de rechercher les fichiers par leur groupe.
 + `-size <taille>` : recherche par taille

Pour toutes les valeurs numériques, il est possible de faire précéder les chiffres de `+` pour **plus grand que** et de `-` pour **plus petit que**. A défaut, on recherche le chiffre exact.  

Exemple: recherchons à partir de `/home`, tous les fichiers contenant la suite de caractères **bon**, appartenant à l'utilisateur **jdoe**, dont la taille est plus petite que **5 ko**.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
find /home -type f -iname "bon"-user jdoe -size -5ko "<motif>"
```

### Recherche avec une base de données

La commande `locate` permet d'effectuer des recherches. Les informatiosn sont indexées tous les jours et de façon automatique.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
locate <texte>
```

Pour mettre à jour la base de données manuellement, il suffit d'exécuter la commande :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
updatedb
```

La commande cherchera tous les fichiers dont le nom contient `<texte>`. **Attention la recherche est sensible à la casse**.

Pour ignorer la casse, il faut ajouter l'argument `-i` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
locate -i <texte>
```

## Recherche de commandes

Les commandes que l'on tape se trouvent dans différents répertoires.

La liste de ces répertoires est contenu dans la variable d'environnement `$PATH`.

Pour voir le contenu, il suffit d'exécuter la commande suivante : 

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
echo $PATH
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

La commande `which` permet de savoir précisement où se trouve la commande, en recherchant dans la variable d'environnement $PATH : `which <command>`


<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
which ifconfig
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
/usr/sbin/ifconfig
```

## Afficher et éditer le contenu d'un fichier

La commande `file` va permettre de déterminer le type de fichier (texte, binaire, etc.) via cette commande : `file <fichier>`

Pour un programme :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
file /sbin/ifconfig
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
/sbin/ifconfig: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=96ce19fd01f2ed4f0a677a4b412ba28142d7ac3b, stripped, too many notes (256)
```

On peut voir `ELF 64-bit LSB shared object` qui indique que c'est un programme.

Pour une image :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
file /usr/share/cockpit/branding/rhel/bg-login.jpg
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
/usr/share/cockpit/branding/rhel/bg-login.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 89x89, segment length 16, progressive, precision 8, 1250x1200, frames 3
```

On peut voir `JPEG image data` qui indique que c'est une image.

Pour un fichier de configuration :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
file /etc/hosts
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```shell
/etc/hosts: ASCII text
```

On peut voir `ASCII text` qui indique que c'est un fichier de type texte.


### cat : afficher le contenu d'un fichier

La commande `cat` permet d'afficher le contenu d'un fichier.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
cat [options] <fichier>
```

La commande `cat` permet d'afficher d'un trait tout le contenu d'un fichier sans faire de pause.

Options de `cat` :

+ `-n` : numéroter les lignes
+ `-s` : supprimer les successions de lignes vides

`less` permet d'afficher le contenu d'un fichier page par page.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
less <fichier>
```

`tail` affiche la fin d'un fichier
La commande `tail` (_queue_) permet d'afficher les dernières lignes d'un fichier

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
tail [options] <fichier>
tail [-n <nombre_de_lignes>] <fichier>
```

Par défaut, ce sont les 10 dernières lignes qui apparaissent mais il est possible de modifier ce nombre avec l'argument `-n`. 
`tail` est très pratique pour des fichiers mis à jour régulièrement comme les fichiers de logs.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
tail -f <fichier>
```

`head` : affiche le début d'un fichier

Par défaut, ce sont les 10 premières lignes qui apparaissent mais il est possible de modifier ce nombre avec l'argument `-n`. 

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
head [options] <fichier>
head [-n <nombre_de_lignes>] <fichier>
```

## Editeur de fichiers vi

`vi` (_Visual Interface_) se prononce à l'anglaise "vi-aille".

Pour lancer `vi` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
vi [<fichier>]
```

`vi` à 2 modes  : **mode Commande** et **mode Insertion**.

Au démarrage on est dans le mode **Commande**

### Mode Insertion = édition de texte

Commentre entre en mode **Insertion** ?

+ `-i` : insérer des caractères à l'endroit du curseur
+ `-I` : insérer des caractères en début de ligne
+ `-o` : ajouter une ligne après la ligne en cours
+ `-O` : ajouter une ligne avant la ligne en cours
+ `-A` : ajouter à la fin de la ligne
+ `-a` : ajouter après le caractère en cours
+ `-r` : remplacer un seul caractère sous le curseur, après quoi on repasse en mode **Commande**.

### Mode Commande

Pour revenir dans le mode **Commande**, il suffit d'appuyer sur la touche [Echap].

#### Sauver / quitter

+ `:q` : puis touche [Entrée] pour sortir de `vi` sans sauvegarder 
+ `:q!` : puis touche [Entrée] pour forcer la sortie de `vi` sans sauvegarder. 
+ `:wq` : puis touche [Entrée] pour sortir en sauvegardant
+ `:w` : puis touche [Entrée] pour sauvegarder le fichier sans quitter
+ `:w nom_de_fichier` : puis touche [Entrée] pour sauvegarder dans le fichier appelé _nom_de_fichier_.

#### Déplacement / recherche

+ `:17` : déplace le curseur à la ligne 17
+ `:$` : déplace le curseur à la fin du fichier
+ `/` : rechercher une chaîne de caractères.
+ `n` : déplace le curseur à l'occurence suivante dans la recherche
+ `N` : déplace le curseur à l'occurence précédente dans la recherche

#### Suppression / copie

+ `x` : supprimer un caractère. Pressez `x` pour supprimer le caractère sous le curseur. 
+ `dd` : supprimer une ligne. Pressez `dd` pour supprimer la ligne en cours (les lignes sont copier en mémoire)
+ `dw` : supprimer un mot (_word_)
+ `yy` : copier (_yank_) la ligne courante
+ `yw` : copier le mot après le curseur 
+ `v` : entrer en visuel pour sélectionner du texte. Déplacez le curseur avec les flèches : le texte est sélectionné. Pressez `y` pour copier le texte sélectionner en mémoire.
+ `p` : coller (_paste_) le texte ou la ligne qui se trouve dans le tampon après la ligne courante.
+ `P` : coller le texte ou la ligne qui se trouve dans le tampon avant la ligne courante.
+ `u` : undo (annuler la dernière modification avec historique)
+ `[CTRL]+g` : connaître le numéro de la ligne courante.

Il est possible de faire précéder la plupart des commandes de suppression et de copie d'un chiffre, pour en multiplier l'effet. Par exemple, appuez sur `6dd` pour supprimer 6 lignes.

## Changement d'utilisateur

Il est possible de changer d'utilisateur en tapant : 

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
su - <utilisateur>
su - sebastien
```

Il est possible de devenir l'utilisateur `root` :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
su -
```

## Sortie du shell

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
exit
# ou
logout
```

## Enregistrement en CLI sur le Portail Client

L'enregistrement se fait en 2 étapes : Enregistrer et Attacher.

Enregistrer : déclarer le compte Red Hat à la machine

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
subscription-manager register
```

Attacher : utiliser un abonnement libre pour la machine

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
subscription-manager attach
```

Si l'enregistrement ne fonctionne pas, vérifier la connexion réseau est active. La machine doit également être à l'heure.

## Présentation de l'interface graphique

Deux interfaces principales se font aujourd'hui concurrence : **KDE** et **GNOME**. Red Hat a fait le choix de prendre **GNOME**.

L'écran est divisé en 3 parties :

+ **Le bureau**, c'est-à-dire le "fond d'écran" avec l'accès au répertoire personnel et à la corbeille.
+ **La barre de programme** en bas, avec les icônes de programmes ouverts.
+ **La barre de menus** en haut, pour le lancement des applications :
 + Menu **Applications** avec différents programmes réparties en catégories : Accessoires, Internet, Son et vidéo, et Outils systèmes.
 + Menu **Emplacements** permettant de lancer l'explorateur de fichiers locaux et sur le réseau, et un accés rapide à des emplacements courants.
 + A droite, l'état de certains services ainsi que la possibilité de fermer ou verrouiller la session, et éteindre la machine.