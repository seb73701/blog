---
title: Comment implémenter un effet de puzzle lors du défilement
description: 
published: true
date: 2023-04-21T10:26:42.489Z
tags: beta, css, puzzle, html, javascript
editor: markdown
dateCreated: 2023-04-21T10:26:38.305Z
---

Ajoutez un effet qui anime des éléments quand l'internaute fait défiler la page.

## 1. Début du document HTML

La première chose à faire est de commencer le document HTML, composé des définitions de son conteneur, ce qui contient les parties `head` et `body`. La section `head` sert principalement à faire référence aux ressources CSS et JavaScript externes, tandis que la partie `body` stocke le contenu HTML visible.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Formes animées qui défilent</title>
    <link rel="stylesheet" type="text/css" href="styles.css" />
    <script src="code.js" type="text/javascript"></script>
  </head>
  <body>
    *** ETAPE 2 ICI
  </body>
</html>
```

## 2. Contenu de body

Le contenu principal consiste en un conteneur qui stocke les éléments qui seront animés. Pour cet exemple ce sont des éléments `span` ayant chacun leur propre numéro et dont la mise en style sera déterminée plus tard dans le CSS. Vous pouvez utiliser d'autres types d'élément pour votre projet, comme des éléments `img`.

```html
<article class="jigsawAnim">
  <span>1</span>
  <span>2</span>
  <span>3</span>
  <span>4</span>
  <span>5</span>
  <span>6</span>
  <span>7</span>
  <span>8</span>
</article>
```

## 3. Mise en style JavaScript

Une fois le HTML fini, créez un nouveau fichier du nom `code.js`. La première partie de ce code sert à rattacher des propriétés de style aux éléments se trouvant dans le conteneur défini lors de l'étape 2. Notez que tout se lance à l'intérieur d'une fonction appliquée à un détecteur d'événement "`load`" de la fenêtre : le code échouera s'il se lance avant que la page ne soit entièrement chargée.

```js
window.addEventListener("load",function(){
  var cssRef = ".jigsawAnim > *";
  var nodes = document.querySelectorAll(cssRef);
  for(var i=0, i<nodes.length; i++){
    nodes[i].style.top = "0";
    nodes[i].setAttribute("data-speed",Math.floor(Math.random()*10)+2);
  }
  *** ETAPE 4 ICI
});
```

## 4. Interactions lors du défilement

L'effet de l'explosion se produit lorsque l'internaute fait défiler la page, donc il faut définir ces modifications visuelles en JavaScript. Cette étape applique un détecteur d'événements "`scroll`" à la fenêtre, qui mettra à jour la rotation, la position et l'opacité des éléments de l'étape 3 avec de nouveaux calculs basés sur la position de défilement de la fenêtre de la page.

```js
window.addEventListener("scroll",function(){
  var nodes = document.querySelectorAll(cssRef);
  for(var i=0; i<nodes.length; i++) {
    
    var speed = window.scrollY/parseInt(nodes[i].getAttribute("data-speed"));
    nodes[i].style.transform = "rotate("+speed+"deg)";
    nodes[i].style.top = speed+"px";
    nodes[i].style.opacity = 1-(speed/100);
    if(i >= (nodes.length-2)/2)nodes[i].style.left = speed+"px";
    else nodes[i].style.left = "-"+speed+"px";
  }
});
                        
```

## 5. Début du CSS
Une fois le code JavaScript terminé, l'étape suivante consiste à commencer la feuille de style CSS; créez un nouveau fichier du nom de `styles.css`. La première règle indique que la taille de la page fait au minimum quatre fois la hauteur de l'écran du navigateur pour garantir de bien avoir un défilement, ce qui est nécessaire afin que l'effet fonctionne.

```css
html, body {
  min-height: 400vh;
}
```

## 6. Style général du puzzle

Le puzzle se compose de deux éléments : le conteneur parent et ses enfants. Le parent a une position fixe par rapport à la fenêtre du navigateur, et non par rapport au document de la page. Les enfants utilisent un positionnement relatif, donc les modifications de leur position se font par rapport à leur parent. Le résultat est que les pièces du puzzle ne sortent pas de l'écran quand la page défile.

```css
.jigsawAnim {
  position: fixed;
  text-align: center;
}

.jigsawAnim > * {
  position: relative;
  display: inline-block;
  top: 0;
  font-size: 5em;
  padding: .5em;
}
```

## 7. Styles uniques

La dernière étape consiste à déterminer les styles uniques des enfants du conteneur **"jigsawAnim"**, auxquels on peut faire référence séparemment grâce au sélecteur `nth:child`. Cet exemple fait référence à chaque "4ème objet +1, 2, 3 ou 4" pour que les règles se répètent tous les quatre objets. Vous pouvez si vous le souhaitez donner un autre numéro aux enfants. Réfléchissez à la manière dont vous pouvez changer l'attribut `background`, par exemple en image matricielle ou en SVG.

```css
.jigsawAnim > *:nth-child(4n+1){
  background: red;
}
.jigsawAnim > *:nth-child(4n+2){
  background: green;
}
.jigsawAnim > *:nth-child(4n+3){
  background: blue;
}
.jigsawAnim > *:nth-child(4n+4){
  background: purple;
}
***
```
