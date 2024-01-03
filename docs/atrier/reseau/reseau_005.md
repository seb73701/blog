---
title: Les certificats, de l’émission à la révocation
description: 
published: true
date: 2023-04-21T10:39:08.591Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:39:04.169Z
---

# Les certificats, de l’émission à la révocation

>Cet article a pour ambition de vous présenter le cycle de vie d’un certificat de la demande jusqu’à la révocation. Cette démonstration mettra en lumière les mécanismes ainsi que les flux menant à l’obtention d’un certificat pour vos machines.

Les certificats sont des composants majeurs des infrastructures informatiques. Ils sont souvent utilisés pour authentifier des entités les unes par rapport aux autres et établir une communication chiffrée. Dans des systèmes massivement distribués adossés à de grandes collections de services, la gestion de leur cohérence peut s’avérer problématique. De plus, les entités étant réparties de façon fortement distribuée sous différents domaines d’administration, la question de la révocation se pose également. Cet article va vous présenter la mise en œuvre d’une infrastructure de gestion de clés « from scratch », uniquement avec OpenSSL pour bien identifier les différents mécanismes.

## 1. Rappels de cryptographie (appliquée)
L’objectif de la cryptographie est très simple : chiffrer une information en clair au moyen d’une clé. Seuls les possesseurs de cette clé peuvent déchiffrer l’information. On distingue deux types de cryptographie : symétrique et asymétrique. La cryptographie symétrique est assez simple. Soit deux personnes Aline (A) et Bodor (B). Ils cherchent à communiquer en chiffrant leurs messages afin de garantir la confidentialité des échanges. Ils possèdent donc la même clé pour chiffrer et déchiffrer leurs messages. La cryptographie symétrique est redoutable en termes de performance, car les algorithmes sont accélérés matériellement et les clés sont assez courtes. Le problème se situe au niveau de la scalabilité, en effet introduisons un troisième larron : Colombo (C). Si tout le monde veut communiquer de façon sécurisée, il faut une clé par paire possible : A-B, A-C et B-C. Il faut donc trois clés. Si on ajoute encore une quatrième personne, on passe à six clés, etc. Les clés sont donc plus liées à une communication pair à pair plutôt qu’à une personne. C’est pour ça que vous pouvez voir que ces clés sont parfois appelées clés de session dans la littérature.

La cryptographie asymétrique a un fonctionnement différent. Chaque personne génère une biclé : une clé publique et une clé privée. Contrairement à la cryptographie symétrique, les clés sont liées à une identité plutôt qu’à une session. La cryptographie asymétrique est donc généralement associée aux problématiques d’authentification. Cette biclé se compose d’une clé publique et une clé privée. Les règles sont :

1. Ce qui est chiffré avec la clé publique de A se déchiffre avec la clé privée de A ;
1. Ce qui est chiffré avec la clé privée de A se déchiffre avec la clé publique de A ;
1. La clé privée reste dans la poche du propriétaire quoi qu’il arrive (ça veut dire qu’elle ne se prête pas et qu’elle ne transite pas par le réseau) ;
1. La clé publique se distribue à tout le monde (par exemple, via des certificats).

L’art de gérer cette cryptographie asymétrique se nomme Infrastructure de Gestion de Clés (IGC) ou Public Key Infrastructure (PKI). Je préfère l’acronyme anglo-saxon PKI, car il met l’accent sur la difficulté de partager les clés publiques.

Mais que peut-on faire avec la cryptographie asymétrique ? Elle sert à deux choses : chiffrer et signer. Je suis A, je veux envoyer un message chiffré à B. Je récupère la clé publique de B (principe 4) et je chiffre le message avec. B pourra déchiffrer le message avec sa clé privée (principe 1). Maintenant, B veut s’assurer que je suis bien à l’origine du message. Je vais ajouter une signature à mon message. Cette signature consiste à appliquer une fonction de hachage (cf. note) sur le message en clair pour créer une empreinte (ou digest). Je chiffre ensuite cette empreinte avec ma clé privée. En récupérant ma clé publique, B peut déchiffrer la signature pour récupérer l’empreinte. B applique ensuite la fonction de hachage sur le message déchiffré pour voir si l’empreinte produite coïncide avec celle reçue (c’est-à-dire issue du déchiffrement de ma signature).

>Fonctions de hachage
>Une fonction de hachage associe une image de taille arbitraire en entrée, une image de taille fixe en sortie. La propriété essentielle est que cette fonction est pratiquement impossible à inverser, c'est-à-dire que si l'image d'une donnée par la fonction se calcule très efficacement, le calcul inverse d'une donnée d'entrée ayant pour image une certaine valeur se révèle impossible sur le plan pratique. >Pour cette raison, on dit d'une telle fonction qu'elle est à sens unique.

## 2. Certificats
Cette section va effleurer les aspects cryptographiques pour se concentrer sur ce qui concerne l’administrateur système. Quelle clé sert à signer quoi ? Ou encore, qui garantit quoi ?

### 2.1 Un certificat, pourquoi ?
Si on réduit un certificat à sa fonction la plus basique, elle est de distribuer une clé publique. Un certificat contient donc deux informations : une clé publique à distribuer et le nom pleinement qualifié (FQDN) du service s’appuyant sur le certificat. Dans notre laboratoire de test, nous allons certifier un serveur web pour communiquer avec lui en HTTPS, c’est-à-dire en HTTP sur SSL.

SSL (Secure Socket Layer) est un protocole de chiffrement de sessions TCP. Le principe est que le client récupère la clé publique du service qui communique en SSL. Une fois cette clé en poche, il s’en sert pour chiffrer une communication avec le service en SSL (cf. principe numéro 1 de la cryptographie asymétrique). Par cette communication, ils échangent une clé symétrique pour chiffrer leurs échanges futurs. En résumé, la clé publique sert juste à chiffrer un échange pour obtenir une clé de session beaucoup plus efficace en termes de performances. Nous savons maintenant ce que contient un certificat et son cadre d’utilisation. Il reste à expliquer ce qu’on certifie exactement et comment.

Nous avons vu qu’un certificat contient un couple clé publique et FQDN. Ces deux éléments sont nécessaires pour ouvrir sur une machine un socket SSL. Il est utilisé par les services nécessitant un canal chiffré et authentifié. Ainsi, lorsqu’un client cherche à joindre un service en SSL type HTTPS, il commence par faire une requête DNS pour localiser l’adresse IP hébergeant le service à connecter. Le client se connecte ensuite au port TCP en SSL. Le service lui présente un certificat signé par une autorité de certification. La signature consiste à chiffrer l’empreinte du certificat avec la clé privée (cf. principe numéro 2 de la cryptographie asymétrique) d’une autorité de certification ou CA (Certificate Authority).

Une CA est donc un tiers de confiance dont le certificat (donc la clé publique) est distribué sur le client. Par exemple, dans les navigateurs web nous trouvons un magasin contenant tous les certificats des CA bien connues type Digicert, Sectigo, etc. En utilisant la clé publique de la CA stockée dans le magasin, un client peut donc vérifier la signature du certificat distribué par le service certifié (serveur web dans notre article). Attention, la clé privée de la CA ne doit JAMAIS sortir de la machine l’hébergeant (cf. principe numéro 3 de la cryptographie asymétrique).

Il y a deux types de CA précisant le niveau de signature du certificat présenté par le service :

1. Auto-signé : lorsque l’on génère la biclé pour le service, le certificat est signé par sa clé privée associée. Le niveau de confiance dans la signature est donc très limité. Cependant, ce genre de certificat peut être tout à fait valable lorsque la seule propriété de sécurité recherchée est le chiffrement du canal de communication avec le service en SSL ;
1. CA type tiers de confiance. Dans ce cas, le certificat livré est signé par un tiers de confiance.

On distingue deux types de CA tiers :

1. CA racine (ou root CA) : cette CA est celle qui « amorce la pompe ». C’est-à-dire que la CA utilise sa biclé pour faire une auto-signée. Elle attend ensuite les demandes des services à certifier. Ces services émettent ce que l’on appelle une CSR (Certificate Signing Request). Cette CSR contient la clé publique du service à certifier ainsi que son FQDN. La CA y appose sa signature avec sa clé privée pour transformer cette CSR en certificat ;
1. CA intermédiaire : la CA intermédiaire se situe entre la CA racine et le service à certifier. La CA intermédiaire dispose d’une clé privée et d’un certificat associé signé par la clé privée de la CA racine. Dans cette topologie, qui est celle de la plupart des infrastructures de gestion de clés, le service envoie sa CSR à certifier à la CA intermédiaire.

La concaténation du certificat racine et du ou des certificats intermédiaires constitue la chaîne de confiance (ou chain of trust). Cette chaîne est nécessaire pour configurer les services s’appuyant sur SSL.

### 2.2 Laboratoire de test
Nous allons illustrer l’utilisation des certificats en nous basant sur le service Apache installé sur une machine nommée www. Nous allons le faire écouter en SSL sur le port TCP 443 en utilisant tout d’abord un certificat auto-signé par le serveur lui-même. Ensuite, nous installerons une CA sur une machine à part nommée ca. Cette machine recevra la CSR du serveur web, signera cette dernière avec sa clé privée puis lui renverra le certificat (CSR signée) à www. La dernière topologie de CA est la plus compliquée. Elle consistera à ajouter une machine faisant office de CA intermédiaire nommée inter. Cette CA possède une clé privée et un certificat signé par la CA de niveau racine. Elle signera les demandes des différents services à certifier sans exposer la clé privée de la CA racine.

Enfin, nous mettrons en place un service de révocation. La révocation consiste à invalider un certificat émis par une CA. Lorsque le client va récupérer le certificat depuis le service, il va vérifier au moyen d’un protocole donné, la validité du certificat présenté. Dans notre article, nous utiliserons OCSP (Online Certificate Status Protocol) qui s’exécutera sur inter pour révoquer le certificat généré. Tous les flux de CSR et signatures sont résumés dans la figure 1.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/lab.png">

Fig. 1 : Flux des CSR et signatures associées.

Tout notre laboratoire de test sera mis en place avec l’outil OpenSSL. Il existe des scripts tout faits adaptés aux grandes PKI, mais ils en masquent un peu le fonctionnement.

## 3. Création d’un auto-signé
Dans ce cas de figure, tout se passe sur le serveur web www. Commençons par installer Apache :

```
root@www:~# apt-get install apache2
```

Créons un répertoire ssl dans /etc/apache2/ pour travailler dedans :

```
root@www:~# mkdir -p /etc/apache2/ssl/
```

Nous pouvons générer la biclé et le certificat en une seule commande :

```
root@www:/etc/apache2/ssl# openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

Analysons les paramètres en séquence de la commande. La commande req indique que c’est le module émission de certificat qui est invoqué. L’argument -x509 accolé à req génère un certificat auto-signé à la place d’une CSR. L’argument -newkey génère une clé privée, ici avec l’algorithme asymétrique RSA d’une longueur de 4096 bits. L’argument -keyout indique le fichier dans lequel stocker la clé privée. L’argument -out indique le fichier dans lequel stocker le certificat, ici il s’agit de cert.pem. L’argument -days indique la durée de validité du certificat. Enfin, l’argument -nodes indique que la clé privée n’est pas chiffrée. Si on scrute les sorties, on voit bien que les trois étapes sont bien suivies.

Étape 1, génération de la clé privée :

```
Generating a RSA private key
.....................................................................++++
.......................................++++
writing new private key to 'key.pem'
```

Étape 2, établissement des attributs de la CSR :

```
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:FR
State or Province Name (full name) [Some-State]:France
Locality Name (eg, city) []:Villetaneuse
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Lab
Organizational Unit Name (eg, section) []:Local
Common Name (e.g. server FQDN or YOUR name) []:www.lab.local
Email Address []:nicolas.greneche@lab.local
```

Dans cette seconde étape, il faut renseigner les attributs du certificat de façon interactive. L’attribut auquel il faut être particulièrement attentif est le Common Name qui doit être fixé à la valeur du FQDN du service à certifier. Ici, il s’agit d’un serveur web donc nous fixons l’attribut avec la valeur www.lab.local. L’étape 3 est invisible, mais il s’agit bien de la signature automatique de la CSR générée précédemment pour obtenir le fichier cert.pem. Passons à la configuration d’Apache.

Tel que livré par les mainteneurs du paquet, Apache écoute en HTTP (clair) sur le port 80. Nous allons commencer par configurer l’emplacement des certificats. Cela se passe dans le fichier /etc/apache2/sites-available/default-ssl.conf :

```
root@www:/etc/apache2/ssl# cat /etc/apache2/sites-available/default-ssl.conf
SSLCertificateFile     /etc/apache2/ssl/cert.pem
SSLCertificateKeyFile   /etc/apache2/ssl/key.pem
```

Dans ce fichier, les deux directives à configurer sont SSLCertificateFile et SSLCertificateKeyFile qui contiennent respectivement le certificat auto-signé et la clé privée générés précédemment. Ensuite, on active le module SSL d’Apache :

```
root@www:/etc/apache2/ssl# a2enmod ssl
Considering dependency setenvif for ssl:
```

Puis le site par défaut qui répondra en HTTPS (SSL port 443) :

```
root@www:/etc/apache2/ssl# a2ensite default-ssl
Enabling site default-ssl.
```

On redémarre Apache :

```
root@www:/etc/apache2/ssl# systemctl restart apache2
```

Depuis le client, lorsque nous allons sur le serveur web depuis un navigateur en tapant l’URL https://www.lab.local, nous avons un avertissement de sécurité qui nous indique que le certificat est auto-signé et qu’à ce titre, il n’est pas fiable pour authentifier le serveur auprès du client (figure 2).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/selsigned-error.png">

Fig. 2 : Avertissement certificat auto-signé.

Nous pouvons tout à fait Accepter le risque et poursuivre. Une fois ce certificat auto-signé validé au niveau du navigateur, il est intéressant d’aller voir dans le magasin des certificats ce qui s’est passé. Sous Firefox, on le trouve dans la section Préférences, puis Vie privée (about:preferences#privacy). Nous voyons qu’un certificat a bien été ajouté dans la section Serveurs (figure 3) et Autorités (de certification, figure 4). Comme il est auto-signé, il fait à la fois office de certificat pour le serveur contacté (www) et d’autorité de certification pour lui-même. En conséquence, ce genre de certificat est inadapté à l’authentification du serveur auprès du client, mais il est tout à fait valide si nous cherchons juste à chiffrer la session HTTP client/serveur.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/magasin-self.png">

Fig. 3 : Magasin des serveurs.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/magasin-self-ca.PNG">

Fig. 4 : Magasin des CA.

## 4. Ajout d’une CA
En vous baladant dans ce magasin, vous allez voir qu’un certain nombre de CA sont pré-installées dans le navigateur. Ainsi, tous les certificats signés par une de ces CA sont reconnus directement par votre navigateur et ne lèvent pas d’alertes. C’est ce service que font payer les autorités de certification type Digicert, Sectigo, etc. Nous allons donc créer la nôtre, l’utiliser pour générer un certificat pour www et l’installer dans le magasin de notre navigateur web.

### 4.1 Préparation de la CA
Commençons par créer la racine (au niveau système de fichiers) de notre CA. Nous allons la mettre dans /opt/ca :

```
root@ca:~# mkdir -p /opt/ca /opt/ca/csr /opt/ca/certs
```

Plaçons-nous dedans :

```
root@ca:~# cd /opt/ca
```

Générons la clé privée de la CA :

```
root@ca:/opt/ca# openssl genrsa -out CA-key.pem 4096
Ici, la directive genrsa indique que nous générons une clé privée. L’argument -ou indique l’emplacement de la clé, ici CA-key.pem d’une taille de 4096 bits.

Generating RSA private key, 4096 bit long modulus (2 primes)
.........................++++
..............................................................++++
e is 65537 (0x010001)
```

Créons ensuite le certificat auto-signé qui amorce la pompe de l’autorité de certification. Ce certificat est amené à être distribué dans les magasins des clients qui vont se connecter au site. Voici la commande :

```
root@ca:/opt/ca# openssl req -new -key CA-key.pem -x509 -days 1000 -out CA-cert.pem
```

Pour les arguments, nous sommes en terrain connu. Nous demandons un nouveau certificat : req, -new et -x509, que nous signerons avec la clé de la CA générée juste avant -key CA-key.pem pour 1 000 jours -days. Le nouveau certificat généré sera stocké dans CA-cert.pem avec -out CA-cert.pem. Il nous redemande exactement les mêmes informations que lors de la génération de l’auto-signé de la section précédente. Évidemment, il faut changer le Common Name pour qu’il corresponde au nom de la machine ca, à savoir ca.lab.local.

```
Common Name (e.g. server FQDN or YOUR name) []:ca.lab.local
```

Passons à la génération et signature du certificat destiné au serveur web.

### 4.2 Certification du serveur web
Comme précédemment, il nous faut commencer par créer une clé privée :

```
root@www:/etc/apache2/ssl# openssl genrsa -out www-key.pem 4096
```

Ensuite, il faut créer une CSR. Cette CSR est destinée à être signée par la CA. Dedans, on retrouve la clé publique et le FQDN du service à certifier. En effet, lors de sa génération, nous devons répondre aux mêmes questions que lors de l’établissement d’un auto-signé dans les sections précédentes. Le champ critique est une nouvelle fois le Common Name :

```
root@www:/etc/apache2/ssl# openssl req -new -key www-key.pem -out www-cert.csr
Common Name (e.g. server FQDN or YOUR name) []:www.lab.local
```

Nous envoyons ensuite la CSR à la machine ca pour signature :

```
root@www:/etc/apache2/ssl# scp www-cert.csr root@ca:/opt/ca/csr/
```

Allons sur la machine ca pour procéder à la signature de la CSR reçue :

```
root@ca:~# cd /opt/ca/certs/
root@ca:/opt/ca/certs# openssl x509 -req -days 365 -in ../csr/www-cert.csr -CA ../CA-cert.pem -CAkey ../CA-key.pem -CAcreateserial -out www-cert.pem
 
Signature ok
subject=C = FR, ST = France, L = Villetaneuse, O = Lab, OU = Local, CN = www.lab.local, emailAddress = nicolas.greneche@lab.local
Getting CA Private Key
```

Nous utilisons la sous-commande x509 d’OpenSSL. Elle indique que nous allons signer un certificat. L’option -req indique que ce certificat est généré à partir depuis une CSR. Ce certificat sera valide 365 jours -days. L’argument -in indique le chemin de la CSR, -CA le certificat de la CA et enfin, -CAkey est le chemin de la clé privée de la CA. L’option -CAcreateserial indique qu’on délègue à OpenSSL la gestion des numéros de série des certificats. Enfin, le -ou indique le nom du fichier contenant le certificat www-cert.pem. Nous le renvoyons au serveur web :

```
root@ca:/opt/ca/certs# scp www-cert.pem root@www:/etc/apache2/ssl/
```

Nous envoyons également le certificat de la CA au serveur web :

```
root@ca:/opt/ca/certs# scp ../CA-cert.pem root@www:/etc/apache2/ssl/
```

Enfin, retournons sur le serveur web pour le configurer :

```
root@www:~# cat /etc/apache2/ssl/sites-enabled/default-ssl.conf
SSLCertificateFile     /etc/apache2/ssl/www-cert.pem
SSLCertificateKeyFile   /etc/apache2/ssl/www-key.pem
SSLCertificateChainFile /etc/apache2/ssl/CA-cert.pem
```

Par rapport à la configuration de l’auto-signé, nous avons ajouté une nouvelle directive SSLCertificateChainFile qui contient la chaîne de certification. Ici, la chaîne est assez courte, il n’y a que la CA. Redémarrons Apache :

```
root@www:~# systemctl restart apache2
```

Sur le client vubuntu, il faut récupérer le certificat de la CA pour l’installer dans le navigateur web :

```
ngreneche@vubuntu:~$ scp root@ca.lab.local:/opt/ca/CA-cert.pem ./
```

L’installation se fait dans le magasin vu précédemment où il faut importer le certificat de la CA récupéré dans l’onglet Autorités. Lorsque nous consultons le site pour la première fois avec le certificat de la CA installé, nous n’avons plus de page d’avertissement. Passons maintenant à la configuration CA racine avec CA intermédiaire.

## 5. Configuration du couple CA racine / CA intermédiaire
La dernière configuration à effectuer est la configuration d’une CA racine certifiant une CA intermédiaire qui va certifier le certificat du serveur web. Dans cette configuration, on n’a aucune interaction directe entre la CA racine et les services finaux à certifier. En ce sens, cette dernière configuration nous rapproche du fonctionnement d’une autorité de certification professionnelle.

### 5.1 Configuration de la CA racine
Nous allons préparer le système de fichiers pour créer une racine pour notre CA racine indépendante des manipulations précédentes. Tout va se passer dans /opt/ca-w-inter.

```
root@ca:~# mkdir /opt/ca-w-inter
root@ca:~# cd /opt/ca-w-inter/
root@ca:/opt/ca-w-inter# mkdir certs crl newcerts private tmp
root@ca:/opt/ca-w-inter# chmod 700 private
root@ca:/opt/ca-w-inter# touch index.txt
root@ca:/opt/ca-w-inter# echo 1000 > serial
```

Commençons par créer la clé privée de la CA racine :

```
root@ca:/opt/ca-w-inter# openssl genrsa -aes256 -out private/ca.key.pem 4096
```

Il y a un nouvel argument -aes256 qui donne un algorithme symétrique de chiffrement de la clé. En pratique, un mot de passe est requis pour déchiffrer la clé avant usage. Il nous est demandé de façon interactive à la création de la clé :

```
Enter pass phrase for private/ca.key.pem:
Verifying - Enter pass phrase for private/ca.key.pem:
```

On fixe les droits :

```
root@ca:/opt/ca-w-inter# chmod 400 private/ca.key.pem
```

Créons le certificat auto-signé racine :

```
root@ca:/opt/ca-w-inter# openssl req -config openssl.cnf -key private/ca.key.pem -new -x509 -days 3650 -sha256 -extensions v3_ca -out certs/ca.cert.pem
```

Par rapport à avant, nous avons un argument -config suivi du fichier openssl.cnf et -sha256 qui force l’utilisation de cet algorithme de hachage pour la signature. Le openssl.cnf est un gros morceau. Le voici :

```ini
[ ca ]
default_ca = CA_root
 
[ CA_root ] # sur machine ca
dir               = /opt/ca-w-inter
certs             = $dir/certs
crl_dir           = $dir/crl
new_certs_dir     = $dir/newcerts
database          = $dir/index.txt
serial            = $dir/serial
RANDFILE          = $dir/private/.rand
private_key       = $dir/private/ca.key.pem
certificate       = $dir/certs/ca.cert.pem
crlnumber         = $dir/crlnumber
crl               = $dir/crl/ca.crl.pem
crl_extensions    = crl_ext
default_crl_days  = 30
default_md        = sha256
name_opt          = ca_default
cert_opt          = ca_default
default_days      = 3650
preserve          = no
policy            = policy_ca_only_sign_inter
 
[ CA_inter ] # sur machine inter
dir               = /opt/ca-w-inter/intermediate
certs             = $dir/certs
crl_dir           = $dir/crl
new_certs_dir     = $dir/newcerts
database          = $dir/index.txt
serial            = $dir/serial
RANDFILE          = $dir/private/.rand
private_key       = $dir/private/intermediate.key.pem
certificate       = $dir/certs/intermediate.cert.pem
crlnumber         = $dir/crlnumber
crl               = $dir/crl/intermediate.crl.pem
crl_extensions    = crl_ext
default_crl_days  = 30
default_md        = sha256
name_opt          = ca_default
cert_opt          = ca_default
default_days      = 365
preserve          = no
policy            = policy_inter_sign_everything
 
[ policy_ca_only_sign_inter ]
countryName             = match
stateOrProvinceName     = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional
 
[ policy_inter_sign_everything ]
countryName             = optional
stateOrProvinceName     = optional
localityName            = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional
 
[ req ]
default_bits        = 4096
distinguished_name  = req_distinguished_name
string_mask         = utf8only
default_md          = sha256
x509_extensions     = v3_ca
 
[ req_distinguished_name ]
countryName                     = Country Name (2 letter code)
stateOrProvinceName             = State or Province Name
localityName                    = Locality Name
0.organizationName              = Organization Name
organizationalUnitName          = Organizational Unit Name
commonName                      = Common Name
emailAddress                    = Email Address
 
countryName_default             = FR
stateOrProvinceName_default     = France
localityName_default            = Villetaneuse
0.organizationName_default      = Lab
organizationalUnitName_default  = Local
emailAddress_default            = nicolas.greneche@lab.local
 
[ v3_ca ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
 
[ v3_intermediate_ca ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
 
[ server_cert ]
basicConstraints = CA:FALSE
nsCertType = server
nsComment = "OpenSSL Generated Server Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer:always
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
authorityInfoAccess = OCSP;URI:http://ocsp-inter.lab.local:2555
 
[ client_cert ]
basicConstraints = CA:FALSE
nsCertType = client, email
nsComment = "OpenSSL Generated Client Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth, emailProtection
 
[ crl_ext ]
authorityKeyIdentifier=keyid:always
 
[ ocsp ]
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, digitalSignature
extendedKeyUsage = critical, OCSPSigning
Nous nous réfèrerons aux sections essentielles à la compréhension au fil de l’eau. Cela commence avec l’argument -extensions qui vaut v3_ca. Nous constatons que l’on trouve une section intitulée de la sorte dans le fichier openssl.cnf. Cette section règlemente l’utilisation du certificat. Par exemple, il est dit que le certificat est un certificat de CA CA:true et que cette contrainte est critique (critical). Une contrainte critique signifie que si une application manipule ce certificat et qu’elle ne considère pas cette extension comme critique, alors elle ne peut pas utiliser le certificat. L’extension keyusage est également critique et dans cette section, elle indique que la clé peut tout signer des empreintes, des CRL et des certificats respectivement digitalSignature, cRLSign, keyCertSign.
```

Les CRL (Certificate Revocation List) sont des listes de certificats révoqués qui sont vérifiées par le client au moment de son utilisation. Si le certificat proposé est révoqué, alors le client refusera de l’utiliser. Nous verrons un exemple de révocation plus tard. Donc, par rapport aux précédentes générations de certificats, ici on nous demande une clé pour déchiffrer la clé privée de la CA et l’utiliser :

```
Enter pass phrase for private/ca.key.pem:
```

On notera également que les attributs du certificat sont préremplis avec les bonnes valeurs. Ceci est dû au fait que l’on trouve une section req_distinguished_name dans le openssl.cnf qui définit toutes ces valeurs :

```
Country Name (2 letter code) [FR]:
State or Province Name [France]:
Locality Name [Villetaneuse]:
Organization Name [Lab]:
Organizational Unit Name [Local]:
Common Name []:ca.lab.local
Email Address [nicolas.greneche@lab.local]:
```

Serrons les droits sur le certificat généré :

```
root@ca:/opt/ca-w-inter# chmod 444 certs/ca.cert.pem
```

Affichons le certificat généré (je n’ai laissé que les sections essentielles) :

```
root@ca:/opt/ca-w-inter# openssl x509 -noout -text -in certs/ca.cert.pem
Certificate:
       [...]
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                A0:47:9B:AE:DE:D8:A5:1F:38:45:D2:89:4A:95:06:AC:87:79:C3:61
            X509v3 Authority Key Identifier:
                keyid:A0:47:9B:AE:DE:D8:A5:1F:38:45:D2:89:4A:95:06:AC:87:79:C3:61
 
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
```

On retombe bien sur ce qui a été défini dans le fichier openssl.cnf. On notera également la présence d’une section CA_root dans ce fichier qui va renseigner l’emplacement sur le système de fichiers des différents composants de la CA racine.

### 5.2 Configuration de la CA intermédiaire
Allons sur la machine inter et créons la racine du système de fichiers dédié à notre CA intermédiaire :

```
root@inter:~# mkdir -p /opt/ca-w-inter/intermediate
root@inter:~# cd /opt/ca-w-inter/intermediate
root@inter:/opt/ca-w-inter/intermediate# mkdir certs crl csr newcerts private
root@inter:/opt/ca-w-inter/intermediate# chmod 700 private
root@inter:/opt/ca-w-inter/intermediate# touch index.txt
root@inter:/opt/ca-w-inter/intermediate# echo 1000 > serial
root@inter:/opt/ca-w-inter/intermediate# echo 1000 > /opt/ca-w-inter/intermediate/crlnumber
```

On récupère le openssl.cnf. Certaines personnes en font un différent pour chaque CA, je préfère tout mettre dans le même fichier et le balader. C’est personnel, il n y a pas vraiment de bonne pratique à ce niveau :

```
root@inter:/opt/ca-w-inter/intermediate# scp root@ca:/opt/ca-w-inter/openssl.cnf /opt/ca-w-inter/
```

Générons la clé privée de la CA intermédiaire :

```
root@inter:/opt/ca-w-inter# openssl genrsa -aes256 -out intermediate/private/intermediate.key.pem 4096
 
Enter pass phrase for intermediate/private/intermediate.key.pem:
Verifying - Enter pass phrase for intermediate/private/intermediate.key.pem:
```

Fixons les droits :

```
root@inter:/opt/ca-w-inter# chmod 400 intermediate/private/intermediate.key.pem
```
Créons la CSR de la CA intermédiaire à faire signer par la CA racine (il faudra entrer le mot de passe de la clé privée de la CA intermédiaire générée précédemment) :
```
root@inter:/opt/ca-w-inter# openssl req -config ./openssl.cnf -new -sha256 -key intermediate/private/intermediate.key.pem -out intermediate/csr/intermediate.csr.pem
```
Envoyons la CSR à signer à la CA racine :
```
root@inter:/opt/ca-w-inter# scp intermediate/csr/intermediate.csr.pem root@ca:/opt/ca-w-inter/tmp/
```
Retournons sur la CA pour procéder à la signature :
```
root@ca:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_root -extensions v3_intermediate_ca -days 3650 -notext -md sha256 -in tmp/intermediate.csr.pem -out tmp/intermediate.cert.pem
 
[...]
Sign the certificate? [y/n]:y
1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
```
Pour cette signature, nous utilisons la sous-commande ca. C’est la sous-commande utilisée par OpenSSL lorsque la signature d’un certificat implique de maintenir une base de données des certificats au sein d’une vraie PKI. D’ailleurs, on constate que cette directive est suivie de -config openssl.cnf -name CA_root qui indique d’utiliser les emplacements de fichiers dans la section CA_root. Nous chargeons ensuite les extensions v3_intermediate_ca. Si on se réfère au fichier openssl.cnf, nous voyons que la seule différence par rapport à V3_ca est pathlen:0. Cette directive signifie qu’il n y a aucune autre CA intermédiaire après celle-ci dans la chaîne de confiance. En gros, cette CA est le dernier intermédiaire avant le service final à certifier. Une fois le certificat signé, nous le renvoyons à la CA intermédiaire inter :

```
root@ca:/opt/ca-w-inter# scp tmp/intermediate.cert.pem root@inter:/opt/ca-w-inter/intermediate/certs/
```

Une fois de retour sur la CA intermédiaire, il nous faut récupérer le certificat de la CA racine pour le concaténer pour créer la chaîne de certification :
```
root@inter:/opt/ca-w-inter# scp root@ca:/opt/ca-w-inter/certs/ca.cert.pem certs/
root@inter:/opt/ca-w-inter# cat intermediate/certs/intermediate.cert.pem certs/ca.cert.pem > intermediate/certs/ca-chain.cert.pem
root@inter:/opt/ca-w-inter# chmod 444 intermediate/certs/ca-chain.cert.pem
```

Dans le fichier ca-chain.cert.pem, nous avons donc le certificat de la CA racine et de la CA intermédiaire l’un à la suite de l’autre.

### 5.3 Certification du service Apache par la CA intermédiaire
Commençons par récupérer la chaîne de certification créée ci-dessus :

```
root@www:~# scp root@inter:/opt/ca-w-inter/intermediate/certs/ca-chain.cert.pem /etc/apache2/ssl/
```

Nous passons ensuite à la création de la clé privée pour le serveur web :
```
root@www:/etc/apache2/ssl# openssl genrsa -out www-w-inter-key.pem 4096
```

Génération de la CSR :
```
root@www:/etc/apache2/ssl# openssl req -new -key www-w-inter-key.pem -out www-w-inter-cert.csr
 
Common Name (e.g. server FQDN or YOUR name) []:www.lab.local
```

Nous l’expédions ensuite à la CA intermédiaire pour signature :
```
root@www:/etc/apache2/ssl# scp www-w-inter-cert.csr root@inter:/opt/ca-w-inter/intermediate/csr/
```

Une fois sur la CA intermédiaire, on le signe :
```
root@inter:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_inter -extensions server_cert -days 365 -notext -md sha256 -in intermediate/csr/www-w-inter-cert.csr -out intermediate/certs/www-w-inter-cert.pem
```

Dans cette commande, nous indiquons que nous allons mettre à jour les fichiers de la CA intermédiaire -config openssl.cnf -name CA_inter. Nous utilisons ensuite les extensions server_cert. Il y a des choses à dire sur cette section. Tout d’abord, notons que dans l’extension keyUsage, les attributs cRLSign et keyCertSign ont laissé place à keyEncipherment. Cet attribut précise que la clé peut être utilisée pour chiffrer d’autres clés. Rappelons que la phase asymétrique de SSL sert à échanger une clé symétrique de session. Or, il faut pouvoir chiffrer cette clé symétrique pour la transmettre. C’est confirmé par l’extension extendedKeyUsage qui contient l’attribut serverAuth nécessaire pour l’utilisation du certificat au sein du protocole SSL, précisément l’authentification du serveur auprès du client. On envoie ensuite le certificat signé sur le serveur web :

```
root@inter:/opt/ca-w-inter# scp /opt/ca-w-inter/intermediate/certs/www-w-inter-cert.pem root@www:/etc/apache2/ssl/
```

Et on modifie les directives SSL du fichier de configuration :

```
root@www:~# cat /etc/apache2/sites-enabled/default-ssl.conf
SSLCertificateFile     /etc/apache2/ssl/www-w-inter-cert.pem
SSLCertificateKeyFile   /etc/apache2/ssl/www-w-inter-key.pem
SSLCertificateChainFile /etc/apache2/ssl/ca-chain.cert.pem
```

On notera que SSLCertificateChainFile contient maintenant la chaîne de certification créée à la fin de la section 5.2. Évidemment, un redémarrage d’Apache s’impose :
```
root@www:~# systemctl restart apache2
```

Sur le client vubuntu, même action que la dernière fois. Il faut récupérer le certificat de la CA racine. Et l’installer dans le magasin :
```
ngreneche@vubuntu:~$ scp root@ca.lab.local:/opt/ca-w-inter/certs/ca.cert.pem ./
```

Une petite sous-commande d’OpenSSL qui peut vous être utile est s_client. C’est une sorte de netcat (aka nc) pour socket SSL qui va notamment se connecter pour afficher toute la chaîne de certification :
```
ngreneche@vubuntu:~$ openssl s_client -connect www.lab.local:443
CONNECTED(00000003)
depth=2 C = FR, ST = France, L = Villetaneuse, O = Lab, OU = Local, CN = ca.lab.local, emailAddress = nicolas.greneche@lab.local
verify error:num=19:self signed certificate in certificate chain
verify return:1
depth=2 C = FR, ST = France, L = Villetaneuse, O = Lab, OU = Local, CN = ca.lab.local, emailAddress = nicolas.greneche@lab.local
verify return:1
depth=1 C = FR, ST = France, O = Lab, OU = Local, CN = inter.lab.local, emailAddress = nicolas.greneche@lab.local
verify return:1
depth=0 C = FR, ST = France, L = Villetaneuse, O = Lab, OU = Local, CN = www.lab.local, emailAddress = nicolas.greneche@lab.local
verify return:1
```

Évidemment, le navigateur ne produit aucune erreur à la connexion au site web. Dans les propriétés du certificat, nous pouvons vérifier la présence d’un serveur OCSP de révocation (figure 5).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/ocsp.png">

Fig. 5 : Informations sur le serveur OCSP.

## 6. La révocation
Pour être exhaustifs sur le sujet, nous allons voir le processus de révocation du certificat précédemment généré pour notre serveur web. Nous allons donc installer un serveur OCSP (Online Certificate Status Protocol) sur inter. Ce serveur va gérer une base de données des certificats révoqués. OpenSSL intègre un service OCSP. Nous allons l’utiliser. Si on examine les extensions server_cert, on y trouve authorityInfoAccess = OCSP;URI:http://ocsp-inter.lab.local:2555. Ce qui veut dire que dans le certificat délivré pour le serveur web et qu’il va présenter au client, le client retrouve l’adresse du serveur OCSP à requêter avent de faire usage du certificat. Il se trouve sur la machine ocsp-inter.lab.local (qui est un alias de inter) et qui écoute sur le port TCP 2555.

### 6.1 Installation du service OCSP
Commençons par générer la clé privée du service OCSP :

```
root@inter:/opt/ca-w-inter# openssl genrsa -aes256 -out intermediate/private/ocsp-inter.lab.local.key.pem 4096
```

Puis générons sa CSR avec comme Common Name le FQDN du serveur hébergeant le service OCSP :

```
root@inter:/opt/ca-w-inter# openssl req -config openssl.cnf -new -sha256 -key intermediate/private/ocsp-inter.lab.local.key.pem -out intermediate/csr/ocsp-inter.lab.local.csr.pem
Common Name []:ocsp-inter.lab.local
```

Enfin, on signe cette CSR avec la clé privée de la CA intermédiaire et en invoquant l’extension ocsp du fichier openssl.cnf :

```
root@inter:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_inter -extensions ocsp -days 365 -notext -md sha256 -in intermediate/csr/ocsp-inter.lab.local.csr.pem -out intermediate/certs/ocsp-inter.lab.local.cert.pem
Using configuration from openssl.cnf
Enter pass phrase for /opt/ca-w-inter/intermediate/private/intermediate.key.pem:
```

Cette extension contient l’attribut extendedKeyUsage qui contient la valeur OCSPSigning. C’est-à-dire que cette clé signe des révocations OCSP.

### 6.2 Création du certificat pour Apache avec révocation
Je vais donner les commandes en séquence, ce sont les mêmes que celles utilisées tout au long de l’article. Création de la clé privée du serveur web :

```
root@www:/etc/apache2/ssl# openssl genrsa -out www-w-inter-ocsp-key.pem 4096
```

Et de la CSR :

```
root@www:/etc/apache2/ssl# openssl req -new -key www-w-inter-ocsp-key.pem -out www-w-inter-ocsp-cert.csr
```

On l’envoie sur la machine inter pour certification :

```
root@www:/etc/apache2/ssl# scp www-w-inter-ocsp-cert.csr root@inter:/opt/ca-w-inter/intermediate/csr/
```

Une fois sur inter, on la signe :
```
root@inter:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_inter -extensions server_cert -days 365 -notext -md sha256 -in intermediate/csr/www-w-inter-ocsp-cert.csr -out intermediate/certs/www-w-inter-ocsp-cert.pem
```

On récupère le certificat sur www :

```
root@www:/etc/apache2/ssl# scp root@inter:/opt/ca-w-inter/intermediate/certs/www-w-inter-ocsp-cert.pem ./
```

Enfin, on configure Apache :

```
root@www:~# cat /etc/apache2/sites-enabled/default-ssl.conf
SSLCertificateFile     /etc/apache2/ssl/www-w-inter-ocsp-cert.pem
SSLCertificateKeyFile   /etc/apache2/ssl/www-w-inter-ocsp-key.pem
SSLCertificateChainFile /etc/apache2/ssl/ca-chain.cert.pem
```

Allons sur le client pour voir ce que le serveur nous présente. Cette commande sert à « aspirer » le certificat présenté par le serveur web :

```
ngreneche@vubuntu:~$ echo -n | openssl s_client -connect www.lab.local:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > cert.pem
```

Dans un second temps, nous le visionnons :
```
ngreneche@vubuntu:~$ openssl x509 -noout -text -in cert.pem
[...]
TLS Web Server Authentication
Authority Information Access:
OCSP - URI:http://ocsp-inter.lab.local:2555
```

On peut voir que le certificat présenté intègre l’emplacement du serveur OCSP à interroger avant usage. Plaçons-nous maintenant sur inter pour lancer le service de révocation :

```
root@inter:/opt/ca-w-inter# openssl ocsp -port 2555 -text -rmd sha256 -index intermediate/index.txt -CA intermediate/certs/ca-chain.cert.pem -rkey intermediate/private/ocsp-inter.lab.local.key.pem -rsigner intermediate/certs/ocsp-inter.lab.local.cert.pem
 
Enter pass phrase for intermediate/private/ocsp-inter.lab.local.key.pem:
ocsp: waiting for OCSP client connections…
```

Cette commande va lancer un serveur OCSP sur le port 2555 de toutes les interfaces disponibles (-port). Il va gérer les certificats indexés dans le fichier /opt/ca-w-inter/ intermediate/index.txt (-index). Enfin, les arguments -rkey et -rsigner permettent de spécifier respectivement la clé et le certificat du service OCSP. Ouvrons une autre console sur inter pour vérifier le certificat signé pour le compte de www :

```
root@inter:/opt/ca-w-inter# openssl ocsp -CAfile intermediate/certs/ca-chain.cert.pem -url http://127.0.0.1:2555 -resp_text -issuer intermediate/certs/intermediate.cert.pem -cert intermediate/certs/www-w-inter-ocsp-cert.pem
 
intermediate/certs/www-w-inter-ocsp-cert.pem: good
        This Update: Oct 29 13:30:41 2020 GMT
```

On vérifie que le certificat www-w-inter-ocsp-cert.pem n’a pas été révoqué par le serveur lancé ci-dessus -url http://127.0.0.1:2555. Ici, le résultat est good. Cela veut dire que le certificat du serveur web est absent des listes de révocation. Nous allons maintenant le révoquer.

### 6.3 Révocation du certificat
Pour révoquer un certificat, nous nous appuyons sur le paramètre revoke de la sous-commande ca d’OpenSSL. On lui spécifie le certificat à révoquer www-w-inter-ocsp-cert.pem :

```
root@inter:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_inter -revoke intermediate/certs/www-w-inter-ocsp-cert.pem
```

Testons à nouveau la validité du certificat de www :

```
root@inter:/opt/ca-w-inter# openssl ocsp -CAfile intermediate/certs/ca-chain.cert.pem -url http://127.0.0.1:2555 -resp_text -issuer intermediate/certs/intermediate.cert.pem -cert intermediate/certs/www-w-inter-ocsp-cert.pem
 
intermediate/certs/www-w-inter-ocsp-cert.pem: revoked
        This Update: Oct 29 13:42:42 2020 GMT
```
Le certificat a donc bien été révoqué. Lorsque nous accédons au serveur web, nous tombons sur la page présentée en figure 6. La révocation est un succès !

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/revoke.png">

Fig. 6 : Certificat révoqué.

## 7. Authentification avec un certificat client
Dans cette dernière section, nous allons générer un certificat pour l’utilisateur voulant se connecter au serveur web. Le navigateur de l’utilisateur devra présenter ce certificat au moment d’accéder au site sous peine de s’en voir refuser l’accès.

### 7.1 Génération du certificat client
Dans cette section, rien de neuf sous le soleil. On génère une clé privée pour l’utilisateur dans un répertoire à part certifs :

```
ngreneche@vubuntu:~$ mkdir certifs
ngreneche@vubuntu:~$ cd certifs/
ngreneche@vubuntu:~/certifs$ openssl genrsa -out ngreneche-client.key 4096
Generating RSA private key, 4096 bit long modulus (2 primes)
......................................++++
.............................................................++++
e is 65537 (0x010001)
```

Puis on génère la CSR :

```
ngreneche@vubuntu:~/certifs$ openssl req -new -key ngreneche-client.key -out ngreneche-client.csr
[...]
Country Name (2 letter code) [AU]:FR
State or Province Name (full name) [Some-State]:France
Locality Name (eg, city) []:Villetaneuse
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Lab
Organizational Unit Name (eg, section) []:Local
Common Name (e.g. server FQDN or YOUR name) []:ngreneche
Email Address []:nicolas.greneche@lab.local
 
Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Et on l’envoie à la CA intermédiaire :

```
ngreneche@vubuntu:~/certifs$ scp ngreneche-client.csr root@inter.lab.local:/opt/ca-w-inter/intermediate/csr/
```

Passons à la signature de cette CSR par la CA intermédiaire. Nous allons reprendre quasiment la même commande que pour signer le certificat réalisé section 5.3 à destination du serveur Apache. La seule différence est l’appel à l’extension client_cert. Les deux attributs critiques sont nsCertType = client, email et extendedKeyUsage = clientAuth, emailProtection qui indiquent que ce certificat pourra être utilisé pour de l’authentification client SSL ou de la signature d’e-mails. Allons sur la CA intermédiaire pour procéder à la signature :

```
root@inter:/opt/ca-w-inter# openssl ca -config openssl.cnf -name CA_inter -extensions client_cert -days 365 -notext -md sha256 -in intermediate/csr/ngreneche-client.csr -out intermediate/certs/ngreneche-client.crt
 
Using configuration from openssl.cnf
Enter pass phrase for /opt/ca-w-inter/intermediate/private/intermediate.key.pem:
Check that the request matches the signature
Signature ok
[...]
            X509v3 Extended Key Usage:
                TLS Web Client Authentication, E-mail Protection
```
Nous constatons que dans la sortie de la signature nous retrouvons les attributs positionnés par l’extension client_cert. Nous pouvons envoyer ce certificat sur le client :

```
root@inter:/opt/ca-w-inter# scp intermediate/certs/ngreneche-client.crt ngreneche@client.lab.local:/home/ngreneche/certifs/
```

Passons à la configuration du couple serveur web / navigateur.

### 7.2 Configuration du serveur web
La configuration du serveur web est très rapide, il y a deux directives à changer dans le /etc/apache2/sites-enabled/default-ssl.conf :

```
root@www:~# cat /etc/apache2/sites-enabled/default-ssl.conf
SSLCACertificateFile /etc/apache2/ssl/ca-chain.cert.pem
SSLVerifyClient require
SSLVerifyDepth 2
```

Le premier paramètre SSLCACertificateFile donne le chemin complet vers le fichier contenant la chaîne de certification qui a signé le certificat client (chez nous c’est la même que pour le certificat serveur). La seconde, SSLVerifyClient à require, indique à Apache qu’un certificat est nécessaire pour accéder au site. Enfin, SSLVerifyDepth à 2 refuse les certificats ayant plus de deux niveaux de CA dans la chaîne. On redémarre ensuite Apache :

```
root@www:~# systemctl restart apache2
```

Lorsqu’on y accède sans certificat, le serveur nous renvoie une erreur nous indiquant qu’un certificat est requis (figure 7).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/error-client.png">

Fig. 7 : Page d’erreur accès sans certificat client.

Nous allons configurer le navigateur web pour y ajouter le certificat du client.

### 7.3 Configuration du navigateur web
Avant d’importer le certificat, il est nécessaire de concaténer la clé privée et le certificat dans un seul fichier. Le format PKCS#12 est utilisé pour stocker les clés privées et les certificats dans un seul fichier chiffré. Nous allons utiliser la commande suivante sur le client pour créer ce fichier :

```
ngreneche@vubuntu:~/certifs$ openssl pkcs12 -export -inkey ngreneche-client.key -in ngreneche-client.crt -out ngreneche-client.p12
 
Enter Export Password:
Verifying - Enter Export Password:
```

Je vous conseille de mettre une passphrase, car le fichier contient votre clé privée. Une fois ce fichier créé, il faut l’importer dans le magasin du navigateur web. Cela se passe dans l’onglet Vos certificats comme indiqué dans la figure 8.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/magasin-client.png">

Fig. 8 : Page d’erreur accès sans certificat client.

Le navigateur vous demandera la passphrase entrée pour protéger le fichier au format PKCS#12. Une fois l’importation réussie, on retourne sur le site qui nous présente cette fois-ci la pop-up de la figure 9.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-123/popup-auth.png">

Fig. 9 : Sélection du certificat à utiliser.

Ce pop-up sert à sélectionner le certificat à utiliser pour s’authentifier au cas où on en ait plusieurs. Ici, nous n’en avons qu’un seul donc comme dirait JP, « la question elle est vite répondue ». Une fois le certificat sélectionné, l’accès au site est autorisé. On notera qu’à l’instar des certificats serveurs, les certificats clients peuvent être révoqués en suivant la même logique.

## Conclusion
Dans cet article, nous avons retracé le cycle de vie d’un certificat SSL de la création à la révocation. Nous nous sommes essentiellement concentrés sur les certificats « serveurs ». Ces certificats sont destinés à authentifier le serveur auprès du client et à chiffrer la communication. C’est le fonctionnement minimal de SSL. Comme nous l’avons vu, SSL peut également authentifier le client auprès du serveur pour une authentification bilatérale. Cette possibilité est à la base de toutes les authentifications sur tokens matériels type USB. En fait, votre token physique fait office de fichier PKCS#12 et il est manipulé par les librairies type OpenSC pour vous authentifier. Vous pourrez retrouver les machines virtuelles du laboratoire utilisées pour l’article à cette URL : https://drive.google.com/file/d/1NQKlNnrllaXfM2F78fn_UxLaaGNAOd8L/view?usp=sharing.


souce : https://connect.ed-diamond.com/Linux-Pratique/lp-123/les-certificats-de-l-emission-a-la-revocation
