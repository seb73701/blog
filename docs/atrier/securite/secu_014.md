---
title: tempo007
description: 
published: true
date: 2023-04-21T10:40:56.202Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:40:52.068Z
---

---
title: Traçage Wi-Fi : qu’en est-il en pratique ?
description: 
published: true
date: 2023-02-03T10:45:00.212Z
tags: wifi
editor: markdown
dateCreated: 2023-01-08T11:00:56.767Z
---

---
title: Traçage Wi-Fi : qu’en est-il en pratique ?
description: 
published: true
date: 2023-01-27T17:52:39.429Z
tags: wifi
editor: markdown
dateCreated: 2023-01-08T11:00:56.767Z
---

---
title: Traçage Wi-Fi : qu’en est-il en pratique ?
description: 
published: true
date: 2023-01-08T11:00:56.767Z
tags: wifi
editor: markdown
dateCreated: 2023-01-08T11:00:56.767Z
---

# Traçage Wi-Fi : qu’en est-il en pratique ?

> Les smartphones sont parfois désignés comme des « mouchards de poche », leurs risques pour la vie privée étant dénoncés. Les problèmes de vie privée qu’ils apportent sont en effet nombreux. Dans cet article, nous allons nous attarder sur un problème particulier : le traçage, et son utilisation dans le monde réel.

Depuis quelques années, le nombre d’appareils possédant une interface Wi-Fi ou Bluetooth a explosé. Ceux-ci émettent en permanence des trames permettant de repérer les réseaux présents, ce qui constitue un problème pour la vie privée de leur possesseur. En effet, ces trames incluent un champ contenant l’adresse MAC de la carte réseau de l’appareil, qui constitue un identifiant unique. Par conséquent, un attaquant passif peut connaître la présence d’un appareil et sa mobilité (présence au cours du temps). Ce problème de conception est exploité par différents acteurs pour effectuer du traçage physique (physical tracking). Ce terme désigne une technologie utilisant des capteurs pour collecter des informations concernant la présence d’appareils dans un lieu. Le Wi-Fi étant la technologie sans fil la plus répandue et la plus simple à sniffer, c’est la cible principale des systèmes de traçage, mais le Bluetooth est aussi largement utilisé.

Mettre en place un système de traçage est très simple, comme nous le verrons plus bas. Quelques capteurs à bas prix suffisent pour mettre en place un système d’écoute passive, puisque la plupart des appareils annoncent leur présence sans nécessité de les solliciter activement. La figure 1 schématise un tel système de traçage.

La présence de ces systèmes explose à travers le monde. Beaucoup d’entreprises ont vu la prolifération récente des appareils Wi-Fi et Bluetooth comme une opportunité pour récolter des statistiques sur des clients, des véhicules ou des populations. Dans cet article, nous allons nous attarder sur ces systèmes. Quels sont leur régulation, leur acceptation par les populations, leur impact sur la vie privée ? Qu’en est-il du consentement ? Quels exemples trouvons-nous de par le monde ?

<img src="https://connect.ed-diamond.com/sites/default/files/articles/gnu-linux-magazine/glmfhs-099/83893/Wi-Fi_tracking.jpg">

*Fig. 1 : Un système de traçage Wi-Fi.*

## 1. Pourquoi le traçage Wi-Fi est-il possible ?
Mais pourquoi les appareils annoncent-ils si bêtement leur présence ? Cela vient du fonctionnement du mécanisme de découverte de service, permettant de connaître la liste des réseaux présents dans un lieu. Deux modes existent pour réaliser cette investigation (voir figure 2) :

- Dans le mode passif, l’appareil attend de recevoir des informations des points d’accès alentours, sous la forme de trames nommées beacons.
- Dans le mode actif, l’appareil envoie régulièrement des trames nommées probe requests, et attend la réponse des points d’accès (probe response).

Le mode actif est plus utilisé, car il permet un gain de vitesse et une économie d’énergie (cela évite d’écouter la ligne en permanence).

En Bluetooth, le traçage est encore plus trivial puisque les appareils annoncent volontairement leur présence, pour permettre l’appariement.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/gnu-linux-magazine/glmfhs-099/83893/actif_passif.jpg">

*Fig. 2 : Les deux modes de découverte de service en Wi-Fi.*

### 1.1 Le traçage Wi-Fi en pratique sous Linux
Pour comprendre à quel point le traçage est facile à effectuer, voyons comment le mettre en pratique sous Linux, avec le cas du Wi-Fi.

#### 1.1.1 Simple écoute
Repérer les appareils autour de soi est de nos jours extrêmement simple. Il suffit de posséder une carte Wi-Fi supportant le mode monitor, qui permet à la carte de garder toutes les trames qu’elle reçoit au lieu de rejeter celles dont l’adresse de destination ne la concerne pas. Or, le driver madwifi prend en charge ce mode sur une grande partie des cartes modernes (pour peu que leur firmware le permette). Si la carte interne d’un ordinateur portable ne supporte pas le mode monitor, des dongles USB feront l’affaire pour une dizaine d’euros. À noter qu’il est difficile, voire impossible, d’activer le mode monitor sur les smartphones Android.

Pour de nombreuses cartes, il n’y aura donc aucune installation complexe à effectuer, l’interface concernée (ici wlan0) pouvant être mise en mode monitor avec la commande suivante :

```
$ sudo ifconfig wlan0 down && sudo iwconfig wlan0 mode monitor && sudo ifconfig wlan0 up
```

Les probe requests peuvent alors être captées avec Wireshark, ou une simple commande tcpdump :

```
$ sudo tcpdump -i wlan0 'wlan type mgt subtype probe-req'
```

#### 1.1.2 Traçage avancé
Des logiciels libres existent pour effectuer la collecte et la visualisation des données récoltées, comme Wombat [1] et Probr [2]. Le premier est un système que nous avons développé afin d’avoir un outil permettant de faire des démos de sensibilisation, et de tester de nouveaux concepts visant à protéger la vie privée dans un tel système (par exemple, un mécanisme simple de sortie de système (opt-out)). Le second possède une interface de visualisation avancée. Citons encore Snoopy [3].

On n’oubliera pas que la mise en place d’un système de collecte de données personnelles, même s’il est techniquement simple, est règlementé (cf. section « régulation »).

## 2. Évolution de l’usage sans fil
Le traçage Wi-Fi, bien que sa possibilité soit connue depuis plus de 10 ans, n’est devenu un véritable problème que depuis quelques années, avec l’explosion du nombre d’appareils incluant des interfaces Wi-Fi. Des études aux États-Unis montrent une progression du nombre d’adultes possédant un smartphone de 35% en 2011 à 77% en 2017 [4][5]. De la même manière, le taux de possession de tablettes va de 3% en 2010 à 51% en 2016. On retrouve une tendance similaire en France, avec un taux de possession de smartphones allant de 29% en 2012 pour monter jusque 65% en 2016 [6]. Une autre étude en France recensait en 2016 que 77% des adultes de 18 à 75 ans déclaraient posséder un smartphone [4]. Bien sûr, les smartphones ne sont pas les seules cibles possibles du traçage. Les objets connectés (montres connectées, bracelets fitness...) constituent une cible supplémentaire.

### 2.1 Possibilités de détection
Il convient cependant de se demander à quel point ces appareils sont des bonnes cibles pour le traçage. Pour être détectables, les appareils doivent respecter plusieurs conditions : être transportés par leur possesseur, allumés, posséder une interface Wi-Fi active, et envoyer activement des trames. Une étude américaine montre que 94% des possesseurs de smartphone l’ont fréquemment sur eux, et que 82% ne l’éteignent rarement ou jamais [7]. Ceci donne des approximations pour les deux premières conditions. Les deux dernières sont plus difficiles à estimer : à notre connaissance, aucune étude ne s’y est attardée.

On peut cependant évaluer une mesure proche : le ratio entre le nombre de personnes présentes et le nombre d’appareils détectés. Les études effectuées donnent des résultats variables en fonction des foules, ce ratio pouvant varier de façon importante, allant de 0.44 à 3.75 pour une étude sur des conférences [8]. Sur des plus grandes foules (villes occidentales), ce ratio se rapproche de 1 [9][10], le fait que des personnes ne possèdent pas d’appareil détectable étant visiblement compensé par le fait que d’autres en possèdent plusieurs. Ce résultat ne garantit cependant pas que tous les appareils soient détectés.

De plus, il ne faut pas oublier que le taux de possession de smartphone dans une population est soumis à de nombreux biais : pays ciblé, âge, catégorie socioprofessionnelle, etc. Les taux de personnes détectées dans deux foules variant sur ces aspects pourront présenter de grandes différences.

## 3. Champs d’application
Lister des installations présentes de par le monde donne une idée des champs d’application possibles de cette technologie. Ces exemples permettent aussi d’étudier les autres aspects détaillés plus loin dans cet article (gestion de la vie privée, du consentement, etc.). Cette liste, non exhaustive, a été dressée au cours de l’été 2017.

Analyse du trafic routier :

- À Houston, un système de traçage Bluetooth fournit des informations sur le trafic au public [11].
- Un système Wi-Fi et Bluetooth est installé sur une route de la grande périphérie de Lyon (voie rapide de contournement Est de Lyon N346) [12].

Un système de détection du trafic basé sur le Bluetooth est installé dans le Maryland [13].
Analyse de fréquentation dans des lieux commerciaux :

- En mai 2017, CB Insight a listé plusieurs dizaines de startups travaillant sur l’analyse de mobilité dans les magasins [14] ;
- Dans des centres commerciaux : à la Défense [15] et à Hamar [16] (Norvège).
- Aux États-Unis, une entreprise nommée Nomi a vendu des systèmes de traçage Wi-Fi à approximativement 45 clients en 2013. Certains d’eux ont déployé ces systèmes en plusieurs lieux. D’après la Federal Trade Commission (FTC), ces installations ont collecté des informations sur pas moins de 9 millions de personnes entre janvier et septembre 2013 [17].
- En plus des magasins, le traçage est utilisé dans d’autres lieux fermés. Citons par exemple deux musées à Londres [10] et les aéroports d’Amsterdam [18] et d’Oslo (pour estimer la longueur des files d’attente pour les fouilles pour ce dernier) [16].

Analyse de foules ou de populations :

- Commuting : des systèmes temporaires visant à analyser les déplacements des personnes se rendant sur leur lieu de travail ont été installés dans le métro de Londres [10], et à New York [8] (pendant respectivement 1 et 6 mois) ;
- En France, plusieurs villes (Rennes et Niort) ont installé des systèmes de traçage couvrant un large espace commercial (centre-ville) [19][20].
- En 2011, le gouvernement chinois a annoncé la mise en place d’un système de traçage à Beijing ciblant pas moins de 17 millions de téléphones [21]. La technologie utilisée n’est pas précisée.
- Une entreprise basée à Singapour développe une technologie de traçage à base de drones, dans le but de créer des profils utilisateur pour du ciblage marketing. En 2015, l’entreprise se targuait de posséder pas moins de 530 millions de profils utilisateurs [22].

Surveillance : diverses preuves tendent à montrer que le traçage serait utilisé par des agences étatiques dans des buts allant de la recherche de téléphones volés [23] à la traque de journalistes [24] ou d’ennemis en situation de guerre [25]. En particulier, la NSA est suspectée de tracer les téléphones portables utilisant de nombreuses méthodes, notamment des modules Wi-Fi montés sur des drones pour surveiller des villes entières. L’agence est même suspectée d’utiliser cette méthode pour traquer des cibles pour planifier les assassinats par attaques de drone américains [26].

## 4. Problèmes de vie privée dans les installations
En tant que systèmes d’information manipulant des données personnelles, on pourrait s’attendre à ce que les systèmes de traçage abordent sérieusement les aspects de protection de la vie privée. En pratique, ce n’est pas le cas.

Les systèmes de traçage Wi-Fi sont souvent présentés comme meilleurs en termes de vie privée que d’autres systèmes équivalents, comme la reconnaissance faciale ou la lecture automatique de plaques d’immatriculation, grâce au chiffrement des données avant stockage. Cependant, ce chiffrement est souvent faible, n’allant pas plus loin qu’un simple hachage de l’adresse MAC, sans sel [17]. Cette approche est inefficace : l’espace d’adressage des adresses MAC possibles et allouées étant très réduit, l’adresse originale peut être retrouvée par force brute en quelques secondes sur un GPU moderne. Un hachage avec sel n’est pas forcément plus efficace, puisqu’un attaquant réussissant à compromettre un système aura de grandes chances de mettre la main sur le sel, stocké au même endroit que les données de traçage [27]. Cette méthode est pourtant presque toujours présentée comme une « anonymisation » des données, alors qu’une véritable anonymisation ne permettrait en aucun cas de retrouver les données originales.

Par l’étude des systèmes existants, il apparaît également que les données sont souvent stockées pour de longues périodes temporelles. Les durées de conservation sont parfois allongées pour faciliter la maintenance, ou pour étendre le domaine d’application originalement prévu. Par exemple, il est admis dans un rapport concernant le système du Maryland que les données sont conservées indéfiniment, alors qu’il était prévu à l’origine une période de rétention maximale d’une semaine [12]. Les capteurs de l’installation de Lyon conservent les données brutes plus d’un mois [13]. À la Défense, les données sont conservées 6 mois [15].

Il apparaît donc que les garanties pour la vie privée sont souvent vues comme des limites pour la mise en place de ces systèmes. Compliquées à assurer, elles limitent l’utilité du système pour les publicitaires, qui aimeraient pouvoir collecter le plus d’informations possibles. Par exemple, pouvoir calculer le taux de revisite d’un lieu implique de conserver les données sur une longue période temporelle, avec au mieux un chiffrement inefficace. On retrouve parfois dans les descriptions des systèmes une incohérence entre l’anonymisation prétendue des données et la capacité annoncée de calculer le taux de revisite, par exemple dans l’installation de Niort [20].

En général, les politiques de confidentialité (privacy policies) des entreprises vendant des systèmes de traçage Wi-Fi sont insuffisantes. Une étude de 2014 trouvait, pour une quinzaine d’entreprises, un mélange entre : de longues périodes de rétention des données, un stockage des données sensibles déléguées à des tiers, un chiffrement inefficace, et une absence de moyen de sortie du système (opt out) [27]. Qui plus est, les capteurs eux-mêmes sont parfois mal sécurisés. Des failles telles que l’absence d’authentification et de chiffrement des communications et des mises à jour du firmware ont été trouvées sur des capteurs utilisés pour le comptage de véhicules dans plusieurs pays [28].

La notification des usagers d’un lieu quant à la présence d’un système de traçage n’est pas toujours correcte, comme le montre par exemple la réclamation administrative de la FTC envers l’entreprise Nomi en 2015 [17]. Elle se présente souvent sous la forme d’un écriteau, voire d’une simple feuille de papier A4 (cf. figure 3).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/gnu-linux-magazine/glmfhs-099/83893/BHV1.jpg">

*Fig. 3 : Une affiche annonçant le traçage Wi-Fi dans un grand magasin parisien (Photo: Adrien Havet : https://twitter.com/adhavet/
status/891693199424729092/
photo/1).*

De manière encore plus critiquable, le traçage est parfois effectué par des fournisseurs de réseau Wi-Fi. Ceux-ci profitent de leur position de man-in-the-middle pour collecter des informations d’intérêt critiquable comme l’âge, le genre ou la photo postée sur des réseaux sociaux [29]. Certaines entreprises d’analyse d’audience vont même plus loin dans le traçage, en croisant les informations recueillies par Wi-Fi et Bluetooth avec d’autres sources d’informations, telles que les caméras de surveillance ou les informations de paiement par carte bleue [30]. Cette pratique est hautement inquiétante en termes de vie privée, car ces différentes sources et leur croisement peuvent fournir de nombreuses informations sensibles sur un individu.

## 5. Consentement
Le consentement est un concept important pour la protection de la vie privée. Cela désigne le fait que les utilisateurs doivent donner leur accord pour la collecte de leurs données personnelles. Si on se base sur la définition généralement admise de la vie privée comme capacité de choisir quelle information personnelle est partagée avec qui, il ne peut y avoir de vie privée sans consentement.

Pour obtenir l’accord d’un utilisateur pour intégrer un système, deux approches existent :

opt-in : l’utilisateur annonce explicitement qu’il veut faire partie du système (et n’en fait donc pas partie par défaut) ;
opt-out : l’utilisateur fait partie du système par défaut, et peut indiquer qu’il veut en sortir.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/gnu-linux-magazine/glmfhs-099/83893/optout_fpf.png">

*Fig. 4 : Le système d’opt-out en ligne du Future of Privacy Forum (https://optout.smart-places.org/). Les utilisateurs entrant leur adresse MAC seront retirés des systèmes de la plupart des entreprises d’analyse d’audience américaines.*

Dans la pratique, nous n’avons jamais vu de système de traçage Wi-Fi fonctionnant par opt-in. Les systèmes possèdent au mieux un système d’opt-out, au pire aucune possibilité d’éviter le traçage. Pour donner un exemple de ce dernier cas, nous n’avons trouvé aucun moyen de sortir des systèmes de traçage de véhicules cités plus haut. Lorsqu’ils existent, les systèmes d’opt-out sont peu visibles ou peu annoncés. Dans l’affaire Nomi-FTC, l’une des plaintes était que l’opt-out n’était pas possible à l’intérieur des magasins [17].

En effet, les systèmes d’opt-out actuels prennent typiquement la forme d’une page web sur laquelle l’utilisateur peut entrer son adresse MAC (voir par exemple la figure 4). D’autres systèmes existent : à Niort, les usagers peuvent scanner un QR code (que l’on suppose rediriger vers une page web). À la Défense, il est simplement indiqué que les utilisateurs peuvent envoyer un e-mail pour s’opposer au traçage... Le problème de tous ces systèmes est que l’utilisateur est supposé pouvoir fournir son adresse MAC. Cela peut être compliqué pour des utilisateurs non avertis, voire presque impossible sur certains appareils ne donnant pas d’accès à cette information, tels les appareils connectés (bracelets fitness, etc.). Il est parfois indiqué que l’usager est libre de désactiver son Wi-Fi pour ne pas être tracé [18][figure 3]. Nous avons montré que ce n’est souvent pas suffisant : empêcher un smartphone Android ou iOS d’émettre des probe requests est plus compliqué qu’il n’y paraît. Une option bien cachée dans les paramètres d’Android permet au système d’émettre des trames même si le Wi-Fi est désactivé (cf. figure 5). De façon similaire, le bouton Wi-Fi du centre de contrôle d’iOS coupe les connexions, mais n’empêche pas le système d’émettre des probe requests (contrairement à celui des paramètres) [31].

<img src="https://connect.ed-diamond.com/sites/default/files/articles/gnu-linux-magazine/glmfhs-099/83893/hidden.png">

*Fig. 5 : Localisation des options d'Android 7.0 autorisant le système à émettre des trames même si le Wi-Fi est désactivé. Il faut aller sur les 3 points en haut à droite pour trouver ces options.*

Nous avons proposé une solution potentielle pour faciliter l’opt-out. Un faux réseau Wi-Fi avec un nom explicite (par exemple « Do not track ») pourrait être annoncé par les systèmes de traçage. Les appareils tentant de s’y connecter seraient alors automatiquement exclus du système. Ainsi, les utilisateurs n’auraient qu’à se connecter à un réseau Wi-Fi, opération simple que la plupart savent déjà effectuer. Comme le téléphone conserve les noms de réseaux connus et essaye de s’y reconnecter dès qu’il repère un réseau ayant un tel nom, l’action n’a à être effectuée qu’une seule fois, et pourrait être commune à plusieurs lieux [32].

Un autre problème à noter vis-à-vis du consentement est le simple fait que les ondes Wi-Fi ne sont pas arrêtées par les murs... Un système de traçage va donc enregistrer des informations de personnes n’entrant pas dans le lieu concerné (par exemple des passants d’une rue à proximité du lieu). Il devient alors problématique de supposer l’entrée dans le lieu comme une forme de consentement, argument pourtant utilisé pour justifier la stratégie d’opt-out. Ce problème est même parfois présenté comme une feature ! (voir http://www.libelium.com/products/meshlium/smartphone-detection/)

## 6. Régulation
Du fait des problèmes de vie privée décrits plus haut, les systèmes de traçage et de mesure d’audience Wi-Fi sont souvent contrôlés par les autorités de régulation.

### 6.1 En France
En France, la CNIL a publié des règles précises que les commerces doivent respecter pour installer de tels systèmes. Pour citer directement le document de référence [33] :

- les données émises par le téléphone portable doivent être supprimées lorsque son porteur sort du magasin ;
- ou l’algorithme d’anonymisation utilisé doit assurer un fort taux de collision, c’est-à-dire qu’un identifiant en base doit correspondre à de nombreuses personnes. L’utilisation d’un tel algorithme permet notamment d’estimer les taux de retour des personnes dans un magasin avec un taux d’erreur non préjudiciable pour le commerçant tout en permettant d’assurer le respect de la vie privée de ses clients.

Pour résumer : les systèmes en place, pour être légaux, doivent donc effacer immédiatement les données, ou les agréger avec un taux d’erreur qui n’est, cependant, pas précisé. Le document précise ensuite que la conservation des données non anonymisées ne peut se faire que sous condition d’un consentement « préalable et éclairé, se manifestant par une action positive » (le consentement ne peut être établi par défaut).

Les entreprises ne respectant pas ces règles peuvent voir les installations rejetées ou interdites. Par exemple, la CNIL a rejeté en 2015 une première demande d’installation à la Défense, décidant que la méthode de chiffrement utilisée ne suffisait pas à garantir l’anonymat des données [34]. Pour être précis, elle note qu’une procédure de hachage avec sel n’empêche pas la société de retrouver les données non anonymisées, de les corréler avec d’autres sources, et d’inférer de nouvelles informations sur les visiteurs (fréquence de visite, etc.). Comme nous le disions plus haut, cette procédure ne garantit pas l’anonymat des données.

La mise en application du Règlement général sur la protection des données (RGPD ou GDPR) apporte une nouvelle dimension à la régulation de ces systèmes. Il va devenir plus difficile pour les entreprises de justifier leur installation : en effet, l’ambiguïté de l’obtention du consentement d’un système d’opt-out a des chances de ne plus suffire avec ces nouvelles règles. En attente de cas réels, nous ne savons pas sur quelles règlementations va s’appuyer la CNIL. Dans toute l’Europe, l’impact du GDPR sur les données de localisation et les systèmes de profilage en général reste pour l’instant encore flou [35].

### 6.2 Ailleurs
D’autres entités de régulation locales freinent également le déploiement de systèmes de traçage. Citons par exemple la Datainspektionen suédoise qui a interdit une installation dans le centre-ville de Västerås (aucun rapport avec la ville de prononciation quasi équivalente de Game of Thrones), car les adresses MAC étaient stockées en clair. Aux États-Unis, la Federal Trade Commission (FTC), bien que moins stricte que d’autres entités équivalentes, a déposé une plainte contre l’entreprise Nomi pour notification insuffisante des usagers et procédures d’opt-out insuffisantes [17]. Au Royaume-Uni, la City of London Corporation a ordonné le retrait de poubelles contenant des traceurs Wi-Fi [36].

## 7. Acceptation
En général, le public accepte mal les systèmes de traçage. Plusieurs sondages ont rapporté un taux de rejet de 70 à 80% [37][38], ces taux montant jusqu’à 90% pour une autorisation complète et sans condition de la collecte. Ces taux diminuent légèrement lorsque le consentement est explicitement demandé, ce qui donne une indication de l’importance du consentement dans l’acceptation de tels systèmes [38].

Lorsque le public est conscient de l’existence de systèmes de traçage, de fortes protestations peuvent s’élever de la part d’associations ou de partis politiques, comme ce fut le cas à Niort et Rennes [39][40]. Dans ce dernier cas, les protestations furent telles que les installations furent suspendues avant même que la CNIL ne donne son avis [41]. Des cas similaires existent aux États-Unis, où des installations dans des magasins n’ont pas plu à leurs clients [42]. L’intervention de figures politiques a mené à la rédaction d’un code de conduite [43], préconisant l’emploi de mécanismes d’opt-out et de signalement explicite de la présence des systèmes [44][45].

Il est intéressant de comparer l’acceptation du traçage physique à celle d’autres formes de traçage, notamment le traçage en ligne (web). Pour le traçage en ligne et la publicité ciblée, deux études trouvent des taux de rejet de 66 à 68% [46][47]. Ce taux monte jusqu’à 86% lorsque l’on explique aux sondés comment leurs données sont collectées, et à 90% si le ciblage publicitaire est le produit de leurs activités hors-ligne [47].

Enfin, ne manquons pas de mentionner le travail de l’Electronic Frontier Fondation (EFF), qui s’est positionnée contre les systèmes de traçage, rappelant que la collecte d’adresses MAC pourrait être considérée comme illégale [44].

## 8. Ingénierie
Certains rapports donnent des informations intéressantes concernant l’ingénierie de systèmes de traçage en situation réelle et en déploiement permanent. Une partie d’un rapport sur le système de traçage de véhicules du Maryland donne de telles indications [13]. Les capteurs respectent une norme nommée NEMA TS2 pour la résistance aux températures et conditions d’humidité extrêmes. Pour obtenir une source d’énergie suffisante, ils sont chacun équipés de panneaux solaires de 30 Watts. La communication des données repose sur le réseau cellulaire, à l’aide de modems GSM. Au total, l’installation coûte 7 200$ par capteur, pour un cycle de vie de 5 ans (incluant hardware, installation et coûts de communication). Des informations similaires sont trouvables pour les installations de Lyon [12][48].

## Conclusion
Nous avons vu dans cet article des exemples de système de traçage Wi-Fi et Bluetooth sous diverses dimensions : fonctionnement, problèmes de vie privée, régulation, gestion du consentement, acceptation. L’étude d’installations existantes montre à quel point nombreuses sont celles pêchant dans leur protection de la vie privée, que ce soit par une gestion discutable de l’obtention du consentement, une sécurisation et une anonymisation insuffisante des données personnelles, ou une mise en place trop lourde et difficile des mécanismes de retrait du système (opt-out).

Nous réaffirmons la nécessité d’intégrer les concepts de privacy-by-design dans de tels systèmes, ce qui est d’ailleurs indiqué dans le GDPR. Les questions de consentement et de protection des données ne peuvent pas être prises à la légère dans des systèmes manipulant les données personnelles de foules entières.

Heureusement, des évolutions au niveau des appareils permettent de limiter peu à peu les possibilités de traçage. Beaucoup de téléphones récents intègrent maintenant des mécanismes de changement aléatoire de l’adresse MAC qui, bien que loin d’être imparfaits, rendent le traçage moins trivial à effectuer [32].

## Remerciements
Merci à Nagham pour sa relecture et ses encouragements.

## Références
[1] Wombat : https://github.com/Perdu/wombat
[2] Probr : http://probr.ch/
[3] Snoopy : https://github.com/sensepost/Snoopy et https://github.com/sensepost/snoopy-ng
[4] DELOITTE, « Usages mobiles », 2016 : https://www2 .deloitte.com/fr/fr/pages/technology-media-and-telecommunications/articles/usages-mobiles-2016.html
[5] M. ANDERSON,« Technology device ownership », 2015 : http://www.pawinternet.org/2015/10/29/technology-device-ownership-2015/
[e3] Pew Research Center. Mobile fact sheet. http://www.pewinternet.org/fact-sheet/mobile/
[6] ARCEP, « L’État d’internet en France 2017 », 2017
[7] L. RAINIE et K. ZICKHUR, « Americans’ views on mobile etiquette » : http://www.pewinternet.org/2015/08/26/chapter-1-always-on-connectivity/
[8] G. WILKINSON, « Digital terrestrial tracking: The future of surveillance », DEFCON 22
[9] C. KONTOKOSTA et N. JOHNSON, « Urban phenology: Toward a real-time census of the city using Wi-Fi data », Computers, Environment and Urban Systems, 2017
[10] J. O'MALLEY, « Exclusive: Here’s What 3 Big Museums Learn By Tracking Your Phone » : http://www.gizmodo.co.uk/2017/04/exclusive-heres-what-museums-learn-by-tracking-your-phone/
[11] Anonymous Wireless Address Matching (Bluetooth) : http://www.houstontranstar.org/faq/traffictech.aspx
[12] G. GROLLEAU, « La captation bluetooth au service des aménagements urbains », 2015
[13] S. YOUNG, « Bluetooth traffic detectors for use as permanently installed travel time instruments », State Highway Administration Research Report, 2013
[14] CB Insights, « The store of the future: 150+ startups transforming brick-and-mortar retail in one infographic », 2017
[15] É. BROUZE, « Tu es resté 22 minutes chez l’opticien jeudi. Le centre commercial le sait » : https://www.nouvelobs.com/rue89/rue89-nos-vies-connectees/20170711.OBS1939/tu-es-reste-22-minutes-chez-l-opticien-jeudi-le-centre-commercial-le-sait.html
[16] Datatilsynet, « Tracking in public spaces », 2016
[17] M. GEUSS,« Creepy but legal phone-tracking company gets wrist slap for empty privacy promise » : https://arstechnica.com/tech-policy/2015/04/creepy-but-legal-phone-tracking-company-gets-wrist-slap-for-empty-privacy-promise/
[18] Royal Shiphol Group Privacy statement : https://www.schiphol.nl/en/privacy-policy/
[19] C. ALLAIN, « Rennes : Des capteurs wifi pour suivre les clients du centre-ville » : https://www.20minutes.fr/rennes/2011831-20170210-rennes-capteurs-wifi-suivre-clients-centre-ville
[20] E. TOURON,« Wifi-VIP : la publicité directe sur les smartphones », https://www.lanouvellerepublique.fr/actu/wifi-vip-la-publicite-directe-sur-les-smartphones
[21] C. KANG,« China plans to track cellphone users, sparking human rights concerns » : http://voices.washingtonpost.com/posttech/2011/03/china_said_it_may_begin.html
[22] B. LEVINE,« Drones overhead in L.A.’s Valley are tracking mobile devices’ locations » : https://venturebeat.com/2015/02/23/drones-over-head-in-las-valley-are-tracking-mobile-devices-locations/
[23] D. KERR, « Russian police spy on people's mobile data to catch thieves » : https://www.cnet.com/news/russian-police-spy-on-peoples-mobile-data-to-catch-thieves/
[24] B. HUANG et E. SNOWDEN, « Against the law: Countering lawful abuses of digital surveillance », 2016
[25] A. MEYER,« Danger Close: Fancy Bear Tracking of Ukrainian Field Artillery Units » : https://www.crowdstrike.com/blog/danger-close-fancy-bear-tracking-ukrainian-field-artillery-units/
[26] J. SCAHILL et G. GREENWALD, « The NSA’s secret role in the U.S. assassination program », The Intercept, 2014
[27] L. DEMIR, M. CUNCHE, et C LAURADOUX, « Analysing the privacy policies of Wi-Fi trackers », In Proc. of the 2014 workshop on physical analytics, 2014
[28] C. CERRUDO, « Hacking US traffic control system », DEFCON, 2014
[29] Krowdthink, « They know where you are - an investigation into the contracts, policies and practices of mobile and Wi-Fi service providers in relation to location tracking », 2016
[30] A. SOLTANI, Technological overview : https://www.ftc.gov/system/files/documents/public_events/182251/mobiledevicetrackingseminar-slides.pdf
[31] C. MATTE, M. CUNCHE et V. TOUBIANA, « Does disabling Wi-Fi prevent my Android phone from sending Wi-Fi frames? », Diss. Inria-Research Centre Grenoble–Rhône-Alpes; INSA Lyon, 2017
[32] C. MATTE,« Wi-Fi Tracking: Fingerprinting Attacks and Counter-Measures », Thèse de doctorat, Université de Lyon, 2017
[33] CNIL, « Mesure de fréquentation et analyse du comportement des consommateurs dans les magasins », https://www.cnil.fr/fr/mesure-de-frequentation-et-analyse-du-comportement-des-consommateurs-dans-les-magasins
[34] Délibération de la CNIL : https://www.legifrance.gouv.fr/affichCnil.do?oldAction=rechExpCnil&id=CNILTEXT000031159401
[35] D. MEYER, « What the GDPR will mean for companies tracking location », https://iapp.org/news/a/what-the-gdpr-will-mean-for-companies-tracking-location/
[36] « City of London Corporation wants 'spy bins' ditched » : https://www.theguardian.com/world/2013/aug/12/city-london-corporation-spy-bins
[37] OpinionLab, « New study: consumers overwhelmingly reject in-store tracking by retailers », https://www.opinionlab.com/newsmedia/new-study-consumers-overwhelmingly-reject-in-store-tracking-by-retailers/
[38] K. FAWAZ, K.-H. KIM, et K. SHIN, « Privacy vs. reward in indoor location-based services », Proceedings on Privacy Enhancing Technologies, 2016
[39] « WiFi VIP - un dispositif coûteux qui porte atteinte à la vie privée des niortais-e-s » : http://deuxsevres.eelv.fr/12/wifi-vip-un-dispositif-couteux-qui-porte-atteinte-a-la-vie-privee-des-niortais-e-s/
[40] « Commerce à Rennes. Dominique Fredj démissionne du carré rennais » : https://www.ouest-france.fr/bretagne/rennes-35000/commerce-rennes-dominique-fredj-demissionne-du-carre-rennais-4860890
[41] « Rennes : les commerçants reportent la mise en service de capteurs WiFi suivant les smartphones » : https://www.20minutes.fr/rennes/2027787-20170309-rennes-commercants-reportent-mise-service-capteurs-wifi-suivant-smartphones
[42] S. CLIFFORD et Q. HARDY, « Attention, Shoppers: Store Is Tracking Your Cell » : https://www.nytimes.com/2013/07/15/business/attention-shopper-stores-are-tracking-your-cell.html
[43] « Mobile Location Analytics - Code of conduct » : https://fpf.org/wp-content/uploads/10.22.13-FINAL-MLA-Code.pdf
[44] P. HIGGINSet L. TIEN,« Mobile tracking code of conduct falls short of protecting consumers » : https://www.eff.org/deeplinks/2013/10/mobile-tracking-code-conduct-falls-short-protecting-consumers
[45] J. MARTINEZ,« Franken still unsatisfied with Euclid’s privacy practices » : http://thehill.com/policy/technology/291299-franken-still-unsatisfied-with-euclids-privacy-practices
[46] K. PURCELL, J. BRENNER et L. RAINIE, « Search engine use 2012 », Pew Internet & American Life Project Washington, 2012
[47] J. TUROW, J. KING, C. HOOFNAGLE, A. BLEAKLEY et M. HENNESSY, « Americans reject tailored advertising and three activities that enable it », Departmental Papers (ASC), 2009
[48] ATEC ITS France, « Évaluations simultanées de différentes technologies innovantes de recueil de données trafic pour le calcul de temps de parcours en temps réel », 2015



source : https://connect.ed-diamond.com/GNU-Linux-Magazine/glmfhs-099/tracage-wi-fi-qu-en-est-il-en-pratique