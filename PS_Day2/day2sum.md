# Day 2 — Transformations and Aggregations

## Reference Links

### Concept Videos

#### PySpark Transformations vs Actions

* https://www.youtube.com/results?search_query=TechTFQ+Spark+Transformations+and+Actions
* https://www.youtube.com/results?search_query=Data+with+Danny+PySpark+Transformations+Actions

#### Lazy Evaluation in Spark

* https://www.youtube.com/results?search_query=Spark+Lazy+Evaluation+Explained

#### PySpark Aggregations

* https://www.youtube.com/results?search_query=PySpark+groupBy+aggregations+Simplilearn
* https://www.youtube.com/results?search_query=PySpark+aggregations+TechBrothersIT

---

## Practice Problem Links

### groupBy(), agg(), count(), avg(), sum()

* https://sparkbyexamples.com/pyspark/pyspark-groupby-explained-with-example/
* https://sparkbyexamples.com/pyspark/pyspark-aggregate-functions/
* https://www.geeksforgeeks.org/pyspark-groupby-dataframe-with-aggregation-or-count/

---

### distinct()

* https://sparkbyexamples.com/pyspark/pyspark-distinct-to-drop-duplicates/
* https://www.geeksforgeeks.org/how-to-find-distinct-values-of-multiple-columns-in-pyspark/

---

### Aggregation Practice Questions

* https://www.interviewquestionspdf.com/2023/08/pyspark-dataframes-practice-questions.html
* https://ashishcoder.com/courses/pyspark/groupby-in-pyspark.html

---

# Topics Covered

## 1. Narrow vs Wide Transformations

---

# Narrow Transformations

Data stays within the same partition.

No shuffle occurs.

Faster operations.

Examples:

* select()
* filter()
* withColumn()

---

# Wide Transformations

Data moves between partitions.

Shuffle occurs.

More expensive operations.

Examples:

* groupBy()
* distinct()
* join()
* orderBy()

---

# 2. Lazy Evaluation

Spark does NOT execute immediately.

Instead:

* Spark builds an execution plan
* Execution starts only when an ACTION occurs

Example:

```python
df.filter(df.sales > 1000)
```

No execution yet.

Execution happens after:

```python
df.show()
```

---

# 3. Transformations vs Actions

| Transformations  | Actions           |
| ---------------- | ----------------- |
| Return DataFrame | Return result     |
| Lazy             | Trigger execution |
| Build DAG        | Execute DAG       |

---

# Common Transformations

```python
filter()
select()
groupBy()
withColumn()
orderBy()
```

---

# Common Actions

```python
show()
collect()
count()
write()
```

---

# 4. Aggregation Patterns

Used for:

* summaries
* reports
* analytics

Functions:

* sum()
* avg()
* count()
* max()
* min()

---

# Important Commands

## groupBy()

```python
df.groupBy("category")
```

Groups rows.

---

## agg()

```python
from pyspark.sql.functions import sum, avg

df.groupBy("category").agg(
    sum("sales"),
    avg("sales")
)
```

---

## count()

```python
df.count()
```

---

## distinct()

```python
df.select("category").distinct()
```

---

# Practice Dataset

```python
data = [
    (1, "Electronics", 5000, "North"),
    (2, "Clothing", 2000, "South"),
    (3, "Electronics", 7000, "North"),
    (4, "Books", 1000, "East"),
    (5, "Clothing", 3000, "South"),
    (6, "Books", 1500, "West"),
    (7, "Electronics", 4000, "East"),
    (8, "Sports", 2500, "West"),
    (9, "Sports", 3500, "North"),
    (10, "Clothing", 2800, "South")
]

columns = ["id", "category", "sales", "region"]

sales_df = spark.createDataFrame(data, columns)
```

---

# Coding Problems

# P1 — Top Category by Revenue

### Objective

Find category with highest total sales.

### Concepts Used

* groupBy()
* aggregations
* sorting

### Solution

```python
from pyspark.sql.functions import sum

sales_df.groupBy("category") \
    .agg(sum("sales").alias("total_sales")) \
    .orderBy("total_sales", ascending=False) \
    .show()
```

---

# P2 — Count Duplicate Categories

### Objective

Find frequency of categories.

### Concepts Used

* groupBy()
* count()

### Solution

```python
sales_df.groupBy("category") \
    .count() \
    .show()
```

---

# P3 — Region Wise Sales Summary

### Concepts Used

* groupBy()
* agg()
* avg()
* sum()
* count()

```python
from pyspark.sql.functions import avg, sum, count

sales_df.groupBy("region").agg(
    sum("sales").alias("total_sales"),
    avg("sales").alias("avg_sales"),
    count("*").alias("transactions")
).show()
```

---

# P4 — Distinct Categories

```python
sales_df.select("category").distinct().show()
```

---

# P5 — High Sales Transactions

```python
sales_df.filter(sales_df.sales > 3000).show()
```

---

# P6 — Sort Sales

```python
sales_df.orderBy(sales_df.sales.desc()).show()
```

---

# P7 — Add Bonus Column

```python
from pyspark.sql.functions import col

sales_df = sales_df.withColumn(
    "bonus",
    col("sales") * 0.10
)
```

---

# P8 — Category + Region Analytics

```python
sales_df.groupBy("category", "region").agg(
    sum("sales").alias("total_sales"),
    avg("sales").alias("avg_sales")
).show()
```

---

# P9 — Identify Transformation Types

| Operation    | Type   |
| ------------ | ------ |
| filter()     | Narrow |
| withColumn() | Narrow |
| groupBy()    | Wide   |
| distinct()   | Wide   |
| orderBy()    | Wide   |

---

# P10 — Mini Sales Summary Report

## Steps

### Step 1 — Filter Sales

```python
filtered_df = sales_df.filter(sales_df.sales > 2000)
```

---

### Step 2 — Add Tax Column

```python
filtered_df = filtered_df.withColumn(
    "tax",
    filtered_df.sales * 0.18
)
```

---

### Step 3 — Aggregation

```python
summary_df = filtered_df.groupBy("category").agg(
    sum("sales").alias("total_sales"),
    avg("sales").alias("avg_sales"),
    count("*").alias("transactions")
)
```

---

### Step 4 — Sort Results

```python
summary_df.orderBy("total_sales", ascending=False).show()
```

---

### Step 5 — Save CSV

```python
summary_df.write.csv("sales_summary", header=True)
```

---

# Important Day 2 Concepts

## Transformations

Operations that create new DataFrames.

Examples:

* filter()
* select()
* withColumn()
* groupBy()

---

## Actions

Operations that trigger execution.

Examples:

* show()
* collect()
* count()
* write()

---

# Real-World Importance

These concepts are heavily used in:

* ETL pipelines
* data engineering
* Spark optimization
* analytics dashboards
* interview coding rounds

---

# Most Important Day 2 Commands

```python
groupBy()
agg()
count()
avg()
sum()
distinct()
filter()
orderBy()
withColumn()
collect()
show()
```
