
```
/user/ubuntu/atlas_higgs.csv 55253673 bytes, 1 block(s):  Under replicated BP-1759922210-172.31.7.59-1496717496059:blk_1073746805_5981. Target Replicas is 2 but found 1 live replica(s), 0 decommissioned replica(s) and 0 decommissioning replica(s).
0. BP-1759922210-172.31.7.59-1496717496059:blk_1073746805_5981 len=55253673 Live_repl=1 [DatanodeInfoWithStorage[172.31.9.28:50010,DS-65d96fa8-c2e4-4783-a32a-bf7ede3b9115,DISK]]
```

The replication count for files submitted as part of your job is controlled by the parameter mapreduce.client.submit.file.replication or mapred.submit.replication in mapred-site.xml. You can adjust this down for clusters that are smaller than 10 nodes.

