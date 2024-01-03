---
title: tempo005
description: 
published: true
date: 2023-04-21T10:39:15.023Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:39:10.866Z
---

---
title: L’empoisonnement de cache DNS : toujours d’actualité ?
description: 
published: true
date: 2023-02-03T10:44:09.055Z
tags: dns, poisoning, cache, dnssec, attaque
editor: markdown
dateCreated: 2023-01-07T16:14:03.228Z
---

---
title: L’empoisonnement de cache DNS : toujours d’actualité ?
description: 
published: true
date: 2023-01-27T17:51:30.737Z
tags: dns, poisoning, cache, dnssec, attaque
editor: markdown
dateCreated: 2023-01-07T16:14:03.228Z
---

---
title: L’empoisonnement de cache DNS : toujours d’actualité ?
description: 
published: true
date: 2023-01-07T16:18:03.806Z
tags: dns, poisoning, cache, dnssec, attaque
editor: markdown
dateCreated: 2023-01-07T16:14:03.228Z
---

# L’empoisonnement de cache DNS : toujours d’actualité ?

> Le Domain Name System (DNS) est l’un des protocoles réseau centraux au bon fonctionnement de l’Internet moderne. Ce protocole permet la résolution de noms « symboliques » – les noms de domaine – en des ressources, notamment des adresses IP. Malgré son omniprésence dans notre quotidien, sa sécurisation a été incrémentale et laborieuse.
{.is-info}

Cet article traite d’une attaque aussi vieille que le DNS, l’empoisonnement de cache, contre laquelle les dernières avancées, comme DNS-over-HTTPS, pourraient permettre de se protéger enfin complètement. Ou le pourront-elles ?

## 1. Rappels sur le fonctionnement nominal du DNS
Le DNS désigne simultanément un protocole de transmission d’informations sur Internet, et une arborescence administrative et technique organisant les données qui sont transmises par ce protocole. Comprendre finement ces deux aspects est nécessaire pour approcher les attaques par empoisonnement de cache, également appelées attaques par pollution de cache.

Les données stockées dans le DNS sont d’ordres multiples : des adresses IP (types A, AAAA...), des informations utiles pour la livraison des courriers électroniques (types MX, TXT...), des politiques de sécurité (types TXT, TLSA…), des clés cryptographiques (types DNSKEY, TLSA…), ou encore des informations techniques nécessaires au bon fonctionnement et à la sécurisation du DNS lui-même (types NS, RRSIG, DS…). Ces données sont rangées sous la forme d’un arbre, dont chaque nœud, chaque intersection, forme un domaine, comme le montre la figure 1.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/mischs-021/arbo_0.jpg">

*Fig. 1 : Illustration de l’arborescence DNS. Chaque intersection constitue un domaine, qui couvre l’ensemble des domaines situés en dessous de lui, et qui sont donc appelés sous-domaines.*

Entre chaque domaine de cette arborescence peut se trouver une séparation, une frontière administrative ou technique, que l’on nomme délégation, représentée par un trait plein dans la figure 1. Cette frontière se manifeste techniquement par des données, stockées sous la forme d’enregistrements DNS de type NS, qui indiquent où trouver les serveurs qui seront responsables du domaine délégué.

L’extrait de console suivant fournit un exemple d’une réponse DNS comportant de tels enregistrements formant une délégation. Dans cet exemple, le domaine john.domaine-fictif.fr est délégué, et deux serveurs font autorité sur ce dernier : srv.autre-domaine.fr, et truc.john.domaine-fictif.fr. Ces informations de délégations sont fournies dans la section Authority de la réponse DNS. Nous verrons ultérieurement le rôle des sections dans les empoisonnements de cache. Comme le nom de domaine truc.john.domaine-fictif.fr est situé à l’intérieur du domaine délégué john.domaine-fictif.fr, une adresse doit être fournie pour ce dernier, dans la section Additional de la réponse DNS de délégation.

```
$ dig -t NS john.domaine-fictif.fr @ns1.domaine-fictif.fr
<snip>
;; QUESTION SECTION:
john.domaine-fictif.fr. IN NS
 
;; AUTHORITY SECTION:
john.domaine-fictif.fr. 172800 IN NS srv.autre-domaine.fr.
john.domaine-fictif.fr. 172800 IN NS truc.john.domaine-fictif.fr.
 
;; ADDITIONAL SECTION:
truc.john.domaine-fictif.fr. 172800 IN A 198.51.100.53
```

Pour comprendre le rapport entre ces sections et les attaques par empoisonnement de cache, il est également nécessaire d’étudier le fonctionnement d’une interrogation DNS. Une interrogation DNS fait intervenir au minimum trois types d’acteurs : le demandeur, le serveur récursif, et les serveurs faisant autorité sur des domaines.

Le demandeur est une application qui a besoin d’une donnée stockée dans le DNS. Ce peut être, par exemple, un navigateur ou un serveur de courriers électroniques, ou tout simplement la commande ping. Ces applications peuvent contenir du code servant à interroger le DNS, ou simplement utiliser les dispositifs mis à disposition par le système d’exploitation : un résolveur primitif (en anglais, stub resolver).

Le serveur récursif, quant à lui, reçoit la requête DNS du demandeur, et interroge, de manière itérative, les serveurs faisant autorité pour obtenir la réponse adéquate. Pour cela, le serveur récursif suit les délégations, comme celles de l’extrait de console précédent. Son fonctionnement est illustré dans la figure 2, où l’on observe avec les étapes 2, 3, et 4, le processus itératif. Comme l’interrogation itérative est relativement coûteuse, en temps, mémoire, et CPU, les serveurs récursifs sont dotés d’une mémoire persistante, dénommée cache, dans laquelle ils stockent les réponses temporairement, afin d’économiser des ressources en cas de réception d’une nouvelle requête DNS identique (étape 6).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/mischs-021/resolve_dns.jpg">

*Fig. 2 : Résolution DNS standard.Une requête est envoyée par un résolveur primitif à un serveur récursif ; celui-ci entreprend le processus de récursion, puis répond au résolveur primitif. Un second résolveur primitif interroge ensuite le même serveur DNS récursif.*

Finalement, les serveurs faisant autorité contiennent les données recherchées. Ils répondent aux requêtes DNS ayant trait à des domaines sur lesquels ils ont reçu l’autorité par d’autres serveurs ayant eux-mêmes autorité sur des domaines « parents ». Seule la racine du DNS n’a pas de domaine parent, et la localisation de ses serveurs doit être spécifiée dans la configuration des serveurs DNS récursifs.

Souvent, l’interrogation du DNS n’est pas aussi triviale que le cas présenté dans la figure 2. En effet, toujours à des fins d’optimisations, parfois également pour des raisons de sécurité, un autre type d’acteurs du DNS peut être employé : les relais. Les relais sont des serveurs qui n’ont pour objectif que de faire suivre les requêtes et les réponses entre plusieurs autres acteurs du DNS (par exemple, entre des résolveurs primitifs et des serveurs récursifs). Généralement, ces serveurs ont également une mémoire persistante (cache), afin de décharger les serveurs récursifs ou d’économiser le transit sur le réseau des requêtes et réponses. Ces relais sont souvent utilisés par le système d’exploitation pour mettre en commun les réponses pour plusieurs applications, ou par les box Internet et autres routeurs SOHO (Small Office, Home Office) pour mettre en commun les réponses pour plusieurs équipements d’un même réseau. La figure 3 illustre un exemple d’interrogation DNS employant des relais.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/mischs-021/relais.jpg">

*Fig. 3 : Résolution DNS avec un relais. Plusieurs applications sollicitent un relais installé directement sur le poste de travail. Ce dernier relaie les requêtes au serveur DNS récursif, qui procède comme dans la figure 2.*

## 2. L’empoisonnement de cache
L’attaque par empoisonnement de cache vise à insérer dans une mémoire persistante (cache) une valeur frauduleuse, qui sera ensuite servie à de multiples demandeurs.

Les conséquences d’une réponse DNS frauduleuse peuvent être très variables : le détournement de connexions vers une adresse IP sous le contrôle de l’attaquant, le contournement de politiques de sécurité, ou encore la fuite d’informations sensibles, comme détaillée dans l’article « SMTP : la killer-app de DNSSEC », publié dans MISC n°97 [SMTP].

Il est intéressant de noter qu’un attaquant peut utiliser les mêmes techniques détaillées ultérieurement dans cet article pour empoisonner juste une réponse pour un demandeur spécifique. Cela est cependant rarement fait, puisqu’étant à la fois potentiellement plus difficile à effectuer, et ayant un impact moins grand, puisqu’une seule application en est victime.

L’empoisonnement de cache peut généralement être exécuté par quatre vecteurs, chacun objet d’une des sections suivantes.

### 2.1 Le cas d’école : l’attaque de l’homme du milieu
Le mode opératoire de l’empoisonnement de cache par l’homme du milieu est simple : l’attaquant se situe en coupure sur le réseau entre le serveur DNS victime et le serveur interrogé. Ce cas peut survenir lorsqu’un routeur est compromis, par exemple. Une variante de cette attaque consiste à manipuler le réseau ou le DNS pour détourner le trafic vers les serveurs de l’attaquant.

Dans ces deux cas, l’homme du milieu peut intercepter toutes les requêtes et réponses DNS. Ainsi, il peut observer les requêtes en transit sur le réseau et fournir des réponses DNS correspondantes contenant les informations frauduleuses de son choix, à la place du serveur interrogé.

Cette attaque est de loin la plus triviale à comprendre, mais sa complexité d’exécution est très variable en fonction du réseau sur lequel elle est mise en œuvre. En effet, sur un réseau local, il est souvent possible de faire des attaques de type ARP poisoning, afin de détourner le trafic local en se faisant passer pour la passerelle du réseau. Sur Internet, en revanche, les attaques ARP (Address Resolution Protocol) ne sont pas possibles. Détourner du trafic reste, bien entendu, possible, notamment grâce à des attaques sur les protocoles de routage dynamiques comme BGP (Border Gateway Protocol). Ces attaques ont cependant le mauvais goût d’être peu discrètes. Cela n’empêche pas des attaquants de la pratiquer, comme en 2018, où le trafic vers les serveurs DNS d’Amazon a été détourné avec une attaque BGP afin de dérober des cryptomonnaies [Dyn]. La NSA a également monté le programme QUANTUMDNS pour effectuer ce genre d’empoisonnement de cache [NSA].

Si l’on omet les autres attaques nécessaires pour se placer en homme du milieu, effectuer un empoisonnement de cache en étant l’homme du milieu est l’affaire d’une dizaine de lignes de Python : intercepter le trafic, inverser le bit de l’en-tête DNS indiquant que ce message DNS est une question ou une réponse, ajouter la réponse frauduleuse au message DNS, et renvoyer le paquet dans « l’autre sens ».

### 2.2 La compromission des serveurs faisant autorité ou de la chaîne d’approvisionnement
L’empoisonnement de cache consiste à faire persister une donnée frauduleuse qui sera redistribuée à des demandeurs qui seront les victimes réelles ; l’empoisonnement n’est pas une fin en soi, mais bien un moyen. En conséquence, un serveur DNS disposant d’une mémoire persistante peut parfaitement recevoir une donnée frauduleuse d’un serveur désigné comme légitime par l’arborescence DNS. Cela peut se produire si ce serveur légitime a été compromis, ou bien lorsque les attaquants mènent une attaque sur la chaîne d’approvisionnement du DNS.

Les attaques sur la chaîne d’approvisionnement du DNS sont très fréquentes. La chaîne d’approvisionnement du DNS est formée, au minimum, par le bureau d’enregistrement auprès duquel est acheté un domaine, et le registre. Les registres sont les organismes qui possèdent les domaines situés juste en dessous de la racine du DNS et qui accréditent les bureaux d’enregistrement pour vendre des noms de domaine. Par exemple, on peut citer l’AFNIC, registre en charge des noms de domaine français, dont le .fr, et OVH ou Gandi pour les bureaux d’enregistrement.

Parmi les attaques sur la chaîne d’approvisionnement, il peut notamment être cité le cas de la compromission des authentifiants permettant de se connecter auprès du bureau d’enregistrement. Une fois authentifié, l’attaquant peut modifier les informations de délégation d’un domaine, et désigner ses propres serveurs comme faisant autorité sur ce domaine. Ces serveurs peuvent alors servir des données frauduleuses qui resteront potentiellement dans la mémoire persistante des serveurs DNS qui les auront interrogés bien après que le propriétaire légitime de ce domaine ait repris le contrôle de son domaine. Un exemple d’une telle attaque sur la chaîne d’approvisionnement peut être le détournement de PayPal et d’eBay, en 2014, par l’intermédiaire d’une attaque du bureau d’enregistrement Mark Monitor [OhHi].

En outre, la nature arborescente du DNS signifie qu’il est possible de servir des données frauduleuses pour un domaine en compromettant n’importe lequel de ses domaines parents. Ainsi, en 2017, Matthew Bryant, un chercheur en sécurité, a détourné une fraction des requêtes pour les domaines se terminant en .io, en identifiant une faiblesse dans la délégation de la racine du DNS vers les serveurs en charge du domaine io [IOFail].

### 2.3 L’exploitation de bugs de logique logicielle
Il est bien entendu que si le serveur ou le logiciel rendant le service DNS sont compromis par un attaquant, ce dernier peut envoyer des réponses de son choix. Cette section de l’article ne s’intéresse cependant pas à ce cas de figure.

Il est ici question du niveau de confiance qu’un serveur DNS récursif a dans les informations reçues dans une réponse DNS. Par exemple, est-ce que le serveur responsable de example.com a le droit de fournir l’adresse IP de www.example.net, dans le cadre d’un « complément de réponse » à une requête qui lui serait envoyée ? Et quid de la réponse illustrée dans l’extrait ci-dessous ? Est-elle acceptable ? Doit-elle être ajoutée à la mémoire persistante (cache) ? Peut-elle remplacer une donnée contraire déjà en mémoire ?

```
$ dig -t A any1.abc.com
<snip>
;; QUESTION SECTION:
any1.abc.com.         IN    A
;; ANSWER SECTION:
any1.abc.com.        IN    A    1.2.3.4
;; AUTHORITY SECTION:
abc.com.    172800    IN    NS    www.abc.com.
;; ADDITIONAL SECTION:
www.abc.com.        172800    IN    A    6.6.6.6
```

La réponse n’est pas simple, car il y a de nombreux facteurs à considérer :

- sur quel(s) domaines(s) le serveur produisant la réponse fait-il autorité ;
- dans quelle section de la réponse DNS l’information a-t-elle été fournie : section réponse (Answer), autorité (Authority) ou complément d’information (Additional) ;
- l’information reçue est-elle signée cryptographiquement, avec les extensions de sécurité du DNS : DNSSEC.

Pendant près de dix ans après la conception du DNS, aucune norme n’a documenté l’arbre de décision permettant de choisir si une donnée est suffisamment digne de confiance pour être stockée en mémoire persistante. C’est seulement avec la RFC 2181, en 1997, que des conseils ont enfin été donnés, après un processus itératif ayant compris essais et erreurs. À ce jour, le serveur DNS BIND implémente sept niveaux de confiance différents pour qualifier une donnée et décider si celle-ci peut être mémorisée, utilisée ou retournée dans le cadre d’une réponse. Les autres serveurs DNS ont implémenté leurs propres variantes. Les réponses DNS n’ont donc pas les mêmes effets en fonction du serveur récursif recevant la réponse.

Cela a notamment été documenté dans l’étude « The Hitchhiker’s Guide to DNS Cache Poisoning » [Hitch], qui compare les implémentations de trois serveurs DNS récursifs (BIND9, Unbound, et MaraDNS), modélise leurs comportements, et utilise un modèle formel pour découvrir des réponses DNS provoquant des empoisonnements de cache. Par exemple, la réponse montrée dans l’extrait de console ci-dessus peut inciter BIND9 et Unbound à enregistrer un serveur DNS faisant autorité additionnel, à une adresse du choix de l’attaquant. Cette réponse ne permettra cependant pas d’écraser l’adresse IP de www.abc.com si celle-ci a déjà été mémorisée dans le cadre d’une réponse où elle figurait dans la section Answer.

Le processus de décision est désormais bien établi dans les implémentations principales. Il reste cependant parfois des bugs, comme la CVE-2009-4022, qui affectait BIND. En outre, les nouvelles implémentations peuvent se faire piéger comme des débutants, comme ce fut le cas pour systemd-resolved, en 2014 [SysD].

### 2.4 L’attaque à l’aveugle
L’attaque à l’aveugle est la plus terrifiante des attaques par empoisonnement de cache ; et pour cause, l’attaquant peut la mener depuis virtuellement n’importe où sur le réseau, y compris parfois Internet dans sa globalité ! Pour cette attaque, il n’est pas nécessaire d’intercepter le trafic DNS de la victime, d’où son nom d’attaque à l’aveugle. L’attaquant se « contente » d’envoyer une réponse (probablement plutôt des millions) à une requête dont il ignore tout, et joue les probabilités afin que sa réponse (l’une d’entre-elles, en tout cas) soit acceptée par le serveur DNS récursif victime comme étant la réponse légitime. Ce dernier met alors à jour sa mémoire persistante avec les informations frauduleuses.

Cette notion de probabilité de succès tient au fait que pour qu’une réponse soit acceptée par un serveur DNS récursif, plusieurs critères doivent être réunis :

- l’identifiant de transaction DNS (QXID), un champ aléatoire présent dans l’en-tête des réponses DNS doit être identique à celui envoyé dans la requête ;
- la réponse doit être reçue (c’est-à-dire qu’elle doit être envoyée à la bonne adresse IP et au bon port) ;
- la réponse doit (sembler) provenir du serveur interrogé (c’est-à-dire que la réponse a été envoyée à partir de l’adresse IP et du numéro de port auxquels la requête a été envoyée).

À cela, il faut également ajouter les éventuelles informations spécifiques au protocole de transport employé : UDP, TCP, D-TLS [DoD], TLS [DoT] ou HTTP/2 [DoH]. Le DNS peut les utiliser tous, même si UDP est actuellement généralement privilégié, pour des raisons de performance. En UDP, aucune information supplémentaire ne doit être devinée par un attaquant tentant de créer une réponse DNS frauduleuse. HTTP/2 repose (dans les faits) sur TLS, qui lui-même transite sur TCP. Chacun de ces protocoles ajoute des informations inconnues de l’attaquant, à commencer par le numéro de séquence TCP (32 bits pseudo-aléatoires). L’attaquant doit devenir ce dernier, car il usurpe l’adresse IP du serveur DNS interrogé et il ne reçoit donc pas le segment TCP SYN/ACK contenant le numéro de séquence du serveur.

D-TLS, quant à lui, introduit, entre autres, des éléments cryptographiques très difficiles à deviner par un attaquant.

En somme, si l’on considère le protocole de transport par défaut, UDP, le nombre de bits inconnus d’un attaquant essayant de fabriquer une réponse frauduleuse est :

- QXID au maximum 16 bits ;
- l’adresse IP du résolveur DNS : certaines infrastructures DNS utilisent plusieurs adresses IP émettrices de requêtes vers les serveurs faisant autorité, ce qui ajoute une incertitude de l’ordre de 0 à 3 ou 4 bits dans les cas extrêmes ;
- l’adresse IP du serveur DNS interrogé : la plupart des domaines disposent d’au moins deux serveurs DNS faisant autorité, soit au moins 1 bit d’incertitude ;
- le port sur lequel la réponse doit être envoyée : si ce dernier est choisi aléatoirement, selon les recommandations [DJB, Res], au maximum 16 bits.

Au total, l’attaquant qui tente de fabriquer une réponse DNS frauduleuse doit donc deviner, dans le cas le plus défavorable pour lui, 16 + 16 + 4 + 1 = 37 bits. Une autre manière de considérer ce résultat est de dire qu’une réponse DNS frauduleuse d’un attaquant à l’aveugle a une probabilité d’au mieux 2-37 (environ une chance sur 137 milliards) d’être acceptée par un serveur DNS récursif.

L’attaquant n’est cependant pas contraint d’envoyer une unique réponse frauduleuse. En effet, le serveur DNS récursif est en attente d’une réponse valide. Il rejettera généralement silencieusement les tentatives infructueuses de l’attaquant, et attendra patiemment une réponse correcte. Cette attente dure le temps que le serveur réellement interrogé réponde. Pendant ce laps de temps, l’attaquant peut donc envoyer autant de tentatives de réponses frauduleuses que la victime est capable de recevoir et de traiter. Une réponse DNS de taille moyenne pouvant faire dans les 100 octets, l’attaquant peut envoyer 125000 réponses frauduleuses par seconde vers une victime capable de traiter 100 Mb/s. En admettant un délai raisonnable de réponse du serveur légitime autour des 100 millisecondes, l’attaquant a donc le temps d’envoyer 12500 réponses frauduleuses. En conséquence, une tentative d’empoisonnement de cache d’une victime capable de gérer 100 Mb/s de trafic, avec un serveur légitime mettant 100 millisecondes à répondre, a une probabilité de succès d’autour de log(12500)/log(2) * 2-37, soit environ 2-23 ou approximativement une chance sur huit millions. Il existe, en outre, des techniques obscures pouvant permettre d’améliorer encore cette probabilité [Bloc, Frag] (note : le calcul ci-dessus est faux, mais constitue une approximation compréhensible acceptable).

Une chance sur 137 milliards peut paraître être un risque négligeable. C’est en particulier le cas si l’on considère qu’en cas d’échec, l’attaquant doit attendre que l’information envoyée par le serveur DNS légitime expire de la mémoire persistante du serveur récursif ciblé. Pendant cette période d’expiration, toutes les requêtes sont, en effet, répondues avec l’information mémorisée ; l’attaquant ne peut donc plus tenter sa chance, car le serveur DNS récursif n’émet plus de requête.

Le risque n’a cependant pas toujours été aussi faible. En 2008, le chercheur en sécurité Dan Kaminsky a publié une attaque permettant un empoisonnement de cache à l’aveugle pouvant s’effectuer en quelques minutes, voire quelques secondes suivant les « optimisations » appliquées. À l’époque, les serveurs DNS récursifs n’utilisaient pas systématiquement un port source aléatoire. La probabilité qu’une réponse frauduleuse puisse être acceptée pouvait donc être aussi basse que 2-16, soit une chance sur 65536. Le seul élément inconnu était l’identifiant de transaction (QXID) ! En outre, Kaminsky a abusé de la mécanique permettant aux serveurs DNS de juger de la confiance qu’ils peuvent porter dans les informations contenues dans une réponse DNS. Ainsi, son attaque consistait à faire des tentatives d’empoisonnement de cache, non pas sur des domaines existants, mais sur des domaines inexistants aux noms aléatoires. Son but n’était pas de compromettre ces domaines inexistants, mais de fournir des informations complémentaires écrasant d’autres données déjà présentes dans la mémoire persistante du serveur DNS récursif victime. De surcroît, le génie de cette attaque était d’utiliser des domaines inexistants, qui sont quasiment innombrables. Grâce à ces noms aléatoires, en cas de tentative infructueuse, au lieu d’attendre que le serveur DNS récursif vide sa mémoire persistante ou invalide la réponse légitime après un certain délai, une nouvelle tentative pouvait être immédiatement effectuée par l’attaquant, avec un nouveau nom aléatoire.

## 3. Défenses
Le DNS a été conçu à la fin des années 80. Ce vénérable protocole a connu de nombreux changements, et moult augmentations. Pendant longtemps, les seules améliorations de sa sécurité ont été des rustines visant à rendre de plus en plus difficiles les attaques à l’aveugle et compenser l’absence de lignes directrices pour les niveaux de confiance à porter dans les données reçues.

C’est ainsi que la RFC 5452, publiée en 2009, documente, après la débâcle causée par l’attaque de Dan Kaminksy, que le port source devrait être aléatoire pour plus de sécurité.

La technique connue sous le nom de 0x20 a également vu le jour. 0x20 est la distance, dans la table ASCII, entre une lettre majuscule et la lettre minuscule correspondant : A (65) et a (97) donnent 97 - 65 = 32 = 0x20 en hexadécimal. Son principe est de changer aléatoirement la casse des lettres d’un nom de domaine dans une requête DNS, et d’escompter voir les mêmes variations de casse dans la réponse DNS. Si la réponse ne contient pas les bonnes majuscules, alors la réponse est rejetée, même si tous les autres paramètres sont bons. Un attaquant à l’aveugle ne peut connaître les lettres qui ont été mises en majuscule ; il doit donc les deviner, en plus de tout le reste.

Ces astuces ne protègent cependant pas contre les autres attaques par empoisonnement de cache évoquées dans la section précédente de cet article. Ainsi, au milieu des années 90, l’IETF (Internet Engineering Task Force), l’organisme qui conçoit les standards de fait de l’Internet, a donc commencé à concevoir des extensions DNS pour la sécurité : DNSSEC. Il a fallu à la communauté plus de dix ans pour arriver à une mouture convenable. Finalement, en 2010, son déploiement global fut rendu possible.

Le principe de DNSSEC a déjà été détaillé de nombreuses fois dans MISC, ainsi que dans un document publié par l’Afnic [Afnic]. Néanmoins, il convient ici d’en présenter les grandes lignes, étant donné qu’il s’agit de l’unique contre-mesure efficace contre l’empoisonnement de cache, à ce jour. DNSSEC permet de protéger l’intégrité des informations contenues dans le DNS, à l’aide de signatures cryptographiques. Ces signatures sont créées avec des clés asymétriques : une clé privée est utilisée pour créer les signatures, et la clé publique associée à la clé privée permet de vérifier ces signatures.

La personne ou le serveur en charge d’un domaine signe donc, avec une clé privée, les informations contenues dans ce domaine (plus spécifiquement, dans sa zone… mais la différence subtile de terminologie peut être ignorée ici). La « magie » mathématique de la cryptographie asymétrique permet à tout acteur DNS de valider l’intégrité cryptographique des données ayant trait à ce domaine. Pour cela, la clé publique est employée. Les signatures et les clés publiques nécessaires à leur vérification sont publiées dans l’arborescence DNS elle-même. À chaque délégation d’un sous-domaine, le domaine parent signe une information permettant de vérifier l’authenticité de la clé qui signe le sous-domaine. Il se crée ainsi une chaîne de clés-signatures, permettant de vérifier toutes les informations du DNS… pour peu que les domaines soient signés.

Les validateurs des signatures DNS sont généralement les acteurs de l’infrastructure DNS qui maintiennent une mémoire persistante : les serveurs DNS récursifs et les relais. Une fois les données vérifiées cryptographiquement, ces serveurs peuvent les stocker, en toute sécurité.

Lorsqu’un résolveur primitif interroge un serveur DNS récursif validant DNSSEC, ce dernier répondra en fournissant les informations depuis sa mémoire, et en mettant un bit à 1 (AD : Authentic Data) pour signaler qu’en ce qui le concerne, ces informations ont été déterminées comme authentiques.

Paradoxalement, et de façon parfaitement incompréhensible, les concepteurs de DNSSEC n’ont pas jugé utile de protéger en intégrité le bit AD… En conséquence, si un relais maintenant une mémoire persistante ne fait pas lui-même la validation DNSSEC, son cache peut être empoisonné par un attaquant, quand bien même le domaine est signé avec DNSSEC ! Il peut être jugé que sur un réseau local maîtrisé, comme dans un réseau d’entreprise, la probabilité d’une telle attaque soit faible. Cependant, il est difficile de tenir le même argument lorsque le serveur DNS récursif validant DNSSEC est celui de résolveurs publics, comme ceux de Google, Cloudflare ou OpenDNS ; en effet, les réponses de ces serveurs transitent sur l’Internet sans protection !

Plusieurs propositions ont été avancées pour combler cette absence.

Le protocole DNSCrypt [Crypt] figure parmi les premières propositions viables. Hélas, malgré ses nombreux mérites et son support par OpenDNS dès 2011, son déploiement reste faible à ce jour.

De son côté, la communauté DNS a, des années après la publication de DNSCrypt, mis au monde DNS-over-TLS (DoT) en 2018. Ses atouts sur son prédécesseur sont au mieux douteux, et son adoption est également anecdotique à l’aube de la nouvelle décennie.

Finalement, toujours en 2018, ce qui aurait pu sembler être un trait d’humour il y a dix ans est devenu réalité : DNS-over-HTTPS a été spécifié. Encombré par HTTP/2, sans en tirer le moindre avantage, ce protocole plaque les messages DNS dans la pile protocolaire « DNS sur HTTP/1.1 sur HTTP/2 sur TLS sur TCP ». Ironiquement, contre toute attente et contrairement à la raison la plus primaire, c’est ce protocole, digne de l’œuvre du docteur Frankenstein, qui semble être destiné à être largement adopté. Ce dernier est, en effet, largement plébiscité par les navigateurs, déjà habitués à traiter avec cette pile protocolaire absurde, et dont les intérêts sont tout autre que la protection de l’intégrité du DNS et de la vie privée. Néanmoins, force est de constater que grâce aux navigateurs, le bit AD aura peut-être enfin une chance d’être protégé lors de son transit sur le réseau, grâce à TLS, et les attaques par empoisonnement de cache deviendront réellement impossibles grâce à DNSSEC.

## Conclusion
Dans cet article, nous avons étudié les différents types d’attaques par empoisonnement de cache, au sens large. Nous avons ainsi pu observer, au travers d’exemples, que ces attaques sont toujours d’actualité, à l’aube de la nouvelle décennie, et plus de 30 ans après la conception du protocole DNS. Enfin, nous avons fait un point sur les techniques de défense disponibles, et celles en cours de déploiement.

Disposer d’une boîte à outils complète pour contrer ces attaques est un luxe dont nous pouvons/allons pouvoir enfin jouir. Hélas, le taux d’adoption de DNSSEC est encore très faible, notamment pour les noms de domaine en .fr. En 2017, l’ANSSI relevait que 10% des noms en .fr étaient signés, et que l’adoption volontaire était anecdotique (1% des domaines qui existaient déjà l’année précédente) [Obs].

En conséquence, étudier les empoisonnements de cache est encore à ce jour pertinent, et cela devrait, hélas, continuer de l’être pour encore de nombreuses années.

## Remerciements
Je tiens à remercier Benjamin Cohen et Piotr Chmielnicki pour leurs suggestions d’amélioration de cet article. Les opinions exprimées dans cet article ne sauraient les engager.

## Références
[Afnic] Dossier DNSSEC de l’Afnic : https://www.afnic.fr/data/divers/public/afnic-dossier-dnssec-2010-09.pdf
[Bloc] Blocking DNS messages is Dangerous : https://www.ssi.gouv.fr/uploads/IMG/pdf/DNS-OARC-2013-Blocking_DNS_Messages_Is_Dangerous.pdf
[Crypt] Site du projet DNSCrypt : https://dnscrypt.info/
[Frag] Fragments Considered Poisonous : https://u.cs.biu.ac.il/~herzbea/security/13-03-frag.pdf
[DJB] DNS forgery : http://cr.yp.to/djbdns/forgery.html
[DoH] DNS Queries over HTTPS (DoH) : https://www.rfc-editor.org/rfc/rfc8484.txt
[DoT] Specification for DNS over Transport Layer Security (TLS) : https://www.rfc-editor.org/rfc/rfc7858.txt
[DoD] DNS over Datagram Transport Layer Security (DTLS) : https://www.rfc-editor.org/rfc/rfc8094.txt
[Dyn] Article sur l’attaque BGP pour détourner les serveurs DNS d’Amazon : https://dyn.com/blog/bgp-hijack-of-amazon-dns-to-steal-crypto-currency/
[Hitch] The Hitchhiker’s Guide to DNS Cache Poisoning : https://www.cs.cornell.edu/~shmat/shmat_securecomm10.pdf
[OhHi] Article sur l’attaque via Mark Monitor : https://mashable.com/2014/02/01/syrian-electronic-army-ebay/
[IOFail] Blog de Matthew Bryant : https://thehackerblog.com/the-io-error-taking-control-of-all-io-domains-with-a-targeted-registration/
[NSA] Quantum DNS : https://arstechnica.com/information-technology/2015/01/nsa-secretly-hijacked-existing-malware-to-spy-on-n-korea-others/
[Obs] Rapport de l’observatoire de la résilience de l’Internet français : https://ssi.gouv.fr/observatoire
[Res] Measures for Making DNS More Resilient against Forged Answers : https://www.rfc-editor.org/rfc/rfc5452.txt
[RFCs] DNS related RFCs : https://www.statdns.com/rfc/
[SMTP] SMTP la killer-app de DNSSEC : https://connect.ed-diamond.com/MISC/MISC-097/SMTP-la-killer-app-de-DNSSEC
[SysD] Systemd-resolved DNS cache poisoning https://seclists.org/oss-sec/2014/q4/592
  
source : https://connect.ed-diamond.com/MISC/mischs-021/l-empoisonnement-de-cache-dns-toujours-d-actualite

