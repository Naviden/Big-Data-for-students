
# Practicing Spark Basics in Databricks (Part 1)

In this lesson, we’ll explore how to get started with **Apache Spark** in **Databricks** and practice some of the fundamental Spark operations, such as filtering, transformations, and actions.

---

## Introduction to Databricks

**Databricks** is a unified analytics platform built on top of Apache Spark. It provides an interactive workspace for data engineers, data scientists, and analysts to collaborate on large-scale data processing projects.

### Key Features of Databricks:
1. **Integrated with Spark**: Databricks is built on Spark, making it a powerful tool for running distributed data processing jobs with minimal setup.
2. **Notebooks**: Databricks provides interactive notebooks that allow you to write code, visualize data, and document your analysis in one place.
3. **Collaborative**: Multiple users can collaborate on the same project, making it ideal for team-based data projects.
4. **Scalable**: Databricks is designed to scale out workloads on cloud-based clusters, allowing users to handle large datasets efficiently.

---

## Setting Up and Running Basic Operations in Spark

To get started with Spark in Databricks, follow these steps:

### Step 1: Create a Databricks Account and Workspace
1. Sign up for a **Databricks** account at [databricks.com](https://databricks.com).
2. Create a new **workspace**.
3. Launch a **notebook** within your workspace.

---

## Basic Operations in Spark

### 1. **Loading Data into Spark**
First, load a dataset into a Spark DataFrame. In Databricks, you can easily load data from various sources such as CSV, Parquet, or Delta formats.

```
# Load a CSV file into a DataFrame
df = spark.read.csv("/path/to/data.csv", header=True, inferSchema=True)
```

### 2. **Filtering Data**
You can filter rows in a DataFrame using the `filter` or `where` function.

```
# Filter rows where 'age' is greater than 30
filtered_df = df.filter(df["age"] > 30)
filtered_df.show()
```

### 3. **Transformations in Spark**
Transformations in Spark are **lazy** operations that define the steps to be performed on the data. These include operations like `filter`, `select`, `groupBy`, etc.

Example: Selecting specific columns and filtering data.

```
# Select specific columns and filter the data
transformed_df = df.select("name", "age").filter(df["age"] > 30)
transformed_df.show()
```

Note: Transformations are not executed until an **action** is called.

### 4. **Actions in Spark**
Actions trigger the execution of the transformations defined. Examples of actions include `show()`, `collect()`, and `count()`.

```
# Count the number of rows after filtering
row_count = transformed_df.count()
print(f"Number of rows: {row_count}")
```

Actions like `count()` or `show()` force the execution of all preceding transformations.

---

## Example: End-to-End Basic Spark Operations

In this example, we’ll load a dataset, perform transformations (filtering, selecting columns), and then execute actions to see the results.

1. **Load a dataset** (e.g., a CSV file of customer data):
```
# Load dataset into a DataFrame
df = spark.read.csv("/databricks-datasets/samples/people/people.csv", header=True, inferSchema=True)
```

2. **Filter rows where age is greater than 30**:
```
# Filter rows
filtered_df = df.filter(df["age"] > 30)
```

3. **Select relevant columns (name and age)**:
```
# Select columns
selected_df = filtered_df.select("name", "age")
selected_df.show()
```

4. **Count the number of rows**:
```
# Count rows
row_count = selected_df.count()
print(f"Total rows: {row_count}")
```