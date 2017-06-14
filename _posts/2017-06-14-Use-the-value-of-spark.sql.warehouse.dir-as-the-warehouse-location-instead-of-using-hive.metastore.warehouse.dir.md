
坑的介绍在<a href="https://issues.apache.org/jira/browse/SPARK-15034">这里</a>
```shell
从spark2.0开始，spark不再加载‘hive-site.xml'中的设置，也就是说，hive.metastore.warehouse.dir的设置无效。
spark.sql.warehouse.dir的默认值为System.getProperty("user.dir")/spark-warehouse，需要在spark的配置文件core-site.xml中设置
```

<a href="https://stackoverflow.com/questions/26545524/there-are-0-datanodes-running-and-no-nodes-are-excluded-in-this-operation">这里</a>还有一个坑,
遇到这种问题时需要清空$HADOOP_HOME/tmp里面的东西

```shell
$ rm -rf $HADOOP_HOME/tmp
$ mkdir -p $HADOOP_HOME/tmp
$ sudo chmod 750 $HADOOP_HOME/tmp
```

```shell
pyspark --master yarn --deploy-mode client --num-executors 7 --executor-cores 2 --conf spark.sql.warehouse.dir=hdfs://user/hive/warehouse
```
