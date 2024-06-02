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
  
### Table Info  
<details>
 <summary> Plans Table </summary>
 <p>
   
**Query #1**
    DESCRIBE plans;
| Field     | Type         | Null | Key | Default | Extra |
| --------- | ------------ | ---- | --- | ------- | ----- |
| plan_id   | int          | YES  |     |         |       |
| plan_name | varchar(13)  | YES  |     |         |       |
| price     | decimal(5,2) | YES  |     |         |       |
 </p>
</details>

<details>
 <summary> Subscriptions Table </summary>
 <p>
   
**Query #2**
    
    DESCRIBE subscriptions;

| Field       | Type | Null | Key | Default | Extra |
| ----------- | ---- | ---- | --- | ------- | ----- |
| customer_id | int  | YES  |     |         |       |
| plan_id     | int  | YES  |     |         |       |
| start_date  | date | YES  |     |         |       |

 </p>
</details>

## 📂 Dataset
Danny has shared the data design for Foodie-Fi and also short descriptions on each of the database tables as shown below:

## ♻️ Data Wrangling
## 🧙‍♂️ Case Study Questions & 🚀 Solutions
