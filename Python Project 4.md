## Python
### :bar_chart: Project 4

Exploring Super Bowl data to uncover insights about TV viewership, game outcomes, and halftime shows. The dataset is made up of three CSV files, one with game data, one with TV data, and one with halftime musician data for 52 Super Bowls through 2018.

The Data:

1. `halftime_musicians`

This dataset contains information about the musicians who performed during the halftime shows of various Super Bowl games. The structure is shown below, and it applies to all remaining files.

|Column      |Description                                                                       |
|------------|----------------------------------------------------------------------------------|
|`super_bowl`|The Super Bowl number (e.g., 52 for Super Bowl LII).                              |
|`musician`  |The name of the musician or musical group that performed during the halftime show.|
|`num_songs` |The number of songs performed by the musician or group during the halftime show.  |

````python
import pandas as pd
halftime_musicians = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\halftime_musicians.csv')
halftime_musicians.head()
````
|super_bowl  |musician                                                                          |num_songs|
|------------|----------------------------------------------------------------------------------|---------|
|52          |Justin Timberlake                                                                 |11       |
|52          |University of Minnesota Marching Band                                             |1        |
|51          |Lady Gaga                                                                         |7        |
|50          |Coldplay                                                                          |6        |
|50          |Beyoncé                                                                           |3        |


2. `super_bowls`
   
This dataset provides details about each Super Bowl game, including the date, location, participating teams, and scores, including the points difference between the winning and losing team (`difference_pts`).

````python
import pandas as pd
super_bowls = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\halftime_musicians.csv')
super_bowls.head()
````

|date        |super_bowl                                                                        |venue|city           |state     |attendance|team_winner         |winning_pts|qb_winner_1   |qb_winner_2|coach_winner  |team_loser          |losing_pts|qb_loser_1    |qb_loser_2|coach_loser   |combined_pts|difference_pts|
|------------|----------------------------------------------------------------------------------|-----|---------------|----------|----------|--------------------|-----------|--------------|-----------|--------------|--------------------|----------|--------------|----------|--------------|------------|--------------|
|2018-02-04  |52                                                                                |U.S. Bank Stadium|Minneapolis    |Minnesota |67612     |Philadelphia Eagles |41         |Nick Foles    |null       |Doug Pederson |New England Patriots|33        |Tom Brady     |null      |Bill Belichick|74          |8             |
|2017-02-05  |51                                                                                |NRG Stadium|Houston        |Texas     |70807     |New England Patriots|34         |Tom Brady     |null       |Bill Belichick|Atlanta Falcons     |28        |Matt Ryan     |null      |Dan Quinn     |62          |6             |
|2016-02-07  |50                                                                                |Levi's Stadium|Santa Clara    |California|71088     |Denver Broncos      |24         |Peyton Manning|null       |Gary Kubiak   |Carolina Panthers   |10        |Cam Newton    |null      |Ron Rivera    |34          |14            |
|2015-02-01  |49                                                                                |University of Phoenix Stadium|Glendale       |Arizona   |70288     |New England Patriots|28         |Tom Brady     |null       |Bill Belichick|Seattle Seahawks    |24        |Russell Wilson|null      |Pete Carroll  |52          |4             |
|2014-02-02  |48                                                                                |MetLife Stadium|East Rutherford|New Jersey|82529     |Seattle Seahawks    |43         |Russell Wilson|null       |Pete Carroll  |Denver Broncos      |8         |Peyton Manning|null      |John Fox      |51          |35            |


3. `tv`
   
This dataset contains television viewership statistics and advertisement costs related to each Super Bowl.

````python
import pandas as pd
tv = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\tv.csv')
tv.head()
````

|super_bowl  |network                                                                           |avg_us_viewers|total_us_viewers|rating_household|share_household|rating_18_49        |share_18_49|ad_cost       |
|------------|----------------------------------------------------------------------------------|--------------|----------------|----------------|---------------|--------------------|-----------|--------------|
|52          |NBC                                                                               |103390000     |                |43.1            |68             |33.4                |78         |5000000       |
|51          |Fox                                                                               |111319000     |172000000       |45.3            |73             |37.1                |79         |5000000       |
|50          |CBS                                                                               |111864000     |167000000       |46.6            |72             |37.7                |79         |5000000       |
|49          |NBC                                                                               |114442000     |168000000       |47.5            |71             |39.1                |79         |4500000       |
|48          |Fox                                                                               |112191000     |167000000       |46.7            |69             |39.3                |77         |4000000       |



## :one: Using a plot, show if TV viewership has increased over time?

````python
import pandas as pd
from matplotlib import pyplot as plt
tv = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\tv.csv')
# Plot average US viewers per Super Bowl
plt.plot(tv['super_bowl'], tv['avg_us_viewers'])
# Add title
plt.title ('Average Number of US Viewers')
plt.show()
````
**Results**

![plot](https://github.com/user-attachments/assets/af4ccb1c-f94b-470d-8848-c6848866c010)


## :two: Count how many matches finished with a point difference greater than 40?

````python
import pandas as pd
from matplotlib import pyplot as plt
super_bowls = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\super_bowls.csv')
# Filter matches where the point difference is greater than 40
matches=super_bowls[ (super_bowls['difference_pts'] > 40) ]
# Count number of such matches
count=matches['difference_pts'].count()
print(count)
````
**Results**

1


## :three: Who performed the most songs in Super Bowl halftime shows? List 10 artists.

````python
import pandas as pd
from matplotlib import pyplot as plt
halftime_musicians = pd.read_csv('C:\Users\Arich\Documents\Python\Project_4\halftime_musicians.csv')
# Sum total songs performed by each musician
halftime_appearance = halftime_musicians.groupby('musician').sum('num_songs')
# Sort musicians by total number of songs performed (descending)
halftime_appearance = halftime_appearance.sort_values('num_songs', ascending= False)
# Show top 10 musicians
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
