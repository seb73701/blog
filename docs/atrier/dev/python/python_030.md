# Apprendre les slots Python

Une optimisation des classes méconnue

## Petit état de l’art
Python 3 a fait apparaître une déclaration d’attributs nommée slots. Pour mieux comprendre les `__slots__` et leur puissance, expliquons d’abord ce qui se passe avec une classe python ordinaire.

Lorsque l’on crée un objet d’une classe, les attributs d’instance sont stockés dans un dictionnaire appelé `__dict__`, dès lors qu’ils sont initialisés (généralement dans la méthode `__init__`). C’est via ce fameux dictionnaire qu’on récupère et affecte les attributs.

Il nous permet également d’ajouter dynamiquement de nouveaux attributs à un objet, cela créera de nouvelles entrées dans `__dict__`.

Exemple :

```python
class Point:

    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y
 
if __name__ == "__main__":
    p = Point(0,1)
    print(p.__dict__)
    # {'x': 0, 'y': 1}
 
    p.z = 5
    print(p.__dict__)
    # {'x': 0, 'y': 1, 'z' : 5}
    print(Point.__dict__)
    # {'__module__': '__main__', '__init__': <function Point.__init__ at 0x000001B34BE55D80>, '__dict__': <attribute '__dict__' of 'Point' objects>, '__weakref__': <attribute '__weakref__' of 'Point' objects>, '__doc__': None}
```

*Classe python classique*

> N.B. : D’ailleurs, la classe elle-même a un attribut `__dict__`, on verra plus tard que l’utilisation des slots va aussi influer sur ce dictionnaire.

> N.B. 2 : L’utilisation du if `__name__ == "__main__":` , permet de n'exécuter le code dans le bloc que s’il est lancé comme point d’entrée (donc si lancé comme script ou en invite de commandes). Cela permet d’isoler ce code, il ne sera pas appelé s’il est importé.
On observe bien qu’au début, le dictionnaire contient seulement les clefs/valeurs des attributs initialisés dans le __init__, et que z a été ajouté dynamiquement. C'est parce qu’en réalité, la première syntaxe équivaut à la deuxième.

|Syntaxe n°1|Syntaxe n°2|
|:----------|:----------|
|p.z = 5|p.__dict__['z'] = 5|

On peut alors soulever deux problématiques concernant la mémoire et la performance de nos objets faisant appel à des dictionnaires under the hood.

- Plus on a d’objets, plus on a de dictionnaires, ce qui impactera sans aucun doute la mémoire utilisée (RAM)
- Écrire et lire dans un dictionnaire, n’est pas le plus optimal.

> N.B. : un dictionnaire Python est une HashMap, la complexité d’un set/get dans le pire scénario est en O(n). Pour plus de détails c’est par [ici](https://wiki.python.org/moin/TimeComplexity#dict).

## Et les slots alors
D’après la [documentation Python](https://docs.python.org/3.8/reference/datamodel.html#slots) (pour ceux qui veulent dans la langue de Molière, c’est ici):

> `__slots__ allow us to explicitly declare data members (like properties) and deny the creation of __dict__ and __weakref__ (unless explicitly declared in __slots__ or available in a parent.)`

**Mais qu’est-ce que ça change ?**

Pour répondre à cette question, je refonds ma classe de tout à l’heure en la définissant avec des slots.

```python
class PointWithSlots:
	__slots__= ('x', 'y')
	def __init__(self, x: int, y: int) -> None:
		self.x = x
		self.y = y
```
*Classe python avec Slots*

Et… C’est tout ! Il suffit d’ajouter le champ `__slots__` au niveau de la classe. Quand on fait ça on annule la création du dictionnaire d’instance et crée un tableau à taille fixe des valeurs de `__slots__`.

Ça apporte plusieurs effets des plus intéressants.

### Des attributs fixes
Le dictionnaire d’instance n’existe plus et seuls les attributs définis dans slots sont autorisés, sinon une erreur est retournée. Les erreurs de typographie au niveau des attributs à débugger ne seront donc plus que de lointains souvenirs.

```python
p = PointWithSlots(0,1)
# {'x': 0, 'y': 1}
try:
	print(p.__dict__)
except AttributeError as e:
	print(e)
# 'PointWithSlots' object has no attribute '__dict__'
try:
	p.z = 5
except AttributeError as e:
	print(e)
# 'PointWithSlots' object has no attribute 'z'
```

*Utilisation d'un objet issu d'une classe avec slots*

> N.B.: Si vous utilisez [mypy](https://mypy.readthedocs.io/en/stable/index.html) comme analyseur statique du code, aucune gestion particulière à apporter concernant les slots. Il détectera les tentatives d’accès à des attributs non déclarés dans le `__slots__` et renverra une erreur.

### Economie de RAM

Ça coule de source, du fait qu’il n’y ait plus de dictionnaire d’instance, et que l’on utilise un tableau à taille fixe, on économise de la mémoire !

**Mais à quel point ?**

Pour quantifier cette économie, on va comparer la taille (en bytes) des objets. Afin de mesurer cette taille mémoire utilisée par ces objets, j’utiliserai pympler et sa fonction `asizeof` à la place de `getsizeof`. La raison est simple, `getsizeof` calcule la taille d’un objet sans inclure la taille des sous-objets référencés. Dans le mille, `__dict__` et `__slots__` sont justement des objets référencés, donc ils seraient ignorés !

Comme expliqué, si on regarde les premiers résultats entre les deux, on ne voit  aucune différence en utilisant `getsizeof`.

```python
a = Point(0,1)
b = PointWithSlots(0,1)
 
print(getsizeof(a)) # 48 B
print(getsizeof(b)) # 48 B
```

*Taille mémoire avec 'getsizeof'*

Tandis qu’en utilisant `asizeof`, on peut voir qu’on divise notre utilisation de la mémoire par **3** sur la taille totale entre les deux objets.

C’est beau quand même 😍!

```python
from pympler import asizeof
 
a = Point(0,1)
b = PointWithSlots(0,1)
 
print(asizeof.asizeof(a)) # 320 B
print(asizeof.asizeof(b)) # 104 B
```

*Taille mémoire avec 'asizeof' de pympler*

**Est-ce que ce gain croît si le nombre d’attributs augmente ?**

Eh bien, malheureusement non, c’est même plutôt l’inverse. Plus on ajoute d’attributs, plus la taille totale des objets augmente à un tel point que la différence entre les deux approches en devient presque anecdotique.

```python
class A:
    def __init__(self) -> None:
        self.a = 'Je suis une grande string qui ne manque pas de a'
        self.b = 'bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb'
        self.c = 'Pour qui sont ces serpents qui sifflent sur vos têtes?'
        self.d = 'dddddddddddddddddddddddddddddddddddddddddddddddddddddddddd'
        self.e = [0] * 10
        self.f = {x for x in range(10)}
        self.g = 'g'
        self.h = 'h'
 
class B:
    __slots__ = 'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h'
 
    def __init__(self) -> None:
        self.a = 'Je suis une grande string qui ne manque pas de a'
        self.b = 'bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb'
        self.c = 'Pour qui sont ces serpents qui sifflent sur vos têtes?'
        self.d = 'dddddddddddddddddddddddddddddddddddddddddddddddddddddddddd'
        self.e = [0] * 10
        self.f = {x for x in range(10)}
        self.g = 'g'
        self.h = 'h'
 
a = A()
b = B()
print(asizeof.asizeof(a)) # 2264 B
print(asizeof.asizeof(b)) # 1832 B
```

*Taille mémoire pour des gros objets*

On peut le voir avec cet exemple d’une classe avec 8 attributs, le gain mémoire passe à un peu moins de **20 %**.
Pour cette raison, on peut conclure que le gain mémoire est vraiment notable si on travaille avec **beaucoup d’instances d’une classe qui contient peu d’attributs**.

## Performance

Le principal atout des slots réside dans l’optimisation des performances.
Revenons sur le fonctionnement par défaut (sans slots) pour accéder à un attribut :

1. `getattribute()`
1. et recherche dans le `dict` l’attribut
1. s’il le trouve `dict[‘x’]` pour renvoyer la valeur
1. sinon cherche dans le dictionnaire de sa classe et continue à remonter récursivement jusqu’à trouver
1. si rien, lance une erreur

**Comment ça se passe avec les slots ?**

Notons les différences entre les dictionnaires des classes. La classe qui a les slots, n’a plus de `__dict__`, de `__weakref__`,  et a deux entrées en plus (nos attributs déclarés dans les slots). Qu’il n’y ait plus le `__dict__`, c’est normal, on l’a déjà vu. Le [weakref](https://docs.python.org/3/library/weakref.html), quant à lui, est un composant interne aux objets Python pour le garbage collector.

On peut d’ailleurs voir que les attributs appartiennent à la classe `member_descriptor`.


```python
print(Point.__dict__)
# {'__module__': '__main__', '__init__': <function Point.__init__ at 0x0000026095FC0700>,
#  '__dict__': <attribute '__dict__' of 'Point' objects>, '__weakref__': <attribute '__weakref__' of 'Point' objects>, '__doc__': None}
print(PointWithSlots.__dict__)
# {'__module__': '__main__', '__slots__': ('x', 'y'), '__init__': <function PointWithSlots.__init__ at 0x0000026095FC0790>, ’x': <member 'x' of 'PointWithSlots' objects>, 
# 'y': <member 'y' of 'PointWithSlots' objects>, '__doc__': None}
print(PointWithSlots.x.__class__)
# <class 'member_descriptor'>
```

*Attributs de classe*

> N.B. : Sans aller trop profondément dans la mécanique interne de Python. Un [descripteur](https://docs.python.org/3/howto/descriptor.html) permet de customiser la recherche, le stockage et la suppression des attributs. Ça signifie qu’on va utiliser les méthodes du descripteur à la place du comportement par défaut. Pour être un descripteur, l’objet doit au moins avoir l'une de ces méthodes (`__get__()`, `__set__()`, `__delete__()`).

> Les descripteurs sont à la base de nombreuses fonctionnalités Python. Les comprendre est la clef d’une meilleure compréhension de Python en général (peut-être un prochain sujet d’article 👀).

Quand on cherche à accéder à l’attribut `p.x`, en interne, on va chercher l’objet `x` dans le dictionnaire de la classe de `p` (j’insiste, le dictionnaire de classe pas d’instance). Si x se trouve avoir une méthode nommée `__get__`, il est considéré comme un descripteur. C’est précisément cette méthode qui est appelée pour répondre à l’accès de notre `p.x`. Sinon, on utilise le comportement par défaut.

Après cette légère digression, pour mieux comprendre le fonctionnement interne de Python, revenons à nos moutons. Pourquoi gagne-t-on en performance avec les slots ?

D’après [Guido van Rossum](http://python-history.blogspot.com/2010/06/inside-story-on-new-style-classes.html) (créateur de Python), cette implémentation du __get__, __set__ utilise un tableau au lieu du dictionnaire. De plus, le tout est implémenté en C ce qui explique que leur accès soit plus rapide. La documentation Python parle d’une amélioration de la vitesse d’accès d’environ [35 %](https://docs.python.org/3/howto/descriptor.html#member-objects-and-slots).

De mon côté, je trouve en moyenne une vitesse de **17 %** à partir de ce script. (Python 3.10 sur un Dell XPS)

Pour comparer la performance, on va regarder le temps d’exécution pris pour réaliser les opérations d’instanciation, de lecture et d’écriture des attributs. En utilisant `repeat` pour chaque approche, on va capturer le temps pris (en seconde) pour un million d’appels de test, et répéter ça n fois.

En ordre de grandeur, on voit que le temps moyen sans slots est de **1,33 s** contre **1,12 s** avec.

```
from timeit import repeat
from statistics import mean
import csv
 
def test(avec_slots=False):
  instance = Point(1, 2) if not avec_slots else PointWithSlots(1, 2)
  def repeat_basic_operations(instance=instance):
    for _ in range(10):
      instance.x, instance.y = 3, 4
      instance.x, instance.y
  return repeat_basic_operations
 
if __name__ == "__main__":
  avec_slots = repeat(test(avec_slots=True), number=1_000_000, repeat=50)
  sans_slots = repeat(test(), number=1_000_000, repeat=50)
 
  print(f'Sans les slots : temps en moyenne={mean(sans_slots)}s, max={max(sans_slots)}s, min={min(sans_slots)}s')
  print(f'Avec les slots : temps en moyenne={mean(avec_slots)}s, max={max(avec_slots)}s, min={min(avec_slots)}s')
  print(f'Gains moyen={(mean(sans_slots)/mean(avec_slots)-1)*100:.4} %')
# Sans les slots : temps en moyenne=1.3282056819999999s, max=1.4418512999999962s, min=1.283558499999998s
# Avec les slots : temps en moyenne=1.1244820380000002s, max=1.1791362999999997s, min=1.0955695999999975s
# Gains moyen=18.12 %
 
# Bonus 
# Pour sortir nos temps d'execution dans un fichier csv pour l'exploiter si besoin
  with open('slots_times.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    # entete des colonnes
    writer.writerow(["Sans slots", "Avec slots"])
    writer.writerows(zip(sans_slots,avec_slots))
```

*Comparaison des performances*

## À utiliser avec parcimonie
Après avoir vu ses avantages, on peut se demander : Pourquoi ne pas avoir des slots dans toutes nos classes ?

Eh bien, parce que cette technique a aussi ses inconvénients et points d’attentions à avoir, une petite liste.

**Attributs fixes**
On pourrait se dire, que l’un des inconvénients est qu’il ne soit plus possible d'ajouter dynamiquement des attributs, mais… c’est toujours possible en fait.

En effet, on peut tout à fait avoir le meilleur des deux mondes. Il suffit d’ajouter `__dict__` aux valeurs de `__slots__` et seuls les nouveaux attributs seront ajoutés dans le dictionnaire.

```python
class PointHybrid:
    __slots__ = ('x', 'y', '__dict__')
 
    def __init__(self, x: int, y: int) -> None:
        self.x = x
        self.y = y
 
p = PointHybrid(1,2)
print(p.__dict__) # {}
 
p.z = 3
print(p.__dict__) # {'z': 3}
```

*Classe python hybride*

### Effet de bord
Par effet de bord, je m’explique, certaines fonctions ou décorateurs (même si un décorateur peut être une fonction) ont besoin de ce fameux dictionnaire d’instance pour fonctionner.

Par exemple, dans le cas de `@cached_property`, on peut pallier ce problème en ajoutant le dict dans les slots, mais il faut avoir conscience que des problèmes puissent exister.

### Héritage
Néanmoins, le plus gros problème réside au niveau de l’héritage. Lorsqu’on utilise les slots, il faut avoir trois grandes règles à l'esprit :

- Les classes filles héritent des slots des classes mères.

Attention, les slots ne sont pas fusionnés. Un slots est rattaché à la classe où il a été défini, donc si on cherche à afficher les `__slots__` d’une instance (ou type), il n’affichera que les valeurs définies dans sa classe de base.

Afin d’afficher l’ensemble des `__slots__` dont il a accès, il faut remonter toute l’arborescence de la classe (en suivant le MRO : [Method Resolution Order](https://www.educative.io/answers/what-is-mro-in-python)), et à chaque classe lui demander son `__slots__` respectif.


```python
from typing import Type
 
class Point3D(PointWithSlots):
    __slots__ = 'z'
 
    def __init__(self, x: int, y: int, z: int) -> None:
        super().__init__(x, y)
        self.z = z
 
def all_slots(cls: Type):
    slots = set()
    for cls in cls.__mro__:
        slots.update(getattr(cls, '__slots__', []))
    return slots
 
print(Point3D.__slots__) # z
print(all_slots(Point3D)) # {'z', 'x', 'y'}
```

*Héritage avec les slots*

Autre point qui mérite d’être noté, si plusieurs classes dans l’arborescence ont les mêmes attributs notés dans leur `__slots__`, les valeurs seront doublées naïvement. En soit, ça ne changera pas grand-chose pour l’utilisateur mais de la mémoire sera utilisée en plus à cause des doublons.

- Lors d’un héritage multiple, il ne peut y avoir qu’une seule des classes mères  qui ait un slots non vide
- Si une classe dans l'arborescence omet de mettre slots dans sa définition, toutes les instances de cette classe et des classes filles auront un `__dict__` (même si celles avant avaient un slots)

## Conclusion
Les slots constituent une fonctionnalité qui mérite d'être gardée à l’esprit lors de nos développements ou lorsqu’on regarde le code d’un autre. Comme on l’a vu, rien qu’en ajoutant une seule ligne dans notre classe, les objets qui en découlent se trouvent avoir divers avantages.

D’une part, une légère optimisation de la vitesse et de la mémoire aux alentours de 20 %. Cependant, il faut avoir en tête que ces améliorations peuvent être balayées très simplement par un oubli dans l’héritage.

D’autre part, à mon sens, avoir une structure d’objet fixe interdisant la création d’attribut est vraiment le principal avantage de l’utilisation des slots. D’expérience, la création dynamique d’attribut peut se révéler être une source d’erreur des plus complexes à débugger. Une simple erreur de typographie sur un attribut peut causer bien des soucis et aucune exception ne sera lancée pour signaler la source du problème.

Alors oui, bien sûr, les slots ne sont pas la seule manière d’interdire cette création, mais elle est clairement la plus simple à mettre en œuvre.

### Petit bonus !

Pour ceux qui utilisent le décorateur `@dataclass` et qui sont sur **Python 3.10**, vous pouvez dès maintenant lier les deux sans aucun problème !

Il vous suffit de faire ceci :

```
from dataclasses import dataclass
 
@dataclass(slots=True)
class MyClass:
    a: str
    b: int
```

source : https://blog.ippon.fr/