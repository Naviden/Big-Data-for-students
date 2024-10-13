
# Hands-on Project: Building a Spark Pipeline

## Goal:
Build a Spark pipeline that reads a large dataset, processes it (filtering, transformations, aggregations), and generates insights.

## Dataset Example: 
You can use a public dataset, such as the **Airbnb NYC listings** dataset from [Kaggle](https://www.kaggle.com/dgomonov/new-york-city-airbnb-open-data). This dataset contains information about Airbnb listings, including price, location, number of reviews, and availability.

## Steps:

---

### 1. Load the Dataset into Databricks (15 minutes)

1. Download the dataset and upload it to Databricks.
   - Go to the Databricks workspace and create a new notebook.
   - Use Databricks’ file system (`dbfs`) to upload the CSV file.

   ```
   # Example: Load CSV into a Spark DataFrame
   file_path = "/dbfs/FileStore/tables/AB_NYC_2019.csv"
   df = spark.read.csv(file_path, header=True, inferSchema=True)
   ```

---

### 2. Data Exploration and Filtering (30 minutes)

2. Perform some basic exploratory data analysis (EDA) to understand the structure of the dataset.
   - Display the first few rows, schema, and data types.
   - Filter listings to include only those with more than 5 reviews and located in Manhattan.

   ```
   # Show the first few rows and schema
   df.show(5)
   df.printSchema()

   # Filter listings in Manhattan with more than 5 reviews
   filtered_df = df.filter((df.neighbourhood_group == 'Manhattan') & (df.number_of_reviews > 5))
   ```

---

### 3. Data Transformation

3. Perform transformations on the filtered dataset.
   - Create a new column that categorizes listings into price tiers (Low, Medium, High) based on their price.
   - Convert the price column from string to float (if necessary).

   ```
   # Create price tiers: Low (<$100), Medium ($100-$200), High (>$200)
   from pyspark.sql.functions import when

   transformed_df = filtered_df.withColumn("price_tier", 
      when(df.price < 100, "Low")
      .when((df.price >= 100) & (df.price <= 200), "Medium")
      .otherwise("High"))
   ```

---

### 4. Aggregations and Analysis

4. Perform aggregations to gain insights.
   - Find the average price of Airbnb listings per neighborhood in Manhattan.
   - Group the listings by price tier and calculate the average number of reviews for each tier.

   ```
   # Average price per neighborhood in Manhattan
   avg_price_neighbourhood = transformed_df.groupBy("neighbourhood").avg("price")
   avg_price_neighbourhood.show()

   # Average number of reviews per price tier
   avg_reviews_per_tier = transformed_df.groupBy("price_tier").avg("number_of_reviews")
   avg_reviews_per_tier.show()
   ```

---

### 5. Visualizing Results

5. Use Databricks’ built-in visualization tools to plot results.
   - Plot the average price per neighborhood as a bar chart.
   - Plot the average number of reviews per price tier.

   You can visualize directly from the notebook in Databricks:

   ```
   # Convert to Pandas for better visualization
   avg_price_neighbourhood_pandas = avg_price_neighbourhood.toPandas()

   # Plot in Databricks
   display(avg_price_neighbourhood_pandas)
   ```

---

### 6. Final Thoughts and Review

6. Conclude the session by reviewing the steps:
   - Discuss how the dataset was loaded, processed, and transformed using Spark.
   - Highlight key Spark functions and transformations used in the pipeline.
   - Optionally, extend the project by adding new features or asking students to try different datasets or queries.

---

## Learning Outcomes:
- Students will understand how to load and process large datasets using Spark in Databricks.
- They will learn basic Spark transformations (e.g., filtering, grouping, and aggregations).
- They will practice visualizing results within Databricks.
