## SQL
### :bar_chart: Project 4

Analyze the `manufacturing_parts` table and determine whether the manufacturing process is performing within acceptable control limits.

This acceptable range is defined by an upper control limit (UCL) and a lower control limit (LCL), the formulas for which are:

$ucl = avgheight + 3 * \frac{stddevheight}{\sqrt{5}}$

$lcl = avgheight - 3 * \frac{stddevheight}{\sqrt{5}}$

The UCL defines the highest acceptable height for the parts, while the LCL defines the lowest acceptable height for the parts. Ideally, parts should fall between the two limits.

The Data:

The data is available in the `manufacturing_parts` table which has the following fields:

- `item_no`: the item number
- `length`: the length of the item made
- `width`: the width of the item made
- `height`: the height of the item made
- `operator`: the operating machine

## :one: Create an alert that flags whether the height of a product is within the control limits for each `operator` using the formulas provided in the workbook. The final query should return the following fields: operator, row_number, height, avg_height, stddev_height, ucl, lcl, alert, and be ordered by the item_no.
The alert column will be your boolean flag.
Use a window function of length 5 to calculate the control limits, considering rows up to and including the current row; incomplete windows should be excluded from the final query output.

````sql
WITH sub AS (
	SELECT c.continent, MIN(year_founded) AS min_year
	FROM businesses AS b
	LEFT JOIN countries AS c
	ON b.country_code=c.country_code
	GROUP BY c.continent),
    sub1 AS (SELECT b.country_code, b.business, b.year_founded
            FROM businesses AS b
            RIGHT JOIN sub
            ON b.year_founded=sub.min_year)
SELECT c.continent, c.country, sub1.business, sub1.year_founded
FROM sub1
LEFT JOIN countries AS c
ON sub1.country_code=c.country_code
````
**Results**

|continent    |country                                    |business                   |year_founded|
|-------------|-------------------------------------------|---------------------------|------------|
|Oceania      |Australia                                  |Australia Post             |1809        |
|Europe       |Austria                                    |St. Peter Stifts Kulinarium|803         |
|Asia         |Japan                                      |Kongō Gumi                 |578         |
|North America|Mexico                                     |La Casa de Moneda de México|1534        |
|Africa       |Mauritius                                  |Mauritius Post             |1772        |
|South America|Peru                                       |Casa Nacional de Moneda    |1565        |

## :two: How many countries per continent lack data on the oldest businesses? Count the number of countries per continent missing business data, including `new_businesses`

````sql
WITH sub AS (
	SELECT country_code
	FROM countries
	WHERE country_code NOT IN (SELECT country_code
				FROM businesses)
            AND country_code NOT IN (SELECT country_code
				FROM new_businesses))
SELECT continent, COUNT(sub.country_code) AS countries_without_businesses
FROM sub
LEFT JOIN countries AS c
ON sub.country_code=c.country_code
GROUP BY continent
ORDER BY continent
````
**Results**
|continent    |countries_without_businesses               |
|-------------|-------------------------------------------|
|Africa       |3                                          |
|Asia         |7                                          |
|Europe       |2                                          |
|North America|5                                          |
|Oceania      |10                                         |
|South America|3                                          |

## :three: Which business categories are best suited to last many years, and on what continent are they? Store your answer with the oldest founding year for each continent and category combination.

````sql
WITH sub AS (
	SELECT MIN(year_founded) AS year_founded,category_code, continent
	FROM businesses AS b
	LEFT JOIN countries AS c
	ON b.country_code=c.country_code
	GROUP BY category_code, continent
	ORDER BY MIN(year_founded))
SELECT sub.continent, c.category, sub.year_founded
FROM sub
LEFT JOIN categories AS c
ON sub.category_code=c.category_code
ORDER BY sub.continent, c.category
````
**Results**

$ucl = avgheight + 3 * \frac{stddevheight}{\sqrt{5}}$

$lcl = avgheight - 3 * \frac{stddevheight}{\sqrt{5}}$
