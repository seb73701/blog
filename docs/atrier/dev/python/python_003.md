# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>50 commandes Python essentielles</span>

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>01. Importer des modules</span>

La force de Python est sa capacité être étendu. La première étape dans de nombreux programmes est d'importer les modules nécessaires.
La déclaration d'importation la plus simple est `import modulename`. Dans ce cas, les fonctions et objets fournis ne sont pas dans le namespace général. Vous devez les appeler en utilisant le nom complet (*modulename.methodname*). Vous pouvez raccourcir la partie "modulename" avec la commande `import modulename as mn`. Vous pouvez ignorer ce problème complètement avec la commande `from modulename import *` pour tout importer à partir du module donné. Ensuite, vous pouvez appeler ces fonctionnalités. Si vous avez besoin de quelques éléments, vous pouvez les importer de façon sélective en remplaçant le "*" par la méthode ou le nom de l'objet.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>02. Recharger des modules</span>

Lorsqu'un premier module est importé, toutes les fonctions d'initialisation sont exécutées.
Cela peut impliquer la création d'objets de données ou l'initiation de connexions. Mais, cela ne se fait que la première fois dans une session donnée. Importer le même module ne ré-exécutera aucun des codes initialisation. Si vous souhaitez que ce code est relancé, vous devez utiliser la commande de rechargement. Le format est `reload (modulename)`. Le dictionnaire de l'importation précédente n'est pas vidé, mais écrasé. Cela implique que toutes les définitions qui ont changé entre l'importation et le rechargement sont mises à jour. Mais si vous supprimez une définition, l'ancien sera bloqué et toujours accessible. Il peut y avoir d'autres effets secondaires, alors utilisez cette commande avec précaution.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>03. Installer de nouveaux modules</span>

Alors que la plupart des commandes doivent être exécutées dans une session Python, il existe quelques commandes essentielles qui doivent être exécutées en dehors de Python. Le premier est `pip`. L'installation d'une module consiste à télécharger le code source et à compiler tout code externe inclus. Heureusement, il existe un dépôt de centaines de modules Python disponibles sur http://pypi.python.org. Au lieu de tout faire manuellement, vous pouvez installer un nouveau module avec `pip install modulename`. Cette commande effectuera une vérification de dépendance et installera les modules manquants avant d'installer celle que vous avez demandée. Vous pourriez avoir besoin de droits d'administrateur pour installer ce module dans la bibliothèque de votre PC. Sur une machine Linux, vous exécutez la commande `pip` avec `sudo`. Sinon, vous pouvez l'installer dans votre répertoire personnel de bibliothèque en ajoutant le paramètre `-user` dans la ligne de commande.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>04. Exécuter un script</span>

L'importation d'un module exécute le code dans le fichier du module via le code de maintenance du module via le code de maintenance du module de Python. Ce code de maintenance traite l'exécution du code d'initialisation. Pour suivre un script Python et exécuter le code brut dans la session en cours, utilisez la commande `execfile ("filename.py")`, où l'option principale est une chaîne contenant le fichier Python à exécuter. Toutes les définitions sont chargées dans les sections locales et globales. Vous pouvez éventuellement inclure deux paramètres dans la commande `execfile`. Ces deux options sont à la fois des dictionnaires, l'un pour un ensemble de variables globales. Si vous livrez un seul dictionnaire global. La valeur de retour de cette commande est `None`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>05. Un shell amélioré</span>

Le shell interactif par défaut est fourni via la commande `python`, mais est plutôt limité. Un shell amélioré est fourni par la commande `ipython`. Il fournit beaucoup de fonctionnalités supplémentaires au développeur de code. Un système d'historique approfondi est disponible, pour accèder non seulement aux commandes de la session en cours, mais aussi aux sessions précédentes. Il existe également des commandes magiques qui offrent des moyens améliorés d'intéragir avec la session Python actuelle. Pour des interactions plus complexes, vous pouvez créer et utiliser des macros. Vous pouvez également examiner l'historique de la session en cours et décompiler le code Python. Vous pouvez même créer des profils qui vous permettent de gérer les étapes d'initialisation que vous devrez effectuer chaque fois que vous utilisez iPython.


## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>06. Evaluer le code</span>

Parfois, vous pouvez avoir des bouts de code qui sont mis en place. Si ces bouts de code sont assemblés en tant que chaîne, vous pouvez exécuter le résultat avec la commande `eval("code_string")`. Les erreurs de syntaxe dans le code sont signalées comme des exceptions. Par défaut, ce code est exécuté dans la session en cours, en utilisant les dictionnaires globaux et locaux. La commande `eval` accepte deux autres paramètres pour fournir un ensemble différent de dictionnairs global et local. S'il n'y a qu'un seul paramètre supplémentaire, on suppose qu'il s'agit d'un dictionnaire global. Vous pouvez transférer un bout de code et créer avec la commande de compilation au lieu de la chaîne de code. La valeur de retour de cette commande est `None`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>07. Asserter des valeurs</span>

A un moment donné, nous avons tous besoin de déboguer un code que nous essayons d'écrire. L'un des outils utiles à cet égard est la notion d'assertion. La commande `assert`prend une expression Python et vérifie si elle est vraie. Si c'est le cas, l'exécution continue. Si ce n'est pas vrai, une erreur `AssertionError` est générée. De cette façon, vous pouvez vérifier les hypothèses faites dans votre code. Vous pouvez éventuellement ajouter un 2e paramètre à la commande `assert`. Ce 2e paramètre est l'expression de python qui s'exécute si l'assertion échoue. Habituellement, il s'agit d'un type de message d'erreur détaillé qui est affiché. Ou, vous voudrez peut-être inclure un code de nettoyage qui tente de remédier à l'affirmation échouée. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>08. Fonctions de mapping</span>

Une tâche courante avec les programmes modernes et de mapper un calcul donné à une liste complète d'éléments. Python fournit la commande `map()` pour accomplir exactement cela. `map` renvoie une liste des résultats de la fonction appliquée à chaque élément d'un objet itératif. `map` peut effectivement prendre plus d'une fonction et plus d'un objet itérable. S’il est donné plus d'une fonction, une liste de tuples est renvoyée, chaque élément du tuple contenant les résultats de chaque fonction. S'il y a plus d'une entrée itérative, alors le mapping suppose que les fonctions prennent plus d'un paramètre d'entrée, donc elles les prendront à partir des itérables. Cela suppose implicitement que les itérables sont de la même taille et qu'ils sont tous nécessaires en tant que paramètres pour la fonction donnée. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>09. Virtualenvs</span>

En raison de la complexité de Python, il est préférable de configurer un environnement propre dans lequel installer les modules dont vous avez besoin pour un projet donné. Dans ce cas, vous pouvez utiliser la commande `virtualenv` pour initialiser l'environnement. Si vous créez un répertoire nommé **ENV**, vous pouvez créer un nouvel environnement avec la commande `virtualenv ENV`. Cela créera les sous-répertoires **bin**, **lib** et **include**. Vous pouvez commencer à utiliser ce nouvel environnement en obtenant le script `ENV/bin/activate`, qui chargera plusieurs variables, telles que le **PATH**. Vous pouvez générer le script `ENV/bin/desactivate` Pour réinitialiser l'environnement de votre Shell à sa condition précédente. De cette façon, vous pouvez avoir des environnements qui n'ont que les modules dont vous avez besoin pour un ensemble de tâches données. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>10. Boucles</span>

Bien que n'étant pas des commandes, tout le monde doit savoir comment utiliser les boucles. Les 2 principaux types de boucles sont un nombre fixe d'itérations (` for `) et une boucle conditionnelle (` while `). Dans une boucle ` for `, vous faites itérer la boucle entre deux valeurs. Vous continuez jusqu’à ce que vous avez traité chaque élément ou que vous rencontriez une commande de sortie de boucle. Dans une boucle `while`, vous exécutez la boucle tant qu’une expression de test est fausse. Alors que les boucles peuvent être quittées plus tôt en utilisant une commande de sortie, vous pouvez sauter des lignes de code dans une boucle à l’aide d’une commande ` continue ` pour arrêter sélectivement ou conditionnellement l’itération actuelle et passer à la prochaine. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>11. La fonction filter</span>

Alors que la commande `map` renvoie un résultat pour chaque élément dans une boucle, `filter` ne renvoie un résultat que si la fonction évaluée renvoie à une valeur `True`. Cela signifie que vous pouvez créer une nouvelle liste d'éléments où seuls les éléments qui satisfont à certaines conditions sont utilisées. À titre d'exemple, si votre fonction vérifie que les valeurs sont des nombres compris entre 0 et 10, elle crée une nouvelle liste sans nombre négatif et aucun chiffre supérieur à 10. Cela pourrait être accompli avec une boucle `for`, mais cette méthode est beaucoup plus propre. Si la fonction fournit pour filtrer et `None`, on suppose que c’est la fonction d'égalité. Seuls les éléments qui évaluent `True` sont renvoyés dans le cadre de la nouvelle liste. Il existe des versions itérables de filtres disponibles dans le module **itertools**. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>12. Fonction Reduce</span>

Dans de nombreux calculs, l'un des calculs que vous devez faire est une opération de réduction. Cela consiste à prendre une liste de valeur et à la réduire à une seule valeur. Avec Python, vous pouvez utiliser la commande `reduce(fonction, itérable)` pour appliquer la fonction de réduction à chaque paire d'éléments de la liste. Par exemple, si vous appliquez l'opération de réduction à la liste des 5 premiers entiers, vous obtiendrez le résultat ((((1+2)+3)+4)+5). Vous pouvez éventuellement ajouter un 3e paramètre pour agir comme terme initialisation. Il est chargé avant tout autre élément, et est renvoyé par défaut si le résultat est nul. Vous pouvez utiliser une fonction lambda comme paramètre de fonction à réduire pour garder votre code aussi compact que possible. Dans ce cas, rappelez-vous qu'il ne faut indiquer que 2 paramètres d'entrée. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>13. Votre liste est-elle vraie (True) ou fausse (False) ?</span>

Dans certains cas, vous avez peut-être collecté un certain nombre d'éléments dans une liste qui peut être évaluée comme `True` ou `False`. Par exemple, vous avez peut-être exploité un certain nombre de possibilités grâce à votre calcul et avez créé une liste de celles qui ont été transmises. Vous pouvez utiliser la commande `any(list)` pour vérifier si certains des éléments de votre liste sont vrais. Si vous devez vérifier que tous les éléments sont vrais (`True`), vous pouvez utiliser la commande `all(list)`. Ces 2 commandes renvoient la valeur `True` si la condition pertinente est satisfaite et un `False` dans le cas contraire. Il se comporte différemment si l'objet itératif est vide. La commande `all` renvoie `True` si le résultat est nul, alors que la commande `any` renvoie la valeur `False` lorsque le résultat est nul. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>14. Enumerate</span>

Parfois, nous devons étiqueter les éléments qui résident dans un objet itératif avec leurs indices afin qu'ils puissent être traités. Vous pouvez le faire en bouclant explicitement chacun des éléments est en créant une liste énumérée. La commande `enumerate` fait ceci en une seule ligne. Il faut un objet itératif et créer une liste de tuples avec le résultat. Chaque tuple possède l'indice 0 de l'élément, ainsi que l'élément lui-même. Vous pouvez éventuellement démarrer l'indexation à partir d'une autre valeur en incluant un 2nd paramètre optionnel. Par exemple, vous pouvez énumérer une liste de noms avec la liste des commandes (enumerate(noms, début=1)). Dans cet exemple, nous avons décidé de commencer l'indexation à 1 au lieu de 0. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>15. Casting</span>

Les variables dans Python ne contiennent aucune information de type, ils peuvent être utilisés pour stocker n'importe quel type d'objet. Les données réelles, cependant, sont d'un autre genre. De nombreux opérateurs, comme l'addition, suppose que les valeurs d'entrée sont du même type. Très souvent, l'opérateur utilise le type de conversion nécessaire si vous avez besoin de convertir vos données d'un type à un autre, il existe une classe de fonctions qui peuvent être utilisées pour effectuer ce processus de conversion. Ceux que vous êtes le plus susceptible d'utiliser sont les `abs`, les ` bin `, les `bool`, les `chr`, les ` complexes `, les ` flottants `, les ` hex `, les `int`, les `long`, les `oct ` et ` str `. Pour les fonctions de conversion il existe un ordre de priorité où certains types. Par exemple, les nombres entiers sont “inférieurs” aux nombres flottants. Lors de la conversion, une certaine quantité d'informations est perdue. Par exemple, lors de la conversion d'un nombre flottant en nombre entier, Python tronque le nombre à zéro. 

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>16. Qu'est-ce que c'est ?</span>

Vous pouvez vérifier pour voir de quelle classe cet objet est une instance avec la commande `isinstance(object,class)`. Cette commande renvoie une valeur booléenne.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>17. Est-ce une subclass ?</span>

La commande `issubclass(class1,class2)` vérifie si class1 est une sous-classe de class2. Si class1 et class2 sont identiques, elle renvoie la valeur `True`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>18. Objets globaux</span>

Vous pouvez obtenir un dictionnaire de la table de symboles globaux pour le module actuel avec la commande `globals()`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>19. Objets locaux</span>

Vous pouvez accéder à un dictionnaire mis à jour de la table de symboles locale actuelle en utilisant la commande `local()`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>20. Variables</span>

La commande `vars(dict)` renvoie des éléments écrits pour un objet. Si vous utilisez `vars()`, il se comporte comme `local()`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>21. Créer une variable globale</span>

Une liste de noms peut être interprétée comme globale pour l'ensemble du bloc de code avec la commande `global names`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>22. Nonlocales</span>

Dans Python 3.x, vous pouvez accéder aux noms du champ d'inclusion le plus proche avec la commande `nonlocal names`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>23. Soulever une exception</span>

Lorsque vous identifiez une condition d'erreur, vous pouvez utiliser la commande `raise` pour vider une exception. Vous pouvez ajouter un type d'exception et une valeur.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>24. Traiter une exception</span>

Les exceptions peuvent être traitées dans un bloc `try-except`. Si le code dans le bloc `try` soulève une exception, alors c'est le code présent dans le bloc `except` qui est exécuté.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>25. Méthodes statiques</span>

Vous pouvez créer une méthode statique, similaire à celle existante sur Java ou C++, avec la commande `staticmethod(nom_de_fonction)`.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>26. Ranges</span>

Vous avez peut-être besoin d'une liste de nombres, peut-être dans une boucle `for`. La commande `range()` peut créer une liste itérative d'entiers. Avec un paramètre, il passe de 0 au nombre donné. Vous pouvez fournir un numéro de début falcutatif, ainsi qu'une taille de pas.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>27. Xrange</span>

Un problème avec les plages d'entiers est que tous les éléments doivent être calculés à l'avance et sotckés en mémoire. La commande `xrange()` prend les mêmes paramètres et fournit le même résultat, mais ne calcule que l'élément suivant lorsqu'il est nécessaire.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>28. Itérations/span>
  
L'itération est une façon très pythonesque de faire les choses. Pour les objets qui ne sont pas intrasèquement itérables, vous pouvez utiliser la commande `iter(object_name)` pour envelopper votre objet et fournir une interface itérative à utiliser avec d'autres fonctions et opérateurs.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>29. Listes triées</span>
  
Vous pouvez utiliser la commande `sorted(list1)` pour trier les éléments d'une liste. Vous pouvez lui indiquer une fonction de comparaison personnalisée, et pour des éléments plus complexes, vous pouvez inclure une fonction clé qui élimine une propriété de classement de chaque élément à des fins de comparaison.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>30. Somme d'éléments</span>
  
Un type spécifique d'opération de réduction, la somme est assez courant pour justifier l'inclusion d'un cas spécial, la commande `sum(iterable_object)`. Vous pouvez inclure un deuxième paramètre qui fournira une valeur de départ à la fonction destinée à faire la somme des éléments concernés.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>31. Modules With</span>
  
La commande `with` permet d'englober un bloc de code avec des méthodes définies par un gestionnaire de contexte. Cela peut aider à nettoyer le code et vous permet de lire plus facilement ce que le code spécifique est supposé faire quelques mois plus tard. Un exemple classique d'utilisation de `with` est avec des opérations sur les fichiers. Vous pouvez utiliser `with open("myfile.txt", "r") as f:`. Cela ouvrira le fichier et le préparera à la lecture. Vous pouvez lire le fichier dans le bloc de code avec `data = f.read()`. La meilleure partie de cette instruction est que le fichier sera fermé en sortant du bloc de code, quel qu'en soit le motif. Donc, même si le bloc de code soulève une exception, vous n'avez pas à vous soucier de fermer le fichier dans le cadre de votre gestionnaire d'exception. Si vous avec un exemple `with` plus compliqué, vous pouvez créer une classe de gestion de contexte pour vous aider à la gérer.
  
> Un exemple classique d'utilisation de `with` est avec des fichiers. La meilleure partie de cette instruction est que le fichier sera automatiquement fermé en sortant du bloc.
  

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>32. Affichage</span>  
  
Le moyen le plus direct d'afficher des données à l'écran passe par la commande d'impression `print`. Cela enverra du texte à la fenêtre de la console. Si vous utilisez la version 2.X de Python, il existe plusieurs façons d'utiliser cette commande. La manière la plus courante consiste à appeler simplement `print` suivi du texte à afficher entre guillemets. Vous pouvez également utiliser la même syntaxe avec toute autre fonction. Donc, l'exemple ci-dessus ressemblent à `print("texte")`. Si vous utilisez la syntaxe de cette fonction, vous pouvez ajouter des paramètres supplémentaires qui vous donnent un meilleur contrôle sur cette sortie. Par exemple, vous pouvez donner le paramètre `file = myfile.txt` et obtenir que la sortie de la commande d'impression se déverse dans un fichier texte donné. Il acceptera également tout objet présentant une représentation de chaîne de caractères.
  
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>33. Memoryview</span>
  
Parfois, vous devez accéder aux données brutes d'un objet, habituellement sous forme de tampon d'octets. Vous pouvez copier ces données et les mettre dans un tableau `bytearray`, par exemple. Mais cela signifie que vous utiliserez une mémoire supplémentaire, et ce ne sera peut-être pas possible pour les gros objets. La commande `memoryview(object_name)` englobe l'objet dans la commande et fournit une interface aux octets bruts. Il donne accès à ces octets un élément à la fois. Dans de nombreux cas, les éléments sont de la taille d'un octet. Mais, selon les détails de l'objet, vous pouvez vous retrouver avec des éléments plus importants que prévu. Vous pouvez trouver la taille d'un élément en octets avec la propriétés `itemize`. Une fois que vous avez créé votre vue mémoire, vous pouvez accéder aux éléments individuels d'une liste (`mem_view[1]. par exemple`).

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>34. Gestion de fichiers</span>
  
Lorsque vous traitez des fichiers, vous devez créer un objet de fichier pour intéragir avec. La commande `file` prend une chaîne avec le nom et l'emplacement du fichier et crée une instance d'objet de fichier. Vous pouvez appeler les méthodes d'objet de fichier comme "ouvrir", "lire" et "fermer", pour extraire les données du fichier. Vous pouvez utiliser la méthode `readline`. Lors de l'ouverture d'un fichier, il existe une commande `open()` explicite. Il prend une chaîne avec le nom du fichier et un paramètre optionnel qui est une chaîne qui définit le mode. La valeur par défaut est d'ouvrir le fichier en lecture seule ("r"). Vous pouvez l'ouvrir pour écrire ("w") et ajouter ("a"). Après avoir le fichier, un objet de fichier est retourné afin que vous puissiez intéragir avec lui. Vous pouvez ensuite le lire, l'écrire et enfin le fermer.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>35. Fonction yield</span>
  
Dans de nombreux cas, une fonction peut nécessiter de passer le contexte d'exécution à une autre fonction. C'est le cas des générateurs. La méthode préférée pour un générateur est qu'il ne fera que calculer la valeur suivante lorsqu'elle est demandée par la méthode `next()`. La commande `yield` enregistre l'état actuel de la fonction d'appel. L'état enregistré du générateur est rechargé et le générateur reprend là où il s'est arrêté afin de calculer la valeur demandée. Il suffit d'avoir de la mémoire disponible pour stocker le minimum pour calculer la valeur requise suivante, plutôt que d'avoir à stocker toutes les valeurs possibles en mémoire simultanément.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>36. Références faibles</span>
  
Vous devez parfois faire référence à un objet, mais être en mesure de le détruire si nécessaire. Une référence faible est celle qui peut être ignorée par le mécanisme de garbage collection. Si les seules références laissées à un objet sont des références faibles, la garbage collection est autorisé à détruire cet objet et à récupérer l'espace pour d'autres utilisations. CEci est utile dans les cas où vous avez des caches ou des mappings de grands ensembles de données qui ne doivent pas rester en mémoire. Si un objet référencé finit par être détruit et que vous essayez d'y accéder, il apparaîtra en tant que `None`. Vous pouvez tester cette condition et recharger les données si c'est une étape nécessaire.
  
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>37. Copie et effacement de données</span>
  
Il existe différentes façons de tracer les données en mémoire lorsque vous avez de pointer les résultats sur un disque dur. L'une d'elles s'appelle pickling. C'est de ce mot que le module **Pickle** tire son nom. Ce dernier constitue un module complet, et pas uniquement une commande. Pour stocker des données sur un disque dur, vous pouvez utiliser la méthode `dump`. Lorsque vous souhaitez copier ces mêmes données à un autre endroit par la suite, vous pouvez utiliser la méthode `load` pour lire les données et les effacer. Le seul inconvénient de Pickle est sa lenteur. Il existeun deuxième module, cPickle, qui offre la même fonctionnalité, mais comme il est écrit en C, il peut être jusqu'à 1000 fois plus rapide. Ce que vous devez garder à l'esprit est que pickle ne stocke aucune information de classe pour un objet. Cela signifie qu'il peut hériter de méthodes et d'attributs différents si la définition de classe a changé entre-temps.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>38. Données rangées</span>

Il faut parfois une permanence structurée de l'objet dans votre session Python. Avec le module **shelve**, vous pouvez créer un magasin d'objets où tout peut y être stocké. Le backend du stockage sur le lecteur peut être ainsi géré par plusieurs systèmes, tels que **dbm** ou **gdbm**. Une fois que vous avez ouvert un module **shelve**, vous pouvez lire et écrire en utilisant des paires de valeurs clés. Une fois terminé, vous devez vous assurer de fermer le module de façon explicite pour le synchroniser avec le fichier. En raison de la manière dont les données peuvent être stockées dans la base de données de sauvegarde, il est préférable de ne pas ouvrir les fichiers en dehors du module **shelve** de Python. Vous pouvez l'ouvrir avec `writeback` réglé sur `True`. Dans l'affirmative, la méthode `sync` se chargera d'écrire mes modifications mises en cache.
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>39. Threads</span>

Vous pouvez lancer plusieurs  threads d’exécution au sein de Python. La commande `thread()` peut créer un nouveau thread d’exécution. Il suit les mêmes techniques que celles des threads POSIX, Lorsque vous créez un thread, vous devez indiquer un nom de fonction, ainsi que tous les paramètres. Gardez à l’esprit que ses threads se comportent comme les threads POSIX. Vous devez gérer les verrous mutex (avec les méthodes `acquire` et `release`), ainsi que `lock`. Lorsque vous avez terminé, vous devez quitter avec `exit` pour vous assurer qu’il est correctement nettoyé et qu’aucune ressource n’est laissé en l’état. Vous disposez aussi d’un contrôle de précision sur les threads, en réglant par exemple la taille de la pile pour faire de la place pour de nouveaux threads.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>40. Saisie de données</span>

Vous devez collecter des commentaires. La commande `input()` prend la forme d’une invite à afficher à l’utilisateur, et attend que l’utilisateur tape une réponse. Une fois que l’utilisateur a terminé de taper et appuie sur la touche Entrée, le texte est retourné à votre programme. Si le module `readline` a été chargé avant l’entrée d’appel, vous aurez une fonctionnalité d’édition et d’historique. Cette commande passe le texte par `eval` en premier et peut provoquer des erreurs non gérées. Vous pouvez utiliser la commande `raw-input()` pour ignorer ce problème. Cette commande renvoie la chaîne non modifiée telle que saisie par l’utilisateur. Encore une fois, vous pouvez utiliser le module `readline` pour améliorer l’édition des données saisies.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>41. Variables internes</span>

Pour les personnes ayant pratiqué d’autres langages de programmation, il existe une notion selon laquelle certaines variables ou méthodes ne sont disponibles que dans le cadre d’un objet. Dans Python, il n’y a pas de concept de ce genre. Tous les éléments d’un objet sont accessibles. Il existe une règle de style qui peut imiter ce type de comportement. Tous les noms qui commencent par un trait de soulignemeent devraient être traités comme s’ils étaient des noms internes et être conservés comme privés de l’objet. Ils ne sont pas cachés, cependant, et il n’y a pas de protection explicite pour ces variables ou méthodes. Il incombe au programmeur d’honorer l’intention de l’auteur de la classe et de ne modifier aucun de ces noms internes. Vous pouvez créer ces types de modifications si cela s’avère nécessaire.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>42. Comparaison d’objets</span>

Il existe plusieurs façons de comparer des objets au sein de Python, avec plusieurs mises en garde. La première est que vous pouvez tester deux choses entre les objets : l’égalité et l’identité. Si vous  testez l’identité, vous testez pour voir si deux noms se référent réellement au même objet d’instance. Cela peut se faire avec la commande `cmp(obj1,obj2)`. Vous pouvez également tester cette condition en utilisant le mot-clé `is`. Par exemple, `obj1 est obj2`. Si vous testez l’égalité, vous testez pour voir si les valeurs des objets mentionnés par les deux noms sont égales. Ce test est traité par l’opérateur `==`, comme dans `obj1 == obj2`. Les tests d’égalité peuvent devenir beaucoup plus compliqués à réaliser pour des objets plus complexes.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>43. Slices</span>

Même si ce n’est pas vraiment une commande, les **slices** sont un concept trop important pour ne pas être mentionnés. Les éléments d’indexation dans les structures de données, comme les listes, sont l’une des choses les plus courantes dans Python. Vous pouvez sélectionner un seul élément en donnant une valeur d’index unique. Sélectionnez une gamme d’éléments en donnant un indice de départ et un index final, séparés par deux points. Elle prend la forme d’une nouvelle liste que vous pouvez enregistrer dans un nouveau nom de variable, modifier la taille des étapes, et sauter certains éléments. Ainsi, vous pouvez saisir chaque élément de la liste `a` avec la tranche `a[1::2]`. Cela commence à l’indice **1**, se poursuit **jusqu’à la fin** et passe les valeurs d’index **2** à la fois. Les tranches peuvent recevoir des valeurs d’indice négatives, auquel cas elles commencent à partir de la fin de la liste.
  
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>44. Expressions lambda</span>

Puisque les objets, et les noms qui leur indiquent, sont des choses vraiement différentes, vous pouvez avoir des objets qui n’ont aucune référence à eux. Un exemple est l’expression `lambda`, laquelle permet de créer une fonction anonyme pour utiliser des techniques de programmation fonctionnelle au sein de Python. Le format est le mot-clé `lambda`, suivi d’une liste de paramètres, puis d’un deux-points et du code de fonction. Vous pouvez construire votre propre fonction pour calquer un nombre avec `lambda x:x * x`. Vous pouvez alors avoir une fonction capable de créer de nouvelles fonctions par programme et les renvoyer au code d’appel. Avec cette fonctionnalité, vous pouvez créer des générateurs de fonctions pour avoir des programmes auto-modifiables. La seule limitation est qu’ils sont limités à une seule expression.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>45. Compiler des objets</span>

Python est un langage interprété, ce qui signifie que le code source que vous écrivez doit être compilé dans un format bytecode. Ce bytecode est ensuite traité par le moteur d’exécution de Python pour parcourir les instructions. Dans votre programme, vous devrez peut-être prendre le contrôle de la conversion du code sur le bytecode et l’exécution des résultats. Peut-être que vous souhaitez constuire votre propre REPL, La commande `compile()` prend en objet de chaîne qui contient une collection de code Python et renvoie un objet qui représente une traduction en bytecode de ce code. Ce nouvel objet peut ensuite être transmis à `eval()` ou `exec()` pour être effectivement exécuté. Vous pouvez utiliser le paramètre `mode =` pour indiquer de compiler quel type de code est compilé. Le mode `unique` est une déclaration unique, `eval` est une expression unique et `exec` est un bloc de code entier.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>46. Méthode `__init__`</span>

Lorsque vous créez une nouvelle classe, vous pouvez inclure une méthode d’initialisation privée qui est appelée lorsqu’une nouvelle instance de la classe est créée. Cette méthode est utile lorsque la nouvelle instance nécessite certaines données pour être chargées dans le nouvel objet.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>47. Méthode `__del__`</span>

Lorsqu’un objet d’instance est sur le point d’être détruit, on recourt à la méthode `__del__`. Cela vous donne la possibilité de faire tout type de nettoyage. Il pourrait par exemple s’agir de la fermeture des fichiers, ou la déconnexion réseau. Une fois ce code exécuté, l’objet est finalement détruit et les ressources sont libérées.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>48. Sortir du programme</span>

Il existe deux pseudo-commandes disponibles pour quitter l’interpréteur Python : `exit()` et `quit()`. Ils prennent tous deux un paramètre facultatif qui définit le code de sortie pour le processus. Si vous souhaitez quitter un script, vous feriez mieux d’utiliser la fonction de sortie du module `sys` (`sys.exit(exit_code)`).

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>49. Valeurs de retour</span>

Les fonctions peuvent nécessiter une certaine valeur pour la fonction d’appel. Etant donné qu’aucun nom n’a de type, cela inclut des fonctions. Les fonctions peuvent utiliser la commande `return` pour renvoyer tout objet à l’appelant.

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>50. Concaténation de chaînes</span>

Nous terminons par l’élément qui démarre la plupart des listes de commandes Python, à savoir la concaténation. La manière la plus simple de construire des chaînes est d’utiliser l’opérateur `+`. Pour inclure d’autres éléments, utilisez la fonction `str()` pour convertir en un objet chaîne.