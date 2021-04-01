---
title: "Install Hive on mac using Homebrew"
categories:
  - blog
tags:
  - Hadoop
  - Hive
---

## Pre-requisites

- Java
- Hadoop [Installation Guide]({% link _posts/2021-03-31-Install-Hadoop.md %})
- Homebrew
- MySql [Installation Guide]({% link _posts/2021-04-01-Install-Mysql.md %})

## Install Hive

```console
brew update
brew install hive
```

## Configure Environmental Variable

```console
# ~/.bashrc
export HADOOP_HOME=/usr/local/Cellar/hadoop/3.3.0/libexec
export HIVE_HOME=/usr/local/Cellar/hive/3.1.2_3/libexec
```

## Run Hive
```console
$ hive
hive> show tables
```