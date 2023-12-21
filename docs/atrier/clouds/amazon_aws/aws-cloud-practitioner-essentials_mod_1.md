---
title: Module 1 - introduction à Amazon Web Services
description: 
published: true
date: 2023-04-21T10:55:26.280Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:55:20.580Z
---

# Introduction à AWS Cloud Practitioner Essentials



## Présentation du cours

Bienvenue au cours AWS Cloud Practitioner Essentials.

Pendant les modules 1 à 10, vous développerez vos connaissances en matière de cloud AWS en étudiant les concepts du cloud AWS, les services AWS, la sécurité, l’architecture, la tarification et le support. 

- Les leçons de chaque module incluent des vidéos, des informations complémentaires et des liens vers des ressources supplémentaires qui vous permettent de mieux comprendre les services AWS.
- Les contrôles des connaissances et les quiz permettent d’examiner les concepts clés qui ont été abordés dans chaque module. Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes afin de renforcer votre compréhension des concepts.

Dans le module 11, le module final, vous découvrirez la structure de l’examen AWS Certified Cloud Practitioner. Vous examinerez également les stratégies qui vous aideront à augmenter la probabilité de réussir l’examen.

Vous terminerez ensuite le cours par une évaluation finale de 30 questions et un bref questionnaire.

Commencez par le Module 1 : introduction à Amazon Web Services.

# Module 1 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à:

- Résumer les avantages d’AWS
- Décrire les différences entre la mise à disposition à la demande et les déploiements cloud
- Résumer le modèle de tarification à l’utilisation

```
video : xxx.mkv
```
*Figure. Module introduction - Welcome to the coffee shop*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Je suis **Blaine Sundrud**, de AWS Training and Certification. J’enseigne la technologie depuis plus longtemps que je ne suis prêt à l’admettre. Après avoir enseigné dans le secteur de la publication, j’ai rejoint AWS, où je donne des cours partout dans le monde dans divers domaines, comme la sécurité, l’architecture cloud, DevOps, le big data, l’IA et le ML, et l’histoire du théâtre. Ma maman était professeur. Mon papa également. Mon grand-père était barman. Je suis né pour ça. 


> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Bonjour, je m’appelle **Morgan Willis**, je suis technologue Cloud Senior chez AWS. J’ai intégré le monde de l’informatique il y a environ 10 ans. En chemin, j’ai décidé qu’il me manquait quelque chose. L’aspect de l’aide et de l’enseignement que je connaissais dans mon premier emploi dans le support informatique me manquait. Je suis donc passé à l’enseignement du développement de logiciels dans différentes régions des USA. Et je suis finalement arrivé ici, chez AWS, où comme technologue Cloud, j’aide les autres à progresser chaque jour dans le domaine du cloud. 

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Je m’appelle **Rudy Chetty**. Je viens de la ville ensoleillée du Cap en Afrique du Sud, la terre du biltong, des boerewors, et du bunny chow. Je suis architecte de solutions, et je travaille chez AWS depuis plus de trois ans. Enseigner est ma passion. Et j’ai hâte que vous vous plongiez dans le cours et que vous appreniez. Merci beaucoup et bonne chance. 

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Ce cours couvre l’essentiel des informations que vous devez comprendre, pour être à l’aise avec AWS, pour comprendre ses avantages pour votre entreprise. 


>![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> AWS propose une gamme impressionnante de services pour chaque entreprise, en commençant par les fondamentaux, comme le calcul, le stockage et les outils de sécurité réseau, par le biais de solutions complexes comme le blockchain, le machine learning, ou l’intelligence artificielle, et les plateformes de conception de robots, jusqu’aux outils les plus spécialisés comme les systèmes de gestion des productions vidéo, et les satellites orbitaux que vous pouvez louer à la minute. 


> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Mais c’est bien plus que ce que nous aurons le temps de voir dans un cours sur les fondamentaux comme celui-ci. Simplifions donc la conversation et commençons par le modèle fondamental de calcul dans le cloud. 

 
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Presque tous les centres de calcul modernes autour d’un modèle de base client-serveur. Je sais que ça peut être plus compliqué que cela, jetons donc un coup d’œil à notre café. 

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Ce café va nous fournir des métaphores sur le monde réel pour vous aider à comprendre pourquoi AWS peut changer le mode de fonctionnement de votre service informatique. 

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Faisons de Morgan la serveuse, la barista. Et je serai le client. Je fais une demande. Dans ce cas, c'est pour le café. Dans le monde de l’informatique, la demande pourrait concerner n’importe quoi. L’analyse du régime de pluies en Afrique du Sud par exemple, ou la dernière radio de votre genou, ou des vidéos de chatons. Quel que soit le secteur, un client fait une demande et, grâce aux autorisations, le serveur répond à cette demande. Tout ce que je veux est une boisson contenant de la caféine. 
> 
> Morgan représente la partie serveur du modèle client-serveur. Chez AWS, il s’appellerait un Amazon Elastic Compute Cloud, ou EC2, une instance EC2, un serveur virtuel. Ainsi, du point de vue architectural, notre transaction est très simple à expliquer. Moi, l’utilisateur, je fais une demande à Morgan, la serveuse. Morgan a confirmé que la demande est légitime, Dans ce cas, lui ai-je donné l’argent ? Il a ensuite renvoyé une réponse qui dans ce cas, est un berry blaster avec des éclats de caramel en supplément. 

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Dans le monde réel à présent, les applications peuvent être plus compliquées qu’une simple transaction avec un serveur unique. Dans une solution d’entreprise qui est plus mature, cela peut être assez complexe. 

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Pour éviter cette complexité, nous allons débuter par quelque chose de simple. Nous allons développer cette discussion pour que tout le monde comprenne facilement la manière dont ces concepts s’appuient les uns sur les autres. Donc, d’ici la fin du cours, ces concepts complexes, seront faciles à comprendre. Commençons par un concept clé d’AWS : vous ne payez que ce que vous utilisez. 

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Ce principe semble logique lorsque vous tenez un café. Les employés ne sont rémunérés que lorsqu’ils travaillent dans le café. Si Rudy et Morgan sont en dehors de leurs horaires, ils ne sont pas payés. Le propriétaire décide simplement du nombre de baristas nécessaire et paie seulement les heures travaillées. Par exemple, le café est sur le point de sortir une nouvelle boisson, le Pumpkin Monster Spice. En prévision de ce lancement, vous pouvez toujours mettre des dizaines de baristas toute la journée dans votre café, au cas où vous auriez une vague inattendue de clients à un moment donné de la journée. Mais soyons honnêtes. La plupart du temps, vous n’avez pas assez de clients pour pouvoir payer tous ces employés. 

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Pourtant, c’est exactement ce qui se produit dans un centre de données sur site. On ne peut pas simplement claquer des doigts et tripler sa capacité. Chez AWS, vous ne payez rien à l’avance. Et vous n’avez pas à vous inquiéter des contraintes de capacité. 

 
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Lorsque vous avez besoin d’instances, ou de baristas, il suffit de cliquer sur un bouton, et ils sont disponibles. Et quand vous n’en avez pas besoin, cliquez à nouveau et ils disparaissent, et vous arrêtez de les payer. De la même façon que vous ne payez pas les employés pour des heures où ils ne travaillent pas. 
 
> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Alors le paiement de ce dont vous avez besoin devient la première valeur clé pour beaucoup pour gérer votre entreprise sur AWS. C’est vraiment pour cela que nous sommes là, pour vous aider à comprendre comment AWS est conçu pour vous aider à mieux gérer votre entreprise. 

 
> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Nous espérons vous voir tout au long du cours, alors que nous approfondissons ces concepts, et nous vous aidons à progresser pour devenir un Cloud Practitioner.
</div>

## Qu’est-ce qu’un modèle client-serveur ?

Vous venez d’en apprendre davantage sur AWS et sur la façon dont presque tout le calcul moderne utilise un modèle client-serveur de base. Récapitulons ce qu’est un modèle client-serveur.

![client_serveur.png](/assets/img/clouds/tempo/client_serveur.png =300x)

*Figure. Une transaction entre un client et un serveur*

En informatique, un client peut être un navigateur web ou une application de bureau avec laquelle une personne interagit pour envoyer des requêtes aux serveurs informatiques. Un serveur peut être un service tel qu’Amazon Elastic Cloud Compute (Amazon EC2), un type de serveur virtuel.

Par exemple, supposons qu’un client fasse une demande pour un article, le score d’un jeu en ligne ou une vidéo amusante. Le serveur évalue les détails de cette requête et la remplit en renvoyant les informations au client.

# Cloud computing

```
video : xxx.mkv
```
*Figure. What is Cloud Computing*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}  
> Avant d'aborder plus en profondeur les rouages d’AWS, appuyons-nous sur une vue d’ensemble pour avoir une bonne définition de travail du cloud. Le cloud computing est la mise à disposition de ressources informatiques à la demande via Internet, avec un paiement à l’utilisation. Décomposons cela. La mise à la disposition à la demande indique qu’AWS dispose des ressources dont vous avez besoin, au moment où vous en avez besoin. Vous n’avez pas à nous le dire à l’avance que vous en aurez besoin. Soudain, vous êtes dans une situation exigeant 300 serveurs virtuels. Vous n’avez que quelques clics à faire pour les lancer. Ou vous avez besoin de 2000 téraoctets of stockage. Vous n’avez pas à nous le dire à l’avance, utilisez simplement le stockage dont vous avez besoin, quand vous en avez besoin. Vous n’en avez plus besoin, vous pouvez les renvoyer, aussi rapidement, et arrêter de les payer immédiatement. Ce type de flexibilité est juste impossible lorsque vous gérez vos propres centres de données. 
> 
> Le concept des ressources informatiques est en fait une grande partie de la philosophie d’AWS. On nous demande souvent pourquoi AWS a tant de produits et la réponse est assez simple : parce que les entreprises en ont besoin. S’il y a des éléments informatiques partagés entre plusieurs entreprises, alors il ne s’agit pas d’un facteur de différenciation. 
> 
> Prenons l’exemple d’une base de données MySQL. Si votre entreprise exécute une base de données MySQL, votre capacité à installer un moteur MySQL fait-elle de vous une meilleure entreprise que vos concurrents ? Probablement pas. Réalisez-vous des sauvegardes qui font de vous une entreprise supérieure aux autres entreprises de votre secteur ? Encore une fois, c’est discutable. Les données à l’intérieur de votre base de données, voici quelque chose de radicalement différent. La manière dont vous construisez vos tables et vous gérez les structures, c’est ce qui vous démarque définitivement de vos concurrents. Mais le moteur n’est qu’un moteur. 
> 
> Chez AWS, nous les appelons les tâches informatiques lourdes et indifférenciées. Des tâches courantes, souvent répétitives, et finalement chronophages ; Voici les tâches pour lesquelles AWS veut vous apporter des solutions. Afin que vous puissiez vous concentrer sur ce qui vous rend unique. Sur Internet, cela semble assez simple, mais cela implique que vous puissiez accéder à ces ressources avec une console de page web sécurisée ou par le biais d’un programme. 
> 
> Pas de contrats supplémentaires ou d’appels de vente nécessaires. Avec le paiement à l’utilisation, nous mettons à nouveau en avant ce que nous avons décrit ici dans le café. Vous n’employez pas du personnel 24 heures sur 24 comme vous le faites aux heures de pointe. En réalité, certaines heures, vous pourriez ne pas avoir de personnel du tout. Alors pourquoi payer pour les environnements de développeurs, par exemple, les week-ends, si vos développeurs ne travaillent pas les week-ends ?
</div>

## Modèles de déploiement pour le cloud computing

Lors de la sélection d’une stratégie de cloud, une entreprise doit tenir compte de facteurs tels que les composants d’applications de cloud requis, les outils de gestion des ressources préférés et les exigences de l’infrastructure informatique héritée.

Les trois modèles de déploiement de cloud computing sont : basé sur le cloud, sur site et hybride. 

Sélectionnez chaque onglet afin d’en savoir plus sur chaque catégorie.

#### tabs {.tabset}
##### DEPLOIEMENT BASE SUR LE CLOUD

- Exécuter l’ensemble des parties de l’application dans le cloud
- Migrer les applications existantes vers le cloud
- Concevoir et créer de nouvelles applications dans le cloud

Dans un modèle de **déploiement basé sur le cloud**, vous pouvez migrer des applications existantes vers le cloud, ou vous pouvez concevoir et créer de nouvelles applications dans le cloud. Vous pouvez créer ces applications sur une infrastructure de bas niveau, ce qui requiert que votre personnel informatique les gère. Vous pouvez également les créer à l’aide de services de niveau supérieur qui réduisent les exigences de gestion, d’architecture et de mise à l’échelle de l’infrastructure principale.

Par exemple, une entreprise peut créer une application composée de serveurs virtuels, de bases de données et de composants de réseaux qui sont entièrement basés sur le cloud.

##### DEPLOIEMENT SUR SITE

- Déployer des ressources à l’aide d’outils de virtualisation et de gestion des ressources
- Accroitre l’utilisation des ressources en utilisant les technologies de gestion des applications et de virtualisation

**Le déploiement sur site** est également connu sous le nom de déploiement de cloud privé. Dans ce modèle, les ressources sont déployées sur site par le biais d’outils de virtualisation et de gestion des ressources.

Par exemple, des applications peuvent s’exécuter sur une technologie qui réside entièrement dans votre centre de données sur site. Bien que ce modèle ressemble beaucoup à une infrastructure informatique héritée, son intégration des technologies de gestion des applications et de virtualisation contribue à accroître l’utilisation des ressources.

##### DEPLOIEMENT HYBRIDE

- Connecter des ressources basées sur le cloud à l’infrastructure sur site
- Intégrer des ressources basées sur le cloud aux applications informatiques héritées

Dans un **déploiement hybride**, les ressources basées sur le cloud sont connectées à une infrastructure sur site. Il sera possible d’adopter cette approche dans un certain nombre de situations. Par exemple, en cas d’applications héritées qui sont mieux gérées sur site, ou de réglementations gouvernementales qui exigent que votre entreprise conserve certains dossier sur site.

Supposons qu’une entreprise souhaite utiliser des services cloud capables d’automatiser l’analytique et le traitement des données par lots. Cependant, elle possède plusieurs applications héritées qui conviennent mieux à une utilisation sur site et ne seront donc pas migrées vers le cloud. Avec un déploiement hybride, l’entreprise pourrait conserver les applications héritées sur site tout en tirant profit des services d’analytique et de données qui s’exécutent dans le cloud.


## Avantages du cloud computing

Envisagez les raisons pour lesquelles une entreprise choisirait d’adopter une approche de cloud computing particulière pour répondre à ses besoins.

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="Remplacement des dépenses initiales par des dépenses variables" value="true">

Les dépenses initiales font référence aux centres de données, aux serveurs physiques et aux autres ressources dans lesquelles vous devez investir avant de les utiliser. Les dépenses variables impliquent que vous payez uniquement les ressources de calcul que vous utilisez, plutôt que d’investir massivement dans des centres de données et des serveurs avant de savoir comment les utiliser.

En adoptant une approche de cloud computing qui confère l’avantage des dépenses variables, les entreprises peuvent implémenter des solutions innovantes tout en réalisant des économies.
</div>

<div class="collapseContent" display="Ne dépensez plus d'argent pour l'exécution et la maintenance de centre de données" value="true">

Le calcul en centres de données exige souvent que vous consacriez plus d’argent et de temps à gérer l’infrastructure et les serveurs. 

L’un des avantages du cloud computing est la capacité de se concentrer moins sur ces tâches et plus sur vos applications et vos clients.

</div>

<div class="collapseContent" display="Maîtrisez la capacité nécessaire" value="true">

Grâce au cloud computing, vous n’avez pas à prédire la capacité d’infrastructure dont vous aurez besoin avant de déployer une application.

Par exemple, vous pouvez lancer des instances Amazon EC2 lorsque nécessaire et payer uniquement le temps de calcul que vous utilisez. Au lieu de payer des ressources inutilisées ou de devoir faire face à une capacité limitée, vous pouvez accéder uniquement à la capacité dont vous avez besoin. Vous pouvez également effectuer une mise à l’échelle ascendante ou une mise à l’échelle descendante en réponse à la demande.

</div>

<div class="collapseContent" display="Profitez d'importantes économies d'échelle" value="true">

À l’aide du cloud computing, vous pouvez obtenir un coût variable moins élevé que celui que vous auriez de votre côté.

Étant donné que l’utilisation par des centaines de milliers de clients peut être agrégée dans le cloud, les fournisseurs tels qu’AWS peuvent bénéficier de plus grandes économies d’échelle. L’économie d’échelle diminue les prix du paiement à l’utilisation.

</div>

<div class="collapseContent" display="Gagnez en vitesse et en agilité" value="true">

La flexibilité du cloud computing facilite le développement et le déploiement d’applications.

Cette flexibilité vous offre plus de temps pour expérimenter et innover. Pour le calcul dans les centres de données, obtenir les nouvelles ressources dont vous avez besoin peut prendre des semaines. Par comparaison, le cloud computing vous permet d’accéder à de nouvelles ressources en quelques minutes.

</div>

<div class="collapseContent" display="Passer à l'international en quelques minutes" value="true">

La présence au niveau mondial du AWS Cloud vous permet de déployer rapidement des applications auprès de clients du monde entier, tout en leur offrant une faible latence. Cela signifie que même si vous êtes situé dans une autre partie du monde que vos clients, ces derniers peuvent accéder à vos applications avec un minimum de délais. 

Plus tard dans ce cours, vous explorerez plus en détail l’infrastructure mondiale AWS. Vous examinerez certains des services que vous pouvez utiliser pour diffuser du contenu à vos clients du monde entier.

</div>

## Ressources supplémentaires

Pour en savoir plus sur les concepts explorés lors du module 1, consultez ces ressources.

- [Glossaire AWS](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html)
- [Livre blanc : Présentation d’Amazon Web Services](https://d0.awsstatic.com/whitepapers/aws-overview.pdf)
- [Principes fondamentaux d’AWS : présentation](https://aws.amazon.com/getting-started/fundamentals-overview/)
- [Qu’est-ce que le cloud computing ?](https://aws.amazon.com/what-is-cloud-computing/)
- [Types de cloud computing](https://aws.amazon.com/types-of-cloud-computing/)
- [Cloud computing avec AWS](https://aws.amazon.com/what-is-aws/)

# Module 1 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées pour renforcer votre compréhension des concepts.

**Qu’est-ce que le cloud computing ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">La sauvegarde des fichiers stockés sur les ordinateurs de bureau et les appareils mobiles pour éviter la perte de données</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Le déploiement d’applications connectées à une infrastructure sur site</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">L’exécution de code sans avoir besoin de gérer ou de mettre en service des serveurs</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">La mise à disposition de ressources informatiques et d’applications à la demande via Internet, avec une tarification à l’utilisation</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Mise à disposition de ressources informatiques et d’applications à la demande via Internet, avec une tarification à l’utilisation**.

Les autres réponses sont incorrectes, car :

- Il est possible de sauvegarder des fichiers dans le cloud, mais cette option de réponse ne décrit pas le cloud computing dans son ensemble.
- Le déploiement d’applications connectées à une infrastructure sur site est un exemple de cas d’utilisation pour un déploiement de cloud hybride. N’oubliez pas que le cloud computing dispose également de modèles de déploiement cloud et sur site (ou cloud privé).
- AWS Lambda est un service AWS qui vous permet d’exécuter du code sans avoir besoin de gérer ou de mettre en service des serveurs. Cette description ne décrit pas le cloud computing dans son ensemble. AWS Lambda est expliqué plus en détail ultérieurement dans le cours.
</div>

**Quel autre nom peut-on donner au déploiement sur site ?**

<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">Déploiement de cloud privé</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Application basée sur le cloud</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Déploiement hybride</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">AWS Cloud</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Déploiement de cloud privé**.

Les autres réponses sont incorrectes, car :

- Les applications basées sur le cloud sont entièrement déployées dans le cloud et aucune de leurs parties ne s’exécute sur site.
- Un déploiement hybride connecte l’infrastructure et les applications entre les ressources basées sur le cloud et les ressources existantes qui ne sont pas dans le cloud, telles que les ressources sur site. Toutefois, un déploiement hybride n’est pas équivalent à un déploiement sur site, car il implique des ressources situées dans le cloud.
- AWS Cloud propose trois modèles de déploiement de cloud : cloud, hybride et sur site. Cette réponse est incorrecte, car le AWS Cloud n’équivaut pas uniquement à un déploiement sur site.
</div>

**Dans quelle mesure la mise à l’échelle du cloud computing vous aide-t-elle à réduire vos coûts ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">Vous n’avez pas à investir dans des ressources technologiques avant de les utiliser.</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">L’utilisation agrégée du cloud par un grand nombre de clients se traduit par une baisse des prix à l’utilisation.</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">L’accès à des services à la demande permet d’éviter une capacité excédentaire ou limitée.</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">Vous pouvez rapidement déployer des applications auprès des clients et leur fournir une faible latence.</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **L’utilisation agrégée du cloud par un grand nombre de clients se traduit par une baisse des prix à l’utilisation**.

Cette réponse décrit comment les clients peuvent bénéficier d’importantes économies d’échelle dans le cloud computing.

Les autres réponses sont incorrectes, car :

- Ne pas devoir investir dans des ressources technologiques avant de les utiliser se réfère à Remplacer les dépenses initiales par les dépenses variables.
- Accéder aux services à la demande pour éviter une capacité excédentaire ou limitée se réfère à Maîtriser la capacité nécessaire.
- Le déploiement rapide des applications auprès des clients et leur faible latence se rapporte à Passer à l’international en quelques minutes..

</div>

Le module suivant examine les services de calcul AWS.