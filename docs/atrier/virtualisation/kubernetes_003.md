---
title: Créer un Custom Resource Controller pour Kubernetes
description: 
published: true
date: 2023-04-21T11:18:50.959Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:18:44.979Z
---

# Créer un Custom Resource Controller pour Kubernetes

> Un peu de YAML et 300 lignes de Python pour mieux comprendre et enrichir les Controllers de Kubernetes !

Sous prétexte d’améliorer la robustesse des Services rendus par un cluster Kubernetes, nous allons montrer, étape par étape, comment créer un Custom Controller qui va gérer un nouveau type de ressources, qu’on nommera les CriticalServices.

Tout d’abord, il s’agira d’écrire une application capable d’invoquer l’API server de Kubernetes. Cette application sera ensuite embarquée dans un POD configurable via une ConfigMap.

Pour finir, nous montrerons comment configurer dynamiquement ce Custom Controller en créant nos propres Custom Resources ! Vous verrez que l’objectif est moins complexe qu’il n’y paraît !

> Le code Python présenté dans cet article a été testé avec Python 3.7 et 3.8. Les sources sont disponibles sur GitHub [1].

## 1. Kubernetes, POD et Services
### 1.1 Petit rappel
Parmi tous les types d’objets gérés par Kubernetes, intéressons-nous aux POD et aux Services : si un POD permet d’encapsuler un ou plusieurs Containers, le Service définit l’abstraction (constituée au minimum d’une adresse IP virtuelle) qui permet à des clients d’accéder au service réseau rendu par un POD/Container.

Comme il est recommandé de créer le Service avant le POD [2], allons-y gaiement et analysons le comportement obtenu.

Les deux fichiers YAML suivants décrivent notre Service et notre POD. Rien de compliqué, il s’agit de lancer un serveur NGinx en le rendant accessible depuis l’extérieur du cluster (d’où le choix du type NodePort dans la définition du Service) :

Le fichier de définition du Service se nomme service.yaml et contient les directives suivantes :

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  selector:
    role: frontend
  type: NodePort
  ports:
    - port: 80
```

Appliquons ce fichier et vérifions que le Service ne possède aucun Endpoint :

```
$ kubectl apply -f service.yaml
service/frontend created
$ kubectl describe svc frontend | grep -E ‘(Endpoints|IP)’
IP:                       10.109.48.92
Endpoints:                <none>
```

La définition de notre POD nginx est contenue dans le fichier nginx-pod.yaml suivant :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    role: frontend
spec:
  containers:
    - name: nginx
      image: nginx:1.7.9
      ports:
        - containerPort: 80
```

La valeur des labels correspond bien au champ selector du Service ! Appliquons ce fichier et vérifions que tout est opérationnel :

```
$ kubectl apply -f nginx-pod.yaml
pod/nginx created
$ kubectl describe svc nginx | grep -E ‘(Endpoints|IP|NodePort)’
IP:                       10.109.48.92
NodePort:                 <unset> 32732/TCP
Endpoints:                10.244.3.203:80
```

Le Service possède bien un Endpoint. Si on est connecté sur un nœud du cluster, une requête sur l’adresse IP du Service (ici : 10.109.48.92) permet de vérifier qu’on peut atteindre le serveur NGinx.

```
$ curl 10.109.48.92
< !DOCTYPE html><html>...page d’accueil de nginx...</html>
```

Si on est sur une machine qui est « à l’extérieur » du cluster, on peut joindre NGinx sur le port 32732 (dans notre exemple), mais en utilisant l’adresse IP du nœud (ici : 3.249.248.137) :

```
$ curl 3.249.248.137:32732
< !DOCTYPE html><html>...page d’accueil de nginx...</html>
```

Parfait !

### 1.2 Le problème...
Si vous avez été attentif (ou si vous le savez déjà !), vous avez remarqué que nous avons créé un Service qui, dans un premier temps, n’a pas trouvé de POD correspondant à la valeur de son selector, ainsi le champ Endpoints avait pour valeur `<none>`. Puis quand nous avons démarré un POD correctement labélisé, le Service a rajouté l’adresse IP de ce POD à sa liste d’Endpoints.

C’est comme cela que fonctionnent les Services : ils peuvent au cours de leur vie avoir une liste d’Endpoints vide. Est-ce normal pour un Service d’être dans cette situation ? Parfois oui, lors de sa création par exemple, mais en général ce n’est pas bon signe : un paquet qui va parvenir sur l’adresse IP du Service ne sera pas transmis à un POD et le client va expérimenter des timeouts. Essayons cela en supprimant le POD :

```
$ kubectl delete -f nginx-pod.yaml
pod "nginx" deleted
```
  
Essayez ensuite de vous connecter sur l’adresse IP de Service :

```
$ curl 3.249.248.137:32732
```
  
...le temps passe...
Pas terrible... Je n’ai pas eu la patience d’attendre la fin du timeout ! Maintenant, imaginez que suite à une erreur, un Service « perde » sa relation avec ses POD. Dans le meilleur des cas, votre génial outil de supervision va vous alerter au bout de plusieurs minutes. Pendant ce temps, les clients vont expérimenter des erreurs ou des timeouts. Ne pourrait-on pas améliorer la réactivité de nos outils et limiter les dégâts pour les clients ?

### 1.3 Une solution
Nous proposons dans cet article de montrer comment surveiller les Services du cluster pour y connecter automatiquement un « POD de secours », lorsqu’un Service ne possédera plus d’Endpoints. Il nous faudra pour cela être capables d’invoquer l’API fournie par Kubernetes via son processus kube-apiserver. Il nous faudra aussi définir notre propre type de ressources, c’est-à-dire une Custom Resource, afin de rendre notre application configurable et correctement intégrée avec les outils comme kubectl.

> Par la suite, nous appellerons un Service sans Endpoints un « Service bancal ».

## 2. Création de notre application locale
### 2.1 Comment effectuer un appel à l’API server (en Python)
L’API de Kubernetes est bien documentée [3] et peut s’utiliser en envoyant les requêtes avec un client HTTP. Par exemple à l’aide du module requests de Python, nous pouvons écrire le code suivant pour obtenir la liste des POD correspondant à une certaine valeur de label et pour un espace de nommage (namespace) particulier :

```python
import requests
import json
 
base_url = "http://apiserver:6443"
namespace = "default"
label = "key=value"
 
url = f"{base_url}/api/v1/namespaces/{namespace}/pods?labelSelector={label}"
r = requests.get(url)
response = r.json()
for p in response['items']]:
    print(p['metadata']['name']
Maintenant que nous avons compris comment appeler l’API à partir d’une requête HTTP, nous allons plutôt utiliser le module officiel pour Python [4] qui se nomme kubernetes (à installer avec pip). Le code précédent s’écrit maintenant :

from kubernetes import client
 
v1 = client.CoreV1Api()
print("Listing pods with their IPs:")
ret = v1.list_pod_for_all_namespaces()
for i in ret.items:
    print(f"{i.status.pod_ip}\t{i.metadata.namespace}\t{i.metadata.name}")
```
  
Les deux exemples de code précédents occultent juste une chose : comment fournit-on nos credentials pour être autorisé à interroger l’API server ?

Avec le module kubernetes pour Python, nous allons utiliser le code suivant qui charge par défaut la configuration issue de notre fichier ~/.kube/config, mais qui nous permet aussi de spécifier notre propre fichier :

```python
import sys
from kubernetes import config
 
if len(sys.argv) > 1:
    config.load_kube_config(sys.argv[1])
else:
    config.load_kube_config()
```

> Si le Client affiche une erreur relative à l’invalidité du certificat fourni par l’API server, vous pouvez ajouter l’option insecure-skip-tls-verify: true dans le fichier de configuration. Si vous obtenez un warning de la part la librairie urllib3 indiquant que « vérifier un certificat est toujours conseillé », et que vous souhaitez supprimer ce warning, ajoutez les lignes suivantes dans votre code :
>
>```python
>import urllib3
>urllib3.disable_warnings()
>```

### 2.2 Surveiller les objets du cluster
N’oublions pas notre objectif : détecter le plus rapidement possible la présence de Services bancals. Nous ne souhaitons pas effectuer un polling régulier, ce qui est une mauvaise pratique en général. Nous allons plutôt profiter d’une fonctionnalité de l’API server qui nous permet de surveiller (watch) les modifications apportées à un ou plusieurs objets : ici, ce seront des Services.

Le code ci-dessous est une boucle infinie qui obtient tout d’abord la liste de tous les Services, tous namespaces confondus, puis qui reçoit les modifications apportées aux Services. Chaque événement (event) rapporté est un document JSON qui indique par son champ type si le Service a été ajouté (ADDED), détruit (DELETED) ou modifié (MODIFIED).

Rien de plus simple alors que de vérifier si un Service possède des Endpoints ou pas. Le fichier watch_service.py est un exemple de code qui effectue un « watch » :

```python
import sys
import logging
from kubernetes import client, config, watch
 
if len(sys.argv) > 1:
    config.load_kube_config(sys.argv[1])
else:
    config.load_kube_config()
 
logging.basicConfig(level=logging.INFO)
 
v1 = client.CoreV1Api()
w = watch.Watch()
 
for event in w.stream(v1.list_service_for_all_namespaces):
    metadata = event['object'].metadata
    spec = event['object'].spec
    logging.info(f"Event {event['type']}, Service Name: {metadata.name}, "
                 f"Service Type: {spec.type}, Namespace: {metadata.namespace}")
 
    # Ignore les Services de type ExternalName ou ceux qui n'ont pas de
    # "selector" comme l'API server ou ceux qui sont en cours de suppression
    selector = spec.selector
    if selector is None or spec.type == 'ExternalName' or event['type'] == 'DELETED':
        logging.info(f"Skip Service {metadata.name}")
        continue
 
    # Cherche les POD correspondant au "selector"
    selectors = [f"{k}={v}" for k, v in spec.selector.items()]
    pods = v1.list_namespaced_pod(metadata.namespace , watch=False,
                                  label_selector=','.join(selectors))
    if not len(pods.items):
        logging.warning(f"Service {metadata.name} from Namespace {metadata.namespace} "
                        f"has no selected POD")
```

Quand on exécute ce script, on obtient la liste de tous les Services actuellement définis et un message doit indiquer que le Service frontend ne possède pas de POD (vous avez détruit son POD en section 1.2) : il est bancal !

```
$ python3.8 watch_service.py
...
WARNING:root:Service frontend from Namespace default has no selected POD
...
```

Que se passe-t-il si nous créons le POD à partir d’un autre terminal ? Eh bien, rien : si vous appliquez le fichier nginx-pod.yaml pour créer le POD comme précédemment, le Service sera fonctionnel, pourtant vous n’aurez reçu aucun nouvel événement susceptible de vous avertir que le Service possède maintenant un Endpoint !

### 2.3 Un peu de parallélisme
Nous pouvons donc surveiller les modifications apportées directement aux Services, c’est à dire, savoir si un Service est créé, détruit ou bien si sa spécification est modifiée. Mais pour savoir si un nouveau POD est supprimé ou ajouté aux Endpoints d’un Service, nous devons aussi surveiller les POD !

Aïe, pour faire deux « watches », il nous faut deux boucles ? Ça sent le parallélisme… Quelles sont nos possibilités en Python : l’idéal serait d’utiliser les co-routines et d’employer les mots-clés async et await. Mais il semble que le client kubernetes pour Python ne soit pas compatible avec ce modèle de programmation. On trouve bien des projets qui proposent une réécriture du module standard pour le rendre compatible avec ce modèle, mais je préfère la stabilité et donc, je préfère utiliser le module officiel. On trouve aussi des exemples qui ne « marchent » pas en réalité – je ne dénoncerai aucun blog ! Dès lors, entre les modes multithread et multiprocess, je choisis... le multiprocess ! L’utilisation du module multiprocess de la librairie standard permet de réaliser cela simplement. Le fichier watch_service_pods_v1.py est notre première version de notre implémentation multiprocess :

```python
# on a ici les initialisations (import, variables globales) du fichier précédent
 
def watch_services():
    # on a ici la boucle « for » du fichier précédent
    ...
 
def watch_pods():
    w = watch.Watch()
      for event in w.stream(v1.list_pod_for_all_namespaces):
        metadata = event['object'].metadata
        spec = event['object'].spec
        logging.info(f"Event {event['type']}, POD Name: {metadata.name}, "
                     f"Namespace: {metadata.namespace}")
 
        # Si le type de l'événement est ADDED ou MODIFIED il faut vérifier si un Service
        # précédemment bancal dispose maintenant d’Endpoints
        # Si le type de l'événement est DELETED, il faut vérifier si un Service est devenu bancal
        if event['type'] == 'ADDED' or event['type'] == 'MODIFIED':
            pass
        elif event['type'] == 'DELETED':
            pass
 
proc_svc = Process(target=watch_services)
proc_pod = Process(target=watch_pods)
proc_svc.start()
proc_pod.start()
proc_svc.join()
proc_pod.join()
```

C’est globalement le même code que précédemment : nous avons ajouté une fonction watch_pods() qui surveille les modifications apportées aux POD, mais sans prendre de décisions particulières. Les fonctions watch_services() et watch_pods() sont exécutées dans des processus différents. On peut vérifier que le code fonctionne et affiche les événements correctement :

```
$ python3.8 watch_services_pods_v1.py
...
WARNING:root:Service frontend from Namespace default has no selected POD
```

Si dans un autre terminal, vous créez le POD NGinx manquant :

```
$ kubectl apply -f nginx-pod.yaml
pod/nginx created
```

Vous verrez apparaître les lignes suivantes :

```
INFO:root:Event ADDED, POD Name: nginx, Namespace: default
INFO:root:Event MODIFIED, POD Name: nginx, Namespace: default
```

Mais rien ne nous prévient que le Service frontend possède maintenant un Endpoint !

Il faut compléter l’algorithme de détection des Services bancals en considérant que si un POD est détruit ou modifié, un Service peut devenir bancal alors que si un POD est créé, un Service bancal peut maintenant posséder un Endpoint !

Comme nous sommes dans un environnement multiprocessus, on décide que les fonctions watch_services() et watch_pods() vont publier leurs événements dans une Queue (objet partagé) et qu’un troisième processus aura pour mission de vider cette Queue et de faire les tests de cohérence entre Services et POD.

Pour pouvoir afficher un message indiquant qu’un Service possède dorénavant un Endpoint, nous mémoriserons dans la liste lame_svc les Services bancals. Le fichier watch_service_pods_v2.py définit donc 3 fonctions lancées chacune par un processus :

```python
def watch_services(q):
    w = watch.Watch()
    for event in w.stream(v1.list_service_for_all_namespaces):
        q.put(event)
 
def watch_pods(q):
    w = watch.Watch()
    for event in w.stream(v1.list_pod_for_all_namespaces):
        q.put(event)
 
def handle_events(q):
    lame_svc = []    # Liste des Services bancals
 
    while True:
        event = q.get()
        ...
 
q = Queue()
procs = [Process(target=watch_services, args=(q,)),
         Process(target=watch_pods, args=(q,)),
         Process(target=handle_events, args=(q,))]
[p.start() for p in procs]
[p.join() for p in procs]
```

Le code de la fonction handle_events() [4] a pour vocation de déterminer si l’événement reçu rend un Service bancal ou bien le rend opérationnel.

### 2.4 Un peu de souplesse et de paramètres
Pour finir, on aimerait ajouter un peu de paramétrisation à notre script Python. En particulier, on introduit la prise en compte de 2 variables d’environnement nommées LOG_LEVEL et NAMESPACES qui permettent de choisir le niveau de verbosité et de sélectionner les namespaces concernés par l’analyse des Services bancals. La valeur de NAMESPACES est une liste de nom de namespaces séparés par des virgules.

La nouvelle version de notre script s’appelle watch_services_pods_v3.py [4] et s’utilise ainsi :

```
$ export LOG_LEVEL=info
$ export NAMESPACES=default,kube-system
$ python3.8 watch_services_pods_v3.py
...
WARNING:root:Service frontend from Namespace default has no selected POD
```

## 3. Intégration de l’application dans un POD
Notre petite application fonctionne localement, mais pour des raisons de cohérence (et pour prévoir la suite !), nous devons la faire exécuter dans un POD qui pourra être contrôlé par un Deployment assurant ainsi une meilleure robustesse.

### 3.1 Création de l’image pour le Container
Pour créer un POD, il faut une image de Container. La création de l’image est réalisée via le fichier Dockerfile-local suivant :

```docker
FROM python:3.8-slim
ADD requirements.txt /
RUN pip install -r /requirements.txt
ADD watch_service_pods_v3.py /
ENTRYPOINT ["python", "/watch_service_pods_v3.py"]
```

Construisons et testons notre image localement :

```
$ docker build -t watch_service_local -f Dockerfile-local .
Successfully tagged watch_service_local:latest
 
$ docker run -it -v `pwd`/ma_conf:/ma_conf watch_service /ma_conf
WARNING:root:Service kubernetes-dashboard from Namespace kube-system has no selected POD
WARNING:root:Service super-service from Namespace linux-mag has no selected POD
```

Remarquez que nous avons passé le fichier de configuration ./ma_conf, qui définit l’accès au cluster Kubernetes comme paramètre de l’entrypoint, après avoir réalisé un montage de ce fichier dans le Container.

Notre image est fonctionnelle, passons à la création du POD...

### 3.2 Création d’un POD et des ressources connexes
Avant d’écrire le fichier YAML qui va décrire le Deployment du POD utilisant cette image, nous allons anticiper deux problèmes : comment accéder à l’API server depuis un POD et comment régler les autorisations d’accès?

#### 3.2.1 Accès à l’API server depuis un POD
Comment notre script Python va-t-il pouvoir se connecter à l’API server du cluster ? Si la solution du fichier de configuration peut encore être utilisée, la documentation officielle expose les différentes solutions [5]. Une solution utilisant un Container « sidecar » qui est utilisé comme Proxy entre l’application et l’API server est décrite dans cet article [6]. Mais la documentation indique aussi une solution pour les utilisateurs de la librairie cliente Python : il suffit d’appeler la bonne fonction de configuration !

Nous créons une nouvelle version watch_service_pods_v4.py. La différence avec la version v3 est affichée ci-dessous :

```
$ diff *v3* *v4*
21c21,24
<     config.load_kube_config()
---
>     try:
>         config.load_kube_config()
>     except:
>         config.load_incluster_config()
```

En cas d’échec du chargement du fichier de configuration par défaut, on appelle la fonction load_incluster_config() !

Il suffit de modifier le Dockerfile-local précédent en remplaçant v3 par v4 (on appellera le nouveau fichier Dockerfile-k8s). Puis on recrée l’image. On pensera bien ensuite à « pousser » notre image sur une Registry accessible par notre cluster. On suppose que cette nouvelle image s’appelle service-watcher-controller:v1 :

```
$ docker build -t watch_service_local:v1 -f Dockerfile-k8s .
Successfully tagged watch_service_local:v1
```

#### 3.2.2 Réglages des autorisations
Notre deuxième souci concerne les autorisations : pour pouvoir consulter les descriptions des Services et des POD, notre application doit posséder les bons privilèges. Ce besoin passe par la création d’un ServiceAccount, d’un ClusterRole et d’un ClusterRoleBinding. Comme nous commençons à créer des objets dans Kubernetes, nous en profitons pour créer aussi un nouveau namespace qui s’appelle linux-mag ! Le namespace est décrit dans le fichier linux-mag-ns.yaml suivant :

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: linux-mag
```

Les autres objets sont décrits dans le fichier authorization.yaml :

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: linux-mag
  name: service-watcher
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: service-watcher-runner
rules:
  - apiGroups: [""]
    resources: ["services", "pods"]
    verbs: ["get", "list", "watch" ]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: run-service-watcher
subjects:
  - kind: ServiceAccount
    name: service-watcher
    namespace: linux-mag
roleRef:
  kind: ClusterRole
  name: service-watcher-runner
  apiGroup: rbac.authorization.k8s.io
```

Appliquons ces définitions :

```
$ kubectl apply -f linux-mag-ns.yaml
namespace/linux-mag created
 
$ kubectl apply -f authorization.yaml
serviceaccount/service-watcher created
clusterrole.rbac.authorization.k8s.io/service-watcher-runner created
clusterrolebinding.rbac.authorization.k8s.io/run-service-watcher created
```

Nous pouvons maintenant définir le fichier qui va déployer notre POD (fichier controller-depl.yaml) :

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: linux-mag
  name: service-watcher
spec:
  selector:
    matchLabels:
      app: service-watcher
  template:
    metadata:
      labels:
        app: service-watcher
    spec:
      serviceAccount: service-watcher
      containers:
      - name: service-watcher
        image: majetraining/service-watcher-controller:v1
```

Créons ce Deployment et testons :

```
$ kubectl apply -f controller-depl.yaml
deployment/service-watcher created
 
$ kubectl get pods | grep service-watcher
service-watcher-7f445449f9-7t99v    1/1     Running   0          3h37m
 
$ kubectl logs service-watcher-7f445449f9-7t99v
WARNING:root:Service kubernetes-dashboard from Namespace kube-system has no selected POD
WARNING:root:Service super-service from Namespace linux-mag has no selected POD
```

Les logs obtenus montrent bien que notre application détecte les Services bancals !

### 3.3 Paramétrisation via une ConfigMap
Rappelons que notre script Python peut être configuré via des variables d’environnement. Nous pourrions définir ces variables directement, avec leurs valeurs, dans le fichier controller-depl.yaml, mais nous préférons découpler les valeurs en les passant au POD via une ConfigMap. Le fichier configmap.yaml est un exemple d’une telle ConfigMap :

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: linux-mag
  name: service-watcher-env-config
data:
  LOG_LEVEL: INFO
  NAMESPACES: default
```

Pour utiliser cette ConfigMap, il faut modifier le fichier de déploiement qui devient le fichier controller-depl-cm.yaml : il suffit de rajouter les 3 lignes suivantes après la ligne « image » à la fin du fichier controller-depl.yaml :

        image: majetraining/service-watcher-controller:v1
        envFrom:
        - configMapRef:
            name: service-watcher-env-config
On crée la ConfigMap, puis on détruit le Deployment courant pour le remplacer par le nouveau :

```
$ kubectl apply -f configmap.yaml
configmap/service-watcher-env-config created
 
$ kubectl delete -f controller-depl.yaml
deployment/service-watcher deleted
 
$ kubectl apply -f controller-depl-cm.yaml
deployment/service-watcher created
```

On peut maintenant reconfigurer le POD en modifiant les valeurs de la ConfigMap. Seul hic : quand une ConfigMap est modifiée, les POD qui l’utilisent doivent être détruits et relancés pour prendre en compte la modification. C’est d’ailleurs cette contrainte qui est donnée en exemple dans cet article [6] et qui a inspiré notre propre Custom Controller. Justement, venons-en au Custom Controller… !

## 4. Création d’une « Custom Resource »
Dorénavant, nous pouvons surveiller les Services bancals ! Mais parmi tous les Services définis dans notre cluster, nous considérons que seuls quelques-uns sont véritablement « critiques » et doivent toujours être associés à au moins un POD.

Notre Controller devra alors déterminer si un Service bancal est un Service critique et dans ce cas, il créera un POD supplétif qui pourra être sélectionné par ce Service bancal, le rendant ainsi disponible ! Si le Service bancal (qui ne l’est donc plus) retrouve à nouveau des POD applicatifs « normaux », alors notre Controller n’oubliera pas de supprimer le POD supplétif qu’il a lancé !

### 4.1 Définition d’un Service critique
Avant de définir formellement un nouveau type de ressources que nous appellerons CriticalService, donnons un exemple d’un tel service critique. Si nous voulons nommer ce CriticalService frontend et l’associer aux Services définis dans le namespace default qui sont labélisés avec la paire clé/valeur role=frontend, on devra écrire le fichier suivant (fichier critical-service.yaml) :

```yaml
apiVersion: mycrd.com/v1
kind: CriticalService
metadata:
  name: frontend
spec:
  namespace: default
  matchLabels:
    - key: role
      value: frontend
```

La valeur de la clé apiVersion est arbitraire, mais sera expliquée dans le paragraphe suivant.

Nous ne pouvons pas appliquer cette définition, car l’API server ne connaît pas encore ce nouveau type de ressource, comme le montre la tentative suivante :

```
$ kubectl apply -f critical-service.yaml
Error from server (NotFound): error when creating "critical-service.yaml": the server could not find the requested resource (post criticalservices.mycrd.com)
```

### 4.2 Définition du nouveau type de ressource CriticalService
La documentation de référence pour la création d’une ressource personnalisée [7] nous explique comment structurer notre définition (fichier critical-service-crd.yaml) :

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: criticalservices.mycrd.com
spec:
  group: mycrd.com
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                namespace:
                  type: string
                matchLabels:
                  type: array
                  items:
                    type: object
                    properties:
                      key:
                        type: string
                      value:
                        type: string
              required: ["matchLabels"]
  scope: Cluster
  names:
    plural: criticalservices
    singular: criticalservice
    kind: CriticalService
    shortNames:
      - crisvc
```

C’est très verbeux et encore, nous n’avons pas défini les champs optionnels, comme les champs description. Voici quelques explications sur le contenu de ce fichier :

|Nom du champ|Description|
|:----------:|:----------|
|`metadata.name`|La valeur de `metadata.name` est de la forme `<nom>.<spec.group>`.|
|`spec.group`|Doit ressembler à un nom de domaine (ici : `mycrd.com`). On retrouve cette valeur comme valeur de `apiVersion` dans la définition d’un `CriticalService`.|
|`spec.versions[]`|Il est possible de servir plusieurs versions. Il faut que le champ `served` soit `true` pour que la version puisse être utilisée. Le champ `storage` indique quelle version est utilisée pour stocker la ressource dans le key-store (etcd), ce qui permettra de réaliser des conversions en cas de futurs changements de version [9]. La valeur de la version apparaît aussi dans la valeur de `apiVersion` dans la définition d’un `CriticalService`.|
|`spec.versions[].schema`|Permet de définir un schéma pour valider les futures définitions des objets de type `CriticalService` (la syntaxe de référence est disponible sur [8]). Dans notre cas, nous indiquons qu’un `CriticalService` pourra définir un champ `namespace` de type `string` et un champ `matchLabels`, obligatoire (`required`), qui sera une liste de paires clés/valeurs. Notez que si un champ est de type `object`, alors le champ `properties` est obligatoire. Si un champ est de type `array`, alors le champ `items` est obligatoire.|
|`spec.scope`|Indique si la ressource est globale (`Cluster`) ou non (`Namespaced`).|
|`spec.names`|Donne le nom de la ressource, au singulier, au pluriel, pour la valeur du champ `kind` et ses éventuelles abréviations.|

Nous pouvons maintenant définir notre Custom Resource et définir un `CriticalService` :

```
$ kubectl apply -f critical-service-crd.yaml
customresourcedefinition.apiextensions.k8s.io/criticalservices.mycrd.com created
 
$ kubectl api-resources | grep crisvc
criticalservices       crisvc      mycrd.com       false       CriticalService
 
$ kubectl apply -f critical-service.yaml
criticalservice.mycrd.com/frontend created
```

## 5. Gestion applicative des CriticalServices
Il nous reste la partie purement applicative qui consiste à modifier notre script watch_service_pods_v4.py (et notre image) pour appliquer les algorithmes suivants :

- il faut s’abonner aux modifications apportées aux objets de type CriticalService ;
- si un Service est détecté comme étant bancal et que c’est un CriticalService, il faut démarrer un nouveau POD à partir d’un modèle de POD (un PODTemplate) déjà prévu, puis il faut faire en sorte que ce POD soit sélectionné par le Service ;
- si un Service qui est un CriticalService reçoit un nouvel Endpoint et que le seul Endpoint qu’il possédait était notre POD, alors il faut détruire ce POD.

### 5.1 Modification des autorisations
Pour implémenter ces modifications, notre nouveau script watch_service_pods_v5.py doit posséder de nouveaux droits. En particulier, il doit pouvoir lire les ressources CriticalService, il doit pouvoir créer et détruire des POD et lire les PODTemplates. Le ClusterRole service-watcher-runner doit être enrichi avec les autorisations suivantes (fichier authorization-v5.yaml) :

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: service-watcher-runner
rules:
  - apiGroups: [""]
    resources: ["services"]
    verbs: ["get", "list", "watch" ]
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "watch", "create", "delete"]
  - apiGroups: [""]
    resources: ["podtemplates"]
    verbs: ["get"]
  - apiGroups: ["mycrd.com"]
    resources: ["criticalservices"]
    verbs: ["watch"]
```

Ensuite :

```
$ kubectl apply -f authorization-v5.yaml
clusterrole.rbac.authorization.k8s.io/service-watcher-runner configured
```

### 5.2 Création du modèle de POD
Pour apporter de la souplesse à notre Custom Controller, nous allons utiliser un modèle de POD (un PODTemplate) pour créer les POD dynamiquement. Par défaut, le nom du modèle sera critical-service-pod-template. Mais il serait facile de rajouter une clé dans notre ConfigMap pour paramétrer ce nom !

Nous proposons d’utiliser une image de base d’un serveur NGinx, mais il serait plus judicieux (et plus facilement paramétrable, peut-être) d’utiliser notre propre image. Quoi qu’il en soit, notre PODTemplate ressemble à ceci (fichier critical-service-pod-template.yaml) :

```yaml
apiVersion: v1
kind: PodTemplate
metadata:
  namespace: linux-mag
  name: critical-service-pod-template
template:
  metadata:
    name: critical-service-pod
  spec:
    containers:
      - name: nginx
        image: nginx:1.7.9
```

Appliquons ce fichier pour définir notre modèle :

```
$ kubectl apply -f critical-service-pod-template.yaml
podtemplate/critical-service-pod-template created
```

> Nous supposons que le POD qui sera lancé à partir de ce PODTemplate écoutera bien sur le port attendu par le Service.

### 5.3 Modification de notre script
Nous vous rappelons que le code intégral du code est disponible sur le dépôt GitHub [4]. Nous nous attachons ici à démontrer comment appeler l’API server via la librairie kubernetes pour Python.

#### 5.3.1 Prise en compte des CriticalServices
Avec l’API Python, les objets qui correspondent aux Custom Resources sont gérés différemment. En particulier, il n’est pas possible d’utiliser la notation pointée pour accéder aux clés du dictionnaire event[‘object’]. Nous devons écrire event[‘object’][‘metadata’][name’] au lieu de event[‘object’].metadata.name. Dommage...

Pour gérer les CriticalServices, nous définissons un nouveau processus qui va appeler la fonction watch_critical_services() :

```python
...
custom_api = client.CustomObjectsApi()
...
def watch_critical_services(q):
    w = watch.Watch()
    for event in w.stream(custom_api.list_cluster_custom_object, group="mycrd.com",
                          version="v1", plural="criticalservices"):
        metadata = event['object']['metadata']
        logging.info(f"Event {event['type']}, CriticalService Name: {metadata['name']}")
        q.put(event)
...
procs = [
            ...
            Process(target=watch_critical_services, args=(q,)),
]
...
```

La fonction handle_events() doit être modifiée : elle va gérer un dictionnaire contenant les noms des CriticalServices et elle doit traiter les événements relatifs à ce type d’objets :

```python
def handle_events(q):
    lame_svc = []
    critical_svc = {}
 
    while True:
        event = q.get()
        if not hasattr(event['object'], 'kind'):
            metadata = event['object']['metadata']
            spec = event['object']['spec']
 
            if event['object']['kind'] != 'CriticalService':
                continue
 
            # Mémorise le CriticalService ou bien le met à jour ou le détruit
            if event['type'] == 'DELETED':
                if metadata['name'] in critical_svc:
                    _delete_critical_svc(metadata['name'], spec)
                    del critical_svc[metadata['name']]
            else: # ADDED ou MODIFIED
                critical_svc[metadata['name']] = spec
                _check_critical_svc(metadata['name'], spec)
            continue
 
    # traitement des Services et des POD
    ...
```

#### 5.3.2 Création d’un POD par défaut et association au CriticalService
La fonction _create_default_pod() a pour rôle la création d’un nouveau POD basé sur le PODTemplate dont le nom peut être passé via une variable d’environnement.

Le POD qui sera créé doit appartenir au même namespace que celui du Service bancal et doit aussi posséder les labels qui feront qu’il sera automatiquement sélectionné par le Service. Le code en est le suivant :

```python
def _create_default_pod(svc):
    '''Pour créer un POD, nous devons déjà récupérer le PODTemplate, puis nous donnerons
       au POD les labels attendus par le Service ainsi qu'une Annotation
       qui nous permettra de le repérer plus facilement.'''
    metadata = svc.metadata
    logging.info(f"Create Default POD from PODTemplate {pod_template} for Service "           
                 f"{metadata.namespace}/{metadata.name}")
 
    resp = None
    try:
        resp = v1.read_namespaced_pod_template(name=pod_template, namespace=pod_template_ns)
    except ApiException as e:
        logging.error("read_namespaced_pod_template error: %s" % e)
        return
 
    # Création de la Spec du POD à lancer
    pod_manifest = {
        'apiVersion': 'v1',
        'kind': 'Pod',
        'metadata': {
            'namespace': metadata.namespace,
            'name': pod_name_prefix + '-' + metadata.name,
            'labels': svc.spec.selector,
            'annotations': { 'service-watcher': 'owned' },
        },
        'spec': resp.template.spec
    }
 
    try:
        resp = v1.create_namespaced_pod(body=pod_manifest, namespace=metadata.namespace)
        logging.info("POD created")
    except ApiException as e:
        logging.error("create_namespaced_pod error: %s" % e)
```

Nous avons ajouté une Annotation à notre POD ce qui nous permettra de repérer nos POD plus facilement. La valeur par défaut de la variable pod_name_prefix est service-watcher, ce préfixe sert à définir le nom du POD créé.

#### 5.3.3 Destruction du POD par défaut
Détruire est toujours plus facile que créer ! Le code de la fonction _delete_default_pod() détruit le POD en utilisant le couple namespace/nom_du_POD (on aurait pu aussi utiliser l’Annotation placée précédemment) :

```python
def _delete_default_pod(svc):
    '''Soit un Service est détruit, soit un POD a été modifié: il faut vérifier
       si le Service défini par "metadata" possède un POD par défaut.
       Si tel est le cas, il faut le détruire.'''
    metadata = svc.metadata
    logging.info(f"Delete Default POD for Service {metadata.namespace}/{metadata.name}")
 
    resp = None
    try:
        # On essaye toujours la destruction...
        resp = v1.delete_namespaced_pod(name=pod_name_prefix + '-' + metadata.name,
                                        namespace=metadata.namespace)
        logging.info("POD deleted")
    except ApiException as e:
        logging.error("delete_namespaced_pod error: %s" % e)
```

#### 5.3.4 Intégralité du code et nouvelle image
Nous vous renvoyons au code du fichier watch_service_pods_v5.py (environ 300 lignes de Python) disponible sur le dépôt [4]. Les fonctions de gestion des événements ont aussi été modifiées pour gérer correctement les cas de création ou de destruction des POD par défaut.

Pour utiliser la nouvelle version de notre « Service Watcher Controller », il faut reconstruire son image en suivant les étapes suivantes :

- modifier le fichier Dockerfile-k8s pour y indiquer la v5 ;
- reconstruire l’image ainsi :

```
$ docker build -t majetraining/service-watcher-controller:v5 -f Dockerfile.k8s-v5 .
$ docker push majetraining/service-watcher-controller:v5
```

- modifier le fichier controller-depl-cm.yaml pour qu’il fasse référence à notre v5, puis appliquer ce fichier avec la commande kubectl apply.

### 5.4 Tests
Il est temps de réaliser quelques tests ! Nettoyons les Services et les POD du namespace default. Nous supposons que nous n’avons pas défini de CriticalService :

```
$ kubectl get crisvc
No resources found in default namespace.
```

Nous définissons alors le Service suivant (fichier service.yaml) :

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
  labels:
    role: frontend
spec:
  selector:
    role: frontend
  type: NodePort
  ports:
    - port: 80
```

Nous pouvons créer le Service et vérifier qu’il ne possède pas d’Endpoints :

```
$ kubectl apply -f service.yaml
service/frontend created
 
$ kubectl describe svc frontend | grep Endpoints
Endpoints:                <none>
```

Vérifions que le PODTemplate de la section 5.2 est toujours présent :

```
$ kubectl get podtemplate -n linux-mag
NAME                            CONTAINERS   IMAGES        POD LABELS
critical-service-pod-template   nginx        nginx:1.7.9   <none>
```

Vérifions aussi que notre Custom Controller est démarré grâce au fichier controller-depl-cm-v5.yaml :

```
$ kubectl get deploy -n linux-mag
NAME              READY   UP-TO-DATE   AVAILABLE   AGE
service-watcher   1/1     1            1           48m
```

Ajoutons maintenant la définition du CriticalService défini en section 4.1 :

```
$ kubectl apply -f critical-service.yaml
criticalservice.mycrd.com/frontend created
```

Normalement, un POD avec le nom service-watcher-frontend a été créé pour devenir un Endpoint du Service frontend :

```
$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
...
service-watcher-frontend            1/1     Running   0          12s
 
$ kubectl describe svc frontend | grep Endpoints
Endpoints:                10.244.3.6:80
```

À l’aide du fichier nginx-pod.yaml suivant, ajoutons maintenant un POD qui possède les labels attendus par le Service :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    role: frontend
spec:
  containers:
    - name: nginx
      image: nginx:1.7.9
      ports:
- containerPort: 80
```

Normalement, le POD nommé service-watcher-frontend créé par notre Custom Controller est détruit et c’est le nouveau POD qui apparaît dans la valeur des Endpoints !

```
$ kubectl describe svc frontend | grep Endpoints
Endpoints:                10.244.3.7:80
```

### 5.5 Diagramme récapitulatif
Au terme de cet article, nous avons créé une dizaine d’objets de types différents (nous n’avons pas mentionné le ReplicaSet qui s’intercale entre le Deployment et le POD). Le diagramme de la figure 1 résume les interactions entre ces objets : oui, nous avons créé toute cette architecture !

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/gnu-linux-magazine/glmf-243/crd.png">

Fig. 1 : Notre architecture finale.

## Conclusion
Kubernetes est considéré comme un orchestrateur riche et puissant, mais plutôt complexe. Cependant, nous espérons qu’au terme de cet article, nous avons réussi à démystifier cette relative complexité en montrant comment intégrer, plutôt facilement, notre propre code à cet environnement. Bien sûr, si cela fut possible, c’est que l’architecture de Kubernetes est bien pensée – mais qui en aurait douté ?

Nous avons donc appris qu’écrire un code qui communique avec l’API server de Kubernetes est une chose aisée, ce qui permet de contrôler les ressources du cluster, pour peu qu’on en ait les droits !

Pour finir, je vous propose des pistes d’amélioration pour notre Custom Controller :

1. S’il est possible d’intégrer du code écrit en Python, Go reste le langage privilégié dans l’écosystème de Kubernetes. Si nous l’avions utilisé, nous n’aurions pas eu à écrire un script multiprocess, les Go-Routines auraient été idéales.
1. Dans notre exemple, le POD créé à partir du PODTemplate n’est pas paramétrable : comment lui passer le contenu de la page par défaut à afficher (une page du style « Site en maintenance », par exemple) ?
1. Nous avons supposé que les Services relayaient les paquets sur le port 80 des POD listés dans leurs Endpoints : comment adapter notre Custom Controller (et le PODTemplate) pour prendre en compte la véritable valeur de ce TargetPort ?

À vos claviers...

## Références
[1] Dépôt GitHub du projet : https://github.com/majeinfo/critsvc-k8s-controller
[2] Les « bonnes pratiques » pour Kubernetes : https://kubernetes.io/docs/concepts/configuration/overview/
[3] Documentation de l’API de Kubernetes : https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md
[4] Les sources et les exemples de l’API officielle pour Python : https://github.com/kubernetes-client/python/
[5] Les différentes méthodes pour accéder à l’API server: https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/
[6] Comment accéder à l’API server depuis un POD à l’aide d’un Container « sidecar » qui fait office de proxy : https://www.magalix.com/blog/extending-the-kubernetes-controller
[7] Comment créer une Custom Resource : https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#customresourcedefinition-v1-apiextensions-k8s-io
[8] Spécification des schémas de validation pour JSON : http://json-schema.org/
[9] Comment gérer les changements de version des Custom Resources : https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definition-versioning/


source : https://connect.ed-diamond.com/GNU-Linux-Magazine/glmf-243/creer-un-custom-resource-controller-pour-kubernetes