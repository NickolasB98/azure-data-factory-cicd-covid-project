# Azure Data Factory COVID-19 Reporting

### Concept of the Project 💡
- This project involves the acquisition of several Covid-19 Datasets from the ECDC website. These datasets are subsequently processed through diverse ADF components to effect transformations. These transformations are executed using ADF and Databricks. The resultant data is then loaded into SQL Datawarehouse with the intention of enabling the Analytics team to draw meaningful and practical insights from these datasets. The primary objective is to comprehensively understand the influence of Covid on the entirety of the European Region throughout the course of the year 2020.

### Task 🎯
- This project's mission is to ingest data from multiple data sources, clean it up, and alter it so that it is more robust and suitable for the goal. Once the data has been cleaned, it should be imported into a central repository, such as a data warehouse or datalake, so that the analytics team can utilize Power BI and other BI tools to access it. The data warehouse will contain information on confirmed cases, regrettable death rates, hospitalization and ICU cases from our weekly data lake counts, as well as testing statistics. Additionally, we can utilize these data to construct ML Models that forecast the spread of COVID in the European region.

### Source Data: 📤
- ECDC (https://www.ecdc.europa.eu/en/covid-19)
- Population Data From Azure Blob Storage (eurostat_data)

### Destination: 📥📍
- Azure Data Lake Gen2 Storage.

## Tools ⚙

### Data Integration/Ingestion

- ADF Data Flows within the Data Factory

### Transformation

- Data Flows within the Data Factory
- DataBricks

### Data Warehouse Solution

- Azure SQL Database

### Visualization

- Power BI Desktop
- Power BI Service

### Approach

### Environment Setup
- Azure Subscription
- Data Factory
- Azure Blob Storage Account
- Data Lake Storage Gen2
- Azure SQL Database
- Azure Databricks Cluster

# Solution Architecture Overview

![architecture_diagram](https://github.com/user-attachments/assets/4adbcf9d-765a-4d9c-9068-4baff49b8ac0)


### DATA EXTRACTION/ INGESTION
Four different datasets were ingested from both the ECDC website and azure blob storage into Datalake Gen2. They are - 

- Cases and Deaths Data
- Hospital Admissions Data
- Population Data
- Test Conducted Data

We used various components of ADF Pipeline activities to ingest the data from both HTTP Data Source and Azure Storage Account to Azure DataLake. Some of those activities are:

- Validation Activity
- Get Metadata Activity
- Copy Activity

### Population Data : Load into Storage Account and move it to Destination Data Lake
Ingest "population by age" data for all EU Countries into the Data Lake to support the machine learning models with the data to predict an increase in Covid-19 mortality rate.

### Solution Flow

<img width="1352" alt="Screenshot at Aug 03 00-59-08" src="https://github.com/user-attachments/assets/9cf89ffb-3fba-49b4-b61a-8ebb5eefdfb0">


### Steps:
1. Create a Linked Service To Azure Blob Storage
2. Create a Source Data Set
3. Create a Linked Service To Azure Data Lake storage (GEN2)
4. Create a Sink Data set
5. Create a Pipeline:
- Execute Copy activity when the file becomes available
- Check metadata counts before loading the data using the IF Condition
- Finally Load Data into our destination
6. ScheduleTrigger


### Pipeline Design :

<img width="1433" alt="Screenshot at Aug 03 01-01-34" src="https://github.com/user-attachments/assets/b81f440c-17f9-4b62-8a62-7b27191beb00">

### ECDC Data from Web to Destination Data Lake

### ECDC Data Content - Four files of CSV :
1. Case & Deaths Data.csv
2. Hospital Admission Data.csv
3. testing.csv
4. country_response.csv


### Solution Flow

<img width="1032" alt="Screenshot at Aug 03 01-03-16" src="https://github.com/user-attachments/assets/17018520-fb1f-4430-860d-c3dc4fccd891">


Steps:
1. Create a Linked Service using an HTTP connector
2. Create a Source Data Set
3. Create a Linked Service To Azure Data Lake storage (GEN2)
4. Create a Sink Data set
5. Create a Pipeline With Parameters & Variables
6. Lookup to get all the parameters from json file, then pass it to ForEach ECDC DATA as shown below
7. Schedule Trigger


<img width="428" alt="Screenshot at Aug 03 01-04-03" src="https://github.com/user-attachments/assets/8cdcbf02-e8fa-4c95-a46a-cfa4028e349b">



### Pipeline Design :


<img width="1439" alt="Screenshot at Aug 03 01-05-59" src="https://github.com/user-attachments/assets/f5be0bb7-bba0-4533-b697-6c43c348fbf8">



# 2. DATA TRANSFORMATION

The Cases and Deaths data together with the Hospital admissions data was transformed using ADF Data flows. The Data Flows transformation used on both dataset include

- Select transformation
- Lookup transformation
- Filter transformation
- Join transformation
- Sort transformation
- Conditional split transformation
- Derived columns transformation
- Sink transformation

# Data Flows (1) Cases & Deaths Data:

### Solution Flow

<img width="1046" alt="Screenshot at Aug 03 01-06-19" src="https://github.com/user-attachments/assets/097827b0-735f-40ec-9b0b-72f9633f3721">



### Steps:
1. Cases And Deaths Source (Azure Data Lake Storage Gen2 )
2. Filter Europe-Only Data
3. Select only the required columns
4. PivotCounts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
5. Lookup Country to get country_code_2_digit,country_code_3_digit columns
6. Select Only the required columns for the Sink
7. Create a Sink dataset (Azure Data Lake Storage Gen2)
8. Used Schedule Trigger


<img width="1393" alt="Screenshot at Aug 03 01-07-30" src="https://github.com/user-attachments/assets/7201b5c7-88ec-4eda-8b96-4e0bb666142b">





# Data Flows (2) Hospital Admissions Data:

### Solution Flow

<img width="1036" alt="Screenshot at Aug 03 01-07-40" src="https://github.com/user-attachments/assets/7bf07682-4206-457a-a118-bd11dab9e9c0">


### Steps:
1. Hospital Admissions Source (Azure Data Lake Storage Gen2 )
2. Select only the required columns
3. Lookup Country to get country_code_2_digit,country_code_3_digit columns
4. Select only the required columns
5. Condition Split Weekly, Daily Split condition
  - indicator=='Weekly new hospital admissions per 100k' || indicator=='Weekly new ICU admissions per 100k'
  - indicator== "Daily hospital occupancy" || indicator=="Daily ICU occupancy"
6. For Weekly Path
- Join with Date to get ecdc_Year_week, week_start_date, week_End_date
- Piovt Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
- Sort data using reported_year_week ASC and Country DESC
- Select only required columns for sink
- Create a sink dataset (Azure Data Lake Storage Gen2)
- Schedule Trigger
7. For Daily Path
- Pivot Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
- Sort Data using reported_year_week ASC and Country DESC
- Select only required columns for sink
- Create a sink dataset (Azure Data Lake Storage Gen2)
- Used Schedule Trigger


<img width="1372" alt="Screenshot at Aug 03 01-08-56" src="https://github.com/user-attachments/assets/b1483ff0-f9f6-4400-a7b1-8b89aa01a37d">



# Databricks Activity (3) -- Population File:


<img width="1030" alt="Screenshot at Aug 03 01-09-11" src="https://github.com/user-attachments/assets/09a53d9f-5df1-453e-b203-1963beef46e3">




# 3. Copy Data to Azure SQL
1- Copy Cases and Deaths
2- Copy hospital admissions data
3- Copy testing data


<img width="1267" alt="Screenshot at Aug 03 01-10-24" src="https://github.com/user-attachments/assets/edaf42fe-b88a-47c0-8eef-68f4c852d439">


# 4. Reporting via Power BI

1. Create a Connection from Azure SQL to Power Bi and load the data
2. Analyze the data to get the total confirmed cases and deaths count
3. Identify the trends in data based on reporting date
4. Publish the report to Power BI Server
5. Publish to web

# Covid-19 Trend in the EU/EEA & UK 2020 by Cases, Deaths, Hospital Occupancy, and ICU Occupancy

<img width="933" alt="Screenshot at Aug 03 01-18-00" src="https://github.com/user-attachments/assets/e6cb9e1d-a809-43c3-bd6f-9e7dfbbb78e9">


# Covid-19 Cases and Death breakdown by population in the UK, France, and Germany

<img width="973" alt="Screenshot at Aug 03 01-18-12" src="https://github.com/user-attachments/assets/4b53b495-3815-4394-8bfe-933ac6835a85">



# Total Number of covid tests carried out vs Confirmed Cases

<img width="991" alt="Screenshot at Aug 03 01-18-26" src="https://github.com/user-attachments/assets/b042a2aa-b8d5-4e10-9fc2-0bad1a817f86">


# Used Technologies
- Azure DataFactory
- Azure Databricks (Pyspark, SparkSql)
- Azure Storage Account
- Azure Data Lake Gen2
- Azure SQL Database


### This project was built initially in ADF Live Mode, but was later on transformed to provide a CI/CD fully automated solution, using Azure DevOps and Git.

<img width="1284" alt="Screenshot at Aug 03 01-42-05" src="https://github.com/user-attachments/assets/0922f3d5-41f8-480c-9775-391d1a8adb16">

The ADF Pipelines are automatically converted to JSON templates, and from now on are used in JSON format in Azure DevOps.

For the fully automated solution, instead of having to utilize the Publish option in the main branch, we make use of a Build Pipeline.

The build pipeline automates the Publish of the main branch from the Dev environment, forwarding the new changes to the Test and Prod environments.

**After successful deployment in the Test enviroment, an approval is needed to forward the changes to the Production env.**

<img width="1267" alt="Screenshot at Aug 03 01-43-13" src="https://github.com/user-attachments/assets/5271d260-ecc6-4132-83ca-91e000915d0c">

Also, for the three environments to have access to the corresponding data lakes, we utilize the System Assigned Managed Identities.

## The Git Repo utilized

<img width="1406" alt="Screenshot at Aug 03 01-32-45" src="https://github.com/user-attachments/assets/dfc3269f-1cfe-4a2c-accd-ce84bfc7e783">

## The Build Pipeline (adf-ci-option-2-build-pipeline.yml)

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/e1758bed-9e2c-445f-adee-e809c72be358">

## The Release Pipeline

<img width="1428" alt="Screenshot at Aug 03 01-31-53" src="https://github.com/user-attachments/assets/3c060635-caf6-45dc-83eb-f5578b685b30">

### Tasks of Release Pipeline

<img width="1439" alt="Screenshot at Aug 03 01-32-22" src="https://github.com/user-attachments/assets/cf6ffa6d-9287-42bc-9db7-84f1c5c73d4c">

