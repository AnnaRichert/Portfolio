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
|'Vict Descent'                    |Victim's descent: A - Other Asian; B - Black; C - Chinese; D - Cambodian; F - Filipino; G - Guamanian; H - Hispanic/Latin/Mexican; I - American Indian/Alaskan Native; J - Japanese; K - Korean; L - Laotian; O - Other; P - Pacific Islander; S - Samoan; U - Hawaiian; V - Vietnamese; W - White; X - Unknown; Z - Asian Indian                                                                                                                                                                                                                                                                                                    |
|'Weapon Desc'                     |Description of the weapon used (if applicable).                                                                                                                                                                                                                                                                      |
|'Status Desc'                     |Crime status.                                                                                                                                                                                                                                                                                                        |
|'LOCATION'                        |Street address of the crime.                                                                                                                                                                                                                                                                                         |


````python
import pandas as pd
crimes = pd.read_csv("crimes.csv")
crimes.head()
````
|DR_NO    |Date Rptd |DATE OCC  |TIME OCC|AREA NAME  |Crm Cd Desc      |Vict Age|Vict Sex|Vict Descent|Weapon Desc|Status Desc|LOCATION                               |
|---------|----------|----------|--------|-----------|-----------------|--------|--------|------------|-----------|-----------|---------------------------------------|
|220314085|2022-07-22|2020-05-12|1110    |Southwest  |THEFT OF IDENTITY|27      |F       |B           |null       |Invest Cont|2500 S  SYCAMORE                     AV|
|222013040|2022-08-06|2020-06-04|1620    |Olympic    |THEFT OF IDENTITY|60      |M       |H           |null       |Invest Cont|3300    SAN MARINO                   ST|
|220614831|2022-08-18|2020-08-17|1200    |Hollywood  |THEFT OF IDENTITY|28      |M       |H           |null       |Invest Cont|1900    TRANSIENT                      |
|231207725|2023-02-27|2020-01-27|635     |77th Street|THEFT OF IDENTITY|37      |M       |H           |null       |Invest Cont|6200    4TH                          AV|
|220213256|2022-07-14|2020-07-14|900     |Rampart    |THEFT OF IDENTITY|79      |M       |B           |null       |Invest Cont|1200 W  7TH                          ST|



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
crimes = pd.read_csv("crimes.csv", dtype={"TIME OCC": str})
bins=[0,17,25,34,44,54,64,crimes['Vict Age'].max()]
labels=["0-17", "18-25", "26-34", "35-44", "45-54", "55-64", "65+"]
crimes['victim_ages']=pd.cut(crimes['Vict Age'],labels=labels,bins=bins)
victim_ages=crimes['victim_ages'].value_counts().sort_index()
victim_ages
````
**Results**
|index    |victim_ages|
|---------|-----------|
|0-17     |4528       |
|18-25    |28291      |
|26-34    |47470      |
|35-44    |42157      |
|45-54    |28353      |
|55-64    |20169      |
|65+      |14747      |

