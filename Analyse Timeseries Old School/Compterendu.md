## École Nationale de Commerce et de Gestion (ENCG) - 4ème Année

<img src="Ma_photo.jpg" style="height:464px;margin-right432px"/>
 
 
 ## Salma Kaiss

 
 ## Nom du jeux: Bank Marketing

## Descriptif:
Voici un résumé — en format **Markdown** — de ce que l’on sait (et ce qu’on ignore) sur l’analyse menée dans le notebook **Timeseries Old School - SARIMA, Moving Average..** de michaelfumery (sur Kaggle). 

---

## 📄 Détail de l’enquête / du notebook

### - Sujet

Le notebook applique des méthodes de **séries temporelles** — en particulier **moyenne mobile (moving average)** et **modèle saisonnier ARIMA (SARIMA)** — afin de prévoir la **consommation**. 
Le terme « consommation » n’est **pas explicitement détaillé** dans le titre comme « énergie », « électricité », « eau », etc. Cependant le notebook évoque « l’ajustement des effets de température sur la consommation en énergie via régression linéaire simple ». 
→ Conclusion : il s’agit très vraisemblablement d’une **consommation d’énergie**.

### - Pays / origine / contexte

* Le notebook ne mentionne **pas de pays spécifique** dans le titre ou la description publique. Je n’ai pas trouvé d’indication claire d’un pays de provenance des données.
* Aucune référence explicite à un secteur géographique ou à un fournisseur national d’énergie.
  → On peut donc dire que **le pays est indéterminé** dans le notebook.

### - Données / Catégorie / Type de données

* Il s’agit d’une **série temporelle univariée** (probablement, consommation d’énergie en fonction du temps). 
* Le dataset utilisé dans le notebook est nommé **dataset_desaisonnalisation_sarima_predict**. 
* Les données contiennent — d’après les logs — une colonne « consommation totale » (type `int64`) et une colonne date/temps.
* Le nombre d’observations est de 1221 (non-null), ce qui suggère des données couvrant plusieurs années (si données mensuelles) ou plusieurs mois/semaines (si fréquence plus élevée). 
* Le notebook effectue un “désaisonnalisation” + modélisation SARIMA + moyenne mobile, ce qui implique qu’il y a des **effets saisonniers** — typique dans la consommation énergétique (ex: chaleur, froid). 

### - Méthodologie (“comment”)

1. **Préparation des données** — chargement du dataset, conversion des dates, mise en forme de la série. 
2. **Désaisonnalisation / ajustement des effets externes** — le notebook indique avoir “ajusté les effets de température” sur la consommation via une régression linéaire simple. 
3. **Modélisation** :

   * Application d’une **moyenne mobile** (moving average) pour lisser la série.
   * Application d’un **modèle saisonnier ARIMA (SARIMA)** — pour capturer la structure saisonnière + tendance. 
4. **Prévision** — génération de forecasts à partir du modèle entraîné. 

### - Qui / Auteur / Quand

* **Auteur** : notebook publié par “michaelfumery” sur Kaggle. 
* **Date de publication du notebook** : le notebook a été publié il y a ~5 ans (la page indique “5.0 years ago”). 
* **Datasets** : le dataset porte le nom “dataset_desaisonnalisation_sarima_predict” . 

---

## ✅ Ce qu’on **sait** — ce qu’on **ne sait pas**

| ✅ Connu                                                                                                          | ❓ Inconnu / Non précisé                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Le but général : prévoir la consommation (probablement énergétique) grâce à SARIMA & moving average              | Le **pays** ou la zone géographique concernée par les données                                                                   |
| Le dataset avec 1221 enregistrements, une colonne “consommation totale” + date                                   | Si les données correspondent à un type d’énergie spécifique (électricité, gaz, etc.)                                            |
| L’auteur du notebook, et la date de publication (~5 ans)                                                         | La granularité de la série (mensuelle, hebdomadaire, journalière…) — le notebook **n’explicite pas clairement** cette fréquence |
| L’utilisation d’une régression pour ajuster l’effet de la température + désaisonnalisation + modélisation SARIMA | Les sources d’origine des données (fournisseur, base publique, données synthétiques, etc.)                                      |

---

## 🔎 Mon évaluation — limites & précautions

* Le fait que le pays ne soit pas indiqué rend **difficile de généraliser** les résultats à un contexte concret (par ex. votre pays, Maroc)
* Sans information sur la **fréquence** des données (mensuelle, journalière…), la **validité de la saisonnalité** est incertaine — or la saisonnalité est essentielle pour que SARIMA ait du sens.
* L’ajustement des effets de température est un bon point, mais **on ignore d’où viennent ces données météo** — ce qui pose question sur la qualité / intégrité des données.
* Le dataset est présenté de manière générique — ce qui suggère qu’il pourrait s’agir d’un **jeu de données d’enseignement / démonstration** plutôt qu’un dataset “terrain” avec contexte clair.

---

### 🔬 Interprétation des Graphiques de Diagnostic

<img width="1619" height="1163" alt="image" src="https://github.com/user-attachments/assets/f47430c4-4a23-49aa-8950-b88add24a67e" />


**1. Résidus Standardisés au Fil du Temps (En Haut à Gauche)**  
- Ce graphique montre les résidus (la différence entre les valeurs observées et les prédictions du modèle) au fil du temps.  
- **Ce qu'il faut rechercher** : Idéalement, les résidus devraient fluctuer aléatoirement autour de zéro sans motif ou tendance discernable. Cela indique que le modèle a capturé la majeure partie de la structure sous-jacente des données. Si des motifs clairs apparaissent (par exemple, une variance croissante/décroissante, une saisonnalité), cela suggère que le modèle n'est peut-être pas entièrement spécifié ou qu'il a manqué des composants importants.

**2. Histogramme et Estimation KDE (En Haut à Droite)**  
- Ce graphique affiche la distribution des résidus, ainsi qu'une ligne d'estimation de la densité de noyau (KDE), qui est ensuite comparée à une distribution normale standard (N(0,1)).  
- **Ce qu'il faut rechercher** : Les résidus devraient idéalement être distribués normalement avec une moyenne de zéro. Si l'histogramme suit de près la courbe de la distribution normale, cela suggère que les erreurs du modèle sont aléatoires et normalement distribuées, ce qui est une hypothèse pour de nombreux modèles statistiques. Des écarts par rapport à la normalité pourraient indiquer des problèmes avec les hypothèses du modèle ou la présence de valeurs aberrantes.

**3. Graphique Q-Q Normal (En Bas à Gauche)**  
- Le graphique Quantile-Quantile (Q-Q) compare les quantiles des résidus standardisés aux quantiles d'une distribution normale théorique.  
- **Ce qu'il faut rechercher** : Si les résidus sont normalement distribués, les points sur le graphique Q-Q devraient s'aligner approximativement le long de la ligne droite à 45 degrés. Tout écart significatif par rapport à cette ligne (par exemple, des courbes en S, des queues épaisses) suggère que les résidus ne sont pas normalement distribués.

**4. Corrélogramme (ACF) des Résidus (En Bas à Droite)**  
- Ce graphique montre la fonction d'autocorrélation (ACF) des résidus standardisés. Il mesure la corrélation d'une série chronologique avec ses propres valeurs passées.  
- **Ce qu'il faut rechercher** : Pour un modèle SARIMA bien ajusté, les résidus devraient ressembler à un bruit blanc, ce qui signifie qu'il ne devrait pas y avoir d'autocorrélation significative à un décalage quelconque. Toutes les barres (coefficients d'autocorrélation) devraient se situer dans la zone ombrée bleue (intervalles de confiance). Si des barres dépassent ces intervalles, cela suggère qu'il reste une autocorrélation dans les résidus que le modèle n'a pas capturée, indiquant que le modèle pourrait être amélioré (par exemple, en ajustant les termes AR, MA ou saisonniers).

