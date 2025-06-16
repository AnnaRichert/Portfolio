## Python
### :bar_chart: Project 5

Exploring interest in workouts at a global and national level. Datasets contain international and national-level data on Google Trends keyword searches related to fitness and related products.

The Data:

1. **`workout`**

|Column             |Description                                                                        |
|-------------------|-----------------------------------------------------------------------------------|
|'month'            |Month when the data was measured.                                                  |
|'workout_worldwide'|Index representing the popularity of the keyword 'workout', on a scale of 0 to 100.|


````python
import pandas as pd
workout = pd.read_csv("C:\Users\Arich\Documents\Project_5\workout.csv")
workout.head()
````

|month              |workout_worldwide                                                                  |
|-------------------|-----------------------------------------------------------------------------------|
|2018-03            |59                                                                                 |
|2018-04            |61                                                                                 |
|2018-05            |57                                                                                 |
|2018-06            |56                                                                                 |
|2018-07            |51                                                                                 |


2. **`three_keywords`**
   
| Column     | Description              |
|------------|--------------------------|
| `'month'` | Month when the data was measured. |
| `'home_workout_worldwide'` | Index representing the popularity of the keyword 'home workout', on a scale of 0 to 100. |
| `'gym_workout_worldwide'` | Index representing the popularity of the keyword 'gym workout', on a scale of 0 to 100. |
| `'home_gym_worldwide'` | Index representing the popularity of the keyword 'home gym', on a scale of 0 to 100. |


````python
import pandas as pd
three_keywords = pd.read_csv("C:\Users\Arich\Documents\Project_5\three_keywords.csv")
three_keywords.head()
````

|month              |home_workout_worldwide                                                             |gym_workout_worldwide|home_gym_worldwide|
|-------------------|-----------------------------------------------------------------------------------|---------------------|------------------|
|2018-03            |12                                                                                 |16                   |10                |
|2018-04            |12                                                                                 |18                   |10                |
|2018-05            |13                                                                                 |16                   |9                 |
|2018-06            |12                                                                                 |17                   |9                 |
|2018-07            |12                                                                                 |17                   |9                 |


3. **`workout_geo`**
   
| Column     | Description              |
|------------|--------------------------|
| `'country'` | Country where the data was measured. |
| `'workout_2018_2023'` | Index representing the popularity of the keyword 'workout' during the 5 year period. |


````python
import pandas as pd
workout_geo = pd.read_csv("C:\Users\Arich\Documents\Project_5\workout_geo.csv")
workout_geo.head()
````

|country            |workout_2018_2023                                                                  |
|-------------------|-----------------------------------------------------------------------------------|
|Guam               |                                                                                   |
|Falkland Islands (Islas Malvinas)|                                                                                   |
|Cook Islands       |                                                                                   |
|Brunei             |                                                                                   |
|Palau              |                                                                                   |

4. **`three_keywords_geo`**
   
| Column     | Description              |
|------------|--------------------------|
| `'country'` | Country where the data was measured. |
| `'home_workout_2018_2023'` | Index representing the popularity of the keyword 'home workout' during the 5 year period. |
| `'gym_workout_2018_2023'` | Index representing the popularity of the keyword 'gym workout' during the 5 year period.  |
| `'home_gym_2018_2023'` | Index representing the popularity of the keyword 'home gym' during the 5 year period. |

````python
import pandas as pd
three_keywords_geo = pd.read_csv("C:\Users\Arich\Documents\Project_5\three_keywords_geo.csv")
three_keywords_geo.head()
````

|Country            |home_workout_2018_2023                                                             |gym_workout_2018_2023|home_gym_2018_2023|
|-------------------|-----------------------------------------------------------------------------------|---------------------|------------------|
|France             |31                                                                                 |25                   |44                |
|Italy              |37                                                                                 |21                   |42                |
|United States      |36                                                                                 |27                   |37                |
|Australia          |30                                                                                 |34                   |36                |
|Argentina          |32                                                                                 |32                   |36                |



## :one: Using a plot, show when was the global search for 'workout' at its peak?

````python
import pandas as pd
import matplotlib.pyplot as plt
workout = pd.read_csv("data/workout.csv")
# Plot monthly popularoty index
plt.plot(workout['month'], workout['workout_worldwide'])
# Rotate x-axis labels and set font size
plt.xticks(rotation=90, fontsize=5)
# Add grid lines
plt.grid(True)
# Add a text label for the peak in April 2020
plt.text('2020-04', 100, 'Peak at 2020-04')
plt.show()
````
**Results**



## :two: Count how many matches finished with a point difference greater than 40?

````python
import pandas as pd
from matplotlib import pyplot as plt
super_bowls = pd.read_csv("C:\Users\Arich\Documents\Project_4\super_bowls.csv")
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
halftime_musicians = pd.read_csv("C:\Users\Arich\Documents\Project_4\halftime_musicians.csv")
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
