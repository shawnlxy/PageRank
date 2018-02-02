# PageRank

## Prerequisites 

* [Docker](https://docs.docker.com/) - Container

### How to build hadoop cluster on Docker

```
$ mkdir bigdata
$ cd bigdata
$ sudo docker pull joway/hadoop-cluster 
$ git clone https://github.com/joway/hadoop-cluster-docker 
$ sudo docker network create --driver=bridge hadoop 
$ cd hadoop-cluster-docker

$ sudo ./start-container.sh

$ ./start-hadoop.sh
```


## Get Started:
Download project:
```
cd ~/src
git clone https://github.com/shawnlxy/PageRank.git
```
Enter hadoop:
```
cd hadoop-cluster-docker
./start-container.sh
./start-hadoop.sh
cd src
```
HDFS operations:
```
cd PageRank/src/main/java/
hdfs dfs -rm -r /transition 

hdfs dfs -mkdir /transition # 在hdfs中创建 /transition目录

hdfs dfs -put transitionsmall.txt /transition 

hdfs dfs -rm -r /output* 

hdfs dfs -rm -r /pagerank* 

hdfs dfs -mkdir /pagerank0 

hdfs dfs -put prsmall.txt /pagerank0 

hadoop com.sun.tools.javac.Main *.java 

jar cf pr.jar *.class 

hadoop jar pr.jar Driver /transition /pagerank /output 
//args0: dir of transition.txt
//args1: dir of PageRank.txt
//args2: dir of unitMultiplication result
//args3: times of convergence（make sure the code run successfully when args3=1, then test args3=40）

```
final result will be stored in /pagerankN(N is iteration number)

we can use hdfs dfs -cat /pagerank1/* to check result

![screen shot 2018-02-02 at 2 08 07 am](https://user-images.githubusercontent.com/36029186/35721399-d267662c-07bf-11e8-9dbe-19ddab88c907.png)

Then we can change small dataset into larger dataset.

Just replace transitionsmall.txt with transition.txt， replace prsmall.txt with pr.txt.

![screen shot 2018-02-02 at 2 13 34 am](https://user-images.githubusercontent.com/36029186/35721412-e4ed88d0-07bf-11e8-8519-3e5d39589ce8.png)

The output in HDFS can be transferred to local
```
hdfs dfs -get <src> <localDest>

for example:
hdfs dfs -get /pagerank1/part-r-00000 pr1.txt
```
