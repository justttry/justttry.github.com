
# Hadoop Datanode问题排除一

查看datanode的logs--http://slave:50075/logs/yarn-ubuntu-nodemanager-ip-172-31-0-241.log

查找Error关键词，可以看到下列错误信息

2017-05-19 02:04:06,480 WARN org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection: Directory /home/ubuntu/Download/hadoop-2.8.0/tmp/nm-local-dir <span style="color:red">error</span>, used space above threshold of <span style="color:red">90.0%</span>, removing from list of valid directories<br/>
2017-05-19 02:04:06,480 WARN org.apache.hadoop.yarn.server.nodemanager.DirectoryCollection: Directory /home/ubuntu/Download/hadoop-2.8.0/logs/userlogs <span style="color:red">error</span>, used space above threshold of <span style="color:red">90.0%</span>, removing from list of valid directories<br/>
2017-05-19 02:04:06,481 INFO org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService: Disk(s) failed: 1/1 local-dirs are bad: /home/ubuntu/Download/hadoop-2.8.0/tmp/nm-local-dir; 1/1 log-dirs are bad: /home/ubuntu/Download/hadoop-2.8.0/logs/userlogs<br/>
2017-05-19 02:04:06,481 ERROR org.apache.hadoop.yarn.server.nodemanager.LocalDirsHandlerService: Most of the disks <span style="color:red">failed</span>. 1/1 local-dirs are bad: /home/ubuntu/Download/hadoop-2.8.0/tmp/nm-local-dir; 1/1 log-dirs are bad: /home/ubuntu/Download/hadoop-2.8.0/logs/userlogs

## 很明显，磁盘空间使用量超过了最大阈值

解决方法：
在yarn-site.xml中配置：
```
    <property>
        <name>yarn.nodemanager.disk-health-checker.max-disk-utilization-per-disk-percentage</name>
        <value>95.0</value>
    </property>
```
然后重启yarn！ <br/>
sbin/yarn-daemon.sh stop nodemanager <br/>
sbin/yarn-daemon.sh start nodemanager <br/>


```python

```
