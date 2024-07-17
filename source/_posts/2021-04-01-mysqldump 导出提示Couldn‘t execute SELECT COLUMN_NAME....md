---
title: mysqldump 导出提示Couldn‘t execute SELECT COLUMN_NAME...
date: 2021-04-01 08:29:53
categories:
- MySql
tags:
- MySql
---



**mysqldump命令:**

　　导出数据库：mysqldump -h ip -u root -p dbname > db.sql;

　　导出数据库中的某个表：mysqldump -h ip -u root -p dbname tablename > tablename.sql;

**错误提示:**

　　mysqldump: Couldn't execute 'SELECT COLUMN_NAME, JSON_EXTRACT(HISTOGRAM, '$."number-of-buckets-specified"') FROM information_schema.COLUMN_STATISTICS

**原因:**

　　因为新版的mysqldump默认启用了一个新标志，通过`--column-statistics=0`来禁用他

**解决方法:**

　　mysqldump --column-statistics=0 -h ip -u root -p dbname > db.sql;