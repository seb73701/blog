# Comment installer Python 3 sur Rocky Linux 8

## Prérequis

- Avoir une VM avec un système d'exploitation sous Rocky Linux 8
- Avoir un système d'exploitation à jour (`dnf update`)

## Installation de Python 3 (par défaut 3.6.8)

La version par défaut actuellement disponible avec la distribution Rocky Linux 8 est la version **3.6.8**

- Installer Python 3.6.8 :  
  
**_Shell :_**

```shell
[root@localhost ]# dnf install python3
```

- Vérifier la version de Python 3 :
  
**_Shell :_**
  
```shell
[root@localhost ]# python3 -V
```
  
**_Sortie :_**

```shell
Python 3.6.8
```  

- Vérifier que Python 3 est bien installé et soit fonctionnel en lançant l'interpréteur de Python :

**_Shell :_**

```shell
[root@localhost ]# python3
```
  
**_Sortie :_**

```shell
Python 3.6.8 (default, Sep 21 2021, 18:40:00)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

## Installation de Python 3.x (> 3.6.8)

La version par défaut actuellement disponible avec la distribution Rocky Linux 8 est la version 3.6.8 mais il est possible d'installer une autre version en parallèle.

- Installer Python 3.x :  
  
**_Shell :_**

```shell
[root@localhost ]# dnf install python3x
```

- Vérifier que Python 3.x est bien installé et soit fonctionnel en lançant l'interpréteur de Python :

**_Shell :_**

```shell
[root@localhost ]# python3.x
```

Exemple pour une version 3.8.6 :

**_Sortie :_**

```shell
Python 3.8.6 (default, Jun 29 2021, 21:14:45)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

## Installation de Python 3.x via les modules DNF

Exemple d'installation de Python 3.8

**_Shell :_**
  
```shell
[root@localhost ]# dnf module install python38
```

Exemple d'installation de Python 3.9

**_Shell :_**
  
```shell
[root@localhost ]# dnf module install python39
```
