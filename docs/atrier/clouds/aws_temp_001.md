---
title: Mise en place d’un socle FTP sécurisé dans le cloud
description: 
published: true
date: 2023-07-14T08:22:56.232Z
tags: 
editor: markdown
dateCreated: 2023-07-14T08:22:56.232Z
---

# Mise en place d’un socle FTP sécurisé dans le cloud

De nombreuses entreprises, que ce soient des PME, des industries, des hôpitaux ou des banques, ont besoin d’échanger des fichiers avec leurs partenaires ou même à l’intérieur de leur organisation. Nous avons tous entendu parler de ces cyber-attaques visant à perturber l’activité de ces entreprises ou à voler leurs données. Mettre en place une solution fiable et sécurisée pour le transfert de fichiers est, de nos jours, indispensable pour ces entreprises.

Même si le net regorge d’articles autour de ce sujet, nous vous proposons d’illustrer, dans cette série d’articles, la démarche que nous avons suivie chez l’un de nos clients, pour installer “SFTP” sur un cloud public tel qu’AWS, et de renforcer cette installation par la mise en place d’un antivirus comme MetaDefender.

Avant de commencer, présentons rapidement SFTP : SFTP (ou Secure Shell File Transfer Protocol) est un protocole de transfert et de gestion de fichiers, habituellement déployé sur le port SSH (22). Cet outil prend en charge à la fois, l’authentification (login/mot de passe, par échange de clé ssh), le chiffrement et le transfert des fichiers à travers le réseau.

A noter qu’il existe le “FTPS” (File Transfer Protocol Secure), qui peut être utilisé en mode “Connexion SSL/TLS” explicite ou “Connexion SSL/TLS implicite”. Le mode “implicite” dédie un port de communication spécifique (990 par défaut), pour l'authentification et l’établissement de la connexion SSL.

A noter, que son semblable non sécurisé "FTP" est absolument à proscrire car il constitue une cible privilégiée des hackers.

Le sujet sera découpé en une série de trois articles

- Déployer un SFTP managé sur le cloud AWS
- Sécuriser l’accès à AWS SFTP
- Sécuriser le stockage de fichiers

## PARTIE 1 - Déployer un SFTP managé sur le cloud AWS
AWS Transfer Family est le service managé du provider cloud AWS pour les protocoles SFTP, FTP et FTPS.

Ce service managé d’AWS offre la possibilité d'avoir un serveur SFTP, FTP et FTPS entièrement administré pour les transferts de fichiers. Le service est sorti fin 2018 sur toutes les régions AWS.

AWS Transfer Family fonctionne avec Amazon S3 ou Amazon EFS pour le stockage des fichiers.

L'intérêt de passer en service managé est de se décharger de l’entretien d’une infrastructure pour un serveur SFTP. AWS prend en charge les problématiques d’infrastructure, de scalabilité et de disponibilité (multi-AZ disponible).

### Mise en place de AWS SFTP (avec Amazon S3)
1. Créer un Bucket S3 ainsi qu’un rôle IAM qui autorise AWS Transfer à accéder au bucket S3.
1. Créer un serveur Transfer Family en choisissant le protocole SFTP et  le stockage de type S3
1. Choisir son ‘Identity Provider’ : Vous avez le choix entre le service managé (username & clés), soit un Active Directory (AWS Directory Service directory) ou un service personnalisé implémenté par vous.
1. Choisir l’endpoint du SFTP et son type : public ou privé
1. Configurer les logs avec Cloudwatch (création d’un log groupe et d’un rôle IAM)

Par le choix du type d’endpoint (public ou privé), on peut choisir d’implémenter un SFTP externe : exposé sur internet ou un SFTP interne : exposé au sein d’un VPC.

Dans notre cas, nous avons besoin d’un SFTP interne mais également exposé à des partenaires externes. Nous avons donc choisi un SFTP interne avec un endpoint VPC. Un endpoint VPC est une connexion, un point d’accès entre un cloud privé virtuel (VPC) et un service AWS.

Pour exposer le SFTP à des partenaires externes, nous avons mis en place un Load Balancer (Network Load Balancer) sur les subnets publics de notre VPC (‘internet facing’) qui pointe sur le serveur SFTP. De plus, dans notre cas, le SFTP avait pour contrainte d’être sur le port 4422, alors que le service SFTP d’AWS propose seulement le port 22. Le NLB permet alors d’exposer en extérieur le SFTP mais également d’installer une redirection de port du port 4422 vers le port 22. L’utilisation du Load Balancer (Network Load Balancer) est présentée par le schéma suivant :

![sftp-partie1-diagram1.png](/assets/img/clouds/tempo/sftp-partie1-diagram1.png)

L’accès au serveur AWS SFTP est sécurisé via un security group pour filtrer les IPs.

Mais attention, si on utilise un NLB avec du forward d’IP alors le security group ne fonctionnera plus car le ‘forward d’IPs’ ne fonctionne pas via les NLB sur AWS avec le service AWS Transfer Family.

Dans ce cas de figure, toute personne externe qui passerait via le NLB arriverait sur le SFTP avec une IP interne et privée à notre VPC sur AWS. Le security group ne pourrait donc plus exercer son travail de filtrage.

En 2021, AWS a prévu dans sa roadmap d’ajouter la possibilité d'avoir un autre port que le 22 sur un serveur SFTP sur AWS Transfer Family.

En attendant, nous avons mis en place le service AWS Network Firewall qui permet de mettre en place un firewall général sur tout un VPC et ses accès externes. Cette solution est expliquée dans la partie 2 de cette suite d'articles.

L’exposition du SFTP est alors :

|Exposition|Port|Accessibilité / Whitelisting|
|:---------|:---|:---------------------------|
|Interne à la VPC|22|Ouvert aux IPs de la VPC (à activer via le security group)|
|Externe via Internet|4422|Ouvert aux IPs autorisées via le Network Firewall|

Pour l’authentification, AWS Transfer propose seulement une authentification avec clés mais nous souhaitions aussi avoir une authentification possible avec login et mot de passe.

Pour customiser l’authentification avec nom d’utilisateur et mot de passe, nous avons suivi ce tuto :

https://aws.amazon.com/fr/blogs/storage/enable-password-authentication-for-aws-transfer-for-sftp-using-aws-secrets-manager/

Après installation de l’authentification nom d’utilisateur et mot de passe, notre architecture de SFTP est alors la suivante :

![sftp-partie1-diagram2.png](/assets/img/clouds/tempo/sftp-partie1-diagram2.png)

Après avoir déployé un serveur SFTP sur AWS dans cette première partie, nous allons voir dans une deuxième partie comment sécuriser l’accès à ce SFTP.

---

## PARTIE 2 - Sécuriser l’accès à AWS SFTP
Comme vu dans le 1er article ‘Déployer un SFTP managé sur le cloud AWS’, AWS Transfer Family ne permet pas de customiser le port exposé du SFTP.  Un système de NLB, Target Group & endpoints a été nécessaire pour pouvoir exposer un port autre que le 22.

En plus du username/password, nous voulions sécuriser l’accès avec du whitelisting d’IP.

Mais plusieurs problèmes surviennent pour sécuriser l’accès au SFTP en filtrant les IPs des partenaires.

- Avec une redirection d’IP via NLB, on peut choisir de conserver les IPs sources mais le service managé SFTP ne permet pas encore cette fonctionnalité en fonctionnant avec un NLB.
- D'autre part, on ne peut pas mettre de security groups sur un NLB.

Comment alors sécuriser le port 4422 en filtrant les IPs ?

→ En première idée, les NACL (Network Access List) semblent pertinents pour bloquer l’accès au port 4422 avec du filtre d’IP. Une NACL est une couche de sécurité qui agit comme un pare-feu (firewall) pour contrôler le trafic entrant et sortant d'un sous-réseau (subnet).

Mais à grande échelle cette solution n’est pas envisageable car une NACL est limitée à 20 règles par liste ACL réseau et peut être augmentée jusqu’à 40 au maximum pour un subnet sur AWS.

La solution choisie est alors le service AWS Network Firewall qui permet de faire du filtrage sur des ports sur tout un VPC.

### AWS Network Firewall
AWS Network Firewall est un service AWS qui permet de filtrer le trafic réseau au niveau d’un VPC. C’est un pare-feu réseau managé ainsi qu'un service de détection et de prévention des intrusions sur le VPC.

AWS Network Firewall permet de mettre en place du filtrage sur le trafic entrant et venant d'une passerelle Internet (Internet gateway), d'une passerelle NAT (NAT Gateway) ou encore via VPN ou AWS Direct Connect. Le pare-feu réseau utilise le système de prévention des intrusions (IPS) open-source, Suricata.

Il y a 3 types de ressources AWS pour AWS Network Firewall:

- un Firewall = qui définit les FirewallPolicies, les subnets ainsi que le VPC
- des FirewallPolicies = qui définissent des règles (RuleGroup) et autres paramètres pour le filtrage du trafic d’un VPC.
- des RuleGroup = qui sont un ensemble de règles pour le filtrage du réseau d’un VPC et les actions correspondantes. AWS Network Firewall utilise deux types de groupes de règles: sans état ou avec état (stateless et stateful).

AWS Network Firewall protège les sous-réseaux publics (public subnets) au sein d’un VPC en filtrant le trafic passant entre les sous-réseaux publics (public subnets) et l'extérieur du VPC.

L'exemple ci-dessous présente l’architecture d'un Firewall avec 2 subnets publics:

- Une Internet Gateway Route Table qui dirige le trafic destiné au subnet public du VPC (10.0.2.0/24 & 10.0.3.0/24) vers le Firewall subnet (vpce-xxx) de la même Zone (AZ).
- Deux Firewall subnets & Route Table qui dirigent le trafic destiné pour n’importe où dans la VPC (10.0.0.0/16) vers ‘local address’ et dirigent le trafic destiné à n’importe où (0.0.0.0/0) vers l’Internet Gateway (igw).
- Un Firewall Endpoint (vpce-xxx) par Firewall subnet (par AZ).
- Les Subnet public du VPC & leur Route Table qui dirigent le trafic destiné pour n’importe où dans la VPC (10.0.0.0/16) vers ‘local address’ et dirigent le trafic destiné à n’importe où (0.0.0.0/0) vers les Firewall subnets de la même Zone (AZ) .


![sftp-partie2-diagram.png](/assets/img/clouds/tempo/sftp-partie2-diagram.png)

Dans notre cas, nous avons créé un Firewall sur notre VPC pour 3 subnets publics (chacun dans une AZ différente) avec le routage nécessaire. Le Network Firewall avait donc une ‘RuleGroup’ nommée ‘sftp’ avec toutes les IPs en whitelist comme rule ‘pass’ sur le port 4422 et par défaut le port 4422 bloqué.

Un paramètre à prendre en compte dans cette solution est le coût. En effet, le coût d’un Firewall peut vite être élevé.

Si on a plusieurs zones, alors il faut un Network Firewall Endpoint par zone au sein de notre région AWS. Ci-dessous, voici les prix pour un Network Firewall Endpoint et pour le trafic passant par le Firewall sur AWS:

|Name|Price|
|:---|:----|
|Network Firewall Endpoint|$0.395/hr|
|Network Firewall Traffic Processing|$0.065/GB|



Mais ce service complet permet d’avoir une gestion d’un Firewall sur tout un VPC.

Par ailleurs, le déploiement de cette architecture est possible en Infra As Code. Nous avons déployé via l’outil CDK d’AWS mais Terraform propose aussi les ressources nécessaires pour déployer AWS Transfer Family.


Après avoir sécurisé l’accès: le port, le login vers notre SFTP managé avec stockage sur S3. Nous allons voir dans une troisième partie comment sécuriser le contenu des fichiers déposés sur le SFTP.

---

## PARTIE 3 - Sécuriser le stockage de fichier
Rappelez-vous, nous avons vu dans le 2eme article comment sécuriser les accès à notre serveur SFTP, mais qu’en est-il de la sécurisation des dépôts de fichiers ?

### Qu’est-ce qu'un antivirus d’analyse de fichier ?
#### Qu’est-ce qu’un malware ?
Un malware est un terme utilisé pour décrire du code malveillant, dont le but est de nuire à notre système informatique et plus largement à notre système d’information. Il regroupe ainsi l’ensemble des programmes et logiciels malveillants pouvant perturber le bon fonctionnement du matériel informatique.

Dans cette large définition se retrouvent tous les dérivés du malware, comme le virus qui exécute une fonction non désirée et nuisible sur un appareil, un ver, prévu pour s’auto répliquer sur le système et se diffuser à travers le réseau, le cheval de Troie, qui se fait passer pour un logiciel légitime et bien d’autres encore.

Ces malwares s’infiltrent de différentes manières, par le biais de téléchargement, de clé usb, de passage par mail, de visite de site frauduleux etc. Il peut s’appeler de différentes manières “programme", "logiciel" ou "fichier malveillant”.

C’est là qu'interviennent les antivirus chargés de détecter des malwares à mettre hors d’état de nuire. Ceux-là sont habituellement installés sur les machines informatiques directement, mais la détection peut se faire en amont via des instances dont leur seule fonction est de vérifier le contenu d’un fichier avant de continuer sa transition jusqu’à destination.

#### Qu’est-ce qu’un antivirus d’analyse de fichier ?
Dans le jargon de la sécurité, on peut appeler ça un scanner de fichier. Celui-ci via analyse, permet de s’assurer qu’un fichier ne contient pas de malware. Cependant, on demande de plus en plus à ce qu’un antivirus soit polyvalent, c’est pourquoi beaucoup de solutions d’antivirus offrent aujourd’hui des fonctionnalités plus avancées, comme celle d’empêcher les fuites de données en vérifiant qu’il n’y a pas d’informations jugées comme confidentielles ou sensibles à l’intérieur d’un fichier.

Aussi, afin de renforcer les performances en termes d’analyse, le scan d’un antivirus ne suffit pas, c’est alors que le multi-scan, également appelé multi-analyse, fait son entrée.

C'est une technologie avancée de détection et de prévention des menaces qui augmente les taux de détection, réduit son temps d’analyse et offre une résilience aux problèmes des fournisseurs de logiciels anti-malware.

OPSWAT a été le pionnier du concept de fichiers à numérisation multiple avec plus de 30 moteurs anti-malware disponibles pour offrir une protection améliorée contre toute une série de cybermenaces.

## METADEFENDER CORE V4
### Présentation de la solution
Opswat, éditeur de l’application Metadefender Core, propose plusieurs solutions de cybersécurité. Celui qui nous intéresse est Metadefender Core V4, leur solution d’analyse de fichier.

L’analyse de fichier se fait à la demande et de manière asynchrone, c'est-à-dire qu’il faut aller chercher le résultat. Nous noterons tout de même que l’utilisation de webhook pour rendre le fonctionnement synchrone est tout à fait envisageable et pris en charge.

### Possibilités de mise en place
Il y a 4 modèles d’utilisation de la solution Metadefender Core :
- **Metadefender Core** : imaginé pour le “On Premise"
- **Metadefender Core AMI** : attendu dans le “Cloud” en IaaS
- **Metadefender Core Cloud** : prévu pour le “Cloud” mais en SaaS
- **Metadefender Core Container** : utilisable en “On Premise" mais également dans le “Cloud”

Ici nous allons présenter l’utilisation de la solution Metadefender Core. Ce modèle-là permet notamment de pouvoir choisir notre image de base (donc soit une AMI AWS, soit une AMI signée de votre main avec du hardening et/ou la pré installation d’agent etc) sur laquelle la solution sera installée.

Il y a un combo de 3 composants pour la gestion du parc Metadefender Core (MD).

**MD Core Server** : il fait office de contrôleur mais analyse aussi des fichiers (il fait donc office de node aussi). C’est sur le serveur que l’on va définir nos préférences de scan, gérer la licence, ajouter/activer/désactiver des fonctionnalités, gérer les utilisateurs, superviser,  en somme tout ce qui se trouve être de l’administration. A lui tout seul, vous pouvez déjà commencer à utiliser la solution.

**MD Core Node** : Son seul rôle est de scanner des fichiers, il ne fait rien d’autre . Ce composant n’est pas du tout obligatoire, il vient en soutien aux MD Core Server. Donc si le MD Core Server est surchargé de demande, il peut déléguer des analyses à son ou ses MD Core Node associés. A noter que la connexion entre serveur et nodes n’est pas sécurisée nativement.

**MD Core Manager** : Si l’on possède plusieurs MD Core Server avec ou sans ses MD Core Node et qu’on veut manager plusieurs MD Core Server d’un seul endroit alors on peut utiliser un MD Core Manager qui peut gérer l’ensemble d’un parc Metadefender (Le Manager ne gère pas le fonctionnement de base de données partagées).

Exemple de fonctionnement avec les 3 composants :

![sftp-partie3-diagram1.png](/assets/img/clouds/tempo/sftp-partie3-diagram1.png)

La licence définit combien de nœuds vous pouvez utiliser, le manager quant à lui est complètement gratuit.

### Installation
Metadefender Core V4 (MD) peut répondre aux besoins de la plus petite infrastructure à la plus grande (à condition de mettre le prix dans la licence car elle définit les nombres de nœuds utilisables).

Petite liste des contraintes que vous devez prendre en compte dans le design de votre infrastructure (de la version 4.19.0 à au moins 4.21.2 de Metadefender).

**Base de données** : La base de données peut être locale, distante, partagée ou non. Si elle est locale, elle peut tout de même être partagée, mais si la machine locale qui la contient tombe, toutes les machines connectées à cette base de données seront hors d’usage. Chacun des serveurs peut avoir sa propre base de données locale, mais la répartition de charge sera plus compliquée car les résultats d’analyses sont contenus dans la base de données, il faut donc être sûr de retomber sur le même serveur pour récupérer le résultat de notre analyse.

**Deployment_ID** : Chaque instance metadefender possède un deploymentID unique (MSCLXXXXXX), il permet l’identification de l’instance du côté de chez Opswat. En effet, lors de l’activation via la licence de la solution sur l’instance, le deploymentID sera lié avec la licence, et cette information sera stockée chez eux (même avec un snapshot, si le snapshot est lancé sur une nouvelle instance, le deploymentID sera différent).
Ce que ça veut dire : si votre licence est activée/liée avec ce deploymentID et que l’instance en question tombe pour une raison X ou Y, la licence sera bloquée avec l’ancien deploymentID, c’est pourquoi il faut désactiver/délier la licence. Deux désactivations sont possibles : en local (un curl local) ou à distance (curl avec le deploymentID renseigné), en clair il faut stocker le deploymentID quelque part (pas de panique, en cas de perte Opswat peut vous redonner le deploymentID lié à votre licence).
L’activation de la licence peut se faire en ligne et hors ligne, mais si vous planifiez d’automatiser le déploiement de votre stack Metadefender, le hors ligne n’est pas une bonne idée, car elle suscite que vous alliez sur le site du vendeur pour y renseigner le  deploymentID. Prévoyez donc une connexion sortante pour la communication de vos instances vers les serveurs opswat pour faciliter l’automatisation.

**Instance_Name** : Il faut remplir un fichier de configuration au préalable de l’installation. Dans celui-ci, on peut y renseigner un nom d’instance (pas l’instance name d’AWS), s’il n’est pas défini, par défaut il sera composé des 6 premiers caractères du deploymentID. Cet instance name sert d’identification à la base de données. Si deux instances ont le même instance name, la base de données (si partagée) va considérer que c’est la même machine, ce qui provoque des problèmes à l'activation de la licence, certes elle sera activée sur les deux instances, mais une seule machine aura ses moteurs antivirus activés, car la base de données ne stockera les moteurs et les informations de mise à jour que d’une seule des deux. L’instance name peut être redéfini après l’installation, dans un fichier de configuration.

[Lien d’installation](https://docs.opswat.com/mdcore/installation/shared-database-mode)

Schéma d’installation (les images sont toujours plus parlantes. Ici la base de données est partagée et distante) :

![sftp-partie3-diagram2.png](/assets/img/clouds/tempo/sftp-partie3-diagram2.png)

1. Dans le fichier de configuration sont renseignés les différents accès de la solution, les accès actuels de la base de données distante et enfin le nom de l’instance metadefender (au sens Metadefender et non pas celui d’AWS) reconnu par la base de données.
1. Durant l’installation sera généré un déploiement ID propre à la machine et une base de données sera créée dans l’instance RDS.
1. L’activation signifie lier le déploiement ID et la licence, ces informations sont aussi stockées chez l’éditeur OPSWAT. Cela peut être utile lorsque l’instance est perdue, dès lors il faudra désactiver la licence pour pouvoir l’utiliser sur une autre machine. Il faut détenir le déploiement ID pour cela, et si celui-ci n’a pas été stocké, il est possible de le demander au support d’OPSWAT.

Quelques particularités :

La base de données peut être locale, ça se configure aussi dans le fichier de configuration avant installation.

L’activation de la licence peut se faire hors ligne, tout un processus existe pour ce faire.

### Exemple de mise en place
Voici un exemple d’infrastructure avec la mise en place du parc metadefender à haute disponibilité et automatisé.

#### Golden Image

Pour la construction d’une Image, Packer est une solution. Il permet de construire des images à partir d'une souche configurable avec l'aide de commandes ansible.

![sftp-partie3-diagram3.png](/assets/img/clouds/tempo/sftp-partie3-diagram3.png)

L’installation se fait entièrement dans l’image build, les services y sont désactivés à la fin du build. Le peaufinement s’effectue dans le user data où l’instance name sera modifié et la solution activée.

#### Schéma d’infrastructure

![sftp-partie3-diagram4.png](/assets/img/clouds/tempo/sftp-partie3-diagram4.png)

Utilisation d’un NLB (un ALB est possible, mais il faut penser au déchiffrement, et au rechiffrement du certificat pour le chiffrement de bout en bout)

#### Déploiement automatisé

![sftp-partie3-diagram5.png](/assets/img/clouds/tempo/sftp-partie3-diagram5.png)

#### Il y a 2 Auto Scaling Group, Pourquoi ?

Les déploiements IDs sont stockés dans une secret et la seule façon de différencier les deux machines et leur déploiement ID est d’utiliser les tags, et 1 ASG permet d’avoir un tag particulier pour l’instance.

Ceci est un exemple de déploiement automatisé de la solution, mais bien sûr elle est évolutive en fonction de de vos propres contraintes et de vos possibilités d'exécution (si vous ne faites pas de la construction d’image, tout peut se faire dans un userdata).

### Utilisation
Metadefender Core fonctionne de manière asynchrone et passive. Il faut la solliciter pour faire une analyse de fichier, et le refaire pour obtenir le résultat de celle-ci.

Il faut donc un intermédiaire qui puisse faire la demande d’analyse. Une lambda peut remplir cette fonction.

![sftp-partie3-diagram6.png](/assets/img/clouds/tempo/sftp-partie3-diagram6.png)

Metadefender peut scanner des fichiers directement depuis un bucket S3. Cependant, la requête API est obligatoirement en HTTP/HTTPS, ça signifie que le bucket S3 est soit publique (Impensable) soit l’URL est pré-signée.

Vous avez maintenant une proposition d’une infrastructure sécurisée complète de gestion de fichiers.

source : https://blog.ippon.fr/