# COURS DE SCIENCE DES DONNÉES
## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année
 
 
 ## Salma Kaiss

 
 ## Nom du jeux: Wine Quality

## Descriptif:

Le dataset "Wine Quality" de l’UCI Machine Learning Repository contient des informations sur des échantillons de vins rouges et blancs de la région Vinho Verde au Portugal. Il se compose de deux jeux de données distincts, totalisant 4898 instances pour le vin blanc et 1599 pour le vin rouge. Chaque échantillon est décrit par 11 variables physico-chimiques continues, telles que l’acidité fixe, l’acidité volatile, l’acide citrique, le sucre résiduel, les chlorures, les niveaux de dioxyde de soufre (libre et total), la densité, le pH, les sulfates et la teneur en alcool. La qualité du vin, notée sur une échelle de 0 à 10, constitue la variable cible obtenue à partir de données sensorielles provenant d’experts. 

Ce jeu de données est souvent utilisé pour créer des modèles de classification ou de régression afin de prédire la qualité des vins à partir de leurs caractéristiques chimiques. Les classes de qualité sont ordonnées mais déséquilibrées, ce qui invite à des approches adaptées notamment en gestion des déséquilibres et en sélection de caractéristiques. Ce dataset est une référence pour l’analyse des données liées à la viticulture et la modélisation prédictive en œnologie.


**Codes Python: Installation du package**

```python
pip install ucimlrepo     
```

# 📝 Interprétation 

Le code utilise `fetch_ucirepo` pour télécharger automatiquement le dataset *Wine Quality* depuis l’UCI Repository.
Il sépare ensuite les données en deux parties : `X` pour les variables explicatives et `y` pour la variable cible.
Enfin, il affiche les métadonnées du dataset ainsi que les informations détaillées sur chaque variable afin de mieux comprendre sa structure.



```python
from ucimlrepo import fetch_ucirepo 
  
# fetch dataset 
wine_quality = fetch_ucirepo(id=186) 
  
# data (as pandas dataframes) 
X = wine_quality.data.features 
y = wine_quality.data.targets 
  
# metadata 
print(wine_quality.metadata) 
  
# variable information 
print(wine_quality.variables)    
```

# 📝 Interprétation 

Le code importe le modèle `KNeighborsClassifier`, un algorithme de classification basé sur les plus proches voisins.
La commande `?KNeighborsClassifier` permet d’afficher sa documentation pour comprendre ses paramètres et son utilisation.



```python
from sklearn.neighbors import KNeighborsClassifier
# to get the online help, type:
?KNeighborsClassifier  
```

# 1 Data analysis

## Load the data and show its summary:x

# 📝 Interprétation 

Le code commence par convertir les données originales du dataset *Wine Quality* en un DataFrame complet, incluant la colonne indiquant la couleur du vin.
Il filtre ensuite uniquement les vins blancs et retire la colonne `color`, puis affiche un résumé du dataset ainsi que les premières lignes pour visualiser la structure des données.



```python
import pandas as pd
import numpy as np

# The dataset has already been fetched using ucimlrepo in cell _5Nt2FgKoPiy
# wine_quality = fetch_ucirepo(id=186)

# Create a full DataFrame from the original data, which includes the 'color' column
df = wine_quality.data.original.copy()

# Filter for white wine data, as the original link was for winequality-white.csv
df = df[df['color'] == 'white'].drop(columns=['color'])

print("\n========= Dataset summary ========= \n")
df.info()
print("\n========= A few first samples ========= \n")
print(df.head())
```


##  Form the arrays X ∈ R^N*d of the input variables and Y∈ R^N the output. What are the wine qualities and the related number of samples ?

# 📝 Interprétation 

Le code sépare les données en plaçant toutes les variables explicatives dans `X` en retirant la colonne `quality`, et en mettant la variable cible `quality` dans `Y`.
Il affiche ensuite la distribution des différentes notes de qualité du vin pour montrer combien d’échantillons appartiennent à chaque catégorie.



```python
 X = df.drop("quality", axis=1) #we drop the column "quality"
 Y = df["quality"]
 print("\n========= Wine Qualities ========= \n")
 print(Y.value_counts())
```

##  To form a binary classification problem, we group the data by quality level.

# 📝 Interprétation 

Ce code transforme la variable `Y` en une classification binaire où les vins ayant une qualité inférieure ou égale à 5 sont étiquetés comme **mauvais** (`0`), tandis que les autres sont considérés comme **bons** (`1`).
Il crée ainsi une nouvelle version simplifiée de la cible pour préparer un modèle de classification binaire.


```python
 # bad wine (y=0) : quality <= 5 and good quality (y= 1) otherwise
 Y = [0 if val <=5 else 1 for val in Y]
```


##   Perform a statistical analysis (mean, variance, correlation ...) of the input variables.
 Comments on the results.


# 📝 Interprétation 

Le code commence par tracer un **boxplot** de toutes les variables explicatives afin de visualiser leur distribution et repérer les valeurs extrêmes, en orientant les boîtes verticalement et en tournant les étiquettes pour faciliter la lecture.
Ensuite, il calcule la **matrice de corrélation** des variables et affiche un **heatmap**, ce qui permet d’identifier quelles caractéristiques sont fortement corrélées entre elles.


```python
  import matplotlib.pyplot as plt
 import seaborn as sns
 plt.figure()
 ax = plt.gca()
 sns.boxplot(data=X,orient="v",palette="Set1",width=1.5, notch=True)
 ax.set_xticklabels(ax.get_xticklabels(),rotation=90)
 plt.figure()
 corr = X.corr()
 sns.heatmap(corr)
```


<img width="552" height="534" alt="image" src="https://github.com/user-attachments/assets/f320b7a4-788e-4df0-85aa-06b5533c3ed7" />


# 📝 Analyse du boxplot

Ce boxplot montre la distribution des différentes caractéristiques chimiques du vin blanc. On observe que **la majorité des variables ont des valeurs relativement faibles** et sont regroupées près de zéro, tandis que certaines variables se distinguent par des valeurs beaucoup plus élevées.

* **free_sulfur_dioxide** et **total_sulfur_dioxide** présentent des valeurs nettement plus grandes que les autres, avec de nombreux **outliers**, ce qui indique une forte variabilité et des mesures extrêmes dans ces deux caractéristiques.
* Des variables comme **residual_sugar**, **chlorides** et **volatile_acidity** montrent également quelques valeurs extrêmes mais dans des proportions plus modérées.
* Les variables telles que **pH**, **density**, **alcohol**, **citric_acid**, et **fixed_acidity** ont des distributions plus compactes et moins d’écarts inhabituels.



<img width="648" height="540" alt="image" src="https://github.com/user-attachments/assets/1d9c0c5b-e916-43cf-9d76-ca61cfb80f9f" />


# 📝 Analyse du heatmap de corrélation

Ce heatmap montre les corrélations entre toutes les variables chimiques du vin blanc. Les couleurs plus claires indiquent une **corrélation positive forte**, tandis que les couleurs foncées indiquent une **corrélation négative** ou faible.

### 🔍 **Points importants :**

* **free_sulfur_dioxide** et **total_sulfur_dioxide** sont fortement corrélés (zone très claire), ce qui indique que lorsque l’un augmente, l’autre augmente souvent aussi.
* **density** est positivement corrélée avec **residual_sugar** et **chlorides**, ce qui est logique car plus il y a de sucre ou de sel, plus le vin devient dense.
* **alcohol** est négativement corrélé avec **density**, ce qui signifie que les vins plus alcoolisés sont généralement moins denses.
* La plupart des autres corrélations sont **faibles** (dominance de couleurs intermédiaires), suggérant que beaucoup de variables varient indépendamment les unes des autres.
* **pH** montre des relations très faibles avec la majorité des autres variables.



# 2 Classification

## Data Split


# 📝 Interprétation 

Le code utilise `train_test_split` pour diviser les données en trois ensembles : un **ensemble d’apprentissage**, un **ensemble de validation**, et un **ensemble de test**, tout en conservant la proportion des classes grâce au paramètre `stratify`.
D’abord, il sépare un tiers des données pour le test, puis il divise le reste en deux parts égales afin de créer les ensembles d’apprentissage et de validation.


```python
 from sklearn.model_selection import train_test_split
 Xa, Xt, Ya, Yt = train_test_split(X, Y, shuffle=True, test_size=1/3,
 stratify=Y)
 Xa, Xv, Ya, Yv = train_test_split(Xa, Ya, shuffle=True, test_size=0.5,
 stratify=Ya)
```

## k nearest neighbor (k-NN) classification

# 📝 Interprétation 

Le code commence par entraîner un modèle **KNN** avec `k = 3` sur les données d’apprentissage, puis il prédit les étiquettes sur l’ensemble de validation et calcule le taux d’erreur.
Ensuite, une boucle teste plusieurs valeurs de `k` (1, 3, 5, …, 35), en évaluant pour chacune l’erreur d’apprentissage et l’erreur de validation, afin d’identifier la valeur de `k` qui minimise l’erreur de validation et constitue ainsi le meilleur choix pour le modèle.



```python
 from sklearn.neighbors import KNeighborsClassifier
 # Fit the model on (Xa, Ya)
 k = 3
 clf = KNeighborsClassifier(n_neighbors = k)
 clf.fit(Xa, Ya)      
    # Predict the labels of samples in Xv
 Ypred_v = clf.predict(Xv)
 # evaluate classification error rate
 from sklearn.metrics import accuracy_score
 error_v = 1-accuracy_score(Yv, Ypred_v)
    # some hints
 k_vector = np.arange(1, 37, 2) #define a vector of k=1, 3, 5, ...
 error_train = np.empty(k_vector.shape)
 error_val = np.empty(k_vector.shape)
 for ind, k in enumerate(k_vector):
 #fit with k
 clf = KNeighborsClassifier(n_neighbors = k)
 clf.fit(Xa, Ya)
 # predict and evaluate on training and validation sets
 Ypred_train = ...
 error_train[ind] = ...
 ...
# some hints: get the min error and related k-value
 err_min, ind_opt = error_val.min(), error_val.argmin()
 k_star = k_vector[ind_opt]
```

## Normalize or not normalize the data ?

# 📝 Interprétation 

Le code utilise `StandardScaler` pour **normaliser** les données d’apprentissage en leur retirant la moyenne et en les divisant par leur écart-type, ce qui donne à chaque variable la même échelle.
Après avoir ajusté le scaler sur `Xa`, il applique cette transformation à `Xa` et `Xv`, afin que les données normalisées soient cohérentes et prêtes pour l’entraînement d’un modèle sensible aux distances comme le KNN.


```python
 from sklearn.preprocessing import StandardScaler
 sc = StandardScaler(with_mean=True, with_std=True)
 sc = sc.fit(Xa)
 Xa_n = sc.transform(Xa)
 Xv_n = sc.transform(Xv)
```








 

