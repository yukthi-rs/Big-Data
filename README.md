# Advanced Big Data Processing through MapReduce Abstraction and HDFS Architecture

An end-to-end distributed big data and machine learning pipeline built with **Docker**, **Hadoop (HDFS)**, **Apache Hive**, **Apache Spark (PySpark)** and **Jupyter Notebook** to store, query, clean and classify sentiment on the Amazon Food Reviews dataset (~286.97 MB / ~290 MB)[cite: 4, 7, 8, 15, 17].



## Overview

Single-node relational systems and local workflows hit memory and processing bottlenecks as dataset size and text complexity grow[cite: 4, 5]. In addition, raw review data often contains unescaped commas, special characters, and multiline text fields that can cause parsing issues, column misalignment, and invalid values during ingestion[cite: 4, 10, 16].

This project demonstrates how to:
* Deploy a containerized Hadoop/HDFS and Hive environment using Docker[cite: 8, 15, 16].
* Ingest a large review dataset into HDFS and query it via HiveQL (`OpenCSVSerde`)[cite: 9, 10, 16].
* Clean malformed records and benchmark analytical aggregations across Apache Hive and Apache Spark[cite: 10, 11, 16].
* Train and evaluate a distributed sentiment classification model using PySpark ML and TF-IDF[cite: 12, 13, 18].



## Architecture & Pipeline Stages

1. **Containerized Cluster & HDFS Storage:** Deployed Hadoop ecosystem components in Docker containers and ingested the Amazon Food Reviews dataset into HDFS (`hdfs dfs -put`)[cite: 8, 9, 15].
2. **Data Warehouse & Exploratory SQL (Hive):** Created an external `amazon_food_reviews` table using `org.apache.hadoop.hive.serde2.OpenCSVSerde` to aggregate total helpfulness votes, average helpfulness votes, and average review length across star ratings (`1`–`5`)[cite: 10, 16].
3. **Distributed Preprocessing & Engine Benchmark (PySpark):** Filtered valid ratings, safely cast numeric fields (`try_cast`), removed missing/invalid entries, and ran comparable aggregation workloads in PySpark vs. Hive[cite: 10, 11, 16].
4. **Feature Engineering & Sentiment Classification:** Filtered out neutral (`3`-star) reviews to create binary positive (`1.0`) and negative (`0.0`) labels, split the dataset (`80%` train / `20%` test, `seed=42`), applied tokenization, `HashingTF`, and TF-IDF, and trained a regularized **Logistic Regression** classifier (10 iterations) in Spark ML[cite: 12, 13, 18].
5. **Evaluation & Visualization:** Evaluated test-set predictions using Spark's `BinaryClassificationEvaluator` (`AUC-ROC`) and visualized ROC curves and sentiment/rating distributions in Jupyter Notebook[cite: 13, 14, 15].



## Key Results

| Metric / Benchmark | Result |
| :--- | :--- |
| **Dataset** | Amazon Food Reviews (~286.97 MB / ~290 MB) |
| **Hive Aggregation Runtime** | `7.62 seconds` |
| **PySpark Aggregation Runtime** | `0.99 seconds` (`0.9881s` in run output) |
| **Query Speedup (Spark vs. Hive)** | `~7.7x faster` |
| **ML Model** | Logistic Regression (Spark ML, TF-IDF features) |
| **Test Set Performance** | `AUC-ROC = 0.9201`[cite: 4, 7, 11, 12, 13, 17] |



## Tech Stack

* **Containers & Orchestration:** Docker[cite: 8, 15, 16]
* **Distributed Storage:** Hadoop HDFS[cite: 4, 7, 9]
* **Data Warehouse / Querying:** Apache Hive (HiveQL / Beeline)[cite: 5, 9, 10, 17]
* **Distributed Processing & ML:** Apache Spark / PySpark (`pyspark.sql`, Spark ML TF-IDF & Logistic Regression)[cite: 11, 12, 17, 18]
* **Analysis & Plotting:** Jupyter Notebook, Matplotlib[cite: 13, 15, 17]



## Future Extensions

* Explore deeper neural or transformer-based architectures for text classification[cite: 18].
* Extend the pipeline toward real-time streaming sentiment analysis using Apache Kafka and Spark Streaming[cite: 18].
* Expose model inference via a cloud-hosted API service for live monitoring or recommendation workflows[cite: 18].
