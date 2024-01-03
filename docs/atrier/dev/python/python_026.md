# Coloration syntaxique de votre code avec Pygments

> Mieux visualiser du code source grâce à sa coloration est une évidence. Découvrez Pygments, un outil de coloration facilement utilisable et extensible.

Pygments [1] est un outil écrit en Python permettant de colorer du code pour plus d’une centaine de langages de programmation ou de description (HTML, XML, TeX), de moteurs de templates (ERB, JSP, Jinja2, etc.) et divers scripts et fichiers de configuration (Vim, fichiers .ini, Apache, Nginx). Deux façons de l’utiliser coexistent :

- comme une bibliothèque ;
- grâce à un exécutable (pygmentize), il est utilisable directement dans un terminal.

Pygments est disponible pour python 2 et 3.

Nous verrons quelques usages de cet outil puis la façon d’y ajouter un langage et un thème.

## 1. Usages
### 1.1 Dans un terminal
L’usage le plus simple se résume à appeler pygmentize uniquement avec le nom du fichier à colorer en paramètre :

```
pygmentize fichiers.pl
```

Cette commande affiche le contenu d’un fichier nommé fichiers.pl sur la sortie standard avec la coloration syntaxique par défaut. Cependant, pygmentize dispose de paramètres pour personnaliser la sortie selon ses préférences :

```
pygmentize -f terminal256 -O style=friendly -g fichiers.pl
```

Le paramètre -f définit le type de sortie souhaitée. De nombreux formats sont disponibles, allant du HTML au Latex en passant par la génération d’une image contenant le code à colorer. terminal256 est un mode pour la sortie standard d’une console.

Le paramètre -O définit la couleur et la graisse des caractères. 29 possibilités sont disponibles dans la version 2.2.0 (nommées algol, autumn, emacs, friendly, vim, etc.).

Le paramètre -g indique que le lexeur à utiliser est déterminé par l’extension de fichier.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-115/pygments.friendly.png">

Application du style friendly au contenu du fichier fichiers.pl.
Cependant, il est bien plus pratique d’ajouter un alias pour pouvoir l’appeler rapidement :

```
alias hcat='pygmentize -f terminal256 -O style=friendly -g'
```

Selon le shell utilisé, il faut l’insérer dans un fichier différent : ~/.bash_profile or ~/.bashrc pour bash, ~/.zshrc pour zsh et ~/.config/fish/config.fish pour fish.

### 1.2 Avec Latex
Minted est un paquet Latex permettant de réaliser une coloration syntaxique de code source. Il utilise pygments pour faire la coloration.

Il est utilisable en insérant le code source au sein du fichier latex :

```latex
\begin{minted}
[
% configuration du style
% (taille des caractères, numérotation des lignes, etc.)
]
{perl}
% ici le code source
\end{minted}
```

Ou en gardant le code source dans un fichier externe :

```latex
\inputminted{perl}{fichiers.pl}
```

Dans les deux exemples précédents, le motif {perl} définit le lexeur utilisé par pygments.

### 1.3 Présentation avec rst2pdf
rst2pdf produit un PDF de présentation à partir d'un fichier texte au format ReStructured Text. Il réutilise aussi pygments pour faire la coloration syntaxique. Les présentations sont par défaut très épurées.

```
rst2pdf.py presentation.txt -o presentation.pdf
```

rst2pdf est disponible dans le paquet éponyme dans Debian et ses distributions dérivées.

### 1.4 Pour le Web
Pygments est utilisé dans des outils web écrits en Python, comme par exemple le moteur de wiki Moinmoin ou le générateur de sites statiques Pelican. Hugo, un autre générateur de sites statiques, utilisait pygments pour la coloration syntaxique, mais l'a remplacé par chroma, un outil en Go. Chroma réutilise les définitions les plus courantes de pygments pour son propre usage. Django, le framework web le plus connu du monde Python dispose aussi d'un module pour s'interfacer avec pygments.

Pour les autres langages, la tendance est plutôt à l’utilisation de solutions natives au langage (côté serveur) ou l’utilisation d’une bibliothèque JavaScript (côté client) comme highlight.js qui dispose de nombreux langages/fichiers de configuration et d’une intégration aisée.

## 2. Ajouter un lexeur
Ajouter un lexeur permet de prendre en compte un nouveau langage ou format de fichier (texte). Le lexeur doit être défini dans une classe Python qui intégrera les expressions rationnelles analysant le texte.

À titre d'illustration, nous allons utiliser une partie du format Smart Game Format (sgf) [2] qui sert à l'enregistrement de certains jeux à deux joueurs ; le plus connu étant le jeu de Go. Le lexeur complet de ce format a été intégré dans la dernière version de pygments (2.4.0, publiée en mai 2019).

> Qu’est-ce qu’un lexeur ?
> 
> Un lexeur est un programme analysant un texte en le découpant en une suite d’entités lexicales (par exemple, un entier, une chaîne de caractères, une définition d’un type, etc.). La phase d’analyse lexicale faite par un lexeur est la première étape d’une compilation d’un programme.
{.is-info}

Le développement d'un nouveau lexeur est facilité par l'utilisation de paramètres supplémentaires à pygmentize. En supposant que la classe définissant le lexeur soit nommée SmartGameFormatLexer et contenue dans le fichier sgf.py et que le fichier à analyser soit nommé partie.sgf, la commande sera :

``` 
pygmentize -l sgf.py:SmartGameFormatLexer -x partie.sgf
```

Un extrait de partie.sgf pourrait être :

```
(;FF[4]GM[1]SZ[19]KM[5.5]
PB[Alice]PW[Bob]
;B[pe];W[cp];B[pp];W[dd])
```

La structure des données est toujours basée sur le motif mot-clef[données].

Il est nécessaire de séparer :

- les mots-clefs (FF, GM, SZ, KM, PB, PW, B et W) ;
- les données qui peuvent être des nombres entiers (comme 4) ou décimaux (5.5) ou encore du texte. Ce dernier est utilisé ici pour les noms des participants (Alice et Bob) ou les coordonnées du plateau (pe représentant la colonne p et la ligne e) ;
- les séparateurs (parenthèses, crochets, deux-points et point-virgule) :

```python
from pygments.lexer import RegexLexer, bygroups
from pygments.token import Name, Number, \
    Punctuation, String
 
 
class SmartGameFormatLexer(RegexLexer):
    name = 'SmartGameFormat'    aliases = ['sgf']    filenames = ['*.sgf']
 
    tokens = {
        'root': [
            (r'[\s():;]', Punctuation),
            # tokens:
            (r'(B|FF|GM|KM|P[BW]|SZ|W)',
             Name),
            # number:
            (r'(\[)([0-9.]+)(\])',
             bygroups(Punctuation, Number, Punctuation)),
            (r'(\[)([\w\s#()+,\-.:?]+)(\])',
             bygroups(Punctuation, String, Punctuation)),
       ],
    }
```

L'attribut filenames (ligne 9) est utilisé par le paramètre -g de pygmentize pour déterminer le lexeur à utiliser pour colorer un fichier (cf. le deuxième exemple de cet article). Seuls les fichiers finissant par .sgf seront analysés par SmartGameFormatLexer. Il est possible d'accepter plusieurs extensions de fichiers : par exemple, pour le lexeur du C++, 12 extensions sont acceptées (.cpp, .c++, .cxx, .hpp…).

Les expressions rationnelles permettent d'éclater le contenu du fichier et de définir le type (chaîne de caractères, opérateur, commentaire, entier, etc.).

L'analyse du dictionnaire des symboles commence par la clef root du dictionnaire tokens en cherchant la première correspondance possible dans la liste avec l’expression rationnelle. Une fois la correspondance trouvée, l’analyse des motifs permet de connaître le type de la chaîne. Par exemple, la chaîne FF va correspondre à l’expression rationnelle r'(B|FF|GM|KM|P[BW]|SZ|W)' et donc sera analysée comme étant de type Name. Il est possible de faire reconnaître plusieurs symboles dans la même expression rationnelle : c’est le cas de la chaîne [4] qui est reconnue par l’expression rationnelle (\[)([0-9.]+)(\]). Les trois motifs entre parenthèses seront affectés aux paramètres passés à la fonction bygroups() (donc [ correspond au type Punctuation, 4 à Integer et ] à Punctuation).

Pour des besoins plus complexes, il est possible d’ajouter des clefs à tokens pour passer à une autre liste d’expressions rationnelles. Dans ce cas, il est possible d’aller une clef à l’autre puis de revenir à la précédente. Il est aussi possible d'appeler d'autres lexeurs dans un lexeur particulier ou d’appeler une fonction spécifique pour gérer des cas particuliers.

## 3. Ajouter un style
Pour ajouter un nouveau thème de couleur, il faut créer un fichier qui sera placé dans le répertoire contenant les styles de pygments. Pour éviter de modifier la version du système, il suffit de créer un environnement local. Python permet cela en créant un virtualenv :

```
python3 -m venv venv./venv/bin/pip install pygmentstouch venv/lib/python3.7/site-packages/pygments/styles/glmf.py
```

Le nom du fichier et la classe définie à l'intérieur sont dépendants du nom que l'on souhaite donner au thème : pour que le style soit nommé glmf, le fichier doit s’appeler glmf.py et la classe qu’il contient doit être nommée GlmfStyle.

Pour tester le résultat en affichant le script Perl précédent :

```
./venv/bin/pygmentize -f terminal256 -O style=glmf -g fichiers.pl
```

Remplissons le fichier glmt.py :

```python
from pygments.style import Style
from pygments.token import Comment, Keyword, Name, Number, \Operator, Punctuation, String
class GlmfStyle(Style):    
    default_style = ""    
    styles = {        
                Comment: 'underline #808',        
                Keyword: 'bold #00f',        
                Name: '#f00',       
                Number: 'bold ansicyan',        
                Operator: 'bg:ansired ansibrightyellow',        
                Punctuation: '#0f0',        
                String: 'bg:#fff #222',    
              }
```

À chaque type de symboles est associé un style à afficher avec une couleur, soit avec une valeur hexadécimale (ici #808 s’affiche en violet), soit avec un nom ansi (comme ansicyan). Il est possible de modifier l’affichage en soulignant (underline), en mettant en gras (bold), en définissant une couleur de fond (bg:#fff pour un fond blanc). Il est aussi possible d’utiliser des italiques (italic), mais cela n’aura pas d’incidence sur une sortie dans un terminal (mais sera visible sur une sortie html par exemple).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/linux-pratique/lp-115/pygments.glmf.png">

Application du style glmf toujours sur le même fichier. C’est tout de même plus… personnel, non ?

## Conclusion
Pygments est une référence classique concernant un besoin de la coloration syntaxique dans l’écosystème Python et est utilisé au-delà. La difficulté d’ajout d’un lexeur est fonction de la complexité du langage à traiter. L’ajout de style est plus simple, mais sa plus-value est probablement plus faible.

## Références
[1] Site officiel de pygments : http://pygments.org/
[2] Définition du format SGF : https://www.red-bean.com/sgf/

source : https://connect.ed-diamond.com/Linux-Pratique/lp-115/coloration-syntaxique-de-votre-code-avec-pygments