---
title: Introduction aux déploiements et configurations automatisés avec Puppet
description: 
published: true
date: 2023-04-21T10:49:09.717Z
tags: réseau, parc informatique, puppet
editor: markdown
dateCreated: 2023-04-21T10:49:05.706Z
---

# Introduction aux déploiements et configurations automatisés avec Puppet

> Dans chaque infrastructure sérieuse, cela demande un temps conséquent pour maintenir le paramétrage des systèmes lorsque cela est réalisé manuellement. En outre, ces opérations manuelles peuvent être à l'origine d’erreurs ou de divergences dans les configurations et cela pose des difficultés lorsque des ajustements de configuration ou des déploiements doivent être réalisés rapidement sur un parc de machines. Puppet est là pour vous faciliter la vie dans ces situations.

## 1. Qu’est-ce que Puppet ?
Puppet est un outil de gestion de configuration exploitant un langage qui lui est propre, dont le but est :

- d’automatiser la configuration des systèmes et le déploiement des logiciels ;
- de déployer rapidement des paramétrages sur un parc de serveurs ;
- de s’assurer que les configurations voulues sont celles qui sont appliquées.

L'outil opère en mode client/serveur. Sur chacune des machines gérées que l'on appelle nodes, un agent lancé en mode démon contacte le serveur Puppet, le puppetmasterafin d’obtenir sa configuration. Le puppetmaster va compiler le code afin de construire la configuration spécifique au nodeappelée catalogue et l’agent se charge de l'appliquer. Un outil de collecte d'informations systèmes exécuté sur l’agent :facter, participe à la construction de ce catalogue depuis les informations inventoriées sur le node (version de la distribution, adresses IP, etc.).

En cas de divergence entre la configuration décrite et celle appliquée, l'agent va se charger de corriger cette dérive en comparant l’état du système et le catalogue. L'agent réalisant cette opération de manière périodique, les opérations décrites dans les manifests sont pensées pour être répétables, ainsi on s'assure de la conformité des systèmes dans le temps.

## 2. Le langage
Le DSL Puppet (Domain-Specific Language) est un langage déclaratif permettant de décrire le fonctionnement attendu des machines dans des fichiers texte appelés manifests. Le code Puppet tourne essentiellement autour des ressources qui vont permettre de gérer les éléments clés des systèmes. Elles sont de différents types etde manière non exhaustive nous avons :

- compte utilisateur ;
- fichier ;
- package ;
- services ;
- crontab.

Ces ressources présentent un certain niveau d'abstraction afin de se consacrer à l'état que l'on veut appliquer sans se soucier des spécificités du système qui sont gérées par les provider. Ainsi, lorsque l’on souhaite l’installation d’un paquet, Puppet grâce à Facter va déduire s’il faut utiliser Yum ou Apt. Bien entendu, il est possible de créer ses propres ressources en Ruby. Un ordonnancement des ressources peut être appliqué lorsqu’il y a des dépendances que l’on souhaite voir gérées.

Ces ressources sont regroupées en classes dont le rôle est de comporter l'ensemble des éléments de configuration d'une fonctionnalité, d'un service ou d'une application. À un niveau plus élevé, les modules sont des regroupements de classes cohérents. Par exemple, nous pourrions avoir un module pour la supervision avec Nagios. Dans ce module, nous trouverions deux classes. Une première pour déployer la partie serveurde supervision qui serait affectée au serveur correspondant. Une seconde classe se chargerait de déployer les agents pour le monitoring des hôtes et serait affectée à l’ensemble des nodes à surveiller.

## 3. Installation
L'éditeur propose des packages pour un grand nombre de distributions, ce qui permet de ne pas perdre de temps dans des détails d’installation. Il n'y a pas non plus de réelles différences pour la suite de l'article si vous êtes habitué à une CentOS ou une Ubuntu. Puppet met à disposition un paquet qui contient la définition du repository adapté à la distribution. Ainsi, pour CentOS 7 vous pourrez récupérer le RPM disponible à l'adresse https://yum.puppet.com/puppet6-release-el-7.noarch.rpm et pour Ubuntu 18.04, il s'agira du paquet https://apt.puppetlabs.com/puppet6-release-bionic.deb. Dans les deux cas, je vous invite à rafraîchir la liste des paquets avant de poursuivre. Dernier point, l'agent puppet peut bien entendu être déployé sur le serveur lui-même, rien ne lui empêche de s’autoconfigurer.

### 3.1 Premiers pas en mode agent-less
En introduction, je vous ai en partie menti en vous disant que Puppet fonctionnait en mode client-serveur. L’agent est capable d’appliquer des manifests locaux. Nous allons pour l'instant uniquement installer le paquet puppet-agent et écrire notre premier manifest qui va simplement installer deux paquets. Voici ce que contient notre fichier packages.pp, son emplacement sur le filesystem est sans importance.

```puppet
Package { 'traceroute':

  ensure => 'installed'

}

Package { 'screen':

  ensure => 'installed'

}
```

Vérifions que la syntaxe est correcte :  

```puppet
puppet parser validate packages.pp
```

S’il n’y a pas d’erreurs, alors la commande précédente ne renvoie rien. Nous pouvons appliquer le manifest :

```
puppet apply packages.pp
Notice: Compiled catalog for puppet in environment production in 0.53 seconds
Notice: /Stage[main]/Main/Package[traceroute]/ensure: created
Notice: Applied catalog in 7.45 seconds
```

### 3.2 En mode client-serveur
Pour démarrer de façon simple, il suffit d'installer le package puppetserver sur le puppetmaster.

```
yum/apt install puppetmaster
```

Ensuite, il faut savoir que les agents communiquent avec le serveur en s'authentifiant par certificat. Il faut donc créer une CA sur le puppetmaster. Il est indispensable pour ce point que le serveur ait un nom d'hôte pleinement qualifié.

```
puppetserver ca setup
Generation succeeded. Find your files in /etc/puppetlabs/puppet/ssl/ca
systemctl enable puppetserver
Synchronizing state of puppetserver.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable puppetserver
systemctl start puppetserver
console
```

Vérifions notre CA :

```
puppetserver ca list --all
Signed Certificates:
   puppet.lab.morot.fr   (SHA256)  C1:B5:23:13:B2:7B:1E:82:98:12:D3:D1:2E:14:F2:A1:60:68:D2:88:38:A8:7E:B4:0C:65:6B:43:0E:74:27:5A     alt names: ["DNS:puppet", "DNS:puppet.lab.morot.fr"]
```

Il nous faut ensuite indiquer à notre agent comment se connecter au serveur. Pour cela, deux moyens :

- soit on fait pointer notre enregistrement DNS puppet.domaine.tld vers le serveur Puppet ;
- soit on le définit manuellement dans le fichier /etc/puppetlabs/puppet/puppet.conf comme ceci :

```puppet
[agent]

server = puppet.lab.morot.fr
```

L’agent se connecte sur le serveur via le port TCP/8140, il faudra veiller à ce que le firewall du serveur autorise les connexions s’il y en a un de configuré.

Il ne nous reste plus qu'à lancer l'agent une première fois pour générer une demande de certificat :

```
puppet agent -t
Info: Downloaded certificate for ca from puppet
Info: Downloaded certificate revocation list for ca from puppet
Info: Creating a new RSA SSL key for agent.lab.morot.fr
Info: csr_attributes file loading from /etc/puppetlabs/puppet/csr_attributes.yaml
Info: Creating a new SSL certificate request for agent.lab.morot.fr
Info: Certificate Request fingerprint (SHA256): D1:BD:DB:9B:DF:1B:59:0E:EE:21:4D:FE:A0:3C:29:D2:72:4A:75:36:71:3E:A2:73:C7:58:5C:8D:B9:16:5E:1A
Info: Certificate for agent.lab.morot.fr has not been signed yet
Couldn't fetch certificate from CA server; you might still need to sign this agent's certificate (agent.lab.morot.fr).
Exiting now because the waitforcert setting is set to 0.
```

Que nous allons signer sur le puppetmaster :

```
puppetserver ca sign --all
Successfully signed certificate request for agent.lab.morot.fr
```

Notre agent est ainsi autorisé à communiquer avec le serveur bien que nous ne lui appliquons aucune classe. Voyons cela dans les étapes qui vont suivre.

## 4. Notre module système pas à pas
Pour la suite de cet article, nous allons construire un module de configuration d'un système de base qui permettra de proposer une configuration standardisée au sein de notre parc de serveurs. Cet exemple universel permettra à chacun de se l'approprier et constitue une bonne base d’apprentissage. Cela nous permettra de nous familiariser avec les principales briques de construction et nous rendre autonomes pour réaliser des modules à la fois plus complets et plus élaborés. Dans un premier temps, nous verrons les différentes étapes de construction du module pour en tout dernier lieu l'appliquer à nos systèmes.

Ce faisant, nous avons besoin de créer une arborescence pour héberger un module :

```
mkdir /etc/puppetlabs/code/environments/production/modules/stdsystem
cd /etc/puppetlabs/code/environments/production/modules/stdsystem
mkdir manifests templates files
```

### 4.1 Nos packages de base
Nous avons vu précédemment comment installer des paquets, voyons maintenant comment faire ceci avec une classe réutilisable et de manière plus élaborée. Notre fichier manifests/packages.pp va ressembler à ceci :

```puppet
class stdsystem::packages {
    $stdpackages = ["screen", "htop", "sudo", "strace", "vim"]
    Package { $stdpackages:
        ensure => "installed"
    }
}
```

Nous venons de voir ainsi que Puppet peut parcourir un tableau et que dans ce cas, il n'est pas nécessaire de définir une boucle ou un itérateur pour installer chacun des packages.

### 4.2 Gérer un service
Chaque serveur dispose d'un certain nombre de services systèmes en fonctionnement : NTP, SSH, SNMP, MTA, etc. Le déploiement d'un service recouvre un ensemble de tâches similaires pour la plupart des services, c’est-à-dire l'installation, la configuration et le redémarrage du service.

Nous allons prendre en exemple l'installation d'un serveur NTP. Par ce biais, nous allons nous assurer que toutes nos machines disposent d'une configuration fonctionnelle et identique. Pour commencer, nous allons créer le fichier files/chrony.conf dans notre module qui contient une configuration minimaliste.

```
pool ntp.ubuntu.com        iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2
keyfile /etc/chrony/chrony.keys
driftfile /var/lib/chrony/chrony.drift
logdir /var/log/chrony
maxupdateskew 100.0
rtcsync
makestep 1 3
```

Et nous allons créer une classe associée pour déployer le service et sa configuration dans le fichier manifests/ntp.pp :

```puppet
class stdsystem::ntp {
    $ntp_name = "chrony"
    Package { $ntp_name: ensure => "present" }
    Service { $ntp_name:
        ensure  => "running",
        enable  => true,
        require => Package[$ntp_name],
    }
    File { "/etc/$ntp_name/$ntp_name.conf":
        mode => "0644",
        owner   => "root",
        group   => "root",
        require => Package[$ntp_name],
        source => "puppet:///modules/stdsystem/$ntp_name.conf",
        notify  => Service[$ntp_name],
    }
}
```

Une variable $ntp_name est utilisée systématiquement afin d'avoir un code plus flexible. Notre classe utilise trois ressources : Package pour installer le nécessaire, Service pour s'assurer que le serveur NTP soit démarré (running) et activé au démarrage (enable) et File pour déployer le fichier de configuration. Ces deux dernières classes ont une dépendance (require) sur l'installation du paquet afin d'assurer un ordonnancement correct des étapes. Le module File exploite l'attribut notify, ainsi en cas de changement dans le fichier de configuration, la classe Puppet sait qu'elle doit procéder au redémarrage du service automatiquement pour prendre en compte les changements.

### 4.3 Facter
Facter est un outil d'inventaire en Ruby intégré à Puppet. Il permet de remonter au serveur un certain nombre d'informations sur le système avant la construction du catalogue, les facts. Il est possible de lister les facts connus en lançant tout simplement la commande facter. Il est également possible d'indiquer explicitement une clé de l'arborescence en paramètre de la commande :  

```
root@agent:~# facter os.distro.description
Ubuntu 18.04.2 LTS
```

Il est à noter que facter peut être utilisé en dehors de Puppet, par exemple avec un script shell. Il est possible d’étendre l’inventaire avec la notion de custom facts, mais cela dépasse le cadre de cet article. L'utilisation des facts devient rapidement indispensable dans les manifests. Par exemple, avec l'utilisation de structures conditionnelles, il va être possible de gérer aisément les différences entre les distributions en particulier lorsqu’un module doit être compatible Red Hat et Debian. Voici comment nous pourrions ainsi gérer la différence de nommage du paquet du serveur SNMP dans notre classe stdsystem::packages :

```puppet
    if $facts[osfamily] == "Debian" {
        $snmpd = "snmpd"
    } else {
        $snmpd = "net-snmp"
    }
    Package { $snmpd: ensure => installed  }
```

Le second cas d'usage notable concerne le cas du déploiement de fichiers de configuration comportant des informations propres au système sur lequel l'agent puppet s’exécute. Ces spécificités peuvent provenir de variables provenant de Facter ou en exploitant les paramètres qui peuvent être passés aux classes. Dans ce cas d’usage, ce ne sont pas des fichiers qui sont déployés, mais des templates compilés avec le catalogue. Nous allons ainsi créer dans le fichier manifests/hosts.pp un manifest qui s'assurera que le fichier /etc/hosts est standardisé.

```puppet
class stdsystem::hosts {
    file {"/etc/hosts":
        ensure => file,
        mode => "0644",
        content => template("${module_name}/hosts.erb"),
    }
}
```

Le contenu de ce manifest ne présente pas de complexité, nous avons simplement ajouté la notion de variable built-in avec la variable $module_name. Notre ressource file fait appel à l'attribut content qui pointe vers un template ERB. Il s'agit d'un fichier texte contenant des tags avec du code Ruby. Nous avons donc un fichier templates/hosts.erb qui exploite les facts. Pour chaque machine sur laquelle la classe sera appliquée, un fichier personnalisé sera généré avec l'adresse IP de l'interface principale ainsi que le nom d'hôte de la machine :

```
127.0.0.1       localhost.localdomain   localhost
<%= @ipaddress %>  <%= @hostname%>.<%=@domain %> <%= @hostname %>
```

### 4.4 Les modules externes
Les ressources de base permettent de démarrer avec Puppet, néanmoins rapidement vous serez limité par celles-ci. La forge Puppet contient un grand nombre de modules pour étendre les fonctionnalités et ajouter de nouveaux types de ressources.

Il existe ainsi des modules prêts à l'emploi pour Apache, MySQL, iptables, gérer des machines sous vSphere, des volumes LVM, etc. Leur qualité est variable, mais vous y trouverez aussi les modules puppetlabs qui eux sont supportés. Voyons comment installer le module stdlib et l'utiliser au sein de notre module.

```
puppet module install puppetlabs-stdlib
[...]
/etc/puppetlabs/code/environments/production/modules
└── puppetlabs-stdlib (v6.0.0)
```

Nous allons maintenant créer une classe dans le fichier manifests/sshd.pp afin de sécuriser notre serveur en désactivant les connexions en root :

```puppet
class stdsystem::sshd {
    file_line { 'sshd_root':
        ensure => present,
        path => "/etc/ssh/sshd_config",
        line => "PermitRootLogin no",
        match => "^PermitRootLogin",
        append_on_no_match => true,
    }
}
```

Pour bien faire, il faudrait ajouter une dépendance pour redémarrer le service ssh ce que je n'ai volontairement pas fait afin de rester concis. La commande puppet module intègre un gestionnaire de module avec des commandes recherche et de (dés)installation pour vous permettre de gérer les modules installés.

### 4.5 Finalisons notre module
Nous avons découpé nos classes pour que celles-ci se trouvent dans un manifest dédié pour plus de lisibilité. Cependant, il nous manque le liant entre ces différentes classes. Elles vont donc être inclues dans le manifest init.pp que nous aurons juste à adosser à nos différents nodes sans avoir besoin d’associer toutes les classes à chaque nœud :

```puppet
class stdsystem {
    include stdsystem::packages
    include stdsystem::ntp
    include stdsystem::sshd
    include stdsystem::hosts
}
```

Enfin, nous allons ajouter un dernier fichier à la racine de notre module nommé metadata.json qui servira à décrire notre module et si on le souhaite le publier :

```json
{
  "name": "jmorot-stdsystem",
  "version": "0.0.1",
  "author": "jmorot",
  "summary": "Configuration Linux de standard.",
  "project_page": null,
  "issues_url": null,
  "source": "",
  "license": "",
  "dependencies": [
    {
        "name": "puppetlabs-stdlib",
        "version_requirement": ">= 1.0.0"
    }
  ]
}
```

### 4.6 Application du module sur un node
Nous touchons au but de notre module : configurer nos serveurs. Jusqu'ici nous avons simplement posé les bases avec la gestion de différentes ressources. Il donc convient enfin de tirer profit de notre module. Nous allons donc créer un fichier /etc/puppetlabs/code/environments/production/manifests/site.pp et associer un node sur lequel l’agent est installé à notre module :

```puppet
node agent.lab.morot.fr {
        include stdsystem
}
```

Si désormais vous relancez l'agent comme suit sur le node, vous constaterez alors que les différentes configurations ont été appliquées et le module stdlib déployé.

```
puppet agent -t
```

Bien entendu, il n'est pas requis de lancer l'agent manuellement. L'agent peut être lancé en tant que service comme n'importe quel autre service sur votre système et c’est même le comportement préféré à adopter. De la sorte, l'agent va interroger automatiquement à intervalle régulier le serveur Puppet. Vous pouvez même gérer ce comportement avec Puppet ! Le lancement manuel tel que nous l’avons fait permet de suivre visuellement les opérations réalisées.

Pour rendre la gestion plus simple lorsque le nombre de node se multiplie, Puppet permet d'utiliser différents mécanismes comme les profils, les regexp voire même les external node classifier pour procéder directement à l'association entre les hôtes et les classes, mais cela nécessiterait un article dédié.

## Conclusion
Pour aller plus loin, j'invite le lecteur intéressé à compléter notre module afin d'en créer un complet et digne de ce nom. Les idées pour cela ne manquent pas, utiliser le module puppetlabs/lvm pour gérer un volume /srv sur un disque dédié, appliquer des règles sysctl pour sécuriser la pile TCP, déployer un agent Nagios NRPE, etc. et bien d’autres choses qu’il est possible d’automatiser.

J'espère que cet aperçu vous a permis d'apprécier l'étendue des possibilités. Puppet permet un grand nombre de choses et beaucoup de fonctionnalités ne peuvent être abordées ici comme Hiera, PuppetDB ou Bolt, mais si vous n'avez pas encore franchi le pas d'un outil de gestion de configuration, Puppet est à la fois puissant et abordable.

## Références
[1] La documentation officielle du serveur : https://puppet.com/docs/puppetserver/6.4/index.html
[2] La forge vers les modules tiers : https://forge.puppet.com/
[3] Les cookbooks pour des solutions à des problèmes courants : https://www.puppetcookbook.com/



source : https://connect.ed-diamond.com/Linux-Pratique/lp-115/introduction-aux-deploiements-et-configurations-automatises-avec-puppet