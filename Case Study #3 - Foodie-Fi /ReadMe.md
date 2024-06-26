# 🥑 Case Study #3 - Foodie-Fi 

<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-designs/3.png" width = "400" height = "400">

##  📕 Table Of Contents
* ### 📝 Background & 🛠️ Problem Statement
* ### 📂 Dataset
* ### 🧙‍♂️ Case Study Questions & 🚀 Solutions

## 📝 Background & 🛠️ Problem Statement
Subscription based businesses are super popular and Danny realised that there was a large gap in the market - he wanted to create a new streaming service that only had food related content - something like Netflix but with only cooking shows!

Danny finds a few smart friends to launch his new startup Foodie-Fi in 2020 and started selling monthly and annual subscriptions, giving their customers unlimited on-demand access to exclusive food videos from around the world!

Danny created Foodie-Fi with a data driven mindset and wanted to ensure all future investment decisions and new features were decided using data. This case study focuses on using subscription style digital data to answer important business questions.
 <p align = "center">
   <img width="500" alt="PR ERD Capture" src="https://user-images.githubusercontent.com/94797745/147705079-71079fdb-e475-48df-8e32-31763dc6b91d.PNG">

## 📂 Dataset
Danny has shared the data design for Foodie-Fi and also short descriptions on each of the database tables as shown below:

<details>
 <summary> Plans Table Structure </summary>
 <p>
   
**Query #1**
    
    DESCRIBE plans;
    
| Field     | Type         | Null | Key | Default | Extra |
| --------- | ------------ | ---- | --- | ------- | ----- |
| plan_id   | int          | YES  |     |         |       |
| plan_name | varchar(13)  | YES  |     |         |       |
| price     | decimal(5,2) | YES  |     |         |       |

Customers can choose which plans to join Foodie-Fi when they first sign up.

* Basic plan customers have limited access and can only stream their videos and is only available monthly at $9.90

* Pro plan customers have no watch time limits and are able to download videos for offline viewing. Pro plans start at $19.90 a month or $199 for an annual subscription.

* Customers can sign up to an initial 7 day free trial will automatically continue with the pro monthly subscription plan unless they cancel, downgrade to basic or upgrade to an annual pro plan at any point during the trial.

* When customers cancel their Foodie-Fi service - they will have a churn plan record with a null price but their plan will continue until the end of the billing period.
 
 </p>
</details>

<details>
 <summary> Plans Table Info </summary>
 <p>

**Query #2**

    SELECT * 
    FROM plans;

| plan_id | plan_name     | price  |
| ------- | ------------- | ------ |
| 0       | trial         | 0.00   |
| 1       | basic monthly | 9.90   |
| 2       | pro monthly   | 19.90  |
| 3       | pro annual    | 199.00 |
| 4       | churn         |        |

 </p>
</details>

<details>
 <summary> Subscriptions Table Structure </summary>
 <p>
   
**Query #3**
    
    DESCRIBE subscriptions;

| Field       | Type | Null | Key | Default | Extra |
| ----------- | ---- | ---- | --- | ------- | ----- |
| customer_id | int  | YES  |     |         |       |
| plan_id     | int  | YES  |     |         |       |
| start_date  | date | YES  |     |         |       |

Customer subscriptions show the exact date where their specific plan_id starts.

If customers downgrade from a pro plan or cancel their subscription - the higher plan will remain in place until the period is over - the start_date in the subscriptions table will reflect the date that the actual plan changes.

When customers upgrade their account from a basic plan to a pro or annual pro plan - the higher plan will take effect straightaway.

When customers churn - they will keep their access until the end of their current billing period but the start_date will be technically the day they decided to cancel their service.

 </p>
</details>

<details>
 <summary> Subscriptions Table Info </summary>
 <p>
   
**Query #4**

    SELECT * 
    FROM subscriptions
    LIMIT 10;

| customer_id | plan_id | start_date |
| ----------- | ------- | ---------- |
| 1           | 0       | 2020-08-01 |
| 1           | 1       | 2020-08-08 |
| 2           | 0       | 2020-09-20 |
| 2           | 3       | 2020-09-27 |
| 3           | 0       | 2020-01-13 |
| 3           | 1       | 2020-01-20 |
| 4           | 0       | 2020-01-17 |
| 4           | 1       | 2020-01-24 |
| 4           | 4       | 2020-04-21 |
| 5           | 0       | 2020-08-03 |
 </p>
</details>

## 🧙‍♂️ Case Study Questions & 🚀 Solutions

**A. CUSTOMER JOURNEY**

Based off the 8 sample customers provided in the below sample from the subscriptions table, write a brief description about each customer’s onboarding journey.

| customer_id | plan_id | start_date |
| ----------- | ------- | ---------- |
| 1	          | 0       |	2020-08-01 |
| 1	          | 1	      | 2020-08-08 |
| 2	          | 0	      | 2020-09-20 |
| 2	          | 3	      | 2020-09-27 |
| 11	         | 0	      | 2020-11-19 |
| 11	         | 4	      | 2020-11-26 |
| 13	         | 0	      | 2020-12-15 |
| 13	         | 1	      | 2020-12-22 |
| 13	         | 2	      | 2021-03-29 |
| 15	         | 0	      | 2020-03-17 |
| 15	         | 2	      | 2020-03-24 |
| 15          |	4	      | 2020-04-29 |
| 16          |	0       |	2020-05-31 |
| 16	         | 1	      | 2020-06-07 |
| 16          |	3	      | 2020-10-21 |
| 18	         | 0	      | 2020-07-06 |
| 18          |	2	      | 2020-07-13 |
| 19          |	0	      | 2020-06-22 |
| 19	         | 2	      | 2020-06-29 |
| 19          |	3	      | 2020-08-29 |

**Solution**
**Query #5**

    SELECT s.customer_id, p.plan_name, s.start_date
    FROM subscriptions s
    JOIN plans p
    ON s.plan_id = p.plan_id
    WHERE s.customer_id IN (1, 2, 11, 13, 15, 16, 18, 19);

| customer_id | start_date | plan_name     |
| ----------- | ---------- | ------------- |
| 1           | 2020-08-01 | trial         |
| 1           | 2020-08-08 | basic monthly |
| 2           | 2020-09-20 | trial         |
| 2           | 2020-09-27 | pro annual    |
| 11          | 2020-11-19 | trial         |
| 11          | 2020-11-26 | churn         |
| 13          | 2020-12-15 | trial         |
| 13          | 2020-12-22 | basic monthly |
| 13          | 2021-03-29 | pro monthly   |
| 15          | 2020-03-17 | trial         |
| 15          | 2020-03-24 | pro monthly   |
| 15          | 2020-04-29 | churn         |
| 16          | 2020-05-31 | trial         |
| 16          | 2020-06-07 | basic monthly |
| 16          | 2020-10-21 | pro annual    |
| 18          | 2020-07-06 | trial         |
| 18          | 2020-07-13 | pro monthly   |
| 19          | 2020-06-22 | trial         |
| 19          | 2020-06-29 | pro monthly   |
| 19          | 2020-08-29 | pro annual    |

Based on the data above, see below the journey of these 8 sample customers:
| Customer ID      | Journey    |
| -----------      | ---------- |
| 1                | This customer signed up for Foodie-Fi on 01 August and downgraded to basic monthly at the end of the trial period |
|  2               | This customer signed up for Foodie-Fi on 20 September and was automatically upgraded to pro annual at the end of the trial period |
| 11               | This customer signed up for Foodie-Fi on 19 November and cancelled their subscription by the end of the trial period  |
| 13               | This customer signed up for Foodie-Fi on 15 December, downgraded to basic monthly at the end of the trial period, and has eventually upgraded to pro monthly  |
| 15               | This customer signed up for Foodie-Fi on 17 March,  downgraded to pro monthly at the end of the trial period, subsequently cancelled their subscription before the next renewal |
| 16               | This customer signed up for Foodie-Fi on 31 May,  downgraded to basic monthly at the end of the trial period, and eventually  upgraded their subscription to pro annual |
| 18               | This customer signed up for Foodie-Fi on 06 July and downgraded to basic monthly at the end of the trial period |
| 19               | This customer signed up for Foodie-Fi on 22 June,  downgraded to pro monthly at the end of the trial period, and eventually  upgraded their subscription to pro annual |


**B. DATA ANALYSIS JOURNEY**

**1. How many customers has Foodie-Fi ever had?**

      SELECT
      	COUNT(DISTINCT(customer_id)) AS NumberOfCustomers
      FROM subscriptions;

**2. What is the monthly distribution of trial plan start_date values for our dataset? - use the start of the month as the group by value**

     SELECT
     	MONTH(s.start_date) AS MonthNum,
     	MONTHNAME(s.start_date) AS Month,
      COUNT(s.customer_id) AS NumberOfSubs
     FROM subscriptions s
     WHERE s.plan_id = 0
     GROUP BY MonthNum, Month
     ORDER BY MonthNum; 

**3. What plan start_date values occur after the year 2020 for our dataset? Show the breakdown by count of events for each plan_name?**

     SELECT
       YEAR(s.start_date) AS FY,
       p.plan_name As Plan,
       COUNT(s.customer_id) As NumberOfSubs
     FROM subscriptions s
     JOIN plans p
     ON s.plan_id = p.plan_id
     WHERE YEAR(s.start_date) > 2020
     GROUP BY FY, Plan;

**4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?**

    SELECT
      COUNT(DISTINCT(s.customer_ID)) AS ChurnedCustomers,
      ROUND(100 * COUNT(s.customer_ID) / (SELECT COUNT(DISTINCT customer_id) FROM subscriptions), 1) AS Percentage
    FROM subscriptions s
    WHERE s.plan_ID = 4 

**5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?**

     WITH ordered_cte AS(
       SELECT
       	customer_id,
       	plan_id,
       	start_date,
       	ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY start_date) AS ordered_row
       FROM subscriptions  		
     )
     
     SELECT
     	COUNT(DISTINCT(customer_id)) AS Churned_Customers,
         ROUND(100 * COUNT(DISTINCT(customer_id)) / (SELECT COUNT(DISTINCT(customer_id)) FROM subscriptions), 0) AS percentage
     FROM ordered_cte
     WHERE ordered_row=2 AND plan_id=4

**6. What is the number and percentage of customer plans after their initial free trial?**

      WITH ordered_cte AS(
        SELECT
        	customer_id,
        	plan_id,
        	start_date,
        	ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY start_date) AS next_row
        FROM subscriptions  		
      )
      
      SELECT
      	p.plan_name,
       COUNT(DISTINCT(customer_id)) AS subscribers,
       ROUND(100 * COUNT(DISTINCT(customer_id)) / (SELECT COUNT(DISTINCT(customer_id)) FROM ordered_cte), 1) AS percentages
      FROM plans p
      JOIN ordered_cte o 
      ON o.plan_id = p.plan_id
      WHERE next_row = 2
      GROUP BY p.plan_name;

**7. What is the customer count and percentage breakdown of all 5 plan_name values at 2020-12-31?**

       WITH recent_subs AS(
         SELECT
       	customer_id,
           plan_id,
           start_date,
           LEAD(start_date) OVER (PARTITION BY customer_id ORDER BY start_date) AS next_row
         FROM subscriptions
         WHERE start_date <= "2020-12-31"
       )
       
       SELECT
       	p.plan_name,
           COUNT(DISTINCT(customer_id)) AS Customer_Count,
           ROUND(100 * COUNT(DISTINCT(customer_id)) / (SELECT COUNT(DISTINCT(customer_id)) FROM subscriptions), 1) AS percentages
       FROM recent_subs rs
       JOIN plans p
       ON p.plan_id = rs.plan_id
       WHERE next_row IS NULL
       GROUP BY p.plan_name;

**8. How many customers have upgraded to an annual plan in 2020?**

      SELECT
      	COUNT(DISTINCT(customer_id)) AS pro_annual_subscribers
       FROM subscriptions
       WHERE plan_id = 3 AND YEAR(start_date) = 2020;

**9. How many days on average does it take for a customer to subscribe to an annual plan from the day they join Foodie-Fi?**

    WITH AnnualSubs AS (
      SELECT
      	customer_id,
      	start_date
      FROM subscriptions
      WHERE plan_id=3
    ),
    
    TrialSubs AS (
      SELECT
      	customer_id,
      	start_date
      FROM subscriptions
      WHERE plan_id=0 
    )
    
    SELECT
    	ROUND(AVG(a.start_date - t.start_date),0) AS AvgTAT
    FROM AnnualSubs a
    JOIN TrialSubs t
    ON a.customer_id=t.customer_id;

**10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)**



**11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?**

     WITH MonthlySubsDowngrade AS (
     	SELECT
       		customer_id,
       		plan_id,
       		start_date,
       		LEAD(plan_id) OVER(PARTITION BY customer_id ORDER BY start_date) AS next_plan
       	FROM subscriptions
       	WHERE YEAR(start_date) = 2020
     )
     
     SELECT
      COUNT(DISTINCT(customer_id)) AS DowngradedSubs  
     FROM MonthlySubsDowngrade
     WHERE plan_id = 2 AND next_plan=1;







