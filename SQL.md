## SQL Cheat Sheet

Ref:

- [SQL Cheat Sheet](https://www.sqltutorial.org/sql-cheat-sheet/)

- 

### 检索 Query

```sql
# Query 1
SELECT [DISTINCT] t1.column1 [AS abbr_col1], aggregate(column2), 
    CASE
        WHEN condition_expr1 THEN result_expr1 [ELSE else_expr1]
        [WHEN condition_expr2 THEN result_expr2 ELSE else_expr2]
    END [AS abbr_col3]
FROM table1 AS t1, table2, an_query(SELECT ...)
WHERE condition_expr1 {AND | OR} col IS [NOT] NULL {AND | OR} expr3 [NOT] IN (val1, val2, val3-val4)
HAVING condition_expr_with_aggregate1 [{AND | OR} ...]
GROUP BY col_name[, ...]
ORDER BY col_name[, ...]
{UNION [ALL] | INTERSECT | MINUS}
# Query 2
another query (SELECT ...)
...
LIMIT line_num
```

Notes:

- 若 column1 在 table1 和 table2 中都出现过，为避免二义性，使用时需要使用「完全限定名」，即 table1.column1 或 table2.column1

- `HAVING` clause allows using aggregate funtion to columns while `WHERE` not.

- 创建联结的三种方式：
  
  1. 在 `FROM` 后指定多个表，默认是笛卡尔积（叉联结），可以用 `WHERE` 指定过滤条件，等价于 `SELECT ... FROM table1 CROSS JOIN table2`
  
  2. 内部链接（等值连接）：`SELECT ... FROM table1 INNER JOIN table2 ON condition_expr`
  
  3. 外部链接（包含没有匹配行的那些行）：`SELECT ... FROM table1 {RIGHT | LEFT | FULL} OUTER JOIN table2 ON condition_expr`

### 管理表 Managing Tables

```sql
CREATE TABLE t(
    c1 type [PRIMARY KEY] [[NOT] NULL] [DEFAULT default_value],
    c2 ...,
    PROMARY KEY (c1[, ...]) # 指定一个或多个主键
    FOREIGN KEY (c1) REFERENCES  t2(c2) # 指定并绑定外键
);

DROP TABLE t_name;

# ALTER TABLE t_name ADD
```



### 修改数据 Modifying Data

```sql

```



## SQLite

```shell
#### get help ####
sqlite> .help

#### open database ####
$ sqlite3 database_name.db # way 1

$ sqlite3 # enter sqlite
sqlite> .open database_name.db # way 2

#### exit database ####
sqlite> .exit

#### run sql files ####
$ sqlite3 database_name.db < sql_file_name.sql # way 1
$ sqlite3 example.db 'SELECT * FROM some_table'; # run some sample sql queries
sqlite> .read sql_file_name.sql # way 2

#### show information ####
sqlite> .database # show database
sqlite> .tables ?TABLE? # List names of tables matching LIKE pattern TABLE
sqlite> .schema tablename # To show the schema for a given tablename

#### useful tools ####
sqlite> .save FILE # Write in-memory database into FILE if you haven't open one yet
```

## MySQL CLI

```shell
#### get help ####
mysql> help # or \h

#### connect to MySLQ ####
$ mysql [--host=localhost] [--port=3306] --user=myname --password [database_name]
$ mysql [-h localhost] [-P 3306] -u myname -p [database_name]

#### open database ####
mysql> USE database_name

#### exit database ####
mysql> exit

#### run sql files ####
$ mysql -h hostname -u user database_name< sql_file_path.sql # way 1
mysql> source sql_file_path.sql; # way 2

#### show information ####
mysql> SHOW DATABASES; # show database
mysql> SHOW TABLES; # List names of tables in a schema
mysql> SHOW COLUMNS FROM table [LIKE 'pattern' | WHERE expr]; # List names of columns in a table

#### useful tools ####
```

## MySQL Shell

```shell
#### get help ####
> \help
> \? sql_cmd # get help for a sql command, like '\? SELECT'
> \? \mysql_shell_cmd # like '\? \connect'

#### connect to MySLQ ####
$ mysqlsh # enter MySQL Shell
> \connect [user[:password]@]hostname[:port]
# e.g. '> \connect lee@localhost' with default port is 3306

#### open database ####
> USE database_name.db

#### exit database ####
> \exit # or \quit
> \disconnect # Disconnects the global session.

#### run sql files ####
> source sql_file_path.sql

#### show information ####
> SHOW DATABASES; # show database
> SHOW TABLES; # List names of tables in a schema
> SHOW COLUMNS FROM table [LIKE 'pattern' | WHERE expr]; # List names of columns in a table

#### useful tools ####
```
