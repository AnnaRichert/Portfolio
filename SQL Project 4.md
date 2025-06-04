## SQL
### :bar_chart: Project 3

Exploring database `unicorns` to analyze trends in high-growth companies (unicorns).

The Data:

- `dates`

|Column    |Description                         |
|----------|------------------------------------|
|company_id|A unique ID for the company.        |
|date_joined|The date that the company became a unicorn.|
|year_founded|The year that the company was founded.|


- `funding`

|Column    |Description                         |
|----------|------------------------------------|
|company_id|A unique ID for the company.        |
|valuation |Company value in US dollars.        |
|funding   |The amount of funding raised in US dollars.|
|select_investors|A list of key investors in the company.|


- `industries`

|Column    |Description                         |
|----------|------------------------------------|
|company_id|A unique ID for the company.        |
|industry  |The industry that the company operates in.|

- `companies`

|Column    |Description                         |
|----------|------------------------------------|
|company_id|A unique ID for the company.        |
|company   |The name of the company.            |
|city      |The city where the company is headquartered.|
|country   |The country where the company is headquartered.|
|continent |The continent where the company is headquartered.|


### :one: First, we need to identify the three best-performing industries based on the number of new unicorns created in 2019, 2020, and 2021 combined.

### :two: Second, from those industries (1), we need to find the number of unicorns within these industries (2), the year that they became a unicorn (3), and their average valuation, converted to billions of dollars and rounded to two decimal places (4).

### :three: Third, with the above information we then finish the query to return a table containing `industry`, `year`, `num_unicorns`, and `average_valuation_billions`. For readability, we sort the results by year and number of unicorns, both in descending order.

````sql
WITH sub AS (
          SELECT industry, COUNT(company_id) 
          FROM dates AS d
          LEFT JOIN industries AS i
          USING (company_id)
          WHERE DATE_PART('year', date_joined) between 2019 AND 2021
          GROUP by industry
          ORDER BY COUNT(company_id)  DESC
          LIMIT 3),
sub1 AS (
          SELECT d.company_id, DATE_PART('year', date_joined) AS year, valuation, industry
          FROM dates AS d
          LEFT JOIN funding AS f
          USING (company_id)
          LEFT JOIN industries AS i
          USING (company_id)
          WHERE industry IN (SELECT sub.industry
                            FROM sub)
          AND DATE_PART('year', date_joined) between 2019 AND 2021)
SELECT sub1.industry, sub1.year, COUNT(sub1.company_id) AS num_unicorns, ROUND(AVG(sub1.valuation)/1000000000,2) AS average_valuation_billions
FROM sub1
GROUP BY sub1.industry, sub1.year
ORDER BY sub1.year DESC, COUNT(sub1.company_id) DESC
````
**Results**

|industry  |year                                |num_unicorns|average_valuation_billions|
|----------|------------------------------------|------------|--------------------------|
|Fintech   |2021                                |138         |2.75                      |
|Internet software & services|2021                                |119         |2.15                      |
|E-commerce & direct-to-consumer|2021                                |47          |2.47                      |
|Internet software & services|2020                                |20          |4.35                      |
|E-commerce & direct-to-consumer|2020                                |16          |4                         |
|Fintech   |2020                                |15          |4.33                      |
|Fintech   |2019                                |20          |6.8                       |
|Internet software & services|2019                                |13          |4.23                      |
|E-commerce & direct-to-consumer|2019                                |12          |2.58                      |
