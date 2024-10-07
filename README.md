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

        What is the average price for a four-bedroom house sold for each year? Round off your answer to two decimal places.

        

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


 


        

        What is the average price of a home per "view" rating having an average home price greater than or equal to $350,000? Determine the run time for this query, and round off your answer to two decimal places.

    Cache your temporary table home_sales.

    Check if your temporary table is cached.

    Using the cached data, run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.

    Partition by the "date_built" field on the formatted parquet home sales data.

    Create a temporary table for the parquet data.

    Run the last query that calculates the average price of a home per "view" rating having an average home price greater than or equal to $350,000. Determine the runtime and compare it to uncached runtime.

    Uncache the home_sales temporary table.

    Verify that the home_sales temporary table is uncached using PySpark.

    Download your Home_Sales.ipynb file and upload it into your "Home_Sales" GitHub repository.
