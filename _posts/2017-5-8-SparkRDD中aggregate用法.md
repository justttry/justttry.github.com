
---
layout: post
title: About Me, A Student of Google!
---

## 理解Spark RDD中的aggregate函数


```python
seqOp = (lambda x, y: (x[0] + y, x[1] + 1))
combOp = (lambda x, y: (x[0] + y[0], x[1] + y[1]))
sc.parallelize([1, 2, 3, 4]).aggregate((0, 0), seqOp, combOp)
```




    (10, 4)


            (0, 0) <-- zeroValue

[1, 2]                  [3, 4]

0 + 1 = 1               0 + 3 = 3
0 + 1 = 1               0 + 1 = 1

1 + 2 = 3               3 + 4 = 7
1 + 1 = 2               1 + 1 = 2       
    |                    |
    v                    v
  (3, 2)                  (7, 2)
      \                    / 
       \                  /
        \                /
         \              /
          \            /
           \          / 
           ------------
           |  combOp  |
           ------------
                |
                v
             (10, 4)
若zeroValue不为(0, 0)时，实测结果和预期结果不一致。


```python

```
