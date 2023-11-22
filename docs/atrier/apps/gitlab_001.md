---
title: Un environnement par branche avec GitLab
description: 
published: true
date: 2023-04-21T10:46:30.192Z
tags: git, gitlab
editor: markdown
dateCreated: 2023-04-21T10:46:26.391Z
---

## Un environnement par branche avec GitLab

Vidéo (1ère partie) : https://youtu.be/74z08KSXha4
Vidéo (2ème partie) : https://youtu.be/w4A9XpFpyNg

Dans la "guerre" des outils de CI/CD, GitLab a certainement gagné les dernières batailles ! Parmi les possibilités que nous apprécions tout particulièrement : déployer des "environnements" dédiés aux branches git pour leur donner un cycle de vie propre.

> *L’article ne sera pas centré sur les "environnements" au sens applicatif du terme (là où on déploie des applications, e.g. un Cloud provider) mais bien sur les "environnements" GitLab.*

## Petit rappel sur les pipelines
La première étape pour créer un *pipeline* GitLab dans un projet est d’ajouter un fichier `.gitlab-ci.yml` à la racine (il est possible d’importer d’autres fichiers de configuration GitLab [dans le cadre d’un monorepo](https://blog.ippon.fr/2020/07/31/construire-la-ci-dun-monorepo-les-parent-child-pipelines-de-gitlab-ci/)).

Un *pipeline* GitLab CI est composé de *jobs* ordonnés par des *stages*. Chaque *job* permet le lancement de scripts dans lesquels nous allons pouvoir tester / builder / déployer le code de nos branches git. La grande idée, maintenant très répandue, de GitLab CI était de lancer les jobs dans des images docker. On a donc accès à tous les outils du marché sans installation ni maintenance complexe.

## Laisser GitLab gérer vos branches git
Si au quotidien, vous avez l’habitude de faire des :

```
git checkout -b myFeature
```


Nous vous conseillons de laisser GitLab le faire. Nous allons donc commencer par la création d’une *issue* dans le *board* du projet :

![create_issue.png](/assets/img/apps/gitlab/create_issue.png)

Ensuite, il suffit d’ouvrir cette issue et de créer une *Merge Request* (MR) associée :

![create_mr.png](/assets/img/apps/gitlab/create_mr.png)

Nous avons alors une *Merge Request* en *draft* sur laquelle faire nos contributions :

![mr_created.png](/assets/img/apps/gitlab/mr_created.png)

Il faut maintenant récupérer les branches distantes en local :

```
git fetch -p
```

> *-p pour prune -> supprimer en local les branches supprimées à distance.*

Puis aller sur notre branche :

```
git checkout 2-create-first-gitlab-pipeline-in-project
```

Le point intéressant est bien là : la branche commence par un **numéro presque unique** (l’ID du ticket GitLab précédemment créé) que l'on va pouvoir utiliser pour identifier notre environnement !

> *Attention : si vous faîtes plusieurs branches pour un même ticket GitLab, ce numéro sera réutilisé pour vos 2 branches. Notre but étant d'obtenir des identifiants uniques pour les environnements GitLab, nous vous déconseillons de faire plusieurs MRs pour un seul ticket.*
{.is-warning}

## Les environnements et les déploiements

> *L’ensemble du code présenté ci-dessous se trouve sur [ce repository GitLab](https://gitlab.com/taufort/git-branches-in-gitlab).*


Maintenant que nos branches ont un nom unique avec un identifiant facilement récupérable, nous pouvons créer un premier pipeline simple :

```yaml
stages:
  - test

.git_branch_short: &git_branch_short |
  export GIT_BRANCH_SHORT="$(echo ${CI_COMMIT_REF_SLUG} | cut -d '-' -f1)"
  echo "GIT_BRANCH_SHORT: ${GIT_BRANCH_SHORT}"

test:
  stage: test
  image: alpine:latest
  before_script:
    - *git_branch_short
  script:
    - echo "Run the tests on branch $GIT_BRANCH_SHORT"
  only:
    refs:
      - master
      - merge_requests
```  

Nous utilisons ici la variable d'environnement `CI_COMMIT_REF_SLUG` qui fait [partie des variables injectées dans les jobs par GitLab CI](https://docs.gitlab.com/ee/ci/variables/predefined_variables.html). Cette variable est calculée depuis une autre variable `CI_COMMIT_REF_NAME` qui représente le nom de la branche git. Elle est particulièrement adaptée pour être utilisée dans des URL. Dans notre cas d’exemple, `CI_COMMIT_REF_SLUG=2-create-first-gitlab-pipeline-in-project`.

Une fonction `sh` nous permet d’extraire uniquement le numéro de la branche et de le mettre dans une variable d'environnement : `GIT_BRANCH_SHORT`.

Pour éviter de définir plusieurs fois cette fonction, nous utilisons une *feature* de YAML qui permet d'insérer directement les instructions définies à l'endroit voulu avec `&YOUR_ANCHOR` pour la définition et `*YOUR_ANCHOR` pour l'insertion.

> La partie `only` est là pour limiter les cas dans lesquels le job est lancé. Elle n'est pas obligatoire pour le bon fonctionnement de cet exemple mais nous vous recommandons fortement de limiter ces exécutions pour éviter une consommation inutile de ressources. Il est bien de noter qu’il est également possible d’utiliser [la directive `rules` de GitLab](https://docs.gitlab.com/ee/ci/yaml/#rules) qui est bien plus puissante que les directives `only` et `except` qui vont plutôt servir pour des cas simples.

Le pipeline n'a, pour le moment, qu'une étape :

![first_job.png](/assets/img/apps/gitlab/first_job.png)

Le nom de notre futur environnement apparaît bien dans les logs :

```
GIT_BRANCH_SHORT: 2
```

Nous pouvons avancer dans la création du *pipeline* pour ajouter un *stage* de *build* (qui construira nos livrables) et un *stage* de *deploy* qui sera, à terme, chargé de faire le déploiement effectif de notre solution :

```yaml
stages:
  - test
  - build
  - deploy_integration

variables:
  DOMAIN: "git-branches-in-gitlab.fr"

.git_branch_short: &git_branch_short |
  export GIT_BRANCH_SHORT="$(echo ${CI_COMMIT_REF_SLUG} | cut -d '-' -f1)"
  echo "GIT_BRANCH_SHORT: ${GIT_BRANCH_SHORT}"

.default: &default
  image: alpine:latest
  before_script:
    - *git_branch_short
  only:
    refs:
      - master
      - merge_requests

test:
  <<: *default
  stage: test
  script:
    - echo "Run the tests on branch $GIT_BRANCH_SHORT"

build:
  <<: *default
  stage: build
  script:
    - echo "Build the application for branch $GIT_BRANCH_SHORT"

deploy_branch:
  <<: *default
  stage: deploy_integration
  script:
    - echo "Deploy the application to integration env"
    - DYNAMIC_ENVIRONMENT_URL="https://${GIT_BRANCH_SHORT}.${DOMAIN}"
    - echo "Env URL ${DYNAMIC_ENVIRONMENT_URL}"
    - echo "DYNAMIC_ENVIRONMENT_URL=$DYNAMIC_ENVIRONMENT_URL" >> deploy.env
  artifacts:
    reports:
      dotenv: deploy.env
  environment:
    name: branch-${CI_COMMIT_REF_SLUG}
    url: $DYNAMIC_ENVIRONMENT_URL
```

Dans le job `deploy_branch`, nous créons un environnement GitLab qui se prénomme `branch-${CI_COMMIT_REF_SLUG}`. Cet environnement apparaît notamment dans la liste des environnements GitLab dans le menu Opérations > Environnements :

![gitlab_environments.png](/assets/img/apps/gitlab/gitlab_environments.png)

Sur les lignes de chaque environnement, on peut voir 3 boutons d’action :

+ Le premier permet de consulter notre application. Ce lien est disponible grâce au fichier `deploy.env` dans lequel se trouve l’URL HTTP de notre application et que nous avons mis comme *artifacts dotenv* ;
+ Le deuxième permet de rejouer le déploiement de cet environnement ;
+ Le troisième permet de stopper l’environnement. Pour le moment, cette action aura simplement pour effet d'arrêter l’environnement dans GitLab.

> *Nous avons utilisé le [map merging de YAML](https://docs.gitlab.com/ee/ci/yaml/#special-yaml-features) pour faire des "héritages" entre les jobs. Nous aurions aussi pu utiliser la [directive GitLab](https://docs.gitlab.com/ee/ci/yaml/#extends) `extends` qui est un autre mécanisme de parent jobs. L’avantage du `extends` est que l’on peut référencer un job défini dans un autre fichier `.gitlab-ci.yml` dans [le cas d’un monorepo](https://blog.ippon.fr/2020/07/31/construire-la-ci-dun-monorepo-les-parent-child-pipelines-de-gitlab-ci/) par exemple (ce qui n’est pas le cas du map merging).*

Il est maintenant temps de "réellement" arrêter notre environnement via un nouveau job :

```yaml
deploy_branch:
  # ...
  environment:
    name: branch-${CI_COMMIT_REF_SLUG}
    url: $DYNAMIC_ENVIRONMENT_URL
    on_stop: destroy_branch
    auto_stop_in: 2 hours

destroy_branch:
  <<: *default
  stage: destroy_branch
  variables:
    GIT_STRATEGY: none
  environment:
    name: branch-${CI_COMMIT_REF_SLUG}
    action: stop
  script:
    - echo "Destroy environment \"branch-${CI_COMMIT_REF_SLUG}\""
  only:
    refs:
      - merge_requests
  when: manual
```
  
Nous avons maintenant ce pipeline :

![destroy_job.png](/destroy_job.png)

Et ce nouvel environnement :

![new_env_with_destroy.png](/new_env_with_destroy.png)

Le nouveau *job* `destroy_branch` définit les scripts pour arrêter notre environnement. Un point d'attention cependant : il faut surcharger la valeur de la variable d'environnement `GIT_STRATEGY` à `none` pour éviter de cloner le *repository* git (ce qui ne sera pas possible si la branche a déjà été supprimée).

> *Si vous avez besoin de scripts présents dans votre repository pour le stage de destroy, il faudra les stocker dans les [artifacts GitLab](https://docs.gitlab.com/ee/ci/yaml/#artifactspaths). Vous pourrez ensuite accéder à ces artifacts en utilisant les [dependencies des jobs](https://docs.gitlab.com/ee/ci/yaml/#dependencies).*

> *Nous avons aussi surchargé la partie only du job pour éviter une suppression de l’environnement lié à la branche master qui doit toujours rester allumé !*

La directive `on_stop: destroy_branch` sur le *job* `deploy_branch` permet de définir le *job* à invoquer lors de la destruction de l’environnement GitLab.

> *La clé `auto_stop_in` permet un arrêt automatique de l'environnement après une période donnée pour éviter de payer pour un environnement inutile. Il sera toujours possible de le redéployer au besoin. On peut aussi empêcher cet arrêt automatique en cliquant sur l'épingle qui est apparue dans les environnements.*

Et... c'est tout ! Enfin, il faut remplacer les scripts qui font des `echo` par de vrais scripts qui gèrent nos déploiements et nos destructions. Vous avez maintenant un cycle de vie GitLab avec des environnements par branche git !

## Quid du déploiement continu jusqu’en production ?
Avec les outils à notre disposition, les réelles difficultés aujourd’hui sont méthodologiques et organisationnelles. Il faut des méthodologies nous donnant une confiance absolue dans nos développements et une organisation prête à accepter ce changement de paradigme !

Source: https://blog.ippon.fr/2021/02/10/un-environnement-par-branche-avec-gitlab/