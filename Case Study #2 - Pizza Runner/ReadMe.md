# 🍕 Case Study #2 - Pizza Runner 

<p align = "center">
<img src = "https://user-images.githubusercontent.com/94797745/147704528-a3fbdc41-8a8b-4895-b4f9-9aa672615e18.png" width = "400" height = "400">

##  📕 Table Of Contents
* ### 📝 Background & 🛠️ Problem Statement
* ### 📂 Dataset
* ### ♻️ Data Wrangling
* ### 🧙‍♂️ Case Study Questions & 🚀 Solutions

## 📝 Background & 🛠️ Problem Statement
Danny was scrolling through his Instagram feed when something really caught his eye - “80s Retro Styling and Pizza Is The Future!”

Danny was sold on the idea, but he knew that pizza alone was not going to help him get seed funding to expand his new Pizza Empire - so he had one more genius idea to combine with it - he was going to Uberize it - and so Pizza Runner was launched!

Danny started by recruiting “runners” to deliver fresh pizza from Pizza Runner Headquarters (otherwise known as Danny’s house) and also maxed out his credit card to pay freelance developers to build a mobile app to accept orders from customers.

## 📂 Dataset
Because Danny had a few years of experience as a data scientist - he was very aware that data collection was going to be critical for his business’ growth.

He has prepared for us an entity relationship diagram of his database design but requires further assistance to clean his data and apply some basic calculations so he can better direct his runners and optimise Pizza Runner’s operations.
You can inspect the entity relationship diagram below:
 <p align = "center">
   <img width="500" alt="PR ERD Capture" src="https://user-images.githubusercontent.com/94797745/147705079-71079fdb-e475-48df-8e32-31763dc6b91d.PNG">
<details>
 <summary> Runner_Orders Table </summary>
 <p>
  
| order_id | runner_id | pickup_time         | distance | duration   | cancellation            |
| -------- | --------- | ------------------- | -------- | ---------- | ----------------------- |
| 1        | 1         | 2020-01-01 18:15:34 | 20km     | 32 minutes |                         |
| 2        | 1         | 2020-01-01 19:10:54 | 20km     | 27 minutes |                         |
| 3        | 1         | 2020-01-03 00:12:37 | 13.4km   | 20 mins    |                         |
| 4        | 2         | 2020-01-04 13:53:03 | 23.4     | 40         |                         |
| 5        | 3         | 2020-01-08 21:10:57 | 10       | 15         |                         |
| 6        | 3         | null                | null     | null       | Restaurant Cancellation |
| 7        | 2         | 2020-01-08 21:30:45 | 25km     | 25mins     | null                    |
| 8        | 2         | 2020-01-10 00:15:02 | 23.4 km  | 15 minute  | null                    |
| 9        | 2         | null                | null     | null       | Customer Cancellation   |
| 10       | 1         | 2020-01-11 18:50:20 | 10km     | 10minutes  | null                    |
 </p>
</details>

<details>
 <summary> Runners Table </summary>
 <p>
  
| runner_id | registration_date |
| --------- | ----------------- |
| 1         | 2021-01-01        |
| 2         | 2021-01-03        |
| 3         | 2021-01-08        |
| 4         | 2021-01-15        |
 </p>
</details>

<details>
 <summary> Customer_Orders Table </summary>
 <p>
  
| order_id | customer_id | pizza_id | exclusions | extras | order_time          |
| -------- | ----------- | -------- | ---------- | ------ | ------------------- |
| 1        | 101         | 1        |            |        | 2020-01-01 18:05:02 |
| 2        | 101         | 1        |            |        | 2020-01-01 19:00:52 |
| 3        | 102         | 1        |            |        | 2020-01-02 23:51:23 |
| 3        | 102         | 2        |            |        | 2020-01-02 23:51:23 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 2        | 4          |        | 2020-01-04 13:23:46 |
| 5        | 104         | 1        | null       | 1      | 2020-01-08 21:00:29 |
| 6        | 101         | 2        | null       | null   | 2020-01-08 21:03:13 |
| 7        | 105         | 2        | null       | 1      | 2020-01-08 21:20:29 |
| 8        | 102         | 1        | null       | null   | 2020-01-09 23:54:33 |
| 9        | 103         | 1        | 4          | 1, 5   | 2020-01-10 11:22:59 |
| 10       | 104         | 1        | null       | null   | 2020-01-11 18:34:49 |
| 10       | 104         | 1        | 2, 6       | 1, 4   | 2020-01-11 18:34:49 |
 </p>
</details>
  
<details>
 <summary> Pizza_Names Table </summary>
 <p>
  
| pizza_id | pizza_name |
| -------- | ---------- |
| 1        | Meatlovers |
| 2        | Vegetarian |
 </p>
</details>
  
<details>
 <summary> Pizza_Recipes Table </summary>
 <p>
  
| pizza_id | toppings                |
| -------- | ----------------------- |
| 1        | 1, 2, 3, 4, 5, 6, 8, 10 |
| 2        | 4, 6, 7, 9, 11, 12      |
 </p>
</details>
  
<details>
 <summary> Pizza_Toppings Table </summary>
 <p>
  
| topping_id | topping_name |
| ---------- | ------------ |
| 1          | Bacon        |
| 2          | BBQ Sauce    |
| 3          | Beef         |
| 4          | Cheese       |
| 5          | Chicken      |
| 6          | Mushrooms    |
| 7          | Onions       |
| 8          | Pepperoni    |
| 9          | Peppers      |
| 10         | Salami       |
| 11         | Tomatoes     |
| 12         | Tomato Sauce |
 </p>
</details>
  
## ♻️ Data Wrangling
### Data Issues
After studying the existing table, we notice some data issues in the existing schema. The issues are:
  
I.	Customer_orders table: Null and NaN values in the extras and exclusions columns
  
II.	Runner_orders table: Null and Nan values in pickup_time, distance columns, duration, and cancellation columns; wrong data type for pickup_time, distance, duration, and columns; units manually entered in distance and duration columns

### Data Cleanup
I.	Customer_orders table: 
*	Null and NaN values in the extras and exclusions columns
             
             UPDATE customer_orders
             SET extras = ""
             WHERE extras LIKE "null"
              OR extras IS NULL;

             UPDATE customer_orders
             SET exclusions = ""
             WHERE exclusions LIKE "null";

II.	Runner_orders table: 
* Null and Nan values in pickup_time, distance columns, duration, and cancellation columns
            
            UPDATE Runner_orders
            SET pickup_time = ""
            WHERE pickup_time LIKE "null"
             OR pickup_time IS NULL;

            UPDATE Runner_orders
            SET distance = ""
            WHERE distance LIKE "null"
             OR distance IS NULL;

            UPDATE Runner_orders
            SET cancellation = ""
            WHERE cancellation LIKE "null"
             OR cancellation IS NULL;

            UPDATE Runner_orders
            SET duration = ""
            WHERE duration LIKE "null"
             OR duration IS NULL;

* Units manually entered in distance and duration columns
  
            UPDATE Runner_orders
            SET distance = TRIM(distance, "km")
            WHERE distance LIKE "%km";

            UPDATE Runner_orders
            SET duration = TRIM(duration, "mins")
            WHERE duration LIKE "%mins";

            UPDATE Runner_orders
            SET duration = TRIM(duration, "minute")
            WHERE duration LIKE "%minute";

            UPDATE Runner_orders
            SET duration = TRIM(duration, "minutes")
            WHERE duration LIKE "%minutes";

* Wrong data type for pickup_time, distance, and duration columns
  
            ALTER TABLE runner_orders RENAME TO old_runner_orders;

            CREATE TABLE runner_orders (
             "order_id" INTEGER
             ,"runner_id" INTEGER
             ,"pickup_time" DATETIME
             ,"distance" REAL
             ,"duration" REAL
             ,"cancellation" VARCHAR(23)
             );

            INSERT INTO runner_orders (
             order_id
             ,runner_id
             ,pickup_time
             ,distance
             ,duration
             ,cancellation
             )
            SELECT order_id
             ,runner_id
             ,pickup_time
             ,distance
             ,duration
             ,cancellation
            FROM old_runner_orders;

### New Tables after Data Cleanup
<details>
 <summary> Runner_Orders Table </summary>
 <p>

| order_id | runner_id | pickup_time         | distance | duration | cancellation            |
| -------- | --------- | ------------------- | -------- | -------- | ----------------------- |
| 1        | 1         | 2020-01-01 18:15:34 | 20       | 32       |                         |
| 2        | 1         | 2020-01-01 19:10:54 | 20       | 27       |                         |
| 3        | 1         | 2020-01-03 00:12:37 | 13.4     | 20       |                         |
| 4        | 2         | 2020-01-04 13:53:03 | 23.4     | 40       |                         |
| 5        | 3         | 2020-01-08 21:10:57 | 10       | 15       |                         |
| 6        | 3         |                     |          |          | Restaurant Cancellation |
| 7        | 2         | 2020-01-08 21:30:45 | 25       | 25       |                         |
| 8        | 2         | 2020-01-10 00:15:02 | 23.4     | 15       |                         |
| 9        | 2         |                     |          |          | Customer Cancellation   |
| 10       | 1         | 2020-01-11 18:50:20 | 10       | 10       |                         |
 
 </p>
</details>

<details>
 <summary> Customer_Orders Table </summary>
 <p>
  
| order_id | customer_id | pizza_id | exclusions | extras | order_time          |
| -------- | ----------- | -------- | ---------- | ------ | ------------------- |
| 1        | 101         | 1        |            |        | 2020-01-01 18:05:02 |
| 2        | 101         | 1        |            |        | 2020-01-01 19:00:52 |
| 3        | 102         | 1        |            |        | 2020-01-02 23:51:23 |
| 3        | 102         | 2        |            |        | 2020-01-02 23:51:23 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 2        | 4          |        | 2020-01-04 13:23:46 |
| 5        | 104         | 1        |            | 1      | 2020-01-08 21:00:29 |
| 6        | 101         | 2        |            |        | 2020-01-08 21:03:13 |
| 7        | 105         | 2        |            | 1      | 2020-01-08 21:20:29 |
| 8        | 102         | 1        |            |        | 2020-01-09 23:54:33 |
| 9        | 103         | 1        | 4          | 1, 5   | 2020-01-10 11:22:59 |
| 10       | 104         | 1        |            |        | 2020-01-11 18:34:49 |
| 10       | 104         | 1        | 2, 6       | 1, 4   | 2020-01-11 18:34:49 |
 </p>
</details>
  
## 🧙‍♂️ Case Study Questions & 🚀 Solutions
This case study has LOTS of questions - they are broken up by area of focus.
  * Pizza Metrics
  * Runner and Customer Experience

### Pizza Metrics
  #### a.	How many pizzas were ordered?
  
              SELECT COUNT(order_id)
              FROM customer_orders;
  
| COUNT(order_id) |
| --------------- |
| 14              |

  #### b.	How many unique customer orders were made?
  
              SELECT COUNT(DISTINCT order_id)
              FROM customer_orders;
  
| COUNT(DISTINCT order_id) |
| ------------------------ |
| 10                       |

#### c.	How many successful orders were delivered by each runner?
  
              SELECT runner_id
               ,COUNT(order_id) AS successful_orders
              FROM runner_orders
              WHERE cancellation NOT IN (
                "Restaurant Cancellation"
                ,"Customer Cancellation"
                )
              GROUP BY runner_id;

| runner_id | successful_orders |
| --------- | ----------------- |
| 1         | 4                 |
| 2         | 3                 |
| 3         | 1                 |

#### d.	How many of each type of pizza was delivered?
  
             SELECT pizza_name
              ,COUNT(r.order_id)
             FROM pizza_names p
             JOIN customer_orders c ON c.pizza_id = p.pizza_id
             JOIN runner_orders r ON r.order_id = c.order_id
             WHERE r.cancellation NOT IN (
               "Restaurant Cancellation"
               ,"Customer Cancellation"
               )
             GROUP BY p.pizza_id;

| pizza_name | COUNT(r.order_id) |
| ---------- | ----------------- |
| Meatlovers | 9                 |
| Vegetarian | 3                 |

#### e.	How many Vegetarian and Meatlovers were ordered by each customer?
  
              SELECT pizza_name
               ,c.customer_id
               ,COUNT(r.order_id)
              FROM pizza_names p
              JOIN customer_orders c ON c.pizza_id = p.pizza_id
              JOIN runner_orders r ON r.order_id = c.order_id
              WHERE r.cancellation NOT IN (
                "Restaurant Cancellation"
                ,"Customer Cancellation"
                )
              GROUP BY c.customer_id
               ,p.pizza_id;

| pizza_name | customer_id | COUNT(r.order_id) |
| ---------- | ----------- | ----------------- |
| Meatlovers | 101         | 2                 |
| Meatlovers | 102         | 2                 |
| Vegetarian | 102         | 1                 |
| Meatlovers | 103         | 2                 |
| Vegetarian | 103         | 1                 |
| Meatlovers | 104         | 3                 |
| Vegetarian | 105         | 1                 |

#### f.	What was the maximum number of pizzas delivered in a single order?
  
               SELECT c.order_id
                ,COUNT(c.order_id) AS num_of_deliveries
               FROM customer_orders c
               JOIN runner_orders r ON c.order_id = r.order_id
               WHERE r.cancellation NOT IN (
                 "Restaurant Cancellation"
                 ,"Customer Cancellation"
                 )
               GROUP BY c.order_id
               ORDER BY num_of_deliveries DESC LIMIT 1;
  
| order_id | num_of_deliveries |
| -------- | ----------------- |
| 4        | 3                 |

#### g.	For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
  
              SELECT c.customer_id
               ,COUNT(c.order_id) AS num_of_orders
               ,SUM(CASE 
                 WHEN c.exclusions = ""
                  AND c.extras = ""
                  THEN 0
                 ELSE 1
                 END) AS num_of_changes
              FROM customer_orders c
              JOIN runner_orders r ON r.order_id = c.order_id
              WHERE r.cancellation NOT IN (
                "Restaurant Cancellation"
                ,"Customer Cancellation"
                )
              GROUP BY c.customer_id;

| customer_id | num_of_orders | num_of_changes |
| ----------- | ------------- | -------------- |
| 101         | 2             | 0              |
| 102         | 3             | 0              |
| 103         | 3             | 3              |
| 104         | 3             | 2              |
| 105         | 1             | 1              |

#### h.	How many pizzas were delivered that had both exclusions and extras?
  
           SELECT SUM(CASE 
              WHEN c.exclusions NOT IN ("")
               AND c.extras NOT IN ("")
               THEN 1
              ELSE 0
              END) AS exclusions_and_extras
           FROM customer_orders c
           JOIN runner_orders r ON r.order_id = c.order_id
           WHERE r.cancellation NOT IN (
             "Restaurant Cancellation"
             ,"Customer Cancellation"
             );
| exclusions_and_extras |
| --------------------- |
| 1                     |

#### i.	What was the total volume of pizzas ordered for each hour of the day?
  
           SELECT STRFTIME("%H", order_time) AS hour
            ,COUNT(order_id) AS num_of_orders
           FROM customer_orders
           GROUP BY hour
           ORDER BY hour;
| hour | num_of_orders |
| ---- | ------------- |
| 11   | 1             |
| 13   | 3             |
| 18   | 3             |
| 19   | 1             |
| 21   | 3             |
| 23   | 3             |

#### j.	What was the volume of orders for each day of the week?
  
         SELECT STRFTIME("%w", order_time) AS day --day of week 0-6 with Sunday==0
          ,COUNT(order_id) AS num_of_orders
         FROM customer_orders
         GROUP BY day
         ORDER BY day;
| day | num_of_orders |
| --- | ------------- |
| 3   | 5             |
| 4   | 3             |
| 5   | 1             |
| 6   | 5             |

 ### Runner and Customer Experience
  #### a.	How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
  
          SELECT registration_date
           ,COUNT(runner_id)
          FROM runners
          GROUP BY STRFTIME("%W", registration_date);
| registration_date | COUNT(runner_id) |
| ----------------- | ---------------- |
| 2021-01-01        | 2                |
| 2021-01-08        | 1                |
| 2021-01-15        | 1                |

#### b.	What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
  
           SELECT r.runner_id
            ,ROUND(AVG((STRFTIME("%M", pickup_time)) - (STRFTIME("%M", order_time)))) AS avg_arrival_time
           FROM runner_orders r
           JOIN customer_orders c ON r.order_id = c.order_id
           GROUP BY r.runner_id;
| runner_id | avg_arrival_time |
| --------- | ---------------- |
| 1         | -4               |
| 2         | 12               |
| 3         | 10               |

#### c.	Is there any relationship between the number of pizzas and how long the order takes to prepare?
  
          SELECT c.order_id
           ,COUNT(c.order_id) AS num_of_orders
           ,c.order_time
           ,r.pickup_time
           ,(STRFTIME("%M", pickup_time) - STRFTIME("%M", order_time)) AS avg_prep_time
          FROM runner_orders r
          JOIN customer_orders c ON r.order_id = c.order_id
          GROUP BY c.order_id
          ORDER BY num_of_orders DESC;
| order_id | num_of_orders | order_time          | pickup_time         | avg_prep_time |
| -------- | ------------- | ------------------- | ------------------- | ------------- |
| 4        | 3             | 2020-01-04 13:23:46 | 2020-01-04 13:53:03 | 30            |
| 10       | 2             | 2020-01-11 18:34:49 | 2020-01-11 18:50:20 | 16            |
| 3        | 2             | 2020-01-02 23:51:23 | 2020-01-03 00:12:37 | -39           |
| 9        | 1             | 2020-01-10 11:22:59 |                     |               |
| 8        | 1             | 2020-01-09 23:54:33 | 2020-01-10 00:15:02 | -39           |
| 7        | 1             | 2020-01-08 21:20:29 | 2020-01-08 21:30:45 | 10            |
| 6        | 1             | 2020-01-08 21:03:13 |                     |               |
| 5        | 1             | 2020-01-08 21:00:29 | 2020-01-08 21:10:57 | 10            |
| 2        | 1             | 2020-01-01 19:00:52 | 2020-01-01 19:10:54 | 10            |
| 1        | 1             | 2020-01-01 18:05:02 | 2020-01-01 18:15:34 | 10            |

#### d.	What was the average distance travelled for each customer?
  
              SELECT c.customer_id
               ,AVG(r.distance)
              FROM customer_orders c
              JOIN runner_orders r ON c.order_id = r.order_id
              WHERE r.cancellation NOT IN (
                "Restaurant Cancellation"
                ,"Customer Cancellation"
                )
              GROUP BY c.customer_id
              ORDER BY c.customer_id;
| customer_id | AVG(r.distance)    |
| ----------- | ------------------ |
| 101         | 20                 |
| 102         | 16.733333333333334 |
| 103         | 23.399999999999995 |
| 104         | 10                 |
| 105         | 25                 |

#### e.	What was the difference between the longest and shortest delivery times for all orders?
  
            SELECT max(duration) - min(duration) AS difference
            FROM runner_orders r
            WHERE r.cancellation NOT IN (
              "Restaurant Cancellation"
              ,"Customer Cancellation"
              );
| difference |
| ---------- |
| 30         |

#### f.	What was the average speed for each runner for each delivery and do you notice any trend for these values?
  
              SELECT r.runner_id
               ,c.customer_id
               ,c.order_id
               ,r.distance
               ,ROUND(r.duration / 60) AS duration_hour
               ,ROUND((distance / duration * 60)) AS avg_speed
              FROM customer_orders c
              JOIN runner_orders r ON c.order_id = r.order_id
              WHERE r.cancellation NOT IN (
                "Restaurant Cancellation"
                ,"Customer Cancellation"
                )
              GROUP BY r.runner_id
               ,c.customer_id
               ,c.order_id
               ,r.distance
               ,r.duration
              ORDER BY c.order_id;
| runner_id | customer_id | order_id | distance | duration_hour | avg_speed |
| --------- | ----------- | -------- | -------- | ------------- | --------- |
| 1         | 101         | 1        | 20       | 1             | 38        |
| 1         | 101         | 2        | 20       | 0             | 44        |
| 1         | 102         | 3        | 13.4     | 0             | 40        |
| 2         | 103         | 4        | 23.4     | 1             | 35        |
| 3         | 104         | 5        | 10       | 0             | 40        |
| 2         | 105         | 7        | 25       | 0             | 60        |
| 2         | 102         | 8        | 23.4     | 0             | 94        |
| 1         | 104         | 10       | 10       | 0             | 60        |

#### g.	What is the successful delivery percentage for each runner?
  
            SELECT runner_id
             ,ROUND(100 * COUNT(pickup_time) / COUNT(order_id)) AS delivery_percent
            FROM runner_orders
            GROUP BY runner_id
            ORDER BY runner_id;
| runner_id | delivery_percent |
| --------- | ---------------- |
| 1         | 100              |
| 2         | 100              |
| 3         | 100              |
