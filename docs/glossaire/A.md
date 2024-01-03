---
title: Glossaire - A
description: 
published: true
date: 2023-04-21T10:36:36.373Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:34:59.028Z
---

## <span style="color: darkorange;">Abonnement</span>

Droit d'utiliser un réseau et/ou de bénéficier de services aux conditions définies dans le contrat, le plus souvent en contrepartie d'un montant fixe périodique. L'abonnement peut aussi être gratuit.  


## <span style="color: darkorange;">Accaparement de noms de domaine (Cybersquatting, Domain Name Grabbing)</span>
  
Action malveillante qui consiste à faire enregistrer un nom de domaine dans le seul but de bloquer toute attribution ultérieure de ce nom au profit de titulaires plus naturels ou légitimes.

Remarques : L’objectif est souvent d’obtenir un avantage financier en échange de la rétrocession du nom ainsi détourné. Cette pratique est particulièrement fréquente pour certains noms de domaine comme .net, .com, ou .org.

Références : Pour régler les litiges du .fr et du .re, se reporter à l’Association française pour le nommage internet en coopération, AFNIC, Les procédures alternatives de résolution de litiges (PARL) du .fr et du .re.


Voir aussi:
+ [Rançongiciel (Ransomware)](/glossaire/R)
+ [Faute de frappe opportuniste, coquille (Typosquatting)](/glossaire/F)
  
## <span style="color: darkorange;">Accès sans fil (Wi-Fi)</span>

Technologie de réseau informatique sans fil pouvant fonctionner pour construire un réseau interne accédant à Internet à haut débit. Cette technologie est basée sur la norme IEEE 802.11 (ISO/CEI 8802-11).

Remarques : L’accès sans fil rend nécessaire l’élaboration d’une politique de sécurité dans les entreprises et chez les particuliers, par le biais de la norme 802.11i (WPA2 ou Wi-Fi Protected Access) notamment.


## <span style="color: darkorange;">Access Control List (ACL)(ou Liste de contrôle d'accès)</span>
  
Ensemble des règles et instructions données à un système d'exploitation pour lui permettre de gérer les besoins d'accès des utilisateurs ou des groupes utilisateurs. Grâce à l'ACL, le système d'exploitation sait s'il doit accorder ou non la permission d'accès à un fichier ou à un dossier à tel ou tel utilisateur.

## <span style="color: darkorange;">Actif informatique (ou Asset)</span>
  
Ressource matérielle ou logicielle, physique ou virtuelle, détenue par une personne, un groupe de personnes ou une entreprise. L'ensemble des actifs informatiques constitue le "parc informatique" de l'organisation.


## <span style="color: darkorange;">Address munging</span>

Voir aussi:
+ [Moisson de courriels (Mail harvesting)](/glossaire/M)



## <span style="color: darkorange;">Address Spoofing (ou IP Spoofing)</span>

Action mailveillante destinée à utiliser délibéremment l'adresse d'un autre système en lieu et place de la sienne.


## <span style="color: darkorange;">Adresse IP</span>

Voir aussi:
+ [Protocole IP](/glossaire/I)

## <span style="color: darkorange;">Advanced Persistent Threat (APT)(ou menace avancée persistante)</span>

Attaque informatique menée par une personne ou un groupe de personnes qui passe inaperçue pendant une longue période de temps. Les APT ont généralement pour but de voler des données à très hautes valeurs stratégiques ou financières.


## <span style="color: darkorange;">ADSL (Asymetric Digital Subscriber Line ou Ligne d'abonné numérique à débit asymétrique)</span>

Technologie de transmission qui améliore les performances d'une ligne téléphonique traditionnelle. L'ADSL permet un accès haut débit à Internet. Son principe consiste à réserver une partie de la ligne au transport de la voix, une autre au transport des données circulant vers l'utilisateur et une troisième partie au transport de données émises par l'utilisateur. On parle d'asymétrie parce que la vitesse d'émission des données (débit) est inférieure à la vitesse de réception.

## <span style="color: darkorange;">Agence Nationale pour la Sécurité des Systèmes d'Information (ANSSI)</span>
  
Créée par décret en juillet 2009, l'Agence nationale de la sécurité des systèmes d'information est un service à compétence nationale rattaché au Secrétaire général de la défense et de la sécurité nationale (SGDSN). L'ANSSI a pour missions de proposer les règles à appliquer pour la protection des systèmes d'information de l'Etat et des Opérateurs d'importance vitale, et de vérifier l'application des mesures adoptées. Elle assure également un service de veille, de détection, d'alerte et de réaction aux attaques informatiques.


## <span style="color: darkorange;">Analyse Heuristique</span>

L’analyse heuristique constitue un ensemble de règles représentées sous forme d’expressions régulières. Elle permet de rechercher les mails dont les entêtes et les corps correspondent à des caractéristiques très particulières connues pour avoir une forte probabilité d’être un spam. 

_Exemples :_
```
/\b(dollard?s?|(e|€)uros?|pound|USD|sterling)\b/
```

Dans l'exemple ci-dessus, on recherche la présence de mots-clés seuls spécifiant une monnaie, les pluriels sont prévus, ainsi que l'utilisation du symbole `€` à la place du '`e`' de "`euro`".

_Applications :_
Dans le cas classique du "Viagra", il est beaucoup plus intéressant de détecter la présence du mot Viagra volontairement mal orthographié (par exemple: "`V|agra`"), plutôt que le terme réel qui peut très bien être utilisé dans une discussion courante entre pharmaciens.


## <span style="color: darkorange;">Anti-relais ou anti-relayage d'emails (Open-Relay)</span>

Le Relayage est une technique qui permet à des tiers de se servir de votre serveur de messagerie pour expédier des mails souvent illicites. Cette utilisation abusive de votre serveur génère un problème de disponibilité et de qualité de service, de plus elle engage la responsabilité de votre société dont l'identité a été usurpée.

_Exemples :_

Exemple de transaction SMTP se terminant par un rejet anti-relayage :

```
z03:~# telnet c03.altospam.com 25
220 c03.altospam.com ESMTP
helo toto.com
250 "HELO OK."
mail from: toto@toto.com
250 "MAIL FROM OK."
rcpt to: dsfsf@dsjfslkdjf.com
550 "Relais interdit !"
```

_Applications :_
Un test très simple (cf. ci-dessus) permet de contrôler si un serveur de messagerie (un MTA) est open-relay ou non. Certaines listes noires recensent automatiquement les serveurs open-relay.

Informations complémentaires :
- [Article Wikipédia sur les open-relays](http://en.wikipedia.org/wiki/Open_mail_relay)

## <span style="color: darkorange;">Architecture informatique</span>
  
Manière dont sont déployés et articulés les différents éléments d'un système d'information (logiciels, matériels, humains, informations), les uns par rapport aux autres. L'architecture désigne donc la structure organisationnelle d'un système, en fonction des décisions stratégiques prises au moment de la conception.

## <span style="color: darkorange;">ATM (Asynchronous Transfer Mode ou mode de transfert asynchrone)</span>
  
Protocole de transmission ultra-rapide d'informations utilisé par les opérateurs pour acheminer des données entre les commutateurs d'abonnés.

## <span style="color: darkorange;">Audit d'Architecture</span>
  
Etude de conformité d'un système d'information par rapport aux exigences de l'environnement métier, aux réglementations en vigueur et aux bonnes pratiques de la sécurité.

## <span style="color: darkorange;">Audit de configuration</span>
  
Etude de paramètrage des éléments d'une infrastructure par rapport aux exigences et de l'environnement métier, aux réglementations en vigueur et aux bonnes pratiques de la sécurité.

## <span style="color: darkorange;">Audit organisationnel</span>
  
Etude des vulnérabilités liées aux différents processus d'exploitation des systèmes d'information et au management de la sécurité au sein de l'entreprise. Cet audit peut couvrir les procédures d'administration, de surveillance et d'exploitation d'un système, la sécurité physique de l'entreprise et son plan de continuité d'activité.

## <span style="color: darkorange;">Authenticité</span>
  
Principe d'apposer une procédure d'authentification sur une ressource informatique et la capacité à prouver l'authenticité de l'information, de manière à ce que l'auteur ne puisse pas en répudier la paternité.


## <span style="color: darkorange;">Authentification / identification</span>

L’authentification a pour but de vérifier l’identité dont une entité se réclame. Généralement l’authentification est précédée d’une identification qui permet à cette entité de se faire reconnaître du système par un élément dont on l’a doté. En résumé, s’identifier c’est communiquer son identité, s’authentifier c’est apporter la preuve de son identité.


## <span style="color: darkorange;">Authentification multi-facteurs ou authentification forte</span>
  
Procédure d'identification combinant au moins deux méthodes d'authentification distinctes et successives parmi "ce que l'on sait" (mot de passe, identifiant, réponse à une question secrète...), "ce que l'on possède" (certificat, téléphone portable, clé USB...), "ce que l'on est" (reconnaissance biométrique) et "ce que l'on sait faire" (geste, signature...).

## <span style="color: darkorange;">Authentification renforcée</span>
  
Procédure d'identification de l'utilisateur plus forte que la saisie d'une information secrète (mot de passe, réponse à une question personnelle...). L'authentification peut être biométrique (empreinte digitale, reconnaissance faciale...) ou matérielle (clé USB, cryptocard...).

## <span style="color: darkorange;">Authentification simple</span>
  
Procédure d'identification composée d'une seule méthode d'authentification de l'utilisateur. Par exemple : code pin, mot de passe, etc.

## <span style="color: darkorange;">Authentification unique (ou Single Sign-On)</span>

Méthode qui permet à un utilisateur d'accéder à plusieurs applications informatiques sécurisées en ne procédant qu'à une seule authentification.

## <span style="color: darkorange;">Authorité de certification (ou certificate authority)</span>
  
Tiers de confiance indépendant en charge de la vérification de l'identité d'une personne ou d'une entité et habilité à déliverer un certificat électronique.