---
title: Audit de code source
description: 
published: true
date: 2023-04-21T10:39:39.282Z
tags: beta, sécurité, audit
editor: markdown
dateCreated: 2023-04-21T10:39:35.106Z
---

Si quatre paires d'yeux valent mieux que deux lorsqu'il s'agit d'examner un code source à la recherche de vulnérabilités potentielles, que penser alors d'une multitude d'yeux automatiques affûtés ? C'est encore mieux !

Les vulnérabilités applicatives sont aujourd'hui à l'origine de l'essentiel des incidents de sécurité (42% des intrusions externes selon une étude Forrester et 20% d'entre elles sont critiques contre seulement 2% des vulnérabilités réseau, selon le rapport EdgeScan 2018), et notamment sur le web. L'un des meilleurs exemples dans ce domaine est la fameuse injection SQL, où un attaquant est en mesure de s'emparer du contenu de la base de données d'une application web en exploitant un défaut de validation des entrées de l'utilisateur.

Ces vulnérabilités trouvent leur origine dans des erreurs de conception ou de programmation commises au moment du développement et découvertes ensuite par un attaquant un peu curieux.

Dans le cas des applications sur étagère, l'entreprise ne peut compter que sur le sérieux et le professionnalisme de l'éditeur pour espérer sinon éviter entièrement les vulnérabilités, du moins en réduire le nombre.

Mais en ce qui concerne celles développées en interne, elle peut agir ! A commencer, bien entendu, en instaurant des revues de code fréquentes, et en formant ses développeurs aux techniques de développement sécurisé. Sur ce point, Microsoft a notamment réalisé un excellent travail documentant abondamment son cycle de développement sécurisé (SDL, pour Security Development Lifecycle : https://www.microsoft.com/en-us/sdl/).

## Analyse automatisée du code source

Si de nombreux cabinets de conseil proposent aussi d'auditer les codes sources, il est également possible d'automatiser ce processus en interne. Le marché propose pour cela des outils logiciels capables d'explorer le code source (et ses dépendances) afin d'identifier des structures de code dangereuses, des oublis pouvant mener à uen faille de sécurité ou des mauvaises pratiques.

Ces outils sont livrés sous forme de solution logicielle à installer localement, mais aussi désormais en mode SaaS avec une facturation à l'usage. Certains sont disponibles sous license Libre, tandis que d'autres sont commerciaux.

Pour bien choisir, il sera nécessaire de s'assurer que le produit considéré supporte bien entendu les langages de développement utilisés en interne, mais aussi qu'il s'intègre à l'IDE choisi par l'équipe, ou puisse être utilisé dans le cadre d'un cycle de développement sécurisé.

Et puis il sera plus sage d'en utiliser plusieurs afin de s'assurer d'une meilleure couverture de l'analyse...