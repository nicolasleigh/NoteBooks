```sql
SELECT prod_id, prod_name, prod_price
FROM Products;
```

> When selecting multiple columns, be sure to specify a comma between each column name, but not after the last column name.

```sql
SELECT DISTINCT vend_id
FROM Products; -- FROM products also correct
```

PostgreSQL column names are case-sensitive (when double-quoted):

```sql
"first_Name"                 -- upper-case "N" preserved
"1st_Name"                   -- leading digit preserved
"AND"                        -- reserved word preserved
```

```sql
first_Name   → first_name    -- upper-case "N" folded to lower-case "n"
1st_Name     → Syntax error! -- leading digit
AND          → Syntax error! -- reserved word
```

The DISTINCT keyword will eliminate those rows where all the selected fields are identical.

```sql
-- Please note the difference
SELECT DISTINCT vend_id, prod_price
FROM Products;

SELECT vend_id, prod_price
FROM Products;
```

Limiting results

```sql
-- Microsoft SQL Server
SELECT TOP 5 prod_name
FROM Products;
```

```sql
-- DB2
SELECT prod_name
FROM Products
FETCH FIRST 5 ROWS ONLY;
```

```sql
-- Oracle
SELECT prod_name
FROM Products
WHERE ROWNUM <=5;
```

```sql
-- MySQL, MariaDB, PostgreSQL, SQLite
SELECT prod_name
FROM Products
LIMIT 5;
```

```sql
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```

> `LIMIT 5 OFFSET 5` instructs supported DBMSs to return five rows starting from row 5. The first number is the number of rows to retrieve, and the second is where to start. The first row retrieved is row 0, not row 1. As such, `LIMIT 1 OFFSET 1` will retrieve the second row, not the first one.
>
> MySQL, MariaDB, and SQLite support a shorthand version of `LIMIT 4 OFFSET 3`, enabling you to combine them as `LIMIT 3,4`. Using this syntax, the value before the , is the OFFSET and the value after the , is the LIMIT (yes, they are reversed, so be careful).
