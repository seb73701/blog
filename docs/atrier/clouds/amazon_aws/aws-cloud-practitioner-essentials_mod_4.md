---
title: Module 4 - Mise en réseau
description: 
published: true
date: 2023-04-21T10:56:10.501Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:56:04.325Z
---

# Module 4 : introduction

## Objectifs d’apprentissage

Dans ce module, vous apprendrez à :

- Décrire les concepts de base de la mise en réseau
- Décrire la différence entre des ressources de réseaux publics et privés 
- Expliquer une passerelle de réseau privé virtuel à l’aide d’un scénario de la vie réelle 
- Expliquer un réseau privé virtuel (VPN) à l’aide d’un scénario de la vie réelle.
- Décrire les avantages d’AWS Direct Connect 
- Décrire les avantages des déploiements hybrides 
- Décrire les couches de sécurité utilisées dans une stratégie informatique
- Décrire les services que les clients utilisent pour interagir avec le réseau mondial AWS

```
video : xxx.mkv
```
*Figure. Module introdution - Networking*

<div class="collapseContent" display="Transcription" value="true">
  
> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Si nous repensons à notre café ou compte AWS, les choses devraient maintenant se dérouler sans heurt. Cependant, imaginons que quelques clients impatients aient envie de passer leur commande directement aux baristas au lieu du caissier devant ? Et bien cela n’a pas de sens de permettre à chaque client d’interagir avec nos baristas car ils sont concentrés sur la préparation de boissons au café fraichement moulu. Que faisons-nous ? 
> 
> C’est ça, on peut le dire ensemble, Amazon Virtual Private Cloud, (VPC comme on aime l’appeler). VPC vous permet de mettre en service une section logiquement isolée du cloud AWS où vous pouvez lancer des ressources AWS dans un réseau virtuel que vous définissez. Ces ressources peuvent être destinées au public pour accéder à Internet, ou à titre privé sans accès à Internet, habituellement pour les services backend comme les bases de données ou les serveurs d’application. Le regroupement public et privé de ressources est appelé sous-réseaux et ce sont des plages d’adresses IP dans votre VPC. 
> 
> Dans notre café, nous avons différents employés et l’un d’eux est le caissier. Il prend les commandes des clients et nous voulons donc que les clients interagissent avec eux, et nous les plaçons donc dans ce que nous appelons un sous-réseau public. Il peut maintenant parler aux clients ou à Internet. Quant à nos baristas, nous souhaitons qu’ils se concentrent sur la préparation du café et pas sur les interactions directes avec les clients, et nous les plaçons donc dans un sous-réseau privé. 
> 
> Passons à la prochaine vidéo où nous allons aborder la mise en réseau.
</div>
  
## Connectivité à AWS

```
video : xxx.mkv
```
*Figure. Connectivity to AWS*

<div class="collapseContent" display="Transcription" value="true">

> ![morgan_01.png](/assets/img/clouds/tempo/morgan_02.png =50x){.align-left}
> Un VPC, ou cloud privé virtuel, est essentiellement votre propre réseau privé dans AWS. Un VPC vous permet de définir votre plage IP privée pour vos ressources AWS, et de placer des éléments comme des instances EC2 et des ELB dans votre VPC. 
> 
> Vous n'allez pas simplement jeter vos ressources dans un VPC et passer à autre chose. Vous les placez dans différents sous-réseaux. Les sous-réseaux sont des morceaux d’adresses IP dans votre VPC qui vous permettent de regrouper des ressources. Les sous-réseaux et les règles de réseaux dont nous parlerons plus tard, contrôlent si les ressources sont disponibles en public ou en privé. Ce que je veux dire, c'est que, pour certains VPC, vous pourriez avoir des ressources accessibles sur Internet auxquelles le public devrait être en mesure d’accéder comme un site web public par exemple. 
> 
> Toutefois, dans d'autres scénarios, vous pourriez avoir des ressources qui ne doivent être accessibles que si quelqu’un est connecté à votre réseau privé. Il peut s'agir de services internes, comme une application RH ou une base de données backend. D'abord, parlons des ressources destinées au public. Pour autoriser le trafic public à partir d'Internet à accéder à votre VPC, vous devez associer une passerelle Internet (IGW) à votre VPC. 
> 
> Une passerelle Internet est comme une porte ouverte au public. Pensez à notre café. Sans porte d'entrée, les clients ne pourraient pas entrer et commander leur café, donc nous installons une porte d’entrée et les clients peuvent entrer et sortir par cette porte en venant et en quittant notre boutique. La porte d'entrée dans cet exemple est comme une passerelle Internet. Sans elle, personne ne peut accéder aux ressources placées à l’intérieur de votre VPC. 
> 
> Ensuite, parlons d'un VPC avec toutes les ressources privées internes. Nous ne voulons pas que n'importe qui puisse accéder à ces ressources. Nous ne voulons donc pas de passerelle Internet connectée à notre VPC. À la place, nous voulons une passerelle privée qui ne laisse entrer les clients que s’ils proviennent d’un réseau approuvé, pas l’Internet public. Cette porte privée s’appelle une passerelle privée virtuelle, et elle vous permet de créer une connexion VPN entre un réseau privé, comme votre centre de données sur site ou un réseau d’entreprise interne et votre VPC. 
> 
> Pour ramener ça au café, ce serait comme avoir un trajet en bus privé allant de mon immeuble au café. Si je veux aller prendre un café, je dois d’abord utiliser mon badge pour entrer dans le bâtiment, et donc confirmer mon identité, puis je peux prendre la ligne de bus secrète jusqu’au café interne que seuls les gens de mon immeuble peuvent utiliser. Donc, si vous voulez établir une connexion VPN chiffrée avec vos ressources AWS internes privées, vous devez attacher une passerelle privée virtuelle à votre VPC. 
> 
> Le problème avec notre ligne de bus super secrète, c’est qu’elle utilise toujours la voie publique. Elle est sujette aux embouteillages et aux ralentissements du reste du monde qui vaque à ses occupations. La même chose est vraie pour les connexions VPN. Elles sont privées et chiffrées, mais elles utilisent toujours une connexion Internet classique dont la largeur de bande est partagée par de nombreux utilisateurs d’Internet. 
> 
> Donc, ce que j’ai fait pour rendre les choses plus fiables et moins sujettes aux ralentissements c’est de créer une porte magique totalement séparée qui mène directement du studio au café. Personne d'autre circulant sur la route ne peut me ralentir, car c’est ma porte directe, personne d’autre ne peut l’utiliser. Quoi, vous n’aviez pas une porte magique secrète dans votre café préféré ? Très bien, passons à autre chose. Le fait est que vous voulez toujours une connexion privée, mais vous voulez qu’elle soit dédiée et partagée avec personne d’autre. Vous voulez une latence aussi réduite que possible avec une sécurité aussi élevée que possible. 
> 
> Avec AWS, vous pouvez y parvenir grâce à AWS Direct Connect. Direct Connect vous permet d’établir une connexion par fibre complétement privée et dédiée, de votre centre de données à AWS. Vous travaillez avec un partenaire Direct Connect dans votre région pour établir cette connexion car, comme ma porte magique, AWS Direct Connect fournit une ligne physique qui connecte votre réseau à votre VPC AWS. Cela peut vous aider à répondre aux besoins réglementaires et de conformité, ainsi qu’à contourner tout problème potentiel de bande passante. Il est également important de noter qu’un VPC peut avoir plusieurs types de passerelles attachées pour plusieurs types de ressources résidant toutes dans le même VPC mais dans différents sous-réseaux. 
> 
> Merci de m'avoir écouté. Je vais rester assis ici et continuer à commander des cafés depuis ma porte magique. À bientôt.
</div>

## Amazon Virtual Private Cloud (Amazon VPC)

Imaginez les millions de clients qui utilisent les services AWS. Imaginez également les millions de ressources que ces clients ont créées, telles que les instances Amazon EC2. Sans frontières autour de toutes ces ressources, le trafic réseau pourrait circuler entre elles sans restriction. 

Un service de réseaux que vous pouvez utiliser pour établir des limites autour de vos ressources AWS : **[Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/vpc/)**.

![aws_vpc_01.png](/assets/img/clouds/tempo/aws_vpc_01.png)

Amazon VPC vous permet de mettre en service une section isolée de AWS Cloud. Dans cette section isolée, vous pouvez lancer des ressources dans un réseau virtuel que vous définissez. Dans un cloud privé virtuel (VPC), vous pouvez organiser vos ressources en sous-réseaux. Un **sous-réseau** est une section d’un VPC qui peut contenir des ressources telles que des instances Amazon EC2.

## Passerelle Internet

Pour autoriser le trafic public à partir d’Internet à accéder à votre VPC, attachez une **passerelle Internet** au VPC.

![vpc_01.png](/assets/img/clouds/tempo/vpc_01.png)

Une passerelle Internet est une connexion entre un VPC et Internet. Vous pouvez voir une passerelle Internet comme la porte que les clients empruntent pour entrer dans le café. Sans passerelle Internet, personne ne peut accéder aux ressources de votre VPC.

> Que se passe-t-il si vous avez un VPC qui inclut uniquement des ressources privées ?
{.is-info}

## Passerelle de réseau privé virtuel

Pour accéder aux ressources privées dans un VPC, vous pouvez utiliser une **passerelle de réseau privé virtuel**. 

Voici un exemple de fonctionnement d’une passerelle de réseau privé virtuel. Vous pouvez voir Internet comme la route entre votre maison et le café. Supposons que vous voyagez sur cette route avec un garde du corps pour vous protéger. Vous utilisez toujours la même route que les autres clients, mais avec une couche de protection supplémentaire. 

Le garde du corps est comme une connexion de réseau privé virtuel (VPN) qui chiffre (ou protège) votre trafic Internet de toutes les autres demandes qui l’entourent. 

La passerelle de réseau privé virtuel est le composant qui permet au trafic Internet protégé d’entrer dans le VPC. Même si votre connexion au café offre une protection supplémentaire, les embouteillages sont possibles, car vous empruntez la même route que les autres clients.

![vpc_02.png](/assets/img/clouds/tempo/vpc_02.png)

Une passerelle de réseau privé virtuel vous permet d’établir une connexion de réseau privé virtuel (VPN) entre votre VPC et un réseau privé, tel qu’un centre de données sur site ou un réseau interne d’entreprise. Une passerelle de réseau privé virtuel autorise le trafic vers le VPC uniquement s’il provient d’un réseau approuvé.

## AWS Direct Connect

**[AWS Direct Connect](https://aws.amazon.com/directconnect/)** est un service qui vous permet d’établir une connexion privée dédiée entre votre centre de données et un VPC.  

Supposons qu’il y ait un immeuble d’appartements avec un couloir reliant directement le bâtiment au café. Seuls les résidents de l’immeuble peuvent emprunter ce couloir. 

Ce couloir privé offre le même type de connexion dédiée qu’AWS Direct Connect. Les résidents peuvent entrer dans le café sans avoir à emprunter la voie publique partagée avec d’autres clients.

![vpc_03.png](/assets/img/clouds/tempo/vpc_03.png)

La connexion privée fournie par AWS Direct Connect vous aide à réduire les coûts de réseau et à augmenter la quantité de bande passante pouvant circuler sur votre réseau.

# Sous-réseaux et listes de contrôle d’accès réseau

```
video : xxx.mkv
```
*Figure. Subnets and network access control lists*

<div class="collapseContent" display="Transcription" value="true">

> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> Bienvenue dans votre VPC. On peut le considérer comme une solide forteresse où rien n'entre ou ne sort sans autorisation explicite. Vous disposez d'une passerelle sur le VPC qui autorise uniquement le trafic entrant ou sortant du VPC. Mais cela ne couvre que le périmètre, et ce n'est qu'une partie de la sécurité du réseau sur laquelle vous devriez vous concentrer dans le cadre de votre stratégie informatique. 
> 
> AWS dispose d'une large gamme d'outils qui couvrent toutes les couches de sécurité : renforcement du réseau, sécurité des applications, identité de l’utilisateur, autorisation et authentification, déni de service distribué ou prévention des DDoS, intégrité des données, chiffrement et bien plus encore. Parlons de quelques-uns de ces éléments clés. Et si vous voulez vraiment en savoir plus sur la sécurité, suivez les liens de cette page pour plus d’informations et une formation complémentaire sur la manière de verrouiller votre infrastructure pour que personne n’ait accès à votre recette, comme tante Robin avec sa recette de tarte au citron. 
> 
> Aujourd'hui, je veux parler de quelques aspects du renforcement du réseau, en regardant ce qui se passe à l'intérieur du VPC. Désormais, la seule raison technique d'utiliser des sous-réseaux dans le VPC est de contrôler l'accès aux passerelles. Les sous-réseaux publics ont accès à la passerelle Internet, mais pas les sous-réseaux privés. Mais les sous-réseaux peuvent aussi contrôler les autorisations de trafic. Les paquets sont des messages provenant d'Internet, et chaque paquet qui franchit les limites du sous-réseau est vérifié par rapport à une liste de contrôle d'accès de réseau ou ACL réseau. Cette vérification permet de savoir si le paquet dispose des autorisations nécessaires pour quitter ou entrer dans le sous-réseau en fonction de qui l'a envoyé et de la façon dont il tente de communiquer. 
> 
> Vous pouvez voir les listes ACL réseau comme des agents de contrôle des passeports. Si vous êtes sur la liste, vous passez. Si vous n'êtes pas sur la liste ou si vous êtes explicitement sur la liste Ne pas entrer, vous êtes bloqué. Les ACL réseau vérifient le trafic entrant et sortant d'un sous-réseau, tout comme le contrôle des passeports. La liste est vérifiée lorsque vous vous rendez dans un pays et en sortez. Et ce n'est pas parce que vous avez pu entrer que vous pourrez sortir. Le trafic approuvé peut être envoyé, et le trafic potentiellement dangereux, comme les tentatives de contrôle d'un système par le biais de requêtes administratives, est bloqué avant de toucher sa cible. Sans contact, aucun piratage n'est possible. 
> 
> La sécurité paraît être de très bonne qualité, mais cela ne répond pas à toutes les questions de contrôle du réseau. Parce qu’une liste ACL réseau évalue seulement un paquet s’il traverse une limite de sous-réseau, en entrant ou sortant. Elle n’évalue pas si un paquet peut atteindre une instance EC2 spécifique ou pas. Il arrive parfois que plusieurs instances EC2 se trouvent dans le même sous-réseau, mais elles peuvent avoir des règles différentes concernant la personne qui peut envoyer les messages et le port vers lesquels ces messages sont autorisés à être envoyés. Vous avez donc besoin d'une sécurité du réseau au niveau de l'instance. 
> 
> Pour résoudre les questions d'accès au niveau de l'instance, nous introduisons des groupes de sécurité. Chaque instance EC2, lorsqu'elle est lancée, est automatiquement transférée à un groupe de sécurité. Par défaut, le groupe de sécurité n'autorise aucun trafic vers l'instance. Tous les ports sont bloqués, toutes les adresses IP qui envoient des paquets sont bloquées. C'est très sécurisé, mais pas forcément très utile. Si vous souhaitez qu'une instance accepte réellement du trafic de l'extérieur, par exemple un message provenant d'une instance frontale ou un message provenant d'Internet. Il est donc évident que vous pouvez modifier le groupe de sécurité pour accepter un type de trafic spécifique. Dans le cas d'un site web, vous souhaitez que le trafic web ou HTTPS soit accepté, mais pas d'autres types de trafic, par exemple un système d'exploitation ou des demandes d'administration. 
> 
> Si les réseaux ACL sont notre contrôle des passeports, un groupe de sécurité est comme le portier de votre bâtiment, le bâtiment étant l'instance EC2 dans ce cas. Le portier consulte une liste pour s'assurer que quelqu'un est autorisé à entrer dans le bâtiment, mais il ne prend pas la peine de vérifier la liste à la sortie. Avec les groupes de sécurité, vous laissez entrer un trafic spécifique et, par défaut, tout le trafic sortant est autorisé. Attends une minute, Blaine. Tu viens de décrire deux moteurs différents qui font chacun exactement le même travail. Laisser les bons paquets entrer, bloquer les mauvais paquets. La principale différence entre un groupe de sécurité et un ACL réseau est que le groupe de sécurité est doté d’un état, ce qui signifie que, comme nous en avons parlé, il possède une sorte de mémoire quand il s'agit de savoir qui laisser entrer ou sortir, et que l'ACL réseau est sans état, ne se souvient de rien, et vérifie chaque paquet qui franchit sa frontière, quelles que soient les circonstances. 
> 
> Cette métaphore est importante à comprendre. Donc je veux illustrer l'aller-retour du paquet qui passe d'une instance à une autre dans un sous-réseau différent. Cette gestion du trafic ne se soucie pas du contenu du paquet lui-même. En fait, il n'ouvre même pas l'enveloppe, il ne peut pas. Tout ce qu'il peut faire est de vérifier que l’expéditeur figure sur la liste approuvée. 
> 
> Bien. Commençons par l'instance A. Nous voulons envoyer un paquet de l'instance A vers l'instance B dans un sous-réseau différent, un même VPC, des sous-réseaux différents. Donc l'instance A envoie le paquet. La première chose qui se produit est que le paquet atteint la frontière du groupe de sécurité de l'instance A. Par défaut, tous les trafics sortants sont autorisés à partir d’un groupe de sécurité. Vous pouvez donc passer directement à côté des portiers et partir. Cool, pas vrai ? Le paquet a passé le groupe de sécurité de l'instance A. Maintenant, il doit quitter la limite du sous-réseau. À la frontière, le passeport doit désormais passer par le contrôle des passeports, le réseau ACL. L'ACL réseau ne se soucie pas de ce que le groupe de sécurité a autorisé. Il a sa propre liste de qui peut passer ou non. Si l'adresse cible est autorisée, vous pouvez continuer votre voyage, ce qui est le cas. 
> 
> Nous avons donc quitté le sous-réseau d'origine et maintenant le paquet va vers le sous-réseau cible où se trouve l'instance B. Maintenant, sur ce sous-réseau cible, nous avons une fois de plus un contrôle des passeports. Ce n'est pas parce que le paquet a été autorisé à quitter son pays d'origine qu'il peut entrer dans le pays de destination, ou le sous-réseau dans ce cas. Ils ont tous deux des contrôleurs de passeports uniques dotés de leurs propres listes de contrôle. Vous devez être approuvé sur les deux listes, sinon vous pourriez être refoulé à la frontière. Il s'avère que le paquet figure sur la liste approuvée pour ce sous-réseau, il est donc autorisé à entrer via l'ACL réseau dans le sous-réseau. Vous avez presque terminé. Maintenant, nous approchons de l'instance cible, l'instance B. Chaque instance EC2 a son propre groupe de sécurité. Si vous voulez entrer dans cette instance, le portier devra vérifier si vous êtes autorisé à entrer, et nous sommes sur la liste. Le paquet a atteint l'instance cible. 
> 
> Une fois que la transaction est terminée, il est temps de rentrer à la maison. C'est le modèle du trafic de retour. C’est le plus intéressant, parce que c’est là que la nature avec état ou sans état des différents moteurs entrent en jeu. Parce que le paquet doit toujours être évalué à chaque point de vérification. Les groupes de sécurité autorisent tout le trafic de retour par défaut. Donc, ils n'ont pas à vérifier une liste pour voir s’ils peuvent sortir. Au lieu de cela, ils permettent automatiquement au trafic de retour de passer quoi qu'il arrive. Contrôle des passeports ici à la frontière du sous-réseau, ces ACL réseau ne se souviennent pas de l'état. Ils ne se soucient pas que le paquet est arrivé ici. Il pourrait être sur une liste qui l'empêche de partir. Chaque entrée et sortie est vérifiée par rapport à la liste correspondante. L'adresse de retour du paquet doit être sur leur liste approuvée pour franchir la frontière. Il a passé la frontière du sous-réseau d’origine mais nous devons également négocier ici le contrôle ACL réseau du passeport. Contrôles sans état signifie que la liste est toujours vérifiée. 
> 
> Le paquet a passé l'ACL réseau au niveau du sous-réseau, ce qui signifie que le paquet est maintenant retourné à l'instance A, mais le groupe de sécurité, le portier, est toujours en charge ici. La principale différence, cependant, c'est que les groupes de sécurité sont avec état. Le groupe de sécurité reconnaît le paquet d'avant. Donc, il n'a pas besoin de vérifier pour voir s'il est autorisé. Rentre à la maison. 
> 
> Maintenant, on dirait que nous avons dépensé beaucoup d'énergie pour récupérer un paquet d'une instance à une autre et en retour. Vous pourriez vous soucier de tous les frais généraux de réseau que cela pourrait engendrer. En réalité, tous ces échanges se produisent instantanément dans le cadre du fonctionnement réel de la mise en réseau AWS. Si vous voulez connaître la technologie qui rend tout cela possible, vous devez vous inscrire à une série de formations complètement différente. Une bonne sécurité réseau tirera parti des ACL réseau et des groupes de sécurité, car la sécurité approfondie est essentielle pour les architectures modernes.
</div>

Pour en savoir plus sur le rôle des sous-réseaux dans un VPC, consultez l’exemple suivant du café.

Tout d’abord, les clients communiquent leurs commandes au caissier. Le caissier passe ensuite les commandes au barista. Ce processus permet à la queue d’avancer sans heurt à mesure que de plus en plus de clients arrivent. 

Supposons que certains clients essaient d’ignorer la file d’attente à la caisse et de communiquer leurs commandes directement au barista. Cela perturbe le flux de trafic et fait en sorte que les clients accèdent à une partie du café qui ne leur est pas autorisée.

![vpc_04.png](/assets/img/clouds/tempo/vpc_04.png)

Pour résoudre ce problème, les propriétaires du café divisent la zone du comptoir en plaçant le caissier et le barista à des postes de travail séparés. Le poste de travail du caissier est orienté vers le public et conçu pour recevoir les clients. La zone du barista est privée. Le barista peut toujours recevoir les commandes du caissier, mais pas directement des clients.

![vpc_05.png](/assets/img/clouds/tempo/vpc_05.png)

Cela est semblable à la façon dont vous pouvez utiliser les services de mise en réseau AWS pour isoler les ressources et déterminer exactement la manière dont le trafic réseau circule.

Dans le café, vous pouvez considérer la zone du comptoir comme un VPC. La zone du comptoir se divise en deux zones distinctes pour le poste de travail du caissier et le poste de travail du barista. Dans un VPC, les **sous-réseaux** sont des zones distinctes utilisées pour regrouper les ressources.

## Sous-réseaux

Un sous-réseau est une section d’un VPC dans laquelle vous pouvez regrouper des ressources en fonction des besoins de sécurité ou opérationnels. Les sous-réseaux peuvent être publics ou privés.

![vpc_06.png](/assets/img/clouds/tempo/vpc_06.png =500x)

**Les sous-réseaux publics** contiennent des ressources qui doivent être accessibles au public, telles que le site web d’une boutique en ligne.

**Les sous-réseaux privés** contiennent des ressources qui ne doivent être accessibles que par l’intermédiaire de votre réseau privé, par exemple une base de données contenant les informations personnelles des clients et l’historique des commandes. 

Dans un VPC, les sous-réseaux peuvent communiquer entre eux. Par exemple, vous pouvez avoir une application qui implique des instances Amazon EC2 dans un sous-réseau public communiquant avec des bases de données situées dans un sous-réseau privé.

## Trafic réseau dans un VPC

Lorsqu’un client demande des données à partir d’une application hébergée dans AWS Cloud, cette requête est envoyée sous forme de paquet. Un **paquet** est une unité de données envoyée sur Internet ou sur un réseau. 

Il entre dans un VPC via une passerelle Internet. Avant qu’un paquet puisse entrer ou quitter un sous-réseau, les autorisations sont vérifiées. Ces autorisations indiquent la personne qui a envoyé le paquet et comment le paquet tente de communiquer avec les ressources d’un sous-réseau.

Le composant VPC qui vérifie les autorisations des paquets pour les sous-réseaux est une **[liste de contrôle d’accès (ACL) réseau](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)**.

## Listes de contrôle d’accès (ACL) réseau

Une liste de contrôle d’accès (ACL) réseau est un pare-feu virtuel qui contrôle le trafic entrant et sortant au niveau des sous-réseaux.

Par exemple, sortez du café et imaginez-vous dans un aéroport. Dans l’aéroport, les voyageurs essaient d’entrer dans un autre pays. Vous pouvez considérer les voyageurs comme des paquets et l’agent de contrôle des passeports comme une ACL réseau. L’agent de contrôle des passeports vérifie les justificatifs d’identité des voyageurs lorsqu’ils entrent et sortent du pays. Si un voyageur figure sur une liste approuvée, il est en mesure d’y accéder. Toutefois, s’il ne figure pas sur la liste approuvée ou figure explicitement sur une liste de voyageurs interdits, il ne peut pas entrer.

![vpc_07.png](/assets/img/clouds/tempo/vpc_07.png =500x)

Chaque compte AWS inclut une liste ACL réseau par défaut. Lors de la configuration de votre VPC, vous pouvez utiliser la liste ACL réseau par défaut de votre compte ou créer des listes ACL réseau personnalisées. 

La liste ACL réseau par défaut de votre compte autorise tout le trafic entrant et sortant, mais vous pouvez modifier cela en ajoutant vos propres règles. Pour les ACL réseau personnalisées, tout le trafic entrant et sortant est refusé jusqu’à ce que vous ajoutiez des règles pour spécifier le trafic à autoriser. En outre, toutes les ACL réseau ont une règle de refus explicite. Cette règle garantit que si un paquet ne correspond à aucune des autres règles de la liste, le paquet est refusé.

## Filtrage des paquets sans état (stateless)

Les ACL réseau effectuent **un filtrage** des paquets sans état. Elles ne se souviennent de rien et vérifient les paquets qui traversent la frontière du sous-réseau dans les deux sens : entrants et sortants. 

Rappelons l’exemple précédent d’un voyageur qui veut entrer dans un autre pays. Cela est semblable à l’envoi d’une demande depuis une instance Amazon EC2 et vers Internet.

Lorsqu’une réponse de paquet pour cette demande revient au sous-réseau, l’ACL réseau ne se souvient pas de votre requête précédente. L’ACL réseau vérifie la réponse du paquet par rapport à sa liste de règles pour déterminer s’il faut l’autoriser ou la refuser.

![vpc_08.png](/assets/img/clouds/tempo/vpc_08.png)

Une fois qu’un paquet est entré dans un sous-réseau, ses autorisations doivent être évaluées pour l’accès aux ressources du sous-réseau, telles que les instances Amazon EC2. 

Le composant VPC qui vérifie les autorisations des paquets pour une instance Amazon EC2 est un **[groupe de sécurité](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)**.

## Groupes de sécurité

Un groupe de sécurité est un pare-feu virtuel qui contrôle le trafic entrant et sortant d’une instance Amazon EC2.

![groupe_secu_01.png](/assets/img/clouds/tempo/groupe_secu_01.png)

Par défaut, un groupe de sécurité refuse tout le trafic entrant et autorise tout le trafic sortant. Vous pouvez ajouter des règles personnalisées pour configurer le trafic à autoriser ou à refuser.

Pour cet exemple, supposons que vous soyez dans un immeuble d’appartements avec un préposé à la porte qui accueille les invités dans le hall. Vous pouvez considérer les invités comme des paquets et le préposé à la porte comme un groupe de sécurité. À l’arrivée des invités, le préposé à la porte vérifie sa liste pour s’assurer qu’ils peuvent entrer dans le bâtiment. Toutefois, le préposé à la porte ne vérifie pas à nouveau la liste lorsque les invités quittent le bâtiment.

Si vous avez plusieurs instances Amazon EC2 dans un sous-réseau, vous pouvez les associer au même groupe de sécurité ou utiliser des groupes de sécurité différents pour chaque instance. 

## Filtrage des paquets avec état (statefull)

Les groupes de sécurité effectuent **un filtrage** des paquets avec état. Ils se souviennent des décisions antérieures prises pour les paquets entrants.

Prenons le même exemple de l’envoi d’une demande à partir d’une instance Amazon EC2 vers Internet. 

Lorsqu’une réponse de paquet pour cette demande est renvoyée à l’instance, le groupe de sécurité se souvient de votre requête précédente. Le groupe de sécurité permet à la réponse de continuer, quelles que soient les règles du groupe de sécurité entrant.

![groupe_secu_02.png](/assets/img/clouds/tempo/groupe_secu_02.png)

Les ACL réseau et les groupes de sécurité vous permettent de configurer des règles personnalisées pour le trafic dans votre VPC. Alors que vous continuez à en apprendre davantage sur la sécurité et le réseau AWS, assurez-vous de comprendre les différences entre les ACL réseau et les groupes de sécurité.

![groupe_secu_03.png](/assets/img/clouds/tempo/groupe_secu_03.png)

## Contrôle des connaissances

Pour cet exercice de révision, imaginez que votre entreprise lance une application de stockage de photos en ligne. Aidez à déterminer quel composant VPC doit être utilisé pour certains aspects de l’application.



Dans ce qui suit, faites correspondre chaque partie de l’application avec le composant VPC approprié.

A = Créer une connexion VPN entre le VPC et le réseau interne d'entreprise
B = Isoler les bases de données contenant les informations personnelles des clients
C = Soutenir le site web destiné au contact
D = Etablir une connexion dédiée entre le centre de données sur site et le VPC

1 = Sous-réseau privé
2 = Passerelle de réseau privé virtuel
3 = Sous-réseau public
4 = AWS Direct Connect

<div class="collapseContent" display="Solution" value="true">

A = Passerelle de réseau privé virtuel
B = Sous-réseau privé
C = Sous-réseau public
D = AWS Direct Connect
</div>
  
  
**Quelle déclaration décrit le mieux la liste de contrôle d’accès réseau par défaut d’un compte AWS ?**

<form>
  <input type="radio" id="q1r1" name="question1" value="1">
  <label for="q1r1">Il est sans état et refuse tout le trafic entrant et sortant.</label><br>
  <input type="radio" id="q1r2" name="question1" value="2">
  <label for="q1r2">Il est avec état et autorise tout le trafic entrant et sortant.</label><br>
  <input type="radio" id="q1r3" name="question1" value="3">
  <label for="q1r3">Il est sans état et autorise tout le trafic entrant et sortant.</label><br>
  <input type="radio" id="q1r4" name="question1" value="4">
  <label for="q1r4">Il est avec état et refuse tout le trafic entrant et sortant.</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">
  
La bonne réponse est **Il est sans état et autorise tout le trafic entrant et sortant**.

Les listes de contrôle d’accès (ACL) réseau effectuent un filtrage des paquets **sans état**. Elles ne se souviennent de rien et vérifient les paquets qui traversent la frontière du sous-réseau dans les deux sens : entrants et sortants.

Chaque compte AWS inclut une liste ACL réseau par défaut. Lors de la configuration de votre VPC, vous pouvez utiliser la liste ACL réseau par défaut de votre compte ou créer des listes ACL réseau personnalisées.

La liste ACL réseau par défaut de votre compte autorise tout le trafic entrant et sortant, mais vous pouvez modifier cela en ajoutant vos propres règles. Pour les ACL réseau personnalisées, tout le trafic entrant et sortant est refusé jusqu’à ce que vous ajoutiez des règles pour spécifier quel trafic doit être autorisé. En outre, toutes les ACL réseau ont une règle de refus explicite. Cette règle garantit que si un paquet ne correspond à aucune autre règle de la liste, le paquet est refusé.

En savoir plus :

- [ACL réseau](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)
</div>
  
  
# Réseaux mondiaux

```
video : xxx.mkv
```
*Figure. Global network*

<div class="collapseContent" display="Transcription" value="true">

> ![rudy_01.jpg](/assets/img/clouds/tempo/rudy_01.jpg =x50){.align-left}
> Nous avons beaucoup parlé de la façon dont vous interagissez avec votre infrastructure AWS. Mais comment vos clients interagissent-ils avec votre infrastructure AWS ? Et bien, si vous disposez d’un site web hébergé sur AWS, les clients entrent généralement sur votre site dans leur navigateur, ils appuient sur Entrée, une certaine magie de produit et le site s’ouvre. 
> 
> Mais comment fonctionne cette magie ? Et bien, c’est comme cette pièce magique que j’ai ici, vous savez, vous prenez une bouchée, et elle revient. Et bien, pas tout à fait comme ça, mais je vais vous parler de deux services, qui seraient utiles dans le cas du site web. Le premier étant Route 53. Route 53 est le service de nom de domaine d’AWS (DNS), et il est hautement disponible et évolutif. Attendez, vous voulez savoir ce qu'est DNS ? Imaginez DNS comme un service de traduction. Mais au lieu de traduire plusieurs langues, il traduit des noms de sites web en adresses IP, ou protocole Internet, des adresses que les ordinateurs peuvent lire. 
> 
> Par exemple, lorsque nous entrons une adresse de site web sur notre navigateur, il contacte Route 53 pour obtenir l’adresse IP du site, par exemple, 192.1.1.1, puis il achemine votre ordinateur ou le navigateur vers cette adresse. Si nous allons plus loin, Route 53 peut diriger le trafic vers différents points de terminaison en utilisant plusieurs stratégies de routage différentes, comme le routage basé sur la latence, le DNS de géolocalisation, la géoproximité, et le round robin pondéré. Si nous prenons le DNS de géolocalisation, cela signifie que nous dirigeons le trafic en fonction de l’emplacement du client. Donc le trafic venant d’Amérique du Nord est acheminé vers la région de l’Oregon, et le trafic d’Irlande est acheminé vers la région de Dublin. 
> 
> À titre d'exemple : Vous pouvez même utiliser Route 53 pour inscrire des noms de domaine, afin de pouvoir acheter et gérer vos propres noms de domaine directement sur AWS. 
> 
> En parlant de sites web, il y a un autre service qui peut aider à accélérer la diffusion des ressources du site Web aux clients, Amazon CloudFront. Souvenez-vous, nous avons déjà parlé des emplacements périphériques dans le cours, ces emplacements assurent la diffusion du contenu aussi près que possible des clients, et dans ce cadre, se trouve le réseau de diffusion du contenu (CDN). Petit rappel, un CDN est un réseau qui permet de diffuser du contenu périphérique aux utilisateurs selon leur emplacement géographique. 
> 
> Si nous revenons à notre exemple de l’Amérique du Nord par rapport à l’Irlande, imaginons que nous ayons un utilisateur à Seattle, qui souhaite accéder à un site web, pour accélérer l’opération, nous hébergeons le site dans l’Oregon, et nous déployons nos ressources web statiques, comme les images et les GIF dans CloudFront en Amérique du Nord. Cela signifie que le contenu est diffusé aussi proche de lui que possible, l’Amérique du Nord dans ce cas, lorsqu’il accède au site. Mais pour les utilisateurs irlandais, cela n’a pas de sens de diffuser ces ressources de l’Oregon, car la latence n’est pas favorable. Nous déployons donc ces mêmes ressources statiques dans CloudFront, mais cette fois-ci dans la région de Dublin. Cela signifie qu’ils peuvent accéder au même contenu, mais à partir d’un endroit plus proche d’eux, ce qui, de fait, améliore la latence. 
> 
> J’espère que vous êtes contents d'avoir pris connaissance de ces deux services. Merci de nous avoir suivis, et je vais disparaître comme ce tissu rouge. Voilà !
</div>
  
## Système de noms de domaine (DNS)

Supposons que l’entreprise X dispose d’un site web hébergé dans AWS Cloud. Les clients saisissent l’adresse web dans leur navigateur et peuvent accéder au site web. Cela se produit en raison de **la résolution du système de noms de domaine (DNS)**. La résolution DNS implique un serveur DNS qui communique avec un serveur web.

Vous pouvez considérer le DNS comme l’annuaire téléphonique d’Internet. La résolution DNS est le processus de conversion d’un nom de domaine en une adresse IP. 

![aws_route53_01.png](/assets/img/clouds/tempo/aws_route53_01.png)

Par exemple, supposons que vous souhaitez visiter le site web de l’entreprise X.

1. Lorsque vous entrez le nom de domaine dans votre navigateur, cette demande est envoyée à un serveur DNS. 
1. Le serveur DNS demande au serveur web l’adresse IP qui correspond au site web de l’entreprise X.
1. Le serveur web répond en fournissant l’adresse IP du site web de l’entreprise X, 192.0.2.0.

## Amazon Route 53

**[Amazon Route 53](https://aws.amazon.com/route53)** est un service web de DNS. Il offre aux développeurs et aux entreprises un moyen fiable d’acheminer les utilisateurs finaux vers des applications Internet hébergées dans AWS. 

![aws_route_53_01.png](/assets/img/clouds/tempo/aws_route_53_01.png)

Amazon Route 53 connecte les demandes des utilisateurs à l’infrastructure s’exécutant dans AWS (telles que les instances Amazon EC2 et les équilibreurs de charge). Il peut acheminer les utilisateurs vers une infrastructure en dehors d’AWS.

Une autre fonction de Route 53 est la possibilité de gérer les registres DNS pour les noms de domaine. Vous pouvez enregistrer de nouveaux noms de domaine directement dans Route 53. Vous pouvez également transférer des registres DNS pour des noms de domaine existants gérés par d’autres bureaux d’enregistrement de domaine. Cela vous permet de gérer tous vos noms de domaine au sein d’un seul emplacement.

Dans le module précédent, vous avez découvert Amazon CloudFront, un service de diffusion de contenu. L’exemple suivant décrit comment Route 53 et Amazon CloudFront collaborent pour fournir du contenu aux clients.

## Exemple : comment Amazon Route 53 et Amazon CloudFront diffusent du contenu

![aws_route53_02.png](/assets/img/clouds/tempo/aws_route53_02.png)

Supposons que l’application de l’entreprise X s’exécute sur plusieurs instances Amazon EC2. Ces instances sont dans un groupe à scalabilité automatique qui s’attache à un équilibreur de charge d’application.

1. Un client demande des données à partir de l’application en se rendant sur le site web de l’entreprise X.
1. Amazon Route 53 utilise la résolution DNS pour identifier l’adresse IP correspondante du site EntrepriseX.com, 192.0.2.0. Ces informations sont renvoyées au client. 
1. La requête du client est envoyée vers l’emplacement périphérique le plus proche via Amazon CloudFront. 
1. Amazon CloudFront se connecte à l’équilibreur de charge d’application qui envoie le paquet entrant à une instance Amazon EC2.

## Contrôle des connaissances

**Quelle déclaration décrit le mieux la résolution DNS ?**


<form>
  <input type="radio" id="q2r1" name="question2" value="1">
  <label for="q2r1">Lancement de ressources dans un réseau virtuel que vous définissez</label><br>
  <input type="radio" id="q2r2" name="question2" value="2">
  <label for="q2r2">Stockage de copies locales de contenu dans des emplacements périphériques à travers le monde</label><br>
  <input type="radio" id="q2r3" name="question2" value="3">
  <label for="q2r3">Connexion d’un VPC à Internet</label><br>
  <input type="radio" id="q2r4" name="question2" value="4">
  <label for="q2r4">Conversion d’un nom de domaine vers une adresse IP</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Conversion d’un nom de domaine vers une adresse IP**.

Par exemple, si vous souhaitez visiter le site web de l’entreprise X, vous saisissez le nom de domaine dans votre PC et cette demande est envoyée à un serveur DNS. Ensuite, le serveur DNS demande au serveur web l’adresse IP qui correspond au site web de l’entreprise X. Le serveur web répond en fournissant l’adresse IP du site web de l’entreprise X, 192.0.2.0.

En savoir plus :

- [Amazon Route 53](https://aws.amazon.com/route53/)
</div>
  
  
# Module 4 : résumé

Dans le module 4, vous avez découvert les concepts suivants :

- Structuration et connexion à un VPC
- Sécurisation des ressources VPC avec des listes de contrôle d’accès réseau et des groupes de sécurité
- Utilisation d’Amazon Route 53 et d’Amazon CloudFront pour diffuser du contenu

```
video : xxx.mkv
```
*Figure. Module summary - Networking*

<div class="collapseContent" display="Transcription" value="true">
  
> ![blaine_02.png](/assets/img/clouds/tempo/blaine_02.png =50x){.align-left}
> La mise en réseau était le domaine exclusif de génies topologiques. Avec AWS, les réseaux sont désormais simplifiés et abstraits pour répondre à la simple question « qui devrait être autorisé à communiquer mutuellement ? » Tant que vous pouvez répondre à cette question, pour pouvez configurer votre réseau sur AWS. 
> 
> Nous avons couvert les bases du VPC, le cloud privé virtuel, la manière dont vous isolez votre charge de travail dans AWS, les principes fondamentaux de sécurité du réseau y compris les passerelles, les ACL réseau, et les groupes de sécurité, toutes les méthodes qui permettent à vos ingénieurs de sécurité de créer un réseau qui autorise un accès sain au trafic tout en supprimant les attaques subversives avant qu’elles n’atteignent votre instance, les façons de se connecter à AWS via VPN et même Direct Connect, les pipelines sécurisées qui sont soit chiffrées sur l’Internet général soit utilisées exclusivement sur fibre par vous et vous seul. 
> 
> Nous avons également parlé des réseaux mondiaux proposés par AWS à l’aide des emplacements périphériques, comment utiliser Route 53 pour DNS, et CloudFront pour mettre du contenu en cache de façon plus proche de vos consommateurs réels. 
> 
> Ce ne sont que quelques-unes des nombreuses utilisations possibles des réseaux AWS mais nous espérons que cela vous aidera à mieux comprendre les différents éléments nécessaires pour prendre un bon départ.
</div>
  
## Ressources supplémentaires

Pour en savoir plus sur les concepts qui ont été explorés dans le module 4, consultez ces ressources.

- [Mise en réseau et diffusion de contenu sur AWS](https://aws.amazon.com/products/networking)
- [Blog sur les réseaux et la diffusion de contenu AWS](https://aws.amazon.com/blogs/networking-and-content-delivery/)
- [Amazon Virtual Private Cloud](https://aws.amazon.com/vpc)
- [Qu’est-ce qu’Amazon VPC ?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [Fonctionnement d’Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html)

# Module 4 : quiz

Testez vos connaissances à l’égard de certains concepts clés de ce module en répondant aux questions de ce quiz.

Après avoir répondu à chaque question, passez en revue les explications détaillées et les liens externes pour renforcer votre compréhension des concepts.

**Votre entreprise dispose d’une application qui utilise des instances Amazon EC2 pour exécuter le site web client et des instances de base de données Amazon RDS pour stocker les informations personnelles des clients. Comment le développeur doit-il configurer le VPC selon les bonnes pratiques ?**

<form>
  <input type="radio" id="q3r1" name="question3" value="1">
  <label for="q3r1">En plaçant les instances Amazon EC2 dans un sous-réseau privé et les instances de base de données Amazon RDS dans un sous-réseau public.</label><br>
  <input type="radio" id="q3r2" name="question3" value="2">
  <label for="q3r2">En plaçant les instances Amazon EC2 dans un sous-réseau public et les instances de base de données Amazon RDS dans un sous-réseau privé.</label><br>
  <input type="radio" id="q3r3" name="question3" value="3">
  <label for="q3r3">En plaçant les instances Amazon EC2 et les instances de base de données Amazon RDS dans un sous-réseau public.</label><br>
  <input type="radio" id="q3r4" name="question3" value="4">
  <label for="q3r4">En plaçant les instances Amazon EC2 et les instances de base de données Amazon RDS dans un sous-réseau privé.</label><br>
</form>


<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **En plaçant les instances Amazon EC2 dans un sous-réseau public et les instances de base de données Amazon RDS dans un sous-réseau privé**.

Un sous-réseau est une section d’un VPC dans laquelle vous pouvez regrouper des ressources en fonction des besoins de sécurité ou opérationnels. Les sous-réseaux peuvent être publics ou privés.

Les sous-réseaux publics contiennent des ressources qui doivent être accessibles au public, telles que le site web d’une boutique en ligne.

Les sous-réseaux privés contiennent des ressources qui ne doivent être accessibles que par l’intermédiaire de votre réseau privé, par exemple une base de données contenant les informations personnelles des clients et l’historique des commandes.

En savoir plus :

- [Amazon VPC](https://aws.amazon.com/vpc)
- [VPC et sous-réseaux](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)
</div>

**Quel composant peut être utilisé pour établir une connexion privée dédiée entre le centre de données de votre entreprise et AWS ?**

<form>
  <input type="radio" id="q4r1" name="question4" value="1">
  <label for="q4r1">Sous-réseau privé</label><br>
  <input type="radio" id="q4r2" name="question4" value="2">
  <label for="q4r2">DNS</label><br>
  <input type="radio" id="q4r3" name="question4" value="3">
  <label for="q4r3">AWS Direct Connect</label><br>
  <input type="radio" id="q4r4" name="question4" value="4">
  <label for="q4r4">Passerelle de réseau privé virtuel</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **AWS Direct Connect**.

Les autres réponses sont incorrectes, car :

- Un sous-réseau privé est une section d’un VPC dans laquelle vous pouvez regrouper des ressources qui ne doivent être accessibles que par l’intermédiaire de votre réseau privé. Bien qu’il soit privé, il n’est pas utilisé pour établir une connexion entre un centre de données et AWS.
- DNS , qui signifie système de noms de domaine, est un répertoire utilisé pour faire correspondre les noms de domaine aux adresses IP.
- Une passerelle réseau privé virtuel vous permet de créer une connexion VPN entre votre VPC et un réseau privé, tel que le centre de données de votre entreprise. Bien que cette connexion soit privée et chiffrée, elle passe par l’Internet public, et non par une connexion dédiée.

En savoir plus :

- [AWS Direct Connect](https://aws.amazon.com/directconnect)
</div>
  
  
**Quelle affirmation décrit le mieux les groupes de sécurité ?**

<form>
  <input type="radio" id="q5r1" name="question5" value="1">
  <label for="q5r1">Ils sont avec état et refusent tout le trafic entrant par défaut.</label><br>
  <input type="radio" id="q5r2" name="question5" value="2">
  <label for="q5r2">Ils sont avec état et autorisent tout le trafic entrant par défaut.</label><br>
  <input type="radio" id="q5r3" name="question5" value="3">
  <label for="q5r3">Ils sont sans état et refusent tout le trafic entrant par défaut.</label><br>
  <input type="radio" id="q5r4" name="question5" value="4">
  <label for="q5r4">Ils sont sans état et autorisent tout le trafic entrant par défaut.</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Les groupes de sécurité sont avec état et refusent tout le trafic entrant par défaut**.

Les groupes de sécurité sont avec état. Cela signifie qu’ils utilisent les modèles et les flux de trafic précédents lors de l’évaluation de nouvelles demandes d’accès à une instance.

Par défaut, les groupes de sécurité refusent tout le trafic entrant, mais vous pouvez ajouter des règles personnalisées pour répondre à vos besoins opérationnels et de sécurité.

En savoir plus :

- [Groupes de sécurité pour votre VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)
</div>


**Quel composant est utilisé pour connecter un VPC à Internet ?**

<form>
  <input type="radio" id="q6r1" name="question6" value="1">
  <label for="q6r1">Sous-réseau public</label><br>
  <input type="radio" id="q6r2" name="question6" value="2">
  <label for="q6r2">Emplacement périphérique</label><br>
  <input type="radio" id="q6r3" name="question6" value="3">
  <label for="q6r3">Groupe de sécurité</label><br>
  <input type="radio" id="q6r4" name="question6" value="4">
  <label for="q6r4">Passerelle Internet</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Passerelle Internet**.

 Les autres réponses sont incorrectes, car :

- Un sous-réseau public est une section d’un VPC qui contient des ressources publiques.
- Un emplacement périphérique est un site qu’Amazon CloudFront utilise pour stocker des copies mises en cache de votre contenu aux fins d’une diffusion plus rapide aux clients.
- Un groupe de sécurité est un pare-feu virtuel qui contrôle le trafic entrant et sortant d’une instance Amazon EC2.

En savoir plus :

- [Passerelles Internet](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
</div>

**Quel service est utilisé pour gérer les registres DNS pour les noms de domaine ?**

<form>
  <input type="radio" id="q7r1" name="question7" value="1">
  <label for="q7r1">Amazon Virtual Private Cloud</label><br>
  <input type="radio" id="q7r2" name="question7" value="2">
  <label for="q7r2">AWS Direct Connect</label><br>
  <input type="radio" id="q7r3" name="question7" value="3">
  <label for="q7r3">Amazon CloudFront</label><br>
  <input type="radio" id="q7r4" name="question7" value="4">
  <label for="q7r4">Amazon Route 53</label><br>
</form>

<div class="collapseContent" display="Solution" value="true">

La bonne réponse est **Amazon Route 53**.

Amazon Route 53 est un service web de DNS. Il offre aux développeurs et aux entreprises un moyen fiable d’acheminer les utilisateurs finaux vers des applications Internet hébergées dans AWS.

Une autre fonction de Route 53 est la possibilité de gérer les registres DNS pour les noms de domaine. Vous pouvez transférer des registres DNS pour des noms de domaine existants gérés par d’autres bureaux d’enregistrement de domaine. Vous pouvez également enregistrer de nouveaux noms de domaine directement dans Route 53.

Les autres réponses sont incorrectes, car :

- Amazon Virtual Private Cloud (Amazon VPC) est un service qui vous permet de mettre en service une section isolée du Cloud AWS. Dans cette section isolée, vous pouvez lancer des ressources dans un réseau virtuel que vous définissez.
- AWS Direct Connect est un service qui vous permet d’établir une connexion privée dédiée entre votre centre de données et votre VPC.  
- Amazon CloudFront est un service de diffusion de contenu. Il utilise un réseau d’emplacements périphériques pour mettre en cache le contenu et le fournir à des clients du monde entier.

En savoir plus :

- [Amazon Route 53](https://aws.amazon.com/route53)
</div>
  
  
Le module suivant étudie les services de stockage et de base de données AWS.