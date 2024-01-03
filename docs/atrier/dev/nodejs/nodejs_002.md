---
title: Downgrade Package Node.js
description: 
published: true
date: 2023-06-11T10:36:20.187Z
tags: 
editor: markdown
dateCreated: 2023-06-11T10:36:20.187Z
---

# Downgrade Package Node.js


## 1. Uninstall current version of the NodeJS from Ubuntu
If you wish to uninstall NodeJS from your Ubuntu system, run the command below.

```
sudo apt-get remove nodejs
```

The command will remove the package but retain the configuration files. To remove both the package and the configuration files run:

```
sudo apt-get purge nodejs
```

As a final step, you can run the command below to remove any unused files and free up the disk space

```
sudo apt-get autoremove
```

Great! We have successfully installed and tested the installation of NodeJS. We also learned how to uninstall NodeJS from Ubuntu and clean up space.

##  Install NodeJS v16.x from Ubuntu

```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

## Block upgrade NodeJS

```
# apt-mark hold nodejs
nodejs passé en figé (« hold »).
```


Sources :

- https://www.digitalocean.com/community/tutorials/install-uninstall-nodejs-ubuntu
- https://github.com/nodesource/distributions
- https://fr-wiki.ikoula.com/fr/Bloquer_la_mise_%C3%A0_jour_d%27un_paquet_avec_APT