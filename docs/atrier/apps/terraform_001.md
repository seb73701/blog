---
title: Tester ses modules Terraform avec Terratest
description: 
published: true
date: 2023-04-21T10:50:15.905Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:50:11.349Z
---

# Tester ses modules Terraform avec Terratest
Comme c’est un article sérieux, il se doit d’avoir une introduction sérieuse.

Alors même si vous n’avez pas cliqué pour lire ça, un oeil sur l'essor de l’infra as code (IaC) et par conséquent, l’utilisation de Terraform et des modules pour peupler ces beaux environnements.

![imagelikeembed.png](/assets/img/apps/terraform/imagelikeembed.png)

Intérêt pour la recherche "Terraform" dans le monde sur Google en UA normée sur les 5 dernières années.
Un joli graphique pour décrire l’évolution de l’utilisation ou du moins l'intérêt pour Terraform dans le monde depuis 2017. On peut donc déduire que cette croissance de popularité sur Google se traduit par une croissance de la communauté Terraform.

Notons tout de même que Terraform n’est pas dans tous les esprits le premier jour de l’an.

Alors l’envie d’adopter des bonnes pratiques dès le départ et de tester les livrables se fait sentir, premièrement pour faciliter la maintenance et surtout pour avoir plus de certitude sur son infrastructure, cœur de business chez beaucoup.

On parle de code, alors pourquoi ne pas s’inspirer de nos confrères développeurs ? Et écrire des tests unitaires sur son infrastructure as code ?

C’est légèrement plus compliqué que ça alors reprenons depuis le début.

## Chaînes de tests
Il est clairement impossible de tester toute son infrastructure à posteriori, les raisons en sont évidentes, trop de dépendances et un trop gros volume de ressources. Les tests s'effectuent alors plutôt à l'échelle d’un module ou d’un ensemble de modules d’une taille raisonnable.

Une première étape serait d’analyser statiquement son code avec tflint, terraform validate et des règles rego pour contrôler des politiques.

Cependant, par la suite, il est compliqué de tester localement son code vu que 99.9% du code Terraform est un ensemble d’appels API vers les cloud providers. Il faut alors forcément le déployer dans un environnement réel. Le plan à réaliser sera donc :

1. Déploiement dans un environnement sandbox
1. Validation du module en question
1. Destruction des ressources générées

## Utilisation de Terratest
Pour effectuer cette pile, il y a 2 méthodes, tout faire à la main ou automatiser. Tout le monde le voit venir, le faire à la main apporte son lot de problèmes comme avoir des décorrélations de states avec les interruptions de process, des validations de module à la console au click button ou des ressources zombies persistantes à la déconstruction qui peuvent coûter cher. Pour éviter tout ça, il existe des outils pour automatiser le processus. C’est pourquoi nous allons nous pencher dans cet article sur Terratest.

Terratest est une librairie Go qui facilite l'écriture de tests automatisés pour votre infra as code. Elle fournit une variété de fonctions pour notamment tester du code Terraform, des modèles Packer ou des images Docker. Elle permet l’exécution de commandes shell en local ou sur des serveurs via SSH, mais également de travailler avec les API AWS/Azure/GCP ou simplement faire des requêtes HTTP. Bref, Terratest est une belle boîte à outils !

Assez de blabla, ouvrons notre IDE préféré !

Vous pouvez retrouver tout le code lié à cet article [ici](https://github.com/tlorreyte/terratest-demo)

Commençons par initialiser un projet de module Terraform.

![pasted_image_0.png](/assets/img/apps/terraform/pasted_image_0.png)

Proposition d’arborescence
Nous y retrouvons un main.tf, un outputs.tf et un variables.tf comme exemple de module Terraform à tester, un répertoire example avec du Terragrunt pour appeler le module et un répertoire test contenant nos tests.

Dans mon exemple, j’utilise Terragrunt pour appeler mes fichiers Terraform, le choix de ce binaire est par habitude et arbitraire. Celà n’apporte rien de plus à la démonstration, si ce n’est le fait de montrer que c’est possible.

En Go, les fichiers de test doivent être nommées avec le suffixe *_test.go. Cette nomenclature est nécessaire pour que l'exécutable Go reconnaisse nos fichiers de tests.

Dans une optique de test first, rédigeons notre test en premier.

```
package test

import (
"crypto/tls"
"fmt"
"testing"

http_helper "github.com/gruntwork-io/terratest/modules/http-helper"

"github.com/gruntwork-io/terratest/modules/terraform"
)

func TestExample(t *testing.T) {

    // Indique où se trouve les fichiers Terragrunt à executer.
    terraformOptions := terraform.WithDefaultRetryableErrors(t, &terraform.Options{
    TerraformDir:    "../example/terratest",
    TerraformBinary: "terragrunt",
    })

    // Detruis les ressources peu importe le code retour de la fonction (defer)
    defer terraform.Destroy(t, terraformOptions)

    // Execute terraform init et terraform apply et retorune une erreur si ça fail
    terraform.InitAndApply(t, terraformOptions)

    validate(t, terraformOptions)
}

func validate(t *testing.T, opts *terraform.Options) {

}
```

Quelques pré-requis liés au go, bien sûr il faut l’installer, puis il faut configurer les dépendances :

```
cd test
go mod init "<module_name>"
go mod tidy
```

Où <module_name> est le nom de votre module, généralement : github.com/<username>/<repo_name>

La fonction TestExample va nous permettre de mettre en place le test.

Premièrement, il faut indiquer où se situent les fichiers terragrunt à appeler avec le paramètre TerraformDir et les binaires à exécuter avec TerraformBinary. Dans l’exemple, l’utilisation de Terragrunt est par habitude mais il est également possible de lancer directement les commandes Terraform.

Puis, il faut indiquer au script d'exécuter le destroy peu importe la sortie de la fonction avec defer, cela permet d'éviter les ressources zombies si la suite plante.

Ajoutons le init et apply.

Il ne nous reste plus qu'à écrire la fonction validate qui contiendra la logique de test (Then).

Vous pouvez dès maintenant mettre en place vos variables d’environnements AWS_ACCESS_KEY_ID, AWS_SECRET_ACCES_KEY ou AWS_PROFILE et lancer les tests : cd test && go test

Il faudrait tout de même savoir comment tester et suivant la ressource en question, plusieurs procédés viennent en tête. Il est possible d’effectuer des requêtes HTTP pour tester un service web (Lambda, API Gateway…), des commandes ssh dans le cas d’une instance, des commandes via les APIs des cloud providers (awscli, gcp…) lorsqu’il s’agit de services managés ou des requêtes SQL pour des bases de données par exemple.

Dans cet article, nous allons prendre l’exemple d’un S3 public hébergeant des fichiers statiques pour une UI. Nous allons donc récupérer le point d’entrée de notre S3 en output de la ressource Terraform et vérifier qu’il est bien ouvert sur internet et accessible depuis notre poste.

  
```  
func validate(t *testing.T, opts *terraform.Options) {
    // Récupère l'output Terraform
    url := fmt.Sprintf("http://%s", terraform.Output(t, opts, "bucket_endpoint"))

    // Setup une configuration TLS avec une structure vide
    tlsConfig := tls.Config{}

    // Vérifie que la requête retourne bien un code 200
    http_helper.HttpGet(t, url, &tlsConfig)
}
```

Dans cette fonction, l’output Terraform bucket_endpoint contenant le point d’entrée de notre S3 est récupéré, par défaut de la forme bucket_name.s3-website-eu-west-1.com.

La requête est effectuée à l’aide de la librairie de fonctions http_helper fournie par Terratest. Le choix de cette librairie est arbitraire.

Le test renvoie bien un fail, lié à la sortie vide de Terraform. Notons tout de même le temps pris par le test, 10,5 secondes. Celà peut paraître assez long pour un test vide, cependant c’est le temps que prend Terraform pour s’initialiser, car il télécharge le module (ici en local) et le provider.

![pasted_image_0(1).png](/assets/img/apps/terraform/pasted_image_0(1).png)
  
Fail du test. Pas de ressource déployée.
Décrivons désormais notre bucket. Le choix d’une ressource assez simple est voulue, pour s’affranchir de la complexité du développement en Terraform. Comme le module est développé sur le provider AWS > 4.0.0 marquant beaucoup de changement sur les ressources S3, nous nous retrouvons avec 11 ressources en définitive.

![image.png](/assets/img/apps/terraform/image.png)
  
Ressources décrites
Le module contient un bucket source, un bucket de logs, des ACL pour chaque bucket, des ressources de configuration pour les CORS, les logs, les policies, le versionning et le service pour le bucket source, et pour finir deux ressources Terraform copiant dans le bucket deux pages HTML pour l’index et l’erreur.

Comme évoqué précédemment, à partir de maintenant, nous allons déployer de réelles ressources. Celà peut engendrer des coûts, il est recommandé d'exécuter les tests dans un environnement sandbox avec des nuke régulières (cf AWS Nuke par exemple).

Les tests se lancent donc et ne renvoient plus d’erreur, mission accomplie.
  
![pasted_image_0(4).png](/assets/img/apps/terraform/pasted_image_0(4).png)
  
Logs de Terratest
  
## Integration en CI
Cet outil exploite tout son potentiel intégré en CI dans le workflow de développement. Sur chaque pull request (ou merge request), la qualité du module sera vérifiée, qu'il s'agisse d'un service en particulier ou d'une stack nécessaire au fonctionnement d'un projet.

En effet, la CI va venir vérifier que les ressources soient bien instanciées et détruites à la volée. Indirectement, si chaque module respecte ce principe, la qualité de l'ensemble de l'infrastructure n'en sera que meilleure.
  
## Coût de maintenance
Un autre avantage, et non des moindres, de tester son code Terraform (peu importe l'outil), est que le coût de maintenance n'est pas très élevé. Les sorties ou les attentes d'un module n'évoluant pas beaucoup, les tests ne changeront pas énormément dans le cycle de vie d'un module.

Reprenons l'exemple de notre module S3. Nous souhaitons désormais que nos fichiers soient chiffrés à l'aide d'une clé stockée sur KMS. L'évolution revient simplement à ajouter une ligne vérifiant que l'option est bien activée une fois la ressource créée.

Dans un autre cas, si une ressource supplémentaire est ajoutée, il suffira de vérifier ses interactions avec les autres, qu'une lambda ait bien les droits d'accès à une table DynamoDB par exemple.

A l’image des tests en développement, il n’en reste pas moins primordial de voir leur intérêt sur le long terme. En effet, leur but premier est de limiter, voire complètement éliminer, la régression des modules. L’investissement mis dans leur développement est de ce fait largement rentable.

Dans cet article, nous avons testé un module. Il est également possible de tester un ensemble de modules mais dans ce cas, plus l’ensemble de modules est volumineux, plus le coût de maintenance des tests est élevé. Le parallèle avec la pyramide des tests peut-être fait.

## Pour aller plus loin
Pour aller plus loin dans l’univers des tests et du clean coding, je ne peux que vous recommander [ce livre blanc](https://fr.ippon.tech/publications/livre-blanc/clean-coding). C’est un excellent point d’entrée pour mieux comprendre les enjeux du clean coding, je vous laisse le soin de faire les parallèles avec l’Infrastructure as Code.

N’hésitez pas également à consulter toute la documentation de [Terratest](https://github.com/gruntwork-io/terratest) pour voir les multiples cas d’usage qu’il couvre et à contribuer au projet !