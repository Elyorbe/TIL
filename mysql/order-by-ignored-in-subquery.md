## ORDER BY in a subquery is ignored
Today I've had a fairly simple but very interesting problem that took me around an hour to solve.
I was just trying to change the ordering of list response in my Spring Boot + MyBatis application.
Below query was working without any problems on my local machine. That's ordering was as I expected: ` ORDER BY sales.created_at DESC`.

```sql
SELECT A.*, @NO AS TOTAL_COUNT
FROM (SELECT @NO := @NO + 1 AS ROWNUM, DATA.*
      FROM (SELECT sales.id,
                   sales.product_id,
                   product.product_name,
                   sales.company_id,
                   company.company_name,
                   sales.unit_type,
                   sales.quantity,
                   sales.unit_price,
                   sales.price,
                   sales.status,
                   sales.created_at
            FROM sales
                     LEFT JOIN product ON sales.product_id = product.id
                     LEFT JOIN company ON company.id = sales.company_id
            WHERE sales.status != 2
            ORDER BY sales.created_at DESC) DATA, -- This line
           (SELECT @NO := 0) TEMP) A;
```
However when I deployed to the production server ordering was being ignored. I checked the logs, tried with debug mode and used query from the logs.
Queries are the same. No succes. Then I realized production database was MariaDB but I was testing it against MySQL on my localhost. 
Then came across with this article [Why is ORDER BY in a FROM Subquery Ignored?](https://mariadb.com/kb/en/why-is-order-by-in-a-from-subquery-ignored/).
Bacially, the problem was with inner subquery ordering. It was being ignored on production server. Moving `ORDER BY` to the outer query fixed the problem.

```sql
SELECT A.*, @NO AS TOTAL_COUNT
FROM (SELECT @NO := @NO + 1 AS ROWNUM, DATA.*
      FROM (SELECT sales.id,
                   sales.product_id,
                   product.product_name,
                   sales.company_id,
                   company.company_name,
                   sales.unit_type,
                   sales.quantity,
                   sales.unit_price,
                   sales.price,
                   sales.status,
                   sales.created_at
            FROM sales
                     LEFT JOIN product ON sales.product_id = product.id
                     LEFT JOIN company ON company.id = sales.company_id
            WHERE sales.status != 2) DATA, 
           (SELECT @NO := 0) TEMP) A ORDER BYcreated_at DESC; -- This line
           
