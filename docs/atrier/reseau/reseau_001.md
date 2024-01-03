---
title: Le protocole HTTP (HyperText Transfer Protocol)
description: 
published: true
date: 2023-04-21T10:38:43.655Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:38:39.416Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[ <i class="fas fa-chevron-circle-left"></i> Réseau ](reseau.md)

# <i class="fas fa-network-wired"></i> Le protocole HTTP (HyperText Transfer Protocol)

![Apache HTTP Server](../../img/network/url-http.jpg)

## <i class="far fa-lightbulb"></i> Informations


**HTTP _(HyperText Transfer Protocol)_** est un protocole de communication conçu pour formuler des demandes à un serveur web et transférer (recevoir) le contenu des pages web. Il fonctionne usuellement grâce à `TCP/IP`, un procotole réseau de plus bas niveau. D'autres procotoles répdandus sur Internet sont **SMTP** et **POP3/IMAP** pour l'envoi et la réception des e-mails, **FTP** pour le transfert de fichiers, **DNS** pour la résolution des noms de domaines en adresses IP, etc.

Nous sommes tous habitués en tant qu'internautes à lire le fameux préfixe `http://` dans la barre d'adresse du navigateur, mails il est plus rare que nous nous intéressions à son fonctionnement. De nos jours, les questions de performance et d'optimisation à l'extrême le placent sur le devant de la scène.

HTTP se base essentiellement sur un dialogue texte, étant donné qu'à l'origine les pages web sont surtout constituées de code HTML.

Ce protocole est très récent et ne cesse d'évoluer en fonction des nouveaux besoin d'Interne.

Il permet ainsi d'accéder à des ressources distantes, de les présenter et de les mettre en forme à l'aide d'un navigateur Internet, en saisissant dans la barre d'adresse l'URL _(Uniform Resource Locator, ou localisateur uniforme de ressources)_.

Cette adresse est constituée des éléments suivants :

- le protocole utilisé, par exemple `http`, `https`, `ftp`, `svn`, etc.
- "`://`", le séparateur obligatoire à indiquer après le protocole.
- les informations d'authentification (facultatives) suivies de "`@`".
- l'adresse IP, le nom d'hôte ou le nom de domaine du serveur distant.
- "`:`" suivi du port d'écoute du serveur distant (facultatif, par défaut : **80** pour `http`, et **443** pour `https`).
- le nom de la ressource demandée, précédée du séparateur "`/`".

Exemples :
- `http://www.eni.labs` : le serveur distant présentera la ressource définie par défaut à la racine du site `www.eni.labs`.
- `http://user:password@www.eni.labs/extranet` : le serveur distant présentera la ressource "`/extranet`" avec une authentification ayant comme nom d'utilisateur "`user`" et comme mot de passe "`password`".
- `http://www.eni.labs:8080/mon_repertoire/ma_page.html` : le serveur distant écoute sur le port TCP "`8080`" et présentera la ressource "`ma_page.html`" située dans le répertoire "`mon_repertoire`".


## Historique

L'historique des versions du protocole est :

- 1989 : version **0.9**
- 1996 : version **1.0** publiée et décrite dans la _[RFC 1945](https://tools.ietf.org/html/rfc1945)_
- 1997 : version **1.1** _([RFC 2068](https://tools.ietf.org/html/rfc2068))_, qui devient le standard de l'IETF (_Internet Engineering Task Force_).
- 1999 : version **1.1** _([RFC 2616](https://tools.ietf.org/html/rfc2616))_, avec notamment l'ajout de la négociation de type de contenu.
- 2012 : annonce de la version **2.0** avec l'introduction du protocole **SPDY**, prononcé "**Speedy**" _(rapide en anglais)_, développé par Google dans le but d'augmenter les capacités du protocole.
- 2014 : en février, avec une revue des spécifications de la version **1.1** pour apporter des améliorations _([RFC 7230](https://tools.ietf.org/html/rfc7230), [RFC 7231](https://tools.ietf.org/html/rfc7231), [RFC 7232](https://tools.ietf.org/html/rfc7232), [RFC 7233](https://tools.ietf.org/html/rfc7233), [RFC 7234](https://tools.ietf.org/html/rfc7234), [RFC 7235](https://tools.ietf.org/html/rfc7235), [RFC 7236](https://tools.ietf.org/html/rfc7236) et [RFC 7237](https://tools.ietf.org/html/rfc7237))_

## Requêtes et en-têtes
