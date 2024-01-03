---
title: Surveillez votre système pour prévenir et détecter toute action malveillante
description: 
published: true
date: 2023-04-21T10:40:30.937Z
tags: détection, surveillance, wazuh, hids
editor: markdown
dateCreated: 2023-04-21T10:40:26.303Z
---

# Surveillez votre système pour prévenir et détecter toute action malveillante

> Wazuh est un fork libre du projet OSSEC qui appartient à la famille des HIDS (Host Intrusion Detection System). Ces systèmes de détection d’intrusions vont surveiller les logs applicatifs, les appels système ainsi que le noyau pour tenter de détecter des compromissions de l’OS.
{.is-info}

Un IDS (Intrusion Detection System) est un programme qui s'appuie sur les points d’instrumentation d’un système d’exploitation afin de détecter d’éventuelles actions malveillantes. Pour un HIDS (Host IDS), un point d’instrumentation est une source d'information générée par le système d'exploitation (tentatives de connexion à la console, arrêt ou démarrage de tel ou tel service, etc.) ou par des logiciels tiers (connexions HTTP à un serveur Apache, authentification sur un serveur MySQL, etc.). Sous Linux, on retrouve toutes ces informations dans le répertoire /var/log. En complément, il existe des NIDS (Network IDS) qui sont placés sur le réseau dans le but de traquer les interactions malveillantes entre machines. Wazuh est donc un HIDS proposant de sélectionner parmi toute cette masse d'informations les événements relevant de la sécurité du système.

## 1. Banc de test
Notre environnement de test comporte quatre machines. La première est une machine nommée « cible » occupant l'IP 192.168.45.1. Elle embarque l'agent Wazuh. En détection d'intrusion, on appelle agent le programme installé sur la machine et qui la surveille. Cet agent doit être le plus compact possible pour limiter la surface d'attaque. En général, quand l'agent constate un souci de sécurité, il remonte immédiatement l'information à un manager. La seconde machine est donc le manager en question et elle répond au nom de wazuh-manager sur l'IP 192.168.45.2. Cette machine embarque la partie de Wazuh nommée wazuh-manager qui orchestre les agents distribués sur les machines du réseau. On notera que les managers peuvent être organisés en cluster pour travailler de concert. La troisième machine s'appelle elk pour Elasticsearch Logstash Kibana. Cette machine va organiser et présenter les données remontées par wazuh-manager dans le but de les rendre exploitables par l'utilisateur final. Elle est sur l'IP 192.168.45.3. Enfin, la dernière machine nommée attaquant va nous servir à générer des événements de sécurité sur « cible ». Elle est en 192.168.45.4. Toutes les machines sont en Debian Buster.

## 2. Installation de Wazuh
Toutes les machines embarquant des composants Wazuh doivent suivre des étapes d'installation très similaires. Les manipulations qui suivent sont donc à réaliser sur cible et wazuh-manager. Nous allons ici présenter les manipulations sur wazuh-manager, mais il faudra faire les mêmes sur cible. Avant tout, il faut installer les paquets nécessaires à l'ajout des dépôts Wazuh :

```
root@wazuh-manager:~# apt-get update
root@wazuh-manager:~# apt-get install curl apt-transport-https lsb-release gnupg2
```

On récupère la clé du dépôt Wazuh et on l'ajoute dans la configuration d'apt :

```
root@wazuh-manager:~# curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | apt-key add -
```

Ensuite, on ajoute le dépôt Wazuh dans la configuration d'apt :

```
root@wazuh-manager:~# echo "deb https://packages.wazuh.com/3.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
```

Lors de l'update, on constate que la base de données d'apt intègre maintenant des paquets provenant de chez Wazuh :

```
root@wazuh-manager:~# apt-get update
[...]
Réception de :4 https://packages.wazuh.com/3.x/apt stable InRelease [5 996 B]
Réception de :5 https://packages.wazuh.com/3.x/apt stable/main amd64 Packages [14,1 kB]
```

### 2.1 Installation du manager et de l'agent
Passons maintenant à la partie spécifique à wazuh-manager. Commençons par installer le paquet sur la machine wazuh-manager :

```
root@wazuh-manager:~# apt-get install wazuh-manager
```

Et vérifions que ça tourne :

```
root@wazuh-manager:~# systemctl status wazuh-manager.service
● wazuh-manager.service - Wazuh manager
    Loaded: loaded (/etc/systemd/system/wazuh-manager.service; enabled; vendor pr
    Active: active (running) since Mon 2019-08-19 18:04:29 CEST; 1min 11s ago
```

Le service wazuh-manager est fonctionnel, passons à l'agent. Installons le paquet sur la machine cible :

```
root@cible:~# apt-get install wazuh-agent
```

L'étape suivante est d'enregistrer l'agent auprès du manager. Ils vont communiquer avec une clé symétrique, c'est-à-dire qu'ils vont se partager un mot de passe connu que d'eux.

### 2.2 Enregistrement de l'agent
Sur le manager, il nous faut ajouter l'agent de cible dans la base de données des agents déployés. Nous allons utiliser la commande manage_agents qui se trouve dans le répertoire /var/ossec/bin de la machine wazuh-manager.

```
root@wazuh-manager:/var/ossec/bin# cd /var/ossec/bin
root@wazuh-manager:/var/ossec/bin# ./manage_agents -n cible -a 192.168.45.1
```

Cette commande ajoute un agent nommé cible (argument -n) sur une machine possédant l'adresse IP 192.168.45.1. Dans notre exemple, il est en effet plus simple d'avoir un nom d'agent identique à celui de la machine où il est installé. Vérifions que la machine est bien enregistrée :

```
root@wazuh-manager:/var/ossec/bin# ./manage_agents -l
Available agents:
    ID: 001, Name: cible, IP: 192.168.45.1
```

Notre machine cible s'est vue attribuer l'ID 001. Lors de cette étape, la fameuse clé a été générée. Il faut l'extraire pour la communiquer à l'agent de cible. Cela se fait toujours avec la même commande en utilisant l'argument -e qui extrait la clé d'un agent identifié par l'ID passé en paramètre :

```
root@wazuh-manager:/var/ossec/bin# ./manage_agents -e 001
Agent key information for '001' is:
MDAxIGNpYmxlIDE5Mi4xNjguNDUuMSAyOWI0YWE1ZTFlOGUzMDA2NWRjMDkxNDUwYmQxM2ZmMzgyZjU0NDg5OGUwZjg2M2I0ZjU4ZjdkNjQ0M2U3Mzgw
```

Il faut donc communiquer cette clé à l'agent de cible. Toujours avec la commande manage_agents, nous allons l'enregistrer avec l'argument -i auquel on passe la valeur de la clé :

```
root@cible:~# /var/ossec/bin/manage_agents -i MDAxIGNpYmxlIDE5Mi4xNjguNDUuMSAyOWI0YWE1ZTFlOGUzMDA2NWRjMDkxNDUwYmQxM2ZmMzgyZjU0NDg5OGUwZjg2M2I0ZjU4ZjdkNjQ0M2U3Mzgw
Agent information:
    ID:001
    Name:cible
    IP Address:192.168.45.1
Confirm adding it?(y/n): y
Added.
```

### 2.3 Démarrage de l'agent
Une fois l'agent configuré, démarrons-le :

```
root@cible:~# systemctl enable wazuh-agent.service
root@cible:~# systemctl start wazuh-agent.service
Arrêtons-nous un instant sur l'affichage du statut :
root@cible:~# systemctl status wazuh-agent.service
● wazuh-agent.service - Wazuh agent
   Loaded: loaded (/etc/systemd/system/wazuh-agent.service; enabled; vendor pres
   Active: active (running) since Tue 2019-08-20 01:46:33 CEST; 7s ago
   [...]
   CGroup: /system.slice/wazuh-agent.service
        ├─2848 /var/ossec/bin/ossec-execd
        ├─2853 /var/ossec/bin/ossec-agentd
        ├─2861 /var/ossec/bin/ossec-syscheckd
        ├─2866 /var/ossec/bin/ossec-logcollector
        └─2875 /var/ossec/bin/wazuh-modulesd
```

Nous constatons qu'il y a tout un tas de services démarrés notamment ossec-syscheckd et ossec-logcollector. En gros, le premier est chargé de surveiller l'intégrité du système (fichiers modifiés, rootkits) et même de réaliser un audit des configurations du système (avec de très bonnes suggestions de remédiations). Le second analyse en continu le répertoire /var/log à la recherche de remontées d'informations suspectes de la part des applications.

Testons maintenant la bonne communication de l'agent avec le manager. Nous allons utiliser la commande agent_control depuis wazuh-manager pour lister les agents qu'il supervise :

```
root@wazuh-manager:/var/ossec/bin# ./agent_control -lc
Wazuh agent_control. List of available agents:
    ID: 000, Name: wazuh-manager (server), IP: 127.0.0.1, Active/Local
    ID: 001, Name: cible, IP: 192.168.45.1, Active
```

Nous allons maintenant nous placer sur attaquant et faire des connexions sur le serveur SSH de cible avec un compte utilisateur inexistant :

```
root@attaquant:~# ssh malmsteen@192.168.45.1
```

Nous entrons trois fois un mot de passe bidon jusqu’à fermeture de la connexion par le serveur SSH de cible. Voyons si nous avons une trace dans les logs du manager (les logs se trouvent dans le fichier /var/ossec/logs/alerts/alerts.log) :

```
root@wazuh-manager:/var/ossec/bin# cat /var/ossec/logs/alerts/alerts.log
[...]
** Alert 1566258917.123626: - syslog,sshd,invalid_login,authentication_failed,pci_dss_10.2.4,pci_dss_10.2.5,pci_dss_10.6.1,gpg13_7.1,gdpr_IV_35.7.d,gdpr_IV_32.2,
2019 Aug 20 01:55:17 (cible) 192.168.45.1->/var/log/auth.log
Rule: 5710 (level 5) -> 'sshd: Attempt to login using a non-existent user'
Src IP: 192.168.45.4
Aug 20 01:55:17 cible sshd[3960]: Failed password for invalid user malmsteen from 192.168.45.4 port 50738 ssh2
```

Nous y trouvons bien une trace de connexion d'un utilisateur inexistant nommé malmsteen. Arrivé ici nous avons validé le bon fonctionnement de l'agent et du manager. La prochaine étape est d'envoyer toutes ces informations dans une plateforme de visualisation.

## 3. Raccordement à ELK
ELK est une pile composée de trois logiciels : Elasticsearch, Logstash et Kibana. Dans notre article, nous allons laisser de côté Logstash et n'utiliser que Elasticsearch et Kibana. Pour faire très simple, Elasticsearch est une base de données extrêmement rapide pour l'indexation (et donc la recherche). L'idée va être d'envoyer tous les logs du manager dans la base de données Elasticsearch. Le programme situé sur wazuh-manager qui va lire les logs et les envoyer à l'instance d'Elasticsearch installée sur elk se nomme Filebeat.

### 3.1 Configuration de Filebeat sur « wazuh-manager »
Nous allons l'installer en ajoutant un dépôt apt ainsi que sa clé. D'abord la clé :

```
root@wazuh-manager:~# curl -s https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
OK
```

Puis le dépôt :

```
root@wazuh-manager:~# echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list
```

Nous validons que les dépôts d'elastic.co sont bien présents :

```
root@wazuh-manager:~# apt-get update
Réception de :1 https://artifacts.elastic.co/packages/7.x/apt stable InRelease [7 124 B]
Atteint :2 http://security.debian.org/debian-security buster/updates InRelease
Réception de :3 https://artifacts.elastic.co/packages/7.x/apt stable/main amd64 Packages [13,2 kB]
[...]
```

Installons Filebeat :

```
root@wazuh-manager:~# apt-get install filebeat=7.3.0
```

Puis descendons un fichier de configuration de base pour Filebeat. Le seul élément à changer sera l'adresse de la base Elasticsearch. Ce fichier est disponible sur le git de elastic.co :

```
root@wazuh-manager:~# curl -so /etc/filebeat/filebeat.yml https://raw.githubusercontent.com/wazuh/wazuh/v3.9.5/extensions/filebeat/7.x/filebeat.yml
```

L'étape suivante est de récupérer un fichier de configuration pour Filebeat. En effet, Filebeat doit avoir connaissance de la structure des logs de Wazuh pour les envoyer à la base Elasticsearch. Ce fichier « template » est disponible sur le git de elastic.co :

```
root@wazuh-manager:~# curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v3.9.5/extensions/elasticsearch/7.x/wazuh-template.json
```

Ensuite, ajoutons le module Wazuh pour Filebeat qui va s'appuyer sur ces deux fichiers pour envoyer les logs formatés à la base Elasticsearch :

```
root@wazuh-manager:~# curl -s https://packages.wazuh.com/3.x/filebeat/wazuh-filebeat-0.1.tar.gz | tar -xvz -C /usr/share/filebeat/module
```

Enfin, nous n'avons plus qu'à indiquer à Filbeat la localisation de la base Elasticsearch dans son fichier de configuration /etc/filebeat/filebeat.yml :

```
root@wazuh-manager:~# cat /etc/filebeat/filebeat.yml
[...]
output.elasticsearch.hosts: ['http://192.168.45.3:9200']
```

On redémarre le tout :

```
root@wazuh-manager:~# systemctl enable filebeat
root@wazuh-manager:~# systemctl start filebeat
```

Et on vérifie :

```
root@wazuh-manager:~# systemctl status filebeat
● filebeat.service - Filebeat sends log files to Logstash or directly to Elastic
    Loaded: loaded (/lib/systemd/system/filebeat.service; enabled; vendor preset:
    Active: active (running) since Tue 2019-08-20 02:23:49 CEST; 30s ago
```

Passons à l'installation de la couche ELK.

### 3.2 Installation de Elasticsearch sur « elk »
À l'instar de wazuh-manager, il faut installer les dépôts de elastic.co. Je passe vite là-dessus :

```
root@elk:~# apt-get install curl apt-transport-https lsb-release gnupg2
root@elk:~# curl -s https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add -
OK
root@elk:~# echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list
deb https://artifacts.elastic.co/packages/7.x/apt stable main
root@elk:~# apt-get update
```

Passons à l'installation de Elasticsearch :

```
root@elk:~# apt-get install elasticsearch=7.3.0
```

Activons-le au démarrage de la machine elk :

```
root@elk:~# systemctl enable elasticsearch.service
```

On ne le démarre pas de suite, car il y a quelques éléments à configurer dans le fichier /etc/elasticsearch/elasticsearch.yml :

```yaml
root@elk:~# cat /etc/elasticsearch/elasticsearch.yml
node.name: elk.lab.secusys.local
network.host: 0.0.0.0
http.port: 9200
cluster.initial_master_nodes: ["elk.lab.secusys.local"]
```

Le premier paramètre node.name est le nom de ce nœud Elasticsearch. En effet, Elasticsearch est une base de données fortement distribuée, chaque participant (ou nœud) est identifié par un nom. Ici, il s'agit du nom pleinement qualifié de la machine elk, soit elk.lab.secusys.local. Le second paramètre network.host indique sur quelle(s) interface(s) Elasticsearch doit écouter les requêtes des clients type Filebeat. Ici, nous avons mis 0.0.0.0 pour dire « toutes les interfaces ». S'en suit http.port qui définit le port sur lequel écouter. Enfin, cluster.initial_master_nodes donne la liste des nœuds habilités à être élus maîtres sur l'infrastructure distribuée Elasticsearch. Démarrons le service :

```
root@elk:~# systemctl start elasticsearch.service
```

Vérifions son bon fonctionnement :

```
root@elk:~# systemctl status elasticsearch.service
● elasticsearch.service - Elasticsearch
    Loaded: loaded (/lib/systemd/system/elasticsearch.service; enabled; vendor pr
    Active: active (running) since Tue 2019-08-20 02:32:19 CEST; 7s ago
```

Vérifions également qu'il écoute bien sur le port 9200 :

```
root@elk:~# netstat -laputn
Connexions Internet actives (serveurs et établies)
Proto Recv-Q Send-Q Adresse locale      Adresse distante Etat PID/Program name

[...]

tcp6     0 0  192.168.45.3:9200      192.168.45.2:48946 ESTABLISHED
```

Nous constatons que ça communique entre wazuh-manager (192.168.45.2) et elk (192.168.45.3). Repassons sur wazuh-manager pour vérifier que Filebeat arrive bien à s'accrocher à Elasticsearch :

```
root@wazuh-manager:~# filebeat setup --index-management -E setup.template.json.enabled=false
ILM policy and write alias loading not enabled.
Index setup finished.
```

C'est tout bon ! Passons à Kibana.

### 3.3 Installation de Kibana
Kibana est un service web de visualisation du contenu de la base Elasticsearch. Il lui faut deux choses pour fonctionner : la localisation de la base Elasticsearch et la façon de présenter les données remontées par Wazuh. Installer Kibana sur elk :

```
root@elk:~# apt-get install kibana=7.3.0
```

Commençons par le plugin Wazuh pour Kibana :

```
root@elk:~# sudo -u kibana /usr/share/kibana/bin/kibana-plugin install https://packages.wazuh.com/wazuhapp/wazuhapp-3.9.5_7.3.0.zip
```

Ensuite configurons Kibana pour que 1) il écoute sur toutes les interfaces réseaux de la machine elk et que 2) il trouve l'instance d'Elasticsearch :

```
root@elk:~# cat /etc/kibana/kibana.yml
server.host: "0.0.0.0"
elasticsearch.hosts: ["http://192.168.45.3:9200"]
```

Le premier paramètre server.host définit sur quelle(s) interface(s) Kibana doit écouter. Ici, on écoute sur toutes (0.0.0.0). Le second elasticsearch.hosts donne l’emplacement réseau du service Elasticsearch. Activons et démarrons Kibana :

```
root@elk:~# systemctl enable kibana.service
root@elk:~# systemctl start kibana.service
```

Vérifions :

```
root@elk:~# systemctl status kibana.service
● kibana.service - Kibana
    Loaded: loaded (/etc/systemd/system/kibana.service; enabled; vendor preset: e
    Active: active (running) since Tue 2019-08-20 03:06:24 CEST; 28s ago
```

Nous pouvons maintenant nous connecter sur l'interface Kibana qui répond sur http://192.168.45.3:5601. Aucun identifiant/mot de passe ne nous est demandé. Nous laissons ça en l'état, car configurer la sécurité de l'accès à Kibana pourrait faire l'objet d'un article entier. Une fois sur l'interface, nous cliquons sur l'élément nommé Discover (Figure 1 en haut à gauche dans le carré rouge). Nous choisissons l'item wazuh-alerts-3.x-* dans le menu déroulant juste à côté. Nous pouvons visualiser les informations brutes remontées par Filebeat (ici une tentative de connexion SSH d'un utilisateur « tipiak » inexistant).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84470/kibana-alertes1.png">

*Fig. 1 : Alertes brutes Wazuh.*

Dans le menu déroulant, un second item wazuh-monitoring-3.x-* est disponible. Nous y trouvons les informations à propos des agents déployés sur le réseau (Figure 2). Le lecteur au regard affûté aura remarqué que l'IP vue pour « cible » est 10.0.2.15, cela est dû au fait que la plateforme de test est en VM et que chaque machine est doublement attachée avec une interface NAT sur le 10.0.2.0/24 (pour sortir sur le net) et une interface sur un réseau interne à l'hyperviseur en 192.168.45.0/24 (sinon les machines ne se verraient pas).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84470/kibana-monitoring1.png">

*Fig. 2 : Informations brutes de monitoring des agents Wazuh.*

Ces deux sources d'informations sont complètes, mais pas vraiment exploitables. Ce qui nous amène à la dernière partie de l'article, à savoir la configuration de l'API Wazuh. Cette partie nous permettra de tirer le meilleur de l'intégration Wazuh dans ELK.

## 4. Configuration de l'API Wazuh
L'API Wazuh est une interface de communication permettant à des programmes tiers d'interroger le service wazuh-manager. Kibana va tirer parti de cette API pour nous permettre de piloter l'infrastructure Wazuh facilement.

Retournons sur wazuh-manager pour configurer l'accès à cette API implémentée en Node.js. Ajoutons donc le dépôt Node.js pour Debian :

```
root@wazuh-manager:~# curl -sL https://deb.nodesource.com/setup_8.x | bash -
```

Et installons le service Node.js ainsi que l'API Wazuh :

```
root@wazuh-manager:~# apt-get install nodejs wazuh-api
```

Comme toujours, vérifions :

```
root@wazuh-manager:~# systemctl status wazuh-api.service
● wazuh-api.service - Wazuh API daemon
    Loaded: loaded (/etc/systemd/system/wazuh-api.service; enabled; vendor preset
    Active: active (running) since Tue 2019-08-20 05:03:20 CEST; 1min 35s ago
```

À partir de cet instant, l'API est accessible par tout le monde et de partout. Ce n'est pas très sérieux pour un HIDS. Nous allons ajouter un couple identifiant/mot de passe pour accéder à l'API. Il faut se rendre dans le répertoire /var/ossec/api/configuration/auth/ pour y configurer un htaccess. Le htaccess est un fichier traditionnellement associé au serveur web demandant une authentification lors de l'accès à certaines URL protégées. Ici, l'URL en question est le point d'entrée de l'API. La génération du fichier se fait via le package htpasswd de la commande node :

```
root@wazuh-manager:~# cd /var/ossec/api/configuration/auth/
root@wazuh-manager:/var/ossec/api/configuration/auth# node htpasswd -c user waz
New password:
Re-type new password:
Adding password for user waz.
```

Nous avons créé un utilisateur « waz » avec comme mot de passe « ossec ». Nous pouvons maintenant redémarrer Elasticsearch et Kibana :

```
root@elk:~# systemctl restart elasticsearch.service
root@elk:~# systemctl restart kibana.service
```

Retournons dans l'interface web Kibana pour cliquer sur la petite frimousse encadrée à gauche dans la Figure 3. Vous allez arriver sur un écran vous demandant de renseigner un accès à l'API Wazuh. Vous pouvez donner les paramètres configurés précédemment à savoir : User « waz », Password « ossec », URL « http://192.168.45.2 » et le port « 55000 ». Une fois ces paramètres rentrés, vous pouvez cliquer sur Overview en haut à gauche et avoir accès à tout un tas de statistiques bien plus exploitables que les données brutes de la section précédente (Figure 3).

<img src="https://connect.ed-diamond.com/sites/default/files/articles/linux-pratique/lphs-046/84470/kibana-overview1.png">

*Fig. 3 : Vue générale des remontées de Wazuh.*

Toutefois, si une fois arrivé à cette étape Kibana se plaint d'index défaillants, vous pouvez les réinitialiser avec les commandes suivantes :

```
root@elk:~# curl -XDELETE 192.168.45.3:9200/.wazuh-version
root@elk:~# curl -XDELETE 192.168.45.3:9200/.wazuh
root@elk:~# curl -XDELETE 192.168.45.3:9200/.kibana
```

Et relancer le couple Elasticsearch/Kibana.

Dernier conseil, le démarrage de ces deux services peut prendre pas mal de temps. Je vous conseille de superviser le démarrage de l'un puis de l'autre avec les commandes suivantes. Pour Elasticsearch :

```
root@elk:~# tail -f /var/log/elasticsearch/elasticsearch.log
```

Et pour Kibana :

```
root@elk:~# tail -f /var/log/syslog | grep kibana
```

## Conclusion
Nous avons vu comment mettre en place et surtout exploiter une flotte de HIDS sur les machines composants un réseau. Un axe d'améliorations serait de sécuriser les communications de l'infrastructure ELK en chiffrant et en authentifiant les échanges entre Kibana et Elasticsearch. Un autre point faible est la communication en clair entre l'API Wazuh et Kibana. Enfin, il serait de bon ton d'ajouter une authentification pour l'accès à Kibana (par exemple via un reverse-proxy).


source : https://connect.ed-diamond.com/Linux-Pratique/lphs-046/surveillez-votre-systeme-pour-prevenir-et-detecter-toute-action-malveillante
