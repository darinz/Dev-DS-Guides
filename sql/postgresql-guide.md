# PostgreSQL Complete Guide

A comprehensive guide to PostgreSQL, covering installation, configuration, advanced features, and PostgreSQL-specific functionality for database administration and development.

## Table of Contents

1. [Introduction](#introduction)
2. [Installation and Setup](#installation-and-setup)
3. [Basic Operations](#basic-operations)
4. [Advanced Data Types](#advanced-data-types)
5. [Indexing Strategies](#indexing-strategies)
6. [Performance Tuning](#performance-tuning)
7. [Advanced Features](#advanced-features)
8. [Backup and Recovery](#backup-and-recovery)
9. [Security](#security)
10. [Monitoring and Maintenance](#monitoring-and-maintenance)
11. [Extensions](#extensions)
12. [Best Practices](#best-practices)
13. [Troubleshooting](#troubleshooting)

## Introduction

PostgreSQL is a powerful, open-source object-relational database system with over 30 years of active development. It offers advanced features, reliability, and extensibility.

### Key Features

- **ACID Compliance**: Full transaction support
- **Extensible**: Custom functions, operators, and data types
- **Advanced Data Types**: JSON, arrays, geometric, network address types
- **Concurrent Access**: Multi-version concurrency control (MVCC)
- **Replication**: Built-in streaming and logical replication
- **Extensions**: Rich ecosystem of extensions
- **Full-Text Search**: Built-in text search capabilities
- **Geographic Objects**: PostGIS extension for spatial data

### PostgreSQL vs Other Databases

| Feature | PostgreSQL | MySQL | SQL Server |
|---------|------------|-------|------------|
| ACID Compliance | Full | InnoDB only | Full |
| JSON Support | Native | Limited | Limited |
| Extensibility | High | Low | Medium |
| Replication | Built-in | Third-party | Built-in |
| Open Source | Yes | Yes | No |

## Installation and Setup

### Installation on macOS

```bash
# Using Homebrew
brew install postgresql

# Start PostgreSQL service
brew services start postgresql

# Create a database
createdb mydatabase

# Connect to database
psql mydatabase
```

### Installation on Ubuntu/Debian

```bash
# Update package list
sudo apt update

# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Start PostgreSQL service
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Switch to postgres user
sudo -i -u postgres

# Create a new user and database
createuser --interactive myuser
createdb -O myuser mydatabase
```

### Installation on Windows

```bash
# Download from https://www.postgresql.org/download/windows/
# Run the installer and follow the setup wizard

# Add PostgreSQL to PATH
# Default installation path: C:\Program Files\PostgreSQL\{version}\bin
```

### Initial Configuration

```bash
# Edit postgresql.conf
sudo nano /etc/postgresql/{version}/main/postgresql.conf

# Common settings
listen_addresses = '*'          # Listen on all interfaces
port = 5432                     # Default port
max_connections = 100           # Maximum connections
shared_buffers = 128MB          # Memory for shared buffers
effective_cache_size = 4GB      # Available memory for caching
work_mem = 4MB                  # Memory for query operations
maintenance_work_mem = 64MB     # Memory for maintenance operations

# Edit pg_hba.conf for authentication
sudo nano /etc/postgresql/{version}/main/pg_hba.conf

# Allow local connections
local   all             all                                     peer
# Allow remote connections (use with caution)
host    all             all             0.0.0.0/0               md5

# Restart PostgreSQL
sudo systemctl restart postgresql
```

## Basic Operations

### Connecting to PostgreSQL

```bash
# Connect to default database
psql

# Connect to specific database
psql -d database_name

# Connect with username
psql -U username -d database_name

# Connect to remote server
psql -h hostname -p port -U username -d database_name

# Connect with password prompt
psql -h hostname -U username -d database_name -W
```

### Basic SQL Commands

```sql
-- List databases
\l

-- Connect to database
\c database_name

-- List tables
\dt

-- Describe table
\d table_name

-- List schemas
\dn

-- List users
\du

-- Show current user
SELECT current_user;

-- Show current database
SELECT current_database();

-- Show version
SELECT version();
```

### Database Management

```sql
-- Create database
CREATE DATABASE mydatabase;

-- Create database with owner
CREATE DATABASE mydatabase OWNER myuser;

-- Create database with encoding
CREATE DATABASE mydatabase 
    WITH ENCODING = 'UTF8' 
    LC_COLLATE = 'en_US.UTF-8' 
    LC_CTYPE = 'en_US.UTF-8';

-- Drop database
DROP DATABASE mydatabase;

-- Rename database
ALTER DATABASE oldname RENAME TO newname;

-- Set database properties
ALTER DATABASE mydatabase SET enable_indexscan = off;
```

### User and Role Management

```sql
-- Create user/role
CREATE USER myuser WITH PASSWORD 'mypassword';

-- Create role with specific privileges
CREATE ROLE myrole WITH 
    LOGIN 
    PASSWORD 'mypassword' 
    CREATEDB 
    CREATEROLE;

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO myuser;

-- Revoke privileges
REVOKE DELETE ON ALL TABLES IN SCHEMA public FROM myuser;

-- Drop user
DROP USER myuser;

-- Change password
ALTER USER myuser WITH PASSWORD 'newpassword';

-- List users and their privileges
\du
```

## Advanced Data Types

### JSON and JSONB

```sql
-- Create table with JSON columns
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    profile JSON,
    settings JSONB
);

-- Insert JSON data
INSERT INTO users (name, profile, settings) VALUES (
    'John Doe',
    '{"age": 30, "city": "New York", "interests": ["music", "sports"]}',
    '{"theme": "dark", "notifications": true, "language": "en"}'
);

-- Query JSON data
SELECT name, profile->>'city' as city FROM users;
SELECT name, settings->>'theme' as theme FROM users;

-- Query JSON arrays
SELECT name, profile->'interests'->0 as first_interest FROM users;

-- Check if JSON contains key
SELECT name FROM users WHERE settings ? 'theme';

-- Check if JSON contains value
SELECT name FROM users WHERE settings @> '{"theme": "dark"}';

-- Index JSONB columns
CREATE INDEX idx_settings ON users USING GIN (settings);
```

### Arrays

```sql
-- Create table with array columns
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    tags TEXT[],
    prices NUMERIC(10,2)[]
);

-- Insert array data
INSERT INTO products (name, tags, prices) VALUES (
    'Laptop',
    ARRAY['electronics', 'computer', 'portable'],
    ARRAY[999.99, 899.99, 799.99]
);

-- Query array data
SELECT name FROM products WHERE 'electronics' = ANY(tags);
SELECT name FROM products WHERE tags @> ARRAY['computer'];

-- Array functions
SELECT array_length(tags, 1) as tag_count FROM products;
SELECT unnest(tags) as tag FROM products WHERE name = 'Laptop';

-- Index array columns
CREATE INDEX idx_tags ON products USING GIN (tags);
```

### Geometric Types

```sql
-- Enable PostGIS extension
CREATE EXTENSION postgis;

-- Create table with geometric data
CREATE TABLE locations (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    point GEOMETRY(POINT, 4326),
    polygon GEOMETRY(POLYGON, 4326)
);

-- Insert geometric data
INSERT INTO locations (name, point) VALUES (
    'Central Park',
    ST_GeomFromText('POINT(-73.9654 40.7829)', 4326)
);

-- Spatial queries
SELECT name, ST_Distance(point, ST_GeomFromText('POINT(-73.9857 40.7484)', 4326)) as distance
FROM locations
ORDER BY distance;

-- Find points within polygon
SELECT name FROM locations 
WHERE ST_Within(point, ST_GeomFromText('POLYGON((...))', 4326));
```

### Network Address Types

```sql
-- Create table with network types
CREATE TABLE network_devices (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    ip_address INET,
    mac_address MACADDR,
    cidr_range CIDR
);

-- Insert network data
INSERT INTO network_devices (name, ip_address, mac_address, cidr_range) VALUES (
    'Router',
    '192.168.1.1',
    '00:11:22:33:44:55',
    '192.168.1.0/24'
);

-- Network queries
SELECT name FROM network_devices WHERE ip_address << '192.168.1.0/24';
SELECT name FROM network_devices WHERE mac_address = '00:11:22:33:44:55';
```

### UUID Type

```sql
-- Enable UUID extension
CREATE EXTENSION "uuid-ossp";

-- Create table with UUID
CREATE TABLE documents (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    title VARCHAR(200),
    content TEXT
);

-- Generate UUIDs
SELECT uuid_generate_v4();
SELECT gen_random_uuid();
```

## Indexing Strategies

### B-tree Indexes

```sql
-- Default index type
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_orders_date ON orders(order_date);

-- Composite indexes
CREATE INDEX idx_users_name_email ON users(last_name, first_name, email);

-- Partial indexes
CREATE INDEX idx_active_users ON users(email) WHERE status = 'active';

-- Unique indexes
CREATE UNIQUE INDEX idx_unique_email ON users(email);
```

### Hash Indexes

```sql
-- Hash indexes for equality comparisons
CREATE INDEX idx_hash_email ON users USING hash(email);
```

### GiST Indexes

```sql
-- For geometric data
CREATE INDEX idx_locations_point ON locations USING gist(point);

-- For full-text search
CREATE INDEX idx_documents_content ON documents USING gist(to_tsvector('english', content));
```

### GIN Indexes

```sql
-- For arrays and JSONB
CREATE INDEX idx_products_tags ON products USING gin(tags);
CREATE INDEX idx_users_settings ON users USING gin(settings);

-- For full-text search
CREATE INDEX idx_documents_content ON documents USING gin(to_tsvector('english', content));
```

### BRIN Indexes

```sql
-- For large tables with natural ordering
CREATE INDEX idx_orders_date_brin ON orders USING brin(order_date);
```

### Index Maintenance

```sql
-- Analyze table statistics
ANALYZE users;

-- Reindex table
REINDEX TABLE users;

-- Reindex specific index
REINDEX INDEX idx_users_email;

-- Check index usage
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;
```

## Performance Tuning

### Configuration Tuning

```sql
-- Memory settings
shared_buffers = 25% of RAM
effective_cache_size = 75% of RAM
work_mem = 4MB to 64MB per connection
maintenance_work_mem = 64MB to 1GB

-- Connection settings
max_connections = 100 to 1000
max_worker_processes = 8
max_parallel_workers_per_gather = 4
max_parallel_workers = 8

-- Write-ahead logging
wal_buffers = 16MB
checkpoint_completion_target = 0.9
wal_writer_delay = 200ms

-- Query planner
random_page_cost = 1.1 (for SSD)
effective_io_concurrency = 200 (for SSD)
```

### Query Optimization

```sql
-- Use EXPLAIN to analyze queries
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';

-- Detailed execution plan
EXPLAIN (ANALYZE, BUFFERS) SELECT * FROM users WHERE email = 'test@example.com';

-- JSON format
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON) SELECT * FROM users WHERE email = 'test@example.com';

-- Force index usage
SELECT * FROM users WHERE email = 'test@example.com';
-- PostgreSQL will automatically choose the best index

-- Use covering indexes
CREATE INDEX idx_covering_users ON users(id, email, name);
SELECT id, email, name FROM users WHERE email = 'test@example.com';
```

### Partitioning

```sql
-- Create partitioned table
CREATE TABLE orders (
    id SERIAL,
    order_date DATE,
    customer_id INT,
    total_amount DECIMAL(10,2)
) PARTITION BY RANGE (order_date);

-- Create partitions
CREATE TABLE orders_2023 PARTITION OF orders
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE orders_2024 PARTITION OF orders
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');

-- List partitioning
CREATE TABLE products (
    id SERIAL,
    name VARCHAR(100),
    category VARCHAR(50)
) PARTITION BY LIST (category);

CREATE TABLE products_electronics PARTITION OF products
    FOR VALUES IN ('electronics', 'computers');

CREATE TABLE products_clothing PARTITION OF products
    FOR VALUES IN ('clothing', 'shoes');
```

### Parallel Query Execution

```sql
-- Enable parallel queries
SET max_parallel_workers_per_gather = 4;
SET parallel_tuple_cost = 0.1;
SET parallel_setup_cost = 1000;

-- Check parallel execution
EXPLAIN (ANALYZE, BUFFERS) SELECT COUNT(*) FROM large_table;
```

## Advanced Features

### Common Table Expressions (CTEs)

```sql
-- Simple CTE
WITH user_stats AS (
    SELECT user_id, COUNT(*) as order_count, SUM(total_amount) as total_spent
    FROM orders
    GROUP BY user_id
)
SELECT u.name, us.order_count, us.total_spent
FROM users u
JOIN user_stats us ON u.id = us.user_id;

-- Recursive CTE
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

### Window Functions

```sql
-- Row numbering
SELECT 
    name,
    total_amount,
    ROW_NUMBER() OVER (ORDER BY total_amount DESC) as rank
FROM users u
JOIN (
    SELECT user_id, SUM(total_amount) as total_amount
    FROM orders
    GROUP BY user_id
) o ON u.id = o.user_id;

-- Partitioned window functions
SELECT 
    category,
    name,
    price,
    ROW_NUMBER() OVER (PARTITION BY category ORDER BY price DESC) as rank_in_category
FROM products;

-- Running totals
SELECT 
    order_date,
    total_amount,
    SUM(total_amount) OVER (ORDER BY order_date) as running_total
FROM orders;
```

### Full-Text Search

```sql
-- Create full-text search index
CREATE INDEX idx_documents_content ON documents 
USING gin(to_tsvector('english', content));

-- Search documents
SELECT title, ts_rank(to_tsvector('english', content), query) as rank
FROM documents, to_tsquery('english', 'postgresql & database') query
WHERE to_tsvector('english', content) @@ query
ORDER BY rank DESC;

-- Search with weights
SELECT title, ts_rank(
    setweight(to_tsvector('english', title), 'A') ||
    setweight(to_tsvector('english', content), 'B'),
    query
) as rank
FROM documents, to_tsquery('english', 'postgresql & database') query
WHERE to_tsvector('english', title || ' ' || content) @@ query
ORDER BY rank DESC;
```

### Materialized Views

```sql
-- Create materialized view
CREATE MATERIALIZED VIEW user_order_summary AS
SELECT 
    u.id,
    u.name,
    COUNT(o.id) as order_count,
    SUM(o.total_amount) as total_spent
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.name;

-- Refresh materialized view
REFRESH MATERIALIZED VIEW user_order_summary;

-- Concurrent refresh (PostgreSQL 9.4+)
REFRESH MATERIALIZED VIEW CONCURRENTLY user_order_summary;
```

### Foreign Data Wrappers

```sql
-- Install postgres_fdw extension
CREATE EXTENSION postgres_fdw;

-- Create foreign server
CREATE SERVER remote_server
    FOREIGN DATA WRAPPER postgres_fdw
    OPTIONS (host 'remote-host', port '5432', dbname 'remote_db');

-- Create user mapping
CREATE USER MAPPING FOR current_user
    SERVER remote_server
    OPTIONS (user 'remote_user', password 'remote_password');

-- Create foreign table
CREATE FOREIGN TABLE remote_users (
    id INT,
    name VARCHAR(100)
) SERVER remote_server OPTIONS (schema_name 'public', table_name 'users');

-- Query foreign table
SELECT * FROM remote_users;
```

## Backup and Recovery

### Logical Backups

```bash
# Create database dump
pg_dump database_name > backup.sql

# Create compressed dump
pg_dump database_name | gzip > backup.sql.gz

# Create custom format dump
pg_dump -Fc database_name > backup.dump

# Create dump with specific options
pg_dump -h localhost -U username -d database_name \
    --exclude-table=temp_table \
    --exclude-table-data=log_table \
    > backup.sql

# Restore from dump
psql database_name < backup.sql

# Restore from custom format
pg_restore -d database_name backup.dump

# Restore with options
pg_restore -h localhost -U username -d database_name \
    --clean --if-exists backup.dump
```

### Physical Backups

```bash
# Enable WAL archiving
# In postgresql.conf:
wal_level = replica
archive_mode = on
archive_command = 'cp %p /path/to/archive/%f'

# Create base backup
pg_basebackup -h localhost -U username -D /path/to/backup -Ft -z -P

# Restore from base backup
# Stop PostgreSQL
sudo systemctl stop postgresql

# Remove data directory
sudo rm -rf /var/lib/postgresql/data

# Restore from backup
sudo -u postgres pg_basebackup -D /var/lib/postgresql/data \
    -Ft -z -P /path/to/backup

# Start PostgreSQL
sudo systemctl start postgresql
```

### Point-in-Time Recovery

```sql
-- Create recovery.conf
restore_command = 'cp /path/to/archive/%f %p'
recovery_target_time = '2023-12-01 10:00:00'
recovery_target_action = 'promote'
```

### Continuous Archiving

```sql
-- Configure archiving
wal_level = replica
archive_mode = on
archive_command = 'cp %p /path/to/archive/%f'
archive_timeout = 300

-- Create archive directory
mkdir -p /path/to/archive
chown postgres:postgres /path/to/archive
```

## Security

### Authentication Methods

```sql
-- Local connections (peer authentication)
local   all             all                                     peer

-- Password authentication
local   all             all                                     md5
host    all             all             127.0.0.1/32            md5

-- Certificate authentication
hostssl all             all             0.0.0.0/0               cert

-- LDAP authentication
host    all             all             0.0.0.0/0               ldap
```

### Row Level Security

```sql
-- Enable RLS
ALTER TABLE users ENABLE ROW LEVEL SECURITY;

-- Create policy
CREATE POLICY user_policy ON users
    FOR ALL
    TO authenticated_users
    USING (id = current_user_id());

-- Policy for different operations
CREATE POLICY user_select_policy ON users
    FOR SELECT
    TO authenticated_users
    USING (id = current_user_id());

CREATE POLICY user_insert_policy ON users
    FOR INSERT
    TO authenticated_users
    WITH CHECK (id = current_user_id());
```

### Encryption

```sql
-- Encrypt sensitive columns
CREATE EXTENSION pgcrypto;

-- Encrypt data
INSERT INTO users (name, encrypted_ssn) VALUES (
    'John Doe',
    pgp_sym_encrypt('123-45-6789', 'encryption_key')
);

-- Decrypt data
SELECT name, pgp_sym_decrypt(encrypted_ssn, 'encryption_key') as ssn
FROM users;
```

### SSL/TLS Configuration

```sql
-- Enable SSL
ssl = on
ssl_cert_file = 'server.crt'
ssl_key_file = 'server.key'
ssl_ca_file = 'ca.crt'

-- Require SSL connections
hostssl all             all             0.0.0.0/0               md5
```

## Monitoring and Maintenance

### System Statistics

```sql
-- View database statistics
SELECT * FROM pg_stat_database WHERE datname = current_database();

-- View table statistics
SELECT schemaname, tablename, n_tup_ins, n_tup_upd, n_tup_del
FROM pg_stat_user_tables
ORDER BY n_tup_ins DESC;

-- View index statistics
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;

-- View active connections
SELECT pid, usename, application_name, client_addr, state
FROM pg_stat_activity
WHERE state = 'active';

-- View locks
SELECT l.pid, l.mode, l.granted, a.usename, a.query
FROM pg_locks l
JOIN pg_stat_activity a ON l.pid = a.pid
WHERE NOT l.granted;
```

### Maintenance Tasks

```sql
-- Vacuum tables
VACUUM users;
VACUUM ANALYZE users;
VACUUM FULL users;

-- Auto-vacuum configuration
autovacuum = on
autovacuum_vacuum_threshold = 50
autovacuum_analyze_threshold = 50
autovacuum_vacuum_scale_factor = 0.2
autovacuum_analyze_scale_factor = 0.1

-- Update table statistics
ANALYZE users;
ANALYZE VERBOSE users;

-- Check table bloat
SELECT schemaname, tablename, n_dead_tup, n_live_tup,
       ROUND(n_dead_tup * 100.0 / NULLIF(n_live_tup + n_dead_tup, 0), 2) as bloat_percent
FROM pg_stat_user_tables
ORDER BY bloat_percent DESC;
```

### Logging and Monitoring

```sql
-- Configure logging
log_destination = 'stderr'
logging_collector = on
log_directory = 'log'
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'
log_rotation_age = 1d
log_rotation_size = 100MB

-- Log specific events
log_statement = 'all'
log_min_duration_statement = 1000
log_checkpoints = on
log_connections = on
log_disconnections = on
log_lock_waits = on
log_temp_files = 0
```

## Extensions

### Popular Extensions

```sql
-- UUID generation
CREATE EXTENSION "uuid-ossp";

-- Full-text search dictionaries
CREATE EXTENSION unaccent;

-- Geographic objects
CREATE EXTENSION postgis;

-- JSON processing
CREATE EXTENSION jsonb_plpython3u;

-- Cryptography
CREATE EXTENSION pgcrypto;

-- Network address types
CREATE EXTENSION ip4r;

-- Temporal data types
CREATE EXTENSION temporal_tables;

-- Advanced indexing
CREATE EXTENSION btree_gin;
CREATE EXTENSION btree_gist;
```

### Custom Extensions

```sql
-- Create extension directory
mkdir -p /usr/share/postgresql/extension

-- Create extension control file
-- my_extension.control
default_version = '1.0'
module_pathname = '$libdir/my_extension'
relocatable = true

-- Create SQL file
-- my_extension--1.0.sql
CREATE OR REPLACE FUNCTION my_function()
RETURNS text AS $$
BEGIN
    RETURN 'Hello from my extension!';
END;
$$ LANGUAGE plpgsql;
```

## Best Practices

### Database Design

```sql
-- Use appropriate data types
-- Use UUID for distributed systems
-- Use TIMESTAMPTZ for timezone-aware timestamps
-- Use NUMERIC for monetary values

-- Normalize data appropriately
-- Use foreign keys for referential integrity
-- Use constraints for data validation

-- Example of good table design
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    customer_id UUID NOT NULL REFERENCES customers(id),
    order_date TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    total_amount NUMERIC(10,2) NOT NULL CHECK (total_amount > 0),
    status order_status NOT NULL DEFAULT 'pending',
    created_at TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Create updated_at trigger
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_orders_updated_at 
    BEFORE UPDATE ON orders 
    FOR EACH ROW 
    EXECUTE FUNCTION update_updated_at_column();
```

### Query Optimization

```sql
-- Use appropriate indexes
-- Use covering indexes for frequently accessed columns
-- Use partial indexes for filtered queries
-- Use composite indexes for multi-column queries

-- Use prepared statements
PREPARE user_query(text) AS
SELECT * FROM users WHERE email = $1;

EXECUTE user_query('test@example.com');

-- Use connection pooling
-- Use PgBouncer or similar tools for connection management

-- Monitor query performance
-- Use pg_stat_statements extension
CREATE EXTENSION pg_stat_statements;

SELECT query, calls, total_time, mean_time
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;
```

### Security Best Practices

```sql
-- Use least privilege principle
-- Create specific roles for different applications
-- Use row-level security for multi-tenant applications
-- Encrypt sensitive data
-- Use SSL/TLS for network connections
-- Regularly update PostgreSQL
-- Monitor access logs
-- Use strong passwords
-- Implement connection limits
```

## Troubleshooting

### Common Issues

```sql
-- Connection issues
-- Check pg_hba.conf configuration
-- Verify network connectivity
-- Check firewall settings

-- Performance issues
-- Check slow query log
-- Analyze query execution plans
-- Monitor system resources

-- Lock issues
SELECT pid, mode, granted, query_start, query
FROM pg_locks l
JOIN pg_stat_activity a ON l.pid = a.pid
WHERE NOT l.granted;

-- Kill long-running queries
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE state = 'active' 
AND query_start < NOW() - INTERVAL '1 hour';

-- Disk space issues
-- Monitor table and index sizes
SELECT schemaname, tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size
FROM pg_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;
```

### Debugging Tools

```sql
-- Enable debug logging
log_statement = 'all'
log_min_duration_statement = 0

-- Use pg_stat_statements for query analysis
CREATE EXTENSION pg_stat_statements;

-- Use auto_explain for automatic query analysis
shared_preload_libraries = 'auto_explain'
auto_explain.log_min_duration = 1000
auto_explain.log_analyze = on

-- Use pg_stat_monitor for detailed monitoring
CREATE EXTENSION pg_stat_monitor;
```

### Performance Monitoring

```sql
-- Monitor database size
SELECT pg_size_pretty(pg_database_size(current_database()));

-- Monitor table sizes
SELECT schemaname, tablename, pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size
FROM pg_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Monitor index usage
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;

-- Monitor slow queries
SELECT query, calls, total_time, mean_time, rows
FROM pg_stat_statements
ORDER BY total_time DESC
LIMIT 10;
```

This comprehensive PostgreSQL guide covers all essential aspects of PostgreSQL administration and development, from basic setup to advanced features and optimization techniques. 