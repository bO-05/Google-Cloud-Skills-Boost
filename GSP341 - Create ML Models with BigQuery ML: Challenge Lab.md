# GSP341 - Create ML Models with BigQuery ML: Challenge Lab
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Task 1: Create a dataset to store your machine learning models

Run in cloud shell

```bash
bq mk austin
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 2: Create a forecasting BigQuery machine learning model

From now onwards, all command run in BigQuery.
* Navigation menu > BigQuery
* click `+ Compose new query` and add the following query
* replace <TRAINING_YEAR> accordingly

```sql
CREATE 
OR REPLACE MODEL austin.location_model OPTIONS (
  model_type = 'linear_reg', labels = [ 'duration_minutes' ]
) AS 
SELECT 
  start_station_name, 
  EXTRACT(
    HOUR 
    FROM 
      start_time
  ) AS start_hour, 
  EXTRACT(
    DAYOFWEEK 
    FROM 
      start_time
  ) AS day_of_week, 
  duration_minutes, 
  address as location 
FROM 
  `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips 
  JOIN `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations ON trips.start_station_name = stations.name 
WHERE 
  EXTRACT(
    YEAR 
    FROM 
      start_time
  ) = < TRAINING_YEAR > 
  AND duration_minutes > 0
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 3: Create the second machine learning model 

replace <TRAINING_YEAR> accordingly

```sql
CREATE 
OR REPLACE MODEL austin.subscriber_model OPTIONS (
  model_type = 'linear_reg', labels = [ 'duration_minutes' ]
) AS 
SELECT 
  start_station_name, 
  EXTRACT(
    HOUR 
    FROM 
      start_time
  ) AS start_hour, 
  subscriber_type, 
  duration_minutes 
FROM 
  `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips 
WHERE 
  EXTRACT(
    YEAR 
    FROM 
      start_time
  ) = < TRAINING_YEAR >
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 4: Evaluate the two machine learning models

Replace <EVALUATION_YEAR> in both Query 1 and Query 2

Query 1

```sql
SELECT 
  SQRT(mean_squared_error) AS rmse, 
  mean_absolute_error 
FROM 
  ML.EVALUATE(
    MODEL austin.location_model, 
    (
      SELECT 
        start_station_name, 
        EXTRACT(
          HOUR 
          FROM 
            start_time
        ) AS start_hour, 
        EXTRACT(
          DAYOFWEEK 
          FROM 
            start_time
        ) AS day_of_week, 
        duration_minutes, 
        address as location 
      FROM 
        `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips 
        JOIN `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations ON trips.start_station_name = stations.name 
      WHERE 
        EXTRACT(
          YEAR 
          FROM 
            start_time
        ) = < EVALUATION_YEAR >
    )
  )
```

Query 2

```sql
SELECT 
  SQRT(mean_squared_error) AS rmse, 
  mean_absolute_error 
FROM 
  ML.EVALUATE(
    MODEL austin.subscriber_model, 
    (
      SELECT 
        start_station_name, 
        EXTRACT(
          HOUR 
          FROM 
            start_time
        ) AS start_hour, 
        subscriber_type, 
        duration_minutes 
      FROM 
        `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips 
      WHERE 
        EXTRACT(
          YEAR 
          FROM 
            start_time
        ) = < EVALUATION_YEAR >
    )
  )
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## Task 5: Use the subscriber type machine learning model to predict average trip durations

Replace <EVALUATION_YEAR> in both Query 1 and Query 2

Query 1:

```sql
SELECT 
  start_station_name, 
  COUNT(*) AS trips 
FROM 
  `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
WHERE 
  EXTRACT(
    YEAR 
    FROM 
      start_time
  ) = < EVALUATION_YEAR > 
GROUP BY 
  start_station_name 
ORDER BY 
  trips DESC
```

Query 2:

```sql
SELECT 
  AVG(predicted_duration_minutes) AS average_predicted_trip_length 
FROM 
  ML.predict(
    MODEL austin.subscriber_model, 
    (
      SELECT 
        start_station_name, 
        EXTRACT(
          HOUR 
          FROM 
            start_time
        ) AS start_hour, 
        subscriber_type, 
        duration_minutes 
      FROM 
        `bigquery-public-data.austin_bikeshare.bikeshare_trips` 
      WHERE 
        EXTRACT(
          YEAR 
          FROM 
            start_time
        ) = < EVALUATION_YEAR > 
        AND subscriber_type = 'Single Trip' 
        AND start_station_name = '21st & Speedway @PCL'
    )
  )
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
## DONE  
