---
title: Réussir sa transition depuis le monde du développement vers le monde du DevOps/Cloud
description: 
published: true
date: 2023-06-21T20:35:05.517Z
tags: 
editor: markdown
dateCreated: 2023-04-22T08:32:17.904Z
---

# Réussir sa transition depuis le monde du développement vers le monde du DevOps/Cloud

![copy-of-william-iven-gcsnospexfs-unsplash-2.jpg](/assets/img/devops/copy-of-william-iven-gcsnospexfs-unsplash-2.jpg)

## Comment je suis arrivé à faire cette transition
Avant de rentrer dans le vif du sujet, je vais commencer par me présenter, qui suis-je et d’où je viens. Je m’appelle Sébastien, j’ai 25 ans et je travaille aujourd’hui en tant qu'ingénieur Cloud DevOps chez Ippon Technologies. Mais comme vous auriez pu le deviner dans le titre, ce poste est plutôt récent.

En effet, je suis sorti de Polytech Nantes avec mon diplôme d’ingénieur informatique il y a maintenant 2 ans. Comme la plupart de mes camarades de promotion, à la sortie de l’école, je me suis très vite dirigé vers le monde du développement. C’est très certainement la voie empruntée par une grande partie des ingénieurs en sortie d’école d’informatique. Ma première expérience dans le monde professionnel était donc un poste en tant que développeur full stack (plus axé backend tout de même) sur des technologies du moment : Java avec du SpringBoot saupoudré d’Angular pour la partie front-end. J’étais également dans un écosystème que je ne connaissais pas, ou très peu :

- Gitlab CI/CD (kezako ?) j’avais connaissance de Gitlab évidemment, par contre je n’avais quasiment jamais entendu parler de CI/CD.
- Ansible, inconnu au bataillon.
- Architecture microservices ? On a dû m'en parler en cours…

2 années s’écoulent. Ma montée en compétences se passe plutôt très bien. En parallèle, je discute beaucoup avec mon colocataire qui travaille aussi dans l’informatique, mais pas du tout dans le monde du développement. Il travaille en tant qu’ingénieur Cloud/DevOps chez Ippon Technologies. Au fil des discussions, il me parle d’automatisation, de résilience, du Cloud, du fait de pouvoir écrire et versionner son infrastructure et plus il m’en parle, plus ça m'intéresse.

Au cours de cet article, je vais donc m’appuyer sur mon expérience personnelle, mais pas que : j’ai pris l’initiative de sonder mes collègues au sein d’Ippon pour connaître leurs parcours. Je vais pouvoir au travers de ces différentes histoires, essayer de donner tous les outils nécessaires pour pouvoir réussir cette transition.

Premièrement, peut-être que des personnes se posent la question, c’est quoi le Cloud et le DevOps ?

- Le DevOps est un ensemble de pratiques qui met l’accent sur la collaboration et la communication entre les développeurs logiciels et les ops, en automatisant le processus de livraison de logiciels et les changements d’infrastructure.
- Le Cloud correspond à l’utilisation d’infrastructures et de services hébergés par des entités tierces.

En lisant les réponses à ce sondage, on se rend très vite compte que les raisons principales pour lesquelles des développeurs se dirigent vers le Cloud/DevOps sont :

- La curiosité. C’est un des traits qui définit beaucoup de personnes. C’est aussi grâce à cette curiosité que l’on peut, d’après moi, améliorer ses compétences.
- Le concept de toil. Toil est un mot inventé par Google désignant des tâches ops qui sont : manuelles, répétitives, automatisables, tactiques, sans valeur durable et de plus en plus compliquées à maintenir avec la croissance du nombre d’utilisateurs. Avoir périodiquement à réaliser ce genre de tâches nous amène à vouloir penser de plus en plus en tant que DevOps. (Pour en lire plus sur le concept de toil et l’élimination de ces tâches : https://sre.google/sre-book/eliminating-toil/)
- L’envie de comprendre la totalité de ce qui compose une application. D’avoir un écosystème qui évolue rapidement, ou beaucoup de choses restent à créer (comme l’écosystème java dans les années 2000).

## Quelles sont les étapes clés de cette transition ?
### La formation
Dans un premier temps, j’ai cherché à connaître l’étendue de l’offre cloud en essayant de comprendre les concepts basiques, ensuite je me suis décidé à relier ses concepts avec un Cloud Provider. Je me suis donc tourné vers les formations.

C’est l’étape la plus importante. Le plus facile pour cette étape est de se fixer une certification à obtenir. Par exemple, que ce soit pour AWS, GCP, AZURE, Kubernetes, Terraform, viser une certification permet de toujours avoir un cap vers lequel se diriger. Pour ma part, c’était la Solution Architect Associate d’AWS. Je ne regrette absolument pas ce choix, la certification permet de connaître les services essentiels d’AWS avec une bonne base sur les spécificités de ceux-ci. En plus, cela m'a donné de la connaissance sur les concepts se cachant derrière ces services (par exemple, c’est bien de savoir qu’il existe plusieurs types de Load Balancer, mais c’est mieux de savoir à quoi ça sert).

Une fois cette certification en poche, je me suis demandé comment manipuler ses ressources ? Je me suis donc naturellement tourné vers Terraform, un outil d’Infrastructure As Code. J’ai donc pu passer la certification HashiCorp Certified Terraform Associate. Intéressante si vous n’avez aucune base sur Terraform. Elle reste très basique et moins pertinente si vous avez déjà un peu d’expérience sur le sujet malheureusement.

### La pratique
La seconde étape est la pratique, j’ai très rapidement démarré une mission pour pouvoir mettre en pratique mes nouvelles compétences. Pour moi, la pratique m’a permis d’enfin pouvoir manipuler des services dont j’avais entendu parler pendant près de 2 mois. Comme on le dit souvent, c’est en forgeant qu’on devient forgeron.

### Les relectures de code
La troisième étape passe par les relectures de code. C’est un très bon moyen pour monter en compétence, il ne faut surtout pas avoir peur en tant que “junior” de faire des revues de code. Il est fort probable que j'aie du contenu à enseigner aux autres grâce à la formation que j'ai suivie. On le sait tous, l’informatique bouge tous les jours, et les services des fournisseurs de cloud aussi. Pour citer un exemple concret, au début de ma première mission, on souhaitait avoir un système de purge sur une table DynamoDB. Une lumière s'est allumée directement dans ma tête. Je me suis rappelé que lors de ma formation, on m’a parlé d’un TTL (Time To Live). Ce champ permettait de gérer la durée de vie d’un document dans DynamoDB nativement (plus besoin de faire un système de purge). J’ai donc proposé d'implémenter cette solution à mon équipe (personne n’y avait songé !).

### L’amélioration continue
La quatrième et dernière étape s’inscrit dans un mécanisme d’amélioration continue. Il ne faut jamais se reposer sur ses acquis. Toujours être ouvert à l’avis des autres, ne pas se braquer, mais plutôt se porter vers le débat. Une chose importante, si vous avez la chance d’avoir des personnes expérimentées à vos côtés, ne pas hésiter à poser des questions. Encore une fois, on revient à la curiosité. Ces personnes sont des mines de connaissances, ce serait vraiment dommage de s’en priver. En plus de ça, il y a de grandes probabilités que ce que je suis en train de faire, ils l’aient déjà fait.

Toujours dans l’amélioration continue, la veille est pour moi, l’élément clé pour devenir un bon ingénieur Cloud/DevOps. Il existe tout un tas de façons de faire de la veille.

Chez Ippon, nous avons l’excellent programme BlackBelt permettant aux collaborateurs de se former sur des choses qui les intéressent. En plus de ça, il y a toute la partie mentoring (le fait d’avoir un mentor et de l’être également) qui est très intéressant.

Il y a aussi tout un tas de groupes meetup permettant de faire de la veille. Je suis personnellement sur le groupe Nantes AWS user group ainsi que Cloud Finops France (sur l’application meetup). Des événements sont organisés de temps à autre sur des sujets la plupart du temps très intéressants (que ce soit des retours d'expérience sur un service, la présentation d’un nouvel outil ou le partage de bonnes pratiques).

Pour finir, il y a tout un tas de plateformes avec des contenus super intéressants. C’est d’ailleurs l’objet du prochain chapitre de cet article.

## Mes recommandations concernant les supports de formation
Je vais diviser ce chapitre en fonction des besoins. Je vais essayer d’être le plus exhaustif possible sur les outils les plus importants à avoir dans sa boîte à outils pour bien commencer.

Commençons par **AWS** (Amazon Web Services):

- La formation de Stéphane Marek [Ultimate AWS Certified Solutions Architect Associate](https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/). C’est celle qui est la plus citée dans mon sondage. C’est aussi celle que j’ai suivie pour obtenir ma certification. Avec cette formation plus un package d’examens blancs, vous serez prêts à passer la certifications sans aucun doute. Pour le package d’examens blancs je vous conseille [Whizlabs](https://www.whizlabs.com/aws-solutions-architect-associate/) personnellement.
- Les FAQs des différents services, la plupart des questions de l'examen portent sur des questions qui trouvent leurs réponses dans la FAQ.
- YouTube et notamment les conférences de l’AWS Summit qui présentent les nouveautés d’AWS, les bonnes pratiques, les architectures en vogue et j’en passe.
- Par exemple, la conférence sur les patterns et bonnes pratiques des architectures serverless que je recommande fortement :  https://www.youtube.com/watch?v=9IYpGTS7Jy0
- Mais encore, sur la partie nouveauté et surtout l’effet Wouah, certainement l’une des conférences qui a fait le plus parler d’AWS :
- https://www.youtube.com/watch?v=8vQmTZTq7nw&pp=ugMICgJmchABGAE%3D
- Les podcasts : “[Le podcast AWS en français](https://stormacq.com/podcasts/index.html)”, présenté par Sébastien Stormacq qui travaille chez AWS. Il y résume une semaine sur deux les grandes actualités d’AWS. L’autre semaine, il invite quelqu’un pour parler en général, d’un retour d’expérience sur tout un tas de sujets liés au Cloud.
- “AWS podcasts” , podcast super intéressant, mais il faut être à l’aise avec l’anglais.
- Pour finir, les groupes de [meetups](https://www.meetup.com/fr-FR/) (meetup étant une application permettant à des personnes partageant une passions commune d’organiser des événements ensemble) avec les users groupes AWS de notre ville. Voici par exemple le group AWS pour la région nantaise : https://www.meetup.com/fr-FR/AWS-Nantes/

Parlons un peu de **GCP** (Google Cloud Platform) désormais (j’ai moins d’informations concernant ce Cloud Provider) :

- [Le livre officiel de Google](https://books.google.fr/books/about/Official_Google_Cloud_Certified_Associat.html?id=eNuMDwAAQBAJ&redir_esc=y) permettant de passer la certification Cloud Engineer de GCP. Attention par contre sur les nouveautés, le livre commence à dater un peu. Par contre pour la certification Architect, une édition vient tout juste de sortir.
- Les Google Cloud Developer group sur meetup.
- La documentation de Google Cloud Platform.
- Une formation faite par Google sur [Coursera](https://fr.coursera.org/professional-certificates/cloud-engineering-gcp).
- Google Cloud Platform podcast, qui est comme son nom l’indique un podcast parlant de GCP.

Comment ne pas citer **Azure**, le Cloud Provider de Microsoft (également moins d’informations concernant ce Cloud Provider) :

- La documentation officielle de Microsoft.
- Whizlabs pour tout ce qui va être examens blancs.
- La chaîne youtube de [Adam Marczak](https://www.youtube.com/c/Azure4Everyone/videos).
- Côté podcasts : The Azure Podcast avec de courtes émissions (environs 30 minutes) sur différents sujets.

Et si je vous dis **Kubernetes** ?

- La documentation officielle de Kubernetes.
- Sur youtube plusieurs noms sont revenu : [Techworld With Nana, That Devops Guy](https://youtube.com/c/TechWorldwithNana) ainsi que [Xavki](https://youtube.com/c/xavki-linux).
- Sur Kubernetes encore plus que les autres, la pratique, la pratique et la pratique. En effet, les certifications côté Kubernetes sont seulement des examens pratiques et pas théoriques. Il faut donc énormément pratiquer avant de pouvoir passer ce genre de certifications.  
- Concernant la pratique, des petits outils plutôt pratiques : [Minikube](https://minikube.sigs.k8s.io/docs/start/) (permet de déployer un cluster en local), [K9s](https://k9scli.io/) qui est lui, un outil de gestion de cluster.
- Côté podcasts : les Castcodeurs et le podcast officiel de Kubernetes (c’est de l’anglais par contre).

Du côté de **Terraform** :

- Une superbe vidéo de [FreeCodeCamp](https://www.youtube.com/watch?v=V4waklkBC38) qui permet de comprendre comment fonctionne Terraform (excellente pour se préparer à la certification HashiCorp Terraform Associate Certification).
- [La documentation officielle de Terraform](https://www.terraform.io/docs).
- Faire des mini-projets pour prendre en main les outils, intégré terraform dans une chaîne de déploiement continu,...

Plus généralement, dans le monde du **DevOps** :

- https://roadmap.sh/devops : une mindmap avec les outils et leurs utilités
- Trois livres : The phoenix project, The unicorn project et Accelerate.
- Pour ce qui est des podcasts : Compagnons du DevOps, No Limit Secu, Les Rendez-vous Tech.
- Un cours sur [OpenClassRoom](https://openclassrooms.com/fr/courses/2035736-mettez-en-place-lintegration-et-la-livraison-continues-avec-la-demarche-devops/6182691-quest-ce-que-lintegration-continue)

Attention, cette liste n’est ni exhaustive ni à suivre à la lettre. Elle est plutôt ici pour vous guider. Internet est vaste et il y a de super formations comme de moins bonnes. L’une des choses les plus importantes est de s’approprier les concepts de base. A quoi bon connaître sur le bout des doigts tous les services d’un Cloud Provider si l’on ne sait pas expliquer l'utilité de chacun ? A quel moment dois-je utiliser l’un plutôt que l’autre ?

Attention aussi à ne pas se lancer dans une quête acharnée du savoir en voulant trop apprendre. Personnellement, ce qui me réussit bien, c’est de prendre un sujet, l’approfondir au maximum, puis ensuite, en choisir un autre.

Maintenant que vous avez toutes les informations qu’il vous faut, j’aimerais conclure cet article sur quelques tips pour réussir sa transition. En effet, chez Ippon, nous avons tout un tas de types de profil, et chaque personne à son histoire. J’ai pu m’en rendre compte avec mon sondage. J’ai notamment posé la question : “Quel serait le conseil le plus important que tu donnerais à quelqu'un souhaitant réaliser cette transition ?” et je vais maintenant faire un récapitulatif des réponses que j’ai pu avoir à cette question.

## Les Ipp’Tips pour réussir sa transition
1. Le tips numéro un, celui qui est revenu quasiment à chaque fois : ne jamais s’arrêter d’apprendre. La curiosité est l’outil principal d’un consultant Cloud/DevOps. Ça tombe bien, les outils que l’on utilise tous les jours ne font qu'évoluer. Il ne se passe pas une journée sans qu’une nouvelle version d’un service d’un Cloud Provider ne voie des nouveautés sortir, une nouvelle version de Terraform ou encore une nouvelle LTS de SonarQube sorte. Sans cette curiosité, on se retrouve très vite dépassé par la vitesse à laquelle avancent les technos.
1. Savoir se remettre en question. Ne pas avoir peur de poser des questions à nos collègues. Discuter avec des personnes d’horizons différents. Toujours se dire que ce que tu es en train de faire aujourd’hui, quelqu’un l’a déjà fait un autre jour. Cette personne a certainement un retour à te faire, des choses qu’elle regrette ou au contraire, des choses dont elle est plutôt fière.
1. Avant de faire le grand saut, se poser la question si l’on pense que le code (dans le sens de développer des features produit) va nous manquer ? S’il est facile, ou du moins, pas trop compliqué de faire machine arrière si jamais on se rend compte que cela ne nous plaît pas. Ne pas hésiter si c’est possible de prendre de petites tâches en relation avec ce que l’on pourrait faire à longueur de journée ensuite. On peut rapidement se rendre compte qu'au final, ça a l'air sympa mais on préfère en faire périodiquement.
1. Pratiquer. C’est en pratiquant que l’on va réellement monter en compétences. Ne pas hésiter à faire des projets persos. Se faire un petit projet sur son temps libre avec une CI/CD permettant de déployer son infrastructure sur un Cloud Provider à l’aide d’un outil d’IaC par exemple. Chez Ippon, on a la chance d’avoir à disposition une sandbox AWS pour pouvoir faire ce genre de projet justement, c’est super utile.

## Pour conclure
Vous avez désormais tous les outils dont vous avez besoin pour pouvoir vous tourner vers le monde du Cloud/DevOps. Ou alors vous vous êtes peut-être rendu compte que ce monde vous intéressait ? J’en serai ravi !

Attention, je ne dis pas que cette transition sera simple, cela dépend de chacun·e. Par contre, après avoir lu cet article, vous aurez à disposition un recueil qui, d’après mes supers collègues et moi-même, résume comment se mettre dans les meilleures conditions pour y arriver.

Pour finir cet article, je tenais à remercier Ippon. Merci de m’avoir fait confiance, je suis arrivé en tant que junior (spoiler : je le suis encore), mais je vois que j’ai évolué, et dans le bon sens. Je me sens beaucoup plus à l’aise de jours en jours. Et tout ça, c’est possible grâce au temps de formation alloué par Ippon avant que je parte en mission, à la disponibilité et l’expertise des collègues avec qui je suis actuellement en mission mais pas que. Il ne faut pas oublier la practice Cloud & DevOps, vers qui j’ai pu me tourner dès lors qu’une question me tourmentait.

Pour les curieux, voici le lien vers les réponses au sondage que j’ai pu faire :
https://docs.google.com/spreadsheets/d/1h1Iwf4utUVyXHNOoURaXN2O0fTrlEkiqPuV-PYS11_8/edit?usp=sharing

source : https://blog.ippon.fr/2022/06/08/reussir-sa-transition-depuis-le-monde-du-developpement-vers-le-monde-du-devops-cloud/