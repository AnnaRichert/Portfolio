## SQL
### :bar_chart: Project 3

Exploring GoodThought NGO database with detailed records of assignments, funding, impacts, and donor activities from 2010 to 2023. The dataset includes:

- `Assignments`:
Details about each project, including its name, duration (start and end dates), budget, geographical region, and the impact score.
- `Donations`:
Records of financial contributions, linked to specific donors and assignments, highlighting how financial support is allocated and utilized.
- `Donors`: Information on individuals and organizations that fund GoodThought’s projects, including donor types.

The below ERD diagram shows a visual representation of the relationships between these data tables:
![diagram](https://github.com/user-attachments/assets/79bf6223-aefd-4599-920f-5e5890ec1258)
## :one: List the top five assignments based on total value of donations, categorized by donor type. The output should include four columns: 1) `assignment_name`, 2) `region`, 3) `rounded_total_donation_amount` rounded to two decimal places, and 4) `donor_type`, sorted by `rounded_total_donation_amount` in descending order. 

````sql
WITH sub AS (
          SELECT a.assignment_name, ROUND(SUM(d.amount),2) AS rounded_total_donation_amount, dd.donor_type
          FROM assignments AS a
          JOIN donations AS d
          ON a.assignment_id=d.assignment_id
          JOIN donors AS dd
          ON d.donor_id=dd.donor_id
          GROUP BY a.assignment_name, dd.donor_type
          ORDER BY rounded_total_donation_amount DESC
          LIMIT 5)
SELECT a.assignment_name, a.region, sub.rounded_total_donation_amount, sub.donor_type
FROM sub
LEFT JOIN assignments AS a
ON sub.assignment_name=a.assignment_name
ORDER BY rounded_total_donation_amount DESC
````
**Results**

|assignment_name|region                              |rounded_total_donation_amount|donor_type  |
|---------------|------------------------------------|-----------------------------|------------|
|Assignment_3033|East                                |3840.66                      |Individual  |
|Assignment_300 |West                                |3133.98                      |Organization|
|Assignment_4114|North                               |2778.57                      |Organization|
|Assignment_1765|West                                |2626.98                      |Organization|
|Assignment_268 |East                                |2488.69                      |Individual  |


## :two: Identify the assignment with the highest impact score in each region, ensuring that each listed assignment has received at least one donation. The output should include four columns: 1) `assignment_name`, 2) `region`, 3) `impact_score`, and 4) `num_total_donations`, sorted by `region` in ascending order. Include only the highest-scoring assignment per region, avoiding duplicates within the same region.

````sql
WITH total_donations AS (
                      SELECT assignment_name, COUNT(amount) AS num_total_donations
                      FROM assignments AS a
                      JOIN donations AS d
                      ON a.assignment_id=d.assignment_id
                      GROUP BY assignment_name 
                      HAVING COUNT(amount) IS NOT NULL
                      ORDER BY COUNT(amount) DESC),
rank AS (
    SELECT t.assignment_name, a.region, a.impact_score, ROW_NUMBER() OVER(PARTITION BY a.region ORDER BY a.impact_score DESC) AS ranking, t.num_total_donations
    FROM total_donations AS t
    LEFT JOIN assignments AS a
    ON t.assignment_name=a.assignment_name)
SELECT assignment_name, region, impact_score, num_total_donations
FROM rank
WHERE ranking =1
ORDER BY region 
````
**Results**
|assignment_name|region                              |impact_score    |num_total_donations|
|---------------|------------------------------------|----------------|-------------------|
|Assignment_316 |East                                |10              |2                  |
|Assignment_2253|North                               |9.99            |1                  |
|Assignment_3547|South                               |10              |1                  |
|Assignment_2794|West                                |9.99            |2                  |
