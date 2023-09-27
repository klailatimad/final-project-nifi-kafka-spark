
## Setting Up Debezium and Kafka for Database Event Streaming

Debezium, coupled with Kafka, empowers your systems to leverage the change data capture mechanism, enabling real-time data integrations and processing. This guide will walk you through setting up Debezium, Zookeeper, Kafka, and the corresponding connectors.

### Step-by-step Guide

#### 1. **Setting Up Zookeeper**

Zookeeper acts as a centralized service and is used to maintain naming and configuration data and to provide flexible and robust synchronization within distributed systems.
```sh
docker run -dit --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper:1.6 # Run Zookeeper Container
```
#### 2. **Creating a Kafka Container**

Kafka uses Zookeeper to manage distributed brokers.
```sh
docker run -dit --name kafka -p 9092:9092 --link zookeeper:zookeeper debezium/kafka:1.6 # Run Kafka Container linked to Zookeeper
```

#### 3. **Setting Up Debezium Connect Container**

This creates connections between Kafka, Zookeeper, and MySQL containers.
```sh
docker run -dit --name connect -p 8083:8083 -e GROUP_ID=1 -e CONFIG_STORAGE_TOPIC=my-connect-configs -e OFFSET_STORAGE_TOPIC=my-connect-offsets -e STATUS_STORAGE_TOPIC=my_connect_statuses --link zookeeper:zookeeper --link kafka:kafka --link mysql:mysql debezium/connect:1.6 # Run Debezium Connect Container
```
After this step, you should have five containers running, verify with `docker ps`.
#### 4. **Verifying Kafka Connect Application**

Test if the Kafka Connect application is up and running.
```sh
curl -H "Accept:application/json" localhost:8083 # Check Kafka Connect
```
Expected response:
```json
{"version":"2.7.1","commit":"61dbce85d0d41457","kafka_cluster_id":"O5xBxRkRRFS3hX194Edn2A"}
```
#### 5. **Checking Running Connectors**
```sh
curl -H "Accept:application/json" localhost:8083/connectors/ # List active connectors
```
#### 6. **Enabling MySQL Debezium Connector for Kafka Connect**
```sh
curl -i -X POST -H "Accept:application/json" -H "Content-Type:application/json" localhost:8083/connectors/ -d '{ "name": "inventory-connector", "config": { "connector.class": "io.debezium.connector.mysql.MySqlConnector", "tasks.max": "1", "database.hostname": "mysql", "database.port": "3306", "database.user": "debezium", "database.password": "dbz", "database.server.id": "184054", "database.server.name": "dbserver1", "database.include.list": "demo", "database.history.kafka.bootstrap.servers": "kafka:9092", "database.history.kafka.topic": "dbhistory.demo" } }' # Enable MySQL Debezium Connector
```
#### 7. **Validating the Creation of Kafka Topic**
```sh
docker exec -it kafka bash # Enter the Kafka container
bin/kafka-topics.sh --list --zookeeper zookeeper:2181 # List Kafka topics
```
Check that the Kafka topic was created.
#### 8. **Verifying Data Receipt by Kafka Topic**
Ensure you are still in the Kafka container and then verify incoming messages.
```sh
bin/kafka-console-consumer.sh --topic dbserver1.demo.bus_status --bootstrap-server '<container_id>':9092 # Check messages in the Kafka topic
```
You should be seeing incoming messages in your Kafka topic.

#### 9. **Creating `bus_status_schema.json`**

Craft the below JSON blob as `bus_status_schema.json` for Spark streaming application schema inference.
```json
{"schema":{"type":"struct","fields":[{"type":"struct","fields":[{"type":"int32","optional":false,"field":"record_id"},{"type":"int32","optional":false,"field":"id"},{"type":"int32","optional":false,"field":"routeId"},{"type":"string","optional":true,"field":"directionId"},{"type":"int16","optional":true,"field":"predictable"},{"type":"int32","optional":false,"field":"secsSinceReport"},{"type":"int32","optional":false,"field":"kph"},{"type":"int32","optional":true,"field":"heading"},{"type":"double","optional":false,"field":"lat"},{"type":"double","optional":false,"field":"lon"},{"type":"int32","optional":true,"field":"leadingVehicleId"},{"type":"int64","optional":true,"name":"io.debezium.time.Timestamp","version":1,"default":0,"field":"event_time"}],"optional":true,"name":"dbserver1.demo.bus_status.Value","field":"before"},{"type":"struct","fields":[{"type":"int32","optional":false,"field":"record_id"},{"type":"int32","optional":false,"field":"id"},{"type":"int32","optional":false,"field":"routeId"},{"type":"string","optional":true,"field":"directionId"},{"type":"int16","optional":true,"field":"predictable"},{"type":"int32","optional":false,"field":"secsSinceReport"},{"type":"int32","optional":false,"field":"kph"},{"type":"int32","optional":true,"field":"heading"},{"type":"double","optional":false,"field":"lat"},{"type":"double","optional":false,"field":"lon"},{"type":"int32","optional":true,"field":"leadingVehicleId"},{"type":"int64","optional":true,"name":"io.debezium.time.Timestamp","version":1,"default":0,"field":"event_time"}],"optional":true,"name":"dbserver1.demo.bus_status.Value","field":"after"},{"type":"struct","fields":[{"type":"string","optional":false,"field":"version"},{"type":"string","optional":false,"field":"connector"},{"type":"string","optional":false,"field":"name"},{"type":"int64","optional":false,"field":"ts_ms"},{"type":"string","optional":true,"name":"io.debezium.data.Enum","version":1,"parameters":{"allowed":"true,last,false,incremental"},"default":"false","field":"snapshot"},{"type":"string","optional":false,"field":"db"},{"type":"string","optional":true,"field":"sequence"},{"type":"string","optional":true,"field":"table"},{"type":"int64","optional":false,"field":"server_id"},{"type":"string","optional":true,"field":"gtid"},{"type":"string","optional":false,"field":"file"},{"type":"int64","optional":false,"field":"pos"},{"type":"int32","optional":false,"field":"row"},{"type":"int64","optional":true,"field":"thread"},{"type":"string","optional":true,"field":"query"}],"optional":false,"name":"io.debezium.connector.mysql.Source","field":"source"},{"type":"string","optional":false,"field":"op"},{"type":"int64","optional":true,"field":"ts_ms"},{"type":"struct","fields":[{"type":"string","optional":false,"field":"id"},{"type":"int64","optional":false,"field":"total_order"},{"type":"int64","optional":false,"field":"data_collection_order"}],"optional":true,"field":"transaction"}],"optional":false,"name":"dbserver1.demo.bus_status.Envelope"},"payload":{"before":null,"after":{"record_id":487,"id":8326,"routeId":7,"directionId":"7_0_7","predictable":1,"secsSinceReport":7,"kph":0,"heading":166,"lat":43.666602,"lon":-79.4111855,"leadingVehicleId":null,"event_time":1656980233000},"source":{"version":"1.8.0.Final","connector":"mysql","name":"dbserver1","ts_ms":1656980233000,"snapshot":"false","db":"demo","sequence":null,"table":"bus_status","server_id":223344,"gtid":null,"file":"mysql-bin.000011","pos":1532138,"row":0,"thread":null,"query":null},"op":"c","ts_ms":1656980233228,"transaction":null}}
```

### Conclusion

You've successfully set up Debezium and Kafka for database event streaming, enabling real-time capturing and streaming of data changes in your MySQL database to Kafka topics. Explore more advanced configurations to fine-tune and optimize your setup according to your use cases.


### Additional Information

-   Explore more about Debezium and Kafka:
    -   [Advanced Debezium Configurations](https://debezium.io/documentation/reference/configuration/advanced.html)
    -   [Kafka Streams Documentation](https://kafka.apache.org/documentation/streams/)

### Troubleshooting

For troubleshooting and detailed instructions, refer to the official [Debezium](https://debezium.io/documentation/reference/operations/debezium-server.html) and [Kafka](https://kafka.apache.org/documentation/) documentation.

### Next Steps

Proceed to [Configuring Spark Streaming](/SparkStreamingSetup.md) for effective data stream processing.
