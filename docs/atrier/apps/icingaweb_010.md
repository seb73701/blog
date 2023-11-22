---
title: Création d'une sonde "check_ping_vpn" pour Linux
description: 
published: true
date: 2023-04-21T10:48:20.210Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:48:15.757Z
---

<html>
<head>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.13.0/css/all.css" integrity="sha384-Bfad6CLCknfcloXFOyFnlgtENryhrpZCe29RTifKEixXQZ38WheV+i/6YWSzkz3V" crossorigin="anonymous">
</head>

[ <i class="fas fa-chevron-circle-left"></i> ](icingaweb2.md)

----

# Création d'une sonde "check_ping_vpn" pour Linux

<span style="color:grey;"><i class="far fa-calendar-alt"></i>&nbsp;&nbsp;2020-08-22</span>

## Introduction

La sonde permet de vérifier la communication entre un serveur et un autre qui se trouve sur un site distant par lequel le flux passe par un tunnel VPN.

La sonde effectue un ping vers l'adresse IP privée du serveur distant et remonte toute anomalie (communication impossible, temps de réponse trop long, perte de paquets).

Elle utilise la **"External Command"** déjà existante et qui pointe vers le fichier binaire "`PluginDir + /check_ping`".

Le fichier binaire possède les arguments suivants :

```
Options:

 -h, --help
    Print detailed help screen
 -V, --version
    Print version information
 --extra-opts=[section][@file]
    Read options from an ini file. See https://www.monitoring-plugins.org/doc/extra-opts.html for usage and examples.
 -4, --use-ipv4
    Use IPv4 connection
 -6, --use-ipv6
    Use IPv6 connection
 -H, --hostname=HOST
    host to ping
 -w, --warning=THRESHOLD
    warning threshold pair
 -c, --critical=THRESHOLD
    critical threshold pair
 -p, --packets=INTEGER
    number of ICMP ECHO packets to send (Default: 5)
 -L, --link
    show HTML in the plugin output (obsoleted by urlize)
 -t, --timeout=INTEGER
    Seconds before connection times out (default: 10)
```

Usage:

```
check_ping -H <host_address> -w <wrta>,<wpl>% -c <crta>,<cpl>% [-p packets] [-t timeout] [-4|-6]
```


Source : https://www.monitoring-plugins.org/doc/man/check_ping.html

## Création des variables

Avant tout, il faut créer les variables qui seront utilisées pour variabilisés notre service.

- Cliquer dans le menu "**Icinga Director**"
- Dans la partie de droite, cliquer sur "**Define Data Fields**" qui se trouve dans la section "**Do more custum data**" 
- Cliquer sur le bouton "**+ Add**"
- Compléter les champs avec les infos ci-dessous

| Field Name | Caption | Description | Data type | Visibility |
| :--------- | :------ | :---------- | :-------: | :--------: |
| **`ping_cpl`** | Critical Packet Loss | critical threshold pair | String | Visible |
| **`ping_crta`** | Critical RTA | critical threshold pair | String | Visible |
| **`ping_address`** | Host | host to ping | String | Visible |
| **`ping_packets`** | Packets | Number of ICMP Echo packets to send (Default : 5) | String | Visible |
| **`ping_timeout`** | Timeout | Seconds before connection times out (default : 10) | String | Visible |
| **`ping_wpl`** | Warning Packet Loss | warning threshold pair | String | Visible |
| **`ping_wrta`** | Warning RTA | warning threshold pair | String | Visible |

## L'objet "CheckCommand" ping (External Command)

La configuration de l'objet "ping" dans "External Command" correspond à ceci :

```json
object CheckCommand "ping" {
    import "plugin-check-command"
    command = [ PluginDir + "/check_ping" ]
    timeout = 1m
    arguments += {
        "-H" = {
            description = "host to ping"
            value = "$ping_address$"
        }
        "-c" = {
            description = "critical threshold pair"
            value = "$ping_crta$,$ping_cpl$%"
        }
        "-p" = {
            description = "number of ICMP ECHO packets to send (Default: 5)"
            value = "$ping_packets$"
        }
        "-t" = {
            description = "Seconds before connection times out (default: 10)"
            value = "$ping_timeout$"
        }
        "-w" = {
            description = "warning threshold pair"
           value = "$ping_wrta$,$ping_wpl$%"
        }
    }
    vars.check_address = {
        arguments = [  ]
        deprecated = false
        name = "<anonymous>"
        side_effect_free = false
        type = "Function"
    }
    vars.check_ipv4 = false
    vars.check_ipv6 = false
    vars.ping_address = "$check_address$"
    vars.ping_cpl = 15
    vars.ping_crta = 200
    vars.ping_wpl = 5
    vars.ping_wrta = 100
}
```

## L'objet : Commands
- Aller dans le menu "**Icinga Director**" > "**Commands**"
- Cliquer sur "**Commands**"
- Cliquer sur "**+ Add**"

```json
object CheckCommand "check_ping_vpn (linux)" {
    import "plugin-check-command"
    command = [ PluginDir + "/check_ping" ]
    arguments += {
        "-H" = {
            description = "host to ping"
            required = true
            value = "$ping_address$"
        }
        "-c" = {
            description = "critical threshold pair"
            required = true
            value = "$ping_crta$,$ping_cpl$%"
        }
        "-p" = {
            description = "number of ICMP ECHO packets to send (Default: 5)"
            value = "$ping_packets$"
        }
        "-t" = {
            description = "Seconds before connection times out (default: 10)"
            value = "$ping_timeout$"
        }
        "-w" = {
            description = "warning threshold pair"
            required = true
            value = "$ping_wrta$,$ping_wpl$%"
        }
    }
}
```

- Cliquer sur le bouton "**Add**"
- Cliquer sur l'onglet "**Arguments**"
- Saisir les informations comme représenté dans le fichier de configuration ci-dessus.


## L'objet : Service Template

- Aller dans le menu "**Icinga Director**" > "**Services**"
- Cliquer sur "**Services Templates**"
- Cliquer sur "**+ Add**"
- Compléter les champs suivants :

| Name | Imports | Check Command | Run on agent | 
| :--- | :------ | :-----------: | :----------: |
| linux-check_ping_vpn-service-template | Default Service Template | check_ping_vpn (linux) | Yes |

```json
template Service "linux-check_ping_vpn-template" {
    import "Default Service Template"

    check_command = "check_ping_vpn (linux)"
}
```

- Enregistrer en cliquant sur le bouton "**Add**"
- Cliquer sur l'onglet "**Fields**"
- Sélectionner dans le champ "**Field**" : **ID Wiki (__wiki_id__)**
- Cliquer sur le bouton "**Add**"

## L'objet : service-apply

- Se connecter au serveur MASTER d'Icinga2
- Se positionner dans le dossier **`/etc/icinga2/zones.d/global-templates/`** :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ cd /etc/icinga2/zones.d/global-templates/
```

- Créer le fichier **`linux-check_ping_vpn-service-apply.conf`** :

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ sudo vim linux-check_ping_vpn-service-apply.conf
```

- Mettre ceci :

```json
// To check VPN by ping command (check_ping)

apply Service "ping_vpn_" for (ping_address => config in host.vars.check_ping_vpn_params) {
    import "linux-check_ping_vpn-service-template"

    display_name = "[PING VPN]" + " " + config["supervised_element"] + " (" + config["ping_address"] + ")"

    assign where "Linux" in host.groups && host.vars.check_ping_vpn_params

    vars.ping_address   = config["ping_address"]
    vars.ping_crta      = config["ping_crta"]
    vars.ping_cpl       = config["ping_cpl"]
    vars.ping_packets   = config["ping_packets"]
    vars.ping_timeout   = config["ping_timeout"]
    vars.ping_wrta      = config["ping_wrta"]
    vars.ping_wpl       = config["ping_wpl"]

    if (config["wiki_id"] == "" || !config["wiki_id"]){
        vars._wiki_id = ""
    } else {
        vars._wiki_id = config["wiki_id"]
    }
}

```

## Vérfication et rechargement du service

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ sudo icinga2 daemon -C
```

<span style="color:darkseagreen;"><i class="fas fa-tv"></i>&nbsp;&nbsp;_Sortie :_</span>

```log
[2020-05-25 15:27:10 +0200] information/cli: Icinga application loader (version: 2.11.3-1)
[2020-05-25 15:27:10 +0200] information/cli: Loading configuration file(s).
[2020-05-25 15:27:11 +0200] information/ConfigItem: Committing config item(s).
[2020-05-25 15:27:11 +0200] information/ApiListener: My API identity: csight-prd-l-master01.host.coreye.net
[2020-05-25 15:27:11 +0200] warning/Zone: The Zone object 'master' has more than two endpoints. Due to a known issue this type of configuration is strongly discouraged and may cause Icinga to use excessive amounts of CPU time.
[2020-05-25 15:27:11 +0200] warning/Zone: The Zone object 'SAT-PAR2' has more than two endpoints. Due to a known issue this type of configuration is strongly discouraged and may cause Icinga to use excessive amounts of CPU time.
[2020-05-25 15:27:11 +0200] warning/Zone: The Zone object 'SAT-PAR1' has more than two endpoints. Due to a known issue this type of configuration is strongly discouraged and may cause Icinga to use excessive amounts of CPU time.
[2020-05-25 15:27:11 +0200] warning/Zone: The Zone object 'SAT-LIL1' has more than two endpoints. Due to a known issue this type of configuration is strongly discouraged and may cause Icinga to use excessive amounts of CPU time.
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'dependency_services_to_host' (in /var/lib/icinga2/api/packages/director/931f0b0a-f228-47b2-a0bf-38f18330e49e/zones.d/director-global/dependency_apply.conf: 1:0-1:56) for type 'Dependency' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'certificate_' (in /etc/icinga2/zones.d/global-templates/linux-certificate-service-apply.conf: 4:1-4:79) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'dns_' (in /etc/icinga2/zones.d/global-templates/linux-check_dns-service-apply.conf: 3:1-3:70) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'mem' (in /etc/icinga2/zones.d/global-templates/linux-check_mem-service-apply.conf: 3:1-3:19) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'memcached_' (in /etc/icinga2/zones.d/global-templates/linux-check_memcached-service-apply.conf: 3:1-3:86) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'mongodb_' (in /etc/icinga2/zones.d/global-templates/linux-check_mongodb-service-apply.conf: 3:1-3:82) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'partition_readonly' (in /etc/icinga2/zones.d/global-templates/linux-check_partition_readonly-service-apply.conf: 3:1-3:34) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'ping_vpn_' (in /etc/icinga2/zones.d/global-templates/linux-check_ping_vpn-service-apply.conf: 3:1-3:89) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'postgres_' (in /etc/icinga2/zones.d/global-templates/linux-check_postgres-service-apply.conf: 3:1-3:84) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'http_' (in /etc/icinga2/zones.d/global-templates/platform-HTTP-service-apply.conf: 1:0-1:75) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'http_' (in /etc/icinga2/zones.d/global-templates/windows-check_http-service-apply.conf: 3:1-3:72) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] warning/ApplyRule: Apply rule 'swap' (in /etc/icinga2/zones.d/global-templates/windows-check_swap-service-apply.conf: 3:1-3:20) for type 'Service' does not match anywhere!
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 10 HostGroups.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 EventCommand.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 FileLogger.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 2 NotificationCommands.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 NotificationComponent.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 14569 Notifications.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 IcingaApplication.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1357 Hosts.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 ApiListener.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 11 Comments.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 CheckerComponent.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1333 Zones.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 ExternalCommandListener.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1339 Endpoints.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 4 ApiUsers.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 OpenTsdbWriter.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 IdoMysqlConnection.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 254 CheckCommands.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 5 TimePeriods.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 1 User.
[2020-05-25 15:27:17 +0200] information/ConfigItem: Instantiated 13212 Services.
[2020-05-25 15:27:21 +0200] information/WorkQueue: #4 (DaemonUtility::LoadConfigFiles) items: 0, rate: 28.8/s (1728/min 1728/5min 1728/15min);
[2020-05-25 15:27:21 +0200] information/WorkQueue: #6 (ApiListener, SyncQueue) items: 0, rate:  0/s (0/min 0/5min 0/15min);
[2020-05-25 15:27:21 +0200] information/WorkQueue: #5 (ApiListener, RelayQueue) items: 0, rate:  0/s (0/min 0/5min 0/15min);
[2020-05-25 15:27:21 +0200] information/WorkQueue: #7 (IdoMysqlConnection, ido-mysql) items: 0, rate:  0/s (0/min 0/5min 0/15min);
[2020-05-25 15:27:24 +0200] information/ScriptGlobal: Dumping variables to file '/var/cache/icinga2/icinga2.vars'
[2020-05-25 15:27:24 +0200] information/cli: Finished validating the configuration file(s).

```

<span style="color:coral;"><i class="fas fa-laptop-code"></i>&nbsp;&nbsp;_Shell :_</span>

```shell
$ sudo systemctl reload icinga2
```

## L'objet : Host

Déclaration dans le fichier `hosts.conf` :

```json
vars.check_ping_vpn_params = {
    "scl" = {
        ping_address = "10.30.112.1"
        ping_crta = "200"
        ping_cpl = "15"
        ping_packets = "5"
        ping_timeout = "10"
        ping_wrta = "100"
        ping_wpl = "5"
        supervised_element = "scl1"
    }
}
```
