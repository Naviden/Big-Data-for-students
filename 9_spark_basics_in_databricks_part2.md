
# Lesson 9: Practicing Spark Basics in Databricks (Part 2)

In this lesson, we’ll continue exploring Apache Spark in Databricks by diving into more advanced Spark operations such as **joins**, **aggregations**, and **window functions**. We will also compare working with **Spark DataFrames** and **RDDs**.

---

## 1. Advanced Spark Operations

### 1.1 Joins in Spark

Joins are essential when working with multiple datasets. Spark supports different types of joins, including inner, left, right, and full outer joins.

#### Example: Inner Join
```
# Load two datasets
df_customers = spark.read.csv("/databricks-datasets/samples/customers/customers.csv", header=True, inferSchema=True)
df_orders = spark.read.csv("/databricks-datasets/samples/orders/orders.csv", header=True, inferSchema=True)
```
```
# Perform an inner join on customer_id
joined_df = df_customers.join(df_orders, df_customers["customer_id"] == df_orders["customer_id"], "inner")
joined_df.show()
```

You can use other types of joins such as `left`, `right`, or `full` by replacing `"inner"` in the `join` method.

---

### 1.2 Aggregations in Spark

Aggregations allow you to compute summaries of your data, such as sums, averages, or counts.

#### Example: Grouping and Aggregating
```
# Group by customer_id and calculate the total order amount
agg_df = df_orders.groupBy("customer_id").agg({"order_amount": "sum"})
agg_df.show()
```

In this example, we grouped the orders by `customer_id` and calculated the total order amount for each customer.

---

### 1.3 Window Functions in Spark

Window functions enable you to perform calculations across a set of rows related to the current row. They’re often used for running totals, rankings, or sliding averages.

#### Example: Window Function for Running Total
```
from pyspark.sql.window import Window
from pyspark.sql.functions import row_number, col

# Define a window specification
window_spec = Window.partitionBy("customer_id").orderBy("order_date")

# Add a running total column
df_with_rank = df_orders.withColumn("rank", row_number().over(window_spec))
df_with_rank.show()
```

This example calculates the ranking of each order per customer based on the order date.

---

## 2. Working with Spark DataFrames and RDDs

### 2.1 Spark DataFrames

DataFrames are the most commonly used data structure in Spark and offer a higher-level API for working with structured data. They provide better performance optimizations and integrations with Spark SQL.

#### Key Features:
- Schema-aware: DataFrames have a schema, similar to a table in a relational database.
- Optimized: DataFrames are optimized using Spark’s **Catalyst** engine for query optimization.

#### Example:
```
# Basic DataFrame operation
df_customers.select("customer_id", "name").show()
```

---

### 2.2 Spark RDDs (Resilient Distributed Datasets)

RDDs are the underlying distributed data structure in Spark and provide more control over data manipulation but are less optimized than DataFrames.

#### Key Features:
- Immutable and distributed: RDDs are split into partitions across a cluster.
- Low-level API: Provides fine-grained control but requires more manual optimization.

#### Example:
```
# Create an RDD from a list
rdd = spark.sparkContext.parallelize([("John", 35), ("Alice", 29), ("Bob", 42)])

# Perform a map operation on the RDD
rdd_mapped = rdd.map(lambda x: (x[0], x[1] + 10))  # Add 10 to each age
rdd_mapped.collect()
```

---

## 3. When to Use DataFrames vs. RDDs

### DataFrames:
- Use DataFrames when working with structured data, especially when you want to leverage Spark’s optimizations and query engine.
  
### RDDs:
- Use RDDs for unstructured data or when you need more control over data processing and transformations. They’re also useful for certain low-level operations where DataFrames may not provide the necessary flexibility.

---

## Conclusion

In this part, we explored advanced Spark operations such as **joins**, **aggregations**, and **window functions**. We also compared working with **Spark DataFrames** and **RDDs** and discussed when to use each. Understanding these advanced operations is key to leveraging the full power of Spark for large-scale data processing.
