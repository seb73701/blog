---
title: Tirez le meilleur parti de votre gestionnaire de configuration Puppet
description: 
published: true
date: 2023-04-21T10:49:03.503Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:58.907Z
---

# Tirez le meilleur parti de votre gestionnaire de configuration Puppet

> Puppet [1], outil de gestion de configuration et d’automatisation, a été présenté précédemment dans les colonnes de Linux Pratique [2]. Je vous propose de découvrir certaines fonctionnalités du logiciel qui ont évolué ou qui méritent de s’y attarder pour exploiter au mieux cet outil.

## 1. Étendre l’inventaire avec les Custom facts
### 1.1 Présentation
Facter est un outil d'inventaire utilisé par Puppet pour collecter un certain nombre d'informations sur le système comme les adresses IP des interfaces, le type de système d'exploitation ou la taille de la mémoire, appelées facts. Ces informations sont transmises au serveur Puppet afin de construire le catalogue, c'est-à -dire l'ensemble des actions spécifiques au node. Il est possible en ligne de commandes de consulter les facts connus de Puppet à l'aide de la commande puppet facts.

Bien que Facter remonte un certain nombre d'éléments sur le système, ceux-ci peuvent se révéler insuffisants pour certains besoins. Cependant, avec quelques lignes de Ruby, il possible de développer de nouveaux facts (custom facts) afin de créer des variables personnalisées qu'il sera possible de réutiliser dans les templates ou les manifests.

### 1.2 Cas pratique
Pour embarquer un custom facts dans un module, il faut le placer dans le répertoire `<module>/lib/facter`. Pour ceux qui ont connu pluginsync pour distribuer des facts, c'est obsolète, vous n'avez plus à gérer cela dans vos fichiers puppet.conf.

Voyons un exemple d'un code Ruby placé dans le répertoire d'un module lib/facter/ipv4_fowarding.rb qui nous retourne si le routage IPv4 est activé au niveau du kernel :

```
Facter.add("ipv4_forwarding") do
  setcode '/bin/cat /proc/sys/net/ipv4/ip_forward'
end
```

Le code est facilement compréhensible :

- facter.Add : indique le nom du fact tel qu'il est exploitable avec Puppet ;
- setcode : sert à déterminer la valeur retournée par le fact. En appelant directement setcode, on renvoie de façon brute la sortie de la commande. L’utilisation des pipes shell est même possible dans une commande.

Cependant, notre fact n'est pas très évolué et surtout, cette commande n'est pas portable en dehors de Linux. Voyons une version plus poussée :

```
Facter.add("ipv4_forwarding") do
  confine :kernel => :Linux
  setcode do
    Facter::Core::Execution.execute('/bin/cat /proc/sys/net/ipv4/ip_forward')
  end
end
```

Cette fois :

- confine limite l'exécution du fact à un kernel Linux, c'est ce que l'on cherche ;
- :kernel est un fact, on voit donc qu'on peut utiliser des variables remontées par facter au sein d’un custom facts ;
- on utilise la méthode Facter::Core::Execution.execute, car dans le cadre d’un custom facts un peu plus complexe, nous aurions probablement besoin de parser plus précisément la sortie de notre commande.

Après un passage de l'agent, il est possible de vérifier que le facts remonte correctement :

```
root@agent:~# puppet facts |grep ipv4_forwarding
    "ipv4_forwarding": "1",
```

Le lancement de la commande facter seule sur un node ne fonctionnera pas, car facter résout autrement les chemins. Il faut soit expliciter la variable Ruby $LOAD_PATH ou en CLI préciser le chemin où trouver les facts avec le paramètre --custom-dir.

```
facter --custom-dir /opt/puppetlabs/puppet/cache/lib/facter/ ipv4_forwarding
0
```

Pour autant, la commande puppet facts reste la méthode recommandée, car elle se charge de résoudre les chemins pour vous avec les mécanismes Puppet :

```
puppet facts |grep -i ipv4_forward
    "ipv4_forwarding": "0",
```

En cas d'anomalie, la commande facter -p comme la commande puppet facts dispose d'un switch --debug qui vous donnera davantage d'informations sur l'échec de fonctionnement d'un fact.

Notre custom fact peut être ensuite utilisé librement dans un manifest ou un template :

```
    if ($ipv4_forwarding == '1') {
        package { 'quagga-ripd': ensure => "present" }
    }
```

## 2. Étendre le langage avec les custom functions
### 2.1 Présentation
De par le mode déclaratif du langage Puppet, on manipule immédiatement des classes et des ressources. Pour autant, certaines fonctions sont utilisées plus que couramment dans les manifests comme la fonction template() afin de déployer des fichiers depuis des modèles au format ERB. Puppet étant extensible, il est possible d'étendre les possibilités d'interprétation en écrivant des fonctions personnalisées en utilisant le langage Ruby. Cependant, comme les fonctions sont exécutées par le puppetmaster, elles ne peuvent prendre en charge des actions nécessitant un accès direct au node.

Les customs functions sont à déployer dans le répertoire `<module>/lib/puppet/parser/functions` pour un module, de manière plus globale à un environnement dans /etc/puppetlabs/code/environments/production/lib/puppet/functions/ ou enfin, dans le répertoire puppet/parser/functions de la variable interne Ruby $LOAD_PATH. Le nom du fichier Ruby doit nécessairement porter le même nom que la fonction, suffixé de .rb. À noter, en cas de modification d'une fonction, il faut redémarrer le puppetmaster.

### 2.2 Mise en œuvre
Voyons comment réaliser une fonction qui fera la résolution DNS d'un nom d'hôte. Cela nous permettra dans nos manifests de ne travailler qu'avec des hôtes et non des adresses IP, mais de générer des templates de configuration avec les adresses IP. Bien entendu, je parle d'adresses qui sont externes au node Puppet, sinon cela n'aurait aucun sens, il serait plus logique d'utiliser directement les facts.

Notre fonction a un usage général, je la place donc dans /etc/puppetlabs/code/environments/production/lib/puppet/functions/dnsresolv.rb.

```
require 'resolv'
 
Puppet::Functions.create_function(:dnsresolv) do
  dispatch :dnsresolv do
    required_param 'String', :hostname
  end
 
  def dnsresolv(hostname)
     addr = Resolv.getaddress(hostname)
     return addr
  end
end
```

Toute custom function est crée par l'API via Puppet::Functions.create_function. Le nom est passé en paramètre. S'il s'agissait d'une fonction spécifique à un module, nous appellerions Puppet::Functions.create_function(:'module::dnsresolv'). La fonction dispatch sert à déterminer les paramètres attendus et la méthode associée. La valeur de retour est celle qui peut être utilisée dans notre manifest :

```
$remotename = dnsresolv('www.ed-diamond.com')
notify { "${remotename}": }
```

Il est même possible d’appeler notre fonction directement depuis un template avec la méthode scope.call_function :

```
<%= scope.call_function("dnsresolv", ["fr.pool.ntp.org"]) %>
```

Le premier paramètre est le nom de la fonction, les arguments sont passés sous forme de tableau.

On peut bien entendu exploiter des facts au sein des custom functions. Prenons le cas du fact qui nous renvoie la mémoire totale en octets (facter memory.system.total_bytes) et faisons une fonction qui nous convertit la valeur en Mo. Les facts peuvent être accédés dans un tableau après l’appel de la méthode closure_scope avec la même arborescence que celle que nous connaissons de facter :

```
Puppet::Functions.create_function(:total_mem_mo) do
  def total_mem_mo()
    scope = closure_scope
    return scope['facts']['memory']['system']['total_bytes'] /1024 / 1024
  end
end
```

## 3. Exported ressources : PuppetDB
### 3.1 Introduction
Une ressource exportée consiste à déclarer une ressource telle qu’on la voudrait voir appliquée sur un node, mais au lieu de l’appliquer, celle-ci est publiée dans une base de données, la PuppetDB, pour être réutilisée par les autres nœuds. Les ressources exportées permettent d’utiliser n’importe quel type de ressources, qu’elles soient natives ou bien fournies par des modules externes.

De plus, les ressources exportées permettent de réutiliser les facts collectés sur certaines machines pour les réutiliser dans des ressources appliquées sur d’autres serveurs. Cela permet ainsi de façon relativement aisée d’automatiser le déploiement de certains services qui vont s’appuyer sur l’environnement d’un autre serveur comme ce qui touche à la supervision, à la sauvegarde ou au déploiement des clés d’hôtes SSH.

### 3.2 Installation
À titre d'illustration, procédons à une installation sous Ubuntu 18.04 sur le même serveur que le serveur Puppet. Puppetlabs propose un module d'excellente qualité pour déployer toute la configuration, cependant il me semble plus didactique de réaliser une installation manuelle. PuppetDB stocke ses informations dans une base PostgreSQL et n'offre pas d'alternative à ce niveau pour l'instant.

```
apt -y install postgresql postgresql-contrib
```

Par défaut, le fichier /etc/postgresql/10/main/pg_hba.conf contient déjà les autorisations de connexion nécessaires :

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
```

On crée ensuite un utilisateur PostgreSQL et une base de données associée :

```
root@puppet:~# su - postgres
postgres@puppet:~$ createuser -P puppetdb
Enter password for new role:
Enter it again:
postgres@puppet:~$ createdb -E UTF8 -O puppetdb puppetdb
postgres@puppet:~$ psql puppetdb -c 'create extension pg_trgm'
CREATE EXTENSION
```

On peut passer à l’étape suivante, installer les paquets dont on a besoin :

```
apt install puppetdb puppetdb-termini
```

Et on indique à PuppetDB comment se connecter à la base créée précédemment en configurant le fichier /etc/puppetlabs/puppetdb/conf.d/database.ini :

```ini
[database]
subname = //localhost:5432/puppetdb
username = puppetdb
password = secret
```

On indique au serveur puppet comment se connecter à puppetdb dans le fichier /etc/puppetlabs/puppet/puppetdb.conf :

```ini
[main]
server_urls = https://puppet.lab.morot.fr:8081
```

On indique ensuite au serveur comment router les requêtes du plugin puppetdb. Cela se fait dans le fichier /etc/puppetlabs/puppet/routes.yaml qui est absent par défaut.

```yaml
---
master:
  facts:
    terminus: puppetdb
    cache: yaml
```

Et enfin, on lui dit qu'il doit y stocker les facts et les catalogues.

Dans le fichier /etc/puppetlabs/puppet/puppet.conf à la section [master], il suffit d'ajouter ces deux lignes :

```ini
storeconfigs = true
storeconfigs_backend = puppetdb
```

Un redémarrage des services plus tard et nous avons finalisé l'installation des différentes briques.

```
systemctl enable puppetdb.service
systemctl restart puppetdb.service
systemctl restart puppetserver.service
```

La documentation Puppet explicite davantage le dimensionnement et le paramétrage pour gérer une plateforme plus fortement chargée. S'il n'y a pas d'erreur dans les étapes précédentes, l'agent puppet doit s'exécuter correctement sur vos nodes.

### 3.3 Mise en œuvre
Afin d'illustrer un cas pratique, nous allons gérer un début de configuration de Nagios avec Puppet. Vous trouverez toutefois d'autres exemples similaires de mise en œuvre de PuppetDB dans la documentation d'Icinga pour rester dans la supervision ou bien du côté du module Puppet de Bacula pour un exemple orienté sauvegarde. Nagios étant le logiciel le plus connu, je vais conserver cet exemple.

Les ressources Nagios ne faisant plus partie des ressources standard, il faut tout d’abord installer le module approprié sur le serveur.

```
puppet module install puppetlabs-nagios_core
```

Pour déclarer une ressource exportée sur un node, il faut simplement préfixer le nom de la ressource d’un double arobase devant le type de ressource à déclarer. L’utilisation de la ressource ne change pas et les fonctionnalités de Puppet comme les facts sont toujours disponibles. Afin que cet exemple soit réutilisable, il conviendrait d’associer l’export des ressources dans une classe dédiée.

```
node agent.lab.morot.fr {
    @@nagios_host { $::fqdn :
        ensure              => present,
        address             => $::ipaddress,
        alias               => $::fqdn,
        use                 => 'generic-host',
        target              => "/etc/nagios3/conf.d/${::fqdn}.cfg",
        mode                => "0644"
    }
 
     @@nagios_service { "${::fqdn}_check-ssh":
        ensure              => present,
        use                 => 'generic-service',
        host_name           => $::fqdn,
        service_description => 'SSH Port',
        check_command       => 'check_ssh!',
        target              => "/etc/nagios3/conf.d/${::fqdn}.cfg",
        mode                => "0644"
    }
}
```

La seconde étape pour déclarer les ressources consiste à procéder à la collecte des ressources en base. La syntaxe <<||>> fait donc office de collecteur pour les ressources exportées dans la base PuppetDB. Il faut donc bien avoir en tête que tant que l’agent ne s’est pas exécuté sur le node et donc qu’aucun catalogue n’a été compilé et stocké en base par le master, les ressources ne sont donc pas exploitables par l’agent qui les consomme. Cela vaut aussi pour les attributs des ressources. Si dans le paramètre target vous changez le chemin, il faudra donc que la ressource soit de nouveau exportée via la compilation d’un nouveau catalogue.

Le manifest ci-dessous installe Nagios en appelant les services et les hôtes exportées précédemment. Au prochain passage de l’agent, Puppet va créer un fichier par host et par service sur le serveur nagios pour chaque ressource exportée dans la base de données PuppetDB.

```
node nagserver {
  Package { 'nagios3': ensure => 'present' }
 
  Service { 'nagios3':
    ensure => running,
    enable => true,
  }
 
  Nagios_host <<||>> {
  }
 
  Nagios_service <<||>> {
  }
}
```

## 4. Dissocier le code et le paramétrage avec HIERA
### 4.1 Le problème du paramétrage des modules et des classes
Dans un précédent article, nous avons vu comment créer un module. Rapidement se pose la problématique de devoir gérer des spécificités liées à un environnement, une distribution, un hôte spécifiquement placé en DMZ, etc.. Historiquement, le mode de conception le plus courant pour un module à usage générique consiste à utiliser un manifest params.pp au sein de chaque module incluant la configuration du module de manière centralisée.

```
class apache::params {
   $port = 80
   case $::osfamily {
      'Debian': {
         $package           = 'apache2'
         $service           = 'apache2'
      }
      'RedHat': {
         $package           = 'httpd'
         $service           = 'httpd''
      }
      default: {
         fail "OS non supporté : ${::osfamily}"
      }
   }
}
```

Ainsi, les classes qui sont ensuite affectées au node héritent de ce manifest, chaque variable est accessible au sein du module sous la forme $module::params::variable.

```
class apache (
   $port            = $apache::params::confdir,
) inherits apache::params {
   [...]
}
```

Cela permet ensuite d'indiquer la configuration spécifique au site dans la configuration des nodes via un passage de paramètre tout ce qu'il y a de plus classique :

```
node 'www1' {
   class {'apache': port => "8080" }
}
```

Bien que ce motif de conception ne soit pas en soi horrible et qu'il soit largement adopté en particulier par bon nombre de modules sur la forge Puppet, il ne s'agit plus de la manière de coder un manifest recommandé, et ce pour plusieurs raisons. En effet, cette syntaxe mélange allègrement code Puppet et configuration, ce qui nuit fortement à la lisibilité et à la maintenabilité de l'environnement de configuration. Cela disperse également le paramétrage à plusieurs niveaux du code.

### 4.2 Mise en œuvre
Hiera est le composant de Puppet qui tente d'apporter une réponse à ces problèmes. C'est une source de configuration textuelle et hiérarchisée, principalement utilisée avec son backend YAML. La hiérarchie Hiera est construite dans un premier fichier hiera.yaml à la racine de chaque environnement Puppet. Pour être honnête, il existe un niveau supérieur, dit global, mais dont l’utilisation est déconseillée. Nous avons donc pour la production le fichier /etc/puppetlabs/code/environments/production/hiera.yaml qui contient typiquement :

```yaml
---version: 5
hierarchy:
   - name: "Per-node data"
     path: "nodes/%{trusted.certname}.yaml"
 
   - name: "Per-OS defaults"
     path: "os/%{facts.os.family}.yaml"
 
   - name: "Common data"
     path: "common.yaml"
 
defaults:
   data_hash: yaml_data
   datadir: data
```

Le fichier hiera.yaml est traité de haut en bas. Les paramètres spécifiques à un node sont donc prioritaires à la configuration commune. Cela sous-tend une logique de simplification consistant à ne paramétrer que les exceptions plus la priorité est élevée. Par défaut, la configuration moins prioritaire est appliquée. Par exemple, si nous devons définir des serveurs DNS et une passerelle SMTP internes utilisés par l'ensemble de notre infrastructure, nous la définirons dans le fichier common.yaml. Mais si nous avons également un serveur en DMZ qui doit être le seul configuré différemment, il utilisera une configuration spécifique au node. Dès lors que notre module sait interroger Hiera, nous n'avons pas besoin de modifier nos manifests pour gérer cette différence. La section defaults est optionnelle dans cet exemple, car les valeurs par défaut sont utilisées. Les fichiers YAML de notre hiérarchie HIERA sont donc recherchés dans le répertoire ou un sous-répertoire de /etc/puppetlabs/code/environments/production/data/. Reprenons l'exemple de nos passerelles SMTP, notre fichier common.yaml contiendrait ceci :

```yaml
---
mta::smtp_relay: mta.int.morot.fr
network::dns_servers:
   - 1.2.3.4
   - 5.6.7.8
```

et un fichier nodes/www1.int.morot.fr.yaml contenant la configuration dédiée au node www1 :

```yaml
---
mta::smtp_relay: mta-out.int.morot.fr
network::dns_servers:
   - 10.20.30.40
   - 50.60.70.80
```

Vérifions donc que Puppet sait résoudre ces valeurs depuis Hiera :

```
# puppet lookup smtp_relay
--- mta.int.morot.fr
# puppet lookup --node www1.int.morot.fr
smtp_relay
--- mta-out.int.morot.fr
```

On récupère effectivement la valeur par défaut ainsi que la valeur spécifique à notre node www1. Rien ne s'oppose à réutiliser ceci dans nos manifests désormais. Si Puppet ne résout pas un paramètre, il est pertinent de lui adjoindre le paramètre --explain qui va fournir une sortie très détaillée sur sa méthodologie de résolution.

### 4.3 Rechercher une valeur depuis un manifest
Pour chercher et exploiter une valeur définie dans Hiera, nous disposons pour nos manifests de la fonction lookup(). Celle-ci comporte pour ainsi dire quatre arguments (le cinquième est une autre façon d’écrire les quatre autres) :

- le nom de la variable ;
- le type de donnée qui sera retournée, par défaut Data, soit un nombre ou une chaîne, mais cela peut être tout type de données permis par le langage comme un Enum, un Boolean, un Array[String] pour un tableau de chaînes comme notre variable network::dns_servers, etc. ;
- la façon de fusionner des valeurs multiples. Hiera va du plus granulaire au plus générique. Si on ne spécifie rien (undef ou first), c’est la valeur la plus granulaire pour le node qui est retournée. Cependant, Hiera peut se comporter différemment en fusionnant les valeurs trouvées dans la hiérarchie ;
- la valeur par défaut qui est retournée si on ne trouve rien dans Hiera.

Voyons ce que cela donne en récupérant la variable comportant les adresses de nos serveurs DNS dans un manifest ainsi que la valeur de la variable ntp::servers et si on ne la trouve pas, on affecte comme valeur par défaut fr.pool.ntp.org :

```
node 'www1' {
   $dns = lookup('network::dns_servers', undef, first, '')
   notify{$dns:}
   $ntp = lookup('ntp::servers', undef, first, 'fr.pool.ntp.org')
   notify{$ntp:}
}
```

À l'exécution, on retrouve bien nos valeurs spécifiques au node et la valeur fallback pour notre NTP :

```
root@www1:~# puppet agent -t
Info: Using configured environment 'production'
[...]
Notice: 10.20.30.40
Notice: /Stage[main]/Main/Node[www1]/Notify[10.20.30.40]/message: defined 'message' as '10.20.30.40'
Notice: 50.60.70.80
Notice: /Stage[main]/Main/Node[www1]/Notify[50.60.70.80]/message: defined 'message' as '50.60.70.80'
Notice: fr.pool.ntp.org
Notice: /Stage[main]/Main/Node[www1]/Notify[fr.pool.ntp.org]/message: defined 'message' as 'fr.pool.ntp.org'
Notice: Applied catalog in 1.70 seconds
```

### 4.4 Paramètres de classes automatiques
Attardons-nous un instant sur la notation mta::smtp_relay. On pourrait tout à fait déclarer une variable sans le préfixe nom::. Il s'agit en fait de l'indication d'une notation pour classe::paramètre. Le couple Puppet Hiera va en pratique tenter de résoudre automatiquement la valeur des paramètres de classes en cherchant une variable classe::nom et par extension, module::classe:paramètre. Si nous avons une classe qui configure un relais SMTP de cette façon :

```
class mta(String $smtp_relay) {
}
```

Alors Puppet va automatiquement chercher dans Hiera une variable mta::smtp_relay, ce qui correspond à la variable que nous avons définie. Il n’y a donc plus à passer les paramètres de classes dans les manifests.

Reprenons maintenant le problème exposé au départ à propos de nos modules et du problème du params.pp. Hiera est comme je le disais une base hiérarchique. Nous pouvons ainsi ajouter une nouvelle couche Hiera à nos modules et placer à la racine de notre module un nouveau fichier hiera.yaml proche de ce que nous avons vu au niveau environnement.

```yaml
---
version: 5
 
defaults:
   data_hash: yaml_data
   datadir: data
hierarchy:
   - name: "OS values"
     path: "os/%{facts.os.name}.yaml"
   - name: "Common values"
     path: "common.yaml"
defaults:
   data_hash: yaml_data
   datadir: data
```

Cela permet ainsi de remplacer le pattern params.pp pour utiliser la logique Hiera à tous les niveaux de notre base de code. La logique qui prévaut ici étant de ne pas faire une hiérarchie spécifique à un node, mais plutôt basée sur les facts afin de ne pas se disperser.

Seule limitation, Hiera n'est pas destinée à être appelée depuis des templates Puppet. Pour le reste, son exploitation reste particulièrement simple et limpide.

## 5. Rôles et profils
À mesure que le nombre de nodes augmente et que la base de code Puppet grossit, commence à apparaître des difficultés croissantes à gérer l'assignation des classes aux nodes.

Dans la plupart des infrastructures, un certain nombre de paramétrages sont communs à l'ensemble des machines (sécurité, authentification, supervision, etc.) et sont ensuite ajoutées les classes qui spécialisent le serveur. Si vous n'avez pas d'external node classifier (ENC), votre manifest ressemble à ceci :

```
node 'www1' {
    include system::base
    include system::authentication
    include system::ntp
    include security::linux
    include monitoring::nrpe
    include ruby::install
    include ruby::railsrequire
    include ruby::rails
    include nginx::server
    include nginx::modpassenger
    include database::mysql
}
```

Bref, l'assignation de ces classes est autant d'éléments dupliqués, ce que l'on ne souhaite pas voir pour d'évidentes raisons de lisibilité et d'efficacité. Pour autant, on souhaite pouvoir gérer simplement des différences de configurations comme la verbosité des logs selon qu'il s'agisse d'un environnement en production ou de développement.

C'est ainsi qu'est apparu le modèle de conception des rôles et des profils qui ajoutent deux niveaux d'abstraction pour faciliter la maintenance de vos manifests. Pour chaque node, on assigne un unique rôle, rien de plus. Le nommage du rôle doit avoir une signification fonctionnelle. Si on était en langage naturel, ce serait comme « Je veux un serveur pour héberger des applications Ruby On Rails ».

Au second niveau d'abstraction, le node contient au moins un profil et plus généralement plusieurs profils qui vont spécifier le type de serveur. C'est à ce niveau qu'est défini la stack technique qui caractérise le rôle du serveur.

Schématiquement, cela donne cette hiérarchie : Node -> Rôle -> Profils -> Modules -> Classes -> Ressources.

Le mécanisme d'abstraction est simplement fourni par les classes, il ne s'agit donc pas d'une syntaxe Puppet dédiée, mais d’une utilisation élégante de celle-ci. Cela permet donc d'utiliser l'héritage de classes, Hiera, le passage de paramètres qui existe déjà dans la syntaxe des manifests. Nous allons donc créer un rôle serveur rubyonrails et affecter notre node à ce rôle. Il n'y a aucun paramètre de classe passé à ce niveau et le serveur n'est affecté qu'à un seul rôle :

```
class roles::rubyonrails {
    include profile::standardserver
    include profile::rails
    include profile::nginx
    include profile::mysql
}
 
node 'rails-server' {
    include roles::rubyonrails
}
```

Enfin, on caractérise notre serveur via les différents profils qui sont affectés. L'idée générale étant de rendre les profils réutilisables pour d'autres rôles. Le profil mysql peut par exemple être réutilisé dans un rôle de serveur Apache/PHP/MySQL, évitant les duplications de code.

```
class profile::standardserver {
    include system::base
    include system::authentication
    include system::ntp
    include security::linux
    include monitoring::nrpe
}
 
class profile::rails {
    include ruby::install
    include ruby::railsrequire
    include ruby::rails
}
 
class profile::nginx {
    include nginx::server
    include nginx::modpassenger
    include monitoring::nginx
}
 
class profile::mysql {
    include database::mysql
    include monitoring::mysql
}
```

L'utilisation de la méthode des rôles et des profils demande un peu de réflexion au départ pour gagner en clarté. Le rôle donne une visualisation « métier » immédiate du serveur tout en se basant sur une pile de profils réutilisables et plus facilement maintenables.

## Conclusion
J’ai tenté de vous présenter Puppet à travers cinq concepts afin d’exploiter davantage les possibilités offertes par cet outil de gestion de configuration. Tout en restant simples et élégantes, ces fonctionnalités deviennent souvent nécessaires dès lors que la base de code et le nombre de nœuds grossissent. Pour autant, tout en étant très puissantes, elles restent accessibles au niveau de leur mise en œuvre.

## Références
[1] https://puppet.com/
[2] https://connect.ed-diamond.com/Linux-Pratique/LP-115/Introduction-aux-deploiements-et-configurations-automatises-avec-Puppet
  
source : https://connect.ed-diamond.com/Linux-Pratique/lp-119/tirez-le-meilleur-parti-de-votre-gestionnaire-de-configuration-puppet