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

| segment	| age_band | 
|---------|----------|
| 1	| Young Adults |
| 2 |	Middle Aged |
| 3 or 4 | Retirees |

* Add a new demographic column using the following mapping for the first letter in the segment values:
  
| segment |	demographic |
|---------|-------------|
| C |	Couples |
| F | Families |

* Ensure all null string values with an "unknown" string value in the original segment column as well as the new age_band and demographic columns

* Generate a new avg_transaction column as the sales value divided by transactions rounded to 2 decimal places for each record

**SINGLE QUERY**

    CREATE TABLE clean_weekly_sales AS (
      SELECT
      	TO_DATE(week_date, 'DD/MM/YY') AS week_date,
      	DATE_PART('week', TO_DATE(week_date, 'DD/MM/YY')) AS week_number,
        DATE_PART('month', TO_DATE(week_date, 'DD/MM/YY')) AS month_number,
        DATE_PART('year', TO_DATE(week_date, 'DD/MM/YY')) AS calendar_year,
        region,
        platform,
        CASE
      	  WHEN segment = 'null' THEN 'unknown'
      	  ELSE segment
      	END AS segment,
        CASE
          WHEN RIGHT(segment, 1) = '1' THEN 'Young Adults'
          WHEN RIGHT(segment, 1) = '2' THEN 'Middle Aged'
          WHEN RIGHT(segment, 1) = '3' OR RIGHT(segment, 1) = '4' THEN 'Retirees'
          ELSE 'unknown'
        END AS age_band,
        CASE
          WHEN LEFT(segment, 1) = 'C' THEN 'Couples'
          WHEN LEFT(segment, 1) = 'F' THEN 'Families'
          ELSE 'unknown'
        END AS demographic,
        sales,
        transactions,
      	ROUND((sales/transactions), 2) AS avg_transaction
      FROM data_mart.weekly_sales);

# 🧙‍♂️ Case Study Questions & 🚀 Solutions

### Data Exploration

* What day of the week is used for each week_date value?

      SELECT
      	DISTINCT(TO_CHAR(week_date, 'Day')) AS day_of_week
      FROM clean_weekly_sales;

The results of the query shows that **Monday** is the always the start of the sales week.

* What range of week numbers are missing from the dataset?

      WITH week_range AS (
        SELECT GENERATE_SERIES(1,52) AS week_number
      )
        
      SELECT 
      	DISTINCT wr.week_number
      FROM week_range wr 
      LEFT JOIN clean_weekly_sales cws
      ON wr.week_number = cws.week_number
      WHERE cws.week_number IS NULL;

From the results of the query, weeks 1-12 and 37-52 are missing from the dataset.

* How many total transactions were there for each year in the dataset?

	    SELECT
	    	calendar_year,
	    	SUM(transactions) AS total_txns
	    FROM clean_weekly_sales
	    GROUP BY calendar_year;

| calendar_year |	sum |
| ------------- | --- |
| 2019 | 365639285 | 
| 2018	| 346406460 |
| 2020	375813651 |

* What is the total sales for each region for each month?

      SELECT
      	region,
      	calendar_year,
          month_number,
          TO_CHAR(week_date,'Month') AS month_name,
          SUM(sales) AS total_sales
      FROM clean_weekly_sales
      GROUP BY region, calendar_year, month_number, month_name
      ORDER BY month_number, region ASC;
  
* What is the total count of transactions for each platform

      SELECT
      	platform,
      	SUM(transactions) AS total_txns
      FROM clean_weekly_sales
      GROUP BY platform;
  
* What is the percentage of sales for Retail vs Shopify for each month?

      WITH platform_sales AS(
        SELECT
        	calendar_year,
        	month_number,
        	platform,
        	SUM(sales) AS monthly_sales
        FROM clean_weekly_sales
        GROUP BY calendar_year, month_number, platform
        ORDER BY calendar_year, month_number, platform
      )
      
      SELECT
      	calendar_year,
          month_number,
      	ROUND(100 * MAX 
          (CASE 
            WHEN platform = 'Retail' THEN monthly_sales END) 
          / SUM(monthly_sales),2) AS retail_percent,
        ROUND(100 * MAX 
          (CASE 
            WHEN platform = 'Shopify' THEN monthly_sales END)
          / SUM(monthly_sales),2) AS shopify_percent
      FROM platform_sales
      GROUP BY calendar_year, month_number
      ORDER BY calendar_year, month_number;

In this query, the MAX function is being used in a clever way to achieve conditional aggregation. Here's the purpose of MAX in this context:

* Conditional selection: The MAX function is combined with a CASE statement to select the sales value for a specific platform (either 'Retail' or 'Shopify').
* Null handling: For rows where the platform doesn't match the condition, the CASE statement returns NULL. The MAX function ignores NULL values.
* Group-wise selection: When used with GROUP BY, MAX will return the non-NULL value (if any) for each group.

* What is the percentage of sales by demographic for each year in the dataset?

  WITH demog_yearly_sales AS(
  SELECT
  	calendar_year,
   	demographic,
    SUM(sales) AS yearly_sales
    FROM clean_weekly_sales
    GROUP BY calendar_year, demographic
    ORDER BY calendar_year, demographic
)

SELECT
	calendar_year,
    ROUND(100 * MAX 
          (CASE 
            WHEN demographic = 'Couples' THEN yearly_sales END) 
          / SUM(yearly_sales),2) AS couples,
    ROUND(100 * MAX 
          (CASE 
            WHEN demographic = 'Families' THEN yearly_sales END)
          / SUM(yearly_sales),2) AS families,
     ROUND(100 * MAX 
          (CASE 
            WHEN demographic = 'unknown' THEN yearly_sales END)
          / SUM(yearly_sales),2) AS unknown
 FROM demog_yearly_sales
 GROUP BY calendar_year
 ORDER BY calendar_year;

* Which age_band and demographic values contribute the most to Retail sales?

SELECT
	age_band,
    demographic,
    SUM(sales) AS total_sales
FROM clean_weekly_sales
WHERE platform = 'Retail'
GROUP BY age_band, demographic
ORDER BY total_sales DESC;

* Can we use the avg_transaction column to find the average transaction size for each year for Retail vs Shopify? If not - how would you calculate it instead?

    SELECT
    	calendar_year,
        platform,
        AVG(avg_transaction) AS avg_txn
    FROM clean_weekly_sales
    GROUP BY calendar_year, platform
    ORDER BY calendar_year, platform;
      

### Before & After Analysis
This technique is usually used when we inspect an important event and want to inspect the impact before and after a certain point in time.

Taking the *week_date* value of *2020-06-15* as the baseline week where the Data Mart sustainable packaging changes came into effect.

We would include all *week_date* values for *2020-06-15* as the start of the period after the change and the previous week_date values would be before.

Using this analysis approach - answer the following questions:

* What is the total sales for the 4 weeks before and after 2020-06-15? What is the growth or reduction rate in actual values and percentage of sales?
* What about the entire 12 weeks before and after?
* How do the sale metrics for these 2 periods before and after compare with the previous years in 2018 and 2019?



