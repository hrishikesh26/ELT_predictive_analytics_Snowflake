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

## Project Workflow

1. **Data Ingestion:**  
   - Execute SQL scripts to initialize the Snowflake environment (create warehouse, database, schema, and tables).  
   - Configure file formats and stages to ingest raw CSV data from AWS S3 into Snowflake tables.

2. **Data Transformation:**  
   - Leverage Snowpark for Python to perform in-database ELT operations.  
   - Utilize DataFrame APIs for aggregations, pivoting, and complex data transformations to generate analytical datasets.

3. **Pipeline Orchestration:**  
   - Automate sequential execution using Snowflake Tasks arranged in a Directed Acyclic Graph (DAG).  
   - Use stored procedures to encapsulate transformation logic, ensuring dependency management and scheduling of ETL/ELT workflows.

4. **Machine Learning:**  
   - Execute ML workflows directly within Snowflake notebooks using Snowpark ML.  
   - Preprocess features, split datasets, and train a linear regression model, then log and manage model versions via the Snowflake Model Registry.

5. **Visualization & Inference:**  
   - Deploy a Streamlit dashboard for real-time inference and interactive data visualization.  
   - Integrate predictive analytics to simulate marketing budget scenarios and generate revenue predictions.


## Executing the Notebooks and Ingestion SQL Scripts

The Snowflake notebooks in this project are designed to run within the Snowflake environment, leveraging Snowpark for Python. To get started, execute the following SQL code in a new SQL worksheet in Snowsight. This code sets up your data warehouse, creates the necessary tables, and ingests data from Amazon S3.

### 1. Setup Environment & Data Ingestion

**Create Warehouse, Database, and Schema:**

```sql
USE ROLE ACCOUNTADMIN;

CREATE WAREHOUSE IF NOT EXISTS DASH_S WAREHOUSE_SIZE=SMALL;
CREATE DATABASE IF NOT EXISTS DASH_DB;
CREATE SCHEMA IF NOT EXISTS DASH_SCHEMA;

USE DASH_DB.DASH_SCHEMA;
USE WAREHOUSE DASH_S;
```

**Create File Format & Stage, then Load `CAMPAIGN_SPEND` Data:**
```sql
CREATE OR REPLACE FILE FORMAT csvformat
  SKIP_HEADER = 1
  TYPE = 'CSV';

CREATE OR REPLACE STAGE campaign_data_stage
  FILE_FORMAT = csvformat
  URL = 's3://sfquickstarts/ad-spend-roi-snowpark-python-scikit-learn-streamlit/campaign_spend/';

CREATE OR REPLACE TABLE CAMPAIGN_SPEND (
  CAMPAIGN VARCHAR(60), 
  CHANNEL VARCHAR(60),
  DATE DATE,
  TOTAL_CLICKS NUMBER(38,0),
  TOTAL_COST NUMBER(38,0),
  ADS_SERVED NUMBER(38,0)
);

COPY INTO CAMPAIGN_SPEND
  FROM @campaign_data_stage;
```
**Create Stage and Table for `MONTHLY_REVENUE` Data:**
```sql
CREATE OR REPLACE STAGE monthly_revenue_data_stage
  FILE_FORMAT = csvformat
  URL = 's3://sfquickstarts/ad-spend-roi-snowpark-python-scikit-learn-streamlit/monthly_revenue/';

CREATE OR REPLACE TABLE MONTHLY_REVENUE (
  YEAR NUMBER(38,0),
  MONTH NUMBER(38,0),
  REVENUE FLOAT
);

COPY INTO MONTHLY_REVENUE
  FROM @monthly_revenue_data_stage;
```
**Create `BUDGET_ALLOCATIONS_AND_ROI` Table and Insert Data:**

```sql
CREATE OR REPLACE TABLE BUDGET_ALLOCATIONS_AND_ROI (
  MONTH VARCHAR(30),
  SEARCHENGINE INTEGER,
  SOCIALMEDIA INTEGER,
  VIDEO INTEGER,
  EMAIL INTEGER,
  ROI FLOAT
)
COMMENT = '{"origin":"sf_sit-is", "name":"aiml_notebooks_ad_spend_roi", "version":{"major":1, "minor":0}, "attributes":{"is_quickstart":1, "source":"streamlit"}}';

INSERT INTO BUDGET_ALLOCATIONS_AND_ROI (MONTH, SEARCHENGINE, SOCIALMEDIA, VIDEO, EMAIL, ROI)
VALUES
('January',35,50,35,85,8.22),
('February',75,50,35,85,13.90),
('March',15,50,35,15,7.34),
('April',25,80,40,90,13.23),
('May',95,95,10,95,6.246),
('June',35,50,35,85,8.22);
```

### 2. Running the Snowflake Notebooks
After setting up the environment and loading the data:
- Open the Snowflake notebooks provided in the repository.
- Execute each cell sequentially. These notebooks are configured to perform data transformations using Snowpark’s DataFrame API and run ML workflows directly within Snowflake.
- Ensure that the object names in the notebooks match those created by the SQL scripts. Adjust names if necessary.

## Snowflake `dash_db` Schema Overview
This view displays the `dash_db` schema in Snowflake, highlighting the structured tables for campaign spend, revenue, and budget allocations. It provides a clear overview of the data organization and inter-table relationships for efficient querying.
<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExZnRianlsMW42bGhidXJteXhlNmZxbXJwcTZ4cmFnNjl3NDB5OXljZiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/cERWDbQPpK1trpTDBb/giphy.gif" alt="Giphy Image" width="600" />

## Data Engineering Pipeline DAG
The DAG visualizes the automated data pipeline, detailing task dependencies and sequential execution. It illustrates how Snowflake Tasks and stored procedures orchestrate the ELT processes—from data ingestion to transformation—ensuring a robust and efficient workflow.
<img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExdHR3OHc2ZzRhbmlwM3V0YXpkejN5a3JxNmJwNWExYnIwZHg3aGg4ZCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/D4iyXmz5wb12rJOJbK/giphy.gif" alt="Giphy Image" width="600" />

## ML Dashboard for Predictive Analytics

This interactive dashboard showcases real-time predictive analytics. Users can adjust gauge parameters to simulate various marketing scenarios, with live model inference generating revenue predictions, facilitating dynamic decision-making.
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExcHNna2pmdTkycDlvZGsyemxkY2h0cmY5cXdyZzRrczI0YjhqcGxmNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/J7ueUrqYBUkyi6C4Xm/giphy.gif" alt="Giphy Image" width="600" />


