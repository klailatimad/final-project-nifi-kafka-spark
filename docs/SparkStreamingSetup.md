
## Setting Up Spark Streaming

This guide provides step-by-step instructions for setting up and running Apache Spark on an EC2 instance and testing it with the PySpark shell for real-time data processing.

### Prerequisites:

-   An AWS EC2 instance.
-   Java and AWS CLI installed on your EC2 instance.
-   Sufficient IAM roles to allow EC2 to communicate with the S3 bucket.
-   The `bus_status_schema.json` and `rest-bus.py` files from the [GitHub Repository](/scripts).


### Step-by-step Guide:

#### 1. **Installing Spark**
```sh 
mkdir project_folder # Create a new directory  
cd project_folder 
wget <Spark_Download_Link> # Use the actual Spark download link 
tar -xf <Downloaded_Spark_Archive> # Extract the downloaded file
```
🛈 **Note:** Replace `<Spark_Download_Link>` and `<Downloaded_Spark_Archive>` with the actual download link and the downloaded Spark archive name, respectively.

#### 2. **Setting up Script and Dependencies**

a. Download the `rest-bus.py` script from the [GitHub Repository](/scripts/rest-bus.py) to your project folder and open it to insert the script provided.
```sh
wget <Link_to_rest-bus.py>
nano rest-bus.py # or use any other text editor to edit the script
```

b. Modify the `BOOTSTRAP_SERVERS` and `s3a://<s3-bucket-name>/` in the `rest-bus.py` file as per your setup.

c. Download the `bus_status_schema.json` file from the [GitHub Repository](/scripts/bus_status_schema.json) and upload it to your S3 bucket, ensuring the EC2 instance has the required IAM roles to interact with the S3 bucket.
```
aws s3 cp <Link_to_bus_status_schema.json> s3://<s3-bucket-name>/bus_status_schema.json
```
#### 3. **Configuring and Testing the Script**

Navigate to the Spark directory and initiate PySpark.
```sh
cd spark-3.3.3/bin
./pyspark
```
If `JAVA_HOME` doesn’t work, you may need to install Java dependencies and set up the `JAVA_HOME` environment variable appropriately. After testing the commands in the PySpark shell, you can exit the shell by typing `exit()` or pressing `Ctrl+D`.

#### 4. **Executing Full Script with spark-submit**
```sh
`../bin/spark-submit --deploy-mode client --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.2,org.apache.hudi:hudi-spark3-bundle_2.12:0.12.3  --conf "spark.serializer=org.apache.spark.serializer.KryoSerializer" /home/ec2-user/project_folder/rest-bus.py`
```
Adjust the configurations and paths as necessary for your setup and requirements.

### Conclusion

This guide illustrated how to set up Spark Streaming with PySpark on an EC2 instance. You have configured the required dependencies, established the correct communication between EC2 and S3, and tested and run a PySpark script.

### Additional Resources

-   For more details and troubleshooting tips, refer to the [Official Spark Documentation](https://spark.apache.org/docs/latest/) and [PySpark Documentation](https://spark.apache.org/docs/latest/api/python/index.html).

### Next Steps
Once the Spark Streaming Setup is complete, proceed to configure AWS Glue Crawler and Athena to organize, analyze, and visualize the processed data efficiently. Please refer to the [AWS Glue/Athena Setup Guide](/docs/AWS-Glue-Athena-Setup.md) for detailed instructions.
