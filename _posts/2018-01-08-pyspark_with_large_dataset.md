---
layout: post
title:  "Enlarge Executor Memory Size and Result Size in PySpark"
date:   2018-01-08
categories: coding
tags: python pyspark
---

I'm working on some sequential pattern mining tasks recently, and I found that Spark has an implementation of it, so I installed PySpark to play with it. However, my data contains hundreds of sequences, and each sequence includes thousands of items, which results in a data set that is way larger than the toy example provided in their tutorial.

To run PrefixSpan on my data, I need to enlarge the executor memory. I suppose to edit a configuration file located at `$SPARK_HOME/conf/spark-defaults.conf`, but here is the tricky part: I do not have a standalone Spark installed, all I have is PySpark, so where should I find `$SPARK_HOME` and that `spark-defaults.conf ` file?

It turns out that PySpark comes with its Spark binaries, so the first thing I need to do is find where the PySpark is. I use Python to print out the location of PySpark:

```bash
Python 3.4.3 (default, Oct 14 2015, 20:28:29)
[GCC 4.8.4] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pyspark
>>> print(pyspark.__file__)
/REDACTED/py3env/lib/python3.4/site-packages/pyspark/__init__.py
```

Okay, it is at `/REDACTED/py3env/lib/python3.4/site-packages/pyspark/`. I `cd`ed to that directory, and find no `conf` folder under it, so I created one and put a `spark-defaults.conf` in it with the following content:

```
spark.driver.memory 16g
spark.executor.memory 16g
spark.driver.maxResultSize 8g
```

Now use `ps -ef | grep spark` to find all `pyspark` processes and kill them with the `kill` command, you may see something looks like

```
py3env/lib/python3.4/site-packages/pyspark/jars/* -Xmx1g org.apache.spark.deploy.SparkSubmit pyspark-shell
```

kill them, kill them all.

After you killed all existing spark instances, try run your code again or simply run `pyspark`, then run `ps -ef | grep spark` again, you will see it is running with the new `-Xmx16g` flag.

Enjoy!