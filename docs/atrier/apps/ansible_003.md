---
title: Découvrez ou redécouvrez Ansible-Vault
description: 
published: true
date: 2023-06-10T06:42:59.051Z
tags: 
editor: markdown
dateCreated: 2023-06-10T06:42:59.051Z
---

# Découvrez ou redécouvrez Ansible-Vault

Ansible a été racheté par Red Hat en 2015. C'est une suite d'outils qu'un simple programme solitaire. L'article va parler spécifiquement de la sécurité avec **Ansible-vault**.

## 1. Ansible Késako

Ansible est un outil de gestion de parc informatique dit "agentless". Cela signifie, que contrairement à d'autres outils d'administration système tels que **Puppet** ou **Chef**, il n'est pas nécessaire d'installer un "agent" (programme) sur les machines managées pour pouvoir les administrer. Ainsi, pas de problème de compatibilité, pas de problèmes de ne pas posséder l'agent pour telle machine avec un système d'exploitation trop ancien que l'on ne peut plus mettre à jour osus peine de perdre la production. Ansible ne demande qu'un client SSH (disponible sur quasiment toutes les machines par défaut bien qu'il faille souvent l'activer) pour les distributions **Linux** et **MacOS** ou **WinRm** pour les machines **Windows**, et **Python 2.7** (disponible aussi sur beaucoup de machines bien qu'il soit aussi possible de faire tourner les commandes en brut sans avoir Python d'installé).

Le deuxième gros avantage de ce genre d'architecture est qu'il n'y a pas d'augmentation de surface d'attaque sur les machines managés. Pas de problème d'ouverture de ports sur la machine ou sur les firewalls d'entreprises.

Cependant, il faut sécuriser avec la plus grande attention la ou les machines où s'exécute Ansible. Comme tout système d'administration, si cette machine est corrompue, c'est l'ensemble de votre système qui est mis à mal.

## 1.1 Installation

Toutes les grandes distributions Linux proposent déjà un package pour Ansible. Ainsi dans la majeure partie des cas, son installation est triviale.

Sur **Debian/Ubuntu** :

```shell
$ sudo apt update && apt install ansible
```

Sur **Arch Linux** :

```shell
$ sudo pacman -S ansible
```

Sur **Red Hat/Fedora** :

```shell
$ yum install ansible
```

Ansible peut aussi être compilé et installé à partir des sources. Pour cela, je vous renvoie à la documentation en ligne qui explique comment faire ([Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)) Sachez de plus qu'Ansible ne supporte pas Windows en tant que machine de contrôle.

## 1.2 Découverte

Lorsque l'on installe Ansible sur sa distribution, on se retrouve avec des fichiers dans `/etc/ansible` :

* `ansible.cfg` : c'est lefichier de configuration d'Ansible. Pour le moment, toutes ses valeurs sont commentées, il est donc inutilisé. Cependant, il peut être pratique lorsque l'on doit partager la configuration du réseau ou des machines sur différents Playbooks.

* `hosts` : parfois absent après installation (il faudra alors le créer), ce fichier est l'inventaire (là ou l'on déclare les machines de notre parc ainsi que des configurations).

En plus d'installer des fichiers sur la machine, les packages Ansible fournissent différents exécutables dans `/usr/bin` :

- `ansible` : c'est le plus simple de tous et certainement celui que vous utiliserez en premier. Ce programme permet d'exécuter des commandes ad-hoc (une même commande sur une ou plusieurs machines);
- `ansible-config` : permet de voir et d'éditer le fichier de configuration d'Ansible;
- `ansible-connection` : Ddeamon permettant la connexion aux machines;
- `ansible-console` : une console REPL (voir plus bas) d'Ansible sur un inventaire prédéfini;
- `ansible-doc` : affiche les modules installés ainsi qu'une courte description de la fonction du chaque module;
- `ansible-galaxy` : permet de créer, envoyer ou récupérer des Playbooks et rôles déjà écris par la communauté;
- `ansible-inventory` : permet d'afficher l'inventaire comme Ansible le voit;
- `ansible-playbook` : la commande principale, permet de faire tourner les playbooks;
- `ansible-pull` : permet de renverser la logique push d'Ansible pour que les noeuds se connectent au serveur;
- `ansible-vault` : permet de chiffrer les playbooks contenant des données sensibles.

> **READ-EVAL-PRINT LOOP (REPL)**
>Une boucle "Lecture-Evaluation-Ecriture" est un environnement de programmation interactif. Le plus connu de ces environnements est votre ligne de commandes Shell. En effet, quand vous entrez une commande celle-ci est lue, interprétée et ensuite, le résultat est affiché. Au final, l'interpréteur revient à son état initial et est en attente d'une nouvelle commande. On a bien une boucle "Lecture-Evaluation-Ecriture".
>
>Mais en plus de votre Shell, de nombreux interpréteurs de scripts vous proposent un environnement REPL. Par exemple avec Python, vous avez sûrement déjà écrit et exécuté des scripts Python, mais aussi utilisé la console Python, l'environnement REPL.
>
>Ansible, quand à lui, a été principalement conçu pour interpréter des ordres venant des playbooks. Mais depuis récemment il propose aussi un environnement REPL pour pouvoir exécuter manuellement plusieurs commandes à la suite. C'est le programme `ansible-console`.

> **IDEMPOTENCE**
> L'idempotence est une notion mathématique qui peut s'appliquer en informatique. On définit une opération comme idempotence si cette opération ne change pas le résultat qu'on applique une fois ou une multitude de fois.
>
> Un exemple est la fonction valeur absolue : `abs(x) = abs(abs(x)) = abs(abs(...abs(x)))`.
>
> Cette notion peut aussi s'appliquer en informatique. Une fonction est dite idempotente si l'état du système n'est pas différent si l'on applique la fonction une fois ou plusieurs fois. Par exemple, le tri d'une liste : une fois la liste triée, elle ne changera pas si l'on applique à nouveau la fonction de tri.
> 
> Dans l'écosystème d'Ansible, la notion d'idempotence a une place centrale. En effet, un playbook d'Ansible ne doit pas être vu comme une suite de commandes à réaliser les unes à la suite des autres, mais plutôt comme un état du système que l'on souhaite obtenir ([Ensuring a Command Module Task is Repeatable with Ansible](https://ryaneschinger.com/blog/ensuring-command-module-task-is-repeatable-with-ansible/)). Il est doncrecommandé, le plus possible, de rendre les playbooks idempotents. Ainsi après avoir fait tourner une première fois Ansible avec le playbook désiré, vous pouvez exécuter Ansible autant de fois que vous le voudrez avec le même playbook, le système ne changera pas d'état. Cela a l'énorme avantage de vous assurer de l'état de votre système  à tout moment. Votre système sera toujours dans la configuration décrite dans les fichiers playbooks d'Ansible. De plus, une personne ne connaissant pas votre système n'a qu'à regarder les playbooks pour en comprendre la configuration.
> 
> On peut même imaginer une architecture où l'on ne modifie plus directement la configuration des machines de notre parc. On va plutôt modifier les fichiers de configurations. Ensuite, des tâches automatiques, par exemple des cronJobs, se chargent d'exécuter Ansible périodiquement pour effectuer les modifications, sachant de plus qu'avec Ansible des vérifications sont faites (par exemple Ansible n'installera pas un package déjà installé sur une même machine).

## 1.3 Les commandes ad-hoc
Bien que ce ne soit pas la manière la plus commune d'utiliser Ansible, il est capable de faire tourner des commandes ad-hoc. Ce sont des commandes que l'on va pouvoir faire tourner sur un ensemble de machines que l'on aura prédéfinies (dans le fichier `/etc/ansible/hosts`).

Voici par exemple une commande utile pour savoir si vos machines sont disponibles sur le réseau :
 
```shell
$ ansible -m ping all

Server3 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
Server2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
Server1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

Ici `-m` permet de désigner le module que l'on souhaite utiliser. En l'occurence, il s'agit du module `ping` et `all` désigne l'ensemble des machines présentes dans l'inventaire (par défaut `/etc/ansible/hosts`). Attention, le module `ping` n'est pas un `ping` ICMP classique. Il s'agit simplement pour Ansible de se connecter à chaque machine pour y faire tourner un script Python répondant "pong". Cette commande permet de rapidement voir quelle machine est ou n'est pas disponible sur le réseau.

## 1.4 Les playbooks et les rôles

Bon c'est très bien de pouvoir faire tourner une même commande sur plusieurs machines à la fois, mais comment fait-on si justement on souhaite faire tourner différentes commandes sur différentes machines. Et si je souhaite faire tourner une suite de commandes sur différentes machines ne possédant pas toutes le même système d'exploitation ou même les mêmes utilisateurs ?

C'est ici que les playhooks deviennent intéressants, car ils permettent justement de répondre à cette problématique.

Les rôles permettent de définir des tâches précises pour être utilisés par les playbooks (installer le serveur **Nginx** et le démarrer, installer les packages de base, etc.).

## 2. Cachez cette variable que je ne saurais voir : ansible-vault

Pour ses playbooks, variables, rôles et handlers, Ansible utilise des fichiers **YAML**. Le YAML pour "YAML Ain't Markup Language" se définit comme un standard de sérialisation facilement compréhensible et éditable par des humains. C'est pour cela qu'il est souvent utilisé dans les fichiers de configuration où l'on demande à des humains de le modifier pour correspondre aux besoins locaux. Les fichiers YAML sont utilisés entre autres par **Symphony**, **Drupal**, **phpMyAdmin** et donc aussi Ansible.

Alors le standard YAML, c'est génial, simple à lire et à modifier, plus léger que du **XML** et du **CSV**, il permet les commentaires contrairement au JSON, le bonheur ! Mais comme les autres formats de sérialisation, il a un gros défaut ! Tout est marqué en clair. Cela ne gêne pas lorsqu'il s'agit d'adresse IP ou d'actions à effectuer par le playbook. Mais ça devient particulièrement handicapant lorsqu'il s'agit de stocker des informations sensibles (par exemple des mots de passe de clés SSH ou de bases de données). On peut ne pas souhaiter que toute l'équipe ait accès à ces informations.

Mais le plus ennuyeux est quand on souhaite utiliser des outils de versionning tels que **git** ! Là, si vous mettez vos clés SSH ou vos mots de passe dans vos fichiers YAML vous les offrez simplement gratuitement aux yeux du monde !

Il faut donc trouver un moyen de cacher ses données sensibles tout en permettant à Ansible de pouvoir les utiliser. Il faut aussi que cela soit simple d'utilisation ! Déplacer un fichier à chaque fois que l'on souhaite utiliser un playbook ou déchiffrer manuellement ne sont pas des solutions viables sur le long terme. La réponse à cette problèmatique est `ansible-vault`. Ce programme permet de chiffrer les playbooks sensibles tout en les rendant utilisables par Ansible qui nous demandera alors un mot de passe pour pouvoir les employer.

## 2.1 Exemple d'utilisation d'Ansible-vault

Ici le but n'est pas de chiffrer l'ensemble de vos fichiers YAML avec `ansible-vault`, cela ne serait pas très performant. Le mieux est de placer les variables sensibles (clés SSH, mot de passe, etc.) dans un fichier (par exemple `secret.yml`) et de ne demander à `ansible-vault` de ne chiffrer que ce ou ces fichiers ([Encrypting content with Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)).

Prenons l'exemple d'un fichier de variables contenant les informations nécessaires pour ajouter un nouvel utilisateur :

```shell
$ cat secret.yml
user: jamesbond
password: secret
```

Ici on peut voir que le mot de passe de cette utilisateur est en clair, et comme nous l'avons dit cela représente un vrai problème en terme de sécurité. Nous allosn donc utiliser `ansible-vault` de la manière suivante :

```
$ ansible-vault encrypt secret.yml
```

Un mot de passe sera demandé.

Pour déchiffrer des fichiers, c'est tout simple via la sous-commande `decrypt` :

```
$ ansible-vault decrypt secret.yml
```

Pour simplement éditer le fichier chiffré via `ansible-vault`, utilisez la sous-commande `edit` :

```
$ ansible-vault edit secret.yml
```

Cette commande vous ouvrira l'éditeur que vous avez sauvegardé dans la variable `$EDITOR` ou par défaut `vi`.

Il est aussi possible de chiffrer une chaîne de caractères donnée en ligne de commandes. Cela peut être utilie pour chiffrer (par exemple) la sortie d'un programme. Cela se fait avec la sous-commande `encrypt_string` :

```
$ ansible-vault encrypt_string 'unephrasetopsecret' --name 'LinuxMag'
New Vault password:
Confirm New Vault password:
LinuxMag: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    3213544564542121354545454512123132454654512132132132454546787894534234165
    3232456575612151210145512312132421212345121234015105545424515135345151151
    5911545412134541351154542242421105101454575130502324431241341503511513515
    1213454657878975454654212312104654564564878979845454121321345464587987897
    1212124546579879813121456465478979879454564546787987546465878789454546544
Encryption successful
```

Le paramètre `--name` permet de donner un nom à la variable (chiffrée) nouvellement créée. Une fois à la commande lancée, on obtient sur la sortie le résultat du chiffrement qu'il nous suffit de mettre dans un playhook pour l'utiliser.

Depuis la version 2.4 d'Ansible, la manière recommandée de passer le mot de passe à `ansible-vault` est de le faire via la ligne de commandes en utilisant l'option `--vault-id`

```
$ ansible-vault view --vault-id pass.txt secret.yml
user: jamesbond
password: secret
```

En plus de ces possibilités, les sous-commandes `rekey`, `create` et `view` permettent respectivement de changer le mot de passe d'un fichier, de créer un nouveau fichier chiffré vide et de voir en clair un fichier chiffré.

Finalement, sachez qu'il est possible, avec `ansible-vault`, de chiffrer n'importe quel type de fichier. Qu'il soit un fichier YAML ou non.

## 2.2 Etude du chiffrement
Bon, mais une fois chiffré ça ressemble à quoi ? Et comment est-il chiffré ? Regardons maintenant un fichier chiffré avec `ansible-vault` :

```
$ cat secret.yml
user: jamesbond
password: secret

$ ansible-vault encrypt secret.yml
New Vault password:
Confirm New Vault password:
Encryption successful

$ cat secret.yml
$ANSIBLE_VAULT;1.1;AES256
3213544564542121354545454512123132454654512132132132454546787894534234165
3232456575612151210145512312132421212345121234015105545424515135345151151
5911545412134541351154542242421105101454575130502324431241341503511513515
1213454657878975454654212312104654564564878979845454121321345464587987897
1212124546579879813121456465478979879454564546787987546465878789454546544
3213544564542121354545454512123132454654512132132132454546787894534234165
3232456575612151210145512312132421212345121234015105545424515135345151151
5911545412134541351154542242421105101454575130502324431241341503511513515
1213454657878975454654212312104654564564878979845454121321345464587987897
1212124546579879813121456465478979879454564546787987546465878789454546544
```

On voit ici qu'`ansible-vault` chiffre les données avec l'algorithme de chiffrement AES.

En fait si on lit la documentation sur `ansible-vault`, on apprend que les messages sont chiffrés avec le mode de chiffrement AES-CTR (depuis la version 1.5 "Love Walks In" de mars 2014) et une taille de clé par défaut de 256 bits (dérivée en utilisant la fonction de dérivation de clés PBKDF2), ainsi qu'une authentification réalisée via le code d'authentification HMAC-SHA256.

## 2.2.1 AES-CTR

### 2.2.1.1 AES

C'est l'algorithme de chiffrement et ici c'est plus particulièrement AES256 puisque la taille de la clé est de 256 bits. C'est aujourd'hui le plus utilisé et le plus sûr des algorithmes de chiffrement symétrique. C'est k'algorithme de chiffrement qui a remporté le concours. AES en 2000, proposé par le NIST ([Advanced Encryption Standard](https://fr.wikipedia.org/wiki/Advanced_Encryption_Standard)). C'est depuis le standard de chiffrement aux USA et donc dans le monde.

### 2.2.1.2 CTR

C'est le mode d'opération. C'est la manière d'agencer les blocs de texte chiffrés entre eux. En effet (pour AES), l'algorithme prend en entrée des blocs de texte chiffrés de même taille. Il faut alors prévoir le cas oùle texte à chiffrer est plus important que 16 octects (hautement probable). C'est là où les modes d'opérations entrent en jeu. Depuis la version 1.5, `ansible-vault` utilise le mode CTR; avant il utilisait le mode CBC (moins rapide et plus vulnérable aux attaques d'oracles de "bourrage"). Une explication détaillée sortirait du cadre de cet article, mais sachez que pour créer de la diffusion, le mode CBC (Cipher Block Chaining) chaîne les blocs entre eux tandis que le mode CTR (Counter) utilise un compteur incrémental. Si vous souhaitez en apprendre plus sur les modes d'opérations, vous pouvez vous diriger sur la page [Mode d'opération (cryptographie)](https://fr.wikipedia.org/wiki/Mode_d%27op%C3%A9ration_(cryptographie)).

## 2.2.2 PBKDF2

PBKDF2 (Password-Based Key Derivation Function 2) est une fonction de dérivation de clés.

Imaginez le problème suivant: vous demandez à l'utilisateur une clé pour chiffrer ses données, et vous voulez utiliser l'algorithme de chiffrement AES256 (par exemple). Vous avez donc besoin d'une clé d'une taille de 32 octects (ni plus, ni moins) Or, il est très peu probable que l'utilisateur vous fournisse une clé de taille 256 bits (32 octects). Il vous faut donc une fonction capable de créer une clé de taille souhaitée à partir de la clé fournie par l'utilisateur. C'est le rôle des fonctions de dérivation de clés.

Ces fonctions ont aussi pour rôle de rendre beaucoup plus difficile les attaques par brute-force. Elles sont délibéremment lentes afin de ralentir une attaque qui tenterait tous les mots de passe. Par exemple, ici la fonction PBKDF2 utilisée dans `ansible-vault` réalise 10000 itérations de la fonction de hachage SHA256. Si un attaquant souhaite casser ce mot de passe par brute-force, il doit à chaque tentative réaliser aussi 10000 itérations de cette fonction de hachage. On voit bien l'effort en plus que cela demande à un attaquant pour casser ce mot de passe. L'inconvénient de cet effort est que nous devons aussi le réaliser sur nos serveurs à chaque fois que l'on souhaite utiliser cette fonction. Et cela peut rapidement devenir problèmatique pour la charge processeurs si on doit le faire à répétition. Il faut donc choisir judicieusement le nombre d'itérations entre sécurité et praticité.

Finalement, les fonctions de dérivation de clés telles que PBKDF2 permettent d'éviter les attaques par rainbow tables en utilisant un sel cryptographique.

Voici le prototype de la fonction PBKDF2 utilisé dans `ansible-vault : Clé_dérivée = PBKDF2(PRF, Password, sel, nombre, dkLen)`. Ici, nous avons :
- `PRF` qui est la fonction pseudo-aléatoire utilisée à chaque itération, ici SHA256;
- `Password`, le mot de passe fourni par l'utilisateur;
- `sel` qui est ici 32 octects aléatoires générés en utilisant la fonction Python `os.urandom`;
- `nombre`, le nombre d'itérations à effectuer, ici **10000**;
- `dkLen` qui est la taille désirée de la clé dérivée (ici 32 octects).

## 2.2.3 HMAC-SHA256

Le chiffrement permet de s'assurer de la confidentialité du message. Cependant, il est aussi intéressant de posséder un mécanisme permettant d'assurer l'intégrité (le fait que le message n'ait pas été modifié) et l'authenticité (le fait que le message provienne bien de la personne dont il est censé provenir). C'est le principe des codes d'authentification de message ou MAC (Message Authentification Code).

Le principe fondamental d'un MAC est d'appliquer une fonction de hachage sur le message en plus de la clé. Par exemple `m = MAC(k, x) = h(k||x`) ou `m = MAC(k, x) = h(x||k)` où `x` représente le message à authentifier, `k` la clé d'authentification et `h` une fonction de hachage cryptographique (par exemple MD5, SHA1, etc.).

La première représentation est dite à préfixe secret et la seconde à suffixe secret.

Bien que cette manière d'authentifier des messages puisse fonctionner en théorie, en pratique chacune des constructions souffre de faiblesse.

Dans le premier cas, un attaquant peut ajouter des données au message et obtenir un autre MAC valide.

Dans le second cas, s'il est possible de trouver une collision dans la fonction de hachage, alors deux messages différents produiront le même MAC. Sachant que l'on a déjà prouvé l'existence de collisions sur MD5 et SHA1, ce type de construction n'est plus du tout sûr. Finalement, rappelons que le paradoxe des anniversaires permet de réduire le nombre de tentatives à effectuer pour trouver une collision ([Attaque des anniversaires](https://fr.wikipedia.org/wiki/Attaque_des_anniversaires)).

HMAC est définie dans la RFC 2104 dont voici le prototype : `HMAC(k, M) = H((K' + opad)||H((K' + ipad)||m))`.

Ici nous avons :

- `H`, une fonction de hachage cryptographique;
- `M`, le message que l'on souhaite authentifier;
- `K`, la clé;
- `K'`, une clé de la taille du bloc d'entrée de la fonction de hachage choisie. Par exemple ici pour SHA256 on prend `k` auquel on ajoute des zéros pour obtenir un bloc de taille 32 octects;
- `||`, la concaténation;
- `+`, le ou exclusif (XOR)(+ dans un cercle)
- `opad` pour outter padding, consiste à répéter l'octet `0x5c` jusqu'à obtenir la taille d'un bloc;
- `ipad` pour inner padding, consiste à répéter l'octet `0x36` jusqu'à obtenir la taille d'un bloc;

Il est important de noter que n'importe quelle fonction de hachage cryptographique peut être utilisée dans cette construction. Dans ce cas, on ajoute le nom après le HMAC (par exemple HMAC-MD5, HMAC-SHA1, etc.). C'est la fonction HMAC-SHA256 qui est utilisée dans `ansible-vault`.

En terme de performance, il est important de remarquer que le message, qui peut être très long, n'est haché qu'une seule fois (au niveau du `ipad`). Ensuite on xore et concatène (très rapide) pour effectuer un autre hash. Ainsi les pertes de performances de HMAC comparées à une fonction de hachage classique sont très faibles. La construction HMAC est aujourd'hui considérée comme sécurisée.

## Conclusion

En ce qui concerne la sécurité des chiffrements, on peut dire qu'`ansible-vault` n'utilise que des protocoles considérés comme sécurisés aujourd'hui. Il peut donc convenir pour chiffrer des playbooks sensibles. On ne peut cependant pas le recommander pour chiffrer tous vos documents. Pour cela, il faudrait auditer le code source de l'application. On finira en concluant que l'on ne peut pas certifier qu'un fichier chiffré avec `ansible-vault` est incassable, mais qu'`ansible-vault` respecte les standards de chiffrement considérés comme sécurisés aujourd'hui.

Source : GNU/Linux Magazine n°222