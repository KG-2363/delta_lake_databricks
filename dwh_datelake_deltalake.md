Data Warehouse --
storing structured data in rows and columns
schema on write i.e schema defined before loading the data
storage cost is high due to structured format and performance tuning
optimized for complex queries and BI tools
ex - google BigQuery, Amazon Redshift, snowflake, Teradata

Data lake --
storing raw, unstructured, structured and semi structured data like images, files etc
All data JSON, CSv, Images, Videos, Logs etc.
schema on read i.e schema is applied or enforced while reading data.
storage cost is low.
ex -AWS S3 + Athena, Azure Data Lake, Hadoop HDFS
Use Case: Data science, machine learning, storing raw logs, IoT data.

Delta lake --
Combines both benefits of dwh and data lake.- lakehouse 
Data type - both structured and unstructured data (hybrid of read/write)
schema enforcement and evolution both.
Performance - High (supports ACID transactions, indexing, caching)
Storage Cost - moderate built on top of data lakes.
Delta Lake on Databricks, Azure Synapse with Delta
Use Case: Real-time analytics, streaming + batch data, ML pipelines, data versioning







