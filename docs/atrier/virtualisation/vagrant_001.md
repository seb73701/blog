---
title: Déployer des environnements de développement avec Vagrant
description: 
published: true
date: 2023-04-21T11:20:19.933Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:20:16.155Z
---

# Déployer des environnements de développement avec Vagrant
>Vagrant est un logiciel libre permettant de déployer rapidement des machines virtuelles. C'est un logiciel développé par la société Hashicorp, déjà connue pour d'autres logiciels comme Terraform, Packer ou Vault. Historiquement lié à VirtualBox, Vagrant s'est désormais largement ouvert à d'autres solutions de virtualisation comme Libvirt ou de conteneurisation comme Docker.

## 1. Bien démarrer
### 1.1 Pourquoi encore un autre outil ?
Vagrant permet de déployer rapidement des environnements à partir de fichiers de description, les Vagrantfile. S’agissant de fichiers textes, ils s’intègrent de fait à toute la chaîne Git ou CI/CD et donc aux bonnes pratiques de développement. L’utilisation d’un outil automatisé permet ainsi de gagner un temps précieux et sans valeur ajoutée sur l’instanciation de machines virtuelles.

L’automatisation du procédé permet également de produire des environnements similaires au sein d’une équipe et à chaque lancement de Vagrant. Les environnements de test sont donc reproductibles, gommant les différences qu’il peut y avoir selon les habitudes de travail et la dérive qui peut se créer sur un environnement au fil du temps.

À titre individuel c’est aussi un moyen de maquetter rapidement un projet ou un outil avant de se pencher sur une infrastructure de production et d’outil type Terraform, plus appropriés.

Vagrant offre une compatibilité avec plusieurs hyperviseurs : VMware, Hyper-V, mais sait également déployer des ressources localement via VirtualBox ou libvirt. C’est aussi un bon moyen de fournir des environnements de tests qui ne monopolisent pas les précieuses ressources des serveurs ou sans avoir à disposition d’infrastructure dédiée, qui plus est disponible sans connexion réseau.

### 1.2 Installation
Sous ma Debian testing, Vagrant est packagé par défaut, donc un simple apt-get install suffit. Pour Ubuntu, CentOS, Fedora, Hashicorp met à disposition des paquets adaptés à la distribution.

Pour les autres, un ZIP contient un binaire à pousser dans un répertoire appelé dans votre PATH. Vagrant, c’est tout simplement ça, tout un workflow unifié autour d’une même CLI.

Un VirtualBox fonctionnel sera nécessaire pour la suite, avec votre utilisateur dans le bon groupe pour éviter de devoir jouer du sudo.

### 1.3 Les boxes
Les boxes Vagrant sont des images de systèmes déployables au sein de tout environnement Vagrant. Il s'agit en pratique d'une façon de packager une distribution afin de permettre un déploiement uniforme quel que soit le système d'exploitation ou l'hyperviseur utilisé. En pratique, toutes les box ne sont pas compatibles avec tous les hyperviseurs, à plus forte raison lorsqu’il s’agit d’une image docker.

De nombreuses boxes sont disponibles sur le site https://app.vagrantup.com et certaines sont officiellement développées par les distributions majeures ou certains éditeurs. Les seules boxes officielles sont toutefois limitées aux boxes Hashicorp en Bento. Un système de versionning permet également de s'assurer de déployer sur une version connue et validée.

Pour télécharger une box, il suffit d’un vagrant box add suivi du nom de la box :

```
vagrant box add centos/7
==> box: Loading metadata for box 'centos/7'
    box: URL: https://vagrantcloud.com/centos/7
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.
 
1) hyperv
2) libvirt
3) virtualbox
4) vmware_desktop
 
Enter your choice: 3
==> box: Adding box 'centos/7' (v2004.01) for provider: virtualbox
    box: Downloading: https://vagrantcloud.com/centos/boxes/7/versions/2004.01/providers/virtualbox.box
Download redirected to host: cloud.centos.org
    box: Calculating and comparing box checksum...
==> box: Successfully added box 'centos/7' (v2004.01) for 'virtualbox'!
```

Et pour lister les boxes installées :

```
vagrant box list
centos/7       (virtualbox, 2004.01)
ubuntu/focal64 (virtualbox, 20210513.0.0)
```

Vous l’aurez compris donc pour la logique sous-jacente, il y a donc notamment les commandes update, remove (et d’autres) pour suivre le cycle de vie des boxes locales.

### 1.4 Le workflow avec Vagrant
Vagrant va chercher à provisionner le type de machine en fonction de ce qui est décrit dans le fichier Vagrantfile du répertoire du projet. Un fichier Vagrantfile est un fichier Ruby, mais qui ne nécessite aucune connaissance de ce langage.

Le fichier Vagrantfile sert à décrire tout ce qui constitue le déploiement : le provider, le dimensionnement de la VM et éventuellement la post-installation. S’agissant d’un fichier texte, il a toute sa place sur un dépôt Git.

Voyons un premier exemple de Vagrantfile. A minima, Vagrant a besoin du nom du déploiement et du type d’image source. Le « 2 » est simplement la version du fichier de configuration.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
end
```

Vérifions ensuite que notre syntaxe est sans erreur :

```
vagrant validate
Vagrantfile validated successfully.
```

S’il n’y a pas d’erreur, on peut lancer l’instanciation de notre projet :

```
vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/focal64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/focal64' version '20210513.0.0' is up to date...
==> default: Setting the name of the VM: 1simple_default_1621158383628_85938
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default:
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default:
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
==> default: Setting hostname...
==> default: Mounting shared folders...
    default: /vagrant => /home/julien/vagrant/1simple
```

L’état du déploiement peut être visualisé avec vagrant status :

```
vagrant status
Current machine states:
default                   running (virtualbox)
[...]
```

À l’instanciation du projet, une paire de clés SSH est créée. Elle est stockée dans l’arborescence du projet vagrant dans .vagrant/machines/NOM_BOX/virtualbox/private_key permettant de se connecter à l’instance sans mot de passe.

```
vagrant ssh default
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-73-generic x86_64)
[...]
Last login: Sun May 16 09:54:12 2021 from 10.0.2.2
vagrant@vagrantbox:~$ logout
```

En fin de cycle, les VM sont ensuite arrêtées et le projet détruit. Lorsque le déploiement n’implique qu’une seule instance, il n’est pas nécessaire de préciser le nom de la VM.

```
vagrant halt default
==> default: Attempting graceful shutdown of VM...
vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Destroying VM and associated drives...
```

Ainsi, le fait de relancer la commande vagrant up va permettre de provisionner un environnement neuf et propre à chaque cycle.

Pour ne pas partir de zéro et avoir un modèle de fichier Vagrant à personnaliser, nous aurions pu partir de la commande vagrant init. En effet, celle-ci crée un fichier Vagrantfile dans le dossier courant qui peut être ensuite personnalisé selon ses souhaits.

### 1.5 Personnalisation
Notre premier exemple servait à mettre le pied à l’étrier sur le cycle de vie d’un environnement Vagrant. Cependant, il sera systématiquement ou presque nécessaire de configurer l’environnement à déployer en fonction des ressources disponibles sur la machine qui héberge les machines virtuelles ou encore en fonction des besoins réels des applicatifs.

Pour cela, Vagrant expose certaines fonctionnalités du provider ou un jeu de fonctionnalités communes. Par exemple, je vais avoir besoin d’accéder à mon serveur web de développement donc je redirige le port local non privilégié 8080 afin de pouvoir accéder à ma VM via http://localhost:8080.

Plus spécifiquement pour VirtualBox, via l’attribut customize il est possible d’exposer n’importe quel paramètre de la commande VboxManage. D’un côté, cela limite la qualité de la couche d’abstraction fournie par le provider Virtualbox pour Vagrant, mais de l’autre celui-ci n’en limite pas les possibilités.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
  config.vm.network "private_network", ip: "192.168.56.10", virtualbox__intnet: "nat"
  
  config.vm.provider :virtualbox do |vbox|
    vbox.gui = false
    vbox.linked_clone = false
    vbox.memory = 2048
    vbox.cpus = 2
    vbox.customize ["modifyvm", :id, "--uart1", "0x3F8", 4]
  end
end
```
J’ai également tendance à utiliser les clones liés afin de limiter l’impact sur la consommation des disques, mais ce point est loin d’être obligatoire.

### 1.6 Multi-machines
Les quelques déploiements précédents étaient tous tournés autour d’une unique VM. Cependant, les infrastructures de production vont plutôt être déployées sur plusieurs serveurs pour :

répondre à des schémas d’architecture multi-tiers ;
combiner plusieurs systèmes d’exploitation ;
distribuer la charge ou la disponibilité du service sur plusieurs machines.
Reproduire ce type d’infrastructure sur une seule machine reviendrait à construire un environnement dev/test différent de la production et donc ne pas couvrir les cas listés ci-dessus. Vagrant est capable de déployer plusieurs VM au sein d’un même projet avec le même workflow. Il suffit pour cela de lister successivement les VM à déployer :

```
Vagrant.configure("2") do |config|
  config.vm.define "revproxy" do |host|
    host.vm.box = "revproxy"
    host.vm.box = "ubuntu/focal64"
  end
  config.vm.define "www", primary: true do |host|
    host.vm.box = "www"
    host.vm.box = "ubuntu/focal64"
  end
  config.vm.define "db" do |host|
    host.vm.box = "db"
    host.vm.box = "ubuntu/focal64"
  end
end
```
Pour provisionner l’infrastructure, les mêmes commandes s’appliquent. Cependant, si l’on souhaite simplement instancier l’un des serveurs de l’infrastructure, par exemple le serveur de base de données, il est possible de préciser celui-ci via vagrant up db, puis vagrant ssh db pour s’y connecter.

Ce listing de VM est un peu ennuyeux cependant, voyons comment factoriser cela en utilisant une variable Ruby de type Symbol :    
```
hosts = [
  { :hostname => 'revproxy', :ip => '192.168.56.10', :mem => 1024, :cpu => 1 },
  { :hostname => 'www',   :ip => '192.168.56.11', :mem => 2048, :cpu => 2 },
  { :hostname => 'db',   :ip => '192.168.56.12', :mem => 2048, :cpu => 2 },
]
 
 
Vagrant.configure("2") do |config|
  hosts.each do |host|
    config.vm.define host[:hostname] do |hostconfig|
      hostconfig.vm.box = "ubuntu/focal64"
      hostconfig.vm.hostname = host[:hostname]
      hostconfig.vm.network :private_network, ip: host[:ip]
      hostconfig.vm.provider :virtualbox do |vbox|
        vbox.gui = false
        vbox.linked_clone = true
        vbox.memory = host[:mem]
        vbox.cpus = host[:cpu]
      end
    end
  end
end
```
Lorsque plusieurs projets comme ces deux versions du multi-instance sont démarrés, il est possible de les visualiser via la commande vagrant global-status. Cela donne également le chemin racine du projet Vagrant sur le système de fichiers.

```
vagrant global-status
id       name     provider   state   directory                           
-------------------------------------------------------------------------
36e198a revproxy virtualbox running /home/julien/vagrant/6multi         
971634f www      virtualbox running /home/julien/vagrant/6multi         
5f4e571 db       virtualbox running /home/julien/vagrant/6multi         
9008b27 revproxy virtualbox running /home/julien/vagrant/7multi         
88fcf71 www      virtualbox running /home/julien/vagrant/7multi         
177b8c7 db       virtualbox running /home/julien/vagrant/7multi
```

### 1.7 Les triggers
Lorsque l’on provisionne un environnement, il peut être nécessaire de déclencher des actions avant ou après le provisionnement d’une VM. Vagrant permet d’utiliser des triggers avant ou après certains états : up, destroy, reload, all, etc.

Les commandes peuvent être lancées aussi bien sur l’hôte que sur l’invité. Dans l’exemple ci-dessous à chaque fois que je provisionne ma VM, je récupère le code à jour depuis le dépôt Git et lorsque je supprime celle-ci, je réalise une sauvegarde de la base de données.

Bon à savoir, par défaut l’exécution de Vagrant échoue si un trigger échoue, mais ce comportement peut être modifié.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
  
  config.trigger.after :up do |trigger|
    trigger.run = {inline: "git clone https://github.com/WordPress/WordPress.git"}
    trigger.on_error = :continue
  end
  config.trigger.before :destroy do |trigger|
    trigger.run_remote = {inline: "mysqldump -u root wordpress > /vagrant/backup.sql"}
  end
end
```

### 1.8 Volumes synchronisés
Pour permettre l’échange de données entre l’hôte et les VM invitées, Vagrant permet de monter un volume sur la VM. Le volume peut être aussi bien un dossier local qu’un montage d’un système de fichiers réseau comme du NFS ou du CIFS.

Vagrant supporte également les options de montage que l’on peut passer à la commande mount. Dans l’exemple ci-dessous, je monte le répertoire html de mon home dans le répertoire par défaut d’Apache en remappant les droits. Si le dossier /home/julien/html n’existe pas, il est créé.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
  config.vm.synced_folder "/home/julien/html/", "/var/www/html", owner: "apache", group: "apache"
create: true
end
```

## 2. Les Provisionners
### 2.1 Provisionner Shell
Provisionner des VM, c’est sympa me direz-vous, mais s’il reste tout le travail de personnalisation des systèmes, nous n’avons guère fait plus que cloner une VM. Une fois que les VM sont provisionnées depuis un modèle de box, c’est un OS vierge qui est fourni quand on arrive au vagrant SSH. Bien entendu rien n’exclut de faire l’installation à suivre à la main, mais ce serait dommage.

Les provisionners sont appelés à deux moments par Vagrant, à l’instanciation avec un vagrant up ou bien avec un vagrant provision pour une instance déjà lancée. Dans l’exemple-ci dessous, vagrant déploie les mises à jour via le shell ainsi que le minimum de packages que j’attends sur mes instances.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
  config.vm.network "private_network", ip: "192.168.56.10", virtualbox__intnet: "nat"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  
  config.vm.provider :virtualbox do |vbox|
    vbox.gui = false
    vbox.memory = 2048
    vbox.cpus = 2
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    apt update
    apt -y dist-upgrade
    apt -y install screen htop nmap
  SHELL
  
end
```

### 2.2 Provisionner Ansible
Bash n’est pas le seul provisionner supporté par Vagrant. En pratique, le support est même très complet, car Vagrant permet d’utiliser la plupart des outils de gestion de configuration qui existent sous Linux : Chef, Puppet, Salt, CFEngine et Ansible ou même cloud-init.

À titre personnel, je n’ai d’expérience qu’avec Puppet et Ansible. Je vous propose de partir sur Ansible, celui-ci étant désormais largement plus répandu. Pour Ansible, partons du petit playbook ci-dessous. Celui-ci déploie les mises à jour et installe un serveur web Apache, tout ce qu’il y a de plus classique.

```yaml
---
- hosts: all
  become: true
  gather_facts: true
  tasks:
    - name: apt upgrade
      apt:
        upgrade: dist
        update_cache: yes
        autoclean: yes
        autoremove: yes
    - name: install apache
      apt:
        name=apache2
        state=present
    - name: Enable service Apache2
      service:
        name: apache2
        state: started
        enabled: yes
```
Le provisionner Ansible nécessite l’installation d’Ansible sur la machine qui lance Vagrant. De la manière la plus simple, il suffit d’indiquer le nom du playbook à lancer. Mon playbook est volontairement succinct, car ce n’est pas ici le propos, mais rien n’interdit des déploiements plus complexes avec des rôles ou l’utilisation d’un Vault par exemple qui sont aussi plus représentatifs des usages réels d’Ansible.

Pour ceux qui ont l’habitude d’Ansible, vous aurez remarqué l’absence notable de l’utilisation d’un inventaire. En pratique, il est possible d’en spécifier un dans la configuration, mais Vagrant a l’intelligence lorsqu’aucun inventaire n’est spécifié d’en autogénérer un pour nous.

```
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.hostname = "vagrantbox"
  config.vm.network "private_network", ip: "192.168.56.10", virtualbox__intnet: "nat"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  
  config.vm.provider :virtualbox do |vbox|
    vbox.gui = false
    vbox.linked_clone = true
    vbox.memory = 1024
    vbox.cpus = 1
  end
  
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.compatibility_mode = "2.0"
  end
 
end
```

## 3. Et pour d’autres providers ?
### 3.1 Quel intérêt ?
Utiliser Vagrant avec un autre provider a un premier intérêt certain c’est que le même outil sera utilisé, quel que soit l’environnement mis à disposition ou le projet sous-jacent.

Ainsi en termes de formation ou de prise en main, la courbe d’apprentissage est plus faible que d’apprendre un autre outil.

### 3.2 Docker
Comme je le disais au départ, Vagrant a pour objectif d’être relativement agnostique vis-à-vis de la solution de virtualisation. Il existe par exemple un provider docker officiel. Dans le code ci-après, je suis parti de l’exemple du Dockerhub pour instancier un cluster WordPress avec Docker compose, mais remis à la sauce Vagrant.

L’image est ici automatiquement téléchargée depuis la registry. Dans le cadre d’une utilisation en mode développement, Vagrant est également capable d’aller construire l’image si on lui spécifie à la plage de l’image le chemin vers le Dockerfile, avec l’attribut build_dir, voire même depuis un dépôt Git en utilisant l’attribut git_repo. Naturellement ces trois choix sont exclusifs.

```
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'
Vagrant.configure("2") do |config|
 
  config.vm.define "db" do |db|
    db.vm.synced_folder ".", "/vagrant", disabled: true
    db.vm.provider "docker" do |d|
      d.image = "mysql:5.7"
      d.name = "wordpressdb"
      d.remains_running = true
      d.volumes = ["/home/julien/mysql:/var/lib/mysql"]
      d.env = {"MYSQL_DATABASE" => "exampledb", "MYSQL_USER" => "exampleuser", "MYSQL_PASSWORD" => "examplepass", "MYSQL_RANDOM_ROOT_PASSWORD" => "'1'"}
    end
  end
  
  config.vm.define "app" do |app|
    app.vm.synced_folder ".", "/vagrant", disabled: true
    app.vm.provider "docker" do |d|
      d.image = "wordpress"
      d.ports = ["8080:80"]
      d.name = "wordpressapp"
      d.remains_running = true
      d.volumes = ["/home/julien/html:/var/www/html"]
      d.env = {"WORDPRESS_DB_HOST" => "db", "WORDPRESS_DB_USER" => "exampleuser", "WORDPRESS_DB_PASSWORD" => "examplepass", "WORDPRESS_DB_NAME" => "exampledb"}
      d.link("wordpressdb:mysql")
    end
  end
end
```

Dans le cas présent, mon conteneur web est lié au conteneur de base de données, il faut donc que vagrant ne lance pas l’instanciation en parallèle des conteneurs, on y ajoute donc le paramètre --no-parallel.

```
vagrant up --no-parallel
Bringing machine 'db' up with 'docker' provider...
Bringing machine 'app' up with 'docker' provider...
==> db: Creating and configuring docker networks...
==> db: Creating the container...
    db:   Name: wordpressdb
    db: Image: mysql:5.7
    db: Volume: /home/julien/mysql:/var/lib/mysql
    db:
    db: Container created: 30dba7edabca0db7
==> db: Enabling network interfaces...
==> db: Starting container...
==> app: Creating and configuring docker networks...
==> app: Creating the container...
    app:   Name: wordpressapp
    app: Image: wordpress
    app: Volume: /home/julien/html:/var/www/html
    app:   Port: 8080:80
    app:   Link: wordpressdb:mysql
    app:
    app: Container created: 19838387018b928d
==> app: Enabling network interfaces...
==> app: Starting container...
```

Vérifions du côté de la CLI Docker qu’il n’y ait pas d’erreur. Parfait, nos conteneurs sont correctement instanciés.

```
docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                  NAMES
19838387018b   wordpress   "docker-entrypoint.s…"   10 seconds ago   Up 9 seconds    0.0.0.0:8080->80/tcp   wordpressapp
30dba7edabca   mysql:5.7   "docker-entrypoint.s…"   11 seconds ago   Up 10 seconds   3306/tcp, 33060/tcp    wordpressdb
```

## Conclusion
Si vous utilisiez un hyperviseur comme VirtualBox ou libvirt pour déployer des VM à la main et si vous avez besoin de régulièrement recréer vos environnements de développement, Vagrant devrait beaucoup apporter à votre trousse à outils. Et pour la production, Terraform est l’étape suivante si vous ne l’utilisez pas encore.

## Références
[1] https://www.vagrantup.com/downloads
[2] https://app.vagrantup.com/boxes/search
[3] https://www.vagrantup.com/docs



source : https://connect.ed-diamond.com/linux-pratique/lp-127/deployer-des-environnements-de-developpement-avec-vagrant