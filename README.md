
# Technical Project Overview: Workforce Analytics Platform

This project leverages the **Databricks Lakehouse architecture** to build a scalable data engineering and machine learning pipeline. It transforms raw workforce data into actionable insights using a tiered Medallion Architecture (Raw → Silver → Gold) governed by Unity Catalog.

## 1. Data Storage & Governance

### Databricks Unity Catalog

Used for secure, governed data storage and access control.

* **Structure:** The `test` catalog contains core schemas like `default` (user tables/views) and `information_schema` (metadata).
* **Unified Management:** All assets—including Delta tables and ML models—are registered under `test.default` for centralized governance.

---

## 2. Data Engineering

### Spark DataFrames & SQL

* **Ingestion:** Raw CSV data is ingested into a managed Delta table (`employee_data`) ensuring **ACID compliance**.
* **Transformation:** Data cleaning, type casting, and feature engineering are handled via Spark SQL and DataFrame API.
* **Silver Layer:** An optimized table (`employee_performance_silver`) is created using **clustered storage** to enhance query performance for downstream analytics.

---

## 3. Machine Learning

### MLflow

* **Experiment Tracking:** Manages the full lifecycle of the **Random Forest Regressor** model.
* **Model Registry:** The model is registered as `test.default.productivity_prediction_model` within Unity Catalog for versioning and reproducibility.
* **Inference:** Python-based inference scripts write prediction results directly to the Gold layer.

### SparkML

* **Scalable Pipelines:** Utilized for high-volume feature engineering, including `StringIndexing`, `VectorAssembler`, and `StandardScaler`.
* **Distributed Training:** Enables training of models directly on large-scale datasets across the cluster.

---

## 4. Data Products & BI

### Delta Tables & SQL Views

* **Gold Layer:** The `gold_ml_productivity_insights` table stores actual vs. predicted productivity scores.
* **Analytics Views:** Business-friendly SQL views (`gold_performance_insights`) join Silver and Gold data to provide:
* Performance gap analysis.
* Departmental rankings.
* ML model status indicators.



### Power BI / Dashboards

* Gold-tier views are optimized for **DirectQuery** or Import mode in Power BI, allowing stakeholders to visualize workforce trends in real-time.

---

## 5. Platform Features

* **Serverless Compute:** Provides cost-effective, auto-scaling compute power for ETL and ML workloads.
* **UC Volumes:** Managed storage at `/Volumes/test/default/mlflow_tmp/` is utilized for temporary MLflow artifacts, satisfying Databricks serverless platform requirements.

---

## Summary Table

| Tool / Feature | Purpose / Usage |
| --- | --- |
| **Unity Catalog** | Data/model governance, access control, and schema management. |
| **Spark SQL/DF** | Data ingestion, cleaning, and complex transformations. |
| **Delta Tables** | Reliable, performant storage (ACID) for Raw, Silver, and Gold layers. |
| **MLflow** | Experiment tracking, model registry, and lifecycle management. |
| **SparkML** | Scalable ML pipelines for feature engineering and training. |
| **SQL Views** | BI-ready analytics, calculated fields, and department rankings. |
| **UC Volumes** | Temporary storage required for SparkML operations on serverless. |
| **Power BI** | Visualization and business consumption of gold-tier insights. |

