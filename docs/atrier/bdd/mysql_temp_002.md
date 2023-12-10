---
title: Planifier une tâche automatisée dans MySQL
description: 
published: true
date: 2023-04-21T10:52:25.613Z
tags: beta, bdd, mysql, sgdbr, event
editor: markdown
dateCreated: 2023-04-21T10:52:20.505Z
---

# Explications

Voir ces liens : 

+ [Working with MySQL Scheduled Event](https://www.mysqltutorial.org/mysql-triggers/working-mysql-scheduled-event/)
+ [Maintaining MySQL Database Tables](https://www.mysqltutorial.org/mysql-database-table-maintenance-statements.aspx)
+ [Le planificateur d’évènements](https://www.editions-eni.fr/open/mediabook.aspx?idR=f1ce5d67fcf2e2488c7410cf8de54568)
+ [Créer une tâche planifiée sur MySQL](https://quillevere.net/programmation/bdd/mysql/creer-tache-planifiee-mysql_49743.htm#:~:text=Il%20est%20possible%20sur%20MySQL,et%20la%20requ%C3%AAte%20%C3%A0%20ex%C3%A9cuter.)
+ [event_scheduler](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html#sysvar_event_scheduler)
+ [The Event Scheduler and MySQL Privileges](https://dev.mysql.com/doc/refman/5.7/en/events-privileges.html)
+ [MySQL - Enabling the Event Scheduler](https://techexpert.tips/mysql/mysql-enabling-event-scheduler/)
+ [MySql Event Scheduler](https://www.surekhatech.com/blog/mysql-event-scheduler)
> + [MySQL Events Scheduler](https://rumitmittal7.medium.com/mysql-events-scheduler-3ee943f76f9b)

Sources :
+ [Event Scheduler Configuration](https://dev.mysql.com/doc/refman/5.7/en/events-configuration.html)


Le planificateur d’évènements ou **event scheduler** est une nouveauté de MySQL apparue avec la version 5.1. Il offre la possibilité à l’administrateur de base de données de déclencher l’exécution de programmes stockés directement sur le serveur MySQL. Ce planificateur de tâches (CRON-like) interne permet donc d’automatiser très simplement des tâches à des intervalles réguliers, ou à heure fixe, sans avoir besoin de configurer le système d’exploitation qui héberge la base de données. Pour pouvoir l’utiliser, il faut tout d’abord l’activer car ce n’est pas le cas par défaut :

## Comment activer "Le planificateur d'évènements"

Pour savoir si le planificateur est actif, il suffit de vérifier la valeur de la variable `event_scheduler` dans la console MySQL via la commande suivante :

```sql
mysql> SHOW VARIABLES LIKE ’event_scheduler’;
```

```sql
mysql> SHOW VARIABLES LIKE ’event_scheduler’;
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| event_scheduler | OFF   | 
+-----------------+-------+
1 row in set (0,00 sec)

```

Pour l'activer, on exécute la commande suivante :

```sql
mysql> SET GLOBAL event_scheduler = ON;
Query OK, 0 rows affected (0,00 sec)
```

On vérifie l'état de la variable :

```sql
mysql> SHOW VARIABLES LIKE ’event_scheduler’;
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| event_scheduler | ON    |
+-----------------+-------+
1 row in set (0,00 sec)
```

Une fois activé, MySQL démarre un processus léger (thread) en tâche de fond. Ce processus est chargé d’exécuter les événements lorsque le moment est venu. Vous pouvez le constater en utilisant ...
