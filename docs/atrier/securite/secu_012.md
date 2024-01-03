---
title: tempo005
description: 
published: true
date: 2023-04-21T10:40:44.080Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:40:39.486Z
---

---
title: Sondes de détection : performances, évaluations et biais
description: 
published: true
date: 2023-02-03T10:44:44.990Z
tags: sonde, tests, méthodologie, performance
editor: markdown
dateCreated: 2023-01-08T10:38:31.678Z
---

---
title: Sondes de détection : performances, évaluations et biais
description: 
published: true
date: 2023-01-27T17:52:30.544Z
tags: sonde, tests, méthodologie, performance
editor: markdown
dateCreated: 2023-01-08T10:38:31.678Z
---

---
title: Sondes de détection : performances, évaluations et biais
description: 
published: true
date: 2023-01-08T10:38:31.678Z
tags: sonde, tests, méthodologie, performance
editor: markdown
dateCreated: 2023-01-08T10:38:31.678Z
---

# Sondes de détection : performances, évaluations et biais

> En avril 2019, l’ANSSI a qualifié les premières sondes pour assurer la supervision de sécurité de réseaux. Les opérateurs d’importance vitale (OIV), les opérateurs de services essentiels (OSE) et, d’une manière générale, les organismes opérant des fonctions sensibles disposent ainsi de produits français de confiance : Cybels Sensor de Thales et Trackwatch Full Edition de Gatewatcher. La méthodologie d’évaluation des sondes n’est, hélas, pas publique. Les ingénieurs sécurité et réseau devant intégrer ces sondes ne disposent donc pas de guides pour effectuer la recette de leur efficacité en production.

Cet article propose un retour d’expérience sur l’évaluation des sondes, notamment sous l’angle de la performance. Cet aspect est, en effet, particulièrement significatif puisque le taux de détection d’une sonde diminue si elle est submergée, quand bien même elle serait équipée des meilleurs signatures et moteurs d’analyse.

## 1. De la notion de budget
Les différents moteurs d’analyse constituant une sonde consomment des ressources machine. Celles-ci sont en quantité finie et dépendante du matériel sur lequel sont installés les mécanismes de détection et d’analyse. Ces ressources comprennent notamment la capacité de calcul (CPU, FPGA, ASIC) et la mémoire (RAM, mémoire de travail des périphériques...).

Il est généralement possible d’effectuer certains compromis calcul/mémoire afin d’optimiser le traitement. Néanmoins, ces ressources confinent, in fine, au temps nécessaire pour traiter un paquet entrant. La variable non ajustable est, en effet, le débit entrant dans une sonde. Ce débit est exprimé en deux unités : le nombre de paquets par seconde, et le nombre d’octets par seconde.

Le nombre de paquets par seconde est significatif, car il définit le budget temporel disponible pour le traitement des paquets. Un million de paquets par seconde signifient que la sonde dispose d’un millionième de seconde pour le traitement de chaque paquet en moyenne. En outre, le nombre de paquets par seconde a une influence sur le nombre d’interruptions (matérielles ou logicielles) qu’un CPU doit traiter. De la même manière qu’un ingénieur est moins productif s’il est constamment interrompu, un CPU perd du temps à chaque interruption. En conséquence, à l’instar de nombreux équipements, il est aisé de noyer une sonde sous de nombreux paquets représentant un trafic extrêmement modéré en octets par seconde.

Le nombre d’octets par seconde reste cependant significatif. Ce dernier a, en effet, un impact sur les temps de transfert des paquets entre les différents composants de la sonde. Cet impact se matérialise notamment pendant les recopies des paquets, entre composants matériels ou logiciels.

Dès lors qu’une sonde épuise son budget pour le traitement d’un paquet, celui des paquets suivants peut en pâtir. S’il ne s’agit que d’un pic de trafic, il est généralement possible d’absorber celui-ci en lissant son coût sur les millisecondes/secondes suivantes. En effet, certains paquets sont traités plus rapidement que d’autres, comme ceux qui sont immédiatement écartés, car faisant partie d’un flux chiffré dont la sonde n’a pas la clé de déchiffrement. Si le pic d’activité se prolonge, la sonde n’a alors d’autre choix que de défausser du trafic. L’endroit et la manière dont le trafic est défaussé sont déterminés en fonction de la localisation du goulot d’étranglement.

## 2. Les goulots d’étranglement
Les paquets reçus peuvent être défaussés par de nombreux composants matériels ou logiciels, dès lors qu’une surconsommation budgétaire survient. Cela peut se produire dès la carte réseau recevant les paquets et ne pouvant pas les diffuser au noyau, ou bien plus tard, quelque part dans le noyau ou dans le logiciel de détection en lui-même.

### 2.1 Pertes par la carte réseau
Les cartes réseau disposent d’une mémoire interne relativement réduite ; il s’agit d’une file d’attente. Chaque paquet entrant dans la carte est inséré dans cette file. Celle-ci est ensuite consommée par un procédé qui transfère les paquets dans la mémoire centrale du serveur. Cela s’effectue par des écritures DMA (Direct Memory Access) ou des variantes, telles qu’IOAT/DMA (I/O Acceleration Technology DMA).

Une carte réseau peut saturer sa file d’attente, si elle n’arrive pas à effectuer les écritures DMA aussi rapidement que les paquets arrivent depuis le réseau. Outre de possibles lenteurs matérielles sur les bus de communication en eux-mêmes, la cause principale de lenteur est le filtrage des accès mémoire par l’IOMMU (I/O Memory Management Unit) [PCIe]. Il s’agit d’un gestionnaire des écritures DMA, capable de limiter les plages mémoires sur lesquelles les périphériques d’un serveur sont en mesure d’écrire, à l’instar d’un pare-feu limitant les accès à un réseau. Sa fonction est cruciale pour la sécurité des serveurs, mais totalement contre-productive si elle résulte en l’incapacité de la sonde à remplir son rôle.

### 2.2 Pertes dans le noyau
Lors de son démarrage, le logiciel de détection fait appel au noyau pour configurer une méthode d’acquisition des paquets. Plusieurs existent, parmi lesquels les plus populaires sont certainement AF_Packet (natif dans Linux), PF_Ring, DPDK, ou encore Netmap.

Généralement, le logiciel de détection configure le noyau afin qu’une plage mémoire soit réservée pour la réception des paquets écrits par DMA.

Plus cette plage mémoire est grande, plus la quantité de paquets qui pourront être stockés en attendant leur traitement est importante. Si toutefois cette plage se remplit plus vite qu’elle n’est consommée par le logiciel de détection, alors des paquets sont défaussés.

La lenteur de la consommation des paquets entrants n’est pas systématiquement à imputer aux lenteurs du logiciel de détection. En effet, le noyau effectue des transformations ou ajoute des métadonnées, qui peuvent être plus ou moins coûteuses. Des pertes peuvent donc se produire avant même que le logiciel de détection ne soit prévenu de la réception de paquets ! Ce cas peut être notamment observé si l’on utilise des programmes XDP (eXpress Data Path) très gourmands, ou des algorithmes peu performants de répartition des paquets (load-balancing) entre les différents processeurs.

Une autre cause de pertes de paquets impliquant le noyau est l’extraction de fichiers. Les logiciels de détection peuvent, en effet, stocker les paquets reçus (fichiers PCAP) ou les fichiers suspects transitant sur le réseau (HTTP, SMB, SMTP...). Si les périphériques de stockage ne sont pas suffisamment rapides, les appels système d’opération sur les fichiers (write(2), sync(2)…) pourraient être plus lents à rendre la main au logiciel de détection, et donc dépasser le budget. En conséquence, une perte de paquets est possible dès lors qu’une grande quantité de fichiers doivent être extraits pour mise en quarantaine et analyses ultérieures.

### 2.3 Pertes dues au logiciel de détection
L’organisation des composants constituant le logiciel de détection et la complexité des règles d’analyse peuvent avoir un impact significatif sur les performances, et entraîner la perte de paquets.

Les moteurs d’analyse sont découpés, en interne, en plusieurs sous-tâches :

- acquérir de nouveaux paquets à traiter ;
- replacer les paquets dans leur contexte (e.g. « fait-il partie d’une session TCP ? », « est-ce un fragment d’un paquet déjà vu ? ») ;
- effectuer des analyses de sécurité des paquets ou des flux, afin d’inférer des événements de sécurité ou d’extraire des fichiers suspects ;
- produire des journaux enregistrant les événements générés.

Ces sous-tâches peuvent être gérées par un ou plusieurs processus. Utiliser plusieurs processus peut être avantageux pour paralléliser les traitements, et ne pas se retrouver avec une surcharge de travail qui pourrait saturer les capacités de traitement d’un unique processus. Hélas, utiliser plusieurs processus n’est pas non plus une panacée, car ces sous-tâches peuvent nécessiter des états partagés. Il est alors nécessaire d’organiser l’accès à ces états, par l’intermédiaire de serveurs mandataires (comme le fait Zeek/Bro) ou de verrous (futex/mutex, comme le fait Suricata).

Une analyse de performance assez poussée du logiciel de détection Suricata a été conduite [SuriLock] ; elle montre que les verrous constituent un des freins principaux, pouvant causer la perte de paquets. Dans cette analyse, il est notamment stipulé que Suricata effectue beaucoup d’accès concurrents aux états des sessions TCP.

Ainsi, un trafic réseau comportant une grande quantité de sessions TCP simultanées ou de nombreuses nouvelles connexions par seconde peut avoir pour conséquence le ralentissement de Suricata. En effet, tous les processus ayant pour objectif de remplacer un paquet dans son contexte seront alors figés en l’attente de la libération des verrous pour accéder aux états partagés.

En outre, et sans même parler de verrous, une grande quantité de sessions TCP peut saturer les tables de hachage dans lesquelles sont stockés les états. Or, la complexité algorithmique d’accès aux éléments des tables de hachage passe d’O(1) (temps constant) à O(n) (temps linéaire, fonction de la quantité de collisions dans la table de hachage) lorsque celles-ci saturent. Il en résulte alors une surconsommation CPU, et donc une surconsommation du budget temporel pour le traitement d’un paquet.

L’architecture logicielle des moteurs d’analyse peut également influencer les performances en cas de mauvaise répartition des paquets reçus entre les différents processus en charge de leur traitement. La perte de paquets survient lorsqu’un CPU est saturé/noyé sous les traitements qu’un processus d’analyse doit entreprendre. Ce cas de figure se manifeste très facilement lorsque la répartition des paquets reçus n’est pas aléatoire, mais tend à concentrer sur le même processus d’analyse tous les paquets relatifs à un même flux (ex. : sessions TCP). Cette répartition par flux est la plus commune et privilégiée, car elle permet de limiter les accès aux ressources partagées et d’augmenter la localité des accès mémoire [SEPTUN]. Le problème est que cette méthodologie ne répartit pas correctement le trafic incluant des tunnels (IPsec, GRE, L2TP, TLS...). En effet, à moins que le programme en charge de la répartition ne fasse de l’inspection profonde (DPI) du trafic (qui n’est même pas toujours possible, par exemple en cas de paquets fragmentés), l’intégralité des paquets d’un tunnel sera analysée par un même processus d’analyse ! Si ce tunnel est très actif, le processus d’analyse sera facilement débordé, et des paquets commenceront à être perdus.

Finalement, les moteurs d’analyse (ex. : dissecteurs, analyseurs de contenu, greffons d’analyse) peuvent être des sources de problèmes de performance, résultant en la perte de paquets. Le pare-feu applicatif de Cloudflare a causé une indisponibilité de l’ensemble de leurs services en juillet 2019 [CloudflareWAF], à cause d’une expression rationnelle gourmande en CPU. De même, une règle de détection mal conçue pourra freiner significativement l’analyse des paquets, et surconsommer le budget.

De l’expérience de l’auteur de cet article, le problème est encore pire avec des greffons écrits en Lua. Ce langage utilise, en effet, exclusivement des co-routines pour simuler le parallélisme. En conséquence, si une instruction Lua met en attente l’interpréteur Lua, sans rendre la main pour que ce dernier exécute une autre co-routine en attendant, l’ensemble des processus d’analyse de paquets ayant recours à un greffon Lua se retrouvent figés [LuaLock].

## 3. Biais introduits par les méthodologies d’évaluation
Les conditions évoquées dans la précédente section de cet article peuvent se manifester lors de la capture de trafic sur des réseaux de production. Néanmoins, étant donné que les sondes sont des composants généralement connectés à des réseaux sensibles, nombre d’opérateurs préfèrent évaluer ces équipements en émulant de tels réseaux, avant d’en sélectionner un et de le raccorder à leur production. De même, les constructeurs de sondes doivent évaluer leur produit afin de s’assurer de leur performance.

L’émulation d’un réseau n’est cependant pas chose aisée, et de nombreux biais peuvent être introduits, faussant positivement ou négativement la perception des performances réelles des sondes ! Cette section détaille quelques erreurs communes que l’auteur de cet article a pu observer ou commettre dans le cadre de son activité professionnelle.

### 3.1 Les outils pour l’émulation de réseaux
Outre les plateformes commerciales, dont l’efficacité et la pertinence des tests pourraient certainement faire l’objet d’études formelles, plusieurs outils libres existent. Parmi ces derniers, il est notamment judicieux de citer :

- tcpdump, qui permet la capture de trafic, le filtrage et le stockage dans des fichiers PCAP [tcpdump] ;
- tcpreplay, qui permet de rejouer des PCAP à plus ou moins grande vitesse, et même d’éditer les PCAP [tcpreplay] ;
- TRex, de Cisco, qui constitue une plateforme de rejeu de trafic complète [TRex] ;
- scapy, une bibliothèque Python dédiée à la manipulation de paquets réseau. Elle permet de capturer du trafic, de l’analyser, de le filtrer et de l’éditer, avant de l’enregistrer sous la forme de fichiers PCAP, ou de renvoyer les paquets sur le réseau [Scapy] ;
- tc qdisc, un ensemble d’outils sous Linux permettant notamment l’émulation de certaines conditions sur un réseau, comme la limitation de débit avec le module tbf, ou la création de latence ou d’instabilités (pertes de paquets, rejeux...) avec le module netem [tc].


#### 3.1.1 Tcpdump
tcpdump permet de capturer et d’enregistrer du trafic sur des réseaux modérément actifs. Lorsque les réseaux sont trop rapides, ce dernier ne parvient pas toujours à collecter tous les paquets reçus. Les flux réseau ainsi enregistrés s’en retrouvent alors corrompus. Quelle que soit la technologie employée pour le rejeu du trafic, il est crucial que les captures réseau employées soient représentatives de la situation que l’on cherche à émuler. En outre, il est normal de souhaiter que le trafic envoyé à la sonde contienne des pertes, des rejeux et des réordonnancements de paquets. Il convient cependant que ceux-ci soient désirés et émulés à dessein, plutôt que le résultat du hasard et d’une méthodologie de capture inadéquate.

Il convient également de noter que la capture de trafic d’un réseau réel peut générer des fichiers PCAP non représentatifs, malgré une source de données bien légitime. En effet, il est absolument capital de nettoyer ces captures, car celles-ci contiennent des demi-flux [CleanPCAP]. Ces demi-flux sont des flux qui ont commencé avant le début de la capture ou qui se termineront après la fin de la capture.

Les demi-flux commencés avant le début de la capture sont problématiques si la sonde est configurée pour ignorer de tels flux. En rejouant ces demi-flux, l’évaluateur néophyte pourrait alors avoir l’impression d’envoyer une quantité importante de trafic à la sonde et que cette dernière se comporte parfaitement, sans perdre le moindre paquet. En réalité, la sonde n’effectuera aucune analyse des paquets reçus et les défaussera, sans lever la moindre alerte, même en présence d’un trafic malveillant.

Les demi-flux ne se terminant pas pendant la capture sont également problématiques si la capture est relativement courte, et rejouée en boucle, par exemple avec tcpreplay. En effet, ces flux ne se terminant jamais, ils vont créer à chaque itération de nouveaux états à maintenir dans la sonde, pouvant ainsi causer une explosion en coût mémoire, et une saturation de différentes tables de hachage internes. Or, si les logiciels de détection sont optimisés pour ne pas s’écrouler face à des DDoS de type SYN Flood, une explosion du nombre de flux tombe dans le spectre des attaques logiques. Les sondes n’ont alors d’autres choix, pour ne pas s’écrouler, que de défausser arbitrairement des flux, parmi lesquels des flux potentiellement malveillants. Pour ce faire, des délais de grâce (timeout) agressifs sont employés.

#### 3.1.2 Tcpreplay
tcpreplay permet de rejouer des PCAP contenant des flux de toute nature et peut être un outil efficace pour évaluer une sonde. Des biais peuvent cependant être introduits dès lors que le PCAP est rejoué en boucle à l’aide de l’option --loop.

Le premier biais constaté par l’auteur de cet article survient si la périodicité des rejeux en boucle est inférieure à la valeur du délai de grâce (timeout) CLOSE_WAIT configuré dans la sonde. Dans l’absolu, CLOSE_WAIT est un état de la machine à états du protocole TCP, qui est indépendant des paquets reçus, et qui évolue seulement après un certain délai. Son objectif est d’empêcher qu’un serveur croie à l’établissement d’une nouvelle connexion TCP à cause de paquets réseau dupliqués trouvant leurs chemins après la fermeture de la session originale. Les sondes devant émuler les piles TCP des serveurs qu’elles protègent, elles doivent avoir un délai de grâce pour l’état CLOSE_WAIT représentatif de ces serveurs. Or, si tcpreplay rejoue en boucle « trop rapidement » (relativement au délai de grâce) le même PCAP, alors le trafic sera partiellement ignoré par la sonde, comme il l’aurait été par le serveur auquel s’adressait le trafic original, en vertu des spécifications du protocole TCP ! Il en résulte une sonde donnant l’impression de traiter énormément de paquets, alors qu’en pratique, elle les ignore.

Pour éviter le biais précédent, il est possible d’utiliser l’option --unique-ip de tcpreplay qui fait varier les adresses IP à chaque nouvelle itération d’un PCAP. Hélas, cette option conduit au second biais !

Ce second biais est une amusante et improbable coïncidence. Il existe, en effet, une interaction entre l’algorithme utilisé par --unique-ip et certaines méthodes de hachage des paquets en vue d’effectuer la répartition des paquets entre les processus d’analyse !

Les méthodes de hachage des paquets pour répartir les flux nécessitent une propriété assez inhabituelle. En effet, il est nécessaire de hacher vers le même processus d’analyse, tant les requêtes que les réponses d’un même flux. Il faut donc utiliser un algorithme dit symétrique, qui hachera les adresses IP de manière identique même si les adresses IP source et destination sont inversées. Or, certains mécanismes de répartition, comme celui de la méthode de capture des paquets PF_Ring, utilisent une simple addition modulaire des octets des adresses IP. Ainsi, un paquet allant de 192.168.0.1 à 192.168.0.2 donnera un haché égal à 192+168+1+192+168+2 = 723 modulo N.

De son côté, l’implémentation de --unique-ip dans tcpreplay se contente de soustraire le nombre d’itérations du PCAP à une adresse IP, et d’ajouter le nombre d’itérations du PCAP à l’autre adresse IP. Or il s‘agit d’une opération mathématique à somme nulle qui conduira donc l’algorithme de répartition des paquets de la sonde à toujours envoyer les paquets vers les mêmes processus d’analyse. Si le PCAP est suffisamment court, alors la sonde se trouvera submergée artificiellement au niveau de certains processus, tandis que tous les autres ne recevront pratiquement aucun flux !

#### 3.1.3 TRex
TRex est une plateforme développée par Cisco permettant le rejeu d’une gamme de PCAP. Chaque PCAP ne doit contenir qu’un seul flux. La configuration de cette plateforme permet ensuite de spécifier la fréquence relative de chaque PCAP par rapport aux autres. Il est alors possible d’envoyer plus ou moins de flux, en sachant que la quantité et la nature des flux envoyés sont maîtrisées.

TRex évite les biais introduits par tcpreplay, en faisant varier les adresses IP de manière aléatoire à chaque nouvel envoi d’un flux. En outre, pour l’expédition des flux, il envoie les paquets d’une adresse A vers une adresse B sur une interface réseau, et les paquets retour, de B vers A sur une autre interface réseau. Cela résulte en une méthode d’acquisition des flux par la sonde qui est plus réaliste, car plus proche de la méthode d’acquisition des flux sur une fibre optique.

La seule ombre au tableau de TRex est sa complexité ; il est, en effet, aisé d’envoyer à une sonde du trafic dépassant l’une de ses spécifications, soit en nombre de paquets par seconde, de nouveaux flux par seconde, de nombre de flux totaux, de fichiers à extraire, ou autre.

#### 3.1.4 Scapy
Scapy est un outil probablement indispensable pour les évaluateurs de sonde. Il leur permet, en effet, de retravailler un fichier PCAP, notamment pour y nettoyer les demi-flux, altérer les flux pour dupliquer des paquets ou en supprimer, corrompre sciemment des sommes de contrôle. Son seul véritable défaut est sa relative lenteur, due principalement à son modèle d’abstraction des paquets, et au langage Python. Il est donc peu pratique d’opérer sur des PCAP de plusieurs gigaoctets.

#### 3.1.5 Tc qdisc
tc qdisc (traffic control -- queue discipline) est un cadriciel reposant sur le noyau Linux et des outils de l’espace utilisateur (userland) pour altérer l’écoulement des paquets sur des interfaces réseau spécifiques. Cet outil est notamment utile lorsqu’un évaluateur cherche à créer des PCAP pour TRex. En effet, il est possible de créer un environnement contrôlé, par exemple avec une paire d’interfaces virtuelles (veth). L’une des interfaces fait alors tourner le serveur et l’autre accueille le client. tc permet d’altérer cet environnement de tests pour y introduire des perturbations volontaires (ex. : délais, pertes, réduction de débit...).

### 3.2 Biais introduits par l’injection de flux non représentatifs
Le comportement d’une sonde peut varier significativement en fonction du trafic reçu. Leurs logiciels de détection doivent, en effet, être en mesure de s’adapter à tout type de trafic susceptible d’être reçu par les équipements qu’elles protègent. Or le traitement d’un grand nombre de petits paquets, ou à l’inverse le traitement de jumbo frames (des trames dont la taille excède les classiques 1514 octets) demande des allocations mémoire contradictoires, ne serait-ce que pour stocker les paquets en attendant leur traitement. De même, de nombreux flux distincts ne se gèrent pas de la même manière que quelques flux bien connus, mais massifs.

Or, une sonde, par défaut, doit savoir gérer toutes ces situations. Leur configuration doit donc être suffisamment générique pour accorder au logiciel la flexibilité nécessaire pour les encaisser. Il en résulte une allocation des ressources potentiellement inadéquates pour des cas extrêmes, comme le traitement d’un trafic réseau à très haut débit. Pire, ces configurations génériques ont tendance à « surbooker » les ressources à disposition. L’hypothèse optimiste est, en effet, que plusieurs situations extrêmes ne se présenteront pas simultanément. Les ressources de la machine étant en quantité finie, il peut alors en résulter des dénis de service du logiciel de détection (ex. : déclenchement de l’OOM Killer de Linux, qui tue les processus consommant beaucoup de mémoire).

Finalement, la nature des flux injectés peut également influencer le comportement de la sonde. Une grande quantité de flux chiffrés est, en général, facile à gérer pour une sonde. Cette dernière n’a, en effet, pas accès aux clés de déchiffrement ; le flux peut donc généralement être ignoré, pour privilégier l’analyse de paquets en clair. À l’inverse, une grande quantité de flux distincts (ex. : sessions TCP ou questions/réponses sur UDP...) peuvent faire exploser les états dans la sonde, comme cela a été détaillé plus haut dans cet article.

## Conclusion
Au fil de cet article ont été présentés différents :

- goulots d’étranglement d’une sonde ;
- ressources critiques ;
- sources de lenteur ;
- biais pouvant être introduits involontairement lors de l’évaluation.

À moins d’avoir développé une expertise notable dans l’intégration de ce genre d’équipements, il est donc nécessaire de se référer à des guides de déploiement, et des méthodologies de tests éprouvées et génériques. Hélas, de tels documents n’existent pas encore, et les ingénieurs doivent réinventer la roue systématiquement, quitte à en produire des carrées, de temps à autre.

Dans le monde des systèmes industriels, souvent considéré par la communauté de la sécurité des systèmes d’information IT comme le vilain petit canard, ce problème a pourtant déjà été résolu ! La certification ANSI/ISA 62443 et le programme ISA-Secure [ISASec] spécifient une méthodologie d’évaluation très précise des systèmes industriels, et des équipements pouvant automatiser ces tests. Les exigences sont donc clairement établies, les méthodologies de tests documentées, et les produits d’évaluation certifiés. Ces tests incluent la conformité, la robustesse au trafic anormal, et la robustesse contre la montée en charge.

Ces éléments manquent cruellement dans le monde des sondes de détection, dont la réaction peut varier significativement, comme décrit dans cet article, en fonction du trafic reçu, de la nature de ses flux et de son intensité.

## Remerciements
Je tiens à remercier mes relecteurs : Erwan Abgrall, Baptiste Bone, Piotr Chmielnicky, Sebastien Larinier, ainsi que ceux qui ont souhaité rester anonymes, et les employés de Gatewatcher. Les avis exprimés dans cet article ne sauraient les engager.

## Références
[CleanPCAP] Script permettant de nettoyer les demi-flux : https://frama.link/RfNczV0d
[CloudflareWAF] Rapport d’incident de Cloudflare impliquant leur pare-feu applicatif web : https://blog.cloudflare.com/cloudflare-outage/
[ISASec] Site de la certification ISA Secure : https://www.isasecure.org/en-US/
[LuaLock] Exemples cassant le modèle de threads collaboratifs de Lua : https://stackoverflow.com/a/18964444
[PCIe] Neugebauer et alii., « Understanding PCIe performance for end host networking », août 2018
[Scapy] Dépôt de l’outil Scapy : https://github.com/secdev/scapy
[SEPTUN] Document sur l’amélioration des performances d’acquisition de Suricata : https://github.com/pevma/SEPTun
[SuriLock] Étude sur l’incidence des verrous dans Suricata : https://xbu.me/article/performance-characterization-of-suricata-thread-models/
[tc] Documentation sur l’émulation d’incidents réseau avec Traffic Control : https://wiki.linuxfoundation.org/networking/netem#packet_duplication
[tcpdump] Site de l’outil Tcpdump : https://www.tcpdump.org/
[tcpreplay] Site de l’outil Tcpreplay : https://tcpreplay.appneta.com/
[Trex] Site de l’outil TRex : https://trex-tgn.cisco.com/


source : https://connect.ed-diamond.com/MISC/misc-106/sondes-de-detection-performances-evaluations-et-biais