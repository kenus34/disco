### Kapacitor

* Time series data is something that is indexed with time. Time plays an important role. All the data coming through will have timestamp.

* Retension policy of influx db is to auto expire data when it becomes irrelavant.

* Kapacitor capabilities:
 1. It helps in alerting when data reaches a certain threshold like slack messages, pagers etc.
 1. In processing ETL job, This means extracting data, Tranforming data and save it in a database.

* Tick script is the data processing language that capacitor uses.

* Batching: Collection of data points grouped togethar in specific time intervel.
  1. In batching kapacitor queries influx db periodically this is a good approach as we store very less data in ram. 
  1. If we dont want to perform alert on every single datapoint we prefer this method. This is best for aggregation.
  1. A little bit of latency is expected in batch tasks.
  1. A batch task adds additional load on influx db due to the additional load due to querying.
  
* Streaming: All data written to influx is also written to kapacitor. Due to this reason streaming is of a higher throughput.
  1. High ram is used. 
  1. This is preffered when we want to do some sort of transformation.
  1. Streaming puts some additional write load on kapacitor.

* Tick Script:
    1. Single quote signifies strings inside expression.
    1. Double quotes signifies values inside lambda expression.
    1. https://www.youtube.com/watch?v=QedTC--ubZM&t=2s
    
* Simple stream tick script:
     ```tickscript
    steam
        |from()
            .measurement("cpu")
        |log()
    ```
* Complex script:
    ```tickscript
      stream
          |from()
              .measurement('cpu')
          |window()
              .period(5m)
              .every(1m)
          |mean('usage_user')
              .as('mean_usage_user ')
          |log()
    ```
    ```tickscript
      stream
          |from()
              .measurement('cpu')
          |where(lamda: "cpu" =='cpu-total')
          |window()
              .period(5m)
              .every(1m)
          |mean('usage_user')
              .as('mean_usage_user ')
          |log()
    ```
Batch Tick script:
    ```tickscript
        batch
            |query('''SELECT mean("usage_user") AS mean_user FROM "telegraf"."autogen"."cpu" ''')
            .period(5m)
            .every(1m)
            | log()
    ```