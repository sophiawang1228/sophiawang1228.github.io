---
title: "Install MySql on mac using Homebrew"
categories:
  - blog
tags:
  - MySql
toc: true
---

## Pre-requisites

- Homebrew

## Installation

```console
brew install mysql
```

## Secure your database

Since by default, brew installs MySql without a password, we need to
manually secure it.

If you are only using MySql locally, you can skip this part.

```console
mysql_secure_installation
```

After this command, you will be prompt to set up levels of your password
validation policy, your password, and some other miscellaneous stuff.
Answer them based on your own situation


## Starting MySql

```console
# start mysql on startup
brew service start mysql

# stop mysql from launching on startup
brew service stop mysql

# launch mysql
mysql.server start

# stop mysql
mysql.server end
```

## Using Mysql

Now that you have your mysql up and running, here's what to do
```console
#connect to the root, you will be prompt to enter password if you have one
mysql -u root -p
```

You're done! Now you can start using MySql.