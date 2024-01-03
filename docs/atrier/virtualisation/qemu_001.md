---
title: Contrôle automatisé de machines virtuelles par QEMU Monitor
description: 
published: true
date: 2023-04-21T10:45:28.275Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:45:24.574Z
---

# Contrôle automatisé de machines virtuelles par QEMU Monitor

> Le projet QEMU, qui offre une approche légère et peu intrusive pour l’émulation et la virtualisation, présente l’avantage d’être entièrement configurable en ligne de commandes. Nous verrons dans ces pages comment scripter les modifications de certaines caractéristiques d’une VM pendant son exécution en arrière-plan.

Les techniques de conteneurisation ne permettant pas de couvrir tous types de besoins, il est parfois nécessaire d’avoir recours à des machines virtuelles sur un poste de travail ou d’administration. Par exemple, les développements ou manipulations sur le noyau sont impossibles dans un conteneur.

En matière de virtualisation, l’éventail des outils est vaste, et les avantages et faiblesses de chacun ont été discutés au fil des numéros de ce magazine. Sans débattre sur le choix de QEMU, nous soulignerons ici le fait que cet outil est autonome : chaque machine virtuelle prend la forme d’un unique processus sur l’hôte, qui ne nécessite pas de service central de contrôle des guests.

C’est cet aspect qui retiendra notre attention dans ces pages, où nous nous proposons un but simple : le contrôle d’une machine virtuelle qui a été lancée en arrière-plan, sans interface graphique. Il s’agira en particulier de modifier les redirections de ports par le biais du QEMU Monitor de la machine guest.

Après une présentation du Monitor et de l’utilisation de sa console, nous montrerons comment le piloter de manière non interactive.

## 1. Rappels sur QEMU
QEMU [1] est un outil d’émulation et de virtualisation.

L’émulation dite en User Mode permet l’exécution de binaires compilés pour des architectures différentes ; notamment, les développeurs pour la plateforme Android l’utilisent au quotidien en démarrant leur app dans le device virtuel.

Nous n’allons pas nous intéresser à l’émulation dans cet article, mais pour simplifier, il suffit souvent d’exécuter ce type de commande :

```
$ qemu-<autre-archi> ./binaire-pour-autre-archi
```

pour lancer en émulation le programme spécifié.

La virtualisation quant à elle, permet comme on le sait de démarrer un système complet, isolé dans une machine virtuelle aux caractéristiques matérielles apparentes choisies.

Le programme à lancer pour démarrer une machine virtuelle sur architecture x86_64 sera de la forme qemu-system-x86_64.

## 2. Une première machine
Survolons rapidement la mise en œuvre d’une machine Debian dont nous supposons téléchargée l’image ISO d’installation. Le but ne sera pas d’étudier dans le détail les possibilités, très vastes, de paramétrage de l’outil QEMU, mais simplement d’entrer en matière.

### 2.1 Préparation
Commençons par créer un disque dur virtuel : c’est l’outil qemu-img [2] qui s’en charge.

```
$ qemu-img create -f qcow2 -o preallocation=off disk1.qcow2 8G
```

Le fichier représentant le disque dur aura le format qcow2, un classique pour QEMU, qui permet notamment la manipulation d’instantanés (voir encadré). Ce format permet aussi de gérer l’élasticité du disque virtuel, avec l’option -o preallocation=off, pour éviter de mobiliser trop tôt sur l’hôte l’intégralité de la capacité de stockage souhaitée (ici 8 Go).

```
$ ls -lh disk1.qcow2
-rw-r--r-- 1 gabriel team 192K Oct 3 16:54 disk1.qcow2
```

### 2.2 Le mode graphique
Lançons une machine avec le minimum de précisions (mémoire, disque dur principal, et CD-ROM) : les réglages par défaut pour la carte réseau virtuelle sont assez bons pour que cette VM ait accès à l’Internet via le réseau de l’hôte.

```
$ qemu-system-x86_64 \
    -m 4G \
    -hda disk1.qcow2 \
    -cdrom debian-11.0.0-amd64-netinst.iso
```

S’ouvre alors une fenêtre dans notre environnement graphique (voir figure 1) qui représente l’écran de la machine virtuelle, dans lequel nous pouvons dérouler l’assistant d’installation du système.

Avec cette commande, le processus reste bloquant pour le terminal, qui ne reprendra la main qu’à l’extinction de la VM.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-129/figure_01.png">

Fig. 1 : Lancement d’une VM sur CD-ROM d’installation de Debian 11 en mode graphique.

Nous prendrons soin de cocher l’installation du service SSH.

Nous supposerons dans la suite que le nom d’hôte de cette VM est vmdebian, avec un utilisateur vm-user. Une fois l’installation terminée, l’interface fenêtrée nous permet d’accepter le redémarrage.

## 3. Le Monitor
En cours d’exécution fenêtrée d’une machine virtuelle, il est possible d’activer un mode spécial de QEMU, le Monitor [3], en tapant les touches <kbd>[Ctrl]+[Alt]+[2]</kbd> (voir figure 2). Il permet de contrôler la VM et d’effectuer des actions d’administration, comme nous allons le voir.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-129/figure_02.png">

Fig. 2 : Bascule manuelle sur la console du Monitor.

La fenêtre se transforme en une sorte de console qui accepte des commandes pour obtenir des informations sur la machine, et pour effectuer certaines actions et modifications. On peut revenir sur l’écran de la VM par [Ctrl]+[Alt]+[1].

### 3.1 Snapshots
Une première action qui vient à l’esprit est la capture d’un instantané de la machine : il s’agit de sauvegarder le contenu intégral de la RAM et de fixer l’état actuel du disque dur au complet, afin de pouvoir revenir ultérieurement à cet état.

> Les instantanés avec le format qcow2
> Le format qcow2, pour Quick Copy On Write, est le seul qui offre la fonctionnalité des instantanés pour QEMU. Agnostique quant au file system, il opère une gestion transparente de liens au niveau des blocs de stockage, en sorte que les instantanés ne dupliquent en fait pas les données. Pour les blocs qui évolueront par la suite dans le cycle de vie de la machine, le lien est simplement détaché : le snapshot conserve l’original, le disque « vivant » poursuit avec une copie créée opportunément.
> 
> L’ensemble (mémoire, disque, snapshots successifs et disque vivant) est empaqueté dans un seul et même fichier, celui créé initialement pour l’image disque de la machine.
{.is-info}


Nous allons capturer un instantané dans la console Qemu Monitor, par la directive savevm :

```
QEMU 6.1.0 monitor - type 'help' for more information
(qemu) savevm snap1
(qemu)
```

Dès lors, dans un autre terminal, on peut inspecter le disque virtuel comme ceci :

```
$ qemu-img snapshot -l disk1.qcow2
Snapshot list:
ID        TAG               VM SIZE                DATE     VM CLOCK     ICOUNT
1         snap1                 0 B 2021-10-03 18:25:01 00:00:00.000          0
```

On constate que le snapshot en lui-même n’occupe pas de place additionnelle, car à ce stade aucun de ses blocs n’a encore fait l’objet d’une recopie dans la poursuite du cycle de vie de la machine.

Un snapshot peut se recharger à chaud dans le moniteur par l’instruction loadvm, et à froid en lançant à nouveau, plus tard, le programme qemu-system-x86_64 avec l’argument -loadvm snap1.

Mais ne nous attardons pas sur ce point qui n’est pas au centre de notre préoccupation ici.

### 3.2 Extinction
Une VM peut s’éteindre de quatre façons différentes :

1. de l’intérieur :

```
vm-user@vmdebian $ sudo shutdown -P now
```

2. par simulation de l’appui sur le bouton Power de l’appareil virtuel, en tapant dans le moniteur :

```
(qemu) system_powerdown
```

ce qui laisse le système guest prendre en charge, le cas échéant, l’action à effectuer (boîte de logout, etc.).

3. ou encore, en fermant la fenêtre QEMU à la souris, ce qui a pour effet de tirer la prise électrique…

4. … également équivalent à faire un [Ctrl]+[C] sur le processus qemu-system-x86_64.

Mais pour l’heure, laissons notre machine allumée.

### 3.3 Interruption
Après avoir pris un instantané (voir 3.1), bien souvent on souhaitera interrompre la machine, sans l’éteindre. Ce serait l’équivalent de la mise en hibernation sur certains systèmes.

Pour cela, dans le Monitor, on tape :

```
(qemu) q
```

(ou encore le mot quit en entier).

Conséquence : fermeture du hublot, fin du processus qemu-system-x86_64. Notons que c’est également équivalent aux façons 2 et 3 du paragraphe 3.2 précédent.

On relancera typiquement la machine avec l’option -loadvm `<snapshotTag>`, pour qu’elle reprenne exactement là où elle en était (RAM et disque) au moment du savevm.

### 3.4 Port forwarding
Supposons que nous ayons besoin de permettre les connexions SSH depuis l’extérieur vers cette machine virtuelle, via un port de l’hôte, par exemple 10022.

Avec les paramètres précédemment utilisés sur la ligne de commandes invoquée en 2.2, cela ne suffit pas. Deux méthodes s’offrent à nous : soit stopper (ou hiberner) la machine pour relancer la commande avec plus d’arguments ; soit utiliser le Monitor à chaud.

Pour la relance, il faudrait compléter la ligne de commandes avec ceci :

```
  -device e1000,netdev=net0 \
  -netdev user,id=net0,hostfwd=tcp::10022-:22
```
  
Les explications détaillées pour les options de réseau sont dans le manuel [4]. Il s’agit simplement ici de préciser la règle hostfwd pour la redirection (voir encadré), or nous sommes obligés de l’incorporer dans la déclaration explicite d’une carte réseau. À part cette règle hostfwd, les valeurs précisées ici sont celles qui sont prises par défaut si les options -device et -netdev sont omises.

> La règle Forward
> La syntaxe de la règle hostfwd est la suivante :
> 
> `[tcp|udp]:[hostaddr]:hostport-[guestaddr]:guestport`
> On peut omettre l’adresse locale de l’hôte et l’adresse virtuelle du guest, ce qui donne, pour rediriger le TCP depuis le port 10022 de l’hôte vers le port 22 de la VM :
> 
> `tcp::10022-:22`
> Sur la ligne de commandes, plusieurs règles hostfwd peuvent être listées, en les séparant par des virgules.
{.is-info}


L’autre possibilité, on l’aura deviné, est d’utiliser le Monitor pour ajouter la règle hostfwd à chaud pendant l’exécution de la machine :

```
QEMU 6.1.0 monitor - type 'help' for more information
(qemu) hostfwd_add tcp::10022-:22
(qemu)
```

ce qui permet aussitôt de vérifier que le port est bien redirigé :

```
$ ssh -p10022 vm-user@localhost
The authenticity of host '[localhost]:10022 ([127.0.0.1]:10022)' can't be established.
ECDSA key fingerprint is SHA256:I5rKPLu+4xd+fMmdicAUBmRyi/yDOjMkbVaPFKGfV2c.
Are you sure you want to continue connecting (yes/no)?
```

### 3.5 Et autres
Le moniteur accepte un grand nombre d’autres directives [3], qui ne font pas l’objet de cet article. On retiendra par exemple screendump pour enregistrer une capture d’écran de la VM, ou encore xp pour obtenir un dump de la mémoire, et jusqu’à sendkey et même mouse_move pour faire ce que leurs noms indiquent.

## 4. Headless
L’aptitude de QEMU à jouer la machine virtuelle dans une fenêtre graphique est intéressante si on a effectivement besoin de manipuler son environnement desktop. Mais dans le cas d’un serveur, c’est superflu.

C’est le cas de notre nouvelle machine. Nous allons la lancer sans hublot.

### 4.1 Lancement
Nous reprenons la même commande, en rechargeant l’instantané snap1, mais cette fois avec l’option -display none :

```
$ qemu-system-x86_64 \
    -m 4G \
    -hda disk1.qcow2 \
    -display none \
    -loadvm snap1 \
    -device e1000,netdev=net0 \
    -netdev user,id=net0,hostfwd=tcp::10022-:22 
```

Nous n’avons plus besoin d’insérer le -cdrom puisque le système est désormais installé. En revanche, il est indispensable de passer le hostfwd, car la connexion SSH devient, maintenant, le seul moyen d’entrer en contact avec la machine.

Le terminal reste en attente sur cette commande bloquante, mais nous pouvons vérifier dans un autre terminal, presque immédiatement, que le service SSH de la VM est bien up.

```
$ ssh -p10022 vm-user@localhost
vm-user@localhost's password:
Linux debian 5.10.0-8-amd64 #1 SMP Debian 5.10.46-5 (2021-09-23) x86_64
 
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.
 
Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Oct 5 13:54:59 2021 from 10.0.2.2
vm-user@vmdebian $
```

Problème : comment faire, maintenant qu’on n’a plus de lucarne, pour basculer sur le Monitor et demander un savevm, ou un hostfwd_add pour ouvrir d’autres ports ?

### 4.2 Un Socket pour le Monitor
Le mode visuel pour le moniteur est celui par défaut, mais ce n’est pas le seul. Puisqu’il s’agit d’une sorte de console, on peut demander à QEMU de la rediriger. Parmi les possibilités (port série et autres déviations d’affichage) celle qui nous intéresse ici est le socket Unix.

#### 4.2.1 Serveur
On ajoutera cette option à la commande de lancement :

```
-monitor unix:/chemin/fichier.sock,server,nowait
```

Ce faisant, nous passons toutes les entrées-sorties du Monitor dans le socket nommé ici /chemin/fichier.sock (le nom du pseudo-fichier est de votre cru).

Il faut obligatoirement préciser les attributs server et nowait. Le premier signifie que QEMU sera le patron du socket et que des clients viendront s’y connecter (sans quoi QEMU essaierait de se connecter en tant que client à un socket supposé préexistant). Le second indique à QEMU de se lancer tout de suite, indifféremment de la présence ou non d’un client pour notre Moniteur, autrement il attendrait une première connexion pour démarrer la VM.

#### 4.2.2 Client interactif
Il ne reste plus qu’à entrer en communication avec le Monitor via le socket. Le plus simple pour obtenir l’équivalent d’un terminal interactif, est d’utiliser le programme socat [5], le fameux relai multi-usage.

Il s’installe classiquement par les gestionnaires de paquets (exemple : apt install socat), et nous l’utilisons comme ceci :

```
$ socat - UNIX-CONNECT:/chemin/fichier.sock
```

où le tiret - stipule de diriger nos stdin et stdout vers le socket Unix spécifié.

Une fois le contact établi avec le Monitor, on s’en sert normalement, nativement dans le terminal. Par exemple :

```
$ socat - UNIX-CONNECT:/chemin/fichier.sock
(qemu) hostfwd_add tcp::10080-:80
(qemu)
```

Noter : taper <kbd>[Ctrl]+[D]</kbd> pour terminer la communication et sortir du programme socat (rappel, quit termine QEMU).

### 4.3 Arrière-plan
Mobiliser un terminal sur l’hôte pendant toute l’exécution de la commande `qemu-system-<arch>`, ce n’est pas l’idéal pour une machine virtuelle qui est supposée être un serveur.

Pour lancer une commande de façon non bloquante, il suffit de préciser l’option -daemonize. Le programme rend alors aussitôt la main, la machine virtuelle tourne en arrière-plan. D’ailleurs l’option -daemonize nécessite le mode -display none ; la commande serait rejetée autrement.

Et puisque nous avons branché son Monitor sur le socket, nous pouvons donc continuer de contrôler QEMU pour cette instance.

## 5. Client scriptable
Nous avons donc vu comment démarrer en arrière-plan, et sans hublot, une machine virtuelle avec ou sans réveil d’hibernation :

```
$ qemu-system-x86_64 \
    -daemonize \
    -monitor unix:/chemin/fichier.sock,server,nowait
    -m 4G \
    -hda disk1.qcow2 \
    -display none \
    -device e1000,netdev=net0 \
    -netdev user,id=net0,hostfwd=tcp::10022-:22 \
    -loadvm snap1
```
  
Cette dernière option -loadvm sera omise pour un démarrage à froid.

L’outil socat nous a permis d’obtenir un terminal interactif sur le Monitor. Voyons désormais comment lui envoyer des instructions depuis un script, c’est-à-dire de façon non interactive.

Une approche naïve, en versant un echo dans notre commande socat, ne fonctionnera pas.

```
$ echo 'savevm snap2' | socat - UNIX-CONNECT:/chemin/fichier.sock
(qemu) s%
$
```

Seule la première lettre de la chaîne semble transmise, et la connexion se coupe.

Essayons avec l’outil Netcat (nc) [6], beaucoup moins polyvalent que socat, car nous n’avons pas besoin ici d’un relai complet en aller-retour interactif :

```
$ echo 'savevm snap2' | nc -U /chemin/fichier.sock
(qemu) s%
$
```

Même résultat.

Pour y arriver, il semble être nécessaire de maintenir ouverte la connexion avec le moniteur un court instant, comme ceci :

```
$ cat <(echo "savevm snap2" ; sleep 1) \
  | nc -U /chemin/fichier.sock
```

Cette commande utilise la technique dite de process substitution [7]. Elle permet ici d’alimenter cat avec ce qui est vu comme un fichier <( ), mais qui est en réalité la sortie d’un sous-processus du shell, en l’occurrence l’enchaînement echo puis sleep.

Ainsi, la connexion au socket par nc s’ouvre dès le début en raison du tube |, puis cat envoie d’abord la « première ligne du fichier », produite par echo, puis une attente est forcée avant que le tube ne soit libéré.

La conséquence est que le Monitor a le temps d’accuser réception d’une ligne d’instruction. La durée de l’attente n’a pas d’importance, même si la capture de l’instantané prend plus de temps.

## Conclusion
Cette technique peut s’employer dans des scripts et alias pour envoyer à vos VM les instructions de sauvegarde, rechargement, ajout et suppression de redirections de ports, et toute la longue liste des autres directives du Monitor.

L’exploitation de machines virtuelles invisibles s’intègre alors très bien dans des tâches administratives diverses, ou de test, ou de CI, de façon automatisable et prédictible.

## Références
[1] Le projet QEMU : https://qemu.org/
[2] Les paramètres de l’outil qemu-img : https://linux.die.net/man/1/qemu-img
[3] QEMU Monitor : https://qemu-project.gitlab.io/qemu/system/monitor.html
[4] Configuration du réseau : https://qemu-project.gitlab.io/qemu/system/invocation.html#hxtool-5
[5] Socat, premiers pas : https://www.redhat.com/sysadmin/getting-started-socat
[6] Manuel Netcat : https://linux.die.net/man/1/nc
[7] Process Substitution : https://tldp.org/LDP/abs/html/process-sub.html


source : https://connect.ed-diamond.com/linux-pratique/lp-129/controle-automatise-de-machines-virtuelles-par-qemu-monitor