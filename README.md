# Home_Sales 

Instructions

    Rename the Home_Sales_starter_code.ipynb file as Home_Sales.ipynb.

    Import the necessary PySpark SQL functions for this assignment.

    # Set Environment Variables
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = f"/content/{spark_version}-bin-hadoop3"

    # Start a SparkSession
import findspark
findspark.init()
    Create a temporary table called home_sales.
    
    # Install Spark and Java
!apt-get update
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop3.tgz
!tar xf $SPARK_VERSION-bin-hadoop3.tgz
!pip install -q findspark

    Read the home_sales_revised.csv data in the starter code into a Spark DataFrame.

    # Read in the AWS S3 bucket into a DataFrame.
from pyspark import SparkFiles
url = "https://2u-data-curriculum-team.s3.amazonaws.com/dataviz-classroom/v1.2/22-big-data/home_sales_revised.csv"

    # Create a temporary view of the DataFrame.
    # Add the file to SparkFiles
    
spark.sparkContext.addFile(url)

    # Read the CSV file into a DataFrame
    
df = spark.read.csv(SparkFiles.get("home_sales_revised.csv"), header=True, inferSchema=True)

df.createOrReplaceTempView("home_sales")

    # Verify that the view has been created by running a basic query
    
spark.sql("SELECT * FROM home_sales LIMIT 5").show()



![image](https://github.com/user-attachments/assets/fc9a73fb-486e-4dc0-abb8-94102bf9a7f8)




    Answer the following questions using SparkSQL:

        # What is the average price for a four bedroom house sold per year, rounded to two decimal places?
avg_price_4_bedroom = spark.sql("""
    SELECT YEAR(date) AS year, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 4
    GROUP BY year
    ORDER BY year
""")
avg_price_4_bedroom.show()

![firefox_5sMbM8xvC0](https://github.com/user-attachments/assets/e9484c0d-4f7e-444d-a66a-77dca3726d65)




        # What is the average price of a home for each year the home was built,
         that have 3 bedrooms and 3 bathrooms, rounded to two decimal places?    

avg_price_3_bed_3_bath = spark.sql("""
    SELECT date_built, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 3 AND bathrooms = 3
    GROUP BY date_built
    """)
avg_price_3_bed_3_bath.show()


![image](https://github.com/user-attachments/assets/773292cc-d920-4589-8d51-daf34d08c5a6)




    #  What is the average price of a home for each year the home was built,
    #  that have 3 bedrooms, 3 bathrooms, with two floors,
    #  and are greater than or equal to 2,000 square feet, rounded to two decimal places?


avg_price_3_bed_3_bath = spark.sql("""
    SELECT date_built, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 3 AND bathrooms = 3
    GROUP BY date_built
    """)
avg_price_3_bed_3_bath.show()

![image](https://github.com/user-attachments/assets/55f3e85c-920e-4b71-8cfa-74d8fbb55ec4)



    # What is the average price of a home per "view" rating, rounded to two decimal places,
    # having an average home price greater than or equal to $350,000? Order by descending view rating.
    # Although this is a small dataset, determine the run time for this query.

avg_price_view = spark.sql("""
    SELECT view, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    GROUP BY view
    HAVING AVG(price) >= 350000
    ORDER BY view DESC
    """)
avg_price_view.show()
start_time = time.time()

print("--- %s seconds ---" % (time.time() - start_time)) 

![firefox_o7qXWXY9p4](https://github.com/user-attachments/assets/b913a4a8-2543-4ad0-8d19-c90446ca3ea4)

  
    # Cache the the temporary table home_sales.
spark.catalog.cacheTable("home_sales")


    # Check if the table is cached.
spark.catalog.isCached('home_sales')

 
![image](https://github.com/user-attachments/assets/81a00161-68cc-40b4-a6f4-73d04a462b00)

 
  
    # Using the cached data, run the last query that calculates the average price of a home per "view" rating having an average home price greater            than or equal to $350,000. Determine the runtime and compare it to uncached runtime.
avg_price_view = spark.sql("""
    SELECT view, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    GROUP BY view
    HAVING AVG(price) >= 350000
    ORDER BY view DESC
    """)

avg_price_view.show()
start_time = time.time()



print("--- %s seconds ---" % (time.time() - start_time))

 
![image](https://github.com/user-attachments/assets/9e0cdf6a-75e5-401f-8700-23b5d0e1cc02)




  
    # Partition by the "date_built" field on the formatted parquet home sales data
df.write.partitionBy("date_built").mode("overwrite").parquet("home_sales_partitioned")
    
    # Read the parquet formatted data.
parquet_df = spark.read.parquet("home_sales_partitioned")
    
  
    # Create a temporary table for the parquet data.
parquet_df.createOrReplaceTempView("home_sales_parquet")

    # Using the parquet Dataframe,run the last query that calculates the average price of a home per "view" rating having
     an average home price greater than or equal to $350,000.Determine the runtime and compare it to uncached runtime. 
    

avg_price_view = spark.sql("""
    SELECT view, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales_parquet
    GROUP BY view
    HAVING AVG(price) >= 350000
    ORDER BY view DESC
    """)

avg_price_view.show()
start_time = time.time()



print("--- %s seconds ---" % (time.time() - start_time))  


![image](https://github.com/user-attachments/assets/94d4636b-6847-4bfe-a8d5-070f917d351d)


          
                                  
    # Uncache the home_sales temporary table.
spark.catalog.uncacheTable("home_sales")    

    # Check if the home_sales is no longer cached
spark.catalog.isCached("home_sales")

![firefox_KhUjgdvCB9](https://github.com/user-attachments/assets/899ef3b9-e1eb-4864-8111-f08f091a6b45)

    # Download your Home_Sales.ipynb file and upload it into your "Home_Sales" GitHub repository.

    
