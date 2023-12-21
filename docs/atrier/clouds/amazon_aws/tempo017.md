---
title: Classifiez facilement du texte avec AWS Comprehend
description: 
published: true
date: 2023-06-21T05:21:34.908Z
tags: 
editor: markdown
dateCreated: 2023-06-21T04:30:11.953Z
---

# Classifiez facilement du texte avec AWS Comprehend

![capture-d--cran-du-2022-07-26-11-16-27.png](/assets/img/clouds/tempo/capture-d--cran-du-2022-07-26-11-16-27.png)

Ce n’est un secret pour personne : la quantité de données produites augmente chaque année. Elle atteindra même selon certaines estimations ([Statista](https://fr.statista.com/infographie/17800/big-data-evolution-volume-donnees-numeriques-genere-dans-le-monde/), [Silicon](https://www.silicon.fr/big-data-datasphere-mondiale-339433.html#:~:text=Selon%20la%20soci%C3%A9t%C3%A9%20am%C3%A9ricaine%20d,40%20zettaoctets%20environ%20en%202019.)) 180 zettaoctets en 2025 (soit 180 000 milliards de Go). Parmi ces données se trouvent des vidéos, du son, des images, mais également du **texte**. Bien qu’il ne soit pas simple de quantifier la proportion de ces données textuelles, il est en revanche relativement facile de se convaincre grâce à la multitude de sources de données existantes que **cette part n’est pas négligeable** (réseaux sociaux, services de communication, sources d’informations, etc.).

Pour tirer parti de ces données et les valoriser, il n’est pas rare de vouloir implémenter **une classification automatique de texte** (tri automatique de mails : cf. figure 1, indexation d’articles, analyse de tweets ou d’avis, etc.). Pour ma part, j’ai été confronté à cette tâche sur un projet consistant à créer un chatbot. Celui-ci était destiné à répondre à des questions concernant les compétences des collaborateurs d’Ippon. Afin de fournir des réponses adéquates, il était nécessaire de savoir quel type de question était posé. L’idée de classifier ces questions a donc rapidement été adoptée.

Dans cet article, je vous propose de découvrir **AWS Comprehend**, un service de **NLP** (Natural Language Processing) permettant, entre autres, de classifier automatiquement du texte en **temps réel**. Je vous présenterai d’abord de façon générale Comprehend. Puis, je vous détaillerai comment j’ai utilisé ce service pour implémenter un service de classification automatique de texte en temps réel sur mon projet de chatbot.

![textclassificationexample.png](/assets/img/clouds/tempo/textclassificationexample.png)

Figure 1 : Illustration de la classification de mails en catégorie “spam” / “non-spam”

Source : [Google developer text classification guide](https://developers.google.com/machine-learning/guides/text-classification)

## Présentation d’AWS Comprehend
Comprehend est un service d’AWS de NLP permettant d’**extraire des informations** et des relations dans des documents. Le terme “document” est très général et désigne tout texte (une phrase, un commentaire de plusieurs lignes, un article entier, etc.) dans un format brut (fichier .txt) ou plus riche (PDF, Word).

![logo_comprehend-5.png](/assets/img/clouds/tempo/logo_comprehend-5.png)

Figure 2 : Logo d'AWS Comprehend

Il est à noter qu’il existe un autre service appelé **Comprehend Medical** qui, comme son nom l’indique, permet de faire du NLP sur des données de santé afin d’extraire différentes informations et relations à caractère médical dans du texte (e.g. les entités “organes”, “posologie” ou “symptôme”). Dans cet article, je ne parlerai cependant pas de ce variant destiné au monde de la santé, mais du service plus générique **Comprehend**.

### Organisation du service
**Comprehend** est composé de 3 sections différentes : “Built-in insights”, “Custom” et “Topic Modeling”, permettant chacune de répondre à différents besoins.

La partie **Built-in Insights** regroupe différents modèles de Machine Learning déjà entrainés (donc prêts à être utilisés, d’où le nom “built-in”) permettant chacun de fournir des informations (ou “insights”) différentes. Il est ainsi possible d’interroger ces modèles afin d’obtenir :

- Les entités d’un texte (noms, lieux, dates, organisations, etc.)
- La langue dominante d’un texte
- Le sentiment général d’un texte (positif, négatif, neutre ou partagé)
- Des informations syntaxiques (verbes, sujets, ponctuation, etc.)
- …

La partie **Custom** du service permet, elle, d'entraîner des modèles sur un dataset personnel afin d’obtenir des prédictions en accord avec son domaine métier. Il est ainsi possible d’entraîner des modèles de **classification de documents** ou de **reconnaissance d’entités personnalisées** retournant respectivement des classes et des entités parmi une liste que vous aurez préalablement définie en fonction de votre besoin métier.

Mise en situation : disons que nous souhaitons reconnaître automatiquement des références de produits dans des mails de réclamation client afin de rediriger les demandes vers les équipes appropriées. La reconnaissance d’entités personnalisées est alors exactement ce qu’il nous faut, puisque nous pourrons entraîner un modèle à reconnaître l’entité “référence produit” dans le contenu des mails.

Enfin, la troisième et dernière section de Comprehend, **Topic Modeling**, permet de regrouper des documents similaires en “topic”. Les topics sont déterminés automatiquement et les documents n’ont pas besoin d’être annotés (classification non supervisée). Ils doivent cependant y avoir un nombre suffisant de documents (au moins 1000) et chaque document doit être composé au minimum de 3 phrases.

Les modèles des 3 parties sont utilisables de façon **synchrone ou asynchrone**, à l’exception de la partie “Topic Modeling” pour laquelle le mode synchrone n’est pas disponible. Le mode synchrone permet d’obtenir des prédictions en temps réel pour 1 à 25 documents simultanément. Pour des batchs de plus de 25 documents, il faut utiliser le mode asynchrone : les prédictions sont alors déposées dans un bucket S3, mais un temps de latence est observé (au moins environ 5 minutes).

### Facturation
La tarification de chacune des 3 parties est différente. Pour les modèles de la section “Built-in Insights”, la facturation se fait au **nombre de caractères envoyés**, par tranche de 100 caractères (appelée une **unité**) au prix de 0.0001$ / unité.

Pour le Topic Modeling, la règle est la suivante : 1$/tâche pour les 100 premiers Mo de données puis 0.004$/Mo au-delà de ces 100 premiers Mo.

Pour la section Custom le système est un peu plus complexe. Il y a tout d’abord des frais liés à la création de chaque modèle : **3$/heure d’entraînement** et 0.5$/mois de **frais de gestion**. Enfin, il faut rajouter à cela les frais liés à l’utilisation des modèles qui diffèrent selon le mode d’utilisation (synchrone/asynchrone) :

- Pour des prédictions asynchrones, le tarif est de 0.0005$/unité envoyée. À titre d’exemple, si l’on souhaitait obtenir des prédictions sur un roman entier (environ 65 000 mots, soit approximativement 400 000 caractères, espaces inclus), le coût serait de 2$.
- Pour le mode synchrone, un **endpoint** doit vous être alloué (ressource permettant de traiter en temps réel les demandes de prédictions). Ainsi le prix dépend de la **durée de fonctionnement** de l’endpoint (à la seconde près) et de la **performance** souhaitée pour l’endpoint. Cette performance correspond au nombre d’unités par seconde que l’endpoint peut traiter et se compte en nombre “d’Unité d’Inférence” (UI). On peut donc au minimum attribuer 1 UI à l’endpoint ce qui correspondrait à un débit de 100 caractères par seconde. Le tarif appliqué est alors de 0.0005$/UI/seconde et est indépendant du nombre de requêtes effectuées. Ainsi, si l’on souhaitait par exemple mettre en place un système de classification de tweet (280 caractères) en temps réel fonctionnant de 8h à 18h du lundi au vendredi pour une charge à traiter d’environ 6 tweets par minute (soit 0,1 tweet/seconde ou 28 caractères/seconde), 1 UI suffirait largement. Le coût serait alors de 90$/semaine (0.0005$ * 1UI * 3600s * 10h * 5j).

Pour d'autres exemples concrets de projets utilisant Comprehend avec un calcul du coût total détaillé étape par étape, je vous conseille cette [page](https://aws.amazon.com/fr/comprehend/pricing/) d’AWS relative à la tarification de Comprehend.

### Comment utiliser Comprehend pour classifier du texte
Le chatbot que j’avais à réaliser devait être capable de répondre en temps réel à 3 types de questions :

- “Qui est + [nom d’une personne] ?” (classe “WHO_IS”),
- “Qui s’y connaît en + [nom d’une technologie] ?” (classe “WHO_KNOWS_ABOUT”),
- “Comment ça va ?” (classe “HOW_ARE_YOU”).

La classification personnalisée de document de façon synchrone était donc exactement ce qu’il me fallait (dans le cadre du projet un document sera une question posée au chatbot).

Laissez-moi maintenant vous décrire les étapes permettant de parvenir à mettre en place cette classification.

#### Création d’un dataset
Pour entraîner un modèle de Machine Learning, il faut des **données annotées**. En temps normal lorsque l’on entraîne un modèle “from scratch”, il faut un très gros dataset avec beaucoup de données. Heureusement, avec Comprehend il ne suffit que de quelques exemples. Il est demandé d’avoir au minimum **10 exemples par classes**, mais il est fortement conseillé d’en avoir plus de 50 afin d’obtenir de meilleures performances.

Le dataset peut prendre la forme d’un **fichier CSV** ou d’un Augmented Manifest File (fichier de sortie de SageMaker Ground Truth, un outil d’annotation de données d’AWS). J’ai personnellement utilisé le format CSV. Ce dernier doit alors respecter le format montré sur la figure 3. Notez que Comprehend ne supporte pas uniquement la classification **multi-classe** (aussi appelé single-label : chaque document est associé à une classe et une seule), mais aussi la classification **multi-label** (chaque document est associé à une classe ou plus).

![datatset_multi_classe-2.png](/assets/img/clouds/tempo/datatset_multi_classe-2.png)

Figure 3 : Format à utiliser pour le dataset au format CSV

Le dataset que j’ai constitué contenait en tout 390 documents. La figure 4 représente la répartition de ces documents sur les 3 classes. Pour information, il n’est pas nécessaire que ce dataset soit mélangé, Comprehend mélangera les données automatiquement avant l’entraînement. Aussi, sachez qu’il est possible, mais pas nécessaire, de fournir à Comprehend 2 datasets (un pour l’entraînement et un pour le test) afin d’avoir le contrôle sur les données qui servent à évaluer les performances du modèle. Si aucun dataset de test n’est précisé, Comprehend le créera lui-même aléatoirement à partir du dataset fourni. Il sera alors uniquement possible de savoir combien de documents ont été utilisés pour la phase de test, mais pas lesquels.

![taxonomy_intention-2.png](/assets/img/clouds/tempo/taxonomy_intention-2.png)

Figure 4 : Taxonomie du dataset utilisé pour la classification des questions

#### Lancement d’un entraînement
Une fois le dataset constitué et déposé dans un bucket S3, il est possible de créer un modèle en lançant un entraînement. Cela peut se faire depuis la console AWS (cf. figure 5) ou grâce à un **appel API** (cf. snippet 1).

![create_model_interface_carre_rouge-1.png](/assets/img/clouds/tempo/create_model_interface_carre_rouge-1.png)

Figure 5 : Interface permettant de lancer un entraînement (bouton “Create new model”)

```python
import boto3

comprehend_client = boto3.client('comprehend', region_name='eu-west-1')
response = comprehend_client.create_document_classifier(
    DocumentClassifierName = <how to name your model>,
    VersionName = <how to name the version of your model>,
    DataAccessRoleArn = <role arn that allows Comprehend to read and write data in S3>,
    InputDataConfig = {
        'DataFormat': 'COMPREHEND_CSV',
        'S3Uri': <s3 uri where the csv dataset is located>,
    },
    OutputDataConfig = {
        'S3Uri': <s3 uri where to write confusion matrix>
    },
    LanguageCode = 'fr',
    Mode = 'MULTI_CLASS'
)

document_classifier_arn = response['DocumentClassifierArn']
```

Snippet 1 : Code Python permettant de lancer un entraînement

À l’issue de l’entraînement (qui a dans mon cas duré **une vingtaine de minutes**), un modèle de classification (ou “**document classifier**” dans le jargon Comprehend) a été créé. Le modèle est alors visible dans l’onglet “Custom classification” et il est possible de visualiser différentes **métriques** permettant d’évaluer les performances du modèle (cf. figure 6). La matrice de confusion est, elle, disponible dans le bucket S3 spécifié au moment du lancement de l’entraînement.

![perf_inteface-2.png](/assets/img/clouds/tempo/perf_inteface-2.png)

Figure 6 : Interface de visualisation des métriques du modèle créé

#### Création d’un endpoint
Une fois le modèle entraîné, il faut créer l’endpoint qui sera chargé de traiter nos demandes de prédictions en temps réel. Cela peut une nouvelle fois se faire via la console (onglet Endpoints > Create endpoint) ou grâce à un appel API (cf. snippet 2).

```python
response = comprehend_client.create_endpoint(
    EndpointName = <how to name the endpoint>,
    ModelArn = document_classifier_arn,
    DesiredInferenceUnits = <how many IU to use for this endpoint>
)
endpoint_arn = response['EndpointArn']
```

Snippet 2 : Code Python permettant de créer un endpoint

En une petite **vingtaine de minutes** l’endpoint est actif : la facturation commence alors (0.0005$/UI/seconde, soit 1.8$/UI/heure ou 43.2$/UI/jour). Notez qu’il est possible de modifier un endpoint existant pour changer le nombre d’UI attribué ou pour utiliser un autre modèle (cf. snippet 3).

```python
response = comprehend_client.update_endpoint(
    EndpointArn = document_endpoint_arn,
    DesiredModelArn = <another document classifier arn>,
    DesiredInferenceUnits = <how many IU to use for this endpoint>
)
```

Snippet 3 : Code Python permettant de modifier un endpoint

#### Obtention des prédictions
Une fois l’endpoint créé, il ne reste plus qu’à obtenir des prédictions. Il est possible de faire cela depuis l’onglet “Real time analysis” de la console, mais traditionnellement cela se fait plutôt sous la forme d’un appel API (cf. snippet 4). La réponse (cf. figure 7) est obtenue en environ 100ms.

```python
response_document = comprehend_client.classify_document(
    Text = "Qui est-ce qui fait du nodejs ?",
    EndpointArn = endpoint_arn
)
```

Snippet 4 : Code Python permettant d’obtenir une prédiction en temps réel

```json
{
    'Classes': [
        {'Name': 'HOW_ARE_YOU', 'Score': 0.010065868496894836},
        {'Name': 'WHO_IS', 'Score': 0.00743631087243557},
        {'Name': 'WHO_KNOWS_ABOUT', 'Score': 0.9824978709220886}
    ]
    'ResponseMetadata': {
		# RequestId, HTTPStatusCode, ...
    }
}
```

Figure 7 : Réponse retournée par l’endpoint

## Conclusion
**Comprehend** est donc un **service de NLP** relativement complet, proposant à la fois d’utiliser des modèles déjà entraînés pour des tâches communes (détection de la langue, analyse de sentiment, etc.), mais aussi **d’entraîner ses propres modèles** personnalisés de classification et de reconnaissance d’entités avec peu de données. Certaines tâches de NLP ne sont toutefois pas couvertes par ce service, comme la détection de paraphrases ou le question answering.

L’utilisation de Comprehend pour créer un modèle de classification personnalisée est **simple**, la [documentation](https://docs.aws.amazon.com/comprehend/latest/dg/how-document-classification.html) claire et les performances du modèle obtenu sur le projet chatbot sont bonnes (accuracy de 94 % sur 3 classes avec un dataset de 390 phrases). Ces résultats dépendent bien évidemment de la qualité des données ainsi que de la difficulté du problème. De plus, le mode synchrone a bien permis d'obtenir des **prédictions en temps réel** (~100ms). L’intégration d’une solution NLP est donc facilement réalisable, d’autant plus que tous les langages couverts par les SDK AWS sont utilisables pour interagir avec Comprehend.

En revanche, le coût de l’utilisation en temps réel de ce genre de modèle “custom” peut rapidement devenir très important. Si votre cas d’utilisation nécessite comme pour le projet chatbot de pouvoir obtenir des prédictions 24h/24 et 7j/7, il vous en coûtera environ **1300$/mois** (prix correspondant à un endpoint possédant 1 unité d’inférence, c’est à dire capable de traiter 100 caractères par seconde, le minimum possible). Cette somme trop conséquente m’a poussé à **mettre de côté le service Comprehend** et à créer mon propre modèle de classification. La création de ce nouveau modèle (obtenu grâce au mécanisme de transfer learning) et son déploiement (via **SageMaker**, le service phare d’AWS pour l’industrialisation de modèles) fera l’objet de deux autres articles, à paraître bientôt.

En définitive, compte tenu de ces coûts de disponibilité très conséquents, la partie “Custom” de Comprehend semble plutôt adaptée à des besoins de **prédictions asynchrones** ou synchrones sur des plages de temps restreintes.

Voir les 2 autres articles de cette série :

---

# Mettez en place une classification de texte performante grâce à un CamemBERT

![illustration_article.png](/assets/img/clouds/tempo/illustration_article.png)

Pour traiter ou valoriser des données textuelles, il est souvent nécessaire d’implémenter une [classification automatique de texte](https://ledatascientist.com/introduction-a-la-categorisation-de-textes/?ref=blog.ippon.fr). Pour ma part, j’ai été confronté à cette tâche sur un projet consistant à créer un chatbot, voué à être utilisé par les commerciaux et directeurs d’agence afin de répondre à des questions concernant les compétences des collaborateurs d’Ippon. Pour fournir des réponses adéquates, il était nécessaire de reconnaître automatiquement quel type de question était posé. Le chatbot devant supporter 3 types de questions (“Qui est + [nom d’une personne] ?”, “Qui s’y connaît en + [nom d’une technologie] ?” et “Comment ça va ?”), j’ai donc cherché à réaliser une classification à 3 classes (respectivement appelées WHO_IS, WHO_KNOWS_ABOUT et HOW_ARE_YOU).

Pour réaliser ce genre de modèle de Machine Learning deux options existent : utiliser une **solution managée ou faire soi-même** à l’aide d’un framework ML. Afin de concentrer mes efforts sur l’industrialisation du modèle, j’ai tout d’abord opté pour le premier choix. J’ai ainsi eu l’occasion d’utiliser [Comprehend](https://aws.amazon.com/fr/comprehend/?ref=blog.ippon.fr), un service de NLP d’AWS permettant, entre autres, de créer facilement des modèles de classification de texte personnalisée à partir d’un petit jeu de données. Toute cette épopée est d’ailleurs racontée dans mon précédent article “Classifiez facilement du texte avec Comprehend”. Si les performances étaient tout à fait satisfaisantes, **le coût de l’utilisation** du modèle en temps réel 24h/24 était en revanche trop élevé (1300$/mois). C’est pourquoi je me suis finalement tourné vers la seconde option : construire le modèle moi-même.

Dans cet article, je vous propose donc de découvrir comment **créer un modèle de classification de texte performant** avec peu de données grâce au transfer learning. Je commencerai par donner quelques rappels théoriques sur cette méthode d’apprentissage ainsi que sur le fonctionnement de la bibliothèque transformers d’Hugging Face ayant servi à la réalisation du transfer learning. J’aborderai ensuite, après cette partie théorique, le côté pratique à travers l'entraînement du modèle, son utilisation et les résultats obtenus.

## Un peu de théorie
### Qu’est ce que le Transfer Learning ?
Pour entraîner un modèle de Machine Learning de façon supervisée (comme le nécessite une classification de document), il faut des **données annotées**. Dans le cas d’un entraînement “from scratch” ce dataset doit être très important afin d’obtenir de bonnes performances. Cependant, on ne dispose bien souvent en réalité que de peu de données annotées, car soit **peu de données** sont à notre disposition, soit il serait trop fastidieux d’en annoter un très grand nombre.

Heureusement, avec le [transfer learning](https://arxiv.org/abs/1808.01974?ref=blog.ippon.fr), petit dataset et bonnes performances ne sont plus incompatibles. Le principe de ce mécanisme est assez simple : utiliser un modèle de Deep Learning (réseau de neurone) pré-entraîné, performant sur une tâche assez générique (différente de la nôtre), et le modifier très légèrement afin d’obtenir une sortie correspondant à notre besoin. Le tout sans faire perdre au modèle les “connaissances” qu’il avait acquises.

La phase de légère modification du modèle original s’appelle le **fine-tuning**. Cette étape se fait en 2 temps et est représentée sur la figure 1. Il faut premièrement **modifier les dernières couches** du modèle initial afin d’obtenir un format de sortie en adéquation avec notre tâche. Puis, il reste à **entraîner ce modèle** légèrement différent sur notre petit jeu de données (spécifique à notre tâche). Peu de données suffisent donc, car il n’y a qu’une petite partie du modèle à entraîner. De ce fait, l’entraînement sera également beaucoup plus rapide que celui du modèle initial et nécessitera ainsi moins de ressources de calcul.

![transfer_learning_3.png](/assets/img/clouds/tempo/transfer_learning_3.png)

Figure 1 : Illustration du mécanisme de fine-tuning lors du transfer learning

Pour réaliser cette étape de fine-tuning, il est possible de faire les choses “à la main” en modifiant le modèle initial avec un framework de ML tel que PyTorch ou TensorFlow. Cependant, il existe des **bibliothèques** abstrayant ces manipulations assez fines grâce à des fonctions de plus haut niveau. C’est le cas de la bibliothèque open source Python [transformers](https://github.com/huggingface/transformers?ref=blog.ippon.fr), créée en 2016 par l’entreprise Hugging Face, et permettant, entre autres, de fine-tuner des modèles de NLP.

En plus d’avoir créé cette bibliothèque, Hugging Face (HF) regroupe des centaines de modèles, parmi lesquels **BERT** (Bidirectional Encoder Representations from Transformers), un des modèles de NLP anglais les plus populaires. Ce dernier, basé sur l’architecture **Transformers** (architecture aujourd’hui à l’état de l’art dans le traitement du langage naturel et qui a inspiré HF pour le nom de sa bibliothèque) a été entraîné pour des tâches génériques sur un grand nombre de documents, lui permettant d’avoir en quelque sorte “appris à parler anglais”.

Les questions posées au chatbot étant en français, je me suis intéressé à 2 modèles de NLP français dérivés de BERT : **CamemBERT** et **FlauBERT** (cf. figure 2). Ces deux modèles publiés en 2020 possèdent des performances assez proches comme le suggère [cet article](https://hal.archives-ouvertes.fr/hal-02784776v3/document?ref=blog.ippon.fr#:~:text=L'entr%C3%A9e%20du%20mod%C3%A8le%20pour,fonctionne%20l%C3%A9g%C3%A8rement%20mieux%20que%20CamemBERT.) (cf. partie 4, tableau 2). Mon choix s’est porté sur CamemBERT car contrairement à son homologue il possède dans HF un "Tokenizer Fast" (version améliorée du tokenizer, un outil que j’aborderai plus tard) nécessaire dans certains tutoriels de HF que je comptais suivre.

![hf_camembert_flaubert.png](/assets/img/clouds/tempo/hf_camembert_flaubert.png)

Figure 2 : Logos des technologies et modèles liés au transfert learning

### Fonctionnement du fine-tuning avec transformers
Le principe de fonctionnement du fine-tuning avec transformers est relativement simple. Tout l'exotisme ne réside que dans la première étape qui consiste à définir une variable représentant le modèle que l’on souhaite fine-tuner. Cela se fera grâce à une méthode appelée `from_pretrained`. Cette méthode devra être appelée via une des classes de la bibliothèque transformers, classe à choisir selon 3 critères :

- le framework de ML que l’on souhaite utiliser pour paramétrer l’entraînement
- le modèle à fine-tuner
- le type de tâche sur laquelle fine-tuner le modèle

Dans mon cas, je souhaitais fine-tuner CamemBERT sur une tâche de classification de texte (appelée “SequenceClassification” dans transformers) en utilisant la syntaxe de TensorFlow (TF) avec laquelle je suis plus à l’aise que PyTorch. C’est pourquoi j’ai choisi la classe `TFCamembertForSequenceClassification` parmi celles décrites dans la [documentation](https://huggingface.co/docs/transformers/model_doc/camembert?ref=blog.ippon.fr). L'instanciation se fait alors comme décrit dans l’extrait de code 1.

```python
import transformers

model = transformers.TFCamembertForSequenceClassification.from_pretrained("camembert-base", config)
```

Extrait de code 1 : Initialisation du modèle

Comme vous pouvez le remarquer, il faut préciser en paramètre de la méthode `from_pretrained` deux arguments :

- L’id exact du modèle à utiliser, car il existe [différentes versions de CamemBERT](https://huggingface.co/camembert?ref=blog.ippon.fr) (modèles plus ou moins légers, avec des entraînements différents, etc.).
- La configuration à utiliser (contenant ici notamment le nombre de classes souhaitées en sortie du modèle) : c’est ce qui permettra à la bibliothèque de savoir comment modifier les dernières couches du modèle.

La suite du fine-tuning est tout ce qu’il y a de plus classique : définition des paramètres d’entraînement (loss, optimizer, métriques) compilation du modèle et lancement de l’entraînement (cf. l’extrait de code 2).

```python
import tensorflow
loss = tensorflow.keras.losses.SparseCategoricalCrossentropy()
optimizer = tensorflow.keras.optimizers.Adam()

model.compile(
    optimizer, 
    loss, 
    metrics=["accuracy"]
)

model.fit(
    training_data,
    validation_data,
    epochs,
    callbacks
)
```
Extrait de code 2 : Paramétrisation et lancement du fine-tuning.

Il est important de noter que les données d’entraînement et de validation (+ celles de test) doivent être **pré-traitées** avant d’être utilisées pour un entraînement. Pour les labels, une simple conversion des noms de classe en id suffira (WHO_IS : 0, WHO_KNOWS_ABOUT : 1 et HOW_ARE_YOU : 2). Pour les questions, une étape de [tokenisation](https://www.analyticsvidhya.com/blog/2020/05/what-is-tokenization-nlp/?ref=blog.ippon.fr) doit être faite afin de les transformer en valeurs numériques. Celle-ci consiste à utiliser un **tokenizer** pour découper le texte en morceaux (des tokens) recensés dans un dictionnaire propre au modèle initial. Chaque token est associé à un id, ce qui permet de convertir le texte initial en une suite de nombres (cf. figure 3). Certains tokens spéciaux sont automatiquement ajoutés lors de la tokenisation, comme au début et à la fin de la question (<s> et </s>). Ces derniers sont propres au modèle et servent à son bon fonctionnement.

![tokenizer.png](/assets/img/clouds/tempo/tokenizer.png)

Figure 3 : Exemple de tokenisation d’une question

## Mise en pratique
### Réalisation du fine-tuning
Si l’implémentation du fine-tuning m’avait l’air assez simple et concise, j’ai toutefois fait face à des **difficultés** pour pré-traiter les données (types des variables) et enregistrer le modèle. Je me suis alors tourné vers le dépôt [github de transformers](https://github.com/huggingface/transformers?ref=blog.ippon.fr). Celui-ci contient un [dossier regorgeant d’exemples](https://github.com/huggingface/transformers/tree/main/examples?ref=blog.ippon.fr) de réalisation de fine-tuning pour différentes tâches en PyTorch ou TensorFlow. Parmi ceux-ci, l’[exemple de classification de texte](https://github.com/huggingface/transformers/tree/main/examples/tensorflow/text-classification?ref=blog.ippon.fr) réalisée avec Tensorflow m’a particulièrement été utile. Ce script (assez long, environ 500 lignes, car très générique) permet de réaliser un **fine-tuning** du modèle de son choix sur une tâche de classification avec un dataset au format CSV ou JSON.

Avec ce script et un jeu de données de **390 exemples de questions** pour le chatbot réparties entre train, validation et test (cf. figure 4) j’ai pu lancer un premier entraînement sur 50 epochs avec une taille de batch de 8. Les suivis de la [loss](https://developers.google.com/machine-learning/crash-course/descending-into-ml/training-and-loss?hl=fr&ref=blog.ippon.fr) et de l’[accuracy](https://developers.google.com/machine-learning/crash-course/classification/accuracy?hl=fr&ref=blog.ippon.fr#:~:text=Accuracy%20is%20one%20metric%20for,predictions%20Total%20number%20of%20predictions) sur ces 50 epochs sont représentés sur les figures 5 et 6. Concernant le temps d’entraînement, j’ai pu observer une durée d’approximativement 20 secondes par epochs soit environ 17 minutes pour les 50 epochs (avec un processeur i7 et 16 Go de RAM).

![dataset_camembert.png](/assets/img/clouds/tempo/dataset_camembert.png)

Figure 4 : Description des 3 splits du dataset

![camembert_loss_evolution.png](/assets/img/clouds/tempo/camembert_loss_evolution.png)

Figure 5 : Évolution de la loss selon le nombre d’epochs

![camembert_accuracy_evolution.png](/assets/img/clouds/tempo/camembert_accuracy_evolution.png)

Figure 6 : Évolution de l’accuracy selon le nombre d’epochs

Ces courbes nous permettent de dire que **les performances sont excellentes**. En effet, la loss de validation diminue jusqu’à atteindre des valeurs très proches de zéro autour des epochs 20 à 30. Ce plateau se retrouve sur la courbe d’évolution de l’accuracy où celle-ci vaut alors 100%.

Suite à ces premiers résultats, un **mécanisme d’early stopping** à été mis en place pour arrêter l'entraînement lorsque le modèle ne s'améliore plus suffisamment (arrêt si diminution de la loss de validation inférieure à 0.01/epoch pendant 10 epochs). Avec ce mécanisme, l’entraînement s’est arrêté au bout de 20 epochs. La **matrice de confusion** du modèle obtenu est représentée sur la figure 7. Celle-ci nous permet de constater qu’aucune erreur n’a été commise par le modèle sur le split de test.

![matrice_confusion_20_epochs.png](/assets/img/clouds/tempo/matrice_confusion_20_epochs.png)

Figure 7 : Matrice de confusion du modèle obtenu

Obtention des prédictions
Après avoir sauvegardé le modèle entraîné avec `model.save_pretrained(path_where_to_save)`, on peut l’utiliser pour obtenir des prédictions en procédant comme suit (cf. extrait de code 3).

```python
# Load the model artifacts
model = TFCamembertForSequenceClassification.from_pretrained(path_to_model)
tokenizer = CamembertTokenizerFast.from_pretrained(path_to_model)
config = CamembertConfig.from_pretrained(path_to_model)

# Tokenize the input so that it can be processed by the model
tokenized_text = tokenizer(input_text,  truncation=True, return_tensors='tf')

# Get predictions and transform raw output to probabilities
raw_output = model(tokenized_text)
logits = raw_output["logits"]
probabilities = tensorflow.nn.softmax(logits[0])

# Build the predictions to be returned
# Note : id2label is a dict mapping label ids (0,1,2) to actual label names ('WHO_IS', ...)
predictions = [{"Class": config.id2label[label_id],
                "Probability": float(probabilities[label_id])}
                for label_id in config.id2label.keys()]
return predictions
```
Extrait de code 3 : Chargement du modèle et prédictions

Le retour contenant les prédictions obtenues pour la phrase “Qui est-ce qui fait du nodejs ?” est le suivant :

```json
[
{'Class': 'HOW_ARE_YOU', 'Probability': 0.0027706660330295563}, 
{'Class': 'WHO_IS', 'Probability': 0.0028453748673200607}, 
{'Class': 'WHO_KNOWS_ABOUT', 'Probability': 0.9943839311599731}
]
```

Prédiction obtenue après traitement

## Conclusion
La **bibliothèque transformers d’Hugging Face** permet donc, entre autres, de créer des modèles de classification de texte grâce au mécanisme de **transfer learning** en fine-tunant des modèles comme **CamemBERT**, un modèle de NLP français. Le principe d’utilisation de la bibliothèque est relativement simple, mais j’ai rencontré quelques difficultés, que j’ai heureusement réussi à surmonter grâce au [github huggingface/transformers](https://github.com/huggingface/transformers?ref=blog.ippon.fr) contenant des exemples de fine-tuning pour différentes tâches (dont la classification de texte).

J’ai ainsi pu réussir à fine-tuner un modèle, qui plus est avec d’**excellents résultats**. Ce modèle obtenu après un entraînement d’environ 7 minutes (20 epochs) n’a effectivement commis aucune erreur sur le split de test contenant une soixantaine d’exemples. Il serait intéressant de voir si ces excellentes performances se vérifient également sur des problèmes plus complexes avec un nombre de classes plus élevé qu’ici (seulement 3 classes).

Ces performances sont à mettre en parallèle avec celles du modèle de classification de texte obtenu avec **Comprehend** (un service de NLP d’AWS) dont il est question dans mon précédent article “Classifiez du texte avec AWS Comprehend”. L’accuracy de ce modèle créé grâce au service d’AWS était de 94%, ce qui est **légèrement en dessous** des 100% du présent modèle obtenu par transfer learning. Sur d’autres aspects en revanche, je pourrai être tenté de donner l’avantage à Comprehend, notamment en ce qui concerne l’industrialisation du modèle.

Je proposerai dans un prochain article une **comparaison** selon différents critères (coût, facilité de mise en œuvre, etc.) des 2 solutions (Comprehend VS CamemBERT), en mettant l’accent sur le travail à réaliser pour industrialiser les modèles dans chacun des cas. Je dresserai ainsi une synthèse de mes différents essais et recherches en présentant et confrontant les **avantages et inconvénients** de chaque stratégie. Ce troisième et dernier article devrait donc pouvoir, je l’espère, vous éclairer dans le choix de l’une ou l’autre solution pour la création de vos futurs modèles de classification de texte.

---

# Créer et industrialiser un modèle ML de classification de texte : AWS Comprehend VS un CamemBERT

![miniature.png](/assets/img/clouds/tempo/miniature.png)

Si parvenir à obtenir un modèle de ML fonctionnel sur sa machine est plutôt monnaie courante aujourd’hui, réussir à **déployer automatiquement** ce modèle **en production** est déjà beaucoup plus rare. Lors de sa conférence “MLOoops : comment faire échouer la mise en production du ML ?“ donnée au Salon de la Data 2022 à Nantes (voir cet [autre article du blog Ippon](https://blog.ippon.fr/2022/09/29/une-journee-au-salon-de-la-data-de-nantes-2022/)), Paul Peton (Tech Lead IA Chez Avanade), dit à ce propos que 85% des projets de ML n’arrivent pas en production. L’industrialisation des modèles a donc encore du terrain à conquérir.

![mlops-1.png](/assets/img/clouds/tempo/mlops-1.png)

Figure 1 : Illustration du workflow MLOps, autre nom donné à l’industrialisation de l’IA

Source : https://blent.ai/le-metier-de-machine-learning-engineer/

Début 2022, lors de mon stage de fin d’études chez Ippon, j’ai dû réaliser avec d’autres stagiaires un chatbot sous la forme d’une application mobile. Destiné aux commerciaux et directeurs/directrices d’agence, ce chatbot est chargé de répondre à des questions concernant les compétences des collaborateurs d’Ippon. En tant que stagiaire data, je gérais la partie IA du chatbot. J’ai ainsi été amené à réaliser un modèle de classification de texte, utilisé pour déterminer, à la réception d’une question, quel type de question a été posé.

Pour réaliser ce modèle, j’ai d’abord utilisé la **solution managée Comprehend** (service NLP d’AWS). Toute cette épopée est d’ailleurs racontée dans mon premier article “Classifiez facilement du texte avec Comprehend”. Cependant, le coût de fonctionnement du chatbot était trop important avec cette solution (~1300$/mois). J’ai ainsi dans un second temps réalisé le modèle moi-même à l’aide d’un **transfer learning** sur le modèle de NLP français **CamemBERT** (voir mon second article “Mettez en place une classification de texte performante grâce à un CamemBERT”).

Dans ce **troisième et dernier article** de la série concernant la classification de texte, je vous propose donc de dresser un bilan en comparant les 2 stratégies de création de modèle que j’ai suivies afin de déterminer quelle solution est la meilleure. Nous confronterons les 2 approches selon 5 critères : le **coût**, la **facilité de création** du modèle, la **facilité d’industrialisation**, la **rapidité** d’inférence et les **performances** du modèle. Nous dresserons ainsi une matrice de décision qui pourra vous servir à faire le choix entre l’une ou l’autre des solutions.

## Rappel du contexte et méthode de comparaison
Pour la suite de cet article, je supposerai que vous avez lu les 2 précédents articles de la série (article n°1, article n°2), mais voici un petit rappel. Le chatbot devant supporter 3 types de questions j’ai cherché à réaliser une classification à 3 classes :

- “Qui est + [nom d’une personne] ?” ⇒ classe WHO_IS
- “Qui s’y connaît en + [nom d’une technologie] ?” ⇒ classe WHO_KNOWS_ABOUT
- “Comment ça va ?” ⇒ classe HOW_ARE_YOU

À l’avenir d’autres types de questions pourront être ajoutés. Les données d’entraînement du modèle de classification sont, de ce fait, amenées à être modifiées, d’où la nécessité d’un déploiement du modèle en production le plus automatisé possible. Pour comparer la solution managée Comprehend à la solution basée sur le transfert learning de CamemBERT sur ce critère de la facilité d’industrialisation, j’attribuerai à chacune une **note de 1 à 5** (5 étant la meilleure note). J’en ferai de même pour tous les autres critères (facilité de création du modèle, coût, performances, etc.).

Aussi, afin de prendre en compte le fait que certains critères sont plus importants que d’autres, un **coefficient entre 1 et 5** sera attribué à chacun. Nous obtiendrons alors une matrice (voir figure 2) qui nous donnera, en faisant la somme pondérée des notes, la meilleure solution selon nos critères et nos pondérations.

![matrice_vierge.png](/assets/img/clouds/tempo/matrice_vierge.png)

Figure 2 : Matrice de décision servant à déterminer quelle solution est la meilleure

Pour le projet chatbot, le **critère le plus important** (coefficient 5) était celui de la **rapidité d’inférence** (le chatbot doit répondre aux questions quasi instantanément). La performance du modèle et le coût de la solution sont également 2 critères importants (coefficient 4). Si l’industrialisation du modèle est essentielle, le fait qu’elle soit facilement réalisable n’était pas spécialement primordial, ce qui explique le coefficient 2. Enfin, cette réflexion est aussi valable pour le dernier critère (facilité de création du modèle) qui a toutefois un peu plus d’importance (d’où le coefficient 3).

Lançons donc maintenant le match en commençant par une comparaison sur la facilité de création du modèle.

## Comparaison n°1 : Facilité de création du modèle
Du point de vue de ce critère, **Comprehend gagne le match** puisqu’il gère toute cette partie de façon autonome. Il suffit effectivement de soumettre un dataset (respectant [certaines contraintes](https://docs.aws.amazon.com/comprehend/latest/dg/prep-classifier-data-multi-class.html?ref=blog.ippon.fr)) pour qu’un entraînement soit automatiquement lancé. **Aucune connaissance en développement de modèles de ML** n’est donc requise. Après l’entraînement, le modèle est enregistré dans Comprehend et des métriques permettant d’évaluer ses performances sont consultables notamment via l’interface AWS.

La tâche est en revanche bien plus ardue avec CamemBERT car il faut faire le **transfer learning soit-même**. Il est de ce fait nécessaire, contrairement à la solution d’AWS, de mettre les mains dans le code et de posséder des connaissances dans le domaine du Machine Learning et du NLP. Il y a toutefois moins d’efforts à fournir que si nous avions dû construire un modèle from scratch. De plus, cette étape peut être grandement facilitée grâce à l’utilisation de la librairie [transformers d’Hugging Face](https://github.com/huggingface/transformers?ref=blog.ippon.fr) (dont le fonctionnement et l’utilisation ont été décrits dans l’article n°2 “Mettez en place une classification de texte performante grâce à un CamemBERT”). J’ai cependant dû me former à cet outil et j’ai rencontré quelques problèmes lors du développement, d’où ma note de 2/5.

Facilité de création du modèle (coefficient 3)

Comprehend : 5/5 - CamemBERT : 2/5

## Comparaison n°2 : Facilité d’industrialisation
Nous nous attaquons ici au plus gros point, même si dans le cadre du projet chatbot, puisqu’il ne s’agissait que d’un stage, l’industrialisation souhaitée était quelque peu allégée par rapport aux standards habituels. Nous ne voulions pas, par exemple, mettre en place de **vérification des performances** du modèle obtenu avant qu’il soit déployé et utilisé. Le gros du travail se concentrait donc sur **l’automatisation des étapes permettant de déployer un modèle**. Concrètement, nous souhaitions que lorsqu’un dataset au format JSON est déposé dans un bucket S3 (espace de stockage sur AWS), un entraînement soit automatiquement lancé puis qu’un endpoint soit créé sans la moindre action de notre part.

Voici donc quelques détails sur le travail que j’ai dû réaliser pour industrialiser le modèle selon ce fonctionnement dans chacun des deux cas.

### Industrialisation avec Comprehend
Pour Comprehend, j’ai réalisé l’architecture suivante (voir figure 3). Elle se lit de gauche à droite et peut être expliquée par le workflow suivant :

- Lorsqu’un **dataset JSON** contenant les données d’entraînement (ne respectant pas les [contraintes](https://docs.aws.amazon.com/comprehend/latest/dg/prep-classifier-data-multi-class.html?ref=blog.ippon.fr) imposées par Comprehend) est déposé dans un bucket S3, une lambda function (start_training) est lancée.
- Cette lambda function va **convertir** le fichier JSON en un fichier CSV (déposé dans un bucket S3) respectant les [contraintes](https://docs.aws.amazon.com/comprehend/latest/dg/prep-classifier-data-multi-class.html?ref=blog.ippon.fr) exigées par Comprehend et lancer un entraînement sur ces données.
- Avant de finir son exécution, la lambda function start_training va **lancer une step function** (service d’automates à états finis d’AWS) chargée de vérifier toutes les 2 minutes grâce au lancement d’une lamba function (check_model_status) l’**état de l'entraînement** (est-il toujours en cours, terminé, en échec, ...).
- Lorsque Comprehend a terminé l’entraînement du modèle, des métriques (notamment la **matrice de confusion**) sont créées et permettent d’évaluer les performances du modèle obtenu.
- Lorsque la step function aura permis de détecter que l’entraînement s’est terminé avec succès, elle lancera une Lambda function (create_endpoint) chargée de **créer un endpoint** référençant le modèle récemment obtenu afin de le **rendre disponible pour l’inférence**.

![industrialisation_comprehend.png](/assets/img/clouds/tempo/industrialisation_comprehend.png)

Figure 3 : Architecture mise en place pour industrialiser le modèle obtenu avec Comprehend

Le code des fonctions Lambda a été écrit en **Python** et l’ensemble des ressources AWS a été géré grâce à [Terraform](https://www.terraform.io/?ref=blog.ippon.fr) (outil d’Infrastructure as Code). Il s’agissait de ma première expérience de réalisation d’une architecture sur AWS, et la réalisation m’a paru plutôt accessible.

Pour information la durée totale du workflow entre le dépôt du fichier JSON et la fin de la création de l’endpoint est d’environ **30 à 35 minutes** (environ 20 minutes pour l’entraînement et 15 minutes pour la création de l’endpoint).

### Industrialisation avec CamemBERT
Pour CamemBERT, j’ai dû réaliser une architecture un peu différente en introduisant le service de Machine Learning de référence d’AWS : [SageMaker](https://aws.amazon.com/fr/sagemaker/?ref=blog.ippon.fr). Ce service se décrit comme une solution permettant de “**créer, entraîner et déployer rapidement** et facilement des modèles de ML pour tous les cas d'utilisation avec une infrastructure, des outils et des flux entièrement gérés”. Notez qu'il n'est pas du tout obligatoire de passer AWS, ni même forcément nécessaire de passer par un cloud provider pour industrialiser le modèle. Il s'agissait cependant du cadre privilégié pour le stage.

Dans mon cas, j’avais déjà créé le script d’entraînement du modèle sur ma machine grâce à un transfer learning de CamemBERT réalisé à l’aide de la librairie [transformers](https://huggingface.co/docs/transformers/index?ref=blog.ippon.fr) (voir mon précédent article). J’ai donc cherché à utiliser uniquement les fonctionnalités d'entraînement et de déploiement des modèles que propose SageMaker. Dans sa [documentation](https://aws.amazon.com/es/blogs/aws-spanish/introduccion-a-sagemaker-studio/?ref=blog.ippon.fr), SageMaker explicite le workflow à suivre permettant de réaliser ces deux étapes grâce au schéma suivant (voir figure 4).

![sagemaker.png](/assets/img/clouds/tempo/sagemaker.png)

Figure 4 : Schéma représentant le workflow d’utilisation d’AWS SageMaker

Source : https://aws.amazon.com/es/blogs/aws-spanish/introduccion-a-sagemaker-studio/

Tout commence avec la brique rouge “Training Data” située en bas à gauche. Ces données vont être utilisées par SageMaker en complément d’une **image Docker stockée sur AWS ECR** (Elastic Container Registry) contenant le code d’entraînement du modèle. Le service pourra ainsi **lancer l’entraînement** sur le [type d’instance](https://aws.amazon.com/fr/sagemaker/pricing/?ref=blog.ippon.fr) d’entraînement de notre choix. À l’issue de l’entraînement, les informations du modèle (ou **model artifacts** : poids, métadonnées, etc.) seront déposées dans un bucket S3.

Il est alors possible de **créer un endpoint**, ressource qui sera le point d’entrée des demandes d’inférence. Lorsqu’une prédiction est demandée par un client, SageMaker charge le modèle et le **code d’inférence** (également stocké dans une image Docker présente sur ECR) sur le type d’instance d’inférence de notre choix. Le client à l’origine de la demande d’inférence reçoit alors une réponse de la part de l’endpoint.

J’ai donc très légèrement adapté ce workflow à notre cas d’utilisation et ajouté des Lambda functions pour faire le lien entre les différentes étapes, ce qui a donné l’architecture suivante (voir figure 5).

![industrialisation_camembert.png](/assets/img/clouds/tempo/industrialisation_camembert.png)

Figure 5 : Architecture mise en place pour industrialiser le modèle obtenu par transfer learning de CamemBERT

Comme pour l’architecture utilisant Comprehend, j’ai utilisé Python et Terraform. J’ai en revanche eu beaucoup plus de mal à réaliser cette solution pour 2 raisons :

1. Je n’étais **pas du tout familier** avec la **création d’images Docker** et les concepts de conteneurisation. J’ai donc dû essuyer quelques plâtres avant d’arriver à mes fins.
1. SageMaker est un service **extrêmement vaste, complet et complexe** que j’ai dû découvrir en partant de 0. Il y a beaucoup de vocabulaire à intégrer et l’on se perd rapidement dans la documentation. Aussi, il y a plusieurs façons de manipuler SageMaker : utiliser le SDK AWS traditionnel ([boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html?ref=blog.ippon.fr) dans mon cas), utiliser le [SDK SageMaker](https://sagemaker.readthedocs.io/en/stable/?ref=blog.ippon.fr) censé nous extraire de la trop grande complexité des appels API de boto3, ou bien utiliser la solution [SageMaker pipelines](https://aws.amazon.com/fr/sagemaker/pipelines/?ref=blog.ippon.fr). Après avoir exploré les 3 pistes pour choisir selon moi la plus accessible, j’ai opté pour le SDK SageMaker. J’ai cependant rencontré plusieurs problèmes dans le développement de mes fonctions Lambda à cause de la taille de la librairie (trop lourde pour être attachée au zip de la lambda) et de bugs toujours existants comme [cette erreur d’import](https://github.com/aws/sagemaker-python-sdk/issues/1200?ref=blog.ippon.fr#issuecomment-605785881).

Avec un peu de persévérance, j'ai finalement réussi à mettre en place cette architecture et faire fonctionner le workflow. Pour information la durée totale du workflow entre le dépôt du fichier JSON et la fin de la création de l’endpoint est d’environ **12 à 14 minutes** (environ 6/7 minutes pour l’entraînement et 6 minutes pour la création de l’endpoint), soit près de 2 fois plus rapide que l’architecture précédente.

Sur ce second critère de facilité d’industrialisation, c’est donc à Comprehend que je donne l’avantage. Il est effectivement nettement **plus simple d’industrialiser le modèle avec Comprehend** (qui intègre déjà par nature une partie de cette industrialisation) plutôt qu’avec CamemBERT où tout est à faire.

Facilité d’industrialisation du modèle (coefficient 2)

Comprehend : 4/5 - CamemBERT : 2/5

## Comparaison n°3 : Rapidité d’inférence
Concernant la rapidité d’inférence du modèle, j’ai testé les 2 solutions en leur soumettant 4 fois une série de 10 demandes de prédictions (en laissant 1 seconde entre chaque demande d’une même série). Pour le modèle obtenu avec Comprehend, j’ai pu observer un **temps de réponse moyen de 106 ms** (écart type de 64 ms) par prédiction. Pour le modèle obtenu avec CamemBERT et hébergé sur SageMaker, j’ai pu cette fois observer un **temps de réponse moyen de 344 ms** (écart type de 82 ms), soit plus de 3 fois plus long que Comprehend (mais ce résultat est tout de même satisfaisant pour notre cas d’utilisation).

Attention toutefois à la question du **volume de demande à traiter** sur une période de temps donnée. Dans notre cas d’utilisation, le chatbot devait recevoir un nombre très limité de questions (nous avons estimé ce nombre à maximum 10 par minute). C’est ainsi que nous avons décidé d’utiliser pour faire l’inférence du modèle basé sur CamemBERT une instance **ml.t2.medium** (la moins chère et moins performante des [instances possibles](https://aws.amazon.com/fr/sagemaker/pricing/?ref=blog.ippon.fr) sur SageMaker). Pour Comprehend, nous n’avons pas la main sur le type d’instance à utiliser pour l’inférence. Il est néanmoins possible de choisir le **nombre d’unités d’inférence** (UI) à attribuer à l’endpoint, qui correspond au nombre de caractères par seconde que ce dernier est capable de traiter. Nous avons choisi le nombre minimum d’UI (1) qui correspond à un débit de 100 caractères/s.

Les temps moyens donnés ci-dessus correspondent à un temps de réponse pour une requête envoyée seule lorsque le modèle n’est pas sollicité. Si les 10 phrases sont envoyées les unes à la suite des autres sans pause entre elles, les résultats sont différents. Pour Comprehend le **temps moyen passe à 309 ms** avec un écart type de 340 ms (le débit maximum de 1 UI est atteint, l'enpoint est saturé, certaines requêtes prennent alors plus d’une seconde). Pour CamemBERT, le temps de réponse reste stable avec **un temps de réponse moyen de 345 ms** (écart type de 85 ms).

Compte tenu de ces **résultats très satisfaisants** pour notre cas d’utilisation (réponse fournie quasi instantanément, même si les 10 questions sont posées les unes à la suite des autres), et du fait qu’il soit assez difficile de **comparer directement et équitablement** les 2 solutions (à cause du manque de transparence de Comprehend sur l’instance utilisée pour faire l’inférence) j’ai décidé de donner la note maximale aux 2 solutions.

Rapidité d’inférence (coefficient 5)

Comprehend : 5/5 - CamemBERT : 5/5

## Comparaison n°4 : Performances
Du côté des performances des modèles les 2 solutions montrent de **très bons résultats**. Voici les matrices de confusions des modèles présentées dans chacun des 2 articles précédents. Pour rappel le dataset entier contient **390 exemples de phrases**. Cependant, la répartition entre les splits de train, de validation et de test ne sont pas les mêmes pour les deux modèles (39 phrases dans le split de test pour Comprehend contre 59 pour CamemBERT).

![confusion_comprehend.png](/assets/img/clouds/tempo/confusion_comprehend.png)

Figure 6 : Matrice de confusion du modèle obtenu avec Comprehend

![confusion_camembert.png](/assets/img/clouds/tempo/confusion_camembert.png)

Figure 7 : Matrice de confusion du modèle obtenu par transfer learning de camemBERT

Avec ces matrices, on peut se dire que le modèle basé sur **CamemBERT semble plus performant** que le modèle obtenu avec Comprehend car il ne commet aucune erreur sur son split de test alors que le modèle Comprehend en fait 2 (et ce alors que le split de test du modèle Comprehend contient moins d’éléments). Il faudrait cependant répéter l’expérience de comparaison plusieurs fois en entraînant et testant les modèles sur **les mêmes splits** afin de pouvoir tirer de vraies conclusions scientifiques et non pas un simple début d’intuition.

Performances (coefficient 4)

Comprehend : 4/5 - CamemBERT : 5/5

## Comparaison n°5 : Coût
Pour ce cinquième et dernier critère de comparaison, il aura fallu faire des recherches assez approfondies. Il n’était effectivement pas simple au début du projet d’estimer le coût d’utilisation des modèles, surtout pour Comprehend dont la [tarification](https://aws.amazon.com/fr/comprehend/pricing/?ref=blog.ippon.fr) est assez complexe. Après m’être plongé dans la documentation de Comprehend et SageMaker j’ai pu obtenir ces estimations (voir figure 8).

![cout.png](/assets/img/clouds/tempo/cout.png)

Figure 8 : Caractéristiques du chatbot et coûts mensuels associés pour les 2 solutions

La différence de coût est frappante. Le fonctionnement du chatbot 24h/24 et 7j/7 avec le mode synchrone (prédiction en temps réel) de **Comprehend est extrêmement coûteux** (presque 1200€/mois). Le mode asynchrone de Comprehend (présenté dans le premier article de cette série) serait beaucoup plus abordable (environ 39€/mois pour les mêmes critères) mais il n’est pas compatible avec notre cas d’utilisation. L’avantage est donc cette fois pour la solution utilisant CamemBERT, bien plus abordable à environ 72€/mois.

Pour information, SageMaker possède depuis avril 2022 un [mode serverless](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html?ref=blog.ippon.fr) extrêmement prometteur et très avantageux en termes de coût (à peine plus de 2€/mois pour les mêmes critères selon mes estimations). Cette piste n’a cependant pas été explorée, car le mode serverless était encore en version de test au début stage lancement du projet. De plus, le temps de [cold start](https://docs.aws.amazon.com/sagemaker/latest/dg/serverless-endpoints.html?ref=blog.ippon.fr) aurait pu être problématique dans notre cas d’utilisation.

Coût (coefficient 4)

Comprehend : 1/5 - CamemBERT : 4/5

## Conclusion
Toutes ces comparaisons nous permettent donc de compléter la matrice de décision présentée au début de cet article, ce qui donne le résultat suivant (voir figure 9). L’addition de l’ensemble des notes pondérées donne la **solution basée sur CamemBERT gagnante de ce grand match**. Les résultats sont toutefois extrêmement serrés (68 vs 71) et la tendance pourrait tout à fait être différente selon vos propres critères, pondérations et compétences.

![matrice_complete.png](/assets/img/clouds/tempo/matrice_complete.png)

Figure 9 : Matrice de décision complétée

J’espère que cette série de 3 articles sur le sujet de la classification de texte vous aura plu et qu’elle vous aura permis d’en savoir plus sur la réalisation d’un projet nécessitant la création et l’industrialisation de ce type de modèle. Vous l’aurez compris, **Comprehend et CamemBERT possèdent tous deux des avantages et inconvénients** et le choix de l’une ou l’autre des solutions dépendra principalement de certains critères. Je vous conseille donc de **vous poser les questions suivantes** et de vous référer ensuite à la liste des avantages et inconvénients ci-dessous pour choisir la meilleure solution.

**Questions importantes auxquelles répondre :**

- Avez-vous besoin de prédiction en temps réel ? Si oui sur quelle période de temps ?
- Avez-vous des connaissances en Deep Learning ?
- Quel est votre budget mensuel pour l’utilisation du modèle ?

**Comprehend : Avantages**

- Service Comprehend de taille raisonnable ⇒ facile à prendre en main
- Pas de connaissances de ML nécessaires
- Bonnes performances
- Inférence rapide
- Industrialisation plutôt facile à réaliser

**Comprehend : Inconvénients**

- Coût en mode synchrone très élevé pour de longue période de disponibilité (~1300 $/mois)
- Effet boîte noire très important (input : dataset, output : modèle)
- Solution restreinte à l’écosystème AWS (pas possible de télécharger son modèle ou de l’héberger ailleurs que sur AWS)

**CamemBERT + SageMaker : Avantages**

- Bonnes performances
- Inférence rapide
- Leviers possibles sur l’apprentissage
- Coût abordable pour du temps réel sur de longues périodes
- Coût presque dérisoire en mode serverless
- SageMaker permet d’avoir la main sur l’hébergement du modèle (quelle instance utiliser, code de pré-traitement, …)
- Solution non restreinte à l’écosystème AWS (on peut remplacer SageMaker par d’autres solutions comme [MLflow](https://mlflow.org/?ref=blog.ippon.fr) pour ne jamais recourir à AWS)

**CamemBERT + SageMaker : Inconvénients**

- Transfer Learning à faire soi-même (librairie transformers d’Hugging Face)
- Industrialisation plus complexe
- SageMaker pas simple à prendre en main (gros service AWS)

source : https://blog.ippon.fr/