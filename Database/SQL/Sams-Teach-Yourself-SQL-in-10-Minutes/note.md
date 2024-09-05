## Retrieving Data

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

## Sorting Retrieved Data

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

## Filtering Data

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

> When using both `ORDER BY` and `WHERE` clauses, make sure that `ORDER BY` comes after the `WHERE`. Otherwise, an error will be generated.

```sql
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';	 -- or !=
```

> <> is the same as !=. !< (not less than) is the same as >= (greater than or equal to).

```sql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```

> As seen in this example, when `BETWEEN` is used, two values must be specified—the low end and high end of the desired range. The two values must also be separated by the `AND` keyword. `BETWEEN` matches all the values in the range, including the specified start and end values.

To determine if a value is `NULL`, you cannot simply check to see if = NULL. Instead, the `SELECT` statement has a special `WHERE` clause that you can use to check for columns with `NULL` values—the `IS NULL` clause.

```sql
SELECT cust_name
FROM Customers
WHERE cust_email IS NULL;
```

## Advanced Data Filtering

```sql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4; -- AND operator

SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'; -- OR operator
```

> In fact, most of the better DBMSs will not even evaluate the second condition in an `OR WHERE` clause if the first condition has already been met. (If the first condition was met, the row would be retrieved regardless of the second condition.)

```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01' AND prod_price >= 10;
-- WHERE clause actually is:
-- WHERE vend_id = 'DLL01' OR (vend_id = 'BRS01' AND prod_price >= 10);
```

> `SQL` (like most languages) processes `AND` operators before `OR` operators.

```sql
SELECT prod_name, prod_price
FROM Products
WHERE (vend_id = 'DLL01' OR vend_id = 'BRS01') AND prod_price >= 10;
```

> Whenever you write `WHERE` clauses that use both `AND` and `OR` operators, use parentheses to explicitly group operators. Don’t ever rely on the default evaluation order, even if it is exactly what you want.

The `IN` operator is used to specify a range of conditions, any of which can be matched. `IN` takes a comma-delimited list of valid values, all enclosed within parentheses. The following example demonstrates this:

```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01','BRS01')
ORDER BY prod_name;
```

If you are thinking that the `IN` operator accomplishes the same goal as `OR`, you are right. The following SQL statement accomplishes the exact same thing as the example above:

```sql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01'
ORDER BY prod_name;
```

> `IN` operators almost always execute more quickly than lists of `OR` operators. The biggest advantage of `IN` is that the `IN` operator can contain another `SELECT` statement, enabling you to build highly dynamic `WHERE` clauses, that is Subqueries.

The `WHERE` clause’s `NOT` operator has one function and one function only: `NOT` negates whatever condition comes next.

```sql
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
ORDER BY prod_name;
```

The preceding example also could have been accomplished using the `<>` operator, as follows:

```sql
SELECT prod_name
FROM Products
WHERE vend_id  <> 'DLL01'
ORDER BY prod_name;
```

> Why use `NOT`? Well, for simple `WHERE` clauses such as the ones shown here, there really is no advantage to using `NOT`. `NOT` is useful in more complex clauses. For example, using `NOT` in conjunction with an `IN` operator makes it simple to find all rows that do not match a list of criteria.

## Using Wildcard Filtering

Wildcard searching can only be used with text fields (strings); you can’t use wildcards to search fields of nontext datatypes.

The most frequently used wildcard is the percent sign (%). Within a search string, % means match any number of occurrences of any character. For example, to find all products that start with the word Fish, you can issue the following SELECT statement:

```sql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE 'Fish%'; -- case sensitive, 'Fish%' not equal to 'fish%'
```

```sql
SELECT prod_id, prod_name
FROM Products
WHERE prod_name LIKE '%bean bag%';
```

> The search pattern `'%bean bag%'` means match any value that contains the text `bean bag` anywhere within it, regardless of any characters before or after that text.

There is one situation in which wildcards may indeed be useful in the middle of a search pattern, and that is looking for email addresses based on a partial address, such as `WHERE email LIKE 'b%@gmail.com'`.

It is important to note that, in addition to matching one or more characters, `%` also matches zero characters. `%` represents zero, one, or more characters at the specified location in the search pattern.

Some DBMSs pad field contents with spaces. For example, if a column expects 50 characters and the text stored is 'Fish bean bag toy' (17 characters), 33 spaces may be appended to the text so as to fully fill the column. This padding usually has no real impact on data and how it is used, but it could negatively affect the just-used SQL statement. The clause `WHERE prod_name LIKE 'F%y'` will only match prod_name if it starts with F and ends with y, and if the value is padded with spaces, then it will not end with y and so 'Fish bean bag toy' will not be retrieved. One simple solution to this problem is to append a second % to the search pattern. 'F%y%' will also match characters (or spaces) after the y. A better solution would be to trim the spaces using functions.

Another useful wildcard is the underscore (\_). The underscore is used just like %, but instead of matching multiple characters, the underscore matches just a single character.

```sql
SELECT prod_id, prod_name
FROM Products
WHERE TRIM(prod_name) LIKE '__ inch teddy bear'; -- Without TRIM, PostgreSQL will find nothing
```

PostgreSQL doesn't support the 'brackets ([]) wildcard', so the following code will return nothing:

```sql
SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%' -- PostgreSQL doesn't support
ORDER BY cust_contact;
```

As you can see, SQL’s wildcards are extremely powerful. But that power comes with a price: wildcard searches typically take far longer to process than any other search types discussed previously. Here are some rules to keep in mind when using wildcards:

- Don’t overuse wildcards. If another search operator will do, use it instead.
- When you do use wildcards, try not to use them at the beginning of the search pattern unless absolutely necessary. Search patterns that begin with wildcards are the slowest to process.
- Pay careful attention to the placement of the wildcard symbols. If they are misplaced, you might not return the data you intended.

## Creating Calculated Fields

Field essentially means the same thing as column and often used interchangeably, although database columns are typically called columns and the term fields is usually used in conjunction with calculated fields.

SQL Server uses `+` for concatenation. DB2, Oracle, PostgreSQL, and SQLite support `||`.

```sql
SELECT vend_name || ' (' || vend_country || ')'
FROM Vendors
ORDER BY vend_name;
```

```sql
SELECT RTRIM(vend_name) || ' (' || RTRIM(vend_country) || ')'  -- In PostgreSQL, I don't see any differences from the above code
FROM Vendors
ORDER BY vend_name;
```

> The `RTRIM()` function trims all space from the right of a value. When you use `RTRIM()`, the individual columns are all trimmed properly.
>
> Most DBMSs support `RTRIM()` (which, as just seen, trims the right side of a string), as well as `LTRIM()`, which trims the left side of a string, and `TRIM()`, which trims both the right and left.

```sql
SELECT RTRIM(vend_name) || ' (' || RTRIM(vend_country) || ')'  AS vend_title -- Using aliases
FROM Vendors
ORDER BY vend_name;
```

> Aliases are assigned with the AS keyword.
>
> Aliases have other uses too. Some common uses include renaming a column if the real table column name contains illegal characters (for example, spaces) and expanding column names if the original names are either ambiguous or easily misread.
>
> Aliases may be single words or complete strings. If the latter is used, then the string should be enclosed within quotes. This practice is legal but is strongly discouraged. While multiword names are indeed highly readable, they create all sorts of problems for many client applications—so much so that one of the most common uses of aliases is to rename multiword column names to single-word names.
>
> Aliases are also sometimes referred to as derived columns, so regardless of the term you run across, they mean the same thing.

```sql
SELECT prod_id, quantity, item_price, quantity*item_price AS expanded_price -- Performing mathematical calculations
FROM OrderItems
WHERE order_num = 20008;
```

## Using Data Manipulation Functions

Functions tend to be very DBMS specific. In fact, very few functions are supported identically by all major DBMSs.

|                          |                                                                                                                                                             |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Extract part of a string | DB2, Oracle, PostgreSQL, and SQLite use `SUBSTR()`. MariaDB, MySQL, and SQL Server use `SUBSTRING()`.                                                       |
| Datatype conversion      | Oracle uses multiple functions, one for each conversion type. DB2, PostgreSQL, and SQL Server use `CAST()`. MariaDB, MySQL, and SQL Server use `CONVERT()`. |
| Get current date         | DB2 and PostgreSQL use `CURRENT_DATE`. MariaDB and MySQL use `CURDATE()`. Oracle uses `SYSDATE`. SQL Server uses `GETDATE()`. SQLite uses `DATE()`.         |

```sql
SELECT vend_name, UPPER(vend_name) AS vend_name_upcase -- Upper case
FROM Vendors
ORDER BY vend_name;
```

> As should be clear by now, SQL functions are not case sensitive, so you can use upper(), UPPER(), Upper(), or substr(), SUBSTR(), SubStr(), and so on.

```sql
SELECT order_num
FROM Orders
WHERE DATE_PART('year', order_date) = 2020;

-- Same as above
SELECT order_num
FROM Orders
WHERE EXTRACT(year FROM order_date) = 2020;

-- Same as above
SELECT order_num
FROM Orders
WHERE order_date BETWEEN TO_DATE('2020-01-01', 'yyyy-mm-dd')  AND TO_DATE('2020-12-31', 'yyyy-mm-dd');
```

## Summarizing Data

It is often necessary to summarize data without actually retrieving it all, and SQL provides special functions for this purpose. Using these functions, SQL queries are often used to retrieve data for analysis and reporting purposes. Examples of this type of retrieval are:

- Determining the number of rows in a table (or the number of rows that meet some condition or contain a specific value).

- Obtaining the sum of a set of rows in a table.

- Finding the highest, lowest, and average values in a table column (either for all rows or for specific rows).

Aggregate functions are functions that operate on a set of rows to calculate and return a single value.

```sql
SELECT AVG(prod_price) AS avg_price -- AVG()
FROM Products
WHERE vend_id = 'DLL01';
```

```sql
SELECT COUNT(*) AS num_cust -- COUNT()
FROM Customers;

SELECT COUNT(cust_email) AS num_cust
FROM Customers;
```

> Use `COUNT(*)` to count the number of rows in a table, whether columns contain values or NULL values.
>
> Use `COUNT(column)` to count the number of rows that have values in a specific column, ignoring NULL values.

```sql
SELECT MAX(prod_price) AS max_price -- MAX()
FROM Products;
```

```sql
SELECT MIN(prod_price) AS min_price -- MIN()
FROM Products;
```

```sql
SELECT SUM(item_price*quantity) AS total_price -- SUM()
FROM OrderItems
WHERE order_num = 20005;
```

The five aggregate functions can all be used in two ways:

- To perform calculations on all rows, specify the ALL argument or specify no argument at all (because ALL is the default behavior).
- To include only unique values, specify the DISTINCT argument.

The ALL argument need not be specified because it is the default behavior. If DISTINCT is not specified, ALL is assumed.

```sql
SELECT AVG(DISTINCT prod_price) AS avg_price -- AVG() with DISTINCT
FROM Products
WHERE vend_id = 'DLL01';
```

`DISTINCT` may only be used with `COUNT()` if a column name is specified. `DISTINCT` may not be used with `COUNT(*)`. Similarly, `DISTINCT` must be used with a column name and not with a calculation or expression.

```sql
SELECT COUNT(*) AS num_items,
      MIN(prod_price) AS price_min,
      MAX(prod_price) AS price_max,
      AVG(prod_price) AS price_avg
FROM Products;
```

## Grouping Data

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
GROUP BY vend_id; -- Same as GROUP BY 1;
```

The `GROUP BY` clause instructs the DBMS to group the data and then perform the aggregate on each group rather than on the entire result set.

The `GROUP BY` clause must come after any `WHERE` clause and before any `ORDER BY` clause.

Some SQL implementations allow you to specify `GROUP BY` columns by the position in the `SELECT` list. For example, `GROUP BY 2,1` can mean group by the second column selected and then by the first.

In addition to being able to group data using `GROUP BY`, SQL also allows you to filter which groups to include and which to exclude. For example, you might want a list of all customers who have made at least two orders. To obtain this data, you must filter based on the complete group, not on individual rows.

You’ve already seen the `WHERE` clause in action. But `WHERE` does not work here because `WHERE` filters specific rows, not groups. As a matter of fact, `WHERE` has no idea what a group is.

So what do you use instead of `WHERE`? SQL provides yet another clause for this purpose: the `HAVING` clause. `HAVING` is very similar to `WHERE`. In fact, all types of `WHERE` clauses you’ve learned about thus far can also be used with `HAVING`. The only difference is that `WHERE` filters rows and `HAVING` filters groups.

```sql
SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;

SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
HAVING orders >= 2; -- ERROR!

SELECT cust_id, COUNT(*) AS orders
FROM Orders
GROUP BY cust_id
WHERE COUNT(*) >= 2; -- ERROR!
```

> As you can see, a `WHERE` clause couldn’t work here because the filtering is based on the group aggregate value, not on the values of specific rows.
>
> Here’s another way to look at it: `WHERE` filters before data is grouped, and `HAVING` filters after data is grouped. This is an important distinction; rows that are eliminated by a `WHERE` clause will not be included in the group.

```sql
SELECT vend_id, COUNT(*) AS num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```

> `HAVING` is so similar to `WHERE` that most DBMSs treat them as the same thing if no `GROUP BY` is specified. Nevertheless, you should make that distinction yourself. Use `HAVING` only in conjunction with `GROUP BY` clauses. Use `WHERE` for standard row-level filtering.

As a rule, anytime you use a `GROUP BY` clause, you should also specify an `ORDER BY` clause. That is the only way to ensure that data will be sorted properly. Never rely on `GROUP BY` to sort your data.

```sql
SELECT order_num, COUNT(*) AS items
FROM OrderItems
GROUP BY order_num
HAVING COUNT(*) >= 3
ORDER BY items, order_num;
```

## Working with Subqueries

Now suppose you wanted a list of all the customers who ordered item RGAN01. What would you have to do to retrieve this information? Here are the steps:

1. Retrieve the order numbers of all orders containing item RGAN01.
2. Retrieve the customer ID of all the customers who have orders listed in the order numbers returned in the previous step.
3. Retrieve the customer information for all the customer IDs returned in the previous step.

```sql
-- Steps 1
SELECT order_num
FROM OrderItems
WHERE prod_id = 'RGAN01';

/* order_num
----------
20007
20008 */

-- Steps 2
SELECT cust_id
FROM Orders
WHERE order_num IN (20007,20008); -- the type of order_num is INT

/* cust_id
---------
1000000004
1000000005 */

-- Steps 1 && 2
SELECT cust_id
FROM Orders
WHERE order_num IN (SELECT order_num
                    FROM OrderItems
                    WHERE prod_id = 'RGAN01');

-- Steps 3
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id IN ('1000000004','1000000005'); -- Add single quote because the type of cust_id is CHAR(10)
```

```sql
-- Steps 1 && 2 && 3
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id IN (SELECT cust_id
                  FROM Orders
                  WHERE order_num IN (SELECT order_num
                                      FROM OrderItems
                                      WHERE prod_id = 'RGAN01'));
```

> Subquery `SELECT` statements can only retrieve a single column. Attempting to retrieve multiple columns will return an error.

Now suppose you wanted to display the total number of orders placed by every customer in your Customers table. To perform this operation, follow these steps:

1. Retrieve the list of customers from the Customers table.

2. For each customer retrieved, count the number of associated orders in the Orders table.

```sql
SELECT cust_name,
      cust_state,
      (SELECT COUNT(*)
       FROM Orders
       WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers
ORDER BY cust_name;
```

> That subquery is executed once for every customer retrieved. In the example above, the subquery is executed five times because five customers were retrieved.

## Joining Tables

Creating a join is very simple. You must specify all the tables to be included and how they are related to each other. Look at the following example:

```sql
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products 
WHERE Vendors.vend_id = Products.vend_id;
```

> The big difference here from the previous code is that two of the specified columns (prod_name and prod_price) are in one table, whereas the other (vend_name) is in another table.

When you join two tables, what you are actually doing is pairing every row in the first table with every row in the second table. The `WHERE` clause acts as a filter to only include rows that match the specified filter condition—the join condition, in this case. Without the `WHERE` clause, every row in the first table will be paired with every row in the second table, regardless of whether they logically go together or not.

The results returned by a table relationship without a join condition. The number of rows retrieved will be the number of rows in the first table multiplied by the number of rows in the second table, that is the `Cartesian Product`.

Make sure all your joins have `WHERE` clauses; otherwise, the DBMS will return far more data than you want. Similarly, make sure your `WHERE` clauses are correct. An incorrect filter condition will cause the DBMS to return incorrect data.

Sometimes you’ll hear the type of join that returns a `Cartesian Product` referred to as a `cross join`.

The join you have been using so far is called an `equijoin`—a join based on the testing of equality between two tables. This kind of join is also called an `inner join`. In fact, you may use a slightly different syntax for these joins, specifying the type of join explicitly. The following SELECT statement returns the exact same data as an earlier example:

```sql
SELECT vend_name, prod_name, prod_price 
FROM Vendors 
INNER JOIN Products ON Vendors.vend_id = Products.vend_id;
```

> The SELECT in the statement is the same as the preceding SELECT statement, but the FROM clause is different. Here the relationship between the two tables is part of the FROM clause specified as INNER JOIN. In this syntax, the join condition is specified using the special ON clause instead of a WHERE clause. The actual condition passed to ON is the same as would be passed to WHERE.
>
> Per the ANSI SQL specification, use of the INNER JOIN syntax is preferred over the simple equijoins syntax used previously. Indeed, SQL purists tend to look upon the simple syntax with disdain. That being said, DBMSs do indeed support both the simpler and the standard formats, so my recommendation is that you take the time to understand both formats but use whichever you feel more comfortable with.

```sql
-- Joining multiple tables
SELECT order_num, prod_name, vend_name, prod_price, quantity 
FROM OrderItems, Products, Vendors 
WHERE Products.vend_id = Vendors.vend_id AND OrderItems.prod_id = Products.prod_id AND order_num = 20007;
```

```sql
-- Previous code
SELECT cust_name, cust_contact
FROM Customers
WHERE cust_id IN (SELECT cust_id
                  FROM Orders
                  WHERE order_num IN (SELECT order_num
                                      FROM OrderItems
                                      WHERE prod_id = 'RGAN01'));
                                      
-- Same query using join
SELECT cust_name, cust_contact 
FROM Customers, Orders, OrderItems 
WHERE Customers.cust_id = Orders.cust_id AND OrderItems.order_num = Orders.order_num AND prod_id = 'RGAN01';
```

