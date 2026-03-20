# The North Face E-Commerce -- Recommandation et Topic Modeling

[![Python](https://img.shields.io/badge/Python-3.13-3776AB?style=flat&logo=python&logoColor=fff)](#)
[![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=fff)](#)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=fff)](#)
[![scikit--learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=fff)](#)
[![spaCy](https://img.shields.io/badge/spaCy-09A3D5?style=flat&logo=spacy&logoColor=fff)](#)
[![JEDHA](https://img.shields.io/badge/JEDHA-blueviolet?style=flat)](#)

---

## About

The North Face souhaite exploiter les descriptions de son catalogue produit pour :

1. **Systeme de recommandation** -- Suggerer des produits similaires a un client consultant une fiche produit ("Vous aimerez aussi...").
2. **Restructuration du catalogue** -- Identifier des thematiques latentes dans les descriptions afin de proposer de nouvelles categories de navigation.

Le projet combine du **NLP** (tokenisation, lemmatisation, TF-IDF) et de l'**apprentissage non supervise** (clustering DBSCAN, topic modeling LSA) pour repondre a ces deux objectifs.

---

## Dataset

| Propriete | Valeur |
|-----------|--------|
| Source | [Kaggle -- Product Item Data](https://www.kaggle.com/cclark/product-item-data?select=sample-data.csv) |
| Nombre de produits | 500 |
| Colonnes | `id`, `description` |
| Langue | Anglais |

Le fichier `sample-data.csv` est place dans `data/raw/`.
Le dossier `data/` est gitignore : telecharger le CSV depuis Kaggle et le placer manuellement dans `data/raw/`.

---

## Installation

```bash
# 1. Cloner le depot
git clone https://github.com/athanormark/THE_NORTH_FACE-_-BLOC-3_JEDHA_FORMATION.git
cd THE_NORTH_FACE-_-BLOC-3_JEDHA_FORMATION

# 2. Creer un environnement virtuel
conda create --name the_north_face python=3.13
conda activate the_north_face

# 3. Installer les dependances
pip install -r requirements.txt

# 4. Telecharger le modele de langue spaCy
python -m spacy download en_core_web_sm

# 5. Lancer le notebook
jupyter notebook notebooks/01_EDA_and_Modeling.ipynb
```

---

## Pipeline

### 1. Preprocessing NLP
- Tokenisation, suppression des stop words, lemmatisation via **spaCy** (`en_core_web_sm`).
- Vectorisation **TF-IDF** (`max_features=1000`) produisant une matrice (500 x 1000).

### 2. Clustering DBSCAN
- Metrique : distance cosinus.
- Deux iterations de tuning sur `eps` (`min_samples=3`) :

| eps | Clusters | Outliers | Commentaire |
|-----|----------|----------|-------------|
| 0.2 | 19 | 401 (80 %) | Trop restrictif |
| **0.63** | **17** | **20 (4 %)** | Cible atteinte (10-20 clusters) |

Principaux clusters identifies :
- **Cluster 1** (295 produits) -- Vetements eco-responsables : `recyclable`, `organic`, `cotton`.
- **Cluster 0** (61 produits) -- Baselayers techniques : `gladiodor`, `odor`, `natural`.
- **Cluster 2** (28 produits) -- Sacs et accessoires : `pocket`, `shoulder`, `strap`.

### 3. Systeme de recommandation
La fonction `find_similar_items(item_id)` retourne 5 produits du meme cluster.
Interface interactive via `input()`.

### 4. Topic Modeling (LSA / TruncatedSVD)
- `TruncatedSVD(n_components=10)` -- Variance expliquee : 25.1 %.
- Topic dominant extrait par document.
- Visualisation par WordClouds.

---

## Resultats

| Topic | Mots-cles | Interpretation | Produits |
|-------|-----------|----------------|----------|
| 1 | `organic`, `cotton`, `recyclable` | Eco-responsable | 301 |
| 2 | `shirt`, `ringspun`, `phthalate`, `ink` | Serigraphie | 33 |
| 3 | `merino`, `odor`, `gladiodor`, `wool` | Laine technique | 38 |
| 4 | `button`, `canvas`, `jean`, `welt` | Casual coton | 21 |
| 5 | `merino`, `wool`, `wash`, `chlorine` | Entretien laine | 17 |
| 6 | `sun`, `upf`, `nylon`, `protection` | Protection solaire | 7 |
| 7 | `strap`, `pocket`, `mesh`, `compartment` | Sacs et rangement | 28 |
| 8 | `spandex`, `tencel`, `bra`, `dress` | Vetements femme | 37 |
| 9 | `photo`, `poster`, `outdoor`, `retail` | Marketing/retail | 8 |
| 10 | `sun`, `upf`, `collar`, `rashguard` | Protection UV | 10 |

---

## Conclusion

Le projet repond aux deux objectifs poses par The North Face :

**1. Systeme de recommandation** : DBSCAN avec distance cosinus identifie 17 clusters thematiques (4% d'outliers). Le cluster dominant regroupe les vetements eco-responsables (295 produits sur 500). La fonction `find_similar_items(item_id)` retourne 5 produits du meme cluster, exploitable en production pour un bloc "Vous aimerez aussi".

**2. Restructuration du catalogue** : LSA (10 composantes, 25.1% de variance expliquee) extrait des topics interpretables : eco-responsable, serigraphie, laine technique, protection UV, sacs et rangement. Ces thematiques peuvent alimenter de nouvelles categories de navigation sur le site e-commerce.

**Limites** : le dataset est restreint (500 produits), la variance expliquee par LSA est faible (normal en NLP haute dimension), et aucune metrique de qualite de clustering (Silhouette Score) n'a ete calculee.

---

## Structure du projet

```text
TheNorthFace_Recommendation/
|
|-- data/
|   |-- raw/                # Donnees sources (sample-data.csv)
|   +-- processed/          # Matrices sauvegardees (optionnel)
|
|-- notebooks/
|   +-- 01_EDA_and_Modeling.ipynb   # Notebook principal
|
|-- assets/
|   +-- images/             # Captures, WordClouds
|
|-- requirements.txt        # Dependances Python
|-- .gitignore
+-- README.md
```

---

## Auteur

Athanor SAVOUILLAN · [GitHub](https://github.com/athanormark)
