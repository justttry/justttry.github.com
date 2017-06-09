
###  报错如下：
<span style="color:red">[Stage 2:=============================>                             (1 + 1) / 2]17/06/09 06:48:54 WARN cluster.YarnSchedulerBackend$YarnSchedulerEndpoint: Container marked as failed: container_1496990795119_0001_02_000002 on host: slave1. Exit status: -1000. Diagnostics: Could not obtain block: BP-1759922210-172.31.7.59-1496717496059:blk_1073744745_3921 file=/user/ubuntu/.sparkStaging/application_1496990795119_0001/py4j-0.10.4-src.zip
org.apache.hadoop.hdfs.BlockMissingException: Could not obtain block: BP-1759922210-172.31.7.59-1496717496059:blk_1073744745_3921 file=/user/ubuntu/.sparkStaging/application_1496990795119_0001/py4j-0.10.4-src.zip </span>

The problem is more likely a lack of correlation between Spark's request for RAM (driver memory + executor memory) and Yarn's container sizing configuration. Yarn settings determine min/max container sizes, and should be based on available physical memory, number of nodes, etc. As a rule of thumb, try making the minimum Yarn container size 1.5 times the size of the requested driver/executor memory (in this case, 1.5 GB).

###  修改设置如下：

```
    <property>  
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>64512</value>
    </property> 
    <property>
        <name>yarn.scheduler.maximum-allocation-mb</name>
        <value>51200</value>
    </property>
    <property>
        <name>yarn.nodemanager.resource.cpu-vcores</name>
        <value>6</value>
    </property> 
```
