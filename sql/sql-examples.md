# SQL Examples and Use Cases

A comprehensive collection of practical SQL examples covering common scenarios, data analysis patterns, and real-world applications.

## Table of Contents

1. [E-commerce Examples](#e-commerce-examples)
2. [User Analytics](#user-analytics)
3. [Financial Analysis](#financial-analysis)
4. [Inventory Management](#inventory-management)
5. [Reporting Queries](#reporting-queries)
6. [Data Cleaning](#data-cleaning)
7. [Performance Optimization](#performance-optimization)
8. [Advanced Analytics](#advanced-analytics)

## E-commerce Examples

### Customer Order Analysis

```sql
-- Sample tables for e-commerce
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    registration_date DATE,
    total_spent DECIMAL(10,2) DEFAULT 0
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATETIME,
    total_amount DECIMAL(10,2),
    status ENUM('pending', 'processing', 'shipped', 'delivered', 'cancelled'),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    unit_price DECIMAL(10,2),
    FOREIGN KEY (order_id) REFERENCES orders(order_id)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock_quantity INT
);

-- Top customers by total spent
SELECT 
    c.customer_id,
    CONCAT(c.first_name, ' ', c.last_name) as customer_name,
    c.email,
    COUNT(o.order_id) as total_orders,
    SUM(o.total_amount) as total_spent,
    AVG(o.total_amount) as avg_order_value
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.status != 'cancelled'
GROUP BY c.customer_id, c.first_name, c.last_name, c.email
ORDER BY total_spent DESC
LIMIT 10;

-- Customer retention analysis
SELECT 
    YEAR(o1.order_date) as year,
    MONTH(o1.order_date) as month,
    COUNT(DISTINCT o1.customer_id) as new_customers,
    COUNT(DISTINCT o2.customer_id) as returning_customers
FROM orders o1
LEFT JOIN orders o2 ON o1.customer_id = o2.customer_id 
    AND o2.order_date < o1.order_date
WHERE o1.status != 'cancelled'
GROUP BY YEAR(o1.order_date), MONTH(o1.order_date)
ORDER BY year, month;

-- Product performance analysis
SELECT 
    p.product_id,
    p.name,
    p.category,
    COUNT(oi.order_item_id) as times_ordered,
    SUM(oi.quantity) as total_quantity_sold,
    SUM(oi.quantity * oi.unit_price) as total_revenue,
    AVG(oi.unit_price) as avg_price
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id
WHERE o.status != 'cancelled'
GROUP BY p.product_id, p.name, p.category
ORDER BY total_revenue DESC;

-- Monthly sales trend
SELECT 
    YEAR(order_date) as year,
    MONTH(order_date) as month,
    COUNT(order_id) as total_orders,
    SUM(total_amount) as total_revenue,
    AVG(total_amount) as avg_order_value
FROM orders
WHERE status != 'cancelled'
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY year, month;

-- Customer lifetime value
WITH customer_metrics AS (
    SELECT 
        customer_id,
        COUNT(order_id) as order_count,
        SUM(total_amount) as total_spent,
        MIN(order_date) as first_order,
        MAX(order_date) as last_order,
        DATEDIFF(MAX(order_date), MIN(order_date)) as customer_lifespan_days
    FROM orders
    WHERE status != 'cancelled'
    GROUP BY customer_id
)
SELECT 
    customer_id,
    order_count,
    total_spent,
    customer_lifespan_days,
    total_spent / NULLIF(customer_lifespan_days, 0) as daily_value,
    total_spent / order_count as avg_order_value
FROM customer_metrics
ORDER BY total_spent DESC;
```

### Inventory Management

```sql
-- Low stock alert
SELECT 
    product_id,
    name,
    category,
    stock_quantity,
    CASE 
        WHEN stock_quantity = 0 THEN 'Out of Stock'
        WHEN stock_quantity <= 10 THEN 'Low Stock'
        WHEN stock_quantity <= 50 THEN 'Medium Stock'
        ELSE 'Well Stocked'
    END as stock_status
FROM products
WHERE stock_quantity <= 50
ORDER BY stock_quantity;

-- Reorder recommendations
WITH product_sales AS (
    SELECT 
        p.product_id,
        p.name,
        p.stock_quantity,
        COALESCE(SUM(oi.quantity), 0) as total_sold_last_30_days,
        COALESCE(AVG(oi.quantity), 0) as avg_daily_sales
    FROM products p
    LEFT JOIN order_items oi ON p.product_id = oi.product_id
    LEFT JOIN orders o ON oi.order_id = o.order_id
    WHERE o.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
        OR o.order_date IS NULL
    GROUP BY p.product_id, p.name, p.stock_quantity
)
SELECT 
    product_id,
    name,
    stock_quantity,
    total_sold_last_30_days,
    avg_daily_sales,
    CASE 
        WHEN stock_quantity < (avg_daily_sales * 7) THEN 'Reorder Soon'
        WHEN stock_quantity < (avg_daily_sales * 14) THEN 'Monitor Closely'
        ELSE 'Stock Adequate'
    END as reorder_status
FROM product_sales
ORDER BY avg_daily_sales DESC;

-- Category performance
SELECT 
    p.category,
    COUNT(p.product_id) as total_products,
    SUM(p.stock_quantity) as total_stock,
    AVG(p.price) as avg_price,
    SUM(oi.quantity * oi.unit_price) as total_revenue
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id
WHERE o.status != 'cancelled' OR o.status IS NULL
GROUP BY p.category
ORDER BY total_revenue DESC;
```

## User Analytics

### User Behavior Analysis

```sql
-- Sample user activity table
CREATE TABLE user_activity (
    activity_id INT PRIMARY KEY,
    user_id INT,
    activity_type ENUM('login', 'purchase', 'view_product', 'add_to_cart'),
    activity_date DATETIME,
    session_id VARCHAR(100),
    page_url VARCHAR(255)
);

-- Daily active users
SELECT 
    DATE(activity_date) as date,
    COUNT(DISTINCT user_id) as daily_active_users,
    COUNT(*) as total_activities
FROM user_activity
GROUP BY DATE(activity_date)
ORDER BY date DESC;

-- User engagement funnel
WITH funnel_data AS (
    SELECT 
        user_id,
        MAX(CASE WHEN activity_type = 'login' THEN 1 ELSE 0 END) as logged_in,
        MAX(CASE WHEN activity_type = 'view_product' THEN 1 ELSE 0 END) as viewed_product,
        MAX(CASE WHEN activity_type = 'add_to_cart' THEN 1 ELSE 0 END) as added_to_cart,
        MAX(CASE WHEN activity_type = 'purchase' THEN 1 ELSE 0 END) as made_purchase
    FROM user_activity
    WHERE activity_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
    GROUP BY user_id
)
SELECT 
    COUNT(*) as total_users,
    SUM(logged_in) as users_logged_in,
    SUM(viewed_product) as users_viewed_product,
    SUM(added_to_cart) as users_added_to_cart,
    Sum(made_purchase) as users_made_purchase,
    ROUND(SUM(logged_in) * 100.0 / COUNT(*), 2) as login_rate,
    ROUND(SUM(viewed_product) * 100.0 / SUM(logged_in), 2) as view_rate,
    ROUND(SUM(added_to_cart) * 100.0 / SUM(viewed_product), 2) as cart_rate,
    ROUND(SUM(made_purchase) * 100.0 / SUM(added_to_cart), 2) as purchase_rate
FROM funnel_data;

-- Session analysis
SELECT 
    session_id,
    user_id,
    MIN(activity_date) as session_start,
    MAX(activity_date) as session_end,
    TIMESTAMPDIFF(MINUTE, MIN(activity_date), MAX(activity_date)) as session_duration_minutes,
    COUNT(*) as activities_count
FROM user_activity
GROUP BY session_id, user_id
ORDER BY session_duration_minutes DESC;

-- Most active users
SELECT 
    ua.user_id,
    c.first_name,
    c.last_name,
    COUNT(*) as total_activities,
    COUNT(DISTINCT DATE(ua.activity_date)) as active_days,
    COUNT(DISTINCT ua.session_id) as total_sessions
FROM user_activity ua
JOIN customers c ON ua.user_id = c.customer_id
WHERE ua.activity_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY ua.user_id, c.first_name, c.last_name
ORDER BY total_activities DESC
LIMIT 20;
```

### Cohort Analysis

```sql
-- User cohort analysis
WITH user_cohorts AS (
    SELECT 
        customer_id,
        DATE_FORMAT(MIN(order_date), '%Y-%m-01') as cohort_month,
        DATE_FORMAT(order_date, '%Y-%m-01') as order_month,
        TIMESTAMPDIFF(MONTH, DATE_FORMAT(MIN(order_date), '%Y-%m-01'), DATE_FORMAT(order_date, '%Y-%m-01')) as month_diff
    FROM orders
    WHERE status != 'cancelled'
    GROUP BY customer_id, order_date
),
cohort_sizes AS (
    SELECT 
        cohort_month,
        COUNT(DISTINCT customer_id) as cohort_size
    FROM user_cohorts
    WHERE month_diff = 0
    GROUP BY cohort_month
),
cohort_retention AS (
    SELECT 
        cohort_month,
        month_diff,
        COUNT(DISTINCT customer_id) as retained_users
    FROM user_cohorts
    GROUP BY cohort_month, month_diff
)
SELECT 
    cr.cohort_month,
    cs.cohort_size,
    cr.month_diff,
    cr.retained_users,
    ROUND(cr.retained_users * 100.0 / cs.cohort_size, 2) as retention_rate
FROM cohort_retention cr
JOIN cohort_sizes cs ON cr.cohort_month = cs.cohort_month
ORDER BY cr.cohort_month, cr.month_diff;
```

## Financial Analysis

### Revenue Analysis

```sql
-- Monthly revenue growth
SELECT 
    YEAR(order_date) as year,
    MONTH(order_date) as month,
    SUM(total_amount) as monthly_revenue,
    LAG(SUM(total_amount)) OVER (ORDER BY YEAR(order_date), MONTH(order_date)) as prev_month_revenue,
    ROUND(
        (SUM(total_amount) - LAG(SUM(total_amount)) OVER (ORDER BY YEAR(order_date), MONTH(order_date))) * 100.0 / 
        LAG(SUM(total_amount)) OVER (ORDER BY YEAR(order_date), MONTH(order_date)), 2
    ) as growth_percentage
FROM orders
WHERE status != 'cancelled'
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY year, month;

-- Revenue by customer segment
WITH customer_segments AS (
    SELECT 
        customer_id,
        SUM(total_amount) as total_spent,
        CASE 
            WHEN SUM(total_amount) >= 1000 THEN 'High Value'
            WHEN SUM(total_amount) >= 500 THEN 'Medium Value'
            WHEN SUM(total_amount) >= 100 THEN 'Low Value'
            ELSE 'Minimal Value'
        END as segment
    FROM orders
    WHERE status != 'cancelled'
    GROUP BY customer_id
)
SELECT 
    segment,
    COUNT(*) as customer_count,
    SUM(total_spent) as total_revenue,
    AVG(total_spent) as avg_customer_value,
    ROUND(SUM(total_spent) * 100.0 / SUM(SUM(total_spent)) OVER (), 2) as revenue_percentage
FROM customer_segments
GROUP BY segment
ORDER BY total_revenue DESC;

-- Product profitability
SELECT 
    p.product_id,
    p.name,
    p.category,
    COUNT(oi.order_item_id) as units_sold,
    SUM(oi.quantity * oi.unit_price) as total_revenue,
    AVG(oi.unit_price) as avg_selling_price,
    p.price as cost_price,
    SUM(oi.quantity * (oi.unit_price - p.price)) as total_profit,
    ROUND(
        SUM(oi.quantity * (oi.unit_price - p.price)) * 100.0 / 
        SUM(oi.quantity * oi.unit_price), 2
    ) as profit_margin_percentage
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id
WHERE o.status != 'cancelled' OR o.status IS NULL
GROUP BY p.product_id, p.name, p.category, p.price
HAVING total_revenue > 0
ORDER BY total_profit DESC;
```

### Cash Flow Analysis

```sql
-- Daily cash flow
SELECT 
    DATE(order_date) as date,
    COUNT(order_id) as orders_count,
    SUM(total_amount) as daily_revenue,
    LAG(SUM(total_amount)) OVER (ORDER BY DATE(order_date)) as prev_day_revenue,
    SUM(total_amount) - LAG(SUM(total_amount)) OVER (ORDER BY DATE(order_date)) as revenue_change
FROM orders
WHERE status = 'delivered'
GROUP BY DATE(order_date)
ORDER BY date DESC;

-- Payment method analysis
SELECT 
    payment_method,
    COUNT(order_id) as orders_count,
    SUM(total_amount) as total_revenue,
    AVG(total_amount) as avg_order_value,
    ROUND(COUNT(order_id) * 100.0 / SUM(COUNT(order_id)) OVER (), 2) as order_percentage
FROM orders
WHERE status != 'cancelled'
GROUP BY payment_method
ORDER BY total_revenue DESC;
```

## Inventory Management

### Stock Level Monitoring

```sql
-- Real-time stock status
SELECT 
    product_id,
    name,
    category,
    stock_quantity,
    CASE 
        WHEN stock_quantity = 0 THEN 'Out of Stock'
        WHEN stock_quantity <= 5 THEN 'Critical Low'
        WHEN stock_quantity <= 20 THEN 'Low Stock'
        WHEN stock_quantity <= 100 THEN 'Normal'
        ELSE 'Overstocked'
    END as stock_level,
    ROUND(stock_quantity * 100.0 / (SELECT MAX(stock_quantity) FROM products), 2) as stock_percentage
FROM products
ORDER BY stock_quantity;

-- Stock turnover analysis
WITH product_turnover AS (
    SELECT 
        p.product_id,
        p.name,
        p.stock_quantity,
        COALESCE(SUM(oi.quantity), 0) as units_sold_30_days,
        COALESCE(SUM(oi.quantity), 0) / 30 as avg_daily_sales,
        p.stock_quantity / NULLIF(COALESCE(SUM(oi.quantity), 0) / 30, 0) as days_of_inventory
    FROM products p
    LEFT JOIN order_items oi ON p.product_id = oi.product_id
    LEFT JOIN orders o ON oi.order_id = o.order_id
    WHERE o.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
        OR o.order_date IS NULL
    GROUP BY p.product_id, p.name, p.stock_quantity
)
SELECT 
    product_id,
    name,
    stock_quantity,
    units_sold_30_days,
    avg_daily_sales,
    ROUND(days_of_inventory, 1) as days_of_inventory,
    CASE 
        WHEN days_of_inventory <= 7 THEN 'Reorder Immediately'
        WHEN days_of_inventory <= 14 THEN 'Reorder Soon'
        WHEN days_of_inventory <= 30 THEN 'Monitor'
        ELSE 'Overstocked'
    END as action_required
FROM product_turnover
ORDER BY days_of_inventory;

-- Seasonal inventory planning
SELECT 
    p.product_id,
    p.name,
    p.category,
    MONTH(o.order_date) as month,
    SUM(oi.quantity) as units_sold,
    AVG(oi.quantity) as avg_monthly_sales
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
LEFT JOIN orders o ON oi.order_id = o.order_id
WHERE o.order_date >= DATE_SUB(NOW(), INTERVAL 12 MONTH)
    AND o.status != 'cancelled'
GROUP BY p.product_id, p.name, p.category, MONTH(o.order_date)
ORDER BY p.product_id, month;
```

## Reporting Queries

### Executive Dashboard

```sql
-- Key performance indicators
SELECT 
    -- Revenue metrics
    (SELECT SUM(total_amount) FROM orders WHERE status != 'cancelled' AND order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)) as monthly_revenue,
    (SELECT SUM(total_amount) FROM orders WHERE status != 'cancelled' AND order_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)) as weekly_revenue,
    (SELECT SUM(total_amount) FROM orders WHERE status != 'cancelled' AND order_date >= DATE_SUB(NOW(), INTERVAL 1 DAY)) as daily_revenue,
    
    -- Order metrics
    (SELECT COUNT(*) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)) as monthly_orders,
    (SELECT COUNT(*) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)) as weekly_orders,
    (SELECT COUNT(*) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 1 DAY)) as daily_orders,
    
    -- Customer metrics
    (SELECT COUNT(DISTINCT customer_id) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)) as monthly_customers,
    (SELECT COUNT(DISTINCT customer_id) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)) as weekly_customers,
    (SELECT COUNT(DISTINCT customer_id) FROM orders WHERE order_date >= DATE_SUB(NOW(), INTERVAL 1 DAY)) as daily_customers,
    
    -- Average order value
    (SELECT AVG(total_amount) FROM orders WHERE status != 'cancelled' AND order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)) as avg_order_value;

-- Top performing products
SELECT 
    p.name,
    p.category,
    COUNT(oi.order_item_id) as times_ordered,
    SUM(oi.quantity) as total_quantity,
    SUM(oi.quantity * oi.unit_price) as total_revenue
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o ON oi.order_id = o.order_id
WHERE o.status != 'cancelled'
    AND o.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY p.product_id, p.name, p.category
ORDER BY total_revenue DESC
LIMIT 10;

-- Customer acquisition and retention
WITH customer_activity AS (
    SELECT 
        customer_id,
        MIN(order_date) as first_order,
        MAX(order_date) as last_order,
        COUNT(order_id) as total_orders,
        SUM(total_amount) as total_spent
    FROM orders
    WHERE status != 'cancelled'
    GROUP BY customer_id
)
SELECT 
    COUNT(*) as total_customers,
    COUNT(CASE WHEN first_order >= DATE_SUB(NOW(), INTERVAL 30 DAY) THEN 1 END) as new_customers_30_days,
    COUNT(CASE WHEN last_order >= DATE_SUB(NOW(), INTERVAL 30 DAY) THEN 1 END) as active_customers_30_days,
    COUNT(CASE WHEN last_order >= DATE_SUB(NOW(), INTERVAL 7 DAY) THEN 1 END) as active_customers_7_days,
    AVG(total_orders) as avg_orders_per_customer,
    AVG(total_spent) as avg_customer_lifetime_value
FROM customer_activity;
```

### Sales Reports

```sql
-- Daily sales report
SELECT 
    DATE(order_date) as sale_date,
    COUNT(order_id) as orders_count,
    SUM(total_amount) as total_sales,
    AVG(total_amount) as avg_order_value,
    COUNT(DISTINCT customer_id) as unique_customers
FROM orders
WHERE status != 'cancelled'
    AND order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY DATE(order_date)
ORDER BY sale_date DESC;

-- Category performance report
SELECT 
    p.category,
    COUNT(DISTINCT o.order_id) as orders_count,
    SUM(oi.quantity) as units_sold,
    SUM(oi.quantity * oi.unit_price) as total_revenue,
    AVG(oi.unit_price) as avg_unit_price,
    COUNT(DISTINCT o.customer_id) as unique_customers
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o ON oi.order_id = o.order_id
WHERE o.status != 'cancelled'
    AND o.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY p.category
ORDER BY total_revenue DESC;

-- Geographic sales report
SELECT 
    c.state,
    c.city,
    COUNT(o.order_id) as orders_count,
    SUM(o.total_amount) as total_sales,
    AVG(o.total_amount) as avg_order_value,
    COUNT(DISTINCT o.customer_id) as unique_customers
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE o.status != 'cancelled'
    AND o.order_date >= DATE_SUB(NOW(), INTERVAL 30 DAY)
GROUP BY c.state, c.city
ORDER BY total_sales DESC;
```

## Data Cleaning

### Data Quality Checks

```sql
-- Find duplicate emails
SELECT 
    email,
    COUNT(*) as duplicate_count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- Find invalid email formats
SELECT 
    customer_id,
    email
FROM customers
WHERE email NOT REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$';

-- Find orders with missing customer information
SELECT 
    o.order_id,
    o.customer_id,
    o.total_amount,
    c.first_name,
    c.last_name
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
WHERE c.customer_id IS NULL;

-- Find products with zero or negative prices
SELECT 
    product_id,
    name,
    price
FROM products
WHERE price <= 0;

-- Find orders with inconsistent totals
SELECT 
    o.order_id,
    o.total_amount as order_total,
    SUM(oi.quantity * oi.unit_price) as calculated_total,
    ABS(o.total_amount - SUM(oi.quantity * oi.unit_price)) as difference
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY o.order_id, o.total_amount
HAVING ABS(o.total_amount - SUM(oi.quantity * oi.unit_price)) > 0.01;
```

### Data Standardization

```sql
-- Standardize phone numbers
UPDATE customers 
SET phone = REGEXP_REPLACE(phone, '[^0-9]', '')
WHERE phone IS NOT NULL;

-- Standardize names (capitalize first letter)
UPDATE customers 
SET 
    first_name = CONCAT(UPPER(LEFT(first_name, 1)), LOWER(SUBSTRING(first_name, 2))),
    last_name = CONCAT(UPPER(LEFT(last_name, 1)), LOWER(SUBSTRING(last_name, 2)))
WHERE first_name IS NOT NULL AND last_name IS NOT NULL;

-- Remove leading/trailing whitespace
UPDATE customers 
SET 
    first_name = TRIM(first_name),
    last_name = TRIM(last_name),
    email = TRIM(email)
WHERE first_name IS NOT NULL OR last_name IS NOT NULL OR email IS NOT NULL;

-- Standardize product categories
UPDATE products 
SET category = CASE 
    WHEN LOWER(category) IN ('electronics', 'electronic', 'tech') THEN 'Electronics'
    WHEN LOWER(category) IN ('clothing', 'clothes', 'apparel') THEN 'Clothing'
    WHEN LOWER(category) IN ('books', 'book', 'literature') THEN 'Books'
    ELSE INITCAP(category)
END;
```

## Performance Optimization

### Query Optimization Examples

```sql
-- Use covering indexes
CREATE INDEX idx_covering_orders ON orders(customer_id, order_date, total_amount, status);

-- Optimize date range queries
SELECT * FROM orders 
WHERE order_date >= '2023-01-01' 
AND order_date < '2023-02-01';

-- Use EXISTS instead of IN for large datasets
SELECT * FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id 
    AND o.total_amount > 1000
);

-- Use LIMIT for pagination
SELECT * FROM orders 
ORDER BY order_date DESC 
LIMIT 20 OFFSET 40;

-- Use appropriate data types
-- Use INT for IDs, DECIMAL for money, DATE for dates

-- Use transactions for multiple operations
START TRANSACTION;
UPDATE products SET stock_quantity = stock_quantity - 1 WHERE product_id = 123;
INSERT INTO order_items (order_id, product_id, quantity, unit_price) VALUES (456, 123, 1, 29.99);
COMMIT;
```

### Index Strategy

```sql
-- Create composite indexes for common query patterns
CREATE INDEX idx_customer_orders ON orders(customer_id, order_date, status);

-- Create indexes for foreign keys
CREATE INDEX idx_order_customer ON orders(customer_id);
CREATE INDEX idx_order_items_order ON order_items(order_id);
CREATE INDEX idx_order_items_product ON order_items(product_id);

-- Create indexes for WHERE clauses
CREATE INDEX idx_orders_status_date ON orders(status, order_date);
CREATE INDEX idx_products_category ON products(category);

-- Create indexes for ORDER BY
CREATE INDEX idx_orders_date_desc ON orders(order_date DESC);

-- Create indexes for JOIN conditions
CREATE INDEX idx_customer_email ON customers(email);
```

## Advanced Analytics

### Predictive Analytics

```sql
-- Customer churn prediction
WITH customer_metrics AS (
    SELECT 
        customer_id,
        COUNT(order_id) as total_orders,
        SUM(total_amount) as total_spent,
        AVG(total_amount) as avg_order_value,
        DATEDIFF(NOW(), MAX(order_date)) as days_since_last_order,
        DATEDIFF(MAX(order_date), MIN(order_date)) as customer_lifespan_days
    FROM orders
    WHERE status != 'cancelled'
    GROUP BY customer_id
)
SELECT 
    customer_id,
    total_orders,
    total_spent,
    avg_order_value,
    days_since_last_order,
    customer_lifespan_days,
    CASE 
        WHEN days_since_last_order > 90 THEN 'High Churn Risk'
        WHEN days_since_last_order > 60 THEN 'Medium Churn Risk'
        WHEN days_since_last_order > 30 THEN 'Low Churn Risk'
        ELSE 'Active Customer'
    END as churn_risk
FROM customer_metrics
ORDER BY days_since_last_order DESC;

-- Seasonal trend analysis
SELECT 
    YEAR(order_date) as year,
    MONTH(order_date) as month,
    COUNT(order_id) as orders_count,
    SUM(total_amount) as total_revenue,
    AVG(total_amount) as avg_order_value,
    LAG(COUNT(order_id)) OVER (ORDER BY YEAR(order_date), MONTH(order_date)) as prev_month_orders,
    LAG(SUM(total_amount)) OVER (ORDER BY YEAR(order_date), MONTH(order_date)) as prev_month_revenue
FROM orders
WHERE status != 'cancelled'
GROUP BY YEAR(order_date), MONTH(order_date)
ORDER BY year, month;

-- Product recommendation engine
WITH customer_purchases AS (
    SELECT 
        o.customer_id,
        oi.product_id,
        COUNT(*) as purchase_count
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    WHERE o.status != 'cancelled'
    GROUP BY o.customer_id, oi.product_id
),
product_similarity AS (
    SELECT 
        cp1.product_id as product1,
        cp2.product_id as product2,
        COUNT(DISTINCT cp1.customer_id) as common_customers
    FROM customer_purchases cp1
    JOIN customer_purchases cp2 ON cp1.customer_id = cp2.customer_id
    WHERE cp1.product_id != cp2.product_id
    GROUP BY cp1.product_id, cp2.product_id
    HAVING common_customers >= 3
)
SELECT 
    p1.name as product_name,
    p2.name as recommended_product,
    ps.common_customers as similarity_score
FROM product_similarity ps
JOIN products p1 ON ps.product1 = p1.product_id
JOIN products p2 ON ps.product2 = p2.product_id
ORDER BY ps.common_customers DESC
LIMIT 20;
```

### A/B Testing Analysis

```sql
-- Sample A/B test results
CREATE TABLE ab_test_results (
    test_id INT,
    user_id INT,
    variant ENUM('A', 'B'),
    conversion BOOLEAN,
    revenue DECIMAL(10,2),
    test_date DATE
);

-- A/B test statistical analysis
SELECT 
    variant,
    COUNT(*) as total_users,
    SUM(conversion) as conversions,
    ROUND(SUM(conversion) * 100.0 / COUNT(*), 2) as conversion_rate,
    AVG(revenue) as avg_revenue,
    SUM(revenue) as total_revenue
FROM ab_test_results
WHERE test_id = 1
GROUP BY variant;

-- Statistical significance test (simplified)
WITH variant_stats AS (
    SELECT 
        variant,
        COUNT(*) as n,
        AVG(conversion) as p,
        VAR_SAMP(conversion) as variance
    FROM ab_test_results
    WHERE test_id = 1
    GROUP BY variant
)
SELECT 
    'A vs B' as comparison,
    ROUND(ABS(va.p - vb.p), 4) as difference,
    ROUND(SQRT((va.variance/va.n) + (vb.variance/vb.n)), 4) as standard_error,
    ROUND(ABS(va.p - vb.p) / SQRT((va.variance/va.n) + (vb.variance/vb.n)), 4) as z_score
FROM variant_stats va, variant_stats vb
WHERE va.variant = 'A' AND vb.variant = 'B';
```

These SQL examples demonstrate practical applications across various business scenarios. They can be adapted and extended based on specific requirements and data structures. 