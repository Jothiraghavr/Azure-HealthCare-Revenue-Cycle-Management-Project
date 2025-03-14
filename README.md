# Azure-HealthCare-Revenue-Cycle-Management-Project

Revenue Cycle Management (RCM)
RCM is the process that hospitals use to manage the financial aspects, from when the patient schedules an appointment until the provider gets paid...

*AIM*

Take data from different sources using Pipeline, the result should be fact tables and dimension tables, to help the reporting team generate the needed KPIs (Key performance indicators).

**Datasets**

EMR Data is stored in Azure SQL DB
2 Databases for 2 diff. hospitals
The 
Insurance Company will provide flat files (CSV) for each hospital in the data lake container, every month.

NPI data is stored in Public API, information for all national doctors.

ICD data: API containing codes for different diseases.

**Solution Architecture**

Medallion Architecture

Different Layers and Stages for data processing and transformation

Landing -> Bronze -> Silver -> Gold

While moving data from the left layer to the right one, we keep on improving, cleansing and transforming the data.

<img width="627" alt="Screenshot 2025-01-26 at 5 15 55 PM" src="https://github.com/user-attachments/assets/26f881e8-c2ba-453e-8b65-74acf469d875" />
<img width="905" alt="Screenshot 2025-01-26 at 5 22 31 PM" src="https://github.com/user-attachments/assets/a50e0c13-cae3-4444-8930-bc3c3949c6d5" />


Landing Layer:

Claims Data (Flat files CSV, Directly in the data 
lake container), Move to the bronze layer using Databricks in parquet file format.

EMR (SQL DB) is not in the landing layer
We will bring EMR's data directly into a bronze layer, by ingesting it through ADF and writing it into a bronze layer in parquet file format.

ICD and NPI data, also directly from API's into bronze layer in parquet file format using Azure Databricks.

In bronze layer: Parquet files
In Silver: Delta Tables
In Gold: Delta Tables

Bronze: Parquet File Format (Source of Truth): Original Data, For The Data Engineer to build upon the data.

Silver: Some data cleaned, Some data enrichment, Common Data Model (CDM) Implemented (For 2 different Hospitals), Implement (Slowly Changing Dimension) SCD2 (To maintain history or changes). Delta Tables perform ACID Transactions.
Data scientists, Data analysts or ML experts can use this as they want some cleansed granular-level data.

Gold: Fact Tables and Dimension Tables Preparation, Final data aggregation, Business teams will take data from this layer. Delta Tables.

Facts: Numeric Data, Never change
Dimensions: Other Information, Can change, Implement SCD2 to maintain the history.

Dimension can be related to other dimensions like the SnowFlake Schema.

**Tech Stacks Used**

To bring EMR Data from SQL DB to the bronze layer (ADLS Gen2)

Azure Data Factory (ADF): For data ingestion
Azure Databricks: Data Transformation/Processing
Azure Data Lake Storage Gen2 (Storage account): Data Storage, To store our parquet files and delta tables.
Azure SQL DB: Data Source, EMR data
Azure Key Vault: For security, to store our secrets/passwords/credentials.

**Implementation**

**Azure Data Factory**

<img width="648" alt="Screenshot 2025-01-26 at 6 52 12 PM" src="https://github.com/user-attachments/assets/6b38fae5-b049-4f3e-96b5-4ee9b4dbfd3f" />

<img width="808" alt="Screenshot 2025-01-29 at 1 28 05 PM" src="https://github.com/user-attachments/assets/5d93b9f7-39cf-483f-8e9b-5f98c771c437" />

**Bronze To Silver**
<img width="433" alt="Screenshot 2025-01-30 at 12 49 35 PM" src="https://github.com/user-attachments/assets/e8757fb9-b629-4d4c-98be-13505d359b65" />

**Silver To Gold**
<img width="610" alt="Screenshot 2025-01-30 at 8 03 35 PM" src="https://github.com/user-attachments/assets/a76be718-7272-473b-9eeb-b1df4b46849e" />

<img width="844" alt="Screenshot 2025-01-31 at 11 17 44 AM" src="https://github.com/user-attachments/assets/d06ee2e9-0cc9-4812-8acf-068728678102" />

**Pipeline Optimization**

<img width="869" alt="Screenshot 2025-01-31 at 11 27 33 AM" src="https://github.com/user-attachments/assets/c5ce5107-72a6-46ce-967b-3490f72178f2" />

<img width="837" alt="Screenshot 2025-01-31 at 11 30 42 AM" src="https://github.com/user-attachments/assets/c0c03ba1-ece1-4503-aa7a-204ee673fcbd" />




