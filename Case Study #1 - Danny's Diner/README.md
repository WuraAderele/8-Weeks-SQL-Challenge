#  🍜 Case Study #1 - Danny's Diner
<p align = "center">
  <img src="https://user-images.githubusercontent.com/94797745/146899828-ea5f21ac-7c29-4227-bce0-b54b4b371e84.png" width = "400" height = "400"/>

#  📕 Table Of Contents
        📝 Background
        🛠️ Problem Statement
        📂 Dataset
        🧙‍♂️ Case Study Questions & 🚀 Solutions


## 📝 Background
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favorite foods: sushi, curry, and ramen.
Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

## 🛠️ Problem Statement
Danny wants to use the data to answer a few simple questions about his customers:
      i.   about their visiting patterns
      ii.  how much money they have spent
      iii. which menu items are their favorite. 
  
Having this deeper connection with his customers will help him deliver a better and more personalized experience for his loyal customers.
He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

 Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!
  
## 📂 Dataset
Danny has shared with you 3 key datasets for this case study:
      i.   sales
      ii.  menu
      iii. members
  
You can inspect the entity relationship diagram below:
 <p align = "center">
  <img width="500" alt="DD DB ERD" src="https://user-images.githubusercontent.com/94797745/146904293-4ea7056d-b7fc-4b2e-b0be-7740aa7c3398.PNG">

To understand the dataset we will be working with, I studied the Entity Relationship Diagrams of the Database and ran a few ###SQLite commands to get an overview of the tables in the database
   ##### DATABASE: dannys_diner
   ##### TABLES: 
       i.   Sales: Captures customer purchases using unique customer ID with corresponding order date and product ID
       ii.  Menu:  Captures relevant data (product ID, name, and price) of food menu items 
       iii. Members: Captures date when customer joined the Danny’s Diner loyalty program
   
   ##### Sales Table
        PRAGMA table_info(sales);
   ##### Result:
   
| cid | name        | type       | notnull | dflt_value | pk  |
| --- | ----------- | ---------- | ------- | ---------- | --- |
| 0   | customer_id | VARCHAR(1) | 0       |            | 0   |
| 1   | order_date  | DATE       | 0       |            | 0   |
| 2   | product_id  | INTEGER    | 0       |            | 0   |

   ##### Sample view of sales table:
        SELECT *
        FROM sales LIMIT 3;
   ##### Result:
| customer_id | order_date | product_id |
| ----------- | ---------- | ---------- |
| A           | 2021-01-01 | 1          |
| A           | 2021-01-01 | 2          |
| A           | 2021-01-07 | 2          |


   ##### Menu Table
        PRAGMA table_info(menu);
   ##### Result:
   
| cid | name         | type       | notnull | dflt_value | pk  |
| --- | ------------ | ---------- | ------- | ---------- | --- |
| 0   | product_id   | INTEGER    | 0       |            | 0   |
| 1   | product_name | VARCHAR(5) | 0       |            | 0   |
| 2   | price        | INTEGER    | 0       |            | 0   |

   ##### Sample view of menu table:
        SELECT *
        FROM menu LIMIT 3;
   ##### Result:
  
| product_id | product_name | price |
| ---------- | ------------ | ----- |
| 1          | sushi        | 10    |
| 2          | curry        | 15    |
| 3          | ramen        | 12    |

   ##### Members Table
        PRAGMA table_info(members);
   ##### Result:
  
| cid | name        | type       | notnull | dflt_value | pk  |
| --- | ----------- | ---------- | ------- | ---------- | --- |
| 0   | customer_id | VARCHAR(1) | 0       |            | 0   |
| 1   | join_date   | DATE       | 0       |            | 0   |

   ##### Sample view of members table:
        SELECT *
        FROM members LIMIT 3;
   ##### Result:

| customer_id | join_date  |
| ----------- | ---------- |
| A           | 2021-01-07 |
| B           | 2021-01-09 |

## 🧙‍♂️ Case Study Questions & 🚀 Solutions
###  1.  What is the total amount each customer spent at the restaurant?
            SELECT customer_id
              ,sum(price) AS amount_spent
            FROM sales
            JOIN menu ON sales.product_id = menu.product_id
            GROUP BY customer_id;
   
| customer_id | amount_spent |
| ----------- | ------------ |
| A           | 76           |
| B           | 74           |
| C           | 36           |


### 2.  How many days has each customer visited the restaurant?
          SELECT customer_id
            ,COUNT(DISTINCT order_date) AS days_visited
          FROM sales
          GROUP BY customer_id;
   
| customer_id | days_visited |
| ----------- | ------------ |
| A           | 4            |
| B           | 6            |
| C           | 2            |


### 3.  What was the first item from the menu purchased by each customer?
          WITH first_item
          AS (
            SELECT sales.customer_id
              ,menu.product_name
              ,ROW_NUMBER() OVER (
                PARTITION BY sales.customer_id ORDER BY sales.order_date
                ) AS rank
            FROM sales
            JOIN menu ON sales.product_id = menu.product_id
            )
          SELECT *
          FROM first_item
          WHERE rank = 1;

| customer_id | product_name | rank |
| ----------- | ------------ | ---- |
| A           | sushi        | 1    |
| B           | curry        | 1    |
| C           | ramen        | 1    |


###  4.  What is the most purchased item on the menu and how many times was it purchased by all customers?
        SELECT sales.product_id
          ,menu.product_name
          ,count(sales.product_id) AS num_of_purchases
        FROM sales
        JOIN menu ON sales.product_id = menu.product_id
        GROUP BY sales.product_id
          ,menu.product_name
        ORDER BY num_of_purchases DESC LIMIT 1;

| product_id | product_name | num_of_purchases |
| ---------- | ------------ | ---------------- |
| 3          | ramen        | 8                |

###  5.  Which item was the most popular for each customer?
        WITH fave_item
        AS (
          SELECT sales.customer_id
            ,menu.product_name
            ,COUNT(sales.product_id) AS popular_order
            ,DENSE_RANK() OVER (
              PARTITION BY sales.customer_id ORDER BY COUNT(sales.product_id) DESC
              ) AS rank
          FROM sales
          JOIN menu ON sales.product_id = menu.product_id
          GROUP BY sales.customer_id
            ,menu.product_name
          )
        SELECT *
        FROM fave_item
        WHERE rank = 1;

| customer_id | product_name | popular_order | rank |
| ----------- | ------------ | ------------- | ---- |
| A           | ramen        | 3             | 1    |
| B           | sushi        | 2             | 1    |
| B           | ramen        | 2             | 1    |
| B           | curry        | 2             | 1    |
| C           | ramen        | 3             | 1    |

###  6. Which item was purchased first by the customer after they became a member?
        WITH first_member_item
        AS (
          SELECT sales.customer_id
            ,sales.product_id
            ,menu.product_name
            ,members.join_date
            ,DENSE_RANK() OVER (
              PARTITION BY sales.customer_id ORDER BY 
              sales.order_date ,sales.customer_id
              ) AS rank
          FROM sales
          JOIN members ON sales.customer_id = members.customer_id
          JOIN menu ON sales.product_id = menu.product_id
          WHERE sales.order_date >= members.join_date
          )
        SELECT customer_id
          ,product_name
        FROM first_member_item
        WHERE rank = 1;
   
| customer_id | product_name |
| ----------- | ------------ |
| A           | curry        |
| B           | sushi        |

   
###  7. Which item was purchased just before the customer became a member?
        WITH prior_member_item
        AS (
          SELECT sales.customer_id
            ,sales.product_id
            ,menu.product_name
            ,sales.order_date
            ,members.join_date
            ,DENSE_RANK() OVER (
              PARTITION BY sales.customer_id ORDER BY
              sales.order_date DESC
              ) AS rank
          FROM sales
          JOIN members ON sales.customer_id = members.customer_id
          JOIN menu ON sales.product_id = menu.product_id
          WHERE sales.order_date < members.join_date
          )
        SELECT customer_id, product_name
        FROM prior_member_item
        WHERE rank = 1;

| customer_id | product_name |
| ----------- | ------------ |
| A           | sushi        |
| A           | curry        |
| B           | sushi        |

###  8. What is the total items and amount spent for each member before they became a member?
        SELECT sales.customer_id
          ,COUNT(sales.product_id) AS total_purchases
          ,SUM(menu.price) AS total_cost
        FROM sales
        JOIN menu ON sales.product_id = menu.product_id
        JOIN members ON sales.customer_id = members.customer_id
        WHERE sales.order_date < members.join_date
        GROUP BY sales.customer_id
        ORDER BY sales.customer_id;

| customer_id | total_purchases | total_cost |
| ----------- | --------------- | ---------- |
| A           | 2               | 25         |
| B           | 3               | 40         |

                                      
###  9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
        WITH price_points
        AS (
          SELECT *
            ,CASE 
              WHEN menu.product_name = "sushi"
                THEN menu.price * 10 * 2
              ELSE price * 10
              END AS points_gained
          FROM menu
          )
        SELECT sales.customer_id
          ,SUM(points_gained) AS customer_points
        FROM price_points
        JOIN sales ON price_points.product_id = sales.product_id
        GROUP BY sales.customer_id

| customer_id | customer_points |
| ----------- | --------------- |
| A           | 860             |
| B           | 940             |
| C           | 360             |

###  10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi -          how many points do customer A and B have at the end of January?
          WITH jan_and_first_week
          AS (
            SELECT *
              ,DATEADD(DAY, 6, join_date) AS first_week
              ,EOMONTH('2021-01-31') AS jan_end
            FROM members
            )
          SELECT jfw.customer_id
            ,SUM(CASE 
                WHEN menu.product_name = 'sushi'
                  THEN 2 * 10 * menu.price
                WHEN sales.order_date BETWEEN jfw.join_date
                    AND jfw.first_week
                  THEN 2 * 10 * menu.price
                ELSE 10 * menu.price
                END) AS total_jan_points
          FROM jan_and_first_week AS jfw
          JOIN sales ON jfw.customer_id = sales.customer_id
          JOIN menu ON sales.product_id = menu.product_id
          WHERE sales.order_date < jfw.jan_end
          GROUP BY jfw.customer_id;

| customer_id | total_jan_points  |
| ----------- | ---------------   |
| A           | 1370              |
| B           | 820               |
