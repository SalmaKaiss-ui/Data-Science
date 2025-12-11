## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

## **Numéro appogée**  : 22004055


 ## Salma Kaiss


<img width="100" height="150" alt="image" src="https://github.com/user-attachments/assets/c4f07a8f-3136-4ee1-a38a-87d49b8a5bc1" />
 
Voici l'intégralité du rapport en format Markdown (MD) :


# Rapport d'Analyse du Modèle CNN pour la Détection de Tumeurs Cérébrales

## Introduction
Le diagnostic précoce des tumeurs cérébrales à l’aide d’imagerie médicale, en particulier les images de résonance magnétique (IRM), joue un rôle essentiel dans la prise en charge des patients. Le but de ce projet est de développer un modèle de classification d’images basé sur un **Convolutional Neural Network (CNN)** pour identifier la présence ou l'absence de tumeurs dans les images IRM cérébrales. Ce rapport présente les étapes de la mise en œuvre du modèle, son entraînement, son évaluation, ainsi que les résultats obtenus.

## 1. Chargement et Préparation des Données

### 1.1 Description du Dataset
Le dataset utilisé dans ce projet provient de l'**IRM cérébrale** et est composé de **253 images** classées en deux catégories :
- **'No Tumor' (sans tumeur)** : 98 images (38.7%)
- **'Yes Tumor' (avec tumeur)** : 155 images (61.3%)

Le dataset est déséquilibré, avec une prépondérance d'images étiquetées **'Yes Tumor'**, ce qui peut avoir un impact sur la performance du modèle en termes de précision des deux classes.

### 1.2 Prétraitement des Données
Pour préparer les données pour l’entraînement, plusieurs transformations ont été appliquées aux images :
- **Redimensionnement :** Les images ont été redimensionnées à une taille uniforme de **128x128 pixels** pour assurer la cohérence des entrées du réseau.
- **Conversion en tenseur :** Les images ont été converties en tenseurs PyTorch à l'aide de `transforms.ToTensor()`.
- **Normalisation :** Les pixels des images ont été normalisés en utilisant une moyenne et un écart-type de [0.5, 0.5, 0.5], ce qui permet d'ajuster les valeurs des pixels entre -1 et 1, facilitant ainsi l'entraînement du modèle.

### 1.3 Division du Dataset
Le dataset a été divisé en trois ensembles :
- **Ensemble d’entraînement (70%) :** 177 images
- **Ensemble de validation (15%) :** 37 images
- **Ensemble de test (15%) :** 39 images

Cette répartition permet d’entraîner le modèle sur une majorité d'images tout en évaluant sa performance sur des données non vues pendant l’entraînement.

## 2. Définition du Modèle CNN (SimpleCNN)

Le modèle utilisé est un **CNN simple** adapté à la classification binaire (tumeur vs. pas de tumeur). Le modèle se compose des éléments suivants :

### 2.1 Architecture du Modèle
1. **Bloc Convolutionnel 1** : 
   - Conv2d avec 32 filtres de taille 3x3, suivi d’une fonction d’activation ReLU et d’un MaxPool2d.
   - Dropout de 25% pour éviter le surapprentissage.

2. **Bloc Convolutionnel 2** : 
   - Conv2d avec 64 filtres, suivi de ReLU, MaxPool2d, et Dropout de 25%.

3. **Bloc Convolutionnel 3** :
   - Conv2d avec 128 filtres, suivi de ReLU, MaxPool2d, et Dropout de 25%.

4. **Couches Fully Connected (FC)** :
   - La sortie de la dernière couche convolutionnelle est aplatie pour être passée à un réseau entièrement connecté.
   - La première couche FC a 512 unités, suivie d’un Dropout de 50% pour la régularisation.
   - La dernière couche FC a 2 unités, représentant les deux classes (No Tumor et Yes Tumor).

### 2.2 Visualisation du Modèle
L'architecture du modèle **SimpleCNN** peut être résumée comme suit :


SimpleCNN(
(conv1): Conv2d(3, 32, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(pool): MaxPool2d(kernel_size=2, stride=2, padding=0)
(dropout1): Dropout(p=0.25)
(conv2): Conv2d(32, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(dropout2): Dropout(p=0.25)
(conv3): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
(dropout3): Dropout(p=0.25)
(fc1): Linear(in_features=32768, out_features=512)
(dropout4): Dropout(p=0.5)
(fc2): Linear(in_features=512, out_features=2)
)


## 3. Entraînement du Modèle

### 3.1 Configuration de l’Entraînement
L’entraînement a été réalisé en utilisant :
- **Fonction de Perte :** `nn.CrossEntropyLoss` pour la classification binaire.
- **Optimiseur :** `optim.Adam` avec un taux d'apprentissage de **0.001**.
- **Dispositif :** CPU a été utilisé pour l'entraînement, ce qui peut être une contrainte en termes de vitesse. L'utilisation d'un GPU serait recommandée pour un entraînement plus rapide.

### 3.2 Historique de l’Entraînement
Le modèle a été entraîné pendant **10 époques**. Voici les résultats pour chaque époque :

| Époque | Perte Entraînement | Précision Entraînement | Perte Validation | Précision Validation |
| :----: | :----------------: | :--------------------: | :--------------: | :------------------: |
|   1    |       1.0830       |         54.24%         |      0.6608      |        64.86%        |
|   2    |       0.6690       |         58.19%         |      0.6431      |        64.86%        |
|   3    |       0.6250       |         64.97%         |      0.5920      |        78.38%        |
|   4    |       0.6546       |         67.80%         |      0.5989      |        67.57%        |
|   5    |       0.5552       |         74.01%         |      0.5315      |        72.97%        |
|   6    |       0.5640       |         64.97%         |      0.5371      |        81.08%        |
|   7    |       0.5407       |         76.84%         |      0.5090      |        81.08%        |
|   8    |       0.4887       |         74.58%         |      0.5480      |        72.97%        |
|   9    |       0.4886       |         77.97%         |      0.4886      |        81.08%        |
|   10   |       0.4332       |         81.92%         |      0.4674      |        83.78%        |

### 3.3 Analyse de l'Historique
L’entraînement a montré une amélioration progressive de la précision en entraînement et en validation. Cependant, on peut noter une légère fluctuation dans les performances de validation, ce qui peut suggérer un léger sur-apprentissage ou un manque de régularisation optimale.

## 4. Évaluation du Modèle sur l’Ensemble de Test

### 4.1 Rapport de Classification
Après 10 époques d'entraînement, le modèle a été évalué sur l'ensemble de test (39 images). Les résultats du rapport de classification sont les suivants :


### Résultats Validation Croisée (Test Set : 39 échantillons)

| **Classe**      | **Precision** | **Recall** | **F1-Score** | **Support** |
|-----------------|---------------|------------|--------------|-------------|
| No Tumor        | 0.60          | 0.75       | 0.67         | 12          |
| Yes Tumor       | **0.88**      | 0.78       | **0.82**     | 27          |
| **accuracy**    | **0.77**      |            |              | **39**      |
| macro avg       | 0.74          | 0.76       | 0.75         | 39          |
| weighted avg    | 0.79          | **0.77**   | 0.78         | 39          |

### Analyse des Performances 

**✅ Points forts cliniques :**
- **Precision Yes Tumor = 88%** : Parmi les cas signalés tumeur, 88% sont vrais positifs (faibles faux-positifs)
- **Recall No Tumor = 75%** : 75% des cas sains détectés correctement
- **F1-Score Tumor = 0.82** : Équilibre optimal precision/recall pour détection pathologique

**⚠️ Axes d'amélioration :**
- **Precision No Tumor = 60%** : 40% des cas sains signalés à tort comme tumoraux (sur-diagnostic)
- **Déséquilibre classes** : 27 tumors vs 12 no-tumor → Stratification + SMOTE recommandés
- **Accuracy globale = 77%** : Benchmark acceptable, optimisable via GridSearchCV (+5-8%)

**Interprétation médicale :**

Recall Tumor 78% = Détecte 78/100 tumeurs réelles
→ Acceptable pour screening, à valider multicentrique
F1-macro 0.75 = Performance équilibrée multiclasse
→ Niveau aide-décision IIa (ESCAT)

### 4.2 Matrice de Confusion

- **Vrais Positifs (VP) :** 21 (images avec tumeur correctement identifiées)
- **Vrais Négatifs (VN) :** 9 (images sans tumeur correctement identifiées)
- **Faux Positifs (FP) :** 3 (images sans tumeur identifiées à tort comme avec tumeur)
- **Faux Négatifs (FN) :** 6 (images avec tumeur identifiées à tort comme

ation de données plus sophistiquées, ou l'exploration d'architectures de modèles plus complexes.



