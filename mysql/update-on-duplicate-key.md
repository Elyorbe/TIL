## INSERT ... ON DUPLICATE KEY UPDATE

If it finds a duplicate unique or primary key, the query will instead perform an UPDATE.

```SQL
INSERT INTO user_info(email, name, created_at)
VALUES ('elyor@elyor.com', 'Elyor',  NOW())
ON DUPLICATE KEY UPDATE updated_at = NOW();
```
