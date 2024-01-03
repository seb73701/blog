---
title: Tirez parti de la colorisation pour faciliter la lecture de vos données
description: 
published: true
date: 2023-04-21T11:15:51.826Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:15:47.800Z
---

# Tirez parti de la colorisation pour faciliter la lecture de vos données

> Dans le numéro 93 de Linux Pratique [1], nous vous avions présenté la commande de colorisation hl (disponible sur GitHub [2]) qui permet de coloriser très simplement des fichiers texte ou résultats de commande. Dans cet article, nous allons étudier les nouvelles fonctionnalités, récemment ajoutées, qui rendent cette commande encore plus puissante.

Récemment, de nouvelles options ont été ajoutées afin de donner à cette commande de nouveaux critères de colorisation pour améliorer encore la lisibilité de certains types de textes. Nous allons vous faire découvrir dans cet article ces toutes dernières possibilités, après vous avoir présenté quelques brefs rappels sur les versions précédentes.

## 1. Rappels
### 1.1 Fonctions des précédentes versions de la commande
La commande hl est une commande système, écrite en langage C, permettant de coloriser de 42 couleurs différentes des fragments de textes lus sur son entrée standard et provenant de fichiers ou de résultats de commandes. Les directives de colorisation sont données au moyen d'expressions régulières et d'options de spécifications de couleurs associées.

Cette commande peut servir aux utilisations suivantes :

- mettre en évidence (repérer) des textes fixes (mots clés) ou des textes génériques (grâce à des expressions régulières) dans des fichiers ou des résultats de commandes ;
- afficher de différentes couleurs des valeurs numériques en fonction de seuils exprimés par des expressions régulières ;
- coloriser du texte placé entre deux balises spécifiées ;
- coloriser, sans changement pour l'utilisateur, la sortie de commandes ou de scripts (sans changement de syntaxe, et sans ajout de pipe ('|') sur la ligne de commandes) ;
- coloriser certains champs de fichiers ;
- mettre en évidence, sur des lignes de texte, les valeurs identiques ou différentes de certains éléments (voir [3]) ;
- aider à la compréhension et à la validation d'expressions régulières grâce à la facilité de réalisation des tests.


### 1.2 Téléchargement et génération
Si vous êtes utilisateur de la distribution ArchLinux et que vous ne voyez pas d'inconvénients à utiliser une version un peu ancienne, vous pourrez utiliser le package hl-git 1.69-4 disponible sur [4]. Si vous souhaitez utiliser la dernière version de hl, il vous faudra générer le binaire manuellement. Pour cela, il est nécessaire de disposer des commandes suivantes :

- gcc (compilateur C) ;
- lex (générateur d'analyseurs lexicaux) ;
- make (commande de génération) ;
- git (logiciel de versionnement).

Les fichiers sources de cette commande sont téléchargeables à partir de [2] grâce à la commande suivante :

```
$ git clone https://github.com/mbornet-hl/hl
```

La génération est effectuée ainsi :

```
$ make clean; make
```

Vous pouvez ensuite, si vous le souhaitez, copier la commande hl ainsi générée dans un répertoire de commandes accessibles à tous les utilisateurs :

```
# cp hl /usr/local/bin
```

### 1.3 Intérêt de la colorisation
À l'époque à laquelle nous vivons, nous voyons l'apparition de radiographies médicales en couleurs, et d'images de scanners médicaux en couleurs. Les couleurs sur les images médicales permettent une meilleure lisibilité des informations, ainsi qu'une analyse plus rapide et plus fiable de ces documents. À un degré d'importance beaucoup plus faible, l'introduction automatique de couleurs dans les informations textuelles a également pour but d'améliorer la lisibilité des informations et de permettre un traitement plus rapide et plus fiable.

### 1.4 Ancien usage de la commande hl
Une majorité de versions précédentes de la commande hl avait l'usage suivant :

```
hl: version 1.67
Usage: hl [-h|-H|-V|-[[%.]eiuvdDEL1234][-[rgybmcwRGYBMCWn] regexp ...][--config_name ...] ]
  -h  : help
  -H  : help + configuration names
  -V  : version
  -v  : verbose
  -u  : do not bufferize output on stdout
  -e  : extended regular expressions
  -i  : ignore case
  -E  : print on stderr
  -r  : red
  -g  : green
  -y  : yellow
  -b  : blue
  -m  : magenta
  -c  : cyan
  -w  : white
  -R  : red     (reverse video)
  -G  : green   (reverse video)
  -Y  : yellow  (reverse video)
  -B  : blue    (reverse video)
  -M  : magenta (reverse video)
  -C  : cyan    (reverse video)
  -W  : white   (reverse video)
  -n  : never colorize
  -%c : specifies the beginning of a range colorized in color 'c'
  -.  : specifies the end of the previous range
  -d  : debug
  -D  : display regular expressions
  -L  : lex debug
  -1  : color brightness (half-bright)
  -2  : color brightness (normal : default)
  -3  : color brightness (bright)
  -4  : color brightness (underscore)
```

On peut voir que la syntaxe comporte essentiellement des options à une lettre (ce qui est courant pour un très grand nombre de commandes Unix), et on note la présence d'une option atypique, l'option -%c (où c désigne un spécificateur de couleur) qui permet en un seul argument de spécifier la couleur à utiliser et le fait que le mode de colorisation est différent des autres (puisqu'il s'agit de coloriser tout le texte lu sur l'entrée standard à partir de la balise de début jusqu'à la balise de fin, et non plus le seul texte correspondant à l'expression régulière associée à l'option de spécification de couleur). Les options précédées de deux tirets (--), affichables par hl -H, ne sont pas des options codées en dur dans le programme, mais désignent en fait des noms de configurations définies dans l'un des deux fichiers de configuration (voir [5]). Ces petites singularités relatives aux options allègent la syntaxe de la commande hl en diminuant le nombre de caractères à taper lors de l'utilisation.

## 2. Nouvelles fonctionnalités
### 2.1 Choix de la nouvelle syntaxe
Les versions précédentes de la commande hl rendent de grands services pour peu que l'on dispose des bonnes configurations, ou que l'on sache les créer facilement. Mais une fonctionnalité était absente, alors qu'elle est présente dans un tableur comme Excel : c'est la possibilité de coloriser une ligne sur deux, afin de repérer plus facilement les informations situées sur une même ligne. D'où l'idée d'ajouter à hl cette possibilité très utile. Mais lors des réflexions sur la façon de l'implémenter, il s'est avéré qu'on pouvait faire quelque chose de plus utile encore en implémentant la possibilité de changer de couleur, non pas systématiquement à chaque ligne, mais en fonction d'un critère sélectionné par une expression régulière, et de pouvoir appliquer ce changement de couleur à la ligne entière ou seulement à quelques portions de la ligne, et toujours en conservant une syntaxe relativement facile à utiliser.

Le problème principal de l'implémentation de cette fonctionnalité fut de trouver cette bonne syntaxe... En effet, il fallait pouvoir sélectionner la nouvelle fonctionnalité d'alternance de couleurs (probablement avec une nouvelle option à une lettre non déjà utilisée : à définir), passer les informations permettant de sélectionner la chaîne de caractères servant de critère de changement de couleur, passer les informations permettant de sélectionner les zones à coloriser, et passer les deux spécificateurs des couleurs à utiliser. Donc beaucoup d'informations à passer en paramètres sur la ligne de commandes. Et bien que l'on dispose d'un fichier de configurations permettant d'alléger considérablement les options de la ligne de commandes, il serait bien agréable de pouvoir utiliser facilement et simplement cette nouvelle fonctionnalité sans imposer la création de configurations. Et puis, en réfléchissant bien au problème, on peut aussi se demander pourquoi on devrait se limiter à une alternance sur deux couleurs : pourquoi pas trois ? Ou quatre ? Ou plus ? Pourquoi imposer une limite, d'ailleurs ? Bon, donc finalement il faudrait aussi être capable de passer une liste de couleurs sur lesquelles la commande hl pourrait faire des cycles de changement. Et tout ça devrait être exprimé de la façon la plus simple possible... Pas évident, a priori, et le travail d'implémentation risquait aussi de ne pas être simple... Mais le but d'un développeur qui se respecte (et qui respecte les utilisateurs de ses programmes) n'est pas de simplifier sa propre tâche, mais plutôt de simplifier celle des utilisateurs de ses programmes, pour des raisons évidentes d'effet de levier sur les gains de temps obtenus. Après un certain nombre d'essais de syntaxes, mon choix s'est finalement porté sur la syntaxe suivante :

- une nouvelle option à une lettre pour sélectionner l'alternance de couleurs : -A ;
- un numéro de sous-expression régulière permettant la sélection de la chaîne servant de critère de changement de couleur, accolé à l'option précédente ;
- une liste de couleurs sur lesquelles la commande hl pourra faire des cycles, collées les unes aux autres, et séparées du numéro précédent par une virgule ;
- une expression régulière, en argument de cette nouvelle option (un peu particulière), pour spécifier le critère de changement ainsi que les portions de la ligne à coloriser (toutes de façon uniforme, toutefois, ne compliquons pas les choses inutilement, le but étant d'améliorer la lisibilité des textes, pas de la dégrader).


Voici quelques exemples de ce que donnerait cette syntaxe :

- premier exemple :

```
-A1,3c3C '^([^ ]+) +([^ ]+) *'
```

qui signifierait : alterner les couleurs cyan brillant (3c) et cyan brillant en vidéo inversée (3C) sur les champs 1 et 2 quand le deuxième champ change de valeur. Le chiffre 1 utilisé désigne en fait la deuxième sous-expression, car la numérotation commence à 0.

- deuxième exemple :

```
-A,3c2y3g '(.*)'
```

qui signifierait : alterner les couleurs cyan brillant (3c), jaune normal (2y) et vert brillant (3g) quand la ligne complète change de valeur.

Et comme je souhaite pouvoir alléger la syntaxe le plus possible, je devrais également utiliser des valeurs par défaut pour tous les paramètres absents :

- pas de numéro de sous-expression de sélection, ni d'expression régulière, si l'on souhaite changer de couleur dès qu'un caractère de la ligne change ;
- pas de spécificateur de couleur si l'on ne souhaite utiliser que deux couleurs ;

ce qui donnerait alors simplement :

```
-A
```

Et comme différents utilisateurs auront probablement des choix différents de couleurs par défaut, il faudra un moyen de spécifier ces couleurs par défaut dans des variables d'environnement :

- HL_A1 : premier spécificateur de couleur ;
- HL_A2 : second spécificateur de couleur.

Leur valeur pourra être initialisée de la façon suivante (en utilisant une syntaxe Bourne shell) :

```
HL_A1='2G'; export HL_A1
HL_A2='3g'; export HL_A2
```

ou encore (en utilisant une syntaxe bash) :

```
export HL_A1='2G'
export HL_A2='3g'
```

Voici donc le nouvel usage de la commande hl (voir [6]) :

```
hl: version 1.84
Usage: hl [-h|-H|-V|-[[%.]eiuvdDEL1234][-[rgybmcwRGYBMCWnA] regexp ...][--config_name ...] ]
  -h  : help
  -H  : help + configuration names
  -V  : version
  -v  : verbose
  -u  : do not bufferize output on stdout
  -e  : extended regular expressions
  -i  : ignore case
  -E  : print on stderr
  -r  : red
  -g  : green
  -y  : yellow
  -b  : blue
  -m  : magenta
  -c  : cyan
  -w  : white
  -R  : red     (reverse video)
  -G  : green   (reverse video)
  -Y  : yellow  (reverse video)
  -B  : blue    (reverse video)
  -M  : magenta (reverse video)
  -C  : cyan    (reverse video)
  -W  : white   (reverse video)
  -n  : never colorize
  -%c : specifies the beginning of a range colorized in color 'c'
  -.  : specifies the end of the previous range
  -d  : debug
  -D  : display regular expressions
  -L  : lex debug
  -1  : color brightness (half-bright)
  -2  : color brightness (normal : default)
  -3  : color brightness (bright)
  -4  : color brightness (underscore)
  -A  : alternate colors when string matched by selection regex changes
  -I  : alternate colors when string matched by selection regex does not change
        Syntax for alternate options : -{A|I}[[s],c1c2...cn]
         where s is a number from 0 to 9 indicating the selection regexp number,
         and c1, c2, ... cn are color specifiers to use
        Alternate colors implies extended regular expressions (-e)
Environment variable HL_DEFAULT is undefined.
Environment variable HL_A1 is undefined.
Environment variable HL_A2 is undefined.
```

### 2.2 Exemples utilisant la nouvelle syntaxe
Nous allons maintenant examiner quelques exemples concrets utilisant la nouvelle fonctionnalité de la commande hl.

Pour illustrer une majorité des exemples de cette deuxième partie, nous allons utiliser le script hl_date qui suit afin de générer des lignes de couples (date, heure) pour lesquelles nous pouvons facilement choisir l'incrément de temps entre deux lignes :

```shell
#!/bin/bash
if [ "$1" = "" -o "$2" != "" ]; then
 echo "Usage: $0 increment" >&2
 exit 1
fi
inc="$1"
curr_date=`date '+%s'`
for i in {1..15}
do
 date -d "@$curr_date" '+%Y-%m-%d %H:%M:%S'
 curr_date=$(($curr_date + $inc))
done
```

Le principe de ce script est simple : récupérer la date courante en secondes depuis l'époque (01-01-1970), l'afficher, puis afficher les quatorze dates qui suivent (soit quinze lignes en tout), chacune d'entre elles étant séparée de la précédente par un incrément en secondes, spécifié en argument du script. Selon la valeur de l'incrément, il sera donc possible de faire facilement varier les secondes, dizaines de secondes, minutes, heures, jours... et donc générer quelques lignes avec des fragments de textes identiques, choisis comme on le souhaite.

Commençons donc avec le premier exemple.

#### 2.2.1 Alternance de couleurs à chaque différence dans une ligne
La commande :

```
$ ./hl_date 1 | hl -A
```

nous donne le résultat de la figure 1.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_01.png">

*Fig. 1 : Alternance de couleur à chaque ligne distincte.*

La commande ./hl_date 1 nous permet de générer des lignes toutes distinctes les unes des autres, et la commande hl -A permet de changer la couleur dès qu'une ligne diffère de la précédente : on obtient donc un changement de couleur à chaque nouvelle ligne. Le critère de changement de couleur n'est pas spécifié (pas d'expression régulière ni de numéro de sous-expression) : par défaut, il s'agit d'une différence quelconque avec la ligne précédente. Les couleurs à utiliser ne sont pas spécifiées : si les variables d'environnement HL_A1 et HL_A2 ne sont pas initialisées, les couleurs utilisées seront respectivement 2B (bleu d'intensité moyenne en vidéo inversée) et 1n (pas de colorisation) ainsi qu'on peut le voir dans le fichier source cr_cpri.h sur [2].

#### 2.2.2 Alternance de couleurs sur changement des dizaines de secondes
La commande :

```
./hl_date 3 | hl -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}:[0-9]{2}:[0-9])[0-9]'
```

donne le résultat de la figure 2.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_02.png">

*Fig. 2 : Alternance de couleur à chaque changement des dizaines de secondes.*

La commande ./hl_date 3 permet d'afficher des dates distantes de 3 secondes, donc d'avoir plusieurs dates ayant le même nombre de dizaines de secondes. Les paramètres de la commande hl indiquent :

- que la sous-expression à utiliser comme critère de changement de couleur est la première sous-expression (-A0 : la numérotation des sous-expressions commence à 0) ;
- qu'il faut utiliser les couleurs 2B (bleu d'intensité moyenne en vidéo inversée) et 2w (blanc d'intensité moyenne) ;
- qu'il faut utiliser comme sélecteur de changement de couleur la partie YYYY-mm-dd HH:MM:S de la ligne de données.


Les lecteurs qui n'ont pas l'habitude des expressions régulières pourront être rebutés par celle qui est utilisée en argument parce qu'une expression telle que celle-ci effraie souvent les débutants par son côté relativement incompréhensible, mais quand on sait l'interpréter, on se rend compte qu'elle est en fait très simple :

- le caractère ^ indique que l'on va rechercher une correspondance en début de ligne ;
- les caractères [12] désignent un caractère parmi les chiffres 1 et 2 : le numéro du millénaire (le chiffre Y1 dans l'année constituée des chiffres Y1Y2Y3Y4) ;
- les caractères [0-9]{3} désignent 3 caractères parmi la suite des chiffres de 0 à 9 : les trois chiffres qui suivent le numéro du millénaire (Y2Y3Y4 dans l'année Y1Y2Y3Y4) ;
- le tiret - désigne le caractère tiret ;
- les caractères [0-9]{2} désignent 2 caractères parmi la suite des chiffres de 0 à 9 : les deux chiffres désignant le mois de la date considérée ;
- le . désigne un caractère quelconque ;
- le : désigne le caractère : ;
- les parenthèses ( ( et ) ) vont servir à effectuer une sélection dans le texte analysé.


Les expressions régulières n'ont donc finalement rien d'effrayant quand on les découpe par petits blocs dont on connaît la signification. Si vous souhaitez vous familiariser avec ce sujet qui est au centre du principe de la commande hl, vous pouvez consulter [7] et [8] : avec un peu de lecture appropriée et un peu de pratique, vous deviendrez rapidement un vrai pro des regex !

#### 2.2.3 Variation de l'exemple précédent
La commande :

```
./hl_date 3 | hl -A1,2B2w '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}:[0-9]{2}:[0-9])[0-9])'
```

donne le résultat de la figure 3.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_03.png">

*Fig. 3 : Alternance de couleur à chaque changement des dizaines de secondes, mais avec colorisation de la date complète.*

Maintenant que vous savez analyser une expression régulière, vous voyez que celle de cet exemple diffère de celle de l'exemple précédent par le fait que des parenthèses ont été ajoutées pour encadrer la totalité de la date, y compris le dernier chiffre. Il y a donc deux sous-expressions dans l'expression régulière spécifiée : celle de l'exemple précédent qui permet la sélection, et l'expression totale sélectionnée par les parenthèses nouvellement ajoutées. L'expression sélectionnée par la première parenthèse rencontrée et par la parenthèse fermante associée (sous-expression de numéro 0) ne sont là que pour sélectionner la zone à coloriser, tandis que l'expression sélectionnée par le deuxième groupe de parenthèses (sous-expression de numéro 1) sert à indiquer le critère de changement de couleur, spécifié à l'aide de l'option -A1. On change de couleur quand on change de dizaine de secondes, mais cette fois-ci on colorise tous les caractères, y compris le caractère des unités de secondes qui n'intervient pas dans le critère de changement.

#### 2.2.4 Alternance de couleurs sur changement des minutes
La commande :

```
./hl_date 25 | hl -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}:[0-9]{2}):[0-9][0-9]'
```

donne le résultat de la figure 4.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_04.png">

*Fig. 4 : Alternance de couleur à chaque changement des minutes.*

Le principe de colorisation est similaire à celui de l'exemple 2.2.2, mais la sélection ne porte que jusqu'au dernier chiffre des minutes. De même, la colorisation ne porte pas sur l'ensemble de la date, mais laisse le dernier séparateur : et les deux chiffres des secondes non touchés par la colorisation.

#### 2.2.5 Variation de l'exemple précédent
La commande :

```
./hl_date 25 | hl -A1,2B2w '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}:[0-9]{2}):[0-9][0-9])'
```

donne le résultat de la figure 5.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_05.png">

*Fig. 5 : Alternance de couleur à chaque changement des minutes, mais avec colorisation de la date complète.*

Cet exemple est similaire à l'exemple 2.2.3, mais la sélection porte sur le changement des minutes. La colorisation porte, là encore, sur la date complète.

#### 2.2.6 Alternance de couleurs sur changement des heures
La commande :

```
./hl_date 1500 | hl -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9]'
```


donne le résultat de la figure 6.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_06.png">

*Fig. 6 : Alternance de couleur à chaque changement d'heure.*

#### 2.2.7 Variation de l'exemple précédent
La commande :

```
./hl_date 1500 | hl -A1,2B2w '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9])'
```


donne le résultat de la figure 7.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_07.png">

*Fig. 7 : Alternance de couleur à chaque changement d'heure, mais avec colorisation de la date complète.*

Cet exemple est similaire à l'exemple 2.2.5, mais porte sur le changement des heures. La colorisation porte sur la date complète. Les précédents exemples sont très proches les uns des autres, mais la répétition est intentionnelle, et effectuée ici dans un but pédagogique. Les différences mineures entre les expressions permettent de bien comprendre leurs fonctions, et les résultats visibles sur les différentes figures permettent de fixer les idées sur les différentes possibilités de la commande de colorisation.

#### 2.2.8 Utilisation de deux jeux de couleurs
La commande :

```
./hl_date 1500 | hl \

 -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9]' \

 -A1,3Y3r '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9])'
```


donne le résultat de la figure 8.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_08.png">

*Fig. 8 : Alternance de couleur à chaque changement d'heure, avec une colorisation très différente du reste de la date.*

Dans les exemples précédents, la colorisation du reste de la date (les caractères ne faisant pas partie de la chaîne de sélection) utilisait les mêmes couleurs que celles utilisées pour coloriser le critère de changement de couleur. En spécifiant le même critère de changement de couleur, avec d'autres couleurs, et en demandant la colorisation de la chaîne complète, on peut distinguer visuellement les deux parties de la date. Les couleurs choisies ici sont volontairement très différentes les unes des autres afin de mettre en évidence les possibilités de la commande.

#### 2.2.9 Variation de l'exemple précédent
La commande :

```
./hl_date 1500 | hl \

 -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9]' \

 -A1,1B1w '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}.[0-9]{2}):[0-9]{2}:[0-9][0-9])'
```


donne le résultat de la figure 9.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_09.png">

*Fig. 9 : Alternance de couleur à chaque changement d'heure, avec deux jeux de couleurs pour la colorisation complète de la date.*

Cet exemple reprend le principe de l'exemple précédent, mais avec des jeux de couleurs un peu mieux assortis, permettant de mieux focaliser l'attention sur le critère de changement de colorisation, qui met en évidence les chaînes identiques.

#### 2.2.10 Alternance de couleurs sur changement des jours
La commande :

```
./hl_date 25000 | hl \

 -A0,2B2w '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}).[0-9]{2}:[0-9]{2}:[0-9][0-9]' \

 -A1,1B1w '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}).[0-9]{2}:[0-9]{2}:[0-9][0-9])'
```

donne le résultat de la figure 10.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_10.png">

*Fig. 10 : Alternance de couleur à chaque changement d'heure.*

La commande spécifiée ici porte sur le changement de jour, l'heure étant colorisée de façon moins voyante.

#### 2.2.11 Variation de l'exemple précédent (jeux de 6 couleurs)
La commande :

```
./hl_date 40000 | hl \

 -A0,1B3c2B2b3B1b '^([12][0-9]{3}-[0-9]{2}-[0-9]{2}).[0-9]{2}:[0-9]{2}:[0-9][0-9]' \

 -A1,1G3y2G2y3G1y '^(([12][0-9]{3}-[0-9]{2}-[0-9]{2}).[0-9]{2}:[0-9]{2}:[0-9][0-9])'
```

donne le résultat de la figure 11.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_11.png">

*Fig. 11 : Alternance de couleur à chaque changement d'heure, avec deux jeux de six couleurs chacun.*

Dans cet exemple, nous utilisons deux jeux de couleurs, mais chaque jeu comporte ici six couleurs au lieu de deux.

#### 2.2.12 Sélection des sous-chaînes à coloriser
Dans les exemples précédents, nous avons souvent colorisé la quasi-totalité de la ligne, mais nous avons en fait la possibilité de ne coloriser que des fragments de lignes en utilisant les parenthèses pour sélectionner les zones à coloriser. Si les zones à coloriser se trouvent devant la zone de sélection du critère de changement de couleur, il faut bien sûr ajuster l'index de sélection de ce critère, comme dans l'exemple suivant :

```
./hl_date 40000 | sed 's/^/ABC DEF GHI /' | hl \

 -A2,1B3c2B2b3B1b '^([^ ]+) [^ ]+ ([^ ]+) ([12][0-9]{3}-[0-9]{2}-[0-9]{2}).[0-9]{2}:[0-9]{2}:[0-9][0-9]' \

 -A1,1G3y2G2y3G1y '^([^ ]+ [^ ]+ [^ ]+) ([12][0-9]{3}-[0-9]{2}-[0-9]{2}).([0-9]{2}):([0-9]{2}):([0-9][0-9])'
```

qui donne le résultat de la figure 12.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_12.png">

*Fig. 12 : Colorisation de fragments de lignes.*

On voit sur la figure 12 que les changements de couleur (bleus d'une part, verts et jaunes d'autre part) sont toujours synchronisés, car nous avons utilisé comme critère de changement de couleur la zone sélectionnée par la même expression régulière, repérée par la 3ème sous-expression dans la première expression régulière (donc -A2 puisque la numérotation commence à 0), et repérée par la 2ème sous-expression dans la seconde expression régulière (donc -A1).

Maintenant que nous avons bien étudié l'effet de la sous-expression de sélection du critère de changement de couleur et des sous-expressions de sélection des chaînes à coloriser, nous allons étudier d'autres possibilités de colorisation mettant en jeu des commandes système.

#### 2.2.13 Colorisation de la sortie de vmstat
La commande :

```
vmstat 1 | head -n 15 | hl -A,2B3w
```

permet de coloriser les quinze premières lignes de la sortie de la commande vmstat. Cette commande génère des lignes d'informations relativement longues, et la colorisation alternée permet à l’œil de passer de la fin de la ligne au début (ou vice-versa) très rapidement, sans risquer de sauter accidentellement une ligne.

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_13.png">

*Fig. 13 : Colorisation de la sortie de vmstat.*

L'absence de spécification d'une expression régulière implique l'utilisation de l'expression régulière par défaut  ^(.*)$ et a pour effet d'alterner les couleurs 2B (bleu d'intensité normale en mode vidéo inversée) et 3w (blanc de forte intensité) dès qu'une ligne diffère de la précédente par au moins un caractère.

#### 2.2.14 Identification des groupes de major devices
La commande suivante permet de lister une partie des périphériques (devices) du répertoire /dev en changeant de couleur à chaque changement de major device :

```shell
/bin/ls -l /dev | grep ',' | sort -k5,5g | \

    egrep -v 'tty[0-9][0-9]|vcs[0-9]' | hl -A1,2Y2y '(.* ([0-9]+),.*)' | less -RX
```

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84472/hl_alternate_figure_14.png">

*Fig. 14 : Colorisation des groupes de major devices identiques.*

Dans cet exemple, le critère d'alternance de couleurs est le changement de numéro majeur de périphérique (major device number) sélectionné par la sous-expression [0-9]+ placée devant le caractère ',' .

## Conclusion
Les nouvelles fonctionnalités de colorisation de la commande hl présentées dans cet article vont vous permettre d'améliorer encore la lecture de vos données, qu'elles proviennent de fichiers de données à traiter qu'il vous faudra épurer, de fichiers de logs, ou de résultats de commandes. Après avoir créé vos propres expressions régulières de sélection, avoir créé les configurations vous permettant d'alléger les lignes de commandes, voire même éventuellement les liens vous permettant d'utiliser vos commandes et scripts de façon transparente, vous disposerez de nouveaux outils vous permettant de gagner du temps lors de vos tâches d'analyses de textes. Investir un peu de temps pour maîtriser cet outil peut s'avérer très rentable à l'utilisation, et peut permettre de diminuer le stress en période de crise, quand l'analyse de résultats divers doit être la plus rapide possible. Faites la comparaison entre les versions colorisées et non colorisées, prenez l'habitude de la commande hl, puis passez sur des systèmes sur lesquels elle n'est pas encore installée : vous verrez qu'elle va très rapidement vous manquer !

## Références
[1] https://connect.ed-diamond.com/Linux-Pratique/LP-093/Colorisez-vos-textes-avec-la-commande-hl
[2] GitHub de téléchargement de la commande hl : https://github.com/mbornet-hl/hl
[3] https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-219/Script-Shell-de-comparaison-de-deux-arborescences
[4] Page web de la version 1.69 de hl pour ArchLinux : https://aur.archlinux.org/packages/hl-git/
[5] hl(5) : page de manuel des fichiers de configurations de la commande hl
[6] hl(1) : page de manuel de la commande hl
[7] « Expressions régulières, Syntaxe et mise en œuvre » - Martial Bornet - Éditions ENI - ISBN 978-2-7460-9806-0
[8] regex(7) : page de manuel décrivant la syntaxe des expressions régulières

## Pour aller plus loin
Ceux qui souhaiteront aller plus loin dans la compréhension du fonctionnement interne de cette commande de colorisation pourront lire l'article suivant, qui décrit les fonctions bibliothèque utilisées par la commande hl pour manipuler les expressions régulières : https://connect.ed-diamond.com/GNU-Linux-Magazine/GLMF-189/Gerer-les-expressions-regulieres-en-langage-C.

Le code source de la commande étant disponible sur [2], ils pourront également se plonger dans le fichier cr_main.c pour examiner les fonctions cr_decode_alternate() (décodage des nouvelles options) et cr_read_input() (colorisation des textes en fonction des expressions régulières spécifiées).

source : https://connect.ed-diamond.com/Linux-Pratique/lphs-046/tirez-parti-de-la-colorisation-pour-faciliter-la-lecture-de-vos-donnees

