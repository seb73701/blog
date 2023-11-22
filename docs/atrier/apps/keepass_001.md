---
title: tempo001
description: 
published: true
date: 2023-04-21T10:48:25.645Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:21.938Z
---

---
title: KeeRest : mettez du DevOps dans votre KeePass
description: 
published: true
date: 2023-02-03T10:43:34.216Z
tags: 
editor: markdown
dateCreated: 2022-12-30T17:19:47.166Z
---

---
title: KeeRest : mettez du DevOps dans votre KeePass
description: 
published: true
date: 2023-01-27T17:57:35.888Z
tags: 
editor: markdown
dateCreated: 2022-12-30T17:19:47.166Z
---

---
title: KeeRest : mettez du DevOps dans votre KeePass
description: 
published: true
date: 2022-12-30T17:45:15.049Z
tags: 
editor: markdown
dateCreated: 2022-12-30T17:19:47.166Z
---

# KeeRest : mettez du DevOps dans votre KeePass

> Nous avions vu dans MISC n°103 comment déployer une base KeePass en mode SaaS ciblant les particuliers ou de petits périmètres professionnels. Dans un autre monde, les pratiques DevOps se démocratisent et demandent d’augmenter l’agilité des développements tout en réduisant les délais de mise en production. Cet article est le fruit d’une collaboration entre un DevOps et un ingénieur SSI pour voir de quelle manière il est possible de tirer profit de KeePass dans ces environnements.

La gestion des mots de passe en entreprise par les services informatiques est une épine dans le pied de nombreux RSSI : stockage de fichiers en clair au fin fond d’un partage CIFS, utilisation inadéquate de solutions de chiffrement prévues pour le transport, fossilisation de pense-bêtes sur le tableau d’affichage qui apparaissent au JT [1]…

En 2020, il n’est plus possible de gérer les mots de passe à l’ancienne, car le nombre de serveurs au sein des entreprises a explosé depuis ces dix dernières années. Néanmoins le besoin d’un accès simplifié à ces secrets est réel. Les cycles de vie d’un système d’information moderne ne permettent plus à l’administrateur de passer par un coffre-fort physique (vu encore récemment chez un client) quand il doit accéder à un système. De plus, les stratégies actuelles encouragent toujours plus d’automatisation et donc de code, en particulier chez les DevOps, ce qui rend tous ces systèmes « manuels » incompatibles avec la plupart des environnements de taille conséquente.

Est-il possible de proposer une solution conciliant ces deux visions ? C’est ce que nous allons nous attacher à montrer dans cet article.

## 1. KeePass, DevOps, REST et sécurité ?
Il convient tout d’abord d’introduire ou de rappeler quelques termes et définitions qui permettent de mieux appréhender la suite de cet article.

### 1.1 KeePass
KeePass est un gestionnaire de mots de passe, permettant de stocker de manière numérique et sécurisée un ensemble de secrets protégés par un mot de passe maître. Ce logiciel a été déjà abordé de nombreuses fois aux Éditions Diamond (MISC n°103 [2], Linux Pratique n°101 [3] et n°105 [4]), et un peu partout sur Internet. Avec une certification de sécurité de premier niveau, de multiples audits de son code source et de nombreux plugins, le gestionnaire de mots de passe KeePass n’est plus à présenter.

### 1.2 DevOps
Le DevOps est une philosophie de travail, associée à un ensemble de pratiques, qui vise à fusionner développement applicatif (Dev) et infrastructure système (Ops), notamment par l’automatisation de leurs processus. Ce mouvement continue d’asseoir sa légitimité, poussant vers toujours plus d’industrialisation des actions système récurrentes dans des contextes d’intégration continue. S’appuyant sur les points communs entre les développeurs et les administrateurs système, la tendance DevOps semble avoir encore de beaux jours devant elle.

Néanmoins, les velléités des DevOps s’opposent parfois à la réalité de la sécurité informatique où tout système exposé finit inévitablement par se faire repérer, (pen)tester… et au final compromettre. Les développements effrénés et, disons parfois « anarchiques » de nos DevOps préférés n’échappent malheureusement pas à ce phénomène. Leurs processus de développement (dits « pipelines » pour les intimes) n’intégrant pas toujours lors de la conception des contrôles de sécurités automatisés.

### 1.3 REST
REST (pour REpresentational State Transfer) est un style d’architecture de service web devenu un standard de fait, dès lors qu’il s’agit d’interagir avec un système à distance. Le succès de ce protocole n’est pas un hasard : simple, interopérable, léger, facile à utiliser et à mettre en œuvre (on ne pouvait pas en dire autant de feu son cousin SOAP). De plus, l’utilisation de HTTP(S) comme protocole de transport, facilite les processus d’ouverture de flux par les équipes réseaux, d’audit, de maintien en condition opérationnelle et sur le plan de la sécurité.

On peut citer en exemple d’API REST celle proposée par haveibeenpwned.com et en particulier le module Password [6] qui permet de tester si un mot de passe est présent dans les dictionnaires « classiques » des attaquants, sans divulguer ni le mot de passe ni son hash [7].

### 1.4 SECaaS : Security as a Service
Dans ce contexte, ne pourrait-on pas envisager d’intégrer la sécurité comme un service (SECaaS) pour les pipelines DevOps ? L’expérience montre que certains services traditionnellement portés en interne par la sécurité ont une forte valeur ajoutée quand ils sont proposés à l’IT : SIEM, consolidation et contrôle de comptes, scan de vulnérabilités, etc. Il est même possible de les ajouter dans l’intégration continue en tant que tests de sécurité. Un service d’entreprise « on-premise » de gestion des mots de passe est donc tout à fait adapté à ces stratégies.

### 1.5 PowerShell
Devenu multiplateforme depuis la version 6, orienté objet, interactif, performant, accédant nativement au framework .NET, le langage de Microsoft a rapidement rencontré son public [8] tant auprès des administrateurs système que des administrateurs de sécurité (voir Empire [9]). Sur la plateforme de Microsoft, PowerShell est depuis ces dernières années l’outil de développement de prédilection des DevOps.

### 1.6 Pourquoi rendre KeePass accessible à distance ?
Comme on l’avait évoqué dans le n°103, les gestionnaires de mots de passe font désormais partie du paysage. Néanmoins, tous ne proposent pas une API pour accéder aux bases de mots de passe via du code. On trouve entre autres sur ce créneau :

- Password Manager Pro ;
- Team Password Manager ;
- PassBolt.

L’intérêt de gérer ses bases de mots de passe via du code est double : fiabiliser et sécuriser.

Un exemple assez parlant est celui de « l’infrastructure as code », ou IaC. En effet, avec des technologies telles que DSC (Desired State Configuration), l’accès à une base de mots de passe via une API REST permettrait de configurer une machine de A à Z, et ce, sans avoir à stocker son mot de passe administrateur local quelque part « en dur » dans un fichier de configuration.

De plus, grâce à un référentiel unique de stockage des mots de passe (la base de données) le gain de temps pour effectuer un audit de conformité des mots de passe est évident. Enfin, dernier point, et non des moindres, le travail en équipe s’en trouve facilité, car tout est centralisé en un point unique.

En résumé, dès que votre système atteint une taille critique permettant d’industrialiser et d’automatiser son fonctionnement, il est souhaitable d’engager une démarche DevOps.

## 2. Le module KeeRest : KeePass REST API

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-108/SchemaArchitectureKeeRest.png">

Fig. 1 : Schéma d’architecture de KeeRest.

À ce stade, nous allons nous attacher à démontrer la possibilité d’accéder à une base KeePass via une API REST et de sécuriser cet accès, le tout mis en œuvre grâce à PowerShell. Les prérequis sont les suivants :

un serveur Windows avec le logiciel KeePass [10] installé ;
une version de PowerShell (supérieure ou égale à la version 5.1) avec le module Polaris installé.

### Task 1 – Interaction avec une base KeePass
KeePass reposant sur le framework .NET, ses dll peuvent être chargées dans PowerShell grâce à la classe Reflection.Assembly, ce qui permet ensuite d’accéder aux objets .NET de KeePass très simplement. Voici le code pour charger ces dll en mémoire :

```
$path = 'C:\Program Files (x86)\KeePass Password Safe 2'
[Reflection.Assembly]::LoadFile("$path\KeePass.exe") | Out-Null
[Reflection.Assembly]::LoadFile("$path\KeePass.XmlSerializers.dll") | Out-Null
```

D’ici, il est possible d’interagir avec une base KeePass au format KDBX. Ci-dessous, un exemple de code qui liste toutes les entrées d’une base :

```
# Ouverture de la base KeePass$KDBX = New-Object KeePassLib.PwDatabase
$IoConnectionInfo = New-Object KeePassLib.Serialization.IOConnectionInfo
$IoConnectionInfo.Path = 'C:\MonfichierKeepassDB.kdbx'
$Key = New-Object KeePassLib.Keys.CompositeKey
$password = 'MonMotDePasseUltraComplexe'
$Key.AddUserKey((New-Object KeePassLib.Keys.KcpPassword($password)))
$KDBX.Open($IoConnectionInfo, $Key, $null)
# Export des entrées sous forme d’objets PowerShell personnalisés
$Entries = $KDBX.RootGroup.GetObjects($true,$true)$Entries | Foreach-Object {
    [PSCustomObject]@{
        Title    = $_.Strings.ReadSafe('Title')
        UserName = $_.Strings.ReadSafe('UserName')
        Password = $_.Strings.ReadSafe('Password')
        URL      = $_.Strings.ReadSafe('URL')
        Notes    = $_.Strings.ReadSafe('Notes')
        LastModificationTime = $_.LastModificationTime
    }
```

À ce stade, l’ensemble des fonctionnalités de KeePass est accessible avec PowerShell. Un exemple ludique [11] montre comment réaliser une attaque par force brute sur une base KeePass via PowerShell.

### Task 2 – Besoin d’une API REST pour générer des mots de passe ?
Tout comme Python avec les modules SimpleHTTPServer ou http.server (respectivement sur Python 2 et 3), PowerShell permet de configurer facilement un petit serveur web grâce à l’objet .NET HttpListener [12], souvent utilisé en test d’intrusion pour exfiltrer des données ou comme récepteur de reverse shell. La communauté a trouvé un tas d’usages plus pacifiques à ce module, et utilisé notamment comme base pour des frameworks d’API REST ou web, on peut citer par exemple : Pode [13], Universal Dashboard [14] ou encore Polaris [15] proposé récemment par Tyler James Leonhardt de l’équipe PowerShell chez Microsoft. C’est ce dernier qu’on utilisera dans cet article.

Voici un exemple de script PowerShell utilisant Polaris qui propose un générateur de mots de passe aléatoires accessible via une simple requête HTTP :

```
# Installation du module à partir de la PS Gallery
Install-Module Polaris -Scope AllUsers
 
# Création d’une route REST
New-PolarisRoute -Path '/randomPassword/:size?' `
                 -Method GET `
                 -ScriptBlock {
 
  [int]$size = $Request.Parameters.size
  $password = (1..$size | ForEach-Object {
                 [char](Get-Random -Minimum 33 -Maximum 127)}) -join ''
  
  # Envoi du mot de passe sous forme de hashtable que l’on exporte en JSON
  $Response.Send( (@{ password = $password } | ConvertTo-Json) )
}
 
Start-Polaris -Port 8080
```

Il est alors possible d’accéder à ce service via la ligne de commande ci-après :

```
Invoke-RestMethod -Uri http://localhost:8080/randomPassword/16
```

Facile, non ?

### Task 3 – Mélanger tout ça !
Maintenant que nous disposons de tous les ingrédients de notre recette, il ne nous reste plus qu’à combiner le tout et laisser la magie de PowerShell opérer.

Pour l’interaction avec KeePass, nous aurions pu faire appel au module PoshKeePass ; néanmoins celui-ci présente quelques lacunes, notamment l’absence de maintien de la connexion à la base, toujours en projet [16]. Il est par conséquent peu efficace sur des usages à grande échelle. Nous avons donc décidé de créer notre module baptisé KeeRest. Vous trouverez donc les fondations de celui-ci sur le GitHub du projet KeeRest [17] ainsi que sur la PowerShell Gallery [18].

Voici un exemple de code pour lire une entrée dans une base KeePass distante en filtrant sur le titre via une expression régulière :

```
# Côté serveur
Start-KeepassRestService -Port 80 -KDBXPassword 'MonMotDePasseUltraComplexe' -KDBXDatabase 'C:\MonfichierKeepassDB.kdbx'
 
# Côté client
Invoke-RestMethod -Uri http://<server.dns.tld>:80/title/<filtre>
```

> Dans le code du projet, le mot de passe attendu par l’option -KDBXPassword doit être protégé en utilisant la classe SecureString. Cette protection est ici volontairement éludée dans un souci de lisibilité. Les risques associés à sa présence en mémoire sont abordés dans l’analyse en fin d’article.
{.is-info}


### Task 4 – Sécuriser le service
Nous avons donc un code fonctionnel, mais pour l’instant notre service REST d’accès aux mots de passe est utilisable sans authentification ni chiffrement…

#### T4.1 Authentifier
Le module Polaris propose nativement le mode d’authentification « Negotiate » de Microsoft, qui est en fait une authentification Kerberos, avec NTLM en secours. Mais comment cela se passe dans les faits, une auth negotiate sur un service http ? Tout simplement :

1. Lors de sa première requête, le client se voit retourner un code « 401 Unauthorized » avec l’en-tête WWW-Authenticate: Negotiate.

```
Client TCP/53654 -> Serveur TCP/80 - Req
GET /whoami HTTP/1.1
Host: localhost:80
Serveur TCP/80-> Client TCP/53654 – 401 Unauthorized
HTTP/1.1 401 Unauthorized
Server: Microsoft-HTTPAPI/2.0
WWW-Authenticate: Negotiate
```

2. Le client doit alors refaire la même requête en ajoutant NTLMSSP_NEGOTIATE et en indiquant cette fois les protocoles qu’il supporte (KRB5, NTLMSSP, etc.).

```
Client TCP/53654 -> Serveur TCP/80 – Req NTLMSSP_NEGOTIATE
GET /whoami HTTP/1.1
Authorization: Negotiate YH0...G3ET        <NTLM_NEGOTIATE>
  MechType : NTLMSSP, MS KRB5, KRB5, SPNEGO
```

Le serveur lui répond alors le protocole choisi et démarre une authentification avec celui-ci.

#### T4.1.1 Rappels sur une authentification NTLM challenge-response

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-108/NTLM_Auth.png">

Fig. 2 : Authentification NTLM.

Dans le cas où le serveur estime qu’il doit utiliser NTLM, l’authentification se vulgarise très simplement ainsi [19] :

1. À l’ouverture de sa session, la machine du client a calculé et stocké le Hash NTLM de l’utilisateur.

2. L’utilisateur envoie une requête NTLMSSP_NEGOTIATE comme expliqué précédemment.

3. Le serveur génère alors un challenge de 16 octets, aléatoires, qu’il retourne au client.

```
Serveur TCP/80-> Client – Req NTLMSSP_CHALLENGE
HTTP/1.1 401 Unauthorized
Server: Microsoft-HTTPAPI/2.0
WWW-Authenticate: Negotiate oYhk...ju4gt    <NTLM_CHALLENGE>…
    SupportedMech : NTLMSSP
```

4. Le client chiffre ensuite ce challenge avec le hash NTLM de l’utilisateur et renvoie le résultat au serveur.

```
Client -> Serveur TCP/80 - Req NTLMSSP_AUTH
GET /whoami HTTP/1.1
Authorization: Negotiate oXcwda...tGy6        <NTLM_REPLY_TO_CHALLENGE>
```

À ce moment, le serveur envoie le login, le challenge et la réponse au contrôleur de domaine (DC) ou à son fournisseur de sécurité local si une authentification locale est possible.

5. Le DC recalcule la réponse au challenge (ce qui est possible puisqu’il conserve une copie de ce hash NTLM dans sa base). Si la réponse est identique à celle envoyée par le client, l’authentification est réussie.

6. Le serveur retourne la réponse au client :

```
Serveur TCP/80 -> Client - 200 OK
GET /whoami HTTP/1.1
User-agent: Mozilla/5.0[…]
WWW-Authenticate: oRsw...p7Vb
<contentu JSON de la réponse du serveur>
```

Comme beaucoup d’entre vous le savent déjà, NTLMSSP n’est plus vraiment un must-have sur le plan de la sécurité… Déjà vulnérable aux attaques pass-the-hash, il utilise des algorithmes de cryptographie obsolètes. Et comme ce protocole existe depuis un certain temps, vous trouverez toute la documentation nécessaire sur Internet pour creuser le sujet par vous-même [20][21].

#### T4.1.2 Rappels sur une authentification Kerberos

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-108/Kerberos_Auth.png">

Fig. 3 : Authentification Kerberos.

Tout comme NTLM, Kerberos fonctionne sur la base d’un secret partagé, et vous trouverez de la même manière toute la documentation nécessaire sur Internet [22]. On peut le vulgariser ici ainsi :

1. À l’ouverture de sa session, la machine du client utilise le mot de passe de l’utilisateur pour obtenir un TGT (Ticket Granting Ticket) auprès du Key Distribution Center (KDC), fonction portée par les DC dans les domaines Microsoft.

```
Client -> KDC TCP/88 - TGTReq
KDC -> Client – TGT
```

2. L’utilisateur envoie une requête NTLMSSP_NEGOTIATE comme expliqué précédemment.

3. Le client effectue alors une recherche DNS inverse pour l’IP du serveur et l’URL, puis il demande au KDC un TGS (Ticket Granting Service) pour ce Service Principal Name (SPN, issu de la recherche DNS), à l’aide de son TGT.

```
Client -> KDC TCP/88 – TGS Req for SPN using TGT
```

4. Si le TGT est valide, le client reçoit un TGS à fournir au serveur. Ce TGS est protégé par le secret partagé entre le KDC et le serveur, il contient les informations identifiant le client.

```
KDC -> Client – TGS for SPN
```

5. Le client transfère le TGS au service web associé avec une nouvelle requête.

```
Client -> Serveur TCP/80 - TGS for service
GET /whoami HTTP/1.1
Authorization: Negotiate oXcwda<KRB5Authtoken>
```

6. Le serveur est en mesure de déchiffrer le TGS grâce à son secret partagé avec le KDC et de valider si celui-ci a bien été émis par le KDC, et ce sans faire appel au contrôleur de domaine puisqu’il s’agit d’un secret partagé entre le KDC et lui.

7. Si le client a envoyé un TGS valide, la requête est servie et une réponse est fournie au client.

```
Serveur TCP/80 -> Client - 200 OK
GET /whoami HTTP/1.1
User-agent: Mozilla/5.0[…]
WWW-Authenticate: oRsw
<contentu JSON de la réponse du serveur>
```

> La mise en œuvre de Kerberos peut être délicate, dans la mesure où Kerberos s’appuie sur beaucoup de briques de base d’une infrastructure (DNS, NTP, AD, etc.). Néanmoins, la mise en œuvre d’une authentification Kerberos pour Polaris a été documentée par vos serviteurs directement dans le projet Polaris [23]. Pour la requête du client, il faut employer l’option -UseDefaultCredentials avec Invoke-RestMethod pour activer l’utilisation du SSO par le client sur un service authentifié.
{.is-info}


#### T4.2 Chiffrer : HTTPS
Les deux modes d’authentification ci-dessus sont simples à mettre en œuvre en démarrant Polaris avec l’option -Auth Negotiate. La mise en place de HTTPS est à peine plus complexe, une fois qu’un certificat et sa clé sont déployés sur le serveur, et nécessite la configuration du tripplet port/protocole/certificat avec PowerShell et netsh [24] :

```
Add-NetIPHttpsCertBinding -IpPort "<IP>:443" -CertificateHash "$Cert_Thumbprint" -ApplicationId "$AppID" -CertificateStoreName "MY"
```

Et la suppression de ce réglage se fait ensuite ainsi :

```
netsh http delete sslcert ipport=<IP>:443
```

Et si vous vous ne vous y retrouvez pas dans vos configurations HTTPS, la commande suivante vous permet de lister celles qui sont actives sur votre machine :

```
netsh http show sslcert
```

#### T4.3 Autoriser : Compartimentation des accès par groupe Active Directory
Une fois notre utilisateur correctement authentifié sur une connexion chiffrée, on peut commencer à restreindre nos accès, par exemple en autorisant uniquement les membres de certains groupes Active Directory à utiliser le service. Et à ce stade Polaris a déjà prémâché le travail en peuplant la variable $Request [25] avec les informations obtenues sur l’utilisateur lors de son authentification. Voici un exemple pour afficher l’activité d’un utilisateur authentifié :

```
Write-Verbose ("User " + $Request.User.Identity.Name + " logged on")
```

Les groupes se trouvent dans : $Request.User.Identity.Groups sous forme de SID, qu’il est possible de traiter ensuite dans les requêtes [26]. On peut aussi envisager des fonctionnements plus « dynamiques et modulaires » consistant à stocker dans les entrées KeePass, dans une propriété personnalisée par exemple, les identifiants des groupes AD ayant les droits de lecture et/ou d’écriture, ce qui permet au serveur d’autoriser à la volée, l’accès à certaines entrées, mais pas à d’autres.

## 3. Analyse de la solution
À ce stade, sans parler de l’implémentation, la preuve de concept proposée ci-dessus permet déjà une première analyse de sécurité. Notre service est authentifié via des mécanismes natifs Microsoft (NTLM) ou communautaires (Kerberos), et supporte les modes Basic et Digest permettant une base d’utilisateurs locale à l’application.

Tous ces mécanismes, loin d’être invulnérables, permettent néanmoins d’envisager un niveau de sécurité acceptable si combinés avec une analyse de risques cohérente de la solution : positionnement dans les réseaux, règles de filtrage, gestion des modes d’authentification autorisés dans les GPO, traçabilité applicative renforcée. La mise en œuvre d’un reverse proxy authentifiant devant le service peut également permettre de s’affranchir au niveau applicatif de ces questions d’authentification.

Il semble souhaitable que le code à mettre en production subisse un audit de sécurité dans la mesure où, une fois l’authentification passée, une faille au niveau applicatif qui permettrait à un attaquant de sortir du flux d’exécution donnerait très probablement un accès complet à la base KeePass associée au service. Le corollaire de ce point est que si l’on souhaite compartimenter les accès au service, la barrière de sécurité est au niveau de la base KeePass. Le mécanisme de compartimentation par groupe AD indiqué dans les entrées et évoqué dans la section précédente ne devrait être envisagé que comme une règle de sûreté, comme une anti-erreur par exemple.

Néanmoins, les bases KeePass permettant les accès simultanés nativement, il est tout à fait possible de multiplier les points d’accès REST à une même base pour différents profils réseau ou simplement de proposer un service en haute disponibilité en dupliquant le service sur deux points d’accès distincts, accédant à une même base partagée. En bonus, on pourrait d’ailleurs proposer un accès plus « user-friendly » à cette base partagée via une interface KeeWeb [27] auto-hébergée [28] par exemple.

### 3.1 Exemples de cas d’usages
Un premier cas d’usage rencontré en entreprise est celui de scripts accédant à des systèmes qui n’intègrent pas le SSO et demandent donc une authentification login/mot de passe traditionnelle. KeeRest permet d’accéder aux mots de passe stockés dans une base KeePass en authentifiant le script via les mécanismes de SSO.

On peut aussi envisager d’enregistrer dans la base des mots de passe générés aléatoirement à l’exécution de scripts, par exemple pour des comptes applicatifs partagés livrés à une unité directement dans leur base KeePass. Il est alors possible de gérer le renouvellement de ces mots de passe dans la base automatiquement.

## Conclusion, REST et PowerShell : la solution DevOps ultime ?
Dans cet article, nous avons vu comment développer en PowerShell un accès à une base KeePass au travers d’une API REST et sécuriser notre solution. En s’appuyant sur des briques de bases robustes et reconnues, il semble qu’il soit possible de trouver une entente entre les besoins DevOps et les exigences de la SSI. On gardera tout de même à l’esprit que cette solution ne cherche ni à adresser des besoins réglementaires ni légaux.

Les sources du POC proposé ici sont disponibles sur GitHub et ne demandent qu’à être utilisées et reprises par la communauté. La collaboration DevOps et Sécurité qui a permis cet article nous démontre une fois de plus l’intérêt d’intégrer la sécurité en amont et au cœur de processus IT de plus en plus agiles.

## Remerciements
Merci à Grégoire et Marion pour leurs relectures, ainsi qu’à l’ensemble de l’équipe du groupe SOC d’EDF qui ont rendu possible cet article !

## Références
[1] Les mots de passe de TV5Monde diffusés en plein 13 heures, Le Point : https://www.lepoint.fr/high-tech-internet/les-mots-de-passe-de-tv5monde-diffuses-en-plein-13-heures-10-04-2015-1920198_47.php
[2] KeePass multiplateforme en authentification forte avec une YubiKey, MISC n°103 : https://connect.ed-diamond.com/MISC/MISC-103/KeePass-multiplateforme-en-authentification-forte-avec-une-YubiKey
[3] Protégez votre vie privée avec KeePass et VeraCrypt, Linux Pratique n°101 : https://connect.ed-diamond.com/Linux-Pratique/LP-101/Protegez-votre-vie-privee-avec-KeePass-et-VeraCrypt
[4] Keepass2Android ou comment marier ergonomie et gestion sécurisée des mots de passe sous Android, Linux Pratique n°105 : https://connect.ed-diamond.com/Linux-Pratique/LP-105/Keepass2Android-ou-comment-marier-ergonomie-et-gestion-securisee-des-mots-de-passe-sous-Android
[5] xkcd n°1629 : Tools, Randall Munroe : https://xkcd.com/1629/
[6] haveibeenpwned.com, module passwords : https://haveibeenpwned.com/Passwords
[7] Est-ce que mon mot de passe a été piraté ?, geekeries.org : https://geekeries.org/2019/03/est-ce-que-mon-mot-de-passe-a-ete-pirate/
[8] The State of the Octoverse : https://octoverse.github.com/#fastest-growing-languages
[9] PowerShell Empire : http://www.powershellempire.com/
[10] Site officiel de KeePass : https://keepass.info/download.html
[11] KeePass + Password File + PowerShell = Brute Force Attack Goodness, hazzy.techanarchy.net : https://hazzy.techanarchy.net/security/keepass-password-file-powershell-brute-force-attack-goodness/
[12] Documentation httplistener, Microsoft : https://docs.microsoft.com/en-us/dotnet/api/system.net.httplistener
[13] PowerShell Pode : https://github.com/Badgerati/Pode
[14] Universal Dashboard : https://universaldashboard.io/
[15] PowerShell Polaris : https://github.com/PowerShell/Polaris
[16] Connection Management Projet, PoshKeePass : https://github.com/PSKeePass/PoShKeePass/projects/4
[17] KeeRest : https://github.com/apetitjean/KeeRest
[18] The central repository for sharing and acquiring PowerShell code including PowerShell modules, scripts, and DSC resources : https://www.powershellgallery.com
[19] Microsoft NTLM : https://docs.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm
[20] The NTLM Authentication Protocol and Security Support Provider : http://davenport.sourceforge.net/ntlm.html
[21] Secrets d’authentification sous Windows : https://www.sstic.org/media/SSTIC2007/SSTIC-actes/Secrets_d_authentification_sous_Windows/SSTIC2007-Article-Secrets_d_authentification_sous_Windows-bordes.pdf
[22] Secrets d’authentification épisode II Kerberos contre-attaque : https://www.sstic.org/media/SSTIC2014/SSTIC-actes/secrets_dauthentification_pisode_ii__kerberos_cont/SSTIC2014-Article-secrets_dauthentification_pisode_ii__kerberos_contre-attaque-bordes_2.pdf
[23] About_Auth Polaris, Pull request #20 : https://github.com/PowerShell/Polaris/pull/203
[24] Polaris HTTPS Example : https://jeremymcgee.com/powershell/polaris/https/2018/10/29/Polaris-HTTPS.html
[25] About $Request and $Response, documentation Polaris : https://powershell.github.io/Polaris/docs/about_RequestResponse.html
[26] About Authentication, documentation Polaris : https://github.com/PowerShell/Polaris/blob/master/docs/about_Authentication.md
[27] KeeWeb : https://keeweb.info/
[28] Selt-Hosting, KeeWeb : https://github.com/keeweb/keeweb#self-hosting

source : https://connect.ed-diamond.com/MISC/misc-108/keerest-mettez-du-devops-dans-votre-keepass
