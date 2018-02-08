---
title: Get a List of HDFS Files in Python
date: 2017-09-26 15:45:43
tags: [spark, hadoop, python]
categories: Spark
comments:
toc: true
thumbnail:
banner:
---

In an ad hoc work, I need to read in files in multiple HDFS directories based on a date range. 

The HDFS data structure is like the following 

>```
/data
    /20170730
        /part-00000
        /...
    /20170731
    /20170801
    /20170802
    ...
    /20170903
```

Provided a date e.g.20170801, I need to read in the files from folder `/data/20170801`, `/data/20170802`, ..., `/data/20170830`, but not others.

So to achieve this inside my python script, I searched online and finally arrived at the following solution.

```py
import subprocess

dir_in = "/data"
args = "hdfs dfs -ls "+dir_in+" | awk '{print $8}'"
proc = subprocess.Popen(args, stdout=subprocess.PIPE, stderr=subprocess.PIPE, shell=True)

s_output, s_err = proc.communicate()
all_dart_dirs = s_output.split()
# ['/data/20170730', '/data/20170731', ...]
```

Then, so fit my specific needs, I just need to do a simple filtering for the list.







