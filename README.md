# Home_Sales 

Instructions

    Rename the Home_Sales_starter_code.ipynb file as Home_Sales.ipynb.

    Import the necessary PySpark SQL functions for this assignment.
# Install Spark and Java
!apt-get update
!apt-get install openjdk-11-jdk-headless -qq > /dev/null
!wget -q https://downloads.apache.org/spark/$SPARK_VERSION/$SPARK_VERSION-bin-hadoop3.tgz
!tar xf $SPARK_VERSION-bin-hadoop3.tgz
!pip install -q findspark

    Read the home_sales_revised.csv data in the starter code into a Spark DataFrame.


# Set Environment Variables
os.environ["JAVA_HOME"] = "/usr/lib/jvm/java-11-openjdk-amd64"
os.environ["SPARK_HOME"] = f"/content/{spark_version}-bin-hadoop3"

# Start a SparkSession
import findspark
findspark.init()
    Create a temporary table called home_sales.

    Answer the following questions using SparkSQL:

        # 3. What is the average price for a four bedroom house sold per year, rounded to two decimal places?
avg_price_4_bedroom = spark.sql("""
    SELECT YEAR(date) AS year, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 4
    GROUP BY year
    ORDER BY year
""")
avg_price_4_bedroom.show()

![firefox_5sMbM8xvC0](https://github.com/user-attachments/assets/e9484c0d-4f7e-444d-a66a-77dca3726d65)





       # 3. What is the average price for a four bedroom house sold per year, rounded to two decimal places?
avg_price_4_bedroom = spark.sql("""
    SELECT YEAR(date) AS year, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 4
    GROUP BY year
    ORDER BY year
""")
avg_price_4_bedroom.show() 

![image](https://github.com/user-attachments/assets/2560c3d9-ed2a-4f3c-ad4c-5e0e43358d02)


        # 4. What is the average price of a home for each year the home was built,
        # that have 3 bedrooms and 3 bathrooms, rounded to two decimal places?
avg_price_3_bed_3_bath = spark.sql("""
    SELECT date_built, ROUND(AVG(price), 2) AS avg_price
    FROM home_sales
    WHERE bedrooms = 3 AND bathrooms = 3
    GROUP BY date_built
    """)
avg_price_3_bed_3_bath.show()

![image](https://github.com/user-attachments/assets/1ffb0602-42bc-43df-8899-f7727d3aa3d1)


    # 6. What is the average price of a home per "view" rating, rounded to two decimal places,
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

    # 7. Cache the the temporary table home_sales.
spark.catalog.cacheTable("home_sales")

    # 8. Check if the table is cached.
spark.catalog.isCached('home_sales')
 
    Using the cached data, run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.

    # 10. Partition by the "date_built" field on the formatted parquet home sales data
df.write.partitionBy("date_built").mode("overwrite").parquet("home_sales_partitioned")

    # 11. Read the parquet formatted data.
parquet_df = spark.read.parquet("home_sales_partitioned")
    
    # 12. Create a temporary table for the parquet data.
parquet_df.createOrReplaceTempView("home_sales_parquet")

    

    Run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.




    # 14. Uncache the home_sales temporary table.
spark.catalog.uncacheTable("home_sales")    

    # 15. Check if the home_sales is no longer cached
spark.catalog.isCached("home_sales")

![firefox_KhUjgdvCB9](https://github.com/user-attachments/assets/899ef3b9-e1eb-4864-8111-f08f091a6b45)



    
