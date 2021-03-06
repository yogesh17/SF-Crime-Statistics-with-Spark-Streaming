# SF Crime Statistics with Spark Streaming

## Overview 

Given a real-world dataset, extracted from Kaggle, on San Francisco crime incidents, provides statistical analyses of the data using Apache Spark Structured Streaming. also creates a Kafka server to produce data, and ingest data through Spark Structured Streaming. 

## Requirements

* Java 1.8.x
* Scala 2.11.x
* Spark 2.4.x
* Kafka
* Python 3.6 or above

## Environment Setup
This project requires creating topics, starting Zookeeper and Kafka servers, and your Kafka bootstrap server. You’ll need to choose a port number (e.g., 9092, 9093..) for your Kafka topic, and come up with a Kafka topic name and modify the zookeeper.properties and server.properties appropriately.

Install requirements using `./start.sh` if you use conda for Python. If you use pip rather than conda, then use `pip install -r requirements.txt`

## Instructions

In order to run the application you will need to start:

### Step 1. Start the Zookeeper and Kafka Server:
```
/usr/bin/zookeeper-server-start config/zookeeper.properties
/usr/bin/kafka-server-start config/server.properties
```

### Step 1.1. Create Kafka Topic:
```
/usr/bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 2 --topic com.sf.crime.police
```

### Step 2. Produce data into topic by kafka Producer:
`python kafka_server.py`

### Step 2.1. Run Kafka consumer to test if Kafka Producer is correctly implemented and producing data:

Option 1: /usr/bin/kafka-console-consumer --topic "com.sf.crime.police" --from-beginning --bootstrap-server localhost:9092
Option 2: python consumer_server.py

### Step 3. Submit Spark Streaming Job :

spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.4 --master local[*] data_stream.py

## Example of Kafka Consumer and Spark Streaming output
### Kafka Consumer Console Output

![kafka consumer output](https://github.com/yogesh17/SF-Crime-Statistics-with-Spark-Streaming/blob/master/Kafka%20Console%20Consumer%20Output.png)

### Progress Reporter

![progress reporter](https://github.com/yogesh17/SF-Crime-Statistics-with-Spark-Streaming/blob/master/Progress%20Report.png)

### Progress Reporter Query
![progress reporter query](https://github.com/yogesh17/SF-Crime-Statistics-with-Spark-Streaming/blob/master/Progress%20Report%20query.png)


### Spark UI
![spark UI](https://github.com/yogesh17/SF-Crime-Statistics-with-Spark-Streaming/blob/master/Spark%20Streaming%20UI.png)


## Question 1

> How did changing values on the SparkSession property parameters affect the throughput and latency of the data?

It affected the **processedRowsPerSecond**. Higher the processedRowsPerSecond, higher the throughput and lesser latency. 

## Question 2
> What were the 2-3 most efficient SparkSession property key/value pairs? Through testing multiple variations on values, how can you tell these were the most optimal?

I tuned the following parameters:

Setting spark.sql.shuffle.partitions to 2 vs 200 improved  **processedRowsPerSecond**
Setting spark.default.parallelism to 100 vs 5000 improved  **processedRowsPerSecond**



