# Day 1 — PySpark Fundamentals

## Reference Links

### Concept Video
- https://www.youtube.com/watch?v=EB8lfdxpirM&list=PLwFJcsJ61ouiU1wvzzRk3pjU8xT9buJhr&index=11

---

# Problems & Practice Links

## Filter Employees with Salary > X
- https://www.geeksforgeeks.org/python/filtering-rows-based-on-column-values-in-pyspark-dataframe/?utm_source=chatgpt.com
- https://sparkbyexamples.com/pyspark/pyspark-where-filter/?utm_source=chatgpt.com

## Add New Column Using Conditions
- https://builtin.com/data-science/pyspark-dataframe?utm_source=chatgpt.com
- https://www.interviewquestionspdf.com/2023/08/pyspark-dataframes-practice-questions.html?utm_source=chatgpt.com

## Good Hands-On Practice Site
- https://ashishcoder.com/courses/pyspark/filter-dataframe-in-pyspark.html?utm_source=chatgpt.com

---

# Topics Covered

## 1. What is PySpark?

PySpark is the Python API for Apache Spark.

Apache Spark is a distributed data processing engine used for:

- big data processing
- ETL pipelines
- machine learning
- streaming
- analytics

PySpark allows us to write Spark code using Python.

---

# 2. Spark Architecture

Spark mainly consists of:

## Driver

The main program that:

- creates SparkSession
- controls execution
- sends tasks to executors

## Executors

Workers that:

- execute tasks
- process data
- return results

## Cluster

A group of machines working together.

---

# 3. SparkSession Setup

SparkSession is the entry point to PySpark.

Example:

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("PySpark Tutorial") \
    .getOrCreate()
```

---

# 4. DataFrames vs Pandas

| PySpark DataFrame         | Pandas DataFrame       |
| ------------------------- | ---------------------- |
| Distributed               | Local                  |
| Handles big data          | Best for small data    |
| Faster for large datasets | Memory limited         |
| Runs on cluster           | Runs on single machine |

---

# 5. Reading Data

Example:

```python
df = spark.read.csv("employees.csv", header=True, inferSchema=True)
```

Important arguments:

- `header=True`
- `inferSchema=True`

---

# 6. Common PySpark Commands

## show()

Displays rows.

```python
df.show()
```

---

## printSchema()

Displays schema.

```python
df.printSchema()
```

---

## select()

Select specific columns.

```python
df.select("name", "salary")
```

---

## filter()

Filter rows based on conditions.

```python
df.filter(df.salary > 5000)
```

---

## withColumn()

Create or modify columns.

```python
from pyspark.sql.functions import col

df.withColumn("bonus", col("salary") * 0.10)
```

---

## drop()

Remove columns.

```python
df.drop("bonus")
```

---

# Coding Problems

# P1 — Filter Employees with Salary > X

## Objective

Filter employees earning more than a certain salary.

## Concepts Used

- filter()
- conditions

## Example

```python
df.filter(df.salary > 5000).show()
```

---

# P2 — Add New Column Using Conditions

## Objective

Create conditional columns using business logic.

## Concepts Used

- withColumn()
- when()
- otherwise()

## Example

```python
from pyspark.sql.functions import when

df = df.withColumn(
    "status",
    when(df.salary > 5000, "High")
    .otherwise("Low")
)
```

---

# P3 — Read CSV → Clean Nulls → Save Output

## Steps

### Step 1 — Read CSV

```python
df = spark.read.csv("products.csv", header=True, inferSchema=True)
```

### Step 2 — Remove Nulls

```python
df_clean = df.dropna()
```

### Step 3 — Save Output

```python
df_clean.write.csv("clean_output", header=True)
```

---

# Practice Problems Completed

# Q4 — Sort Products by Price

## Problem

Display all products sorted from highest price to lowest price.

## Solution

```python
df.orderBy(df.price.desc()).show()
```

## Concepts

- sorting
- orderBy()

---

# Q5 — Create Inventory Value Column

## Problem

Create a new column:

```text
inventory_value = quantity * price
```

## Solution

```python
from pyspark.sql.functions import col

df = df.withColumn(
    "inventory_value",
    col("quantity") * col("price")
)
```

## Concepts

- withColumn()
- arithmetic operations

---

# Q6 — Discounted Price Column

## Problem

Create discounted price based on category.

Rules:

- Electronics → 15% discount
- Clothing → 10% discount
- Books → 5% discount
- Others → 8% discount

## Solution

```python
from pyspark.sql.functions import when

df = df.withColumn(
    "discounted_price",
    when(df.category == "Electronics", df.price * 0.85)
    .when(df.category == "Clothing", df.price * 0.90)
    .when(df.category == "Books", df.price * 0.95)
    .otherwise(df.price * 0.92)
)
```

## Concepts

- conditional columns
- when()
- otherwise()

---

# Q7 — Stock Status Column

## Problem

Create stock status column.

Conditions:

- quantity < 10 → Low Stock
- quantity between 10 and 30 → Medium Stock
- quantity > 30 → High Stock

## Solution

```python
from pyspark.sql.functions import when

df = df.withColumn(
    "stock_status",
    when(df.quantity < 10, "Low Stock")
    .when((df.quantity >= 10) & (df.quantity <= 30), "Medium Stock")
    .otherwise("High Stock")
)
```

## Concepts

- conditional transformations
- multiple conditions

---

# Q8 — Convert Data Types

## Problem

Convert:

- quantity → IntegerType
- price → DoubleType

Then print schema.

## Solution

```python
from pyspark.sql.types import IntegerType, DoubleType

df = df.withColumn("quantity", df.quantity.cast(IntegerType()))
df = df.withColumn("price", df.price.cast(DoubleType()))

df.printSchema()
```

## Concepts

- cast()
- printSchema()

---

# Q9 — Filter High Inventory Value

## Problem

Show products where:

```text
quantity * price > 5000
```

## Solution

```python
df.filter((df.quantity * df.price) > 5000).show()
```

## Concepts

- filtering
- derived columns
- expressions

---

# Q10 — Top 5 Most Expensive Products

## Problem

Display top 5 products with highest price.

## Solution

```python
df.orderBy(df.price.desc()).limit(5).show()
```

## Concepts

- sorting
- limiting rows

---

# Q11 — Category Analytics

## Problem

For each category find:

- total number of products
- average price
- total inventory value

## Solution

```python
from pyspark.sql.functions import sum, avg, count

df.groupBy("category").agg(
    count("*").alias("total_products"),
    avg("price").alias("avg_price"),
    sum(df.quantity * df.price).alias("total_inventory")
).show()
```

## Concepts

- groupBy()
- aggregations
- count()
- avg()
- sum()

---

# Q12 — Clean Null Values

## Problem

Replace:

- null quantity → 0
- null price → average price

## Solution

```python
from pyspark.sql.functions import avg

avg_price = df.select(avg("price")).collect()[0][0]

df_clean = df.fillna({"quantity": 0})
df_clean = df_clean.fillna({"price": avg_price})
```

## Explanation

`collect()` returns rows as a list.

Example:

```python
[Row(avg(price)=123.45)]
```

- First `[0]` → first row
- Second `[0]` → first column value

Final result:

```python
123.45
```

---

# Important Day 1 Concepts

# Transformations

Operations that create new DataFrames.

Examples:

- select()
- filter()
- withColumn()

Transformations are lazily evaluated.

Spark does not execute them immediately.

---

# Actions

Operations that trigger execution.

Examples:

- show()
- collect()
- count()

Actions force Spark to execute transformations.

---

# Environment Setup Commands

## Activate Virtual Environment

```bash
source .pyspark-env/bin/activate
```

---

## Start JupyterLab

```bash
jupyter lab
```

---

## Stop JupyterLab

Press:

```text
CTRL + C
```

Then type:

```text
y
```

---

## Exit Virtual Environment

```bash
deactivate
```

---

# Useful Notes

## Why Files Appeared in VS Code Repo

Your `.ipynb` and `.md` files were saved into the same repository because:

- VS Code was opened inside that repo folder
- JupyterLab was launched from the same terminal directory

Example:

```bash
cd ~/Desktop/Data-engineering/Spark
jupyter lab
```

Jupyter saves notebooks relative to the folder where it was launched.

So both:

- `.md`
- `.ipynb`

were automatically created inside the same repo/project folder.

---

# Java + Spark Setup Notes

## Check Java Version

```bash
java -version
```

Expected:

```text
openjdk version "17.x.x"
```

---

## Activate PySpark Environment

```bash
source .pyspark-env/bin/activate
```

---

## Start JupyterLab

```bash
jupyter lab
```

---

## Important

You do NOT need to reinstall Java every day.

Once Java 17 is correctly installed and configured, you only need:

```bash
source .pyspark-env/bin/activate
```

and

```bash
jupyter lab
```

to start working again.