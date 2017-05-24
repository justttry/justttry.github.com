
错误:<span style="color:red">TypeError: Cannot interpret feed_dict key as Tensor: Tensor Tensor("Placeholder_2:0", shape=(500, 500), dtype=float32) is not an element of this graph.</span>

```
Exception in thread Thread-2:                                                   
Traceback (most recent call last):
  File "/home/ubuntu/anaconda2/lib/python2.7/threading.py", line 801, in __bootstrap_inner
    self.run()
  File "/home/ubuntu/anaconda2/lib/python2.7/threading.py", line 754, in run
    self.__target(*self.__args, **self.__kwargs)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/distkeras/job_deployment.py", line 281, in run
    self.read_trained_model()
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/distkeras/job_deployment.py", line 207, in read_trained_model
    self.trained_model = deserialize_keras_model(unpickle_object(f.read()))
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/distkeras/utils.py", line 127, in deserialize_keras_model
    model.set_weights(weights)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/models.py", line 700, in set_weights
    self.model.set_weights(weights)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/engine/topology.py", line 1973, in set_weights
    K.batch_set_value(tuples)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/keras/backend/tensorflow_backend.py", line 2153, in batch_set_value
    get_session().run(assign_ops, feed_dict=feed_dict)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/tensorflow/python/client/session.py", line 778, in run
    run_metadata_ptr)
  File "/home/ubuntu/anaconda2/lib/python2.7/site-packages/tensorflow/python/client/session.py", line 933, in _run
    + e.args[0])
TypeError: Cannot interpret feed_dict key as Tensor: Tensor Tensor("Placeholder_2:0", shape=(500, 500), dtype=float32) is not an element of this graph.

```

错误原因：多线程、分布式环境下，恢复Model时的Tensor Graph和生成Model时不同。

解决方法：将生成Model的Tesor Graph复制到恢复线程(或者DataNode)中。

```
Env:Ubuntu 16.4
    spark
    keras
```

Solution:

1.Right after loading or constructing your model, save the TensorFlow graph:

```python
graph = tf.get_default_graph()
```

2.In the other thread (or perhaps in an asynchronous event handler), do:

```python
global graph
with graph.as_default():
    (... do inference here ...)
```

I learned about this from https://github.com/fchollet/keras/issues/2397

## 具体操作如下:

```python
## main.py
...
from keras.models import Sequential
...
model = Sequential()
model.add(Dense(500, input_shape=(nb_features,)))
model.add(Activation('relu'))
model.add(Dropout(0.4))
model.add(Dense(500))
model.add(Activation('relu'))
model.add(Dense(nb_classes))
model.add(Activation('softmax'))

model.summary()

from deployment import graph
graph.append(tf.get_default_graph())
```


```python
## deployment.py

graph = []

class Job(object):
	...
    def run(self):
        time.sleep(1)
        while not self.is_finished():
            time.sleep(10)
        global graph
        with graph[0].as_default():        
            (... do inference here ...)
 ```
