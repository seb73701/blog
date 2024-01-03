---
title: Toute votre infrastructure Debian en YAML avec DebOps
description: 
published: true
date: 2023-04-21T10:34:51.110Z
tags: ansible, devops, debian, debops
editor: markdown
dateCreated: 2023-04-21T10:34:47.097Z
---

# Toute votre infrastructure Debian en YAML avec DebOps

> Administrer toutes les briques de son infrastructure Debian à l’aide d’un outil de gestion de configuration est une tâche complexe. Outre la quantité de code nécessaire, il faut représenter les interactions entre les logiciels de l’infrastructure.
{.is-info}

Projet communautaire basé sur Ansible, DebOps répond à cette problématique avec plus de 160 rôles liés les uns aux autres. De plus, les utilitaires fournis par le projet permettent de simplifier l’utilisation des rôles, ainsi que le respect des bonnes pratiques Ansible. Ceci en fait un outil unique pour automatiser vos infrastructures.

Le projet DebOps est né en octobre 2013 sous l'impulsion de Maciej Delmanowski (@drybjed), administrateur système polonais et actuel mainteneur. Ce projet s'articule autour d'un unique dépôt Git [1], appelé monorepo et composé :

- de rôles Ansible qui agissent ensemble (164 dans la version stable 1.1.1 utilisée dans cet article) ;
- de playbooks Ansible ;
- de programmes Python (debops*) destinés à faciliter l'utilisation des rôles et playbooks ;
- d’une documentation exhaustive [2].

Le tout distribué sous licence GPL 3.0.

> L’ensemble des fichiers utilisés dans cet article sont téléchargeables depuis le dépôt Git : https://github.com/nqb/glmf-debops/
{.is-info}


> **Notions Ansible : playbook, inventaire et rôle**
> Un playbook est un fichier YAML composé de plays. Un play est une liste de tâches Ansible qui s’exécute sur des hôtes déclarés dans l’inventaire.
> 
> L’inventaire, dans sa version statique, est un fichier ou dossier qui répertorie la liste des hôtes à administrer avec Ansible, la manière de s’y connecter et leurs groupes d’appartenance. Le sous-dossier group_vars regroupe les variables propres aux groupes, alors que le sous-dossier host_vars contient les variables propres à chaque hôte. Les fichiers d’inventaire sont au format INI ou YAML.
> 
> Un rôle est un play décomposé en sous-répertoires, chacun ayant une fonction précise. Un rôle doit être appelé depuis un play, car il ne contient pas de directive indiquant sur quelles machines il doit s’exécuter. Tout rôle dispose d’un comportement par défaut, si aucune de ses variables n’est surchargée.
{.is-info}


Cet ensemble est utilisé pour automatiser l’installation, la configuration et la mise à jour d’infrastructures basées sur la distribution Debian ou ses dérivées (Ubuntu, Devuan et Raspbian) à l'aide d'Ansible et de son inventaire. Au moment de l'écriture, la version stable 1.1.1 supporte Debian Buster, dernière distribution stable publiée par le projet Debian.

À titre d'exemples, voici une liste de déploiements qui peuvent être réalisés avec cet outil en configurant uniquement un inventaire Ansible :

- site statique servi par NGinX et sécurisé avec Let's Encrypt ;
- logiciel d'inventaire réseau (IPAM) et datacenter (DCIM) avec NetBox ;
- infrastructure de déploiement d'images Debian en utilisant dnsmasq, iPXE et un fichier de réponses ;
- forge GitLab et ses « runners ».

Le cas d’usage que nous allons détailler dans cet article consiste à mettre en place un proxy cache APT avec Apt-Cacher NG pour accélérer l'installation de paquets Debian. Voici l’architecture cible :

```

   Machine "master"                          Machine "node1"
 
+--------------------+                   +--------------------+
|                    |                   |                    |
| Contrôleur Ansible |   Réseau privé    | Nœud               |
|                    |                   |                    |
| Proxy cache APT    +-------------------+                    |
|                    |                   |                    |
| Client APT         |                   | Client APT         |
|                    |                   |                    |
+--------------------+                   +--------------------+
 
debops.vagrant.test                    debops-node1.vagrant.test
```

Les deux machines seront administrées à l’aide de DebOps. Le programme APT sera configuré sur les deux machines afin qu’elles utilisent le proxy cache installé sur le contrôleur. Toutes ces actions vont être réalisées dans un environnement reproductible grâce au logiciel Vagrant.

## 1. Environnement utilisé pour cet article
### 1.1 Prérequis : Vagrant et Virtualbox
Pour démarrer avec DebOps, nous allons utiliser les logiciels Vagrant et Virtualbox comme indiqué dans la documentation du projet [3]. L'avantage de cette méthode est de disposer en quelques minutes d'un laboratoire de test préconfiguré avec DebOps, complètement reproductible. L'inconvénient, c'est que l'installation de DebOps ne sera pas détaillée ici, je vous renvoie donc à la documentation officielle sur ce sujet [4].

Étant donné que Vagrant [5] et Virtualbox [6] peuvent être installés sur GNU/Linux, Windows ou macOS, l'installation de ces logiciels n'est pas présentée dans cet article. Nous vous invitons à suivre les liens donnés en références avant de poursuivre. Les versions 2.2.5 et 5.2.32 de Vagrant et Virtualbox ont été utilisées pour cet article.

### 1.2 Démarrage des machines virtuelles
Une fois ces logiciels installés, nous allons récupérer le code source du projet et utiliser le fichier Vagrantfile qui se trouve à sa racine pour démarrer deux machines virtuelles. Voici les commandes à lancer :

```
$ wget https://github.com/debops/debops/archive/v1.1.1.tar.gz
$ tar x -zf v1.1.1.tar.gz ; cd debops-1.1.1/
$ ANSIBLE_FROM=pypi VAGRANT_HOSTNAME=debops VAGRANT_NODES=1 vagrant up
```

Une fois que Vagrant a terminé son travail, nous disposons de deux machines virtuelles : master et node1 (appelées respectivement le contrôleur Ansible et le nœud dans la suite de l'article) préconfigurées de la façon suivante :

- les machines fonctionnent sous Debian Buster ;
- le domaine DNS est vagrant.test ;
- chaque machine connaît l’IP de l’autre grâce à une entrée dans son fichier /etc/hosts ;
- l'utilisateur vagrant dispose de tous les droits sur les machines via sudo ;
- la connexion SSH entre le contrôleur Ansible et le nœud est sécurisée par clés et ne nécessite pas de mot de passe ;
- ansible est installé depuis le dépôt de paquets PyPI sur le contrôleur Ansible dans sa dernière version stable ;
- le programme debops est installé depuis les sources sur le contrôleur Ansible ;
- un projet DebOps d'exemple est créé dans le dossier ~/src/controller sur le contrôleur Ansible ;

Dans la suite de l'article, l'ensemble des commandes seront exécutées depuis le contrôleur Ansible. Pour vous y connecter, vous devez utiliser la commande : vagrant ssh master dans le répertoire où se trouve le fichier Vagrantfile.

Toutes les conditions sont maintenant réunies pour administrer nos machines via Ansible et DebOps en créant notre premier projet.

## 2. Premier projet DebOps
### 2.1 Commandes DebOps
Le programme debops, préinstallé sur le contrôleur Ansible, fournit une liste de commandes préfixées par debops destinées à simplifier l’organisation et l'utilisation du code Ansible. Voici un tableau qui définit leur usage :

|Commande|Usage|
|:------:|:---:|
|`debops`|équivalent de la commande `ansible-playbook` adaptée au projet DebOps|
|`debops-task`|équivalent de la commande `ansible`|
|`debops-init`|création d'un projet avec une arborescence de base|
|`debops-defaults`|affichage des paramètres par défaut d'un rôle|
|`debops-update`|utilisation de la version de développement|
|`debops-padlock`|chiffrement des secrets contenus dans un projet|

Voyons comment les utiliser pour créer un premier projet DebOps.

### 2.2 Création d'un premier projet : debops-init
Les bonnes pratiques du projet Ansible [7] recommandent d'organiser les fichiers utilisés par ansible selon une arborescence spécifique. DebOps suit ces recommandations en structurant ces fichiers dans des répertoires indépendants appelés projets. Ces répertoires sont ensuite destinés à être suivis en version à l'aide de Git afin de disposer d'un historique des changements. La commande debops-init permet de créer un projet avec une arborescence de base.

Commençons par créer notre premier projet sur le contrôleur Ansible :

```
$ mkdir ~/src/myproject ; cd ~/src/myproject
$ debops-init .
```

Dans la suite de l’article, l’ensemble des commandes seront exécutées depuis le répertoire ~/src/myproject.

L'arborescence créée est la suivante :

```
$ tree
.
├── ansible
│   ├── inventory
│   │   ├── group_vars
│   │   │   └── all
│   │   ├── hosts
│   │   └── host_vars
│   ├── playbooks
│   └── roles
├── .debops.cfg
├── .gitattributes
└── .gitignore
```

Pour bénéficier d'un inventaire prérempli qui contient les informations pour joindre notre contrôleur Ansible et notre nœud, nous allons synchroniser l’inventaire du projet DebOps d’exemple ~/src/controller/ansible/inventory créé lors du provisioning Vagrant, avec notre nouveau répertoire ~/src/myproject/ansible/inventory :

```
$ rsync -av ~/src/controller/ansible/inventory/ ~/src/myproject/ansible/inventory/
```

Notre dossier ~/src/myproject/ansible/inventory/ contient maintenant trois groupes :

- debops_all_hosts : liste des machines à administrer avec DebOps, contient le contrôleur Ansible et le nœud (fichier hosts) ;
- master : contient notre contrôleur Ansible (fichier master) ;
- nodes : contient notre nœud (fichier nodes).

Afin de vérifier que nous sommes en capacité de joindre les deux machines à l’aide d’Ansible, nous pouvons utiliser la commande debops-task sur le groupe debops_all_hosts :

```
$ debops-task debops_all_hosts --module-name ping
debops | SUCCESS => {
[..]
    "ping": "pong"
}
debops-node1 | SUCCESS => {
[..]
    "ping": "pong"
}
```

Maintenant que la connexion entre les machines est établie, nous allons présenter les types de versions de DebOps.

### 2.3 Versions du projet DebOps et commande debops-update
Au moment de l'écriture, le projet DebOps propose deux types de versions [8] :

- une version de développement : les rôles et playbooks Ansible sont récupérés directement depuis la branche master du dépôt Git et utilisés sur les machines à administrer ;
- des versions stables : les rôles et playbooks Ansible sont fournis avec le paquet Python debops. Ces versions sont supportées pendant 1 an après leur date de sortie et reçoivent des correctifs depuis la version de développement. Une nouvelle version stable sort tous les trois mois.

debops a été installé sur le contrôleur Ansible de notre laboratoire à partir des sources de la version 1.1.1. La branche Git stable-1.1 permet de récupérer tous les correctifs depuis sa sortie.

Si vous utilisez une version stable, vous pouvez basculer vers la version de développement de façon globale ou pour un projet spécifique en utilisant la commande debops-update :

```
# globale : installation dans ~/.local/share/debops/debops
$ debops-update
 
# spécifique : installation dans ~/src/myproject/debops
$ debops-update .
DebOps monorepo has not been found, installing into ./debops
```

> **Attention !**
> debops-update utilisée sans argument dans l’environnement Vagrant détectera l’installation de debops faite depuis les sources comme déjà existante et échouera car le dossier .git est manquant. Vous devrez utiliser la forme « spécifique » si vous voulez utiliser la version de développement.
{.is-warning}


Un nouvel appel à cette commande permet de récupérer les dernières mises à jour faites dans le dépôt Git. Il suffit de supprimer le répertoire créé pour rebasculer sur la branche stable.

Si vous avez utilisé la commande debops-update dans votre répertoire, nous vous invitons à supprimer le répertoire créé. En effet, les commandes debops utiliseront toujours ce répertoire en premier par rapport à une installation globale. Nous allons maintenant commencer à éditer notre inventaire pour configurer nos machines.

## 3. Playbook bootstrap.yml : amorçage
### 3.1 Inventaire minimal
Pour placer une machine dans une configuration adéquate et ensuite l’administrer avec DebOps, on utilise le playbook bootstrap.yml. Comme tous les playbooks DebOps, il fait appel à des rôles DebOps dans un ordre particulier. Ces derniers se servent de l’inventaire Ansible ou de leurs valeurs par défaut pour configurer les machines.

Ce playbook requiert au minimum la variable d'inventaire netbase__domain pour configurer correctement le domaine DNS de chaque machine ainsi que son nom.

Le dossier d’inventaire ~/src/myproject/ansible/inventory/group_vars/all regroupe les paramètres qui s’appliquent à tous les hôtes de l’inventaire Ansible. Nous allons donc créer le fichier YAML netbase.yml dans ce répertoire. Voici son contenu :

```
---
netbase__domain: 'vagrant.test'
```

Une bonne pratique du projet Ansible consiste à placer les variables d’un rôle dans un fichier d’inventaire portant le nom du rôle. On peut noter que les noms des variables des rôles DebOps sont préfixés par le nom du rôle et de deux sous-tirets.

### 3.2 Exécution du playbook
Pour lancer le playbook bootstrap.yml, nous allons utiliser la commande debops de la façon suivante :

```
# note : le nom du playbook ne comporte pas l’extension .yml
$ debops bootstrap
```

Sur notre contrôleur Ansible, cette commande va exécuter le playbook bootstrap.yml qui se trouve dans le dossier ~/.local/share/debops/debops/ansible/playbooks sur le groupe d’hôtes debops_all_hosts qui contient notre contrôleur Ansible et notre nœud.

À la fin de l’exécution de ce playbook, vos deux machines disposent :

- d’un dossier /etc/ansible/facts.d qui contient des scripts utilisés par les rôles DebOps (rôle debops.core) ;
- d’un domaine DNS et d’un nom de domaine configurés (rôle debops.netbase) ;
- d’une configuration sudo qui donne tous les droits aux membres du groupe admins (rôle debops.sudo) ;
- d’un utilisateur vagrant, membre du groupe admins, autorisé à se connecter en SSH via sa clé privée à condition qu’elle existe (rôle debops.system_users). Le rôle se sert de la variable d’environnement USER sur le contrôleur Ansible pour déterminer le nom de l’utilisateur à créer.

Avant de poursuivre avec l’exécution du playbook common.yml, il est important de détailler certains éléments du fonctionnement de DebOps, notamment les facts.

> Un playbook bootstrap-ldap.yml existe et permet de préconfigurer des machines avec le support du LDAP. Ceci permet, par exemple, d'accéder aux nœuds via SSH en utilisant un compte de l'annuaire.
{.is-info}


## 4. Fonctionnement de DebOps
### 4.1 Local Facts
Les rôles DebOps utilisent la fonctionnalité de local facts d’Ansible. Grâce aux scripts déposés dans /etc/ansible/facts.d sur chaque machine, un rôle DebOps connaît l'état d'une machine sous forme de variables au moment de l'exécution d'un playbook.

Par exemple, les local facts du rôle debops.core rendent accessibles les adresses IP de tous les contrôleurs Ansible sous la forme d'une variable: ansible_local.core.ansible_controllers. Cette variable est ensuite utilisée par le rôle debops.sshd pour configurer le démon sshd afin d'autoriser uniquement l'accès en tant que root depuis les contrôleurs Ansible.

Ceci donne une grande souplesse aux rôles DebOps et leur permet d'être dépendants les uns des autres.

### 4.2 Dépendances entre les rôles
Ce qui fait la force du projet DebOps, c’est qu’il se comporte comme un gestionnaire de paquets, mais pour l’infrastructure : il gère les dépendances entre les logiciels qui la composent. Un bon exemple est le playbook nginx.yml, utilisé pour installer NGinX via le rôle debops.nginx. Il va aussi configurer le firewall ferm à l’aide du rôle debops.ferm afin qu’il ouvre les flux HTTP et HTTPS sur les nœuds.

Pour gérer ces dépendances, le projet DebOps s’appuie sur une collection importante de playbooks.

### 4.3 Organisation et utilisation des playbooks
Les playbooks se trouvent dans l’arborescence ~/.local/share/debops/debops/ansible/playbooks dans notre environnement. Pour chaque rôle DebOps, on trouve un playbook portant son nom (sans le préfixe debops.) dans le sous-dossier service. Il existe également des playbooks thématiques qui les regroupent. C’est le cas de virt.yml qui inclut l’ensemble des services liés à la virtualisation : lxc.yml, docker_server.yml, libvirtd.yml, libvirtd_qemu.yml et libvirt.yml. Les services de base, mis en place sur chaque nœud, sont regroupés dans common.yml. Enfin, site.yml est le playbook maître : il les regroupe tous.

Comme indiqué au début de l’article, un playbook définit les hôtes sur lesquels il va s’exécuter. Le projet DebOps utilise les groupes d’inventaire couplés aux playbooks nominatifs pour déployer les services. Par exemple, le playbook dnsmasq.yml s’exécutera uniquement sur les membres du groupe d’inventaire debops_service_dnsmasq. Pour les services communs à tous les nœuds, le groupe debops_all_hosts est utilisé : il doit contenir l’ensemble des machines administrées par DebOps.

C’est ce groupe qui est utilisé par le playbook common.yml.

## 5. Playbook common.yml : services de base
### 5.1 Configuration de l’inventaire
Le playbook common.yml va déployer les services de base sur toutes les machines du groupe debops_all_hosts. En version stable 1.1.1, il invoque 48 rôles.

Nous allons maintenant configurer trois rôles via l’inventaire en créant trois fichiers dans le répertoire ~/src/myproject/ansible/inventory/group_vars/all : nullmailer.yml, sshd.yml et unattended_upgrades.yml.

Pour les copier directement dans votre inventaire, vous pouvez utiliser les commandes suivantes :

```
$ git clone https://github.com/nqb/glmf-debops.git $HOME/glmf
$ cp -v $HOME/glmf/ansible/inventory/group_vars/all/{nullmailer,sshd,unattended_upgrades}.yml ~/src/myproject/ansible/inventory/group_vars/all/
```

#### 5.1.1 Le fichier nullmailer.yml
nullmailer [9] est un simple relais SMTP : il relaye les messages qu’il reçoit à un ou plusieurs serveurs SMTP configurés. Voici le fichier d’inventaire :

```yaml
---
# which relay to use, nullmailer__domain equals to ansible_domain
nullmailer__relayhost: 'mail.{{ nullmailer__domain }}'
 
# to which address send all emails
nullmailer__adminaddr: [ 'support@{{ nullmailer__domain }}' ]
 
# to receive emails from user@ansible_hostname.nullmailer__domain
nullmailer__defaulthost: '{{ ansible_hostname }}'
```

Avec cette configuration, tous les mails émis par nos machines seront relayés au SMTP mail.vagrant.test et transmis à l’adresse support@vagrant.test. Les champs From des mails contiendront le nom de chaque machine, exemple : root@debops-node1.vagrant.test.

#### 5.1.2 Le fichier sshd.yml
La variable suivante permet d’activer l’authentification par mot de passe lors d’une connexion SSH. Ce n’est pas le cas par défaut.

```yaml
---
# enable password authentication
sshd__password_authentication: 'yes'
```

#### 5.1.3 Le fichier unattended_upgrades.yml
Le programme unattended-upgrades permet de mettre à jour automatiquement des paquets sur des machines Debian. Le rôle debops.unattended_upgrades active, par défaut, la mise à jour des paquets qui reçoivent des correctifs de sécurité. Tous les rôles DebOps sont dotés d’une variable (nom_rôle__enabled) permettant de les désactiver. C’est ce que nous faisons pour le rôle debops.unattended_upgrades avec l’inventaire suivant :

```yaml
---
# disable debops.unattended_upgrades role
unattended_upgrades__enabled: False
```

Le programme unattended-upgrades ne sera pas installé sur nos machines.

### 5.2 Exécution du playbook
Si vous avez suivi, vous pouvez exécuter le playbook common.yml à l’aide de la commande suivante :

```
$ debops common --limit debops
$ debops common --limit debops-node1
```

> **Attention !**
> L’usage de deux commandes distinctes est volontaire pour éviter que le playbook s’arrête à cause d’un bug identifié [10].
{.is-warning}


Après plus de 300 tâches Ansible exécutées, dont certaines sont communes au playbook bootstrap.yml, vos deux machines se trouvent dans un état de configuration avancé :

- le gestionnaire de paquets APT est configuré pour installer les paquets depuis les dépôts Debian en préférant les backports. Les mises à jour de sécurité sont activées (rôles debops.apt et debops.apt_preferences) ;
- les utilitaires htop, tmux, mc, tree et d’autres sont installés (rôle debops.apt_install) ;
- les accès distants aux machines sont contrôlés par le firewall ferm, les TCP Wrappers et le démon sshd (rôles debops.ferm, debops.tcpwrappers et debops.sshd) ;
- les mails émis sont traités par nullmailer et relayés vers le SMTP configuré (rôle debops.nullmailer) ;
- les machines sont synchronisées via une source de temps avec OpenNTPd (rôle debops.ntp) ;
- les journaux des services déployés sont gérés par logrotate et rsyslog (rôles debops.logrotate et debops.rsyslog).

Ce playbook, comme tous les autres du projet, peut être relancé plusieurs fois de suite, il ne générera pas de changements sur les machines. Ceci permet d’assurer une conformité de l’infrastructure dans le temps.

Le dossier ~/src/myproject/ansible/secret a été créé, voyons maintenant son usage.

### 5.3 Gestion des secrets
Suite à l’exécution du playbook common.yml, le dossier ~/src/myproject/ansible/secret a été créé sur le contrôleur Ansible à l’aide du rôle debops.secret. Il s’agit d’un coffre-fort qui contient tous les secrets générés par les rôles ou nécessaires à leur exécution.

DebOps n’utilise pas ansible-vault, la commande de chiffrement d’Ansible, pour chiffrer les fichiers de ce répertoire, car elle ne supporte pas la génération de mots de passe aléatoires. Par défaut, les fichiers ne sont pas chiffrés et suivis en version, mais ils peuvent l’être à l’aide de la commande debops-padlock [11] ou git-crypt [12].

L’arborescence ~/src/myproject/ansible/secret contient les répertoires suivants :

- credentials : mots de passe générés pour chaque machine, par exemple : le mot de passe du compte root généré par le rôle debops.root_account ;
- dhparam : paramètres Diffie-Hellman utilisés pour sécuriser les communications TLS et SSL des services installés par les rôles DebOps. Permet de disposer de la fonctionnalité de Perfect Forward Secrecy (PFS) [13] (rôle debops.dhparam) ;
- ldap/credentials : mots de passe générés pour les futurs comptes de service dans l’annuaire (pour sudo et sshd) si on installe un annuaire LDAP avec DebOps (rôles debops.sudo et debops.sshd) ;
- pki : infrastructure à clé publique permettant de générer des certificats X.509 signés par une autorité de certification publique ou privée avec le support du protocole ACME [14]. Les rôles DebOps disposent ainsi de certificats prêts à être utilisés pour sécuriser les services déployés (rôle debops.pki).

Le playbook apt_cacher_ng.yml que nous allons appliquer sur notre contrôleur Ansible va également se servir de ce répertoire.

## 6. Playbooks apt_cacher_ng.yml et apt_proxy.yml
### 6.1 Cible
Maintenant que nos deux machines disposent du socle de base, nous pouvons commencer à déployer de nouveaux services. Pour rappel, le but est de disposer d’un proxy cache APT utilisable par l’ensemble de nos machines afin d’accélérer l’installation des paquets Debian.

Pour arriver à cet objectif, nous allons utiliser deux playbooks : apt_cacher_ng.yml pour déployer Apt-Cacher NG sur notre contrôleur Ansible et apt_proxy.yml pour configurer nos deux machines afin qu’elles utilisent le proxy cache APT.

### 6.2 apt_cacher_ng.yml : installation et configuration d’Apt-Cacher NG
#### 6.2.1 Présentation
Le programme Apt-Cacher NG est un proxy au même titre que Squid dédié à l’installation de paquets. Il écoute sur le port TCP 3142 et relaye les requêtes vers les dépôts de paquets. Il met à disposition une interface d’administration et de statistiques sur ce même port, accessible via un navigateur.

Le playbook apt_cacher_ng.yml que nous allons utiliser permet d’aller plus loin. Voici les actions complémentaires qu’il réalise, en plus d’installer Apt-Cacher NG :

- installation d’un reverse proxy NGinX pour sécuriser l’accès à l’interface d’administration en HTTPS à l’aide de la PKI disponible sous secret/pki/ (rôle debops.nginx) ;
- ouverture des ports 3142, 80 et 443 dans le firewall de la machine (rôle debops.ferm) ;
- restriction de l’accès à l’interface d’administration par un mot de passe généré aléatoirement et stocké dans le dossier secret/credentials/debops.vagrant.test/ (rôle debops.apt_cacher_ng).

#### 6.2.2 Édition de l’inventaire et exécution
Ce playbook s’applique uniquement aux hôtes membres du groupe debops_service_apt_cacher_ng. Commençons par placer notre contrôleur Ansible dans ce groupe :

```
$ echo -e "\n[debops_service_apt_cacher_ng]\n$(hostname)" >> ~/src/myproject/ansible/inventory/hosts
```

Nous allons maintenant créer le fichier apt_cacher_ng.yml dans le répertoire ~/src/myproject/ansible/inventory/group_vars/all pour configurer le rôle. Voici son contenu :

```yaml
---
apt_cacher_ng__fqdn: 'debops.{{ ansible_domain }}'
```

Cette variable spécifie que l’accès à l’interface d’administration d’Apt-Cacher NG se fera sur le nom debops.vagrant.test en passant par le reverse proxy NGinX.

Pour exécuter le playbook sur notre contrôleur Ansible, deux possibilités :

```
$ debops service/apt_cacher_ng
# ou
$ debops --limit debops
```

La première méthode exécute uniquement le playbook apt_cacher_ng.yml sur les machines membres du groupe debops_service_apt_cacher_ng, donc sur notre contrôleur Ansible. La deuxième méthode invoque le playbook site.yml, c’est-à-dire le playbook common.yml (détaillé précédemment), puis le playbook apt_cacher_ng.yml. Elle est donc plus longue, mais permet de s’assurer de la conformité de notre contrôleur Ansible. L’argument --limit permet d’exclure debops-node1 de l’exécution.

Définie dans le dossier all de notre inventaire, la variable apt_cacher_ng__fqdn est visible par l’ensemble des machines de l’inventaire. Ceci nous permet de la réutiliser pour configurer notre nœud à l’aide de apt_proxy.yml sans se répéter.

### 6.3 apt_proxy.yml : configuration du proxy APT
#### 6.3.1 Présentation
La configuration d’un proxy pour le programme APT passe par la création d’un fichier dans le dossier /etc/apt/apt.conf.d/ de chaque machine. C’est le rôle de notre playbook qui utilise uniquement le rôle debops.apt_proxy.

Nous allons réutiliser la variable apt_cacher_ng__fqdn pour configurer notre inventaire.

#### 6.3.2 Édition de l’inventaire et exécution
Nous allons éditer notre inventaire pour configurer le proxy APT pour notre contrôleur Ansible (qui héberge également le proxy cache) et pour notre nœud. Afin d’avoir des paramètres différents, il est nécessaire de créer deux répertoires dans l’inventaire :

- le répertoire nodes qui contient les variables d’inventaire des membres du groupe nodes, dont notre nœud ;
- le répertoire debops_service_apt_cacher_ng qui contient les variables d’inventaire des membres du groupe debops_service_apt_cacher_ng, dont notre contrôleur Ansible.

Voici la commande à utiliser :

```
$ mkdir -v ~/src/myproject/ansible/inventory/group_vars/{nodes,debops_service_apt_cacher_ng}
```

Contenu du fichier apt_proxy.yml dans le dossier nodes :

```yaml
---
apt_proxy__http_url: 'http://{{ apt_cacher_ng__fqdn }}:3142'
apt_proxy__https_url: 'https://{{ apt_cacher_ng__fqdn }}:3142'
```

Contenu du fichier apt_proxy.yml dans le dossier debops_service_apt_cacher_ng :

```yaml
---
apt_proxy__http_url: 'http://127.0.0.1:3142'
apt_proxy__https_url: 'https://127.0.0.1:3142'
```

Étant donné que notre contrôleur Ansible ne fait pas partie du groupe nodes, cette configuration s’applique uniquement à lui : elle lui permet d’utiliser le proxy cache APT en local.

Enfin, pour appliquer la configuration sur nos nœuds, il reste à exécuter le playbook :

```
$ debops service/apt_proxy
```

Vous pouvez vérifier la validité de votre configuration en rafraîchissant le cache APT de vos machines à l’aide de la commande suivante :

```
$ debops-task all --become --module-name command --args "apt update"
```

Cette configuration est facilement extensible et directement utilisable sur votre infrastructure.

### 6.4 Extension et réutilisation
Si nous commençons à administrer de nouveaux nœuds via notre projet, nous avons uniquement besoin de les ajouter à notre inventaire dans les groupes debops_all_hosts et nodes. Étant donné que le playbook bootstrap.yml inclut le rôle debops.apt_proxy, les nouveaux nœuds seront paramétrés avec le proxy cache APT dès l’étape du bootstrap, accélérant encore plus l’installation de nouvelles machines. À partir de là, déployer deux machines ou 100 avec un proxy APT ne fait pas beaucoup de différence.

Vous pouvez tester l’ajout d’un nouveau nœud avec la commande suivante sur votre machine hôte, dans le répertoire qui contient le Vagrantfile :

```
$ VAGRANT_NODES=2 VAGRANT_HOSTNAME=debops vagrant up node2
```

La configuration avancée que nous venons de réaliser repose uniquement sur des fichiers d’inventaire. En modifiant les variables netbase__domain et apt_cacher_ng__fqdn, notre projet DebOps peut être réutilisé sur une autre infrastructure.

## Conclusion
Grâce au projet DebOps et à l’environnement Vagrant qu’il met à disposition, nous avons vu comment administrer des machines via Ansible à l’aide des commandes debops, de l’inventaire et des playbooks du projet (bootstrap.yml, common.yml, playbooks nominatifs et site.yml). Le cas d’usage présenté n’est qu’un aperçu de ce qu’il est possible de faire avec DebOps.

L’usage avancé d’Ansible par les rôles et playbooks DebOps nous permet de représenter notre infrastructure en YAML. Ce code est simple, auto documenté, versionnable, facilement extensible et réutilisable. Déployer de nouveaux services ou s’assurer de la conformité de son infrastructure devient une tâche plus aisée, à condition d’être rigoureux et de bien connaître les logiciels à déployer.

Si vous n’utilisez pas encore d’outils comme DebOps pour administrer vos infrastructures Debian, la mise en place de nouvelles machines pour migrer vers Debian Buster peut être une opportunité pour sauter le pas. Le jeu en vaut la chandelle.

## Références
[1] Code source du projet DebOps : http://github.com/debops/debops
[2] Documentation du projet DebOps : http://debops.org
[3] Guide de démarrage rapide avec Vagrant : https://docs.debops.org/en/master/introduction/quick-start.html#quick-start-with-vagrant
[4] Installation de DebOps : https://docs.debops.org/en/master/user-guide/install.html
[5] Page de téléchargement de Vagrant : https://www.vagrantup.com/downloads.html
[6] Page de téléchargement de Virtualbox : https://www.virtualbox.org/wiki/Downloads
[7] Bonnes pratiques du projet Ansible : https://ansible.github.io/workshops/decks/ansible_best_practices.pdf.
[8] Les versions DebOps : https://docs.debops.org/en/master/news/releases.html
[9] Le site officiel de nullmailer : http://untroubled.org/nullmailer/
[10] Arrêt du common playbook : https://github.com/debops/debops/issues/1032
[11] Utilisation de debops-padlock : https://docs.debops.org/en/master/debops-tools/scripts/debops-padlock.html
[12] Utilisation de git-crypt : https://docs.debops.org/en/master/user-guide/install.html#additional-useful-software
[13] Définition de Perfect Forward Secrecy : https://fr.wikipedia.org/wiki/Confidentialit%C3%A9_persistante
[14] Définition du protocole ACME : https://fr.wikipedia.org/wiki/ACME_(protocole)



source : https://connect.ed-diamond.com/GNU-Linux-Magazine/glmf-232/toute-votre-infrastructure-debian-en-yaml-avec-debops