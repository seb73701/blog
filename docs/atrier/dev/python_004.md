# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir une VM avec un système d'exploitation sous Rocky Linux 8
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir un système d'exploitation à jour (`dnf update`)

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Installation de Python3

## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de la version par defaut (3.6.8)

> La version par défaut actuellement disponible avec la distribution Rocky Linux 8 est la version **3.6.8**
{.is-info}

- Installer **Python3 (3.6.8)** :  
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install python3
```

- Vérifier la version de **Python3** :
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
  
```shell
python3 -V
```
  
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.6.8
```  

- Vérifier que **Python3** est bien installé et soit fonctionnel en lançant l'interpréteur de Python :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
python3
```
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.6.8 (default, Sep 21 2021, 18:40:00)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```    
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de la version 3.8

> La version par défaut actuellement disponible avec la distribution Rocky Linux 8 est la version **3.6.8** mais il est possible d'installer une autre version en parallèle.
{.is-info}

- Installer **Python3.8** :  
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install python38
```
  
- Vérifier la version de **Python3.8** :
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
  
```shell
python3.8 -V
```
  
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.8.6
```  

- Vérifier que **Python3.8** est bien installé et soit fonctionnel en lançant l'interpréteur de Python :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
python3.8
```
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.8.6 (default, Jun 29 2021, 21:14:45)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```    
  
## <span style="color:darkgreen;"><i class="fas fa-caret-down" style="margin-right:10px;"></i>Installation de la version 3.9

> La version par défaut actuellement disponible avec la distribution Rocky Linux 8 est la version **3.6.8** mais il est possible d'installer une autre version en parallèle.
{.is-info}

- Installer **Python3.9** :  
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>

```shell
dnf install python39
```
  
- Vérifier la version de **Python3.9** :
  
<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>
  
```shell
python3.9 -V
```
  
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.9.2
```  

- Vérifier que **Python3.9** est bien installé et soit fonctionnel en lançant l'interpréteur de Python :

<span style="color:coral;"><i class="fas fa-laptop-code" style="margin-right:10px;"></i>_Shell :_</span>  
  
```shell
python3.9
```
  
<span style="color:green;"><i class="fas fa-desktop" style="margin-right:10px;"></i>_Sortie :_</span>

```shell
Python 3.9.2 (default, Jun 11 2021, 17:22:26)
[GCC 8.4.1 20200928 (Red Hat 8.4.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
```