# Hadoop
 
## Big data 

```xml
Volume, Velocity and Varity (all the 3 factors contribute to big data)


```

## Installation 
```xml
On Mac (Fundamentally, HDP requires an x86 chip to run on and cannot run on Apple M1 chip)
------
Download and install Oracle Virtual box
https://www.virtualbox.org/wiki/Downloads
Install it

Next download Hortonworks Data Platform from the below link (download version 2.6.5 from the older versions)
https://www.cloudera.com/downloads/hortonworks-sandbox/hdp.html
Double click on the image and import it into the virtualbox
After installation, start it and then launch it from the browser using the link:
http://127.0.0.1:1080/
Default username/pwd -> maria_dev/maria_dev 



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







### References
```xml
https://www.udemy.com/course/the-ultimate-hands-on-hadoop-tame-your-big-data
```