---
title: Votre site web statique sur Amazon S3
description: 
published: true
date: 2023-04-21T10:58:19.273Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:58:13.919Z
---

# Votre site web statique sur Amazon S3

Amazon Web Services (AWS) est le leader sur le marché du cloud. On s’intéressera dans cet article à son service Simple Storage Service (S3) pour déployer un site internet en toute simplicité.

Pour ce faire, il est nécessaire d’avoir un compte AWS. Si vous n’en possédez pas, vous pouvez créer un compte Free Tier qui vous permettra de bénéficier de plusieurs services gratuitement, notamment S3, sous réserve de ne pas dépasser les limites annoncées par Amazon.

Pour plus d’informations, vous pouvez aller sur le site suivant : https://aws.amazon.com/fr/free/

A l'issue de cet article, vous pourrez créer un site hébergé sur S3 protégé via un certificat SSL/TLS associé à un nom de domaine si vous le souhaitez.

## Services appréhendés dans cet article

<img src="https://blog.ippon.fr/content/images/2022/04/services.png">

1. S3 pour héberger votre site web statique.
1. CloudFront pour diffuser votre site à travers le web et lui associer un certificat SSL/TLS.
1. Route 53 pour créer et gérer votre nom de domaine.
1. Certificate Manager pour créer un certificat SSL/TLS.

## Votre application web
On ne peut déployer sur S3 qu’une application front. C'est-à-dire une application qui est exécutable par un client (votre navigateur).

Par exemple : une application écrite avec le framework Angular ou React, un simple site HTML/CSS etc.

Si vous avez découpé votre application en deux parties : front-end et back-end, la partie front-end peut très bien être déployée sur S3.

Pour la partie back-end, vous pourrez la déployer sur Elastic Beanstalk : un service qui permet de déployer facilement une application (écrite en Go, Java, .NET, Node.js, PHP, Python et Ruby).

Ou vous pouvez vous orienter vers une approche serverless avec les services API Gateway et Lambda.

Ce ne sont que des exemples mais sachez qu’en fonction de votre architecture, il existe une multitude de services qui pourront répondre à vos besoins.


## S3
### 1) Créer un bucket S3
Pour commencer, sur la page d’accueil du service S3, créez un nouveau compartiment (bucket en anglais).

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/bucket_conf.png">

Choisissez la région AWS la plus proche de votre position pour limiter la latence. Pour ma part, c’est la région eu-west-3 (Paris).

Le nom doit obligatoirement correspondre à l’URL d’accès à votre site. Si l’URL de votre site est : `mys3website.fr` alors le nom du compartiment doit être le même.

Si vous ne le faites pas, vous ne pourrez pas créer un enregistrement (record) sur Route 53 pour y associer votre site S3 à votre nom de domaine.

### 2) Uploader votre site
Votre bucket étant créé, vous pouvez désormais charger les fichiers relatifs à votre site.

Cette étape peut être payante. A la date d’écriture du présent article, les requêtes PUT, COPY, POST et LIST sont facturées à 0,0053$ par tranche de 1000 requêtes pour la région eu-west-3 (Paris).

Vous avez le droit à 2000 requêtes gratuites par mois avec un compte Free Tier.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/upload_site.png">

Pour ma part, j’ai importé un simple fichier HTML nommé index.html.


### 3) Configuration du bucket
Maintenant que votre site est uploadé dans votre bucket, il ne reste plus qu’à le configurer pour qu’il devienne un site web statique.

Dans l’onglet “Propriétés”, aller dans la partie “Hébergement du site web statique” et cliquer sur le bouton “Modifier”.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/hebergement.png">

Vous n’avez plus qu'à activer l’hébergement de site web statique et de spécifier la page d'entrée de votre site.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/index_document.png">

Dans mon cas, il s’agit du fichier index.html mais il peut s’agir d’un autre point d’entrée pour vous.

Une fois validé, votre site web est accessible sur le web. Une URL sera générée par Amazon.


<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/endpoint.png">

Si vous accéder à cette URL, vous obtiendrez un code 403 Forbidden.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/forbidden.png">

En effet, tous les éléments de ce bucket S3 sont privés. Pour pouvoir y accéder, il faut rendre l’accès à ce bucket et ses éléments publics.

Pour ce faire :

Dans l’onglet “Autorisations”, modifiez la configuration “Bloquer l’accès public”.
Puis décochez “Bloquer tous les accès publics”.

Vous n’avez plus qu’à valider.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/block.png">

Enfin, il vous faut rajouter une stratégie de compartiment (bucket policy) pour autoriser l’accès en lecture aux fichiers de votre bucket.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::mys3website.fr/*"
        }
    ]
}
```



Vous pouvez modifier le Sid qui est un identifiant de la stratégie (en sachant qu’il est optionnel) et la partie Resource avec l’ARN de votre bucket S3.

Vous retrouverez l’ARN de votre ressource dans la même page :

<img src="https://blog.ippon.fr/content/images/2022/04/arn.png">

N'oubliez pas d'ajouter à la fin de l’ARN un “/*” pour inclure les fichiers du bucket sinon, vous ne pourrez pas valider votre stratégie de compartiment.

Il faut prendre conscience que les fichiers présents dans votre bucket sont accessibles publiquement. Ce n’est pas nécessairement une bonne pratique pour un environnement en production. Plus bas, je vous présente une meilleure façon de faire en passant par une distribution CloudFront.

### 4) Site web opérationnel enfin presque
A cette étape, votre site est opérationnel. Néanmoins, deux éléments peuvent encore vous gêner :

Votre site n’est pas sécurisé car il utilise le protocole HTTP.
L’URL de votre site laisse à désirer.


<img src="https://blog.ippon.fr/content/images/2022/04/url.png">

## Route 53
C’est le moment de s’intéresser à un autre service Amazon : Route 53. C’est un DNS (système de noms de domaines) et il va nous permettre de créer un nom de domaine pour votre site (ou de transférer un nom de domaine si vous en possédez déjà un sur une autre plateforme).

Pour en savoir plus : https://aws.amazon.com/fr/route53/

On va partir du principe que vous n’en avez pas.

La création d’un nom de domaine est payante même avec un compte Free Tier donc libre à vous de poursuivre ce tutoriel ou non.


### 1) Enregistrement d’un domaine
Sur la page d’accueil, aller sur le menu à gauche et cliquer sur “Domaines enregistrés”.

<img src="https://blog.ippon.fr/content/images/2022/04/registered_domains.png">

Vous avez également la possibilité de transférer un domaine. Que ce soit l’un ou l’autre, cela peut prendre plusieurs heures jusqu’à 48h pour que la création/transfert soit finalisée.

Cliquer sur “Enregistrer un domaine”. Vous devriez avoir la page ci-dessous.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/choose_domain_name.png">

Libre à vous de choisir le nom de domaine qui vous convient. Amazon vous affiche les prix pour les différentes extensions.

Le prix correspond à l’enregistrement pour une année.

Vous pouvez continuer jusqu’à la validation de votre nom de domaine. Vous aurez des informations personnelles à renseigner avant de valider votre paiement.


### 2) Zone hébergée
Etant donné que Route 53 est un DNS, on va s’en servir en tant que tel.

Vous pouvez créer une zone hébergée (disponible dans le menu à gauche).

<img src="https://blog.ippon.fr/content/images/2022/04/zone.png">

Mettez bien le nom de domaine que vous venez de créer.

Le type de cette zone hébergée est publique car on veut acheminer le trafic public vers notre site S3.

## Certificate Manager
Ce service vous permet de mettre en service des certificats SSL/TLS et de les gérer. Cette étape est gratuite, que vous ayez un compte Free Tier ou non car on va associer le certificat à un service Amazon : CloudFront.

Vu qu’on va utiliser plusieurs services d’Amazon : S3, CloudFront, Route 53 et Certificate Manager. L’intégration a été simplifiée et mettre en place un certificat SSL/TLS devient presque un jeu d’enfant.

Certificate Manager n’est pas un service global, les certificats sont alloués par région.

Dans notre cas, il est très important de choisir la région us-east-1 (Virginie du Nord).

Tout autre certificat alloué dans une région différente ne sera pas reconnu par CloudFront.

Une fois que vous êtes sur la bonne région, vous pouvez demander un certificat.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/certificate_request.png">

Par défaut, c’est un certificat public. Vous pouvez aller à la page suivante.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/certificate_domain_name.png">

Vous devez renseigner le nom de domaine de votre site. Vous êtes libre d’ajouter des sous-domaines (ex : api.mys3website.fr) si vous en avez la nécessité.

Pour la méthode de validation, choisissez la méthode recommandée : la validation par DNS.

Vu qu’on a déjà créé notre zone hébergée, ce type de validation est parfait dans notre cas.

Vous pouvez valider votre demande. Elle sera au statut  “En attente de validation”.

Pour finaliser votre demande, il vous faut créer un enregistrement DNS dans Route 53.

Pas d’inquiétude, Certificate Manager le fait pour vous.

<img src="https://blog.ippon.fr/content/images/2022/04/records_creation.png">

Il suffit de cliquer sur le bouton “Création d’enregistrement dans Route 53” et valider.

Normalement, au bout d’une minute, le certificat est validé et prêt à être utilisé.

## CloudFront
On se rapproche de la fin. Il vous faut maintenant créer une distribution sur CloudFront.

Cette étape est essentielle car elle va nous permettre d’associer le certificat SSL/TLS que l’on vient de créer via Certificate Manager à notre distribution.

Au-delà même de ce point, il est intéressant d’avoir une distribution. CloudFront est un CDN (Content Delivery Network) et permet de distribuer notre site à travers des points de présence répartis sur les 4 coins du globe (plus de 310 points) et donc réduire grandement les latences que pourraient avoir les utilisateurs de votre site.

Pour en savoir plus : https://aws.amazon.com/fr/cloudfront/

### 1) Création d’une distribution
La première étape est de choisir une origine. Dans notre cas, c’est le bucket S3 que nous avons créé.

Il vous sera proposé automatiquement.

<img src="https://blog.ippon.fr/content/images/2022/04/origin-1.png">

Ensuite, vous pouvez restreindre l’accès de votre bucket à la distribution CloudFront.

Vous ne pourrez plus vous connecter à partir de l’URL générée par S3 (qui est en HTTP) mais uniquement à travers l’URL générée par CloudFront (qui sera en HTTPS).

Ce qui est préférable car plus sécurisé.

<img src="https://blog.ippon.fr/content/images/2022/04/oai.png">

Vous pouvez créer un nouvel OAI et le sélectionner puis demander à ce que la politique de compartiment se mette à jour automatiquement.

Vous pouvez garder les autres options par défaut.


<img src="https://blog.ippon.fr/content/images/2022/04/protocole.png">

Sélectionner la redirection HTTP vers HTTPS pour que votre site soit uniquement accessible en HTTPS.

Enfin, vous pouvez renseigner votre certificat.

<img src="https://blog.ippon.fr/content/images/2022/04/customized_certificate.png">

Votre certificat devrait être récupéré automatiquement si vous l’avez bien créé dans la région us-east-1.

Si vous vous êtes trompé de région dans la création de votre certificat, ce n’est pas grave, vous pouvez le créer à nouveau dans la bonne région et finaliser la création de votre distribution.

Vous pouvez laisser les autres options par défaut et créer votre distribution.


### 2) Dernière étape
Une fois que votre distribution est créée, validée et déployée (ce qui peut prendre quelques minutes), vous aurez une nouvelle URL générée par CloudFront qui sera du type : https://_ID_CLOUDFRONT_.cloudfront.net.
Le certificat devrait être actif. Si vous allez sur votre lien, vous devriez avoir le petit cadenas de sécurité indiquant que la connexion est sécurisée.

<img src="https://blog.ippon.fr/content/images/2022/04/cadenas-1.png">

Désormais, il n’y a plus qu’à associer votre nom de domaine à la distribution CloudFront.

## Route 53
Sur Route 53, aller dans votre zone hébergée et créer un nouvel enregistrement (record).

<img src="https://blog.ippon.fr/content/images/2022/04/record.png">

Vous pouvez laisser le nom vide si vous voulez associer uniquement votre nom de domaine.
Gardez le type d’enregistrement à A.

<img src="https://blog.ippon.fr/content/images/2022/04/record_a.png">

Activer l’option “Alias” car on veut associer l’enregistrement à un service Amazon CloudFront.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/04/alias.png">

Choisissez la distribution que vous venez juste de créer. Elle devrait être récupérée automatiquement. Si ce n’est pas le cas, vous avez dû rater une étape de cet article.
Vous pouvez créer l’enregistrement.

Il peut se passer un peu de temps avant que le DNS propage le changement.


## S3 optionnel
Dernière étape, non obligatoire pour bloquer l’accès de votre bucket S3 au public à l’exception faite de la distribution CloudFront.

Vous devez repartir dans la partie “Autorisations” de votre bucket et modifier l’option “Bloquer tous les accès publics". Activer l’option et confirmer la modification.

Puis dans la stratégie de compartiment, vous allez voir la nouvelle stratégie créée automatiquement par CloudFront.

Vous pouvez supprimer celle qu’on a créée au début de cet article et garder celle de CloudFront.

Et voilà ! Votre site internet est uniquement disponible à travers la distribution CloudFront et en HTTPS.

## Conclusion
Nous avons pu voir 4 services d’AWS et comment les faire interagir entre eux.

Par souci de simplification, je ne suis pas entré en détail sur les services abordés. Chaque fonctionnalité d’un service pourrait être l’objet d’un article.

Si vous voulez approfondir les différents sujets abordés, je vous conseille de lire la documentation d’Amazon qui est précise et explicite.

A partir de ce lien : https://docs.aws.amazon.com/ vous aurez accès à une documentation complète des différents services d’AWS.

source : https://blog.ippon.fr/2022/04/06/votre-site-web-statique-sur-amazon-s3/
