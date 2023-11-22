---
title: Agile : Introduction et premiers pas avec JIRA
description: 
published: true
date: 2023-06-11T10:55:20.353Z
tags: 
editor: markdown
dateCreated: 2023-06-11T10:55:20.353Z
---

# Agile : Introduction et premiers pas avec JIRA

Quand on parle d’agilité, on pense souvent framework avec Scrum, Kanban, SAFe, Extreme Programming… mais aussi rôle comme le Scrum Master, le Product Owner, les Developers…

Connaître la théorie des différentes méthodologies, c'est bien ! Mais quand vient le moment de les appliquer, très vite on se rend compte qu’on a besoin d’être bien outillé dans notre quotidien.

Atlassian propose une suite d’applications permettant de faciliter plusieurs aspects dans un projet. L’un d’entre eux permet à la fois aux chefs de projets ou product owner d’avoir une visibilité sur les sujets en cours et à venir, de les organiser, les dispatcher... Il sert également de support visuel pour les développeurs dans le cadre d’un projet en mode Agile : Jira.

Il y a de fortes chances que vous ayez déjà eu à utiliser Jira dans votre quotidien, mais savez-vous vraiment comment le rendre le plus efficace possible ? Savez-vous comment bien le paramétrer ? Connaissez-vous ses points forts et ses limites ?

Je vous propose, à travers une suite d’articles, de vous guider sur son fonctionnement et sur les bonnes pratiques à appliquer pour que vous obteniez demain votre black belt Jira !

## ATLASSIAN
Avant de rentrer dans le vif du sujet, voici une rapide présentation des acteurs qui ont créé ce magnifique outil qu’est Jira mais aussi sur les autres produits que vous serez amenés à rencontrer en travaillant sur celui-ci.

ATLASSIAN est un éditeur de logiciel fondé par deux Australiens, Scott Farquhar et Mike Cannon-Brookes. L’entreprise développe des produits pour le développement et la gestion de projet.

La suite comprend de nombreux outils comme Confluence, wiki collaboratif ; Bitbucket, gestionnaire de ressource ou encore Trello et Jira pour la gestion de projet.

La force de l’ensemble de ses outils est d’avoir une interconnexion très forte entre l’un ou l’autre de ses produits. De ce fait, il est très facile d’écrire un article sur Confluence et de le lier au sein d’un ticket Jira par exemple.

Cela permet d’avoir un ensemble cohérent d’outils, centralisé par une authentification unique et permettant de maîtriser tous les maillons du développement d’un produit dans un même environnement.

Toutefois, certains maillons de cette chaîne restent encore perfectibles comme Bitbucket qui peut souffrir d’une latence de fonctionnement plus forte que certains concurrents comme GitHub.

Qu’en est-il de Jira ?

## Jira
Jira est un outil de gestion et de développement de projet. Il sert de suivi à la fois pour les chefs de projets qui peuvent reporter et planifier les sujets mais aussi pour les développeurs qui peuvent voir les tâches qui leurs sont confiées et les faire avancer à travers un workflow.

La solution existe en mode gratuit et en mode payant. Voici quelques différences entre les deux :

|   |Version gratuite|Version payante|
|:--|:---------------|:--------------|
|Nombre d’utilisateurs|10|20 000|
|Règles d’automatisation|Uniquement sur un projet|Applicable sur un ensemble de projets|
|Sandbox|X|V|
|Espace de stockage|2 Go|Illimité|

L’ensemble des différences sont disponibles sur le site de [Jira](https://www.atlassian.com/fr/software/jira).

Voici à quoi ressemble l’outil sur un projet en cours (ici exemple sur un projet Scrum) :

<img src="https://blog.ippon.fr/content/images/2022/07/1.png">

On peut y voir plusieurs zones :

- Un bandeau en haut de page (en bleu) permettant de naviguer à travers différents éléments transverses aux différents projets
- Un bandeau à gauche (en rouge) permettant d’accéder à divers éléments du projet en cours
- La zone principale (en vert) avec un tableau du projet (ici une vue sur le sprint en cours)

Il est possible de personnaliser quasiment l’ensemble des informations affichées. Jira est conçu pour se coller au plus près possible des besoins du projet !

## Création d’un projet Jira
Avant tout, il vous sera nécessaire d’avoir un [compte Atlassian](https://id.atlassian.com/signup?continue=https%3A%2F%2Fid.atlassian.com%2Fstart%3FatlOrigin%3DeyJpIjoiNjI1YmI1MGFkOTk5NDBjOWJkOWMwMzk2ZmI5MzA1Y2EiLCJwIjoid2FjLWdsb2JhbGRyb3Bkb3duIn0).

Ensuite, rendez-vous sur le site de Jira pour créer votre projet.

NB : cet article s’appuie sur la version gratuite de Jira en version Cloud.

Premier conseil concernant le nom du site : prenez un nom qui regroupe plutôt le côté organisationnel de votre entreprise (ou qui fait office de regroupement de projet) et de garder un nom plus précis pour le projet.

Attention, avant de valider la création du projet, faites attention à la clé qui sera pré-remplie en fonction du nom du projet. Cette clé pourra être changée par la suite.

Gardez une clé pertinente en fonction du nom du projet pour comprendre rapidement de quel sujet vous parlez. Par exemple, pour le projet “refonte-site-Y”, on peut donc imaginer une clé “RSY”. Les tickets créés dans ce projet auront donc comme clé d’identification “RSY-<N°>”.

Vous aurez également la possibilité de choisir le modèle de projet (Kanban, Scrum…) et son type (Team-managed ou Company-managed).

Il est possible de switcher de modèle ou de type durant la vie du projet mais je le déconseille.

En effet, il est plus ou moins possible de changer ces paramètres mais vous risquez de perdre en cohérence durant la vie du projet si vous switcher plusieurs fois. Mieux vaut faire un nouveau projet quitte à migrer quelques tickets d’un projet à l’autre au besoin.

Les projets types “Team-managed” sont adaptés aux équipes indépendantes qui souhaitent contrôler leurs propres processus et pratiques de travail dans un espace autonome.

Les projets types “Company-managed” sont configurés et maintenus par les administrateurs Jira. Ils sont conçus pour les entreprises qui souhaitent standardiser une méthode de travail entre de nombreuses équipes, comme le partage d'un workflow.

Le choix du type de projet se fera donc en fonction de la typologie du projet. Dans le cadre d’une petite équipe qui s’auto-gère, le type “Team-managed” sera le plus adapté, tandis que dans le cadre d’une structure plus carré (ou sur la création d’un projet au sein d’une entité déjà bien rodée), le type “Company-managed” sera celui à choisir.

## Fonctionnement général
Jira s’appuie sur un système de ticketing et de tableau.

Les tickets sont réparties entre les types suivants :

- Story
- Task
- Bug
- Epic
- Sub-task

De base, voici comment se présente leur création :

<img src="https://blog.ippon.fr/content/images/2022/07/2.png">

Plusieurs champs sont éditables et il est possible de paramétrer, par exemple, les champs affichés, pré-remplir des champs à la création…

Nous n’entrerons pas en détail ici sur l’utilité et le fonctionnement de l’ensemble des champs. Mais on peut tout de même retenir quelques points :

- Le champ “Components” contient une liste d'éléments non modifiable par les utilisateurs. Cela permet de spécifier, sans se tromper, sur la casse par exemple, le groupe concerné (UI, Back…)
- On peut lier les tickets entre eux en spécifiant par exemple que telle tâche est bloquée par telle autre etc..
- En fonction des typologies de projet, certains champs ne seront pas forcément disponibles (comme le sprint à associer)

Sur le tableau, il est ensuite possible de changer l’état d’un ticket en le glissant directement comme un post-it ou en éditant les champs dans le ticket.

Il est possible de créer des sous-tâches pour découper les actions à faire, assigner les tickets à des utilisateurs….

Son fonctionnement reste très intuitif mais aussi très complet. Une dose de paramétrage est nécessaire pour pouvoir l’adapter en fonction de ses besoins !

## Personnalisation de votre projet
A la création d’un projet, vous aurez accès à un environnement très minimaliste.

L’intérêt est donc de paramétrer Jira en fonction de vos besoins sur le projet.

Nous allons parcourir quelques notions qui seront détaillées dans d’autres articles. Tous les aspects de personnalisation sont disponibles dans la [documentation de Jira](https://support.atlassian.com/jira-software-cloud/).

A noter que certains points comme l’édition des [détails du projet](https://support.atlassian.com/jira-software-cloud/docs/edit-a-projects-details/) ne seront pas abordés. Le but est de comprendre les points essentiels pour le bon fonctionnement de votre équipe.

### Les tableaux
Les tableaux (ou “boards”) sont les éléments centraux de Jira. Ils servent principalement de management visuel et à rapidement voir l’avancement du projet ou du sprint en temps réel.

Pour les projets Scrum ou Kanban, il en existe par défaut 2 : celui des tâches en cours et le backlog. Pour les projets Kanban, le backlog n’est pas visible par défaut dans le bandeau de gauche.

Le premier affiche tous les tickets sans restriction. Il suit un workflow de base très minimaliste :

- Liste des états : “TO DO”, “IN PROGRESS” et “DONE”
- Création d’un ticket > État “TO DO”
- Possibilité de passer d’un état à l’autre sans aucune restriction.

Pour voir l’ensemble des tableaux, vous pouvez cliquer sur la touche “/” de votre clavier ou cliquer sur le champ de recherche (la loupe dans le bandeau du haut) et cliquer sur “Boards”. C’est également ici que vous avez la possibilité d’en créer de nouveaux.

### Les workflows
Dans Jira, chaque ticket suit un enchaînement d’état en suivant un schéma précis : le workflow.

Avant toute chose, il faut garder à l’esprit qu’un workflow n’est pas un tableau. Le tableau est une vue sur l’ensemble de l’état des tickets filtrés ou non tandis que le workflow définit le cycle de vie d’un ticket.

Pour gérer vos workflows, rendez-vous dans les paramètres globaux > Workflows si vous êtes administrateur du site ou alors en cliquant sur le bouton “View workflow” quand vous changez le statut d’un ticket si vous êtes administrateur du projet.

L’administration des workflow se fait soit un mode Diagram, soit un mode Text. La seule grosse différence entre les deux modes est la manière de présenter les steps et les transitions.

A partir de ce point, il est donc possible de construire le cycle de vie des tickets correspondant au besoin réel du projet.

Je conseille très fortement de construire son propre workflow même simpliste car cela vous permet d’une part de vous habituer à utiliser la fonctionnalité mais surtout d’avoir la main directement dessus si vous êtes amené à faire évoluer votre workflow (et je suis sûr à 99% que cela sera le cas ! ).

Je vous conseille également de le rendre le moins “permissif” possible. Cela ne signifie pas qu’il doit être contraignant mais plutôt d’expliciter clairement tous les chemins possibles d’un ticket.

Prenons l’exemple suivant :

<img src="https://blog.ippon.fr/content/images/2022/07/3-2.jpg">

Le rond gris correspond à l’action de création d’un ticket. Après sa création, il se trouve dans l’état “Open”. Il va transiter de l’état “In Progress”, “Test” et “Done” dans cet ordre strictement.

Dans le cas d’un ticket créé qui doit finalement être abandonné, on devra forcément effectuer le même enchaînement. Cela n’aura pas de sens dans le traitement de celui-ci ni même dans son historique.

C’est pourquoi il faudra donc faire évoluer le workflow de la manière suivante :

<img src="https://blog.ippon.fr/content/images/2022/07/4.png">

A noter également l’importance de bien nommer les transitions entre les états. Cela permet de comprendre rapidement la signification de passer un ticket de l’état “Open” à “Done” (ici l’acte d’abandonner la tâche).

Pour plus d’informations, voici le [lien de la documentation Jira](https://support.atlassian.com/jira-software-cloud/docs/what-are-jira-workflows/) sur les workflows.

### Les filtres
Dernier point à aborder, les filtres. Cet élément permet de limiter la liste des tickets sur un tableau ou pour une extraction précise.

Pour accéder à leurs gestion, cliquer sur le menu “Filters” > “View all filters”.

Il existe deux modes pour la gestion d’un filtre, le mode basique ou le mode avancée.

Avec le mode basique, vous pouvez sélectionner les filtres via des listes déroulantes à appliquer.

<img src="https://blog.ippon.fr/content/images/2022/07/5.png">

Le mode avancée utilise le langage JQL.

<img src="https://blog.ippon.fr/content/images/2022/07/6-1.png">

Encore une fois, l’idée n’est pas de détailler ici son fonctionnement, vous trouverez toutes les informations nécessaires dans la documentation sur les filtres en mode [basique](https://support.atlassian.com/jira-software-cloud/docs/search-for-issues-in-jira/) ou [avancée](https://support.atlassian.com/jira-software-cloud/docs/use-advanced-search-with-jira-query-language-jql/).

Je vous conseille de vous familiariser avec le mode basique pour comprendre comment on peut filtrer l’ensemble des tickets et ensuite basculer en mode avancée pour voir le résultat en JQL.

Il faut principalement retenir que les filtres sont par exemple utiles pour la création de tableaux pour limiter les tickets à afficher.

## Conclusion
Jira est un outil très complet avec un niveau de personnalisation très avancé.

Il est à la fois utile pour l’équipe de développement en l’utilisant comme un véritable management visuel mais aussi pour les managers ou PO qui ont besoin d’avoir rapidement une vision générale sur l’avancement des tâches.

Dans cet article j’ai surtout voulu vous montrer comment débuter avec Jira en tant qu’administrateur et les premières approches que j’estime importantes à comprendre avant d’aller plus loin. Il est très difficile de résumer en quelques paragraphes tout ce qui est possible de faire.

Mon dernier conseil est d’apprendre à utiliser l’outil avant de vous lancer dans un contexte projet. Suivez la documentation, elle sert à la fois de guide mais aussi d’aide pour répondre à un maximum de questions.

Source : https://blog.ippon.fr/