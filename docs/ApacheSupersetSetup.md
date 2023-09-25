## Setting Up Apache Superset for Visualization

Apache Superset allows you to create and share dashboards and explore data with SQL. Here, we will run Apache Superset in a Docker container and connect it to Amazon Athena.

### Prerequisites:

-   Docker installed on your machine.
-   AWS Access and Secret Keys for an IAM user.

### Step-by-step Guide:

#### 1. **Pulling the Docker Image**

You can pull a prebuilt Apache Superset image:
`docker pull stantaov/superset-athena:0.0.1` 

#### 2. **Running the Docker Container**

Deploy the pulled Superset image by running the following command.

`docker run -d -p 8088:8088 --name superset stantaov/superset-athena:0.0.1` 

#### 3. **Setting up the Local Admin Account**

With your local Superset container running, setup your local admin account:
```sh
docker exec -it superset superset fab create-admin \
               --username admin \
               --firstname Superset \
               --lastname Admin \
               --email admin@superset.com \
               --password admin` 
```
#### 4. **Updating the Local Database**

Update the local database to the latest with:

`docker exec -it superset superset db upgrade` 

#### 5. **Loading Examples (Optional)**

If you want to load examples, run:

`docker exec -it superset superset load_examples` 

#### 6. **Setting up Roles**

Setup roles with:

`docker exec -it superset superset init` 

#### 7. **Logging In**

Navigate to [http://localhost:8088/login/](http://localhost:8088/login/) and login using `admin/admin`.

#### 8. **Creating a New Database Connection**

To connect Superset with Athena, go to Data → Databases, click on “+ Database,” and provide the database name and SQLALCHEMY URI, using the following connection string:

`awsathena+rest://{aws_access_key_id}:{aws_secret_access_key}@athena.{region_name}.amazonaws.com/{schema_name}?s3_staging_dir={s3_staging_dir}&work_group={work_group_name}` 

- Replace the placeholders `{aws_access_key_id}`, `{aws_secret_access_key}`, `{region_name}`, `{schema_name}`, `{s3_staging_dir}`, and `{work_group_name}` with appropriate values.
- Use `awsathena+rest` as the recommended SQL Alchemy driver.

#### 9. **Building Charts/Dashboards or Running SQL Queries**

Select the dataset for your database and start building charts, dashboards, or run SQL queries in Superset against your dataset.

### Conclusion:

By following these steps, you should have Apache Superset up and running in a Docker container, connected to Amazon Athena, allowing you to visualize and analyze your data.

### Additional Resources:

-   For more details on Apache Superset, refer to the [Official Superset Documentation](https://superset.apache.org/docs/introduction).
-   For Docker-related information, consult the [Official Docker Documentation](https://docs.docker.com/get-started/overview/).
