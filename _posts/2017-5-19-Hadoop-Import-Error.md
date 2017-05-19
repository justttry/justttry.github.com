
## 错误如下 (http://slave:8042/node/containerlogs/container_1495180985612_0001_01_000002/ubuntu/stderr?start=-4096)
nd = serializer.loads(command.value)<br/>
  File "/home/ubuntu/Download/spark-2.1.1/python/pyspark/serializers.py", line 454, in loads
    return pickle.loads(obj)<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/distkeras/workers.py", line 13, in <module>
    from distkeras.utils import deserialize_keras_model<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/distkeras/utils.py", line 5, in <module>
    from keras import backend as K<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/__init__.py", line 3, in <module>
    from . import activations<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/activations.py", line 4, in <module>
    from . import backend as K<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/backend/__init__.py", line 73, in <module>
    from .tensorflow_backend import *<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/backend/tensorflow_backend.py", line 1, in <module>
    import tensorflow as tf<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/tensorflow/__init__.py", line 24, in <module>
    from tensorflow.python import *<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/tensorflow/python/__init__.py", line 54, in <module>
    from tensorflow.core.framework.graph_pb2 import *<br/>
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/tensorflow/core/framework/graph_pb2.py", line 6, in <module><br/>
    from google.protobuf import descriptor as _descriptor<br/>
<span style="color:red">ImportError</span>: No module named google.protobuf<br/>

1.确定需要导入的库是否正确安装<br/>
2.PYTHONPATH中是否指定路径<br/>
```
export PYTHONPATH=/home/ubuntu/Download/spark-2.1.1/python:/home/ubuntu/anaconda2/lib/python2.7/site-packages
```

There is another possibility, if you are running a python 2.7.11 or other similar versions, 
```
    sudo pip install protobuf
```
is ok. But if you are in a anaconda environment, you should use 
```
    sudo conda install protobuf
```
