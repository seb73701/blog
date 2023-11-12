# CRÉATION D'ENVIRONNEMENTS VIRTUELS (`python3 -m venv xxx`)

## Introduction

![python-virtual-environments.png](../../_media/developpement/python/python-virtual-environments.png ':size=250 :no-zoom')


Les programmes Python utilisent souvent des paquets et modules qui ne font pas partie de la bibliothèque standard. Ils nécessitent aussi, parfois, une version spécifique d'une bibliothèque, par exemple parce qu'un certain bogue a été corrigé ou encore que le programme a été implémenté en utilisant une version obsolète de l'interface de cette bibliothèque.

Cela signifie qu'il n'est pas toujours possible, pour une installation unique de Python, de couvrir tous les besoins de toutes les applications. Basiquement, si une application **`A`** dépend de la version **`1.0`** d'un module et qu'une application **`B`** dépend de la version **`2.0`**, ces dépendances entrent en conflit et installer la version **`1.0`** ou **`2.0`** laisse une des deux applications incapable de fonctionner.

La solution est de créer un environnement virtuel, un dossier auto-suffisant qui contient une installation de Python pour une version particulière de Python ainsi que des paquets additionnels.

![python_virtual_env_01.png](../../_media/developpement/python/python_virtual_env_01.png ':size=x300')

Différentes applications peuvent alors utiliser des environnements virtuels différents. Pour résoudre l'exemple précédent où il existe un conflit de dépendances, l'application `A` a son environnement virtuel avec la version `1.0` installée pendant que l'application `B` a un autre environnement virtuel avec la version `2.0`. Si l'application `B` requiert que la bibliothèque soit mise à jour à la version `3.0`, cela n'affecte pas l'environnement de `A`.

![python_virtual_env_02.png](../../_media/developpement/python/python_virtual_env_02.png ':size=x300')

?> Un environnement virtuel est un environnement Python semi-isolé qui autorise les paquets à être installés pour une application particulière, plutôt que d'être installés au niveau du système.


?> `venv` est l'outil standard pour créer des environnements virtuels, et est intégré à Python depuis la version 3.3. Depuis Python 3.4, il installe aussi `pip` dans tous les environnements virtuels créés.


?> `virtualenv` est une alternative tierce à `venv` (*et son prédécesseur*). Elle permet la création d'environnements virtuels pour les versions de Python antérieures à 3.4, qui ne fournissent pas de `venv`, ou ne sont pas capables d'installer automatiquement `pip` dans les environnements créés.

---

## Création d'environnements virtuels

Le module utilisé pour créer et gérer des environnements virtuels s'appelle `venv`. `venv` installe en général la version de Python la plus récente dont vous disposez. Si plusieurs versions de Python sont sur votre système, vous pouvez choisir une version particulière en exécutant `python3.X` où `X` indique la version de votre choix.

Pour créer un environnement virtuel, décidez d'un dossier où vous voulez le placer et exécutez le module `venv` comme un script avec le chemin du dossier :

<span style="color:lightgrey;">_Shell_</span>

```shell
python3 -m venv tutorial-env
```

La commande devra créer le dossier `tutorial-env` si ce dernier n'existe pas ainsi que des sous-dossiers contenant une copie de l'interpréteur de Python et les fichiers nécessaire à l'environnement.



Un répertoire habituel pour un environnement virtuel est `.venv`. Ce nom fait que le répertoire est généralement caché dans votre explorateur de fichiers, et donc non gênant, tout en lui donnant un nom qui explique pourquoi le répertoire existe. Il empêche également de rentrer en conflit avec les fichiers de définition de variable d'environnement `.env` que certains outils utilisent.

---

## Activation de l'environnement

Une fois l'environnement virtuel créé, vous pouvez l'activer.

<span style="color:lightgrey;">_Shell_</span>

```shell
source tutorial-env/bin/activate
```

(Ce script est écrit pour le shell **bash**. Si vous utilisez **csh** ou **fish**, utilisez les variantes `activate.csh` ou `activate.fish`.)


Activer l'environnement virtuel change le prompt de votre ligne de commande pour afficher le nom de l'environnement virtuel que vous utilisez. Cela modifie aussi l'environnement afin, lorsque vous tapez `python`, d'exécuter la version spécifique de Python installée dans l'environnement. Par exemple :

<span style="color:lightgrey;">_Python_</span>

```python
(tutorial-env) [root@localhost projets]# python
Python 3.6.8 (default, Sep 21 2021, 18:40:00)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> sys.path
['', '/usr/lib64/python36.zip', '/usr/lib64/python3.6', '/usr/lib64/python3.6/lib-dynload', '/root/projets/tutorial-env/lib64/python3.6/site-packages', '/root/projets/tutorial-env/lib/python3.6/site-packages']
>>>
```

---

## Désactivation de l'environnement

Pour sortir ou changer d'environnement, il suffit de taper la commande suivante pour quitter l'interpréteur Python :

<span style="color:lightgrey;">_Python_</span>

```python
>>> quit()
(tutorial-env) [root@localhost projets]#
```

Pour sortir de l'environnement :

<span style="color:lightgrey;">_Python_</span>

```python
(tutorial-env) [root@localhost projets]# deactivate
[root@localhost projets]#
```