---
title: Module 10 - La transition vers le cloud
description: 
published: true
date: 2023-04-21T10:55:34.434Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:55:28.906Z
---

# Module 10 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Résumer les cinq piliers du framework Well-Architected.  
- Expliquer les six avantages du cloud computing.

```
video : xxx.mkv
```
*Figure. Module introduction - The Cloud Journey*

<div class="collapseContent" display="Transcription" value="true">


> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Nous avons abordé beaucoup de différents services AWS dans ce cours. Comme vous le savez, vous utilisez chaque service individuel en tant que module de service pour vos solutions. Il y a un nombre infini d'architectures que vous pouvez créer pour résoudre n’importe quel problème sur AWS. d’une manière simple ou compliquée. C'est bien d'avoir beaucoup d'options, mais comment savoir si l’architecture que vous avez créée est bien ? 
> 
> Analysons cette architecture de base à trois niveaux. Est-ce qu'elle a l'air bien ? Il y a un équilibreur de charge, quelques instances, et une base de données backend. Ça me semble bien, mais qu'en est-il de cette architecture ? Cette architecture a été répliquée sur des zones de disponibilité ou AZ. C'est important pour la fiabilité. Si une AZ rencontre des problèmes, votre application reste opérationnelle dans la deuxième AZ. Il est important de pouvoir repérer les défaillances dans les architectures comme cet exemple simple. 
> 
> Mais tous les exemples ne sont pas aussi simples. Heureusement, il y a des outils pour vous aider. Nous allons parler du Well-Architected Framework. C'est un outil qui vous permet d'évaluer les architectures que vous créez pour en mesurer l’excellence selon différentes catégories. Well-Architected Framework a des piliers et ces piliers sont l'excellence opérationnelle, la sécurité, la fiabilité, l'efficacité des performances et l'optimisation des coûts.
</div>

# Le framework AWS Well-Architected

```
video : xxx.mkv
```
*Figure. The AWS Well-Architected Framework*


<div class="collapseContent" display="Transcription" value="true">


> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> AWS Well-Architected Framework est conçu pour permettre aux architectes, aux développeurs et aux utilisateurs d’AWS de créer une infrastructure sécurisée, très performante, résiliente et efficace pour leurs applications. Il se compose de cinq piliers pour assurer une approche cohérente pour l’évaluation et la conception d’architectures. 
> 
> Le premier pilier est l’excellence opérationnelle. Ce pilier est axé sur l'exécution et la supervision des systèmes afin de créer une valeur opérationnelle et, par-là, de perfectionner en permanence les processus et les procédures. Par exemple, automatiser les modifications avec des pipelines de déploiement ou répondre aux événements déclenchés. 
> 
> Le deuxième pilier est la sécurité. Comme vous le savez, la première des priorités chez AWS. Ce pilier l'illustre parfaitement, en vérifiant l’intégrité des données et par exemple en protégeant les systèmes en utilisant le chiffrement. 
> 
> Le troisième pilier est la fiabilité. Il se concentre sur la planification des restaurations, comme la reprise après une interruption d’Amazon DynamoDB. Ou un défaut du nœud EC2, et sur la façon dont vous gérez les changements pour répondre à la demande des entreprises et des clients. 
> 
> Le quatrième pilier est l’efficacité des performances et il est axé sur l'utilisation efficace des ressources informatiques. Par exemple, utiliser le type Amazon EC2 approprié en fonction des exigences de la charge de travail et de la mémoire, pour prendre des décisions éclairées, et maintenir l’efficacité à mesure que les besoins des entreprises évoluent. 
> 
> Enfin, il y a l'optimisation des coûts, qui vise à optimiser l'ensemble des coûts. Il s'agit de contrôler les dépenses et, par exemple, de vérifier si vous avez surestimé la taille du serveur EC2. Vous pouvez alors réduire les coûts en choisissant une taille plus rentable. 
> 
> Auparavant, vous auriez dû évaluer votre infrastructure AWS avec l'aide d'un architecte de solutions. Non pas que vous ne pouviez pas et que vous n’êtes plus encouragé à le faire, mais nous écoutons les retours des clients, et nous avons décidé de lancer le Framework en tant qu’outil en libre-service, l’outil Well-Architected. Vous pouvez y accéder via AWS Management Console. Créez une charge de travail et exécutez-la dans votre compte AWS. Pour générer un rapport indiquant les éléments à traiter. 
> 
> Ça ressemble un peu à un système de feux de circulation. Le vert indique que tout est OK, et que vous pouvez continuer. Orange suggère d’examiner l’élément car il peut être amélioré. Et rouge, et bien vous devez examiner l’élément car il existe des risques. L'outil indique les potentiels problèmes qu'il a détectés et vous présente un plan d’architecture en se basant sur les bonnes pratiques. J’ajoute que vous pouvez toujours remplacer ces paramètres si cela ne s’applique pas à votre situation. C'est très personnalisable. Petite anecdote intéressante : en Afrique du Sud, mon pays d’origine, nous appelons les feux de circulation des robots. 
> 
> En ce qui concerne l'outil lui-même, il ressemble à l’écran suivant. Comme je l'ai dit, nous nommons notre charge de travail, et c’est ce qui apparaît dans la section une. La deuxième section répertorie les piliers et les listes déroulantes comprenant les questions de chaque pilier. La section trois présente les questions elles-mêmes. La section quatre indique le pilier et la question. La section cinq reprend le contexte et la recommandation. La section six concerne votre réponse ; vous pouvez activer ou désactiver la question en fonction de son applicabilité. C'est important, car cela affecte votre score global. Je ne peux pas oublier la section sept, qui comporte de courtes vidéos qui expliquent comment répondre à une question particulière. 
> 
> Bref, j'ai présenté le framework Well-Architected et l’outil. J'espère que vous avez apprécié apprendre comment évaluer vos charges de travail.
</div>

## Le framework AWS Well-Architected

Le **[Framework AWS Well-Architected](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)** vous aide à comprendre comment concevoir et exploiter des systèmes fiables, sécurisés, efficaces et rentables dans AWS Cloud. Il vous permet d’évaluer vos architectures de manière cohérente par rapport aux bonnes pratiques et aux principes de conception ainsi que d’identifier les points à améliorer.

![aws_well-architected_01.png](/assets/img/clouds/tempo/aws_well-architected_01.png)

Le framework Well-Architected est basé sur cinq piliers : 

- Excellence opérationnelle
- Sécurité
- Fiabilité
- Efficacité des performances
- Optimisation des coûts

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Excellence opérationnelle" value="true">

**L’excellence opérationnelle** constitue la capacité à exécuter et à surveiller les systèmes pour assurer une valeur opérationnelle et pour améliorer en continu les processus et procédures de prise en charge.  

Les principes de conception pour l’excellence opérationnelle dans le cloud incluent l’exécution d’opérations sous forme de code, l’annotation de documentation, l’anticipation des défaillances et l’application fréquente de petites modifications réversibles.
</div>

<div class="collapseContent" display="Sécurité" value="true">

Le **pilier sécurité** représente la capacité à protéger les informations, les systèmes et les ressources tout en fournissant une valeur opérationnelle par le biais de stratégies d’évaluation et de limitation des risques.

Lorsque vous étudiez la sécurité de votre architecture, appliquez les bonnes pratiques suivantes :

- Automatiser les bonnes pratiques de sécurité lorsque cela est possible
- Appliquer la sécurité à toutes les couches
- Protéger les données en transit et au repos
</div>

<div class="collapseContent" display="Fiabilité" value="true">

**La fiabilité** est la capacité d’un système à effectuer les opérations suivantes :

- Récupérer après les interruptions de service ou d’infrastructure
- Acquérir dynamiquement des ressources de calcul pour répondre à la demande
- Atténuer les perturbations telles que les erreurs de configuration ou les problèmes de réseau temporaires

La fiabilité comprend le test des procédures de reprise, la mise à l’échelle horizontale pour renforcer la disponibilité globale du système, ainsi que la reprise automatique après une défaillance.
</div>

<div class="collapseContent" display="Efficacité des performances" value="true">

**L’efficacité des performances** constitue la capacité à utiliser efficacement les ressources de calcul pour répondre aux exigences du système et maintenir l’efficacité au fur et à mesure que la demande change et que les technologies évoluent. 

Pour évaluer l’efficacité des performances de votre architecture, il faut expérimenter plus souvent, utiliser des architectures sans serveur et concevoir des systèmes pouvant vous faire passer à l’international en quelques minutes.
</div>

<div class="collapseContent" display="Optimisation des coûts" value="true">

**L’optimisation des coûts** est la capacité à exécuter des systèmes de façon à assurer une valeur opérationnelle au prix le plus bas.

L’optimisation des coûts inclut l’adoption d’un modèle de consommation, l’analyse et l’attribution des dépenses, et l’utilisation de services gérés pour réduire le coût de possession.
</div>

## Contrôle des connaissances

**Quel pilier du framework AWS Well-Architected met l’accent sur la capacité d’une charge de travail à exécuter de manière cohérente et appropriée les fonctions prévues ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Excellence opérationnelle</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Efficacité des performances</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Sécurité</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Fiabilité</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Fiabilité**.

Les autres réponses sont incorrectes, car :

- Le pilier Excellence opérationnelle inclut la capacité à exécuter efficacement des charges de travail, à obtenir des informations sur leurs opérations et à améliorer continuellement les processus de prise en charge pour assurer une valeur opérationnelle.
- Le pilier Efficacité des performances met l’accent sur l’utilisation efficace des ressources de calcul pour répondre aux exigences du système et pour maintenir cette efficacité à mesure que la demande change et que les technologies évoluent.
- Le pilier Sécurité comprend la protection des données, des systèmes et des ressources, ainsi que l’utilisation des technologies cloud pour améliorer la sécurité de vos charges de travail.

En savoir plus :

- [Livre blanc : AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
</div>

# Avantages de AWS Cloud

```
video : xxx.mkv
```
*Figure. Benefits of the AWS Cloud*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Bien. Maintenant que vous avez appris tout ce qu'il faut savoir sur AWS, vous voilà prêt à construire votre infrastructure. Donc même si ce n'est pas le cas, vous devriez maintenant bien comprendre les différents services proposés par AWS et comment vous pouvez les utiliser comme des blocs de construction pour créer des solutions pour votre entreprise, évolutives flexibles et fiables. Maintenant, vous devriez aussi comprendre une partie de la terminologie utilisée par AWS. Il est important de connaître cette terminologie pour progresser dans votre transition vers le cloud. Comme vous lisez les articles de blog ou la documentation AWS, vous devriez maintenant très bien connaître les acronymes et les phrases que vous verrez. Lors de votre transition, vous devriez également vous rappeler des six principaux avantages liés à l’utilisation du cloud AWS. Vous commencerez à évaluer quelles applications et solutions seront intégrées dans le cloud. Le fait de connaître ces avantages peut véritablement vous aider à décider en toute connaissance de cause. 
> 
> C'est un point très important à retenir. Lorsque vous construisez des centres de données traditionnels sur site, vous avez besoin d’investir énormément d’argent dès le départ rien que pour commencer. Vous devez investir de l'argent pour l’espace physique réel du centre de données, tout le matériel, le personnel qui rangera et empilera ce matériel et les frais généraux liés à l’exécution du centre de données. Pour les moyennes et grandes entreprises, cela peut représenter des centaines de milliers ou des millions de dollars. Quelle que soit l'utilisation de ce centre de données, vous avez un coût fixe et vous pouvez configurer votre budget en fonction de ce coût fixe. La facturation avec AWS est fondamentalement différente du modèle de facturation classique dont nous venons de parler. Votre facture avec AWS variera tous les mois en fonction de votre consommation des ressources. Maintenant, le plus important à ce propos est que si vous venez de commencer vous n’avez pas besoin de débourser un million de dollars pour investir dans votre environnement AWS. Au lieu de cela, vous pouvez commencer petit à petit, payer uniquement pour ce que vous utilisez et plus vous utilisez de ressources au fil du temps plus votre facture évoluera en fonction de cette croissance. Maintenant, si votre facture dépasse le budget que vous avez prévu, vous pouvez économiser de l’argent en désactivant les instances inutilisées, en effaçant d’anciennes ressources que vous n’utilisez pas, en optimisant vos applications, en utilisant des outils comme AWS Trusted Advisor pour vous aider à trouver comment économiser de l’argent. Si votre facture était élevée un certain mois, cela ne veut pas dire que vous ne pouvez pas payer moins le mois suivant. Cela ne vaut pas seulement pour la facturation des centres de données sur site. Voici le premier avantage. Le deuxième avantage est celui-ci : 
> 
> Profitez d'importantes économies d'échelle. Cet avantage est plutôt simple. AWS construit d'importantes capacités, partout dans le monde. Afin de construire tous ces centres de données AWS achète d'énormes quantités de matériel. AWS est également un expert en termes de création de centres de données efficaces. Nous pouvons acheter le matériel à un prix inférieur en raison de cet important volume, puis nous l’installons et l’exécutons efficacement. Grâce à ces éléments, obtenez un coût variable moins élevé que celui que vous auriez en exploitant votre propre centre de données. Passons au troisième avantage : 
> 
> Maîtrisez la capacité nécessaire. Lorsque vous construisez un centre de données classique, vous devez estimer la capacité dont vous aurez besoin pour votre entreprise. Prenons un exemple : vous estimez que vous aurez une base d'utilisateurs de 10 millions de personnes pour les trois prochaines années. Vous achetez suffisamment de matériel pour soutenir cette croissance dans le temps puis (inspire et expire) il s’avère que vous avez seulement environ 500 000 utilisateurs beaucoup moins que ce à quoi vous vous attendiez. Eh bien, devinez quoi ? Vous êtes toujours coincé avec ce coût et ce matériel pour les 10 millions d’utilisateurs. Vous avez surestimé votre capacité. A contrario, si vous sous-estimez votre capacité, et vous devez maintenant vous arranger pour développer votre centre de données, et gérer la capacité supplémentaire avant que vos clients n’aient une mauvaise expérience ou avant que vous perdiez des clients. Cela prend du temps, et il est très probable que vous ne suivrez pas, ou vous devrez faire un effort surhumain pour y parvenir. Maîtriser votre capacité à l'avance peut être problématique si vous la surestimez ou si vous la sous-estimez. Avec AWS, vous n'avez pas besoin de deviner votre capacité. Provisionnez plutôt les ressources dont vous avez besoin actuellement, puis utilisez la mise à échelle pour chaque ressource pour les mettre à l’échelle vers le haut ou vers le bas, en fonction de la capacité dont vous avez réellement besoin jour après jour, étant donné que la mise à l’échelle se fait en quelques minutes avec AWS au lieu de semaines ou de mois avec des ressources sur site. L'avantage suivant est celui que je préfère. 
> 
> Gagnez en vitesse et en agilité. Avec AWS, vous pouvez facilement essayer de nouvelles choses. Vous pouvez basculer entre plusieurs environnements de test et expérimenter pour découvrir de nouvelles façons de résoudre un problème. Et si cette approche ne fonctionne pas, vous pouvez simplement supprimer ces ressources et cesser d’encourir des coûts. Les centres de données classiques n'offrent pas cette même flexibilité. Dès lors, si vous voulez mener une expérience qui nécessitera l’achat et l’installation de nouveaux serveurs, un échec de l’expérience vous coûtera énormément coût car vous aurez déjà acheté le serveur. La flexibilité d'AWS permet de stimuler l'innovation, ainsi que la facilité de mise en service des ressources. L'installation d'une nouvelle base de données sur site peut prendre des semaines, et chez AWS, elle prend quelques minutes. Cette capacité à obtenir rapidement les ressources dont vous avez besoin vous aide à accéder plus vite au marché. Bien. Passons à l'avantage suivant : 
> 
> Ne dépensez plus d'argent pour l'exécution et la maintenance des centres de données. Si vous n'êtes pas une entreprise de centres de données, pourquoi dépenser autant d’argent et de temps pour l’exécution des centres de données ? Laissez AWS gérer les lourdes tâches indifférenciées et concentrez-vous plutôt sur ce qui permet à votre entreprise de tirer son épingle du jeu. En quoi votre offre est-elle meilleure que celle de vos concurrents ? AWS vous permet de vous concentrer sur cette question et de satisfaire vos clients en proposant de nouvelles solutions innovantes, au lieu de vous concentrer sur l’exécution du matériel dans un centre de données. Et pour terminer. 
> 
> Déploiement à l'international en quelques minutes. Imaginons : vous êtes une entreprise basée aux États-Unis et vous souhaitez développer vos activités en Allemagne. Vous possédez des centres de données aux États-Unis, qui desservent cette région, mais vous aurez besoin de centres de données en Allemagne pour desservir cette région. Traditionnellement, vous auriez besoin de personnel en Allemagne pour faire fonctionner le centre de données pour vous. Avec AWS, vous pouvez simplement répliquer votre architecture dans une région en Allemagne. Et si la configuration est effectuée correctement, vous pouvez le faire de manière automatisée en utilisant des outils comme AWS CloudFormation. Vous aurez peut-être besoin de plusieurs mois ou années pour vous développer dans une autre région du monde. Avec AWS, cela ne prend que quelques minutes. 
> 
> C'est tout. Vous avez découvert les six principaux avantages de l'utilisation d'AWS. Toute l'équipe AWS vous félicite pour avoir terminé la formation AWS Cloud Practitioner Essentials. 
> 
> Si vous voulez en savoir encore plus à ce sujet, consultez les autres cours que nous proposons pour plus de renseignements sur les produits et services AWS. 
> 
> Nous vous souhaitons beaucoup de réussite pour votre transition vers le cloud.
</div>

## Avantages du cloud computing

Le fait d’opérer dans AWS Cloud offre de nombreux avantages par rapport au calcul dans des environnements sur site ou hybrides. 

Dans cette section, vous découvrirez six avantages du cloud computing :

- Remplacer les dépenses initiales par les dépenses variables
- Bénéficier d’importantes économies d’échelle
- Maîtriser la capacité nécessaire
- Gagner en vitesse et en agilité
- Ne plus dépenser d’argent pour l’exécution et la maintenance des centres de données
- Passer à l’international en quelques minutes

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Remplacer les dépenses initiales par les dépenses variables" value="true">

Les dépenses initiales incluent les centres de données, les serveurs physiques et autres ressources dans lesquelles vous devez investir avant d’utiliser les ressources de calcul. 

Plutôt que d’investir massivement dans des centres de données et des serveurs avant de savoir comment vous allez les utiliser, vous payez uniquement lorsque vous consommez des ressources de calcul.
</div>

<div class="collapseContent" display="Bénéficier d’importantes économies d’échelle" value="true">
 
A l’aide du cloud computing, vous pouvez obtenir un coût variable moins élevé que celui que vous auriez de votre côté. 

Étant donné que l’utilisation par des centaines de milliers de clients est agrégée dans le cloud, les fournisseurs, tels qu’AWS, peuvent réaliser de plus grandes économies d’échelle. Les économies d’échelle font baisser les prix de la tarification à l’utilisation.
</div>

<div class="collapseContent" display="Maîtriser la capacité nécessaire" value="true">

Grâce au cloud computing, vous n’avez pas à prédire la capacité d’infrastructure dont vous aurez besoin avant de déployer une application.

Par exemple, vous pouvez lancer des instances Amazon Elastic Compute Cloud (Amazon EC2) si nécessaire et payer uniquement le temps de calcul que vous utilisez. Au lieu de payer des ressources qui ne sont pas utilisées ou dont la capacité est limitée, vous pouvez accéder uniquement à la capacité dont vous avez besoin et exécuter une mise à l’échelle en fonction de la demande. 
</div>

<div class="collapseContent" display="Gagner en vitesse et en agilité" value="true">

La flexibilité du cloud computing facilite le développement et le déploiement d’applications.

Cette flexibilité offre également à vos équipes de développement plus de temps pour expérimenter et innover.
</div>

<div class="collapseContent" display="Ne plus dépenser d’argent pour l’exécution et la maintenance des centres de données" value="true">

Le cloud computing dans les centres de données exige souvent que vous consacriez plus d’argent et de temps à gérer l’infrastructure et les serveurs. 

L’un des avantages du cloud computing est la capacité de se concentrer moins sur ces tâches et plus sur vos applications et vos clients.
</div>

<div class="collapseContent" display="Passer à l’international en quelques minutes" value="true">

La présence mondiale de AWS Cloud vous permet de rapidement déployer des applications auprès des clients du monde entier, tout en leur offrant une faible latence.
</div>

## Contrôle des connaissances

**Quel processus confère d’importantes économies d’échelle ?**

<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">Déploiement d’une application dans plusieurs régions du monde</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Tarification à l’utilisation moindre en raison de l’utilisation groupée des services par les clients AWS</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Paiement du temps de calcul au fur et à mesure de votre utilisation, plutôt que d’investir des frais initiaux dans des centres de données</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">Mise à l’échelle de la capacité de vos infrastructures pour répondre à la demande</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Tarification à l’utilisation moindre en raison de l’utilisation groupée des services par les clients AWS**.

Étant donné que l’utilisation par des centaines de milliers de clients est agrégée dans le cloud, les fournisseurs, tels qu’AWS, peuvent réaliser de plus grandes économies d’échelle. Les économies d’échelle font baisser les prix de la tarification à l’utilisation. 

Les autres réponses sont incorrectes, car :

- Déploiement d’une application dans plusieurs régions du monde : il s’agit d’un exemple de l’avantage Passer à l’international en quelques minutes.
- Paiement du temps de calcul au fur et à mesure de votre utilisation, plutôt que d’investir des frais initiaux dans des centres de données : il s’agit d’un exemple de l’avantage Remplacer les dépenses initiales par les dépenses variables.
- Mise à l’échelle de la capacité de vos infrastructures pour répondre à la demande : il s’agit d’un exemple de l’avantage Maîtriser la capacité nécessaire.

En savoir plus :

- [Six avantages du cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
</div>

## Etapes suivantes

Prenez le temps de passer en revue les étapes restantes du cours :

1. Passer en revue le résumé et les questions du quiz du **module 10**.
1. Parcourir le **module 11** pour en savoir plus sur la structure et les stratégies de l’examen AWS Certified Cloud Practitioner.
1. Tester vos connaissances des concepts clés abordés dans le cadre de ce cours en effectuant l’**évaluation finale**.
1. Répondre à l’**enquête** pour partager vos commentaires concernant le cours.

# Module 10 : résumé

Dans le module 10, vous avez découvert les concepts suivants :

- Les cinq piliers du framework AWS Well-Architected :
  + Excellence opérationnelle
  + Sécurité
  + Fiabilité
  + Efficacité des performances
  + Optimisation des coûts
- Six avantages du cloud computing :
  + Remplacer les dépenses initiales par les dépenses variables.
  + Bénéficier d’importantes économies d’échelle.
  + Maîtriser la capacité nécessaire.
  + Gagner en vitesse et en agilité.
  + Ne plus dépenser d’argent pour l’exécution et la maintenance des centres de données.
  + Passer à l’international en quelques minutes.
  
## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été abordés dans le module 10, consultez ces ressources.

- [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/)
- [Livre blanc : AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
- [Centre AWS Architecture](https://aws.amazon.com/architecture)
- [Six avantages du cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
- [Blog sur l’architecture AWS](https://aws.amazon.com/blogs/architecture)

# Module 10 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quel pilier du framework AWS Well-Architected inclut la capacité à efficacement exécuter des charges de travail et à obtenir des informations sur leurs opérations ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Optimisation des coûts</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">Excellence opérationnelle</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">Efficacité des performances</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Fiabilité</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Excellence opérationnelle**.

Les autres réponses sont incorrectes, car :

- Le pilier Optimisation des coûts se concentre sur la capacité à exécuter des systèmes pour assurer une valeur opérationnelle au prix le plus bas.
- Le pilier Efficacité des performances met l’accent sur l’utilisation efficace des ressources de calcul pour répondre aux exigences du système et maintenir cette efficacité à mesure que la demande change et que les technologies évoluent.
- Le pilier Fiabilité met l’accent sur la capacité d’une charge de travail à exécuter de manière cohérente et appropriée ses fonctions prévues.

En savoir plus :

- [Livre blanc : AWS Well-Architected Framework](https://d1.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf)
</div>

**Quels sont les avantages du cloud computing ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q4r1" name="question4" value="1">
  <label for="q4r1">Gagnez en vitesse et en agilité.</label><br>
  <input type="checkbox" id="q4r2" name="question4" value="2">
  <label for="q4r2">Bénéficier d’économies d’échelle moindres.</label><br>
  <input type="checkbox" id="q4r3" name="question4" value="3">
  <label for="q4r3">Remplacer les dépenses initiales par les dépenses variables.</label><br>
  <input type="checkbox" id="q4r4" name="question4" value="4">
  <label for="q4r4">Maintenir une capacité d’infrastructures.</label><br>
  <input type="checkbox" id="q4r5" name="question4" value="5">
  <label for="q4r5">Ne dépensez plus d’argent pour l’exécution et la maintenance des centres de données.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes : 

- **Gagner en vitesse et en agilité.**
- **Ne plus dépenser d’argent pour l’exécution et la maintenance des centres de données.**

Les six avantages du cloud computing sont :

- Remplacer les dépenses initiales par les dépenses variables
- Bénéficier d’importantes économies d’échelle
- Cesser de deviner la capacité nécessaire
- Gagner en vitesse et en agilité
- Ne plus dépenser d’argent pour l’exécution et la maintenance des centres de données
- Passer à l’international en quelques minutes

En savoir plus :

- [Six avantages du cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.html)
</div>

Le module suivant donne un aperçu de l’examen AWS Certified Cloud Practitioner.