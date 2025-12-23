# Data Warehouse --

storing structured data in rows and columns
schema on write i.e schema defined before loading the data
storage cost is high due to structured format and performance tuning
schema inflexible
optimized for complex queries and BI tools
ex - google BigQuery, Amazon Redshift, snowflake, Teradata


# Data lake --

storing raw, unstructured, structured and semi structured data like images, files etc
All data JSON, CSv, Images, Videos, Logs etc.
schema on read i.e schema is applied or enforced while reading data.
storage cost is low.
ex -AWS S3 + Athena, Azure Data Lake, Hadoop HDFS
Use Case: Data science, machine learning, storing raw logs, IoT data.

Data lake Challenges --
No transactional guarantees
no schema enforcement
performance issues with many small files 
no built in data quality controls
difficult metadata management

Basic data repr - JSON, CSV -- simple format, no transactional, schema enforcement, proper metadata mgt.
efficient encoding - Parquet, ORC -- better compression and storage but lacked transaction support
Tabular transactional storage - Delta lake, Apache Iceberg -- adds transaction support, schema checks and time travel features.




# Delta lake --

An open source storage format bringing ACID transactions to the data lake.
created by databricks 
Built on top of Parquet,allows fine grained updates, maintains history of all changes ( making time travel and rollback possible)
Unified batch and streaming
scalable metadata handling
Combines both benefits of dwh and data lake.- lakehouse 
Data type - both structured and unstructured data (hybrid of read/write)
schema enforcement and evolution both.
Performance - High (supports ACID transactions, indexing, caching)
Storage Cost - moderate built on top of data lakes.
Delta Lake on Databricks, Azure Synapse with Delta
Use Case: Real-time analytics, streaming + batch data, ML pipelines, data versioning

# Medallion Architecture --

multi layered data architecture -

Bronze -- Like for like with source copy with history (the data lake)
          It stores data exactly as it arrives for full traceability

Silver -- Cleaned and conformed ( but not aggregated data)
          cleaned, deduplicated and enriched but without changing its level of details - no aggregations are done here

Gold   -- Business level features and aggregates
          KPIs, dashboards, features are applied


# Unity Catalog --

single control plane for all data assets
fine grained access control (tables, columns, rows )
automated data lineage tracking
centralized auditing and compliance 


A control plane is the centralized system responsible for managing and orchestrating configurations and policies.

Unity Catalog acts as a single control plane for :
    managing data permissions (table, column, row level access)
    handling lineage, auditing and compliance
    sharing data assets across multiple workspaces

Understanding data lake versioning --

Each INSERT, UPDATE, DELETE operation 
   computes a "delta" from the current version
     as files added or removed
   creates new data files (if necessary)
   commits a new version to the delta log
     becomes the latest or current version

Each SELECT Version
   gets the manifest of data files from the version in the _delta_log
   returns the data from the amalgam of files in the version


Delta Lake tracks changes through a _delta_log folder in each table's directory
Every data update creates a new log version, recording only the files added or removed

DELTA LAKE OPERATIONS --

Fine grained DELETE and UPDATE operations
 (row level modifications)

MERGE operations



  
  

# MERGE 
  Fine grained DELETE and UPDATE operations
  (row level modifications)
  
  
# SCHEMA EVOLUTION
 Including the create table or replace table operation

# TIME TRAVEL 
 Ability to select previous versions of data
 The delta lake transaction log allows you to make row level updates and deletes without rewriting the full dataset.
 Schema evolution is supported - We can Create or replace schema and still access the previous schema through previous versions. The log enables time travel and let us restore data from a specific version or a timestamp for easy recovery or auditing.


 spark.read.format("delta").load("table_path") -reads from delta lake tables
 df.write.format("delta").mode("append").save()

 DELTA LAke is the default for databricks --
 spark.read(table_path)
 df.write.mode(append).save(table_path)


 DESCRIBE HISTORY delta_orders -- shows version history of a delta table 
 DESCRIBE DETAIL delta_orders -- info about the table itself
   gives specific details about the table, also has numFiles col which shows 1 -- we will explore in OPTIMIZE command
   
 
 OPTIMIZE & VACCUM help manage file sizes and storage, improving performance and keep table tidy.


 # DELTA LAKE performace and optimisation 

 
# OPTIMIZE  -- table optimization
compact small files and index columns 
spark.sql("OPTIMIZE my_delta_table ZORDER by date")


# VACCUM -- log cleaning operations
spark.sql("VACCUM my_delta_table RETAIN 168 hours")


ALTER TABLE my_table SET TBLPROPERTIES(
'delta.autoOptimize.optimizeWrite' = "true"
'delta.autoOptimize.autoCompact' = "true"

# APACHE ICEBERG 

open table format created by NETFLIX
similiar goals to delta lake 

ACID transaction
schema evolution
Hidden partitioning and partition evolution
Branch/tag support 

spark.read.format("iceberg").load(my_table)

df.writeTo("my_table").append()

Hidden Partitioning --
 Iceberg abstracts partition details from users
 partition transforms(like dates into months) happen behind the scenes 
 users query natural columns, iceberg handles partition pruning automatically
 can change partition scheme without changing queries

 Partition Evolution --
  can change how a table is partitioned without rewriting data
  e.g change from daily to monthly partitions seamlessly
  Old and new partition schemes can coexist
  Queries automatically uses the most efficient partition scheme
  enables gradual migration of partitioning strategy 



















    










         










