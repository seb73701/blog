---
title: Glossaire Apache
description: 
published: true
date: 2023-04-21T10:37:25.299Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:37:20.775Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[ <i class="fas fa-chevron-circle-left"></i> Accueil ](../../README.md)

# Glossaire "Apache"

## A
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Algorithme
> Une formule sans ambiguité ou un jeu de règles destinées à résoudre un problème en un nombre fini d'étapes. Les algorithmes de chiffrement sont en général appelés _Ciphers_.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Algorithme de chiffrement (Cipher)
> Un algorithme ou un système de chiffrement des données. Quelques exemples : DES, IDEA, RC4, etc.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") APR
> _Voir "[Bibliothèques pour la portabilité d'Apache](#biblioth%C3%A8ques-pour-la-portabilit%C3%A9-dapache-apache-portable-runtime-apr)"_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Archive Tar (Tarball)
> Un paquetage de fichiers rassemblés dans une archive à l'aide de l'utilitaire `tar`. Les distributions d'Apache sont stockées dans des Archives Tar compressées ou en utilisant `pkzip`.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Authentification
> L'identification formelle d'une entité du réseau comme un serveur, un client, ou un utilisateur.
> _Voir : [Authentification, Autorisation, et contrôle d'accès](http://httpd.apache.org/docs/2.4/howto/auth.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Autorité de Certification (Certification Authority) (CA)
> Un tiers de confiance habilité à signer des certificats pour des entités du réseau qu'il a authentifiées selon des critères basés sur la sécurité. Les autres entités du réseau peuvent alors utiliser la signature pour vérifier qu'une CA a authentifié le porteur du certificat.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_


## B
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Bibliothèques pour la portabilité d'Apache (Apache Portable Runtime) (APR)
> Un jeu de bibliothèques qui fournit la plupart des interfaces de base entre le serveur et le système d'exploitation. APR est développé parallèlement au serveur HTTP Apache comme projet indépendant.
> _Voir : [Apache Portable Runtime Project](http://apr.apache.org/)_

## C
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Certificat (Certificate)
> Un ensemble de données servant à authentifier des entités du réseau comme un serveur ou un client. Un certificat contient des ensembles d'informations X509 à propos de son propriétaire (appelé sujet/subject) et de l'[Autorité de Certification (Certification Authority) ou CA ](#autorit%C3%A9-de-certification-certification-authority-ca)signataire (appelée le fournisseur/issuer), ainsi que la [clé publique (public key)](#cl%C3%A9-publique-public-key) du propriétaire et la signature de la CA. Les entités du réseau vérifient ces signatures en utilisant les certificats des Autorités de Certification.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Chiffrement à Clé Publique (Public Key Cryptography)
> L'étude et l'application des systèmes de chiffrement asymétriques, qui utilisent une clé pour le chiffrement et une autre pour le déchiffrement. Les deux clés correspondantes constituent une paire de clés. Appelé aussi chiffrement asymétrique.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Clé Privée (Private Key)
> La clé secrète dans un système de [chiffrement à clé publique](#chiffrement-%C3%A0-cl%C3%A9-publique-public-key-cryptography), utilisée pour déchiffrer les messages entrants et signer les messages sortants.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Clé Publique (Public Key)
> La clé accessible au public dans un système de [chiffrement à clé publique](#chiffrement-%C3%A0-cl%C3%A9-publique-public-key-cryptography), utilisée pour chiffrer les messages destinés uniquement à son propriétaire et déchiffrer les signatures faites par son propriétaire.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") CONNECT
> Une [méthode](#m%C3%A9thode-method) HTTP pour encapsuler des données brutes dans HTTP. Elle peut aussi être utilisée pour encapsuler d'autres protocoles, comme le protocole SSL.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Contexte (Context)
> Une portion des [fichiers de configuration](#fichier-de-configuration-configuration-file) dans laquelle certains types de [directives](#directive) sont autorisés.
> _Voir : [Termes utilisés pour décrire les directives d'Apache](http://httpd.apache.org/docs/2.4/mod/directive-dict.html#Context)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Contrôle d'accès (Access Control)
> La restriction d'accès à des zones du réseau. Habituellement dans un contexte Apache, la restriction d'accès à certaines __URLs__.
> _Voir : [Authentification, Autorisation et Contrôle d'accès](http://httpd.apache.org/docs/2.4/howto/auth.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Couche des Points de connexion Sécurisés (Secure Sockets Layer) (SSL)
> Un protocole créé par Netscape Communications Corporation pour l'authentification et le chiffrement généraux des communications dans les réseaux TCP/IP. L'utilisation la plus connue est _HTTPS_, autrement dit le Protocole de Transfert Hypertexte (HTTP) au dessus de SSL.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Sous-requête
> Apache possède une API des sous-requêtes pour les modules qui permettent l'évaluation complète ou partielle par le serveur de chemins d'autres systèmes de fichiers ou d'URL. Par exemple, la directive [`DirectoryIndex`](http://httpd.apache.org/docs/2.4/mod/mod_dir.html#directoryindex), les modules [`mod_autoindex`](http://httpd.apache.org/docs/2.4/mod/mod_autoindex.html) et [`mod_include`](http://httpd.apache.org/docs/2.4/mod/mod_include.html) utilisent cette API.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Cryptographie Symétrique (Symmetric Cryptography)
> L'étude et l'application des _Algorithmes de chiffrement_ qui utilisent une clé secrète unique pour les opérations de chiffrement et de déchiffrement.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

## [<i class="fas fa-caret-square-up"></i>](#glossaire-apache) D
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Dégradé pour l'exportation (Export-Crippled)
> Diminué en terme de puissance cryptographique (et de sécurité) afin de respecter les Règles de l'Administration des Exportations des Etats-Unis (Export Administration Regulations ou EAR). Les logiciels de cryptographie dégradés pour l'exportation sont limités à une clé de petite taille, et produisent un Texte crypté qui peut en général être décrypté par force brute.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Demande de signature de certificat (Certificate Signing Request) (CSR)
> La soumission d'un [certificat](#certificat-certificate) non signé à une [Autorité de certification](#autorit%C3%A9-de-certification-certification-authority-ca), qui le signe avec la [Clé privée](#cl%C3%A9-priv%C3%A9e-private-key) de leur Certificat de CA. Une fois le CSR signé, il devient un vrai certificat.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Directive
> Une commande de configuration qui contrôle un ou plusieurs aspects du comportement d'Apache. Les directives sont placées dans le [fichier de configuration](#fichier-de-configuration-configuration-file)
> _Voir : [Index des directives](http://httpd.apache.org/docs/2.4/mod/directives.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Directive de configuration (Configuration Directive)
> _Voir : [Directive](#directive)_

## E
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") En-tête (Header)
> La partie de la requête et de la réponse [HTTP](#protocole-de-transfert-hypertexte-hypertext-transfer-protocol-http) qui est envoyée avant le contenu proprement dit, et contient des méta-informations décrivant le contenu.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Expression Rationnelle (Regular Expression) (Regex)
> Une méthode pour décrire un modèle sous forme de texte - par exemple, "tous les mots qui commencent par la lettre A" ou "tous les numéros de téléphone à 10 chiffres" ou encore "Toutes les phrases contenant 2 virgules, et aucun Q majuscule". Les expressions rationnelles sont très utiles dans Apache car elles vous permettent d'appliquer certains attributs à des ensembles de fichiers ou ressources avec une grande flexibilité - par exemple, tous les fichiers `.gif` et `.jpg` situés dans tout répertoire nommé "`images`", pourraient être enregistrés comme "`/images/.*(jpg|gif)$`". Lorsque l'on utilise des expressions rationnelles pour la substitution de chaînes, les variables spéciales `$1 ... $9` contiennent des références arrières vers les parties regroupées (entre parenthèses) de l'expression qui correspond. La variable spéciale `$0` contient une référence arrière vers l'ensemble de l'expression qui correspond. Pour insérer un caractère littéral "dollar" dans la chaîne de remplacement, il faut l'échapper avec un anti-slash. Pour des raisons historiques, la variable `&` peut être utilisée en tant qu'alias de `$0` dans certains cas, mais ceci n'est plus possible depuis la version 2.3.6. Apache utilise les Expressions Rationnelles Compatibles avec Perl fournies par la librairie [PCRE](http://www.pcre.org/). Vous trouverez plus d'information à propos de la syntaxe des expressions rationnelles PCRE sur ce site, ou dans le [Wikipedia de la PCRE](http://en.wikipedia.org/wiki/PCRE).

## F
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Fichier de configuration (Configuration File)
> Un fichier texte contenant des [Directives](#directive) qui contrôlent la configuration d'Apache.
> _Voir : [Fichiers de configuration](#fichier-de-configuration-configuration-file)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Filtre (Filter)
> Un traitement appliqué aux données envoyées ou reçues par le serveur. Les filtres en entrée traitent les données envoyées au serveur par le client, alors que les filtres en sortie traitent les documents sur le serveur avant qu'ils soient envoyés au client. Par exemple, le filtre en sortie INCLUDES traite les documents pour les [Server Side Includes (Inclusions côté Serveur)](#inclusions-c%C3%B4t%C3%A9-serveur-server-side-includes-ssi).
> _Voir : [Filtres](http://httpd.apache.org/docs/2.4/filter.html)_

## G
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Gestionnaire (Handler)
> Une représentation interne à Apache de l'action à entreprendre quand un fichier est appelé. En général, les fichiers ont des gestionnaires implicites, basés sur le type de fichier. Normalement, tous les fichiers sont directement servis par le serveur, mais certains types de fichiers sont "gérés" séparément. Par exemple, le gestionnaire `cgi-script` désigne les fichiers qui doivent être traités comme CGIs.
> _Voir : [Utilisation des gestionnaires d'Apache](http://httpd.apache.org/docs/2.4/handler.html)_

## H
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Hachage (Hash)
> Un algorithme mathématique à sens unique, irréversible, générant une chaîne de longueur fixe à partir d'une autre chaîne de longueur quelconque. Des chaînes différentes en entrée vont normalement produire des chaînes différentes en sortie (selon la fonction de hachage).

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Hébergement Virtuel (Virtual Hosting)
> Servir des sites web multiples en utilisant une seule instance d'Apache. Les Hôtes virtuels basés sur IP différencient les sites web en se basant sur leur adresse IP, alors que les Hôtes virtuels basés sur le nom utilisent uniquement le nom d'hôte et peuvent en conséquence héberger de nombreux sites avec la même adresse IP.
> _Voir la [Documentation des Hôtes Virtuels d'Apache](https://httpd.apache.org/docs/2.4/fr/vhosts/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") .htaccess
> Un [fichier de configuration](#fichier-de-configuration-configuration-file) placé à un certain niveau de l'arborescence du site web, et appliquant des [directives](#directive) de configuration au répertoire dans lequel il est placé, ainsi qu'à tous ses sous-répertoires. En dépit de son nom, ce fichier peut contenir pratiquement tout type de directive, et pas seulement des directives de contrôle d'accès.
> _Voir : [Fichiers de configuration](https://httpd.apache.org/docs/2.4/fr/configuring.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") httpd.conf
> Le [fichier de configuration](#fichier-de-configuration-configuration-file) principal d'Apache. Sa localisation par défaut est `/usr/local/apache2/conf/httpd.conf`, mais ceci peut être changé en utilisant des options de compilation ou d'exécution.
> _Voir : [Fichiers de configuration](https://httpd.apache.org/docs/2.4/fr/configuring.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") HTTPS
> Le Protocole de Transfert Hypertexte (Sécurisé), le mécanisme de communication cryptée standard sur le World Wide Web. Il s'agit en fait de HTTP au dessus de [SSL](#couche-des-points-de-connexion-s%C3%A9curis%C3%A9s-secure-sockets-layer-ssl).
> _Voir : [chiffrement SSL/TLS](https://httpd.apache.org/docs/2.4/fr/ssl/)_

## I
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Identificateur de Ressource Uniformisé (Uniform Resource Identifier) (URI)
> Une chaîne de caractères compacte servant à identifier une ressource abstraite ou physique. Elle est formellement définie par la [RFC 2396](http://www.ietf.org/rfc/rfc2396.txt). Les URIs utilisées sur le world-wide web sont souvent appelées [URLs](#localisation-de-ressource-uniformis%C3%A9e-uniform-resource-locator-url).

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Inclusions Côté Serveur (Server Side Includes) (SSI)
> Une technique permettant d'englober des directives de traitement dans des fichiers HTML.
> _Voir : [Introduction aux Inclusions Côté Serveur](https://httpd.apache.org/docs/2.4/fr/howto/ssi.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Indication du nom du serveur (SNI)
> Une fonctionnalité SSL permettant de spécifier le nom du serveur désiré dans le message initial de la négociation SSL, de façon à ce que le serveur web puisse choisir la bonne configuration de serveur virtuel à utiliser pendant le déroulement de la négociation SSL. Cette fonctionnalité a été ajoutée à SSL lorsque sont apparues les extensions TLS, RFC 3546.
> _Voir [la FAQ SSL](https://httpd.apache.org/docs/2.4/fr/ssl/ssl_faq.html) et la [RFC 3546](http://www.ietf.org/rfc/rfc3546.txt)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Interface commune avec les programmes externes (Common Gateway Interface) (CGI)
> La définition standard d'une interface entre un serveur web et un programme externe pour permettre à ce dernier de traiter des requêtes. Il existe une [RFC informationnelle](http://www.ietf.org/rfc/rfc3875) qui en couvre les spécificités.
> _Voir : [Contenu dynamique avec CGI](https://httpd.apache.org/docs/2.4/fr/howto/cgi.html)_


## L
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Localisation de Ressource Uniformisée (Uniform Resource Locator) (URL)
> Le nom/adresse d'une ressource sur l'Internet. Il s'agit du terme informel commun pour ce qui est formellement défini comme [Identificateur de Ressource Uniformisé](#identificateur-de-ressource-uniformis%C3%A9-uniform-resource-identifier-uri). Les URLs sont généralement construites selon un schéma, comme http ou https, un nom d'hôte, et un chemin. Une URL pour cette page pourrait être `http://httpd.apache.org/docs/2.4/glossary.html`.

## M
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Mandataire (Proxy)
> Un serveur intermédiaire qui se situe entre le client et le serveur d'origine. Il prend en compte les requêtes des clients, les transmet au serveur d'origine, puis renvoie la réponse du serveur d'origine au client. Si plusieurs clients demandent le même contenu, le mandataire peut l'extraire de son cache, plutôt que le demander au serveur d'origine à chaque fois, ce qui réduit le temps de réponse.
> _Voir : [mod_proxy](https://httpd.apache.org/docs/2.4/fr/mod/mod_proxy.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Mandataire inverse (Reverse Proxy)
> Un serveur [mandataire](#mandataire-proxy) qui est vu du client comme un serveur d'origine. Ceci peut s'avérer utile pour dissimuler le serveur d'origine réel au client pour des raisons de sécurité, ou pour répartir la charge.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Méthode (Method)
> Dans le contexte [HTTP](#protocole-de-transfert-hypertexte-hypertext-transfer-protocol-http), une action à effectuer sur une ressource spécifiée dans la ligne de requête par le client. Parmi les méthodes disponibles dans HTTP, on trouve `GET`, `POST`, et `PUT`.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Module
> Une partie indépendante d'un programme. De nombreuses fonctionnalités d'Apache sont fournies par des modules que vous pouvez choisir d'inclure ou d'exclure. Les modules qui sont compilés dans le binaire [`httpd`](https://httpd.apache.org/docs/2.4/fr/programs/httpd.html) sont appelés _modules statiques_, alors que les modules qui existent séparément et peuvent être chargés optionnellement à l'exécution sont appelés _modules dynamiques_ ou [DSOs](#objet-dynamique-partag%C3%A9-dynamic-shared-object-dso). Les modules qui sont inclus par défaut sont appelés modules de base. De nombreux modules disponibles pour Apache ne se trouvent pas dans l'[archive](#archive-tar-tarball) du Serveur HTTP Apache . Il sont appelés _modules tiers_.
> _Voir : [Index des modules](https://httpd.apache.org/docs/2.4/fr/mod/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Mot de Passe (Pass Phrase)
> Le mot ou la phrase qui protège les fichiers de clés privées. Il empêche les utilisateurs non autorisés de les déchiffrer. En général, il s'agit simplement de la clé secrète de chiffrement/déchiffrement utilisée pour les [Algorithmes de chiffrement](#algorithme-de-chiffrement-cipher).
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

## N
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Nom de domaine entièrement qualifié (Fully-Qualified Domain-Name) (FQDN)
> Le nom unique d'une entité du réseau, comprenant un nom d'hôte et un nom de domaine qui peuvent être résolus en une adresse IP. Par exemple, `www` est un nom d'hôte, `example.com` est un nom de domaine, et `www.example.com` est un nom de domaine entièrement qualifié.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Nombre Magique des Modules (Module Magic Number) (MMN)
> Le Nombre Magique des Modules est une constante définie dans le code source d'Apache et associée à la compatibilité binaire des modules. Sa valeur est modifiée quand des structures internes d'Apache, des appels de fonctions et d'autres parties significatives de l'API sont modifiées de telle façon que la compatibilité binaire ne peut plus être garantie. En cas de changement de MMN, tous les modules tiers doivent être au moins recompilés, et parfois même légèrement modifiés afin de pouvoir fonctionner avec la nouvelle version d'Apache.

## O
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Objet Dynamique Partagé (Dynamic Shared Object) (DSO)
> [Modules](#module) compilés en dehors du binaire Apache [`httpd`](https://httpd.apache.org/docs/2.4/fr/programs/httpd.html) et qui peuvent être chargés à la demande.
> _Voir : [Support des objets dynamiques partagés](https://httpd.apache.org/docs/2.4/fr/dso.html)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") OpenSSL
> L'ensemble d'outils Open Source pour SSL/TLS
> _Voir [OpenSSL](http://www.openssl.org/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Outil de gestion des extensions Apache (APache eXtension Tool) (apxs)
> Un script Perl qui aide à la compilation des sources de [modules](#module) sous forme d'Objets Dynamiques Partagés (Dynamic Shared Objects ou [DSOs](#objet-dynamique-partag%C3%A9-dynamic-shared-object-dso)) et facilite leur installation dans le serveur Web Apache.
> _Voir : Page de manuel : [`apxs`](https://httpd.apache.org/docs/2.4/fr/programs/apxs.html)_

## P
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Plein Texte (Plaintext)
> Le texte non chiffré.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Protocole de Transfert Hypertexte (HyperText Transfer Protocol) (HTTP)
> Le protocole de transmission standard utilisé sur le World Wide Web. Apache implémente la version 1.1 du protocole, référencée comme HTTP/1.1 et définie par la [RFC 2616](http://ietf.org/rfc/rfc2616.txt)

## R
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Résumé de message (Message Digest)
> Un hachage du message, qui peut être utilisé pour vérifier que son contenu n'a pas été altéré durant le transfert.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

## S
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Sécurité de la couche Transport (Transport Layer Security) (TLS)
> Le protocole successeur de SSL, créé par l'Internet Engineering Task Force (IETF) pour l'authentification et le chiffrement généraux des communications dans les réseaux TCP/IP. TLS version 1 est pratiquement identique à SSL version 3.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Session
> Les informations sur le contexte d'une communication en général.

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Signature numérique (Digital Signature)
> Un bloc de texte crypté qui valide un certificat ou un autre fichier. Une [Autorité de certification](#autorit%C3%A9-de-certification-certification-authority-ca) crée une signature en générant une empreinte de la _Clé publique_ fournie avec le _Certificat_; la CA chiffre ensuite l'empreinte avec sa propre _Clé privée_. Seule la clé publique de la CA peut décrypter la signature, ce qui permet de vérifier que la CA a bien authentifié l'entité du réseau qui possède le _Certificat_.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") SSLeay
> La bibliothèque originelle d'implémentation de SSL/TLS développée par Eric A. Young

## T
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Texte crypté (Ciphertext)
> Le résultat du passage d'un document [Plaintext (Plein texte)](#plein-texte-plaintext) par un [Cipher](#algorithme-de-chiffrement-cipher).
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_

#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Type MIME (MIME-type)
> Une méthode pour décrire le type de document transmis. Son nom vient du fait que son format est issu des Multipurpose Internet Mail Extensions (Extensions Multi-usages de la Messagerie par Internet) . Il comprend un type majeur et un type mineur, séparés par un slash (barre oblique). On trouve entre autres types `text/html`, `image/gif`, et `application/octet-stream`. Dans HTTP, le type MIME est transmis dans l' [en-tête](#en-t%C3%AAte-header) `Content-Type`.
> _Voir : [mod_mime](https://httpd.apache.org/docs/2.4/fr/mod/mod_mime.html)_


## V
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") Variable d'environnement (Environment Variable) (env-variable)
> Ce sont des variables nommées gérées par le shell du système d'exploitation, et servant au stockage d'informations et à la communication entre les programmes. Apache possède aussi des variables internes considérées comme variables d'environnement, mais stockées dans des structures internes à Apache, et non dans l'environnement du shell.
> _Voir : [Les variables d'environnement dans Apache](https://httpd.apache.org/docs/2.4/fr/env.html)_


## X
#### [<i class="fas fa-angle-up"></i>](#glossaire-apache "Haut") X.509
> Une norme de certificat d'authentification recommandée par l'International Telecommunication Union (ITU-T) et utilisée pour l'authentification SSL/TLS.
> _Voir : [chiffrement SSL/TLS](http://httpd.apache.org/docs/2.4/ssl/)_


---

_Source : https://httpd.apache.org/docs/2.4/fr/glossary.html_