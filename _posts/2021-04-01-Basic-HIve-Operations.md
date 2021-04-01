---
title: "Basic Hive Operations"
categories:
  - blog
tags:
  - Hadoop
  - Hive
---

# DDL

DDL is used for tablewise operations such as create/alter/drop tables. For more information see
[Hive Data Definition Language](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL)
```console
# create table1 with a integer column col1 and string column col2
hive> CREATE TABLE table1(col1 INT, col2 STRING);

# create table with partition
hive> CREATE TABLE table2(col1 INT, col2,STRING) PARTITIONED BY(ds STRING)

# list all table
hive> SHOW TABLES

# list all table with matching pattern(end with s)
hive> SHOW TABLES '.*s'

# list columns
hive> DESCRIBE table1

# alter tables
hive> ALTER TABLE table1 RENAME TO table3;
hive> ALTER TABLE table3 ADD COLUMNS (new_col INT);
hive> ALTER TABLE table2 ADD COLUMNS (new_col2 INT COMMENT 'a comment');
hive> ALTER TABLE table2 REPLACE COLUMNS (foo INT, bar STRING, baz INT COMMENT 'baz replaces new_col2');
hive> ALTER TABLE invites REPLACE COLUMNS (foo INT COMMENT 'only keep the first column');

# drop table
hive> DROP TABLE table3;
```
