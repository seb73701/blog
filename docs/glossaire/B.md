---
title: Glossaire - B
description: 
published: true
date: 2023-04-21T10:37:08.136Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:35:04.579Z
---

## <span style="color: darkorange;">Backdoor</span>

Voir aussi:
+ [Porte dérobée (Backdoor)](/glossaire/P)


## <span style="color: darkorange;">Balayage de ports (Port scanning)</span>

Technique qui consiste à envoyer des paquets de données sur les différents ports d’une machine, puis à en déduire les états (la disponibilité) de ces ports en fonction de la réponse retournée, si elle existe. Remarques : Cette technique peut être utilisée pour découvrir les services fonctionnant sur la machine, en faisant une extrapolation entre les ports vus en l’état ouvert, et les services fonctionnant traditionnellement avec ce port. Les paquets de données peuvent être envoyés sur les ports linéairement (l’un à la suite de l’autre), ou bien de manière plus discrète, en modifiant les temps d’envoi des paquets, en les envoyant dans un ordre aléatoire ou à partir de plusieurs adresses IPs.


Voir aussi:
+ [Code malveillant, logiciel malveillant (Malicious software, malware)](/glossaire/C)
+ [Porte dérobée (Backdoor)](/glossaire/P)
+ [Test d’intrusion (Penetration Test)](/glossaire/T)


## <span style="color: darkorange;">Bas Débit</span>
Désigne une connexion Internet utilisant le réseau téléphonique commuté (RTC), avec un débit inférieur à 128 kbit/s.


## <span style="color: darkorange;">Bastion</span>
Système de très haut niveau de sécurité qui vise à protéger un réseau ou une partie d'un réseau des menaces extérieures.


## <span style="color: darkorange;">Behaviour monitoring</span>
Surveillance des communications entrantes et sortantes d'un réseau. Les interactions entre les "hôtes" et les "clients" sont systématiquement analysées pour vérifier que rien ne sort de l'ordinaire. Les outils d'analyse comportementale informatique permettent de détecter des comportements à risque de la part des utilisateurs légitimes du système d'information.


## <span style="color: darkorange;">BIT (contraction de Binary Digit ou chiffre binaire)</span>
Unité informatique qui permet de mesurer une quantité de données, par exemple, le kilobit (Kb) et le mégabit (Mb). 8 bits forment 1 octet.


## <span style="color: darkorange;">Blacklist (ou liste noire)</span>
Ensemble des noms d'individus ou d'entités (concrètes ou virtuelles) jugés indésirables ou hostiles par une personne, un groupe ou une entreprise.


## <span style="color: darkorange;">Blue Team</span>
Groupe de personnes internes à l'entreprise dont la principale mission est de dépister et de lutter contre les attaques informatiques venant de l'extérieur.


## <span style="color: darkorange;">Bluetooth</span>
Sous le nom Bluetooth se cache la norme 802.15.1 qui permet des communications radio à courte portée donc sans fil. Bluetooth ou « La dent bleue » provient du nom d’un ancien roi viking. Il existe plusieurs versions, dont les plus répandues sont actuellement v1.2 (débit théorique de 1 Mbit/s) et v2.0 (débit théorique de 3 Mbit/s).


Voir aussi:
+ [Wi-Fi (Wireless Fidelity)](/glossaire/W)


## <span style="color: darkorange;">Bombardement de courriels (Mail bombing)</span>

Envoi d’une grande quantité de courriels à un destinataire unique dans une intention malveillante.

Remarques : Forme particulière de déni de service contre les systèmes de courriers électroniques.


Voir aussi:
+ [Courriel (e-mail, mail)](/glossaire/C)
+ [Déni de service (Denial of Service, DoS)](/glossaire/D)
+ [Pourriel, polluriel (spam)](/glossaire/P)


## <span style="color: darkorange;">Bombe programmée, bombe logique (Logic bomb)</span>

Logiciel malveillant conçu pour causer des dommages à un système informatique et qui est déclenché lorsque certaines conditions sont réunies.

Remarques : Certains virus contiennent une fonction de bombe logique : déclenchement à date fixe, déclenchement quand une adresse réticulaire (URL) particulière est renseignée dans le navigateur, etc.


Voir aussi:
+ [Code malveillant, logiciel malveillant (Malicious software, malware)](/glossaire/C)

## <span style="color: darkorange;">Botnet</span>

Voir aussi:
+ [Réseaux de machines zombies (Botnet)](/glossaire/R)

## <span style="color: darkorange;">Boucle locale</span>
Partie du réseau téléphonique d'un opérateur comprise entre la prise murale de l'abonné et le commutateur, installé dans le central téléphonique de rattachement.

## <span style="color: darkorange;">Bounce</span>

Les Bounces sont les emails de non-délivrance émis par des serveurs de mails. Ce ne sont pas des spams, puisqu'ils sont théoriquement légitimes. Cependant étant donné que beaucoup de spammeurs usurpent l'identité d'individus légitimes, il arrive que certains Bounces vous parviennent alors que vous n'êtes pas à l'origine du mail initial. Cela peut polluer votre boîte aux lettres. D'autre part, en temps normal il y a peu de raisons de recevoir de Bounces quand on utilise le même serveur de messagerie en émission et en réception des emails, car en cas d'erreur de délivrance, le message d'information est distribué directement en local (sauf si le serveur du destinataire n'est pas correctement configuré).

Vidéo explicative : [Qu'est ce que le bounce ou ndr ?](https://www.youtube.com/watch?v=1zJw2ubr7hk)

_Exemples :_
Exemple de message de Bounce. Le sujet du mail de retour est: "*Undelivered Mail Returned to Sender*" et le contenu commence ainsi: "*This is the mail system at host m-frontend.domaine.com. I'm sorry to have to inform you that your message could not be delivered to one or more recipients. It's attached below (...).*".

_Applications :_
Reconnaître un bounce est très simple, car il est très souvent émis par postmaster, ou [null] et contient un sujet et un contenu facilement identifiable. Par contre, il est extrêmement complexe de savoir si ce bounce provient ou pas à l'origine d'un spam. 


## <span style="color: darkorange;">Box</span>
Modem multiservices permettant d'accéder à des offres Internet, téléphone et télévision.

## <span style="color: darkorange;">Broadcast</span>

Voir aussi:
+ [Diffusion générale (broadcast)](/glossaire/D)

## <span style="color: darkorange;">Buffer Overflow</span>
Technique d'exploitation d'une vulnérabilité dans le code d'un programme qui ne vérifie pas correctement la taille des données qu'il manipule.


## <span style="color: darkorange;">Bug (ou bogue)</span>
Défaut de conception d'un programme informatique entrainant le dysfonctionnement de tout ou partie d'un système.

## <span style="color: darkorange;">Bug bounty</span>

Méthode de détection des failles de sécurité d'un système récompensant la personne ou le groupe de personnes à l'origine de la découverte.
