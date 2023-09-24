
## Creating MySQL - Debezium

Before deploying Apache NiFi, we need to set up a MySQL database with an extra layer called Debezium for Change Data Capture (CDC).

### Step-by-step Guide

1. **Run MySQL with Debezium:**
```sh
docker run -dit --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=debezium -e MYSQL_USER=mysqluser -e MYSQL_PASSWORD=mysqlpw debezium/example-mysql:1.6
```
2. **Access MySQL Container**:
When MySQL starts running, execute the following commands to access the MySQL container and create a new database with a table to store data from the Bus API.
```sh
docker exec -it '<container_id>' bash
mysql -u root -p # Password: debezium
```
3. **Create Database and Table**:
```sql
CREATE DATABASE demo;
USE demo;
CREATE TABLE bus_status (
    record_id INT NOT NULL AUTO_INCREMENT,
    id INT NOT NULL,
    routeId INT NOT NULL,
    directionId VARCHAR(40),
    predictable BOOLEAN,
    secsSinceReport INT NOT NULL,
    kph INT NOT NULL,
    heading INT,
    lat REAL NOT NULL, 
    lon REAL NOT NULL,
    leadingVehicleId INT,
    event_time DATETIME DEFAULT NOW(),
    PRIMARY KEY (record_id)
);
```
### Set up Apache Nifi
4. **Run Apache Nifi Container**:
```sh
docker run --name nifi -p 8080:8080 -p 8443:8443 --link mysql:mysql -d apache/nifi:1.12.0
```
Access NiFi interface by navigating to `http://<your_host>:8080/nifi/`.

### Configure Processors and Establish Pipeline
5. **Create and Configure InvokeHTTP Processor:**

-   Connect to the Rest Bus API endpoint and collect JSON blobs.
-   Set Run Schedule to 30 seconds to prevent IP from getting blacklisted.
-   Add the Rest Bus API endpoint under Remote URL.
```sh
http://restbus.info/api/agencies/ttc/routes/7/vehicles
```
6. **Add and Configure ConverterJSONToSQL Processor:**

-   Convert JSON blobs into SQL inserts and insert new records into the MySQL table.
-   Set up JDBC connection pool and specify connection URL, class name, and username/password.
```sh
jdbc:mysql://mysql:3306/demo
com.mysql.jdbc.Driver
```
### Additional Configurations and Verifications
**Download and Configure JDBC Connector Libraries:**
```sh
docker exec -it nifi bash
mkdir custom-jars
cd custom-jars
wget http://java2s.com/Code/JarDownload/mysql/mysql-connector-java-5.1.17-bin.jar.zip
unzip mysql-connector-java-5.1.17-bin.jar.zip
```
7. **Verify Data Insertion into MySQL:**
```sh
docker exec -it mysql bash mysql -u root -p # Password: debezium
```
```sql
use demo; select * from bus_status limit 10;
```
### Processing Different Bus Routes and Templating

Using Processor groups and templates, create multiple pipelines to process different bus routes. This aids in better organization and version control.
### Troubleshooting

If you encounter any issues during the setup, refer to the official [MySQL](https://dev.mysql.com/doc/), [Debezium](https://debezium.io/documentation/reference/operations/debezium-server.html), and [Apache NiFi](https://nifi.apache.org/docs.html) documentation for troubleshooting tips and detailed instructions.

### Next Steps
After setting up MySQL - Debezium, proceed to [Configuring Debezium and Kafka](DebeziumAndKafkaSetup.md) for data extraction from the bus API.
