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
MapReduce - It consist of mappers that transform the data across the cluster while reducers aggregate this data 
Pig - Its a high level programming API that allows us to write simple scripts that chain quries and get complex answers 
Hive - It takes the underlining distributed data and makes it look like a SQL database (query engine)
Apache Ambari - Its the visual view of the cluster 
MESOS - Its an alternative to YARN 
Spark - It is used to run query on the data using scripts or other programming languages like Java, Python, Scala etc. It can handle streaming data and do machine learning. 
Tez - Similar to Spark 
HBase - Columanar data store 
Storm - It is a way of processing streaming data 
Oozie - It schedules job across the cluster 
Zookeeper - Used for co-ordinating everything across the cluster, which nodes are up and which nodes join and leave the cluster 
Data Injestion Systems - Kafka (connectors used for transformation and push data into Hadoop), Flume (Way to transfer weblogs to the Hadoop cluster), Sqoop (It is a command-line interface application for transferring data between relational databases and Hadoop, it is now deprecated)
External Data Storage - MySQL, Cassandra, MongoDB
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

## Apache Ambari 
```xml
Dashboard can be accessed using 
http://127.0.0.1:8080/

Dashboard gives all the metrics of the cluser
All the services that are running can be monitored from the side menu or from the services tab
Host tab gives a view of every host running on the cluster
Alert tab is used for configuring existing alerts and setting up new alerts
Admin tab gives the details of all installed services and their versions

```



### References
```xml
https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data
```