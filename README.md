In this small project I learn how to process streaming stock market data using Apache Kafka and various AWS services. The main components include a producer that simulates stock market data, Kafka for streaming, and a series of AWS tools for data storage, cataloging, and querying. Nevertheless, a large part of this project is just setting things up in AWS.

# Architecture

<img src="architecture.jpg" />

* Stock Data Simulation: A Python app simulates stock market data from a CSV dataset, acting as a Kafka Producer.
* Apache Kafka on EC2: Kafka, hosted on Amazon EC2, streams data from the producer to downstream consumers.
* Data Storage in S3: A Kafka Consumer writes the data to Amazon S3 (with AWS CLI), creating a data lake for analysis.
* Automated Cataloging with AWS Glue: An AWS Glue Crawler scans S3 to detect and catalog new data, making it queryable.
* SQL Analysis with Athena: Amazon Athena enables SQL-based querying on the S3 data using the Glue Data Catalog.

# Dataset

Being a broke student (🥲), I don't have access to real stock API so I used a static file and try to simulate a real time stream of data from my Kafka producer.

The dataset could be found in the repo [indexProcessed](./indexProcessed.csv).

# My learning notes on Kafka

## What is Kafka?

A distributed event store for stream processing. Example of stream processing web streaming/analytics.

A stream is an unbound sequence of ordered, immutable data. Event is an immutable fact regarding something that has occurred in our system.

## Components

1. Producers
- Things that produce data: sensors, clients,..
2. Kafka brokers
   Single member (brokers, nodes, computing units like ec2 instances,... different names mean the same thing) that "writes" data in a Kafka server. A group of brokers if called a cluster.
3. Consumers
- Consume/read the data: reports, dashboards, ml, processes...
4. Zookeeper
- Zookeeper is the resource manager of the Kafka managers (cluster management, failure detection, access control, synchronisation). This one is a Key Value Store.
5. Topics
- The logical buckets (sub-section of messages) in the Kafka Broker. Defined by the developer, unlimited number. Each topic can have 0-n partitions, which are log files with all information of the partition. For example, topic "transaction" can have each customer as a partition. In the partition, the information of the customer is stored in that single partition.
   All the topics and partitions are duplicated across brokers, so if one broker fail then you can access them in another broker (*but isn't this redundant?*)
6. Log files (Data Partition)
- Files in which incoming events are written at the end of the file (appended).

<br>
> [Note]: I've just barely scratched the surface here in stream processing, gotta study more lah :>
