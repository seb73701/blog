---
title: Prise d'instantané avec VirtualBox
description: VirtualBox
published: true
date: 2023-04-21T11:20:32.075Z
tags: beta, virtualisation, virtualbox, vbox, oracle
editor: markdown
dateCreated: 2023-04-21T11:20:27.621Z
---

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Explication

Effectuer une sauvegarde virtuelle rapide en prenant un instantané.

Un instantané est une sauvegarde de la machine virtuelle qui peut être effectuée en direct. Il s'agit d'un moyen rapide de conserver l'état des lieux avant l'installation d'un logiciel, l'ajout de patchs ou de pilotes. L'utilisation d'instantanés permet de revenir à un état antérieur, c'est une sorte de restauration du système. Toutes les technologies de virtualisation offrent cette possibilité.

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prérequis

<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir VirtualBox d'installé 
<i class="fas fa-check" style="color:green;margin-right:10px;"></i>Avoir de l'espace disque

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Prendre un instantané

Sélectionner la machine virtuelle sur l'interface principale de Virtualbox.
Cliquer sur l'icône en bout de ligne puis sur **Instantanés** depuis le sous-menu.

![virtualbox_instantanes.png](/assets/img/virtualisation/virtualbox/virtualbox_instantanes.png)

Cliquer sur le bouton **Prendre** qui se trouve dans la partie de droite.


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Nommer et décrire l'instantané

Saisir un nom pour l'instantané et une description si nécessaire.

![virtualbox_instantanes_nommer.png](/assets/img/virtualisation/virtualbox/virtualbox_instantanes_nommer.png) 
  
  
# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Terminer l'instantané

Cliquer sur **OK** pour enregistrer l'instantané et notez qu'il apparaît désormais dans la liste de ceux de la machine virtuelle.

![virtualbox_instantanes_liste.png](/assets/img/virtualisation/virtualbox/virtualbox_instantanes_liste.png)


# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Restaurer un instantané

Sélectionner dans la liste l'instantané qui doit être restauré
Cliquer avec le bouton droit de la souris dessus et choisir l'option **Restaurer**

# <span style="color:darkgreen;"><i class="fas fa-caret-right" style="margin-right:10px;"></i>Confirmer la restauration de l'instantané

Confirmez la restauration en sélectionnant **Restaurer** sur l'écran d'avertissement. 
  
> **Notez bien que l'état en cours de la machine sera perdu et ne pourra plus être récupéré.**
{.is-warning}


