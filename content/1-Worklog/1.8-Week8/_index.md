---
title: "Week 8 Worklog"
date: 2026-05-31
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
### Week 8 Objectives:
* Get familiar with Data Engineering on AWS: Glue, Athena, QuickSight.
* Process real-time data with Amazon Kinesis and MSK (Kafka).
* Build a data pipeline from ingestion → processing → analytics → visualization.
* Practice real data analysis scenarios and optimise query costs.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                        | Start Date | Completion Date | Reference Material                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Overview of Data Engineering on AWS: <br>&emsp; + Data Lake vs Data Warehouse vs Data Lakehouse <br>&emsp; + Batch vs Stream processing <br>&emsp; + Lambda vs Kappa architecture <br> - Learn AWS Glue: <br>&emsp; + Data Catalog <br>&emsp; + Crawler (auto schema) <br>&emsp; + ETL Job (PySpark) <br>&emsp; + Glue Studio (visual ETL) <br>&emsp; + Glue DataBrew (no-code data prep)                                          | 06/01/2026 | 06/01/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Amazon Athena: <br>&emsp; + Serverless query engine on S3 <br>&emsp; + Formats: Parquet, ORC, Avro, JSON, CSV <br>&emsp; + Partitioning & Compression <br>&emsp; + Federated Query (RDS, DynamoDB) <br>&emsp; + Query Result Location <br> - **Practice:** Build a Data Catalog for CSVs on S3 and write SQL for monthly revenue analysis and top 10 best-selling products                                                                                                                                                  | 06/02/2026 | 06/02/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Amazon Kinesis: <br>&emsp; + Kinesis Data Streams (low-level, custom consumer) <br>&emsp; + Kinesis Data Firehose (managed delivery → S3/Redshift/OpenSearch) <br>&emsp; + Kinesis Data Analytics (SQL tumbling window) <br> - Learn Amazon MSK (Managed Streaming for Apache Kafka): <br>&emsp; + Cluster, broker, topic <br>&emsp; + Multi-AZ replication <br> - **Practice:** Simulate IoT log stream → Firehose → S3 (Parquet) → Athena                                          | 06/03/2026 | 06/03/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn Amazon QuickSight: <br>&emsp; + Dataset, Analysis, Dashboard <br>&emsp; + Sources: Athena, RDS, S3, Redshift <br>&emsp; + Calculated field, filter, parameter, KPI <br>&emsp; + ML Insights (anomaly detection, forecasting) <br>&emsp; + Embedded Analytics <br> - **Practice:** Build dashboards for revenue analysis by region, time, and product group; share with the team                                                                                                                | 06/04/2026 | 06/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 8 capstone lab:** <br>&emsp; + Pipeline: Kinesis Firehose → S3 (raw JSON) → Glue ETL (clean) → S3 (Parquet partition) → Athena → QuickSight <br>&emsp; + Sample data: real-time sales log <br>&emsp; + Wrap up & update worklog; write blog "Data Pipeline on AWS"                                                                                                                                                                          | 06/05/2026 | 06/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 8 Achievements:
* Mastered key Data Engineering concepts:
  * Distinguished Data Lake (S3), Data Warehouse (Redshift), Data Lakehouse (S3 + Delta Lake)
  * Understood batch processing (Glue, EMR) vs stream processing (Kinesis, MSK)
  * Compared Lambda architecture (batch + speed layer) vs Kappa (stream only)

* Mastered AWS Glue:
  * Used Crawler to auto-generate schema for data on S3
  * Wrote ETL Jobs in PySpark to clean & normalise data (drop nulls, cast types, rename columns)
  * Stored metadata in the Glue Data Catalog (queryable from Athena, Redshift Spectrum)
  * Glue Studio for visual ETL design
  * Glue DataBrew for code-free data preparation

* Queried data with Athena:
  * Ran SQL on Parquet/CSV files on S3 without ETL
  * Partition (year/month/day) + Parquet format reduced cost by 70-90%
  * Used CTAS to pre-create aggregated tables
  * Athena Workgroups separating cost between teams
  * Federated Query connecting directly to RDS PostgreSQL/MySQL

* Streamed data with Kinesis:
  * **Kinesis Data Streams:** consumed real-time events with custom consumers (Lambda, KCL)
  * **Kinesis Data Firehose:** buffered (60s or 128 MB) then delivered to S3/Redshift/OpenSearch
  * **Kinesis Data Analytics:** ran SQL tumbling-window queries on streams
  * Compared with MSK: choose Kinesis when fully-managed is needed; MSK when already on Kafka

* Visualised data with QuickSight:
  * Datasets from Athena (relational datasets, SPICE in-memory datasets)
  * Dashboard for revenue, top products, and churn rate
  * Drag-and-drop for calculated fields, filters, and parameters
  * ML Insights for anomaly detection and time-series forecasting
  * Embedded Analytics for dashboards inside other web apps

* Completed a complete data pipeline:
  * **Ingestion:** Kinesis Firehose collected real-time logs
  * **Storage:** S3 stored raw JSON (partitioned by year/month/day)
  * **Processing:** Glue ETL cleaned and converted to Parquet
  * **Querying:** Athena ran serverless SQL
  * **Visualization:** QuickSight dashboard shared with the team

* Wrote a sharing blog on AWS data pipelines for the community.
* New habits formed:
  * Every S3 dataset is partitioned by date to optimise query cost
  * Glue Jobs are scheduled (cron) via EventBridge
  * Tag datasets with `DataClass = Confidential/Public` for access control
  * Track Athena costs through Cost Explorer and set dedicated budgets
* ...