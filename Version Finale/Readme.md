## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

## **Numéro appogée**  : 22004055


 ## Salma Kaiss


<img width="100" height="150" alt="image" src="https://github.com/user-attachments/assets/c4f07a8f-3136-4ee1-a38a-87d49b8a5bc1" />




---

# RAPPORT TECHNIQUE : DÉTECTION DE TUMEURS CÉRÉBRALES PAR DEEP LEARNING

**Projet :** Classification d'images IRM cérébrales  
**Dataset :** Brain MRI Images for Brain Tumor Detection  
**Modèle :** VGG16 avec Transfer Learning  
**Date :** Janvier 2026

---

## SOMMAIRE

1. Introduction
2. Méthodologie
   - 2.1. Architecture du modèle
   - 2.2. Préparation des données
   - 2.3. Stratégie d'entraînement
3. Résultats et Analyse
   - 3.1. Interprétation de la matrice de confusion
   - 3.2. Analyse des courbes d'apprentissage
   - 3.3. Métriques de performance
4. Discussion et Limites
5. Conclusion
6. Recommandations

---

## 1. INTRODUCTION

### 1.1. Contexte médical

La détection précoce des tumeurs cérébrales est un enjeu de santé publique majeur. Les erreurs de diagnostic peuvent avoir des conséquences fatales : un faux négatif (tumeur non détectée) retarde le traitement et met en danger la vie du patient. L'imagerie par résonance magnétique (IRM) est l'examen de référence, mais l'interprétation des images reste complexe et sujette à la fatigue des radiologues.

### 1.2. Objectif du projet

Ce projet vise à développer un système d'intelligence artificielle pour assister les radiologues dans la classification binaire d'images IRM cérébrales :
- **Classe 0 (NO)** : Absence de tumeur
- **Classe 1 (YES)** : Présence de tumeur

L'IA doit prioriser la **sensibilité** (Recall) pour minimiser les faux négatifs, au détriment de la précision si nécessaire.

### 1.3. Approche technique

Nous utilisons un réseau de neurones convolutif pré-entraîné (VGG16) avec la technique du **Transfer Learning**. Cette approche permet d'exploiter les connaissances visuelles apprises sur ImageNet (14 millions d'images) et de les adapter à l'imagerie médicale, même avec un dataset limité (~250 images).

---

## 2. MÉTHODOLOGIE

### 2.1. Architecture du modèle

**VGG16** (Visual Geometry Group - 16 couches) est un réseau convolutif profond composé de :
- **13 couches convolutives** : Extraction de features visuelles (contours, textures, formes)
- **5 couches de pooling** : Réduction de dimensionnalité
- **3 couches fully connected** : Classification finale

**Adaptation pour la tâche binaire :**
- Gel complet des couches convolutives (extraction de features génériques)
- Remplacement de la dernière couche : 1000 classes → **2 classes** (tumeur / pas tumeur)
- Entraînement uniquement de la tête de classification (8,194 paramètres sur 138M)

### 2.2. Préparation des données

**Data Augmentation (Train Set) :**
- `RandomResizedCrop(224)` : Variation de position et zoom pour simuler différents cadrages IRM
- `RandomHorizontalFlip()` : Flip horizontal pour exploiter la symétrie cérébrale
- `RandomRotation(15°)` : Rotation pour simuler les variations d'angle du patient dans le scanner
- `ColorJitter(0.2, 0.2)` : Variation de luminosité/contraste pour simuler différents réglages de scanner
- `Normalize(mean=[0.485, 0.456, 0.406])` : Normalisation avec les statistiques ImageNet

**Validation Set :** Transformations déterministes uniquement (Resize → CenterCrop) pour garantir une évaluation reproductible.

**Split :** 80% train (205 images) / 20% validation (51 images)

### 2.3. Stratégie d'entraînement

- **Fonction de loss :** Cross-Entropy (pénalise exponentiellement les erreurs confiantes)
- **Optimiseur :** Adam (learning rate = 0.001)
- **Batch size :** 32 images
- **Époques :** 20 itérations complètes sur le train set
- **Checkpointing :** Sauvegarde automatique du modèle avec la meilleure validation accuracy

---

## 3. RÉSULTATS ET ANALYSE

### 3.1. Interprétation de la Matrice de Confusion

La matrice de confusion révèle la distribution des prédictions sur le validation set (51 images) :

```
                  Prédit NO    Prédit YES
Réel NO (sain)      10            4         → 14 patients sains
Réel YES (tumeur)    0           37         → 37 patients avec tumeur
```

**Analyse détaillée :**

| Métrique | Valeur | Calcul | Interprétation |
|----------|--------|--------|----------------|
| **True Negatives (TN)** | 10 | - | Patients sains correctement identifiés |
| **False Positives (FP)** | 4 | - | Fausses alarmes (patients sains diagnostiqués malades) |
| **False Negatives (FN)** | 0 | - | **AUCUNE tumeur ratée** ✅ |
| **True Positives (TP)** | 37 | - | Tumeurs correctement détectées |
| **Recall / Sensibilité** | **100%** | 37/(37+0) | Le modèle détecte TOUTES les tumeurs |
| **Precision** | 90.2% | 37/(37+4) | 90% des alarmes sont justifiées |
| **Accuracy globale** | 92.2% | (10+37)/51 | Taux de classification correct |
| **F1-Score** | 94.9% | 2×(0.902×1.0)/(0.902+1.0) | Équilibre Precision-Recall |

**🎯 Point critique médical :** Le modèle atteint **0 faux négatif** (Recall = 100%). Cela signifie qu'aucune tumeur n'a été ratée dans l'ensemble de validation. C'est l'objectif prioritaire en dépistage médical, car un faux négatif peut entraîner la mort du patient par retard de traitement.

**⚠️ Compromis acceptable :** Les 4 faux positifs (28.6% des patients sains) génèrent des examens complémentaires inutiles, mais ce coût est négligeable comparé au risque de rater une tumeur.

### 3.2. Analyse des Courbes d'Apprentissage

#### A. Courbe de Loss (Gauche)

**Observation :**
- **Train Loss (bleu)** : Décroissance régulière de 0.55 à 0.17 sur 20 époques
- **Validation Loss (orange)** : Décroissance de 0.39 à 0.25, avec oscillations modérées

**Interprétation :**
1. **Convergence saine** : Les deux courbes diminuent en parallèle, signe d'un apprentissage de patterns généralisables.
2. **Pas d'overfitting sévère** : La validation loss ne remonte pas de façon alarmante. L'écart train/val reste stable (~0.05-0.10), indiquant que le modèle généralise correctement.
3. **Oscillations en validation** : Normales avec un petit dataset (51 images). Chaque batch de validation (32 images) représente 63% du validation set, donc les fluctuations locales sont amplifiées.

**Point technique :** Le fait que la train loss finisse plus basse que la val loss (0.17 vs 0.25) est attendu. Le train set bénéficie de l'augmentation (le modèle s'adapte aux variations), tandis que le val set utilise des transformations fixes.

#### B. Courbe d'Accuracy (Droite)

**Observation :**
- **Validation Accuracy** : Oscille entre 86% et 92.3% avec une tendance générale stable autour de 89-90%
- **Pas de tendance à la hausse après l'époque 5** : L'accuracy a atteint un plateau

**Interprétation :**
1. **Convergence rapide** : Le modèle atteint 92% dès l'époque 1, grâce au Transfer Learning. Les poids VGG16 pré-entraînés fournissent une "base" déjà performante.
2. **Oscillations importantes (±6%)** : Dues à la petite taille du validation set. Avec seulement 51 images, le déplacement d'une seule prédiction incorrecte représente ~2% d'accuracy.
3. **Plateau à 90%** : Le modèle a atteint sa capacité maximale avec les données disponibles. Pour améliorer davantage, il faudrait :
   - Plus de données (dataset plus large)
   - Fine-tuning des couches convolutives
   - Ensembling de plusieurs architectures

**🔍 Diagnostic d'apprentissage :** Les courbes indiquent un apprentissage **sain mais limité par la taille du dataset**. Il n'y a pas de sur-apprentissage critique, mais le modèle ne peut pas progresser au-delà de 92% avec les 205 images d'entraînement disponibles.

### 3.3. Comparaison avec les Standards Médicaux

**Performances humaines (radiologues expérimentés) :**
- Sensibilité : 85-95% (selon la littérature)
- Spécificité : 90-98%

**Performances du modèle :**
- Sensibilité : **100%** (supérieure à la moyenne humaine)
- Spécificité : 71.4% (inférieure, à cause des 4 FP)

**Conclusion :** Le modèle excelle dans la détection (aucune tumeur ratée) mais génère plus de fausses alarmes qu'un expert humain. Il est donc adapté comme **outil de pré-criblage** (première ligne de dépistage), où les cas positifs seraient ensuite vérifiés par un radiologue.

---

## 4. DISCUSSION ET LIMITES

### 4.1. Forces du modèle

1. **Sensibilité parfaite (100%)** : Aucune tumeur ratée sur le validation set. C'est l'objectif médical prioritaire.
2. **Convergence rapide** : Grâce au Transfer Learning, le modèle atteint 92% dès les premières époques, sans nécessiter des semaines d'entraînement.
3. **Robustesse à la variabilité** : L'augmentation des données permet au modèle de gérer différents cadrages, orientations et contrastes d'IRM.

### 4.2. Limites identifiées

1. **Taille du dataset (256 images)** : 
   - Le validation set (51 images) est trop petit pour une évaluation statistiquement robuste.
   - Les oscillations d'accuracy (±6%) sont dues à cette limite.
   - **Recommandation :** Valider sur un dataset externe de 1000+ images avant tout déploiement clinique.

2. **Déséquilibre des classes (73% tumeur / 27% sain)** :
   - Le modèle est biaisé vers la prédiction "tumeur".
   - Cela explique les 4 faux positifs (sur-détection).
   - **Solution :** Appliquer un class weighting (pénaliser davantage les erreurs sur la classe minoritaire).

3. **Spécificité sous-optimale (71.4%)** :
   - 28.6% des patients sains reçoivent une fausse alarme.
   - Impact : Coûts d'examens complémentaires, stress psychologique.
   - **Compromis accepté** : En dépistage, on préfère sur-détecter que sous-détecter.

4. **Absence d'explicabilité** :
   - Le modèle est une "boîte noire" de 138M de paramètres.
   - Impossible de savoir sur quelle région de l'IRM il se base pour décider.
   - **Solution :** Intégrer Grad-CAM pour générer des heatmaps d'attention.

5. **Validation mono-centre** :
   - Toutes les images proviennent du même dataset Kaggle (probablement un seul hôpital).
   - Risque de biais de scanner (protocole d'acquisition, réglages spécifiques).
   - **Recommandation :** Tester sur des IRM multi-centres, multi-scanners (GE, Siemens, Philips).

---

## 5. CONCLUSION

Ce projet démontre que le **Transfer Learning** avec VGG16 est une approche viable pour la classification d'images IRM cérébrales, même avec un dataset limité (256 images). Le modèle atteint une sensibilité de 100% (aucune tumeur ratée) et une accuracy globale de 92.2% sur le validation set.

**Résultats clés :**
- ✅ **0 faux négatif** : Objectif médical critique atteint
- ✅ **Convergence rapide** : Performances élevées dès l'époque 1
- ✅ **Pas d'overfitting sévère** : Le modèle généralise correctement
- ⚠️ **4 faux positifs** : Taux de fausses alarmes de 28.6% (acceptable en dépistage)

**Statut du modèle :** L'IA actuelle est un **prototype de recherche** prometteur, mais nécessite une validation clinique rigoureuse avant tout déploiement en conditions réelles. Les performances sur un petit validation set (51 images) ne garantissent pas une robustesse à grande échelle.

---

## 6. RECOMMANDATIONS

### 6.1. Améliorations techniques immédiates

1. **Validation externe** : Tester sur un dataset indépendant de 1000+ images pour confirmer les performances.
2. **Fine-tuning** : Dégeler progressivement les dernières couches convolutives de VGG16 pour améliorer la spécificité.
3. **Class weighting** : Appliquer un poids 3× à la classe "no tumor" pour réduire les faux positifs sans sacrifier le Recall.
4. **Grad-CAM** : Implémenter des heatmaps d'attention pour expliquer les décisions du modèle aux radiologues.

### 6.2. Développements avancés

1. **Ensembling** : Entraîner 5 architectures différentes (VGG16, ResNet50, EfficientNet, DenseNet, InceptionV3) et faire voter les modèles.
2. **Segmentation** : Passer d'une classification binaire à une segmentation pixel-par-pixel de la tumeur (localisation précise).
3. **Multi-classes** : Étendre le modèle pour classifier les types de tumeurs (gliome, méningiome, adénome hypophysaire).

### 6.3. Parcours vers la production

1. **Phase 1** : Validation clinique prospective sur 5000+ IRM annotées par 3 radiologues certifiés.
2. **Phase 2** : Étude multi-centres (5 hôpitaux, 3 continents) pour tester la robustesse inter-scanner.
3. **Phase 3** : Certification médicale (FDA, CE) comme dispositif d'aide à la décision de classe IIa.
4. **Phase 4** : Intégration dans les systèmes PACS hospitaliers avec interface utilisateur dédiée.
5. **Phase 5** : Monitoring continu des performances en conditions réelles (détection de model drift).

---

**🎯 Conclusion finale :** Ce projet illustre la puissance du Deep Learning appliqué à l'imagerie médicale. Avec une méthodologie rigoureuse (Transfer Learning, Data Augmentation, évaluation sur métriques médicales), on peut créer des outils d'assistance au diagnostic performants. Cependant, le chemin vers un déploiement clinique sûr nécessite une validation à grande échelle et une collaboration étroite entre data scientists et médecins.

---


