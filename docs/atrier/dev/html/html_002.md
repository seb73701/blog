---
title: Ajouter une vidéo de présentation de son app
description: 
published: true
date: 2023-04-21T10:26:07.111Z
tags: app, beta, vidéo
editor: markdown
dateCreated: 2023-04-21T10:26:03.031Z
---

Inspiré par https://worldbrush.net

## 1. Faire la vidéo de démo

Pour promouvoir votre app, rien de tel que d'avoir une vidéo de présentation. Voici la structure de code à ajouter pour créer une mise en page similaire à celle de World Brush, avec le téléphone, la vidéo et le texte.

```html
<div class="container">
  <div class="phone">
    <video src="assets/screen.mp4" class="vid" loop preload="auto" muted="true" autoplay="true"></video>
  </div>
  <div class="text">
    <p>Ajoutez votre texte ici</p>
  </div>
</div>
```

## 2. Créer le CSS

La mise en page est principalement réalisée en CSS. Pour commencer, `body` occupe toute la hauteur du navigateur et nous supprimons les marges par défaut.

```css
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}
```

## 3. Conteneur centré

Nous centrons maintenant le conteneur sur la page en réglant sa hauteur et sa largeur, puis en le positionnant à 50% du haut et de la gauche. La moitié de la hauteur et de la largeur est supprimée grâce aux marges pour centrer le conteneur.

```css
.container {
  position: absolute;
  width: 880px;
  height: 780px;
  background-size: 440.7px 780px;
  left: 50%;
  top: 50%;
  margin-left: -440px;
  margin-top: -390px;
}
```

## 4. Positionner le téléphone

La section avec le téléphone se trouvera sur la gauche. Nous devons donc ajuster la largeur et la hauteur de l'image pour que le téléphone soit bien positionné sur l'écran. L'image est placée sur l'arrière-plan et a la même taille que la `div`

```css
.phone {
  width: 440px;
  height: 780px;
  background-size: 440.7px 780px;
  background-image: url(assets/device.png);
}

```

## 5. Ajouter la vidéo

La vidéo est un élément enfant du téléphone. Elle doit donc avoir exactement la bonne hauteur et la bonne largeur, et être positionnée au même endroit que l'écran du téléphone. La vidéo est en lecture automatique et boucle dans la partie HTML du code.

```css
.vid {
  position: relative;
  width: 288px;
  height: 511.2px;
  background-size: 288px 511.2px;
  left: 50%;
  top: 50%;
  margin-left: -144px;
  margin-top: -292px;
}
```

## 6. Positionner le texte

La dernière étape consiste à positionner le texte sur le côté droit de la page. Sauvegardez la page et testez-la dans le navigateur. Vous devriez voir la vidéo et le téléphone s'aligner parfaitement et afficher une démo de l'app dont vous faites la publicité.

```css
.text {
  position: absolute;
  width: 320px;
  height: 780px;
  top: 30%;
  right: 0px;
}
```

## Fichiers sources

![device.png](/assets/img/developpement/device.png)

Vidéo : [screen.mp4](/assets/img/developpement/screen.mp4)


## Code source (complet)

```html
<!DOCTYPE html>
<html lang="">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
    <style>
        body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
        
        .container {
            position: absolute;
            width: 880px;
            height: 780px;
            background-size: 440.7px 780px;
            left: 50%;
            top: 50%;
            margin-left: -440px;
            margin-top: -390px;
        }
        
        .phone {
            width: 440.7px;
            height: 780px;
            background-size: 440.7px 780px;
            background-image: url(assets/device.png);
        }
        
        .vid {
            position: relative;
            width: 288px;
            height: 511.2px;
            background-size: 288px 511.2px;
            left: 50%;
            top: 50%;
            margin-left: -144px;
            margin-top: -292.5px;
        }
        
        .text {
            position: absolute;
            width: 320px;
            height: 780px;
            top: 30%;
            right: 0px;
        }

    </style>
</head>

<body>
    <div class="container">
        <div class="phone">
            <video src="assets/screen.mp4" class="vid" loop preload="auto" muted="true" autoplay="true"></video>
        </div>
        <div class="text">
            <p>Leverage agile frameworks to provide a robust synopsis for high level overviews. Iterative approaches to corporate strategy foster collaborative thinking to further the overall value proposition. Organically grow the holistic world view of disruptive innovation via workplace diversity and empowerment.</p>

            <p>Bring to the table win-win survival strategies to ensure proactive domination. At the end of the day, going forward, a new normal that has evolved from generation X is on the runway heading towards a streamlined cloud solution. User generated content in real-time will have multiple touchpoints for offshoring.</p>
        </div>
    </div>

</body>

</html>

```