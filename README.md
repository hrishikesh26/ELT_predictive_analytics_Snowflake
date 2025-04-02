# Snowflake ELT & ML Automation with Snowpark
This project delivers a comprehensive, automated data processing and predictive analytics framework built within Snowflake using Snowpark for Python. It begins with SQL scripts that set up your environment creating warehouses, databases, schemas, and tables, as well as defining file formats and stages for seamless AWS S3 data ingestion. Once data is loaded into Snowflake, ELT transformations are executed via Snowpark’s DataFrame API, aggregating and preparing the data for analysis. The project further automates data pipelines through stored procedures and DAG-based task scheduling, ensuring continuous updates. A dedicated Snowpark ML workflow trains, evaluates, and manages predictive models using Snowflake’s Model Registry, while a Streamlit dashboard offers real-time insights and interactive visualization, making this solution ideal for data engineers, data analysts, and analytics engineers.

## Technologies

- **Snowflake:** Cloud data warehouse for storage and processing.
- **Snowpark for Python:** API for in-database ELT and ML workflows.
- **DAG (Directed Acyclic Graph):** Defines and visualizes task dependencies to automate and schedule data pipeline workflows.
- **AWS S3:** Source for raw data ingestion.
- **SQL:** Scripting for environment setup and data ingestion.
- **Python:** Used for data transformations and ML tasks.
- **Streamlit:** Framework for building interactive dashboards.
- **Tasks & Stored Procedures:** Automate and schedule data pipelines.
- **Model Registry:** Manage and version ML models.
