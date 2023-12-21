---
title: Module 7 - introduction
description: 
published: true
date: 2023-04-21T10:56:39.002Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:33.172Z
---

# Module 7 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Résumer les approches de contrôle de votre environnement AWS.
- Décrire les avantages d’Amazon CloudWatch.
- Décrire les avantages d’AWS CloudTrail.
- Décrire les avantages d’AWS Trusted Advisor.

```
video : xxxx.mkv
```
*Figure. Module introduction - Monitoring and analytics*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> En tant que propriétaire du café, je veux avoir un œil sur le café tout au long de la journée, pour m’assurer que tout se passe bien. Dans le café, je peux voir les gens prendre leur café, et tout se passe généralement bien. Mais je ne peux pas rester sur place pour surveiller la situation toute la journée. J’aimerais pouvoir quitter le café et éventuellement y revenir à la fin de la journée, et pouvoir vérifier les activités du magasin pendant que je n’y étais pas. 
> 
> Par exemple, je veux connaître le nombre de cafés vendus sur la journée. Le temps d'attente moyen avant qu'un client puisse commander un café. Et si nous avons manqué de stock pour n'importe quel produit. Mieux encore, j'aimerais pouvoir être automatiquement alertée si les temps d’attente sont trop longs. De cette façon, je peux appeler un autre employé ou me rendre sur place. Toutes les entreprises, y compris ce café, peuvent utiliser des métriques pour évaluer l’exécution des systèmes et des processus. 
> 
> Cette idée d'observer les systèmes, de collecter des métriques, d’évaluer ces métriques dans le temps, puis de les exploiter pour prendre des décisions ou des mesures, c’est ce qu’on appelle la surveillance. 
> 
> Il est important de configurer la surveillance dans le cloud. Grâce à la nature élastique des services AWS, qui se mettent dynamiquement à l’échelle, vous pouvez surveiller de près vos ressources AWS pour vous assurer que vos systèmes s’exécutent comme prévu. 
> 
> Par exemple, si une instance EC2 est surexploitée, vous pouvez déclencher un événement de mise à l’échelle qui lance automatiquement une autre instance EC2. Ou, si une application commence à envoyer des réponses d'erreur à un taux anormalement élevé, vous pouvez alerter un employé afin qu’il analyse ce qui se passe. 
> 
> Dans les prochaines vidéos, nous aborderons une variété d'outils qui vous aident à surveiller votre environnement AWS. Cette surveillance vous permettra de mesurer les performances de vos systèmes, vous avertira lorsque les chose ne se déroulent pas correctement, et vous aidera même à déboguer et à résoudre les problèmes à mesure qu’ils surviennent.
</div>


# Amazon CloudWatch

```
video : xxx.mkv
```
*Figure. Amazon CloudWatch*

<div class="collapseContent" display="Transcription" value="true">
  
>![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Nous préparons maintenant beaucoup de cafés et servons les clients, et tout semble se dérouler à merveille dans notre café. Toutefois, étant donné que nous utilisons de plus en plus les machines à expresso, nous utilisons des tasses, nous ouvrons et nous fermons en permanence de réfrigérateur, nous vous nous assurer d’être alertés avant que quelque chose tourne mal. Par exemple, il faut peut-être nettoyer ou réparer une machine à expresso. Le fait est, en tant que propriétaire d'une entreprise, vous devez avoir de la visibilité sur l’état de vos systèmes. Est-ce que les choses se passent bien ? Constatez-vous une dégradation de l'expérience client ? Y a-t-il fréquemment des erreurs de commandes, ou est-ce que chaque client reçoit bien sa commande ? Vous vous posez toutes sortes de questions concernant la réussite de vos opérations. 
> 
> La même idée s'applique aux systèmes sur AWS. Vous avez besoin d'un moyen de surveiller l’état et le fonctionnement de vos solutions. Heureusement, vous ne devez pas créer votre propre plateforme de surveillance. Nous l'avons fait pour vous. 
> 
> Je vous présente Amazon CloudWatch. CloudWatch surveille en temps réel votre infrastructure AWS et les applications que vous exécutez sur AWS. Il suit et contrôle les métriques. Considérez les métriques comme des variables de vos ressources. Par exemple, la quantité d'expressos préparés par une machine à expresso, ou l’utilisation du CPU par une instance. 
> 
> Prenons l'exemple de notre café. Supposons que la machine à expresso doit être nettoyée tous les 100 expressos. CloudWatch nous permet de créer une métrique personnalisée appelée Nombre d’expressos. Lorsque la métrique atteint 100, nous voulons alerter le personnel afin qu’il nettoie la machine. C'est simple, n'est-ce pas ? Eh bien, CloudWatch vous permet d'y parvenir en créant ce qu’on appelle une alarme CloudWatch. Vous définissez un seuil pour une métrique, et lorsque ce seuil est atteint, CloudWatch peut générer une alerte et enclencher une action. Cela signifie que nous pouvons créer une alerte pour la métrique personnalisée, dans ce cas 100, puis effectuer une action. 
> 
> Mieux encore, les alarmes CloudWatch sont intégrées à SNS. Nous pouvons donc envoyer un SMS au gestionnaire pour lui demander de nettoyer la machine. Vous pouvez créer toutes sortes d'alarmes personnalisées pour les métriques de tous les différents types de ressources AWS. 
> 
> Et si nous voulons regrouper toutes ces métriques pour obtenir un point de vue unique ? Eh bien, nous pouvons utiliser la fonction de tableau de bord CloudWatch. Imaginez un tableau de bord, sur un écran. qui répertorie les métriques pratiquement en temps réel. Dans notre cas, nous pouvons créer un tableau de bord CloudWatch qui répertoriera toutes nos machines à expresso et le nombre d’expressos afin que puissions les surveiller de manière proactive. Ces tableaux de bord s'actualiseraient automatiquement à leur ouverture, afin que nous puissions consulter nos ressources à jour. 
> 
> Enfin, quels sont les avantages de l'utilisation d'un service comme CloudWatch ? D'abord, vous pouvez accéder à toutes les métriques depuis un seul endroit. Cela vous permet de recueillir des métriques et des journaux depuis toutes vos ressources AWS, vos applications et vos services s’exécutant sur AWS et sur site, ce qui vous aide à diviser les silos de données, pour obtenir en toute simplicité une visibilité sur l’ensemble du système. 
> 
> Vous pouvez aussi obtenir de la visibilité sur vos applications, votre infrastructure, et vos services, et obtenir ainsi des informations sur votre pile distribuée afin d’établir la corrélation et visualiser les métriques et les journaux et ainsi identifier rapidement et résoudre les problèmes. (MTTR) et améliorer le coût total de possession (TCO). Dans notre café, si le MTTR des heures de nettoyage des machines de restaurant est moindre, nous pouvons faire des économies sur le TCO. Il s'agit en d'autres termes de libérer d'importantes ressources, comme développeurs, pour se concentrer sur l'accroissement de la valeur opérationnelle. 
> 
> Enfin, vous générez des informations pour optimiser les applications et les ressources opérationnelles. Vous pouvez, par exemple, agréger l'utilisation sur une flotte entière d'instances EC2 pour en tirer des informations opérationnelles et d'utilisation. Maintenant notre personnel peut s’occuper de servir les cafés, plutôt que nettoyer les machines en permanence avant que cela ne soit réellement nécessaire. 
> 
> Ça y est, vous connaissez les détails concernant CloudWatch. Si vous voulez bien m'excuser, je vais me rendre à la machine à café pour remplir mon café.
</div>


## Amazon CloudWatch

**[Amazon CloudWatch](https://aws.amazon.com/cloudwatch/)** est un service web qui vous permet de contrôler et de gérer diverses métriques et de configurer des actions d’alarme en fonction des données de ces métriques.

![aws_cloudwatch_01.png](/assets/img/clouds/tempo/aws_cloudwatch_01.png)

CloudWatch utilise des **[métriques](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)** pour représenter les points de données de vos ressources. Les services AWS envoient des métriques à CloudWatch. CloudWatch utilise ensuite ces métriques pour créer automatiquement des graphiques montrant l’évolution des performances au fil du temps.

## Alarmes CloudWatch

Avec CloudWatch, vous pouvez créer des **[alarmes](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)** qui effectuent automatiquement des actions lorsque la valeur de votre métrique est supérieure ou inférieure à un seuil prédéfini. 

Supposons par exemple que les développeurs de votre entreprise utilisent des instances Amazon EC2 à des fins de développement ou de test d’applications. Si les développeurs oublient parfois d’arrêter les instances, les instances continueront à s’exécuter, entraînant des frais. 

Dans ce scénario, vous pouvez créer une alarme CloudWatch qui arrête automatiquement une instance Amazon EC2 lorsque le pourcentage d’utilisation de CPU est inférieur à un certain seuil pendant une période spécifiée. Lors de la configuration de l’alarme, vous pouvez demander de recevoir une notification chaque fois que cette alarme est déclenchée.

## Tableau de bord CloudWatch

Tableau de bord CloudWatch affichant les mesures pour Amazon RDS, Amazon EC2 et Amazon EBS

![aws_cloudwatch_02.png](/assets/img/clouds/tempo/aws_cloudwatch_02.png)

La fonction de **[tableau de bord](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)** CloudWatch vous permet d’accéder à l’ensemble des métriques de vos ressources à partir d’un seul emplacement. Par exemple, vous pouvez utiliser un tableau de bord CloudWatch pour contrôler l’utilisation du CPU d’une instance Amazon EC2, le nombre total de demandes adressées à un compartiment Amazon S3, etc. Vous pouvez même personnaliser des tableaux de bord distincts à des fins métier, pour des applications ou des ressources différentes.

# AWS CloudTrail

```
video : xxx.mkv
```
*Figure. AWS CloudTrail*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> La caisse enregistreuse, l’un des premiers dispositifs d'autovérification du monde. Le principe est simple : la confiance tout en vérifiant. Votre magasin est géré par un vendeur en qui vous avez confiance, mais vous voulez vous assurer que l’argent de la caisse correspond aux ventes réelles. Chaque transaction est donc enregistrée et compilée. À la fin de la journée, vous connaissez les chiffres exacts de la caisse. 
> 
> La vérification des transactions informatiques est un élément essentiel dans la plupart des structures de conformité. Dans un centre de données physique, il y a tellement d’endroits où un humain peut, même par accident, effectuer des modifications sans que celles-ci ne soient enregistrées. Chez AWS, ce problème disparaît, car tout est programmatique. 
> 
> Je vous présente AWS CloudTrail, l’outil complet d’audit d’API. Le moteur est simple, chaque requête adressée à AWS, que ce soit pour lancer une instance EC2, ou ajouter une ligne à une table DynamoDB, ou modifier les autorisations d’un utilisateur. Chaque requête est enregistrée dans le moteur CloudTrail. Le moteur enregistre exactement la personne à l'origine de la requête, quel opérateur, quand a-t-il envoyé l’appel d’API ? Où se trouvait-il ? Son adresse IP, ainsi que la réponse. Est-ce que quelque chose a changé ? Quel est le nouvel état ? La requête a-t-elle été rejetée ? 
> 
> Du point de vue de l'audit, c’est fantastique. Imaginons que vous aviez recours à un auditeur qui vérifie et s’assure qu’aucune personne extérieure ne peut accéder à votre base de données. C'est une bonne chose. Vous créez un groupe de sécurité qui verrouille le trafic externe. Mais rappelez-vous qu'un administrateur racine dispose toujours des autorisations pour modifier ces paramètres. 
> 
> Comment prouvez-vous à l'auditeur que les paramètres du groupe de sécurité n’ont jamais changé ? La réponse est CloudTrail. CloudTrail peut enregistrer ces journaux indéfiniment dans des compartiments S3 sécurisés. En outre, avec des méthodes inviolables telles que le verrouillage de coffre, vous pouvez prouver la provenance absolue de tous ces journaux d’audit de sécurité critiques.
</div>

## AWS CloudTrail

**[AWS CloudTrail](https://aws.amazon.com/cloudtrail/)** enregistre les appels API sur votre compte. Les informations enregistrées incluent l’identité de l’utilisateur à l’origine de l’appel d’API, l’heure de l’appel d’API, l’adresse IP source de l’utilisateur ayant effectué l’appel d’API, etc. Vous pouvez comparer CloudTrail à une « traînée » de miettes de pain (ou un journal d’actions) que quelqu’un a laissée derrière lui.

![aws_cloudtrail_01.png](/assets/img/clouds/tempo/aws_cloudtrail_01.png)

Rappelez-vous que vous pouvez utiliser des appels d’API pour allouer, gérer et configurer vos ressources AWS. Avec CloudTrail, vous pouvez afficher un historique complet de l’activité des utilisateurs et des appels d’API pour vos applications et ressources. 

Les événements sont généralement mis à jour dans CloudTrail dans les 15 minutes suivant un appel d’API. Vous pouvez filtrer les événements en spécifiant l’heure et la date auxquelles un appel d’API s’est produit, l’utilisateur qui a demandé l’action, le type de ressource impliquée dans l’appel d’API, etc.

## Exemple : événement AWS CloudTrail

Supposons que le propriétaire du café navigue dans la section AWS Identity and Access Management (IAM) de l’AWS Management Console. Il découvre qu’un nouvel utilisateur IAM nommé Mary a été créé, mais il ne sait pas qui, quand ou quelle méthode a créé l’utilisateur.

Pour répondre à ces questions, le propriétaire accède à AWS CloudTrail.

![aws_cloudtrail_02.png](/assets/img/clouds/tempo/aws_cloudtrail_02.png)

Dans la section « Event History » (historique des événements) CloudTrail, le propriétaire applique un filtre pour afficher uniquement les événements de l’action d’API « CreateUser » (création utilisateur) dans IAM. Le propriétaire localise l’événement pour l’appel d’API qui a créé un utilisateur IAM pour Mary. Ce registre d’événement fournit des détails complets :

le 1er janvier 2020 à 9 h, l’utilisateur IAM John a créé un nouvel utilisateur IAM (Mary) par le biais de l’AWS Management Console.

## CloudTrail Insights

Dans CloudTrail, vous pouvez également activer **[CloudTrail Insights](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-insights-events-with-cloudtrail.html)**. Cette fonction facultative permet à CloudTrail de détecter automatiquement les activités API inhabituelles sur votre compte AWS. 

Par exemple, CloudTrail Insights peut détecter si un nombre plus élevé d’instances Amazon EC2 que d’habitude a récemment été lancé dans votre compte. Vous pouvez ensuite consulter tous les détails de l’événement pour déterminer les actions que vous devez effectuer ensuite.

## Contrôle des connaissances

**Quelles tâches pouvez-vous effectuer à l’aide d’AWS CloudTrail ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q1r1" name="question1" value="1">
  <label for="q1r1">Surveiller votre infrastructure et vos ressources AWS en temps réel</label><br>
  <input type="checkbox" id="q1r2" name="question1" value="2">
  <label for="q1r2">Suivre les activités des utilisateurs et les requêtes d’API dans l’ensemble de votre infrastructure AWS</label><br>
  <input type="checkbox" id="q1r3" name="question1" value="3">
  <label for="q1r3">Afficher les métriques et les graphiques pour surveiller les performances des ressources</label><br>
  <input type="checkbox" id="q1r4" name="question1" value="4">
  <label for="q1r4">Filtrer les journaux pour faciliter l’analyse opérationnelle et le dépannage</label><br>
  <input type="checkbox" id="q1r5" name="question1" value="5">
  <label for="q1r5">Configurer les actions et alertes automatiques en réponse aux métriques</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Suivre les activités des utilisateurs et les requêtes d’API dans l’ensemble de votre infrastructure AWS**
- **Filtrer les journaux pour faciliter l’analyse opérationnelle et le dépannage**

Les autres réponses sont des tâches que vous pouvez effectuer dans Amazon CloudWatch.

En savoir plus :

- [AWS CloudTrail](https://aws.amazon.com/cloudtrail)
</div>

# AWS Trusted Advisor

```
video : xxx.mkv
```
*Figure. AWS Trusted Advisor*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Lorsque vous gérez une entreprise, vous pouvez avoir besoin de conseillers externes qui vous disent : ce processus devrait être rationalisé. Ou, j'ai quelques bons conseils pour vous faire sur la manière d’économiser de l’argent sur vos frais généraux. Ou même, j'ai remarqué que je pouvais facilement entrer, aller derrière votre caisse et ouvrir le tiroir sans que personne ne le remarque. Ça n'est pas bien. 
> 
> Ce que je veux dire par là, parfois il est bon d’avoir quelqu’un qui connaît les bonnes pratiques du secteur, et sait ce qu’il faut rechercher, venir et vous dire ce qu’il faut changer pour être plus efficace, plus sûr, ou économiser de l’argent. 
> 
> AWS dispose d'un conseiller automatisé appelé AWS Trusted Advisor. C’est un service que vous pouvez utiliser dans votre compte AWS pour évaluer vos ressources par rapport à cinq piliers. Les piliers sont l’optimisation des coûts, les performances, la sécurité, la tolérance aux pannes et les quotas de service. Trusted Advisor effectue des vérifications en temps réel pour chaque pilier de votre compte, basées sur les bonnes pratiques AWS, et compile les éléments catégorisés afin que vous puissiez les examiner, et directement les consulter dans la console AWS. 
> 
> Certaines vérifications sont gratuites et incluses dans votre compte AWS, et d’autres sont disponibles selon le niveau de votre plan de support. Les vérifications comprennent, si vous n'avez pas activé la Multi-Factor Authentication pour votre utilisation racine, vous allez en être informé. Si vos instances EC2 sont sous-utilisées et qu’elles peuvent être désactivées pour économiser de l’argent, ou si vous avez des volumes EBS qui n’ont pas été sauvegardés dans un délai raisonnable, vous allez également en être informé. Afin de mieux comprendre, analysons AWS Trusted Advisor dans mon propre compte. 
> 
> Je suis déjà connecté à la console, et je vais saisir Trusted Advisor dans la barre de recherche. Cela me renvoie au tableau de bord de Trusted Advisor et je vais cliquer sur le premier pilier de l’optimisation des coûts pour connaître les résultats. Vous constatez tout de suite que le service m'indique trois niveaux d’éléments. Le cercle rouge représente l'action recommandée. Le triangle vert, qui implique une enquête recommandée. Et le carré vert, qui signifie qu'aucun problème n'a été détecté. 
> 
> Heureusement, je n'ai pas d'éléments rouges pour l’optimisation des coûts mais j’ai des éléments orange. Sous vérifications de l’optimisation des coûts, je peux en savoir plus sur chaque vérification exécutée par Trusted Advisor. Pour ce compte, j'ai quelques instances RDS qui sont inactives, et quelques instances EC2 et volumes EBS sous-utilisés. Je peux verticalement mettre à l'échelle ces instances en les diminuant pour économiser, ou si elles ne sont pas utilisées du tout je peux supprimer ces instances et ces volumes EBS. Je dois procéder à une petite enquête pour déterminer les actions à appliquer. 
> 
> Cliquons sur le pilier des performances. Ici, je n'ai pas d'avertissements, mais on peut quand même lire les détails des vérifications effectuées par Trusted Advisor. Comme celle-ci par exemple, qui indique des volumes EBS dont les performances pourraient avoir été affectées par la capacité de débit de l’instance EC2 à laquelle ils sont attachés. 
> 
> Ensuite, sélectionnons le pilier de sécurité. Dans ce compte de démonstration, il y a quatre alertes au niveau des actions recommandées. Trusted Advisor essaie de m’alerter en m'informant que mes stratégies de mots de passe pour les utilisateurs IAM sont faibles, que l’authentification multifacteur n’est pas activée pour l’utilisateur racine, et que des groupes de sécurité autorisent l’accès public aux instances EC2. Tous ces éléments compromettent les ressources de ce compte et devraient être traités le plus rapidement possible. 
> 
> Passons à la tolérance aux pannes. Pour ce pilier, quelques éléments sont considérés comme insuffisants. Tout d'abord, Trusted Advisor m'informe que qu’il existe des volumes EBS sans instantanés sur ce compte. Pour rappel, un instantané est une sauvegarde. Sans sauvegardes, en cas de défaillance des volumes EBS, je perdrais ces données. 
> 
> Une autre alerte concerne l'équilibrage entre les zones de disponibilité EC2, qui est au niveau des actions recommandées. Cela signifie que mes instances EC2 ne sont pas correctement lancées sur les zones de disponibilité. En cas de problème sur une zone de disponibilité, mon application pourrait subir une panne. Le déploiement entre les zones de disponibilité serait la réponse à ce problème. 
> 
> Enfin, passons aux limites de services. Ce pilier vous indique lorsque vous approchez ou atteignez les limites de services AWS. Nombre de quotas de services sont des quotas flexibles, ce qui signifie qu’il existe des restrictions à lever dans une certaine mesure. Il est intéressant de savoir lorsque vous vous approchez l’une de ces limites, et lorsqu’il est temps de soumettre un ticket de support à AWS pour les changer. Dans ce compte, je peux constater que j'ai cinq VPC, ce qui est la limite régionale. Cette limite de service est donc atteinte. 
> 
> Trusted Advisor peut vous orienter dans la bonne direction en ce qui concerne les cinq piliers. Vous pouvez configurer des alertes par e-mail pour la facturation, les opérations, et les contacts de sécurité, selon les vérifications de votre compte. Assurez-vous d'activer Trusted Advisor pour commencer à prendre des mesures et optimiser votre compte AWS.
</div>

## AWS Trusted Advisor

**[AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)** est un service web qui inspecte votre environnement AWS et fournit des recommandations en temps réel, conformément aux bonnes pratiques AWS.

Trusted Advisor compare ses résultats aux bonnes pratiques AWS selon cinq catégories : l’optimisation des coûts, les performances, la sécurité, la tolérance aux pannes et les limites de service. Pour les vérifications de chaque catégorie, Trusted Advisor propose une liste d’actions recommandées et des ressources supplémentaires pour en savoir plus sur les bonnes pratiques AWS. 

Les conseils fournis par AWS Trusted Advisor peuvent bénéficier à votre entreprise, à toutes les étapes du déploiement. Par exemple, vous pouvez utiliser AWS Trusted Advisor pour vous aider lors de la création de nouveaux flux de travail et du développement de nouvelles applications. Vous pouvez également l’utiliser pendant que vous apportez des améliorations continues aux applications et aux ressources existantes.

## Tableau de bord AWS Trusted Advisor

![aws_truster_advisor_01.jpg](/assets/img/clouds/tempo/aws_truster_advisor_01.jpg =750x)

Tableau de bord AWS Trusted Advisor indiquant le nombre d’éléments sans problème détectés, les enquêtes et actions recommandées pour les catégories Optimisation des coûts, Performance, Sécurité, Tolérance aux pannes et Limites de service. L’optimisation des coûts indique des économies mensuelles potentielles de 7 516,85 USD.
Lorsque vous accédez au tableau de bord Trusted Advisor sur l’AWS Management Console, vous pouvez consulter les vérifications effectuées pour l’optimisation des coûts, les performances, la sécurité, la tolérance aux pannes et les limites de service.

Pour chaque catégorie :

- La coche verte indique le nombre d’éléments pour lesquels **aucun problème n’a été détecté**.
- Le triangle orange représente le nombre d’**enquêtes recommandées**.
- Le cercle rouge représente le nombre d’**actions recommandées**.


# Module 7 : résumé

Dans le module 7, vous avez découvert les concepts suivants :

- Amazon CloudWatch
- AWS CloudTrail
- AWS Trusted Advisor

```
video : xxx.mkv
```
*Figure. Module summary - Monitoring and analytics*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Comprendre ce qui se passe dans votre environnement est essentiel pour maintenir des applications efficaces, sûres et conformes. 
> 
> Nous avons expliqué comment CloudWatch peut offrir une compréhension pratiquement en temps réel de la manière dont votre système se comporte y compris vous alerter en cas de conditions qui nécessitent votre attention. CloudWatch vous permet également d’examiner ces métriques dans le temps à mesure que vous réglez votre système pour des performances maximales. 
> 
> Nous avons expliqué comment CloudTrail vous permet de savoir exactement qui a fait quoi, quand et où. Il répond à toutes vos questions d'audit AWS, sauf le « pourquoi ». 
> 
> Enfin, nous avons parlé de Trusted Advisor, qui compile plus de 40 préoccupations courantes en matière de coûts, de performances, de sécurité et de résilience le tout dans un tableau de bord exploitable. 
> 
> Il y a évidemment de nombreux autres outils de surveillance et d’analyse à votre disposition, mais cela donne un aperçu de la variété des services qu’AWS offre à votre entreprise.
</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été abordés dans le module 7, consultez ces ressources.

- [Gestion et gouvernance sur AWS](https://aws.amazon.com/products/management-tools)
- [Contrôle et observabilité](https://aws.amazon.com/products/management-tools/use-cases/monitoring-and-observability/)
- [Configuration, conformité et audit](https://aws.amazon.com/products/management-tools/use-cases/configuration-compliance-and-auditing/)
- [Blog sur la gestion et la gouvernance AWS](https://aws.amazon.com/blogs/mt/)
- [Livre blanc : gouvernance AWS à grande échelle](https://docs.aws.amazon.com/whitepapers/latest/aws-governance-at-scale/introduction.html)

# Module 7 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelles actions pouvez-vous effectuer à l’aide d’Amazon CloudWatch ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q2r1" name="question2" value="1">
  <label for="q2r1">Contrôler l’utilisation et les performances de vos ressources</label><br>
  <input type="checkbox" id="q2r2" name="question2" value="2">
  <label for="q2r2">Recevoir des conseils en temps réel pour améliorer votre environnement AWS</label><br>
  <input type="checkbox" id="q2r3" name="question2" value="3">
  <label for="q2r3">Comparer votre infrastructure aux bonnes pratiques AWS en cinq catégories</label><br>
  <input type="checkbox" id="q2r4" name="question2" value="4">
  <label for="q2r4">Accéder aux métriques à partir d’un tableau de bord unique</label><br>
  <input type="checkbox" id="q2r5" name="question2" value="5">
  <label for="q2r5">Détecter automatiquement les activités inhabituelles du compte</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Contrôler l’utilisation et les performances de vos ressources**
- **Accéder aux métriques à partir d’un tableau de bord unique**

Les autres réponses sont incorrectes, car :

- Recevoir des recommandations en temps réel pour améliorer votre environnement AWS peut être effectué par le biais d’AWS Trusted Advisor.
- Comparer votre infrastructure aux bonnes pratiques AWS selon les cinq catégories peut être effectué par le biais d’AWS Trusted Advisor.
- Détecter automatiquement les activités inhabituelles du compte peut être effectué par le biais d’AWS CloudTrail.

En savoir plus :

- [Amazon CloudWatch](https://aws.amazon.com/cloudwatch)
</div>

**Quel service vous permet de vérifier la sécurité de vos compartiments Amazon S3 en vérifiant les autorisations d’accès libre ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Amazon CloudWatch</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">AWS CloudTrail</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">AWS Trusted Advisor</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Amazon GuardDuty</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Trusted Advisor**.

AWS Trusted Advisor est un service web qui inspecte votre environnement AWS et fournit des recommandations en temps réel, conformément aux bonnes pratiques AWS. L’inspection inclut des contrôles de sécurité, comme les compartiments Amazon S3 avec des autorisations d’accès libre.

Les autres réponses sont incorrectes, car :

- Amazon CloudWatch est un service web qui vous permet de contrôler et de gérer diverses métriques pour les ressources qui exécutent vos applications.
- AWS CloudTrail est un service web qui vous permet de consulter les détails des activités des utilisateurs et des appels d’API qui ont eu lieu dans votre environnement AWS.
- Amazon GuardDuty est un service qui assure une détection intelligente des menaces pour votre environnement et vos ressources AWS. Il identifie les menaces en surveillant en continu l’activité du réseau et le comportement des comptes au sein de votre environnement AWS.

En savoir plus :

- [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor)
</div>

**Quelles sont les catégories incluses dans le tableau de bord AWS Trusted Advisor ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q4r1" name="question4" value="1">
  <label for="q4r1">Fiabilité</label><br>
  <input type="checkbox" id="q4r2" name="question4" value="2">
  <label for="q4r2">Performances</label><br>
  <input type="checkbox" id="q4r3" name="question4" value="3">
  <label for="q4r3">Mise à l’échelle</label><br>
  <input type="checkbox" id="q4r4" name="question4" value="4">
  <label for="q4r4">Élasticité</label><br>
  <input type="checkbox" id="q4r5" name="question4" value="5">
  <label for="q4r5">Tolérance aux pannes</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **Performances**
- **Tolérance aux pannes**

AWS Trusted Advisor inspecte en continu votre environnement AWS et fournit des recommandations sur les bonnes pratiques selon cinq catégories : l’optimisation des coûts, les performances, la sécurité, la tolérance aux pannes et les limites de service.

En savoir plus :

- [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor)
</div>

Le module suivant examine la tarification et le support AWS.