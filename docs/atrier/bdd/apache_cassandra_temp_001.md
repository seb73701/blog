---
title: Cassandra
description: 
published: true
date: 2023-04-21T10:51:02.076Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:50:57.868Z
---

====== Cassandra ======
----
[[https://www.guru99.com/cassandra-security.html]]

----
===== Actions =====
===== Java (openJDK) =====

  * Se connecter au serveur
  * Vérifier si le JDK est déjà installé :
<code>rpm -qa | grep -i jdk</code>

  * Installation du JDK Java :
<code>yum install java-1.8.0-openjdk</code>

  * Vérifier l'installation :
<code>java -version</code>

===== Python =====

Python est nécessaire pour l'interpréteur CSQL. Par défaut, Python 2.7 est installé sous les systèmes Linux CentOS.

  * Vérifier si Python est installé :
<code>python --version</code>


===== Ajout Repo Cassandra (Syracuse) =====

  * Contrôle si le repo est déjà installé ():

<code>yum repolist</code>

  * Si non, alors on l'installe :

===== Installation de Cassandra =====

<code>yum install cassandra</code>

===== Configuration de Cassandra (sur chaque noeud) =====

Source : [[https://docs.datastax.com/en/cassandra/3.0/cassandra/initialize/initMultipleDS.html]]

  * Modification des fichiers de configuration de Cassandra

<code>vim /etc/cassandra/conf/cassandra.yaml</code>

  * Modifier les informations ci-dessous en fonction des informations se trouvant dans la section "Description" :

<code>
cluster_name: 'CassandraCluster'

seed_provider:
    # Addresses of hosts that are deemed contact points.
    # Cassandra nodes use this list of hosts to find each other and learn
    # the topology of the ring.  You must change this if you are running
    # multiple nodes!
    - class_name: org.apache.cassandra.locator.SimpleSeedProvider
      parameters:
          # seeds is actually a comma-delimited list of addresses.
          # Ex: "<ip1>,<ip2>,<ip3>"
          - seeds: "10.20.43.11,10.20.43.12,10.20.43.13,10.20.43.14"

listen_address: 10.20.43.11

rpc_address: 10.25.113.11

endpoint_snitch: GossipingPropertyFileSnitch
</code>

  * Modifier le fichier ''/etc/cassandra/conf/cassandra-rackdc.properties''

<code>vim /etc/cassandra/conf/cassandra-rackdc.properties</code>

  * Modifier les informations ci-dessous en fonction des informations se trouvant dans la section "Description" :

<code>
dc=PAR1
rack=rack1
</code>

  * Renommer le fichier ''/etc/cassandra/conf/cassandra-topology.properties''

<code>mv /etc/cassandra/conf/cassandra-topology.properties /etc/cassandra/conf/cassandra-topology.properties.orig</code>

===== Démarrage du Cluster Cassandra =====

<WRAP center round important 80%><color red>**Attention, il faut absolument démarrer les serveurs déclarés comme "Seed" en premier.**</color></WRAP>

<code>systemctl start cassandra</code>

  * Vérifier que le service est démarré correctement :

<code>systemctl status cassandra</code>

  * Contrôler les logs

<code>tail -f /var/log/cassandra/cassandra.log</code>

  * Contrôler au niveau du cluster Cassandra :

<code>nodetool status</code>

  * Tester la connexion (depuis csight-prd-l-tsdb01) :

<code>
cqlsh 10.25.113.11 -u cassandra -p cassandra
Connected to CassandraCluster at 10.25.113.11:9042.
[cqlsh 5.0.1 | Cassandra 3.11.2 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cassandra@cqlsh>
</code>

===== Changement du mot de passe par défaut du compte "cassandra" =====

  * Se connecter sur l'un des serveurs (Seed)
  * Ouvrir une console CQL
<code>
cqlsh -u cassandra -p cassandra 10.25.113.11 9042
Connected to CassandraCluster at 10.25.113.11:9042.
[cqlsh 5.0.1 | Cassandra 3.11.2 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cassandra@cqlsh>
</code>
  * Changer le mot de passe par défaut de Cassandra :

<code>cassandra@cqlsh> alter user cassandra with password 'new_password';</code>

  * Autoriser le déploiement des compte utilisateurs

<code>ALTER KEYSPACE system_auth WITH replication = {'class': 'NetworkTopologyStrategy', 'PAR1': 3, 'PAR2': 3};</code>

  * Envoyer les informations au cluster

<code>nodetool repair system_auth</code>
===== Création d'un compte administrateur Cassandra =====

Cassandra oblige a créer un nouveau compte administrateur et bloquer le compte "cassandra". Suivez ces étapes pour la mise en place

  * Se connecter sur l'un des serveurs (Seed)
  * Ouvrir une console CQL
<code>
cqlsh 10.25.113.11 -u cassandra
Password :
Connected to CassandraCluster at 10.25.113.11:9042.
[cqlsh 5.0.1 | Cassandra 3.11.2 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cassandra@cqlsh>
</code>
  * Créer le nouveau rôle :

<code>
cassandra@cqlsh> CREATE ROLE cassdba WITH PASSWORD = 'new_password' AND LOGIN = true AND SUPERUSER = true;
</code>

  * On vérifie que le rôle a bien été crée :

<code>
cassandra@cqlsh> LIST ROLES;

 role      | super | login | options
-----------+-------+-------+---------
 cassandra |  True |  True |        {}
   cassdba |  True |  True |        {}

(2 rows)
cassandra@cqlsh> 
</code>

  * On se déconnecte pour se connecté avec le compte ''cassdba'' :
<code>
# cqlsh 10.55.113.11 -u cassdba
Password:
Connected to CassandraCluster at 10.55.113.11:9042.
[cqlsh 5.0.1 | Cassandra 3.11.2 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cassdba@cqlsh>
</code>

  * Modifier le fichier ''/etc/cassandra/conf/cassandra.yaml'' :

<code>vim /etc/cassandra/conf/cassandra.yaml</code>

<code>
authenticator: PasswordAuthenticator

authorizer: CassandraAuthorizer
</code>

  * Redémarrer Cassandra :

<code>systemctl restart cassandra</code>

  * On vérifie :

<code>
cassdba@cqlsh> LIST ROLES;

 role      | super | login | options
-----------+-------+-------+---------
 cassandra | False | False |        {}
   cassdba |  True |  True |        {}

(2 rows)
</code>

===== Création d'un compte utilisateur pour Coresight (métriques) =====

  * Se connecter sur l'un des serveurs (Seed)
  * Ouvrir une console CQL
<code>
cqlsh 10.25.113.11 -u cassdba
Password :
Connected to CassandraCluster at 10.25.113.11:9042.
[cqlsh 5.0.1 | Cassandra 3.11.2 | CQL spec 3.4.4 | Native protocol v4]
Use HELP for help.
cassandra@cqlsh>
</code>
  * Créer le nouvel utilisateur:

<code>
cassdba@cqlsh> CREATE USER coresight WITH PASSWORD 'password' NOSUPERUSER;
</code>
