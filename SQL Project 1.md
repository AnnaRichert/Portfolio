## SQL
### :bar_chart: Project 1

Exploring datasets from BusinessFinancing.co.uk regarding the oldest companies in business in almost every country.

The Data:

- **`businesses`** and **`new_businesses`**

|Column       |Description                                |
|-------------|-------------------------------------------|
|business     |Name of the business (varchar)             |
|year_founded |Year the business was founded (int)        |
|category_code|Code for the business category (varchar)   |
|country_code |ISO 3166-1 three-letter country code (char)|

````sql
SELECT *
FROM businesses
LIMIT 5
````

|business                      |year_founded|category_code|country_code|
|------------------------------|------------|-------------|------------|
|Hamoud Boualem                |1878        |CAT11        |DZA         |
|Communauté Électrique du Bénin|1968        |CAT10        |BEN         |
|Botswana Meat Commission      |1965        |CAT1         |BWA         |
|Air Burkina                   |1967        |CAT2         |BFA         |
|Brarudi                       |1955        |CAT9         |BDI         |

````sql
SELECT *
FROM new_businesses
````

|business                      |year_founded|category_code|country_code|
|------------------------------|------------|-------------|------------|
|Fiji Times                    |1869        |CAT13        |FJI         |
|J. Armando Bermúdez & Co.     |1852        |CAT9         |DOM         |



- **`countries`**

|Column       |Description                                |
|-------------|-------------------------------------------|
|country_code |ISO 3166-1 three-letter country code (varchar)|
|country      |Name of the country (varchar)              |
|continent    |Name of the continent the country exists in (varchar)|

````sql
SELECT *
FROM countries
LIMIT 5
````

|country_code                  |country|continent|
|------------------------------|-------|---------|
|AFG                           |Afghanistan|Asia     |
|AGO                           |Angola |Africa   |
|ALB                           |Albania|Europe   |
|AND                           |Andorra|Europe   |
|ARE                           |United Arab Emirates|Asia     |


- **`categories`**

|Column       |Description                                |
|-------------|-------------------------------------------|
|category_code|Code for the business category (varchar)   |
|category     |Description of the business category (varchar)|

````sql
SELECT *
FROM categories
LIMIT 5
````
|category_code                 |category|
|------------------------------|--------|
|CAT1                          |Agriculture|
|CAT2                          |Aviation & Transport|
|CAT3                          |Banking & Finance|
|CAT4                          |Cafés, Restaurants & Bars|
|CAT5                          |Conglomerate|


## :one: What is the oldest business on each continent?

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

## :three: Which business categories are best suited to last many years, and on what continent are they? Store the answer with the oldest founding year for each continent and category combination.

````sql
WITH sub AS (
	SELECT MIN(year_founded) AS year_founded, category_code, continent
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
|continent    |category                                   |year_founded|
|-------------|-------------------------------------------|------------|
|Africa       |Agriculture                                |1947        |
|Africa       |Aviation & Transport                       |1854        |
|Africa       |Banking & Finance                          |1892        |
|Africa       |Distillers, Vintners, & Breweries          |1933        |
|Africa       |Energy                                     |1968        |
|Africa       |Food & Beverages                           |1878        |
|Africa       |Manufacturing & Production                 |1820        |
|Africa       |Media                                      |1943        |
|Africa       |Mining                                     |1962        |
|Africa       |Postal Service                             |1772        |
|Asia         |Agriculture                                |1930        |
|Asia         |Aviation & Transport                       |1858        |
|Asia         |Banking & Finance                          |1830        |
|Asia         |Cafés, Restaurants & Bars                  |1153        |
|Asia         |Conglomerate                               |1841        |
|Asia         |Construction                               |578         |
|Asia         |Defense                                    |1808        |
|Asia         |Distillers, Vintners, & Breweries          |1853        |
|Asia         |Energy                                     |1928        |
|Asia         |Food & Beverages                           |1820        |
|Asia         |Manufacturing & Production                 |1736        |
|Asia         |Media                                      |1931        |
|Asia         |Mining                                     |1913        |
|Asia         |Postal Service                             |1800        |
|Asia         |Retail                                     |1883        |
|Asia         |Telecommunications                         |1885        |
|Asia         |Tourism & Hotels                           |1584        |
|Europe       |Agriculture                                |1218        |
|Europe       |Banking & Finance                          |1606        |
|Europe       |Cafés, Restaurants & Bars                  |803         |
|Europe       |Consumer Goods                             |1649        |
|Europe       |Defense                                    |1878        |
|Europe       |Distillers, Vintners, & Breweries          |862         |
|Europe       |Manufacturing & Production                 |864         |
|Europe       |Media                                      |1999        |
|Europe       |Medical                                    |1422        |
|Europe       |Mining                                     |1248        |
|Europe       |Postal Service                             |1520        |
|Europe       |Telecommunications                         |1912        |
|Europe       |Tourism & Hotels                           |1230        |
|North America|Agriculture                                |1638        |
|North America|Aviation & Transport                       |1870        |
|North America|Banking & Finance                          |1891        |
|North America|Distillers, Vintners, & Breweries          |1703        |
|North America|Food & Beverages                           |1920        |
|North America|Manufacturing & Production                 |1534        |
|North America|Media                                      |1909        |
|North America|Retail                                     |1670        |
|North America|Tourism & Hotels                           |1770        |
|Oceania      |Banking & Finance                          |1861        |
|Oceania      |Postal Service                             |1809        |
|South America|Banking & Finance                          |1565        |
|South America|Cafés, Restaurants & Bars                  |1877        |
|South America|Defense                                    |1811        |
|South America|Food & Beverages                           |1660        |
|South America|Manufacturing & Production                 |1621        |



