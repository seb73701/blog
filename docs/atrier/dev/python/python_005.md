# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Le côté fonctionnel classique de Python

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Premiers pas
  
Lancer la commande `python3` sans argument permet d'accéder directement à l'interpréteur.

Il est aussi possible de taper des instructions et blocs de code Python pour les exécuter.

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>

```python
[root@localhost projets]# python3
Python 3.6.8 (default, Sep 21 2021, 18:40:00) 
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> print("Hello World !")
Hello World !
```

> **`print()`** est, comme dans d'autres langages, la fonction qui permet d'envoyer quelque chose sur la sortie standard, l'écran en règle générale.
{.is-info}

Prenez votre éditeur favori et créez un fichier `hello.py` contenant la ligne :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Python :_</span>  
  
```python
print("Hello World !")
```

Puis lancez la commande :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
python3 hello.py
```

<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span> 

```
Hello World !
```

Le résultat est identique.

![da-zero-a-python-in-45-minuti-andrea-colangelo-12-638.jpg](/assets/img/developpement/python/da-zero-a-python-in-45-minuti-andrea-colangelo-12-638.jpg)
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>La commande `python3`

La commande `python3` sans argument déclenche l'interpréteur ; cela permet de lancer des instructions et des fonctions.

`python3` avec un fichier comme argument suppose qu'il s'agira d'instructions et de blocs de code Python à exécuter.

L'extension par defaut des fichiers est suffixé par `.py`.

D'autres arguments peuvent être transmis à la commande `python3`. Pour optenir la liste, il suffit de taper :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>   
  
```shell
python3 -?
```

Voici l'usage de la commande `python3`

```
usage: python3 [option] ... [-c cmd | -m mod | file | -] [arg] ...
```

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>L'indentation comme syntaxe

Avec Python, l'indentation fait partie de la syntaxe du langage ; elle est obligatoire et doit être cohérente pour obtenir un code syntaxiquement correct.

Pour tous les langages informatiques, le composant atomique est l'instruction. Ces instructions sont regroupées en suites logiques : les blocs de code.

Le langage Python ne déroge pas à cette règle, sauf que dans son cas particulier, le bloc de code est défini par son indentation.

Un programme Python est donc structuré par des blocs de code exécutés sous certaines conditions par certaines instructions.

Et dans ces instructions structurantes se retrouvent les tests, les boucles, la définition des fonctions, entre autres, comme pour les autres langages.

Rien d'inhabituel jusqu'ici, mais en Python, cette structure a une syntaxe très simple :

> un entête et un bloc de code

L'entête est constitué par l'instruction (`if`, `while`, `def`,...) ainsi que ses arguments, le tout terminé par le caractère `:`.

Ensuite vient le bloc de code, défini par une indentation supérieure à l'en-tête, comme ceci :

```
<instruction> :
   <bloc de code>
```

Cette forme simple peut être imbriquée autant de fois que nécessaire.

Voici un schéma pour illustrer ce concept :

![schema_code_01.png](/assets/img/developpement/python/schema_code_01.png)

Bien qu'il ne s'agisse pas d'instructions en lagage Python, grâce à l'indentation, on "devine" la structure générale du programme. Voici un exemple très simple pour illustrer la notion de bloc de code :

<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*./indentation4.py*</span>     
     
```python
# fichier : indentation4.py

import random
nombre_au_hasard = random.randint(1,5)
TROUVE = False
nombre_essai = 0

while not TROUVE:
   essai = input("Tapez un nombre de 1 à 5 : ")

   if int(essai) == nombre_au_hasard:
      TROUVE == True
   else:
      print("\n Désolé... \n")
   nombre_essai += 1
print("\n Bravo vous avez trouvé en %s essai(s)" % nombre_essai )
```

Et sa version façon "bloc de code" :

![schema_code_02.png](/assets/img/developpement/python/schema_code_02.png)

Représenté de cette manière, on voit bien l'impact de l'indentation comme syntaxe sur la lisibilité du code.

Voici quelques explications sur ce script.

Le premier bloc charge le module random, qui permet la génération de nombres aléatoires.

Suivent les trois initialisations de variables et la déclaration d'une boucle `while` qui s'exécutera son bloc de code tant que la variable `TROUVE` est fausse.

Dans ce bloc de code, on initialise une variable `essai` avec une saisie de l'utilisateur.

Puis on test si la valeur saisie (préalablement transformée en nombre entier) est égale au nombre choisi au hasard entre 1 et 5 inclus.

Dans l'affirmative, on affecte la valeur `True` (Vrai) à la variable `TROUVE`.

Sinon, on affiche un texte pour avertir l'utilisateur que ce n'est pas la bonne valeur.

Dernière ligne de ce bloc de code : l'incrémentation du compteur d'essais.

Ensuite, on réévalue la condition de la boucle `while` ; si celle-ci est vraie, on exécute encore le bloc de code de celle-ci.

En sortie de boucle, on informe l'utilisateur qu'il a trouvé la bonne valeur en lui indiquant le nombre d'essais.

Et pour illustrer la différence, voici une version dans un langage que vous connaissez un peu mieux : l'inévitable bash.

<span style="color:lightgrey;"><i class="far fa-file-code" style="margin-right:10px;"></i>*./indentation4.sh*</span>     
     
```bash
#! /bin/bash

# fichier : indentation4.sh

RND=$((RANDOM%5+1))
NB_ESSAI=0
FOUND=false
while [ $FOUND != true ]
do
   echo "Tapez un chiffre de 1 à 5"
   read essai
   if [ "$essai" == "$RND" ]
   then
      echo "TROUVE"
      FOUND=true
   else
      echo "Essayez encore ..."
   fi
   NB_ESSAI=$(($NB_ESSAI+1))
done
echo "BRAVO Trouvé en $NB_ESSAI essai(s)"
```