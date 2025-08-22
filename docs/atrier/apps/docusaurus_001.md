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
    