# Comment installer la librairie "matplotlib" sur Windows 10

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-09-20</span>


## Prérequis

+ Python 3 installé

## Installation de la librairie "matplotlib"

+ Ouvrir une invite de commande (`cmd.exe`)
+ Ouvrir l'explorateur de fichiers et allez dans le répertoire d'installation de Python où se trouve le fichier `python.exe`.
+ Disposez les deux fenêtres côte à côte.
+ Faire un glisser/déplacer du fichier `python.exe` vers l'invite de commande. Cela va écrire le chemin complet permettant d'accéder au programme `python.exe`.

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

![Python - Installation Matplotlib](../../../img/dev/python/python_install_pygame_01.png)

+ A la suite de `python.exe`, ajoutez ` -m pip install matplotlib` :

![Python - Installation Matplotlib](../../../img/dev/python/python_install_matplotlib_01.png)

> Attention, cette commande nécessite une syntaxe parfaite. Si on change une seule chose ou si on ajoute un espace, cela ne marchera pas.

+ Appuyez sur la touche `[Entrée]` pour lancer le téléchargement et l'installation de la librairie **Matplotlib**.

![Python - Installation Matplotlib](../../../img/dev/python/python_install_matplotlib_02.png)

> Le message "**Successfully installed ... matplotlib-x.x.x ...**" indique que le processus d'installation s'est terminé avec succès. Comme on peut le voir ci-dessus, certaines dépendances ont aussi étaient installées.