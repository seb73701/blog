# Comment installer Python 3 sur Windows 10

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-09-20</span>


## Prérequis

Avant de vouloir installer Python 3, on va vérifier si une version est déjà installée.

+ Appuyez sur la touche `[Windows]`, tapez `cmd.exe`.

ou 

+ Appuyez sur les touches `[Windows]+[R]` puis tapez `cmd.exe`.


Dans l'invite de commande, tapez la commande ci-dessous.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```
python --version
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

![Python - Installation](../../../img/dev/python/python_version.png)

Tapez ensuite la commande ci-dessous pour connaître le chemin d'installation de Python.

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```
where python
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

![Python - Installation](../../../img/dev/python/python_where.png)


Si rien n'apparaît alors c'est que Python n'est pas installé et l'on peut poursuivre les actions ci-dessous.

## Installer Python 3

+ Ouvrir le navigateur et aller sur le site **python.org**
+ Cliquez sur l'onglet **Downloads** en haut de l'écran. La page web détecte automatiquement le système d'exploitation et propose de télécharger la dernière version de Python.
+ Cliquez sur le bouton **Python 3.x.x**

> On peut installer plusieurs versions du moteur Python sur la machine, il n'y aura aucun conflit si on choisit des répertoires différents. Par contre on ne peut pas avoir deux fois le même numéro de version, l'installateur va refuser une deuxième installation. Les professionnels utilisent un `virtualenv`, système particulier pour gérer plusieurs versions de Python en parallèle.

+ Exécuter le fichier téléchargé
+ La fenêtre d'assistant d'installation s'ouvre, cliquez sur le bouton **Customize installation**.

![Python - Installation](../../../img/dev/python/python_install_01.png)

+ Cliquez sur le bouton **Next**.

![Python - Installation](../../../img/dev/python/python_install_02.png)

+ Dans la fenêtre **Advanced Options**, choisir un emplacement pour le répertoire d'installation de Python. Pour cela, il faut cliquer sur le bouton **Browse**, de sélectionner le chemin voulue et de cliquer sur le bouton **OK**.

![Python - Installation](../../../img/dev/python/python_install_03.png)

+ Cliquer sur le bouton **Install**.

![Python - Installation](../../../img/dev/python/python_install_04.png)

+ A la fin de l'installation, un message indique si celle-ci s'est bien déroulée. Je recommande de cliquer sur le bouton **Disable path length limit**.

![Python - Installation](../../../img/dev/python/python_install_05.png)

+ Cliquez sur le bouton **Close**.

![Python - Installation](../../../img/dev/python/python_install_05a.png)

Voilà, l'installation est terminée !

## Tester l'installation

Après avoir installer Python, on va procéder à un test pour valider que l'installation est conforme.

+ Ouvrir le dossier où on a installer Python
+ Double-cliquez sur le fichier ``python.exe`` pour lancer le mode Python en ligne de commande :

![Python - Test](../../../img/dev/python/python_install_06.png)

+ Tapez la commande ci-dessous :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Python :_</span>

```python
print(4+8)
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

![Python - Test](../../../img/dev/python/python_install_07.png)