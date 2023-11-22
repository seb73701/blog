---
title: Créer du Code As Infra avec Terraformer
description: 
published: true
date: 2023-06-20T20:10:22.744Z
tags: 
editor: markdown
dateCreated: 2023-06-20T20:10:22.744Z
---

# Créer du Code As Infra avec Terraformer
![1280px-terraform_logo.svg.png](/assets/img/apps/terraform/1280px-terraform_logo.svg.png)

## Introduction
Terraform est aujourd’hui un des leader dans l’infrastructure as code. En revanche, malgré sa documentation complète et riche, cela reste parfois compliqué et intimidant de se lancer dans du développement de code ou alors dans une migration vers de l’infra as code quand on a déjà une infrastructure complexe existante. De plus, le HCL (HashiCorp Language) n’est pas forcément intuitif et similaire à d’autres langages au premier abord.

Plus aucune peur aujourd’hui, il existe des outils tel que [Terraformer](https://github.com/GoogleCloudPlatform/terraformer) pour pouvoir générer du code depuis une infrastructure existante !

## Présentation
Terraformer est un outil CLI écrit en Go. Comme expliqué en introduction, il est utilisé pour générer des fichiers Terraform à partir d’une infrastructure existante. Il a été développé par Waze SRE et est un outil open-source et utilisable sur de nombreuses plateformes et cloud providers (Kubernetes, Datadog, AWS, GCP …)

Dans cet article, nous allons donc déployer quelques services exclusivement AWS via la console et les importer en Terraform via cet outil.

Pour pouvoir réaliser un “import” via Terraformer, il suffit donc de détenir une infrastructure sur un Cloud Provider et de réaliser la commande d’import en ligne de commandes qui va permettre de générer des fichiers Terraform correspondant à notre architecture déjà existante.

## Utilisation
### Pré-requis

- Terraform doit être installé sur votre machine : https://learn.hashicorp.com/tutorials/terraform/install-cli
- Terraformer doit être installé sur votre machine https://github.com/GoogleCloudPlatform/terraformer#installation

### Premier développement
Pour notre premier développement, nous allons déployer une instance EC2 assez classique que l’on peut souvent rencontrer, l’infrastructure sera donc la suivante :

- Une instance EC2
- Un security group acceptant les ports 443 (HTTPS) depuis tout internet et le port 22 (SSH) depuis une IP précise
- Un rôle IAM attaché à l’EC2 permettant de se connecter en SSM à l’instance

Pour faciliter l’import via Terraformer par la suite, toutes les ressources seront taguées via le tag suivant : Terraformer: demo-ippon

Pour cela, 2 manières différentes :

- La première, que je ne recommande pas, est d’importer via le flag -ressources="*" :

```
terraformer import aws --resources="*" --filter="Name=tags.Terraformer;Value=demo-ippon" --regions="eu-west-3" --profile=default
```


Le problème de cette méthode est qu’on se retrouve avec un grand nombre de ressources vides dans notre dossier généré “generated/aws”.

- La deuxième, plus précise et recommandée, est donc de filtrer les ressources souhaitées :

```
terraformer import aws --resources=ec2_instance,iam,sg --filter="Name=tags.Terraformer;Value=demo-ippon" --regions="eu-west-3" --profile=default
```


On se retrouve alors avec un dossier pour chaque ressource dans notre dossier “generated/aws”.




> Note: Si l’on avait pas spécifié la région dans les flags, nous aurions des sous-dossiers correspondant aux régions de nos services

Dans le cas d’une instance EC2, on remarque que les fichiers terraform sont créés de manière assez "brute". Comme on peut le voir, rien n’est variabilisé :

![capture_d’écran_le_2022-09-15_à_12.37.14.png](/assets/img/apps/terraform/capture_d’écran_le_2022-09-15_à_12.37.14.png)


De plus, dans le cas du security group, chaque entrée consiste en un bloc, il n’y a aucune notion de for_each ou de dynamic qui sont pratiqués dans Terraform. De plus, toutes les variables pour la ressource aws_instance sont utilisées alors qu’elles ne sont pas forcément obligatoires ce qui fait que l’on se retrouve avec des fichiers très verbeux mais fonctionnels !

## Deuxième développement
Pour ce deuxième développement, nous allons déployer un bucket S3 hébergeant un site web statique, l’infrastructure sera donc la suivante :

- Un bucket S3 public
- Un `index.html` téléversé dans le bucket
- Une bucket policy autorisant le `GetObject`

> NB: Nous avons encore une fois mis le tag Terraformer: demo-ippon

Ne perdons pas de temps, et importons cette infrastructure !

```
terraformer import aws --resources=s3 --filter="Name=tags.Terraformer;Value=demo-ippon" --regions="eu-west-3" --profile=default
```


On obtient bien notre code Terraform pour ce bucket s3. En revanche, il se trouve qu’on obtient une version dépréciée de la ressource s3. En effet, depuis la version 4.0 de Terraform, la ressource S3 bucket (et ses blocs de configuration) ont  été splitté en une multitude de ressources comme aws_s3_bucket_server_side_encryption_configuration, aws_s3_bucket_policy etc … Dans cette importation on a qu’une seule ressource S3 comme on pouvait avoir antérieurement. De plus, le fichier index.html que l’on a téléversé ne se retrouve pas en tant que ressource et on ne nous laisse pas vraiment le choix de le faire ou non.

## Pour aller plus loin
Pour pouvoir tester cet utilitaire au maximum et pouvoir en tirer une réelle conclusion, je conseillerai de tester cet outil sur plusieurs services mono / multi régions et aussi sous différents Cloud Providers tel que Google pour lequel il a été initialement créé et pour lequel il est le plus mis à jour et développé. Il est fort possible que cet outil puisse s’avérer très puissant selon les cas d’usage.

## Conclusion et limites
Terraformer est un outil très utile pour débuter ou pour valider ses choix en termes de développement d'infrastructure as code. En revanche, il n’est pas conseillé de l’utiliser pour faire de réelles migrations ou déploiement d'infrastructures de production. En effet, il peut être utile pour partir d’une base mais le manque de ressources comme on l’a vu pour S3 et la verbosité qu’il peut apporter sur certaines ressources comme EC2 peuvent être un point bloquant. En revanche, l’utilitaire étant développé initialement pour Google Cloud, il se peut qu’il soit plus efficace avec ce dernier. En jetant rapidement un coup d'œil au code pour AWS, on remarque des choses manquantes.

Pour terminer, je dirais donc que Terraformer est un outil que j’aurais bien aimé connaître quand j’ai débuté Terraform et qu’il peut être utile lors de POCs ou de déploiement de nouveaux services que l’on ne connaît pas mais qu’il ne faut pas se fier totalement à lui.

Vous trouverez les ressources pris en charge pour AWS à ce lien :

https://github.com/GoogleCloudPlatform/terraformer/blob/master/docs/aws.md

Source : https://blog.ippon.fr/