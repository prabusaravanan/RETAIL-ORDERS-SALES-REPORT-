Retail Orders Sales Report
Project Overview
          This project focuses on analyzing sales data from a retail store, processing order details, and generating insightful  visualizations using Python. The dataset contains order information such as prices, discounts, shipping modes, and sales profit.
Technologies Used
    •	Python (pandas, numpy, matplotlib, seaborn)
    •	MySQL (for database connectivity)
    •	Data Visualization Libraries
    •	Jupyter Notebook (for execution)
Data Loading and Preprocessing
    1.	Loading the dataset
    2.	import numpy as np
    3.	import pandas as pd
    4.	import matplotlib.pyplot as plt
    5.	import seaborn as sns
    6.	%matplotlib inline
    7.	test_df = pd.read_csv("orders.csv", na_values=["Not Available", "unknown"])
    8.  The dataset is read from a CSV file.
    9.  Missing values labeled as "Not Available" or "unknown" are treated as NaN.
Column Formatting:
    9.	test_df.columns = test_df.columns.str.lower()
    10. test_df.columns = test_df.columns.str.replace(" ", "_")
            o Column names are converted to lowercase.
            o	Spaces in column names are replaced with underscores for easier access.
Data Cleaning and Transformation
    1.	Calculating Discount, Sale Price, and Profit:
    2.	test_df["discount"] = test_df["list_price"] * test_df["discount_percent"] * 0.01
    3.	test_df["sale_price"] = test_df["list_price"] - test_df["discount"]
    4.  test_df["profit"] = test_df["sale_price"] - test_df["cost_price"]
        o	The discount is calculated using the discount percentage.
        o	The sale price is determined by subtracting the discount from the list price.
        o	Profit is obtained by subtracting the cost price from the sale price.
4.	Date Formatting:
    test_df["order_date"] = pd.to_datetime(test_df["order_date"], format="%Y-%m-%d")
        o	Ensures the order_date column is in datetime format
5.	test_df.drop(columns=["list_price", "cost_price", "discount_percent"], inplace=True, errors='ignore')
    test_df.dropna(subset=['ship_mode'], inplace=True)
        o	Drops unnecessary columns.
        o	Removes rows where the ship_mode is missing.
Database Connection (MySQL)
    1.	Connecting to MySQL Database:
    2.	import pymysql
    3.	conection = pymysql.connect(host='localhost', user='root', password='sankarprabu9894', database='prabu')
  mycursor = conection.cursor()
      o	Establishes a connection to the MySQL database.
      o	Verifies the connection.
Checking Available Databases:
   1.mycursor.execute('SHOW DATABASES')
   2.for x in mycursor:
    print(x)
      o	Lists all available databases.
Data Analysis and Visualization
  1.  State-wise Order Count:
  2.	plt.figure(figsize=(12, 6))  
  3.	sns.catplot(x="state", kind="count", data=test_df, height=6, aspect=2)
  4.	plt.tight_layout()
      plt.show()
      o	Displays the distribution of orders across different states.
Top 5 States by Order Count (Pie Chart):
    6.	top_states = test_df["state"].value_counts().nlargest(5)
    7.	plt.figure(figsize=(5, 6))
    8.	plt.pie(top_states, labels=top_states.index, autopct='%1.1f%%', startangle=180, colors=['skyblue', 'orange',    	'green', 'red', 'purple'])
    9.	plt.title("Top 5 States by Order Count")
        plt.show()
        o	Highlights the top 5 states with the highest number of orders.
Profit Distribution by State (Top 10):
    1.	state_profit = test_df.groupby("state")["profit"].sum()
    2.	top_states_profit = state_profit.nlargest(10)
    3.	plt.figure(figsize=(8, 8))
    4.	plt.pie(top_states_profit, labels=top_states_profit.index, autopct='%1.1f%%', startangle=140,   
    colors=plt.cm.Paired.colors)
    5.	plt.title("Profit Distribution by State (Top 10)")
        plt.show()
            o	Displays the distribution of profit among the top 10 states.
Bottom 10 States by Profit:
    1.	bottom_10_states = test_df.groupby("state")["profit"].sum().nsmallest(10)
    2.	plt.figure(figsize=(10, 4))
    3.	sns.barplot(x=bottom_10_states.index, y=bottom_10_states.values, palette="Reds_r")
    4.	plt.xlabel("State")
    5.	plt.ylabel("Total Profit")
    6.	plt.title("Bottom 10 States by Profit")
    7.	plt.xticks(rotation=10, ha="right")
    8.	plt.tight_layout()
        plt.show()
          o	Displays the states with the lowest profits using a bar chart.
Top 10 Cities by Quantity Ordered:
    1.	city_quantity = test_df.groupby("city")["quantity"].sum().nlargest(10)
    2.	plt.figure(figsize=(12, 6))
    3.	sns.barplot(x=city_quantity.index, y=city_quantity.values, palette="Blues_r")
    4.	plt.xlabel("City")
    5.	plt.ylabel("Total Quantity")
    6.	plt.title("Top 10 Cities by Quantity Ordered")
    7.	plt.xticks(rotation=50, ha="right")
    8.	plt.tight_layout()
        plt.show()
          o	Shows the top 10 cities with the highest product quantities ordered.

Conclusion
      This project successfully loads, cleans, and processes retail sales data while providing insightful visualizations.
      The analysis highlights sales trends across different states and cities, identifying key regions with high and low
      performance in terms of orders and profits. The integration with MySQL also ensures that the data can be stored and
      retrieved efficiently for further analysis.
 
Project Overview
      This project involves analyzing retail sales data, storing it in a MySQL database, and running queries to extract
      meaningful insights. The dataset contains order details such as order ID, date, shipping mode, product details, sales
      price, and profit.
Technologies Used
    •	MySQL (for data storage and querying)
    •	Python (for loading data into MySQL)
    •	Data Analysis using SQL queries
MySQL Database Setup
    1.	Create and Use Database:
    2.	SHOW DATABASES;
    3.	CREATE DATABASE prabu;
USE prabu;
    o	Lists existing databases.
    o	Creates a new database named prabu.
    o	Selects prabu for further operations.
Create orders Table:
5.	CREATE TABLE orders (
6.	    order_id VARCHAR(50) PRIMARY KEY,      
7.	    order_date DATE NOT NULL,              
8.	    ship_mode VARCHAR(50),                 
9.	    segment VARCHAR(50),                   
10.	    country VARCHAR(100),                  
11.	    city VARCHAR(100),                    
12.	    state VARCHAR(100),                    
13.	    postal_code VARCHAR(20),            
14.	    region VARCHAR(50),                  
15.	    category VARCHAR(50),                  
16.	    sub_category VARCHAR(50),              
17.	    product_id VARCHAR(50),               
18.	    quantity INT NOT NULL,                
19.	    discount DECIMAL(5,2) DEFAULT 0,       
20.	    sale_price DECIMAL(10,2) NOT NULL,     
21.	    profit DECIMAL(10,2)                   
);
      o	Creates a structured table with relevant fields.
Verify Table Creation:
  DESCRIBE orders;
    SELECT * FROM orders LIMIT 10;
        o	Displays table structure.
        o	Fetches a sample of 10 records.
        
Data Validation and Queries
    1.	Check for Missing Values in ship_mode:
        SELECT COUNT(*) FROM orders WHERE ship_mode IS NULL;
            o	Counts the number of records where ship_mode is missing.
    2.	Retrieve Sample Orders:
        SELECT order_id, order_date, city FROM orders LIMIT 100;
            o	Fetches 100 sample orders displaying order ID, date, and city.
Business Insights Using SQL Queries
    1.	Top 10 Revenue-Generating Products:
    2.	SELECT product_id, SUM(sale_price) AS sales
    3.	FROM orders
    4.	GROUP BY product_id
    5.	ORDER BY sales DESC
        LIMIT 10;
          o	Identifies the top 10 products by total sales revenue.
Top 10 Products with Highest Sales by City and State:
    SELECT product_id, city, state, SUM(sale_price) AS sales
	  FROM orders
	  GROUP BY product_id, city, state
	  ORDER BY sales DESC
    LIMIT 10;
        o	Shows the highest revenue-generating products by city and state.
Total Profit by Product Category:
	  SELECT category, SUM(profit) AS total_profit 
	  FROM orders 
    GROUP BY category;
        o	Highlights profit distribution across different regions
Overall Total Profit:
    SELECT SUM(profit) AS total_profit FROM orders;
        o	Computes total profit across all orders.
Top 10 Cities with Most Orders:
    SELECT city, state, COUNT(order_id) AS total_orders
    FROM orders
    GROUP BY city, state
    ORDER BY total_orders DESC
    LIMIT 10;
        o	Identifies cities with the highest number of orders.
Conclusion
        This project successfully integrates retail sales data into MySQL, enabling efficient storage, retrieval, and
        analysis. Using SQL queries, we gain valuable insights into sales trends, revenue distribution, and profitability
        across different regions, cities, and product categories. These insights can help businesses optimize their
        operations and target high-performing areas effectively

        
        
     
       
         
    


            
           
           
          
        


       




    
