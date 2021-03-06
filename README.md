# :tada:Spark-Util 
There are many components in bigdata ecology. As the mainstream technology of bigdata, spark often combines with other components in actual development. But spark official did not respond to the toolkit to support it.
Here is mainly encapsulated some common components with spark. For some simple needs, or newcomers may be more suitable. The following code is used in the production line and runs smoothly. Bug has not been discovered for the time being. If you use any bug or new idea in the process, you can leave a message.------LinMingQiang  <br>
## Support
---
|                    | scala version      |Kafka version       | hbase 1.0+         | es   2.3.0         |kudu  1.3.0         |SSL         |
|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| **spark 1.3.x**    | 2.10               | 0.8               | :ok_hand: | :star2: | :eggplant: |NO |
| **spark 1.6.x**    | 2.10               | 0.8               | :baby_chick: | :santa: | :corn: |NO |
| **spark 1.6.x**    | 2.10               | 0.10+               | :baby_chick: | :santa: | :corn: |YES |
| **spark 2.0.x**    | 2.10/2.11          | 0.8+               | :smiley: | :cherries: | :peach: |YES |
---

## :jack_o_lantern: Table of contents
- [Spark kafka /sparkstreaming kafka](#Spark-kafka)
- [Spark Hbase](#spark-Hbase)
- [Spark ES Util](#Spark-ES-Util)
- [Spark Kudu](#Spark-Kudu)
- [Flink kafka](#Flink-kafka)
- [Kafka Util](#Kafka-Util)
- [Hbase Util](#Hbase-Util)
- [Database util](#database-util)
- [Elasticserach shade](#Elasticserach-shade)
- [Rabbitmq util](#Rabbitmq-util)
- [Splunk](#Splunk)

<a name="Spark-kafka"></a>
Spark kafka
------------
 - Encapsulated spark/sparkstreaming to read Kafka with Low level integration (offset in zookeeper)。Provides many configuration parameters to control the way to read Kafka data
 - Support topic to add new partition
 - Supporting RDD data to write to Kafka
 - Support Kafka SSL (0.10+,spark 1.6+)
 - Add parameters ： 'kafka.consumer.from' To dynamically decide whether to get Kafka data from last or from consumption point
 - The version support of spark2.x Kafka 0.10+ is provided.（0.8, there is a big change compared to the 0.10 version.）
 - https://github.com/LinMingQiang/spark-kafka
 ```
   val kp = SparkKafkaContext.getKafkaParam(brokers,groupId,"consum","earliest") 
   val skc = new SparkKafkaContext(kp,sparkconf) 
   val kafkadataRdd = skc.kafkaRDD(topics,last,msgHandle)
   //...do something 
   kafkadataRdd.updateOffsets(groupId)//update offset to zk
 ```

<a name="spark-Hbase"></a>
Spark Hbase
------------
 * spark scan hbase data to RDD <br>
  scan -> RDD[T]
 * spark RDD[T] get from hbase to RDD[U] <br>
  RDD[T] -> Get -> RDD[U]
 * spark RDD[T] write to hbase <br>
  RDD[T] -> Put -> Hbase
 * spark RDD[T] update with hbase data  <br>
  RDD[T] -> Get -> Combine -> RDD[U] <br>
 * spark RDD[T] update with hbase data then put return to hbase <br>
  RDD[T] -> Get -> Combine -> Put -> Hbase
 - https://github.com/LinMingQiang/spark-util/tree/spark-hbase
 ```
    val conf = new SparkConf().setMaster("local").setAppName("tets")
    val sc = new SparkContext(conf)
    val hc = new SparkHBaseContext(sc, zk)
    hc.hbaseRDD(tablename, f).foreach { println }
    hc.scanHbaseRDD(tablename, new Scan(), f)
```
 
<a name="Spark-ES-Util"></a>
Spark ES Util
------------
- ElasticSearch integration for Apache Spark  <br>
- Scanning es data into RDD <br>
- https://github.com/LinMingQiang/spark-util/tree/spark-es
```
sc.esRDD("testindex/testtype", query)

```

<a name="Spark-Kudu"></a>
Spark Kudu
------------
- Read kudu data into RDD <br>
- Write RDD data to kudu <br>
- draw lessons from: https://github.com/tmalaska/SparkOnKudu
- https://github.com/LinMingQiang/spark-util/tree/spark-kudu

<a name="Flink-kafka"></a>
Flink kafka
------------
* This is a simple example. Read Kafka data, implement WordCount statistics and write to HBase <br>
- https://github.com/LinMingQiang/flink-demo

<a name="Splunk"></a>
Splunk
------------
* Splunk is a log display and monitoring system.
* Installation and use of Splunk <br>
- https://github.com/LinMingQiang/spark-util/tree/splunk

<a name="Kafka-Util"></a>
Kafka Util
------------
* Operate the tool class of kafka, provide offset to record topic by day, mainly used for day recalculation, hour recalculation and other functions  <br>
- https://github.com/LinMingQiang/spark-util/tree/kafka-util

<a name="Hbase-Util"></a>
Hbase Util
------------
* The tool class that operates Hbase, inquires the region information of HBase table, used for manual split some excessive region  <br>
- https://github.com/LinMingQiang/spark-util/tree/hbase-util

<a name="Database-util"></a>
database util
------------
* Provides a connection tool for each database. include: es,hbase,mysql,mongo  <br>
- https://github.com/LinMingQiang/spark-util/tree/database-util

<a name="Elasticserach-shade"></a>
Elasticserach shade
------------
* Resolving conflicts between ES and spark and Hadoop related packages <br>
- https://github.com/LinMingQiang/spark-util/tree/es-shaed

<a name="Rabbitmq-util"></a>
Rabbitmq util
------------
* A tool class for sending and consuming MQ messages  <br>
https://github.com/LinMingQiang/spark-util/tree/rabbitmq-util

