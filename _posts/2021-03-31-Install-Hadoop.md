---
title: "Install Hadoop on mac using Homebrew"
categories:
  - blog
tags:
  - Hadoop
---


The following steps are taken from this [tutorial](https://medium.com/beeranddiapers/installing-hadoop-on-mac-a9a3649dbc4d).

After successfully installed and launching hadoop, you can view:

- ResourceManager: [https://localhost:9870](https://localhost:9870)
- JobTracker: [https://localhost:8088](https://localhost:8088)
- Node specific info: [https://localhost:8042](https://localhost:8042)

## Pre-requistes

 Before installing hadoop, first we need to have the following pre-requistes installed.

- Java 8 or above
- [Homebrew](https://brew.sh)
- ssh

### Setup ssh

```console
#check if ssh is working
ssh localhost

#if not, run following to enable ssh
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

Enable Remote Login: “System Preferences” -> “Sharing”. Check “Remote Login”


## Install Hadoop

```console
brew install hadoop
```


## Configure Hadoop

 Hadoop files are located in `/usr/local/Cellar/hadoop/3.3.0/libexec`. Add this to your `bash_profile`

```console
# ~/.bash_profile

export HADOOP_HOME=/usr/local/CEllar/hadoop/3.3.0/libexec
```

Then go to your `$HADOOP_HOME/etc/hadoop` directory and edit the following files
### hadoop-env.sh

```console
# get java home location
/usr/libexec/java_home

# open hadoop-env.sh and add the result to variable JAVA_HOME
```

### core-site.xml
```xml
<!-- Put site-specific property overrides in this file. -->
<configuration>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
    <description>A base for other temporary directories</description>             
  </property>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:8020</value>
  </property>
</configuration>
```

### mapred-site.xml
```xml
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:8021</value>
  </property>
</configuration>
```

### hdfs-site.xml
```xml
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```

## Format HDFS

```console
cd /usr/local/opt/hadoop
hdfs namenode -format
```

## Start Hadoop
```console
#cd to your HADOOP_HOME dir first

#start namenode and datanode
sbin/start-dfs.sh 

#start resourseManager and nodeManager
sbin/start-yarn.sh
```

## check running service
```console
jps
```

## Stop Hadoop
```console
sbin/stop-all.sh
```



