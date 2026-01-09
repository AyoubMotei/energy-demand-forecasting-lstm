# Prévision de la Demande Énergétique pour Smart Grid via LSTM

## Contexte du projet

La start-up **EcoVolt** opère un réseau intelligent de distribution d’électricité (*Smart Grid*) destiné notamment à l’alimentation de bornes de recharge pour véhicules électriques.

Afin d’anticiper les pics de consommation et d’éviter les risques de surcharge du réseau, EcoVolt souhaite mettre en place un **modèle de prévision à court terme** capable de :

> **Prédire la consommation électrique de l’heure suivante (t+1)**  
> à partir des **24 heures précédentes**, en tenant compte du **mix énergétique**.

Ce projet constitue une **preuve de concept (POC)** basée sur des données historiques horaires.

---

##  Objectifs du projet

### Objectif métier
- Anticiper les variations de la demande énergétique
- Aider à la prise de décision pour la gestion du réseau électrique

### Objectifs pédagogiques
- Comprendre le fonctionnement des **réseaux de neurones récurrents (RNN)**
- Implémenter un **modèle LSTM multivarié**
- Travailler sur un **problème réel de séries temporelles**
- Justifier les choix de **préprocessing**, d’**architecture** et d’**évaluation**
- Analyser et interpréter les résultats obtenus

---

##  Problématique IA

- **Type de problème** : Régression supervisée sur série temporelle
- **Entrée (X)** : Séquence de 24 heures consécutives
- **Sortie (y)** : Consommation électrique à l’heure suivante
- **Modèle utilisé** : LSTM (Long Short-Term Memory)

---

##  Description du dataset

Le dataset est constitué de données **horaires** et contient les colonnes suivantes :

###  Temps
- `DateTime` : Date et heure de la mesure

### Variable cible
- `Consumption` : Consommation électrique totale (MW)

###  Variables explicatives (production)
- `Production`
- `Nuclear`
- `Wind`
- `Solar`
- `Hydroelectric`
- `Coal`
- `Oil and Gas`
- `Biomass`

=> Toutes les variables sont **numériques**, ce qui les rend directement exploitables par un modèle LSTM.

---

## Étapes techniques du projet

### 1️. Préparation des données temporelles

- Conversion de la colonne `DateTime` en format datetime
- Tri chronologique des données
- Mise en index temporel

 **Pourquoi ?**
- L’ordre temporel est critique en séries temporelles
- Les données ne doivent jamais être mélangées (pas de shuffle)
- Cela évite toute fuite de données du futur vers le passé

---

### 2️. Normalisation des données

- Utilisation de **MinMaxScaler**
- Mise à l’échelle des variables entre 0 et 1

 **Justification**
- La normalisation est indispensable en Deep Learning
- Elle améliore la stabilité et la convergence du modèle
- MinMaxScaler est particulièrement adapté aux fonctions d’activation `tanh` et `sigmoid` utilisées dans les LSTM

---

### 3️. Création des séquences (Windowing)

Transformation des données tabulaires en données séquentielles :

- Fenêtre glissante de **24 heures**
- Chaque échantillon devient une séquence temporelle

 **Format attendu par le LSTM** :
 (samples, timesteps, features)

 
- `samples` : nombre de séquences
- `timesteps` : 24 heures
- `features` : nombre de variables énergétiques

---

### 4️. Split Train / Test temporel

- Séparation chronologique des données
- 80 % pour l’entraînement
- 20 % pour le test

 **Pourquoi pas de train_test_split classique ?**
- Pour éviter toute fuite de données temporelles
- Pour simuler un scénario réel de mise en production

---

### 5️. Architecture du modèle LSTM

Architecture utilisée :

- 1 couche **LSTM (64 unités)**
- 1 couche **Dropout** pour limiter le surapprentissage
- 1 couche **Dense** pour la prédiction finale

**Choix techniques**
- 64 unités permettent de capturer les dépendances temporelles sans surcomplexifier le modèle
- Une seule couche LSTM est suffisante pour un POC
- Optimiseur : Adam
- Fonction de perte : Mean Squared Error (MSE)

---

### 6️. Entraînement du modèle

- Nombre d’époques : 30
- Batch size : 32
- Validation split : 20 %

 Suivi de l’apprentissage via les courbes :
- `loss` (entraînement)
- `val_loss` (validation)

---

### 7️. Évaluation du modèle

#### Courbes loss / val_loss
- Diminution progressive et parallèle
- Absence de divergence
- Pas de surapprentissage détecté

####  Graphe Valeurs Réelles vs Valeurs Prédites
- Forte superposition des courbes
- Bonne capture des tendances et cycles journaliers
- Erreurs principalement sur des pics extrêmes (événements atypiques)

---

##  Résultats et interprétation

- Le modèle généralise correctement sur le jeu de test
- Les prédictions sont cohérentes avec la réalité métier
- Le LSTM capte efficacement les dépendances temporelles
- Le modèle est **exploitable dans un contexte Smart Grid**

---


---

##  Technologies utilisées

- Python
- Pandas, NumPy
- Scikit-learn
- TensorFlow / Keras
- Matplotlib
- Google Colab

---

## 👤 Auteur

**Ayoub Motei**  


---


