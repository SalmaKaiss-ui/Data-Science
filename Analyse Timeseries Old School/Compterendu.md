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

