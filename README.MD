FlexiMart Data Architecture Project
Student Name: RITESH KUMAR Student ID: bitsom_ba_25071329 Email: riteshkumar10205@gmail.com Date: 08/01/2026

Project Overview
The FlexiMart Data Architecture Project implements an end-to-end design and implementation of a modern data system, transitioning from an operational RDBMS to a high-performance Data Warehouse. It features normalized MySQL database for transactional integrity, ETL pipeline for data migration, a MongoDB NoSQL catalog for flexible product data and a Star Schema Data Warehouse for complex OLAP analytical querying.

Repository Structure
├── part1-database-etl/ │ ├── etl_pipeline.py │ ├── schema_documentation.md │ ├── business_queries.sql │ └── data_quality_report.txt ├── part2-nosql/ │ ├── nosql_analysis.md │ ├── mongodb_operations.js │ └── products_catalog.json ├── part3-datawarehouse/ │ ├── star_schema_design.md │ ├── warehouse_schema.sql │ ├── warehouse_data.sql │ └── analytics_queries.sql └── README.md

Technologies Used
Python 3.14.0: (Pandas, MySQL-Connector) for ETL processing.
MySQL 8.0: Host for both Operational (OLTP) and Warehouse (OLAP) environments.
MongoDB 6.0: NoSQL document store for flexible product attributes.
Markdown: For technical documentation and schema design reports.
Setup Instructions
Database Setup
# Create databases
mysql -u root -p -e "CREATE DATABASE fleximart;"
mysql -u root -p -e "CREATE DATABASE fleximart_dw;"

# Run Part 1 - ETL Pipeline
python part1-database-etl/etl_pipeline.py

# Run Part 1 - Business Queries
mysql -u root -p fleximart < part1-database-etl/business_queries.sql

# Run Part 3 - Data Warehouse
mysql -u root -p fleximart_dw < part3-datawarehouse/warehouse_schema.sql
mysql -u root -p fleximart_dw < part3-datawarehouse/warehouse_data.sql
mysql -u root -p fleximart_dw < part3-datawarehouse/analytics_queries.sql

### MongoDB Setup

```bash
mongosh < part2-nosql/mongodb_operations.js
Key Learnings
Through this project, I gained hands-on experience in building ETL pipeline, learning how to handle data quality issues like missing values and duplicates using Python. I learned how NoSQL databases like MongoDB handle schema flexibility more effectively than traditional RDBMS. I designed a Star Schema that optimizes heavy analytical workloads compared to normalized transactional stuctures.

Challenges Faced
Data Quality Issues in the source: The raw files contained inconsistent and missing data like inconsistent date formats and duplicate records. Solution: I used Pandas to standardize the date formats and also performed deduplication in the python ETL pipeline during the transformation stage.

Database connectivity and credential issues: Faced issues while pushing the transformed data from the ETL pipeline to MySQL database. The system threw errors with the credentials while connecting to the database. Solution: Resolved this by validating database permissions and using SQLAlchemy for creating an engine and stable connection.