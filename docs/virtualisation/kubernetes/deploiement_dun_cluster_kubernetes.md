# DEPLOIEMENT D'UN CLUSTER KUBERNETES

Il existe aujourd'hui des services Kubernetes dans la plupart des clouds publics qui facilitent la création d'un cluster grâce à quelques instructions en ligne de commande. Une solution basée sur le cloud nécessite de payer pour bénéficier de ces ressources ainsi que d'avoir une connexion réseau active.

Le développement en local peut être plus attractif et l'outil `minikube` fournit un moyen simple d'obtenir un cluster Kubernetes local qui s'exécute sur une VM installée sur son ordinateur portable ou un ordinateur de bureau. Bien que cela soit avantageux financièrement, `minikube` crée seulement un cluster à noeud unique.

Une alternative apparue récemment consiste à exécuter un cluster **Docker-in-Docker**, qui peut faire tourner un cluster à plusieurs noeuds sur une seule machine. Ce projet étant encore en version bêta, il faut cependant garder à l'esprit que l'on peut rencontrer des problèmes inattendus.

## 1. Installation de Kubernetes sur un fournisseur de cloud public

### 1.1 Installation de Kubernetes avec Google Kubernetes Engine (GKE)

<details>

La **Google Cloud Platform (GCP)** propose un service Kubernetes hébergé appelé **Google Kubernetes Engine (GKE)**.

**Prérequis**

- Avoir un compte GCP actif
- Avoir installé l'outil `gcloud` (https://cloud.google.com/sdk/downloads)

> Il sera peut-être nécessaire d'activer d'abord l'API du moteur Kubernetes en utilisant la commande `gcloud services enable container`

- Définir une zone par défaut une fois que `gcloud` soit installé :

_Commande :_

```shell
gcloud config set compute/zone us-west1-a
```

- Créér un cluster :

_Commande :_

```shell
gcloud container clusters create kuar-cluster --num-nodes=3
```

- Obtenir des informations d'identification sur le cluster avec la commande (après quelques minutes) :

_Commande :_

```shell
gcloud container clusters get-credentials kuar-cluster
```

Les instructions complètes pour créer un cluster GKE se trouvent dans la documentation Google Cloud Platform (https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster?hl=fr).

</details>

### 1.2 Installation de Kubernetes avec Azure Kubernetes Service (AKS)

<details>

Microsoft Azure offre un service Kubernetes hébergé dans le cadre du service de conteneur Azure. La méthode la plus simple pour démarrer avec le service de container Azure est d'utiliser le shell intégré Azure Cloud du portail Azure. On peut activer l'interpréteur de commande en cliquant sur l'icône du shell dans la barre d'outils en haut à droite : <i class="fa-solid fa-terminal"></i>

Le shell bénéficie de l'outil `az` automatiquement installé et configuré pour fonctionner avec l'environnement Azure.

On peut également installer l'interface de ligne de commande `az` sur l'ordinateur local.

**Prérequis**

- Avoir un compte Azure actif

- Se connecter avec son compte

_Commande :_

```shell
az login
```

- Créer le groupe de ressources :

_Commande :_

```shell
az group create --name=my-cluster --location=westus
```

- Créer le cluster :

_Commande :_

```shell
az aks create --resource-group=kuar --name=kuar-cluster
```

- Obtenir des informations d'identification sur le cluster :

_Commande :_

```shell
az aks get-credentials --resource-group=kuar --name=kuar-cluster
```

- Installer l'outil `kubectl` :

_Commande :_

```shell
az aks install-cli
```

Des instructions plus complètes pour l'installation de Kubernetes sur Azure se trouvent dans la documentation Azure (https://learn.microsoft.com/fr-fr/azure/aks/learn/quick-kubernetes-deploy-cli).

</details>