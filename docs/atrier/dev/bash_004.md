---
title: BASH
description: 
published: true
date: 2023-06-08T20:27:00.158Z
tags: bash
editor: markdown
dateCreated: 2023-06-08T20:27:00.158Z
---

# BASH

## INTRODUCTION

### DEFINITION DU SHELL
---

Le shell est un programme ayant pour fonction d'assurer l'interface entre l'utilisateur et le système Unix. C'est un **interpréteur de commandes**.

Plusieurs shells sont disponibles sur les plates-formes Unix.

### CARACTERISTIQUES D'UN INTERPRETEUR DE COMMANDES
---

Les interpréteurs de commandes disponibles en environnement Unix ont en commun les fonctionnalités suivantes :

- Ils proposent un **jeu de caractères spéciaux** permettant de déclencher des actions particulières.
- Ils possèdent **des commandes internes et des mots clés** parmi lesquesls certains sont utilisés pour faire de la programmation.
- Ils utilisent **des fichiers d'installation** permettant à un utilisateur de paramétrer son environnement de travail.

Chaque shell propose ses propres caractères spéciaux, commandes internes, mots clés et fichiers de paramétrage. Heureusement, les interpréteurs les plus utilisés actuellement dérivent tous du **shell Bourne** et ont, par conséquent, un certain nombre de fonctionnalités en commun.

### INTERPRETEURS DE COMMANDES (SHELLS)
---

- Le [**Bourne Shell**](https://fr.wikipedia.org/wiki/Bourne_shell) (`sh`) est un <u>shell ancien</u> qui <u>est de moins en moins utilisé</u> sur les plates-formes Unix.
  
- Le [**C-shell**](https://fr.wikipedia.org/wiki/C_shell) (`csh`) est <u>incompatible avec le Bourne</u> mais qui offre des fonctionnalités supplémentaires telles que l'historique des commandes,le contrôle de tâches, ainsi que la possibilité de créer des alias de commandes. Ces trois aspects seront repris plus tard dans le **Korn Shell**. Le **C-shell** est <u>peu utilisé</u> dans le monde Unix.

- Le [**Korn Shell**](https://fr.wikipedia.org/wiki/Korn_shell) (`ksh`) est une version enrichit du **Bourne Shell**, il <u>devient un standard</u>. Le **`ksh98`** _(version datant de 1998)_ est, avec le **Bourne Again Shell**, le <u>shell le plus utilisé actuellement</u>. Il a servi de base à la normalisation du shell ([IEEE POSIX 1003.2](https://ieeexplore.ieee.org/document/6880751)), représentée par le **shell POSIX** (similaire au **`ksh98`**).

- En 1993, une nouvelle version du [**Korn Shell**](https://fr.wikipedia.org/wiki/Korn_shell) voir le jour (**`ksh93`**). Celle-ci <u>présente une compatibilité arrière</u> avec le **`ksh98`**, à quelques exceptions près.

- [**Bourne Again Shell**](https://fr.wikipedia.org/wiki/Bourne-Again_shell) (`bash`). Il est conforme à la norme **POSIX** à laquelle il a ajouté quelques extentions. Ce shell est l'interpréteur fourni <u>en standard sur les systèmes Linux</u>. Il est également disponible en standard ou en téléchargement sur les systèmes Unix. La dernière version du **bash** porte le numéro **5.2**.


#### AVEC QUEL SHELL FAUT-IL PROGRAMMER ?
---

##### SCRIPTS DE DEMARRAGE

Sur certaines plates-formes, les scripts de démarrage sont interprétés par un **Bourne Shell**. Si l'on souhaite modifier ces scripts ou créer de nouveaux scripts de démarrage, il faut dans ce cas se restreindre à la syntaxe **Bourne Shell**. De manière plus générale, il faut utiliser la syntaxe qui correspond au shell qui interprète les scritps de démarrage.

##### AUTRES SCRIPTS

Dans les cas les plus fréquents (scripts de traitement exécutés en mode de fonctionnement Unix normal), le développeur choisira soit le `bash`, soit le `ksh` (88 ou 93), selon le shell à disposition sur sa plate-forme. Le `ksh` et le `bash` ayant de très nombreuses fonctionnalités en commun, cela ne pose donc aucun problème d'écrire un script compatible avec ces deux shells.

> **Remarque**
> 
> Si les spécificités du `ksh93` sont utilisées, le script ne sera pas compatible avec les shells `sh`, `ksh` (88) et `bash`.


#### NOM DES EXECUTABLES
---

Le nom des exécutables diffère selon le système Unix utilisé et sa version.

|**Bourne Shell**|
|:---------------|

|Système d'exploitation|Chemin|
|:---------------------|:-----|
|Solaris 10|`/sbin/sh`|
|Solaris 11|`/usr/sunos/bin/sh`|
|AIX 7.1|`/usr/bin/bsh`|

> **Remarque**
>
> Il a été supprimé sur HP-UX à partir de la version 11.


|**Korn Shell 88**|
|:----------------|

|Système d'exploitation|Chemin|
|:---------------------|:-----|
|Solaris 10|`/usr/bin/ksh`|
|Solaris 11|`/usr/sunos/bin/ksh`|
|AIX|`/usr/bin/ksh`|
|HP-UX|`/usr/bin/ksh`|


|**Korn Shell 93**|
|:----------------|

|Système d'exploitation|Chemin|
|:---------------------|:-----|
|Solaris 10|`/usr/bin/ksh93`|
|Solaris 11|`/usr/bin/ksh`|
|AIX|`/usr/bin/ksh93`|
|HP-UX|`/usr/bin/ksh`|


|**POSIX**|
|:----------------|

|Système d'exploitation|Chemin|
|:---------------------|:-----|
|HP-UX|`/usr/bin/sh`|


> **Remarque**
>
> Les pages de manuel sh, ksh, etc. renseignent sur la correspondance entre les shells et exécutables.

---

## MECANISMES ESSENTIELS DU SHELL
---

### COMMANDES INTERNES ET EXTERNES
---

#### LES COMMANDES EXTERNES
---

Une commande externe est un fichier localisé dans l'arborescence. Par exemple, lorsqu'un utilisateur lance la commande `ls`, le shell demande au noyau Unix de charger en mémoire me fichier `/usr/bin/ls`.

Sont considérés comme commandes externes les fichiers possédant l'un des formats suivants :

- Fichiers au format binaire exécutable
- Fichiers au format texte représentant un script de commandes (qui peut être écrit en Shell ou dans un autre langage tel que le Perl)

La commande `file` donne une indication sur le type de données contenues dans un fichier.

<u>Exemples</u>

La commande `ls` est un fichier au format binaire exécutable. Résultat de la commmande `file` :

```shell
$ file /usr/bin/ls
/usr/bin/ls: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=00eb8b042eb7e1e85dfd6820ed9c764e8f361d98, stripped
```

La commande `monscript` est un script shell. Résultat de la commande `file` :

```shell
$ file monscript
monscript: ASCII text
```

La commande `monscript.pl` est un script shell. Résultat de la commande `file` :

```shell
$ file monscript.pl
monscript.pl: ASCII text
```

Les commandes externes sont exécutées par l'intermédiaire d'un shell enfant.

- 1ère étape : Duplication du shell

