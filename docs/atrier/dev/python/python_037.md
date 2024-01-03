# Structure du langage - Les Variables

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-10-18</span>


## Usage

Une variable sert à représenter une valeur par un nom.

+ En informatique, une variable est toujours connue, car elle est associée à une case mémoire qui contient sa valeur. Le concept d'inconnue n'existe pas !
+ En informatique, une variable évolue, sa valeur change au cours du temps suivant les actions effectuées durant l'exécution du programme. Lorsque l'on modifie une variable, la nouvelle valeur écrase la précédente et l'ancienne valeur est oubliée. En mathématiques et en physique, les inconnues ont une unique valeur !

## Nommage

Si on doit retenir une seule consigne c'est le nommage car elle est la plus importante dans la vie d'un informaticien. Elle est souvent oubliée, maltraitée, contournée par habitude et cela conduira à l'échec. On parle du choix du nom d'une variable.

Une petite application fait dans les 1 000 à 10 000 lignes de code : une calculette, une horloge, un post-it de bureau. Une application moyenne compte environ 100 000 lignes : un éditeur de texte ou un éditeur d'image basique. Dès que l'on pense à des applications lourdes : un éditeur de texte complet (Word, Office), un navigateur internet, un client e-mail, un logiciel graphique (Photoshop, Illustrator), un site web professionnel, on peut compter plusieurs millions de lignes de code.

La priorité devient tout autre : lisibilité et organisation. Ce sont les deux maîtres mots de l'informaticien. Il doit à tout prix structurer sa pensée, organiser son code et le rendre le plus lisible possible.

Il faut être conscient qu'il y a beaucoup de choses à faire en programmant : mettre ne place une structure logique censée effectuer un traitement pas forcément évident à comprendre, gérer la syntaxe du langage, taper au clavier sans se tromper, lire le sujet, gérer le bruit environnant... Bref, tout cela occupe déjà 90% de notre esprit. En choisissant de prendre des noms de variables anonymes, voir confus, voir ambigus, on ajoute une couche de complexité qui finit par embrouiller noter pensée.

Il ne faut pas sous-estimer les pièges apportés par un certain laisser-aller sur le nommage des variables. Il n'est pas nécessaire de choisir des noms très longs ou des quasi-phrases pour désigner une variable, cela finirait par nuire à la lisibilité et ce serait contre-productif. Mais déjà, vous pouvez faire commencer un nom de variable désignant un prix par `P` ou nommer un vecteur entre deux points `A` et `B` par `vAB`, c'est un bon début ! Nommer judicieusement les variables reste une tâche difficile, mais l'effort en vaut la chandelle.

## Créer des variables !

Il semble aussi difficile, pour les débutants comme pour les programmeurs avancés, de créer une variable pour chaque chose à calculer. Par exemple, il reste fréquent de trouver deux variables pour stocker trois valeurs. Supposons que nous avons deux variables et que nous calculions leur `somme`. Au lieu de créer une troisième variable intitulée somme, vous préférez stocker, le résultat dans la première variable. Premièrement, vous écrasez la valeur qu'elle contenait, pensant que vous ne vous en servirez plus. Ensuite, vous stockez une somme dans une variable dont le nom ne correspond pas à une somme, c'est dangereux. Troisièmement, lorsque vous aurez finalement besoin de la valeur écrasée, il sera impossible d'y avoir accès directement. Il faudra alors effectuer un tour de passe-passe pour la recalculer en faisant une soustraction. Tout cela est épuisant et dangereux.

Voici un exemple :

```python
(1) t = 18
(2) t = t * (1+k)
(3) p = p + t
(4) Afficher( t / (1+k) , t , p)
```

Ici la version lisible avec un nommage clair :

```python
(1) Prix = 18 # prix hors taxe
(2) Prix = Prix * (1+TVA)
(3) Somme = Somme + Prix
(4) Afficher( Prix, Prix/(1+TVA), Somme)
```

Dans ce code, le prix hors taxe défini à la ligne 1 est écrasé à la ligne 2 pour être remplacé par le prix TTC. La valeur 18 n'existe plus et lorsque le programmeur veut l'afficher à la ligne 4, il doit faire le calcul en divisant par la grandeur (1+TVA). Voici la version utilisant trois variables, pas vraiment plus longue, mais où tout coule de manière limpide entre chaque étape :

```python
PrixHT = 18
PrixTTC = PrixHT*(1+TVA)
Somme = Somme + PrixTTC
Afficher( PrixHT, PrixTTC, Somme)
```

## Affectation
Il s'agit de l'opération la plus fréquente dans un programme. Elle consiste à associer un nom à un objet. Dans le langage Python, un objet désigne un nombre, une chaîne de caractères, mais aussi des choses plus complexes comme une date, une liste de nombres ou encore des images... En Python, il y a aucune différence de traitement entre ces différents types d'objets. Par souci de simplicité, nous présentons uniquement des exemples avec des nombres, mais tout ce que nous énonçons ici s'applique à toutes les autres catégories d'objets.

L'affectation est une opération se lisant de droite à gauche : on prend une valeur à droite du symbole = pour l'associer à la variable à gauche de ce symble. Ainsi, dans la partie gauche d'une affectation, on doit uniquement trouver le nom d'une variable. Dans la partie de droite, on peut avoir :

+ **Une valeur** : comme le nombre 12 ou la chaîne de caractères "Bonjour".
+ **Une variable** : on va donc lire la valeur à laquelle cette variable est associée.
+ **Une expression** : une succession de calculs fournissant un résultat, par exemple `abs(a+3)`.

> Une expression se compose de valeurs, de variables, de fonctions (valeur absolue, racine carrée...) ou d'opérations (+,-,/...). Il n'y a pas de limite à la longeur d'une expression, elle pourrait faire plusieurs pages! 

> Une variable à un sens différent suivant qu'elle se trouve à gauche ou à droite du symbole `=`. Si la variable se trouve à gauche, elle désigne la variable qui sera associée au résultat de l'expression à droite du symbole. Si la variable se trouve à droite, elle sera automatiquement remplacée par la valeur à laquelle elle est associée. Ainsi en écrivant `a = b + 5`, on remplace de suite la variable `b` par la valeur à laquelle elle est associée, par exemple 7. Ensuite, on évalue l'expression en effectuantle calcul, puis on associe à la valeur 12 à la variable `a`. A aucun moment, l'ancienne valeur associée à la variable `a` n'est utilisée.

> A tort, ou à raison, le symbole `=` a été choisi pour récupérer l'affectation. Mais il est inadéquat, car en mathématiques on peut écrire aussi bien `a=b` ou `b=a`. Dans l'ensemble des langages informatiques, le symbole `=` se lit de droite à gauche. On évalue d'abord l'expression à droite et on l'associe au nom de variable se trouvant à gauche. Le symbole flèche gauche `<--` aurait été plus judicieux. Il est d'ailleurs présent dans les langages de certaines calculatrices scientifiques.

Quelles sont les actions effectuées lors de l'exécution de cette ligne ?

```python
prix_ticket = 10 * 5 + 3
```

+ **Analyse du nom de la variable prix_ticket** : deux options sont possibles.
Soit ce nom est inconnu, et dans ce cas, l'interpréteur crée une nouvelle variable portant ce nom. Soit il est connu, ce qui sous-entend que la variable est déjà associée à quelques chose.
+ **Evaluation de l'expression** : à droite du symbole `=`, on trouve un calcul.
L'interpréteur va d'abord évaluer cette expression et trouver la valeur 53 correspondant à un nombre entier.
+ **Réservation de la mémoire** : la valeur 53 est stockée dans le processeur comme elle serait affichée sur votre écran de calculette. Au prochain calcul, elle risque de disparaître, il faut donc lui réserver une place en mémoire. Ce sera dans la mémoire RAM de l'ordinateur.
+ **Stockage** : maintenant qu'une cellule mémoire est réservée, la valeur 53 est écrite à l'intérieur.
+ **Association** : une liaison entre la variable `prix_ticket` et la cellule mémoire contenant la valeur 53 est créée.

Les noms de variables doivent respecter une dizaine de règles. Nous donnons les plus importantes ici :
+ Le nom ne doit pas commencer par un chiffre !
+ Le nom ne doit pas contenir de caractères spéciaux : `! % @ # + = \ ( ) [ ] = & : . , ; $`..., exception faite du trait souligné appelé underscore "`_`" présent sur la touche `[8]` du clavier.
+ Le nom peut contenir des caractères accentués du langage courant : `é ù ç`. Cependant, comme il est difficile de gérer tous les caractères de toutes les langues existantes, on a tendance à utiliser seulement les 26 lettres de l'alphabet, le symbole souligné `_` et les chiffres car ils sont présents sur tous les claviers européens.

## Les types des variables

Durant l'affectation, le type des variables est choisi automatiquement en fonction de ce qu'on trouve à droite du symbole `=`. Nous utilisons la fonction Python `type()` afin de savoir ce qui a été décidé. La fonction `print()` permet d'afficher le résultat à l'écran :

```python
>>> a = 1
>>> print(type(a))
<class 'int'>
>>> a = 1.0
>>> print(type(a))
<class 'float'>
>>> a = 'Bonjour'
>>> print(type(a))
<class 'str'>
```

> Dans la mémoire RAM de l'ordinateur, on stocke l'information dans des cases appelées octets. Chaque octet correspond à un paquet de 8 bits chacun pouvant valoir 0 ou 1. Ainsi, pour stocker chaque type d'information : du texte (`string`), des nombres entiers (`integer`), des nombres à virgule (`float`), des dates... il va falloir encoder chaque données sous forme de 0 et de 1 suivant une norme donnée. Pour les nombres à virgule, on utilise la norme IEEE 754 par exemple. Pour le texte, on utilise le standard Unicode lié à la norme ISO/CEI 10646 afin de permettre des échanges de texte dans différentes langues au niveau mondial. Pour la représentation des couleurs à l'écran, on utilise le standard sRGB (Red Green Blue) défini par la norme CIE 61966. Vous l'aurez compris, il y a une norme pour tout !

### Quand utiliser un type entier

C'est une très bonne question ! En effet, si l'on peut utiliser des nombres à virgule, on peut se passer des nombres entiers ! En fait, tout dépend...

+ Si vous traitez des prix par exemple, vous pouvez stocker toutes vos données dans des nombres à virgule, même si un des prix est un chiffre rond. En effet, si vous avez les valeurs 12€99, 10€30 et 12€, vous n'avez aucun interêt à choisir des types de stockage différents en fonction de la valeur. Vous pouvez choisir le type nombre à virgule pour représenter l'ensemble des prix et c'est une sage décision.
+ Lorsque vous voulez désigner un élément dans une liste, prendre un nombre à virgule est maladroit. Si vous voulez désigner le deuxième prénom dans la liste ['Bob','Alice','Edouard'], cette position doit être décrite par un nombre entier. Vous ne demanderiez jamais l'élément se trouvant à la position 2.34 !
+ Pour les variables qui comptent des choses, cela peut poser problème de choisir un nombre à virgule. Si vous comptez le nombre de filles dans une classe et que vous vouliez savoir s'il y a autant que de garçons, vous allez comparer deux nombres à virgule en faisant par exemple : 18.00000 est-il égal à 18.00000 ? Or, un nombre à virgule en informatique a une précision limitée, et avec 1/1 000 000 de précision, la comparaison précédente peut être vraie ou fausse suivant les erreurs de précision accumulées durant l'exécution. Les tests d'égalité sur les nombres à virgule sont une source d'erreurs aléatoires fréquentes !

Préférez un nombre entier pour comparer des valeurs avec le test d'égalité.

### Les opérations entre types différents
Si on additionne un nombre à virgule (`float`) et un nombre entier (`integer`) ! De quel type sera le résultat ? Le principe suivant s'applique : le résultat correspondra au type `float`, c'est le nombre à virgule qui l'emporte sur le nombre entier (`integer`). Le mécanisme est automatique. Il s'agit du choix par défaut et il se révélera dans la pratique le plus adéquat.

```python
>>> a = 1
>>> b = 3.14
>>> c = a + b
>>> print(type(c))
<class 'float'>
>>> print(c)
4.140000000000001
```

> Remarque :
> Si on affecte une opération `+`, `-` ou `*` entre un type entier (`integer`) et un type `float`, le résultat final sera un type `float`.

Le résultat d'une opération entre deux nombres entiers (`integer`) reste entier. Cependant, il existe un cas en Python ou l'opération entre deux nombres entiers est un nombre à virgule. Il s'agit de la division. Pour éviter toute ambiguïté, on peut utiliser le symbole double de la division `//` qui force le résultat obtenu à être un nombre entier :

```python
>>> a = 9
>>> b = a / 2
>>> print(b)
4.5
>>> print(type(b))
<class 'float'>

>>> b = a // 2
>>> print(b)
4
>>> print(type(b))
<class 'int'>
```

### Convertir un type en un autre

Il est possible de transformer une chaîne de caractères en nombre. La fonction pour effectuer ce travail est `int()`:

```python
>>> a = '123'
>>> b = int(a)
>>> print(b + 1)
124
>>> print(type(b))
<class 'int'>
```

Traitez une chaîne de caractères correspondant à un nombre à virgule, il faut utiliser la fonction `float()` :

```python
>>> a = '123.5'
>>> b = float(a)
>>> print(b + 1)
124.5
>>> print(type(b))
<class 'float'>
```

> Remarque :
> A noter qu'une fois converties en valeurs numériques, on peut manipuler les quantités comme bon nous semble avec les opérations arithmétiques habituelles.

Convertir un nombre à virgule en valeur entière. Pour cela, la fonction `int()` est utile :

```python
>>> a = 123.5
>>> b = int(a)
>>> print(b)
123
>>> print(type(b))
<class 'int'>
```

Convertir un nombre entier ou un nombre à virgule en chaîne de caractères, la fonction `str()` est disponible :

```python
>>> a = 7
>>> b = 3.14
>>> c = str(a)
>>> d = str(b)
>>> print(type(c))
<class 'str'>
>>> print(type(d))
<class 'str'>
>>> print(c,d)
7 3.14
```

En résumé, les fonctions de conversion depuis un type vers un autre type sont présentées dans le tableau suivant :

|De / Vers|Nombre entier|Nombre flottant|Chaîne de caractères|
| ------- | :---------: | :-----------: | :----------------: |
|Nombre entier||`float()`|`str()`|
|Nombre flottant|`int()`||`str()`|
|Chaîne de caractères|`int()`|`float()`||

## Les opérations sur les chaînes de caractères

### Le standard Unicode

Le standard Unicode maintient une table de code contenant plus de 100.000 caractères et autres symboles, ceci pour environ une centaine de systèmes d'écritures. Les valeurs comprises entre `0` et `128` sont similaires aux valeurs de la table de caractères ASCII apparue dans les années 1960 et encore largement utilisée dans les ordinateurs jusque dans les années 2000. Dans ces tables, la valeur `32` correspond au caractère `espace`, la plage de valeurs `48` à `57` correspond aux chiffres de `0` à `9`, la plage `65` à `90` donne les lettre majuscules de `A` à `Z` et la plage `97` à `122` correspond aux lettres minuscules de `a` à `z`.

Il existe deux fonctions `chr()` et `ord()` qui permettent de convertir un code en caractère, et inversement, en suivant le standard Unicode. Ainsi si on fait une recherche rapide sur Internet, on trouve que le symbole `smiley` correspond au code `128512`, et que l'écriture de ce code en hexadécimale est `0x1F600`. en langage Python, il n'y a pas de type de caractère comme dans d'autres langages. Les fonctions retournant un caractère retournent ainsi une chaîne avec ce seul caractère à l'intérieur.

Voici un exemple :

```python
>>> A = chr(128512)
>>> B = chr(0x1F600)
>>> print(A)
😀
>>> print(B)
😀
>>> print(type(A))
<class 'str'>
>>> print(ord("A"))
65
>>> print(ord("0"))
48
>>>
```

### Concaténer deux chaînes

Le type chaîne de caractères permet d'utiliser l'opération `+` pour accoler deux chaînes en une.

```python
>>> a = 'Bonjour'
>>> b = 'toi'
>>> c = a + ' ' + b
>>> print(c)
Bonjour toi
```

On peut tenter d'ajouter un nombre à une chaîne de caractères. Plusieurs options sont possibles. Soit le langage est plutôt souple et permissif et il va convertir automatiquement le nombre en chaîne et l'accoler à l'autre chaîne. Soit le langage est strict et il va donner un message d'erreur.

```python
>>> a = 'Bonjour'
>>> b = 1
>>> c = a + ' ' + b
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

Le langage Python a donc un comportement strict sur ce point. Il suffit d'utiliser une fonction qui transforme un entier en chaîne de caractères à savoir la fonction `str()` :

```python
>>> a = 'Bonjour'
>>> b = 1
>>> c = a + ' ' + str(b)
>>> print(c)
Bonjour 1
```

### Connaître la longueur d'une chaîne

On peut connaître la longueur d'une chaîne de caractères en utilisant la fonction `len()` :

```python
>>> A = 'Bonjour'
>>> print(len(A))
7
```

### Accéder à un caractère et à une sous-chaîne

On peut accéder à chaque caractère en écrivant son indice entouré d'une paire de crochets. L'indice `0` est associé au premier caractère en partant de la gauche. Ainsi le dernier caractère à droite porte l'indice `6` dans l'exemple précédent. La notation `A[1]` désigne le deuxième caractère en partant de la gauche, soit la lettre "**o**". Pour désigner le dernier caractère, on peut écrire l'indice `-1`, qui correspond à la première lettre en partant de la droite, soit "**e**" dans cet exemple. L'indice `-2` correspond à la deuxième lettre en partant de la droite, et ainsi de suite :

```python
>>> A = "Bonjour"
>>> print(A[0])
B
>>> print(A[1])
o
>>> print(A[-1])
r
>>> print(A[-2])
u
```

Le langage Python permet d'utiliser des plages d'indices pour extraire une sous-chaîne. L'écriture `0:2` signifie que l'on prend tous les caractères de la position `0` jusqu'à la position `2` non incluse, ce qui donne la chaîne "**Bo**". Lorsque l'on écrit la plage d'indices `2:-2`, on extrait les lettres à partir de la troisième position en partant de la gauche jusqu'à la deuxième position non comprise en partant de la droite on obtient ainsi la chaîne "**njo**". Pour afficher les caractères à partir de la lettre "**j**" jusqu'à la fin, il faut écrire la plage `3:` sans préciser d'indice après le symbole. Et pour sélectionner les premiers caractères jusqu'à la lettre "**n**" d'indice `2`, on utilise la plage `:3` sans préciser d'indice de départ.

```python
>>> A = "Bonjour"
>>> print(A[0:2])
Bo
>>> print(A[2:-2])
njo
>>> print(A[3:])
jour
>>> print(A[:3])
Bon
```

### Transformer une chaîne en majuscules/minuscules

Il est possible d'effectuer des opérations de transformation sur les chaînes. On peut citer par exemple les fonctions `upper()` et `lower()` qui retournentune chaîne composée uniquement de majuscules ou de minuscules respectivement. On accéde à ces fonctions à travers une variable de type chaîne en utilisant la syntaxe `LaChaine.LaFonction()`.

Contrairement à la fonction `len()` qui s'applique sur une variable : `len(chaine)`, ces fonctions sont appelées depuis une variable avec l'utilisation du symbole `.` placé à droite du nom de la variable.
Voici quelques exemples :

```python
>>> A = "Salut !!"
>>> print(A.upper())
SALUT !!
>>> print(A.lower())
salut !!
```

> Remarque
> Les fonctions `upper()` et `lower()`, ne modifient pas la chaîne en cours, elles retournent une nouvelle chaîne, résultat du traitement.


### Rechercher et remplacer une chaîne

La fonction `find()` permet de rechercher une chaîne à l'intérieur d'une autre chaîne et de retourner sa position. Dans la chaîne "Bonjour", si nous recherchons la chaîne "Bon", nous la trouvons en tout début, c'est-à-dire à l'indice **0**. De la même manière, la chaîne "jour" est détectée à la quatrième lettre, donc à l'indice **3**. si aucune correspondance n'est trouvée, la valeur **-1** est retournée.

```python
>>> A = "Bonjour !"
>>> print(A.find("Bon"))
0
>>> print(A.find("jour"))
3
>>> print(A.find("toto"))
-1
```

On peut remplacer une certaine partie de la chaîne par une autre grâce à la fonction `replace()` :

```python
>>> A = "Bonjour !"
>>> B = A.replace("Bon","Abat-")
>>> print(A)
Bonjour !
>>> print(B)
Abat-jour !
```

> Remarque
> La fonction `replace()` retourne une nouvelle chaîne sans modifier la chaîne originale, comme les fonctions `upper()` et `lower()`.