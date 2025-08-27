---
title: Installation d'un blog Docusauraus
description: 
published: true
date: 2025-08-22
tags: wiki,blog,docusaurus
editor: markdown
---

## Prérequis

### Installation d'une VM via WSL

- Création de la VM

```shell
wsl --import docusaurus E:\wslworkspace\docusaurus E:\wslworkspace\vmtest.tar
```

- Fermer la fenêtre du terminal
- Ouvrir le Terminal Windows
- Sélection dans le menu la VM
- Mettre à jour la VM

```shell
apt update
apt upgrade -y
```

- Renommer la VM

```shell
vim /etc/hosts

hostname -F /etc/hosts
```

- Fermer le terminal
- Ouvrir VSCode
- Dans le menu de gauche, cliquer sur "Explorateur distant".
- Dans la liste qui apparaît, cliquer avec le bouton droit sur "docusaurus"
- Sélectionner l'option : "Se connecter dans la fenêtre actuelle"
- Patienter quelques secondes
- Cliquer sur "Explorateur", puis ouvrir le dossier "/root/"
- Dans le terminal de VSCode, installer Node.js (voir ci-dessous)

### Installation de Node.js

```shell
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"

# Download and install Node.js:
nvm install 22

# Verify the Node.js version:
node -v # Should print "v22.18.0".
nvm current # Should print "v22.18.0".

# Verify npm version:
npm -v # Should print "10.9.3".

# Upgrade npm
npm install -g npm@11.5.2

removed 9 packages, and changed 42 packages in 2s

25 packages are looking for funding
  run `npm fund` for details
```

```shell
npm fund
root
```

Source : https://nodejs.org/en/download/

## Installation de docusaurus

```shell
root@docusaurus:~# npx create-docusaurus@latest doc-it classic
✔ Which language do you want to use? › JavaScript
[INFO] Creating new Docusaurus project...
[INFO] Installing dependencies with npm...
[SUCCESS] Created doc-it.
[INFO] Inside that directory, you can run several commands:

  `npm start`
    Starts the development server.

  `npm run build`
    Bundles your website into static files for production.

  `npm run serve`
    Serves the built website locally.

  `npm run deploy`
    Publishes the website to GitHub pages.

We recommend that you begin by typing:

  `cd doc-it`
  `npm start`

Happy building awesome websites!

root@docusaurus:~# 
```

- On se positionne dans le dossier : 

```shell
 cd doc-it/
```

- Démarrer le serveur en local

```shell
npm start

> doc-it@0.0.0 start
> docusaurus start

[INFO] Starting the development server...
[SUCCESS] Docusaurus website is running at: http://localhost:3000/

✔ Client
  Compiled successfully in 10.23s

client (webpack 5.101.3) compiled successfully
```

----

### Moteur de recherche

```shell
npm run swizzle @docusaurus/theme-classic SearchBar
```

```shell
root@docusaurus:~/doc-it# npm run swizzle @docusaurus/theme-classic SearchBar

> doc-it@0.0.0 swizzle
> docusaurus swizzle @docusaurus/theme-classic SearchBar

✔ Which language do you want to use? › JavaScript
✔ Which swizzle action do you want to do? › Wrap (Safe)
[SUCCESS] 
Created wrapper of SearchBar from @docusaurus/theme-classic in 
- "/root/doc-it/src/theme/SearchBar.js"
```

----

## PLUGINS

- https://github.com/flexanalytics/plugin-image-zoom
- https://github.com/praveenn77/docusaurus-lunr-search
- https://medium.com/@techwritershub/set-up-and-customize-a-documentation-site-with-docusaurus-ca7e00a398d3
- https://www.builtatlightspeed.com/category/docusaurus
- https://github.com/webbertakken/awesome-docusaurus
- https://docusaurus.io/fr/docs/search#using-local-search
- https://github.com/webbertakken/awesome-docusaurus#readme

## FONT AWESOME

Source : https://docs.fontawesome.com/web/use-with/react

Installation des packages :

```shell
npm i --save @fortawesome/react-fontawesome@latest
npm i --save @fortawesome/fontawesome-svg-core
npm i --save @fortawesome/free-solid-svg-icons
npm i --save @fortawesome/free-regular-svg-icons
npm i --save @fortawesome/free-brands-svg-icons
```

Code à intégrer sur la page : `doc-it/src/components/HomepageFeatures/index.js`

```javascript
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
import { library } from '@fortawesome/fontawesome-svg-core'

/* import all the icons in Free Solid, Free Regular, and Brands styles */
import { fas } from '@fortawesome/free-solid-svg-icons'
import { far } from '@fortawesome/free-regular-svg-icons'
import { fab } from '@fortawesome/free-brands-svg-icons'

library.add(fas, far, fab)
```

Pour utiliser des icônes, il faut mettre : 

```javascript
      <div>
        <FontAwesomeIcon icon="fa-solid fa-dog" />
        <FontAwesomeIcon icon="fa-regular fa-circle-user" />
        <FontAwesomeIcon icon="fa-brands fa-threads" />
      </div>
```

Pour la partie customisation, voir ce lien : https://docs.fontawesome.com/web/use-with/react/style

Pour mettre de la couleur, il faut utiliser du code React :

```javascript
<FontAwesomeIcon icon="fa-solid fa-circle" style={{ color: "red" }} />
```

ou si l'on souhaite utiliser une feuille de style personnalisée : 

```javascript
<FontAwesomeIcon icon="fa-solid fa-dog" className={styles.red} />
```

Mais il faut mettre dans le CSS : 

```css
.red {
  color:red;
}
```
