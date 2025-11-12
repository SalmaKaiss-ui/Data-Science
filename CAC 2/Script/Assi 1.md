# COURS DE SCIENCE DES DONNÉES
## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

<img src="Ma_photo.jpg" style="height:464px;margin-right432px"/>
 
 
 ## Salma Kaiss

 
 ## Nom du jeux: Bank Marketing

## Descriptif:

L'étude sur le dataset "Bank Marketing" a été réalisée par S. Moro, P. Cortez, et P. Rita, et publiée dans Decision Support Systems en 2014. Ce dataset porte sur les campagnes de marketing direct par téléphone menées par une banque portugaise. L'objectif principal de la classification est de prédire si un client souscrira un dépôt à terme ("yes" ou "no").

Population et Données

La population étudiée est composée de clients de la banque contactés lors de campagnes téléphoniques.

Le dataset complet contient 41 188 exemples collectés entre mai 2008 et novembre 2010.

Chaque exemple contient jusqu'à 20 variables, incluant des données démographiques (âge, profession, état civil, éducation), des informations financières (solde du compte, prêts en cours), et des détails liés à la campagne (nombre de contacts, durée des appels, résultat des campagnes précédentes).

Le dataset comprend aussi des indicateurs socio-économiques comme le taux d'emploi, indice des prix à la consommation, indice de confiance des consommateurs, taux Euribor à 3 mois, et nombre d'employés.

Description succincte

Il s'agit d'une étude de classification multivariée visant à déterminer la probabilité qu'un client souscrive un produit bancaire après une campagne d'appels téléphoniques. Les campagnes peuvent nécessiter plusieurs appels pour convaincre un client. Le jeu de données est utilisé pour analyser et prédire le succès des campagnes de marketing direct bancaire.

Cette base de données est très utilisée pour entraîner et tester des modèles de machine learning dans le domaine bancaire et marketing, notamment pour améliorer les stratégies de campagne en ciblant mieux les clients potentiels.



**Codes Python: Installation du package**

```python
pip install ucimlrepo
```

La commande pip install ucimlrepo installe la bibliothèque Python ucimlrepo, qui permet d’accéder facilement aux jeux de données du UCI Machine Learning Repository.


**Codes Python: Importation de  dataset au code**

```python
from ucimlrepo import fetch_ucirepo

# fetch dataset
bank_marketing = fetch_ucirepo(id=222)

# data (as pandas dataframes)
X = bank_marketing.data.features
y = bank_marketing.data.targets

# metadata
print(bank_marketing.metadata)

# variable information
print(bank_marketing.variables)
```
Ce code importe la fonction fetch_ucirepo pour télécharger le jeu de données Bank Marketing depuis la bibliothèque UCI Machine Learning Repository (identifiant 222).
Les variables X et y contiennent respectivement les caractéristiques (features) et les cibles (targets) du jeu de données, tandis que les deux dernières lignes affichent les métadonnées et les informations sur les variables du dataset.


**Codes Python: Affichage des statistiques descriptives**

```python
import pandas as pd
import pandas as pd
display(X.describe(include='all'))
display(y.describe(include='all'))
```

Ce code importe la bibliothèque pandas (deux fois, mais une seule suffit) et utilise la fonction describe() pour afficher des statistiques descriptives sur les données.
La fonction display() montre un résumé complet (avec include='all') des caractéristiques X et de la variable cible y, incluant les valeurs manquantes, les moyennes, les fréquences, etc.

**Codes Python: Visualisation des statistiques descriptives pour X**

```python
import matplotlib.pyplot as plt
import seaborn as sns

# Identify numerical and categorical columns in X
numerical_cols = X.select_dtypes(include=['int64', 'float64']).columns
categorical_cols = X.select_dtypes(include=['object', 'category']).columns

print("Visualizing Numerical Features in X:")
for col in numerical_cols:
    plt.figure(figsize=(8, 5))
    sns.histplot(X[col].dropna(), kde=True)
    plt.title(f'Distribution of {col}')
    plt.xlabel(col)
    plt.ylabel('Frequency')
    plt.show()
```
Ce code utilise **Matplotlib** et **Seaborn** pour visualiser la distribution des variables **numériques** du jeu de données `X`.
Il identifie d’abord les colonnes numériques, puis pour chacune, il affiche un **histogramme avec courbe de densité (kde)** afin d’observer la répartition des valeurs et repérer d’éventuelles tendances ou anomalies.


**Codes Python: Visualisation des statistiques descriptives pour Y**

```python
import matplotlib.pyplot as plt
import seaborn as sns

print("Visualizing Target Variable y:")
plt.figure(figsize=(6, 4))
sns.countplot(data=y, x='y', palette='viridis')
plt.title('Counts of Target Variable (y)')
plt.xlabel('Subscription to Term Deposit')
plt.ylabel('Count')
plt.show()
```

Ce code visualise la **variable cible** `y` à l’aide d’un **diagramme en barres** pour montrer la fréquence de chaque catégorie (par exemple, “oui” ou “non” à la souscription d’un dépôt à terme).
Il utilise **Seaborn** pour tracer le graphique (`countplot`) et **Matplotlib** pour personnaliser le titre et les axes.













