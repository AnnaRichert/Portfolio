## Python
### :bar_chart: Project 2

Exploring dataset called `schools.csv` and answering questions about New York City (NYC) public school SAT performance.

The Data:

- **`schools.csv`**


````python
import pandas as pd
schools = pd.read_csv('C:\Users\Arich\Documents\Python\Project_2\schools.csv')
schools.head()
````

|school_name                                   |borough|building_code|average_math|average_reading|average_writing|percent_tested|
|----------------------------------------------|-------|-------------|------------|---------------|---------------|--------------|
|New Explorations into Science Technology and Math High School|Manhattan    |M022        |657            |601            |601           |
|Essex Street Academy                          |Manhattan|M445         |395         |411            |387            |78.9          |
|Lower Manhattan Arts Academy                  |Manhattan|M445         |418         |428            |415            |65.1          |
|High School for Dual Language and Asian Studies|Manhattan|M445         |613         |453            |463            |95.9          |
|Henry Street School for International Studies |Manhattan|M056         |410         |406            |381            |59.7          |


## :one: Which NYC schools have the best math results?
- The best math results are at least 80% of the *maximum possible score of 800* for math.
- Saving the results in a pandas DataFrame called `best_math_schools`, including `"school_name"` and `"average_math"` columns, sorted by `"average_math"` in descending order.


````python
import pandas as pd
schools = pd.read_csv('C:\Users\Arich\Documents\Python\Project_2\schools.csv')
# Filter schools having at least 0.8*800=640 and sorting the results
best_schools = schools.query('average_math >= 640').sort_values('average_math', ascending= False)
# Select schools' names and score
best_math_schools = best_schools[['school_name', 'average_math']]
print(best_math_schools)
````
**Results**

|school_name                                   |average_math|
|----------------------------------------------|------------|
|Stuyvesant High School                        |754         |
|Bronx High School of Science                  |714         |
|Staten Island Technical High School           |711         |
|Queens High School for the Sciences at York College|701         |
|High School for Mathematics Science and Engineering at City College|683   |
|Brooklyn Technical High School                |682         |
|Townsend Harris High School                   |680         |
|High School of American Studies at Lehman College|669         |
|New Explorations into Science Technology and Math High School|657   |
|Eleanor Roosevelt High School                 |641         |


## :two: What are the top 10 performing schools based on the combined SAT scores?
- Saving the results as a pandas DataFrame called `top_10_schools` containing the `"school_name"` and a new column named `"total_SAT"`, with results ordered by `"total_SAT"` in descending order (`"total_SAT"` being the sum of math, reading, and writing scores).

````python
import pandas as pd
schools = pd.read_csv('C:\Users\Arich\Documents\Python\Project_2\schools.csv')
# Create new column which is a sum of total scores
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']
# Sort by the new column
schools_sorted = schools.sort_values('total_SAT', ascending = False)
# Select top 10 schools
top_10_schools=schools_sorted[['school_name', 'total_SAT']].head(10)
print(top_10_schools)
````
**Results**

|school_name                                   |total_SAT|
|----------------------------------------------|---------|
|Stuyvesant High School                        |2144     |
|Bronx High School of Science                  |2041     |
|Staten Island Technical High School           |2041     |
|High School of American Studies at Lehman College|2013     |
|Townsend Harris High School                   |1981     |
|Queens High School for the Sciences at York College|1947     |
|Bard High School Early College                |1914     |
|Brooklyn Technical High School                |1896     |
|Eleanor Roosevelt High School                 |1889     |
|High School for Mathematics Science and Engineering at City College|1889  |


## :three: Which single borough has the largest standard deviation in the combined SAT score?
- Saving the results as a pandas DataFrame called largest_std_dev.
- The DataFrame should contain one row, with:
-`"borough"` - the name of the NYC borough with the largest standard deviation of `"total_SAT"`.
-`"num_schools"` - the number of schools in the borough.
-`"average_SAT"` - the mean of `"total_SAT"`.
-`"std_SAT"` - the standard deviation of `"total_SAT"`.
 - Rounding all numeric values to two decimal places.


````python
import pandas as pd
schools = pd.read_csv('C:\Users\Arich\Documents\Python\Project_2\schools.csv')
# Create new column which is a sum of total scores
schools['total_SAT'] = schools['average_math'] + schools['average_reading'] + schools['average_writing']
# Aggregate by borough: count, mean, std of total SAT, rounded to 2 decimals
schools_agg = schools.groupby ('borough') ['total_SAT'].agg(['count', 'mean', 'std']).round(2)
# Sort boroughs by standard deviation (descending)
schools_sorted = schools_agg.sort_values('std', ascending = False)
# Rename columns for clarity
schools_sorted['num_schools'] = schools_sorted['count']
schools_sorted['average_SAT'] = schools_sorted['mean']
schools_sorted['std_SAT'] = schools_sorted['std']
# Select relevant columns
largest_std_dev= schools_sorted[['num_schools', 'average_SAT', 'std_SAT']]
print(largest_std_dev.head(1))
````
**Results**
|borough                                       |num_schools|average_SAT|std_SAT|
|----------------------------------------------|-----------|-----------|-------|
|Manhattan                                     |89         |1340.13    |230.29 |
