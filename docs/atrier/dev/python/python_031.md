# FastAPI

![logo-teal.png](/assets/img/developpement/python/logo-teal.png)

## Présentation
[FastAPI](https://fastapi.tiangolo.com/) est un framework pour la création d’API (synchrone ou non) avec Python 3.6+.
Le framework a été créé par [Sebastian Ramirez](https://github.com/tiangolo) en 2018. Et bien que FastAPI soit jeune, il tient déjà de grandes promesses, et souhaite moderniser les frameworks orientés API.

Pour cela, ce framework nouvelle génération mise sur plusieurs points clés :

- Rapidité : des performances à même de rivaliser avec NodeJS et Go (grâce à Starlette et Pydantics).
- Développement : simplicité de Python et framework intuitif.
- Robuste : code prêt pour la production.
- Standardisé : basé (et complètement compatible) avec des standards open source, notamment OpenAPI et JSON schema.
- Documentation : pas besoin d'ajout de code, elle est automatiquement générée à partir de l'existant.

## FastAPI repose sur deux géants
[Starlette](https://www.starlette.io/): pour la partie web
↪ framework [ASGI](https://asgi.readthedocs.io/) (Asynchronous Server Gateway Interface) très léger.
Un **ASGI** est une interface entre un serveur web Python (synchrone ou non) et des applications, permettant le traitement de différents types de protocoles (par exemple HTTP, HTTP/2 et Websocket).

[Pydantics](https://pydantic-docs.helpmanual.io/): pour les données
↪ C'est lui qui va gérer toute la validation des données, leur sérialisation et la documentation automatique du modèle (basée sur le schéma JSON). En plus de ça, Pydantics applique les type hints au moment de l'exécution, et fournit des erreurs explicites lorsqu'elles ne sont pas valides.

Petit aparté pour expliquer ce que sont les type hints (ou les indications de type dans la langue de Molière) :

Python dans sa version 3.5[1] les fait apparaître. Par exemple :

```python
def greeting(name: str) -> str:
    return ‘Hello’ + name
```

Les collections `dict`, `list`, `set`, `tuple` sont utilisables via le module typing.

Elles ressemblent aux types en TypeScript dans leur syntaxe. Leur but est principalement informatif et visent à aider pour l’analyse statique et le refactoring et non pas à un contrôle du typage lors de l’exécution, quand bien même cela reste possible par un tiers (ici Pydantics).
Les auteurs tiennent à rappeler que :

> “Python restera un langage à typage dynamique et il n’est pas question de rendre les indications de types obligatoires“

## Installation
Avant toute chose, il faut installer Python 3.6. si ce n'est pas déjà fait voici le lien https://www.python.org/downloads/.

L’installation se fait en toute simplicité (comme n’importe quelle installation de module python d’ailleurs), on utilisera notre fidèle `pip` (installateur de modules pour Python).

### Environnement virtuel
Mais avant de réaliser l’installation, je ne peux que vous conseiller de créer au préalable un environnement virtuel Python. Cela va permettre d’isoler l’installation des dépendances et de ne pas les écrire directement sur la machine mais dans l’environnement. Imaginons par exemple qu’un projet utilise la version 1.0 d’une librairie et que la version 2.0 soit nécessaire sur un autre. On se retrouve dans une situation où, on ne peut qu’exécuter l’un ou l’autre dû à des conflits entre les versions.

C’est là que les environnements virtuels sont la solution. Chaque projet aura son environnement avec la version de la librairie dont il aura besoin.

Leur utilisation est conseillée pour tous les projets Python, alors pensez-y :) !

Afin de le créer et le gérer, il faut utiliser le module [venv](https://docs.python.org/3/library/venv.html#module-venv).
Pour le créer il suffit de taper la commande suivante dans le dossier de votre choix

```shell
python -m venv example-venv
```

Cela créera un dossier example-venv qui contiendra une copie de l’interpréteur Python, la librairie standard et divers fichiers de support. Un fois l'environnement créé, il faut l’activer :

- Sur Windows : `.\example-venv\Scripts\activate`
- Sur Linux/Mac : `source example-venv/bin/activate`

Une fois exécuté, votre terminal affichera entre parenthèses le nom de votre environnement.
Pour le désactiver, il faut taper `deactivate`
Et voilà, nous sommes fin prêts à installer FastAPI !

### FastAPI
Pour l'installer, il suffit de taper (dans votre virtual env): `python -m pip install fastapi`

FastAPI est désormais prête à être utilisée. Il nous faut ensuite un serveur ASGI pour la production, la documentation nous propose Uvicorn ou Hypercorn, qui peuvent eux aussi être installés avec pip : `pip install uvicorn[standard]`

## Utilisation
### Hello World
Commençons par un classique "Hello World".

```python
# main.py
from fastapi import FastAPI
 
app = FastAPI()
 
@app.get("/")
async def root():
    return {"message": "Hello World"}
```

### Etape 1
On importe la classe FastAPI (elle hérite de Starlette), elle encapsule toutes les fonctionnalités pour notre API.

### Etape 2
On crée une instance nommée `app`.
Cette variable est importante, ça sera notre principal point d'entrée pour la création de notre API. C'est également elle qui est appelée pour lancer notre serveur, avec cette commande : `uvicorn main:app` (`main` est notre module `main.py`)

### Etape 3
On crée un "path operation decorator".
Le décorateur `@app.get("/")` est là pour attribuer à la fonction qui suit la charge des requêtes:

- d'opération HTTP `GET`
- de route `/`

Ainsi, un "path operation decorator" est là pour définir l'opération HTTP et le chemin/route du endpoint.

### Etape 4
On définit la fonction associée au endpoint. Elle sera appelée à chaque fois que FastAPI recevera une requête correspondant à son décorateur (ici d'URL `/` et d'opération `GET`).
Dans notre cas, `root` est une fonction asynchrone, qui renvoie un dictionnaire avec le message "Hello World".

## Documentation
L'une des grandes fonctionnalités de FastAPI, c'est sa documentation d'API automatiquement générée (à l'instar de Django REST Framework).

Ainsi, si on va à l'adresse du serveur suivie de :

- `/docs` : on aura accès au Swagger UI de l'API.
- `/redoc` : on aura accès à OpenAPI (la version 3+ de Swagger).

![capture-1.png](/assets/img/developpement/python/capture-1.png)
*(figure. SwaggerUI de notre Hello World)*

## Allons un peu plus loin
Maintenant que nous avons les notions de bases. Complexifions un peu en intégrant d'autres éléments dans nos requêtes :

Les **path parameters** sont des variables présentes dans l’URL, le plus souvent utilisées pour spécifier la ressource dans une collection.
Par exemple : `https//api.github.com/users/:username/repos` permettrait d’avoir les repos rattachés à nom d'utilisateur.

Les **query parameters** se trouvent aussi dans l’URL mais à la fin derrière le point d’interrogation et séparés par l'esperluette &. Ils sont a contrario utilisés pour enrichir/contrôler le retour des ressources, les trier, les filtrer, etc.
Par exemple : `https//api.github.com/users/:username/repos?sort=created_date&order=asc` permet d’avoir accès à l’ensemble des repos d'un utilsateur triés par date de création dans l’ordre croissant.

### Path parameters

```python
from fastapi import FastAPI
 
app = FastAPI()
 
@app.get("/users/{user_id}")
async def get_user(user_id: int):
     return {"user_id": user_id}
```

Dans le décorateur, on définit une variable `user_id` dans la route, et sa valeur sera transmise à la fonction dans l'argument du même nom.

Signaler le type du path parameter dans la fonction, permet d'activer les mécanismes de validation et de conversion de données.
Donc si on exécute cette exemple et qu'on va à http://127.0.0.1:8000/users/3, on aura la réponse suivante : `{"user_id": 3 }`.
Alors qu'aller à http://127.0.0.1:8000/users/bar nous renverra une erreur.

Il est bien sûr possible d'en avoir plusieurs, il suffit de référencer chaque path parameter en tant que paramètre dans la fonction.

```python
@app.get("/planets/{planet}/moons/{moon}")
async def get_moon(planet: str, moon: str):
    return {"planet": planet, "moon": moon }
```

### Query parameters

```python
numbers = [n for n in range(20)]
 
@app.get("/numbers")
async def get_numbers(skip: int = 0, limit: int = 20):
    return numbers[skip: skip + limit]
```

Contrairement aux path parameters, ils sont seulement définis dans la fonction (et dans ce cas avec des valeurs par défaut), c'est comme ça que FastAPI les différencie.

Pour signaler qu'un query parameter est optionnel, il faut le spécifier dans son type `Optional[type] = None`. Sinon, il sera un obligatoire.

### Request body
Un request body est la donnée contenue dans le corps d'une requête HTTP. Afin de valider, convertir et manipuler un request body, il est préférable de modéliser la structure de données qui lui correspond.

```python
from typing import Optional
from pydantic import BaseModel
 
class User(BaseModel):
    id: int
    name: str
    surname: Optional[str] = None 
    email: str
```

Exemple pour utilisateur défini par un id, un nom, un prénom et un email. Pour cela, on crée un `schéma` de données comme une classe héritant de `BaseModel` provenant de Pydantic. Elle apporte un certain nombre d'attributs/fonctions dont certaines permettent l'exportation au format JSON, l'intégration à un **ORM** (la documentation propose des tutoriels avec SQLAlchemy, Peewee et Couchbase). Et, on définit les différents attributs avec leur type.

Cet héritage de classe permet aussi de garantir qu'une instance créée depuis un request body est conforme au modèle. Il passera par le même système de validation que pour les queries et path parameters.

Ces modèles peuvent être utilisés pour représenter des structures de données en base, des requests bodies ou encore des responses bodies.

Regardons comment faire :

```python
@app.post("/users/")
async def create_user(user: User):
    return user
```

Comme précédemment, le request body est déclaré comme paramètre dans la fonction.

On peut bien évidemment déclarer les trois en même temps, FastAPI se chargera alors de les dissocier et d’attribuer à chacun sa valeur respective.

- Si le paramètre est aussi déclaré dans le path, il sera interprété comme path parameter
- S’il est un type élémentaire (int, float, string, bool, etc), il sera interprété comme query parameter
- S’il dérive d’un type issu de `BaseModel` de Pydantic, il sera interprété comme request body

## Tests
La réalisation de tests est un élément important du développement afin d’assurer une certaine qualité de code.

Pour leur réalisation et leur exécution, la documentation de FastAPI recommande l’utilisation de [Pytest](https://docs.pytest.org/), un framework de tests. Il se charge de détecter les fichiers de tests (commencer les noms des fonctions et fichiers par “`test_`”), de les exécuter et de fournir le résultat.

Afin de tester les différents points d’entrées de notre API, on va créer un `TestClient` qui permet de réaliser des requêtes sur notre API.

```python
from fastapi import FastAPI
from fastapi.testclient import TestClient
 
app = FastAPI()
 
@app.get("/")
async def read_main():
    return {"msg": "Hello World"}
 
client = TestClient(app)
 
def test_read_main():
    response = client.get("/")
    assert response.status_code == 200
    assert response.json() == {"msg": "Hello World"}
```

Dans cet exemple, on vérifie la réponse et son statut (ceci est un exemple, il est, d'ailleurs, plus que conseillé de mettre les tests dans un fichier séparé).

Pytest permet aussi la réalisation de **mocks** afin de tester des composants indépendamment des autres.

```python
def test_read_note(test_app, monkeypatch):
    test_data = {"id": 1, "title": "something",
                 "description": "something else"}
 
    async def mock_get(id):
        return test_data
 
    monkeypatch.setattr(crud, "get", mock_get)
 
    response = test_app.get("/notes/1")
    assert response.status_code == 200
    assert response.json() == test_data
```

**Note : L'ensemble du code n'a pas été fourni.**

Ici, `monkeypatch` (fonctionnalité de pytest) va remplacer la fonction `crud.get` par `mock_get` afin de vérifier le comportement du controller (la donnée renvoyée et son statut).

En plus de cela, il est également possible de surcharger des dépendances, utiliser une base de données de tests, tester des websockets, etc. D’autres fonctionnalités viendront s’ajouter au cours du temps.

## Conclusion
Dans cet article, nous avons vu les rudiments du framework FastAPI qui, malgré son jeune âge, offre bien plus de possibilités que les quelques exemples évoqués dans cet article.

Voici une liste non exhaustive de ce que FastAPI permet également :

- La gestion des cookies, formulaires, fichiers, etc.
- L’utilisation de bases de données relationnelles ou distribuées.
- L’utilisation des websockets.
- La sécurité par identification ou par autorisation.
- Et bien d’autres choses encore !

Je ne peux que vous conseiller d'y plonger un peu plus en parcourant sa [documentation](https://fastapi.tiangolo.com/) qui est aux petits oignons.

Ce framework porte très bien son nom car il est extrêmement rapide, battant à plates coutures d’autres concurrents comme Django et Flask (deux autres frameworks Python) et se trouve aussi avoir un avantage par rapport à ExpressJs.
Le tableau montre les scores obtenus des frameworks suivant leur temps de réponse.

![unnamed.png](/assets/img/developpement/python/unnamed.png)

Classement de frameworks suivant leurs différents scores aux tests de performance[2]
J’espère que cet article vous aura donné envie d'en apprendre davantage sur FastAPI ou sur Python en général.

D'ailleurs Python 3.10 est sorti très récemment :-)!

Références
- Source: [PEP](https://www.python.org/dev/peps/pep-0484/)
- Source: [TechEmpower Framework Benchmarks](https://www.techempower.com/benchmarks/#section=test&runid=7464e520-0dc2-473d-bd34-dbdfd7e85911&hw=ph&test=composite&l=v2p4an-db&a=2&f=4ftlb4-1-2ns3k-2hwcg-ezeh0-18y68-4-4r21q8-w-18y6k-1w4qp-sg)

source : https://blog.ippon.fr/