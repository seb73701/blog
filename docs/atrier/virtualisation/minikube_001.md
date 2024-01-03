---
title: Minikube - Liens divers
description: 
published: true
date: 2023-04-21T11:19:19.413Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:19:16.004Z
---

# Articles

<code>
Pour ceux que ca intéresse, voici quelques lignes de commande pour déployer une instance minikube sur son poste via VirtualBox.
J’imagine que cela pourra en intéresser plus d’un souhaitant jouer avec cet outil en vogue.
Par défaut, minikube tente de créer une nouvelle interface en « 192.168.99.1/24 » pour VirtualBox qui nécessite une élévation de droit.
Cependant, notre poste en possède déjà une en « 192.168.56.1/24 ». Il suffit de le signaler à l’exe pour s’éviter ce désagrément.
Voici les commandes à exécuter via PowerShell. Les fichiers ainsi générés se trouveront dans un sous répertoire nommé « .minikube ».
Pour ma part, je le fais dans « C:\datas » qui n’est pas sauvegardé par la DPSI. Par conséquent, dans « C:\datas\.minikube »

cd C:\datas
$path = Get-Location

$url_minikube = "https://github.com/kubernetes/minikube/releases/download/v1.0.1/minikube-windows-amd64.exe"
$output_minikube = "$path\minikube.exe"

$url_kubectl = "https://storage.googleapis.com/kubernetes-release/release/v1.14.0/bin/windows/amd64/kubectl.exe"
$output_kubectl = "$path\kubectl.exe"

# Télécharger les deux exe
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
(New-Object System.Net.WebClient).DownloadFile($url_minikube, $output_minikube)
(New-Object System.Net.WebClient).DownloadFile($url_kubectl, $output_kubectl)

# Créer une vm minikube dans VirtualBox
$env:MINIKUBE_HOME = "$path"
.\minikube.exe start --host-only-cidr=192.168.56.1/24

# Lister les pods pour valider le bon fonctionnement
.\kubectl.exe get pods --all-namespaces

Et enfin, quelques commandes pour l’administrer :

# Ouvrir un nouvel onglet dashboard sur son navigateur
#.\minikube.exe dashboard

# Démarrer la vm minikube dans VirtualBox
#.\minikube.exe start

# Stopper la vm minikube dans VirtualBox
#.\minikube.exe stop

# Détruire la vm minikube dans VirtualBox
#.\minikube.exe delete

</code>

+ [Installing Minikube and Kubernetes on Windows 10](https://technology.amis.nl/2017/10/24/installing-minikube-and-kubernetes-on-windows-10/)
+ [First steps with Istio on Kubernetes on Minikube on Windows 10](https://technology.amis.nl/2017/10/25/first-steps-with-istio-on-kubernetes-on-minikube-on-windows-10/)
+ [Running Kubernetes 1.10 using MiniKube on Windows 10 (adding kubectl and helm/tiller)](https://technology.amis.nl/2018/08/13/running-kubernetes-1-10-using-minikube-on-windows-10-adding-kubectl-and-helm-tiller/)
+ [How to learn Kubernetes with Minikube](https://www.hpe.com/us/en/insights/articles/how-to-learn-kubernetes-with-minikube-1812.html)