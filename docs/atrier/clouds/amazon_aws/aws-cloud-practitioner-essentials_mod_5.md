---
title: Module 5 - Stockage et bases de données
description: 
published: true
date: 2023-04-21T10:56:20.175Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:13.142Z
---

# Module 5 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Résumer le concept de base du stockage et des bases de données.
- Décrire les avantages d’Amazon Elastic Block Store (Amazon EBS).
- Décrire les avantages d’Amazon Simple Storage Solution (Amazon S3).
- Décrire les avantages d’Amazon Elastic File System (Amazon EFS).
- Résumer les différentes solutions de stockage.
- Décrire les avantages d’Amazon Relational Database Service (RDS).
- Décrire les avantages d’Amazon DynamoDB.
- Résumer les différents services de base de données.

```
video : xxx.mkv
```
*Figure. Module introduction - Storage and databases*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Et bien, nous avons un café qui fonctionne bien, maintenant. Nous avons des clients, beaucoup qui sont satisfaits. En fait, nous disposons d’une architecture élastique, évolutive, résistante aux catastrophes, et à coûts optimisés dont le réseau est maintenant mondial et hautement sécurisé et peut être déployé entièrement par programmation. 
> 
> Nous avons maintenant besoin d’une façon de montrer notre appréciation à tous les clients fidèles. Que diriez-vous d'un programme de fidélité pour les visiteurs fréquents ? Nous pourrions aussi distribuer des cartes à pointer, mais honnêtement, leur suivi est difficile et nous aimerions les utiliser pour mieux connaître nos clients. 
> 
> Nous avons besoin de cartes numériques, ce qui signifie que vous devons pouvoir faire le suivi de nos clients, ce qu’ils commandent, les quantités achetées. Cela les aidera, les clients, à obtenir tous les avantages auxquels ils ont droit et cela nous aidera à mieux connaître notre clientèle. 
> 
> Cela signifie que nous avons besoin de bases de données. De bases de données et de stockage. Et pas n'importe quelle base de données. Nous devons nous assurer que vous choisissons la bonne base de données pour la tâche et bon stockage pour les types de données. 
> 
> Vous pouvez avoir de nombreux besoins différents d'utilisation de données et de types de données. Parlons des différents services AWS disponibles pour vous aider à construite la solution de données parfaite.
</div>
  

# Stockages d’instances et Amazon Elastic Block Store (Amazon EBS)

```
video : xxx.mkv
```
*Figure. Instance stores and Amazon Elastic Block Store*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Lorsque vous utilisez Amazon EC2 pour exécuter vos applications commerciales, ces applications doivent accéder au processeur, à la mémoire, au réseau et au stockage. Les instances EC2 vous donnent accès à tous ces différents composants, et dès maintenant, concentrons-nous sur l’accès au stockage. Lorsque les applications s’exécutent, elles ont souvent besoin d’accéder au stockage de niveau bloc. 
> 
> Imaginez que le stockage de niveau de bloc est un endroit pour stocker des fichiers. Un fichier étant une série d’octets stockés dans des blocs sur le disque. Lorsqu’un fichier est mis à jour, la série entière de blocs n’est pas remplacée. Au lieu de cela, les parties qui changent sont mises à jour. Cela en fait un type de stockage efficace lorsque vous travaillez avec des applications comme les bases de données, les logiciels d’entreprise ou les systèmes de fichiers. 
> 
> Lorsque vous utilisez votre ordinateur portable ou votre ordinateur personnel, vous accédez au stockage de niveau bloc. Tout le stockage de niveau bloc est votre disque dur, dans le cas présent. Les instances EC2 ont également des disques durs. Il en existe quelques types différents. 
> 
> Lorsque vous lancez une instance EC2, en fonction du type d’instance EC2 que vous avez lancée, elle peut vous fournir un stockage local appelé volumes de stockage d’instance. Ces volumes sont attachés physiquement à l’hôte sur lequel sont exécutées vos instances EC2. Et vous pouvez y écrire comme sur un disque dur normal. Le problème est que, comme ce volume est attaché à l’hôte physique sous-jacent, si vous arrêtez ou vous résiliez votre instance EC2, toutes les données inscrites sur le volume de stockage d’instance sont effacées. En effet, si vous démarrez votre instance à partir d’un état d’arrêt, cette instance EC2 démarrera probablement sur un autre hôte. Un hôte où ce volume n'existe pas. Vous savez que les instances EC2 sont des machines virtuelles, et donc l’hôte sous-jacent peut passer d’arrêter à démarrer une instance. 
> 
> En raison de la nature éphémère ou temporaire des volumes de stockage d’instance, ils sont utiles dans des situations où des données peuvent être perdues, étant inscrites sur le lecteur. Comme les données temporaires, les données Scratch, et les données qui peuvent être facilement recréées sans conséquences. 
> 
> Très bien, je vous demande de n’écrire de données importantes sur les lecteurs qui accompagnent les instances EC2. Je sais que cela peut être un peu préoccupant car, évidemment, vous aurez besoin d’un endroit où écrire les données qui persistent en dehors de cycle de vie d’une instance EC2. il n’est pas souhaitable que toute votre base de données soit supprimée à chaque fois que vous arrêtez une instance EC2. Ne vous inquiétez pas, c’est là qu’un service appelé Amazon Elastic Block Store, ou EBS, entre en jeu. 
> 
> Avec EBS, vous pouvez créer des disques durs virtuels appelés volumes EBS, que vous pouvez attacher à vos instances EC2. Ce sont des lecteurs séparés des volumes de stockage d’instance locale, et ils ne sont pas directement liés à l’hôte et facilement exécutables. Cela signifie que les données que vous écrivez sur un volume EBS peuvent persister entre les arrêts et les démarrages d’une instance EC2. 
> 
> Les volumes EBS existent dans toutes les tailles et tous les types différents. Pour les utiliser vous devez définir la taille, le type et les configurations du volume dont vous avez besoin. Mettre en service le volume, puis l’attacher à votre instance EC2. De là, vous pouvez configurer votre application pour écrire dans le volume et c’est prêt. Si vous arrêtez puis démarrez l’instance EC2, les données dans le volume persistent. 
> 
> Le cas d’utilisation pour les volumes EBS est d’avoir un disque dur qui est persistant, et sur lequel vos applications peuvent écrire, il est probablement important que vous sauvegardiez ces données. EBS vous permet de prendre des sauvegardes incrémentielles de vos données appelées instantanés. Il est très important de prendre des instantanés réguliers de vos volumes EBS. De cette manière, si un lecteur est corrompu, vous ne perdez pas vos données. Et vous pouvez restaurer ces données à partir d'un instantané.
</div>
  
  
## Stockages d’instances

Les volumes de stockage de niveau bloc se comportent comme des disques durs physiques.

Un **[stockage d’instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)** fournit un stockage temporaire de niveau bloc pour une instance Amazon EC2. Un stockage d’instance désigne un stockage sur disque qui est physiquement connecté à l’ordinateur hôte pour une instance EC2 et qui possède donc la même durée de vie que l’instance. Lorsque l’instance est résiliée, vous perdez toutes les données du stockage d’instance.

![aws_stockage_01.png](/assets/img/clouds/tempo/aws_stockage_01.png)


#### Tabs {.tabset}
##### Etape 1

Une instance Amazon EC2 avec un stockage d’instances attaché est en cours d’exécution.

![aws_stockage_02.png](/assets/img/clouds/tempo/aws_stockage_02.png)

##### Etape 2

L’instance est arrêtée ou résiliée.

![aws_stockage_03.png](/assets/img/clouds/tempo/aws_stockage_03.png)

##### Etape 3

Toutes les données du stockage d’instance attaché sont supprimées.

![aws_stockage_04.png](/assets/img/clouds/tempo/aws_stockage_04.png)

##### Résumé
Les instances Amazon EC2 sont des serveurs virtuels. Si vous démarrez une instance à partir d’un état arrêté, l’instance peut démarrer sur un autre hôte, où le volume de stockage d’instance précédemment utilisé n’existe pas. Par conséquent, AWS recommande les stockages d’instance pour les cas d’utilisation impliquant des données temporaires dont vous n’avez pas besoin à long terme.

![aws_stockage_05.png](/assets/img/clouds/tempo/aws_stockage_05.png)

**[Amazon Elastic Block Store (Amazon EBS)](https://aws.amazon.com/ebs)** est un service qui fournit des volumes de stockage de niveau bloc, que vous pouvez utiliser avec des instances Amazon EC2. Si vous arrêtez ou résiliez une instance Amazon EC2, toutes les données du volume EBS attaché restent disponibles.

Pour créer un volume EBS, vous définissez la configuration (comme la taille et le type de volume) et vous le mettez en service. Après avoir été créé, un volume EBS peut être attaché à une instance Amazon EC2.

Étant donné que les volumes EBS sont destinés à des données qui doivent être conservées, il est important de sauvegarder les données. Vous pouvez effectuer des sauvegardes progressives des volumes EBS en créant des instantanés Amazon EBS.

## Instantanés Amazon EBS

![aws_stockage_06.png](/assets/img/clouds/tempo/aws_stockage_06.png)

Un **[instantané EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)** est une sauvegarde progressive. Cela signifie que la première sauvegarde d’un volume copie toutes les données. Pour les sauvegardes suivantes, seuls les blocs de données qui ont été modifiés depuis le dernier instantané sont enregistrés. 

Les sauvegardes progressives sont différentes des sauvegardes complètes, dans lesquelles toutes les données d’un volume de stockage sont copiées à chaque sauvegarde. La sauvegarde complète inclut les données qui n’ont pas été modifiées depuis la dernière sauvegarde.

## Contrôle des connaissances

Dans la section suivante, associez les caractéristiques qui décrivent les stockages d’instance et les volumes Amazon EBS aux bons services. Faites glisser les éléments pour les placer dans la catégorie appropriée.

![aws_stockage_07.png](/assets/img/clouds/tempo/aws_stockage_07.png)

![aws_stockage_08.png](/assets/img/clouds/tempo/aws_stockage_08.png)

![aws_stockage_09.png](/assets/img/clouds/tempo/aws_stockage_09.png)

![aws_stockage_10.png](/assets/img/clouds/tempo/aws_stockage_10.png)

# Amazon Simple Storage Service (Amazon S3)

```
video : xxx.mkv
```
*Figure. Amazon Simple Storage Service*

<div class="collapseContent" display="Transcription" value="true">
  
> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Bienvenue dans cette vidéo sur Amazon Simple Storage Service, ou S3. D’après le nom, vous avez probablement deviné qu’il s’agit d’un service de stockage et qu’il est...simple. La plupart des entreprises ont des données devant être stockées quelque part. Dans le café, ce serait les tickets, les images, les fichiers Excel, les vidéos de formation du personnel, et même des fichiers texte, par exemple. Pour stocker ces fichiers, S3 est très utile car il s’agit d’un magasin de données qui vous permet de stocker et récupérer une quantité illimitée de données à toute échelle. 
> 
> Les données sont stockées comme objets, mais au lieu de les stocker dans un répertoire, vous les stockez dans des compartiments. Imaginez un fichier qui se trouve sur votre disque dur, qui est un objet, et imaginez un répertoire, qui est un compartiment. La taille maximale des objets que vous pouvez télécharger est de cinq téraoctets. Vous pouvez également créer plusieurs versions des objets pour les protéger et que des objets ne soient pas accidentellement effacés. Cela signifie que vous conservez toujours les versions précédentes d’un objet, comme un journal de suivi. Vous pouvez même créer plusieurs compartiments et les stocker dans différentes classes ou niveaux de données. Vous pouvez ensuite créer des autorisations pour limiter qui peut voir ou même accéder à des objets.
> 
> Vous pouvez même séparer des données entre différents niveaux. Ces niveaux offrent des mécanismes pour divers cas d’utilisation de stockage différents comme les données auxquelles il faut accéder fréquemment, par rapport aux données d’audit qui doivent être conservées pendant plusieurs années. Passons en revue ceux qui sont importants, d'accord ? 
> 
> Le premier ici s’appelle S3 Standard et est livré avec 11 neufs de durabilité. Cela signifie qu’un objet stocké dans S3 Standard a une probabilité de 99,999999999 pourcent - ça fait beaucoup de neuf - de rester intact après une période d’un an. C'est plutôt cool. De plus, les données sont stockées pour que AWS puisse résister à la perte simultanée de données dans deux unités de stockage distinctes. C'est parce que les données sont stockées dans au moins trois installations. Donc plusieurs copies résident à travers les emplacements. Une autre façon pratique d’utiliser S3 est l’hébergement de site web statique, où un site web statique est un ensemble de fichiers HTML et chaque fichier est semblable à une page physique du site réel. Vous pouvez le faire en chargeant simplement tous vos HTML vos actifs web statiques etc. dans un compartiment puis en cochant une case pour l’héberger en tant que site web statique. Vous pouvez alors saisir l’URL du compartiment et voilà ! Site web instantané. Et on dit statique, mais cela ne signifie pas que vous ne pouvez pas avoir des animations ou d’éléments en mouvement sur votre site web. C'est une super façon de démarrer ce blog de café. 
> 
> La classe de stockage suivante est appelée S3 Accès peu fréquent ou S3-IA. C’est parfait pour les données consultées peu fréquemment, mais nécessitant un accès rapide le cas échéant. Cela signifie que c’est un endroit parfait pour stocker les sauvegardes, les fichiers de reprise après sinistre, ou tout objet qui requiert un stockage à long terme. 
> 
> Une autre classe ou un autre niveau de stockage se prête à cet exemple que nous avons eu plus tôt sur les données d’audit. Imaginons que nous devons conserver des données pendant plusieurs années à des fins d’audit. Et nous n'avons pas besoin qu'elles soient récupérées très rapidement. Alors vous pouvez utiliser Amazon S3 Glacier pour archiver ces données. Pour utiliser Glacier, vous pouvez simplement y déplacer des données ou créer des coffres-forts et les remplir d’archives. Et si vous avez des exigences de conformité pour conserver les données pendant une certaine période de temps, vous pouvez utiliser une stratégie de verrouillage de coffre S3 Glacier et verrouiller votre coffre. Vous pouvez définir des contrôles, tels que la technique Écriture unique/ lecture multiple (WORM), dans une stratégie de verrouillage de coffre et empêcher que la stratégie fasse l'objet de modifications ultérieures. Une fois verrouillée, la stratégie ne peut plus être modifiée. Vous avez aussi trois options pour la récupération, qui vont de plusieurs minutes à des heures, et vous avez l’option de charger directement vers Glacier ou à l’aide des stratégies de cycle de vie S3. 
> 
> En fait, je crois que nous n'avons pas parlé des stratégies de cycle de vie jusqu’à maintenant. Ce sont des stratégies que vous pouvez créer et qui peuvent déplacer automatiquement des données entre niveaux. Par exemple, si nous avons besoin de garder un objet dans S3 Standard pendant 90 jours, et ensuite le déplacer vers S3-IA pendant les 30 prochains jours. Puis après les 120 jours au total, nous voulons qu'il soit déplacé dans S3 Glacier. Avec les stratégies de cycle de vie, vous créez cette configuration sans modifier votre code d’application et elle effectuera ces déplacements automatiquement pour vous. C'est un autre exemple de service AWS géré qui allège les démarches, afin que vous puissiez vous concentrer davantage sur les besoins de votre entreprise. 
> 
> Il existe aussi d'autres classes de stockage, comme S3 unizone - Accès peu fréquent et S3 Glacier Deep Archive, à votre disposition. Joyeux stockage.
</div>
  
  
## Stockage d’objets

![aws_stockage_11.png](/assets/img/clouds/tempo/aws_stockage_11.png)

Dans le **stockage d’objets**, chaque objet se compose de données, de métadonnées et d’une clé.

Les données peuvent être une image, une vidéo, un document texte ou tout autre type de fichier. Les métadonnées contiennent des informations sur les données, leur utilisation, la taille de l’objet, etc. La clé d’un objet est son identifiant unique.

> Rappelons que lorsque vous modifiez un fichier dans un stockage par bloc, seules les pièces qui ont été modifiées sont mises à jour. Lorsqu’un fichier dans le stockage d’objets est modifié, l’objet entier est mis à jour.
{.is-info}

## Amazon Simple Storage Service (Amazon S3)

**[Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/s3/)** est un service qui fournit un stockage au niveau objet. Amazon S3 stocke les données sous forme d’objets dans des compartiments.

![aws_s3_01.png](/assets/img/clouds/tempo/aws_s3_01.png)

Vous pouvez télécharger n’importe quel type de fichier sur Amazon S3, comme des images, des vidéos, des fichiers texte, etc. Par exemple, vous pouvez utiliser Amazon S3 pour stocker des fichiers de sauvegarde, des fichiers multimédias pour un site web ou des documents archivés. Amazon S3 offre un espace de stockage illimité. La taille maximale de fichier pour un objet dans Amazon S3 est de 5 To.

Lorsque vous téléchargez un fichier dans Amazon S3, vous pouvez définir des autorisations pour en contrôler la visibilité et l’accès. Vous pouvez également utiliser la fonction de contrôle des versions Amazon S3 pour suivre les modifications apportées à vos objets au fil du temps.

## Classes de stockage Amazon S3

Avec Amazon S3, vous ne payez que les services que vous utilisez. Vous pouvez choisir parmi [une gamme de classes de stockage](https://aws.amazon.com/s3/storage-classes) pour trouver la solution adaptée à vos besoins opérationnels et financiers. Lorsque vous sélectionnez une classe de stockage Amazon S3, tenez compte de ces deux facteurs :

- Fréquence à laquelle vous prévoyez de récupérer vos données
- Mesure dans laquelle vos données doivent être disponibles

Pour en savoir plus sur les classes de stockage Amazon S3, sélectionnez le symbole + en regard de chaque catégorie.

<div class="collapseContent" display="S3 Standard" value="true">

- Conçu pour les données fréquemment consultées
- Stocke les données dans un minimum de trois zones de disponibilité

S3 standard offre une disponibilité élevée pour les objets. Cela en fait un bon choix pour un large éventail de cas d’utilisation, tels que les sites web, la distribution de contenu et l’analytique de données. S3 standard a un coût plus élevé que les autres classes de stockage destinées aux données rarement consultées et au stockage d’archives.
</div>

<div class="collapseContent" display="S3 Standard - Accès peu fréquent" value="true">

- Idéal pour les données rarement consultées
- Semblable à S3 standard, mais proposé à un prix de stockage inférieur et à un prix de récupération plus élevé

S3 standard – Accès peu fréquent est idéal pour les données rarement consultées, mais nécessitant une disponibilité élevée en cas de besoin. S3 Standard et S3 Standard – Accès peu fréquent stockent les données dans un minimum de trois zones de disponibilité. S3 Standard – Accès peu fréquent offre le même niveau de disponibilité que S3 Standard, mais est proposé à un prix de stockage inférieur et à un prix de récupération plus élevé.
</div>

<div class="collapseContent" display="S3 unizone - Accès peu fréquent" value="true">

- Stocke les données dans une zone de disponibilité unique
- Prix de stockage inférieur à S3 standard – Accès peu fréquent

Par rapport à S3 standard et S3 standard – Accès peu fréquent, qui stockent les données dans un minimum de trois zones de disponibilité, S3 unizone – Accès peu fréquent les stocke dans une zone de disponibilité unique. Cela en fait une bonne classe de stockage dans les conditions suivantes :

- Vous souhaitez économiser sur le stockage
- Vous pouvez facilement reproduire vos données en cas de défaillance de la zone de disponibilité
</div>

<div class="collapseContent" display="S3 Intelligen-Tiering" value="true">

- Idéal pour les données avec des modèles d’accès inconnus ou changeants
- Nécessite des frais mensuels de contrôle et d’automatisation par objet

Dans la classe de stockage S3 Intelligent-Tiering, Amazon S3 contrôle les schémas d’accès des objets. Si vous n’avez pas accédé à un objet pendant 30 jours consécutifs, Amazon S3 le déplace automatiquement vers le niveau d’accès peu fréquent, S3 standard – Accès peu fréquent. Si vous accédez à un objet du niveau d’accès peu fréquent, Amazon S3 le déplace automatiquement vers le niveau d’accès fréquent, S3 Standard.
</div>

<div class="collapseContent" display="S3 Glacier" value="true">

- Stockage économique conçu pour l’archivage des données
- Capable de récupérer des objets entre quelques minutes et quelques heures

S3 Glacier est une classe de stockage économique idéale pour l’archivage des données. Par exemple, vous pouvez utiliser cette classe de stockage pour stocker des registres clients archivés ou des photos et des fichiers vidéo plus anciens.
</div>

<div class="collapseContent" display="S3 Glacier Deep Archive" value="true">

- Classe de stockage d’objets la moins coûteuse idéale pour l’archivage
- Capable de récupérer des objets dans un délai de 12 heures

Lorsque vous devez choisir entre Amazon S3 Glacier et Amazon S3 Glacier Deep Archive, tenez compte de la rapidité à laquelle vous devez récupérer les objets archivés. Vous pouvez récupérer des objets stockés dans la classe de stockage S3 Glacier entre quelques minutes et quelques heures. En comparaison, vous pouvez récupérer des objets stockés dans la classe de stockage S3 Glacier Deep Archive dans un délai de 12 heures.
</div>

## Contrôle des connaissances

**Vous souhaitez stocker des données qui sont rarement consultées, mais qui doivent être immédiatement disponibles en cas de besoin. Quelle classe de stockage Amazon S3 devriez-vous utiliser ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">S3 Intelligent-Tiering</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">S3 Glacier Deep Archive</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">S3 standard – Accès peu fréquent</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">S3 Glacier</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **S3 standard – Accès peu fréquent**.

La classe de stockage S3 standard – Accès peu fréquent est idéale pour les données rarement consultées, mais nécessitant une disponibilité élevée en cas de besoin. S3 Standard et S3 Standard – Accès peu fréquent stockent les données dans un minimum de trois zones de disponibilité. S3 standard – Accès peu fréquent offre le même niveau de disponibilité que S3 Standard, mais à un prix de stockage moindre.

Les autres réponses sont incorrectes, car :

- Dans la classe de stockage S3 Intelligent-Tiering, Amazon S3 contrôle les modèles d’accès des objets. Si vous n’avez pas accédé à un objet pendant 30 jours consécutifs, Amazon S3 le déplace automatiquement vers le niveau d’accès peu fréquent, S3 standard – Accès peu fréquent. Si vous accédez à un objet du niveau d’accès peu fréquent, Amazon S3 le déplace automatiquement vers le niveau d’accès fréquent, S3 Standard.
- S3 Glacier et S3 Glacier Deep Archive sont des classes de stockage économiques idéales pour l’archivage de données. Ces services ne seraient pas le meilleur choix dans ce scénario, qui requiert une disponibilité élevée. Vous pouvez récupérer des objets stockés dans la classe de stockage S3 Glacier entre quelques minutes et quelques heures. En comparaison, vous pouvez récupérer des objets stockés dans la classe de stockage S3 Glacier Deep Archive dans un délai de 12 heures.
</div>
  
  
## Comparaison entre Amazon EBS et Amazon S3

```
video : xxx.mkv
```
*Figure. Comparing Amazon EBS and Amazon S3*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> AWS Cloud Practitioners, bienvenue au choc des classes de stockage ! Dans la catégorie du stockage par bloc, avec une capacité pouvant atteindre 16 téraoctets chacun, et une capacité unique de survivre à la résiliation de leurs instances Amazon EC2, leur état solide, et leurs plateaux en rotation, ce sont les Amazon Elastic Block Storage ! 
> 
> Dans la catégorie du stockage régional des objets, avec une capacité de stockage illimitée, et des objets individuels d’une taille de 5 000 gigaoctets, spécialisés dans l’Écriture unique/lecture multiple, 99 ,999 999 999 % durables, ce sont les Amazon Simple Storage Service !
> 
> Face à face, chaque classe de stockage dispose de la meilleure conception distribuée de façon dynamique pour différentes exigences de stockage. Quelle classe de stockage finira par triompher dans ce féroce combat ? Pour savoir qui sera le gagnant vous devez préciser un cas d’utilisation. 
> 
> Premier round. Imaginons que vous exécutez un site web d’analyse de photos où les utilisateurs chargent une photo d’eux, et votre application trouve les animaux qui leur ressemblent. Vous disposez potentiellement de millions de photos d’animaux qui doivent toutes être indexées et éventuellement consultées par des milliers de personnes en même temps. C'est le cas d'utilisation parfait pour S3. S3 est déjà activé sur le web. Chaque objet a déjà une URL dont vous pouvez contrôler les droits d’accès pour décider qui peut voir ou gérer l’image. Elle est distribuée à l’échelle régionale, ce qui signifie qu’elle a 11 neuf de durabilité, inutile donc de se préoccuper des stratégies de sauvegarde. S3 est votre stratégie de sauvegarde. De plus les économies sont énormes et dépassent la même charge de stockage sur EBS. Avec l’avantage supplémentaire d’être sans serveur, aucune instance Amazon EC2 n’est requise. On dirait que S3 est le vainqueur de ce round. 
> 
> Mais attention, round deux, vous avez un fichier vidéo de 80 gigaoctets sur lequel vous effectuez des corrections. Pour connaître la meilleure classe de stockage, nous devons comprendre la différence entre le stockage d’objets et le stockage par bloc. Le stockage d’objets traite n’importe quel fichier comme un objet complet et discret. C’est idéal pour les documents, et les images, et les fichiers vidéos qui sont chargés et consommés en tant qu’objets entiers but chaque fois que l’objet est modifié, vous devez télécharger à nouveau l’intégralité du fichier. Il n'y a pas de mises à jour delta. Le stockage par bloc décompose ces fichiers en petites parties ou blocs de composants. Cela signifie que, pour ce fichier de 80 gigaoctets, lorsque vous modifiez une scène du film et sauvegardez le changement, le moteur ne met à jour que les blocs où se trouvent ces bits. Si vous faites plusieurs micro-modifications, avec EBS, Elastic Block Storage, est le cas d’utilisation parfait. Si vous utilisez S3, à chaque fois que vous enregistrez des modifications, le système doit charger les 80 gigaoctets, la totalité, à chaque fois. EBS remporte clairement le deuxième round. 
> 
> Cela signifie que si vous utilisez des objets complets ou seulement des modifications occasionnelles, S3 l’emporte. Si vous effectuez des fonctions complexes de lecture, écrite ou modifications, alors EBS est sans aucun doute votre vainqueur par K.O. Votre vainqueur dépend de votre charge de travail individuelle. Chaque service est le bon pour des besoins spécifiques. Une fois que vous avez compris ce dont vous avez besoin, vous saurez quel est le service qui est votre champion !
</div>
  
  
# Amazon Elastic File System (Amazon EFS)

```
video : xxx.mkv
```
*Figure. Amazon Elastic File System*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Le suivant sur la liste des services de stockage, c'est Amazon Elastic File System, ou ce que nous appelons EFS. EFS est un système de fichiers géré. Il est extrêmement courant pour les entreprises d’avoir des systèmes de fichiers partagés dans leurs applications. Par exemple, vous pouvez avoir plusieurs serveurs exécutant des analyses sur de grandes quantités de données stockées sur un système de fichiers partagés. Ces données ont traditionnellement été hébergées sur site. Dans ces centres de données sur site, vous devriez vous assurer que le stockage que vous avez peut prendre en charge la quantité de données que vous stockez. Assurez-vous que les sauvegardes sont effectuées, et que les données sont stockées de manière redondante et de gérer tous les serveurs hébergeant ces données. 
> 
> Heureusement, avec AWS, vous n'avez pas besoin de vous inquiéter de l’achat de tout ce matériel et de maintenir l’ensemble du système de fichiers d’un point de vue opérationnel. Avec EFS, vous pouvez garder des systèmes de fichiers existants en place mais laissez AWS faire le gros du travail c’est-à-dire la mise à échelle et la réplication. EFS vous permet d’avoir plusieurs instances qui accèdent aux données dans EFS en même temps. Il effectue une mise à échelle horizontale et verticale au besoin sans rien avoir à faire pour que cette mise à échelle se produise. Super sympa, non ? Vous pourriez penser : Amazon EBS me permet aussi de stocker des fichiers auxquels je peux accéder à partir d’instances EC2. Quelle est exactement la différence ici ? 

 
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> AWS Cloud Practitioners, contents de vous revoir ! 

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> D'accord, nous n'avons pas besoin de faire tout ça. La réponse est vraiment simple. Les volumes Amazon EBS s’attachent aux instances EC2 et sont une ressource de niveau zone de disponibilité. Afin d'attacher EC2 à EBS, vous devez être dans la même AZ. Vous pouvez y enregistrer des fichiers. Vous pouvez également y exécuter une base de données, ou y stocker des applications. C'est un disque dur. Si vous mettez en service un volume EBS de deux téraoctets et le remplissez, il n’est pas automatiquement mis à l’échelle pour vous donner plus d’espace de stockage. Voilà pour EBS. Amazon EFS peut avoir plusieurs instances de lecture et d’écriture en même temps. 
> 
> Mais ce n'est pas seulement un disque dur vierge sur lequel vous pouvez écrire, c'est un vrai système de fichiers pour Linux. C'est aussi une ressource régionale. Ce qui signifie que toute instance EC2 dans la région peut écrire dans le système de fichiers EFS. Lorsque vous écrivez plus de données dans EFS, il se met automatiquement à l’échelle. Pas besoin de mettre en service plus de volumes.
</div>


## Stockage de fichiers

Dans le **stockage de fichiers**, plusieurs clients (comme des utilisateurs, des applications, des serveurs, etc.) peuvent accéder aux données stockées dans des dossiers de fichiers partagés. Dans cette approche, un serveur de stockage utilise le stockage par bloc avec un système de fichiers local pour organiser les fichiers. Les clients accèdent aux données par le biais de chemins de fichiers.

Comparé au stockage par bloc et au stockage d’objets, le stockage de fichiers est idéal lorsqu’un grand nombre de services et de ressources doivent accéder simultanément aux mêmes données.

**[Amazon Elastic File System (Amazon EFS)](https://aws.amazon.com/efs/)** est un système de fichiers évolutif utilisé avec les AWS Cloud services et les ressources sur site. Amazon EFS augmente et diminue automatiquement au fil de vos ajouts et suppressions de fichiers. Ce service peut évoluer à la demande jusqu’à des pétaoctets sans perturber les applications.

![aws_efs_01.png](/assets/img/clouds/tempo/aws_efs_01.png)

## Comparaison entre Amazon EBS et Amazon EFS

Sélectionnez chaque carte-mémoire pour la retourner.

**Amazon EBS**
Un volume Amazon EBS stocke les données dans une seule zone de disponibilité. 
Pour attacher une instance Amazon EC2 à un volume EBS, l’instance Amazon EC2 et le volume EBS doivent être dans la même zone de disponibilité.

![aws_ebs_01.png](/assets/img/clouds/tempo/aws_ebs_01.png)

**Amazon EFS**
Amazon EFS est un service régional. Il stocke les données dans et entre plusieurs zones de disponibilité. 
Le stockage en double vous permet d’accéder aux données simultanément à partir de l’ensemble des zones de disponibilité de la région où se trouve un système de fichiers. En outre, les serveurs sur site peuvent accéder à Amazon EFS à l’aide d’AWS Direct Connect.

# Amazon Relational Database Service (Amazon RDS)

```
video : xxx.mkv
```
*Figure. Amazon Relational Database Service*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Donc, vous stockez des données à propos de votre café dans divers systèmes. Mais vous réalisez que vous devez maintenir des relations entre plusieurs types de données. Et par relations j’entends dans le cas où un client commanderait la même boisson plusieurs fois, peut-être que vous voulez leur offrir une remise promotionnelle sur leur prochain achat. Et vous avez besoin d'un moyen de garder une trace de cette relation quelque part. Dans ce cas, la meilleure idée est d’utiliser un système de gestion de base de données relationnelles (SGBDR). Cela signifie essentiellement que nous stockons les données de manière à ce qu’elles se rapportent à d’autres données. 
> 
> Par exemple, si nous avions une saisie ou un enregistrement du client, nous les stockons dans une table client. Nous pourrions alors avoir une saisie pour l'adresse physique, que nous stockons sur une table d’adresses correspondante. Nous relions ensuite les deux via un attribut commun et pouvons interroger les données hébergées dans les deux tables. 
> 
> La façon la plus courante d'interroger les données est d’écrire des requêtes SQL. Et cela fonctionne sur une variété de systèmes de bases de données. En parlant de systèmes de bases de données, quels sont certains des bien plus connus qu'AWS prend en charge ? Il y a MySQL, PostgreSQL, Oracle, Microsoft SQL Server, et bien d’autres. Si vous avez un environnement sur site, vous exécutez probablement l'un d'entre eux et ils sont très probablement hébergés dans votre centre de données. 
> 
> Mais y a-t-il un moyen de facilement les déplacer vers le cloud ? Et bien la réponse est oui, vous pouvez effectuer ce qu’on appelle un portage, et migrer votre base de données pour s’exécuter sur Amazon EC2. Cela signifie que vous avez le contrôle sur les mêmes variables que dans votre environnement sur site comme système d’exploitation, mémoire, processeur, capacité de stockage, etc. C'est une entrée rapide dans le cloud et vous pouvez les migrer à l’aide de pratiques standard ou en utilisant quelque chose comme un service de migration de base de données, que nous verrons dans une vidéo ultérieure. 
> 
> L’autre option pour exécuter vos bases de données dans le cloud est d’utiliser un service plus géré appelé Amazon Relational Database Service (RDS). Il prend en charge tous les principaux moteurs de base de données, comme ceux que nous avons mentionné plus tôt, mais ce service présente d’autres avantages. Ils comprennent l’application de correctifs automatique, de sauvegarde, de redondance, de basculement, de reprise après sinistre, tout ce que vous devriez normalement gérer vous-même. Cela en fait une option très attrayante pour les clients AWS, car il vous permet de vous concentrer sur les problèmes de votre entreprise pas sur la mise à jour de bases de données. Et si vous êtes un administrateur de bases de données, ces tâches peuvent être assez chronophages et difficiles. 
> 
> Donc comment vous rendre les choses encore plus faciles pour exécuter les charges de travail de bases de données sur le cloud ? On passe à l'étape suivante et on les migre ou déploie vers Amazon Aurora. C'est notre option de base de données relationnelle gérée. Elle se présente sous deux formes : MySQL et PostgreSQL, Et elle coûte 1/10e du coût des bases de données commerciales. C'est une base de données très rentable. Parmi les autres avantages, vos données sont répliquées entre les installations, et vous avez des copies à tout moment. Vous pouvez déployer jusqu’à 15 réplicas en lecture, et vous pouvez donc décharger vos lectures et mettre à échelle les performances. De plus, il y a des sauvegardes continues vers S3, donc vous avez toujours une sauvegarde prête à être restaurée. Vous obtenez également un point de récupération dans le temps, pour que vous puissiez récupérer des données à partir d’une période spécifique. 
> 
> Et voilà, les bases de données relationnelles en quelques mots.
</div>

## Bases de données relationnelles

Dans une **base de données relationnelle**, les données sont stockées d’une manière qui les relie à d’autres éléments de données. 

Le système de gestion des stocks du café est un exemple de base de données relationnelle. Chaque registre de la base de données comprendrait des données pour un élément unique, comme le nom du produit, la taille, le prix, etc.

Les bases de données relationnelles utilisent le **langage de requête structurée (SQL)** pour stocker et interroger les données. Cette approche permet de stocker les données d’une manière facilement compréhensible, cohérente et évolutive. Par exemple, les propriétaires du cafés peuvent écrire une requête SQL pour identifier tous les clients qui achètent le plus souvent un latte moyen.

Exemple de données dans une base de données relationnelle :

|ID|Nom du produit|Taille|Prix|
|:--|:--|:--|:--|
|1|Café moulu torréfié moyen|350 ml|5,30 USD|
|2|Café moulu torréfié foncé|600 ml|9,27 USD|

## Amazon Relational Database Service

**[Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/)** est un service qui vous permet d’exécuter des bases de données relationnelles dans AWS Cloud.

#### tabs {.tabset}
##### Amazon RDS

![aws_rds_01.png](/assets/img/clouds/tempo/aws_rds_01.png)

Le diagramme montre les principales caractéristiques et les avantages d’Amazon Relational Database Service (Amazon RDS). Trois sections s'affichent de gauche à droite.

La première section présente une illustration d’un ordinateur portable. La première section, intitulée « Connexion », indique « Connectez votre application à l’un des sept moteurs RDS d’Amazon ». Une flèche pointe vers la deuxième section.

La deuxième section présente l’icône architecturale d’Amazon RDS avec le titre « Base de données relationnelle d’Amazon » et le texte « Configurez, exploitez et développez une base de données relationnelle dans le cloud en quelques clics ». Dans la case de droite, sous l’en-tête « Fonctions gérées par Amazon RDS », on trouve six petites cases, chacune mettant en évidence une fonction clé. Ces fonctions sont les suivantes : « Sécurité et conformité », « Performance et évolutivité », « Correctifs et mises à niveau automatisés », « Durabilité et redondance des données », « Surveillance et alerte » et « Sauvegarde et récupération ». Une flèche pointe vers la troisième section.

La troisième section n’a pas de titre, mais affiche cinq petites cases, chacune mettant en évidence un avantage de l’utilisation d’Amazon RDS. De haut en bas, les avantages énumérés sont les suivants : « Accent sur l’innovation », « Migrer sans réarchitecturer les applications », « Moins de temps pour gérer les bases de données », « Améliorer l’efficacité des bases de données et des infrastructures », et « Réduire les dépenses en immobilisations et opérationnelles ».

##### Amazon RDS Custom

![aws_rds_02.png](/assets/img/clouds/tempo/aws_rds_02.png)

Le diagramme montre le processus d’utilisation client d’Amazon Relational Database Service (Amazon RDS). Deux sections s’affichent de gauche à droite.

La première section présente une illustration d’un ordinateur portable. La première section dit : « Connectez-vous au point de terminaison Amazon RDS ».

La deuxième section comporte l’icône architecturale d’Amazon RDS avec le titre « Amazon RDS Custom » et le texte « Un service de base de données géré pour les applications qui nécessitent un système d’exploitation et une personnalisation de base de données. Disponible pour RDS Custom pour Oracle et RDS Custom pour SQL Server. » Les trois étapes de RDS Custom sont indiquées dans l’encadré de droite. La première étape montre l’illustration de trois cubes et indique « Toutes les ressources AWS déployées dans votre compte AWS ». La deuxième étape porte le titre « Périmètre de soutien » et le texte explicatif « Surveiller et signaler les configurations personnalisées non valides ». La deuxième étape se trouve dans une boîte avec quatre petites cases à gauche avec des icônes de service et du texte pour « Amazon EC2 », « Amazon S3 », « AWS Systems Manager » et « Amazon EBS », de haut en bas. La troisième étape montre un ordinateur portable avec le texte « Installer et exécuter des applications existantes, personnalisées et intégrées ».


##### Amazon RDS on AWS Outposts

![aws_rds_03.png](/assets/img/clouds/tempo/aws_rds_03.png)

Le diagramme montre comment les données sont déplacées en utilisant Amazon Relational Database Service (Amazon RDS) sur AWS Outposts. Trois sections s'affichent de gauche à droite.

La première section présente une illustration d’un ordinateur portable. La première section est intitulée « Choisissez votre moteur » et le texte dit « Choisissez dans le RDS d’Amazon pour MySQL, PostgreSQL ou SQL Server ».

La deuxième section représente une région AWS en l’encadrant dans une boîte avec le texte « Région AWS » dans le coin supérieur gauche. La deuxième section présente deux icônes de service avec du texte pour « Amazon RDS » et Amazon KMS dans la boîte.

Au-dessus d’une flèche bilatérale, indiquant le mouvement des données dans les deux sens entre les sections deux et trois, se trouve une icône de cadenas avec le texte « 1. AWS déploie Amazon RDS vers votre centre de données au moyen d’une connexion sécurisée. »

La troisième section représente le Centre de données du client en l’encadrant dans une boîte avec le texte « Centre de données du client » dans le coin supérieur gauche. À l’intérieur de la boîte, la troisième section comprend deux prochaines étapes avec le texte » 2. Gérez votre base de données Amazon RDS dans AWS Outposts en utilisant la même console AWS, CLI et API. Les bases de données sont chiffrées à l’aide de l’AWS KMS” et “3. Les sauvegardes et les instantanés sont automatiquement stockés localement ou dans la région AWS » à l’aide des options de stockage AWS, telles que « Amazon S3 » et « Amazon EBS », énumérées ci-dessous à l’étape 3.

### 

Amazon RDS est un service géré qui automatise les tâches telles que l’approvisionnement de matériel, la configuration des bases de données, l’application de correctifs et les sauvegardes. Grâce à ces fonctionnalités, vous consacrerez moins de temps aux tâches administratives et davantage de temps à utiliser les données pour innover vos applications. Vous pouvez intégrer Amazon RDS à d’autres services pour répondre à vos besoins commerciaux et opérationnels, tels que l’utilisation d’AWS Lambda pour interroger votre base de données à partir d’une application sans serveur.

Amazon RDS propose plusieurs options de sécurité différentes. De nombreux moteurs de bases de données Amazon RDS offrent le chiffrement au repos (protection des données lorsqu’elles sont stockées) et le chiffrement en transit (protection des données lorsqu’elles sont envoyées et reçues).

## Moteurs de base de données Amazon RDS

Amazon RDS est disponible pour six moteurs de bases de données, qui optimisent la mémoire, les performances ou les entrées/sorties (I/O). Les moteurs de bases de données pris en charge comprennent :

- Amazon Aurora
- PostgreSQL
- MySQL
- MariaDB
- Oracle Database
- Microsoft SQL Server

## Amazon Aurora

**[Amazon Aurora](https://aws.amazon.com/rds/aurora/)** est une base de données relationnelle d’entreprise. Ce service est compatible avec les bases de données relationnelles MySQL et PostgreSQL. Il est jusqu’à cinq fois plus rapide que les bases de données MySQL standard et jusqu’à trois fois plus rapide que les bases de données PostgreSQL standard.

![aws_aurora_01.png](/assets/img/clouds/tempo/aws_aurora_01.png)

Amazon Aurora contribue à réduire les coûts de votre base de données en diminuant les opérations d’entrée/de sortie (I/O) inutiles, tout en garantissant la fiabilité et la disponibilité de vos ressources de bases de données. 

Envisagez Amazon Aurora si vos charges de travail nécessitent une disponibilité élevée. Ce service réplique six copies de vos données sur trois zones de disponibilité et sauvegarde en permanence vos données sur Amazon S3.

# Amazon DynamoDB

```
video : xxx.mkv
```
*Figure. Amazon DynamoDB*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Parlons d'Amazon DynamoDB. Au niveau le plus élémentaire, DynamoDB est une base de données. Il s'agit d'une base de données sans serveur ce qui signifie que vous n'avez pas besoin de gérer les instances sous-jacentes ni l'infrastructure qui l'alimente. 
> 
> Avec DynamoDB, vous créez des tables. Une table DynamoDB est tout simplement un endroit où vous pouvez stocker et interroger vos données. Les données sont organisées en éléments, et les éléments ont des attributs. Les attributs sont tout simplement des fonctionnalités différentes de vos données. Si vous avez un élément dans votre table, ou 2 millions d’éléments dans votre table, DynamoDB gère le stockage sous-jacent pour vous. Et vous n'avez pas besoin de vous soucier de la mise à l'échelle du système, verticale ou horizontale. 
> 
> DynamoDB stocke ces données de manière redondante dans les zones de disponibilité et les recopie pour vous sur plusieurs lecteurs. Cela réduit énormément la charge d’exploitation d’une base de données hautement disponible. 
> 
> DynamoDB, en plus d'être massivement évolutif, est également très performant. Le temps de réponse de DynamoDB se compte en millisecondes. Et lorsque vous avez des applications avec potentiellement des millions d’utilisateurs, la scalabilité et des temps de réponse fiables et rapides es important. 
> 
> Mais DynamoDB n'est pas une base de données normale. Comme il n’utilise pas le langage de requête très largement utilisé, sequel, ou SQL. Les bases de données relationnelles comme une base de données MySQL standard, exige que vous ayez mis en place un schéma bien défini. Cela pourrait consister en une ou plusieurs tables qui peuvent se rapporter les unes aux autres. Vous utilisez ensuite SQL pour interroger les données. 
> 
> Cela convient à de nombreux cas d’utilisation et a longtemps constitué le type de base de données standard. Toutefois, ces types de bases de données SQL rigides peuvent présenter des problèmes de performances et de mise à l’échelle lorsqu’elles sont sous contrainte. Le schéma rigide le fait également pour que vous ne puissiez pas avoir de variation des types de données que vous stockez dans une table. Donc il se peut qu'il ne soit pas le mieux adapté à un ensemble de données mais il est un peu moins rigide et accessible à un taux très élevé. 
> 
> C’est ici que les bases de données non relationnelles ou NoSQL entrent en jeu. DynamoDB est une base de données non relationnelle. Les bases de données non relationnelles ont tendance à avoir des schémas flexibles simples, pas des schémas rigides complexes, avec des tables multiples qui se rapportent toutes les unes aux autres. 
> 
> Avec DynamoDB, vous pouvez ajouter ou retirer des attributs aux éléments de la table, à tout moment. Tous les éléments de la table n'ont pas à avoir les mêmes attributs. C'est idéal pour les ensembles de données ayant une certaine variation d’un article à l’autre. En raison de cette flexibilité, vous ne pouvez pas y exécuter de requêtes SQL complexes. Au lieu de cela, vous écririez des requêtes basées sur un petit sous-ensemble d’attributs conçus comme des clés. 
> 
> C’est pourquoi les requêtes que vous exécutez sur des bases de données non relationnelles ont tendance à être plus simples et se concentrent sur un ensemble d’éléments provenant d’une table et non sur des requêtes couvrant plusieurs tables. Ce modèle de requête, ainsi que d'autres facteurs, notamment la façon dont le système sous-jacent est conçu, permet à DynamoDB d'être très rapide en termes de temps de réponse et d'être hautement évolutif. 
> 
> Voici les éléments à retenir : DynamoDB est une base de données non relationnelle NoSQL, conçue sur mesure. Cela signifie qu’il a des cas d’utilisation spécifiques et qu’il n’est pas le mieux adapté à toutes les charges de travail. Son temps de réponse se compte en millisecondes. Il est entièrement géré, et hautement évolutif. Voici un parfait exemple : le jour du Prime Day en 2019, sur les 48 heures du Prime Day, 7,11 milliards d’appels ont eu lieu avec DynamoDB API, avec un pic de 45,4 millions de requêtes par seconde. C’est incroyablement évolutif le tout sans la gestion des bases de données sous-jacentes. C’est plutôt pas mal.
</div>
  
![aws_dynamodb_01.png](/assets/img/clouds/tempo/aws_dynamodb_01.png)

Le diagramme présente les principales fonctionnalités d'Amazon DynamoDB et les intégrations avec d'autres services AWS. Les trois sections s'affichent de gauche à droite.

La première section présente une illustration de l'icône du service DynamoDB dans une boîte intitulée « Configurer les fonctionnalités clés ». Six cases sont affichées dans deux colonnes avec trois rangées chacune, les caractéristiques suivantes étant représentées par case : dans la première colonne, « NoSQL Workbench » dans la case supérieure gauche, « Cryptage au repos» dans la case centrale gauche, et « Mode de capacité à la demande » dans la case inférieure gauche. Dans la deuxième colonne, « Tables globales » boîte supérieure droite, « Récupération ponctuelle » boîte centrale droite et « Supports PartiQL » boîte inférieure droite. Sous les six cases contenant chacune une fonctionnalité, et sous le titre « Configurer les fonctionnalités clés » se trouve une phrase énumérant les fonctionnalités supplémentaires de DynamoDB.

La deuxième section présente une illustration d'une loupe qui examine des blocs de données. La flèche de gauche signifie que les données proviennent de DynamoDB et la flèche de droite signifie que les données vont dans les services AWS avec lesquels DynamoDB s'intègre. Sous la loupe se trouve le titre « Export, Analyze, Stream Data », accompagné d'une phrase qui explique les types de fonctions qui peuvent être exécutées lors de l'intégration avec d'autres services AWS.

La troisième section est une boîte contenant cinq boîtes plus petites représentant les cinq services avec lesquels DynamoDB s'intègre. La case supérieure contient « Amazon S3 », la deuxième case contient « AWS Glue Elastic Views », la troisième case contient « Amazon Kinesis Data Streams », la quatrième case contient « AWS CloudTrail » et la cinquième et dernière case contient « Amazon Cloudwatch ».

## Bases de données non relationnelles

Dans une **base de données non relationnelle**, vous créez des tables. Une table est un endroit où vous pouvez stocker et interroger des données.

Les bases de données non relationnelles sont parfois appelées « bases de données NoSQL », car elles utilisent des structures autres que des lignes et des colonnes pour organiser les données. Les paires clé-valeur sont un type d’approche structurelle pour les bases de données non relationnelles. Avec les paires clé-valeur, les données sont organisées en éléments (clés) et les éléments possèdent des attributs (valeurs). Vous pouvez considérer les attributs comme des fonctions différentes de vos données.

Dans une base de données clé-valeur, vous pouvez ajouter ou supprimer des attributs des éléments de la table à tout moment. En outre, tous les éléments de la table ne doivent pas avoir les mêmes attributs. 

Exemple de données dans une base de données non relationnelle :

|Clé|Valeur|
|:--|:--|
|1|Nom : John Doe|
| |Adresse : 123 Any Street|
| |Boisson préférée : latte moyen|
|2|Nom : Mary Major|
| |Adresse : 100 Main Street|
| |Anniversaire : 5 juillet 1994|

## Amazon DynamoDB

**[Amazon DynamoDB](https://aws.amazon.com/dynamodb/)** est un service de base de données clé-valeur. Il offre des performances inférieures à 10 millisecondes à n’importe quelle échelle.

Pour en savoir plus sur les fonctions de DynamoDB, sélectionnez chaque carte-mémoire pour la retourner.

**Sans serveur**
DynamoDB est sans serveur, ce qui signifie que vous ne devez pas mettre en service, appliquer des correctifs ou gérer des serveurs. 
Vous n’avez pas non plus besoin d’installer, de maintenir ou d’exploiter un logiciel.

**Mise à l'échelle automatique**
Au fur et à mesure que la taille de votre base de données augmente ou diminue, DynamoDB se met automatiquement à l’échelle pour s’adapter aux changements de capacité, tout en assurant des performances constantes. 
Ce service convient parfaitement aux cas d’utilisation nécessitant des performances élevées lors de la mise à l’échelle.

```
video : xxx.mkv
```
*Figure. Comparing Amazon RDS and Amazon DynamoDB*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> AWS Cloud Practitioners, content de vous revoir au championnat des bases de données ! Dans la catégorie relationnelle, conçu pour supprimer les charges lourdes indifférenciées des administrateurs de votre base de données avec une disponibilité élevée automatique et la reprise fournie. Vous contrôlez les données, vous contrôlez le schéma, vous contrôlez le réseau. Vous exécutez Amazon RDS. Oui, oui. 
> 
> Dans la catégorie NoSQL, utilisant une paire clé-valeur qui ne requiert aucun schéma avancé, capable de fonctionner comme une base de données mondiale juste en appuyant sur un bouton. Il a un débit énorme. Il a un potentiel de mise à l'échelle en pétaoctet. Il a un accès API détaillé. C'est Amazon DynamoDB. 
> 
> Face à face. Chaque catégorie de base de données est conçue pour améliorer exactement les environnements existentiels passionnant que vous envisagez. Quelle base de données l'emportera dans cette nouvelle lutte mondiale herculéenne ? Une fois encore, le gagnant dépendra de votre cas d'utilisation. 
> 
> Premier round. Les bases de données relationnelles existent depuis le moment où les entreprises ont commencé à utiliser des ordinateurs. Être capable de créer des analyses de données réparties sur plusieurs tables est la force de tout système relationnel. Dans ce round, vous avez un système de gestion de chaîne d’approvisionnement de vente que vous devez analyser pour en trouver les points faibles. RDS l’emporte ici car il est conçu pour l’analytique d’entreprise car vous avez besoin de jointures relationnelles complexes. RDS emporte facilement le premier round. 
> 
> Deuxième round. Le cas d'utilisation et à peu près tout le reste… Ça a l’air bizarre, mais malgré ce que votre fournisseur de base de données autonome hérité vous a fait croire, la raison pour laquelle la plupart des gens utilisent des bases de données relationnelles coûteuses, n’a rien à voir avec les relations complexes. En fait, les gens finissent en grande partie par mettre les tables de recherche dans ces bases de données. 
> 
> Pour ce round, imaginez avoir une liste de contact des employés avec les noms, numéros de téléphone, e-mails, identifiants d'employés. Eh bien, tout ça se rapporte à une table unique. Je pourrais utiliser une base de données relationnelle pour cela, mais tout ce qui fait que les bases de données relationnelles soient fantastiques, toutes ces fonctionnalités complexes, créé des frais généraux et du retard et des dépenses si vous ne l’utilisez pas réellement. C'est là que les bases de données non relationnelles, DynamoDB assènent le coup de grâce. En éliminant tous les frais généraux, DynamoDB vous permet de construire des bases de données, puissantes et incroyablement rapides où vous n’avez pas besoin de fonctionnalité de jointure complexe. DynamoDB est donc le champion incontesté. 
> 
> Encore une fois, le vainqueur dépend de votre charge de travail individuelle. Chaque service est le bon pour des besoins spécifiques. Une fois que vous avez compris ce dont vous avez besoin, vous saurez à nouveau quel est le service qui est votre champion !
</div>
  
## Contrôle des connaissances

**Quelles sont les situations dans lesquelles vous devez utiliser Amazon Relational Database Service (Amazon RDS) ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q2r1" name="question2" value="1">
  <label for="q2r1">Exécuter une base de données sans serveur</label><br>
  <input type="checkbox" id="q2r2" name="question2" value="2">
  <label for="q2r2">Utiliser SQL pour organiser les données</label><br>
  <input type="checkbox" id="q2r3" name="question2" value="3">
  <label for="q2r3">Stocker des données dans une base de données clé-valeur</label><br>
  <input type="checkbox" id="q2r4" name="question2" value="4">
  <label for="q2r4">Mettre à l’échelle jusqu’à 10 milliards de demandes par jour</label><br>
  <input type="checkbox" id="q2r5" name="question2" value="5">
  <label for="q2r5">Stocker les données dans une base de données Amazon Aurora</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- Utiliser SQL pour organiser les données
- Stocker les données dans une base de données Amazon Aurora

Les trois autres réponses sont des scénarios pour lesquels vous devez utiliser Amazon DynamoDB.
</div>
  
  
# Amazon Redshift

```
video : xxx.mkv
```
*Figure. Amazon Redshift*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Nous venons de passer beaucoup de temps à discuter des types de flux de travail qui nécessitent des données rapides, fiables et actuelles. Les bases de données pouvant gérer des milliers de transactions par seconde, un stockage qui est hautement disponible et massivement durable. Mais parfois nous avons un besoin d’entreprise qui va au-delà de ce qui se passe maintenant par rapport à ce qui s’est passé. Cette analyse de données est le domaine d’une classe de données totalement différente. Vous pouvez bien sûr utiliser la taille unique pour tous les modèles d’une base de données unique pour tout, mais les bases de données modernes conçues pour une grande vitesse, une ingestion en temps réel et des requêtes peuvent ne pas être les mieux adaptées. 
> 
> Laissez-moi vous expliquer. Afin de gérer la vitesse de la fonctionnalité de lecture/écriture en temps réel, la plupart des bases de données relationnelles ont tendance à très bien fonctionner à certaines capacités. La quantité de contenu réellement stockée. Le problème de l’analyse historique, les données qui répondent à des questions comme, « montrez-moi comment la production s’est améliorée depuis que nous avons commencé », c’est que la collecte de données ne s’arrête jamais. En fait, avec la télémétrie moderne et l’explosion de l’IoT, le volume de données va submerger même la base de données relationnelle la plus résistante. 
> 
> Il y a pire. En plus du volume, la variété des données peut aussi poser problème. Vous voulez diriger des projets de veille stratégique sur des données qui proviennent de différents magasins de données comme votre inventaire, vos finances, et vos systèmes de vente au détail ? Une seule requête contre plusieurs bases de données, ça sonne bien, mais les bases de données classiques ont du mal à le gérer. 
> 
> Lorsque les données deviennent trop complexes à gérer avec des bases de données relationnelles classiques, vous pénétrez dans le monde des entrepôts de données. Les entrepôts de données sont spécialement conçus pour ce type de big data qui concernent une analyse historique par opposition à une analyse opérationnelle. 
> 
> Soyons clairs. Historique peut s'appliquer à partir du moment où l'on regarde les ventes de la dernière heure dans tous les magasins. L'essentiel est que les données sont maintenant définies. Nous ne vendons plus depuis la dernière heure parce que c'est maintenant dans le passé. Comparez cette question à « combien de sacs de café avons-nous en stock à cet instant ?» Ce qui pourrait changer au fur et à mesure que nous parlons. Tant que votre question d'entreprise regarde en arrière, un entrepôt de données est la bonne solution pour ce secteur de veille stratégique. 
> 
> Il y a beaucoup de solutions d'entrepôt de données sur le marché. Si vous avez déjà une favorite en cours d’exécution sur AWS c’est juste une question d’obtenir les données transférées. Mais au-delà de cela, il peut encore y avoir beaucoup de charges lourdes indifférenciées impliquées dans le maintien d’un entrepôt de données pour qu’il soit, ajusté, résilient et continuellement mis à l’échelle. Ne serait-ce pas bien si votre équipe de l’entrepôt de données pouvait se concentrer sur les données au lieu de devoir inévitablement alimenter et prendre soin du moteur ? 
> 
> Je vous présente Amazon Redshift. Il s'agit de l'entreposage de données en tant que service. C'est extrêmement évolutif. Les nœuds Redshift dans plusieurs tailles de pétaoctets sont très communs. En fait, en coopération avec Amazon Redshift Spectrum, vous pouvez directement exécuter une seule requête SQL sur des exaoctets de données non structurées s’exécutant dans les lacs de données. 
> 
> Mais ça va plus loin que d'être juste capable de gérer des ensembles de données beaucoup plus volumineux. Redshift utilise une variété d’innovations qui vous permettent d’atteindre jusqu’à 10 fois plus de performances que les bases de données classiques quand il s’agit de ces types de charge de travail de veille stratégique. 
> 
> Je ne vais pas entrer dans les détails pour vous expliquer son fonctionnement. Nous avons des cours complets que vos équipes de données et vous pouvez suivre pour savoir comment il est construit, et pourquoi cela peut entraîner de tels résultats améliorés. L’important pour vous est de comprendre que lorsque vous avez besoin de solutions de big data de veille stratégique, Redshift vous permet de démarrer avec un seul appel d’API. Moins de temps d'attente pour les résultats, plus de temps pour obtenir des réponses.
</div>

## Amazon Redshift

**[Amazon Redshift](https://aws.amazon.com/redshift)** est un service de stockage de données que vous pouvez utiliser pour l’analytique des données Big Data. Il permet de collecter des données provenant de nombreuses sources et vous aide à comprendre les relations et les tendances entre vos données.

![aws_redshift_01.png](/assets/img/clouds/tempo/aws_redshift_01.png)

# AWS Database Migration Service

```
video : xxx.mkv
```
*Figure. AWS Database Migration Service*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Nous avons parlé des bases de données et des différentes options de base de données sur AWS. Mais que se passe-t-il si vous avez une base de données qui est déjà sur site ou déjà dans le cloud ? 
> 
> Heureusement, AWS propose un service appelé Amazon Database Magic. Je veux dire Amazon Database Migration Service (DMS), pour aider les clients à le faire. DMS aide les clients à migrer les bases de données existantes sur AWS de manière sécurisée et facile. Vous migrez essentiellement des données entre une base de données source et cible. Le plus intéressant est que la base de données source reste complètement opérationnelle pendant la migration, ce qui réduit au minimum les temps d'arrêt des applications reposant sur la base de données. Encore mieux, les bases de données source et cible n'ont pas à être du même type. 
> 
> Mais commençons par les bases de données qui sont du même type. Ces migrations sont appelées homogènes et peuvent se faire de MySQL vers Amazon RDS pour MySQL, Microsoft SQL Server vers Amazon RDS pour SQL Server ou même Oracle vers Amazon RDS pour Oracle. Ce processus est assez simple comme les structures de schéma, les types de données, et les codes de base de données sont compatibles entre la source et la cible. 
> 
> Comme indiqué, la base de données source peut être située sur le site, exécutée sur des instances Amazon EC2 ou encore une base de données Amazon RDS. La cible elle-même peut être une base de données dans Amazon EC2 ou Amazon RDS. Dans ce cas, vous créez une tâche de migration avec des connexions aux bases de données source et cible. Ensuite, démarrez la migration avec un clic sur le bouton. AWS Database Migration Service s'occupe du reste. 
> 
> Le deuxième type de migration se produit lorsque les bases de données source et cible sont de différents types. C'est ce qu'on appelle des migrations hétérogènes, et c’est un processus en deux étapes. Comme les structures de schéma, les types de données et les code de base de données sont différents entre la source et la cible, nous devons d’abord les convertir à l’aide de l’outil AWS Schema Conversion Tool. Cela convertira le schéma source et le code pour correspondre à ceux de la base de données cible. L’étape suivante consiste à utiliser DMS pour migrer les données à partir de la base de données source vers la cible. 
> 
> Mais ce ne sont pas les seuls cas d'utilisation pour DMS. D’autres incluent le développement et testent les migrations de bases de données, la consolidation des bases de données, et même la réplication continue de bases de données. Le développement et les tests de migration permettent de le développer pour effectuer des tests par rapport aux données de production, mais sans affecter les utilisateurs de la production. Dans ce cas, vous utilisez DMS pour migrer une copie de votre base de données de production vers vos environnements de développement ou de test, soit une fois, soit en permanence. 
> 
> Lorsque vous avez plusieurs bases de données, la consolidation consiste à les regrouper en une base de données centrale. 
> 
> Enfin, la réplication continue consiste à utiliser DMS pour réaliser une réplication continue des données. Cela pourrait être pour la reprise après sinistre ou en raison d’une séparation géographique. 
> 
> Si vous souhaitez en savoir plus, veuillez consulter notre section de ressources. Et voilà les amis, DMS en un mot.
</div>


## AWS Database Migration Service (AWS DMS)

**[AWS Database Migration Service (AWS DMS)](https://aws.amazon.com/dms/)** vous permet de migrer des bases de données relationnelles, des bases de données non relationnelles ainsi que d’autres types de stockages de données.

#### tabs {.tabset}
##### Hétérogène

![aws_dms_heterogene_01.png](/assets/img/clouds/tempo/aws_dms_heterogene_01.png)

##### Homogène

![aws_dms_homogene_01.png](/assets/img/clouds/tempo/aws_dms_homogene_01.png)

### 

Avec AWS DMS, vous déplacez les données entre une base de données source et une base de données cible. [Les bases de données sources et cibles](https://aws.amazon.com/dms/resources) peuvent être du même type ou de types différents. Votre base de données source reste opérationnelle pendant la migration, ce qui réduit au minimum les temps d’arrêt des applications reposant sur la base de données. 

Par exemple, supposons que vous disposiez d’une base de données MySQL stockée sur site dans une instance Amazon EC2 ou dans Amazon RDS. Considérez la base de données MySQL comme votre base de données source. Grâce à AWS DMS, vous pouvez migrer vos données vers une base de données cible, comme une base de données Amazon Aurora.

## Autres cas d’utilisation d’AWS DMS

Sélectionnez chaque carte pour la retourner.

**Développement et test des migrations de bases de données**
Permettre aux développeurs de tester les applications par rapport aux données de production sans affecter les utilisateurs de la production

**Consolidation de bases de données**
Combinaison de plusieurs bases de données en une base de données

**Réplication en continu**
Envoi de copies continues de vos données vers d’autres sources cibles au lieu d’effectuer une migration ponctuelle

# Services de bases de données supplémentaires

```
video : xxx.mkv
```
*Figure. Additional database services and database accelerators*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Avant de conclure sur les bases de données et le stockage, Je veux revenir sur le sujet avec lequel nous avons commencé tout cela. Choisir la bonne base de données, plutôt que de forcer vos données à répondre aux exigences de votre base de données. Peu importe ce qu'un fournisseur de base de données vous dit, il n’existe pas de base de données universelle qui réponde à tous les besoins. Nous avons déjà abordé un certain nombre de variétés de bases de données, mais il y a d’autres bases de données qu’AWS offre pour répondre aux exigences commerciales spéciales, que nous n’avions pas le temps d’aborder. Mais il vaut mieux savoir qu'elles sont là si vous en avez besoin. 
> 
> Par exemple, nous avons parlé de DynamoDB, et c'est génial pour les bases de données de paires clé-valeur. Mais que faire si vous avez besoin de plus que de petits attributs ? Que faire si vous avez besoin d'un système de gestion de contenu complet ? Voici Amazon DocumentDB, fantastique pour la gestion des contenus, les catalognes, et les profils utilisateur. 
> 
> Que faire si vous ajoutez un réseau social que vous voulez suivre avec ce type de sites où il faut savoir qui est lié à qui ? C’est très compliqué à gérer dans une base de données relationnelle classique Amazon Neptune: une base de données orientée graphe, conçue pour les réseaux sociaux et les moteurs de recommandation, également parfait pour la détection des fraudes. 
> 
> Ou peut-être avez-vous une chaîne d'approvisionnement, que vous devez suivre pour vous assurer de ne rien perdre. Ou vous avez des dossiers bancaires ou financiers qui exigent une immuabilité à 100 % ou certaines personnes vous diront : « oh, c’est ça la chaîne de blocs ». Peut-être que maintenant, si vous avez besoin d’une solution de chaîne de blocs, vous seriez au courant, non ? Nous proposons Amazon Managed Blockchain. Mais ce n'est probablement pas ce dont vous avez vraiment besoin ici. Il résout une partie de l’équation, mais ajoute un énorme composant de décentralisation, ce n’est pas ce que les régulateurs financiers ont envie de voir. Ce dont vous avez vraiment besoin c’est d’un registre inaltérable alors Amazon QLDB, ou Quantum Ledger Database. Il s'agit d'un système de registre inaltérable dans lequel aucune entrée ne peut être supprimée des audits. 
> 
> Les bases de données seules sont super, mais s’il y a un moyen de les rendre plus rapides, ne serait-ce pas fantastique ? Mais vous savez que je ne dirais pas ça s’il n’y avait pas d’options d’accélérateur pouvant être utilisé dans un nombre de scénarios uniques. À commencer par ajouter les couches de mise en cache au-dessus de vos bases de données qui peuvent aider à améliorer les temps de lecture des requêtes courantes de millisecondes à microsecondes Amazon ElastiCache peut fournir ces couches de mise en cache sans que votre équipe n’ait à se soucier des charges lourdes liées au lancement, à l’amélioration et à la maintenance. Et il peut être utilisé avec les deux versions de Memcached et Redis. 
> 
> Ou si vous utilisez DynamoDB, essayez d’utiliser DAX, DynamoDB Accelerator, une couche de mise en cache native conçue pour améliorer considérablement les temps de lecture de vos données non relationnelles.
> 
> L'essentiel à comprendre est qu'AWS veut s'assurer que vous utilisez le meilleur outil pour le travail.
</div>

## Services de bases de données supplémentaires

Pour en savoir plus, sélectionnez le symbole + en regard de chaque catégorie.

### Amazon DocumentDB

**[Amazon DocumentDB](https://aws.amazon.com/documentdb)** est un service de base de données documentaire qui prend en charge les charges de travail MongoDB. (MongoDB est un programme de base de données documentaire.)

#### Tabs {.tabset}

##### Elastic Clusters

![aws_documentdb_elastic_01.png](/assets/img/clouds/tempo/aws_documentdb_elastic_01.png)

##### Migration des charges de travail MongoDB autogérées

![aws_documentdb_autogeree_01.png](/assets/img/clouds/tempo/aws_documentdb_autogeree_01.png)

Configurez, sécurisez et mettez à l'échelle des bases de données compatibles MongoDB sans vous soucier de l'exécution d'un logiciel de gestion de cluster, de la configuration des sauvegardes ou de la surveillance des charges de travail de production. [Pour savoir comment procéder, consultez le guide de migration Amazon DocumentDB »](https://docs.aws.amazon.com/documentdb/latest/developerguide/docdb-migration.html)

### Amazon Neptune

**[Amazon Neptune](https://aws.amazon.com/neptune)** est un service de base de données orientée graphe. 

![aws_neptune_01.png](/assets/img/clouds/tempo/aws_neptune_01.png)

Vous pouvez utiliser Amazon Neptune pour créer et exécuter des applications qui fonctionnent avec des ensembles de données hautement connectés, tels que les moteurs de recommandation, la détection des fraudes et les graphiques de connaissances.

### Amazon Quantum Ledger Database (Amazon QLDB)

**[Amazon Quantum Ledger Database (Amazon QLDB)](https://aws.amazon.com/qldb)** est un service de base de données de registre.

![aws_qldb_01.png](/assets/img/clouds/tempo/aws_qldb_01.png)

Vous pouvez utiliser Amazon QLDB pour consulter l’historique complet de toutes les modifications apportées aux données de votre application.


### Amazon Managed Blockchain
**[Amazon Managed Blockchain](https://aws.amazon.com/managed-blockchain)** est un service que vous pouvez utiliser pour créer et gérer des réseaux blockchain avec des frameworks open source.

![aws_managed_blockchain_01.png](/assets/img/clouds/tempo/aws_managed_blockchain_01.png)

Blockchain est un système de registre distribué qui permet à plusieurs parties d’exécuter des transactions et de partager des données sans autorité centrale.


### Amazon ElastiCache
**[Amazon ElastiCache](https://aws.amazon.com/elasticache)** est un service qui ajoute des couches de mise en cache au-dessus de vos bases de données afin d’améliorer les temps de lecture des requêtes courantes. 

![aws_elasticache_01.png](/assets/img/clouds/tempo/aws_elasticache_01.png)

Il prend en charge deux types de stockages de données : Redis et Memcached.


### Amazon DynamoDB Accelerator (DAX)
**[Amazon DynamoDB Accelerator (DAX)](https://aws.amazon.com/dynamodb/dax/)** est un cache en mémoire pour DynamoDB. 

![aws_dax_01.png](/assets/img/clouds/tempo/aws_dax_01.png)

Ce service fait passer les temps de réponse de quelques millisecondes à quelques microsecondes.

# Module 5 : résumé

Dans le module 5, vous avez découvert les concepts suivants :

- Stockage d’instances Amazon EC2 et Amazon EBS
- Amazon S3
- Amazon EFS
- Bases de données relationnelles et Amazon RDS
- Bases de données non relationnelles et DynamoDB
- Amazon Redshift
- AWS DMS
- Accélérateurs et services de bases de données supplémentaires

```
video : xxx.mkv
```
*Figure. Module summary - Storage and databases*

<div class="collapseContent" display="Transcription" value="true">


> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =50x){.align-left}
> Nous avons terminé un autre module. C'est super. Vous avez appris sur tous les différents types de mécanismes de stockage AWS. Récapitulons-les, d'accord ? 
> Nous avons tout d’abord parlé des volumes Elastic Block Store, et vous les attachez aux instances EC2 pour avoir un stockage local qui n’est pas éphémère. 
> Vous avez découvert S3 et la manière de stocker des objets dans AWS en un clic ou un appel d’API. 
> Nous avons même discuté des différentes options de base de données relationnelle disponibles dans AWS. Ou pour les charges de travail qui ont juste besoin d’une paire clé-valeur, nous avons l’offre non relationnelle appelée DynamoDB. 
> Ensuite, il y avait EFS pour les cas d'utilisation du stockage de fichiers. 
> Nous avons aussi Amazon Redshift pour tous nos besoins d'entrepôt de données. 
> Et pour faciliter la migration de bases de données existantes, nous avons DMS ou Database Migration Service. 
> Nous avons également abordé les services de stockage moins connus, comme DocumentDB, Neptune, QLDB, et Amazon Managed Blockchain. 
> Nous avons enfin parlé des solutions de mise en cache comme ElastiCache et DynamoDB Accelerator. 
> Cela fait beaucoup d'endroits pour stocker différents types de données et j’espère que vous avez découvert le bon endroit pour stocker chaque type.
</div>
  
  
## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été explorés dans le module 5, consultez ces ressources.

- [Stockage dans le cloud sur AWS](https://aws.amazon.com/products/storage)
- [Blog sur le stockage AWS](https://aws.amazon.com/blogs/storage/)
- [Tutoriels pratiques : stockage](https://aws.amazon.com/getting-started/hands-on/?awsf.getting-started-category=category%23storage&awsf.getting-started-content-type=content-type%23hands-on)
- [Témoignages de clients AWS : stockage](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23storage)
- [AWS Database Migration Service](https://aws.amazon.com/dms/)
- [Bases de données sur AWS](https://aws.amazon.com/products/databases)
- [Découverte approfondie d’une catégorie : bases de données](https://aws.amazon.com/getting-started/deep-dive-databases/)
- [Blog sur les bases de données AWS](https://aws.amazon.com/blogs/database/)
- [Témoignages de clients AWS : bases de données](https://aws.amazon.com/solutions/case-studies/?customer-references-cards.sort-by=item.additionalFields.publishedDate&customer-references-cards.sort-order=desc&awsf.customer-references-location=*all&awsf.customer-references-segment=*all&awsf.customer-references-product=product%23vpc%7Cproduct%23api-gateway%7Cproduct%23cloudfront%7Cproduct%23route53%7Cproduct%23directconnect%7Cproduct%23elb&awsf.customer-references-category=category%23databases)

# Module 5 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Quelles classes de stockage Amazon S3 sont optimisées pour les données d’archivage ? (Sélectionnez-en DEUX.)**

<form>
  <input type="checkbox" id="q3r1" name="question3" value="1">
  <label for="q3r1">S3 Standard</label><br>
  <input type="checkbox" id="q3r2" name="question3" value="2">
  <label for="q3r2">S3 Glacier</label><br>
  <input type="checkbox" id="q3r3" name="question3" value="3">
  <label for="q3r3">S3 Intelligent-Tiering</label><br>
  <input type="checkbox" id="q3r4" name="question3" value="4">
  <label for="q3r4">S3 standard – Accès peu fréquent</label><br>
  <input type="checkbox" id="q3r5" name="question3" value="5">
  <label for="q3r5">S3 Glacier Deep Archive</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

Les deux bonnes réponses sont les suivantes :

- **S3 Glacier**
- **S3 Glacier Deep Archive**

Les objets stockés dans la classe de stockage S3 Glacier peuvent être récupérés entre quelques minutes et quelques heures. Par comparaison, les objets stockés dans la classe de stockage S3 Glacier Deep Archive peuvent être récupérés dans les 12 heures.

Les autres réponses sont incorrectes, car :

- S3 standard est une classe de stockage idéale pour les données fréquemment consultées, et non pour les données d’archivage.
- S3 Intelligent-Tiering contrôle les modèles d’accès des objets et les déplace automatiquement entre les classes de stockage S3 standard et S3 standard – Accès peu fréquent. Ce service n’est pas conçu pour les données d’archivage.
- S3 standard – Accès peu fréquent est idéal pour les données rarement consultées, mais nécessitant une disponibilité élevée en cas de besoin.

En savoir plus :

- [Classes de stockage Amazon S3](https://aws.amazon.com/s3/storage-classes/)
</div>
  
  
**Quelles déclarations relatives aux volumes Amazon EBS et aux systèmes de fichiers Amazon EFS sont VRAIES ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Les volumes EBS stockent les données dans une seule zone de disponibilité. Les systèmes de fichiers Amazon EFS stockent les données dans plusieurs zones de disponibilité.</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">Les volumes EBS stockent les données dans plusieurs zones de disponibilité. Les systèmes de fichiers Amazon EFS stockent les données dans plusieurs zones de disponibilité.</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">Les volumes EBS et les systèmes de fichiers Amazon EFS stockent les données dans une seule zone de disponibilité.</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Les volumes EBS et les systèmes de fichiers Amazon EFS stockent les données dans plusieurs zones de disponibilité.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Les volumes EBS stockent les données dans une seule zone de disponibilité. Les systèmes de fichiers Amazon EFS stockent les données dans plusieurs zones de disponibilité**.
Un volume EBS doit se trouver dans la même zone de disponibilité que l’instance Amazon EC2 à laquelle il est attaché.
Les données d’un système de fichiers Amazon EFS sont accessibles simultanément à partir de l’ensemble des zones de disponibilité de la région où se trouve le système de fichiers.

En savoir plus :

- [Volumes Amazon EBS](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-volumes.html)
- [Amazon EFS : fonctionnement](https://docs.aws.amazon.com/efs/latest/ug/how-it-works.html)
</div>
  
  
**Vous souhaitez stocker des données dans un service de stockage d’objets. Quel service AWS convient le mieux à ce type de stockage ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Amazon Managed Blockchain</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Amazon Elastic File System (Amazon EFS)</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Amazon Elastic Block Store (Amazon EBS)</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Amazon Simple Storage Service (Amazon S3)</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Simple Storage Service (Amazon S3)**.

Les autres réponses sont incorrectes, car :

- Amazon Managed Blockchain est un service que vous pouvez utiliser pour créer et gérer des réseaux blockchain avec des frameworks open source. Blockchain est un système de registre distribué qui permet à plusieurs parties d’exécuter des transactions et de partager des données sans autorité centrale
- Amazon Elastic File System (Amazon EFS) est un système de fichiers évolutif utilisé avec les AWS Cloud services et les ressources sur site Ce service ne stocke pas les données en tant que stockage d’objets.
- Amazon Elastic Block Store (Amazon EBS) est un service qui fournit des volumes de stockage de niveau bloc que vous pouvez utiliser avec des instances Amazon EC2.

En savoir plus :

- [Amazon S3](https://aws.amazon.com/s3)
- [Qu’est-ce que le stockage d’objets dans le cloud ?](https://aws.amazon.com/what-is-cloud-object-storage/)
</div>

**Quelle affirmation décrit le mieux Amazon DynamoDB ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">Un service qui vous permet d’exécuter des bases de données relationnelles dans AWS Cloud</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Un service de base de données clé-valeur sans serveur</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">Un service que vous pouvez utiliser pour migrer des bases de données relationnelles, des bases de données non relationnelles et d’autres types de stockages de données</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">Une base de données relationnelle d’entreprise
</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Un service de base de données clé-valeur sans serveur**.
Amazon DynamoDB est un service de base de données clé-valeur. Il est sans serveur, ce qui signifie que vous ne devez pas mettre en service, appliquer des correctif ou gérer des serveurs.
Les autres réponses sont incorrectes, car :

- Un service qui vous permet d’exécuter des bases de données relationnelles dans AWS Cloud décrit Amazon Relational Database Service (Amazon RDS).
- Un service que vous pouvez utiliser pour migrer des bases de données relationnelles, des bases de données non relationnelles et d’autres types de stockages de données décrit AWS Database Migration Service (AWS DMS).
- Une base de données relationnelle d’entreprise décrit Amazon Aurora.

En savoir plus :

- [Amazon DynamoDB](https://aws.amazon.com/dynamodb)
</div>

**Quel service est utilisé pour interroger et analyser des données dans un entrepôt de données ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">Amazon Redshift</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">Amazon Neptune</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">Amazon DocumentDB</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">Amazon ElastiCache</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Redshift**.

Amazon Redshift est un service de stockage de données que vous pouvez utiliser pour l’analytique des données Big Data. Utilisez Amazon Redshift pour collecter des données provenant de nombreuses sources et vous aider à comprendre les relations et les tendances entre vos données.

Les autres réponses sont incorrectes, car :

- Amazon Neptune est un service de base de données orientée graphe. Vous pouvez utiliser Amazon Neptune pour créer et exécuter des applications qui fonctionnent avec des jeux de données hautement connectés, tels que les moteurs de recommandation, la détection des fraudes et les graphiques de connaissances.
- Amazon DocumentDB est un service de base de données documentaire qui prend en charge les charges de travail MongoDB.
- Amazon ElastiCache est un service qui ajoute des couches de mise en cache au-dessus de vos bases de données afin d’améliorer les temps de lecture des demandes courantes.

En savoir plus :

- [Amazon Redshift](https://aws.amazon.com/redshift)
</div>
  
Le module suivant examine la sécurité dans AWS Cloud.