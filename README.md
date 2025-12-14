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

## 🏗️ Architecture

Le projet se divise en deux workflows distincts :

1.  **Workflow Moderne :** Neo4j (Stockage) ➔ Spark GraphX (Traitement) ➔ Zeppelin (Visualisation).
2.  **Workflow Hadoop :** HDFS (Stockage) ➔ Apache Giraph (Traitement BSP).

---

## 🚀 Installation et Démarrage

### Prérequis
* Docker & Docker Compose installés sur la machine.
* 4 Go de RAM minimum alloués à Docker.

### 1. Cloner le dépôt
```bash
git clone [https://github.com/miskaraminaa/benchmark-giraph-vs-graphx.git](https://github.com/miskaraminaa/benchmark-giraph-vs-graphx.git)
cd benchmark-giraph-vs-graphx
