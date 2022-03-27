### Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

```
root@anton-v-m:~# docker pull postgres:13.6
root@anton-v-m:~# docker run --name psql13 -d -e POSTGRES_PASSWORD=Netology -v /opt/psql13:/var/lib/postgresql/data postgres:13.6

```
Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

вывода списка БД
```
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)


```
подключения к БД
```
postgres=# \conninfo
You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgresql" at port "5432".
```

вывода списка таблиц
```
postgres-# \dtS+
                                        List of relations
   Schema   |          Name           | Type  |  Owner   | Persistence |    Size    | Description 
------------+-------------------------+-------+----------+-------------+------------+-------------
 pg_catalog | pg_aggregate            | table | postgres | permanent   | 56 kB      | 
 pg_catalog | pg_am                   | table | postgres | permanent   | 40 kB      | 
 pg_catalog | pg_amop                 | table | postgres | permanent   | 80 kB      | 
 pg_catalog | pg_amproc               | table | postgres | permanent   | 64 kB      | 
 pg_catalog | pg_attrdef              | table | postgres | permanent   | 8192 bytes | 
 pg_catalog | pg_attribute            | table | postgres | permanent   | 456 kB     | 
 pg_catalog | pg_auth_members         | table | postgres | permanent   | 40 kB      | 
 pg_catalog | pg_authid               | table | postgres | permanent   | 48 kB      | 
 pg_catalog | pg_cast                 | table | postgres | permanent   | 48 kB      | 
 pg_catalog | pg_class                | table | postgres | permanent   | 136 kB     | 
 pg_catalog | pg_collation            | table | postgres | permanent   | 240 kB     | 
 pg_catalog | pg_constraint           | table | postgres | permanent   | 48 kB      | 
 pg_catalog | pg_conversion           | table | postgres | permanent   | 48 kB      | 
 pg_catalog | pg_database             | table | postgres | permanent   | 48 kB      | 
 pg_catalog | pg_db_role_setting      | table | postgres | permanent   | 8192 bytes | 
 pg_catalog | pg_default_acl          | table | postgres | permanent   | 8192 bytes | 
 pg_catalog | pg_depend               | table | postgres | permanent   | 488 kB     | 
 pg_catalog | pg_description          | table | postgres | permanent   | 376 kB     | 
 pg_catalog | pg_enum                 | table | postgres | permanent   | 0 bytes    | 
 pg_catalog | pg_event_trigger        | table | postgres | permanent   | 8192 bytes | 
...


```
вывода описания содержимого таблиц
```
postgres-# \dS+ pg_amproc
                                    Table "pg_catalog.pg_amproc"
     Column      |   Type   | Collation | Nullable | Default | Storage | Stats target | Description 
-----------------+----------+-----------+----------+---------+---------+--------------+-------------
 oid             | oid      |           | not null |         | plain   |              | 
 amprocfamily    | oid      |           | not null |         | plain   |              | 
 amproclefttype  | oid      |           | not null |         | plain   |              | 
 amprocrighttype | oid      |           | not null |         | plain   |              | 
 amprocnum       | smallint |           | not null |         | plain   |              | 
 amproc          | regproc  |           | not null |         | plain   |              | 
Indexes:
    "pg_amproc_fam_proc_index" UNIQUE, btree (amprocfamily, amproclefttype, amprocrighttype, amprocnum)
    "pg_amproc_oid_index" UNIQUE, btree (oid)
Access method: heap

```
выхода из psql

```
\q                     quit psql
```

### Задача 2  
Используя psql создайте БД test_database.
```
postgres=# create database test_database;
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

```

Изучите бэкап БД.

Восстановите бэкап БД в test_database.
```
root@c9a5b2ef62f1:/# psql -U postgres test_database < /var/lib/postgresql/data/test_dump.sql 
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

```
Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
```
test_database=# ANALYZE VERBOSE orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE

```

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.

значения для `orders`
```
test_database=# select tablename, avg_width from pg_stats where tablename='orders';
 tablename | avg_width 
-----------+-----------
 orders    |         4
 orders    |        16
 orders    |         4
(3 rows)

```

максимальное значение:
```
test_database=# select tablename, max(avg_width) from pg_stats where tablename='orders' group by tablename;
 tablename | max 
-----------+-----
 orders    |  16
(1 row)

```

### Задача 3  
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

транзакция
```
test_database=# begin;
create table orders_1 (check (price>499)) inherits ( orders);
create table orders_2 (check (price <=499)) inherits ( orders);
insert into orders_1 select * from orders where (price>499);
insert into orders_2 select * from orders where (price <=499);
delete from only orders;
create index orders_1_initsale on orders_1 (price);
create index orders_2_initsale on orders_2 (price);
create rule orders_1_insert as on insert to orders where (price>499) do instead insert into orders_1 values (NEW.*);
create rule orders_2_insert as on insert to orders where (price <=499) do instead insert into orders_2 values (NEW.*);
commit;
BEGIN
CREATE TABLE
CREATE TABLE
INSERT 0 3
INSERT 0 5
DELETE 8
CREATE INDEX
CREATE INDEX
CREATE RULE
CREATE RULE
COMMIT

```

таблицы с данными
```
test_database=# select * from orders_1;
 id |       title        | price 
----+--------------------+-------
  2 | My little database |   500
  6 | WAL never lies     |   900
  8 | Dbiezdmin          |   501
(3 rows)

test_database=# select * from orders_2;
 id |        title         | price 
----+----------------------+-------
  1 | War and peace        |   100
  3 | Adventure psql time  |   300
  4 | Server gravity falls |   300
  5 | Log gossips          |   123
  7 | Me and my bash-pet   |   499
(5 rows)

test_database=# select * from only orders;
 id | title | price 
----+-------+-------
(0 rows)

test_database=# select * from orders;
 id |        title         | price 
----+----------------------+-------
  2 | My little database   |   500
  6 | WAL never lies       |   900
  8 | Dbiezdmin            |   501
  1 | War and peace        |   100
  3 | Adventure psql time  |   300
  4 | Server gravity falls |   300
  5 | Log gossips          |   123
  7 | Me and my bash-pet   |   499
(8 rows)

```


Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

```
ответ: да
при создании можно было создать секционированную таблицу
навесить правила или триггеры
или с помощью PARTITION BY
```

### Задача 4
Используя утилиту pg_dump создайте бекап БД test_database.  

```
root@c9a5b2ef62f1:/# pg_dump -U postgres test_database > /var/lib/postgresql/data/t_database.sql

```

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?
```
возможно, его нужно заархивировать
```

