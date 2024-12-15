# Exploratory Data Analysis (EDA) on Netflix TV Shows and Movies Dataset using Apache Spark

## Table of Contents
1. [Running Spark on Your Computer](#running-spark-on-your-computer)
2. [Loading the Dataset in Spark Shell](#loading-the-dataset-in-spark-shell)
3. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
4. [Visualization](#visualization)

---

### Running Spark on Your Computer

1. **Install Java:**
   Ensure Java is installed on your system by checking the version in the command prompt:
   ```
   java -version
   ```
   If not installed, download it [here](https://www.java.com/download/ie_manual.jsp).

2. **Set Up Spark:**
   - Download the Spark Starter Kit.
   - Extract the Spark Starter Kit to the `C:\` drive.
   - Navigate to the Spark directory in the command prompt:
     ```
     cd C:\spark-windows
     cd spark-2.1.1
     cd bin
     ```
   - Launch the Spark Shell:
     ```
     spark-shell
     ```

---

### Loading the Dataset in Spark Shell

1. **Import Necessary Libraries:**
   ```scala
   import org.apache.spark.sql.SparkSession
   ```

2. **Initialize SparkSession:**
   ```scala
   val spark = SparkSession.builder()
     .appName("NetflixTitles")
     .master("local[*]")
     .getOrCreate()
   ```

3. **Load the Dataset:**
   ```scala
   val titlesDF = spark.read
     .option("header", "true")  // Assumes the file has a header
     .option("inferSchema", "true")  // Automatically infer column types
     .csv("C:/data/Netflix_Titles.csv")
   ```

4. **Verify Dataset Load:**
   Display the first 10 rows:
   ```scala
   titlesDF.show(10)
   ```

---

### Exploratory Data Analysis (EDA)

1. **Check the Schema:**
   ```scala
   titlesDF.printSchema()
   ```

2. **Basic Statistics:**
   ```scala
   titlesDF.describe().show()
   ```

3. **Finding Missing Values:**
   ```scala
   import org.apache.spark.sql.functions._
   titlesDF.select(titlesDF.columns.map(c => sum(col(c).isNull.cast("int")).alias(c)): _*).show()
   ```

4. **Distinct Titles:**
   ```scala
   titlesDF.select("title").distinct().show()
   titlesDF.groupBy("title").count().show()
   ```

5. **Average Rating by Title:**
   ```scala
   titlesDF.groupBy("title")
           .agg(avg("rating").alias("Avg_Rating"))
           .orderBy("title")
           .show()
   ```

6. **Maximum and Minimum Ratings per Title:**
   ```scala
   titlesDF.groupBy("title")
           .agg(
             max("rating").alias("Max_Rating"),
             min("rating").alias("Min_Rating")
           )
           .orderBy("title")
           .show()
   ```

7. **Distribution of Ratings:**
   ```scala
   titlesDF.groupBy("rating").count().orderBy("rating").show()
   ```

8. **Filter Rows:**
   - Titles with ratings above a threshold:
     ```scala
     titlesDF.filter("rating > 8.5").show()
     ```
   - Top-rated titles:
     ```scala
     titlesDF.orderBy(desc("rating")).show()
     ```

9. **Create a New Column:**
   ```scala
   titlesDF.withColumn("Adjusted_Rating", col("rating") * 1.1).show()
   ```

---

### Visualization

1. **Save the Processed Dataset:**
   ```scala
   titlesDF.write.option("header", "true").csv("C:/Processed_Netflix_Titles.csv")
   ```

---

This guide outlines how to perform EDA on the Netflix TV Shows and Movies dataset using Apache Spark.

