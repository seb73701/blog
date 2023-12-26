# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les opérateurs</span>
  
Python dispose de tous les opérateurs dont le langage moderne a besoin.
  
Une petite précision sur l'opérateur d'affectation : celui-ci a une particularité qu'il est nécessaire de connaître.
  
`=` est l'opérateur d'affectation en Python, à ne surtout pas confondre avec `==`, qui est un opérateur de comparaison  

`=` permet d'affecter à une variable toute expression syntaxiquement correcte en langage Python.
  
_Exemple :_

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>  
  
```python
>>> a = 1
>>> b = "COUCOU"
```

> Mais l'affectation en Python peut être multiple et même parallèle.
{.is-info}

_Exemple d'affectation multiple_

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>  
  
```python
>>> a = b = c = 1
>>> print(a,b,c)
```  

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
1 1 1
```  

L'affectation parallèle est très souvent utilisée, voire même essentielle, dès que les types de données séquentielles (listes et dictionnaires) auront été abordés.
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>  
  
```python
>>> texte, valeur = ['la réponse est', 42]
>>> print(texte, valeur)
```
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```python
la réponse est 42
```   
  
Quelques explications :
- A gauche du signe `=` : deux variables séparés par une virgule `,`.
- A droite du signe `=` : une liste en Python, un objet permettant de stocker d'autres objets ; le crochet droit `[` permet de commencer la liste, un crochet gauche pour terminer la liste, et entre les deux viennent les composants séparés par une virgule.
  
Une chaîne de caractères, délimitée par des simples quotes (ou guillements), une virgule comme séparateur, et un chiffre.
  
Le premier objet de la liste sera affecté à la première variable, le deuxième à la deuxième variable, etc.
  
> Attention, cependant : s'il n'y a pas assez de valeurs ou de variables, ou trop de valeurs et de variables, bref, si l'équilibre n'est pas respecté, cela déclenchera une erreur de syntaxe.
{.is-warning}

_**Nouveauté de la version 3.8**_
L'opérateur **Walrus**, ou tête de morse en français, `:=` (deux points égal) permet d'affecter et d'utiliser une variable en même temps.
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> print(" TEST OP MORSE", num := 2500)
 TEST OP MORSE 2500
>>> num
2500
```

Tout de suite, on voit l'utilisation suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> while saisie := input ("Votre choix"):
...     if saisie == "QUIT":
...             break
...     print(" => %s " % saisie)
```

au lieu de :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> while CONTINUE:
...     saisie = input("Votre choix : ")
...     if saisie == "QUIT":
...             CONTINUE = False
...     print("=> %s " % saisie)
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les variables</span>

Pour nommer les variables, il faut utiliser les lettres, les chiffres et le caractère `_` (espace souligné / underscore).

> Attention, la casse des caractères est prise en compte, `ma_variable` est `Ma_Variable` sont deux références différentes.
{.is-warning}

Mais elles ne peuvent pas commencer par un chiffre, et il existe des mots réservés par le langage, dont voici la liste.

Mots réservés :

```python
and del from none true as elif global nonlocal try assert else if not while break except 
import or with class false in pass yield continue finally is raise def for lambda return
```

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Les instructions de base</span>

Voici quelques instructions basiques : 
- les boucles
- les tests
- la fonction `print()`

La syntaxe de la boucle `for` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
for <variable> in <liste de valeurs>:
	<bloc de code>

# Le bloc de code est exécuté tant qu'il y a des valeurs dans la liste et la variable est affectée avec la valeur courante.
```

Celle de la boucle `while` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
while <condition>:
	<bloc de code>

# Tant que la condition est vraie le bloc de code est exécuté.
```

Celle de la boucle `if` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
if <condition>:
	<bloc de code 1>
else:
	<bloc de code 2>

# Si la condition est vraie alors on exécute le bloc de code 1 sinon le bloc de code 2.
```

Parfois, dans des exemples, on peut trouver l'utilisation d'un module ou d'une librairie :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
import random # import du module de génération de nombre aléatoire
```

mais aussi `with` :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
# Permet de créer un contexte particulier
with <expression> as <variable>:
	<bloc de code>

# exemple avec l'ouverture d'un fichier (cas typique)
with open(fichier) as f:
	<bloc de code>

# à la fin du bloc de code le fichier sera fermé et la variable 'f' qui représente le fichier sera éliminée.
```

Voici quelques exemples de la fonction `print()`.

La fonction `print()` peut être utilisée avec les mêmes formats que la fonction `printf()` du langage C, avec la syntaxe suivante :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
print(<format> % <valeur>)
```

`<format>` suit la syntaxe de `printf` (`%s` pour les chaînes, `%d` pour les entiers...).

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> print("<%s>" % "HTML")
<HTML>
>>> print("%d €" % 1000)
1000 €
```

S'il y a plusieurs masques de formats dans la chaîne, alors `<valeur>` doit être une liste de valeurs appelée `tuple`.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
>>> print("<%s>" % "HTML")
<HTML>
>>> print("%d €" % 1000)
1000 €
>>> 
>>> print(" %s - %s - %s " % (1,2,"TROIS"))
 1 - 2 - TROIS 
>>> print(" la valeur est de %3.2f %s " % (12.34, "Euros"))
 la valeur est de 12.34 Euros 
```