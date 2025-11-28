## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

<img src="Ma_photo.jpg" style="height:464px;margin-right432px"/>
 
 
 ## Salma Kaiss

 
 ## Nom du jeux: Analyse timeseries old school

# 📊 Analyse des Séries Temporelles : Modélisation SARIMA et Moyenne Mobile

## 📑 Sommaire
1. [📚 Introduction](#introduction)
2. [🎯 Objectif de l'étude](#objectif-de-letude)
3. [🛠️ Méthodologie](#methodologie)
   - [📊 Préparation des Données](#preparation-des-donnees)
   - [🧮 Modélisation](#modelisation)
     - [🔮 SARIMA](#sarima)
     - [📉 Moyenne Mobile](#moyenne-mobile)
4. [🔍 Analyse Exploratoire](#analyse-exploratoire)
   - [📈 Visualisation des Données](#visualisation-des-donnees)
   - [📊 Analyse Statistique](#analyse-statistique)
5. [🧪 Modélisation et Résultats](#modelisation-et-resultats)
   - [🔮 Modèle SARIMA](#modele-sarima)
   - [📉 Modèle Moyenne Mobile](#modele-moyenne-mobile)
6. [🔚 Conclusion et Perspectives](#conclusion-et-perspectives)
7. [📋 Statut des Étapes d'Analyse](#statut-des-etapes-danalyse)

---

## 📚 Introduction

L'analyse des séries temporelles est essentielle pour modéliser et prédire des phénomènes économiques, financiers ou environnementaux. Dans cette étude, nous appliquons les modèles **SARIMA** (Seasonal ARIMA) et **Moyenne Mobile** pour analyser une série temporelle, en vue de prévoir des valeurs futures. 

### 🎯 Objectif
L'objectif de cette étude est de :

1. **Modéliser et prédire** les valeurs futures d'une série temporelle. 🔮
2. **Comparer les performances** de deux modèles populaires : **SARIMA** et **Moyenne Mobile**. ⚖️
3. **Évaluer la précision** des modèles à travers des indicateurs comme le RMSE et le MAE. 📊

### 📋 Statut des Étapes d'Analyse
- **Chargement des données** : ✅ Terminé
- **Prétraitement des données** : ✅ Terminé
- **Identification des objectifs** : ✅ Terminé

---

## 🎯 Objectif de l'étude

Le but principal de cette analyse est de comparer l'efficacité de deux modèles pour prédire une série temporelle :

- **SARIMA (Seasonal ARIMA)** : Ce modèle est conçu pour capturer les effets saisonniers et les tendances dans les séries temporelles. 🌦️
- **Moyenne Mobile** : Modèle plus simple, il lisse la série pour en faire des prévisions en fonction de moyennes passées. 📉

Nous évaluerons les résultats obtenus par chaque modèle en utilisant des critères comme l'erreur quadratique moyenne (RMSE) et l'erreur absolue moyenne (MAE).

### 📋 Statut des Étapes d'Analyse
- **Identification des objectifs de l'analyse** : ✅ Terminé
- **Choix des modèles à comparer** : ✅ Terminé
- **Définition des métriques de performance** : ✅ Terminé

---

## 🛠️ Méthodologie

### 📊 Préparation des Données

- **Chargement des données** : Les données sont importées à partir d'un fichier CSV ou d'une base de données. 📥
- **Nettoyage des données** : Les valeurs manquantes sont traitées et les données sont ajustées pour être prêtes à l'analyse. 🧹
- **Vérification de la stationnarité** : Un test de stationnarité (Dickey-Fuller) est effectué pour déterminer si une différenciation est nécessaire. 🔍

### 📋 Statut des Étapes d'Analyse
- **Chargement des données** : ✅ Terminé
- **Nettoyage des données** : ✅ Terminé
- **Test de stationnarité** : ✅ Terminé
- **Vérification des valeurs manquantes** : ✅ Terminé

---

### 🧮 Modélisation

#### 🔮 SARIMA

Le modèle **SARIMA** est utilisé pour gérer les séries temporelles saisonnières. Le processus de modélisation comprend les étapes suivantes :

1. **Identification des paramètres** : On choisit les valeurs optimales pour les paramètres `(p, d, q)` et saisonniers `(P, D, Q)`. 🛠️
2. **Entraînement du modèle** : Le modèle est formé à partir des données historiques. 📚
3. **Prédictions** : Le modèle prédit les valeurs futures de la série temporelle. 🔮

#### 📉 Moyenne Mobile

Le modèle de **Moyenne Mobile** est une technique simple de lissage. Il suit les étapes suivantes :

1. **Choix de la fenêtre** : On définit la taille de la fenêtre pour le lissage des valeurs. 🪟
2. **Calcul de la moyenne** : Une moyenne est calculée pour chaque point de la série temporelle sur la fenêtre définie. ➗
3. **Prédictions** : La moyenne mobile permet de prévoir les valeurs futures. 🔮

### 📋 Statut des Étapes d'Analyse
- **Choix des paramètres du modèle SARIMA** : ✅ Terminé
- **Entraînement du modèle SARIMA** : ✅ Terminé
- **Calcul de la moyenne mobile** : ✅ Terminé
- **Prédiction des valeurs futures avec SARIMA et Moyenne Mobile** : ✅ Terminé

---

## 🔍 Analyse Exploratoire

### 📈 Visualisation des Données

Avant de modéliser, il est crucial de visualiser la série temporelle pour observer les tendances, la saisonnalité et les anomalies. Cette étape aide à mieux comprendre le comportement des données.

- **Graphique de la série temporelle** : Représentation de la série avec ses tendances et saisons. 📊
- **Décomposition de la série** : Séparation en trois composants : tendance, saisonnalité, et résidus. 🔍

### 📊 Analyse Statistique

- **Test de Stationnarité** : Le test de Dickey-Fuller est utilisé pour vérifier si la série est stationnaire. 📉
- **ACF/PACF** : L'autocorrélation (ACF) et l'autocorrélation partielle (PACF) aident à identifier les paramètres nécessaires pour le modèle SARIMA. 🧠

### 📋 Statut des Étapes d'Analyse
- **Visualisation de la série temporelle** : ✅ Terminé
- **Décomposition de la série** : ✅ Terminé
- **Test de stationnarité (Dickey-Fuller)** : ✅ Terminé
- **ACF/PACF pour identification des paramètres SARIMA** : ✅ Terminé

---

## 🧪 Modélisation et Résultats

### 🔮 Modèle SARIMA

Le modèle SARIMA a été appliqué après avoir trouvé les bons paramètres pour les ordres saisonniers et non saisonniers. Les résultats incluent :

- **Graphique des prévisions SARIMA** : Visualisation des prévisions par rapport aux valeurs réelles. 📊
- **Mesures d'erreur** : RMSE et MAE pour évaluer la performance du modèle. 📉

### 📉 Modèle Moyenne Mobile

Le modèle de Moyenne Mobile est plus simple à mettre en œuvre et donne une approximation des tendances futures :

- **Graphique des prévisions Moyenne Mobile** : Comparaison des prévisions avec celles de SARIMA. 🔄
- **Mesures d'erreur** : Évaluation de la précision de la moyenne mobile avec des critères comme RMSE et MAE. 🧮

### 📋 Statut des Étapes d'Analyse
- **Prévisions SARIMA réalisées** : ✅ Terminé
- **Comparaison des modèles SARIMA et Moyenne Mobile** : ✅ Terminé
- **Évaluation des performances des modèles (RMSE, MAE)** : ✅ Terminé

---

## 🔚 Conclusion et Perspectives

### 📝 Conclusion

Les résultats montrent que le modèle **SARIMA** surpasse la **Moyenne Mobile** en termes de précision, particulièrement dans les séries temporelles présentant des saisons ou des tendances fortes. Les erreurs de prévision sont plus faibles avec SARIMA. 📉

### 🚀 Perspectives

1. **Amélioration des modèles** : Tester des variantes de modèles, comme ARIMA ou même des modèles non linéaires. 🔬
2. **Exploration de techniques avancées** : Intégrer des modèles comme les **Réseaux de Neurones** ou les **LSTM** pour les séries temporelles complexes. 🤖
3. **Validation croisée** : Utiliser une validation croisée pour tester la robustesse des modèles dans divers scénarios. 🔄

### 📋 Statut des Étapes d'Analyse
- **Rédaction des conclusions** : ✅ Terminé
- **Propositions de perspectives futures** : ✅ Terminé

---

## 📋 Principales Découvertes

- Le modèle **SARIMA** offre des prévisions plus précises, en particulier lorsqu'il y a des effets saisonniers dans la série temporelle. 🔮
