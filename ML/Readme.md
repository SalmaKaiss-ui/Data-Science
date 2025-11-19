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

Le code utilise fetch_ucirepo pour télécharger automatiquement le dataset Wine Quality depuis l’UCI Repository.
Il sépare ensuite les données en deux parties : X pour les variables explicatives et y pour la variable cible.
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


Le code importe le modèle KNeighborsClassifier, un algorithme de classification basé sur les plus proches voisins.
La commande ?KNeighborsClassifier permet d’afficher sa documentation pour comprendre ses paramètres et son utilisation.


```python
from sklearn.neighbors import KNeighborsClassifier
# to get the online help, type:
?KNeighborsClassifier  
```

1 Data analysis

Load the data and show its summary:

Le code commence par convertir les données originales du dataset Wine Quality en un DataFrame complet, incluant la colonne indiquant la couleur du vin.
Il filtre ensuite uniquement les vins blancs et retire la colonne color, puis affiche un résumé du dataset ainsi que les premières lignes pour visualiser la structure des données.


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






