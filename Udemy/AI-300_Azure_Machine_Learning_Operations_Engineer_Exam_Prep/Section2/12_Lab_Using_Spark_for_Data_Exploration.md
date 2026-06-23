# 📘 Lab: Using Spark for Data Exploration

_From: 12_Lab_Using_Spark_for_Data_Exploration.mp3_

## SUMMARY

This lab walks through how to use **serverless Spark compute** in Azure Machine Learning to perform data exploration, schema definition, SQL querying, and visualization on a multi‑file sales dataset. You attach a Spark compute session, upload CSV files to a datastore, register them as a **URI Folder Data Asset**, load them into a Spark DataFrame, define schema, run SQL queries, and visualize results using Matplotlib and Seaborn.

> “Let’s get comfortable running Spark queries and build ideas around using Spark for data analysis and engineering.”

---

## KEY CONCEPTS

### **Serverless Spark Compute**

- Auto‑scaling Spark cluster managed by Azure
- No infrastructure setup
- Ideal for ETL, data exploration, and preprocessing
- Must be explicitly attached to the notebook

---

### **URI Folder Data Asset**

A data asset referencing an entire folder of files (e.g., multiple CSVs).  
Used when:
- You want to treat multiple files as a single dataset
- Spark should load all files using a wildcard path

---

### **Spark DataFrame**

- Distributed, tabular data structure
- Supports schema definition
- Enables transformations, filtering, aggregations
- Can be queried using Spark SQL

---

### **Temporary SQL View**

A SQL‑queryable view created from a DataFrame.  
Used to:
- Run SQL queries
- Perform aggregations
- Derive business insights

---

### **Visualization Workflow**

Spark DataFrame → Pandas DataFrame → Matplotlib/Seaborn  
Because visualization libraries do not operate directly on Spark DataFrames.

---

## DEFINITIONS

- **Spark Session** — entry point to Spark functionality
- **StructType / StructField** — schema definition objects
- **Wildcard Path (`*`)** — loads all matching files in a folder
- **Gross Revenue** — computed metric: `(unit_price * quantity) + tax`

---

## STEP‑BY‑STEP PROCESSES

### **1. Attach Serverless Spark Compute**

- Switch notebook compute from managed instance → serverless Spark
- Wait 3–5 minutes for Spark session initialization

### **2. Create MLClient**

- Provide subscription ID, resource group, workspace name
- Authenticate
- Verify default datastore

### **3. Upload Sales Data**

- Files: `2019.csv`, `2020.csv`, `2021.csv`
- Upload to: `sales-data/` folder in datastore
- Ensure correct folder path

### **4. Register URI Folder Data Asset**

- Type: `uri_folder`
- Name: `sales-data`
- Contains all three CSVs

### **5. Load Data into Spark DataFrame**

- Use wildcard path: `sales-data/*.csv`
- `spark.read.load(..., format="csv")`
- Display DataFrame

### **6. Define Schema**

- Use `StructType` with explicit column names + types
- Reload DataFrame with schema applied

### **7. Create Temporary SQL View**

- `df.createOrReplaceTempView("sales_order")`
- Run SQL queries

### **8. Run Business Insight Query**

Compute gross revenue per year:

- Extract year from `order_date`
- Compute `(unit_price * quantity) + tax`
- Group by year
- Order by year

### **9. Visualize Results**

- Convert Spark DataFrame → Pandas
- Use Matplotlib or Seaborn to plot bar charts

---

## CODE BLOCKS

### **Create Spark DataFrame**

```python
df = spark.read.load(
    path,
    format="csv",
    header=True
)
df.show()
```

### **Define Schema**

```python
from pyspark.sql.types import *

schema = StructType([
    StructField("sales_order_number", StringType()),
    StructField("sales_order_line_number", IntegerType()),
    StructField("order_date", DateType()),
    StructField("customer_name", StringType()),
    StructField("email", StringType()),
    StructField("unit_price", FloatType()),
    StructField("quantity", IntegerType()),
    StructField("tax", FloatType())
])

df = spark.read.load(path, format="csv", schema=schema)
df.show()
```

### **Create Temporary View + Query**

```python
df.createOrReplaceTempView("sales_order")

result = spark.sql("""
    SELECT
        YEAR(order_date) AS order_year,
        SUM(unit_price * quantity + tax) AS gross_revenue
    FROM sales_order
    GROUP BY order_year
    ORDER BY order_year
""")
result.show()
```

### **Visualization**

```python
import matplotlib.pyplot as plt

pdf = result.toPandas().dropna()
plt.bar(pdf["order_year"], pdf["gross_revenue"])
plt.show()
```

---

## EXAMPLES

### Example: Gross Revenue Output

|Year|Gross Revenue|
|---|---|
|2019|…|
|2020|…|
|2021|…|

### Example: Visualization

- X‑axis: Year
- Y‑axis: Gross Revenue
- Bar chart using Matplotlib or Seaborn

---

## ACTIONABLE TAKEAWAYS

- Always attach serverless Spark before running Spark code.
- Use **URI Folder** assets for multi‑file datasets.
- Define schema explicitly to avoid incorrect column types.
- Spark SQL is powerful for business insights.
- Convert to Pandas before visualization.
- Spark is ideal for ETL → MLTable → model training workflows.

---

## TAGS

#AzureML #Spark #DataEngineering #ServerlessSpark #DataExploration #URIDataAsset #SQL #Visualization #AI300

---

## BACKLINK SUGGESTIONS

- [[Spark DataFrame API]]
- [[Spark SQL API]]
- [[Azure ML – Datastores & Data Assets]]
- [[Azure ML – MLTable Format]]
- [[Azure ML – Data Processing Pipelines]]
- [[Azure ML – Serverless Spark]]
