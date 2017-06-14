
坑的介绍在<a href="https://issues.apache.org/jira/browse/SPARK-15034">这里</a>
```shell
从spark2.0开始，spark不再加载‘hive-site.xml'中的设置，也就是说，hive.metastore.warehouse.dir的设置无效。
spark.sql.warehouse.dir的默认值为System.getProperty("user.dir")/spark-warehouse，需要在spark的配置文件core-site.xml中设置
```
