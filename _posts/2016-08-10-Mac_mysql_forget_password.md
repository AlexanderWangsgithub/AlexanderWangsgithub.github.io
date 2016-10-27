---
layout: post
title: Mac下mysql忘记密码
categories: [blog]
tags: [Database]
description: 
---

Mac下mysql忘记密码

1. Stop mysql in system reference.

2. `sudo ./usr/local/mysql-5.7.13-osx10.11-x86_64/bin/mysqld_safe --skip-grant-tables`

3. Use command `mysql` login in.

4. Update password of root to null.

   ```
   update mysql.user set authentication_string="" where user="root";
   exit;
   ```
5. Set new password.
`/usr/local/mysql-5.7.13-osx10.11-x86_64/bin/mysqladmin -u root -p password wanggang`


```
mysql> update mysql.user set authentication_string=PASSWORD('gangwang') where user='root' and host='localhost';
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
ysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'gangwang' ;
Query OK, 0 rows affected (0.10 sec)

mysql> update mysql.user set authentication_string=PASSWORD('gangwang') where user='root' and host='localhost';
Query OK, 0 rows affected, 1 warning (0.01 sec)
Rows matched: 1  Changed: 0  Warnings: 1

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> exit;
```
