---
title: Optimiser sa configuration de Bash
description: 
published: true
date: 2023-04-21T11:15:38.403Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:15:34.080Z
---

Nouveau sur bash, vous cherchez à apprendre de nouvelles astuces ? Utilisateur confirmé, vous voulez optimiser encore plus votre configuration ? Cet article est pour vous.

## 1. Introduction
Dans cet article, nous allons voir différentes astuces pour optimiser sa pratique de bash en fonction de ses besoins personnels. On supposera que vous connaissez déjà les rudiments de bash, son fonctionnement et ses raccourcis basiques. Utilisateurs avancés, ne fuyez pas ! Vous découvrirez certainement quelques options ou commandes utiles…

Le but de l'article est vraiment d'apprendre à faire sa propre configuration. Gardons à l'esprit le fait que des outils existent pour obtenir directement une configuration avancée **[Oh-my-bash]**.

Pour ne pas vous embêter à recopier manuellement les exemples, vous retrouverez tous les morceaux de code proposés dans l'article à l'adresse suivante : [https://github.com/Perdu/lp_article_bash](https://github.com/Perdu/lp_article_bash).

## 2. Commandes utiles
Commençons par mentionner quelques commandes indispensables, soit parce qu’elles sont très utiles, soit parce qu’on s’en resservira par la suite.

### 2.1 jobs
Je me dois d'introduire ici la commande `jobs`, car on va y revenir souvent dans la suite de l'article. Il est très utile d'apprendre à jongler entre plusieurs programmes dans un terminal avec celle-ci. Beaucoup de programmes peuvent être interrompus avec Ctrl-Z, ce qui les met en arrière-plan. `jobs` permet de lister ces programmes, `fg` de revenir aux précédents programmes interrompus, ou aux autres en donnant leur numéro (visible avec `jobs`) en argument. `bg` permet de continuer l'exécution indépendante d'un programme mis en arrière-plan, par exemple pour pouvoir continuer à utiliser un visualiseur de documents.

### 2.2 Options de cd
Vous savez vous déplacer avec `cd` ? Très bien ! Mais connaissez-vous tous ses arguments ? `cd` sans option permet de retourner directement à la racine de son `home.cd` - permet de retourner au dossier précédemment parcouru.

### 2.3 Fichiers temporaires
Sans doute connaissez-vous les sous-commandes, qu'on appelle avec la syntaxe `$(commande)` ou `commande` pour récupérer directement le résultat d'une commande en tant qu'argument d'une autre commande (particulièrement utiles dans les scripts pour affecter le résultat d'une commande à une variable). Mais savez-vous que de manière similaire, il est possible d'utiliser le résultat d'une commande en tant que fichier, avec la syntaxe `<(commande)` ? Cela peut éviter de créer manuellement des fichiers temporaires pour lancer une commande qui ne prend ses entrées que par des fichiers. Par exemple, pour comparer le résultat de deux scripts avec `diff` :

```
diff <(python un_script.py) <(python un_autre_script.py)
```

## 3. Raccourcis clavier indispensables
Vous savez vous déplacer dans la ligne de commandes, couper des mots, naviguer dans l'historique proche. Bien, mais après ? Voici quelques raccourcis qui font gagner de secondes précieuses au quotidien :

- Vous quittez votre terminal en fermant la fenêtre ? Peut-être tapez-vous `exit`, ou même avez-vous créé un alias pour fermer plus rapidement ? Sachez que la commande Ctrl-D envoie un caractère EOF (End-Of-File), qui indique la fin de la réception de caractères à lire. Autrement dit, en mode interactif, cela termine la session shell (si c'est le seul caractère de la ligne). Sachez que cette commande fonctionne avec de nombreux programmes en ligne de commandes : comme EOF est utilisé pour connaître la fin des entrées dans un script non-interactif, peu de programmes seront réticents à terminer de cette manière.
- Vous utilisez plusieurs commandes à la suite sur le même fichier ? Retenez bien ce raccourci : Ctrl-Maj-. (point). Cela colle le dernier argument de la commande précédente. Très utile !
- Marre de passer de la souris au clavier ? Shift+Inser colle les données du presse-papier primaire (celui rempli par une simple sélection à la souris) dans le terminal. Ce n'est certes pas un raccourci de bash lui-même et ce n'est pas supporté par tous les terminaux, mais c'est très utile.
- Alt-_ (souligné/underscore) annule la dernière action. Moins utile que dans un éditeur de texte, mais toujours bon à savoir !

## 4. .bashrc : options utiles
Maintenant que nous avons vu quelques généralités, attardons-nous sur la configuration de bash lui-même, qui s'effectue dans le fichier `~/.bashrc` (ou `/etc/bashrc` pour une configuration générale appliquée à tous les utilisateurs).

Je passe sur la configuration de base, qui est déjà certainement fournie par votre distribution (lisez-la !), pour discuter directement d'astuces diverses.

### 4.1 Options utiles
Jaloux de votre collègue utilisant zsh qui frime avec son « globstar » ? Cette fonctionnalité permet d'utiliser « ** » comme super-wildcard permettant de chercher des correspondances dans tous les sous-dossiers, de manière récursive. Par exemple, `ls **/*.txt` affichera les fichiers .txt dans tous les sous-dossiers du dossier courant (et sous-sous-dossiers, etc.). Eh bien cette fonction existe dans bash ! Il suffit de l'activer :

```shell
shopt -s globstar
```

Ensuite, le `CDPATH` est un must. Dans cette variable, on peut ajouter des dossiers qui seront parcourus par bash en plus du dossier courant pour se déplacer avec `cd`. Par exemple, `CDPATH="/home/$USER"` permettra de se déplacer dans un sous-dossier de son `home` depuis n'importe où !

### 4.2 L'historique
L'historique est une partie indispensable de la ligne de commandes. Voici quelques configurations bien pratiques :

```ini
HISTCONTROL=ignoredups:ignorespace # ne pas conserver les doublons et les lignes vides dans l'historique
HISTIGNORE="ls:[bf]g:cd_*:history*" # ne pas conserver de commandes précises dans l'historique (à configurer)
HISTSIZE=1000000 # augmenter la taille maximale de l'historique pour conserver toutes ses commandes effectuées
```

Petite astuce supplémentaire : pour ne pas enregistrer une ligne dans l'historique, on peut rendre la variable `HISTFILE` vide `(HISTFILE=)`, ou tout simplement débuter la ligne de commandes par un espace.

### 4.3 Le prompt
Le prompt est le début de la ligne de commandes. Par défaut, il a cette allure :

```shell
[user@machine dossier]$
```

Celle-ci peut se configurer. C'est là tout un art ! Par exemple, le mien ressemble à cela :

```shell
user:~[1]:)♥
```

Le nombre entre crochets sert à compter les jobs en arrière-plan. Le smiley indique le résultat de la précédente commande. Et un cœur, c'est quand même plus sympa qu'un dollar !

En exclusivité, je vous donne la définition complète, commentée :

```shell
# La fonction pour obtenir le nombre de programmes en arrière-plan
nb_jobs() {
    nb_jobs=$(jobs -s | wc -l)
    if [ "$nb_jobs" -gt 0 ]
    then
        echo -n "[$nb_jobs]"
    fi
}
# Définition des couleurs et de la graisse (les crochets évitent des problèmes de formatage)
couleur=31 # couleur spécifique à cette machine, à changer pour chaque machine
col="\[\033[01;${couleur}m\]"
white='\[\033[00m\]'
grey='\[\033[01;90m\]'
bold="\[$(tput bold)\]"
normal="\[$(tput sgr0)\]"
# On récupère le code de retour de la commande précédente pour afficher un smiley triste en cas d'erreur
smiley() {
    RC=$?
    [[ ${RC} == 0 ]] && echo -e ':)' || echo -e ':('
}
# Et si on affichait un caractère différent en fin de prompt si on est en SSH sur une machine distante ?
ssh_case() {
    if [ ! -z "${SSH_CLIENT}" ]
    then
        echo -e "♡"
    else
        echo -e "♥"
    fi
}
# La définition finale du prompt
PS1="$col\u$white:$col\w$grey\$(nb_jobs)$white$bold\$(smiley)$normal$white\$(ssh_case) "
```

N'hésitez pas à modifier tout cela selon votre propre convenance ! Au passage, je déconseille l'utilisation de caractères spéciaux ou de lignes trop longues, qui peuvent parfois poser quelques soucis.

## 5. Alias
Dans son fichier `.bashrc`, on peut configurer des alias, qui permettront d'appeler des commandes complexes en quelques caractères. Voici quelques alias parmi les plus utiles que j'ai pu définir en plusieurs années de configuration de bash :

```shell
alias ..='cd ..'
alias ai='sudo aptitude install' # ou le gestionnaire de paquets de votre distribution
alias c2p='xclip -o -selection clipboard | xclip' # passe les données du presse-papier clipboard (copiées avec Ctrl-C) vers le presse-papier primaire (clic-roulette)
alias dl='youtube-dl -i --audio-format mp3 --extract-audio -o '\''%(title)s.%(ext)s'\''' # télécharger une vidéo sur youtube et la convertir en mp3
alias dt='dmesg | tail' # voir les derniers messages du noyau
alias e='emacs -nw' # ouvrir un fichier avec emacs dans le terminal
alias get_cert_fingerprint='openssl x509 -noout -fingerprint -in' # extraire l'empreinte d'un certificat
alias get_ssh_fingerprint='awk '\''{print $2}'\'' /etc/ssh/ssh_host_ecdsa_key.pub | base64 -d | sha256sum -b | awk '\''{print }'\'' | xxd -r -p | base64' # obtenir l'empreinte du serveur SSH de la machine en cours
alias jprint='python -mjson.tool' # afficher un fichier JSON de manière lisible
alias maj='. ~/.bashrc' # recharger bashrc (dont les alias) après une modification
alias p8='ping 8.8.8.8' # pour les tests réseau
alias random_pass='< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c8 ; echo' # générer un mot de passe aléatoire
alias se='sudoedit' # éditer un fichier root sans faire tourner son éditeur en root
alias sus='systemctl suspend' # mettre la machine en veille
alias unixtime='date +%s' # obtenir le timestamp unix actuel
alias zkill='kill -9 $(jobs -lp)' # tuer les jobs en arrière-plan (utiles si bloqués)
```

C'est évidemment une sélection personnelle, et chacun trouvera divers alias plus ou moins utiles.

On peut aussi saisir l'opportunité pour gérer ses erreurs de frappe les plus courantes comme des alias pour éviter de se faire gronder par bash à chaque fois, par exemple :

```shell
alias gf='fg'
alias gti='git'
# etc.
```

Attention : les alias peuvent casser l'autocomplétion bash (quand vous utilisez TAB pour terminer une commande). Il faut bidouiller un peu pour la réparer avec `complete`. Comme c'est du bidouillage plutôt bancal, je vous mets mes solutions sur le dépôt posté plus haut, sans les détailler ici. Pour donner tout de même un exemple simple et montrer l'utilité de `complete` au-delà de ce problème, voilà comment on peut (re)créer la complétion pour des lecteurs PDF et leurs alias en ne permettant de compléter que les noms de fichiers PDF et les répertoires :

```shell
alias z='zathura'
alias ev='evince'
# compléter des lecteurs de pdf avec les fichiers pdf et les répertoires seulement
complete -f -X '!*.[pP][dD][fF]' -o plusdirs evince zathura ev z
```

### 5.1 Fonctions
Contrairement à d'autres shells, Bash ne permet pas d'insérer des arguments à l'intérieur d'un alias. Pour cela, il faudra utiliser une fonction. Par exemple :

```shell
function unix_to_date() {
        date -d @"$1"
}
```

Cet exemple ne fonctionnerait pas avec un simple alias (`alias unix_to_date='date -d @'`), car l’appel de ce dernier nécessite l’ajout d’un espace après l’arobase, ce qui ne respecte pas la syntaxe de date.

On peut évidemment définir des fonctions plus complexes, avec notamment des arguments. Comme ce n’est pas le but de cet article, je vous invite à lire un tutoriel sur la programmation bash pour en savoir plus, par exemple **[prog-bash]**.

## 6. Couleurs
Une bonne manière de s'y retrouver plus facilement dans son environnement en ligne de commandes est d'activer les couleurs dans les programmes où c'est possible. Les configurations par défaut sont souvent en monochrome, pour éviter les problèmes de compatibilité éventuels. Si on peut se le permettre, autant les activer !

Dans bash lui-même, la gestion des couleurs est un peu spéciale. On écrit en couleur en faisant précéder le texte de la commande suivante : `"\033[01;${couleur}m"`. À la place de la variable $couleur, on indique un nombre de 0 à 255, par exemple 0 pour le blanc, 31 pour le rouge ou 32 pour le vert (on trouve facilement des tableaux de correspondance en ligne **[couleurs]**). Tout le texte suivant sera de la couleur choisie, jusqu'à ce qu'on en change. Savoir cela peut être utile pour, par exemple, configurer un prompt coloré (relisez le paragraphe 4.3, c'est ce que j'ai fait !).

Pour certaines commandes, la couleur s'active avec une option. On peut créer un alias pour l'activer par défaut, ce qui est parfois fait dans les fichiers .bashrc fournis par défaut par la distribution, par exemple sous Ubuntu. L'exemple suivant en est d'ailleurs tiré :

```shell
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
    alias diff='diff --color=auto'
fi
```

On peut aussi activer des couleurs pour man avec l'astuce suivante, tirée du wiki d'Arch Linux **[arch-wiki]** :

```shell
# Couleurs pour less, utilisées notamment par man
man() {
    LESS_TERMCAP_md=$'\e[01;31m' \
    LESS_TERMCAP_me=$'\e[0m' \
    LESS_TERMCAP_se=$'\e[0m' \
    LESS_TERMCAP_so=$'\e[01;44;33m' \
    LESS_TERMCAP_ue=$'\e[0m' \
    LESS_TERMCAP_us=$'\e[01;32m' \
    command man "$@"
}
```

Pour `diff`, on peut installer le paquet `colordiff`, un petit script perl qui s'occupera de rajouter de la couleur à la sortie de la commande.

Enfin, votre gestionnaire de paquets doit peut-être être configuré pour utiliser la couleur, par exemple sur le résultat des recherches. Par exemple, sous Arch Linux, il faut décommenter la ligne `Color` dans `/etc/pacman.conf`. Pour `apt`, les possibilités de configuration sont plus minimes, puisque seule la barre de chargement peut être améliorée. Pour cela, il faut créer un fichier `/etc/apt/apt.conf.d/99colorbar` contenant les lignes suivantes :

```shell
Dpkg::Progress-Fancy "1";
APT::Color "1";
```

Notons au passage que la sortie de `git` est colorée par défaut, mais si au contraire vous souhaitez désactiver ce comportement, vous pouvez le faire avec la commande suivante :

```shell
git config --global color.ui false
```

## 7. .inputrc
Alors, préparez-vous. Dans cette section, on va faire de la magie !

`.inputrc` est le fichier de configuration de la librairie `readline`, utilisée par de nombreux logiciels en ligne de commandes (dont bash) pour lire les entrées clavier. C'est un fichier intéressant pour effectuer des configurations surprenantes, bien plus avancées que ce qu'on a vu dans les sections précédentes.

### 7.1 Un exemple pour vous convaincre
`.inputrc` permet de déclarer des raccourcis clavier effectuant une combinaison de commandes. On peut donc invoquer d'autres raccourcis dans un raccourci ! Par exemple, cette ligne à l'aspect barbare associe à Alt-g l'effacement de la ligne en cours (Ctrl-A puis Ctrl-K), l'écriture de la commande `grep - r "" .`, puis le déplacement du curseur entre les doubles quotes (plusieurs Ctrl-B). Rien que ça !

```shell
"\eg": '\C-a\C-kgrep -r "" .\C-b\C-b\C-b'
```

### 7.2 Obtenir directement le fichier le plus récent
Et si on importait des informations contextuelles directement dans la ligne de commandes ? Par exemple, coller directement le nom du fichier le plus récent du dossier en cours, ça vous fait rêver ? Moi, oui ! C'est extrêmement utile ! Mais par quels arcanes obscurs peut-on obtenir ce résultat ?

Premièrement, on va définir un alias permettant d'obtenir le fichier le plus récent du dossier en cours dans notre `~/.bashrc` :

```shell
alias latest='/bin/ls -t -1 -d * | head -n 1'
```

Ensuite, c'est là que la magie opère : on va utiliser l'expansion shell pour obtenir notre résultat. Dans notre `~/.inputrc`, on va définir la commande suivante :

```shell
"\e:": '\\\'$(latest)\\\' \e\C-e'
```

Celle-ci lie à un raccourci clavier (ici Alt-:), une suite d'entrées clavier qui colle notre alias en tant que sous-commande (`$(latest)`), puis lance une expansion shell pour l'exécuter et la remplacer par son résultat (Ctrl-Alt-E). On ajoute des quotes (qu'on échappe) pour que le nom du fichier soit utilisable directement même s'il contient des espaces, et le tour est joué.

Attention : l'expansion shell résout tous les alias et sous-commandes. Si vous en avez d'autres dans votre ligne de commandes, ils seront également exécutés !

### 7.3 Naviguer dans l'historique
Vous aimez retrouver rapidement des commandes dans votre historique, mais vous trouvez l'utilisation de Ctrl-R lourde ? Essayez ceci :

```shell
"\e[1;5A": history-search-backward
"\e[1;5B": history-search-forward
```

À l'aide de Ctrl-flèche haut, vous obtiendrez directement la dernière commande commençant de la même manière que ce que vous avez déjà écrit ! Plus besoin d'effacer la ligne pour lancer Ctrl-R !

Note : en fonction des environnements, il faut parfois adapter l'appel à flèche haut et bas, par exemple avec `"\e[A"`. Pour trouver quel est votre cas, tapez les combinaisons de touches dans `sed -n l`.

### 7.4 D'autres exemples
Là encore, je vous présente une sélection des raccourcis les plus utiles que j'ai pu inventer au cours des années :

```shell
"\ej": "\C-a\C-kjobs -sl\n" # Voir les jobs en arrière-plan
"\es": "\C-p\C-asudo \C-e" # Ajouter "sudo" à la dernière commande
"\ep": '\C-a\C-kps aux | grep -i ""\C-b' # Chercher un processus dans les processus en cours
"\ei": "\C-a\C-kssh -t machine LANG=fr_FR.UTF-8 tmux attach\C-m" # Se connecter en SSH à une machine et ouvrir sa session tmux en un seul raccourci !
"\eF": '\C-a\C-kfind . -iname "**" | tee >(tr -d \'\\n\' | xclip)\C-a\ef\ef\C-f\C-f\C-f' # Chercher des fichiers par nom, et coller les résultats dans le presse-papier au passage
"\eh": '\C-a\C-khistory | grep \'\'\C-b' # Chercher une commande dans son historique
"\em": '\C-a\C-kmake\C-m' # Lancer make avec Alt-m. Très agréable !
"\ea": ' \\\'$(ls | dmenu)\\\'\e\C-e' # Utiliser dmenu pour sélectionner un fichier dans le dossier courant. Pratique pour récupérer des noms de fichiers complexes ou contenant des caractères spéciaux
```

## 8. Pour aller plus loin
Voilà, nous avons vu comment améliorer notre environnement bash grâce à l'utilisation des alias, couleurs, raccourcis clavier peu connus, du fichier de configuration `.inputrc`… Mais tout cela n'était qu'un survol ! On peut toujours plus personnaliser sa configuration.

Je trouve cela utile de synchroniser sa configuration bash entre plusieurs machines en utilisant git, ce que je fais avec une méthode expliquée dans cet article **[linuxfr-git]**. Ainsi, on se retrouve à un `git clone` d'importer notre configuration personnalisée sur une nouvelle machine !

Pour creuser plus, il existe de nombreux logiciels particulièrement adaptés à un environnement en ligne de commandes. Par exemple, apprendre à maîtriser et configurer tmux est un must, car ce logiciel permet entre autres de jongler entre des terminaux virtuels dans le même terminal (donc sans avoir à gérer une palanquée de fenêtres ou onglets divers). Cela permet aussi de créer des sessions persistantes, ce qui est très pratique quand on accède à une machine via SSH. Finalement, il existe de nombreux logiciels avec une interface ncurses, qui permettent d'interagir avec le logiciel sans sortir du terminal. On citera par exemple ranger pour la gestion des fichiers, ou encore `vim` ou `emacs` (avec son option `-nw`) pour les éditeurs de texte. Utiles en SSH, pour éviter le passage fréquent du clavier à la souris, ou par goût, tout simplement !

## Références
[Oh-my-bash] https://ohmybash.nntoan.com/, inspiré de son équivalent pour zsh

[Linuxfr-git] https://linuxfr.org/users/perdu/journaux/gerer-ses-fichiers-de-config-avec-git

[prog-bash] https://doc.ubuntu-fr.org/tutoriel/script_shell

[couleurs] https://misc.flogisoft.com/bash/tip_colors_and_formatting

[arch-wiki] https://wiki.archlinux.org/index.php/Color_output_in_console#Using_less


Source : https://connect.ed-diamond.com/Linux-Pratique/lp-124/optimiser-sa-configuration-de-bash