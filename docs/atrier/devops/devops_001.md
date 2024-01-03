---
title: Quand opter pour une architecture microservices ?
description: 
published: true
date: 2023-04-21T10:34:32.397Z
tags: beta, microservices, architecture
editor: markdown
dateCreated: 2023-04-21T10:34:28.040Z
---

## Quand opter pour une architecture microservices ?

Avec maintenant presque une décennie de recul, et un [engouement](https://microservices.io/) réel depuis quelques années, les microservices constituent un style d’architecture couramment adopté... avec plus ou moins de bonheur. Le temps est donc venu de faire le point sur ses avantages et ses contre-indications, ainsi que de dresser la liste des étapes qui permettent de réussir avec ce concept.

Au-delà de la programmation, le développement informatique moderne a pour objet l’intégration de solutions existantes, et la réutilisation d’algorithmes éprouvés en particulier. C’est pourquoi les architectures logicielles vont vers toujours plus de modularité, de découplage. Dans cette optique, l’approche dite “micro-services” opère une sorte de fusion entre les **architectures orientées services** (ou [SOA](https://en.wikipedia.org/wiki/Service-oriented_architecture), faiblement couplées) et les **périmètres bornés**, ou *bounded (business) context*. Un concept repris du [domain-driven design](https://en.wikipedia.org/wiki/Domain-driven_design) qui implique par exemple que chaque fonctionnalité soit dotée de son propre schéma de données et de sa propre chaîne dans l'usine logicielle, et où les données fournies restent limitées au strict nécessaire. Quoi qu'il en soit, rappelez-vous que ce ne sont pas les microservices qui délimitent vos fonctions, c'est plutôt votre conception fonctionnelle qui va aboutir à la création de services modulaires, en tenant compte de la fréquence et du volume des échanges entre eux. En appliquant un principe de responsabilité unique (ou [SRP](https://en.wikipedia.org/wiki/Single-responsibility_principle)), on s’autorise à utiliser pour chaque cas d’utilisation la technologie adéquate (du fait d’échanges via des protocoles agnostiques, par exemple en se basant sur une [architecture hexagonale](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software)) avec des "sas" d'entrée-sortie), et on bénéficie également d’une meilleure évolutivité en ayant le moins d’effets de bord possible. En tout cas, c'est le but recherché, bien qu'il ne soit pas toujours évident de procéder à cette dichotomie avec suffisamment de clairvoyance.

En analysant différents cas, nous avons souvent abordé la question de savoir si ce type d’architecture devait être généralisé dans les solutions développées, notamment dans le cadre de la migration des produits vers un déploiement [Kubernetes](https://kubernetes.io/fr/) (c’est-à-dire la virtualisation orchestrée, via des conteneurs). Mais il n’y a pas de réponse évidente :

+ D’un côté, il est clair que la modularité extrême des microservices apporte une plus grande souplesse de déploiement et d’exécution (*scalability*). Malgré une latence plus importante, ils bénéficieront largement d’instances orchestrées par la plateforme, avec beaucoup d'évolutivité et de résilience. Ces avantages sont loin d'être négligeables, en vérité, si l'on se place dans une perspective de croissance de l'activité à plus ou moins long terme.

+ Mais, d’autre côté, les microservices auront un cycle de vie et une supervision en production plus complexes ; les messages vont se multiplier et il faudra s’efforcer de garder la trace des transactions métier (voir le protocole *[two-phase commit](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)* et le *saga pattern* qui regroupe plusieurs transactions, etc). Sans oublier évidemment, que les taux de transfert sur les réseaux sont sans commune mesure avec ceux ayant cours au sein d'une seule machine (cache local, mémoire vive, disque dur).

+ On peut pas non plus attendre des microservices qu'ils permettent une moindre charge de développement. Certains tests et risques de régression peuvent se trouver allégés, mais la réduction du *time to market* donne une fausse impression d'accélération, en termes de fonctionnalités. Les processus complexes prendront toujours du temps à concevoir et à implémenter, en impliquant plusieurs équipes.

+ Par ailleurs, on ne doit pas se lancer dans un tel chantier sans prévoir qu’un certain temps sera nécessaire au découpage et à l’optimisation des services. Vous gagnerez beaucoup de temps en externalisant les problématiques transverses (audit, authentification et sécurité en particulier) vers des passerelles gérant déjà le routage et l’équilibrage de la charge.

+ Enfin, le nouveau système devra immanquablement remplacer un certain nombre de jointures par des échanges réseaux, qui ne seront pas forcément transactionnels. Après tout, qui veut *réellement* d'une transaction distribuée ?! Il ne faut pas perdre de vue que les objectifs de délimitation des différents schémas de stockage dans le système d’information iront aussi à l’encontre d’un éventuel objectif de rassemblement des données de référence, par exemple dans le cadre d’un chantier de [Master Data Management](https://en.wikipedia.org/wiki/Master_data_management). De nouvelles stratégies (ledger, datalake, etc.) et de nouvelles responsabilités viendront donc compléter les pratiques de développement et de supervision.


La décision d’appliquer ou non une architecture microservices s’avère donc plus délicate en pratique, que ne semble l’indiquer l’engouement affiché pour elle, avec une profusion d’articles et de cadriciels dédiés, dans les langages les plus populaires. Est-ce que toutes les applications doivent systématiquement être converties ? Nous ne le pensons pas. Est-ce qu’avec leur conversion les applications verront tous leurs problèmes résolus pour autant ? Nous ne le pensons pas non plus. Voici donc quelques critères permettant de s’organiser :

+ Vos équipes (actuelles ou futures) ont-elles l'organisation adéquate et les bon outils pour gérer les besoins importants de synchronisation entre des projets multiples ? Ces dépendances ne doivent pas être prises à la légère, et les pratiques de livraison doivent absolument s'y adapter (*blue/green, canary* ou autre). Des tests d'intégration plus poussés (*chaos monkey*) nécessitant l'implication de plus de personnes doivent être systématiquement prévus.
	+ Pour quelques bonnes idées sur la façon de procéder, intéressez-vous aux méthodes d'agilité à l'échelle, comme SAFe, LeSS ou Scrum Of Scrums (SOS).

+ Vos applications ont-elles besoin de supporter une charge telle (pas forcément très importante, mais surtout fluctuante) que la mise à l’échelle automatique soit devenue incontournable à la maîtrise de votre solution ?
	+ Les temps de réponse de leurs sources de données ont-ils été optimisés (avec un cache, un moteur de recherche, le partitionnement des données) ?
	+ Vos traitement peuvent-ils être réorganisés autour de bus de messages, afin de lisser la charge avec des échanges asynchrones ?
	+ Peut-être qu’une solution à base de configuration centralisée, de *service discovery* et de distribution de charge (*load balancing*) côté client ou serveur (avec une API Gateway), peut les stabiliser ?

+ Avez-vous clairement identifié les goulots d’étranglement dans votre système d’informations, avant d’envisager une refonte complète ?
	+ Les applications distribuées souffrent souvent de problèmes liés aux ressources partagées (verrous sur les bases de données, concurrence d’accès au stockage des fichiers).
	+ Utilisez-vous toujours des transactions avec un niveau d’isolation pertinent (*read only* pour les requêtes de consultation, en mode CQRS) ?
	+ Si les mises à jour des informations du système sont intensives, avez-vous envisagé la possibilité de synchroniser des réplicats en lecture seule pour répondre aux pics de consultation ?
	+ Il faudra d’abord investir dans la mise en place des techniques de programmation distribuée (notamment *locking, circuit breaking, leader election*) et adopter une approche *stateless*, afin de profiter d’un déploiement orchestré des conteneurs.

+ Vos outils de supervision sont-ils adaptés à un système d’instances multiples, afin que la vision obtenue vous permette de voir les vrais problèmes ?
	+ Kubernetes excelle dans la gestion et le regroupement des instances en “services” cohérents, mais ces fonctions demandent à être maîtrisées. Le choix d'une plateforme OpenShift, plus intégrée, peut vous permettre de mettre le pied à l'étrier graduellement.
	+ Êtes-vous prêts à investir (éventuellement progressivement) dans la méthodologie *twelve-factor*, afin de faire évoluer votre application en SaaS si besoin ?
	+ Si votre système devenait complexe, vous voudrez certainement utiliser une surcouche de surveillance des nombreux échanges, un *service mesh*. Celui-ci sera matérialisé par des *proxies* réseau, des sortes d’agents applicatifs basés sur le *pattern* "sidecar". Les solutions Istio et Consul Connect nous semblent être des pistes intéressantes à cet égard.

Comme on le voit, toutes les applications ne justifient pas l’emploi des "grands moyens", tandis que pas mal de solutions intermédiaires permettent d'orienter leur évolution selon un rythme soutenable, tout en continuant à gérer des services plus monolithiques. Nos équipent accompagnent tous les jours des projets se trouvant à des stades différents de cette migration. Notre recommandation est de n’envisager la modularisation fine des services que dans la mesure où ce découpage doit avoir un impact sur les performances ou l’évolutivité des produits. Vous disposerez ainsi d’une granularité d'analyse vous permettant de savoir quelles nouvelles refontes architecturales seront vraiment nécessaires. La meilleure pratique étant d'établir, préalablement à la migration architecturale, une cartographie des projets existants et des besoins, puis une évaluation précise des enjeux.

Pour aller plus loin et maîtriser les subtilités qui se cachent derrière la décomposition des applications monolithiques en micro-services, nous vous recommandons les conférences de Sam Newman et la lecture de son livre : [Monolith to Microservices - Evolutionary Patterns to Transform Your Monolith](https://youtu.be/GBTdnfD6s5Q).
Voici selon lui les trois principales justifications d'une telle migration, qui doit toujours se faire progressivement (*strangler fig pattern*) :

+ Déploiement indépendant et instantané ; dans un contexte de haute disponibilité, cela limite le risque fonctionnel et évite d'avoir à coordonner des équipes nombreuses
+ Concentration des processus autour de données à isoler ; ce qui rend aussi plus compliqué le fait d'abîmer la conception d'origine, car les structures de données sont souvent plus faciles à interpréter que le code
+ Autonomie organisationnelle des équipes projet ; avec un passage de compétences facilité, ce qui est une bonne motivation en soi pour un changement de culture

Source: https://blog.ippon.fr/2021/01/18/quand-opter-pour-une-architecture-de-micro-services/