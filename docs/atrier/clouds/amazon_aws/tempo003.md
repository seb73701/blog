---
title: Sauvegarder vos nuages AWS en quelques minutes
description: 
published: true
date: 2023-04-21T10:58:35.767Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:58:29.803Z
---

# Sauvegarder vos nuages AWS en quelques minutes

## Introduction
Dans un monde où la plupart des infrastructures sont hébergées dans le Cloud et où les données se baladent de région en région, la sauvegarde reste plus que jamais un enjeu majeur dans un SI. Même si on n’est pas responsable de l’architecture sous-jacente lorsqu’on parle de Cloud, on est quand même responsable de notre aptitude ou non à pouvoir revenir en arrière en cas d’accident, de panne ou d'attaque (type ransomware par exemple). Cet aspect se retrouve notamment dans le [Shared Responsibility Model](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/shared-responsibility.html) introduit par AWS qui explique que les responsabilités de sécurité sont partagées entre l’utilisateur et AWS.

La plupart des lecteurs de cet article auront probablement déjà fait face à une tâche de backup qui est longue et ennuyante et où l’on doit souvent utiliser de nombreux outils pour sauvegarder des services différents. Pour n’en citer que quelques-uns : archivage sur disque dur, velero, RClone …

Aujourd’hui, cet article va vous présenter une manière de réaliser des backups au sein de tous vos services AWS et même plus encore, au niveau de toute votre organisation !

## Présentation
AWS Backup est un service ayant moins de 3 ans permettant de centraliser et automatiser la protection de vos données. Ce service prend en compte plusieurs services (EC2, EBS, S3, RDS, Aurora, DynamoDB, DocumentDB, EFS, FSx, Storage Gateway …) Je profite d’ailleurs de la sortie récente en Europe des backups pour S3  pour écrire cet article qui fera probablement gagner du temps et du confort à nombre d’entre vous.

Un des aspects intéressants, notamment économiquement, est que les backups peuvent être réalisés en full ou en incrémental. Pour rappel, un backup incrémental ne rajoute au dernier backup que les dernières modifications alors qu’un backup full crée à nouveau une copie entière à chaque sauvegarde. Cela permet de réduire le nombre de backups et le temps de réalisation de ces derniers.

Autre aspect intéressant, ces backups peuvent être réalisées cross account et cross region sans limite, mis à part peut-être que les backups sont forcément en mode “full” selon le mode de copie souhaité et le service sélectionné.

## Pré requis
Pour pouvoir créer des backups, il y a quatre opérations préalables à réaliser :

- Configurer le rôle permettant de créer les backups
- Activer les services que vous voulez sauvegarder dans les paramètres du service AWS Backup
- Créer la clé KMS permettant de chiffrer les backups
- Créer le Backup Vault dans le(s) compte(s) / région(s) sélectionné(e)(s)

## Mise en place
### Création du rôle
Via le service IAM, créer un rôle et choisir l’option permettant de créer un service rôle en sélectionnant AWS Backup.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/05/backup-role.png">

### Policy du rôle
Une fois le rôle créé, il faut lui attacher les policies permettant de faire les backups. Il y a 2 policies managées par AWS qu’il faut ajouter par défaut, et 4 si vous voulez utiliser AWS Backup pour sauvegarder vos buckets S3 :

- AWSBackupServiceRolePolicyForBackup
- AWSBackupServiceRolePolicyForRestores
- AWSBackupServiceRolePolicyForS3Backup
- AWSBackupServiceRolePolicyForS3Restores

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/05/backup-policy.png">

## Paramétrage du service Backup
Ensuite, nous allons activer les services que nous désirons sauvegarder dans AWS Backup (AWS Backup > Settings > Configure resources) :

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/05/backup-settings.png">

## Création de la clé KMS et du vault
Avant de créer le backup vault, il faut créer une clé KMS et autoriser le rôle de backup à l’utiliser. Vous pouvez maintenant créer le vault simplement via l’interface en n’oubliant pas de sélectionner la clé KMS tout juste créée pour le chiffrement.

<img src="https://blog.ippon.fr/content/images/size/w1000/2022/05/backup-vault.png">

Nous sommes maintenant prêt à utiliser AWS Backup.

## Mise en place des backups
### Backup manuel
Réalisons maintenant  un backup d’un bucket S3 ! Attention, il y a certaines conditions à respecter selon les services que l’on veut sauvegarder. Pour S3, il faut que le versionning soit activé. Je crée alors un bucket et j’update un fichier dessus.

<img src="file:///E:/Telechargement/articles/Sauvegarder%20vos%20nuages%20AWS%20en%20quelques%20minutes_files/Capture%20d%E2%80%99%C3%A9cran,%20le%202022-04-22%20%C3%A0%2013.29.02.png">

Une fois chose faite, direction le service AWS Backup. Nous allons créer une sauvegarde de notre bucket ! Pour ce faire, direction AWS Backup > Protected resources > Create on-demand backup.

Je décide ici de faire un backup avec la fenêtre par défaut et qui aura une durée de rétention de 1h.

<img src="file:///E:/Telechargement/articles/Sauvegarder%20vos%20nuages%20AWS%20en%20quelques%20minutes_files/Capture%20d%E2%80%99%C3%A9cran,%20le%202022-04-22%20%C3%A0%2013.58.07.png">

On peut alors lancer le backup. Cela prend environ 1 minute pour un bucket contenant un fichier texte dans mon cas. A partir de maintenant, notre bucket S3 apparaît dans l’onglet “Protected Resources” puisqu’au moins 1 backup a été réalisé sur ce dernier.

On peut maintenant restaurer ce backup en allant dans notre backup vault et trouver notre “recovery point” correspondant au backup qu’on a réalisé et restaurer notre bucket à ce moment.

### Backup Plan
Nous venons de réaliser notre premier backup mais celui-ci a été déclenché manuellement en mode full . Cela fonctionne très bien mais n’est pas adapté à une situation de production  avec de nombreux buckets et / ou de nombreux services. De plus, dans notre démonstration, notre bucket ne faisait que quelques Ko. Mais imaginons un bucket de 10 To, à chaque backup nous recréons une nouvelle sauvegarde de 10 To ce qui fait vite augmenter les coûts. Grâce à la fonctionnalité de Backup Plan, nous pouvons réaliser des “continuous backup” qui permettront de rajouter au backup existant, le delta de différence dans celui-ci et de ne payer alors que la taille du bucket en terme de sauvegarde. Attention tout de même, cette fonctionnalité n’est disponible que sur les services S3 et RDS pour le moment.

Pour cela on va pouvoir définir un plan qui va s’appliquer sur toutes les ressources possédant un tag spécifique. Pour notre démonstration, nous allons faire un cas simple, nous allons créer deux autres buckets et positionner le tag _backup: true _et créer le backup plan (avec l’option continuous activée).

<img src="https://lh4.googleusercontent.com/Z0s4--7Wuk64gxKaT-T2vNarJvfh2yWispmbl_ARwYdXXRauEPFNUeupKJ9RlR3TD-c3awNrMjzTNUwRJpmeYkr3XQveimjkqhsLjXTs5wZkU0brJidCSdxTLykG4EE0MSO1TMrE4T7XpSez9g">

Avec ce plan, toutes les heures, toutes les ressources avec un tag backup:true (3 buckets S3 dans notre cas) seront sauvegardées continuellement et nous pourrons alors faire des restaurations à l’heure souhaitée On peut aussi affecter des ressources aux backups plans pour pouvoir rajouter un filtre en plus des tags comme par exemple : réaliser un backup quotidien pour les buckets S3 possédant le tag backup: true et un backup hebdomadaire pour les EC2 possédant ce même tag.

### Architecture

<img src="https://lh3.googleusercontent.com/T6p05jKJr1hZ7M6jusNTKCdoMZbryBTdou-cgHDdd_WXGhmoWdKa6Gd-s1qrXb5swwWg_c5axnmPQw4TEVUkFHqnTSbeCwpEZB3d7nvtEpSGYScrMT0Y3d3Li52fysu3L-T6ZxBXs97-5Gfxgg">

## Backup Policy
Dans notre exemple, nous avons réalisé nos backups sur un compte unique. Mais imaginons que nous nous situons dans une organisation contenant une centaine de comptes AWS et une dizaine d’unités d’organisation, nous n’avons pas forcément envie de passer sur tous les comptes pour définir notre backup plan, nous allons pouvoir définir une backup policy pour pouvoir l’attacher à divers comptes.

La backup policy ne définit pas beaucoup plus de valeurs d’entrées que les backups plans, mise à part la région sur laquelle effectuer les backups. En revanche, on ne peut pas filtrer parmi les ressources possédant le même tag.

### Architecture
NB : Le compte AWS-RootAB doit être parent des comptes AWS-ApplicationA et AWS-ApplicationAB

<img src="https://lh3.googleusercontent.com/1ShzrFrCY0rYM6UZnOqx0wmy2hxKOxZIBcQeXmCiHHEAgdCePGYSwBWtZ8USC16TntnFdnRwowmYc9SBscbfz7FU8P1MXOmZnxBRkuWjRcUZWAR0dMG8LxzIvQgbHcw--pRdhFJxIq5PQf6GWA">

## Pricing
Lors de la mise en œuvre d’un nouveau service AWS l’analyse du modèle de coût est indispensable afin d’optimiser les dépenses et d’estimer les coûts à venir.,Le coût de réplication par copie peut aller de 0,022 $/Gb pour les buckets S3 à 0,1 $/Gb pour les bases RDS. On peut vite se rendre compte de l’avantage à utiliser la feature continuous.

Exemple : J’ai une base RDS de 90Gb que je backup tous les jours et qui augmente d’environ 1Gb par jour, à la fin du mois j’aurais une facture de 105$ avec des full backups et une facture de 12$.

Un autre tarif à prendre en compte si votre architecture est multi région, le coût du transfert de données inter régions et inter comptes est de 0,02 $/Gb. On remarque alors que parfois le coût du transfert est plus élevé que le prix du backup.

## Code Terraform
Vous trouverez sur le repository suivant du code terraform permettant de déployer un backup et de réaliser des backups dans un vault chiffré. Vous pouvez jouer sur les variables pour ajuster les ressources à sauvegarder et les durées de rétention. Vous trouverez aussi un exemple de backup policy réalisant une sauvegarde par jour avec une copie cross account et cross region.

https://github.com/afreyermuth98/terraform-aws-backup

## Pour aller plus loin
- Créer des backups plan avec des copy cross account / cross region. Attention, il faut que tous les paramètres et pré-requis permettant de réaliser un backup soient mis en place dans le compte / la région de destination
- Utiliser d’autres modes de chiffrement avec des clés différentes selon les régions et les comptes, ou encore utiliser des clés HSM
- Réaliser des backups multi-comptes au sein d’AWS Organization
- Monitorer via Cloudwatch les jobs de backups et notifier les succès / erreurs

## Limitations / Perspectives d’évolutions
- Les services à sauvegarder  ne peuvent être filtrés que par tag au niveau des backup policy et non par service donc si l’on veut réaliser des plans de backup différents, il faut différencier les tags, par exemple backup-s3, backup-rds, backup-ec2 etc …
- Le backup S3 ne supporte pour le moment pas le cross region / cross account. (On peut quand même mettre en place des solutions de substitutions comme la réplication du bucket avec un backup dans la région de réplication)
- Le backup RDS ne supporte pas le continuous backup sur le cross region / cross account

## Conclusion
AWS Backup permet de sauvegarder une grande partie de vos services AWS à tout niveau. De quelques services à tous les services avec un filtrage, avec ou sans chiffrement, inter-régions et / ou inter-comptes tout en ayant une possibilité de mettre en place des politiques au sein de l’organisation. Le service est encore nouveau et évolue rapidement, certaines fonctionnalités ne sont pas encore complètes mais ça ne saurait tarder.

source : https://blog.ippon.fr/2022/05/24/sauvegarder-vos-nuages-aws-en-quelques-minutes/