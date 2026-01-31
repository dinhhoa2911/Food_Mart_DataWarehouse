# 📊 ITJOBANALYSE: Data Lakehouse & Job Demand Prediction System

![Project Status](https://img.shields.io/badge/Status-Active-brightgreen)
![Docker](https://img.shields.io/badge/Containerized-Docker-blue)
![License](https://img.shields.io/badge/License-MIT-lightgrey)

## 📖 Introduction

**ITJOBANALYSE** is a modern Data Lakehouse project designed to collect, process, and analyze IT recruitment data in Vietnam (sourced from TopDev, VietnamWorks). By leveraging the **Medallion Architecture**, the system transforms raw data into actionable insights.

A key feature of this project is the integration of the **Facebook Prophet** machine learning algorithm to forecast future recruitment trends, helping decision-makers understand the labor market demand.

## 👥 Team Members

| Student Name | ID | Role |
| :--- | :--- | :--- |
| **Lê Tuấn Hưng** | 21130368 | Team Leader / Data Engineer |
| **Nguyễn Ngọc Hiếu** | 21130351 | Data Analyst |
| **Trần Văn Thắng** | 21130528 | Data Scientist |

## 🏗️ System Architecture

The entire system is deployed on a **Docker** infrastructure, ensuring reproducibility and scalability. It integrates **Apache Iceberg** to bring ACID transactions to the Data Lake.

```mermaid
graph LR
    subgraph Docker_Environment ["🐳 Docker Infrastructure"]
        direction LR
        
        subgraph Ingestion_Processing ["Ingestion & Processing"]
            Spark("✨ Apache Spark<br>(ETL & ML Training)")
            Airflow("🌪️ Apache Airflow<br>(Orchestration)")
        end

        subgraph Storage_Layer ["Storage & Metadata"]
            Hive("🐝 Hive Metastore<br>(Catalog)")
            HDFS[("🐘 Hadoop HDFS<br>(Physical Storage)")]
            Iceberg{{"❄️ Apache Iceberg<br>(Table Format)"}}
        end

        subgraph Serving_Layer ["Serving & Visualization"]
            Trino("🐰 Trino<br>(Query Engine)")
            Superset("📊 Apache Superset<br>(BI Dashboard)")
        end
    end

    Source["Data Sources<br>(TopDev, VietnamWorks)"] --> Spark
    Airflow --> Spark
    Spark --> HDFS
    Spark --> Hive
    Hive -.-> Iceberg
    HDFS --- Iceberg
    Trino --> Iceberg
    Superset --> Trino


🛠️ Tech Stack
Component,Technology,Description
Storage,Hadoop HDFS,Distributed file system for storing raw data and Parquet files.
Table Format,Apache Iceberg,"Provides schema evolution, time travel, and hidden partitioning."
Compute,Apache Spark,Unified engine for heavy ETL processing and ML model training.
Orchestration,Apache Airflow,Workflow automation for scheduling crawling and ETL jobs.
Query Engine,Trino,Fast distributed SQL query engine for interactive analytics.
Visualization,Apache Superset,Modern BI tool for exploring data and building dashboards.
Algorithm,Facebook Prophet,Time-series forecasting model for predicting job demand.

🔄 Data Flow (Medallion Architecture)Bronze Layer (Raw):Ingest raw data (JSON/CSV) scraped from recruitment websites.Stored in HDFS.Silver Layer (Cleansed):Data cleaning, deduplication, and standardization using Spark.Stored as Iceberg Tables.Gold Layer (Aggregated):Business-level aggregations (e.g., jobs by industry, average salary by skill).Optimized for Trino queries.Prediction Layer:Prophet model consumes historical data from Silver/Gold layers.Forecast results are written back to the Gold layer for Superset visualization.🚀 Getting StartedPrerequisitesDocker Desktop (configured with at least 8GB RAM).Git.Installation StepsClone the repository:Bashgit clone [https://github.com/hung21130368/ITJOBANALYSE.git](https://github.com/hung21130368/ITJOBANALYSE.git)
cd ITJOBANALYSE
Start the environment:Bashdocker-compose up -d
Verify running containers:Bashdocker ps
Accessing the ServicesServiceURLDefault CredentialsApache Airflowhttp://localhost:8080airflow / airflowApache Supersethttp://localhost:8088admin / adminTrino Coordinatorhttp://localhost:8081(No password)Spark Masterhttp://localhost:8090-HDFS Namenodehttp://localhost:9870-📊 Dashboard FeaturesThe Superset dashboard provides the following insights:Market Overview: Total active jobs, distribution by city (HCM, Hanoi, Da Nang).Salary Analysis: Average salary trends by programming language (Java, Python, etc.).Demand Forecasting: A line chart showing historical data vs. predicted demand for the next 6 months.📂 Project StructurePlaintext├── dags/                   # Airflow DAGs for ETL and ML pipelines
├── docker/                 # Docker configuration files
├── notebooks/              # Jupyter Notebooks for EDA and Prophet modeling
├── scripts/                # Python scripts for crawling and Spark jobs
├── data/                   # Sample datasets
├── docker-compose.yml      # Main Docker orchestration file
└── README.md               # Project documentation
