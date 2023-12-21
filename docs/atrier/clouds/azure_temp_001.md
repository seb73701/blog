---
title: Gérer sa souscription Azure
description: 
published: true
date: 2023-07-07T10:01:35.548Z
tags: 
editor: markdown
dateCreated: 2023-07-07T10:01:35.548Z
---

# Gérer sa souscription Azure

Nous allons voir ici comment mettre en place une stratégie pour supprimer l’ensemble de vos ressources chaque soir ou chaque semaine, c’est à vous de décider. En effet, c’est bien beau d’avoir une souscription Azure pour mettre en place des POC ou faire des tests pour vos développeurs. Cependant, la facture peut être salée si vous n’avez pas mis en place une stratégie de nettoyage régulière de vos ressources. Dans le cas présent, nous allons mettre en place deux choses pour nettoyer vos ressources.

Dans un premier temps, nous allons mettre en place une tache planifiée via un automation account qui va supprimer les ressources ayant un tag spécifique, dans notre cas : ‘`expireOn`’


Sur le groupe de ressource de l’automation account, nous mettrons un lock pour éviter qu’il soit supprimé malencontreusement.

Ensuite,  nous allons mettre en place la stratégie qui va automatiquement mettre un tag sur chaque ressource nouvellement créée.

## Création de l’automation account

Pour cela, depuis le portail Azure rechercher **Automation Accounts**


Bien penser à cliquer sur **Yes** sur la fenêtre de création de l’Automation Accounts


Ensuite, sélectionner **Runbook** dans le bandeau de gauche, donner un nom à votre Runbook puis choisissez PowerShell comme type de Runbook et copier le contenu du script :


Les sources sont disponibles ici : https://github.com/FBoucher/AzSubscriptionCleaner

Puis valider par **Publish**. Cliquer ensuite sur **Modules** puis cliquer sur **Browse Gallery** pour ajouter les modules suivant en respectant l’ordre d’installation :

1. Az.Accounts
1. Az.ResourceGraph
1. Az.Resources

Maintenant, nous pouvons mettre en place la tâche planifiée. Pour cela, cliquer sur votre Runbook créé précédemment puis cliquer ensuite sur **Link To Schedule**


Cliquer ensuite sur **Create a new schedule** pour planifier l’exécution du script


Donner un nom à votre tâche planifier puis valider par **Create**, penser à sélectionner **Recurring** pour que celle-ci puisse s’exécuter chaque jour ou chaque vendredi suivant la planification que vous souhaitez.


Pour sécuriser votre groupe de  ressources et éviter qu’il soit supprimé malencontreusement, pensez à mettre un Lock. Pour cela, sélectionnez votre resource groupe contenant l’automation Account depuis le portail Azure, cliquer sur **Lock** dans le bandeau de gauche puis cliquer sur **Add** pour ajouter un lock.


Il reste maintenant à mettre en place la stratégie. Mais vous pouvez déjà tester le bon fonctionnement de celle-ci en mettant le tag `expired On` avec une date au format `YYYY-MM-dd` sur une de vos ressources puis exécuter le Runbook 😉

## Création de la stratégie

Pour mettre en place la stratégie, cliquer sur **Policy** dans votre portail Azure


Cliquer ensuite sur **Definitions** puis sur **Policy Definition**


Sélectionner ensuite votre souscription, cliquer sur **Select** puis donner un nom à votre stratégie


Donner lui ensuite une description puis créer une nouvelle catégorie et copier/coller le json suivant puis valider par **Save** :

```json
{
 "mode": "All",
 "policyRule": {
   "if": {
     "allOf": [
        {
             "field": "tags['expiredOn']",
             "exists": "false"
         }
     ]
  },
  "then": {
    "effect": "append",
     "details": [
        {
           "field": "tags['expiredOn']",
            "value": "[utcNow()]"
         }
     ]
   }
 },
 "parameters": {}
}
```

Retourner ensuite sur **Définitions** puis sélectionner votre stratégie, pour la trouver plus facilement, cliquer sur **Custom** dans le bandeau du haut au niveau du champ **Type** puis sélectionner **Assign** en cliquant sur le champ …


Au niveau des exclusions, penser à exclure le ressource groupe de votre Automation Account puis valider 😊

source : https://blog.ippon.fr/