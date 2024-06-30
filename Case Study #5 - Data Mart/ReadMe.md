# 🛒 Case Study #4 - Data Mart 

<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-designs/5.png" width = "400" height = "400">

##  📕 Table Of Contents
* ### 📝 Background & 🛠️ Problem Statement
* ### 📂 Dataset
* ### ♻️ Data Wrangling
* ### 🧙‍♂️ Case Study Questions & 🚀 Solutions

# 📝 Background & 🛠️ Problem Statement
Data Mart is Danny’s latest venture and after running international operations for his online supermarket that specialises in fresh produce - Danny is asking for your support to analyse his sales performance.

In June 2020 - large scale supply changes were made at Data Mart. All Data Mart products now use sustainable packaging methods in every single step from the farm all the way to the customer.

Danny needs your help to quantify the impact of this change on the sales performance for Data Mart and it’s separate business areas.

The key business question he wants you to help him answer are the following:

* What was the quantifiable impact of the changes introduced in June 2020?
* Which platform, region, segment and customer types were the most impacted by this change?
* What can we do about future introduction of similar sustainability updates to the business to minimise impact on sales?

# 📂 Dataset
We have been provided with the below entity relationship diagram for an understanding of the dataset:
<p align = "center">
<img src = "https://8weeksqlchallenge.com/images/case-study-5-erd.png" width = "400" height = "400">
From this diagram, we can see that there is only one table: data_mart.weekly_sales. 

*week_date* represents the start of the sales week
*region* represents the locations in which Data Mart has presence
*platform* represents the kind of store front is used to serve Data Mart e.g. retail, shopify
*segment* relates to demographics information of the customer
*customer_type* relates to whether the customer is new, existing, or a guest customer
*transactions* specifies the count of unique purchses made through Data Mart
*sales* specifies the dollar amount of purchases

**Sample Rows From Dataset:**

| week_date | region | platform | segment | customer_type | transactions | sales    |
| --------- | ------ | -------- | ------- | ------------- | ------------ | -------- |
| 31/8/20   | ASIA   | Retail   | C3      | New           | 120631       | 3656163  |
| 31/8/20   | ASIA   | Retail   | F1      | New           | 31574        | 996575   |
| 31/8/20   | USA    | Retail   | null    | Guest         | 529151       | 16509610 |
| 31/8/20   | EUROPE | Retail   | C1      | New           | 4517         | 141942   |
| 31/8/20   | AFRICA | Retail   | C2      | New           | 58046        | 1758388  |
| 31/8/20   | CANADA | Shopify  | F2      | Existing      | 1336         | 243878   |
| 31/8/20   | AFRICA | Shopify  | F3      | Existing      | 2514         | 519502   |
| 31/8/20   | ASIA   | Shopify  | F1      | Existing      | 2158         | 371417   |
| 31/8/20   | AFRICA | Shopify  | F2      | New           | 318          | 49557    |
| 31/8/20   | AFRICA | Retail   | C3      | New           | 111032       | 3888162  |

# ♻️ Data Wrangling
In a single query, perform the following operations and generate a new table in the *data_mart* schema named *clean_weekly_sales*:

* Convert the *week_date* to a DATE format

* Add a week_number as the second column for each week_date value, for example any value from the 1st of January to 7th of January will be 1, 8th to 14th will be 2 etc

* Add a month_number with the calendar month for each week_date value as the 3rd column

* Add a calendar_year column as the 4th column containing either 2018, 2019 or 2020 values

* Add a new column called age_band after the original segment column using the following mapping on the number inside the segment value


