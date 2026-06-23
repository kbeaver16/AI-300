# 📘 Introduction to Spark for Data Exploration & Processing

_From: 11_Introduction_to_Spark_for_Data_Exploration_and_Processing.mp3_

## SUMMARY

Apache Spark is a **distributed data processing engine** designed to handle the challenges of big data: volume, variety, velocity, and veracity. In Azure Machine Learning, Spark is used for **data exploration, cleansing, transformation, and feature engineering**—either through **serverless Spark compute** or via **Azure Databricks**. Spark’s DataFrame and SQL APIs allow scalable, parallel processing of large datasets, making it a foundational tool before model training.

> “Spark is a very powerful cross‑platform technology… and all the skills you learn here transfer directly to Databricks.”

---

## KEY CONCEPTS

### **Big Data Problem (The 4 V’s)**

- **Volume** — massive amounts of data
- **Variety** — structured, semi‑structured, unstructured (CSV, PDFs, images)
- **Velocity** — high‑speed data generation from concurrent users
- **Veracity** — uncertainty, incomplete or inconsistent data

Spark exists to process and extract insights from this environment.

---

### **Apache Spark**

A distributed computing engine designed for:
- Large‑scale data processing
- Parallel execution
- Fault tolerance
- High‑performance analytics

Spark is:
- **Open source**
- **Cross‑platform**
- **Deeply integrated with Databricks**
- **Supported natively in Azure ML via serverless Spark compute**

---

### **Spark Architecture**

- **Driver Node**
    - Receives the job
    - Breaks work into smaller tasks
    - Coordinates execution
- **Worker Nodes**
    - Execute tasks in parallel
    - Scale horizontally
- **Cluster**
    - Group of worker nodes
    - Auto‑scaling handled by Azure when using serverless Spark

---

### **Spark APIs**

Spark exposes multiple APIs:
#### **1. Spark DataFrame API (Primary for Azure ML)**
- Tabular, distributed data structure
- Supports:
    - Filtering
    - Aggregations
    - Joins
    - Cleansing
    - Transformations
#### **2. Spark SQL API**
- Create temporary SQL views
- Run SQL queries on DataFrames
- Ideal for analysts or SQL‑first users
#### **3. Spark Streaming API**
- Real‑time data processing
- Integrates with Kafka, Event Hubs, file streams
#### **4. Spark MLlib**
- Distributed machine learning algorithms
- Scales ML workloads across clusters
#### **5. GraphX**
- Graph processing and analytics

---

### **Spark DataFrame Characteristics**

- **Distributed** — processed across many nodes
- **Immutable** — every transformation creates a new DataFrame
- **Versioned** — supports lineage tracking
- **Schema‑aware** — rows + columns with defined types
- **Scalable** — handles millions of rows efficiently

---

### **Spark in Azure Machine Learning**

Spark can be used in two primary ways:

#### **1. Serverless Spark in Azure ML**
- No cluster management
- Auto‑scaling
- Pay‑per‑use
- Ideal for:
    - Data cleansing
    - Feature engineering
    - ETL before training
#### **2. Azure Databricks + Azure ML Integration**
- Databricks handles:
    - Heavy data engineering
    - Large‑scale ETL
    - Delta Lake storage
- Azure ML handles:
    - Model training
    - Model evaluation
    - Deployment (real‑time or batch endpoints)

---

## DEFINITIONS

- **Driver Node** — orchestrates Spark jobs
- **Worker Node** — executes tasks in parallel
- **Spark Session** — entry point for Spark functionality
- **Temporary View** — SQL‑queryable representation of a DataFrame
- **MLlib** — Spark’s distributed ML library

---

## EXAMPLES

### Example: Creating a Spark DataFrame

```python
df = spark.read.load(
    "products.csv",
    format="csv",
    header=True
)
df.show(10)
```

### Example: Creating a Temporary SQL View

```python
df.createOrReplaceTempView("products")

spark.sql("""
    SELECT category,
           COUNT(product_id) AS product_count
    FROM products
    GROUP BY category
    ORDER BY category
""").show()
```

### Example: Equivalent PySpark Transformation

```python
from pyspark.sql import functions as F

df.groupBy("category") \
  .agg(F.count("product_id").alias("product_count")) \
  .orderBy("category") \
  .show()
```

---

## STEP‑BY‑STEP PROCESSES

### **How Spark Processes Data**

1. User submits a job
2. Driver node analyzes the job
3. Driver splits work into tasks
4. Tasks distributed to worker nodes
5. Worker nodes process data in parallel
6. Results returned to driver
7. Driver assembles final output

---

### **Using Spark in Azure ML**

1. Attach a **serverless Spark compute** to your notebook
2. Load data into a Spark DataFrame
3. Clean, transform, and engineer features
4. Save processed data as an **MLTable**
5. Use MLTable for model training

---

## ACTIONABLE TAKEAWAYS

- Spark is essential for scalable data processing before ML training.
- Serverless Spark in Azure ML removes cluster management overhead.
- Spark DataFrames + SQL API = flexible, powerful data engineering workflow.
- Spark skills transfer directly to Databricks.
- Use Spark for ETL → MLTable → model training in Azure ML.

---

## TAGS

#AzureML #Spark #DataEngineering #BigData #Databricks #MLTable #ETL #DistributedComputing #AI300

---

## BACKLINK SUGGESTIONS

- [[Azure ML – Datastores & Data Assets]]
- [[Azure ML – MLTable Format]]
- [[Azure ML – Compute Options]]
- [[Azure ML – Data Processing Pipelines]]
- [[Azure Databricks Overview]]
- [[Spark DataFrame API]]
- [[Spark SQL API]]
