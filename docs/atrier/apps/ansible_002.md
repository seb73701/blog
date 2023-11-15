---
title: Comment tester un rôle Ansible avec Molecule ?
description: 
published: true
date: 2023-04-21T10:46:05.642Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:46:01.834Z
---

# Comment tester un rôle Ansible avec Molecule ?

>Depuis quelques années Ansible est devenu la référence des outils d’automatisation. Ansible applique des Playbooks au format YAML sur vos machines virtuelles, vos équipements réseau, vos conteneurs, vos clusters Kubernetes... bref, il se veut universel. Mais rapidement, même avec une infrastructure modeste, vous devez organiser vos Playbooks. Pour cela, vous utilisez des Rôles que souvent vous développez vous-même. Ces Rôles vont forcément évoluer et dès lors comment être sûr qu’ils seront toujours fonctionnels et idempotents ? C’est là qu’intervient Molecule : cet outil va vous permettre de tester et valider vos Rôles et ainsi vous assurer que votre dépôt Git ne contiendra que des Rôles dûment opérationnels !

## 1. Notre exemple de Rôle à tester
Pour la brièveté de cet article, nous prendrons un exemple plutôt simple, il s’agit d’installer un serveur Apache en créant un rôle nommé `webserver`. Nous supposons que le lecteur a une connaissance de base des rôles Ansible et en particulier de l’arborescence standard qui les définit [1]. Le contenu du fichier `webserver/tasks/main.yml` est le suivant :

```yaml
- name: Install httpd package
  yum:
    name: httpd
    state: installed
 
- name: Start webserver
  service:
    name: httpd
    state: started
    enabled: yes
 
- name: Configure to listen on port 8080
  lineinfile:
    path: /etc/httpd/conf/httpd.conf
    regexp: '^Listen '
    insertafter: '^#Listen '
    line: 'Listen 8080'
  notify: restart httpd
 
- name: Create a test page
  lineinfile:
    path: /var/www/html/test.html
    owner: apache
    group: apache
    mode: 0640
    create: yes
    line:testpage
```

L’application de ce rôle doit donc installer un package nommé `httpd` qui permet de démarrer un service nommé lui aussi `httpd`. Puis nous faisons en sorte que le serveur écoute sur le port `8080` et nous créons une page nommée `test.html` qui contient la chaîne `testpage`. Remarquez que la modification du fichier de configuration d’Apache (`httpd.conf`) génère une notification qui sera gérée en fin d’application du rôle par le gestionnaire décrit dans le fichier `webserver/handlers/task.yml` défini ainsi :

```yaml
- name: restart httpd
  service:
    name: httpd
    state: restarted
```

Notre objectif est de vérifier de manière automatique que ce rôle remplit bien son objectif, mais aussi qu’il est idempotent conformément aux bonnes pratiques d’Ansible !

## 2. Fonctionnement de Molecule
Avant de se lancer dans l’installation et la configuration de Molecule [2], il faut comprendre son fonctionnement, car nous aurons des choix à faire ! Nous ne parlerons ici que de la version 3 de ce produit (c’est la version la plus récente).

Le principe de Molecule est d’appliquer le rôle sur une ou plusieurs « plateformes » puis d’appliquer un ou plusieurs scénarios de tests pour, au final, valider au non le bon fonctionnement de ce rôle.

La première décision que nous devons prendre concerne la création et la gestion des plateformes de tests. Cette responsabilité est dévolue à un driver. Plusieurs choix sont possibles :

- le driver `delegated` nécessite l’utilisation d’une machine de test, accessible via ssh (ou via winrm si c’est un Windows). C’est sur cette machine que sera appliqué le rôle. Vous avez la responsabilité de la gestion de cette ressource. C’est le driver par défaut ;
- le driver `docker` utilise Docker et appliquera le rôle à tester sur un conteneur ;
- il existe aussi des drivers pour podman, OpenStack, Vagrant, GCE, VMWare, Azure, AWS-EC2...

Pour des raisons de simplicité et d’efficacité, nous choisirons la solution `docker`.

La deuxième décision à prendre concerne la syntaxe qui définira les tests de validation. À partir de la version 3 de Molecule, il n’y a plus que deux verifiers supportés pour exprimer les tests :

- `ansible` qui utilise la syntaxe et les modules de Ansible ;
- `testinfra` [3] qui est un module Python et permet de faire des tests… en Python.

Montrons dès maintenant un exemple de test exprimé à l’aide des deux verifiers disponibles : imaginons que nous voulions vérifier la présence du fichier `/var/www/html/test.html` sur notre plateforme de tests.

Ce test serait exprimé ainsi avec le verifier ansible (il y aura quelques subtilités que nous verrons plus loin) :

```yaml
- name : Vérifie la présence de /var/www/html/test.html
 stat :
    path : /var/www/html/test.html
  register : page
 
- name : Vérifie les permissions
  assert :
    that :
      - page.stat.exists
      - page.stat.mode == "0640"
```

Et il serait exprimé ainsi avec le verifier `testinfra` :

```python
def test_html_page(host) :
    page = host.file("/var/www/html/test.html")
    assert page.exists
    assert page.mode == 0640
```

Comme je trouve élégant de tester un rôle Ansible avec Ansible lui-même et que cela évite d’apprendre encore une nouvelle syntaxe – même si ici testinfra permet plus de concision –, nous choisirons le verifier ansible... qui est aussi le verifier par défaut.

## 3. Installation de Molecule
Molecule est écrit en Python, donc nous invoquons la commande `pip` pour l’installer. Comme nous allons utiliser `docker` comme verifier et `ansible-lint` pour vérifier la qualité du code, nous installons molecule avec le support de `docker` et `lint`, ce qui donne :

```
$ pip install molecule[docker,lint] --user
```

Vérifions que la commande molecule est fonctionnelle. Le cas échéant, il faudra peut-être installer (toujours à l'aide de la commande `pip`), les modules `docker-py`, `yamllint`, `ansible-lint` et `libselinux-python` !

```
$ molecule –version
molecule : 3.3.4 using python 3.9
    ansible:2.11.3
    delegated:3.3.4 from molecule
    docker:0.2.4 from molecule_docker
```

Nous disposons bien des drivers delegated et docker. Nous aurions pu obtenir aussi cette liste ainsi :

```
$ molecule drivers
  delegated
  docker
```

## 4. Intégration de Molecule avec un rôle Ansible
Pour que Molecule puisse tester un rôle, certains nouveaux répertoires et fichiers doivent être créés dans le répertoire du rôle (ici, le répertoire `./webserver`).

Molecule peut aussi créer le squelette d’un nouveau rôle en y incluant ses propres fichiers, par exemple :

```
$ molecule init role nouveau_role --driver-name docker
```

Mais dans notre cas, le rôle étant déjà créé, nous devons uniquement y ajouter le sous-répertoire molecule. Si nous sommes déjà dans le répertoire `./webserver`, la commande à exécuter devient :

```
$ cd webserver
$ molecule init scenario --role-name webserver –-driver-name docker
```

Examinons le contenu du répertoire `./molecule` :
```
$ tree molecule
molecule
└── default
    ├── converge.yml
    ├── create.yml
    ├── destroy.yml
    ├── INSTALL.rst
    ├── molecule.yml
    └── verify.yml
```
Nous constatons la présence d’un sous-répertoire nommé `./default` : ce répertoire correspond en réalité à un nom de scénario. En effet Molecule permet qu’un rôle puisse être validé par plusieurs scénarios et comme nous n’avons pas précisé de nom de scénario lors de l’exécution de la commande molecule init, c’est le nom default qui a été utilisé !

Commençons par décrire brièvement le contenu de chaque fichier du sous-répertoire `./default` :

|fichier|description|
|:-----:|:---------:|
|`converge.yml`|C’est un simple Playbook qui a pour objectif d’invoquer le rôle à tester|
|`create.yml`|Playbook qui sert à créer (provisionner) l’instance de test – utile dans le cas où on souhaite créer une machine virtuelle de test « à la volée ». Avec le driver docker, ce fichier est normalement inutile|
|`destroy.yml`|Playbook pour supprimer l’instance de test. Là aussi, avec docker nous n’en aurons pas besoin|
|`molecule.yml`|Sert à la configuration des outils utilisés (ansible-galaxy, docker, les différents « linters », la plateforme de tests, le verifier...)|
|`verify.yml`|Comme nous utilisons Ansible en tant que verifier, ce fichier contient les tâches de validation|

## 5. Configuration du scénario
### 5.1 Le fichier molecule.yml
Voici le contenu de notre fichier `molecule.yml` :

```yaml
dependency:
  name: galaxy
driver:
  name: docker
lint: |
  set -e
  yamllint
  ansible-lint
platforms:
  - name: webserver-test
    hostname: webserver-test
    image: couchbase/centos7-systemd
    command: /usr/sbin/init
    privileged: True
provisioner:
  name: ansible
verifier:
  name: ansible
  enabled: True
```

Quelques explications sur le contenu du fichier :

- `dependency` : indique comment récupérer les rôles dont notre rôle `webserver` pourrait dépendre. Ici on utilise `ansible-galaxy` et dans ce cas la clé role-file donnerait le nom d’un fichier contenant les rôles à installer et la clé requirements-file donnerait le nom d’un fichier contenant les collections à installer. La seule alternative à `ansible-galaxy` est l’utilisation du shell ;
- `driver` : spécifie quel sera le driver qui va créer et détruire l'environnement de test ;
- `lint` : indique une ou plusieurs commandes qui seront appliquées sur les fichiers YAML pour en vérifier la syntaxe et la qualité ;
- `platforms` : définit une ou plusieurs instances qui seront testées. Avec le driver docker, nous devons spécifier une image de base. Comme notre Playbook installe Apache comme un service, il faut utiliser une image qui contienne `systemd`. On aurait aussi pu ajouter la clé nommée `dockerfile` qui aurait permis de créer « à la volée » notre propre image sur la base de celle indiquée par la clé image ;
- `provisioner` : il n'y a pas d'autre choix qu’ansible. C'est le nom du module qui provisionne les instances et gère leur cycle de vie ;
- `verifier` : choisit le module de vérification. Historiquement, il y avait goss, mais maintenant seuls ansible et testinfra sont supportés (voir section 2).

À la section 6, nous verrons que nous pouvons ajouter des définitions supplémentaires.

### 5.2 Le fichier verify.yml
Pour définir les tests à appliquer sous forme de Playbook, nous complétons le fichier `verify.yml` avec le contenu suivant :

```yaml
- name: Verify
  hosts: all
  gather_facts: true
  tasks:
  - name: Collect packages as facts
    package_facts:
 
  - name: Collect services as facts
    service_facts:
 
  - name: Collect ports as facts
    listen_ports_facts:
 
  - name: Check httpd installed and running
    assert:
      that:
      - "'httpd' in ansible_facts.packages"
      - "ansible_facts.services['httpd.service'].state == 'running'"
      - "8080 in ansible_facts.tcp_listen | map(attribute='port')"
 
  - name: Check /test.html exists and contains the string "testpage"
    uri:
      url: http://127.0.0.1:8080/test.html
      return_content: yes
    register: page
    failed_when: "'testpage' not in page.content"
```

L’invocation du module assert effectue les tests à l’aide d’assertions. Ce sont ces assertions qui permettront de déterminer si le rôle a bien rempli sa fonction. En particulier, nous vérifions trois éléments :

1. qu’un package nommé httpd est présent ;
1. qu’un service nommé httpd.service est en cours d’exécution ;
1. qu’un processus est à l’écoute du port 8080/tcp.

L’appel au module uri permet de vérifier que la page ajoutée contient bien la chaîne testpage.

> Pour pouvoir écrire les assertions, nous avons dû préalablement invoquer les modules `package_facts`, `service_facts` et `listen_ports_facts` pour peupler `ansible_facts`.

### 5.3 Le fichier converge.yml
Ce fichier est généralement très simple, il s’agit d’un véritable Playbook dont l’objectif est d’invoquer le rôle à tester :

```yaml
- name: Converge
  hosts: all
  tasks:
    - name: "Include webserver"
      include_role:
        name: webserver
```

## 6. En route pour la validation...
Si vous êtes dans le répertoire du rôle `webserver`, la commande habituelle pour lancer notre scénario est :

```
$ molecule test
...
INFO     default scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
...
 
INFO     Running default > lint
COMMAND: set -e
yamllint .
ansible-lint
...
PLAY [Destroy] *****************************************************************
 
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=webserver-test)
...
PLAY [Create] ******************************************************************
 
TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item={'command': '/usr/sbin/init', 'hostname': 'webserver-test', 'image': 'couchbase/centos7-systemd', 'name': 'webserver-test', 'privileged': True})
...
PLAY [Converge] ****************************************************************
 
TASK [Gathering Facts] *********************************************************
ok: [webserver-test]
 
TASK [Include webserver] *****************************************************
...
PLAY RECAP *********************************************************************
webserver-test             : ok=7    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
 
INFO     Running default > idempotence
 
PLAY [Converge] ****************************************************************
 
TASK [Gathering Facts] *********************************************************
ok: [webserver-test]
 
TASK [Include webserver] *****************************************************
...
 
PLAY RECAP *********************************************************************
webserver-test             : ok=6    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
 
INFO     Idempotence completed successfully.
...
INFO     Running default > verify
INFO     Running Ansible Verifier
 
PLAY [Verify] ******************************************************************
 
TASK [Gathering Facts] *********************************************************
ok: [webserver-test]
...
PLAY RECAP *********************************************************************
webserver-test             : ok=10   changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
 
INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING Skipping, cleanup playbook not configured.
INFO     Running default > destroy
...
PLAY [Destroy] *****************************************************************
 
TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=webserver-test)
...
PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

L’affichage est trop verbeux pour être intégralement consigné ici, mais vous devriez constatez que molecule enchaîne un certain nombre d’étapes dont vous pouvez obtenir la liste ainsi :

```
$ molecule matrix test
INFO     Test matrix
---
default:
  - dependency
  - lint
  - cleanup
  - destroy
  - syntax
  - create
  - prepare
  - converge
  - idempotence
  - side_effect
  - verify
  - cleanup
  - destroy
```

Vous remarquez aussi dans l’affichage de la commande molecule test que le test d’idempotence est fructueux : à la première exécution du script, 7 tâches sont ok et 5 ont effectué une modification (`changed=5`). À la deuxième exécution du script, 6 tâches ont été jouées, mais aucune n’a apporté de modification à la cible ! Je laisse à votre sagacité l’explication du nombre de tâches qui n’est que de 6 au lieu de 7 lors du rejeu (bravo si vous trouvez la bonne réponse sans exécuter l’exemple) !

> Vous pouvez invoquer directement une étape particulière à l’aide de son nom, par exemple, pour vérifier la qualité des fichiers YAML :
>```
>$ molecule lint
>```
>Vous pouvez aussi redéfinir l’enchaînement des étapes associées aux commandes de molecule. Par exemple, si vous souhaitez supprimer des étapes lors de l’invocation de molecule test, vous pouvez ajouter le code suivant au fichier `molecule.yml` :
>```
>scenario :
>  test_sequence :
>  - dependency
>  - lint
>  - destroy
>  - syntax
>  - create
>  - prepare
>  - converge
>  - idempotence
>  - verify
>  - destroy
>```
>Le nom des étapes évoque bien leur objectif :
>
>- `dependency` : installe les dépendances éventuelles à partir de Galaxy ;
>- `lint` : lance les linters ;
>- `syntax` : effectue seulement une analyse syntaxique ;
>- `create` : crée le conteneur ou la machine virtuelle de tests ;
>- `prepare` : exécute le fichier `prepare.yml` sur la VM ou le conteneur ;
>- `converge` : exécute le rôle à partir des spécifications du fichier `converge.yml` ;
>- `idempotence` : vérifie l’idempotence !
>- `verify` : exécute le fichier `verify.yml` du scénario de test ;
>- `destroy` : détruit l’environnement de test (remarquez que l’opération est aussi réalisée au début, car il faut lire ce qui suit....!).

En cas d’échec de vos tests, vous pouvez les relancer en ajoutant l’option --destroy=never qui conserve les artefacts créés (ici le conteneur) pour vous permettre de mieux poursuivre vos investigations :

```
$ molecule test --destroy=never
...
INFO Running default > destroy
WARNING   Skipping, ‘--destroy=never’ requested.
 
$ docker ps
CONTAINER      ...    NAMES
1bc1b67d6c7f   ...    webserver-test
```

On constate que le conteneur est toujours disponible. On peut toujours ouvrir un shell avec un `docker exec`, mais aussi à l’aide de la commande suivante :

```
$ molecule login
INFO     Running default > login
[root@webserver-test /]# cat /var/www/html/test.html
testpage
```

Vraiment pratique !

## Conclusion
Avez-vous remarqué que Molecule supportait aisément l’application de la stratégie de tests dite « TDD » (Test Driven Development) ? Rien ne vous empêche de créer les tests dans le fichier `verify.yml` puis de vérifier qu’ils vont échouer. Vous devez alors corriger le problème en implémentant votre rôle et ceci étape par étape.

La documentation officielle vous montre aussi des exemples d’intégration dans un pipeline CI/CD [4]. Vous n’avez donc plus d’excuse pour ne pas proposer des rôles exempts de bugs et toujours dûment testés !

## Références
[1] Structure d’un rôle Ansible : https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#role-directory-structure
[2] Documentation complète de Molecule : https://molecule.readthedocs.io/en/latest/
[3] Documentation de testinfra : https://testinfra.readthedocs.io/en/latest/modules.html
[4] Exemples d’intégration dans un pipeline CI/CD : https://molecule.readthedocs.io/en/latest/ci.html

source : https://connect.ed-diamond.com/linux-pratique/lp-128/comment-tester-un-role-ansible-avec-molecule