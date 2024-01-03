---
title: Déploiement d’une page de maintenance sur OpenShift via GitLab
description: 
published: true
date: 2023-07-03T13:22:04.464Z
tags: 
editor: markdown
dateCreated: 2023-07-03T13:08:01.186Z
---

# Déploiement d’une page de maintenance sur OpenShift via GitLab

De nos jours, nous désirons que l’expérience utilisateur soit la moins impactée possible par les mises à jour ou les changements critiques effectués sur nos applications. Notre réflexion porte donc naturellement sur la manière d’effectuer ces changements avec 0 downtime. Cependant, posséder une page de maintenance prête à être déployée peut s’avérer très utile et c’est ce dont nous allons parler dans cet article. Allons-y !

## Pourquoi mettre en place une page de maintenance ?
Je vais aborder dans cette partie un cas de figure rencontré au cours de mes expériences de consultant.

Un client avait mis en place plusieurs applications ayant des liens avec du legacy pour certaines tâches. Ce lien était tel que l'indisponibilité du legacy était préjudiciable pour certaines actions sur ces applications. Lesdites applications reposaient sur de l’OpenShift et nous utilisions la stratégie du rolling update pour les mises jours visant le 0 downtime. Ce n’était pas le cas de la partie legacy pour laquelle, des phases de maintenance étaient observées. On a donc décidé de mettre en place un moyen de renvoyer aux clients une page de maintenance lors de la consultation de certains liens. Ceci dans l’optique de mettre certaines parties des applications hors service lors de phases de maintenance de la partie legacy.

Nous avions pensé tout d’abord à effectuer des redirections via la configuration DNS, cependant nous étions dans une phase de mise en place d’un WAF externe à notre cloud provider et tout le trafic ne transitait pas encore par le même service d’entrée.

Ainsi est venu l’idée de déployer temporairement un pod sur le cluster OpenShift dans le même namespace que le service à passer en mode de maintenance. Ce pod devait simplement renvoyer une page de maintenance.

Voici le cahier des charges que nous nous étions fixés :

- Rediriger le flux vers le pod distribuant la page de maintenance pour l’ensemble des services ayant un lien avec la partie du legacy qui est indisponible ;
- S’assurer que lorsque le service est indisponible, l’utilisateur ne reçoit pas de code 5xx ;
- Une fois le service de nouveau accessible, re-basculer les requêtes utilisateurs vers ce service.

![mise_en_place_mode_maintenance.png](/assets/img/virtualisation/tempo/mise_en_place_mode_maintenance.png)

*(Figure. Mise en place du mode maintenance)*

Comme nous pouvons l’observer dans le schéma précédent, nous souhaitons créer un nouveau service vers lequel est redirigé le flux reçu par la Route A. Ce service doit cibler le pod nommé "Maintenance Page" déjà abordé précédemment.

Nous allons à présent nous concentrer sur les étapes visant à mettre en place ce processus.

## Comment mettre en place une page de maintenance?
### Phase 1: Build de l’image maintenance-page
Tout d’abord nous souhaitons construire l’image et la stocker dans un registre docker.

Pour faire cette action OpenShift permet de "builder" les images suivant la stratégie définie dans l’objet BuildConfig. Voici le BuildConfig que nous utilisons :

`buildConfig-template.yaml` :

```yaml
apiVersion: "build.openshift.io/v1"
kind: "BuildConfig"
metadata:
  labels:
    build: "maintenance-page"
    version: "v1"
  name: "maintenance-page"
spec:
  failedBuildsHistoryLimit: 5
  nodeSelector: null
  output:
    to:
      kind: "ImageStreamTag"
      name: "maintenance-page:v1"
    pushSecret:
      name: "docker-registry-secret"
  source:
    binary: {}
    type: Binary
  strategy:
    dockerStrategy: {}
    type: "Docker"
```

Nous définissons dans la partie metadata de cette configuration :

- le nom de l’image: maintenance-page ;
- la version qui sera incrémentée au fur et à mesure des futurs build ;

De même, nous définissons dans la partie spec:

- le nombre maximum de build qui échoue à retenir ;
- le choix de ne pas déployer la page de maintenance sur un nœud spécifique ;
- la création d’une ressource imageStreamTag associée à cette image ;
- la stratégie de build utilisée, ici Docker ;
- et pour finir la source qui permet de construire l’image.

Définissons maintenant le Dockerfile permettant de construire notre image :

```docker
FROM nginxinc/nginx-unprivileged:1.20
COPY default.conf /etc/nginx/conf.d/default.conf
COPY index.html /usr/share/nginx/html/index.html
```

Nous utilisons ici pour base du build l’image nginx officielle et nous copions à l’intérieur de l’image construite les fichiers default.conf et index.html que nous définissons ainsi :

`default.conf`:

```json
server {
   listen       8080;
   server_name  localhost;

   location / {
       root   /usr/share/nginx/html;
       index  index.html;
       try_files $uri $uri/ /index.html;
   }

   error_page   500 502 503 504  /50x.html;
   location = /50x.html {
       root   /usr/share/nginx/html;
   }

}
```

Nous désirons que le serveur nginx écoute sur le port 8080 et renvoie la page définie dans le fichier index.html aux requêtes reçues.

exemple simple de fichier `index.html`:

```html
<!doctype html>
<title>Maintenance Page</title>

<article>
   <h1>We&rsquo;ll be back soon!</h1>
   <div>
       <p>Sorry we are under maintenance procedure, please come back later ...</p>
       <p>&mdash; Maintenance Team</p>
   </div>
</article>
```

Nous avons ici les composants suffisants pour construire l’image maintenance-page.

Déployons la buildConfig à l'aide de la commande suivante :

```shell
oc create -f buildConfig-template.yaml
```

Nous utiliserons cette commande pour appliquer tous les futurs templates.

Avant de construire l’image, nous allons déployer une ressource ImageStream. Cette dernière a pour but de faire le lien entre l’image construite et le déploiement de la page de maintenance.

`imagestream-template.yaml` :

```yaml
apiVersion: "image.openshift.io/v1"
kind: "ImageStream"
metadata:
  labels:
    app: "maintenance-page"
  name: "maintenance-page"
```

Construisons ensuite l’image `maintenance-page` depuis le dossier principal où nous stockons le Dockerfile, les fichiers `default.conf` et `index.html` :

```shell
oc start-build maintenance-page --from-dir=.
```

Au cours du build, le dossier présent en local est "uploadé" vers le cluster OpenShift, un pod est déployé et le build s’effectue à l’intérieur de ce pod. Nous créons enfin une ressource imageStreamTag associée à l'image résultante.

Nous pouvons par la suite apposer le tag "latest" sur l’image buildée via la commande:

```shell
oc tag "maintenance-page:v1" maintenance-page:latest
```

### Phase 2: Déploiement de la page de maintenance:
Sur OpenShift nous pouvons créer une ressource deployment, laquelle permet de définir la stratégie de déploiement de notre page de maintenance :

`deployment-template.yaml` :

```yaml
kind: "Deployment"
apiVersion: "apps/v1"
metadata:
  name: "maintenance-page"
  labels:
    app: "maintenance-page"
  annotations:
    # L'annotation permet de faire le lien entre l'ImageStreamTag
    # "maintenance-page:latest" et le déploiement de la page de
    # maintenance. Ainsi un nouveau déploiement est effectué à
    # chaque modification de l'image ciblée par ce tag
    image.openshift.io/triggers: |-
      [
        {
          "from": {
          "kind": "ImageStreamTag",
          "name": "maintenance-page:latest"
          },
          "fieldPath": "spec.template.spec.containers[0].image"
        }
      ]

spec:
  # L'ancien pod n'est pas détruit tant que le nouveau pod n'est 
  # pas disponible
  strategy:
    type: "RollingUpdate" 
  replicas: 1
  template:
    metadata:
      labels:
        app: "maintenance-page"
        deployment: "maintenance-page"
      name: "maintenance-page"
    spec:
      containers:
        - name: "maintenance-page"
          image: maintenance-page:latest
          imagePullPolicy: "IfNotPresent"
          # Le container écoute le port 8080 afin de servir
          # la page de maintenance
          ports:
            - name: http
              containerPort: 8080
          resources:
            requests:
              memory: "12Mi"
            limits:
              memory: "24Mi"
  # Le selector fait le lien entre le pod déployé et le
  # déploiement via le label `deployment`
  selector:
    matchLabels:
      deployment: "maintenance-page"
```

Afin de diriger le trafic vers ce pod, nous créons un service via le template suivant :

`service-template.yaml` :

```yaml
kind: "Service"
apiVersion: "v1"
metadata:
  name: "maintenance-page"
  labels:
    app: "maintenance-page"
spec:
  # Ce service est lié aux pods ayant le label `deployment`
  # suivant:
  selector:
    deployment: "maintenance-page"
  # Ce service écoute le port `8080`
  ports:
    - port: 8080
      name: http
```

Pour finir il ne reste plus qu’à créer une route afin de router le trafic vers la page de maintenance ainsi qu’une NetworkPolicy autorisant ce même trafic :

`network-policy-template.yaml` :

```yaml
kind: "NetworkPolicy"
apiVersion: "networking.k8s.io/v1"
metadata:
  name: "maintenance-page"
  labels:
    app: "maintenance-page"
spec:
  ingress:
    - ports:
        - port: 8080
          protocol: TCP
  # Nous autorisons le pod ayant pour label "maintenance-page" à
  # recevoir du trafic depuis l'extérieur
  podSelector:
    matchLabels:
      app: "maintenance-page"
  policyTypes:
    - Ingress
```

Nous pouvons changer le service ciblé par la route déjà existante.

Ou tout simplement déployer une nouvelle route avec :

- `SERVICE_NAME`: le nom du service à passer en maintenance ;
- `HOSTNAME`: l’url menant à ce même service (exemple: service.domain.com)

`route-template.yaml` :

```yaml
kind: "Route"
apiVersion: "route.openshift.io/v1"
metadata:
  labels:
    app: "${SERVICE_NAME}-maintenance-page"
  name: "${SERVICE_NAME}-maintenance-page"
  annotations:
    # Si vous utilisez le contrôleur openshift-acme, l'annotation
    # suivante permet d'automatiser la génération des certificats
    # SSL/TLS.
    kubernetes.io/tls-acme: "true"
spec:
  host: "${HOSTNAME}"
  to:
    name: "maintenance-page"
  tls:
    termination: "Edge"
    insecureEdgeTerminationPolicy: "Redirect"
```

Puis dans un second temps supprimer l’ancienne route via la commande suivante:

```shell
oc delete route/${ROUTE_NAME}
```

Si vous vous rendez sur l’host précédemment défini vous devriez recevoir en retour le message suivant :

![exemple_page_maintenance-1.png](/assets/img/virtualisation/tempo/exemple_page_maintenance-1.png)

Votre service est donc en maintenance! Tester le bon fonctionnement de votre application au cours ou après la phase de maintenance avant de rouvrir le flux est toutefois possible ;)

Pour ce faire, vous avez la possibilité d’effectuer un port-forward sur l’un de vos pods via la commande suivante :

```shell
oc port-forward ${POD_NAME} 8443:8443
```

Dans l’exemple précédent nous avons effectué un port-forward depuis le pod ${POD_NAME} vers notre machine via le port 8443 (en supposant que le port 8443 soit utilisé par le pod). Nous pouvons donc accéder à l’application via le localhost:

```
http://localhost:8443
```

Nous pouvons placer en maintenance ainsi plusieurs applications et diriger tout le trafic vers le pod maintenance-page en créant des routes distinctes pour chaque application sur le modèle précédent :

![multiple_applications_en_maintenance.png](/assets/img/virtualisation/tempo/multiple_applications_en_maintenance.png)

Une fois la maintenance terminée, nous redirigeons le trafic sur le service habituel. L’application retrouve ainsi son comportement attendu.

Nous pouvons pour finir, détruire toutes les ressources précédemment créées si nous n’avons plus d’application en maintenance ou les conserver dans le cas contraire.

## Conclusion
Avoir un processus de mise en place d’une page de maintenance est donc un atout indéniable pour une application et surtout pour les utilisateurs finaux. D’autant plus si votre application possède des liens critiques avec des services observant une phase d’indisponibilité lors de maintenances (comme c’est le cas ici avec le legacy).

Nous avons pu observer toutes les étapes menant à la mise en place de cette page. J’espère que cet article a été instructif et vous sera utile pour vos travaux en cours ou futurs.

Cependant, bien qu'effectuer ces actions manuellement soit possible, je suggère d’automatiser ce processus d’autant plus si il est effectué fréquemment.

J’aborderai l’automatisation de ce processus dans un second article dans lequel nous utiliserons la CI de GitLab.

---

Dans l’article Déploiement d’une page de maintenance sur OpenShift via GitLab (1), nous avons pu aborder les différentes étapes conduisant à la mise en place d’une page de maintenance. Dans ce second article, nous allons automatiser le processus via GitLab.

J’ai fait le choix d’utiliser dans cette mise en pratique le gestionnaire de packages Helm. Nous retrouvons donc les ressources énoncées dans le premier article auxquels nous ajoutons les fichiers Chart.yaml et values.yaml :

```shell
maintenance-page-helm
├── Chart.yaml
├── values.yaml
├── buildconfig.yaml
├── imagestream.yaml
├── deployment.yaml
├── service.yaml
└── networkpolicy.yaml
```

Nous gardons la ressource route hors du scope du package Helm afin de pouvoir créer autant de routes que nécessaires pour les applications en maintenance.

Voici les templates Helm permettant de “builder” et déployer la page de maintenance ainsi que les ressources nécessaires à sa future exposition:

`buildconfig.yaml` :

```yaml
apiVersion: "build.openshift.io/v1"
kind: "BuildConfig"
metadata:
  labels:
    app: {{ .Chart.Name }}
    app.kubernetes.io/managed-by: Helm
    version: {{ .Values.imageTag }}
  name: {{ .Chart.Name }}
  annotations:
    meta.helm.sh/release-name: {{ .Chart.Name }}
spec:
  triggers: []
  failedBuildsHistoryLimit: 5
  nodeSelector: null
  output:
    to:
      kind: "ImageStreamTag"
      name: "{{ .Chart.Name }}:{{ .Values.imageTag }}"
    pushSecret:
      name: "docker-registry-secret"
  source:
    binary: {}
    type: Binary
  strategy:
    dockerStrategy: {}
    type: "Docker"
```

`imagestream.yaml` :

```yaml
apiVersion: "image.openshift.io/v1"
kind: "ImageStream"
metadata:
  labels:
    app: {{ .Chart.Name }}
    app.kubernetes.io/managed-by: Helm
  name: {{ .Chart.Name }}
  annotations:
    meta.helm.sh/release-name: {{ .Chart.Name }}
```

`deployment.yaml` :

```yaml
kind: "Deployment"
apiVersion: "apps/v1"
metadata:
  name: {{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
    app.kubernetes.io/managed-by: Helm
    version: {{ .Values.imageTag }}
  annotations:
    meta.helm.sh/release-name: {{ .Chart.Name }}
    image.openshift.io/triggers: |-
      [
        {
          "from": {
          "kind": "ImageStreamTag",
          "name": "{{ .Chart.Name }}:latest"
          },
          "fieldPath": "spec.template.spec.containers[0].image"
        }
      ]
spec:
  strategy:
    type: RollingUpdate
  replicas: 1
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
      name: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: maintenance-page:latest
          imagePullPolicy: "Always"
          ports:
            - name: http
              containerPort: 8080
          resources:
            requests:
              memory: {{ .Values.resources.memoryRequest }}
            limits:
              memory: {{ .Values.resources.memoryLimit }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
```

`service.yaml` :

```yaml
kind: "Service"
apiVersion: "v1"
metadata:
  name: {{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
    app.kubernetes.io/managed-by: Helm
  annotations:
    meta.helm.sh/release-name: {{ .Chart.Name }}
spec:
  selector:
    app: {{ .Chart.Name }}
  ports:
    - port: 8080
      name: http
```

`networkpolicy.yaml` :

```yaml
kind: "NetworkPolicy"
apiVersion: "networking.k8s.io/v1"
metadata:
  name: {{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
    app.kubernetes.io/managed-by: Helm
  annotations:
    meta.helm.sh/release-name: {{ .Chart.Name }}
spec:
  ingress:
    - ports:
        - port: 8080
          protocol: TCP
  podSelector:
    matchLabels:
      app: {{ .Chart.Name }}
  policyTypes:
    - Ingress
```

Voici les fichiers `Chart.yaml` et `values.yaml` essentiels au fonctionnement de Helm:

`Chart.yaml` :

```yaml
apiVersion: v2
name: maintenance-page
description: A Helm chart to deploy maintenance page
version: v0.0.1
```

`values.yaml` :

```yaml
imageTag: ''

resources:
  memoryRequest: 12Mi
  memoryLimit: 24Mi
```

Enfin, voici le template visant à créer les routes menant à la page de maintenance :

`route.yaml.template` :

```yaml
kind: "Route"
apiVersion: "route.openshift.io/v1"
metadata:
  labels:
    app: "${APP_NAME}-maintenance-page"
  name: "${APP_NAME}-maintenance-page"
  annotations:
    kubernetes.io/tls-acme: "true"
spec:
  host: "${HOSTNAME}"
  ${SUBPATH_TO_PASS}
  to:
    name: "maintenance-page"
  tls:
    termination: "Edge"
    insecureEdgeTerminationPolicy: "Redirect"
```

## Passons à la mise en place de la CI
Avant de commencer, je souhaiterais préciser que toutes les références à la "route" réfèrent à la ressource route utilisée par OpenShift.

J’ai fait le choix de découper le pipeline GitLab en trois jobs:

- build-and-deploy ;
- deploy-route ;
- delete

Le job de build-and-deploy permet dans un premier temps de construire l’image Docker de la page de maintenance et de la stocker sur un registre Docker, puis dans un second temps de la déployer sans l’exposer.

Le job deploy-route permet de réaliser le routage du trafic vers la page de maintenance.

Le dernier job, delete, permet un retour à la normale. L’application est donc de nouveau disponible et les ressources liées à la page de maintenance sont supprimées si aucune autre application n’est en phase de maintenance.

Lors de la mise en maintenance d’une application, nous lançons donc la CI GitLab en définissant en entrée le nom de la route menant à l’application à placer en maintenance. De même, nous pouvons définir si nécessaire un subpath à placer en maintenance (le reste de l’application restera donc accessible). Si le paramètre subpath est vide, toute l’application est placée en maintenance. Enfin le job delete est à lancer manuellement à la fin de la phase de maintenance afin que l’application redevienne accessible. La CI GitLab est lancée une fois pour chaque application à placer en maintenance.

Nous définissons dans la partie before_script, le script d’authentification au cluster OKD et de sélection du projet dans lequel déployer la page de maintenance:

```yaml
 before_script:
   - oc login --token=$OKD_TOKEN --insecure-skip-tls-verify=true $OKD_URL
   - oc project $PROJECT
```

## Phase de build et de déploiement

```yaml
build-maintenance-page:
 stage: build-and-deploy
 script:
   - helm upgrade -i maintenance-page ./maintenance-page-helm --namespace $PROJECT
       --set imageTag=$IMAGE_TAG
   - oc start-build maintenance-page --from-dir=docker --follow
   - oc tag maintenance-page:$IMAGE_TAG maintenance-page:latest
   - oc rollout status -w deployment/maintenance-page
```

Dans ce premier job, nous déployons le Chart Helm permettant la mise en place de la page de maintenance. Puis nous lançons le build depuis le dossier /docker du projet GitLab. Le dossier /docker doit inclure les fichiers Dockerfile, default.conf et index.html abordés dans l’article précédent. Enfin nous attendons que le pod déployé soit disponible.

### Amélioration de la phase de build et déploiement de la page de maintenance :
Nous allons maintenant compléter cette phase afin d’obtenir le résultat escompté.

Afin de ne pas déployer ou "builder" une nouvelle version de la page de maintenance à chaque mise en maintenance d’une nouvelle application, nous pouvons ajouter une variable "BUILD_MAINTENANCE_PAGE". Cette variable est définie à la valeur true lorsque nous souhaitons "builder" puis déployer une nouvelle version de la page de maintenance.

L’utilisateur spécifie de même la variable "IMAGE_TAG":

```yaml
build-maintenance-page:
 stage: build-and-deploy
 script:
   - helm upgrade -i maintenance-page ./maintenance-page-helm --namespace $PROJECT
       --set imageTag=$IMAGE_TAG
   - |
if [[! -z $BUILD_MAINTENANCE_PAGE ]]; then
    oc start-build maintenance-page --from-dir=docker --follow;
    oc tag ${DOCKER_REGISTRY}/maintenance-page:$IMAGE_TAG maintenance-page:latest;
          oc rollout status -w dc/maintenance-page;
    fi
```

## Routage du trafic vers la page de maintenance
Ici nous voulons que les utilisateurs du service ne reçoivent pas de code 5xx en retour de leurs requêtes au service indisponible. Nous devons donc créer dans un premier temps, la route menant vers la page de maintenance, puis dans un second temps détruire la route menant au service.

Nous admettons que l’utilisateur spécifie en entrée du pipeline, le nom de la route "ROUTE_NAME" à mettre en maintenance. De même l’utilisateur peut placer en maintenance un lien spécifique de l’application en définissant "SUBPATH" en entrée du pipeline.

Par exemple, si l’utilisateur souhaite passer en maintenance l’application ayant pour hostname: example.com, il définit en paramètres:

- `ROUTE_NAME`: (le nom de la route OpenShift menant vers l’application)

S’il souhaite ne mettre en maintenance seulement un "SUBPATH" de cette application, il définit en paramètres:

- `ROUTE_NAME`: (le nom de la route OpenShift menant vers l’application)
- `SUBPATH`: /subpath_example

![mise_en_place_routage_trafic_vers_page_de_maintenance.png](/assets/img/virtualisation/tempo/mise_en_place_routage_trafic_vers_page_de_maintenance.png)

Processus de mise en place du routage du trafic vers la page de maintenance

Voici le job GitLab lié au processus précédent:

```yaml
deploy-maintenance-page-route:
 stage: deploy-route
 script:
   - export HOSTNAME=`oc get route $ROUTE_NAME -o jsonpath={.spec.host}`
   - export APP_NAME=${ROUTE_NAME}
   - |
	if [[ "$SUBPATH" != "" ]]; then
	  export SUBPATH_TO_PASS="path: $SUBPATH";
	else
	  export SUBPATH_TO_PASS="";
     fi
   - envsubst < route.yaml.template > route.yaml
   - oc apply -f route.yaml;
   - |
     if [[ "$SUBPATH_TO_PASS" == "" ]]; then
       oc delete route/${ROUTE_NAME} || true;
	fi
```

Nous pouvons donc avec les jobs précédents, construire et déployer une page de maintenance dans un namespace. Puis mettre en place le routage du trafic pour un ou plusieurs services vers le pod renvoyant la page de maintenance à l’aide du dernier job présenté.

Dans la partie suivante, nous verrons comment mettre en place le job permettant de sortir de maintenance le(s) application(s).

## Suppression de la page de maintenance :
Une fois la maintenance terminée, nous avons besoin d’un job pouvant être exécuté manuellement par l’utilisateur pour rediriger le trafic vers l’application sortant de la phase de maintenance.

![processus_de_sortie_de_phase_de_maintenance.png](/assets/img/virtualisation/tempo/processus_de_sortie_de_phase_de_maintenance.png)

Processus de sortie de la phase de maintenance

Afin de faciliter la mise en place de cette dernière phase, je suggère de créer un projet GitLab et de stocker vos templates de mise en place des routes dans ce dernier. Ainsi vous pouvez aisément récupérer ces templates au cours du job et appliquer le bon template afin de créer une route vers le service sortant de maintenance.

Voici une suggestion de script pouvant être mis en place (SUBPATH correspond au PATH du processus ci-dessus et GOOD_ROUTE_TEMPLATE, le template récupéré, habituellement utilisé pour créer la route menant vers l’application en maintenance):

```yaml
delete-maintenance-page-route:
 stage: delete
 when: manual
 script:
   - |
     if [[ "$SUBPATH" == "" ]]; then
       oc process -f ${GOOD_ROUTE_TEMPLATE} | oc apply -f -;
     fi
     oc delete route/$ROUTE_NAME-maintenance-page;
     if [[ ! $(oc get route | grep maintenance-page 2> /dev/null) ]]; then
       helm del maintenance-page
     fi
```

Les ressources nécessaires à la page de maintenance ne sont donc supprimées seulement si elles ne sont plus utiles.

## Conclusion
Dans ce second article nous avons pu observer comment automatiser le déploiement et la suppression d’une page de maintenance via GitLab.

Comme annoncé en introduction du premier article, je ne saurais que vous conseiller de mettre en place un tel processus. D’autant plus si certains de vos services critiques doivent observer une phase d’indisponibilité lors d’éventuelles mises à jour ou autres opérations.

De même, placer en maintenance toutes applications liées à un service indisponible permet de ne pas les exposer au cours de ce laps de temps critique. Ainsi, vous limiterez les tentatives d’attaques sur ces derniers le temps de la maintenance. Nous pouvons également noter que recevoir côté utilisateur une page de maintenance est plus rassurant qu’un code 5xx ;)

source : https://blog.ippon.fr/