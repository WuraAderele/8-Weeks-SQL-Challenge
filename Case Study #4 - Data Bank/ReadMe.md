# 🏦 Case Study #4 - Data Bank 

<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-designs/4.png" width = "400" height = "400">

##  📕 Table Of Contents
* ### 📝 Background & 🛠️ Problem Statement
* ### 📂 Dataset
* ### 🧙‍♂️ Case Study Questions & 🚀 Solutions

## 📝 Background & 🛠️ Problem Statement
There is a new innovation in the financial industry called Neo-Banks: new aged digital only banks without physical branches.

Danny thought that there should be some sort of intersection between these new age banks, cryptocurrency and the data world…so he decides to launch a new initiative - Data Bank!

Data Bank runs just like any other digital bank - but it isn’t only for banking activities, they also have the world’s most secure distributed data storage platform!

Customers are allocated cloud data storage limits which are directly linked to how much money they have in their accounts. There are a few interesting caveats that go with this business model, and this is where the Data Bank team need your help!

The management team at Data Bank want to increase their total customer base - but also need some help tracking just how much data storage their customers will need.

This case study is all about calculating metrics, growth and helping the business analyse their data in a smart way to better forecast and plan for their future developments!

## 📂 Dataset
The Data Bank team have prepared a data model for this case study as well as a few example rows from the complete dataset below to get you familiar with their tables.

<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-4-erd.png" width = "600" height = "300">

From studying the entity relationship diagram above and info provided regarding the tables, we understand the below:

* The *regions* table contains the details of where bank nodes (branches/stores) exist around the world.
* The *customer_nodes* table contains the details of customers and how they are randomly distributed across various nodes in different regions.
* The *customer_transactions* contains info about customer deposits, withdrawals. or purchases. 

### A. Customer Nodes Exploration

**1. How many unique nodes are there on the Data Bank system?**
This requires a simple query that will use the DISTINCT keyword to count the number of nodes in the *customer_nodes* table.

      SELECT
      	COUNT(DISTINCT(node_id)) AS NumofNodes
      FROM customer_nodes;

**2. What is the number of nodes per region?**

      SELECT
      	region_id,
      	COUNT(DISTINCT(node_id)) AS NumofNodes
      FROM customer_nodes
      GROUP BY region_id
      ORDER BY region_id;

**3. How many customers are allocated to each region?**

      SELECT
      	region_name,
      	COUNT(DISTINCT(customer_id)) AS NumofCustomers
      FROM customer_nodes
      JOIN regions
      ON regions.region_id = customer_nodes.region_id
      GROUP BY region_name
      ORDER BY region_name;

**4. How many days on average are customers reallocated to a different node?**

To calculate this, we first need to know how many days a customer has spent in each 

        WITH DaysInNode AS(
        	SELECT
          		customer_id,
          		node_id,
          		(end_date - start_date) AS NumofDays
          	FROM customer_nodes
          	WHERE end_date != '9999-12-31'
        ),
        
        TotalDaysInNode AS(
        	SELECT 
              customer_id,
              node_id,
          	  SUM(NumofDays) AS TotalDays
          	FROM DaysInNode
          	GROUP BY customer_id, node_id
        )
        
        SELECT 
        	ROUND(AVG(TotalDays), 0) AS AvgDaysInNode
        FROM TotalDaysInNode;


**5. What is the median, 80th and 95th percentile for this same reallocation days metric for each region?**


### B. Customer Transactions

**1. What is the unique count and total amount for each transaction type?**

      SELECT
            txn_type,
            COUNT(customer_id) AS TotalTxns,
            SUM(txn_amount) AS TotalAmount
       FROM customer_transactions
       GROUP BY txn_type;
    
**2. What is the average total historical deposit counts and amounts for all customers?**

      WITH DepositTxns AS(
      	SELECT
        		customer_id,
        		COUNT(customer_id) AS NumofDeposits,
        		AVG(txn_amount) AS AvgDeposits
        	FROM customer_transactions
        	WHERE txn_type = 'deposit' 
        	GROUP BY customer_id
      )
      
      SELECT
      	ROUND(AVG(NumofDeposits),0),
          ROUND(AVG(AvgDeposits),0)
       FROM DepositTxns

**3. For each month - how many Data Bank customers make more than 1 deposit and either 1 purchase or 1 withdrawal in a single month?**

      WITH CountTxnTypes AS (
      	SELECT
        		customer_id,
        		MONTHNAME(txn_date) AS Month,
        		SUM(CASE WHEN txn_type = 'deposit' THEN 1 ELSE 0 END) AS NumofDeposits,
        		SUM(CASE WHEN txn_type = 'purchase' THEN 1 ELSE 0 END) AS NumofPurchases,
        		SUM(CASE WHEN txn_type = 'withdrawal' THEN 1 ELSE 0 END) AS NumofWithdrawals
        	FROM customer_transactions
        	GROUP BY customer_id, Month
      )
      
      SELECT
      	Month,
          COUNT(customer_id) AS NumofCustomers
      FROM CountTxnTypes
      WHERE NumofDeposits > 1 AND (NumofPurchases >= 1 OR NumofWithdrawals >= 1)
      GROUP BY Month;
      
**4. What is the closing balance for each customer at the end of the month?**

**5. What is the percentage of customers who increase their closing balance by more than 5%?**

### C. Data Allocation Challenge
To test out a few different hypotheses - the Data Bank team wants to run an experiment where different groups of customers would be allocated data using 3 different options:

Option 1: data is allocated based off the amount of money at the end of the previous month
Option 2: data is allocated on the average amount of money kept in the account in the previous 30 days
Option 3: data is updated real-time
For this multi-part challenge question - you have been requested to generate the following data elements to help the Data Bank team estimate how much data will need to be provisioned for each option:

running customer balance column that includes the impact each transaction
customer balance at the end of each month
minimum, average and maximum values of the running balance for each customer
Using all of the data available - how much data would have been required for each option on a monthly basis?

