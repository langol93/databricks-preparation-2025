# 🚀 Databricks Certification Cheat Sheet

## 📊 Spark Structured Streaming

### Basic Streaming Operations
```python
# Read from streaming source
df = spark.readStream \
    .format("delta") \
    .table("source_table")

# Write to streaming sink
query = df.writeStream \
    .format("delta") \
    .outputMode("append") \
    .option("checkpointLocation", "/path/to/checkpoint") \
    .table("target_table") \
    .start()

# Trigger options
.trigger(once=True)                    # Process once
.trigger(processingTime="10 seconds")  # Micro-batch
.trigger(availableNow=True)           # Process all available data
```

### Windowing & Watermarking
```python
from pyspark.sql.functions import window, col

# Tumbling window
windowed_df = df \
    .withWatermark("timestamp", "10 minutes") \
    .groupBy(window(col("timestamp"), "5 minutes")) \
    .count()

# Sliding window
sliding_df = df \
    .groupBy(window(col("timestamp"), "10 minutes", "5 minutes")) \
    .count()
```

### Auto Loader
```python
# Auto Loader configuration
df = spark.readStream \
    .format("cloudFiles") \
    .option("cloudFiles.format", "json") \
    .option("cloudFiles.schemaLocation", "/path/to/schema") \
    .load("/path/to/source")
```

---

## 🏗️ Delta Live Tables (DLT)

### Basic DLT Syntax
```python
import dlt
from pyspark.sql.functions import *

# Bronze table (streaming)
@dlt.table(
    comment="Raw data ingestion",
    table_properties={"quality": "bronze"}
)
def bronze_table():
    return spark.readStream.format("cloudFiles") \
        .option("cloudFiles.format", "json") \
        .load("/path/to/raw/data")

# Silver table with expectations
@dlt.table(
    comment="Cleaned and validated data"
)
@dlt.expect_or_drop("valid_timestamp", "timestamp IS NOT NULL")
@dlt.expect("valid_id", "id > 0")
def silver_table():
    return dlt.read_stream("bronze_table") \
        .select("id", "timestamp", "value") \
        .filter(col("value").isNotNull())

# Gold table (aggregated)
@dlt.table
def gold_table():
    return dlt.read("silver_table") \
        .groupBy("id") \
        .agg(sum("value").alias("total_value"))
```

### DLT Expectations
```python
# Data quality expectations
@dlt.expect("valid_email", "email RLIKE '^[^@]+@[^@]+\\.[^@]+$'")
@dlt.expect_or_drop("not_null_id", "id IS NOT NULL")
@dlt.expect_or_fail("critical_check", "amount > 0")
@dlt.expect_all({"valid_date": "date >= '2020-01-01'", 
                 "valid_amount": "amount BETWEEN 0 AND 1000000"})
```

---

## 🔒 Unity Catalog & Security

### Catalog Management
```sql
-- Create catalog hierarchy
CREATE CATALOG IF NOT EXISTS my_catalog;
CREATE SCHEMA IF NOT EXISTS my_catalog.my_schema;

-- Grant permissions
GRANT USE CATALOG ON CATALOG my_catalog TO `user@company.com`;
GRANT CREATE TABLE ON SCHEMA my_catalog.my_schema TO `data_engineers`;
GRANT SELECT ON TABLE my_catalog.my_schema.my_table TO `analysts`;

-- Show grants
SHOW GRANTS ON CATALOG my_catalog;
SHOW GRANTS ON TABLE my_catalog.my_schema.my_table;
```

### PII Data Handling
```sql
-- Dynamic view for data masking
CREATE VIEW masked_users AS
SELECT 
    user_id,
    CASE 
        WHEN is_member('sensitive_data_access') 
        THEN email 
        ELSE 'xxx@xxx.com' 
    END AS email,
    CASE 
        WHEN is_member('pii_access') 
        THEN phone 
        ELSE 'XXX-XXX-XXXX' 
    END AS phone
FROM users;

-- Row-level security
CREATE VIEW secure_data AS
SELECT * FROM sensitive_table
WHERE department = current_user() OR is_member('admin');
```

### Change Data Feed (CDF)
```sql
-- Enable CDF on table
ALTER TABLE my_table SET TBLPROPERTIES (delta.enableChangeDataFeed = true);

-- Read CDF
SELECT * FROM table_changes('my_table', 2, 5);  -- versions 2 to 5
SELECT * FROM table_changes('my_table', '2023-01-01', '2023-01-02');  -- timestamp range

-- CDF columns: _change_type, _commit_version, _commit_timestamp
```

---

## ⚡ Performance Optimization

### File Optimization
```sql
-- Optimize table (compaction)
OPTIMIZE my_table;

-- Z-order optimization
OPTIMIZE my_table ZORDER BY (column1, column2);

-- Vacuum old files
VACUUM my_table RETAIN 168 HOURS;  -- 7 days

-- Liquid Clustering (new feature)
CREATE TABLE clustered_table (
    id BIGINT,
    date DATE,
    category STRING
) USING DELTA
CLUSTER BY (date, category);
```

### Join Optimization
```python
# Broadcast join (for small tables)
from pyspark.sql.functions import broadcast
result = large_df.join(broadcast(small_df), "key")

# Bucketed tables
large_df.write \
    .bucketBy(10, "key") \
    .saveAsTable("bucketed_table")

# Partition pruning
df.filter(col("date") >= "2023-01-01").filter(col("date") < "2023-02-01")
```

### Caching Strategies
```python
# Cache DataFrame
df.cache()
df.persist(StorageLevel.MEMORY_AND_DISK)

# Cache table
spark.sql("CACHE TABLE my_table")

# Uncache
df.unpersist()
spark.sql("UNCACHE TABLE my_table")
```

---

## 🚀 Databricks Asset Bundles (DABs)

### Basic DAB Configuration (databricks.yml)
```yaml
bundle:
  name: my_project
  
variables:
  catalog_name:
    default: dev_catalog
    
targets:
  dev:
    variables:
      catalog_name: dev_catalog
    workspace:
      host: https://dev.cloud.databricks.com
      
  prod:
    variables:
      catalog_name: prod_catalog
    workspace:
      host: https://prod.cloud.databricks.com

resources:
  jobs:
    my_job:
      name: "My ETL Job - ${var.catalog_name}"
      tasks:
        - task_key: "etl_task"
          notebook_task:
            notebook_path: "./src/etl_notebook"
          job_cluster_key: "main_cluster"
      job_clusters:
        - job_cluster_key: "main_cluster"
          new_cluster:
            spark_version: "13.3.x-scala2.12"
            node_type_id: "i3.xlarge"
            num_workers: 2

  pipelines:
    my_pipeline:
      name: "My DLT Pipeline - ${var.catalog_name}"
      catalog: ${var.catalog_name}
      target: my_schema
      libraries:
        - notebook:
            path: "./src/dlt_pipeline"
```

### DAB Commands
```bash
# Initialize new DAB project
databricks bundle init

# Validate configuration
databricks bundle validate -t dev

# Deploy to target environment
databricks bundle deploy -t dev

# Run deployed job
databricks bundle run my_job -t dev

# Destroy resources
databricks bundle destroy -t dev
```

---

## 🔧 Common SQL Functions & Operations

### Date/Time Functions
```sql
-- Current timestamp
SELECT current_timestamp(), current_date();

-- Date arithmetic
SELECT date_add(current_date(), 30);
SELECT date_sub(current_date(), 7);

-- Date formatting
SELECT date_format(current_timestamp(), 'yyyy-MM-dd HH:mm:ss');

-- Extract date parts
SELECT year(current_date()), month(current_date()), dayofweek(current_date());
```

### String Functions
```sql
-- String manipulation
SELECT concat('Hello', ' ', 'World');
SELECT substring('Databricks', 1, 4);  -- 'Data'
SELECT regexp_replace('abc123def', '[0-9]+', 'XXX');  -- 'abcXXXdef'

-- JSON functions
SELECT get_json_object('{"name":"John","age":30}', '$.name');
SELECT from_json('{"a":1,"b":2}', 'a INT, b INT');
```

### Array/Map Functions
```sql
-- Array operations
SELECT array(1, 2, 3, 4);
SELECT array_contains(array(1, 2, 3), 2);  -- true
SELECT explode(array('a', 'b', 'c'));

-- Map operations
SELECT map('key1', 'value1', 'key2', 'value2');
SELECT map_keys(map('a', 1, 'b', 2));  -- ['a', 'b']
```

---

## 🎯 Exam-Specific Tips

### Common Patterns to Remember

#### Streaming Query Structure
```python
# Standard streaming pattern
query = spark.readStream \
    .format("source_format") \
    .option("key", "value") \
    .load("path") \
    .transform(lambda df: df.select(...)) \
    .writeStream \
    .format("sink_format") \
    .outputMode("append|complete|update") \
    .option("checkpointLocation", "path") \
    .trigger(processingTime="interval") \
    .start()
```

#### Error Handling in Streaming
```python
# Handle bad records
df = spark.readStream \
    .format("json") \
    .option("badRecordsPath", "/path/to/bad/records") \
    .option("multiline", "true") \
    .load("/path/to/data")
```

#### Performance Monitoring
```python
# Check streaming query status
query.status
query.lastProgress
query.recentProgress

# Spark UI analysis points
# - Look for shuffle operations
# - Check task distribution
# - Monitor memory usage
# - Identify slow stages
```

---

## 📚 Quick Reference Tables

### Output Modes
| Mode | Description | Use Case |
|------|-------------|----------|
| append | Only new rows | Most streaming scenarios |
| complete | Entire result table | Aggregations without watermark |
| update | Changed rows only | Aggregations with watermark |

### Trigger Types
| Trigger | Description | Use Case |
|---------|-------------|----------|
| processingTime | Fixed interval | Regular batch processing |
| once | Single execution | One-time processing |
| availableNow | Process all available | Catch-up processing |

### DLT Expectation Actions
| Function | Action | Description |
|----------|--------|-------------|
| expect | Log violation | Continue processing |
| expect_or_drop | Drop bad records | Remove invalid data |
| expect_or_fail | Fail pipeline | Stop on violation |

---

## 🚨 Common Pitfalls to Avoid

### Streaming
- ❌ Forgetting checkpoint location
- ❌ Using complete mode with large datasets
- ❌ Not handling late data with watermarks
- ❌ Mixing batch and streaming operations incorrectly

### Performance
- ❌ Too many small files
- ❌ Unnecessary shuffles
- ❌ Over-partitioning data
- ❌ Not using appropriate join strategies

### Security
- ❌ Overly permissive access controls
- ❌ Not masking PII in views
- ❌ Forgetting to enable audit logging
- ❌ Hardcoding credentials in code

### Deployment
- ❌ Not using environment-specific configurations
- ❌ Missing dependency management
- ❌ Inadequate testing before deployment
- ❌ Not implementing proper CI/CD practices

---

*Keep this cheat sheet handy during your exam preparation and review! 📖✨*