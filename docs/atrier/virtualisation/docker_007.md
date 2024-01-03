---
title: DOCKER - Prise en main et mise en pratique
description: 
published: true
date: 2023-06-08T20:33:45.441Z
tags: docker
editor: markdown
dateCreated: 2023-06-08T20:33:45.441Z
---

# DOCKER - Prise en main et mise en pratique

Vagrant permet de piloter des systèmes de virtualisation, il utilise les API de Virtualbox, VMware ou n'importe quel autre système fournissant un "provisioner".

## Installer Vagrant sous Linux

```
sudo apt update
sudo apt-get install vagrant
```

Créer un répertoire qui va contenir tout le nécessaire pour la machine virtuelle.

```
mkdir myvm
```

Se positionner dedans

```
cd myvm
```

<u>Initialiser une description Vagrant</u> 

```
vagrant init
```

Editer le fichier `Vagrantfile` avec l'éditeur `vim` :

```
vim Vagrantfile
```

Modifier le fichier généré comme ci-dessous :

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
    config.vm.provider "docker" do |d|
        d.image = "nginx"
        d.ports = ["80:80"]
    end
end
```

<u>Lancer l'instance Vagrant</u>

```
vagrant up --provider=docker
```


