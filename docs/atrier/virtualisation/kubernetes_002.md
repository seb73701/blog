---
title: Cas pratique sur la sécurisation d'un cluster Kubernetes
description: 
published: true
date: 2023-04-21T11:18:42.838Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:18:38.369Z
---

# Cas pratique sur la sécurisation d'un cluster Kubernetes

> Cet article présente trois exemples de problèmes de sécurité rencontrés sur des clusters Kubernetes, causés par un manque de maîtrise des applications déployées sur un cluster par ses administrateurs ou par les développeurs des applications s’y exécutant. Nous donnons ensuite des pistes afin de mieux maîtriser et sécuriser ces applications.

## 1. Présentation de la machinerie Kubernetes
Kubernetes est un orchestrateur de conteneurs, qui exécute des applications – typiquement distribuées sous forme d'images Docker – sur un ensemble de machines appelées cluster. Nous présenterons d'abord les objets contenant les données qui définissent entièrement la configuration du cluster, accessibles via des API. Ces objets sont créés et consommés par la machinerie (extensible) de Kubernetes, que nous présenterons ensuite. Pour terminer cette présentation de Kubernetes, nous présenterons différents modes d'installation et leurs impacts sur la sécurité.

### 1.1 API Server et Custom Resource Definitions
La pierre angulaire de Kubernetes est son API server, qui offre l'accès aux objets décrivant la configuration du cluster. Ces données sont stockées dans une base de données distribuée clef-valeur etcd [1].

Les principaux types d'objets qui seront utilisés dans cet article sont :

- Les Pods, qui représentent un ensemble de conteneurs s'exécutant sur la même machine, partageant le même réseau (sous Linux, même network namespace), et ayant parfois des répertoires partagés entre eux ;
- Les Deployments, qui prescrivent l'exécution d'un certain nombre d'instances identiques d'un même Pod ;
- Les Roles et ClusterRoles, qui décrivent des permissions, c’est-à-dire des ensembles d'actions ;
- Les RoleBindings ou ClusterRoleBindings, qui attribuent les permissions décrites par un Role ou ClusterRole à un compte utilisateur ou de service ;
- Les Services, qui décrivent comment une application (c'est-à-dire un ensemble de Pods) est rendue accessible depuis l'extérieur du cluster ;
- Les NetworkPolicies, qui contiennent des règles de filtrage à appliquer entre Pods, ou entre un Pod et des services externes au cluster ;
- Les PodSecurityPolicies définissent les politiques de sécurité à l'échelle du cluster, c’est-à-dire par exemple interdire l'exécution de Pods privilégiés ayant des privilèges équivalents à root sur les machines, ou encore restreindre les syscalls pouvant être appelés par les applications hébergées par le cluster.
- 
La plupart des objets appartient à un namespace, qui sépare logiquement les composants faisant partie de la machinerie, des applications particulières, ou encore de séparer les objets créés par plusieurs utilisateurs du cluster (clients, équipes, projets, applications...).

La documentation de Kubernetes [2] décrit précisément tous ces objets et leurs propriétés.

Il est possible (et fréquent) d'installer des applications sur un cluster, qui vont définir de nouveaux types d'objets (Custom Resource Definitions), qui seront consommés par l'application. Nous pouvons par exemple citer le cas de Prometheus Operator [3], qui définit notamment un objet Prometheus permettant de demander le déploiement d'une instance du système de monitoring Prometheus [4].

### 1.2 Overlay network, proxy, scheduler, kubelet
Les API de Kubernetes sont « consommées » par un des programmes faisant partie de la machinerie du cluster. Les principaux composants sont :

- Le scheduler, qui affecte les Pods aux machines qui constituent le cluster ;
- Le kubelet, qui se charge d'exécuter sur chaque machine les Pods affectés par le scheduler ;
- Le proxy, qui assure l'acheminement du trafic destiné à une IP de Service vers les Pods correspondants ;
- L'overlay network (optionnel), qui peut entre autres appliquer les NetworkPolicies.

### 1.3 Mode d'installation
Plusieurs modes d'installation sont envisageables pour un cluster :

- Louer un service dans un cloud afin de ne n'avoir à se préoccuper que des applications que nous installons sur le cluster et de leurs données. La sécurité de la machinerie du cluster relèvera dans ce cas de la responsabilité du fournisseur.
- Installer soi-même Kubernetes sur des machines louées (infrastructure-as-a-service), ce qui implique de devoir maintenir et auditer la machinerie de Kubernetes ainsi que les systèmes d'exploitation, et de choisir des composants optionnels apportant les fonctionnalités qui sont habituellement incluses dans les offres « Kubernetes as a service », comme la gestion du stockage, de l'overlay network, ou encore des load balancers.

Quel que soit le choix qui est fait, pour maîtriser la sécurité des applications hébergées sur un cluster, il faut être bien conscient des différents composants qui le constituent. Pour chaque composant, il faut bien identifier :

- Les garanties de sécurité qu'ils apportent (ex. : savoir si le système d'overlay network choisi est bien capable d'appliquer les NetworkPolicies définies) ;
- Les entités qui sont responsables de leur installation, maintenance et mise à jour : il peut s'agit d'un fournisseur de services cloud, ou d'une équipe interne en charge de l'administration des systèmes d'exploitation.

## 2. Exemples de vulnérabilités rencontrées sur des clusters Kubernetes et contre-mesures associées
Les vulnérabilités logicielles présentes dans les composants de Kubernetes, ou plus largement dans les composants usuellement présents dans ce type d'infrastructures, ne seront pas abordées ici. Nous allons voir plutôt des vulnérabilités provenant d'un manque de maîtrise des applications hébergées sur un cluster, ou provenant de configurations insuffisamment durcies.

### 2.1 Abus de privilèges d'applications privilégiées
#### 2.1.1 Exemple de Helm version 2
Helm est un « gestionnaire de packages » pour Kubernetes, qui installe des applications sur un cluster à partir de modèles (appelés charts), qui sont habituellement paramétrables pour s’adapter aux besoins. De nombreux charts sont proposés par la communauté, ce qui facilite l'installation d'applications.

Dans la version 2 de Helm, un composant appelé Tiller est installé sur le cluster et se charge d'y installer les applications. Les utilisateurs interagissent avec Tiller au moyen d'un utilitaire en ligne de commandes, helm. La version 3 n’est pas concernée par le scénario présenté ici, puisque les déploiements s’effectuent avec les permissions de l’utilisateur qui exécute helm.

Le composant Tiller est particulièrement sensible du point de vue de la sécurité : en effet, puisqu'il doit être capable de déployer des applications, il est en mesure de créer des objets (Deployment, Role...). Ses privilèges sont donc élevés, il peut prendre le contrôle du cluster, voire probablement des machines qui composent le cluster si aucune PodSecurityPolicy n'est définie pour interdire l'exécution de Pods privilégiés. Par défaut, sur un cluster où aucun mécanisme d'autorisation n'est activé, il a l'autorisation de déployer des applications dans n'importe quel namespace Kubernetes, et n'authentifie pas ses clients, comme le rappelle ce message lors de l'installation de Tiller :

```
$ helm init
(...)
Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://v2.helm.sh/docs/securing_installation/
```

Dans ce cas de figure, il suffit de pouvoir établir une connexion réseau vers le service gRPC exposé par Tiller pour déployer une application. Ce port n’est par défaut pas accessible depuis l’extérieur du cluster, mais il est accessible aux administrateurs du cluster par un mécanisme qui ne sera pas détaillé ici, ainsi qu’aux applications s'exécutant sur le cluster en l'absence de NetworkPolicies l'interdisant. Sur un tel cluster, cette faiblesse pourra être exploitée par un attaquant ayant pris le contrôle d'une application vulnérable s'exécutant sur le cluster, qui aura accès au port gRPC de Tiller (puisque par défaut, aucune NetworkPolicy ne l’interdira), et pourra ainsi passer des ordres à Tiller et prendre le contrôle de l'ensemble du cluster.

Lorsque Helm est installé sur un cluster sur lequel le mécanisme d'autorisation RBAC [5] est activé, le composant Tiller n'a pas de permissions par défaut. Pour lui permettre de déployer des applications, il faudra définir des autorisations. Nous commencerons par définir un compte de service dédié nommé tiller :

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
```

Ensuite, nous pourrons par exemple attribuer les privilèges cluster-admin à ce compte de service. Le Role cluster-admin est un groupe défini par défaut, qui détient tous les pouvoirs.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Selon les besoins, ces autorisations pourront s'appliquer à l'ensemble du cluster, ou à un nombre réduit de namespaces Kubernetes.

De la même manière que Tiller, les applications de déploiement continu (CI/CD) comme Jenkins [8] ou ArgoCD [9], sont typiquement capables de déployer de nouvelles applications ou de créer des objets Kubernetes, et doivent donc être examinées soigneusement lors de l'audit d'un cluster. Ces applications ne s'exécutent d'ailleurs pas nécessairement sur le cluster.

#### 2.1.2 Contre-mesure : revue des permissions définies
Lors d'un audit d'un cluster Kubernetes, il convient de déterminer quelles sont les actions qui peuvent être effectuées par les utilisateurs (externes au cluster, c’est-à-dire les administrateurs de la machinerie du cluster, ou les développeurs des applications qui s’y exécutent) ou comptes de service (utilisés par les applications hébergées sur le cluster).

Tout d'abord, il faut bien comprendre la configuration spécifique du cluster que l’on étudie :

les mécanismes d'authentification autorisés ;
les mécanismes d'autorisation autorisés ; en particulier, le mécanisme AlwaysAllow autorise les utilisateurs authentifiés à effectuer n'importe quelle action ;
les mécanismes d'Admission Control configurés, qui s'exécutent après la phase d'autorisation d'une requête, et sont parfois utilisés comme un mécanisme de sécurité supplémentaire (ex. : OPA [7]).
Pour repérer les applications privilégiées s'exécutant sur le cluster, nous pouvons commencer par rechercher celles qui ont accès à un jeton via leur système de fichiers : en effet, les applications ne possédant pas de jeton ne pourront pas s'authentifier auprès de l'API Server, et ne seront donc pas des applications privilégiées vis-à-vis du cluster. Habituellement, la plupart des applications n’ont pas besoin d’interagir avec la machinerie Kubernetes et n'ont donc pas besoin de jeton. Pour identifier les applications possédant un jeton, nous pourrons utiliser la commande suivante, qui recherche tous les Pods dont la propriété automountServiceAccountToken est soit non définie, soit définie à true (valeur par défaut) ;

```json
$ kubectl get pods --all-namespaces -o json \
    | jq '.items \
    | map(select(.spec.automountServiceAccountToken != false)) \
    | map({podname: .metadata.name, \
           accountname: (.spec.serviceAccountName // "default")})'
[
  {
    "podname": "kube-proxy-c4h9m",
    "accountname": "kube-proxy"
  },
  {
    "podname": "kube-scheduler-minikube",
    "accountname": "default"
  },
...
]
```

Une fois les Pods disposant d'un compte de service identifiés, il faut lister les objets de type RoleBindings et ClusterRoleBindings associés. Ces objets font référence à des Roles et ClusterRoles, qui décrivent les permissions associées. Nous pourrons alors déterminer quels sont les privilèges de chaque application, et voir s'il s'agit d'une application privilégiée ou non. Il faudra ensuite vérifier que ces privilèges sont bien acceptables, et si, comme dans l’exemple présenté concernant Helm v2, il n’existe pas de chemin permettant à un attaquant d’abuser de ces privilèges.

### 2.2 Supply chain : constitution et acheminement des images Docker
#### 2.2.1 Exemple : stockage d'images Docker non sécurisées
Les conteneurs s'exécutant sur un cluster sont la plupart du temps distribués sous forme d'images Docker. Lors de l'audit d'un cluster hébergeant des applications « maison » développées par l'entreprise qui administre le cluster, ou fournies par un éditeur de logiciels se préoccupant peu de la sécurité, il n'est malheureusement pas rare que les images soient distribuées par un simple dépôt d'images Docker Registry avec une configuration par défaut, c'est-à-dire sans chiffrement du trafic réseau avec TLS, sans gestion des permissions, donc accessible à tous en lecture... et en écriture. Les images sont également rarement signées, ce qui n'arrange rien, mais fera le bonheur de l’auditeur.

Un attaquant ayant un accès réseau à ce dépôt d'images (parfois exposé directement, parfois via un rebond suite à la prise de contrôle d’une application hébergée sur un cluster insuffisamment durci) peut alors s'en donner à cœur joie, et remplacer les images par des versions modifiées par ses soins, et attendre qu'elles soient utilisées pour prendre le contrôle de l'application, puis éventuellement du cluster si les privilèges de l’application maintenant contrôlée par l’attaquant le permettent. Les applications cloud-native étant généralement accessible en HTTP(S), l'auteur suggère de remplacer le traditionnel shell netcat par un webshell GoTTY [10].

#### 2.2.2 Contre-mesure : protection des images
Pour limiter les risques, il est crucial de renforcer l'acheminement des images :

- Le dépôt d'images doit avoir une gestion fine des permissions, en particulier dans le cas où le cluster et le dépôt se reposent sur un référentiel d’utilisateurs différents, ayant des permissions différentes (ex. : plusieurs équipes, multi-tenant). En particulier, les permissions d'écriture doivent être restreintes aux seuls systèmes légitimes pour ces opérations, comme les systèmes d'intégration continue (CI) et les administrateurs. En aucun cas une application lambda déployée sur le cluster ne doit avoir de droits en écriture sur le dépôt (ce qui arrive malheureusement parfois lorsque le dépôt d’images a été mis en place à la hâte) ;
- L'utilisation de TLS pour les communications avec le dépôt d'images doit être obligatoire ;
- Si le cluster est utilisé par plusieurs clients, il est possible de protéger les opérations en lecture sur le dépôt par des secrets différents pour chaque client, pour éviter que d’éventuels secrets présents dans les images Docker (pratique déconseillée, mais courante) ne soient accessibles par un autre client. Dans la définition des Pods, il faudra référencer ce secret via l'option de configuration imagePullSecrets, qui sera utilisée par la machinerie de Kubernetes, mais ne sera pas mise à disposition de l’application qui n’en a pas besoin) ;
- Il convient de configurer le cluster pour n'autoriser que les images signées par des utilisateurs de confiance, et de protéger correctement les clefs de signature.

L'utilisation du dépôt d'images open source Harbor [11] permet de régler une partie des problèmes, grâce au support de permissions fines, et à la possibilité de rejeter les images non signées.

### 2.3 Accès à des applications internes
Dans un cluster, il est fréquent que les applications exposent des services destinés à être accessibles uniquement par d'autres applications internes au cluster, voire réservées aux administrateurs du cluster. On peut citer par exemple :

- Dans un Pod, avoir en écoute sur un port donné un service qui se charge d'exposer des métriques relatives au fonctionnement de l'application qui tourne dans le Pod, qui seront consommées par un outil de collecte de métriques comme Prometheus [4] ;
- Des micro-services internes considérant que tous leurs utilisateurs sont de confiance et n'effectuant ni authentification ni contrôle des autorisations, toutes leurs connexions entrantes provenant normalement du cluster ;
- Des interfaces de tracing pouvant exposer des données utilisateurs sensibles (ex. : Jaeger [12]) ;
- Des interfaces de contrôle, particulièrement intéressantes, comme c'est le cas pour Istio [13], permettant à un attaquant d'intercepter le trafic réseau entre deux micro-services ;
- Des consoles d’administrations comme le dashboard Kubernetes [14] ;
- L'API Server lui-même.

Malheureusement, lorsqu'une seule application s'exécutant sur le cluster est compromise, si aucune règle de filtrage n'est définie, tous ces services vont devenir disponibles... ce qui peut avoir des conséquences dramatiques !

#### 2.3.1 Contre-mesure : définition de règles de filtrage (NetworkPolicies)
Heureusement, Kubernetes possède un mécanisme très pratique pour contrôler finement les flux réseau autorisés sur un cluster : les NetworkPolicies.

Ces politiques de filtrage sont appliquées par l'overlay network ou par l'opérateur cloud chez qui le cluster a été loué. Il faut vérifier qu'elles seront bien honorées : ce n'est pas le cas par exemple avec certains overlay networks (ex. : Flannel [15]).

Les NetworkPolicies permettent de déclarer les flux réseau (TCP ou UDP) autorisés, et de spécifier les sources et destinations soit par des plages d'adresses IP, soit, plus simplement, par les labels portés par les Pods. Ce mécanisme de labels est très pratique, et permet de décrire simplement une règle comme « les serveurs HTTP sont autorisés à contacter le serveur MySQL sur le port TCP/3306 » (voir exemple ci-dessous), sans se soucier du nombre d'instances en cours d'exécution de chaque composant (qui peut varier dans le temps), de son adresse IP, ou du serveur physique sur lequel il s'exécute.

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: api-allow-mysql-ingress
spec:
  podSelector:        # Cette politique s'applique à tous
    matchLabels:      # les Pods ayant le label
      k8s-app: mysql  # k8s-app: mysql (ex. serveur MySQL)
  ingress: # cette politique ne concerne
  - ports: # que les flux entrants
    - port: 3306
      protocol: TCP # optionnel
    from:                   # le trafic provenant
    - podSelector:          # des Pods ayant le label
        matchLabels:        # uses-mysql: true sera
          uses-mysql: true  # autorisé par cette politique
```

Il conviendra également de définir, dans chaque namespace, une NetworkPolicy interdisant tout trafic qui n'est pas autorisé explicitement par une autre règle. Des exemples de NetworkPolicies répondant à des cas d’usage variés sont présentés en [16].

## 3. Outils d'analyse automatique de la sécurité d'un cluster
Heureusement pour les auditeurs et concepteurs d'applications hébergées sur un cluster Kubernetes, il existe de nombreux outils automatiques qui vérifient ou améliorent le niveau de sécurité d'un cluster et des applications qui y sont hébergées.

### 3.1 Analyse automatique d'images Docker
Les applications hébergées sont la plupart du temps distribuées sous la forme d'images Docker. Si nous avons vu précédemment les conséquences que peut avoir une configuration mal maîtrisée, il ne faut cependant pas négliger les vulnérabilités pouvant se trouver dans les applications elles-mêmes. Un attaquant prenant le contrôle d’une application a en effet accès à une surface d’attaque bien plus importante que lorsqu’il est à l’extérieur du cluster.

Clair [17] est un outil open source, capable d'analyser le contenu de chaque couche composant une image docker, utilisant les métadonnées publiées par de nombreuses distributions (RHEL, Ubuntu, Debian, Alpine...) pour détecter la présence de packages contenant des vulnérabilités connues. Clair est utilisable par le biais de son API, et peut émettre des notifications lors de la détection de problèmes, ce qui le rend particulièrement bien adapté à l'utilisation au sein d'un pipeline d'intégration/déploiement continu.

Klar [18] est outil en ligne de commandes simple d'utilisation permettant de déclencher un scan par Clair, et d'en obtenir les résultats comme montré dans l'exemple ci-dessous. Cet outil est notamment utilisé par GitLab pour scanner les images produites par les pipelines d'intégration continue gitlab-ci.

```
$ klar debian:stretch-slim
...
-----------------------------------------
CVE-2017-15670: [High]
Found in: glibc [2.24-11+deb9u3]
Fixed By: 2.24-11+deb9u4
The GNU C Library (aka glibc or libc6) before 2.27 contains an off-by-one error leading to a heap-based buffer overflow in the glob function in glob.c, related to the processing of home directories using the ~ operator followed by a long string.
https://security-tracker.debian.org/tracker/CVE-2017-15670
-----------------------------------------
```


### 3.2 Analyse automatique de configuration d'un cluster
L'analyse manuelle de la configuration d'un cluster pouvant être fastidieuse, l'auditeur pressé pourra identifier des problèmes rapidement en utilisant des outils automatiques faciles d'emploi et open source, dont la sortie souvent pertinente devra cependant être vérifiée manuellement. Il est intéressant de les utiliser soit ponctuellement (en audit par exemple), ou périodiquement de manière automatique.

#### 3.2.1 Analyse statique d’un Pod
Kubesec [19] est un outil d'analyse statique des objets de type Pod dont voici un exemple d'utilisation sur un cluster de développement non sécurisé minikube [6] :


```json
$ kubectl get pods -n kube-system -o yaml etcd-minikube > pods.yml
$ docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin < pods.yml
(...)
{
  "selector": "containers[] .resources .limits .cpu",
  "reason": "Enforcing CPU limits prevents DOS via resource exhaustion"
},
(...)
{
  "selector": "containers[] .securityContext .runAsNonRoot == true",
  "reason": "Force the running image to run as a non-root user to ensure least privilege"
},
(...)
```

Cet outil donne généralement de bons conseils, comme sur cet exemple :

- Définir des quotas de ressources pour éviter que cette application ne consomme trop de RAM ou de CPU et ne perturbe les autres applications ;
- Ne pas faire tourner le service avec un utilisateur privilégié ;
- Restreindre les appels systèmes pouvant être utilisés par l'application grâce à une liste d'autorisation seccomp.

#### 3.2.2 Analyse de configuration d'un cluster
Il est également intéressant d'effectuer une revue de configuration automatique au niveau du cluster avec l'outil open source kube-bench [20].

```
$ wget https://raw.githubusercontent.com/aquasecurity/kube-bench/master/job.yaml
$ kubectl apply -f job.yaml
$ kubectl logs -l app=kube-bench --tail=-1
```

Cet outil passe en revue de nombreux points de contrôle, donne une explication des problèmes détectés et propose des corrections à apporter. À titre d'exemple, voici un extrait de la sortie sur un cluster non sécurisé :

```
(...)
[FAIL] 2.2.5 Ensure that the proxy kubeconfig file permissions are set to 644 or more restrictive (Scored)
(...)
2.2.5 Run the below command (based on the file location on your system) on the each worker
node. For example,
chmod 644 /etc/kubernetes/proxy.conf
(...)
```

## Conclusion
L'utilisation de Kubernetes dans une infrastructure apporte une complexité importante, qui augmente avec le nombre de composants du vaste écosystème cloud-native [21] utilisé : chaque cluster est différent, il est important d'en maîtriser les spécificités pour pouvoir évaluer son niveau de sécurité, et les fonctionnalités de sécurité qu’il offre aux applications qu'il héberge.

Pour étudier plus en profondeur et de façon pratique la sécurité de clusters Kubernetes, le lecteur est invité à essayer la dizaine de scénarios d'attaques sur un cluster volontairement vulnérable kubernetes-goat [22].

## Remerciements
Merci à Émilien Gaspar et Laurent Butti pour leurs précieux conseils lors de la préparation de cet article.

Merci au comité de programme SSTIC, aux collègues et spectateurs pour leurs conseils et leur soutien pour la présentation réalisée à SSTIC 2020, qui a servi de base pour cet article.

## Références
[1] Stockage clef-valeur distribué etcd : https://etcd.io/
[2] Documentation Kubernetes : https://kubernetes.io/docs
[3] Opérateur Prometheus : https://github.com/prometheus-operator/prometheus-operator
[4] Collecte de métriques Prometheus : https://prometheus.io/
[5] Documentation RBAC : https://kubernetes.io/docs/reference/access-authn-authz/rbac/
[6] Cluster de développement et test minikube : https://minikube.sigs.k8s.io/docs/
[7] Définition de politiques de sécurité avec Open Policy Agent : https://www.openpolicyagent.org/
[8] Intégration continue / déploiement continu avec Jenkins : https://www.jenkins.io/
[9] Intégration continue / déploiement continu avec ArgoCD : https://argoproj.github.io/argo-cd/
[10] Webshell GoTTY : https://github.com/yudai/gotty
[11] Dépôt d'images Docker open source Harbor : https://goharbor.io/
[12] Tracing distribué Jaeger : https://www.jaegertracing.io/
[13] Service mesh Istio : https://istio.io/
[14] Console d’administration Kubernetes : https://github.com/kubernetes/dashboard
[15] Overlay network Flannel : https://github.com/coreos/flannel
[16] Exemples de NetworkPolicies : https://github.com/ahmetb/kubernetes-network-policy-recipes
[17] Analyse statique d'images Docker avec Clair : https://github.com/quay/clair
[18] Client en ligne de commandes pour Clair, Klar : https://github.com/optiopay/klar/
[19] Revue de configuration d'un Pod avec kubesec : https://github.com/controlplaneio/kubesec
[20] Analyse de configuration d'un cluster avec kube-bench : https://github.com/aquasecurity/kube-bench
[21] Écosystème « cloud-native » : https://landscape.cncf.io/
[22] Environnement Kubernetes vulnérable kubernetes-goat : https://github.com/madhuakula/kubernetes-goat

source : https://connect.ed-diamond.com/MISC/misc-112/cas-pratique-sur-la-securisation-d-un-cluster-kubernetes