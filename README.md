# Dragon Egg Intelligence

**Exploration des données et benchmark de modèles de Machine Learning**

Ce projet est consacré à l'analyse de mesures physiques et énergétiques d'œufs de dragons. L'objectif est de préparer les données, de comparer plusieurs modèles de Machine Learning et d'identifier les modèles les plus adaptés à trois problématiques métier.

## Objectifs

Le projet étudie trois problématiques :

- **Classification :** prédire le clan d'un dragon (Feu, Foudre ou Glace).
- **Régression :** estimer l'énergie arcanique à partir des caractéristiques physiques d'un œuf.
- **Détection d'anomalies :** identifier les œufs présentant des mesures inhabituelles afin de déclencher une vérification.

## Technologies utilisées

- Python
- Pandas et NumPy
- Matplotlib et Seaborn
- Scikit-learn
- Jupyter Notebook

## Structure du projet

```text
TP_Dragons/
├── TP_Dragons.ipynb
├── data/
│   └── dragons.csv
└── outputs/
    ├── dragons_prepares.csv
    ├── rapport_qualite.csv
    ├── benchmark_classification.csv
    ├── benchmark_regression.csv
    ├── benchmark_anomalies.csv
    ├── indicateurs_anomalies.csv
    ├── observations_signalees.csv
    ├── validation_croisee_classification.csv
    └── validation_croisee_regression.csv
```

Les fichiers du dossier `outputs/` sont générés par le notebook. Ils ne seront disponibles qu'après l'exécution des cellules d'export.

## Données

Le jeu de données contient initialement 607 lignes et 7 colonnes.

| Variable | Description |
|---|---|
| `id_oeuf` | Identifiant de l'œuf |
| `temperature_nid_c` | Température du nid en degrés Celsius |
| `humidite_nid_pct` | Humidité du nid en pourcentage |
| `energie_arcanique` | Mesure de l'énergie arcanique |
| `poids_oeuf_kg` | Poids de l'œuf en kilogrammes |
| `vibration_coquille_hz` | Fréquence des vibrations de la coquille |
| `clan_dragon` | Clan associé à l'œuf : Feu, Foudre ou Glace |

## Travaux réalisés

### 1. Exploration et nettoyage

- Analyse des dimensions, des types et des valeurs manquantes.
- Recherche des doublons et vérification des identifiants.
- Harmonisation des noms de colonnes et des catégories.
- Conversion des mesures numériques.
- Contrôle des domaines de valeurs et des mesures inhabituelles.
- Analyse des distributions et des corrélations.
- Préparation d'un jeu de données réutilisable.

Après nettoyage, le jeu de données contient **600 lignes et 7 colonnes**, sans doublons complets.

Les valeurs inhabituelles ne sont pas supprimées automatiquement : elles sont examinées selon leur cohérence et leur contexte.

### 2. Classification du clan

La cible est `clan_dragon`. Les variables explicatives sont les cinq mesures numériques. L'identifiant `id_oeuf` est exclu des prédicteurs.

Modèles comparés :

- DummyClassifier
- Régression logistique
- Arbre de décision
- Random Forest Classifier
- K-Nearest Neighbors (KNN)
- SVM avec noyau RBF

Les performances sont évaluées avec l'accuracy, la précision, le rappel, le F1-score macro et la matrice de confusion. Une validation croisée stratifiée complète l'évaluation.

### 3. Régression de l'énergie arcanique

La cible est `energie_arcanique`. Les prédicteurs sont la température, l'humidité, le poids et les vibrations de la coquille.

Modèles comparés :

- DummyRegressor
- Régression linéaire
- Random Forest Regressor
- K-Nearest Neighbors Regressor
- HistGradientBoostingRegressor

Les métriques utilisées sont la MAE, la RMSE et le coefficient R². Les graphiques des valeurs réelles et prédites ainsi que l'analyse des résidus permettent d'examiner les erreurs.

### 4. Détection des anomalies

Trois méthodes sont comparées :

- Isolation Forest
- Local Outlier Factor (LOF)
- Méthode IQR fondée sur les quartiles

Les valeurs manquantes sont imputées et les variables numériques sont standardisées pour les méthodes qui en ont besoin.

Une observation atypique n'est pas nécessairement une erreur. Les résultats constituent des alertes exploratoires à vérifier.

## Benchmark des modèles

Les tableaux ci-dessous présentent une synthèse des principaux résultats obtenus. Les résultats détaillés sont exportés dans le dossier `outputs/`.

### Classification

| Modèle | Accuracy sur le test | F1 macro sur le test | F1 macro moyen en validation croisée |
|---|---:|---:|---:|
| DummyClassifier | 33,33 % | 16,67 % | 16,6 % |
| KNN | 98,33 % | 98,33 % | 98,75 % |
| Random Forest | 98,33 % | 98,33 % | Environ 98,5 % |
| SVM RBF | 98,33 % | 98,33 % | Environ 98,5 % |

KNN, Random Forest et SVM RBF obtiennent des performances proches sur le jeu de test. KNN présente le meilleur F1 macro moyen parmi les modèles testés en validation croisée.

### Régression

| Modèle retenu | MAE sur le test | RMSE sur le test | R² sur le test | MAE moyen en validation croisée | RMSE moyen en validation croisée |
|---|---:|---:|---:|---:|---:|
| KNN | 8,14 | 10,26 | 0,553 | 9,57 | 12,85 |

KNN est le meilleur candidat selon le RMSE moyen de validation croisée. Ses erreurs restent néanmoins notables : la prédiction de l'énergie arcanique demeure perfectible.

Les résultats complets des cinq modèles sont disponibles dans [`outputs/benchmark_regression.csv`](outputs/benchmark_regression.csv).

### Détection des anomalies

| Méthode | Observations signalées | Pourcentage |
|---|---:|---:|
| Isolation Forest | 30 | 5 % |
| LOF | 30 | 5 % |
| IQR | 12 | 2 % |

Les méthodes ne signalent pas nécessairement les mêmes observations. Les œufs signalés par plusieurs méthodes sont à examiner en priorité.

Les détails sont disponibles dans :

- [`outputs/benchmark_anomalies.csv`](outputs/benchmark_anomalies.csv)
- [`outputs/indicateurs_anomalies.csv`](outputs/indicateurs_anomalies.csv)
- [`outputs/observations_signalees.csv`](outputs/observations_signalees.csv)

### Fichiers de résultats

| Fichier | Contenu |
|---|---|
| [`rapport_qualite.csv`](outputs/rapport_qualite.csv) | Indicateurs de qualité avant et après nettoyage |
| [`dragons_prepares.csv`](outputs/dragons_prepares.csv) | Jeu de données préparé |
| [`benchmark_classification.csv`](outputs/benchmark_classification.csv) | Métriques des modèles de classification |
| [`benchmark_regression.csv`](outputs/benchmark_regression.csv) | Métriques des modèles de régression |
| [`benchmark_anomalies.csv`](outputs/benchmark_anomalies.csv) | Nombre et pourcentage de signalements par méthode |
| [`validation_croisee_classification.csv`](outputs/validation_croisee_classification.csv) | Résultats de validation croisée en classification |
| [`validation_croisee_regression.csv`](outputs/validation_croisee_regression.csv) | Résultats de validation croisée en régression |
| [`observations_signalees.csv`](outputs/observations_signalees.csv) | Détail des observations signalées |

## Installation

Python 3.10 ou une version compatible avec les dépendances du projet est recommandé.

### 1. Récupérer le dépôt

```bash
git clone <https://github.com/Chaima425/TP_Dragons.git>
cd TP_Dragons
```

### 2. Créer un environnement virtuel

Sous Linux ou WSL :

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Sous Windows PowerShell :

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Installer les dépendances

```bash
python -m pip install --upgrade pip
python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyter ipykernel
```

## Exécution du projet

Depuis le dossier du projet, lancer :

```bash
jupyter notebook
```

Ou ouvrir le notebok et puis exécuter les cellules dans l'ordre.

Le notebook effectue l'exploration, le nettoyage, la modélisation, l'évaluation et la détection d'anomalies. Les résultats sont ensuite exportés dans le dossier `outputs/`.

**Important :** le chemin du fichier CSV dans le notebook doit correspondre à l'emplacement réel des données.Sinon le chemin utilisé par le notebook doit être adapté en conséquence.

## Limites

- Les résultats dépendent du jeu de données et de la séparation entre entraînement et test.
- Les performances observées ne garantissent pas les mêmes résultats sur de nouvelles données.
- Les anomalies détectées doivent être vérifiées avant toute décision.
- Les identifiants répétés doivent être examinés pour déterminer s'ils correspondent à plusieurs mesures d'un même œuf.
- Si plusieurs lignes correspondent au même œuf, une séparation par identifiant peut être nécessaire pour éviter une évaluation trop optimiste.
- Les résultats prédictifs ne démontrent pas de causalité entre les variables.

## Conclusion

Ce projet met en œuvre les principales étapes d'une démarche de Data Science : exploration, nettoyage, préparation des données, comparaison de modèles, évaluation et détection d'anomalies.

La classification des clans donne de bons résultats sur les données testées. La prédiction de l'énergie arcanique est plus limitée et pourrait être améliorée. La détection d'anomalies permet d'identifier des observations inhabituelles qui méritent une analyse complémentaire.

Les modèles retenus constituent des candidats prometteurs, mais leur fiabilité doit être confirmée sur des données indépendantes et après vérification des éventuels identifiants répétés.