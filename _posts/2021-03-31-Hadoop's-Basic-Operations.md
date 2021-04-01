---
title: "Hadoop's basic operations"
categories:
  - blog
tags:
  - Hadoop
---

Here are some basics operation for Hadoop

```console
hadoop fs ls # list base directory

hadoop fs -mkdir -p /somedir # create some directory under base

hdfs dfs -put /root/sth /somedir # copy some local directory to /somedir

hadoop fs -rm # same as rm

hadoop fs -rm -r # same as rm -r
```
