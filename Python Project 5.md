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
|United States      |100                                                                                |
|Canada             |86                                                                                 |
|Australia          |77                                                                                 |
|New Zealand        |73                                                                                 |
|Lebanon            |73                                                                                 |


## :one: Using a plot, show when was the global search for 'workout' at its peak?

````python
import pandas as pd
import matplotlib.pyplot as plt
workout = pd.read_csv("C:\Users\Arich\Documents\Project_5\workout.csv")
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



## :two: Using a plot, of the keywords available, what was the most popular during the covid pandemic, and what is the most popular now?

````python
import pandas as pd
import matplotlib.pyplot as plt
three_keywords = pd.read_csv("C:\Users\Arich\Documents\Project_5\three_keywords.csv")
# Plot line graphs for three keyword trends
three_keywords.plot(x='month', y=['home_workout_worldwide', 'gym_workout_worldwide', 'home_gym_worldwide'])
plt.show()
````
**Results**


peak_covid = "home workout"
now = "gym workout"

## :three: Using a plot, what country has the highest interest for workouts among the following: United States, Australia, or Japan? 

````python
import pandas as pd
import matplotlib.pyplot as plt
# Load data and index country names
workout_geo = pd.read_csv("data/workout_geo.csv", index_col = 0)
# Select data for United States, Australia, and Japan
filtered = workout_geo.loc[['United States','Australia','Japan]]
# Create a scatter plot for the selected countries
plt.scatter(x = ['United States','Australia','Japan'], y = 'workout_2018_2023',data = filtered)
plt.show()
````
**Results**

top_country = "United States"
