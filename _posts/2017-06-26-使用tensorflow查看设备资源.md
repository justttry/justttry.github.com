

```python
sc
```




    <pyspark.context.SparkContext at 0x7fd8f4a766d0>




```python
from tensorflow.python.client import device_lib
```


```python
dl = device_lib.list_local_devices()
```


```python
dl
```




    [name: "/cpu:0"
     device_type: "CPU"
     memory_limit: 268435456
     locality {
     }
     incarnation: 18352378554694701109, name: "/gpu:0"
     device_type: "GPU"
     memory_limit: 145358848
     locality {
       bus_id: 1
     }
     incarnation: 3909662228664244928
     physical_device_desc: "device: 0, name: Tesla K80, pci bus id: 0000:00:1e.0"]




```python
cpu = dl[0]
```


```python
print cpu.name
print cpu.device_type
```

    /cpu:0
    CPU



```python
gpu = dl[1]
```


```python
print gpu.name
print gpu.device_type
```

    /gpu:0
    GPU

