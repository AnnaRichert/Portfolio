## Python
### :bar_chart: Project 1

Exploring Nobel Prize data from The Nobel Foundation. It contains all prize winners from the outset of the awards from 1901 to 2023.

The Data:

- **`nobel`**

````python
import pandas as pd
nobel = pd.read_csv("C:\Users\Arich\Documents\Project_1\nobel.csv")
nobel.head()
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
nobel = pd.read_csv("C:\Users\Arich\Documents\Project_1\nobel.csv")
top_gender=nobel['sex'].mode()
top_gender
````
**Results**

|sex |
|----|
|Male|

## :two: How many matches finished with a point difference greater than 40?

````python
import pandas as pd
from matplotlib import pyplot as plt
super_bowls = pd.read_csv("C:\Users\Arich\Documents\Project_1\super_bowls.csv")
matches=super_bowls[ (super_bowls['difference_pts'] > 40) ]
count=matches['difference_pts'].count()
print(count)
````
**Results**

1


## :three: Who performed the most songs in Super Bowl halftime shows?

````python
import pandas as pd
from matplotlib import pyplot as plt
halftime_musicians = pd.read_csv("C:\Users\Arich\Documents\Project_1\halftime_musicians.csv")
halftime_appearance = halftime_musicians.groupby('musician').sum('num_songs')
halftime_appearance = halftime_appearance.sort_values('num_songs', ascending= False)
halftime_appearance.head(10)
````
**Results**
|musician                                      |super_bowl|num_songs|
|----------------------------------------------|----------|---------|
|Justin Timberlake                             |90        |12       |
|Beyoncé                                       |97        |10       |
|Diana Ross                                    |30        |10       |
|Grambling State University Tiger Marching Band|79        |9        |
|Bruno Mars                                    |98        |9        |
|Katy Perry                                    |49        |8        |
|Spirit of Troy                                |43        |8        |
|The Florida State University Marching Chiefs  |18        |7        |
|Lady Gaga                                     |51        |7        |
|Prince                                        |41        |7        |


