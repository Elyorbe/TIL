## Stored Function

`Stored functions` are useful when we want to use common formulas or business rules.
Different from `stored procedures`, stored function can be used in SQL statements wherever an expression is used.
In other words, `stored functions` are invoked within an expression and returns a single value directly to the caller to be used in the expression.

MySQL CREATE FUNCTION syntax:

```sql
DELIMITER $$

CREATE FUNCTION function_name( param1,
    param2,…
)
RETURNS datatype
[NOT] DETERMINISTIC   -- always returns the same result for the same input parameters
BEGIN
    -- statements
    END $$

DELIMITER ;
```

Sample query:

```sql
DROP FUNCTION IF EXISTS permission_type_id_by_name;

DELIMITER $$

CREATE FUNCTION permission_type_id_by_name(
    name varchar(32)
)
    RETURNS BIGINT
    DETERMINISTIC
BEGIN
    DECLARE permission_type_id BIGINT DEFAULT 0;
    SELECT id
    INTO permission_type_id
    FROM permission_type
    WHERE permission_type.name = name;
    RETURN (permission_type_id);
END $$

DELIMITER ;

```
