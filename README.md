# 🏔️ The North Face E-Commerce : Boosting Online Sales

![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/scikit_learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Spacy](https://img.shields.io/badge/SpaCy-09A3D5?style=for-the-badge&logo=spacy&logoColor=white)
![Git](https://img.shields.io/badge/GIT-E44C30?style=for-the-badge&logo=git&logoColor=white)

## 📋 Contexte du Projet

**The North Face**, leader de l'équipement outdoor, souhaite exploiter son catalogue de produits pour augmenter le panier moyen de ses clients. Le département marketing a identifié deux leviers de croissance basés sur la Data Science :

1.  **Système de Recommandation (Cross-Selling)** : Suggérer des produits similaires à un client naviguant sur une page produit (ex: "Vous aimerez aussi...").
2.  **Restructuration du Catalogue** : Identifier des thématiques latentes (cachées) pour créer de nouvelles catégories de navigation (ex: "Gamme Éco-Responsable", "Protection Solaire").

Ce projet utilise des techniques de **NLP (Natural Language Processing)** et d'**Apprentissage Non Supervisé** pour traiter les descriptions textuelles non structurées.

## 🏗️ Architecture Technique (Pipeline)

Le projet suit un pipeline de traitement de données strict :

### 1. Preprocessing (NLP)
Nettoyage des descriptions brutes pour réduire le bruit et la dimensionnalité :
* **Tokenization** : Découpage des phrases en mots.
* **Stop Words Removal** : Suppression des mots courants sans valeur sémantique (le, la, the, and...).
* **Lemmatization** : Normalisation des mots (ex: "running", "runs" → "run") via la librairie **Spacy**.

### 2. Feature Extraction
Transformation du texte en vecteurs numériques interprétables par la machine :
* **TF-IDF (Term Frequency - Inverse Document Frequency)** : Cette méthode pondère les mots. Elle donne plus d'importance aux mots rares et spécifiques (ex: "Gore-Tex", "Merino") qu'aux mots génériques (ex: "shirt", "wear").

### 3. Modélisation Non Supervisée
* **Clustering (DBSCAN)** : Regroupement des produits par densité.
    * *Avantage* : Gère les "outliers" (bruit) et ne nécessite pas de définir le nombre de clusters à l'avance.
    * *Métrique* : Distance Cosinus (adaptée aux espaces textuels).
* **Topic Modeling (LSA/TruncatedSVD)** : Réduction de dimension pour extraire les sujets principaux du corpus.

## 📊 Analyse des Résultats

### 1. Clustering & Recommandation
Après optimisation des hyperparamètres (`eps=0.4`, `min_samples=3`), nous avons obtenu :
* **36 Clusters distincts** regroupant des produits très similaires.
* **Réduction du bruit** : Le nombre d'articles non classés est passé de ~400 à 268.

**Exemple de fonctionnement :**
Lorsqu'un utilisateur consulte le produit *ID 19 (Cap 1 Graphic T-shirt)*, l'algorithme recommande avec succès d'autres produits de la gamme "Cap 1" (Kids, Scoop, etc.), prouvant la capacité du modèle à comprendre les gammes de produits.

### 2. Thèmes Latents (Topic Modeling)
L'analyse LSA a permis de faire émerger des catégories qui ne sont pas explicitement dans le catalogue, offrant des opportunités marketing :
* **Topic "Éco-Responsable"** : Mots clés `organic`, `cotton`, `recyclable`, `threads`.
* **Topic "Technique Hiver"** : Mots clés `merino`, `wool`, `insulation`.
* **Topic "Protection Solaire"** : Mots clés `sun`, `upf`, `protection`, `skin`.

## 🚀 Guide d'Installation

Pour reproduire cette analyse en local :

1.  **Cloner le dépôt :**
    ```bash
    git clone [https://github.com/votre-username/TheNorthFace_Recommendation.git](https://github.com/votre-username/TheNorthFace_Recommendation.git)
    cd TheNorthFace_Recommendation
    ```

2.  **Créer l'environnement virtuel (recommandé avec Conda) :**
    ```bash
    conda create --name the_north_face python=3.8
    conda activate the_north_face
    ```

3.  **Installer les dépendances :**
    ```bash
    # Installation des librairies via pip
    pip install -r requirements.txt
    
    # Téléchargement du modèle de langue anglaise pour Spacy
    python -m spacy download en_core_web_sm
    ```

4.  **Lancer le Notebook :**
    ```bash
    jupyter notebook notebooks/01_EDA_and_Modeling.ipynb
    ```

## 📂 Structure du Projet

```text
TheNorthFace_Recommendation/
│
├── data/
│   ├── raw/             # Données sources (sample-data.csv)
│   └── processed/       # Matrices TF-IDF sauvegardées (optionnel)
│
├── notebooks/
│   └── 01_EDA_and_Modeling.ipynb  # Notebook principal (Nettoyage, Clustering, LSA)
│
├── assets/
│   └── images/          # Captures d'écran, WordClouds pour le rapport
│
├── requirements.txt     # Liste des dépendances
└── README.md            # Documentation du projet
```

## 👤 Auteur
Athanor SAVOUILLAN