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

SQL statements are made up of clauses, some required and some optional. A clause usually consists of a keyword and supplied data. An example of this is the SELECT statement’s FROM clause

```sql
SELECT prod_name 
FROM Products 
ORDER BY prod_name;
```

> When specifying an `ORDER BY` clause, be sure that it is the last clause in your `SELECT` statement. If it is not the last clause, an error will be generated.
>
> Although more often than not the columns used in an `ORDER BY` clause will be ones selected for display, this is actually not required. It is perfectly legal to sort data by a column that is not retrieved.

In addition to being able to specify sort order using column names, `ORDER BY` also supports ordering specified by relative column position:

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price, prod_name;

-- Another way
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY 2, 3;
```

> `ORDER BY 2` means sort by the second column in the `SELECT` list, the `prod_price` column. `ORDER BY 2, 3` means sort by `prod_price` and then by `prod_name`.
>
> The primary advantage of this technique is that it saves retyping the column names. But there are some downsides too. First, not explicitly listing column names increases the likelihood of you mistakenly specifying the wrong column. Second, it is all too easy to mistakenly reorder data when making changes to the `SELECT` list (forgetting to make the corresponding changes to the `ORDER BY` clause). And finally, obviously you cannot use this technique when sorting by columns that are not in the `SELECT` list.
>
> This technique cannot be used when sorting by columns that do not appear in the `SELECT` list. However, you can mix and match actual column names and relative column positions in a single statement if needed.

```sql
SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price DESC; -- sort prod_price in descending order

SELECT prod_id, prod_price, prod_name 
FROM Products 
ORDER BY prod_price DESC, prod_name; -- sort prod_price in descending order and prod_name in ascending order
```

> If you want to sort descending on multiple columns, be sure each column has its own `DESC` keyword.
>
> In practice, however, `ASC` is not usually used because ascending order is the default sequence (and is assumed if neither `ASC` nor `DESC` is specified).

In dictionary sort order, A is treated the same as a, and that is the default behavior for most `DBMSs`. However, most good `DBMSs` enable database administrators to change this behavior if needed. (If your database contains lots of foreign language characters, this might become necessary.) The key here is that, if you do need an alternate sort order, you may not be able to accomplish this with a simple `ORDER BY` clause. You may need to contact your database administrator.











