# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Introduction</span>

Avec Python, tout est objet, au sens informatique du terme, y compris les types de données (les nombres, les chaînes de caractères, les fonctions...).

Voici une description des types de données les plus courants :

- les booléens
- les numériques
- les alphanumériques
- les conteneurs ou séquentiels


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les booléens</span>

Dans les premières versions de Python, le type booléen n'existait pas et on utilisait simplement 0 pour False (Faux) et 1 pour True (Vrai).

Ce type booléen contient deux constantes : `True` (Vraie) et `False` (Faux).

Tout objet peut être comparé à une valeur booléenne et quasiment tout est considéré comme Vraie,sauf :

- les constantes `False` et `None`
- la valeur 0
- la chaîne de caractères vide ('')
- les listes tuples et autres conteneurs vides ( `[]` `{}` `()` ... )

Utilisé dans la recherche de la vérité, le type booléen est associé à trois opérateurs et à huit formes de comparaisons.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les opérateurs booléens</span>

Les opérateurs classiques sont `or`,`and` et `not`.

L'opérateur le plus simple `not` renvoi un booléen.

`not` X renvoi `False` si X est Vrai, sinon retourne `True`.

> Par contre, attention avec les opérateurs `and` et `or` : ceux-ci ne renvoient pas systèmatiquement un booléen.
{.is-warning}

|Opérations|Résultats|
|:--------:|:--------|
|`X or Y`|Si **X** est faux, retourne **Y**, sinon, retourne **X**|
|`X and Y`|Si **X** est faux, retourne **X**, sinon retourne **Y**|


Voici le tableau croisé des résultats pour `X or Y`.

5 et 8 sont des entiers pris au hasard, le but est d'illustrer avec une valeur numérique quelconque.

|X `OR` Y|Y = `True`|Y = `False`|Y = 8|Y = 0|Y = `()`|Y = `[]`|Y = `{}`|
|:----:|:------:|:-------:|:---:|:---:|:----:|:----:|:----:|
|**X = True**|`True`|`True`|`True`|`True`|`True`|`True`|`True`|
|**X = False**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = 5**|5|5|5|5|5|5|5|
|**X = 0**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = `()`**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = `[]`**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = `{}`**|`True`|`False`|8|0|`()`|`[]`|`{}`|

Et voici pour l'opérateur `and` :

|X `AND` Y|Y = `True`|Y = `False`|Y = 8|Y = 0|Y = `()`|Y = `[]`|Y = `{}`|
|:----:|:------:|:-------:|:---:|:---:|:----:|:----:|:----:|
|**X = True**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = False**|`False`|`False`|`False`|`False`|`False`|`False`|`False`|
|**X = 5**|`True`|`False`|8|0|`()`|`[]`|`{}`|
|**X = 0**|0|0|0|0|0|0|0|
|**X = `()`**|`()`|`()`|`()`|`()`|`()`|`()`|`()`|
|**X = `[]`**|`[]`|`[]`|`[]`|`[]`|`[]`|`[]`|`[]`|
|**X = `{}`**|`{}`|`{}`|`{}`|`{}`|`{}`|`{}`|`{}`|

En utilisation courante, avec les instructions `if` ou `while`, cela ne change pas grand-chose.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les comparaisons logiques</span>

Il y a huit comparateurs en langage Python.

|Comparateur|Description|
|:---------:|:----------|
|`>`|Supérieur à|
|`<`| Inférieur à|
|`>=`|Supérieur ou égal à|
|`<=`|Inférieur ou égal à|
|`==`|Egal à|
|`!=`|Différent de|
|`is`|Le même objet ?|
|`is not`|N'est pas le même objet|

Sauf pour les types numériques, les objets de types différents ne peuvent être égaux, la plupart des comparaisons rendront `False`.

Par contre, il n'est pas possible de démontrer la supériorité d'un type de données sur un autre, on n'obtient qu'une erreur de type (`TypeError`).

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les numériques</span>

Trois grandes familles de numériques :
- les entiers
- les flottants
- les complexes

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les entiers</span>

Les entiers dans Python version 3 ont une précision illimitée, il y a plus d'entier court et d'entier long, juste des entiers.

> Le module `sys` permet l'accès à des variables système maintenues par l'interpréteur.
{.is-info}

Dans ce module se trouve une valeur qui se nomme `maxsize` ; pour l'obtenir, il faut importer le module `sys` et afficher la valeur :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> import sys
>>> print(sys.maxsize)
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
9223372036854775807
``` 

Mais ce n'est pas une limite, car il est possible de faire la chose suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> maximum = sys.maxsize
>>> print(maximum+1)
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
9223372036854775808
``` 

Ce nombre `maxsize` correspond à l'entier maximum pour le format de stockage par défaut du langage Python, mais si le besoin est plus important, pas de problème. Par contre, les opérations sur les entiers seront un peu plus longues.

> Effectuer des calculs en dehors des limites des nombres entiers ou sur des nombres flottants, permet avec un simple script Python de pousser l'utilisation d'un processeur à 100%, pour faire des tests par exemple.
{.is-info}

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les flottants</span>

Les nombres flottants en Python sont issus du type double du langage C, et on peut trouver dans la constante `sys.float_info` les informations spécifiques de ce type en fonction du matériel.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> print(sys.float_info)
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
sys.float_info(max=1.7976931348623157e+308, max_exp=1024, max_10_exp=308, min=2.2250738585072014e-308, min_exp=-1021, min_10_exp=-307, dig=15, mant_dig=53, epsilon=2.220446049250313e-16, radix=2, rounds=1)
```

Pour utiliser des flottants, il suffit d'ajouter un `.` ou `.0`.

_Exemple_

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> type(1)
<class 'int'>
>>> type(1.)
<class 'float'>
>>> type(1.0)
<class 'float'>
```

Python permet de flirter avec l'infini et au-delà avec le type flottant et les syntaxes suivantes :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> float('inf')						## pour l'infini positif
inf
>>> float('-inf')						## pour l'infini négatif
-inf
```

Et pour au-delà de l'infini, utilisez la syntaxe suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> float('nan')						## nan = Not A Number
nan
```

Le type flottant permet d'effectuer sans problème les opérations classiques de base.

Mais si l'on souhaite aller plus loin, il existe des modules et librairies complémentaires : `Decimal`, `math`, `NumPy`, etc. 

> Le module `Decimal` est prévu pour fournir le même modèle mathématique que celui appris à l'école.
{.is-info}


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les opérations</span>

Python dispose des quatres opérations de base, mais aussi de quelques autres bien pratiques.


<table>
  <tr>
  	<th>Opération</th>
    <th>Description</th>
    <th>Exemple</th>
  </tr>
	<tr>
    <td style="font-weight:bold;text-align:center;">-X</td>
    <td>Négation</td>
    <td><code>>>> a = 1</code><br><code>>>> -a</code><br><code>-1</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">+X</td>
    <td>Ne change rien</td>
      <td><code>>>> a = 1</code><br><code>>>> +a</code><br><code>1</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">X % Y</td>
    <td>Modulo, reste de x/y</td>
      <td><code>>>> 10 % 3</code><br><code>1</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">X // Y</td>
    <td>Division entière de X/Y</td>
      <td><code>>>> 10 // 3</code><br><code>3</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">abs(X)</td>
    <td>Valeur absolue de X</td>
    <td><code>>>> abs(-5)</code><br><code>5</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">pow(X, Y) ou X**Y</td>
    <td>X puissance Y</td>
    <td><code>>>> 2**6</code><br><code>64</code><br><code>>>> pow(2,6)</code><br><code>64</code>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">divmod(x,y)</td>
    <td>Division entière et reste de la division</td>
    <td><code>>>> divmod(10,5)</code><br><code>(2,0)</code></td>
  </tr>
  	<tr>
    <td style="font-weight:bold;text-align:center;">round(x[,y])</td>
    <td>Arrondi à y chiffres après la virgule.<br>Si le dernier chiffre après la virgule est > 5, cela arrondit au chiffre supérieur.<br>Si y est omis, cela vaut pour 0 chiffre après la virgule.</td>
    <td>
      <code>>>> round(10/3,2)</code><br><code>3.33</code><br>
      <code>>>> round(10/3)</code><br><code>3.333333333333335</code><br>
      <code>>>> round(5/3,2)</code><br><code>1.67</code><br>
      <code>>>> round(1.552,2)</code><br><code>1.55</code><br>
      <code>>>> round(1.556,2)</code><br><code>1.56</code><br>
    </td>
  </tr>
</table>

Les conversions

|Opération|Description|Résultat|
|:-------:|:----------|:-------|
|`int(x)`|Permet la conversion d'un type de données en entier, si possible|`int("1")` donnera 1 ; par contre, `int("1.0")` déclenche une erreur|
|`float(x)`|Permet de convertir un type de données en flottant, dans la mesure du possible| |

Il existe également des modules dédiées pour les calculs scientifiques ou l'utilisation de matrices (https://scipy.org)

Dernier point : quand on utilise une formule un peu longue, comme tout le monde ne connaît pas forcément les priorités sur les opérations arithmétiques, il est préférable d'utiliser les parenthèses.

Exemple : 

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> x = 1
>>> y = 2
>>> z = 3
>>> x * 3 - y + z - 4
0
>>> (x * 3) - ((y + z) - 4)
2
```

Danns le premier cas, Python interprète dans l'ordre de gauche à droite :

```shell
x * 3 = 1 * 3 = 3
- y   = 3 - 2 = 1
+ z   = 1 + 3 = 4
- 4   = 0
```

ce qui peut être très différent du résultat attendu.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les alphanumériques</span>

Voici le moment d'étudier les chaînes de caractères en Python.

Il s'agit du type "string", à la différence des numériques, qui sont des types de données composés d'un seul élément, il est un type de données séquentielle (d'où l'image des maillons d'une chaîne).

Le langage permet d'utiliser une chaîne comme un objet unique, la chaîne dans sa globalité, mais aussi d'accéder à chaque élément (les caractères) de cet objet.

A savoir : la taille d'une chaîne de caractères n'a pas de limite que la mémoire physique allouable.

La création d'une chaîne de caractères est simple, ce type de données doit juste être délimité par un simple guillemet (quote) `'` ou un double guillement `"` et il est même possible d'utiliser un triple double guillemet `"""`.

Exemple : 

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> a = "hello world"
```

L'utilisation des doubles guillemets ou des simples guillemets n'a pas d'importance, sauf quand il s'agit de mixer les deux.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> a = "Voici un cas typique d'utilisation des doubles quotes"
>>> #                          ^
>>> # -------------------------|
>>> 
>>> a = ' je lui ai dit "Attention aux doubles quotes" '
>>> #                   ^                            ^
>>> # ------------------|----------------------------|
```

Les triples guillemets permettent de saisir du texte sur plusieurs lignes, ce qui peut être très pratique.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> a = """
... Hello the world !
... ============
... """
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Les opérations applicables aux chaînes de caractères</span>

<table>
  <tr>
  	<th>Opération</th>
    <th>Résultat</th>
    <th>Description</th>
  </tr>
	<tr>
    <td><code>"Hello" + "World"</code></td>
    <td><code>"HelloWorld"</code></td>
    <td>Concaténation</td>
  </tr>
  <tr>
    <td><code>"_O_" * 3</code><br><code>"=" * 5</code></td>
    <td><code>"_O__O__O_"</code><br><code>"====="</code></td>
    <td>Répétition</td>
  </tr>
  <tr>
    <td>
      <code>"0123456789"[5]</code><br>
      <code>"ABCDEF"[3]</code><br>
      <code>"0123456789"[0]</code><br>
      <code>"0123456789"[-1]</code><br>
      <code>"0123456789"[-2]</code>
    </td>
    <td>
      <code>"5"</code><br>
      <code>"D"</code><br>
      <code>"0"</code><br>
      <code>"9"</code><br>
      <code>"8"</code>
    </td>
    <td>Accès par index<br>Index négatif partant de la fin</td>
  </tr>
  <tr>
    <td><code>"0123456789"[2:6]</code></td>
    <td><code>"2345"</code></td>
    <td>Découpage / Slicing</td>
  </tr>
  <tr>
    <td><code>for x in "ABCD" :<br>&nbsp;&nbsp;&nbsp;print(x)</code></td>
    <td>
      <code>"A"</code><br>
      <code>"B"</code><br>
      <code>"C"</code><br>
      <code>"D"</code><br>
    </td>
    <td>Itération</td>
  </tr>
  <tr>
    <td>
      <code>"5" in "0123456789"</code><br>
      <code>"A" in "0123456789"</code><br>
      <code>"234" in "0123456789"</code>
    </td>
    <td>
      <code>True</code><br>
      <code>False</code><br>
      <code>True</code>
    </td>
    <td>Appartenance / est dans</td>
  </tr>
</table>


---  
  
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Titre2</span>

(texte)

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
(code)
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
(code)
```  
  
  
Ajouter le contenu ci-dessous dans le fichier :


<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*/etc/systemd/system/wikijs.service*</span>
  
```ini
#########################
#    WIKIJS             #
#    SERVICE            #	
##########################

```