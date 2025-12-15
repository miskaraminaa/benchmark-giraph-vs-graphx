# Benchmark Big Data : Apache Giraph vs Spark GraphX & Intégration Neo4j

![Big Data](https://img.shields.io/badge/Big%20Data-Project-blue) ![Docker](https://img.shields.io/badge/Docker-Compose-green) ![Spark](https://img.shields.io/badge/Apache-Spark%20GraphX-orange) ![Giraph](https://img.shields.io/badge/Apache-Giraph-red) ![Neo4j](https://img.shields.io/badge/Neo4j-GraphDB-lightgrey)

Ce dépôt contient l'implémentation complète et le rapport d'un projet d'étude comparative entre deux frameworks majeurs de traitement de graphes distribués : **Apache Giraph** (modèle BSP) et **Apache Spark GraphX** (modèle RDD).

Le projet inclut également une chaîne de traitement moderne utilisant **Neo4j** pour le stockage et **Apache Zeppelin** pour l'analyse interactive, le tout orchestré via **Docker**.

## 📋 Objectifs

1.  Mettre en œuvre une architecture distribuée pour l'analyse de grands graphes.
2.  Comparer les performances (temps d'exécution) de l'algorithme **PageRank**.
3.  Analyser la cohésion du réseau via **Connected Components** et **Triangle Count**.

## 🗂️ Dataset

* **Source** : SNAP (Stanford Network Analysis Project)
* **Nom** : [Wiki-Vote](https://snap.stanford.edu/data/wiki-Vote.html)
* **Métriques** : 7 115 nœuds, 103 689 arêtes orientées.

---

# Architecture du Projet

L’architecture est divisée en deux workflows principaux, tous deux conteneurisés via **Docker** afin de garantir une exécution locale reproductible.

---

## Workflow 1 : Neo4j + Spark GraphX + Zeppelin

### Stockage
- **Neo4j** comme base de données de graphes native (NoSQL).
- Utilisation de **Cypher** pour les opérations CRUD et les traversées de graphes.

### Traitement analytique
- **Spark GraphX** pour :
  - Charger le graphe depuis Neo4j.
  - Le transformer en **RDD**.
  - Exécuter des algorithmes itératifs tels que **PageRank**, **Connected Components** et **Triangle Count**.

### Interface
- **Apache Zeppelin** pour :
  - Créer des notebooks interactifs.
  - Configurer les dépendances.
  - Charger et explorer les données (ex. : distribution des degrés).
  - Visualiser les résultats analytiques.

### Étapes clés
- Mise en place de **Docker Compose** (services : Neo4j, Spark Master, Spark Worker, Zeppelin).
- Import du dataset **Wiki-Vote** au format CSV dans Neo4j via Cypher.
- Connexion de Spark à Neo4j pour charger le graphe en **DataFrame/RDD**.
- Analyse exploratoire et exécution des algorithmes avec **benchmarking**.

---

## Workflow 2 : Hadoop + Apache Giraph

### Infrastructure
- **Cluster Hadoop** avec **HDFS** pour le stockage distribué.

### Traitement
- **Apache Giraph** pour l’exécution itérative de **PageRank** en mode *vertex-centric* (implémentation en Java).

### Étapes clés
- Intégration d’un cluster **Hadoop + Giraph** dans Docker.
- Export du graphe **Wiki-Vote** vers **HDFS** au format texte (liste d’arêtes).
- Exécution du job PageRank avec une configuration spécifique (supersteps, métriques).
- Analyse des résultats (logs, timers, compteurs MapReduce).
- Visualisation interactive des résultats via **Zeppelin**.

---

## Réseau et conteneurisation
- Interconnexion de tous les services via un réseau Docker dédié (**graph-network**).
- Utilisation de volumes persistants pour les données (ex. : `./neo4j-data:/data`).
- Plugins Neo4j :
  - **APOC**
  - **Graph Data Science** pour des fonctionnalités avancées.

---

## Dataset
- **Wiki-Vote** : graphe orienté représentant des votes sur Wikipedia.
- Préparé au format **CSV**, importé dans **Neo4j**, puis exporté vers **HDFS** pour le traitement avec **Giraph**.


---
# Étude Comparative

## Performances
- **Spark GraphX** est plus rapide et interactif pour des graphes de taille modeste.
- **Apache Giraph** excelle en **scalabilité** pour des graphes très volumineux (jusqu’à des trillions d’arêtes).
- Temps d’exécution de **PageRank** sur le dataset *Wiki-Vote* :
  - Giraph : ~13,7 s (incluant l’overhead du cluster).
  - Spark : baseline plus rapide pour ce cas d’usage.

## Modèles de programmation
- **Data-centric** (Spark GraphX).
- **Vertex-centric** (Giraph).
- Spark offre une meilleure **expérience développeur**, notamment pour l’analyse exploratoire et le prototypage rapide.

## Synthèse
Le choix de la technologie dépend du cas d’usage :
- **Interactivité et analyse exploratoire** → Spark GraphX.
- **Puissance brute et très grande échelle** → Apache Giraph.

---

# Perspectives

- Extension à des **datasets de plus grande taille**.
- Intégration d’**autres algorithmes** de graphes (ex. : *Community Detection*).
- Déploiement sur un **cluster cloud** afin d’évaluer la scalabilité réelle.


## 🚀 Installation et Démarrage

### Prérequis
* Docker & Docker Compose installés sur la machine.
* 4 Go de RAM minimum alloués à Docker.

### 1. Cloner le dépôt
```bash
git clone [https://github.com/miskaraminaa/benchmark-giraph-vs-graphx.git](https://github.com/miskaraminaa/benchmark-giraph-vs-graphx.git)
cd benchmark-giraph-vs-graphx
