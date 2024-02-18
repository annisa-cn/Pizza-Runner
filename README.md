# Pizza-Runner
a dataset about pizza runners and its customer purchased

making database to store the information
```
CREATE DATABASE [Pizza Runner]
USE [Pizza Runner]
```

create table runners and input data to the table
```
CREATE TABLE runners (
  "runner_id" INTEGER,
  "registration_date" DATE
)

INSERT INTO runners ("runner_id", "registration_date") VALUES
  (1, '2021-01-01'),
  (2, '2021-01-03'),
  (3, '2021-01-08'),
  (4, '2021-01-15')
```

create table Customer Orders and input data to the table
```
CREATE TABLE customer_orders (
  "order_id" INTEGER,
  "customer_id" INTEGER,
  "pizza_id" INTEGER,
  "exclusions" VARCHAR(4),
  "extras" VARCHAR(4),
  "order_time" DATETIME
)

INSERT INTO customer_orders
  ("order_id", "customer_id", "pizza_id", "exclusions", "extras", "order_time")
VALUES
  ('1', '101', '1', '', '', '2020-01-01 18:05:02'),
  ('2', '101', '1', '', '', '2020-01-01 19:00:52'),
  ('3', '102', '1', '', '', '2020-01-02 23:51:23'),
  ('3', '102', '2', '', NULL, '2020-01-02 23:51:23'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '1', '4', '', '2020-01-04 13:23:46'),
  ('4', '103', '2', '4', '', '2020-01-04 13:23:46'),
  ('5', '104', '1', 'null', '1', '2020-01-08 21:00:29'),
  ('6', '101', '2', 'null', 'null', '2020-01-08 21:03:13'),
  ('7', '105', '2', 'null', '1', '2020-01-08 21:20:29'),
  ('8', '102', '1', 'null', 'null', '2020-01-09 23:54:33'),
  ('9', '103', '1', '4', '1, 5', '2020-01-10 11:22:59'),
  ('10', '104', '1', 'null', 'null', '2020-01-11 18:34:49'),
  ('10', '104', '1', '2, 6', '1, 4', '2020-01-11 18:34:49')
```

create table Runner Orders and input data to the table
```
CREATE TABLE runner_orders (
  "order_id" INTEGER,
  "runner_id" INTEGER,
  "pickup_time" VARCHAR(19),
  "distance" VARCHAR(7),
  "duration" VARCHAR(10),
  "cancellation" VARCHAR(23)
)

INSERT INTO runner_orders
  ("order_id", "runner_id", "pickup_time", "distance", "duration", "cancellation")
VALUES
  ('1', '1', '2020-01-01 18:15:34', '20km', '32 minutes', ''),
  ('2', '1', '2020-01-01 19:10:54', '20km', '27 minutes', ''),
  ('3', '1', '2020-01-03 00:12:37', '13.4km', '20 mins', NULL),
  ('4', '2', '2020-01-04 13:53:03', '23.4', '40', NULL),
  ('5', '3', '2020-01-08 21:10:57', '10', '15', NULL),
  ('6', '3', 'null', 'null', 'null', 'Restaurant Cancellation'),
  ('7', '2', '2020-01-08 21:30:45', '25km', '25mins', 'null'),
  ('8', '2', '2020-01-10 00:15:02', '23.4 km', '15 minute', 'null'),
  ('9', '2', 'null', 'null', 'null', 'Customer Cancellation'),
  ('10', '1', '2020-01-11 18:50:20', '10km', '10minutes', 'null')
```

create table Pizza Names and input data to the table
```
Pizza Names
CREATE TABLE pizza_names (
  "pizza_id" INTEGER,
  "pizza_name" TEXT
)

INSERT INTO pizza_names
  ("pizza_id", "pizza_name")
VALUES
  (1, 'Meatlovers'),
  (2, 'Vegetarian')
```

create table Pizza Recipes and input data to the table
```
--Pizza Recipes
CREATE TABLE pizza_recipes (
  "pizza_id" INTEGER,
  "toppings" TEXT
)

INSERT INTO pizza_recipes
  ("pizza_id", "toppings")
VALUES
  (1, '1, 2, 3, 4, 5, 6, 8, 10'),
  (2, '4, 6, 7, 9, 11, 12')
```

create table Pizza Toppings and input data to the table
```
CREATE TABLE pizza_toppings (
  "topping_id" INTEGER,
  "topping_name" TEXT
)

INSERT INTO pizza_toppings
  ("topping_id", "topping_name")
VALUES
  (1, 'Bacon'),
  (2, 'BBQ Sauce'),
  (3, 'Beef'),
  (4, 'Cheese'),
  (5, 'Chicken'),
  (6, 'Mushrooms'),
  (7, 'Onions'),
  (8, 'Pepperoni'),
  (9, 'Peppers'),
  (10, 'Salami'),
  (11, 'Tomatoes'),
  (12, 'Tomato Sauce')
```

Cleaning dataset customer_order
```
  DROP TABLE IF EXISTS #temp_customer_order
  CREATE TABLE #temp_customer_order (
  "order_id" INTEGER,
  "customer_id" INTEGER,
  "pizza_id" INTEGER,
  "exclusions" VARCHAR(4),
  "extras" VARCHAR(4),
  "order_time" DATETIME
  )

  INSERT INTO #temp_customer_order
  SELECT * FROM customer_orders

  UPDATE #temp_customer_order
  SET exclusions = NULL
  WHERE exclusions = 'null' OR exclusions = ''

  UPDATE #temp_customer_order
  SET extras = NULL
  WHERE extras = '' OR extras = 'null'
```

Cleaning dataset runner_order
```
  DROP TABLE IF EXISTS #temp_runner_order
  CREATE TABLE #temp_runner_order (
  "order_id" INTEGER,
  "runner_id" INTEGER,
  "pickup_time" VARCHAR(19),
  "distance" VARCHAR(7),
  "duration" VARCHAR(10),
  "cancellation" VARCHAR(23)
  )

  INSERT INTO #temp_runner_order
  SELECT * FROM runner_orders

  UPDATE #temp_runner_order
  SET pickup_time = NULL
  WHERE pickup_time = 'null'

  UPDATE #temp_runner_order
  SET distance = NULL
  WHERE distance = 'null'

  UPDATE #temp_runner_order
  SET duration = NULL
  WHERE duration = 'null'

  UPDATE #temp_runner_order
  SET cancellation = NULL
  WHERE cancellation = '' OR cancellation = 'null'
```

QUESTIONS:

--How many pizzas were ordered?
	SELECT COUNT(order_id) AS OrderedNumber
	FROM #temp_customer_order

--How many unique customer orders were made?
	SELECT COUNT (DISTINCT order_id) AS CustOrder
	FROM #temp_customer_order

--How many successful orders were delivered by each runner?
	SELECT runner_id, COUNT(order_id) AS order_count
	FROM #temp_runner_order
	WHERE duration IS NOT NULL
	GROUP BY runner_id

--How many of each type of pizza was delivered?
	
	ALTER TABLE pizza_names
	ALTER COLUMN pizza_name VARCHAR(50)

	SELECT pn.pizza_name, COUNT(tco.customer_id) AS Qty
	FROM #temp_customer_order tco
	INNER JOIN pizza_names pn
	ON tco.pizza_id = pn.pizza_id
	INNER JOIN #temp_runner_order tro
	ON tco.order_id = tro.order_id
	WHERE NOT tro.duration = 'null'
	GROUP BY pizza_name

--How many Vegetarian and Meatlovers were ordered by each customer?
	SELECT tco.customer_id, pn.pizza_name, COUNT(tco.customer_id) AS no_of_pizza
	FROM #temp_customer_order tco
	JOIN pizza_names pn
		ON tco.pizza_id = pn.pizza_id
	GROUP BY tco.customer_id, pn.pizza_name
	ORDER BY tco.customer_id

--What was the maximum number of pizzas delivered in a single order?
	DROP TABLE IF EXISTS #temp_orderXrunner
	SELECT co.order_id, co.customer_id, co.pizza_id, co.exclusions, co.extras, co.order_time
		, ro.runner_id, ro.pickup_time, ro.distance, ro.duration, ro.cancellation
	INTO #temp_orderXrunner
	FROM #temp_customer_order co
	JOIN #temp_runner_order ro
		ON co.order_id = ro.order_id
	WHERE duration IS NOT NULL

	--removing counters from distance & duration column
	UPDATE #temp_orderXrunner
	SET distance = REPLACE(distance, 'km', '')
	WHERE distance LIKE '%km'

	UPDATE #temp_orderXrunner
	SET distance = TRIM(distance)

	UPDATE #temp_orderXrunner
	SET duration = LEFT(duration, 2)
	WHERE duration LIKE '%min%'

	--chaning datatype for column distance & duration in #temp_orderXrunner
	ALTER TABLE #temp_orderXrunner
	ALTER COLUMN distance NUMERIC

	ALTER TABLE #temp_orderXrunner
	ALTER COLUMN duration NUMERIC

	SELECT * FROM #temp_orderXrunner

	SELECT TOP(1) order_id, COUNT(customer_id) AS no_of_pizza
	FROM #temp_orderXrunner
	GROUP BY order_id
	ORDER BY no_of_pizza DESC

--For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
	SELECT customer_id,  
		COUNT(
		CASE
		WHEN exclusions IS NOT NULL OR extras IS NOT NULL THEN 1
		END) AS changed,
		COUNT(
		CASE
		WHEN exclusions IS NULL AND extras IS NULL THEN 1
		END) AS no_changed
	FROM #temp_orderXrunner
	GROUP BY customer_id

--How many pizzas were delivered that had both exclusions and extras?
	SELECT COUNT(customer_id) AS [pizza with exclusions & extras]
	FROM #temp_orderXrunner	
	WHERE exclusions IS NOT NULL AND extras IS NOT NULL

--What was the total volume of pizzas ORDERED for each hour of the day?
	SELECT DATEPART(hour, order_time) AS [hour], COUNT(order_id) AS no_of_pizza
	FROM #temp_customer_order
	GROUP BY DATEPART(hour, order_time)

--What was the volume of orders for each day of the week?
	SELECT DATENAME(weekday, order_time) AS [day], COUNT(order_id) AS no_of_pizza
	FROM #temp_customer_order
	GROUP BY DATENAME(weekday, order_time)

	--DATEPART(interval, date) returns a specified part of a date as an integer value
	--DATENAME(interval, date) returns a specified part of a date as a string value

--How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
	SELECT DATEPART(week, registration_date) AS [week], COUNT(runner_id) AS #runners_registered
	FROM runners
	GROUP BY DATEPART(week, registration_date)

--What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?
	SELECT runner_id, AVG(DATEDIFF(minute, order_time, pickup_time)) AS [AVG_time_to_pickup_order]
	FROM #temp_orderXrunner
	GROUP BY runner_id

--Is there any relationship between the number of pizzas and how long the order takes to prepare?
	SELECT order_id, COUNT(pizza_id) AS no_of_pizza, AVG(DATEDIFF(minute, order_time, pickup_time)) AS [AVG_time_to_pickup_order]
	FROM #temp_orderXrunner
	GROUP BY order_id

	-- assumed that preparation time = pick up time to the HQ
	-- the greater the number of pizzas in one order, the longer the preparation time takes

--What was the average distance travelled for each customer?
	SELECT customer_id, AVG(CAST(distance AS NUMERIC)) AS [AVG_Distance]
	FROM #temp_orderXrunner
	GROUP BY customer_id

--What was the difference between the longest and shortest delivery times for all orders?
	SELECT MAX(duration) - MIN(duration) AS time_diff
	FROM #temp_orderXrunner

--What was the average speed for each runner for each delivery and do you notice any trend for these values?
	SELECT runner_id, order_id, distance, duration, AVG((distance / (duration/60))) AS [avg_speed (km/h)]
	FROM #temp_orderXrunner
	GROUP BY runner_id, order_id, distance, duration
	ORDER BY runner_id, distance

	--runners tend to use higher speed in short distance delivery, leading to shorter delivery time

--What is the successful delivery percentage for each runner?
	SELECT tro.runner_id, (CAST(COUNT(duration) AS decimal(3,2)) / CAST(COUNT(tco.order_id) AS decimal(3,2)) *100) AS [delivery_perc]
	FROM #temp_customer_order tco
	JOIN #temp_runner_order tro
		ON tco.order_id = tro.order_id
		GROUP BY tro.runner_id

	--COUNT of duration (delivered order) and order_id needs to be converted first so that the division can comes out in decimal

--join all of the information together to form a table which has the following information for successful deliveries?
--customer_id, order_id, runner_id, order_time, pickup_time, time between order and pickup, delivery duration, average speed, total number of pizzas
	CREATE VIEW [Success Delivery] AS
	SELECT co.customer_id, co.order_id, ro.runner_id, co.order_time, ro.pickup_time, 
			DATEDIFF(minute, order_time, pickup_time) AS [order-pickup],
			ro.duration, 
			AVG (CAST(TRIM(REPLACE(distance, 'km', '')) AS numeric) / 
				CAST(LEFT(ro.duration,2) AS numeric) / 60 ) 
			AS [avg_speed],
			COUNT(co.pizza_id) AS no_of_pizza
	FROM customer_orders co
	JOIN runner_orders ro
		ON co.order_id = ro.order_id
	WHERE ro.duration <> 'null'
	GROUP BY co.customer_id, co.order_id, ro.runner_id, co.order_time, ro.pickup_time, ro.duration

	SELECT * FROM [Success Delivery]
	


