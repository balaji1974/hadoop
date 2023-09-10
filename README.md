# Hadoop
 
## Big data 

```xml
Volume, Velocity and Varity (all the 3 factors contribute to big data)

```

## Hadoop & its components
```xml
It is an open source platform for distributed storage and distributed processing of large datasets 
on computer clusters built on commodity hardware 

HDFS - Hadoop distributed file system - Storage is distributed across cluster and makes the cluster look like one giant storage space.

YARN - Yet another resource negotiator - It manages resources like what gets to run and when, what nodes are available and which are not etc. Its the heartbeat of the cluster. 
MESOS - Its an alternative to YARN 

MapReduce - It consist of mappers that transform the data across the cluster while reducers aggregate this data 
Tez - Alternative to MapReduce, DAG (directed acyclic graph) concept
Spark - It is used to run query on the data using scripts or other programming languages like Java, Python, Scala etc. It can handle streaming data and do machine learning, AG (directed acyclic graph) concept and in-memory processing makes it quite fast. 

Pig - Its a high level programming API that allows us to write simple scripts that chain quries and get complex answers 
Hive - It takes the underlining distributed data and makes it look like a SQL database (query engine)

HBase - Columanar NoSQL data store
External Data Storage - MySQL, Cassandra, MongoDB

Apache Ambari - Its the visual view of the cluster 

Storm - It is a way of processing streaming data 
Oozie - It schedules job across the cluster 

Zookeeper - Used for co-ordinating everything across the cluster, which nodes are up and which nodes join and leave the cluster 
Data Injestion Systems - Kafka (connectors used for transformation and push data into Hadoop), Flume (Way to transfer weblogs to the Hadoop cluster), Sqoop (It is a command-line interface application for transferring data between relational databases and Hadoop, it is now deprecated)

Query Engine - Apache Drill (Write SQL query on NoSQL databases), Hue (alternative to Ambari to write interactive queries), Apache Phoenix (similar to Drill but also gives OLTP gurantees), Presto (another query engine), Apache Zeppelin (another Query tool)

```


## Installation - Based on Oracle Virual Box & Hartonworks Data Platform on MAC 
```xml
On Mac (Fundamentally, HDP requires an x86 chip to run on and cannot run on Apple M1 chip) - This is being developed now
------------------------------------------------------------------------------------------------------------------------
Download and install Oracle Virtual box
https://www.virtualbox.org/wiki/Downloads
Install it

Next download Hortonworks Data Platform from the below link (download version 2.6.5 from the older versions)
https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html
Double click on the image and import it into the virtualbox
After installation, start it and then launch it from the browser using the link:
http://127.0.0.1:1080/
Default username/pwd -> maria_dev/maria_dev 

or go directly into ambari dashboard from the url:
http://127.0.0.1:8080/

You can also ssh into the sandbox using the command:
ssh -p 2222 maria_dev@127.0.0.1
password will be maria_dev


Download the data to be used from the below link:
https://grouplens.org/datasets/movielens/
Download the 100k dataset 

Steps to import the data set using Hive
Go to the dashboard -> Click on the views donoted by 9 dots (on the right)
Go to hive view -> Upload table

File type -> CSV -> settings -> Delimited by Tab (asc 9) -> select file u.data from the dataset downloaded
File name = ratings & Column names are user_id, movie_id, rating, rating_time 
And click on upload table and wait for it to be completed

Next 
ile type -> CSV -> settings -> Delimited by | (asc 124) -> select file u.item from the dataset downloaded
File name = movies & Column names are movie_id, name, <and leave other columns as is > 
And click on upload table and wait for it to be completed

Next goto the SQL tab 
and run the following query: 
SELECT movie_id, count(movie_id) as ratingCount
FROM ratings
GROUP BY movie_id
ORDER BY ratingCount
DESC;

SELECT name
FROM movie_names 
WHERE movie_id=50;


```

## HDFS 
```xml
It is optimized for handling large files
Data is split into blocks (128MB per block - default) and stored across a large cluster of computers 
It consist of name node which keeps track of where the blocks live and a data node  where the actul block of data is stored. 

You can also ssh into the sandbox using the command:
ssh -p 2222 maria_dev@127.0.0.1
password will be maria_dev
```

## Hadoop commands 
```xml

hadoop fs -ls  -> To list the hadoop filesystem 
hadoop fs -mkdir ml-100k -> To create a directory called ml-100k
ls -> This will display the local filesystem and not the hdfs filesystem
pwd -> to display the present working directory of the local filesystem
wget http://media.sundog-soft.com/hadoop/ml-100k/u.data -> This command fetches the file u.data from remote server into the local file system
hadoop fs -copyFromLocal u.data ml-100k/u.data -> This command will copy the file u.data from the local fileystem into the HDFS filesystem's directory ml-100k
hadoop fs -rm ml-100k/u.data -> This will remove the file u.data from the HDFS filesystem
hadoop fs -rmdir ml-100k -> This command will remove the ml-100k directory from the HDFS filesystem 
hadoop fs -> this will display all the commands available in the hadoop filesystem 

```

## Mapreduce 
```xml
Divides data into partitions that are mapped (transformed) and reduced (aggregated) by mapper and reducer functions we define 

Sequence of activies 
Mapper -> Shuffle and Sort -> Reducer 

Mapreduce is written in Java 

It consist of Resource Manager and Node Manager.

The ResourceManager (RM) is responsible for tracking the resources in a cluster, and scheduling applications (YARN)
Node manager is the slave daemon of Yarn. The Hadoop Yarn Node Manager is the per-machine/per-node framework agent who is responsible for containers, monitoring their resource usage and reporting the same to the ResourceManager.

An Application master monitors the worker tasks for errors or hanging 

```

## Change Apache Ambari admin password
```xml
From terminal: 
ssh maria_dev@127.0.0.1 -p 2222
password: maria_dev

Inside the ambari sandbox run the command
su root 
password: hadoop

You will asked for the password again and then the new password along with retyping the new password

After this run the command ambari-admin-password-reset
This will ask for the new password for admin along with retyping the password 

After resetting the server will restart

```

## Apache Ambari 
```xml
Dashboard can be accessed using 
http://127.0.0.1:8080/

Dashboard gives all the metrics of the cluser
All the services that are running can be monitored from the side menu or from the services tab
Host tab gives a view of every host running on the cluster
Alert tab is used for configuring existing alerts and setting up new alerts
Admin tab gives the details of all installed services and their versions
File view gives the view of the HDFS file system
Hive view is used to running hive related queries
Pig view is used to run the pig scripts


```


## Apache Pig 
```xml
Writing Map-Reduce programs takes a long time and sometime complex
Pig Lating is a SQL like syntax to do map and reduce step 
It is highly extensible with user-defined functions

For running the sample scripts below: upload the files u.data and u.item using the ambari file upload tool
into the user/maria_dev/ml-100k folder

Next go the pig view and create a new pig script and run the entire scripts listed below by creating a name for the script and hitting the execute button

Sample pig script: (find movies with ratings > 4)
-------------------------------------------------
ratings = LOAD '/user/maria_dev/ml-100k/u.data' AS (userID:int, movieID:int, rating:int, ratingTime:int);

metadata = LOAD '/user/maria_dev/ml-100k/u.item' USING PigStorage('|')
	AS (movieID:int, movieTitle:chararray, releaseDate:chararray, videoRealese:chararray, imdblink:chararray);
   
nameLookup = FOREACH metadata GENERATE movieID, movieTitle,
	ToUnixTime(ToDate(releaseDate, 'dd-MMM-yyyy')) AS releaseTime;
   
ratingsByMovie = GROUP ratings BY movieID;

avgRatings = FOREACH ratingsByMovie GENERATE group as movieID, AVG(ratings.rating) as avgRating;

fiveStarMovies = FILTER avgRatings BY avgRating > 4.0;

fiveStarsWithData = JOIN fiveStarMovies BY movieID, nameLookup BY movieID;

oldestFiveStarMovies = ORDER fiveStarsWithData BY nameLookup::releaseTime;

DUMP oldestFiveStarMovies;

---------------------------
Next run the same script by going to the script tab and clicking the execute on Tez check box. 
This will run much faster than before. 


Sample pig script: (find movies with worst ratings < 2)
-------------------------------------------------------
ratings = LOAD '/user/maria_dev/ml-100k/u.data' AS (userID:int, movieID:int, rating:int, ratingTime:int);

metadata = LOAD '/user/maria_dev/ml-100k/u.item' USING PigStorage('|')
	AS (movieID:int, movieTitle:chararray, releaseDate:chararray, videoRealese:chararray, imdblink:chararray);
   
nameLookup = FOREACH metadata GENERATE movieID, movieTitle;
   
groupedRatings = GROUP ratings BY movieID;

averageRatings = FOREACH groupedRatings GENERATE group AS movieID, AVG(ratings.rating) AS avgRating, 
	COUNT(ratings.rating) AS numRatings;

badMovies = FILTER averageRatings BY avgRating < 2.0;

namedBadMovies = JOIN badMovies BY movieID, nameLookup BY movieID;

finalResults = FOREACH namedBadMovies GENERATE nameLookup::movieTitle AS movieName, 
	badMovies::avgRating AS avgRating, badMovies::numRatings AS numRatings;

finalResultsSorted = ORDER finalResults BY numRatings DESC;

DUMP finalResultsSorted;

---------------------------


Other key terms in Pig useful for creating relations
LOAD, STORE, DUMP, FILTER, DISTINCT, FOREACH/GENERATE, MAPREDUCE, STREAM, 
SAMPLE, JOIN, COGROUP, GROUP, CROSS, CUBE, ORDER, RANK, LIMIT, UNION, SPLIT 

Diagnostics - DESCRIBE, EXPLAIN, ILLUSTRATE 
User Defined Functions (UDFs) - REGISTER, DEFINE, IMPORT
Other functions and loaders - AVG, CONCAT, COUNT, MAX, MIN, SIZE, SUM 
Storage Classes - PigStorage, TextLoader, JsonLoader, AvroStorage, ParquetLoader, OrcStorage, HBaseStorage 


```


## Apache Spark 
```xml
Its a fast and general engine for large scale data processing
Its scalable with a choice of using mulitple cluster managers like YARN, MESOS or Spark cluster manager
Its many times faster than MapReduce and its DAG (Directed Acyclic Graph) engine optimizes workflow
It can be coded in Python, Java or Scala (Spark is written in scale and it gives the best performance)
Its built on the concept of RDD (Resilient Distributed Dataset)
Its main components are Spark Streaming, Spark SQL, MLLib, GraphX and Spark Core. 


RDD - Resilent Distributed Dataset
Spark shell creates a spark context object which creates a RDD
Eg. 
rdd=sc.parallelize([1,2,3,4])
squaredRDD=rdd.map(lambda x:x*x) 


Work with Spark 
----------------
Ambari dashboard (login using admin user and password) -> Services -> Spark 2 -> Config tab ->  
Advanced spark2-log4j-properties -> 
Change the line-> log4j.rootCategory=INFO, console
To -> log4j.rootCategory=ERROR, console
And save it, then RESTART services 

spark-submit <scriptname> -> this will submit a spark script 

DataFrames: Contains row objects, can run sql quries, has a schema, can read and write to JSON, Hive, parquet and communicates with JDBC/ODBC, Tableau 


```

## Apache Hive - Relational Data Store 
```xml
It translates SQL queries to MapReduce or Tez jobs on your cluster 
HiveQL (SQL like syntax) - Interactive, Scalable, Easy OLAP, Highly optimized and Highly extensible 


Login into Ambari using maria_dev user -> Hive View -> Drop all existing tables -> DROP TABLES <name>;

Upload Table -> File Type -> Settings -> Field Delimiter -> ASCII value 9 (tab delimiter)
Upload from Local -> select u.data -> Enter Table name (ratings) 
Column Names -> userId, movieId, rating, epochseconds -> upload table 


Upload Table -> File Type -> Settings -> Field Delimiter -> ASCII value 124 (pipe delimiter)
Upload from Local -> select u.item -> Enter Table name (names)
Column Names -> movieId, title -> upload table 


Create View and JOIN - Example 
-------------------------------
CREATE VIEW IF NOT EXIST topMovieIDs AS 
SELECT movieid, count(movieid) as ratingCount FROM ratings
GROUP BY movieid 
ORDER BY ratingCount DESC;

SELECT n.title, ratingCount 
FROM topMovieIDs t JOIN names n on t.movieid=n.movieid;

DROP VIEW topMovieIDs;


Schema On Read
--------------
CREATE TABLE ratings (
	userID INT,
	movieID INT,
	rating INT,
	time INT
)
ROW FORMAT DELIMITED 
FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE;

LOAD DATA LOCAL INPATH '${env:HOME}/ml-100k/u.data'
OVERWRITE INTO TABLE ratings 

LOAD DATA -> moves data from distributed filesystem into Hive
LOAD DATA LOCAL -> copies data from your local filesystem into Hive
CREATE EXTERNAL TABLE -> Hive does not take ownership of the table created 
PARTITION BY -> To store data in a partitioned subdirectory 
STRUCT -> To store structured data in the column level 

```

## RDBMS with Hadoop 
```xml

Sqoop -> SQL + Hadoop -> Kicks of MapReduce jobs to import/export large volume of data from RDBMS 

sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver --table movies -> Move data from mysql to HDFS table

sqoop import --connect jdbc:mysql://localhost/movielens --driver com.mysql.jdbc.Driver -table movies --hive-import -> Move data from mysql to Hive

sqoop export --connect jdbc:mysql://localhost/movielens -m 1 --driver com.mysql.jdbc.Driver --table exported_movies --export-dir /apps/hive/warehouse/movies --input-fields-terminated-by '\0001' -> Export data from Hive to MySQL (target table must already be present with columns in same order)


Hartonworks comes with mysql preinstalled
Connect to mysql 
ssh maria_dev@127.0.0.1 -p 2222
password: maria_dev

mysql -u root -p
password: hortonworks1

```

## NoSQL with Hadoop 
```xml

HBase
-----
Login into Ambari dashboard using admin user
Go to HBase -> Service Action -> Start 

Next login into the VM using: ssh maria_dev@127.0.0.1 -p 2222
Next start the rest service -> sudo /usr/hdp/current/hbase-master/bin/hbase-daemon.sh start rest -p 8000 --infoport 8001

Now we can use a REST service to do operations on the HBase (insert/update/delete)

Finally run
Next start the rest service -> sudo /usr/hdp/current/hbase-master/bin/hbase-daemon.sh stop rest


From the command prompt in the VM you can login into Hbase using the command: 
hbase shell


Cassandra
---------
No master node and hence no single point of failure, it follows eventual consistency 

Steps to install and work with Cassendra -> 
 1. ssh maria_dev@127.0.0.1 -p 2222
 2. Enter pwd
 3. su root 
 4. Enter su pwd
 5. python -V -> Check to see if python version is 2.7 or above
 6. cd /etc/yum.repos.d 
 7. vi datastax.repo
 8. Type the following and save the file 
 	[datastax]
	name = DataStax Repo for Apache Cassendra
	baseurl = http://rpm.datastax.com/community
	enabled = 1
	gpgcheck = 0
 9. yum install dsc30
10. cqlsh -> to enter into cassandra 
11. CREATE KEYSPACE movielens with replication = {'class': 'SimpleStrategy', 'replication_factor': '1'} AND durable_writes=true;
12. USE movielens;
12. CREATE TABLE users (user_id int, age int, gender text, occupation text, zip text, PRIMARY KEY(user_id));
13. DESCRIBE users;
14. SELECT * FROM users;

Spark - Cassandra -> To complete later


MongoDB
-------


***Check the nosql repo for HBase setup and usage


```


## Guildlines for choosing Hadoop cluster
```xml
A common rule of thumb is to have no more than 20 nodes per namenode, and to have one datanode for every 1-2 TB of data. Additionally, having one node for every 4-8 CPU cores and 16-32 GB of RAM is a good guideline for processing power.

```







### References
```xml
https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data
https://saturncloud.io/blog/what-is-the-ideal-number-of-nodes-in-a-hadoop-cluster/
```