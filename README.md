# Step 1: Start Your Docker Containers
## 1. Navigate to Your Docker Compose File:
Open a terminal and navigate to the directory where your docker-compose.yml file is located
## 2. Start Docker Containers:
```bash
docker-compose up -d
```
# Step 2: Copy Data from Host to DataNode
## 1. Identify Your DataNode Container:
Check the name of your DataNode container. You can list all running containers with:

```bash
docker ps
```
## 2. Copy Data from Host to DataNode:
Use the docker cp command to copy your local data file to the DataNode container. Replace <datanode_container> with the actual name or ID of your DataNode:

```bash
docker cp /path/to/local/file.csv <datanode_container>:/hadoop/dfs/data/
```
# Step 3: Access the HDFS Command Line
## 1. Access the NameNode Container:
Use the following command to access the NameNode container (or any other container where you can run HDFS commands):

```bash
docker exec -it <namenode_container> /bin/bash
```
# Step 4: Create Directories in HDFS
## 1. Create a Directory in HDFS:
Once inside the container, use the following command to create a directory in HDFS:

```bash
hdfs dfs -mkdir /datasets/amazon_dataset
```

# Step 5: Move Data from DataNode to HDFS
## 1. Move Data to HDFS:
Assuming you uploaded the file to the DataNode, you can now move it to HDFS:

```bash
hdfs dfs -put /hadoop/dfs/data/file.csv /datasets/mydata/
```

# Step 6: Query HDFS
## 1. Access Hive CLI 
You can run queries using Hive. Access Hive through the Hive CLI :

```bash 
hive
```

## 2. Create an External Table:

Create an external table to query the data (as outlined in a previous response):
```sql
CREATE EXTERNAL TABLE my_table (
    index INT,
    order_id STRING,
    order_date STRING,
    status STRING,
    fulfilment STRING,
    sales_channel STRING,
    ship_service_level STRING,
    style STRING,
    sku STRING,
    category STRING,
    size STRING,
    asin STRING,
    courier_status STRING,
    qty INT,
    currency STRING,
    amount FLOAT,
    ship_city STRING,
    ship_state STRING,
    ship_postal_code STRING,
    ship_country STRING,
    promotion_ids STRING,
    b2b STRING,
    fulfilled_by STRING,
    unnamed_22 STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LOCATION '/datasets/mydata';
```
## 3. Run a Query to Fetch Data:
```sql 
SELECT * FROM my_table LIMIT 10;
```
This will return the first 10 rows of your dataset.


# Step 7: Query HDFS using spark

## Connect to spark container

```bash
docker exec -it spark-master bash
```

Inside the container, you can launch the Spark shell with:

```bash
spark-shell --master spark://spark-master:7077

```

## Query sample data to test conectivity 

make sure to enter every spark command in single line

```bash 
val df = spark.read
  .option("header", "true")
  .csv("hdfs://namenode:9000/datasets/mydata/Amazon-Sale-Report.csv")

df.show(10)

```
