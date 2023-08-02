# Portfolio Project:
## Real-time application that collects GPS data steamed by IoT devices located on buses. The application achieves an efficient, scalable, and fault-tolerant data streaming solution, enabling real-time data analysis and insights.

![image](https://github.com/klailatimad/final-project-nifi-kafka-spark/assets/122483291/b9f04ed2-6723-4b1e-8db3-d6cc69e8cff3)

### Utilized Apache NiFi, CDC (Debezium), Kafka, Spark Structured Streaming, Docker, MySQL, and Superset for building the application.
### Data source: public transit bus data from the open API "Rest Bus"
### Implemented data ingestion from Rest Bus API to a MySQL database.
### Employed Kafka Connect, Kafka, and Zookeeper for monitoring Write Ahead Logs (WAL) on MySQL and writing data to a MySQL table.
### Spark structured streaming was utilized for processing streaming data from Kafka.
### Used Hudi for efficient and reliable batch writes and upserts to an S3-based data store.
### Leveraged Athena's interactive querying capabilities to gain insights from the real-time data in the AWS S3 bucket.
### Implemented Apache Superset as the visualization layer to create an interactive dashboard showcasing the findings.
 
