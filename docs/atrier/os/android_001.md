---
title: Installer Android sur son PC
description: 
published: true
date: 2023-04-21T11:13:33.896Z
tags: beta
editor: markdown
dateCreated: 2023-04-21T11:13:29.880Z
---

Vous n'avez pas de smartphone, de tablette, voire de PC hybride pour jouir pleinement du système d'exploitation Android, propriété de Google ? Alors pourquoi ne pas tout simplement l'installer sur votre machine ?

Android est un système d'exploitation qui peut être installé ou virtualisé sur tous les appareils munis d'un processeur Intel ou AMD. Cette technique est particulièrement intéressante pour tous ceux qui veulent transformer un PC hybride ou portable en véritable tablette, ou encore pour tous ceux qui veulent tester des applications ou un système avant même d'investir ses quelques derniers dans un matériel dernier cri. Ceux qui souhaitent adpater définitivement le système sur un support nomade pourront se lancer dans uen installation définitive sur une partition du système. Pour les autres, il sera possible d'utiliser le Live CD sans installation ou encore installer Android sur un disque virtuel. Voici la marche à suivre.

## Une ISO à télécharger facilement

Pas besoin d'aller chercher midi à quatorze heures pour installer Android sur sa machine : il existe un portail en ligne qui propose absolument toutes les versions d'Android, pour tous les types d'appareils. Vous pouvez profiter d'une installation sur votre ordinateur, mais vous pouvez également en profiter pour mettre à jour votre tablette vers un système un peu voir beaucoup plus performant. Pour commencer, rendez-vous sur le site : https://www.android-x86.org. Ensuite, vous pourrez accéder à la liste des téléchargements sous l'onglet "Download".

## Utiliser VirtualBox pour commencer

Le logiciel VirtualBox est un outil que vous devez obligatoirement avoir sur un PC. En effet, il permet d'installer très facilement différents systèmes d'exploitation pour profiter d'un autre univers ou pour tout simplement tester les performances d'un OS avant de l'adopter définitivement. Depuis VirtualBox, vous devrez dans un premier temps créer un nouveau disque virtuel. Ici, vous devrez depuis la page d'accueil, cliquer sur "Nouvelle" dans la barre des raccourcis. Ensuite, vous devrez nomer votre disque "Android", puis choisir "Linux" sous "Type", et "Other Linux" sous "Version". N'oubliez pas de bien sélectionner la version correspondant à l'architecture de votre PC. Laissez la taille recommandée par défaut, puis suivez les instructions sans modifier les paramètres proposés.

## Associez votre ISO

Ca y est ! Votre disque virtuel est prêt : vous pouvez désormais attaquer la partie la plus délicate. Oui, nous disons "Délicat" car lorsqu'on ne connaît pas la marche à suivre, il est difficile de lancer sa ROM. Nous vous proposons donc, depuis VirtualBox, de sélectionner le nouveau disque qui a été créé, puis de cliquer sur "Configuration" en haut de fenêtre. Rendez-vous directement dans l'onglet "Stockage", puis cliquez sur l'icône du disque vide dans la liste. Ensuite, vous devrez cliquer sur la petit icône du disque situé en haut à droite de la fenêtre, puis choisir "Choisissez un fichier de disque optique virtuel". Parcourez vos dossiers jusqu'à retrouver votre fichier ISO. Sélectionnez-le !

## Accéder à l'interface d'applications
Une fois que votre logiciel VirtualBox est correctement configuré, il sera très simple pour vous de lancer le système d'exploitation en cliquant sur "Démarrer". La, une nouvelle fenêtre s'ouvre avec le lancemenet de l'OS. Lorsque vous aurez accès à la page bleue de configuration, nous vous proposons dans un premier temps de sélectionner l'option "Run Android x86 without installation". Autrement dit, vous allez exécuter le Live CD et n'installerez rien sur votre disque dur. L'objectif sera ici de tester et découvrir l'interface. Vous devrez ensuite appuyer sur <kbd>Tab</kbd> ou <kbd>Entrée</kbd> pour lancer le système d'exploitation. Les fichiers ISO ne sont pas toujours très fiables : si votre système de démarre pas, pensez à tester un autre fichier ISO proposé sur le portail d'Android x86.

## Télécharger et tester des applications
Vous pouvez désormais découvrir le monde d'Android, et cela directement depuis votre PC ! Lorsque le système est lancé, vous allez devoir commencer par renseigner quelques informations propres à votre compte Google ou encore aux réglages de base de l'application. Avec votre souris, vous pourrez parcourir les différentes pages d'applications et utiliser les touches principales, à savoir : retour, accueil et gestionnaire de fenêtres. Pour reproduire un glissé du doigt, cliquez avec votre souris et faites glisser dans le sens de défilement. En cliquant sur la page d'applications, vous aurez accès à un large catalogue d'outils, de jeux ou encore de paramètrages. Testez absolument tout sans la moindre limite, Android est gratuit et à votre disposition !

## Faire fonctionner le wifi
Le wifi n'est pas toujours reconnu sur les versions pour VirtualBox, tout simplement parce que seules les clés Wifi en USB sont reconnues, voire la connexion Ethernet. Afin de configurer au mieux votre ISO et ainsi profiter d'une connexion Internet sur le système d'exploitation virtuel, vous drevrez vous rendre dans les paramètres de configuration du PC, puis accéder à l'onglet "Réseau" pour choisir les différentes cartes Wifi. Cette étape sera cruciale pour permettre à votre système d'accéder aux informations en ligne. D'ailleurs, certaines applications exigent un accès à Internet pour fonctionner.