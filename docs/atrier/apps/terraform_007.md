---
title: Gestion de ressources critiques avec Terraform sur AWS
description: 
published: true
date: 2023-06-21T21:43:28.948Z
tags: 
editor: markdown
dateCreated: 2023-06-21T21:43:28.948Z
---

# Gestion de ressources critiques avec Terraform sur AWS

![terra2.png](/assets/img/apps/terraform/terra2.png)

## Qu’est ce que Terraform ?
Terraform est un logiciel développé par HashiCorp permettant de faire de l’Infrastructure As Code (IaC), c’est-à-dire écrire du code pour gérer son infrastructure. En ayant son infrastructure sous cette forme, on peut la détruire, la reconstruire et même la reproduire dans divers environnements à l’identique.

Ainsi, l'IaC permet d'introduire la possibilité de « tester son infrastructure ». Ce n’est pas un langage de programmation à proprement parler mais plutôt un outil de gestion de ressources qui possède son propre langage déclaratif, HCL. Le code Terraform ainsi généré pourra être versionné avec notre outil de version control préféré, Git par exemple. Cela nous permettra d'obtenir un historique de l'état de notre architecture.

De plus, Terraform permet également de faire des modules, qui peuvent être vu comme une librairie que l'on peut versionner et ré-utiliser.

## Contexte

Les ressources gérées par Terraform, qui peuvent être une machine virtuelle ou une base de données par exemple, sont stockées dans un fichier appelé **TFstate**. Il faut le voir comme une liste d'états de son infrastructure qui va être mise à jour au fur et à mesure des changements qu’on y opère. Ce fichier est créé automatiquement par Terraform. Il peut être stocké en local ou dans un [backend](https://www.terraform.io/docs/backends/types/index.html?ref=blog.ippon.fr) comme un bucket s3 par exemple. Hormis pour des tests en local, la seconde option est la plus répandue et conseillée. Elle facilitera notamment le travail en collaboration et permettra un chiffrement du TFstate at rest.

Prenons le cas d'un projet qui possède deux ressources (un bucket s3 et un user IAM) dans son infrastructure, stockées dans un seul TFstate. Partons du principe que le bucket s3 contient des données sensibles et qu'il est interdit de le supprimer. Le projet impose de le mettre dans un second TFstate afin de le protéger et d'éviter de le casser. En somme, on aura donc non plus un mais deux TFstates. A savoir, un qui pourra être détruit et reconstruit sans problème et un autre auquel nous ne toucherons pas.

Il est important de noter que pour réaliser un transfert de ressources d’un TFstate vers un autre cela implique généralement une suppression de ressources. Et nous allons justement voir comment gérer nos ressources critiques sans avoir à les supprimer.




Par ressources critiques, j’entends celles que l'on souhaite impérativement garder intactes. Mais cela dépend du niveau de criticité des données que chacun a vis-à-vis de ses ressources, cela peut être par exemple une table dynamoDB, ou dans notre cas un bucket s3.


Dans la prochaine partie, nous allons mettre en place les prérequis nécessaires pour réaliser notre découpage de TFstate.

## Prérequis
Ici, nous allons utiliser Terraform v0.12 pour déployer nos ressources sur AWS.

En amont, nous déployons les ressources via Terraform. Le code est assez simple. Il est composé de 3 répertoires comme ci-dessous.



Ci-dessous vous trouverez le contenu des 3 fichiers principaux.

À noter que dans le fichier bucket.tf ci-dessous on rajoute l’argument “prevent_destroy” afin que Terraform ne détruise pas la ressource en cas d’actions involontaires.

```json
 # bucket.tf

resource "aws_s3_bucket" "test" {
  bucket = "dzeoijfoizejfnoze-critical-bucket"
  acl    = "private"
  
  tags = {
    Name        = "bucket"
    Environment = "Dev"
  }

  lifecycle {
    prevent_destroy = true
  }
}
```

```json
# provider.tf

provider "aws" {
  version = "~> 2.0"
  region  = "eu-west-1"
}

terraform {
  backend "s3" {
    bucket = "dzeoijfoizejfnoze-my-tfstates"
    key    = "bucket"
    region = "eu-west-1"
  }
}
```

```json
# iam.tf

resource "aws_iam_user" "user" {
  name = "non-critical-user"
}
```

Le répertoire “mica-old” est le répertoire à partir duquel on va construire ce que l’on référencera comme l’ancien TFstate. Pour cela, on va déployer notre infrastructure, à savoir un bucket s3 et un user IAM. On lance donc les deux commandes suivantes :

```shell
mica-old$ terraform init
mica-old$ terraform apply
```


## Étapes du transfert
L'infrastructure étant mise en place, nous pouvons maintenant procéder à la séparation des ressources en 2 TFstates distincts.

- **Sauvegarde**

Avant tout, il est conseillé de faire une sauvegarde de l’ancien TFstate que nous souhaitons découper. À noter que nous pouvons également activer le versionning sur le bucket pour garder un historique des versions du fichier et ainsi revenir à une version ultérieure.

Pour faire la sauvegarde, on utilise la commande “aws s3 cp” comme ceci :

```shell
aws s3 cp s3://FOLDER/terraform-old.tfstate s3://FOLDER/terraform-old-save.tfstate
```

- **Import**

Ensuite, nous allons importer les ressources critiques dans un premier TFstate. En effet, il est possible qu’une ressource soit présente dans plusieurs TFstates et nous allons justement tirer profit de cet avantage.

Cela se fait à l’aide de la commande suivante “terraform import” qui prend deux paramètres :

```shell
terraform import RESOURCE_NAME RESOURCE_ID
```

Nous connaissons `RESOURCE_NAME`, en effet nous l’avions noté tout à l’heure, il s’agit de “aws_s3_bucket.test”. Quant à  `RESOURCE_ID`, il correspond à l’ID du bucket que l’on a défini plus haut dans notre fichier bucket.tf “dzeoijfoizejfnoze-critical-bucket”. Il ne nous reste plus qu'à remplacer ces valeurs. On obtient donc la commande “terraform import aws_s3_bucket.test dzeoijfoizejfnoze-critical-bucket” que l’on lance depuis le répertoire “mica-bucket” afin de réaliser l’import.


Avec la commande “terraform state list” on voit que notre ressource critique, le bucket S3, est maintenant gérée par le TFstate du répertoire “mica-bucket”.


Dans cet exemple, ceci a été fait manuellement, mais dans le cas où il y a de nombreuses ressources ainsi que de nombreux environnements à gérer, il est préférable d’utiliser des pipelines avec Gitlab CI ou autres pour automatiser le processus.



Voici un exemple pour le job d’import dans Gitlab CI :

```yaml
# .gitlab-ci.yml

stages:
  - manage_tfstates

import_job:
  script:
    - echo "IMPORT"
    - cd mica-bucket
    - terraform import aws_s3_bucket.test dzeoijfoizejfnoze-critical-bucket
  stage: manage_tfstates
  when: manual
```


- **Remove**

Maintenant que la ressource est importée dans “mica-bucket”, nous allons la retirer du TFstate créé à partir du répertoire “mica-old” pour éviter toute incohérence en cas de modification de la ressource. Et ce avec la commande “terraform state rm aws_s3_bucket.test”.


- **Destroy**

Il faut noter que dans cette partie nous allons détruire une ressource. Une fois cela fait, il sera impossible de revenir en arrière même avec la sauvegarde que nous avions effectuée à l’étape n°1. Et ce car le TFstate n’est qu’un état représentatif de notre infrastructure.

Dans cette étape, nous allons détruire les ressources restantes, c'est-à-dire celles qui ne sont pas critiques. Dans notre cas, étant donné que le bucket n'est plus présent dans le TFstate, nous allons détruire l'user IAM grâce à la commande “terraform destroy” depuis le répertoire “mica-old”.


- **Apply**

Après avoir fait cela, nous pouvons recréer les ressources non critiques dans un second TFstate. Pour ce faire, on entre dans le répertoire “mica-iam” et on lance la commande “terraform apply”.



Ceci a donc eu pour effet de créer le user IAM qui sera stocké dans le TFstate non critique.

## Conclusion
Pour rappel un TFstate est un fichier décrivant l'état de notre infrastructure Terraform. Il est donc vivement conseillé de stocker ce fichier dans un bucket s3 versionné et chiffré.

Nous sommes donc partis d’un TFstate contenant deux ressources dont une essentielle. Ce fichier est désormais vide et deux nouveaux ont été créés contenant respectivement :

- les ressources critiques (bucket s3)
- les ressources non critiques (user IAM)


Ceci a été réalisé en plusieurs étapes, dont l’import des ressources critiques dans un autre TFstate. Cette étape est vraiment clé pour ne pas avoir à supprimer de ressources lors d’un transfert de TFstate.  

Ainsi, nous pouvons effectuer des opérations sur les ressources gérées par le TFstate non essentiel, comme par exemple les détruire sans avoir à toucher à nos ressources essentielles qui elles, sont stockées dans un autre TFstate.

Le code Terraform de cet article a été créé en utilisant la version 0.12 de Terraform. Notons toutefois qu’il existe un outil permettant d’installer une version spécifique du logiciel, à savoir tfenv. Ci-dessous les commandes pour l’installer et passer à la version 0.12.24 sous macOS.

```shell
brew install tfenv
tfenv install 0.12.24
```
source : https://blog.ippon.fr/