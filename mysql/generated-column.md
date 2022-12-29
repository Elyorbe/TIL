## Generated column
Generate computed column values based on predefined expressions 

### Syntax
```sql
col_name data_type [GENERATED ALWAYS] AS (expr)
  [VIRTUAL | STORED] [NOT NULL | NULL]
  [UNIQUE [KEY]] [[PRIMARY] KEY]
  [COMMENT 'string']
```

Generated column expression on `CREATE TABLE`

```sql
CREATE TABLE purchase (
    price DOUBLE,
    vat DOUBLE GENERATED ALWAYS AS (price / 10) STORED COMMENT 'Fixed 10% VAT amount'
);
INSERT INTO purchase(price)
VALUES(250),(75),(3254);
```

Selecting `purchase` table produces
```sql
SELECT * FROM purchase;
```
```
+-------+-------+
| price |  vat  |
+-------+-------+
|   250 |    25 |
|    75 |   7.5 |
|  3254 | 325.4 |
+-------+-------+
```

Adding generated column to existing table
```sql
ALTER TABLE purchase
ADD COLUMN total_price DOUBLE
GENERATED ALWAYS AS (price + vat) VIRTUAL
COMMENT 'Price including VAT' AFTER vat;
```
`total_price` is not stored in the database but calculated on read since it is defined as `VIRTUAL`.

Selecting `purchase` table produces
```sql
SELECT * FROM purchase;
```
```
+-------+-------+-------------+
| price |  vat  | total_price |
+-------+-------+-------------+
|   250 |    25 |         275 |
|    75 |   7.5 |        82.5 |
|  3254 | 325.4 |      3579.4 |
+-------+-------+-------------+
```

**Note**: Subqueries are not supported in generated columns. Thus you can't create generated columns based on other tables. 
It should be handled at the application level.
