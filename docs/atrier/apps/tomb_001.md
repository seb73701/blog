---
title: Garder ses secrets avec Tomb
description: 
published: true
date: 2023-04-21T10:50:35.076Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:50:30.280Z
---

# Garder ses secrets avec Tomb

> Pour conserver des documents privés sur un disque dur ou un support amovible, il est nécessaire d'avoir recours au chiffrement. L'outil Tomb permet de manipuler simplement des répertoires chiffrés par le standard LUKS, pensé dans un esprit de confort et de mobilité.

Nous montrons dans cet article les quelques commandes principales de Tomb pour une utilisation quotidienne de dossiers chiffrés dans diverses conditions environnementales.

## 1. Le chiffrement LUKS
Les systèmes Linux ont la faculté d'exploiter de façon transparente un file system chiffré via un module du noyau appelé LUKS [1], pour Linux Unified Key Setup. Cette fonctionnalité permet notamment de chiffrer un disque dur entier lors de l'installation, mais elle sait également protéger un volume logique à l'intérieur d'un file system existant.

La mise en œuvre de cryptsetup, pour le chiffrement, et dm-crypt pour le device-mapping, est complexe et nécessite beaucoup de manipulations et de paramétrage. C'est à ce problème que répond la solution Tomb [2], non sans un certain humour glacé quant au champ lexical.

## 2. Principe général de Tomb
Il faut tout d'abord préciser que tomb se présente sous la forme d'un simple script, destiné à être exécuté par tout utilisateur, et donc pas limité au root.

Malheureusement, l’installation n’est pas des plus simples. Mais il ne faut pas se laisser intimider. Nous la détaillerons plus bas, mais auparavant quelques explications conceptuelles s’imposent.

L'outil permet de créer un (gros) fichier qui servira de coffre-fort (ou, en l'occurrence, de tombe). La tombe se scelle au moyen d'une clé, qu’on génère pour l'occasion, et qui est elle-même protégée par une phrase de passe.

Après verrouillage, le fichier est cryptographiquement sûr et peut se transporter en toute confiance sur support amovible, ou dans un ordinateur portable. Vous devrez conserver précieusement la clé (c'est un petit fichier), sous peine de condamner votre tombe pour toujours. Mais attention à ne pas la laisser à la portée de vos attaquants, car, munis de la clé, la force de la tombe se réduirait à la qualité de la phrase de passe.Telle quelle, la tombe est donc un fichier illisible. Mais le script tomb permet bien sûr de l'ouvrir, en créant provisoirement un montage : car c'est en réalité un simple container LUKS. Vous précisez le répertoire d'accueil, et la tombe s'ouvre à vous sous la forme d'un dossier comme toute autre partie du file system. Les documents que vous y déposez sont tacitement et automatiquement chiffrés par le noyau, qui maintient à jour le fichier binaire qui représente le volume.Après utilisation, vous fermez la tombe et le point de montage disparaît. Il ne reste plus, en apparence, qu’un bloc de marbre.

## 3. Mise en œuvre
### 3.1 Installation
L’outil est en fait un gros script shell qui exploite ce que le noyau, ainsi que d’autres briques logicielles classiques, ont à offrir.

Il repose sur les dépendances suivantes (indiquées ici pour Debian ; le lecteur adaptera pour sa distribution) :

```
~ $ sudo apt-get install zsh gnupg cryptsetup pinentry-curses
```

Rassurez-vous, il n’est pas nécessaire de faire de zsh votre nouveau shell (ce qui serait cependant une bonne idée [3], indépendamment de cet article). Simplement, le programme tomb lui-même est un script zsh et il vous faut donc l’interpréteur.

Une fois les prérequis installés, l’outil, qui n’est généralement pas disponible dans les gestionnaires de paquets, doit être téléchargé [4] et décompressé.

```
~ $ tar xzf Tomb-2.7.tar.gz
~ $ cd Tomb-2.7
~/Tomb-2.7 $ sudo make install
```

Vous disposerez désormais de la commande tomb, pour tous les utilisateurs.

### 3.2 Commandes de base
Tomb met l'accent sur la simplicité de son API, en offrant une abstraction très réussie de toute lamécanique sous-jacente concernant la manipulation des volumes logiques chiffrés et des points de montage.De plus, l'outil a été pensé pour un usage quotidien, des ouvertures et fermetures parfois répétées plusieurs fois dans un intervalle court. Il est optimisé pour être intégré à un milieu en conditions réelles : cacher des documents dans un coffre-fort, c'est bien ; travailler sous l'illusion qu'ils se trouvent par exemple dans mon habituel dossier ~/Documents (et qu'ils en disparaissent tout seuls quand je ferme la tombe), c'est mieux !

Tomb réalise ce tour de passe-passe au moyen de ce que le projet appelle des hooks : ce sont des procédures configurables pour mettre en place des mappings de répertoires et fichiers.Voyons maintenant comment créer une tombe, l'ouvrir, la fermer, et automatiser les points de montage.

##### 3.2.1 Creuser une tombe
Une tombe est un fichier qui se crée par :

```
~ $ tomb dig -s <size> <tombfile>
```

Vous pouvez placer le fichier tombfile où bon vous semble. Il ne doit pas déjà exister, et il sera aussitôt construit avec la taille spécifiée par l'argument size, obligatoire, exprimé en Mio (10 minimum, c’est une contrainte de dm-crypt).Par exemple :

```
~ $ tomb dig -s 1000 ~/Documents/mon-coffre-fort.tomb
```

Pour le transport, ce sera bien cet unique fichier qu'il conviendra de prendre avec soi. Pratique.

Notez qu’il est possible de modifier la taille par la suite, mais seulement pour l’agrandir.

#### 3.2.2 Chiffrer
Le fichier ainsi construit ne vaut rien : il doit encore être préparé pour le chiffrement. On n'a creusé qu'un trou, qu'il faut bétonner et verrouiller.Fabriquons d'abord une clé chez le forgeron :

```
~ $ tomb forge <keyfile>
```

où keyfile est le nom du petit fichier de clé privée que Tomb doit générer. Il doit être inexistant, et le programme demandera interactivement une phrase de passe pour protéger la clé.

Mais la manipulation nous donne du souci :

```
tomb . An active swap partition is detected…
tomb [W] This poses a security risk.
tomb [W] You can deactivate all swap partitions using the command:
tomb [W] swapoff -a
tomb [W] [#163] I may not detect plain swaps on an encrypted volume.
tomb [W] But if you want to proceed like this, use the -f (force) flag.
tomb [E] Operation aborted.
```

Le swap pose ici un problème de sécurité, car en cas de faible mémoire vive disponible, le système peut être amené à déverser en clair une parie de son contenu vers le disque. Tomb refuse poliment de fonctionner dans ces conditions, et propose de désactiver provisoirement le swap (tous les swaps sur tous les disques) avec la commande :

```
~ $ sudo swapoff -a
```

Nous pourrons le réactiver juste après fabrication de la clé, par sudo swapon -a.

Cependant, il n’est pas toujours possible de manipuler ce paramètre du système, surtout lorsqu’on utilise Tomb sans sudo, ce qui figurait pourtant parmi les promesses de départ. En outre, même avec le swap activé, nous travaillons souvent sur un système doté d’une grande quantité de mémoire encore disponible, et pouvons présumer que le programme n’aura pas besoin d’y avoir recours (ou que personne ne viendra espionner la partition swap dans l’intervalle – mais la bienveillance de l’entourage ne fait pas partie des hypothèses pour cet article…).

Nous pouvons alors faire ce que le message d’erreur suggère : utiliser le flag -f pour forcer l’opération en dépit de l’existence d’un swap.

```
~ $ tomb forge ma-cle-secrete -f
    ┌──────────────────────────────────────────┐
    │ Type the new password to secure your key         │
    │                                                  │
    │ Password: ______________________________         │
    │                                                  │
    │       <OK>                  <Cancel>             │
    └──────────────────────────────────────────┘
```

Notez que le forçage pour cause de swap est nécessaire lors de la création de la clé, ainsi que lors de son ouverture. Une fois la tombe ouverte, vous pourrez rallumer le swap.

Le fichier généré est en fait un message PGP, chiffré par AES256 avec la phrase de passe fournie. C’est la résultante, binaire, que Tomb utilise pour le chiffrement du volume.

Muni de cette clé, scellons la tombe, par :

```
~ $ tomb lock -k <keyfile> <tombfile>
tomb . Locking using cipher: aes-xts-plain64
tomb . Formatting Luks mapped device.
tomb . Formatting your Tomb with Ext3/Ext4 filesystem.
tomb . Done locking using Luks dm-crypt aes-xts-plain64
```

Voici pour le travail de préparation : nous disposons désormais d’un fichier tombfile ayant une certaine taille préétablie, qui constitue le contenant d’un volume chiffré par la clé keyfile.

Il est possible de réutiliser la même clé pour plusieurs tombes, et nous verrons plus loin un cas pratique.

Il ne nous reste plus qu’à apprendre à ouvrir le volume pour l’utiliser. Attention à ne pas oublier la phrase de passe, et encore plus attention à défendre jalousement l'accès à la clé, qu'il ne faut sous aucun prétexte laisser sur le même support que la tombe. L'idéal, nous le verrons, est de conserver les clés en lieu sûr, distant, et de les passer en pipe pour ouvrir les tombes.

#### 3.2.3 Ouvrir la tombe
La tombe est creusée, et verrouillée. Pour l'utiliser et y déposer nos premiers documents, nous devons l'ouvrir. Un simple :

```
~ $ tomb open -k keyfile tombfile -f
```

a pour effet de déverrouiller provisoirement la tombe (après demande de la phrase de passe) et de créer automatiquement un montage dans /media/, dans un sous-dossier dont le nom est celui de base du fichier de la tombe (ici : tombfile).Mais, naturellement, ceci ne fonctionne pas toujours (supposons que vous n'ayez pas de droits sur /media), et il est préférable de ne pas s'y fier. Aussi vous préciserez le point de montage de votre choix, à l'ouverture :

```
~ $ mkdir ~/Documents/my-protected-vault
~ $ tomb open -k keyfile tombfile ~/Documents/my-protected-vault -f
```

Vous tapez interactivement votre phrase de passe, et vous pourrez utiliser le dossier spécifié pour y ranger vos fichiers secrets et les utiliser dans vos applications. Ils sont dé/chiffrés de façon transparente par le système.

Attention : si plusieurs personnes utilisent la machine, notez que tout le temps que la tombe est ouverte et montée, les comptes autorisés du système (donc, bien sûr le root) peuvent avoir accès à son contenu.

#### 3.2.4 Lister les tombes ouvertes
Pour voir l’ensemble des tombes en cours d’usage, exécuter :

```
~ $ tomb list
tomb . [secret] open on /media/secret using (rw,nodev,noatime)
tomb . [secret] open since Mon 30 Mar 2020 04:31:18 PM IDT
tomb . [secret] open by gabriel from /dev/pts/0
tomb . [secret] size 3.8G of which 2.1G (57%) is used: 1.6G free
tomb . [secret] hooks tomb.secret.1585575074.loop0 on /home/gabriel/.ssh/id_rsa
tomb . [confidentiel] open on /home/gabriel/Documents/confidentiel using (rw,nodev,noatime)
tomb . [confidentiel] open since Wed 01 Apr 2020 12:01:13 AM IDT
tomb . [confidentiel] open by gabriel from /dev/pts/4
tomb . [confidentiel] size 6.8M of which 83K (2%) is used: 6.2M free
```

Comme on peut le voir dans cet exemple, deux tombes sont ouvertes : secret et confidentiel. On trouve dans la liste le niveau de remplissage des volumes virtuels correspondants, et les bindings actifs (voir 3.3).

#### 3.2.5 Refermer
Nous rendons la tombe muette à nouveau par :

```
~ $ tomb close tombfile
```

Le dossier hôte redevient vide, et les bindings (voir 3.3) disparaissent.

Cependant, parfois, ça coince : si un processus est en cours d’exécution, avec un descripteur de fichier ouvert sur un des fichiers de la tombe, celle-ci refusera de se fermer.

```
~ $ vim /media/mytomb/secretfile &[1] + 3802 suspended (tty output) vim /media/mytomb/secretfile
~ $ tomb close mytomb
tomb . Closing tomb [mytomb] mounted on /media/mytombumount: /media/mytomb: target is busy.
tomb [E] Tomb is busy, cannot umount!
```

Dans ce cas, il faut claquer le couvercle ! C’est la commande slam qui fera l’affaire, en terminant les processus concernés.

```
~ $ tomb slam mytomb
tomb (*) Slamming tomb [mytomb] mounted on /media/mytomb
tomb  . [mytomb] sending TERMkill: (3802)
tomb  . Closing tomb [mytomb] mounted on /media/mytomb
tomb (*) Tomb [mytomb] closed: your bones will rest in peace.
```

> Attention : Il peut être utile de connaître cette particularité du logiciel : chaque tombe ouverte porte un nom, et il s’agit du nom de base de son fichier (sans dossier ni extension). Lorsqu’on referme une tombe, l’argument est le nom de la tombe, non pas le nom du fichier. Par conséquent, si vous ouvrez plusieurs tombes du même nom (c’est-à-dire, avec des fichiers de tombe ayant un même nom de base, mais des extensions différentes, ou bien figurant dans des dossiers différents), vous ne pourrez plus préciser exactement laquelle refermer. Le close aura alors pour effet de les refermer toutes.
{.is-info}


### 3.3 Bindings
Nous avons vu pour le moment comment manipuler un volume chiffré comme s’il s’agissait d’un support amovible. Pourtant, nous voulons parfois protéger certains emplacements précis de notre environnement habituel, intimement tissés dans les dossiers du quotidien plutôt qu’isolés dans le dossier chiffré. Par exemple, nous pourrions vouloir disposer d’un ensemble de clés privées dans ~/.ssh/ pour les connexions sans mot de passe, mais sans pourtant les y laisser en clair de façon permanente. Ceci peut s’obtenir avec des montages bien choisis à travers les file systems et c’est automatisable avec Tomb.

Dès que vous ouvrez une tombe, le programme exploite, s’il existe, le fichier nommé bind-hooks à sa racine. C’est un simple fichier texte à deux colonnes, pour indiquer quel dossier ou document relatif (dans la tombe) faire apparaître à quel emplacement du système. Un chemin relatif sera interprété par rapport au répertoire maison de l’utilisateur.

Par exemple, pour monter sur ~/.ssh/id_rsa un fichier qui se trouve dans la tombe au chemin mes-cles/serveur-dev, on stockerait à la racine de la tombe un fichier bind-hooks comme ceci :

```
mes-cles/serveur- dev   .ssh/id_rsa
mes-cles/serveur-prod   .ssh/id_rsa-prod
dossier-secret/doc      Documents/fichier
```

Une fois la tombe ouverte (3.2.3) ces fichiers se trouveront aux bons emplacements (en plus de leur place d’origine dans la tombe). Le bonus est que le fichier cible pouvait pré-exister, et son contenu n’est pas écrasé par le binding : vous le récupérez à la fermeture.

```
~ $ echo 'clair' > ~/Documents/fichier~
$ echo ~/Documents/fichier
clair
~ $ tomb open -f -k macle matombe ~/tombe
tomb  . Commanded to open tomb matombe
tomb  . Valid tomb file found: matombe
tomb  . Key is valid.
tomb (*) Opening matombe on /home/gabriel/tombe
tomb  . This tomb is a valid LUKS encrypted device.
tomb  . Cipher is "aes" mode "xts-plain64" hash "sha512"
tomb  . A password is required to use key macle
tomb  . Password OK.
tomb (*) Success unlocking tomb matombe
~ $ cat ~/tombe/dossier-secret/doc
secret
~ $ cat ~/Documents/fichier
secret
~ $ tomb close matombe 
~ $ cat ~/Documents/fichier
clair
```

Il ne s’agit bien sûr pas de copies, mais de montages : toute modification dans ces fichiers persiste dans la tombe. Si vous montez un fichier (plutôt qu’un répertoire), vous ne pourrez pas le supprimer de son emplacement fictif :

```
~ $ rm ~/.ssh/id_rsa
rm: cannot remove '/home/gabriel/.ssh/id_rsa': Device or resource busy
```

mais si vous le supprimez de l’intérieur de la tombe (à son emplacement naturel), le fichier monté continue d’être présent à son chemin fictif, jusqu’à la fermeture de la tombe (après quoi il disparaît vraiment de la tombe).

## 4. Utilisation avancée
Nous allons maintenant aborder trois cas pratiques pour augmenter la sûreté en situation de mobilité, ou pour faciliter encore l’utilisation quotidienne. D’autres recettes sont proposées sur le site du projet.

### 4.1 Over SSH
Vous l’aurez compris, l’élément critique est bien le fichier qui contient la clé. Une tombe qui passerait entre de mauvaises mains ne pourrait se craquer qu’au prix de l’effort nécessaire à casser une clé. Mais, si l’attaquant se procure la clé, il ne lui reste plus qu’à casser sa phrase de passe, ce qui représente un ordre de grandeur considérablement amoindri.

Il peut être préférable de laisser la clé en lieu sûr, sur une machine distante, plutôt que de la transporter (physiquement) au risque de se la faire subtiliser. Dans ce cas, au moment de s’en servir, on procèdera de la façon suivante :

```
~ $ ssh user@machine 'cat keyfile' | tomb open tombfile -f -k -
```

Notez la commande tomb open avec le tiret final -k - pour lui faire lire la clé sur l’entrée standard. Naturellement la phrase de passe continue de vous être demandée interactivement.

### 4.2 Stéganographie
Une clé est un fichier qui parle de lui-même : un fichier texte contenant un message PGP :

```
~ $ cat my-key-----BEGIN PGP MESSAGE-----
 
1F0IzvnfFXHZq9w3PGTcphN4jw71mu9W3/iMJ1Qrq61uzxLf13ZnN77siXkfSFBt
...
x2fzkGY7uMk6bLpCgjh7gsNHSabrntsaU6xfq8q+skzzbbY5qNw6SAp90LWPPKd7
UB7RJ0kX/yaimg==
-----END PGP MESSAGE-----
```

Si quelqu’un la cherche dans vos documents, elle n’est pas très discrète. Mais Tomb offre la possibilité de la cacher dans une image JPEG, au moyen de la technique dite de stéganographie : l’art de dissimuler un message dans un autre. En effet, il reste dans le format JPEG une certaine place libre pour des données supplémentaires, non utiles à l’encodage de l’image elle-même, mais statistiquement chaotiques pour qui essaierait de les exploiter sans connaître leur nature. L’idée est de se choisir une image inoffensive et facilement mémorisable, afin d’y incruster la clé qui devient alors indécelable à l’œil et peut ainsi être transportée sans éveiller les soupçons.

Il faut auparavant installer l’utilitaire steghide :

```
~ $ sudo apt-get install steghide
```

Suite à quoi deux nouvelles sous-commandes deviennent disponibles pour Tomb. Voici comment enterrer une clé dans une image :

```
~ $ tomb bury -k keyfile image.jpeg
```

Ceci aura pour effet de transformer l’image indiquée (sans créer de copie). Attention, l’image ainsi obtenue est plutôt sensible : pas question de l’éclaircir ou la redimensionner, sous peine de détruire la clé qui s’y cache.

La clé peut ensuite être déterrée pour ouvrir la tombe de cette façon :

```
~ $ tomb exhume image-cle.jpeg | tomb open tombfile -f -k -
```

Vous constaterez que cette opération demande deux fois la phrase de passe : c’est que, pour incruster un document dans un autre, steghide nécessite une phrase de passe. Mais Tomb, au moment où vous faites le bury, reprend automatiquement celle de la clé elle-même. Donc l’image contient un document stéganographique incrusté par votre phrase de passe de la clé, et une fois celle-ci extraite, Tomb ouvre votre tombfile grâce à cette clé pour laquelle il vous demande la phrase de passe.

Si vous préférez utiliser un mot de passe différent pour l’image et pour la clé, vous devrez l’incruster par vous-même :

```
~ $ steghide embed -ef keyfile -cf image-cle.jpeg
```

### 4.3 Plusieurs tombes
On peut vouloir répartir ses dossiers confidentiels sur plusieurs petites tombes avec la même clé, plutôt qu’une seule très volumineuse. Mais cela devient fastidieux de les ouvrir, puisque chaque petite tombe demandera la phrase de passe. Dans ce cas, voici un indice : Tomb sait prendre une phrase de passe en argument. Bien évidemment, hors de question d’écrire la vraie phrase de passe sur la ligne de commandes, et d’ailleurs les options sont explicites :

```
~ $ tomb open tombfile -k keyfile --unsafe --tomb-pwd <clearPassword>
```

(vous ne pourrez pas dire que vous ne saviez pas)

Mais nous pouvons utiliser une tombe intermédiaire, contenant la clé : on chiffre les tombes avec une clé qu’on protège par une phrase de passe légère (et qu’on peut risquer de compromettre) ; on enferme cette clé dans une petite tombe utilitaire, verrouillée par une autre clé qu’on protège, elle, avec la phrase de passe solide.

Ainsi, on ouvre la boîte utilitaire avec la phrase de passe solide (demandée au clavier) ; elle contient une clé intermédiaire avec laquelle on peut scripter l’ouverture des tombes de documents, et on referme aussitôt la tombe utilitaire.

```
~ $ tomb open tomb-key -k keyfile
... Please enter pass phrase ...
~ $ for tombfile in "mytomb1 mytomb2"; do \      
        open $tombfile -k /media/tomb-key/my-key \        
        --unsafe --tomb-pwd weakPassword \    
done
~ $ tomb slam tomb-key
```

## Conclusion
Si le logiciel VeraCrypt [5] a redonné de l’espoir aux nombreux déçus du déchu TrueCrypt [6], il n’offre pas plus que son prédécesseur une intégration satisfaisante avec le shell. À l’opposé, cryptsetup donne la puissance maximale, mais comme souvent avec les outils de bas niveau, sa prise en main est trop ardue.

Tomb s’adresse à un plus grand public, et a prouvé sa souplesse et son efficacité pour un usage quotidien et poussé. Il n’y a plus de raison valable pour s’abstenir de chiffrer sérieusement ses dossiers critiques.

## Références
[1] LUKS : https://gitlab.com/cryptsetup/cryptsetup
[2] Site du projet Tomb : https://www.dyne.org/software/tomb/
[3] https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-217/Zsh-et-PowerLevel9k-pour-un-shell-de-malaaaaade
[4] Page de téléchargement pour l’installation : https://files.dyne.org/tomb/
[5] https://connect.ed-diamond.com/Linux-Pratique/LP-101/Protegez-votre-vie-privee-avec-KeePass-et-VeraCrypt
[6] La mystérieuse fin de TrueCrypt : https://en.wikipedia.org/wiki/TrueCrypt#End_of_life_announcement

source : https://connect.ed-diamond.com/Linux-Pratique/lphs-048/garder-ses-secrets-avec-tomb