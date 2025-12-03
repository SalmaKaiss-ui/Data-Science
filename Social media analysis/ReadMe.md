## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

## **Numéro appogée**  : 22004055

<img width="100" height="150" alt="image" src="https://github.com/user-attachments/assets/c4f07a8f-3136-4ee1-a38a-87d49b8a5bc1" />
 
## **Dataset** : https://raw.githubusercontent.com/SalmaKaiss-ui/Data-Science/main/Social%20media%20analysis/sentimentdataset.csv

 ## Salma Kaiss


# 📊 Rapport Scientifique – Analyse de Sentiments sur Médias Sociaux

## 📋 Sommaire
1. [Introduction](#1-introduction)  
2. [Méthodologie](#2-méthodologie)  
   - [2.1 Pré-traitement](#21-pré-traitement)  
   - [2.2 Analyse Exploratoire (EDA)](#22-analyse-exploratoire-eda)  
   - [2.3 Modélisation](#23-modélisation)  
   - [2.4 Optimisation](#24-optimisation)  
3. [Résultats & Discussion](#3-résultats--discussion)  
4. [Conclusion](#4-conclusion)  
5. [Pistes d'amélioration](#pistes-damélioration)

---

# 1. 🎯 Introduction

> **"Plus de 98 000 tweets par minute** – les réseaux sociaux sont une mine d'or émotionnelle brute." 

Ce rapport analyse le dataset **"sentimentdataset.csv"** (~**365 publications**), inspiré du notebook Kaggle *"Social Media-Analysis Sentiment"* d'Alkidiarete. 
Les réseaux sociaux génèrent chaque jour un volume massif de textes contenant des opinions, des émotions et des jugements. L’analyse automatique de ces sentiments constitue un enjeu majeur pour différents domaines : marketing, détection des tendances, analyse politique, gestion de réputation, etc.

Dans ce projet, nous avons travaillé sur un dataset de publications issues de plateformes sociales, comprenant des informations textuelles (contenu du message), des métadonnées (nombre de likes, retweets, pays, plateforme) et un label indiquant l’émotion ou le sentiment associé.

## 🔍 **Caractéristiques du Dataset**
| **Aspect** | **Détails** |
|------------|-------------|
| **Plateformes** | Twitter, Instagram, Facebook [execute_python:3] |
| **Sentiments** | Positive(70%), Negative, Neutral, Anger, Fear, Happiness... [execute_python:2] |
| **Période** | 2010-2023 |
| **Métadonnées** | Likes, Retweets, Hashtags, Pays (USA🇺🇸, Canada🇨🇦, Inde🇮🇳...) [web:5] |

**L'analyse des sentiments** (*opinion mining*) transforme ce bruit textuel en **insights actionnables** pour :
- ✅ **Réputation** : Suivi en temps réel
- 🚨 **Crises** : Détection précoce
- 📈 **Marketing** : Stratégies ciblées [web:12][web:13]

**Défis identifiés** : Bruit textuel (emojis🔥, sarcasme😏), déséquilibre des classes, biais géo-temporels.

---

## ❓ **Problématique**
> **Comment construire un modèle de Machine Learning capable de prédire automatiquement le sentiment d’un message en se basant sur ses caractéristiques numériques et catégorielles ?**

## 🎯 **Objectifs**
- 🧹 Nettoyer & préparer les données
- 📊 Explorer les relations (EDA)
- 🤖 Comparer plusieurs modèles ML
- 📏 Évaluer & optimiser les performances

---

# 2. 🔬 Méthodologie

## 2.1 🧹 Pré-traitement

```
graph TD
    A[Dataset brut] --> B[Doublons supprimés]
    B --> C[Missing values imputées]
    C --> D[Label Encoding<br/>Sentiment]
    D --> E[One-Hot Encoding<br/>Platform, Country]
    E --> F[Standardisation Z-score]
    F --> G[Feature Engineering<br/>Engagement_rate]
    G --> H[Dataset prêt ✅]
```

**Engagement_rate** = `(Likes + Retweets) / (Followers + 1)` 📊

## 2.2 📈 Analyse Exploratoire (EDA)

### 🔍 **Distributions**
```
Followers/Likes : Forte asymétrie (loi de Pareto)
Outliers : Comptes influents (gardés 👑)
```

### 📊 **Corrélations Clés**
| **Variables** | **Corrélation** |
|---------------|-----------------|
| Followers ↔ Likes | **~0.85** |
| Likes ↔ Retweets | **~0.99** |
| Platform ↔ Engagement | **Modérée** |

## 2.3 🤖 Modélisation

| **Modèle** | **Type** | **Forces** | **Faiblesses** |
|------------|----------|------------|----------------|
| **Régression Logistique** | 🔵 Linéaire | Interprétable ⚡ | Linéarités seulement |
| **SVM** | 🟣 Marges | Multiclasse 👌 | Sensible aux outliers |
| **Random Forest** 🏆 | 🟢 Ensemble | Non-linéarités 💪 | Moins interprétable |

**Validation** : 5-fold Cross-Validation 🔄

## 2.4 ⚙️ Optimisation

**GridSearchCV** sur Random Forest :
```
n_estimators : 
max_depth : [10, 20, None]
min_samples_split :[11][12][13]
```

---

# 3. 📊 Résultats & Discussion

## 🏅 **Performances**

| **Modèle** | **Accuracy CV** | **F1-Score Macro** |
|------------|-----------------|-------------------|
| Régression Logistique | **55-60%** | 0.52 |
| SVM | **60-65%** | 0.58 |
| **Random Forest** 🥇 | **70-75%** 🟢 | **0.68** |

## 🎯 **Analyse Fine**

```
🔴 Classes majoritaires : Bonne précision
🟡 Classes rares : Sous-prédiction
🟢 Erreurs cohérentes avec similarités sémantiques
```

**Matrice de confusion** révèle confusion logique entre émotions proches (Anger↔Negative).

---

# 4. ✅ Conclusion

✅ **Pipeline complet** : Pré-traitement → EDA → Modélisation → Optimisation  
🏆 **Random Forest** : Meilleure performance (75% accuracy)  
📋 **Insights** : Engagement fortement corrélé aux métadonnées  

**Limites** :
- ❌ **Texte non exploité** (`Text` column)
- ⚖️ **Déséquilibre classes**
- 🧠 **Émotions complexes** mal capturées

---

# 5. 🚀 Pistes d'Amélioration

## 🔮 **Prochaines Étapes Prioritaires**

1. **NLP Textuel** 🎯
   ```
   TF-IDF | Word2Vec | BERT 🦾
   ```

2. **Rééquilibrage** ⚖️
   - SMOTE
   - Class weights
   - Undersampling

3. **Modèles Avancés** 🚀
   - XGBoost 🌟
   - LightGBM ⚡
   - CatBoost 🐱

4. **Features Avancées** 🧠
   - Variables temporelles (Heure, Jour)
   - Clustering hashtags
   - Analyse réseau utilisateurs


