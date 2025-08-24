# 🎯 Databricks Certification Practice Exam

## 📋 Instructions
- **Time Limit**: 90 minutes for 60 questions
- **Passing Score**: 70% (42 correct answers)
- **Question Types**: Multiple choice, multiple select, scenario-based
- **No external resources** during practice

---

## 🔥 Practice Questions

### Module 1: Streaming & Data Engineering (15 questions)

#### Question 1
Which trigger option should you use for a streaming query that needs to process all available data once and then stop?

A) `.trigger(processingTime="0 seconds")`
B) `.trigger(once=True)`
C) `.trigger(availableNow=True)`
D) `.trigger(continuous="1 second")`

<details>
<summary>Answer</summary>
<b>C) `.trigger(availableNow=True)`</b><br>
availableNow processes all available data in the source and then stops, making it ideal for catch-up scenarios.
</details>

#### Question 2
In Delta Live Tables, which expectation function will cause the pipeline to fail if the condition is not met?

A) `@dlt.expect()`
B) `@dlt.expect_or_drop()`
C) `@dlt.expect_or_fail()`
D) `@dlt.expect_all()`

<details>
<summary>Answer</summary>
<b>C) `@dlt.expect_or_fail()`</b><br>
expect_or_fail will terminate the pipeline execution if the expectation is violated.
</details>

#### Question 3
What is the correct way to enable watermarking in a streaming DataFrame?

A) `.watermark("timestamp", "10 minutes")`
B) `.withWatermark("timestamp", "10 minutes")`
C) `.setWatermark("timestamp", "10 minutes")`
D) `.enableWatermark("timestamp", "10 minutes")`

<details>
<summary>Answer</summary>
<b>B) `.withWatermark("timestamp", "10 minutes")`</b><br>
withWatermark is the correct method to enable watermarking on a timestamp column.
</details>

#### Question 4
Which Auto Loader option specifies the location where schema evolution information is stored?

A) `cloudFiles.schemaLocation`
B) `cloudFiles.schemaPath`
C) `cloudFiles.schemaEvolution`
D) `cloudFiles.metadataPath`

<details>
<summary>Answer</summary>
<b>A) `cloudFiles.schemaLocation`</b><br>
schemaLocation is where Auto Loader stores schema inference and evolution metadata.
</details>

#### Question 5
In the medallion architecture, what is the typical data quality level of the Bronze layer?

A) Highly curated and business-ready
B) Raw data with minimal processing
C) Aggregated and summarized data
D) Real-time streaming data only

<details>
<summary>Answer</summary>
<b>B) Raw data with minimal processing</b><br>
Bronze layer contains raw, unprocessed data as ingested from source systems.
</details>

---

### Module 2: Data Privacy & Security (15 questions)

#### Question 6
Which Unity Catalog object provides the highest level of data organization?

A) Schema
B) Table
C) Catalog
D) Metastore

<details>
<summary>Answer</summary>
<b>D) Metastore</b><br>
Metastore is the top-level container that holds multiple catalogs in Unity Catalog hierarchy.
</details>

#### Question 7
What SQL function can be used to check if the current user belongs to a specific group?

A) `current_user()`
B) `is_member('group_name')`
C) `user_in_group('group_name')`
D) `check_membership('group_name')`

<details>
<summary>Answer</summary>
<b>B) `is_member('group_name')`</b><br>
is_member() function checks if the current user is a member of the specified group.
</details>

#### Question 8
To enable Change Data Feed on an existing Delta table, which command should you use?

A) `ALTER TABLE table_name ENABLE CHANGE DATA FEED`
B) `ALTER TABLE table_name SET TBLPROPERTIES (delta.enableChangeDataFeed = true)`
C) `CREATE TABLE table_name WITH CHANGE DATA FEED`
D) `UPDATE TABLE table_name SET changeDataFeed = true`

<details>
<summary>Answer</summary>
<b>B) `ALTER TABLE table_name SET TBLPROPERTIES (delta.enableChangeDataFeed = true)`</b><br>
This is the correct syntax to enable CDF on an existing table.
</details>

#### Question 9
Which CDF column indicates the type of change operation performed?

A) `_change_version`
B) `_change_type`
C) `_change_operation`
D) `_change_action`

<details>
<summary>Answer</summary>
<b>B) `_change_type`</b><br>
_change_type column shows whether the operation was insert, update, or delete.
</details>

#### Question 10
What is the difference between pseudonymization and anonymization?

A) No difference, they are the same
B) Pseudonymization is reversible, anonymization is not
C) Anonymization is reversible, pseudonymization is not
D) Both are always reversible

<details>
<summary>Answer</summary>
<b>B) Pseudonymization is reversible, anonymization is not</b><br>
Pseudonymization replaces identifiers with pseudonyms that can be reversed, while anonymization permanently removes identifying information.
</details>

---

### Module 3: Performance Optimization (15 questions)

#### Question 11
What is the primary cause of the "small file problem" in Delta Lake?

A) Too few partitions
B) Frequent small writes without compaction
C) Large cluster size
D) Insufficient memory

<details>
<summary>Answer</summary>
<b>B) Frequent small writes without compaction</b><br>
Many small writes create numerous small files, which degrades read performance and should be addressed with OPTIMIZE.
</details>

#### Question 12
Which command is used to compact small files in a Delta table?

A) `COMPACT table_name`
B) `OPTIMIZE table_name`
C) `MERGE table_name`
D) `CONSOLIDATE table_name`

<details>
<summary>Answer</summary>
<b>B) `OPTIMIZE table_name`</b><br>
OPTIMIZE command compacts small files and can also perform Z-ordering for better data layout.
</details>

#### Question 13
What is the purpose of Z-ordering in Delta Lake?

A) Alphabetical sorting of data
B) Chronological ordering by timestamp
C) Co-locating related data for better data skipping
D) Reducing file sizes

<details>
<summary>Answer</summary>
<b>C) Co-locating related data for better data skipping</b><br>
Z-ordering co-locates related information in the same set of files, improving data skipping effectiveness.
</details>

#### Question 14
When should you use a broadcast join?

A) When both tables are very large
B) When one table is small enough to fit in memory
C) When tables have many partitions
D) When performing outer joins only

<details>
<summary>Answer</summary>
<b>B) When one table is small enough to fit in memory</b><br>
Broadcast joins are efficient when one table is small enough to be broadcasted to all executor nodes.
</details>

#### Question 15
What does the VACUUM command do?

A) Compacts small files
B) Removes old file versions beyond retention period
C) Optimizes table statistics
D) Rebuilds table indexes

<details>
<summary>Answer</summary>
<b>B) Removes old file versions beyond retention period</b><br>
VACUUM removes old data files that are no longer referenced by the table, freeing up storage space.
</details>

---

### Module 4: Deployment & DevOps (15 questions)

#### Question 16
What is the main configuration file for Databricks Asset Bundles?

A) `config.yml`
B) `databricks.yml`
C) `bundle.yaml`
D) `deployment.yml`

<details>
<summary>Answer</summary>
<b>B) `databricks.yml`</b><br>
databricks.yml is the main configuration file that defines the DAB structure and resources.
</details>

#### Question 17
Which DAB command is used to deploy resources to a target environment?

A) `databricks bundle push -t dev`
B) `databricks bundle deploy -t dev`
C) `databricks bundle publish -t dev`
D) `databricks bundle release -t dev`

<details>
<summary>Answer</summary>
<b>B) `databricks bundle deploy -t dev`</b><br>
The deploy command deploys the bundle resources to the specified target environment.
</details>

#### Question 18
In a DAB configuration, what section defines environment-specific settings?

A) `environments`
B) `targets`
C) `configs`
D) `deployments`

<details>
<summary>Answer</summary>
<b>B) `targets`</b><br>
The targets section defines different deployment environments like dev, staging, and prod.
</details>

#### Question 19
What is the purpose of variables in DAB configurations?

A) To store sensitive credentials
B) To enable environment-specific customization
C) To define resource dependencies
D) To configure cluster settings only

<details>
<summary>Answer</summary>
<b>B) To enable environment-specific customization</b><br>
Variables allow the same bundle to be deployed to different environments with different configurations.
</details>

#### Question 20
Which CI/CD practice is recommended for DAB deployments?

A) Deploy directly from local machine
B) Use automated testing and deployment pipelines
C) Manual deployment to production
D) Deploy all environments simultaneously

<details>
<summary>Answer</summary>
<b>B) Use automated testing and deployment pipelines</b><br>
Automated CI/CD pipelines ensure consistent, tested deployments across environments.
</details>

---

## 🧠 Scenario-Based Questions

### Scenario 1: Streaming Pipeline Design
You need to build a real-time analytics pipeline that:
- Ingests JSON files from cloud storage
- Handles schema evolution automatically
- Processes data in 5-minute windows
- Handles late data up to 10 minutes
- Writes results to a Delta table

#### Question 21
Which combination of technologies would you use?

A) Auto Loader + Structured Streaming + Delta Lake
B) COPY INTO + Batch processing + Parquet
C) File notification + Spark SQL + CSV
D) Manual file monitoring + Pandas + JSON

<details>
<summary>Answer</summary>
<b>A) Auto Loader + Structured Streaming + Delta Lake</b><br>
This combination provides automatic schema evolution, real-time processing, and ACID transactions.
</details>

#### Question 22
What would be the correct watermark configuration?

A) `.withWatermark("timestamp", "5 minutes")`
B) `.withWatermark("timestamp", "10 minutes")`
C) `.withWatermark("timestamp", "15 minutes")`
D) No watermark needed

<details>
<summary>Answer</summary>
<b>B) `.withWatermark("timestamp", "10 minutes")`</b><br>
Watermark should match the maximum expected delay for late data (10 minutes in this case).
</details>

### Scenario 2: Performance Optimization
A Delta table has performance issues:
- 10,000 small files (average 1MB each)
- Queries frequently filter by date and customer_id
- Table size is 500GB
- Read queries are slow

#### Question 23
What optimization strategy would be most effective?

A) Increase cluster size only
B) OPTIMIZE with Z-ORDER BY (date, customer_id)
C) Partition by customer_id
D) Convert to Parquet format

<details>
<summary>Answer</summary>
<b>B) OPTIMIZE with Z-ORDER BY (date, customer_id)</b><br>
This addresses both the small file problem and optimizes data layout for the common filter columns.
</details>

#### Question 24
After optimization, what maintenance command should be run?

A) `ANALYZE TABLE`
B) `REFRESH TABLE`
C) `VACUUM`
D) `MSCK REPAIR TABLE`

<details>
<summary>Answer</summary>
<b>C) `VACUUM`</b><br>
VACUUM removes old file versions created during optimization, freeing up storage space.
</details>

---

## 📊 Scoring Guide

### Score Calculation
- **24/24 (100%)**: Excellent - Ready for exam
- **21-23 (87-95%)**: Very Good - Minor review needed
- **18-20 (75-83%)**: Good - Focus on weak areas
- **15-17 (62-74%)**: Fair - Significant study needed
- **<15 (<62%)**: Poor - Extensive preparation required

### Areas to Focus Based on Score
- **Streaming**: Questions 1-5, 21-22
- **Security**: Questions 6-10
- **Performance**: Questions 11-15, 23-24
- **Deployment**: Questions 16-20

---

## 🎯 Next Steps Based on Performance

### If you scored 85%+
✅ Take a full practice exam
✅ Review any missed concepts
✅ Schedule your certification exam
✅ Do final review of cheat sheet

### If you scored 70-84%
📚 Focus on modules where you missed questions
📚 Redo relevant labs and exercises
📚 Take this practice exam again in 3 days
📚 Review specific documentation for weak areas

### If you scored <70%
🔄 Return to structured study plan
🔄 Complete all hands-on labs
🔄 Focus on fundamental concepts
🔄 Retake practice exam weekly until improvement

---

## 💡 Exam Day Tips

### Before the Exam
- [ ] Get good sleep (7-8 hours)
- [ ] Eat a proper breakfast
- [ ] Review cheat sheet one final time
- [ ] Arrive early or set up home environment

### During the Exam
- [ ] Read questions carefully
- [ ] Eliminate obviously wrong answers first
- [ ] Don't spend too much time on difficult questions
- [ ] Review flagged questions if time permits
- [ ] Trust your preparation and instincts

### Time Management
- **60 questions in 90 minutes = 1.5 minutes per question**
- **First pass**: Answer easy questions quickly (45 minutes)
- **Second pass**: Tackle medium difficulty questions (30 minutes)
- **Final pass**: Address difficult questions and review (15 minutes)

---

*Good luck with your practice and the real exam! You've got this! 🚀*