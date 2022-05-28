# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1
```bash
valyan@valyan-pc:~$ docker pull postgres:13
13: Pulling from library/postgres
214ca5fb9032: Already exists 
e6930973d723: Already exists 
aea7c534f4e1: Already exists 
d0ab8814f736: Already exists 
648cc138980a: Already exists 
7804b894301c: Already exists 
cfce56252c3f: Already exists 
8cce7305e3b6: Already exists 
aa0c0227e86a: Pull complete 
8246cee774af: Pull complete 
8a72a860cc25: Pull complete 
d30d1a75aee3: Pull complete 
7dd91cedb27f: Pull complete 
Digest: sha256:e3354da5b11f8c52eca6ae00725acf5bb0d07c17d33ceea95d706188e14a0dd5
Status: Downloaded newer image for postgres:13
docker.io/library/postgres:13
valyan@valyan-pc:~$ docker volume create vol_postgres
vol_postgres
valyan@valyan-pc:~$ docker run --rm --name pg-docker -e POSTGRES_PASSWORD=postgres -ti -p 5432:5432 -v vol_postgres:/var/lib/postgresql/data postgres:13

valyan@valyan-pc:~$ docker exec -it pg-docker bash
root@70f17c4b15d5:/# psql -u postgres
/usr/lib/postgresql/13/bin/psql: invalid option -- 'u'
Try "psql --help" for more information.
root@70f17c4b15d5:/# psql -U postgres
psql (13.7 (Debian 13.7-1.pgdg110+1))
Type "help" for help.

postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
   
-----------+----------+----------+------------+------------+--------------------
---
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres        
  +
           |          |          |            |            | postgres=CTc/postgr
es
(3 rows)

postgres=# \c postgres
You are now connected to database "postgres" as user "postgres".
postgres=# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner   
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | postgres
 pg_catalog | pg_am                   | table | postgres
 pg_catalog | pg_amop                 | table | postgres
 pg_catalog | pg_amproc               | table | postgres
 pg_catalog | pg_attrdef              | table | postgres
 pg_catalog | pg_attribute            | table | postgres
 pg_catalog | pg_auth_members         | table | postgres
 pg_catalog | pg_authid               | table | postgres
 pg_catalog | pg_cast                 | table | postgres
 pg_catalog | pg_class                | table | postgres
 pg_catalog | pg_collation            | table | postgres
 pg_catalog | pg_constraint           | table | postgres
 pg_catalog | pg_conversion           | table | postgres
 pg_catalog | pg_database             | table | postgres
 pg_catalog | pg_db_role_setting      | table | postgres
 pg_catalog | pg_default_acl          | table | postgres
 pg_catalog | pg_depend               | table | postgres
 pg_catalog | pg_description          | table | postgres
 pg_catalog | pg_enum                 | table | postgres
 pg_catalog | pg_event_trigger        | table | postgres

...skipping 1 line
 pg_catalog | pg_foreign_data_wrapper | table | postgres
 pg_catalog | pg_foreign_server       | table | postgres
 pg_catalog | pg_foreign_table        | table | postgres
 pg_catalog | pg_index                | table | postgres
 pg_catalog | pg_inherits             | table | postgres
 pg_catalog | pg_init_privs           | table | postgres
 pg_catalog | pg_language             | table | postgres
 pg_catalog | pg_largeobject          | table | postgres
 pg_catalog | pg_largeobject_metadata | table | postgres
 pg_catalog | pg_namespace            | table | postgres
 pg_catalog | pg_opclass              | table | postgres
 pg_catalog | pg_operator             | table | postgres
 pg_catalog | pg_opfamily             | table | postgres
 pg_catalog | pg_partitioned_table    | table | postgres
 pg_catalog | pg_policy               | table | postgres
 pg_catalog | pg_proc                 | table | postgres
 pg_catalog | pg_publication          | table | postgres
 pg_catalog | pg_publication_rel      | table | postgres
 pg_catalog | pg_range                | table | postgres
 pg_catalog | pg_replication_origin   | table | postgres
 pg_catalog | pg_rewrite              | table | postgres
 pg_catalog | pg_seclabel             | table | postgres
 pg_catalog | pg_sequence             | table | postgres

...skipping 1 line
 pg_catalog | pg_shdescription        | table | postgres
 pg_catalog | pg_shseclabel           | table | postgres
 pg_catalog | pg_statistic            | table | postgres
 pg_catalog | pg_statistic_ext        | table | postgres
 pg_catalog | pg_statistic_ext_data   | table | postgres
 pg_catalog | pg_subscription         | table | postgres
 pg_catalog | pg_subscription_rel     | table | postgres
 pg_catalog | pg_tablespace           | table | postgres
 pg_catalog | pg_transform            | table | postgres
 pg_catalog | pg_trigger              | table | postgres
 pg_catalog | pg_ts_config            | table | postgres
 pg_catalog | pg_ts_config_map        | table | postgres
 pg_catalog | pg_ts_dict              | table | postgres
 pg_catalog | pg_ts_parser            | table | postgres
 pg_catalog | pg_ts_template          | table | postgres
 pg_catalog | pg_type                 | table | postgres
 pg_catalog | pg_user_mapping         | table | postgres
(62 rows)

postgres=# \d[S+] NAME
invalid command \d[S+]
Try \? for help.
postgres=# \dS+ pg_index
                                      Table "pg_catalog.pg_index"
     Column     |     Type     | Collation | Nullable | Default | Storage  | Sta
ts target | Description 
----------------+--------------+-----------+----------+---------+----------+----
----------+-------------
 indexrelid     | oid          |           | not null |         | plain    |    
          | 
 indrelid       | oid          |           | not null |         | plain    |    
          | 
 indnatts       | smallint     |           | not null |         | plain    |    
          | 
 indnkeyatts    | smallint     |           | not null |         | plain    |    
          | 
 indisunique    | boolean      |           | not null |         | plain    |    
          | 
 indisprimary   | boolean      |           | not null |         | plain    |    
          | 
 indisexclusion | boolean      |           | not null |         | plain    |    
          | 
 indimmediate   | boolean      |           | not null |         | plain    |    
          | 
 indisclustered | boolean      |           | not null |         | plain    |    
          | 

...skipping 1 line
 indcheckxmin   | boolean      |           | not null |         | plain    |    
          | 
 indisready     | boolean      |           | not null |         | plain    |    
          | 
 indislive      | boolean      |           | not null |         | plain    |    
          | 
 indisreplident | boolean      |           | not null |         | plain    |    
          | 
 indkey         | int2vector   |           | not null |         | plain    |    
          | 
 indcollation   | oidvector    |           | not null |         | plain    |    
          | 
 indclass       | oidvector    |           | not null |         | plain    |    
          | 
 indoption      | int2vector   |           | not null |         | plain    |    
          | 
 indexprs       | pg_node_tree | C         |          |         | extended |    
          | 
 indpred        | pg_node_tree | C         |          |         | extended |    
          | 
Indexes:
    "pg_index_indexrelid_index" UNIQUE, btree (indexrelid)
    "pg_index_indrelid_index" btree (indrelid)

...skipping 1 line

postgres=# \q
root@70f17c4b15d5:/#
```
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя `psql`.

Воспользуйтесь командой `\?` для вывода подсказки по имеющимся в `psql` управляющим командам.

**Найдите и приведите** управляющие команды для:
- вывода списка БД
- подключения к БД
- вывода списка таблиц
- вывода описания содержимого таблиц
- выхода из psql

## Задача 2
```bash
valyan@valyan-pc:~$ docker cp /home/valyan/test_dump.sql pg-docker:/var/lib/postgresql/data/test_dump.sql


valyan@valyan-pc:~$ docker exec -it pg-docker bash

root@70f17c4b15d5:/var/lib/postgresql/data# ls
base	      pg_ident.conf  pg_serial	   pg_tblspc	postgresql.auto.conf
global	      pg_logical     pg_snapshots  pg_twophase	postgresql.conf
pg_commit_ts  pg_multixact   pg_stat	   PG_VERSION	postmaster.opts
pg_dynshmem   pg_notify      pg_stat_tmp   pg_wal	postmaster.pid
pg_hba.conf   pg_replslot    pg_subtrans   pg_xact	test_dump.sql

postgres=# CREATE DATABASE test_database;
CREATE DATABASE
postgres=# \l
                                   List of databases
     Name      |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
---------------+----------+----------+------------+------------+-----------------------
 postgres      | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
               |          |          |            |            | postgres=CTc/postgres
 template1     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
               |          |          |            |            | postgres=CTc/postgres
 test_database | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
(4 rows)

postgres=# \q
root@70f17c4b15d5:~# psql -U postgres test_database < /var/lib/postgresql/data/test_dump.sql
SET
SET
SET
SET
SET
 set_config 
------------
 
(1 row)

SET
SET
SET
SET
SET
SET
CREATE TABLE
ALTER TABLE
CREATE SEQUENCE
ALTER TABLE
ALTER SEQUENCE
ALTER TABLE
COPY 8
 setval 
--------
      8
(1 row)

ALTER TABLE
root@70f17c4b15d5:~# \c test_database
bash: c: command not found
root@70f17c4b15d5:~# psql -U postgres
psql (13.7 (Debian 13.7-1.pgdg110+1))
Type "help" for help.

postgres=# \c test_database
You are now connected to database "test_database" as user "postgres".
test_database=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner   
--------+--------+-------+----------
 public | orders | table | postgres
(1 row)

test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width 
-----------
         4
        16
         4
(3 rows)


```
Используя `psql` создайте БД `test_database`.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-04-postgresql/test_data).

Восстановите бэкап БД в `test_database`.

Перейдите в управляющую консоль `psql` внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу [pg_stats](https://postgrespro.ru/docs/postgresql/12/view-pg-stats), найдите столбец таблицы `orders` 
с наибольшим средним значением размера элементов в байтах.

**Приведите в ответе** команду, которую вы использовали для вычисления и полученный результат.

## Задача 3
```bash
test_database=# alter table orders rename to orders_origin;
ALTER TABLE
test_database=# create table orders_shard (id integer, title varchar(80), price integer) partiton by range price;
ERROR:  syntax error at or near "partiton"
LINE 1: ...rd (id integer, title varchar(80), price integer) partiton b...
                                                             ^
test_database=# create table orders_shard (id integer, title varchar(80), price integer) partition by range price;
ERROR:  syntax error at or near "price"
LINE 1: ... title varchar(80), price integer) partition by range price;
                                                                 ^
test_database=# create table orders_shard (id integer, title varchar(80), price integer) partition by range(price);
CREATE TABLE
test_database=# create table orders1 partition of orders_shard for values from (0) to (499);
CREATE TABLE
test_database=# create table orders2 partition of orders_shard for values from (499) to (9999999);
CREATE TABLE
test_database=# insert into orders_shard (id, title, price) select * from orders_origin;
INSERT 0 8
test_database=# 
```
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и
поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили
провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

## Задача 4

Используя утилиту `pg_dump` создайте бекап БД `test_database`.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца `title` для таблиц `test_database`?

```bash
root@70f17c4b15d5:/var/lib/postgresql/data# pg_dump -U postgres test_database>test_database_dump.sql
root@70f17c4b15d5:/var/lib/postgresql/data# ls
base	       pg_logical    pg_stat	  pg_wal		test_database_dump.sql
global	       pg_multixact  pg_stat_tmp  pg_xact		test_dump.sql
pg_commit_ts   pg_notify     pg_subtrans  postgresql.auto.conf
pg_dynshmem    pg_replslot   pg_tblspc	  postgresql.conf
pg_hba.conf    pg_serial     pg_twophase  postmaster.opts
pg_ident.conf  pg_snapshots  PG_VERSION   postmaster.pid
root@70f17c4b15d5:/var/lib/postgresql/data# cat test_database_dump.sql
--
-- PostgreSQL database dump
--

-- Dumped from database version 13.7 (Debian 13.7-1.pgdg110+1)
-- Dumped by pg_dump version 13.7 (Debian 13.7-1.pgdg110+1)

SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SELECT pg_catalog.set_config('search_path', '', false);
SET check_function_bodies = false;
SET xmloption = content;
SET client_min_messages = warning;
SET row_security = off;

SET default_tablespace = '';

--
-- Name: orders_shard; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE public.orders_shard (
    id integer,
    title character varying(80),
    price integer
)
PARTITION BY RANGE (price);


ALTER TABLE public.orders_shard OWNER TO postgres;

SET default_table_access_method = heap;

--
-- Name: orders1; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE public.orders1 (
    id integer,
    title character varying(80),
    price integer
);
ALTER TABLE ONLY public.orders_shard ATTACH PARTITION public.orders1 FOR VALUES FROM (0) TO (499);


ALTER TABLE public.orders1 OWNER TO postgres;

--
-- Name: orders2; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE public.orders2 (
    id integer,
    title character varying(80),
    price integer
);
ALTER TABLE ONLY public.orders_shard ATTACH PARTITION public.orders2 FOR VALUES FROM (499) TO (9999999);


ALTER TABLE public.orders2 OWNER TO postgres;

--
-- Name: orders_origin; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE public.orders_origin (
    id integer NOT NULL,
    title character varying(80) NOT NULL,
    price integer DEFAULT 0
);


ALTER TABLE public.orders_origin OWNER TO postgres;

--
-- Name: orders_id_seq; Type: SEQUENCE; Schema: public; Owner: postgres
--

CREATE SEQUENCE public.orders_id_seq
    AS integer
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE public.orders_id_seq OWNER TO postgres;

--
-- Name: orders_id_seq; Type: SEQUENCE OWNED BY; Schema: public; Owner: postgres
--

ALTER SEQUENCE public.orders_id_seq OWNED BY public.orders_origin.id;


--
-- Name: orders_origin id; Type: DEFAULT; Schema: public; Owner: postgres
--

ALTER TABLE ONLY public.orders_origin ALTER COLUMN id SET DEFAULT nextval('public.orders_id_seq'::regclass);


--
-- Data for Name: orders1; Type: TABLE DATA; Schema: public; Owner: postgres
--

COPY public.orders1 (id, title, price) FROM stdin;
1	War and peace	100
3	Adventure psql time	300
4	Server gravity falls	300
5	Log gossips	123
\.


--
-- Data for Name: orders2; Type: TABLE DATA; Schema: public; Owner: postgres
--

COPY public.orders2 (id, title, price) FROM stdin;
2	My little database	500
6	WAL never lies	900
7	Me and my bash-pet	499
8	Dbiezdmin	501
\.


--
-- Data for Name: orders_origin; Type: TABLE DATA; Schema: public; Owner: postgres
--

COPY public.orders_origin (id, title, price) FROM stdin;
1	War and peace	100
2	My little database	500
3	Adventure psql time	300
4	Server gravity falls	300
5	Log gossips	123
6	WAL never lies	900
7	Me and my bash-pet	499
8	Dbiezdmin	501
\.


--
-- Name: orders_id_seq; Type: SEQUENCE SET; Schema: public; Owner: postgres
--

SELECT pg_catalog.setval('public.orders_id_seq', 8, true);


--
-- Name: orders_origin orders_pkey; Type: CONSTRAINT; Schema: public; Owner: postgres
--

ALTER TABLE ONLY public.orders_origin
    ADD CONSTRAINT orders_pkey PRIMARY KEY (id);


--
-- PostgreSQL database dump complete
--
```

Для уникальности можно добавить индекс, заодно и перечитываться будет быстрее:  
CREATE INDEX ON orders ((lower(title)));