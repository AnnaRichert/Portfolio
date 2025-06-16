## Python
### :bar_chart: Project 1

Exploring Nobel Prize data from The Nobel Foundation. It contains all prize winners from the outset of the awards from 1901 to 2023.

The Data:

- **`nobel.com`**

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
print(nobel.head())
````

|year|category  |prize                                         |motivation                                                                                                                                                                                                                                        |prize_share|laureate_id|laureate_type|full_name                   |birth_date|birth_city       |birth_country   |sex |organization_name |organization_city|organization_country|death_date|death_city|death_country|
|----|----------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|-----------|-------------|----------------------------|----------|-----------------|----------------|----|------------------|-----------------|--------------------|----------|----------|-------------|
|1901|Chemistry |The Nobel Prize in Chemistry 1901             |"in recognition of the extraordinary services he has rendered by the discovery of the laws of chemical dynamics and osmotic pressure in solutions"                                                                                                |1/1        |160        |Individual   |Jacobus Henricus van 't Hoff|1852-08-30|Rotterdam        |Netherlands     |Male|Berlin University |Berlin           |Germany             |1911-03-01|Berlin    |Germany      |
|1901|Literature|The Nobel Prize in Literature 1901            |"in special recognition of his poetic composition, which gives evidence of lofty idealism, artistic perfection and a rare combination of the qualities of both heart and intellect"                                                               |1/1        |569        |Individual   |Sully Prudhomme             |1839-03-16|Paris            |France          |Male|null              |null             |null                |1907-09-07|Châtenay  |France       |
|1901|Medicine  |The Nobel Prize in Physiology or Medicine 1901|"for his work on serum therapy, especially its application against diphtheria, by which he has opened a new road in the domain of medical science and thereby placed in the hands of the physician a victorious weapon against illness and deaths"|1/1        |293        |Individual   |Emil Adolf von Behring      |1854-03-15|Hansdorf (Lawice)|Prussia (Poland)|Male|Marburg University|Marburg          |Germany             |1917-03-31|Marburg   |Germany      |
|1901|Peace     |The Nobel Peace Prize 1901                    |null                                                                                                                                                                                                                                              |1/2        |462        |Individual   |Jean Henry Dunant           |1828-05-08|Geneva           |Switzerland     |Male|null              |null             |null                |1910-10-30|Heiden    |Switzerland  |
|1901|Peace     |The Nobel Peace Prize 1901                    |null                                                                                                                                                                                                                                              |1/2        |463        |Individual   |Frédéric Passy              |1822-05-20|Paris            |France          |Male|null              |null             |null                |1912-06-12|Paris     |France       |



## :one: What is the most commonly awarded gender?

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Find the most common gender
top_gender=nobel['sex'].mode()
print(top_gender)
````
**Results**

|sex |
|----|
|Male|



## :two: What is the most commonly awarded birth country (using a different approach)?

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Find the most common birth country
top_country = nobel['birth_country'].value_counts().index[0]
print(top_country)
````
**Results**

'United States of America'



## :three: Which decade had the highest ratio of US-born Nobel Prize winners to total winners in all categories?

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Create a 'decade' column
nobel['decade'] = (nobel['year'] // 10) * 10
# Count total winners per decade
total_per_decade = nobel.groupby('decade').size()
# Count US-born winners per decade
us_per_decade = nobel[nobel['birth_country']=='United States of America'].groupby('decade').size()
# Combine results into a DataFrame
result = pd.DataFrame({
    'total_laureates': total_per_decade,
    'us_born_laureates': us_per_decade})
# Calculate the ratio and add it as a new column
result['us_ratio'] = result['us_born_laureates'] / result['total_laureates']
# Sort by ratio in descending order to see the highest at the top
result = result.sort_values('us_ratio', ascending=False)
result.head(1)
````
**Results**
|decade|total_laureates|us_born_laureates|us_ratio    |
|------|---------------|-----------------|------------|
|2000  |123            |52               |0.4227642276|



## :four: Which decade and Nobel Prize category combination had the highest proportion of female laureates? Store as a dictionary where the decade is the key and the category is the value. There should only be one key:value pair.

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Create a 'decade' column
nobel['decade'] = (nobel['year'] // 10) * 10
# Count total winners per decade
total = nobel.groupby(['decade','category']).size()
# Count female winners per decade
female = nobel[nobel['sex']=='Female'].groupby(['decade','category']).size()
# Combine results into a DataFrame
result = pd.DataFrame({
    'total_per_decade': total,
    'females': female})
# Calculate the ratio and add it as a new column
result['ratio']=result['females'] / result['total_per_decade']
# Find the index with the max value
max = result['ratio'].idxmax()
# Format as Dictionary
max_female_dict = {max[0]: max[1]}
print(max_female_dict)
````

**Results**

{2020: 'Literature'}



## :five: Who was the first woman to receive a Nobel Prize, in which year and category? Save the result in a string

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Filter for female laureates
women = nobel[nobel['sex']=='Female']
# Find the lowest value in column
first_woman = women[women['year']==women['year'].min()]
# Extract name, year and category
first_woman_name = first_woman['full_name'].iloc[0]
first_woman_year = first_woman['year'].iloc[0]
first_woman_category = first_woman['category'].iloc[0]
print(f"\n The first woman to win a Nobel Prize was {first_woman_name}, in {first_woman_year} in the category of {first_woman_category}.")

````

**Results**

The first woman to win a Nobel Prize was Marie Curie, née Sklodowska, in 1903 in the category of Physics.



## :six: Which individuals or organizations have won more than one Nobel Prize throughout the years? Save the result in a list.

````python
import pandas as pd
nobel = pd.read_csv('C:\Users\Arich\Documents\Python\Project_1\nobel.csv')
# Count how many times individuals and organisations appear
counts = nobel['full_name'].value_counts()
# Filter those who won more than 1
multiple_winners = counts[counts>1]
# Store the results in a list
repeat_list = list(multiple_winners.index)
print(repeat_list)
````

**Results**

['Comité international de la Croix Rouge (International Committee of the Red Cross)',
'Linus Carl Pauling',
'John Bardeen',
'Frederick Sanger',
'Marie Curie, née Sklodowska',
'Office of the United Nations High Commissioner for Refugees (UNHCR)']
