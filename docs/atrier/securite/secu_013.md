---
title: Détection des rançongiciels, ou comment lutter contre nos propres outils de sécurité ?
description: 
published: true
date: 2023-04-21T10:40:49.951Z
tags: détection, ransonware, chiffrement, honeyspot, malware
editor: markdown
dateCreated: 2023-04-21T10:40:46.032Z
---

# Détection des rançongiciels, ou comment lutter contre nos propres outils de sécurité ?

> Votre ordinateur a été la cible d’une attaque par un rançongiciel et vous n’aviez pas de sauvegarde ? Dommage. Dans cet article, nous présentons des techniques de détection de rançongiciels qui auraient pu empêcher cela.

Pour détecter un rançongiciel, bien évidemment toute la batterie des antivirus classiques est déployée. L’apprentissage automatisé sous toutes ses formes est utilisé pour analyser les logs réseaux ou le binaire du programme. On pourrait écrire plusieurs centaines de pages rien que pour toutes ces techniques de détection. Dans cet article, nous préférons nous concentrer sur la lutte contre les comportements spécifiques des rançongiciels.

## 1. Le chiffrement détourné
Un rançongiciel (ransomware) est un logiciel malveillant qui prend en otage des données informatiques. Il demande ensuite à l’utilisateur de payer une rançon en échange de leur restitution. Il existe différents types de rançongiciels. Certains ciblent quelques fichiers spécifiques de votre ordinateur et les exfiltrent, puis menacent d’en diffuser le contenu. Cette nouvelle génération de rançongiciels connue sous le nom de doxware n’est aujourd’hui qu’à son commencement. La plupart des rançongiciels appelés en anglais crypto-ransomware utilisent une autre technique pour prendre les fichiers en otage : ils les chiffrent ! Ils s’attaquent ainsi à tous les types de fichiers (.docs, .jpg, .txt, etc.) sans en analyser le contenu. La rançon est demandée en échange de la clé de déchiffrement.

La confidentialité des données informatiques est toujours ou presque assurée par du chiffrement. L’utilisateur n’en a souvent même pas conscience ou n’y prête plus attention. Par exemple : le « s » de https signifie, entre autres, que les communications avec le site web sont chiffrées ; ou encore, sur certains disques durs, l’écriture de données chiffrées de manière transparente pour l’utilisateur. Ainsi, les rançongiciels détournent un outil quotidien de la sécurité informatique à des fins malveillantes. C’est leur caractéristique la plus marquante. Tel est pris qui croyait prendre !

Décrypter le chiffrement d’un rançongiciel revient à faire de la cryptanalyse. Or il est évident que les rançongiciels performants utilisent les standards actuels de chiffrement. Généralement, un protocole est utilisé pour exfiltrer une ou des clés de chiffrement symétrique. L’algorithme de chiffrement symétrique le plus utilisé par les rançongiciels est l’Advanced Encryption Standard (AES). Il est impensable de tenter d’attaquer l’algorithme de chiffrement lui-même pour se protéger des rançongiciels. Si quelqu’un y parvenait, cela reviendrait à devoir changer le standard. Mais, alors, comment se préserver d’un attaquant bien à jour de l’état de l’art en matière de sécurité ?

Par ailleurs, une fois les fichiers chiffrés c’est trop tard ! Le seul espoir est alors de payer la rançon, ce qui ne garantit en aucun cas la restitution de vos données. Néanmoins, le modèle économique le plus courant des rançongiciels est de demander des sommes peu élevées et viser de nombreuses victimes. Aussi, la plupart de ces derniers restituent les données. En effet, si l’information que la clé de déchiffrement n’est pas fournie en échange de la rançon se propage, alors aucune des futures victimes ne se risquera à payer. Par ailleurs, les auteurs de rançongiciels ciblant des entreprises calculent le montant de la rançon de manière à ce que celui-ci soit inférieur au coût de la remise en service avec une sauvegarde des données. Malgré tout, l’Agence Nationale de la Sécurité des Systèmes d’Information (ANSSI) déconseille de payer les rançons afin de ne pas encourager les auteurs des rançongiciels. Il est préconisé de faire des sauvegardes régulières des systèmes et de s’assurer de la capacité de les restaurer rapidement.

## 2. Lutte contre les rançongiciels
Idéalement pour lutter contre un rançongiciel, comme pour tout logiciel malveillant, il faut le détecter et le stopper, dès son intrusion, avant qu’il ne lance l’attaque. Malheureusement, ce n’est pas toujours une réussite. Dans ce paragraphe, nous nous intéressons aux techniques de détection du comportement d’un rançongiciel, après infection. Pour lutter contre un tel comportement, il faut détecter l’attaque, c’est-à-dire le chiffrement, très rapidement pour perdre le moins de fichiers possible. On considère que chaque fichier chiffré est un fichier perdu.

### 2.1 Via le chiffrement
#### 2.1.1 Mauvaise implémentation cryptographique
Implémenter correctement une fonction de chiffrement n’est pas évident. Une mauvaise implémentation peut apporter des failles non prises en compte par l’algorithme théorique. Un exemple typique est LockCrypt, sorti en 2017, dont les auteurs ont cherché à concevoir leur propre algorithme de chiffrement. On se doute bien que le résultat fut un désastre. En effet, il est assez facile de retrouver la clé sans payer.

Une erreur classique des premiers rançongiciels, comme une des premières versions de SynoLocker, est l’utilisation d’un mauvais mode de chiffrement par blocs. Le mode ECB (Electronic Code Book) consiste à chiffrer des blocs de messages indépendamment les uns des autres. Or l’inconvénient de cette méthode est que si on chiffre deux fois le même message, on obtient le même chiffré. C’est pourquoi dans une implémentation robuste ce mode n’est jamais utilisé. De nombreux autres modes, comme par exemple le mode GCM (Galois/Counter Mode) qui permet de garantir l’intégrité en plus de la confidentialité, sont utilisés.

Plus prosaïquement, le rançongiciel peut oublier d’effacer la clé de chiffrement de la mémoire vive de l’ordinateur. C’est le cas de Wannacry avec certaines vieilles versions de Windows. Le rançongiciel utilise l’API cryptographique correctement, mais cette dernière contient une vulnérabilité. En effet, les clés ne sont pas effacées après manipulation et il est donc possible d’en retrouver une partie, comme montré par Adrien Guinet [1].

Ou encore une mauvaise implémentation peut être vulnérable à la cryptanalyse par attaques temporelles. Ces attaques se focalisent sur le temps de calcul d’opérations critiques de l’algorithme de chiffrement, afin d’en retrouver la clé. Il existe un décrypteur pour STOP djvu (apparemment) basé sur ce principe, mais les détails de ce décrypteur n’ont pas été publiés pour le moment.

Ces exemples comme beaucoup d’autres sont des erreurs d’implémentation qui peuvent être utilisées pour lutter contre des rançongiciels peu élaborés. Les rançongiciels les plus récents ont gagné en expérience et ne font plus ce genre d’erreurs.

#### 2.1.2 L’utilisation d’outils cryptographiques
Pour éviter des failles liées à l’implémentation, beaucoup de rançongiciels ne développent pas leur propre bibliothèque cryptographique. Certains utilisent tout simplement les outils de leur victime, comme les outils Windows. La « crypto API » de Windows (MS CAPI de son vrai nom) offre de nombreux services de sécurité, comme le chiffrement, aux applications de l’espace utilisateur. Le rançongiciel n’a plus qu’à se servir. Cependant une contre-mesure efficace est de surveiller l’appel à ces bibliothèques [2].

Malheureusement, certains rançongiciels performants embarquent dans leur code les fonctions de chiffrement implémentées de façon robuste.

#### 2.1.3 Pots de miel
Qui dit chiffrement dit modification des données. Beaucoup de contre-mesures existantes se basent sur la détection de ces modifications. Une méthode pour se préserver des rançongiciels est la technique du « pot de miel » (honeypot) ou canari. L’utilisateur crée des fichiers de leurres, par exemple ma_compta.docx dans le répertoire Documents dont il surveille l’intégrité régulièrement. Si ce fichier ma_compta.docx est modifié, le rançongiciel est détecté.

#### 2.1.4 Entête de fichier
Les entêtes de fichiers sont généralement très structurés. Ils contiennent des précisions sur le format du fichier. Il est possible de réaliser un apprentissage de l’arrangement des bits composant un entête [3]. Aussi, lorsqu’un rançongiciel modifie ces entêtes pour les chiffrer, cela détruit complètement la structure. Cela se détecte rapidement et avant qu’il ait pu chiffrer le document en entier. Malheureusement aujourd’hui il est fréquent de rencontrer des rançongiciels qui ne chiffrent qu’une partie des documents et bien évidemment pas les entêtes.

#### 2.1.5 Détection de l’aléatoire
La confusion et la diffusion sont deux propriétés utilisées dans un algorithme de chiffrement. Elles ont été introduites par Shannon. La confusion correspond à une volonté de rendre la relation entre le texte clair et le texte chiffré la plus complexe possible. La diffusion est l’idée qu’un biais sur le texte clair ou sur la clé ne doit pas se retrouver sur le texte chiffré. Pour un algorithme de chiffrement avec une bonne diffusion, l’inversion d’un seul bit en entrée doit changer chaque bit en sortie avec une probabilité d’un demi, indépendamment les uns des autres.

Un rançongiciel utilise généralement les standards de chiffrement qui respectent ces propriétés. Or la confusion et la diffusion ont pour conséquence que le chiffré obtenu a une répartition des bits proche de l’aléatoire uniforme. Une méthode de détection de chiffrement est alors de surveiller l’écriture de données qui semblent être le fruit du hasard dans un fichier.

L’outil classique de détection d’aléa est l’entropie de Shannon. Elle correspond à la mesure de la quantité d’information contenue dans un octet. Plus elle est élevée plus cela signifie que la distribution des données se rapproche d’une distribution uniformément aléatoire. Cependant, pour être précise, celle-ci a besoin de beaucoup de données. Or dans le cas de la détection du chiffrement, il ne faut pas attendre pour agir et bloquer un processus douteux. Ainsi, dans le monde académique, ces dernières années se déroule une compétition de l’outil statistique le plus approprié [3,4,5].

### 2.2 Autres comportements suspects propres aux rançongiciels
#### 2.2.1 Parcours des fichiers
Afin de chiffrer les fichiers de sa victime, un rançongiciel parcourt tous les dossiers de l’ordinateur en partant de sa racine. Aussi ce parcours des fichiers est un marqueur de compromission. Malheureusement, si on observe une ouverture et une modification systématique des fichiers, c’est que nombre d’entre eux sont déjà perdus. Cet indicateur fait également beaucoup de faux positifs, comme une sauvegarde journalière par exemple.

#### 2.2.2 La note de rançon
La rançon doit généralement être payée via une monnaie virtuelle difficilement traçable comme le bitcoin. Mais attention, pas question que l’utilisateur ne paye pas, car il n’a pas compris ce qu’il devait faire. Certains rançongiciels vont jusqu’à installer un chat pour aider leur victime à payer. Néanmoins la plupart des notes de rançon ne sont qu’un simple fichier texte déposé à différents emplacements stratégiques de l’ordinateur.

Aussi les notes de rançon doivent être les plus visibles et compréhensibles que faire se peut. Parfois, il y a une note de rançon par fichier chiffré. L’écriture de ces notes de rançon est un autre indice de l’infection par un rançongiciel. Pour détecter ces apparitions de notes de rançon, on peut se focaliser sur des mots clés communs à toutes ces notes. Cette contre-mesure n’est d’aucune efficacité face à un rançongiciel qui dépose sa rançon dans un chat ou une image.

Par ailleurs, quand la première note de rançon apparaît, bien souvent une partie des fichiers est déjà chiffrée.

### 2.3 Autres mécanismes de défense
#### 2.3.1 Suivis des transactions
Des études ont également porté sur la recherche des transactions bitcoins correspondant à des paiements de rançon [6]. Elles permettent d’identifier des adresses utilisées par les attaquants, mais elles ne préservent pas des infections.

#### 2.3.2 Copy-on-Write
Une contre-mesure simple contre les rançongiciels est l’utilisation d’un système de fichiers moderne tel que ZFS qui implémente le copy-on-write. C’est-à-dire que toute nouvelle écriture d’un fichier est faite à un nouvel emplacement (si possible) et n’écrase pas la version précédente du fichier. Ainsi en cas d’infection, l’écriture du fichier chiffré n’écrase pas la version en clair. En conséquence, il devient possible de retrouver l’ancienne version du fichier, non chiffrée, sans devoir payer la rançon. Réaliser des « snapshots » régulièrement permet de se protéger très efficacement.

## 3. Mise en pratique
### 3.1 Tester des contre-mesures
Au Laboratoire Haute Sécurité (LHS) de l’INRIA à Rennes, des chercheurs travaillent sur la mise en pratique de solutions anti-rançongiciels.

Tester des contre-mesures pour lutter contre ces derniers, comme pour tous les logiciels malveillants, n’est pas évident.

Pour commencer, il faut disposer d’au moins un ordinateur de test qui n’est pas relié au reste du réseau, afin de ne pas infecter ses collègues par inadvertance.

Par ailleurs, les rançongiciels ont une durée de vie limitée dans le temps. Aussi la base de données des logiciels malveillants doit se tenir à jour, il n’y a pas d’intérêt de tester des contre-mesures sur un rançongiciel devenu inactif.

Une autre difficulté est de convaincre le rançongiciel qu’il n’est pas dans un environnement de test. En effet, beaucoup d’entre eux ne se déclenchent pas dans un environnement virtualisé. De plus, ils attendent souvent un signal leur prouvant une activité sur la machine pour exhiber leur comportement malveillant. Ce signal attendu peut être l’utilisation du clavier, une requête internet, etc. Une fois ce signal détecté, certains d’entre eux attendent encore un temps, allant de la minute au jour, avant d’attaquer. Tous ces comportements compliquent bien évidemment les tests de contre-mesures.

### 3.2 Efficacité d’une protection
Pour avoir un outil de protection contre les rançongiciels efficace, il faut prendre en considération trois critères :

1. la rapidité de détection pour ne pas perdre de fichiers ou le moins possible ;
1. avoir un faible impact en termes de : mémoire, calcul, etc., afin de ne pas ralentir le système ;
1. limiter les faux positifs.

Aussi, il est ingénieux de multiplier les contre-mesures, autant pour stopper un maximum de rançongiciels que limiter les faux positifs. La difficulté est alors de ne pas trop ralentir le système malgré toutes les contre-mesures mises en place.

Par ailleurs, souvent il est nécessaire de demander à l’utilisateur son avis avant de bloquer un processus. Il est normalement le plus apte à savoir ce qui est censé se passer sur sa machine. Malheureusement, de nombreuses études en sociologie montrent que les personnes pas ou peu familiarisées avec l’informatique ne réagissent pas toujours de manière optimale face aux messages envoyés par leur antivirus. On a tous en tête le ou la collègue qui essaie de fermer la fenêtre de dialogue plutôt que de répondre à la question. La demande de prise de décision par l’utilisateur ne doit pas non plus être trop fréquente. Un outil qui sollicite trop souvent l’utilisateur risque de l’agacer et de se voir désinstaller.

Dans la suite de cet article, les détails d’implémentation de contre-mesures développées au LHS sont présentés [3].

### 3.3 Tout se joue dans le noyau
Windows est le système d’exploitation le plus utilisé sur ordinateur de bureau, de nombreux rançongiciels s’attaquent à celui-ci. Toutefois, des rançongiciels récents peuvent également cibler des objets connectés comme des appareils photo. Néanmoins, la majorité s’attaque à Windows.

Le système d’exploitation Windows n’utilise que deux niveaux de privilèges : l’espace utilisateur et l’espace noyau. Le code qui s’exécute dans le noyau dispose de tous les droits sur le système.

À l’inverse, un code qui s’exécute en mode utilisateur n’a qu’une infime partie des privilèges. Les applications, et donc les rançongiciels, fonctionnent dans l’espace utilisateur. Elles n’ont pas le droit de lire ou écrire directement dans un espace mémoire autre que le leur. La lecture, ou l’écriture de fichiers, ou les appels à certaines bibliothèques passent par des appels système qui sont traités dans le noyau. Il est donc idéal de se placer dans le noyau pour surveiller les applications pour détecter et stopper celles qui seraient malveillantes. Nous montrons, dans la suite, des mécanismes de protection utilisant des « hooks » sur certains appels système.

De plus, récemment, un nouveau rançongiciel a fait son apparition. Celui-ci détourne les programmes légitimes de la machine pour faire exécuter son travail de chiffrement. Il faut donc surveiller plusieurs processus à la fois.

#### 3.3.1 Insertion d’une porte dérobée dans MS CAPI
Les rançongiciels n’embarquant pas leur propre bibliothèque cryptographique font généralement appel à MS CAPI. Bien qu’il s’agisse de l’ancienne version de librairie cryptographique Windows, celle-ci est supportée sur un grand nombre de machines. S’il est envisageable de surveiller les appels à cette dernière, il est également possible de faire encore mieux en y introduisant une porte dérobée. Pour chiffrer avec cette API, il faut spécifier le fournisseur de service cryptographique (CSP pour Cryptographic Service Provider) ou se rabattre sur celui par défaut. On peut créer un nouveau CSP fournissant tous les algorithmes cryptographiques qui vont bien, mais avec une fonctionnalité en plus (notre porte dérobée). Celle-ci consiste à sauvegarder les clés de chiffrement symétriques. C’est possible puisqu’elles sont nécessairement manipulées par les algorithmes de chiffrement dans le CSP. Ainsi, si une attaque de rançongiciel est détectée, il suffit de réutiliser les clés soigneusement enregistrées pour tout déchiffrer.

En pratique, il faut encore que le nouveau CSP soit reconnu légitime, donc signé par Microsoft ; mais également qu’il soit utilisé, quel que soit le CSP demandé par l’application. Il est alors nécessaire de modifier le fonctionnement normal de la MS CAPI pour obtenir cet effet.

Attention, cette pratique est en réalité un « hook » de la librairie cryptographique de Windows. Elle peut se retourner contre l’utilisateur si une personne mal intentionnée met la main sur la sauvegarde de toutes les clés de chiffrement.

#### 3.3.2 Pilote minifilter
Sous Windows, pour qu’une application interagisse avec un fichier sur le disque dur, plusieurs couches logicielles sont concernées. Pour commencer, le programme utilise des appels systèmes comme read ou write. À la fin, il utilise un pilote pour le système de fichiers (ntfs.sys dans notre cas).

Entre les deux, Windows permet d’ajouter des minifilters pour agir entre les write et le système de fichiers par exemple. Ces minifilters peuvent permettre de proposer un chiffrement transparent du disque ou de surveiller les activités de lecture/écriture par un antivirus. Le minifilter se voit associer une valeur, appelée altitude, spécifiant son ordre d’exécution parmi tous les minifilters. Il est donc possible de créer un pilote minifilter de surveillance effectuant des mesures sur les données écrites sur le disque dur. Ainsi, à chaque utilisation de l’appel système write, il peut collecter : les données à écrire, mais aussi certaines métadonnées, dont le nom du fichier, la valeur de décalage de l’écriture dans le fichier, les ID du processus et du fil d’exécution. À partir des données à écrire, il peut évaluer à l’aide d’outils statistiques s’il s’agit d’un contenu chiffré. L’altitude du minifilter de surveillance doit lui permettre d’être prioritaire. En effet, s’il s’exécute après un minifilter de chiffrement du disque légitime, il détecte forcément du contenu chiffré.

Au LHS, les outils statistiques utilisés sont un test de Khi2 et des chaînes de Markov. Le test de Khi2 permet de déterminer avec un faible nombre d’octets si un contenu est aléatoire ou non. Les chaînes de Markov sont un outil très performant pour l’apprentissage des données. Elles détectent avec seulement quelques bits si un entête de fichier est normal ou non.

## Conclusion
Les techniques présentées ici en combinaison avec l’analyse des logs réseaux et du binaire des programmes forment un rempart robuste à la plupart des rançongiciels. Malheureusement, elles ne sont pas suffisantes face à l’arrivée d’un nouveau rançongiciel conçu de manière optimale. Celui-ci est nouveau, donc pas encore connu des antivirus, l’analyse de son binaire et de ses logs réseaux ne donne rien. Bien programmé, il fait de la cryptographie embarquée de façon robuste sans utiliser les outils de sa victime. Il ne chiffre pas tous les fichiers, mais uniquement certains d’entre eux. Ainsi, il évite les fichiers de leurres et un parcours systématique du répertoire utilisateur. Il chiffre de manière intelligente, c’est-à-dire qu’il ne modifie pas les entêtes et conserve l’entropie d’origine des fichiers. Voire il ne chiffre pas lui-même les fichiers et détourne des programmes bénins de l’ordinateur de la victime. Il n’écrit sa note de rançon qu’une fois son travail de chiffrement totalement achevé et plutôt dans une image que dans un fichier texte.

Face à cet hypothétique rançongiciel, nous constatons que la guerre n’est pas finie et qu’il y a encore des solutions à inventer. Il est donc conseillé d’effectuer des sauvegardes régulièrement, car il s’agit de la meilleure protection contre les rançongiciels.

## Remerciements
Je remercie Ronan Lashermes pour ses relectures constructives.

## Références
[1] Adrien Guinet, https://github.com/aguinet/wannakey
[2] Aurélien Palisse, Hélène Le Bouder, Jean-Louis Lanet, Colas Le Guernic, and Axel Legay. « Ransomware and the legacy crypto API. » In 11th International Conference, CRiSIS, 2016.
[3] Aurélien Palisse. « Analyse et détection de logiciels de rançon. » Thèse de doctorat, Rennes 1, 2019.
[4] Nolen Scaife, Henry Carter, Patrick Traynor, and Kevin RB Butler. « Cryptolock (and drop it): stopping ransomware attacks on user data. » In IEEE 36th International Conference on Distributed Computing Systems (ICDCS). IEEE, 2016.
[5] Amin Kharraz and Engin Kirda. « Redemption : Real-time protection against ransomware at end-hosts. » In International Symposium on Research in Attacks, Intrusions, and Defenses. Springer, 2017.
[6] Mauro Conti, Ankit Gangwal, and Sushmita Ruj. « On the economic significance of ransomware campaigns : A bitcoin transactions perspective. » Computers & Security, 2018.


source : https://connect.ed-diamond.com/MISC/misc-107/detection-des-rancongiciels-ou-comment-lutter-contre-nos-propres-outils-de-securite