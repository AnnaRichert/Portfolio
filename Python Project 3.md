## Python
### :bar_chart: Project 1

Analyzing Los Angeles Police Department (LAPD) datasets of crime data to identify patterns in criminal behavior.

The Data:

1. crimes.csv

|Column                            |Description                                                                                                                                                                                                                                                                                                          |
|----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|'DR_NO'                           |Division of Records Number: Official file number made up of a 2-digit year, area ID, and 5 digits.                                                                                                                                                                                                                   |
|'Date Rptd'                       |Date reported - MM/DD/YYYY.                                                                                                                                                                                                                                                                                          |
|'DATE OCC'                        |Date of occurrence - MM/DD/YYYY.                                                                                                                                                                                                                                                                                     |
|'TIME OCC'                        |In 24-hour military time.                                                                                                                                                                                                                                                                                            |
|'AREA NAME'                       |The 21 Geographic Areas or Patrol Divisions are also given a name designation that references a landmark or the surrounding community that it is responsible for. For example, the 77th Street Division is located at the intersection of South Broadway and 77th Street, serving neighborhoods in South Los Angeles.|
|'Crm Cd Desc'                     |Indicates the crime committed.                                                                                                                                                                                                                                                                                       |
|'Vict Age'                        |Victim's age in years.                                                                                                                                                                                                                                                                                               |
|'Vict Sex'                        |Victim's sex: F: Female, M: Male, X: Unknown.                                                                                                                                                                                                                                                                        |
|'Vict Descent'                    |Victim's descent:                                                                                                                                                                                                                                                                                                    |
|A - Other Asian                   |                                                                                                                                                                                                                                                                                                                     |
|B - Black                         |                                                                                                                                                                                                                                                                                                                     |
|C - Chinese                       |                                                                                                                                                                                                                                                                                                                     |
|D - Cambodian                     |                                                                                                                                                                                                                                                                                                                     |
|F - Filipino                      |                                                                                                                                                                                                                                                                                                                     |
|G - Guamanian                     |                                                                                                                                                                                                                                                                                                                     |
|H - Hispanic/Latin/Mexican        |                                                                                                                                                                                                                                                                                                                     |
|I - American Indian/Alaskan Native|                                                                                                                                                                                                                                                                                                                     |
|J - Japanese                      |                                                                                                                                                                                                                                                                                                                     |
|K - Korean                        |                                                                                                                                                                                                                                                                                                                     |
|L - Laotian                       |                                                                                                                                                                                                                                                                                                                     |
|O - Other                         |                                                                                                                                                                                                                                                                                                                     |
|P - Pacific Islander              |                                                                                                                                                                                                                                                                                                                     |
|S - Samoan                        |                                                                                                                                                                                                                                                                                                                     |
|U - Hawaiian                      |                                                                                                                                                                                                                                                                                                                     |
|V - Vietnamese                    |                                                                                                                                                                                                                                                                                                                     |
|W - White                         |                                                                                                                                                                                                                                                                                                                     |
|X - Unknown                       |                                                                                                                                                                                                                                                                                                                     |
|Z - Asian Indian                  |                                                                                                                                                                                                                                                                                                                     |
|'Weapon Desc'                     |Description of the weapon used (if applicable).                                                                                                                                                                                                                                                                      |
|'Status Desc'                     |Crime status.                                                                                                                                                                                                                                                                                                        |
|'LOCATION'                        |Street address of the crime.                                                                                                                                                                                                                                                                                         |


````python
import pandas as pd
crimes = pd.read_csv("crimes.csv")
crimes.head()
````
|super_bowl  |musician                                                                          |num_songs|
|------------|----------------------------------------------------------------------------------|---------|
|52          |Justin Timberlake                                                                 |11       |
|52          |University of Minnesota Marching Band                                             |1        |
|51          |Lady Gaga                                                                         |7        |
|50          |Coldplay                                                                          |6        |
|50          |Beyoncé                                                                           |3        |


## :one: Which hour has the highest frequency of crimes?

````python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes['hour']=crimes['TIME OCC'].str[:2].astype(int)
sns.countplot(x='hour', data=crimes)
plt.show()
````
**Results**

![plot2](https://github.com/user-attachments/assets/47f48f53-9ec8-48e0-aaf7-1d01a84c107f)

## :two: Which area has the largest frequency of night crimes (crimes committed between 10pm and 3:59am)?

````python
import pandas as pd
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
crimes['hour']=crimes['TIME OCC'].str[:2].astype(int)
night=crimes[(crimes['TIME OCC']>='2200') | (crimes['TIME OCC']<='0359')]
peak_night_crime_location=night.groupby("AREA NAME")["hour"].count().sort_values(ascending=False).index[0]
peak_night_crime_location
````
**Results**

Central

## :three: Identify the number of crimes committed against victims of different age groups. Save as a pandas Series called `victim_ages`, with age group labels `"0-17"`, `"18-25"`, `"26-34"`, `"35-44"`, `"45-54"`, `"55-64"`, and `"65+"` as the index and the frequency of crimes as the values.

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
