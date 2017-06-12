
### 报错如下：
<span style="color:red">17/06/09 10:21:42 WARN scheduler.TaskSetManager: Lost task 3.0 in stage 1.0 (TID 4, slave1, executor 4): org.apache.hadoop.hdfs.BlockMissingException: Could not obtain block: BP-1759922210-172.31.7.59-1496717496059:blk_1073746824_6000 file=/user/ubuntu/data_path/training_set.parquet/part-00005-4499448f-ac1c-41bd-940c-448f6174663d.snappy.parquet

输入命令
```
    hadoop fsck / -files -blocks -locations
```
可以看到

```
/user/ubuntu/atlas_higgs.csv 55253673 bytes, 1 block(s):  Under replicated BP-1759922210-172.31.7.59-1496717496059:blk_1073746805_5981. Target Replicas is 2 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
0. BP-1759922210-172.31.7.59-1496717496059:blk_1073746805_5981 len=55253673 Live_repl=1 [DatanodeInfoWithStorage[172.31.9.28:50010,DS-65d96fa8-c2e4-4783-a32a-bf7ede3b9115,DISK]]
```

The replication count for files submitted as part of your job is controlled by the parameter mapreduce.client.submit.file.replication or mapred.submit.replication in mapred-site.xml. You can adjust this down for clusters that are smaller than 10 nodes.


修改设置如下：
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.client.submit.file.replication</name>
        <value>2</value>
    </property>
</configuration>
```

依然不起作用

<span style="color:green">重新格式化namenode之后问题消失。可能是由于当前hadoop节点和之前的配置不同导致的。</span>
