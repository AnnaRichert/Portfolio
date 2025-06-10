## SQL
### :bar_chart: Project 2

Exploring datasets from a hypothetical Super Store.

The Data:

- **`orders`**

|Column    |Definition                          |Data type       |Comments                                     |
|----------|------------------------------------|----------------|---------------------------------------------|
|row_id    |Unique Record ID                    |INTEGER         |                                             |
|order_id  |Identifier for each order in table  |TEXT            |Connects to order_id in returned_orders table|
|order_date|Date when order was placed          |TEXT            |                                             |
|market    |Market order_id belongs to          |TEXT            |                                             |
|region    |Region Customer belongs to          |TEXT            |Connects to region in people table           |
|product_id|Identifier of Product bought        |TEXT            |Connects to product_id in products table     |
|sales     |Total Sales Amount for the Line Item|DOUBLE PRECISION|                                             |
|quantity  |Total Quantity for the Line Item    |DOUBLE PRECISION|                                             |
|discount  |Discount applied for the Line Item  |DOUBLE PRECISION|                                             |
|profit    |Total Profit earned on the Line Item|DOUBLE PRECISION|                                             |


````sql
SELECT *
FROM orders
LIMIT 5
````

|row_id                        |order_id|order_date             |ship_date              |ship_mode     |customer_id|customer_name   |segment  |city         |state           |country      |postal_code|market|region        |product_id     |sales   |quantity|discount|profit  |shipping_cost|order_priority|
|------------------------------|--------|-----------------------|-----------------------|--------------|-----------|----------------|---------|-------------|----------------|-------------|-----------|------|--------------|---------------|--------|--------|--------|--------|-------------|--------------|
|957                           |MX-2014-105921|2014-05-28T00:00:00.000|2014-06-03T00:00:00.000|Standard Class|ZC-21910   |Zuschuss Carroll|Consumer |San Salvador |San Salvador    |El Salvador  |           |LATAM |Central       |TEC-AC-10004626|342.08  |2       |0       |0       |21.713       |Medium        |
|24359                         |ID-2013-61442|2013-01-15T00:00:00.000|2013-01-21T00:00:00.000|Standard Class|JB-16000   |Joy Bell-       |Consumer |Manila       |National Capital|Philippines  |           |APAC  |Southeast Asia|OFF-BI-10001400|122.4   |5       |0.15    |0       |21.71        |Low           |
|32298                         |CA-2012-124891|2012-07-31T00:00:00.000|2012-07-31T00:00:00.000|Same Day      |RH-19495   |Rick Hansen     |Consumer |New York City|New York        |United States|10024      |US    |East          |TEC-AC-10003033|2309.65 |7       |0       |762.1845|933.57       |Critical      |
|26341                         |IN-2013-77878|2013-02-05T00:00:00.000|2013-02-07T00:00:00.000|Second Class  |JR-16210   |Justin Ritter   |Corporate|Wollongong   |New South Wales |Australia    |           |APAC  |Oceania       |FUR-CH-10003950|3709.395|9       |0.1     |-288.765|923.63       |Critical      |
|25330                         |IN-2013-71249|2013-10-17T00:00:00.000|2013-10-18T00:00:00.000|First Class   |CR-12730   |Craig Reiter    |Consumer |Brisbane     |Queensland      |Australia    |           |APAC  |Oceania       |TEC-PH-10004664|5175.171|9       |0.1     |919.971 |915.49       |Medium        |



- **`returned_orders`**

|Column    |Definition                          |Data type       |
|----------|------------------------------------|----------------|
|returned  |Yes values for Order / Line Item Returned|TEXT            |
|order_id  |Identifier for each order in table  |TEXT            |
|market    |Market order_id belongs to          |TEXT            |

````sql
SELECT *
FROM returned_orders
LIMIT 5
````

|returned                      |order_id|market                 |
|------------------------------|--------|-----------------------|
|Yes                           |MX-2013-168137|LATAM                  |
|Yes                           |US-2011-165316|LATAM                  |
|Yes                           |ES-2013-1525878|EU                     |
|Yes                           |CA-2013-118311|United States          |
|Yes                           |ES-2011-1276768|EU                     |


- **`people`**

|Column    |Definition                          |Data type       |
|----------|------------------------------------|----------------|
|person    |Name of Salesperson credited with Order|TEXT            |
|region    |Region Salesperson in operating in  |TEXT            |

````sql
SELECT *
FROM people
LIMIT 5
````

|region                        |person|
|------------------------------|------|
|Central                       |Anna Andreadi|
|South                         |Chuck Magee|
|East                          |Kelly Williams|
|West                          |Matt Collister|
|Africa                        |Deborah Brumfield|


- **`products`**

|Column    |Definition                          |Data type       |
|----------|------------------------------------|----------------|
|product_id|Unique Identifier for the Product   |TEXT            |
|category  |Category Product belongs to         |TEXT            |
|sub_category|Sub Category Product belongs to     |TEXT            |
|product_name|Detailed Name of the Product        |TEXT            |


````sql
SELECT *
FROM products
LIMIT 5
````

|product_id                    |category|sub_category|product_name                                                      |
|------------------------------|--------|------------|------------------------------------------------------------------|
|TEC-AC-10003033               |Technology|Accessories |Plantronics CS510 - Over-the-Head monaural Wireless Headset System|
|FUR-CH-10003950               |Furniture|Chairs      |Novimex Executive Leather Armchair, Black                         |
|TEC-PH-10004664               |Technology|Phones      |Nokia Smart Phone, with Caller ID                                 |
|TEC-PH-10004583               |Technology|Phones      |Motorola Smart Phone, Cordless                                    |
|TEC-SHA-10000501              |Technology|Copiers     |Sharp Wireless Fax, High-Speed                                    |


## :one: Find the top 5 products from each category based on highest total sales. The output should be sorted by `category` in ascending order and by `sales` in descending order within each category, i.e. within each category product with highest margin should sit on the top.

````sql
WITH sub AS (
            SELECT p.category, p.product_name, 
                    ROUND (SUM(o.sales::NUMERIC),2) AS product_total_sales, 
                    ROUND (SUM(o.profit::NUMERIC),2) AS product_total_profit,
                    RANK() OVER(PARTITION BY p.category ORDER BY SUM(o.sales::NUMERIC) DESC) AS product_rank
            FROM orders AS o
            INNER JOIN products AS p
            ON o.product_id=p.product_id
            GROUP BY  p.category, p.product_name
            ORDER BY p.category, SUM(o.sales::NUMERIC) DESC)
SELECT *
FROM sub
WHERE product_rank <= 5
````

**Results**

|category  |product_name                        |product_total_sales|product_total_profit|product_rank|
|----------|------------------------------------|-------------------|--------------------|------------|
|Furniture |Hon Executive Leather Armchair, Adjustable|58193.48           |5997.25             |1           |
|Furniture |Office Star Executive Leather Armchair, Adjustable|51449.8            |4925.8              |2           |
|Furniture |Harbour Creations Executive Leather Armchair, Adjustable|50121.52           |10427.33            |3           |
|Furniture |SAFCO Executive Leather Armchair, Black|41923.53           |7154.28             |4           |
|Furniture |Novimex Executive Leather Armchair, Adjustable|40585.13           |5562.35             |5           |
|Office Supplies|Eldon File Cart, Single Width       |39873.23           |5571.26             |1           |
|Office Supplies|Hoover Stove, White                 |32842.6            |-2180.63            |2           |
|Office Supplies|Hoover Stove, Red                   |32644.13           |11651.68            |3           |
|Office Supplies|Rogers File Cart, Single Width      |29558.82           |2368.82             |4           |
|Office Supplies|Smead Lockers, Industrial           |28991.66           |3630.44             |5           |
|Technology|Apple Smart Phone, Full Size        |86935.78           |5921.58             |1           |
|Technology|Cisco Smart Phone, Full Size        |76441.53           |17238.52            |2           |
|Technology|Motorola Smart Phone, Full Size     |73156.3            |17027.11            |3           |
|Technology|Nokia Smart Phone, Full Size        |71904.56           |9938.2              |4           |
|Technology|Canon imageCLASS 2200 Advanced Copier|61599.82           |25199.93            |5           |


## :two: Calculate the quantity for orders with missing values in the `quantity` column by determining the unit price for each `product_id` using available order data, considering relevant pricing factors such as discount, market, or region. Then, use this unit price to estimate the missing quantity values. The calculated values should be stored in the `calculated_quantity` column.

````sql
WITH missing AS (
              SELECT product_id, discount, market, region, sales, quantity
              FROM orders 
              WHERE quantity IS NULL), 
unit_prices AS (
              SELECT o.product_id, CAST(o.sales / o.quantity AS NUMERIC) AS unit_price
              FROM orders AS o
              RIGHT JOIN missing AS m 
              ON o.product_id = m.product_id
              AND o.discount = m.discount
              WHERE o.quantity IS NOT NULL)
SELECT DISTINCT m.*, ROUND(CAST(m.sales AS NUMERIC) / up.unit_price,0) AS calculated_quantity
FROM missing AS m
INNER JOIN unit_prices AS up
ON m.product_id = up.product_id;
````
**Results**
|product_id|discount                            |market          |region  |sales|quantity|calculated_quantity|
|----------|------------------------------------|----------------|--------|-----|--------|-------------------|
|FUR-ADV-10000571|0                                   |EMEA            |EMEA    |438.96|        |4                  |
|FUR-ADV-10004395|0                                   |EMEA            |EMEA    |84.12|        |2                  |
|FUR-BO-10001337|0.15                                |US              |West    |308.499|        |3                  |
|TEC-STA-10003330|0                                   |Africa          |Africa  |506.64|        |2                  |
|TEC-STA-10004542|0                                   |Africa          |Africa  |160.32|        |4                  |

