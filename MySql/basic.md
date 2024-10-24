# Complete MySQL Guide 2024

## Table of Contents
- [Database Operations](#database-operations)
- [Table Operations](#table-operations)
- [Data Operations](#data-operations)
- [Advanced Features](#advanced-features)
  - [Indexing](#indexing)
  - [Joins](#joins)
  - [Transactions](#transactions)
  - [Views](#views)
- [Performance & Security](#performance--security)
  - [Best Practices](#best-practices)
  - [User Management](#user-management)

## Database Operations

Create database:
```sql
CREATE DATABASE dbname;
```

List databases:
```sql
SHOW DATABASES;
```

Select database:
```sql
USE dbname;
```

Delete database:
```sql
DROP DATABASE dbname;
```

## Table Operations

Create table:
```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

Show table structure:
```sql
DESCRIBE users;
```

Modify table:
```sql
ALTER TABLE users ADD COLUMN age INT;
ALTER TABLE users MODIFY COLUMN age TINYINT;
ALTER TABLE users DROP COLUMN age;
```

Delete table:
```sql
DROP TABLE users;
```

## Data Operations

Insert data:
```sql
INSERT INTO users (username, email) 
VALUES ('john', 'john@email.com');
```

Select data:
```sql
SELECT * FROM users;
SELECT username, email FROM users WHERE id = 1;
```

Update data:
```sql
UPDATE users 
SET email = 'new@email.com' 
WHERE id = 1;
```

Delete data:
```sql
DELETE FROM users WHERE id = 1;
```

## Advanced Features

### Indexing

Create index:
```sql
CREATE INDEX idx_username ON users(username);
```

Show indexes:
```sql
SHOW INDEX FROM users;
```

### Joins

Inner join:
```sql
SELECT orders.id, users.username 
FROM orders 
INNER JOIN users ON orders.user_id = users.id;
```

Left join:
```sql
SELECT users.username, orders.id 
FROM users 
LEFT JOIN orders ON users.id = orders.user_id;
```

### Transactions

```sql
START TRANSACTION;
-- Your SQL statements here
COMMIT;
-- or ROLLBACK;
```

### Views

```sql
CREATE VIEW active_users AS 
SELECT * FROM users 
WHERE last_login > DATE_SUB(NOW(), INTERVAL 7 DAY);
```

## Performance & Security

### Best Practices

1. Use prepared statements to prevent SQL injection
2. Create appropriate indexes for frequent queries
3. Regular backups using `mysqldump`
4. Set proper user permissions
5. Use connection pooling for applications

### User Management

Create user:
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
```

Grant privileges:
```sql
GRANT SELECT, INSERT ON dbname.* TO 'username'@'localhost';
```

Show user privileges:
```sql
SHOW GRANTS FOR 'username'@'localhost';
```

---
Last updated: October 2024
