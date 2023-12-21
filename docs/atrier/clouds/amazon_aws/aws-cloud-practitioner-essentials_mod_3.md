---
title: Module 3 - Infrastructure mondiale et fiabilité
description: 
published: true
date: 2023-04-21T10:56:01.689Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:55:55.398Z
---

# Module 3 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Résumer les avantages de l’infrastructure mondiale AWS
- Décrire le concept de base des zones de disponibilité
- Décrire les avantages des emplacements périphériques et d’Amazon CloudFront
- Comparer différentes méthodes de approvisionnement des services AWS

```
video : xxx.mkv
```
*Figure. Module Introduction - Global infrastructure and reliability*


<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> J'aimerais vous parler de la haute disponibilité. Imaginons que vous vouliez prendre un café dans notre café, cependant aujourd’hui n’est pas un jour comme les autres dans notre ville. Aujourd’hui, un défilé va passer dans notre rue pour fêter toutes les migrations cloud qui ont été une réussite. Ce défilé va passer juste en face de notre boutique. C’est fabuleux, qui n’aime pas regarder ces chars et les ballons, et qu’on lui jette des bonbons dans la rue ? Néanmoins, c’est également une mauvaise nouvelle parce que pendant que le défilé est en train de descendre la rue, nos clients qui viennent chercher un café en voiture seront détournés et ne pourront pas s’arrêter à la boutique. Cela entraînera une baisse des ventes et rendra également des clients malheureux. 
> 
> Heureusement pour nous, nous nous sommes préparés. Il y a longtemps, nous avons pensé à ce qu’il se passerait si, pour une quelconque raison ne pouvaient temporairement pas accéder à la boutique ? Par exemple en cas de défilé ou d’inondation ou tout autre événement bloquant l’accès à la boutique ? Peu importe la raison, nous devons être hautement disponibles pour nos clients. 
> 
> Donc, je vais vous confier un secret. Ce n’est pas l'unique emplacement de notre café. Le café est en fait une chaîne et nous avons des boutiques dans toute la ville. De cette façon, si un défilé passe dans cette rue ou s’il y a une panne de courant dans notre quartier, pas d’inquiétude. Les clients peuvent toujours acheter leur café en se rendant dans l’une de nos boutiques situées à quelques pâtés de maisons. Nous faisons encore des recettes et ils prennent leur café. Tout va bien. 
> 
> AWS a agi de manière similaire dans sa manière de mettre en place son infrastructure mondiale. Il n'est pas suffisant d'avoir un centre de données gigantesque qui concentre toutes les ressources. Si quelque chose devait arriver à ce centre de données, comme une panne de courant ou une catastrophe naturelle, les applications de tous les clients tomberaient en panne à la fois. Vous avez besoin de haute disponibilité et de tolérance aux pannes. Il s'avère qu'il n'est même pas suffisant d’avoir deux gigantesques centres de données. Au lieu de cela, AWS opère dans toutes sortes de zones différentes dans le monde entier, et ces zones s’appellent des régions. Nous aborderons ces régions plus en profondeur dans les prochaines vidéos. En attendant, je vais m’installer ici et me détendre, parce que je sais que mon entreprise est hautement disponible peu importe les défilés qui bloquent la rue.
</div>
  
  
## Instaurer une présence mondiale

![ha_01.png](/assets/img/clouds/tempo/ha_01.png)

Pour comprendre l’infrastructure globale AWS, imaginez le café. Si un événement tel qu’un défilé, une inondation ou une panne de courant a des répercussions sur un endroit, les clients peuvent toujours prendre leur café en se rendant dans un autre café à seulement quelques pâtés de maisons.

Ceci est semblable au fonctionnement de l’infrastructure mondiale AWS.

## Infrastructure mondiale AWS

```
video : xxx.mkv
```
*Figure. AWS Regions*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Dans le but de comprendre l'infrastructure mondiale d'AWS, Je voudrais commencer par aborder votre besoin commercial fondamental. Vous avez une application que vous devez exécuter, ou du contenu que vous souhaitez stocker, ou encore des données de vous aimeriez analyser. En résumé vous avez des éléments qui doivent évoluer et fonctionner quelque part. Historiquement, les entreprises devaient exécuter les applications dans leurs propres centres de données, car elles n’avaient pas le choix. Une fois qu’AWS est devenu disponible, les entreprises comme la vôtre ont alors pu exécuter leurs applications dans d’autres centres de données qui ne leur appartenaient pas. 
> 
> Mais mon propos va beaucoup plus loin, je veux que vous compreniez un problème fondamental du centre de données, peu importe qui l’a construit et à qui il appartient. Des événements peuvent se produire et vous faire perdre la connexion à ce bâtiment. Si vous utilisez votre propre centre de données, vous devez comprendre comment répondre à la question de savoir ce que vous ferez si une catastrophe frappe votre bâtiment. Vous pouvez établir un second centre de données, mais les prix de l’immobilier à eux seuls pourraient vous freiner, encore plus que toutes les doubles dépenses pour le matériel, les employés, l’électricité, la climatisation et la sécurité. La plupart des entreprises finissent simplement par stocker des sauvegardes quelque part, et espérer ensuite que la catastrophe ne se produise jamais. Et l'espoir ne constitue pas un bon plan d'affaires. 
> 
> AWS répond à la question de ce qu’il se passe en cas de catastrophe en construisant nos centres de données dans de grands groupes que nous appelons régions. Voici comment ces groupes sont conçus. 
> 
> Partout sur la planète, AWS construit des régions au plus proche de l'endroit où le trafic professionnel l’exige. Des endroits comme Paris, Tokyo, Sao Paulo, Dublin, Ohio. Au sein de chaque région, nous avons plusieurs centres de données dotés de tous les services de calcul, de stockage, et d’autres services dont vous avez besoin pour exécuter vos applications. Toutes les régions peuvent être connectées entre elles par le biais d’un réseau de fibre à haute vitesse, contrôlé par AWS, une opération véritablement globale, d’un bout à l’autre si nécessaire. Avant d’aborder l’architecture de chaque région, il est important de savoir que vous, décideur d’entreprise, avez la possibilité de choisir dans quelle région vous souhaitez vous établir. Et chaque région est isolée de l’autre au sens où aucune donnée quelle qu’elle soit n’entre ou ne sort de votre environnement dans cette région sans que vous accordiez l’autorisation explicite que ces données puissent être déplacées. Il s'agit d'une discussion essentielle du point de vue de la sécurité. 
> 
> Par exemple, des exigences de conformité gouvernementales peuvent exister, stipulant que vos informations financières à Francfort ne peuvent pas sortir d’Allemagne. AWS fonctionne exactement de la même manière. Toute donnée stockée dans la région de Francfort ne quitte jamais la région de Francfort, ou les données de la région de Londres ne quittent jamais Londres, ou celles de Sydney ne sortent jamais de Sydney, sauf si vous demandez explicitement, avec les bonnes informations d’identification et les bonnes autorisations, que les données soient exportées. 
> 
> La souveraineté régionale des données est une partie essentielle de la conception des régions AWS. Les données étant soumises aux réglementations locales et aux lois nationales dans laquelle se trouve la région. Donc, étant donné que vos données, votre application, évolue et fonctionne dans une région, l’une des première décisions à prendre est quelle région choisir ? Il y a quatre facteurs commerciaux à prendre en considération dans le choix d’une région. 
> 
> Numéro un, la conformité. Avant toute chose, vous devez d'abord prendre connaissance des exigences de conformité. Vos données doivent-elles se trouver au sein des frontières du Royaume-Uni ? Dans ce cas, vous devriez choisir la région « Londres », point à la ligne. Aucune des autres options n'a d'importance. Disons sinon que vous devez exécuter à l'intérieur des frontières chinoises. Alors, vous devriez choisir une de nos régions chinoises. Cependant la plupart des entreprises ne sont pas soumises à une réglementation aussi stricte. Ainsi, si votre région n’est régie par aucune conformité ou aucun contrôle réglementaire, vous pouvez alors examiner les autres facteurs. 
> 
> Numéro deux, la proximité. La proximité de votre clientèle est un facteur majeur à cause de la vitesse de la lumière, cela reste la loi de l’univers. Si la plupart de vos clients se trouvent à Singapour, envisagez de vous établir dans la région de Singapour. Vous pouvez évidemment choisir la Virginie, mais le temps nécessaire à l’envoi des informations, la latence, entre les USA et Singapour, aura toujours un impact. Nous sommes certes en train de développer l’informatique quantique, mais nous sommes encore loin de la mise en réseau quantique. Le temps dont la lumière à besoin pour voyager à travers le monde doit toujours être pris en compte. Vous établir à proximité de votre clientèle est généralement le bon choix. 
> 
> Numéro trois, la disponibilité des fonctions. Parfois, la région la plus proche peut ne pas avoir toutes les fonctions AWS qui vous intéressent. Mais l'un des avantages d'AWS est que nous innovons constamment au nom de nos clients. Chaque année, AWS publie des milliers de nouvelles fonctions et produits spécialement pour répondre aux demandes et aux besoins des clients. Mais il arrive que ces nouveaux services exigent beaucoup de nouveau matériel physique qui doivent être construits par AWS pour pouvoir rendre le service opérationnel. Parfois, cela signifie que nous devons construire le service dans une région à la fois. Imaginons que vos développeurs souhaitent jouer avec Amazon Braket, notre nouvelle plateforme d’informatique quantique. Dans ce cas, ils doivent se rendre dans les régions où le matériel est déjà installé. Est-il possible que cette fonction soit à terme disponible dans toutes les régions ? Oui, c’est possible mais si souhaitez l’utiliser aujourd’hui, alors, cela peut constituer votre facteur de décision. 
> 
> Numéro quatre, la tarification. Même lorsque le matériel est identique d’une région à l’autre, l’exploitation est plus chère à certains endroits, par exemple au Brésil. La structure fiscale actuelle du Brésil fait que cela coûte beaucoup plus cher à AWS d’exécuter des services identiques ici que dans de nombreux autres pays. La même charge de travail à Sao Paulo peut coûter disons 50 % de plus à exploiter que dans l’Oregon aux États-Unis. Le prix peut être déterminé par de nombreux facteurs, et AWS a donc une tarification détaillée très transparente que nous continuerons à évoquer dans ce cours. En outre, sachez que chaque région possède une feuille de prix différente. Donc si le budget est votre principale préoccupation, même si vos clients vivent au Brésil, il vaudrait peut-être mieux opérer dans un autre pays, encore une fois, si le prix est votre motivation première. 
> 
> En résumé, quatre facteurs clés pour choisir une région : Conformité, proximité, disponibilité des fonctions et tarification. À notre retour, nous étudierons les éléments changeants à l'intérieur d'une région.
</div>
  
## Sélection d’une région

Lorsque vous déterminez la région appropriée pour vos services, données et applications, tenez compte des quatre facteurs commerciaux suivants. 

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Conformité en termes de gouvernance des données et exigences légales" value="true">

En fonction de votre entreprise et de votre emplacement, vous devrez peut-être exécuter vos données à partir de zones spécifiques. Par exemple, si votre entreprise a besoin que toutes ses données résident dans les limites du Royaume-Uni, vous choisirez la région Londres. 

Toutes les entreprises ne disposent pas de réglementations sur les données spécifiques à l’emplacement. Vous devrez donc peut-être vous concentrer davantage sur les trois autres facteurs.
</div>

<div class="collapseContent" display="Proximité avec vos clients" value="true">

La sélection d’une région proche de vos clients vous aidera à obtenir du contenu plus rapidement. Par exemple, votre entreprise est basée à Washington, DC, et nombre de vos clients vivent à Singapour. Vous pouvez envisager d’exécuter votre infrastructure dans la région de Virginie du Nord à proximité du siège social de l’entreprise et d’exécuter vos applications à partir de la région de Singapour.
</div>
  
<div class="collapseContent" display="Services disponibles dans une région" value="true">

Parfois, la région la plus proche peut ne pas avoir toutes les fonctions que vous souhaitez proposer aux clients. AWS innove fréquemment en créant de nouveaux services et en développant les fonctions au sein des services existants. Cependant, pour rendre de nouveaux services disponibles dans le monde entier, AWS doit parfois développer le matériel physique une région à la fois. 

Supposons que vos développeurs souhaitent créer une application qui utilise Amazon Braket (plateforme de calcul quantique AWS). Au moment de ce cours, Amazon Braket n’est pas encore disponible dans toutes les régions AWS à travers le monde, de sorte que vos développeurs auront à l’exécuter dans l’une des régions qui l’offre déjà.
</div>
  
<div class="collapseContent" display="Tarification" value="true">

Supposons que vous envisagez d’exécuter des applications aux États-Unis et au Brésil. La mise en place de la structure fiscale brésilienne pourrait coûter 50 % de plus pour gérer la même charge de travail dans la région de São Paulo par rapport à la région de l’Oregon. Vous apprendrez plus en détail que plusieurs facteurs déterminent la tarification, mais pour l’instant sachez que le coût des services peut varier d’une région à l’autre.
</div>
  
  
```
video : xxx.mkv
```
*Figure. Availability Zones*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Si une région se trouve là où se trouvent tous les éléments de votre application, certains d’entre vous pourraient penser que nous n’avons jamais vraiment résolu le problème présenté dans la dernière vidéo. Permettez-moi de reformuler le problème. Il est préférable de ne pas exécuter votre application dans un seul bâtiment parce qu’un seul bâtiment peut échouer pour toute une série de raisons inévitables. 
> 
> Vous pensez peut-être que, si mon entreprise doit être à l’épreuve des sinistres, alors elle ne peut pas fonctionner à un seul endroit. Vous avez absolument raison. AWS est tout à fait d’accord et c’est pourquoi nos régions ne se trouvent pas au même endroit. Tout d’abord, AWS a des centres de données, de nombreux centres de données, partout dans le monde, et chaque région est composée de plusieurs centres de données. 
> 
> AWS appelle un centre de données unique ou un groupe de centres de données, une zone de disponibilité (AZ). Chaque zone de disponibilité comprend un ou plusieurs centres de données individuels, dotés d'une alimentation redondante, d'un réseau et d'une connectivité. Lorsque vous lancez une instance Amazon EC2, elle lance une machine virtuelle sur un matériel physique installé dans une zone de disponibilité. Cela signifie que chaque région AWS est constituée de plusieurs zones de disponibilité séparées physiquement et isolées dans une région géographique. 
> 
> Mais nous ne construisons pas de zones de disponibilité les unes à côté des autres car si un événement à grande échelle a lieu, comme une catastrophe naturelle par exemple, vous pourriez perdre la connectivité de tout ce qui se trouve dans cette zone de disponibilité. La question de ce qui se passe dans le cas d’un désastre est importante et si vous connaissez la planification de la reprise après sinistre, vous pourriez même avoir une idée d’où je veux en venir. 
> 
> Si vous n’exécutez qu’une seule instance EC2, dans un seul bâtiment, ou une seule zone de disponibilité et une catastrophe à grande échelle se produit, votre application pourra-t-elle toujours servir vos clients ? La solution évidente à cela est d’exécuter plusieurs instances EC2, comme nous l’avons montré plus tôt dans l’exemple de mise à échelle. Mais l'essentiel est de ne pas les exécuter dans le même bâtiment. Ne les exécutez même pas dans la même rue, éloignez-les aussi loin que possible avant que la vitesse de la lumière vous dise d’arrêter si vous voulez toujours une communication à faible latence. Il s’avère que la vitesse de la lumière nous permettra de déplacer ces zones de disponibilité à des dizaines de kilomètres les unes des autres tout en conservant une latence en millisecondes à un chiffre entre ces zones de disponibilité. Si une catastrophe se produit, votre application continue sans souci car ce sinistre n’atteint qu’une partie de votre capacité. 
> 
> Et comme nous l’avons vu dans la dernière section, vous pouvez rapidement lancer plus de capacité dans les zones de disponibilité restantes, et donc permettre à votre entreprise de continuer à fonctionner sans interruption. Et comme bonne pratique avec AWS, nous recommandons de toujours exécuter votre application dans au moins deux zones de disponibilité d’une région. Cela signifie le déploiement redondant de votre infrastructure dans deux AZ différentes. 
> 
> Mais les régions sont plus que des endroits où exécuter EC2. De nombreux services AWS s’exécutent au niveau de la région, ce qui signifie qu’ils s’exécutent de façon synchrone sur plusieurs AZ sans aucun effort supplémentaire de votre part. Prenez l'ELB dont nous avons parlé précédemment. Il s'agit en fait d'une construction régionale. Elle s’exécute sur toutes les zones de disponibilité, et communique avec les instances EC2 qui sont en cours d’exécution dans une zone de disponibilité spécifique. Les services régionaux sont par définition déjà hautement disponibles sans frais supplémentaires ni efforts de votre part. 
> 
> Lorsque vous planifiez une haute disponibilité, tout service répertorié comme un service à portée régionale, vous vous en serez déjà occupé. À notre retour, nous aborderons l’extérieur des régions pour des solutions supplémentaires.
</div>
  
  
## Zones de disponibilité

![ha_02.png](/assets/img/clouds/tempo/ha_02.png)


Une **zone de disponibilité** est un centre de données unique ou un groupe de centres de données au sein d’une région. Les zones de disponibilité sont situées à des dizaines de kilomètres les unes des autres. C’est assez proche pour avoir une faible latence (le temps entre le moment où le contenu est demandé et le moment où il est reçu) entre les zones de disponibilité. Toutefois, si un sinistre survient dans une partie de la région, elle est suffisamment éloignée pour réduire les risques que plusieurs zones de disponibilité soient affectées.

![ha_03.png](/assets/img/clouds/tempo/ha_03.png)

#### tabs {.tabset}
##### Etape 1

###### Instance Amazon EC2 dans une seule zone de disponibilité

![ha_04.png](/assets/img/clouds/tempo/ha_04.png)

Une instance EC2 est déployée dans la zone de disponibilité us-west-1a.
Supposons que vous exécutez une application sur une seule instance Amazon EC2 dans la région Californie du Nord. L’instance est en cours d’exécution dans la zone de disponibilité us-west-1a. Si us-west-1a échouait, vous perdriez votre instance.

##### Etape 2

###### Instances Amazon EC2 dans plusieurs zones de disponibilité

![ha_05.png](/assets/img/clouds/tempo/ha_05.png)

Les instances EC2 sont déployées dans les zones de disponibilité us-west-1a et us-west-1b.
Une bonne pratique consiste à exécuter des applications dans au moins deux zones de disponibilité dans une région. Dans cet exemple, vous pouvez choisir d’exécuter une deuxième instance Amazon EC2 dans us-west-1b.

##### Etape 3

###### Echec de zone de disponibilité

![ha_06.png](/assets/img/clouds/tempo/ha_06.png)

Si us-west-1a échouait, votre application serait toujours en cours d’exécution dans us-west-1b.

##### Résumé
La planification des échecs et le déploiement des applications sur plusieurs zones de disponibilité constituent un élément important de la création d’une architecture résiliente et hautement disponible.

## Contrôle des connaissances

**Parmi les déclarations suivantes, laquelle décrit le mieux le concept de zones de disponibilité ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Une zone géographique qui contient des ressources AWS</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Un centre de données unique ou un groupe de centres de données au sein d’une région</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Centre de données utilisé par un service AWS pour effectuer des opérations spécifiques à un service.</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Un service que vous pouvez utiliser pour exécuter l’infrastructure AWS dans votre propre centre de données sur site dans une approche hybride</label><br>
</form>  


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Un centre de données unique ou un groupe de centres de données au sein d’une région.**

Les autres réponses sont incorrectes, car :

- Une région est une zone géographique qui contient des ressources AWS.
- Un emplacement périphérique est un centre de données qu’un service AWS utilise afin d’effectuer des opérations spécifiques à un service. Les emplacements périphériques sont étudiés dans la section suivante de ce module.
- AWS Outposts est un service que vous pouvez utiliser pour exécuter une infrastructure, des services et des outils AWS dans votre propre centre de données sur site, selon une approche hybride. AWS Outposts est étudié ultérieurement dans ce module.

En savoir plus :

- [Infrastructure mondiale AWS](https://aws.amazon.com/about-aws/global-infrastructure)
- [Régions et zones de disponibilité](https://aws.amazon.com/about-aws/global-infrastructure/regions_az)
</div>
  
# Emplacements périphériques

```
video : xxx.mkv
```
*Figure. Edge locations*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left} 
> L'un des bons aspects de l’infrastructure mondiale AWS est la façon dont elle est conçue pour vous aider à mieux servir vos clients. Souvenez-vous que, pour choisir une région, l’un des principaux critères était la proximité avec votre clientèle, mais que se passe-t-il si vous avez des clients partout dans le monde ou dans des villes qui ne sont pas à proximité d’une de vos régions ? Pensez à notre café. Si vous avez une bonne clientèle dans une nouvelle ville, vous pouvez construire une filiale pour servir ces clients. 
> 
> Vous n’avez pas besoin de construire un nouveau siège social ou du point de vue informatique, si vous avez des clients à Mumbai qui doivent accéder à vos données, mais les données sont hébergées en dehors de la région de Tokyo, au lieu de demander à tous les clients situés à Mumbai d’envoyer des demandes jusqu’à Tokyo, pour accéder aux données, il suffit de placer localement une copie ou de mettre en cache une copie à Mumbai. La mise en cache de copies de données plus proche des clients et partout dans le monde utilise le concept de réseaux de diffusion de contenu (CDN). 
> 
> Les CDN sont couramment utilisés, et sur AWS, nous appelons notre CDN Amazon CloudFront. Amazon CloudFront est un service qui aide à diffuser des données, des vidéos, des applications, et des API aux clients du monde entier avec une latence réduite et une vitesse de transfert élevée. Amazon CloudFront utilise ce qu’on appelle des emplacements périphériques, partout dans le monde, pour accélérer la communication avec les utilisateurs, peu importe où ils se trouvent. 
> 
> Les emplacements périphériques sont séparés des régions, et vous pouvez donc transmettre du contenu de l’intérieur de la région vers un groupe d’emplacements périphériques dans le monde, afin d’accélérer la communication et la diffusion des contenus. Les emplacements périphériques AWS exécutent plus que juste CloudFront. Ils exécutent un service de nom de domaine, (DNS), appelé Amazon Route 53, et aident les clients à se diriger vers les bons emplacements web avec une latence basse et fiable. 
> 
> Et si votre entreprise choisit d’utiliser, les services AWS à l’intérieur de son propre bâtiment ? Pas de souci. AWS peut s'en occuper pour vous. Je vous présente AWS Outposts, où AWS installera principalement une mini-région entièrement opérationnelle, à l’intérieur même de votre propre centre de données. Il est détenu et exploité par AWS et utilise 100 % des fonctionnalité AWS, mais il est isolé dans votre propre bâtiment. Ce n’est pas la solution dont la plupart des clients ont besoins, mais si vous avez des problèmes spécifiques qui ne peuvent être résolus qu’en restant dans votre propre bâtiment, nous comprenons, AWS Outposts peut vous aider. 
> 
> Et bien, il y a beaucoup d’autres choses à dire à propos de l’infrastructure mondiale AWS, mais arrêtons-nous ici pour l’instant. Voici donc les points clés. En premier, les régions sont des zones isolées géographiquement, où vous pouvez accéder aux services nécessaires pour votre entreprise. Ensuite, les régions contiennent des zones de disponibilité, qui permettent une exécution sur des bâtiments séparés physiquement, à des dizaines de kilomètres de distance, tout en maintenant votre application unifiée logiquement. Les zones de disponibilité vous aident à faire face aux questions de haute disponibilité et de reprise après sinistre, sans effort supplémentaire de votre part, et enfin, les emplacements périphériques AWS exécutent Amazon CloudFront pour que le contenu soit proche de vos clients, où qu’ils se trouvent dans le monde.
</div>
  
## Emplacements périphériques

Un emplacement périphérique est un site qu’Amazon CloudFront utilise pour stocker des copies mises en cache de votre contenu plus près de vos clients, aux fins d’une diffusion plus rapide.

Sélectionnez chaque marqueur pour en savoir plus.

![ha_08.png](/assets/img/clouds/tempo/ha_08.png)

![ha_09.png](/assets/img/clouds/tempo/ha_09.png)

![ha_10.png](/assets/img/clouds/tempo/ha_10.png)

![ha_11.png](/assets/img/clouds/tempo/ha_11.png)

# Comment mettre en service des ressources AWS

```
video : xxx.mkv
```
*Figure. How to provision AWS resources - Part 1*

<div class="collapseContent" display="Transcription" value="true">
  
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Nous avons parlé de plusieurs ressources AWS différentes, ainsi que de l'infrastructure mondiale AWS. Vous vous demandez peut-être comment réellement interagir avec ces services ? Et la réponse est les API. Dans AWS, tout est un appel d'API. Une API est une interface de programmation d'application. Cela signifie qu’il existe des moyens prédéterminés d’interagir avec les services AWS. Et vous pouvez invoquer ou appeler ces API pour mettre en service, configurer, et gérer vos ressources AWS. 
> 
> Par exemple, vous pouvez lancer une instance EC2 ou créer une fonction AWS Lambda. Chacune de ces demandes serait différente comme les appels d’API vers AWS. Vous pouvez utiliser AWS Management Console, AWS Command Line Interface, AWS Software Development Kits, ou plusieurs autres outils comme AWS CloudFormation, pour créer des demandes à envoyer aux API AWS, et créer et gérer les ressources AWS. 
> 
> Parlons d'abord d'AWS Management Console. AWS Management Console est basée sur un navigateur. Grâce à la console, vous pouvez gérer visuellement vos ressources AWS et d’une manière facile à assimiler. C’est parfait pour démarrer et construire vos connaissances des services. Elle est très utile pour construire des environnements de test ou afficher des factures AWS bills, afficher le contrôle et travailler avec d’autres ressources non techniques. AWS Management Console est probablement ce que vous découvrirez en premier en explorant AWS. 
> 
> Toutefois, une fois que vous êtes lancé dans un environnement de type production, il vaut mieux ne pas compter sur le style pointer-cliquer que la console vous propose pour créer et gérer vos ressources AWS. Par exemple, pour créer une instance Amazon EC2, vous devez cliquer sur plusieurs écrans, effectuer toutes les configurations souhaitées, puis lancer votre instance. Si vous souhaitez ensuite lancer une autre instance EC2, vous devez retourner dans la console et cliquer à nouveau sur les écrans pour qu’elle soit opérationnelle. En demandant aux humains d'effectuer cette sorte de mise en service manuelle, vous vous exposez à des erreurs potentielles. Il est assez facile d’oublier de cocher une case ou de mal orthographier quelque chose lorsqu’on fait tout manuellement. 
> 
> La réponse à ce problème consiste à utiliser des outils vous permettant de créer des scripts ou de programmer des appels d’API. L’un de ces outils est AWS Command Line Interface (CLI). CLI vous permet de réaliser des appels d’API en utilisant le terminal sur votre machine. Ceci est différent du style de navigation visuelle de la Management Console. Écrire des commandes en utilisant CLI permet de créer des scripts et rend les actions reproductibles. Ainsi, vous pouvez écrire et exécuter vos commandes pour lancer une instance EC2. Et si vous souhaitez en lancer une autre, il suffit d’exécuter à nouveau la commande pré-écrite. Cela la rend moins sujette à l'erreur humaine. Et vous pouvez exécuter ces scripts automatiquement, selon un calendrier ou en les déclenchant avec un autre processus. 
> 
> L’automatisation est très importante pour un déploiement réussi et prévisible du cloud dans le temps. Une autre façon d’interagir avec AWS consiste à utiliser AWS Software Development Kits (SDK). SDK vous permettent d’interagir avec les ressources AWS via différents langages de programmation. Cela permet aux développeurs de créer plus facilement des programmes qui utilisent AWS sans les API de bas niveau, ainsi que d’éviter la création manuelle de ressources dont nous venons de parler. Nous y reviendrons sous peu.
</div>

## Comment interagir avec des services AWS

Pour en savoir plus sur chaque catégorie, sélectionnez chaque onglet.

### Tabs {.tabset}
#### AWS Management Console

**AWS Management Console** est une interface basée sur le web qui vous permet d’accéder à vos services AWS et de les gérer. Vous pouvez accéder rapidement aux services récemment utilisés et rechercher d’autres services par nom, mot clé ou acronyme. La console comprend des assistants et des flux de travail automatisés qui permettent de simplifier le processus d’exécution des tâches.

Vous pouvez également utiliser l’application mobile de la console AWS pour effectuer des tâches telles que la surveillance des ressources, l’affichage des alarmes et l’accès aux informations de facturation. Plusieurs identités peuvent rester connectées à l’application mobile AWS Console en même temps.

![aws_console.png](/assets/img/clouds/tempo/aws_console.png =300x)

#### Interface de Ligne de Commande (CLI)

Pour gagner du temps lors de demandes d’API, vous pouvez utiliser **AWS Command Line Interface (AWS CLI)**. AWS CLI vous permet de contrôler plusieurs services AWS directement à partir de la ligne de commande au sein d’un seul outil. AWS CLI est disponible pour les utilisateurs sous Windows, macOS et Linux. 

En utilisant AWS CLI, vous pouvez automatiser les actions effectuées par vos services et applications à l’aide de scripts. Par exemple, vous pouvez utiliser des commandes pour lancer une instance Amazon EC2, connecter une instance Amazon EC2 à un groupe Auto Scaling spécifique, entre autres.

![aws_cli.png](/assets/img/clouds/tempo/aws_cli.png =300x)

#### Kits de développement logiciel (SDK)

Une autre option pour accéder et gérer les services AWS sont les **kits de développement logiciel (SDK)**. Les kits SDK vous permettent d’utiliser plus facilement les services AWS via une API conçue pour votre langage de programmation ou votre plateforme. Les kits SDK vous permettent d’utiliser les services AWS avec vos applications existantes ou de créer des applications entièrement nouvelles qui s’exécuteront sur AWS.

Pour vous aider à utiliser les kits SDK, AWS fournit de la documentation et des exemples de code pour chaque langage de programmation pris en charge. Les langages de programmation pris en charge incluent C++, Java, .NET et bien plus encore.

![aws_sdk.png](/assets/img/clouds/tempo/aws_sdk.png =300x)

###

```
video : xxx.mkv
```
*Figure. How to provision AWS resources - Part 2*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Allez, continuons de parler de la façon d'interagir avec AWS. Vous disposez de AWS Management Console, CLI, et des SDK, qui sont tous des moyens de se débrouiller seul pour mettre en service et gérer votre environnement AWS. Si vous souhaitez mettre en service une ressource, vous pouvez vous connecter à la console et pointer-cliquer, vous pouvez écrire des commandes, ou vous pouvez écrire des programmes pour le faire. Il existe également d’autres manières de gérer votre environnement AWS à l’aide d’outils de gestion comme AWS Elastic Beanstalk, et AWS CloudFormation. 
> 
> AWS Elastic Beanstalk est un service qui vous aide à mettre en service les environnements basés sur Amazon EC2. Au lieu de cliquer sur la console ou d’écrire plusieurs commandes pour développer votre réseau, les instances EC2, la mise à échelle et Elastic Load Balancers. Vous pouvez à la place fournir votre code d’application et les configurations souhaitées au service AWS Elastic Beanstalk, qui prend ensuite ces informations et construit votre environnement pour vous. AWS Elastic Beanstalk facilite également la sauvegarde des configurations de l’environnement, afin qu’elles puissent être déployées à nouveau facilement. AWS Elastic Beanstalk vous facilite la tâche en en n’ayant pas à mettre en service ni à gérer tous ces éléments séparément, tout en vous donnant la visibilité et le contrôle des ressources sous-jacentes. Vous pouvez vous concentrer sur votre application metier et pas l’infrastructure. 
> 
> AWS CloudFormation est un autre service que vous pouvez utiliser pour des déploiements automatiques et reproductibles. AWS CloudFormation est un outil infrastructure as code permettant de définir une grande variété de ressources AWS de manière déclarative en utilisant des documents de texte JSON ou YAML appelés modèles CloudFormation. Ce type de format déclaratif vous permet de définir ce que vous souhaitez construire sans préciser les détails de la manière exacte de le construire. CloudFormation vous permet de définir ce que vous voulez et le moteur CloudFormation s’occupera des détails sur l’appel des API pour que tout soit construit. 
> 
> Il n'est pas seulement limité aux solutions basées sur EC2. CloudFormation prend en charge de nombreuses ressources AWS de stockage, de bases de données, d’analytique, de machine learning, et bien plus encore. Une fois que vous définissez vos ressources dans un modèle CloudFormation, CloudFormation analyse le modèle et commence à mettre en service toutes les ressources que vous avez définies en parallèle. CloudFormation gère tous les appels d’API backend AWS pour vous. Vous pouvez exécuter le même modèle CloudFormation sur plusieurs comptes ou plusieurs régions, et il créera des environnements identiques entre eux. L’erreur humaine est moins probable car il s’agit d’un processus entièrement automatisé. 
> 
> Pour récapituler, AWS Management Console est fantastique pour apprendre et offrir un visuel à l’utilisateur. AWS Management Console est un outil manuel. Donc, dès le départ, ce n'est pas une bonne option pour l'automatisation. Vous pouvez à la place utiliser CLI pour écrire vos interactions avec AWS à l’aide du terminal. Vous pouvez utiliser les SDK pour écrire des programmes et interagir avec AWS pour vous ou vous pouvez utiliser les outils de gestion comme AWS Elastic Beanstalk ou AWS CloudFormation.
</div>

## AWS Elastic Beanstalk

Avec **[AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)**, vous fournissez le code et les paramètres de configuration, et Elastic Beanstalk déploie les ressources nécessaires pour effectuer les tâches suivantes :

- Ajustement de la capacité
- Répartition de charge
- Mise à l’échelle automatique
- Surveillance de l’état des applications

![aws_elastic_beanstalk_01.png](/assets/img/clouds/tempo/aws_elastic_beanstalk_01.png)

## AWS CloudFormation

Avec **[AWS CloudFormation](https://aws.amazon.com/fr/cloudformation/)**, vous pouvez traiter votre infrastructure comme du code. Cela signifie que vous pouvez créer un environnement en écrivant des lignes de code au lieu d’utiliser l’AWS Management Console pour mettre individuellement en service des ressources.

![aws_cloudformation_01.png](/assets/img/clouds/tempo/aws_cloudformation_01.png)

AWS CloudFormation met en service vos ressources de manière sûre et reproductible, ce qui vous permet de créer fréquemment votre infrastructure et vos applications sans devoir effectuer des actions manuelles ou écrire des scripts personnalisés. Il détermine les bonnes opérations à effectuer lors de la gestion de votre pile et annule automatiquement les modifications s’il détecte des erreurs.

# Module 3 : résumé

Dans le module 3, vous avez découvert les concepts suivants :

- Régions et zones de disponibilité
- Emplacements périphériques et Amazon CloudFront
- AWS Management Console, AWS CLI et kits SDK
- AWS Elastic Beanstalk
- AWS CloudFormation

```
video : xxx.mkv
```
*Figure. Module summary - Global infrastructure and reliability*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Fantastique, nous avons couvert beaucoup de choses. Et pas seulement parce que nous avons parlé de l’infrastructure mondiale AWS. 
> 
> Mais c’est vrai que nous avons abordé comment les clusters logiques des centres de données créent des zones de disponibilité, les zones de disponibilité quant à elles créent des régions, qui sont réparties à l’échelle mondiale. Puis vous choisissez dans quelles régions et zones de disponibilité vous souhaitez opérer et comme bonne pratique, vous devez toujours déployer l’infrastructure sur au moins deux zones de disponibilité. Et certains services AWS comme Elastic Load Balancing, Amazon SQS, et Amazon SNS vous proposent déjà cela. 
> 
> Nous avons également parlé des emplacements périphériques et comment vous pouvez y déployer du contenu pour accélérer la diffusion à vos clients. Nous avons même mentionné les dispositifs périphériques comme AWS Outposts grâce auxquels vous pouvez exécuter l’infrastructure AWS à partir de votre propre centre de données. 
> 
> Nous avons également abordé la manière de mettre en service des ressources AWS par le biais de diverses options comme AWS Management Console, SDK, CLI, AWS Elastic Beanstalk, et AWS CloudFormation, et vous avez appris comment configurer votre infrastructure en tant que code. 
> 
> J’espère que vous savez maintenant qu’AWS est disponible partout dans le monde et à quel point il est facile de démarrer l’allocation de ressources.
</div>


## Ressources supplémentaires

Consultez ces ressources pour en savoir plus sur les concepts qui ont été explorés dans le module 3.

- [Infrastructure mondiale](https://aws.amazon.com/about-aws/global-infrastructure/)
- [Carte interactive de l’infrastructure mondiale AWS](https://www.infrastructure.aws/)
- [Régions et zones de disponibilité](https://aws.amazon.com/about-aws/global-infrastructure/regions_az)
- [Blog sur les réseaux et la diffusion de contenu AWS](https://aws.amazon.com/blogs/networking-and-content-delivery/)
- [Outils pour créer sur AWS](https://aws.amazon.com/tools/)
- [Témoignages de clients AWS : diffusion de contenu](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23content-delivery)

# Module 3 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelle déclaration est VRAIE pour l’infrastructure globale AWS ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Une région se compose d’une seule zone de disponibilité.</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Une zone de disponibilité se compose de deux régions ou plus.</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Une région se compose de deux ou plusieurs zones de disponibilité.</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Une zone de disponibilité se compose d’une seule région.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Une région se compose de deux ou plusieurs zones de disponibilité**.

Par exemple, la région Amérique du Sud (São Paulo) est sa-east-1. Elle comprend trois zones de disponibilité : sa-east-1**a**, sa-east-1**b**, et sa-east-1**c**.

En savoir plus :

- [Infrastructure mondiale AWS](https://aws.amazon.com/about-aws/global-infrastructure)
- [Régions et zones de disponibilité](https://aws.amazon.com/about-aws/global-infrastructure/regions_az)
</div>

**Quels facteurs devraient être pris en compte lors du choix d’une région ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q2r1" name="question2" value="1">
  <label for="q2r1">Conformité en termes de gouvernance des données et exigences légales.</label><br>
  <input type="checkbox" id="q2r2" name="question2" value="2">
  <label for="q2r2">Proximité avec vos clients</label><br>
  <input type="checkbox" id="q2r3" name="question2" value="3">
  <label for="q2r3">Accès à un support technique 24/7</label><br>
  <input type="checkbox" id="q2r4" name="question2" value="4">
  <label for="q2r4">Possibilité d’attribuer des autorisations personnalisées à différents utilisateurs.</label><br>
  <input type="checkbox" id="q2r5" name="question2" value="5">
  <label for="q2r5">Accès à AWS Command Line Interface (AWS CLI)</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- Conformité en termes de gouvernance des données et exigences légales
- Proximité avec vos clients

Les deux autres facteurs à prendre en considération lors de la sélection d’une région sont la tarification et les services disponibles dans une région.

Les autres réponses sont incorrectes, car :

- Le niveau de support que vous choisissez ne dépend pas de la région. Les formules AWS Support sont étudiées ultérieurement dans ce cours.
- L’attribution d’autorisations personnalisées à différents utilisateurs est une fonction possible dans toutes les régions AWS.
- Command Line Interface AWS (AWS CLI) est disponible dans toutes les régions AWS.

En savoir plus :

- [Choisir des régions et des zones de disponibilité](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/RegionsAndAZs.html)
</div>
  
  
**Quelle déclaration décrit le mieux Amazon CloudFront ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Un service qui vous permet d’exécuter une infrastructure dans une approche cloud hybride</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">Un moteur de calcul sans serveur pour les conteneurs</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">Amazon Simple Queue Service (Amazon SQS) est un service qui vous permet d’envoyer, de stocker et de recevoir des messages entre les composants logiciels via une file d’attente.</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Un service mondial de diffusion de contenu.</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Un service mondial de diffusion de contenu**.

Amazon CloudFront est un service de diffusion de contenu. Il utilise un réseau d’emplacements périphériques pour mettre en cache le contenu et le fournir à des clients du monde entier. Lorsque le contenu est mis en cache, il est stocké localement sous forme de copie. Ce contenu peut être des fichiers vidéo, des photos et des pages web, entre autres.

Les autres réponses sont incorrectes, car :

- AWS Outposts est un service qui vous permet d’exécuter une infrastructure selon une approche cloud hybride.
- AWS Fargate est un moteur de calcul sans serveur pour les conteneurs.
- Amazon Simple Queue Service (Amazon SQS) est un service qui vous permet d’envoyer, de stocker et de recevoir des messages entre les composants logiciels via une file d’attente.

En savoir plus :

- [Amazon CloudFront](https://aws.amazon.com/cloudfront)
</div>

**Quel site Amazon CloudFront utilise-t-il pour mettre en cache des copies de contenu pour une livraison plus rapide aux utilisateurs, quel que soit leur emplacement ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Région</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">Zone de disponibilité</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">Emplacement périphérique</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Origine</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">
La bonne réponse est **Emplacement périphérique**.

Les autres réponses sont incorrectes, car :

- Une région est un emplacement géographique distinct avec plusieurs emplacements isolés les uns des autres.
- Une zone de disponibilité est une partie entièrement isolée de l’infrastructure mondiale AWS.
- Une origine est le serveur à partir duquel CloudFront obtient vos fichiers. Les exemples d’origines CloudFront incluent des compartiments Amazon Simple Storage Service (Amazon S3) et des serveurs web. Remarque : Amazon S3 est étudié ultérieurement dans ce cours.

En savoir plus :

- [Infrastructure Amazon CloudFront](https://aws.amazon.com/cloudfront/features/?nc=sn&loc=2#Amazon_CloudFront_Infrastructure)
</div>
  
**Quelle action pouvez-vous effectuer avec AWS Outposts ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Automatiser les actions pour les services et applications AWS à l’aide de scripts.</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Accéder à des assistants et à des flux de travail automatisés pour effectuer des tâches dans les services AWS.</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Développer des applications AWS dans les langages de programmation pris en charge</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Étendre l’infrastructure et les services AWS à votre centre de données sur site.</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Étendre l’infrastructure et les services AWS à votre centre de données sur site**.

Les autres réponses sont incorrectes, car :

- AWS Command Line Interface (AWS CLI) est utilisée pour automatiser les actions des services et applications AWS par le biais de scripts.
- L’AWS Management Console comprend des assistants et des flux de travail que vous pouvez utiliser pour effectuer des tâches dans les services AWS.
- Les SDK vous permettent de développer des applications AWS dans les langages de programmation pris en charge.

En savoir plus :

- [AWS Outposts](https://aws.amazon.com/outposts/)
</div>

Le module suivant explore la mise en réseau dans AWS Cloud.