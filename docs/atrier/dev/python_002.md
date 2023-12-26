# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>50 astuces pour Python</span>

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>01. Exécuter des scripts Python</span>

Sur la plupart des UNIX, vous pouvez exécuter des scripts Python à partir de la ligne de commande.

```shell
$ python mypyprog.py
```


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>02. Lancer des programmes</span>

L’interpréteur interactif Python vous permet de faire vos premiers pas dans la programmation Python. Vous venez d’émettre chaque commande (>>>), une par une, et la réponse est immédiate.

L’interpréteur Python se lance avec la commande :

```shell
$ python
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>03. Saisie dynamique</span>

En Java, en C++ et dans d’autres langages, vous devez préciser le type de données de la valeur de retour de la fonction et de chaque argument de la fonction. Python est un langage typiquement typé, vous ne devez jamais spécifier le type de données d’aucun élément. En fonction de la valeur que vous attribuez, Python effectuera de lui-même un suivi des données au fur et à mesure que vous taperez vos commandes.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>04. Commandes Python</span>

Python utilise les retours chariot pour séparer les instructions, deux-points et une indentation pour séparer les blocs de code. Les langages de programmation compilés, tels que C et C++, utilisent des points-virgules pour séparer les instructions et crochets pour les blocs de code.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>05. Opérateurs `==` et `=`</span>

Python utilise `==` pour la comparaison et `=` pour l’affectation. Python ne prend pas en charge l’affectation en ligne, donc il n’y a aucune chance d’attribuer accidentellement la valeur lorsque vous souhaitez réellement la comparer.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>06. Concaténation de chaînes</span>

L’opérateur `+` concatène des chaînes de caractères.

```python
>>> print ‘kun’+’al’
kunal
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>07. La méthode `__init__`</span>

La méthode `__init__` est exécutée dès qu’un objet d’une classe est instancié. Cette méthode est utile pour effectuer toute initialisation que vous voulez faire avec votre objet. La méthode `__init__` est analogue à un constructeur en C++, C# ou Java.

Exemple :

```python
class Person:
    def __init__(self, name):
        self.name = name
    def sayHi(self):
        print("Hello, my name is", self.name)

p = Person("Sebastien")
p.sayHi()
```

Affichage :

```shell
$ python .\test_class.py
Hello, my name is Sebastien
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>08. Modules</span>

Pour que vos programmes soient gérables à mesures qu’ils augmentent, divisez-les en plusieurs fichiers. Python permet de mettre plusieurs définitions de fonctions dans un fichier et de les utiliser comme module importable dans d’autres scripts et programmes. Ces fichiers ont une extension `.py`.

Exemple :

```python
# file my_function.py
def minmax(a,b) :
	if a ≤ b:
	min, max = a, b
	else:
	min, max = b, a
	return min, max
Module Usage
import my_function
x,y = my_function.minmax(25, 6.3)
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>09. Module définition de noms</span>

Exemple :

La fonction intégrée `dir()` peut être utilisée pour savoir quels noms définit un module, il renvoie une liste triée de chaînes.

```python
>>> import time
>>> dir(time)
['_STRUCT_TM_ITEMS', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'altzone', 'asctime', 'ctime', 'daylight', 'get_clock_info', 'gmtime', 'localtime', 'mktime', 'monotonic', 'monotonic_ns', 'perf_counter', 'perf_counter_ns', 'process_time', 'process_time_ns', 's
leep', 'strftime', 'strptime', 'struct_time', 'thread_time', 'thread_time_ns', 'time', 'time_ns', 'timezone', 'tzname']
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>10. Documentation interne</span>

Vous pouvez voir la documentation interne d’un nom de module en regardant `.__doc__`

Exemple :

```python
>>> import time
>>> print(time.__doc__)
This module provides various functions to manipulate time values.

There are two standard representations of time.  One is the number
of seconds since the Epoch, in UTC (a.k.a. GMT).  It may be an integer
or a floating point number (to represent fractions of seconds).
The Epoch is system-defined; on Unix, it is generally January 1st, 1970.
The actual value can be retrieved by calling gmtime(0).

The other representation is a tuple of 9 integers giving local time.
The tuple items are:
  year (including century, e.g. 1998)
  month (1-12)
  day (1-31)
  hours (0-23)
  minutes (0-59)
  seconds (0-59)
  weekday (0-6, Monday is 0)
  Julian day (day in the year, 1-366)
  DST (Daylight Savings Time) flag (-1, 0 or 1)
If the DST flag is 0, the time is given in the regular time zone;
if it is 1, the time is given in the DST time zone;
if it is -1, mktime() should guess based on the date and time.
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>11. D’arguments à un script</span>

Python vous permet d’accèder à tout ce que vous avez transmis à un script tout en l’appelant. Le contenu de la « ligne de commande » est stocké dans la liste `sys.argv`.

```python
>>> import sys
>>> print(sys.argv)
['']
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>12. Chargement de modules au démarrage</span>

Vous pouvez charger des modules ou des commandes prédéfinis lors du démarrage d’un script Python en utilisant la variable d’environnement `$PYTHONSTARTUP`. Vous pouvez la configurer `$PYTHONSTARTUP` dans un fichier qui contient les instructions charger les modules ou les commandes nécessaires.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>13. Convertir une chaîne en objet date</span>

Vous pouvez utiliser la fonction `DateTime` pour convertir une chaîne en un objet date.

Exemple :
```python
from DateTime import DateTime
dateobj = DateTime(string)
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>14. Convertir une liste en chaîne à afficher</span>

Vous pouvez convertir une liste en chaîne de l’une des façons suivantes.

1ere méthode :

```python
>>> mylist = [‘spam’,‘ham’,‘eggs’]
>>> print ‘, ‘.join(mylist)
spam, ham, eggs
```

2eme méthode :

```python
print ‘\n’.join(mylist)
spam
ham
eggs
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>15. Auto-complétion de l’interpréteur</span>

Vous pouvez activer l’auto-complétion automatique dans l’interpréteur Python en ajoutant ces lignes à votre fichier **.pythonrc** (ou votre fichier Python de démarrage) :

```python
import rlcompleter, readline
readline.parse_and_bind(‘tab : complete’)
```

Cela permettra à Python de remplir partiellement la fonction, la méthode et le nom des variables quand vous appuyez sur la touche Tab.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>16. Documentation</span>

Vous pouvez afficher une interface graphique pour rechercher la documentation Python en utilisant la commande :

```shell
$ pydoc -g
```

Il sera nécessaire d’installer le package **python-tk** pour que cela fonctionne.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>17. Serveur de documentation</span>

Vous pouvez démarrer un serveur HTTP sur le port donné sur la machine locale. Cela vous donnera un accès agréable à toute la documentation de Python, y compris la documentation du module tiers.

```shell
$ pydoc -p <portNumber>
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>18. Logiciel de développement</span>

Il existe de nombreux outils pour aider au développement de Python. Voici quelques éléments importants :

- **IDLE** : L’IDE intégrée Python, avec l’autocomplétion, l’aide contextuelle de signature de fonction et l’édition de fichier.
- **Ipython** : Un autre shell Puthon amélioré avec fin d’onglet et autres fonctionnalités.
- **Eric3** : Un IDE Python graphique avec autocomplétion, navigateur de classe, shell intégré et débogeur.
- **WingIDE** : Commercial Python IDE avec licence libre disponible en opensource pour les développeurs

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>19. Une fonction en sortant de l’interpréteur</span>

Utilisez le module **atexit** pour exécuter des fonctions au moment de sortir de l’interpréteur Python.

Exemple :

```python
def sum() :
	print(4+5)
def message() :
	print(« Exécution »)
import atexit
atexit.register(sum)
atexit.register(message)
```

Affichage :

```shell
Exécution
9
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>20. Convertir un nombre entier en binaire, en hexadécimal et en octal</span>

Utilisez `bin()`, `hex()` et `oct()` pour convertir un entier au format binaire, décimal et octal respectivement.

Exemple :

```python
>>> bin(24)
‘0b11000’
>>> hex(24)
‘0x18’
>>> oct(24)
‘030’
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>21. Convertir un jeu de caractères en UTF-8</span>

Vous pouvez utiliser la fonction suivante pour convertir n’importe quel jeu de caractères en UTF-8.

```python
data.decode(« input_charset_here »).encode(‘utf-8’)
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>22. Enlever des doublons présents dans une liste</span>

Si vous souhaitez supprimer les doublons d’une liste, mettez simplement chaque élément dans un dictionnaire (par exemple avec `none` en tant que valeur) puis vérifiez avec la commande `dict.keys()`.

```python
from operator import setitem
def distinct(1) :
	d = {}
	map(setime, (d,)*len(1), 1, [])
	return d.keys()
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>23. Boucles do-while</span>

Etant donné que Python n’intègre pas (encore) de boucles `do-while` ou `do-until`, vous pouvez utiliser la méthode suivante pour obtenir un résultat similaire :

```python
while True:
	do_something()
	if condition():
		break
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>24. Détection du système d’exploitation</span>

Pour exécuter des fonctions spécifiques à la plate-forme, il est très utile de détecter la plate-forme sur laquelle s’exécute l’interpréteur Python. Utilisez `sys.platform` pour connaître la plateforme d’exécution :

Exemple :

```python
>>> import sys
>>> sys.platform
‘linux2’
```

Avec Mac OS X Snow Leopard

```python
>>> import sys
>>> sys.platform
‘darwin’
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>25. Activer et désactiver le garbage collection</span>

Parfois, vous pouvez activer ou désactiver le mécanisme de garbage collection au moment de l’exécution. Vous pouvez utiliser le module **gc** pour activer ou désactiver le garbage collection.

Exemple :

```python
>>> import gc
>>> gc.enable
<built-in function enable>
>>> gc.disable
<built-in function disable>
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>26. Des modules en C pour les performances</span>

De nombreux modules Python sont livrés avec des modules C équivalents. L’utilisation de ces modules C améliorera significativement la performance des applications complexes.

Exemple :

```python
cPickle instead of Pickle, cStringIO instead of StringIO
```


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>27. Calculer le maxi, le mini et la somme d’une liste ou d’un itérable</span>

Pour cela utilisez les fonctions intégrées suivantes :

`max` : renvoie l’élément le plus important de la liste.
`min` : renvoie l’élément le plus petit de la liste
`sum` : Cette fonction renvoie la somme de tous les éléments de la liste. Elle accepte un second argument optionnel : la valeur de départ pour la somme (par défaut à 0).

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>28. Des nombres fractionnés</span>

L’instance de fraction peut être créée à l’aide de la construction suivante :

```python
Fraction([numerator [,denominator]])
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>29. Réaliser des opérations</span>

Les opérations fonctionnent sur des nombres entiers et flottants, à l’exception des nombres complexes, un module séparé est utilisé, appelé **cmath**.

Exemple :

```python
math.acos(x) : renvoie l’arc cosinus de x.
math.cos(x) : renvoie le cosinus de x.
math.factorial(x) : renvoie la factorielle de x.
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>30. Travailler avec des tableaux</span>

Le module **array** fournit un moyen efficace d’utiliser des tableaux dans vos programmes.. Le module **array** définit le type suivant :

```python
array(typecode [, initializer])
```` 

Une fois que l’objet tableau est créé, disons `myarray`, vous pouvez y appliquer de nombreuses méthodes. Voici quelques éléments importants :

```python
myarray.count(x) : renvoie le nombre d’occurrences de x dans a.
myarray.extend(x) : ajoute x à la fin du tableau.
myarray.reverse() : inverse l’ordre du tableau
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>31. Trier des éléments</span>

Les commandes suivantes trient des listes.

```python
bisect.insort(list, item[, low[, hight]])
```

Insère l’élément dans la liste dans l’ordre trié. Si l’élément est déjà dans la liste, la nouvelle entrée est insérée à droite de toutes les entrées existantes.

```python
bisect.insort_left(list, item[, low[, high]])
```

Insère l’élément dans la liste dans l’ordre trié. Si l’élément est déjà dans la liste, la nouvelle entrée est insérée à gauche de toutes les entrées existantes.


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>32. Utiliser la recherche d’expressions</span>

Vous pouvez utiliser la fonction `re.search()` avec une expression. Voyez l’exemple ci-dessous.

Exemple :

```python
>>> import re
>>> s = "Kunal est méchant"
>>> if re.search("K", s): print "Vrai!"
# char literal
...
Vrai!
>>> if re.search("[@A-Z]", s): print "Vrai" # char class
...# si l'initiale correspond
Vrai!
>>> if re.search("\d", s): print "Vrai!" # digits class
...
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>33. Le format bzip2 (.bz2)</span>

Utilisez le module `bz2` pour lire et écrire des données en utilisant l'algorithme de compression bzip2.

```python
bz2.compress() : for bz2 compression
bz2.decompress() : for bz2 decompression
```

Exemple :

```python
# File bz2-example.py
import bz2
MESSAGE = "Kunal est méchant"
compressed_message = bz2.compress(MESSAGE)
decompressed_message = bz2.decompress(compressed_message)
print "original:", repr(MESSAGE)
print "message compressé: ", repr(compressed_message)
print "message décompressé: ", repr(decompressed_message)
```

Affichage :

```python
[~/src/python $:] python bz2-example.py
original: 'Kunal est méchant'
compressed message: 'BZh91AY&SY\xc4\x0fG\x98\x00\x00\x02\x15\x800\x00\x00\x84%\x8a\x00".....'
decompressed message: 'Kunal est méchant'
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>34. Utiliser la base de données SQLite</span>

SQLite est devenue une base de données intégrée très populaire en raison de sa configuration facile et ses performances supérieures. Utilisez le module `sqlite3` pour travailler avec les bases de données SQLite.

Exemple :

```python
>>> import sqlite3
>>> connection = sqlite.connect('test.db')
>>> curs = connection.cursor()
>>> curs.execute('''create table item)
... (id integer primary key, itemno text unique,
... scancode text, descr text, price real)''')
<sqlite3.Cursor object at 0x1004a2b30>
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>35. Utiliser des fichiers zip</span>

Utilisez le module `zipfile` pour utiliser des fichiers zip :

```python
zipfile.ZipFile(filename [, mode [, compression [,allowZip64]]])
```

Ouvre un fichier zip, en y ajoutant éventuellement son chemin d'accès.

```python
zipFile.close()
```

Ferme le fichier d'archive. Vous devez utiliser `close()` avant de quitter votre programme ou les enregistrements essentiels ne seront pas écrits.

```python
zipFile.extract(member[, path[, pwd]])
```

Extrait un élément de l'archive vers le répertoire de travail actuel. L'élément doit être désigné par son nom complet (ou un objet `zipinfo`). `path` spécifie un répertoire différent où l'extraire. L'élément peut être un nom de fichier ou un objet `zipinfo`. `pwd` est le mot de passe utilisé pour les fichiers cryptés.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>36. Utiliser des caractères pour chercher et retrouver un fichier</span>

Vous pouvez utiliser le module `glob` pour trouver tous les noms de chemin correspondant à un modèle selon les règles utilisées par le shell UNIX. `*,?` et les plages de caractères exprimées avec `[ ]` seront recherchées.

Exemple : 

```python
>>> import glob
>>> glob.glob('./[0-9].*')
['./1.gif', './2.txt']
>>> glob.glob('*.gif')
['1.gif', 'card.gif']
>>> glob.glob('?.gif')
['1.gif']
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>37. Copier, supprimer et renommer un fichier</span>

Vous pouvez utiliser le module `shutil` pour effectuer des opérations courantes à un où plusieurs fichiers. Attention, gardez à l'esprit que ce module ne fonctionne qu'avec les fichiers spéciaux comme les canaux nommés, les périphériques de blocs, etc.

```python
shutil.copy(src,dst)
```

Copie le fichier src dans le fichier ou répertoire dst.

```python
shutil.copymode(src,dst)
```

Copie les autorisations de fichier de src à dst.

```python
shutil.move(src,dst)
```

Déplace un fichier ou un répertoire vers dst

```python
shutil.copytree(src,dst,symlinks [,ignore])
```

Copie récursivement un répertoire entier dans dst

```python
shutil.rmtree(path [, ignore_errors [, onerror]])
```

Supprime un répertoire complet.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>38. Exécuter des commandes UNIX</span>

Cette posibilité n'existe pas sur Python3 - à la place vous devez utiliser le module `subprocess`.

Exemple :

```python
>>> import commands
>>> commands.getoutput('ls')
'bz2-example.py\ntest.py'
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>39. Consulter le contenu des variables</span>

Vous pouvez utiliser le module `os` pour collecter des informations spécifiques au système d'exploitation contenues dans les variables d'environnement.

Exemple :

```python
>>> import os
>>> os.path
<module 'posixpath' from '/usr/lib64/python3.6/posixpath.py'>
>>> os.environ
environ({'LS_COLORS': 'rs=0:di=38;5;33:ln=38;5;51:mh=00:pi=40;38;5;11:so=38;5;13:do=38;5;5:bd=48;5;232;38;5;11:cd=48;5;232;38;5;3:or=48;5;232;38;5;9:mi=01;05;37;41:su=48;5;196;38;5;15:sg=48;5;11;38;5;16:ca=48;5;196;38;5;226:tw=48;5;10;38;5;16:ow=48;5;10;38;5;21:st=48;5;21;38;5;15:ex=38;5;40:*.tar=38;5;9:*.tgz=38;5;9:*.arc=38;5;9:*.arj=38;5;9:*.taz=38;5;9:*.lha=38;5;9:*.lz4=38;5;9:*.lzh=38;5;9:*.lzma=38;5;9:*.tlz=38;5;9:*.txz=38;5;9:*.tzo=38;5;9:*.t7z=38;5;9:*.zip=38;5;9:*.z=38;5;9:*.dz=38;5;9:*.gz=38;5;9:*.lrz=38;5;9:*.lz=38;5;9:*.lzo=38;5;9:*.xz=38;5;9:*.zst=38;5;9:*.tzst=38;5;9:*.bz2=38;5;9:*.bz=38;5;9:*.tbz=38;5;9:*.tbz2=38;5;9:*.tz=38;5;9:*.deb=38;5;9:*.rpm=38;5;9:*.jar=38;5;9:*.war=38;5;9:*.ear=38;5;9:*.sar=38;5;9:*.rar=38;5;9:*.alz=38;5;9:*.ace=38;5;9:*.zoo=38;5;9:*.cpio=38;5;9:*.7z=38;5;9:*.rz=38;5;9:*.cab=38;5;9:*.wim=38;5;9:*.swm=38;5;9:*.dwm=38;5;9:*.esd=38;5;9:*.jpg=38;5;13:*.jpeg=38;5;13:*.mjpg=38;5;13:*.mjpeg=38;5;13:*.gif=38;5;13:*.bmp=38;5;13:*.pbm=38;5;13:*.pgm=38;5;13:*.ppm=38;5;13:*.tga=38;5;13:*.xbm=38;5;13:*.xpm=38;5;13:*.tif=38;5;13:*.tiff=38;5;13:*.png=38;5;13:*.svg=38;5;13:*.svgz=38;5;13:*.mng=38;5;13:*.pcx=38;5;13:*.mov=38;5;13:*.mpg=38;5;13:*.mpeg=38;5;13:*.m2v=38;5;13:*.mkv=38;5;13:*.webm=38;5;13:*.ogm=38;5;13:*.mp4=38;5;13:*.m4v=38;5;13:*.mp4v=38;5;13:*.vob=38;5;13:*.qt=38;5;13:*.nuv=38;5;13:*.wmv=38;5;13:*.asf=38;5;13:*.rm=38;5;13:*.rmvb=38;5;13:*.flc=38;5;13:*.avi=38;5;13:*.fli=38;5;13:*.flv=38;5;13:*.gl=38;5;13:*.dl=38;5;13:*.xcf=38;5;13:*.xwd=38;5;13:*.yuv=38;5;13:*.cgm=38;5;13:*.emf=38;5;13:*.ogv=38;5;13:*.ogx=38;5;13:*.aac=38;5;45:*.au=38;5;45:*.flac=38;5;45:*.m4a=38;5;45:*.mid=38;5;45:*.midi=38;5;45:*.mka=38;5;45:*.mp3=38;5;45:*.mpc=38;5;45:*.ogg=38;5;45:*.ra=38;5;45:*.wav=38;5;45:*.oga=38;5;45:*.opus=38;5;45:*.spx=38;5;45:*.xspf=38;5;45:', 'SSH_CONNECTION': '10.0.2.2 64234 10.0.2.13 22', 'LANG': 'fr_FR.UTF-8', 'HISTCONTROL': 'ignoredups', 'HOSTNAME': 'localhost.localdomain', 'which_declare': 'declare -f', 'XDG_SESSION_ID': '1', 'USER': 'root', 'PWD': '/root', 'HOME': '/root', 'SSH_CLIENT': '10.0.2.2 64234 22', 'SSH_TTY': '/dev/pts/0', 'MAIL': '/var/spool/mail/root', 'TERM': 'xterm-256color', 'SHELL': '/bin/bash', 'SHLVL': '1', 'LOGNAME': 'root', 'DBUS_SESSION_BUS_ADDRESS': 'unix:path=/run/user/0/bus', 'XDG_RUNTIME_DIR': '/run/user/0', 'PATH': '/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin', 'HISTSIZE': '1000', 'LESSOPEN': '||/usr/bin/lesspipe.sh %s', 'BASH_FUNC_which%%': '() {  ( alias;\n eval ${which_declare} ) | /usr/bin/which --tty-only --read-alias --read-functions --show-tilde --show-dot $@\n}', '_': '/usr/bin/python3'})
>>> os.name
'posix'
>>> os.linesep
'\n'
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>40. Envoyer  des emails</span>

Vous pouvez utiliser le module `smtplib` pour envoyer des courriels à l'aide de l'interface client SMTP (Simple Mail Transfer Protocol).


```python
smtplib.SMTP([host [, port]])
```

Exemple (envoyer un mail en utilisant le serveur de mails SMTP de Google Mail):

```python
import smtplib
#Utilisez vos propres identifiants de messagerie
fromaddr = 'emtteur@gmail.com'
toaddrs = 'destinataire@gmail.com'
msg = 'Je suis un geek Python, voici la preuve !'
# Credentials
# Utilisez vos propres identifiants et mot de passe Google Mail
username = 'emetteur@gmail.com'
password = 'xxxxxxxxxxx'
# Le mail proprement dit
server = smtplib.SMTP('smtp.gmail.com:587')
# Google Mail utilise une méthode sécurisée pour les connexions SMTP
server.starttls()
server.login(username,password)
server.sendmail(fromaddr, toaddrs, msg)
server.quit()
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>41. Se connecter à un serveur FTP</span>

`ftplib` est un module FTP client à part entière pour Python. Pour établir une connexion FTP, vous pouvez utiliser la fonction suivante :

```python
ftplib.FTP([host [, user [, passwd [, acct [, timeout]]]]])
```

Exemple :

```python
host = "ftp.redhat.com"
username = "anonymous"
password = "xxxx@gmail.com"
import ftplib
import urllib2
ftp_serv = ftplib.FTP(host,username,password)
# Telecharger un fichier
u = urllib2.urlopen("ftp://ftp.redhat.com/pub/redhat/linux/README")
# Affiche le contenu du fichier
print(u.read())
```

Affichage :

```shell
[~/src/python $:] python ftpclient.py
```

Les versions plus anciennes de Red Hat Linux ont été déplacées vers ftp://archive.download.redhat.com/pub/redhat/linux/.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>42. Lancer une page web avec le navigateur web</span>

Le module **webbrowser** fournit un moyen pratique de lancer des pages Web en utilisant le navigateur Web par défaut.

Exemple (lancer la page google.com avec le navigateur par défaut) :

```python
>>> import webbrowser
>>> webbrowser.open('http://google.com')
True
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>43. Créer des sommes hash de sécurité</span>

Le module `hashlib` prend en charge une pléthore d'algorithmes de hash sécurisés.

Exemple (création d'un hash pour un texte donné) :

```python
>>> import hashlib
# sha1 Digest
>>> hashlib.sha1(b"MI6 Information Top Secret 007").hexdigest()
'5eaf3fc7fda1bce0a7424791359a2708eb6f4265'
# sha224 Digest
>>> hashlib.sha224(b"MI6 Information Top Secret 007").hexdigest()
'74cea46edb7b001d59c4db07c8636ba0e157d21237d4a7cc6b834e43'
# sha256 Digest
>>> hashlib.sha256(b"MI6 Information Top Secret 007").hexdigest()
'b59619679840ebd6a0b15542e6480597f4157c665f63ed9fcf63b9dfa138c1b0'
# sha384 Digest
>>> hashlib.sha384(b"MI6 Information Top Secret 007").hexdigest()
'e2af488fce5e3c4989c082e605026672a9642e70d0d5b4d080fee1abe61b7d1652248c4971ee53924c1ace637f4d77af'
# sha512 Digest
>>> hashlib.sha512(b"MI6 Information Top Secret 007").hexdigest()
'f2bed47e1995d6e315ffcef69dee258b818390956ea608a9936b3fde6273e02f351eb2637295fb19ec2d102ba3b330c6113bc47085b19468e4e0a4b13d586767'

# MD5 Digest
>>> hashlib.md5(b"MI6 Information Top Secret 007").hexdigest()
'53deb49cea97d351018bcf9694077ec5'
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>44. Nombres aléatoires</span>

Vous pouvez utiliser le module `random` pour générer une variété de nombres aléatoires. Le plus utilisé est `random.seed([x])`. Il initialise le générateur de nombres aléatoires de base. Si x est omis ou Aucun, l'heure du système est utilisée. L'heure système est utilisée pour initialiser le générateur lorsque le module est importé.


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>45. Utiliser des fichiers CSV (comma-separated values)</span>

Les fichiers CSV sont très populaires pour l'échange de données sur le Web. En utilisant le module `csv`, vous pouvez lire et écrire des fichiers CSV.

Exemple :

```python
import csv
# ecrit des données sous forme de valeurs séparées par des virgules
writer = csv.writer(open('stocks.csv', 'wb', buffering=0))
writer.writerows([('GOOG', 'Google, Inc.', 505.24, 0.47, 0.09),('YHOO', 'Yahoo! Inc.', 27.38, 0.33, 1.22),('CNET', 'CNET Networks, Inc.', 8.62, -0.13, -1.49)])
# lire les données des stocks, impimer les messages d'état
stocks = csv.reader(open('stocks.csv', 'rb'))
status_labels = {-1:'down', 0:'unchanged', 1:'up'}
for ticker, name, price, change, pct in stocks:
    status = status_labels[cmp(float(change), 0.0)]
    print '%s is %s (%s%%)' % (name, status, pct)
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>46. Des modules pour les outils de configuration</span>

`setuptools` est un paquet Python qui vous permet de télécharger, compiler, installer, mettre à niveau et désinstaller les packages très facilement. Pour utiliser `setuptools`, vous devrez l'installer à partir du gestionnaire de paquets de votre distribution. Après l'installation, vous pouvez utiliser la commande `easy_install` pour effectuer des tâches de gestion de paquets Python.

Exemple (installation de simplejson à l'aide de setuptools) :

```shell
kunal@ubuntu: ~$ sudo easy_install simplejson
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>47. Ecrire dans le journal</span>

Vous pouvez utiliser le module `syslog` pour écrire dans le journal système. `syslog` agit comme une interface pour les routines bibliothèque syslog UNIX.

Exemple :

```python
import syslog
syslog.syslog('mygeekapp: started logging')
for a in ['a','b','c']:
    b = 'mygeekapp: I found letter '+a
    syslog.syslog(b)
syslog.syslog('mygeekapp: the script goes to sleep now, bye, bye !')
```

Affichage :

```shell
$ python mylog.py
$ tail -f /var/log/messages
Nov 8 17:14:34 ubuntu -- MARK --
Nov 8 17:15:34 ubuntu python: mygeekapp: started logging
Nov 8 17:16:34 ubuntu python: mygeekapp: I found letter a 
Nov 8 17:17:34 ubuntu python: mygeekapp: I found letter b
Nov 8 17:18:34 ubuntu python: mygeekapp: I found letter c
Nov 8 17:19:34 ubuntu python: mygeekapp: the script goes to sleep now, bye, bye !
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>48. Générez des documents PDF</span>

`ReportLab` est un module très populaire pour générer des documents PDF avec Python.

Voici les étapes à suivre pour installer ReportLab :

```shell
$ wget http://www.reportlab.org/ftp/ReportLab_2_3.tar.gz
$ tar xvfz ReportLab_2_3.tar.gz
$ cd ReportLab_2_3
$ sudo python setup.py install
```

Une fois l'installation réussie, vous devriez voir apparaître le message suivant :

```shell
####################SUMMARY INFO####################
####################################################
#Attempting install of _rl_local, sgmlop & pyHnj
#extensions from '/home/kunal/python/ReportLab_2_3/src/rl_addons/rl_accel'
...
...
```

Exemple :

```python
>>> from reportlab.pdfgen.canvas import Canvas
# Select the canvas of letter page size
>>> from reportlab.lib.pagesizes import letter
>>> pdf = Canvas("bond.pdf", pagesize = letter)
# import units
>>> from reportlab.lib.units import cm, mm, inch, pica
>>> pdf.setFont("Courier", 60)
>>> pdf.setFillColorRGB(1, 0, 0)
>>> pdf.drawCentredString(letter[0] / 2, inch * 6, "MI6 CLASSIFIED")
>>> pdf.setFont("Courier", 40)
>>> pdf.drawCentredString(letter[0] / 2, inch * 5, "For 007's Eyes Only")
# Close the drawing for current page
>>> pdf.showPage()
# Save the pdf page
>>> pdf.save()
```

Affichage :

```shell
@image:pdf.png
@title: PDF Output
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>49. Utiliser l'API de Twitter</span>

Vous pouvez vous connecter à Twitter en utilisant le module `Python-Twitter`.

Voici les étapes à suivre pour installer le module Python-Twitter :

```shell
$ wget http://python-twitter.googlecode.com/files/python-twitter-0.6.tar.gz
$ tar xvfz python-twitter-0.6.tar.gz
$ cd python-twitter-0.6
$ sudo python setup.py install
```

Exemple (pour obtenir la liste des followers):

```python
>>> import twitter
# Use you own twitter account here
>>> mytwi = twitter.Api(username='kunaldeo',password='xxxxxxxxxx')
>>> friends = mytwi.GetFriends()
>>> print [u.name for u in friends]
[u'Matt Legend Gemmell', u'jono wells', u'The MDN Blog', ...]
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>50. Une recherche Yahoo</span>

Vous pouvez utiliser le SDK de recherche de Yahoo! pour accéder à Yahoo! depuis Python. Voici les étapes à suivre pour l'installer :

```shell
$ wget http://developer.yahoo.com/download/files/yws-2.12.zip
$ unzip yws-2.12.zip
$ cd yws-2.12/Python/pYsearch*/
$ sudo python setup.py install
```

Exemple :

```python
# importing news search API
>>> from yahoo.search.news import NewsSearch
>>> srch = NewsSearch('YahooDemo', query='London') 
# Fetch Results
>>> info = srch.parse_results()
>>> info.total_results_available
41640
>>> info.total_results_returned
10
>>> for result in info.results:
... print "'%s', from %s" % (result['Title'], result['NewsSource'])
...
'Afghan Handover to Be Planned at London Conference, Brown Says', from Bloomberg
.........
```
