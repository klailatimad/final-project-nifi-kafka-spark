## Setting Up AWS Glue Crawler and Athena

### Prerequisites:

-   AWS Account with necessary permissions for AWS Glue and Athena.
-   Processed data in S3 bucket.

### Step-by-step Guide:

#### 1. **Create a Table using AWS Glue Crawler**

  a. Navigate to AWS Glue Console and choose 'Add tables using a crawler'. 

  b. Assign a name to the crawler, for example, `routes_demo_crawler`. 

  c. Select the data store as S3 and include the path ending in `/routes` where your data resides. 

  d. Choose `Apache Hive` as the Table type and `Apache Parquet` as the file format.

  e. In the 'Add Columns' section, bulk add the following columns for your table:

```sh
record_id int, id int, routeId int, directionId string, kph int, predictable int, secsSinceReport int, heading int, lat double, lon double, leadingVehicleId int, event_time timestamp
```
Ensure that the data types are assigned correctly as per the requirements:
```sql
record_id INT NOT NULL AUTO_INCREMENT,
id INT NOT NULL,
routeId INT NOT NULL,
directionId VARCHAR(40),
kph INT NOT NULL,
predictable BOOLEAN,
secsSinceReport INT NOT NULL,
heading INT,
lat DOUBLE NOT NULL,
lon DOUBLE NOT NULL,
leadingVehicleId INT,
event_time DATETIME DEFAULT NOW()
```

f. Set up the crawler's output to a new or existing database and run the crawler.

#### 2. **Querying Data using Amazon Athena**

  a. Navigate to the Amazon Athena Console. 
  
  b. Select the database that the Glue Crawler populated. 
  
  c. Attempt to run a test query, such as:

`select * from "<database_name>"."<table_name>" limit 100` 

You might encounter an error like:

```lua
No output location provided. An output location is required either through the Workgroup result configuration setting or as an API input.` 
```
To resolve this, specify an output path in the Athena settings:

-   Go to ‘Settings’ in Amazon Athena.
-   Specify the query result location by creating a new folder in your desired S3 bucket.

  d. After resolving the output location issue, re-run your queries to analyze the data in the `routes_demo` table.

### Conclusion:

This guide helped you set up AWS Glue to create a table for your dataset and use Amazon Athena for running queries on this data. Ensure the data types and settings are correctly configured to avoid any errors during querying.

### Additional Resources:

-   For more information on AWS Glue, refer to the [AWS Glue Documentation](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html).
-   For Amazon Athena, refer to the [Amazon Athena Documentation](https://docs.aws.amazon.com/athena/latest/ug/what-is.html).

### Next Steps for Visualization:
Once you have set up AWS Glue and Athena, you can use [Apache Superset](https://superset.apache.org/) to visualize your data. For setting up Apache Superset in a docker container and connecting it to Athena, refer to the [Apache Superset Setup](/docs/ApacheSupersetSetup.md).
