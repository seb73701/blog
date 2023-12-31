---
title: Affichez des textes en dégradé simplement avec HTML et CSS
description: 
published: true
date: 2023-06-12T19:43:47.596Z
tags: 
editor: markdown
dateCreated: 2023-06-12T19:43:47.596Z
---

# Affichez des textes en dégradé simplement avec HTML et CSS

Donnez un peu de vie (et de fantaisie) à vos interfaces avec des textes en dégradés.

<img src="https://cellar-c2.services.clever-cloud.com/content/2023/05/magicpattern-Jek0JdZ0uyw-unsplash.jpg">

**Envie d'un peu de fantaisie, de "néon" ou de kitsch dans vos designs**, rien de tel que du texte coloré avec un dégradé pour pimenter votre site ou votre application web !

## Le fonctionnement
En CSS, il existe une propriété appelée "background-clip" et qui permet d'avoir une **fonctionnalité très utilisée dans les logiciels de designs : le "masque de forme"**.

Pour avoir un texte en dégradé, il suffira de créer un "gradient" en arrière-plan de notre texte, et de créer le masque de forme sur notre texte.

> Voici un exemple très concret, avec quelques lignes de code seulement.
## Tutoriel
### HTML

```html
<span>Gradient text</span>
```

Prenons un élément de texte simple, il peut en réalité s'agir de n'importe quel élément, mais **celui-ci doit pouvoir contenir une couleur de fond**. C'est pour cette raison que la première ligne de notre CSS transforme cet élément en inline-block.

### CSS

```css
display: inline-block;
background: linear-gradient(to left, rgb(255,0,0), rgb(121,121,244));
background-clip: text;
-webkit-background-clip: text;
-webkit-text-fill-color: transparent;
```

**On vient ajouter notre dégradé** (ici un linear-gradient) et notre masque de forme (background-clip) et le tour est joué !

### Le rendu

<img src="https://cellar-c2.services.clever-cloud.com/content/2023/05/image.png">

**Ici le rendu a été fait sur un bouton noir pour améliorer la lisibilité**, mais libre à vous de choisir vos couleurs !

### À noter
**La couverture de cette fonctionnalité (background-clip)** atteint les 98.04% de support (avec préfixe webkit), vous y aurez donc accès sur quasiment tous les navigateurs (hors Internet Explorer).

**L'utilisation des dégradés est très sensible tant sur le design qu'au niveau de l'accessibilité**, et de manière encore plus forte sur du texte. Soyez sûr de vous avant de l'implémenter dans vos design web.

Le masque de forme peut être utilisé pour **créer bien d'autres choses intéressantes**, que je vous laisserai [découvrir ici](https://freefrontend.com/css-background-clip-examples/?ref=code-garage.fr) !

Source : https://code-garage.fr/blog/affichez-des-textes-en-degrades-simplement-avec-html-et-css/