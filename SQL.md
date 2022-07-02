## SQL Cheat Sheet

Ref:

- [SQL Cheat Sheet](https://www.sqltutorial.org/sql-cheat-sheet/)

- 

### 检索 Query

```sql
# Query 1
WITH [RECURSIVE] cte_name [(col1, col2)] AS (
    ... some SQL quries
)
SELECT [DISTINCT] t1.column1 [AS abbr_col1], aggregation_func(col_name), window_func() OVER (
    PARTITION BY col_name_for_group
    ORDER BY col_name_for_rank [DESC]
    ) [AS abbr_col_name]
    CASE
        WHEN condition_expr1 THEN result_expr1
        WHEN condition_expr2 THEN result_expr2
        ELSE else_expr2
    END [AS abbr_col3]
FROM table1 AS t1, table2, an_query(SELECT ...)
WHERE condition_expr1 {AND | OR} col IS [NOT] NULL {AND | OR} expr3 [NOT] IN (val1, val2, val3-val4)
    {AND | OR} str_expr1 LIKE '%str_pattern_' {AND | OR} str_expr2 REGEXP 'str_regexp'
GROUP BY col_name[, ...]
HAVING condition_expr_with_aggregate1 [{AND | OR} ...]
ORDER BY col_name [DESC][, ...]
{UNION [ALL] | INTERSECT | MINUS}
# Query 2
another query (SELECT ...)
...
LIMIT line_num [OFFSET offset_num]
```

Notes:

- 公用表表达式 Common Table Expressions：可以用 `WITH AS` 建立一个临时的表，只对后面紧接的单条SQL语句生效。使用 `RECURSIVE` 可以得到递归的结果，如
  
  ```sql
  ### 打印 1 到 10
  WITH RECURSIVE cteSource (counter) AS (
      ( SELECT 1 ) UNION ( SELECT counter + 1 FROM cteSource WHERE counter < 10 )
  )
  SELECT * FROM cteSource;
  ```

- 若 column1 在 table1 和 table2 中都出现过，为避免二义性，使用时需要使用「完全限定名」，即 table1.column1 或 table2.column1

- 函数：分为聚合函数 Aggregation functions 和窗口函数 Windows functions
  
  - 同：都是对 `WHERE` 或者 `GROUP BY` 子句处理后的结果进行操作；异：普通的聚合函数只能用来计算一行内的结果或把所有行聚合成一行结果，而窗口函数支持为每一行生成一个结果。
  
  - 聚合函数，如 `COUNT`, `AVG`, `MAX`, `MIN`, `SUM`
    
    - `COUNT`, `AVG`, `SUM` 支持 `DISTINCT`，如 `COUNT(DISTINCT col_name)`
  
  - 窗口函数：特征式带有 `OVER`，分为聚合函数作为窗口函数和专用窗口函数
    
    - 专用窗口函数，也称为 OLAP(OnLine Analytical Processing) 函数，包括排名函数（如 `RANK`, `ROW_NUM`, `DENSE_RANK`）和偏移函数
      
      - 用法：排名窗口函数括号内一般为空，偏移函数括号内一般有参数，
      - `PARTITION BY` 子句用于划分窗口分区，如果没有指定 `PARTITION BY` 子句，则整个查询与分析结果集作为一个窗口分区。
      - `ORDER BY` 子句用于对窗口分区内的行进行排序
    
    - 聚合函数作为窗口函数
      
      - 用法：函数后面括号里面不能为空，需要指定聚合的列名。如 `select sum(num) over as current_sum from table_name`
      
      - 意义：截止到本行数据的统计数据，即第 `i` 行的计算只包含 `1~i` 行的数据。

- `GROUP BY` 后面的列应该包括 `SELECT` 子句后面所有的非聚合函数的列

- `HAVING` 子句允许使用「聚合函数」或「聚合函数的结果列名」作为过滤条件，`WHERE` 子句只能使用已有的列名，且不能包含聚合函数。

- 创建联结的三种方式：
  
  1. 在 `FROM` 后指定多个表，默认是笛卡尔积（叉联结），可以用 `WHERE` 指定过滤条件，等价于 `SELECT ... FROM table1 CROSS JOIN table2`
  
  2. 内部链接（等值连接）：`SELECT ... FROM table1 INNER JOIN table2 ON condition_expr`
  
  3. 外部链接（包含没有匹配行的那些行）：`SELECT ... FROM table1 {RIGHT | LEFT | FULL} OUTER JOIN table2 ON condition_expr`

- 字符串操作：
  
  - 字符串匹配
    
    - `LIKE`：'%' 匹配任意子字符串（包含空）、'_' 匹配任一字符
    
    - `REGEXP`：匹配正则表达式
  
  - 字符串拼接：'||' 可以将两个字符串拼接为一个字符串

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
