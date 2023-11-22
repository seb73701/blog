---
title: Comment afficher l'adresse email des expéditeurs dans la boîte de réception
description: 
published: true
date: 2023-04-21T10:48:44.756Z
tags: beta, outlook, adresse
editor: markdown
dateCreated: 2023-04-21T10:48:40.540Z
---

Par défaut, Microsoft Outlook ne permet pas d'afficher l'adresse email dans les colonnes disponibles dans la boîte de réception.

Il affiche simplement le nom du contact dans la colonne **"DE"** :

![outlook_contact_nom.png](/assets/img/apps/microsoft_outlook/outlook_contact_nom.png)

Pour avoir l'information, il va falloir créer un champ basé sur une formule.

## 1. Création du champ "Email"

+ Cliquer sur le dossier **"Boîte de réception"** de notre compte
+ Dans le ruban, cliquer sur l'onglet **"Affichage"**

![outlook_affichage_colonne_01.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_01.png)

+ Cliquer sur l'option **"Paramètres d'affichage"**
+ Cliquer sur le bouton **"Colonnes..."** dans la fenêtre qui apparaît

![outlook_affichage_colonne_02.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_02.png)

+ Cliquer sur le bouton **"Nouvelle colonne..."**

![outlook_affichage_colonne_03.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_03.png)

+ Compléter les champs suivants :
	+ Nom : `Email`
  + Type : `Formule`

+ Cliquer sur le bouton **"Modifier"**

![outlook_affichage_colonne_04.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_04.png)

+ Saisir la formule ci-dessous

```
[De]+IIf(InStr([SearchFromEmail],"@")=0,"",IIf(De=[SearchFromEmail],""," <"+[SearchFromEmail]+">"))
```

> Attention, le champ `[De]` peut changer en fonction de la langue du logiciel ceci peut donner `[From]` pour la version anglaise).
{.is-warning}


![outlook_affichage_colonne_05.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_05.png)

+ Valider sur le bouton **"OK"**

![outlook_affichage_colonne_06.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_06.png)

+ Cliquer sur le bouton **"OK"** pour valider.


## 2. Positionnement de la colonne "Email"

+ Dans la liste de droite, cliquer sur la ligne **"Email"**, puis cliquer sur le bouton **"Monter"** jusqu'à la position voulue de la colonne.

![outlook_affichage_colonne_07.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_07.png)

+ Dans la liste de droite, cliquer sur la ligne **"De"**, puis cliquer sur le bouton **"<-Supprimer"** pour retirer la colonne de l'affichage.

## 3. Vérification de la colonne "Email"

+ Cliquer sur le dossier **"Boîte de réception"** de notre compte
+ Valider qie la colonne **"Email"** contient bien l'adresse email de l'expéditeur à la place du nom du contact.

![outlook_affichage_colonne_08.png](/assets/img/apps/microsoft_outlook/outlook_affichage_colonne_08.png)