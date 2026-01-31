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

