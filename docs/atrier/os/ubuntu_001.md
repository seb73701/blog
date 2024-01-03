---
title: Bloquer la mise à jour d'un paquet avec APT
description: 
published: true
date: 2023-06-11T10:43:38.376Z
tags: 
editor: markdown
dateCreated: 2023-06-11T10:43:38.376Z
---

# Bloquer la mise à jour d'un paquet avec APT

## Introduction
Cet article porte sur la mise en place d'un blocage de paquet avec le gestionnaire `apt`

## Déroulé
Dans notre exemple, nous allons bloquer la mise à jour du paquet `apache2` :

```
# apt-mark hold apache2
apache2 passé en figé (« hold »).
```

On peut ensuite retrouver les paquets bloqués à l'aide de la commande :

```
# apt-mark showhold
apache2
```

Nous pouvons le voir aussi avec `dpkg` :

```
# dpkg -l | grep apache2
hi  apache2
```

Le souhait est en `h` pour `hold` au lieu de `i` pour `installed`, le deuxième `i` est pour l'état qui lui est installé.


Pour débloquer le paquet, il suffit d'effectuer la commande :

```
# apt-mark unhold apache2
Annulation de l'état figé pour apache2.
```

source : https://fr-wiki.ikoula.com/fr/Bloquer_la_mise_%C3%A0_jour_d%27un_paquet_avec_APT