# COUNT(*) OVER()

Often we need the total count of the resultset  besides the rows data itself on a separate column.
One way is to create a subquery and count the results. But the better way is using `COUNT()` window function.

```sql
SELECT id, name, COUNT(*) over () AS total FROM products; 
```
