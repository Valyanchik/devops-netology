# Домашнее задание к занятию "6.3. MySQL"


## Задача 1

```bash
##Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
valyan@valyan-pc:~$ docker pull mysql:8.0
8.0: Pulling from library/mysql
c32ce6654453: Pull complete 
415d08ee031a: Pull complete 
7a38fec2542f: Pull complete 
352881ee8fe9: Pull complete 
b8e20da291b6: Pull complete 
66c2a8cc1999: Pull complete 
d3a3a8e49878: Pull complete 
e33a48832bec: Pull complete 
410b942b8b28: Pull complete 
d5323c9dd265: Pull complete 
e51041021063: Pull complete 
b68b4cbc496e: Pull complete 
Digest: sha256:dc3cdcf3025c3257e8047bb0eaee9d5a42d9f694f84fc5e7b6d12710ba7f6fcb
Status: Downloaded newer image for mysql:8.0
docker.io/library/mysql:8.0
valyan@valyan-pc:~$ docker volume create vol_mysql
vol_mysql
valyan@valyan-pc:~$ docker run --rm --name mysql-docker -e MYSQL_ROOT_PASSWORD=mysql -ti -p 3306:3306 -v vol_mysql:/etc/mysql/ mysql:8.0
2022-05-25 19:58:51+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.29-1debian10 started.

##Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и восстановитесь из него.
alyan@valyan-pc:~$ docker cp test_dump.sql mysql-docker:/etc/mysql/test_dump.sql

##Перейдите в управляющую консоль `mysql` внутри контейнера.
valyan@valyan-pc:~$ docker exec -it mysql-docker bash
root@bd569d236bdc:~# mysql -u root -p 
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 20
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> create database test_db;
Query OK, 1 row affected (0.01 sec)

mysql> exit
Bye

## restoring db from backup
root@bd569d236bdc:~# mysql -u root -p -D test_db < /etc/mysql/test_dump.sql
Enter password: 
root@bd569d236bdc:~# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 20
Server version: 8.0.29 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)

###Используя команду `\h` получите список управляющих команд.
mysql> \h
For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/
List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for 'help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Dont write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Dont show warnings after every statement.
resetconnection(\x) Clean session context.
query_attributes Sets string parameters (name1 value1 name2 value2 ...) for the next query to pick up.
ssl_session_data_print Serializes the current SSL session data to stdout or file
For server side help, type 'help contents'

##Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.
mysql> \s
--------------
mysql  Ver 8.0.29 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		20
Current database:	
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.0.29 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	latin1
Conn.  characterset:	latin1
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			29 min 31 sec

Threads: 2  Questions: 58  Slow queries: 0  Opens: 159  Flush tables: 3  Open tables: 77  Queries per second avg: 0.032
--------------

##Подключитесь к восстановленной БД и получите список таблиц из этой БД.
mysql> use test_db
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)

##Приведите в ответе количество записей с `price` > 300.
mysql> select count(*) from orders where price >300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)

```
## Задача 2



   


Создайте пользователя test в БД c паролем test-pass, используя:
- плагин авторизации mysql_native_password
- срок истечения пароля - 180 дней 
- количество попыток авторизации - 3 
- максимальное количество запросов в час - 100
- аттрибуты пользователя:
    - Фамилия "Pretty"
    - Имя "James"
```bash
mysql> create user 'test'@'localhost' identified by 'test-pass';
Query OK, 0 rows affected (0.03 sec)

mysql> alter user 'test'@'localhost'
    -> identified by 'test-pass'
    -> with
    -> max_queries_per_hour 100
    -> password expire interval 180 day
    -> failed_login_attempts 3 password_lock_time 2;
Query OK, 0 rows affected (0.01 sec)

##Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.
mysql> grant select on test_db.orders to 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.03 sec)

##Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и приведите в ответе к задаче.
mysql> select * from information_schema.user_attributes where user='test';
+------+-----------+-----------+
| USER | HOST      | ATTRIBUTE |
+------+-----------+-----------+
| test | localhost | NULL      |
+------+-----------+-----------+
1 row in set (0.00 sec)

mysql>
```
## Задача 3

```bash
##Установите профилирование `SET profiling = 1`.
mysql> SET profiling = 1;
Query OK, 0 rows affected, 1 warning (0.00 sec)

##Изменияем engine на MyISAM
mysql> ALTER TABLE orders ENGINE = MyISAM;
Query OK, 5 rows affected (0.06 sec)
Records: 5  Duplicates: 0  Warnings: 0

##Изменияем engine на InnoDB
mysql> ALTER TABLE orders ENGINE = InnoDB;
Query OK, 5 rows affected (0.05 sec)
Records: 5  Duplicates: 0  Warnings: 0

##Изучите вывод профилирования команд `SHOW PROFILES;`.
mysql> SHOW PROFILES;
+----------+------------+------------------------------------+
| Query_ID | Duration   | Query                              |
+----------+------------+------------------------------------+
|        1 | 0.00020000 | show profiles all                  |
|        2 | 0.00025225 | SET profiling = 1                  |
|        3 | 0.00024775 | SET profiling = 1                  |
|        4 | 0.05763725 | ALTER TABLE orders ENGINE = MyISAM |
|        5 | 0.04482025 | ALTER TABLE orders ENGINE = InnoDB |
+----------+------------+------------------------------------+
5 rows in set, 1 warning (0.00 sec)

##Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.
mysql> SELECT TABLE_NAME,ENGINE,ROW_FORMAT,TABLE_ROWS,DATA_LENGTH,INDEX_LENGTH FROM information_schema.TABLES WHERE table_name = 'orders' and  TABLE_SCHEMA = 'test_db' ORDER BY ENGINE asc;
+------------+--------+------------+------------+-------------+--------------+
| TABLE_NAME | ENGINE | ROW_FORMAT | TABLE_ROWS | DATA_LENGTH | INDEX_LENGTH |
+------------+--------+------------+------------+-------------+--------------+
| orders     | InnoDB | Dynamic    |          5 |       16384 |            0 |
+------------+--------+------------+------------+-------------+--------------+
1 row in set (0.01 sec)
```
Как видно из вывода команды SHOW PROFILES смена движка на MyISAM выполнилась за 0.05763725 секунды а на InnoDB за 0.04482025 секунды

## Задача 4 

```bash
##Изучите файл `my.cnf` в директории /etc/mysql.

root@bd569d236bdc:/etc/mysql# cat my.cnf
# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

# Custom config should go here
!includedir /etc/mysql/conf.d/
```
Измените его согласно ТЗ (движок InnoDB):
- Скорость IO важнее сохранности данных
- Нужна компрессия таблиц для экономии места на диске
- Размер буффера с незакомиченными транзакциями 1 Мб
- Буффер кеширования 30% от ОЗУ
- Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.
Т.к в контейнере нет текстовых редакторов, создаем конфиг на хостовой машине и копируем его с заменой в контейнер при помощи docker cp:  
```bash
valyan@valyan-pc:~$ nano my.cnf
valyan@valyan-pc:~$ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED             STATUS             PORTS                                                  NAMES
bd569d236bdc   mysql:8.0   "docker-entrypoint.s…"   About an hour ago   Up About an hour   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   mysql-docker
valyan@valyan-pc:~$ docker cp my.cnf mysql-docker:/etc/mysql/my.cnf

##Подключаемся к контейнеру и смотрим, что конфиг-файл изменился по указанному пути
valyan@valyan-pc:~$ docker exec -it mysql-docker bash
root@bd569d236bdc:/# cat /etc/mysql/my.cnf
[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

#Set IO Speed
# 0 - скорость
# 1 - сохранность
# 2 - универсальный параметр
innodb_flush_log_at_trx_commit = 0 

#Set compression
# Barracuda - формат файла с сжатием
innodb_file_format=Barracuda

#Set buffer
innodb_log_buffer_size	= 1M

#Set Cache size
key_buffer_size = 3000М

#Set log size
max_binlog_size	= 100M
```
Движок InnoDB установили на предыдущем шаге.  
Размер памяти для буфера в 30% доступной памяти (получилось 3000МБ) устанавливал исходя из установленной на хостовой машине ОЗУ:
```bash
valyan@valyan-pc:~$ free -h
               total        used        free      shared  buff/cache   available
Память:      9,7Gi       5,4Gi       319Mi       341Mi       3,9Gi       3,6Gi
Подкачка:      2,0Gi        19Mi       2,0Gi
```


