# Module 2 HW Answers
- Question 1. Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?

128.3 MiB

this can be output by adding the following tasks after extract:

```yml

  - id: get_size
    type: io.kestra.plugin.core.storage.Size
    uri: "{{render(vars.data)}}" # Reference the output file URI

  - id: log_size
    type: io.kestra.plugin.core.log.Log
    message: "The size of the file is {{ outputs.get_size.size / (1024 *1024)}} mb."

```

- Question 2. What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution? 

`green_tripdata_2020-04.csv`

- Question 3. How many rows are there for the Yellow Taxi data for all CSV files in the year 2020? (1 point)
```sql
SELECT count(*) filename FROM public.yellow_tripdata
where filename like 'yellow_tripdata_2020%'
```
output: 4648499

- Question 4. How many rows are there for the Green Taxi data for all CSV files in the year 2020?
```sql
SELECT count(*) filename FROM public.green_tripdata
where filename like 'green_tripdata_2020%'
```
output: 1734051

- Question 5. How many rows are there for the Yellow Taxi data for the March 2021 CSV file? 

```sql
SELECT count(*) filename FROM public.yellow_tripdata
where filename='yellow_tripdata_2021-03.csv'
```
output: 1925152

- Question 6. How would you configure the timezone to New York in a Schedule trigger?

Add a timezone property set to America/New_York in the Schedule trigger configuration

```yml
triggers:
  - id: green_schedule
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 9 1 * *"
    timezone: America/New_York
    inputs:
      taxi: green

  - id: yellow_schedule
    type: io.kestra.plugin.core.trigger.Schedule
    cron: "0 10 1 * *"
    timezone: America/New_York
    inputs:
      taxi: yellow
```