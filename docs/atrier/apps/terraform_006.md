---
title: Comment et pourquoi industrialiser la manipulation des fichiers d'état de son infrastructure Terraform
description: 
published: true
date: 2023-06-21T21:32:43.619Z
tags: 
editor: markdown
dateCreated: 2023-06-21T21:32:43.619Z
---

# Comment et pourquoi industrialiser la manipulation des fichiers d'état de son infrastructure Terraform ?

Comme on a pu le voir dans [mon précédent article](https://blog.ippon.fr/2022/04/04/terraform-dans-tous-ses-etats-1-2/), Terraform possède une notion d'état qui semble assez importante à comprendre et à savoir manipuler. En effet, lorsque vous avez une infrastructure en production, ces manipulations deviennent inévitables pour pouvoir refactoriser son code sans provoquer des pertes de données ou d'interruptions de services. Ces manipulations sont assez propices aux erreurs et demandent de redoubler d'efforts pour bien vérifier ses plans Terraform.

Pour autant, ces opérations n'ont pas à être une corvée et des outils permettant de les industrialiser existent désormais. Nous en verrons un puis plongerons dans la solution apportée par Hashicorp depuis la version 1.1 de Terraform (version d'actualité à l'heure où j'écris l'article). Il est possible que cette solution évolue au fil des versions.

## Préambule
Comme dans l'article précédent, cet article utilise la notion de [module Terraform](https://www.terraform.io/language/modules?ref=blog.ippon.fr) ainsi que le [workflow habituel de Terraform](https://www.terraform.io/intro/core-workflow?ref=blog.ippon.fr) (terraform plan et terraform apply).

Vous pouvez retrouver les exemples de code de cet article ici. Ils utilisent tous le cloud provider AWS, il vous faudra donc un compte pour les effectuer avec moi. Ceux qui souhaitent suivre ces exemples peuvent trouver les différentes instructions dans le README du dépôt.

## Avec des outils externes : présentation de tfmigrate
[tfmigrate](https://github.com/minamijoyo/tfmigrate?ref=blog.ippon.fr) est l'un des outils qui a pour vocation de ramener la gestion du fichier d’état dans le dépôt git. Concrètement, le fonctionnement de cet outil est simple : on indique dans un fichier (au format HCL) les différentes migrations (commandes state rm / state mv / import) à effectuer. Ces fichiers sont passés à la commande tfmigrate qui fonctionne sous le même workflow que Terraform:

- plan : pour vérifier si la migration pourra correctement s'effectuer ;
- apply : pour exécuter réellement la migration.

Afin de tester l'outil, nous allons nous baser sur notre exemple d'extraction de plusieurs ressources en un module (utilisez le dossier part2/tfmigrate/start du dépôt d'exemples).

> ⚠ ️ Pour vos tests, vous devrez configurer un remote backend, car tfmigrate ne supporte pas le backend de type local. Il vous faudra donc créer un bucket pour stocker vos fichiers d’état Terraform.

```json
data "aws_ami" "ubuntu" {
  ...
}
 
resource "aws_instance" "john_doe" {
  ...
}
 
resource "aws_instance" "foo_bar" {
  ...
}
```

extrait du code contenu dans `part2/tfmigrate/start`


On l'applique avec terraform apply puis on extrait les ressources dans un module “workstation”.

Après ces modifications (dossier part2/tfmigrate/step1), le prochain terraform plan va nous présenter des changements (destruction et recréation) sur les instances ec2. On se rappelle que la résolution précédemment choisie (cf. [mon article précédent](https://blog.ippon.fr/2022/04/04/terraform-dans-tous-ses-etats-1-2/)) était d'utiliser la commande terraform state mv sur nos deux instances. Imaginons un instant qu'on ait à effectuer ce refactoring sur plusieurs environnements qui utilisent le même code terraform pour se déployer. On aurait à effectuer la même manipulation d'état sur tous ces environnements. Pour autant, elle pourrait ne pas se faire le même jour dans le cas où on souhaite utiliser les environnements pour tester les développements effectués. Il faudra donc se rappeler qu'elles sont à faire lors des montées de version.

Grâce à l'utilisation de tfmigrate, nous allons pouvoir définir les étapes de migration à effectuer et les exécuter simplement et de manière automatisée via une CI/CD.

On traduit tout d'abord nos deux commandes state mv dans la structure de données comprise par tfmigrate, cela nous donne le fichier migrate-to-workstation-module.hcl dont le contenu est le suivant :

```json
migration "state" "workstation_module_introduction" {
  actions = [
    "mv aws_instance.foo_bar module.foo_bar.aws_instance.this",
    "mv aws_instance.john_doe module.john_doe.aws_instance.this",
  ]
}
```

`part2/tfmigrate/step1/migrate-to-workstation-module.hcl`

On peut ensuite exécuter tfmigrate plan migrate-to-workstation-module.hcl, vérifier qu'il n'y a pas d'erreur, puis exécuter tfmigrate apply migrate-to-workstation-module.hcl pour effectuer les migrations. Un terraform plan permet finalement d'être sûr que la migration s'est correctement effectuée en nous montrant qu'il n'y a rien a faire.

Auparavant, nous avons spécifié le fichier de migration sur la ligne de commande. On ne procèderait pas comme cela via un outil de CI/CD. De plus, quand bien même nous ajouterions la capacité d'exécuter tous les scripts de migrations à notre CI/CD, il faudrait qu'elle sache lesquels ont déjà été exécutés. C'est pour ces deux raisons que tfmigrate intègre une fonctionnalité d'historique.

Pour configurer la gestion de l'historique de tfmigrate, il faut créer un fichier .tfmigrate.hcl à la racine du dossier contenant notre configuration terraform.

```json
tfmigrate {
  # où trouver nos script de migration
  migration_dir = "./migrations"
  history {
    # où stocker l'historique d'exécution des migrations
    storage "s3" {
      # je réutilise mon bucket de tf states par simplicité, mais on
      # peut très bien utiliser un bucket différent pour tfmigrate.
      bucket = "iqcyuysxds-ameynard-tfstates"
      key    = "tfmigrate/history.json"
    }
  }
}
```

disponible dans le dossier `part2/tfmigrate/end`

Une fois ce fichier de configuration placé, vous pourrez simplement placer vos fichiers de migration dans le dossier migrations et laisser tfmigrate gérer les scripts qui doivent être appliqués en fonction de l'historique. Il n'est d'ailleurs plus nécessaire de spécifier de script de migration aux commandes tfmigrate plan et tfmigrate apply.

Globalement, tfmigrate est l'implémentation d'un outil de migration de base de données pour Terraform en partant du principe que le fichier d'état est la base de données de notre infrastructure. Cela nous permet ainsi de réintégrer nos opérations manuelles dans git sous forme de script facilement lisible et exécutable par notre CI/CD lors de nos montées de versions ou passage en environnements supérieurs. Attention tout de même, l'outil est très bien pour les utilisations basiques de Terraform, mais peut trouver ses limites assez rapidement. Lors de mes tests j'ai pu notamment voir que l'outil supporte mal les workspaces Terraform. De plus, la gestion de l'historique est seulement disponible via AWS S3 ou en mode local. Cependant, l'outil est (comme beaucoup) open source et en Go, donc n’hésitez pas [à écrire une PR si vous avez besoin d'une fonctionnalité en particulier](https://github.com/minamijoyo/tfmigrate?ref=blog.ippon.fr) 🙂.

## Depuis Terraform 1.1: Avec le nouveau bloc moved de Terraform
Cela fait longtemps qu’HashiCorp est conscient des étapes manuelles induites par la gestion du fichier d'état de Terraform. Depuis la version 1.1, nous pouvons (enfin) indiquer les migrations à effectuer directement dans notre code Terraform grâce au block moved. Nous allons voir comment celui-ci s'utilise. Repartons de notre code avec nos deux ressources aws_instance (dossier part2/moved/start du dépôt d'exemples), appliquons avec un terraform apply et modifions le code pour utiliser notre nouveau module wokstation.

Cela nous produit le code suivant :

```json
terraform {
 required_providers {
   aws = {
     source  = "hashicorp/aws"
     version = "~> 3.0"
   }
 }
 backend "s3" {
   key    = "terraform-state-article/moved-block.tfstate"
 }
}
 
provider "aws" {
  region = "eu-west-1"
}
 
module "workstation" {
 for_each = toset(["John Doe", "Foo Bar"])
 source = "../../../modules/workstation"
 
 user_fullname = each.value
}
```

disponible dans le dossier `part2/moved/end` (commentez les blocs moved)

Si on effectue un terraform plan, on retrouve les deux destructions/recréations dûes au changement de chemin dans notre code Terraform. Grâce au block moved, nous allons pouvoir décrire les deux déplacements à effectuer. Ce block prend deux paramètres : from et to indiquant respectivement l'ancien chemin et le nouveau. Dans notre exemple, voici donc les deux blocs moved que nous devrons rajouter :

```json
moved {
 from = aws_instance.foo_bar
 to = module.workstation["Foo Bar"].aws_instance.this
}
 
moved {
 from = aws_instance.john_doe
 to = module.workstation["John Doe"].aws_instance.this
}
```

disponible dans le dossier `part2/moved/end` (décommentez ces blocs si besoin)

Une fois ceci fait, lançons un terraform plan. On s'attend à avoir un plan vide.

![unnamed.png](/assets/img/apps/terraform/unnamed.png)

output d'un terraform plan apres avoir ajouté les blocs moved dans le code

On voit bien, via le résumé du plan, qu'il n'y a rien à faire (0 ajout, 0 changement et 0 destruction). Terraform nous indique tout de même que deux ressources ont été déplacées. Ce comportement nous permet de comprendre que cette information sera inscrite dans le tfstate.

Bien qu'HashiCorp ait pris conscience de l'importance d'ajouter cette fonctionnalité à Terraform, celle-ci possède encore quelques limitations. Admettons par exemple que vous vouliez passer d'un module privé que vous développez à un module publique provenant d'un registry. Dans notre exemple, on pourrait chercher à remplacer notre module workstation par le module ec2-instance que l'on peut trouver sur le registry officiel. Après quelques modifications faites sur notre code (non incluse ici, cf dossier part2/moved_limits/end), on se retrouve avec l'erreur suivante :

![2-2.png](/assets/img/apps/terraform/2-2.png)

erreur lors de l'utilisation de blocs moved pour déplacer des resources vers un module publique

Pour gérer ce cas, vous allez devoir utiliser la méthode décrite dans la première partie de cet article, c'est-à-dire utiliser la commande terraform state mv.

Pour conclure, ce nouveau bloc devrait vous permettre d'éviter un grand nombre d'étapes de gestion manuelles et ainsi faciliter le refactoring de votre code Terraform. Pour l'utiliser, vous devrez être passé à la version 1.1 de Terraform. Bien que la fonctionnalité possède une limitation franche, cela ne devrait pas être trop problématique, le passage d'un module privé a un module public étant une opération assez rare. Finalement, le bloc moved n'implémente pas les autres commandes de gestion du fichier d’état que sont terraform state rm et terraform import. Si vous avez souvent besoin de ces fonctionnalités, l'outil tfmigrate pourrait être un meilleur choix pour vous.

## Conclusion
L'industrialisation des manipulations du fichier d'état via un outil intégré à votre CI ou directement dans votre code Terraform (depuis la v1.1) est importante pour éviter les oublis et erreurs de manipulation dans vos différents environnements. L'automatisation de ces opérations sur le fichier d'état Terraform devrait aussi avoir une sorte d'effet rebond et vous permettre de plus souvent refactoriser votre code.

Finalement, Terraform accorde une forte confiance aux développeurs et relecteurs des plans. Cependant, il se peut que le relecteur aille trop vite et ne détecte pas qu'une migration sera nécessaire à l’apply. Les outils proposés dans cet article ne fournissent pas de solution à ce problème. Afin d'éviter ce genre de cas, vous pouvez mettre en place des protections via l'utilisation du lifecycle prevent_destroy ou via l'outil Terraform Sentinel (nécessite Terraform Cloud).

Si vous ne voyez toujours pas dans quels cas la manipulation d'état peut être nécessaire n'hésitez pas [à lire (ou relire) mon précédent article](https://blog.ippon.fr/2022/04/04/terraform-dans-tous-ses-etats-1-2/). Vous pouvez aussi jeter un œil à l'article de Micheal sur [comment découper votre code d'infra en plusieurs parties, critique et non critique](https://blog.ippon.fr/2020/04/27/gestion-de-ressources-critiques-avec-terraform-sur-aws/). Il présente un autre cas où la manipulation du fichier d'état Terrform peut être nécessaire.

source : https://blog.ippon.fr/