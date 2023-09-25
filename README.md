# NiFi-CDC-Kafka-Spark-Streaming Transit Pipeline

This real-time application is designed to collect and process GPS data streamed by IoT devices located on buses, providing an efficient, scalable, and fault-tolerant data streaming solution and enabling real-time data analysis and insights.

## Table of Contents
- [Overview](#overview)
- [Technologies Used](#technologies-used)
- [Setup Guides](#setup-guides)
- [Scripts](#scripts)

## Overview
The primary goal of this project is to construct a real-time application capable of collecting GPS data streamed by IoT devices located on buses, aiming to optimize transit routes. The application employs a combination of technologies including Apache NiFi, CDC (Debezium), Kafka, Spark Structured Streaming, Docker, MySQL, and Apache Superset for visualization.

### Data Processing and Visualization
- Data is ingested from the "Rest Bus" API into a MySQL database.
- CDC with Debezium captures changes and transmits to Kafka topics.
- Spark Structured Streaming is used for processing this streaming data.
- Apache Superset serves as the visualization layer to create interactive dashboards.

![System Architecture](https://github.com/klailatimad/final-project-nifi-kafka-spark/assets/122483291/b9f04ed2-6723-4b1e-8db3-d6cc69e8cff3)

For more details about the technologies utilized, please refer to the individual setup guides listed below.

## Technologies Used
- **Apache NiFi**
- **CDC (Debezium)**
- **Kafka**
- **Spark Structured Streaming**
- **Docker**
- **MySQL**
- **Superset**
- **AWS (Athena, Glue, S3)**

## Setup Guides
- **[NiFi Setup](/docs/NiFiSetup.md):** Provides instructions for setting up Apache NiFi.
- **[CDC Setup](/docs/CDCSetup.md):** Offers a guide for setting up Change Data Capture.
- **[Debezium And Kafka Setup](/docs/DebeziumAndKafkaSetup.md):** Guide for setting up Debezium and Kafka.
- **[Spark Streaming Setup](/docs/SparkStreamingSetup.md):** Instructions for setting up Spark Streaming.
- **[AWS Glue & Athena Setup](/docs/AWSGlueAthenaSetup.md):** Details the steps for utilizing AWS Glue Crawlers and Amazon Athena.


## Scripts
- **[bus_status_schema.json](/scripts/bus_status_schema.json):** JSON schema for the bus status.
- **[rest_bus.py](/scripts/rest_bus.py):** Python script for rest bus.

### Additional Resources
- [Official Apache NiFi Documentation](https://nifi.apache.org/docs.html)
- [Official Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Official Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [Official Apache Superset Documentation](https://superset.apache.org/docs/introduction)
- [Official Docker Documentation](https://docs.docker.com/get-started/overview/)
- [Official AWS Athena Documentation](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)
- [Official AWS Glue Documentation](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)
