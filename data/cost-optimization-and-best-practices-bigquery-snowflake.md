---
date: 2025-06-12T13:08
title: Cost Optimization and Best Practices for BigQuery and Snowflake
tags: 
    - Data
---
<!-- 2025-06-12-1308 (June 12, 2025 01:08:13 PM) -->

# Cost Optimization and Best Practices for BigQuery and Snowflake

## Snowflake Cost Optimization

### 1. Warehouse Management

```sql
-- Auto-suspend warehouses when idle (in seconds)
ALTER WAREHOUSE my_warehouse SET AUTO_SUSPEND = 60;

-- Auto-resume when queries come in
ALTER WAREHOUSE my_warehouse SET AUTO_RESUME = TRUE;

-- Use multi-cluster for better scaling instead of oversizing
ALTER WAREHOUSE my_warehouse SET 
  MIN_CLUSTER_COUNT = 1 
  MAX_CLUSTER_COUNT = 3
  SCALING_POLICY = 'ECONOMY';
```

### 2. Query Optimization

```sql
-- Use EXPLAIN to see query execution plan
EXPLAIN SELECT * FROM large_table WHERE complex_condition;

-- Minimize data scanned using partitioning
CREATE TABLE sales (
  sale_date DATE,
  amount NUMBER
)
CLUSTER BY (sale_date);

-- Cache results of common queries
CREATE OR REPLACE SECURE VIEW monthly_sales AS
SELECT month(sale_date) as month, sum(amount) as total
FROM sales 
GROUP BY 1;
```

### 3. Storage Optimization

```sql
-- Use transient tables for temporary data
CREATE TRANSIENT TABLE temp_analysis AS
SELECT * FROM main_data;

-- Set shorter retention periods
ALTER DATABASE analytics 
SET DATA_RETENTION_TIME_IN_DAYS = 1;

-- Use appropriate compression
CREATE TABLE compressed_logs
COMPRESSION = 'ZSTD'
AS SELECT * FROM raw_logs;
```

### 4. Pricing Tiers

- **Standard**: Basic tier, pay-as-you-go
- **Enterprise**: Advanced security, higher limits
- **Business Critical**: Maximum compliance and availability
- **Virtual Private Snowflake (VPS)**: Isolated environment

## BigQuery Cost Optimization

### 1. Query Optimization

```sql
-- Use column selection instead of SELECT *
SELECT specific_column1, specific_column2
FROM dataset.huge_table
WHERE filter_condition;

-- Leverage partitioning
CREATE OR REPLACE TABLE mydataset.partitioned_table
PARTITION BY DATE(timestamp_column)
AS SELECT * FROM mydataset.source_table;

-- Use clustering for frequently filtered columns
CREATE OR REPLACE TABLE mydataset.clustered_table
PARTITION BY DATE(timestamp_column)
CLUSTER BY user_id
AS SELECT * FROM mydataset.source_table;
```

### 2. Cost Controls

```sql
-- Set custom quotas per user/project
bq update --project_id=my-project \
  --max_bytes_billed=1000000000000 mydataset

-- Use query parameters instead of string concatenation in Python client:
query_parameters = [
  bigquery.ScalarQueryParameter("min_date", "DATE", "2023-01-01")
]
job_config = bigquery.QueryJobConfig(query_parameters=query_parameters)
query = "SELECT * FROM dataset.table WHERE date >= @min_date"
```

### 3. Capacity Planning

```python
# Flat-rate pricing example
from google.cloud import bigquery

client = bigquery.Client()
reservation = {
    "slot_capacity": 100,
    "ignore_idle_slots": False
}
path = f"projects/my-project/locations/us/reservations/my-reservation"
client.create_reservation(path, reservation)
```


### 4. Pricing Tiers

- **Standard Edition**: Basic tier
- **Enterprise Edition**: Advanced security features
- **Enterprise Plus Edition**: Maximum security and governance

## Free Tier Details

### BigQuery Free Tier Limits

- 10 GB active storage
- 1 TB queries per month
- Maximum of 100 concurrent users
- API request quotas:
  - 300 table/dataset operations per day
  - 1,500 jobs per day
  - 100 concurrent API requests

### Snowflake Trial Limits

- 30 days only
- $400 credit limit
- 1 virtual warehouse
- Storage limits based on credit usage
- All features available during trial


# Best Practices for Data Warehouse Cost Management

## Snowflake Best Practices

### 1. Data Architecture

```sql
-- Design tables for optimal query patterns
CREATE TABLE transactions (
    transaction_date DATE,
    customer_id VARCHAR,
    amount DECIMAL(18,2),
    product_id VARCHAR
)
CLUSTER BY (transaction_date, customer_id);

-- Use appropriate table types
CREATE TRANSIENT TABLE staging_data -- No history/protection for ETL
CREATE TABLE important_data -- Full protection for critical data
CREATE TEMPORARY TABLE session_analysis -- For single session work
```

### 2. Warehouse Configuration

```sql
-- Right-size for the workload
CREATE WAREHOUSE reporting_wh WITH 
    WAREHOUSE_SIZE = 'MEDIUM'
    MAX_CLUSTER_COUNT = 2
    AUTO_SUSPEND = 300 -- 5 minutes
    AUTO_RESUME = TRUE
    INITIALLY_SUSPENDED = TRUE;
    
-- Separate warehouses by workload type
CREATE WAREHOUSE etl_wh; -- For data loading
CREATE WAREHOUSE bi_wh; -- For BI tools
CREATE WAREHOUSE adhoc_wh; -- For data scientists
```

### 3. Query Performance

```sql
-- Use result cache when possible
ALTER SESSION SET USE_CACHED_RESULT = TRUE;

-- Create materialized views for common queries
CREATE MATERIALIZED VIEW daily_sales_mv AS
    SELECT date_trunc('day', transaction_date) AS day,
           sum(amount) AS total_sales
    FROM transactions
    GROUP BY 1;

-- Monitor long-running queries
SELECT query_id, user_name, warehouse_name, 
       execution_status, total_elapsed_time/1000/60 as minutes
FROM snowflake.account_usage.query_history
WHERE total_elapsed_time > 600000 -- 10+ minutes
ORDER BY total_elapsed_time DESC
LIMIT 20;
```

### 4. Resource Monitoring

```sql
-- Create resource monitors
CREATE RESOURCE MONITOR dev_monitor
  WITH CREDIT_QUOTA = 500
  TRIGGERS ON 75 PERCENT DO NOTIFY
          ON 90 PERCENT DO NOTIFY
          ON 100 PERCENT DO SUSPEND;
          
ALTER WAREHOUSE dev_warehouse SET RESOURCE_MONITOR = dev_monitor;
```

## BigQuery Best Practices

### 1. Table Design

```sql
-- Use partitioning for large tables
CREATE OR REPLACE TABLE project.dataset.events
PARTITION BY DATE(timestamp)
AS SELECT * FROM project.dataset.events_source;

-- Use clustering for common filters
CREATE OR REPLACE TABLE project.dataset.user_events
PARTITION BY DATE(event_date)
CLUSTER BY user_id, event_name
AS SELECT * FROM project.dataset.events_source;
```

### 2. Query Optimization

```sql
-- Preview query costs
SELECT COUNT(DISTINCT user_id) as user_count
FROM `project.dataset.large_table`
WHERE _PARTITIONDATE BETWEEN '2023-01-01' AND '2023-01-31'
-- Check "Query validator" in UI before running
```

- Use query parameters in application code:
```python
query = """
    SELECT * FROM `project.dataset.transactions`
    WHERE transaction_date >= @start_date
      AND amount > @min_amount
"""

job_config = bigquery.QueryJobConfig(
    query_parameters=[
        bigquery.ScalarQueryParameter("start_date", "DATE", "2023-01-01"),
        bigquery.ScalarQueryParameter("min_amount", "FLOAT", 100.0),
    ]
)
```

### 3. Scheduled Operations

```sql
-- Materialize common analytical views
CREATE OR REPLACE TABLE project.dataset.monthly_metrics
AS
SELECT 
  DATE_TRUNC(transaction_date, MONTH) as month,
  SUM(amount) as revenue,
  COUNT(DISTINCT customer_id) as customers
FROM project.dataset.transactions
GROUP BY 1;

-- Expire old data using TTL settings
CREATE OR REPLACE TABLE project.dataset.events
PARTITION BY DATE(event_date)
OPTIONS(
  partition_expiration_days=60,
  description="Events with 60-day retention"
);
```

### 4. Cost Controls

```bash
# Set custom cost controls
bq query --maximum_bytes_billed=1000000000 'SELECT * FROM huge_table'
```

```sql
-- Use authorized views for controlled access instead of duplicating data
CREATE VIEW project.dataset.limited_data AS
SELECT field1, field2
FROM project.dataset.sensitive_data
WHERE conditions_met = TRUE;
```

## Universal Best Practices

1. **Regular Audits**
   - Review query patterns and storage monthly
   - Remove unused tables/views
   - Identify expensive queries for optimization

2. **Data Lifecycle Management**
   - Define explicit retention policies
   - Archive cold data to cheaper storage tiers
   - Implement automated cleanup jobs

3. **Education & Governance**
   - Train users on cost-efficient query patterns
   - Create query templates for common analytics
   - Implement approval flows for large jobs

4. **Monitoring & Alerting**
   - Set up cost thresholds with alerts
   - Track usage trends over time
   - Create dashboards for cost visibility
