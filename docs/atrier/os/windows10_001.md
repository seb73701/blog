---
title: Comment activer le client OpenSSH de Windows 10
description: Système d'Exploitation - Windows 10
published: true
date: 2023-04-21T11:17:01.632Z
tags: windows, windows_10, openssh, ssh
editor: markdown
dateCreated: 2023-04-21T11:16:57.348Z
---

# Informations

**OpenSSH** est un outil de connectivité pour les connexions distantes qui utilisent le protocole **SSH**. Il chiffre tout le trafic entre le client et le serveur pour éliminer les écoutes clandestines, le détournement de connexion et d’autres attaques.

**OpenSSH (client)** peut être utilisé pour connecter les appareils Windows 10 (*build 1809 et ultérieur*) à **OpenSSH (Server)** qui peut être installé sur un serveur (physique ou virtuel) et fonctionnant sous un environnement Linux ou Windows.


# Installer le client OpenSSH

## Installer le client OpenSSH avec l'interface graphique

- Ouvrir la fenêtre "**Paramètres"** de Windows 10 avec le raccourcis "`[ Win + I ]`"
- Cliquer sur **Applications**.

![win10_parametres_applications.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications.png)

- Cliquer sur l'onglet "**Applications et fonctionnalités**"

![win10_parametres_applications_ajout_fonct.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct.png)

- Cliquer sur le lien "**Gérer les fonctionnalités facultatives**".

![win10_parametres_applications_ajout_fonct_facultatives.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct_facultatives.png)

- Vérifier que l'agent n'est pas déjà installé.

![win10_parametres_applications_ajout_fonct_facultatives_05.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct_facultatives_05.png)

- Cliquer sur le bouton "**Ajouter une fonctionnalité**". 

![win10_parametres_applications_ajout_fonct_facultatives_02.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct_facultatives_02.png)

- Choisir l'option "**Client OpenSSH**"

![win10_parametres_applications_ajout_fonct_facultatives_03.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct_facultatives_03.png)

- Cliquer sur le bouton "**Installer**".

> Il n'est pas nécessaire de redémarrer la machine pour pouvoir utiliser le client.
{.is-info}

- Ouvrir une invite de commandes en **mode Administrateur**

- Taper la commande `ssh` pour vérifier que la fonctionnalité est bien installée.

![win10_parametres_applications_ajout_fonct_facultatives_04.png](/assets/img/systeme_exploitation/win10/win10_parametres_applications_ajout_fonct_facultatives_04.png)

## Installer le client OpenSSH avec PowerShell


> Pour installer le client OpenSSH avec PowerShell, il faut exécuter PowerShell **en tant qu’administrateur**. 
{.is-info}


- Exécuter l’applet de commande suivante pour s'assurer qu’OpenSSH est disponible :

```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

La sortie suivante doit être retournée si aucun n’est déjà installé :

```powershell
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent

# ou si un élément est installé :

Name  : OpenSSH.Client~~~~0.0.1.0
State : Installed

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent

```

- Installer le composant client :

```powershell
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

La sortie suivante devrait être retournée :

```powershell
Path          :
Online        : True
RestartNeeded : False
```

# Se connecter à OpenSSH Server

> Exécuter PowerShell **en tant qu’administrateur**.
{.is-info}

Pour se connecter à une VM (port 22) :

```shell
ssh username@servername
```

Pour se connecter à une VM VirtualBox avec une redirection de port (port 62022) :

```shell
ssh -p 62022 root@127.0.0.1
```

Une fois connecté, un message semblable à celui-ci apparaît :


```shell
The authenticity of host '[127.0.0.1]:62022 ([127.0.0.1]:62022)' can't be established.
ECDSA key fingerprint is SHA256:iTmZPZAaVXfMob+W7RvyeEQ1rPWVVqg/dRAOiq/mmkw.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Si on sélectionne **yes**, le serveur est ajouté à la liste des hôtes SSH connus sur le client Windows.

Alors il faut entrer le mot de passe. Par mesure de sécurité, le mot de passe n’est pas affiché lors de la saisie.

```shell
The authenticity of host '[127.0.0.1]:62022 ([127.0.0.1]:62022)' can't be established.
ECDSA key fingerprint is SHA256:iTmZPZAaVXfMob+W7RvyeEQ1rPWVVqg/dRAOiq/mmkw.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[127.0.0.1]:62022' (ECDSA) to the list of known hosts.
root@127.0.0.1's password:
```

Une fois connecté, vous voyez l’invite de l’interpréteur de commandes Windows :

```shell  
Last login: Tue Oct 19 16:43:57 2021 from 10.0.2.2
[root@localhost ~]#
```


# Sources

- [Installer OpenSSH (Microsoft.com)](https://docs.microsoft.com/fr-fr/windows-server/administration/openssh/openssh_install_firstuse)