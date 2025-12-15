# Benchmark Big Data : Apache Giraph vs Spark GraphX & Intégration Neo4j

![Big Data](https://img.shields.io/badge/Big%20Data-Project-blue) ![Docker](https://img.shields.io/badge/Docker-Compose-green) ![Spark](https://img.shields.io/badge/Apache-Spark%20GraphX-orange) ![Giraph](https://img.shields.io/badge/Apache-Giraph-red) ![Neo4j](https://img.shields.io/badge/Neo4j-GraphDB-lightgrey)

Ce dépôt présente l’implémentation complète ainsi que le rapport d’un projet de **benchmark comparatif** entre deux frameworks majeurs de traitement distribué de graphes : **Apache Giraph** (modèle BSP, vertex-centric) et **Apache Spark GraphX** (modèle data-centric basé sur RDD).

Le projet s’appuie sur une chaîne de traitement moderne intégrant **Neo4j** pour le stockage des graphes et **Apache Zeppelin** pour l’analyse interactive. L’ensemble de l’infrastructure est entièrement **conteneurisé avec Docker**, garantissant une exécution locale reproductible.

---

## Objectifs

1. Concevoir et déployer une architecture distribuée pour l’analyse de graphes.
2. Comparer les performances de l’algorithme **PageRank** sur Spark GraphX et Apache Giraph.
3. Étudier la structure du graphe à l’aide des algorithmes **Connected Components** et **Triangle Count**.

---

## Dataset

- **Source** : SNAP (Stanford Network Analysis Project)
- **Nom** : [Wiki-Vote](https://snap.stanford.edu/data/wiki-Vote.html)
- **Caractéristiques** : 7 115 nœuds et 103 689 arêtes orientées

---

# Architecture du Projet

L’architecture repose sur deux workflows distincts, chacun correspondant à une approche spécifique du traitement de graphes distribués. Les deux pipelines sont déployés via **Docker Compose** afin d’assurer cohérence et reproductibilité.

---

## Workflow 1 : Neo4j, Spark GraphX et Zeppelin

### Stockage
- **Neo4j** est utilisé comme base de données de graphes native (NoSQL).
- Les opérations de manipulation et de traversée du graphe sont réalisées via le langage **Cypher**.

### Traitement analytique
- **Spark GraphX** permet de :
  - Charger les données depuis Neo4j.
  - Convertir le graphe en **RDD**.
  - Exécuter des algorithmes itératifs tels que **PageRank**, **Connected Components** et **Triangle Count**.

### Interface d’analyse
- **Apache Zeppelin** est utilisé pour :
  - La création de notebooks analytiques.
  - La configuration dynamique des dépendances.
  - L’analyse exploratoire (distribution des degrés, métriques globales).
  - La visualisation des résultats.

### Étapes principales
- Déploiement des services via **Docker Compose** (Neo4j, Spark Master, Spark Worker, Zeppelin).
- Import du dataset Wiki-Vote au format CSV dans Neo4j.
- Connexion de Spark à Neo4j pour le chargement du graphe.
- Analyse exploratoire et exécution des algorithmes avec mesure des performances.

---

## Workflow 2 : Hadoop et Apache Giraph

### Infrastructure
- Cluster **Hadoop** avec **HDFS** pour le stockage distribué.

### Traitement
- **Apache Giraph** est utilisé pour l’exécution de **PageRank** selon un modèle *vertex-centric*, implémenté en Java.

### Étapes principales
- Déploiement d’un cluster Hadoop intégrant Giraph via Docker.
- Export du graphe Wiki-Vote vers HDFS au format texte (liste d’arêtes).
- Exécution du job PageRank avec des paramètres spécifiques (supersteps, métriques).
- Analyse des résultats à partir des logs, compteurs MapReduce et temps d’exécution.
- Visualisation des résultats via Zeppelin.

---

## Réseau et conteneurisation

- Tous les services communiquent via un réseau Docker dédié (**graph-network**).
- Des volumes persistants sont utilisés pour conserver les données (ex. : `./neo4j-data:/data`).
- Plugins Neo4j intégrés :
  - **APOC**
  - **Graph Data Science** pour les traitements avancés.

---

## Préparation du Dataset

Le dataset **Wiki-Vote** est :
- Importé dans **Neo4j** au format CSV pour les analyses avec Spark GraphX.
- Exporté vers **HDFS** afin d’être traité par **Apache Giraph**.

---

# Étude Comparative

## Performances

- **Spark GraphX** se distingue par sa rapidité et son interactivité sur des graphes de taille modérée.
- **Apache Giraph** est mieux adapté aux graphes de très grande taille, offrant une meilleure scalabilité.
- Résultats PageRank sur Wiki-Vote :
  - Giraph : ~13,7 secondes (incluant l’overhead du cluster).
  - Spark GraphX : temps inférieur pour ce volume de données.

## Modèles de programmation

- **Spark GraphX** adopte une approche *data-centric*, facilitant l’analyse exploratoire.
- **Apache Giraph** repose sur un modèle *vertex-centric*, plus proche des paradigmes BSP.
- Spark offre globalement une meilleure productivité pour le développement et l’expérimentation.

## Synthèse

Le choix de la solution dépend du contexte :
- **Analyse interactive et prototypage rapide** : Spark GraphX.
- **Traitement massif à grande échelle** : Apache Giraph.

---

# Perspectives

- Passage à des **datasets de plus grande dimension**.
- Intégration d’algorithmes supplémentaires (ex. : *Community Detection*).
- Déploiement sur un **cluster cloud** pour évaluer la scalabilité en conditions réelles.

---

## Installation et Démarrage

### Prérequis
- Docker et Docker Compose installés.
- Au minimum 4 Go de RAM alloués à Docker.
  
---

  Ce projet a été réalisé **en collaboration avec [ayoubharati](https://github.com/ayoubharati) et [aziz-mohammed](https://github.com/aziz-mohammed)**.


### Clonage du dépôt
```bash
git clone https://github.com/miskaraminaa/benchmark-giraph-vs-graphx.git
cd benchmark-giraph-vs-graphx
