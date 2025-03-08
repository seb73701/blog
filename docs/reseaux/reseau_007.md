# MEMO - CISCO - NOTIONS DE BASE SUR LES RESEAU

Préparation au module 1 de la certification CCNA 200-120

## Séquence de démarrage du périphérique

<table>
<tr>
    <td style="background-color:red;width:100px;text-align:center;">ROM</td>
    <td style="width:100px;text-align:center;">--></td>
    <td style="background-color:red;width:150px;text-align:center;">POST</td>
    <td style="width:300px;text-align:center;">Tests POST</td>
</tr>
<tr>
    <td style="background-color:red;width:100px;text-align:center;">ROM</td>
    <td style="width:100px;text-align:center;">--></td>
    <td style="background-color:red;width:150px;text-align:center;">Bootstrap</td>
    <td style="width:300px;text-align:center;">Chargement du Bootstrap</td>
</tr>
</table>

<table>
<tr>
    <td style="background-color:yellow;width:100px;text-align:center;">Flash</td>
    <td style="width:100px;text-align:center;">--></td>
    <td rowspan="2" style="background-color:yellow;width:150px;text-align:center;">IOS</td>
    <td rowspan="2" style="width:300px;text-align:center;">Recherche + Chargement ISO</td>
</tr>
<tr>
    <td style="background-color:yellow;width:100px;text-align:center;">TFTP</td>
    <td style="width:100px;text-align:center;">--></td>
</tr>
</table>

<table>
<tr>
    <td style="background-color:green;width:100px;text-align:center;">NVRAM</td>
    <td style="width:100px;text-align:center;">--></td>
    <td rowspan="3" style="background-color:green;width:150px;text-align:center;">Configuration</td>
    <td rowspan="3" style="width:300px;text-align:center;">Recherche + Chargement du fichier de configuration (ou mode de configuration)</td>
</tr>
<tr>
    <td style="background-color:green;width:100px;text-align:center;">TFTP</td>
    <td style="width:100px;text-align:center;">--></td>
</tr>
<tr>
    <td style="background-color:green;width:100px;text-align:center;">Console</td>
    <td style="width:100px;text-align:center;">--></td>
</tr>
</table>

---

## Navigation dans les modes de configuration de Cisco IOS

```shell
Router>
```

Prompt du mode utilisateur

```shell
Router>enable 
```

Bascule du mode utilisateur au mode privilégié.

```shell
Router# 
```

Prompt du mode privilégié.

```shell
Router#disable 
```

Bascule du mode privilégié au mode utilisateur

```shell
Router#configure terminal
```

Bascule en mode privilégié au mode de configuration globale.

```shell
Router(config)# 
```

Prompt du mode de configuration globale.

```shell
Router(config)#end (ou CTRL+Z) 
```

Permet de sortir du mode de configuration globale ou d'un mode de configuration spécifique et de revenir au mode privilégié.

```shell
Router(config)#line console 0 
```

Bascule du mode de configuration globale au mode de configuration de ligne spécifié (ici console).

```shell
Router(config-line)# 
```

Prompt du mode de configuration de ligne.

---

## Raccourci-clavier

|Raccourci-clavier|Description|
|-----------------|-----------|
|<kbd>CTRL</kbd> + <kbd>A</kbd>|Déplace le curseur sur le premier caractère de la commande affichée sans effacer cette dernière.|
|<kbd>CTRL</kbd> + <kbd>C</kbd>|Supprime la commande affichée pour en saisir une nouvelle.<br>Permet également de quitter le mode de configuration|
|<kbd>CTRL</kbd> + <kbd>D</kbd>|Supprimer un caractère à l'emplacement du curseur.|
|<kbd>CTRL</kbd> + <kbd>E</kbd>|Déplace le curseur sur le dernier caractère de la commande affichée sans effacer cette dernière.|
|<kbd>CTRL</kbd> + <kbd>K</kbd>|Efface tous les caractères à partir du curseur jusqu'à la fin de la ligne de commande.|
|<kbd>CTRL</kbd> + <kbd>R</kbd>|Rappelle la dernière ligne de commande interrompue par la réception d'un message ISO.|
|<kbd>CTRL</kbd> + <kbd>U</kbd> ou <kbd>CTRL</kbd> + <kbd>X</kbd>|Efface tous les caractères à partir du curseur jusqu'au début de la ligne de commande.|
|<kbd>CTRL</kbd> + <kbd>W</kbd>|Efface le mot à gauche du curseur.|
|<kbd>CTRL</kbd> + <kbd>Z</kbd>|Permet, à partir du mode de configuration globale, de revenir au code d'exécution privilégié.|
|<kbd>CTRL</kbd><kbd>&#8593; Shift</kbd> + <kbd>9 (azerty)</kbd>|Interrompt la recherche d'une erreur de saisie de commande soumise à la résolution de noms DNS ou bien un processus IOS tel `ping` ou `traceroute`.|
|<kbd>Echap</kbd> + <kbd>B</kbd>|Déplace le curseur d'un mot vers la gauche.|
|<kbd>Echap</kbd> + <kbd>F</kbd>|Déplace le curseur d'un mot vers la droite.|

---

## Configuration initiale du routeur et du commutateur

```shell
(config)#hostname name
```

Attribue un nom d'hôte au périphérique.

```shell
(config)#no hostname
```

Supprime le nom d'hôte attribué au périphérique et redéfinit le nom d'hôte par défaut.

```shell
(config)#enable secret password
```

Définit un mot de passe et active sa demande lors du passage au mode d'exécution privilégié. Le mot de passe sera stocké chiffré.

```shell
(config)#banner motd # Message #
```

Définit et active une bannière d'information ponctuelle de type MOTD _(Message Of The Day)_ lors d'un accès au périphérique avant la phase d'authentification.

```shell
(config)#no ip domain-lookup
```

Désactive la tentative de résolution de noms via le service DNS lors de la saisie erronée d'une commande.

```shell
(config)#line {console 0| vty0 15}
```

Bascule du mode de configuration globale au mode de configuration de ligne (port console ou terminaux virtuels VTY).

```shell
(config-line)#password password
```

Définit un mot de passe pour protéger l'accès au port console ou aux ports VTY.

```shell
(config-line)#login
```

Active la demande de saisie de mot de passe pour l'accès au port console ou aux ports VTY.

```shell
(config-line)#no login
```

Désactive la demande de saisie de mot de passe pour l'accès au port console ou aux ports VTY.

```shell
(config-line)#exec-timeout minutes [seconds]
```

Déconnecte automatiquement tout utilisateur en ligne en cas d'inactivité pendant le délai défini.

```shell
(config-line)#logging synchronous
```

Synchronise l'affichage des messages ISO avec la saisie des commandes.

---

## Configuration du commutateur

