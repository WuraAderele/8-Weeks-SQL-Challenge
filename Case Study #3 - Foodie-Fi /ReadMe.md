# 🥑 Case Study #3 - Foodie-Fi 

<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-designs/3.png" width = "400" height = "400">

##  📕 Table Of Contents
* ### 📝 Background & 🛠️ Problem Statement
* ### 📂 Dataset
* ### ♻️ Data Wrangling
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
 <summary> **Plans Table Structure** </summary>
 <p>
   
**Query #1**
    
    DESCRIBE plans;
    
| Field     | Type         | Null | Key | Default | Extra |
| --------- | ------------ | ---- | --- | ------- | ----- |
| plan_id   | int          | YES  |     |         |       |
| plan_name | varchar(13)  | YES  |     |         |       |
| price     | decimal(5,2) | YES  |     |         |       |

Customers can choose which plans to join Foodie-Fi when they first sign up.

Basic plan customers have limited access and can only stream their videos and is only available monthly at $9.90

Pro plan customers have no watch time limits and are able to download videos for offline viewing. Pro plans start at $19.90 a month or $199 for an annual subscription.

Customers can sign up to an initial 7 day free trial will automatically continue with the pro monthly subscription plan unless they cancel, downgrade to basic or upgrade to an annual pro plan at any point during the trial.

When customers cancel their Foodie-Fi service - they will have a churn plan record with a null price but their plan will continue until the end of the billing period.
 
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
   
**Query #1**

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

## ♻️ Data Wrangling
## 🧙‍♂️ Case Study Questions & 🚀 Solutions
