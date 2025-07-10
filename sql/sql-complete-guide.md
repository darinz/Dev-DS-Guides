# SQL Complete Reference Guide

A comprehensive guide to Structured Query Language (SQL), covering database fundamentals, query operations, optimization techniques, and best practices for data management and analysis.

## Table of Contents

1. [Introduction](#introduction)
2. [Database Fundamentals](#database-fundamentals)
3. [Data Definition Language (DDL)](#data-definition-language-ddl)
4. [Data Manipulation Language (DML)](#data-manipulation-language-dml)
5. [Data Query Language (DQL)](#data-query-language-dql)
6. [Data Control Language (DCL)](#data-control-language-dcl)
7. [Advanced Queries](#advanced-queries)
8. [Joins and Relationships](#joins-and-relationships)
9. [Subqueries and CTEs](#subqueries-and-ctes)
10. [Aggregation and Grouping](#aggregation-and-grouping)
11. [Window Functions](#window-functions)
12. [Indexing and Optimization](#indexing-and-optimization)
13. [Transactions and ACID](#transactions-and-acid)
14. [Database Design](#database-design)
15. [Common Database Systems](#common-database-systems)
16. [Best Practices](#best-practices)
17. [Troubleshooting](#troubleshooting)

## Introduction

SQL (Structured Query Language) is a standard language for managing and manipulating relational databases. It provides a powerful and flexible way to store, retrieve, update, and delete data.

### Key Concepts

- **Relational Database**: Data organized in tables with relationships
- **Table**: Collection of related data organized in rows and columns
- **Row (Record)**: Individual data entry in a table
- **Column (Field)**: Specific data type or attribute
- **Primary Key**: Unique identifier for each row
- **Foreign Key**: Reference to primary key in another table
- **Index**: Data structure to improve query performance

### SQL Categories

- **DDL (Data Definition Language)**: CREATE, ALTER, DROP
- **DML (Data Manipulation Language)**: INSERT, UPDATE, DELETE
- **DQL (Data Query Language)**: SELECT
- **DCL (Data Control Language)**: GRANT, REVOKE

## Database Fundamentals

### Database Types

```sql
-- Relational Database (RDBMS)
-- Examples: MySQL, PostgreSQL, Oracle, SQL Server

-- NoSQL Databases
-- Examples: MongoDB, Cassandra, Redis
```

### Basic Database Operations

```sql
-- Create database
CREATE DATABASE database_name;

-- Use database
USE database_name;

-- Show databases
SHOW DATABASES;

-- Drop database
DROP DATABASE database_name;
```

### Data Types

```sql
-- Numeric Types
INT, INTEGER         -- 32-bit integer
BIGINT               -- 64-bit integer
SMALLINT             -- 16-bit integer
DECIMAL(p,s)         -- Fixed-point decimal
FLOAT                -- Single precision
DOUBLE               -- Double precision

-- String Types
CHAR(n)              -- Fixed-length string
VARCHAR(n)           -- Variable-length string
TEXT                 -- Long text
LONGTEXT             -- Very long text

-- Date and Time Types
DATE                 -- Date (YYYY-MM-DD)
TIME                 -- Time (HH:MM:SS)
DATETIME             -- Date and time
TIMESTAMP            -- Timestamp with timezone

-- Boolean Type
BOOLEAN, BOOL        -- True/False values

-- Binary Types
BLOB                 -- Binary large object
LONGBLOB             -- Very large binary object
```

## Data Definition Language (DDL)

### Creating Tables

```sql
-- Basic table creation
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Table with foreign key
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    order_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2) NOT NULL,
    status ENUM('pending', 'processing', 'shipped', 'delivered') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Table with constraints
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    stock_quantity INT NOT NULL DEFAULT 0 CHECK (stock_quantity >= 0),
    category_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

### Modifying Tables

```sql
-- Add column
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- Add column with position
ALTER TABLE users ADD COLUMN middle_name VARCHAR(50) AFTER first_name;

-- Modify column
ALTER TABLE users MODIFY COLUMN email VARCHAR(150);

-- Change column name
ALTER TABLE users CHANGE COLUMN phone phone_number VARCHAR(20);

-- Drop column
ALTER TABLE users DROP COLUMN middle_name;

-- Add constraint
ALTER TABLE users ADD CONSTRAINT unique_phone UNIQUE (phone_number);

-- Drop constraint
ALTER TABLE users DROP CONSTRAINT unique_phone;

-- Add index
ALTER TABLE users ADD INDEX idx_email (email);

-- Drop index
ALTER TABLE users DROP INDEX idx_email;
```

### Dropping Tables

```sql
-- Drop table
DROP TABLE table_name;

-- Drop table if exists
DROP TABLE IF EXISTS table_name;

-- Drop table with foreign key constraints
DROP TABLE table_name CASCADE;
```

## Data Manipulation Language (DML)

### INSERT Operations

```sql
-- Insert single row
INSERT INTO users (username, email, password_hash) 
VALUES ('john_doe', 'john@example.com', 'hashed_password');

-- Insert multiple rows
INSERT INTO users (username, email, password_hash) VALUES
    ('jane_smith', 'jane@example.com', 'hashed_password1'),
    ('bob_wilson', 'bob@example.com', 'hashed_password2'),
    ('alice_brown', 'alice@example.com', 'hashed_password3');

-- Insert with SELECT
INSERT INTO user_backup (username, email, created_at)
SELECT username, email, created_at FROM users WHERE created_at < '2023-01-01';

-- Insert with default values
INSERT INTO users (username, email) VALUES ('new_user', 'new@example.com');
```

### UPDATE Operations

```sql
-- Update single row
UPDATE users SET email = 'new_email@example.com' WHERE id = 1;

-- Update multiple columns
UPDATE users 
SET email = 'updated@example.com', 
    updated_at = CURRENT_TIMESTAMP 
WHERE id = 1;

-- Update with JOIN
UPDATE orders o
JOIN users u ON o.user_id = u.id
SET o.status = 'shipped'
WHERE u.email = 'customer@example.com';

-- Update with subquery
UPDATE products 
SET price = price * 1.1 
WHERE category_id IN (
    SELECT id FROM categories WHERE name = 'Electronics'
);

-- Update with CASE statement
UPDATE products 
SET price = CASE 
    WHEN price < 50 THEN price * 1.05
    WHEN price < 100 THEN price * 1.10
    ELSE price * 1.15
END;
```

### DELETE Operations

```sql
-- Delete specific rows
DELETE FROM users WHERE id = 1;

-- Delete with condition
DELETE FROM orders WHERE status = 'cancelled';

-- Delete with JOIN
DELETE o FROM orders o
JOIN users u ON o.user_id = u.id
WHERE u.email = 'spam@example.com';

-- Delete with subquery
DELETE FROM products 
WHERE category_id IN (
    SELECT id FROM categories WHERE name = 'Discontinued'
);

-- Delete all rows (truncate is faster)
TRUNCATE TABLE temp_table;
```

## Data Query Language (DQL)

### Basic SELECT

```sql
-- Select all columns
SELECT * FROM users;

-- Select specific columns
SELECT id, username, email FROM users;

-- Select with alias
SELECT id AS user_id, username AS name FROM users;

-- Select with calculation
SELECT id, username, YEAR(created_at) AS join_year FROM users;

-- Select with condition
SELECT * FROM users WHERE created_at >= '2023-01-01';

-- Select with multiple conditions
SELECT * FROM users 
WHERE created_at >= '2023-01-01' 
AND email LIKE '%@gmail.com';

-- Select with ORDER BY
SELECT * FROM users ORDER BY created_at DESC;

-- Select with LIMIT
SELECT * FROM users ORDER BY created_at DESC LIMIT 10;

-- Select with OFFSET (pagination)
SELECT * FROM users ORDER BY created_at DESC LIMIT 10 OFFSET 20;
```

### WHERE Clauses

```sql
-- Comparison operators
SELECT * FROM products WHERE price > 100;
SELECT * FROM products WHERE price <= 50;
SELECT * FROM products WHERE price != 0;
SELECT * FROM products WHERE price IS NULL;
SELECT * FROM products WHERE price IS NOT NULL;

-- Logical operators
SELECT * FROM products 
WHERE price > 50 AND stock_quantity > 0;

SELECT * FROM products 
WHERE category_id = 1 OR category_id = 2;

SELECT * FROM products 
WHERE NOT (price > 100 OR stock_quantity = 0);

-- IN operator
SELECT * FROM products WHERE category_id IN (1, 2, 3);

-- BETWEEN operator
SELECT * FROM products WHERE price BETWEEN 10 AND 100;

-- LIKE operator
SELECT * FROM users WHERE username LIKE 'john%';
SELECT * FROM users WHERE email LIKE '%@gmail.com';
SELECT * FROM users WHERE username LIKE '_ohn%';

-- Regular expressions (MySQL)
SELECT * FROM users WHERE username REGEXP '^[a-z]+$';
```

### DISTINCT and UNIQUE

```sql
-- Remove duplicates
SELECT DISTINCT category_id FROM products;

-- Count unique values
SELECT COUNT(DISTINCT category_id) FROM products;

-- Multiple columns
SELECT DISTINCT category_id, price FROM products;
```

## Data Control Language (DCL)

### User Management

```sql
-- Create user
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';

-- Create user with specific host
CREATE USER 'username'@'%' IDENTIFIED BY 'password';

-- Drop user
DROP USER 'username'@'localhost';

-- Change password
ALTER USER 'username'@'localhost' IDENTIFIED BY 'new_password';
```

### Privileges

```sql
-- Grant privileges
GRANT SELECT, INSERT, UPDATE, DELETE ON database_name.* TO 'username'@'localhost';

-- Grant all privileges
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';

-- Grant specific table privileges
GRANT SELECT, INSERT ON database_name.table_name TO 'username'@'localhost';

-- Grant with option to grant to others
GRANT SELECT ON database_name.* TO 'username'@'localhost' WITH GRANT OPTION;

-- Revoke privileges
REVOKE DELETE ON database_name.* FROM 'username'@'localhost';

-- Revoke all privileges
REVOKE ALL PRIVILEGES ON database_name.* FROM 'username'@'localhost';

-- Show privileges
SHOW GRANTS FOR 'username'@'localhost';
```

### Roles (PostgreSQL)

```sql
-- Create role
CREATE ROLE analyst_role;

-- Grant role to user
GRANT analyst_role TO username;

-- Grant privileges to role
GRANT SELECT ON ALL TABLES IN SCHEMA public TO analyst_role;

-- Revoke role
REVOKE analyst_role FROM username;

-- Drop role
DROP ROLE analyst_role;
```

## Advanced Queries

### Complex WHERE Conditions

```sql
-- Multiple conditions with parentheses
SELECT * FROM orders 
WHERE (status = 'pending' OR status = 'processing')
AND total_amount > 100
AND order_date >= '2023-01-01';

-- EXISTS operator
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.user_id = u.id 
    AND o.total_amount > 1000
);

-- NOT EXISTS
SELECT * FROM categories c
WHERE NOT EXISTS (
    SELECT 1 FROM products p 
    WHERE p.category_id = c.id
);
```

### CASE Statements

```sql
-- Simple CASE
SELECT id, username,
CASE status
    WHEN 'active' THEN 'User is active'
    WHEN 'inactive' THEN 'User is inactive'
    WHEN 'suspended' THEN 'User is suspended'
    ELSE 'Unknown status'
END AS status_description
FROM users;

-- Searched CASE
SELECT id, username, total_amount,
CASE 
    WHEN total_amount > 1000 THEN 'High Value'
    WHEN total_amount > 500 THEN 'Medium Value'
    WHEN total_amount > 100 THEN 'Low Value'
    ELSE 'Minimal Value'
END AS customer_tier
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;
```

## Joins and Relationships

### INNER JOIN

```sql
-- Basic inner join
SELECT u.username, o.order_date, o.total_amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- Multiple table join
SELECT u.username, p.name, oi.quantity, oi.price
FROM users u
INNER JOIN orders o ON u.id = o.user_id
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p ON oi.product_id = p.id;
```

### LEFT JOIN

```sql
-- Left join (all users, even without orders)
SELECT u.username, o.order_date, o.total_amount
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- Find users without orders
SELECT u.username
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;
```

### RIGHT JOIN

```sql
-- Right join (all orders, even without users)
SELECT u.username, o.order_date, o.total_amount
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;

-- Find orphaned orders
SELECT o.id, o.order_date, o.total_amount
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id
WHERE u.id IS NULL;
```

### FULL OUTER JOIN

```sql
-- Full outer join (PostgreSQL)
SELECT u.username, o.order_date, o.total_amount
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;

-- MySQL equivalent using UNION
SELECT u.username, o.order_date, o.total_amount
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
UNION
SELECT u.username, o.order_date, o.total_amount
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id
WHERE u.id IS NULL;
```

### CROSS JOIN

```sql
-- Cross join (Cartesian product)
SELECT u.username, c.name
FROM users u
CROSS JOIN categories c;

-- Self join
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.id;
```

## Subqueries and CTEs

### Subqueries in WHERE

```sql
-- Subquery in WHERE clause
SELECT * FROM products 
WHERE price > (SELECT AVG(price) FROM products);

-- Subquery with IN
SELECT * FROM users 
WHERE id IN (SELECT DISTINCT user_id FROM orders);

-- Subquery with EXISTS
SELECT * FROM categories c
WHERE EXISTS (
    SELECT 1 FROM products p 
    WHERE p.category_id = c.id 
    AND p.price > 100
);
```

### Subqueries in SELECT

```sql
-- Scalar subquery
SELECT username, 
       (SELECT COUNT(*) FROM orders WHERE user_id = users.id) as order_count
FROM users;

-- Subquery in calculation
SELECT username, 
       (SELECT SUM(total_amount) FROM orders WHERE user_id = users.id) as total_spent
FROM users;
```

### Common Table Expressions (CTEs)

```sql
-- Simple CTE
WITH user_orders AS (
    SELECT user_id, COUNT(*) as order_count, SUM(total_amount) as total_spent
    FROM orders
    GROUP BY user_id
)
SELECT u.username, uo.order_count, uo.total_spent
FROM users u
JOIN user_orders uo ON u.id = uo.user_id;

-- Multiple CTEs
WITH 
user_stats AS (
    SELECT user_id, COUNT(*) as order_count, SUM(total_amount) as total_spent
    FROM orders
    GROUP BY user_id
),
high_value_users AS (
    SELECT user_id FROM user_stats WHERE total_spent > 1000
)
SELECT u.username, us.order_count, us.total_spent
FROM users u
JOIN user_stats us ON u.id = us.user_id
JOIN high_value_users hvu ON u.id = hvu.user_id;

-- Recursive CTE (PostgreSQL)
WITH RECURSIVE category_tree AS (
    -- Base case
    SELECT id, name, parent_id, 1 as level
    FROM categories
    WHERE parent_id IS NULL
    
    UNION ALL
    
    -- Recursive case
    SELECT c.id, c.name, c.parent_id, ct.level + 1
    FROM categories c
    JOIN category_tree ct ON c.parent_id = ct.id
)
SELECT * FROM category_tree;
```

## Aggregation and Grouping

### Aggregate Functions

```sql
-- Basic aggregates
SELECT 
    COUNT(*) as total_orders,
    SUM(total_amount) as total_revenue,
    AVG(total_amount) as avg_order_value,
    MIN(total_amount) as min_order,
    MAX(total_amount) as max_order
FROM orders;

-- Conditional aggregates
SELECT 
    COUNT(*) as total_users,
    COUNT(CASE WHEN status = 'active' THEN 1 END) as active_users,
    COUNT(CASE WHEN status = 'inactive' THEN 1 END) as inactive_users
FROM users;

-- Aggregates with DISTINCT
SELECT 
    COUNT(DISTINCT user_id) as unique_customers,
    COUNT(DISTINCT product_id) as unique_products
FROM orders o
JOIN order_items oi ON o.id = oi.order_id;
```

### GROUP BY

```sql
-- Basic grouping
SELECT category_id, COUNT(*) as product_count, AVG(price) as avg_price
FROM products
GROUP BY category_id;

-- Multiple columns
SELECT category_id, status, COUNT(*) as count
FROM products
GROUP BY category_id, status;

-- Grouping with HAVING
SELECT category_id, COUNT(*) as product_count, AVG(price) as avg_price
FROM products
GROUP BY category_id
HAVING COUNT(*) > 5 AND AVG(price) > 50;

-- Grouping with ORDER BY
SELECT category_id, COUNT(*) as product_count
FROM products
GROUP BY category_id
ORDER BY product_count DESC;
```

### ROLLUP and CUBE

```sql
-- ROLLUP (MySQL)
SELECT category_id, status, COUNT(*) as count
FROM products
GROUP BY category_id, status WITH ROLLUP;

-- CUBE (PostgreSQL)
SELECT category_id, status, COUNT(*) as count
FROM products
GROUP BY CUBE(category_id, status);
```

## Window Functions

### Basic Window Functions

```sql
-- Row number
SELECT username, total_amount,
       ROW_NUMBER() OVER (ORDER BY total_amount DESC) as rank
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;

-- Rank with ties
SELECT username, total_amount,
       RANK() OVER (ORDER BY total_amount DESC) as rank
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;

-- Dense rank
SELECT username, total_amount,
       DENSE_RANK() OVER (ORDER BY total_amount DESC) as rank
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;
```

### Partitioned Window Functions

```sql
-- Partition by category
SELECT name, category_id, price,
       ROW_NUMBER() OVER (PARTITION BY category_id ORDER BY price DESC) as rank_in_category
FROM products;

-- Running total
SELECT order_date, total_amount,
       SUM(total_amount) OVER (ORDER BY order_date) as running_total
FROM orders;

-- Moving average
SELECT order_date, total_amount,
       AVG(total_amount) OVER (
           ORDER BY order_date 
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) as moving_avg_3_days
FROM orders;
```

### Advanced Window Functions

```sql
-- Lead and Lag
SELECT order_date, total_amount,
       LAG(total_amount) OVER (ORDER BY order_date) as previous_amount,
       LEAD(total_amount) OVER (ORDER BY order_date) as next_amount
FROM orders;

-- Percent rank
SELECT username, total_amount,
       PERCENT_RANK() OVER (ORDER BY total_amount) as percentile
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;

-- NTILE (divide into buckets)
SELECT username, total_amount,
       NTILE(4) OVER (ORDER BY total_amount DESC) as quartile
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;
```

## Indexing and Optimization

### Creating Indexes

```sql
-- Single column index
CREATE INDEX idx_email ON users(email);

-- Composite index
CREATE INDEX idx_user_status ON users(username, status);

-- Unique index
CREATE UNIQUE INDEX idx_unique_email ON users(email);

-- Partial index (PostgreSQL)
CREATE INDEX idx_active_users ON users(username) WHERE status = 'active';

-- Full-text index (MySQL)
CREATE FULLTEXT INDEX idx_product_search ON products(name, description);
```

### Index Types

```sql
-- B-tree index (default)
CREATE INDEX idx_price ON products(price);

-- Hash index (PostgreSQL)
CREATE INDEX idx_hash_email ON users USING hash(email);

-- GiST index (PostgreSQL)
CREATE INDEX idx_gist_location ON locations USING gist(coordinates);
```

### Query Optimization

```sql
-- Use EXPLAIN to analyze queries
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';

-- Force index usage
SELECT * FROM users FORCE INDEX (idx_email) WHERE email = 'test@example.com';

-- Use covering index
CREATE INDEX idx_covering ON users(id, username, email);
SELECT id, username, email FROM users WHERE id > 1000;
```

### Performance Tips

```sql
-- Use LIMIT for large result sets
SELECT * FROM orders ORDER BY order_date DESC LIMIT 100;

-- Avoid SELECT *
SELECT id, username, email FROM users;

-- Use appropriate data types
-- Use INT instead of VARCHAR for IDs
-- Use DECIMAL for monetary values

-- Use transactions for multiple operations
START TRANSACTION;
INSERT INTO orders (user_id, total_amount) VALUES (1, 100);
UPDATE users SET order_count = order_count + 1 WHERE id = 1;
COMMIT;
```

## Transactions and ACID

### Transaction Basics

```sql
-- Start transaction
START TRANSACTION;

-- Multiple operations
INSERT INTO orders (user_id, total_amount) VALUES (1, 100);
UPDATE users SET order_count = order_count + 1 WHERE id = 1;

-- Commit transaction
COMMIT;

-- Or rollback on error
ROLLBACK;
```

### ACID Properties

```sql
-- Atomicity: All or nothing
START TRANSACTION;
INSERT INTO orders (user_id, total_amount) VALUES (1, 100);
INSERT INTO order_items (order_id, product_id, quantity) VALUES (LAST_INSERT_ID(), 1, 2);
-- If any operation fails, all are rolled back
COMMIT;

-- Consistency: Data integrity
-- Foreign key constraints ensure consistency
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Isolation: Concurrent access
-- Use appropriate isolation levels
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Durability: Persistence
-- Once committed, changes are permanent
```

### Isolation Levels

```sql
-- Read Uncommitted (lowest isolation)
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Read Committed (default in most databases)
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Repeatable Read
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Serializable (highest isolation)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### Locking

```sql
-- Row-level locking
SELECT * FROM users WHERE id = 1 FOR UPDATE;

-- Shared lock
SELECT * FROM users WHERE id = 1 LOCK IN SHARE MODE;

-- Table lock
LOCK TABLES users WRITE;
-- operations
UNLOCK TABLES;
```

## Database Design

### Normalization

```sql
-- First Normal Form (1NF): Atomic values
-- Bad: users table with comma-separated phone numbers
-- Good: Separate phone_numbers table

-- Second Normal Form (2NF): No partial dependencies
-- Bad: orders table with product_name (depends on product_id)
-- Good: Separate products table

-- Third Normal Form (3NF): No transitive dependencies
-- Bad: orders table with category_name (depends on product_id)
-- Good: Separate categories table
```

### Entity-Relationship Design

```sql
-- One-to-One relationship
CREATE TABLE user_profiles (
    user_id INT PRIMARY KEY,
    bio TEXT,
    avatar_url VARCHAR(255),
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- One-to-Many relationship
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    order_date DATETIME,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Many-to-Many relationship
CREATE TABLE user_roles (
    user_id INT,
    role_id INT,
    PRIMARY KEY (user_id, role_id),
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (role_id) REFERENCES roles(id)
);
```

### Constraints

```sql
-- Primary key constraint
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50)
);

-- Foreign key constraint
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Check constraint
CREATE TABLE products (
    id INT PRIMARY KEY,
    price DECIMAL(10,2) CHECK (price > 0),
    stock_quantity INT CHECK (stock_quantity >= 0)
);

-- Unique constraint
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);

-- Not null constraint
CREATE TABLE users (
    id INT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL
);
```

## Common Database Systems

### MySQL

```sql
-- MySQL-specific features
-- AUTO_INCREMENT
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50)
);

-- ENGINE specification
CREATE TABLE logs (
    id INT PRIMARY KEY,
    message TEXT
) ENGINE=InnoDB;

-- MySQL functions
SELECT NOW(), CURDATE(), CURTIME();
SELECT CONCAT(first_name, ' ', last_name) as full_name FROM users;
SELECT DATE_FORMAT(created_at, '%Y-%m-%d') as date_only FROM users;
```

### PostgreSQL

```sql
-- PostgreSQL-specific features
-- SERIAL for auto-increment
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50)
);

-- JSON support
CREATE TABLE user_profiles (
    user_id INT PRIMARY KEY,
    profile_data JSONB
);

-- PostgreSQL functions
SELECT NOW(), CURRENT_DATE, CURRENT_TIME;
SELECT first_name || ' ' || last_name as full_name FROM users;
SELECT TO_CHAR(created_at, 'YYYY-MM-DD') as date_only FROM users;
```

### SQL Server

```sql
-- SQL Server-specific features
-- IDENTITY for auto-increment
CREATE TABLE users (
    id INT IDENTITY(1,1) PRIMARY KEY,
    username VARCHAR(50)
);

-- SQL Server functions
SELECT GETDATE(), GETUTCDATE();
SELECT CONCAT(first_name, ' ', last_name) as full_name FROM users;
SELECT FORMAT(created_at, 'yyyy-MM-dd') as date_only FROM users;
```

## Best Practices

### Naming Conventions

```sql
-- Use descriptive names
CREATE TABLE user_accounts;  -- Good
CREATE TABLE ua;             -- Bad

-- Use consistent naming
CREATE TABLE user_profiles;  -- Good
CREATE TABLE UserProfiles;   -- Inconsistent

-- Use plural for table names
CREATE TABLE users;          -- Good
CREATE TABLE user;           -- Bad

-- Use singular for column names
CREATE TABLE users (
    id INT,
    username VARCHAR(50)     -- Good
);
```

### Query Optimization

```sql
-- Use appropriate indexes
CREATE INDEX idx_user_email ON users(email);

-- Avoid SELECT *
SELECT id, username, email FROM users;

-- Use LIMIT for large datasets
SELECT * FROM orders ORDER BY order_date DESC LIMIT 1000;

-- Use appropriate data types
-- Use INT for IDs, DECIMAL for money, etc.

-- Use transactions for multiple operations
START TRANSACTION;
-- multiple operations
COMMIT;
```

### Security Best Practices

```sql
-- Use parameterized queries (prevent SQL injection)
-- Bad: "SELECT * FROM users WHERE id = " + user_input
-- Good: "SELECT * FROM users WHERE id = ?"

-- Limit user privileges
GRANT SELECT, INSERT ON database_name.* TO 'username'@'localhost';

-- Use strong passwords
CREATE USER 'username'@'localhost' IDENTIFIED BY 'strong_password_123!';

-- Regular backups
-- mysqldump -u username -p database_name > backup.sql

-- Monitor and log access
-- Enable query logging and access logs
```

## Troubleshooting

### Common Errors

```sql
-- Error 1062: Duplicate entry
-- Solution: Use INSERT IGNORE or ON DUPLICATE KEY UPDATE
INSERT IGNORE INTO users (email, username) VALUES ('test@example.com', 'testuser');

-- Error 1452: Cannot add or update a child row
-- Solution: Ensure foreign key exists
INSERT INTO orders (user_id, total_amount) VALUES (999, 100);  -- user_id 999 doesn't exist

-- Error 1054: Unknown column
-- Solution: Check column names and table structure
DESCRIBE table_name;

-- Error 1146: Table doesn't exist
-- Solution: Check table name and database
SHOW TABLES;
USE correct_database;
```

### Performance Issues

```sql
-- Slow queries
-- Use EXPLAIN to analyze
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';

-- Missing indexes
-- Check if indexes exist
SHOW INDEX FROM table_name;

-- Large result sets
-- Use LIMIT and pagination
SELECT * FROM orders ORDER BY order_date DESC LIMIT 50 OFFSET 100;

-- Lock contention
-- Use appropriate isolation levels
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### Debugging Queries

```sql
-- Enable query logging
SET GLOBAL general_log = 'ON';
SET GLOBAL log_output = 'TABLE';

-- Check slow query log
SHOW VARIABLES LIKE 'slow_query_log';

-- Analyze query performance
EXPLAIN FORMAT=JSON SELECT * FROM users WHERE email = 'test@example.com';

-- Check table statistics
ANALYZE TABLE users;
```

This comprehensive SQL guide covers all essential aspects of SQL, from basic operations to advanced techniques. Use it as a reference for database development, data analysis, and application development. 