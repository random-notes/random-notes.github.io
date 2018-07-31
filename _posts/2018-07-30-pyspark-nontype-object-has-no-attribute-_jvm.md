---
layout: post
title:  "PySpark AttributeError: 'NoneType' object has no attribute '_jvm'"
date:   2018-07-30
categories: coding
tags: python pyspark
---

I encountered this `PySpark` problem recently and discovered that the root cause is overriding the Python internal functions...

If you are having the same

```
pyspark AttributeError: 'NoneType' object has no attribute '_jvm'
```

error, check if you accidentally include all PySpark SQL functions using the following code:

```python
from pyspark.sql.functions import *
```

This will override Python's functions such as `min`, `max`, `sum`, and cause the aforementioned problem.

Changing it to

```python
import pyspark.sql.functions as F
```

and call PySpark's `min`, `max` functions via `F.min` and `F.max` will fix it.

Hope this helps.
