# Azure Data Factory COVID-19 Reporting (Fully Automated CI/CD)

## Project Background
This project focuses on analyzing COVID-19 data across the European region for the year 2020. The goal is to ingest, transform, and load data from multiple sources into a centralized data warehouse, enabling the analytics team to derive meaningful insights and build predictive models. The project leverages **Azure Data Factory (ADF)**, **Azure Databricks**, and **Azure SQL Database** to create a fully automated data pipeline with CI/CD integration.

### Key Business Metrics
- **Dataset:** COVID-19 cases, deaths, hospital admissions, testing data, and population statistics.
- **Data Sources:** ECDC website and Azure Blob Storage.
- **Business Goal:** Understand the impact of COVID-19 in Europe and provide actionable insights for decision-making.

---

## Data Structure & Initial Checks
The dataset consists of the following tables:

### Source Data:
1. **Cases and Deaths Data:** Daily confirmed cases and deaths.
2. **Hospital Admissions Data:** Weekly and daily hospital and ICU admissions.
3. **Population Data:** Population statistics by age for EU countries.
4. **Testing Data:** COVID-19 testing statistics.

### Destination:
- **Azure Data Lake Gen2 Storage:** For raw and transformed data.
- **Azure SQL Database:** For final reporting and analysis.

---

## Executive Summary
### Overview of Findings
This project successfully implemented a fully automated data pipeline to ingest, transform, and analyze COVID-19 data across Europe. Key findings include:
1. **Data Integration:** Seamless ingestion of data from multiple sources into Azure Data Lake Gen2.
2. **Data Transformation:** Robust transformation of raw data using ADF Data Flows and Databricks.
3. **Centralized Reporting:** Loading of transformed data into Azure SQL Database for Power BI reporting.
4. **Automation:** CI/CD integration using Azure DevOps for pipeline deployment and scheduling.

This project was built initially in ADF Live Mode, but was later on transformed to provide a CI/CD fully automated solution, using Azure DevOps and Git.

<img width="821" alt="image" src="https://github.com/user-attachments/assets/4aa0f470-5443-41cb-aaf9-3916c5b8434a" />

The ADF Pipelines are automatically converted to JSON templates, and from now on are used in JSON format inside Azure DevOps.

For the fully automated solution, instead of having to utilize the Publish option in the main branch, we make use of a Build Pipeline.

The build pipeline automates the Publish of the main branch from the Dev environment, forwarding the new changes to the Test and Prod environments.

After successful deployment in the Test enviroment, an approval is needed to forward the changes to the Production env.

<img width="829" alt="image" src="https://github.com/user-attachments/assets/3dc6ebb7-8c7f-4f8a-9f17-40df451032db" />

Also, for the three environments to have access to the corresponding data lakes, we utilize the System Assigned Managed Identities.

The Git Repo utilized

<img width="826" alt="image" src="https://github.com/user-attachments/assets/c0158ff1-8f51-4233-98a8-7a7db90ad449" />

The Build Pipeline (adf-ci-option-2-build-pipeline.yml)

<img width="824" alt="image" src="https://github.com/user-attachments/assets/c5e35fcc-fa8c-47ee-8b48-de88bd82f3c6" />

The Release Pipeline

<img width="825" alt="image" src="https://github.com/user-attachments/assets/1483f214-8430-4d2f-9845-a85a26f6df1a" />

Tasks of Release Pipeline

<img width="824" alt="image" src="https://github.com/user-attachments/assets/32d729ea-437f-4790-a6e4-e9a5194aa73b" />

---

## Insights Deep Dive

### Category 1: COVID-19 Spread and Impact (Overall)

#### Main Insight 1: **Overall Burden and Trends**
- **Finding:** Across the EU/EEA and UK, as of late 2020, there were millions of confirmed COVID-19 cases and hundreds of thousands of deaths, with upward trends observed in both cases and deaths throughout the year.
- **Supporting Data:** 
  - The first dashboard prominently displays the aggregate numbers of confirmed cases and deaths.
  - Trend charts in the first dashboard show the growth of cases and deaths over time.
- **Implication:** These figures demonstrate the significant and escalating impact of the pandemic across the region during 2020.
- **Visualization:** <img width="826" alt="image" src="https://github.com/user-attachments/assets/5830c1f1-fa7f-49aa-8308-c312d1878574" />


---

### Category 2: National Trends Comparison (UK, France, Germany)

#### Main Insight 1: **Varying Peaks and Trajectories**
- **Finding:** The UK, France, and Germany all experienced peaks in COVID-19 cases and deaths, but the timing, magnitude, and shape of these peaks varied across the countries.
- **Supporting Data:** 
  - The second dashboard's charts directly compare the case and death trends for each country.
- **Implication:** These differences likely reflect variations in factors such as public health policies, social behavior, population density, and healthcare system capacity.
- **Visualization:** <img width="824" alt="image" src="https://github.com/user-attachments/assets/49724459-de1a-46c4-9bcc-12b4c481f1f4" />


---

### Category 3: Testing Capacity and Effectiveness

#### Main Insight 1: **Testing Volume and Correlation**
- **Finding:** Significant variations exist in the total number of COVID-19 tests carried out across different countries. The relationship between testing volume and confirmed cases can be analyzed.
- **Supporting Data:** 
  - The third dashboard provides data on the total number of tests performed in various countries.
  - The "Tests done Vs Confirmed Cases" chart shows the correlation between the two.
- **Implication:** Understanding testing capacity is crucial for assessing the true spread of the virus. Analyzing the relationship between tests and cases can provide insights into the effectiveness of testing strategies.
- **Visualization:** <img width="825" alt="image" src="https://github.com/user-attachments/assets/c820b92a-f435-4cdd-a64a-aa1aaba07660" />


---

## Recommendations

Based on the insights from the three dashboards, we recommend the following actions:

1. **In-Depth Comparative Analysis:** Conduct further research to understand the factors contributing to the differences in case and death trends across countries. Consider demographic data, public health policies, lockdown measures, social behavior, and healthcare system capacity.
2. **Healthcare Resource Planning:** Use insights on hospital and ICU occupancy to inform resource allocation, capacity planning, and surge preparedness for healthcare systems.
3. **Testing Strategy Evaluation:** Analyze the relationship between testing volume and confirmed cases to evaluate the effectiveness of testing strategies. Investigate potential disparities in access to testing.
4. **Public Health Interventions:** Combine the insights from all dashboards to assess the impact of public health interventions and inform future strategies. Consider the timing and stringency of lockdowns, social distancing measures, and other interventions.
5. **Data Transparency and Standardization:** Advocate for transparent and standardized data collection and reporting to facilitate more accurate comparisons and collaborative research efforts.
6. **Further Research:** Conduct further research to explore the long-term effects of the pandemic, including impacts on different demographic groups, economic consequences, and mental health implications.

---

## Assumptions and Caveats
1. **Assumption 1:** Data from the ECDC website is accurate and up-to-date.
2. **Assumption 2:** Population data from Azure Blob Storage is consistent across all EU countries.
3. **Assumption 3:** The data reflects only the European region and may not be applicable globally.

---

## Tools and Technologies Used
- **Data Integration/Ingestion:** Azure Data Factory (ADF)
- **Transformation:** ADF Data Flows, Azure Databricks (PySpark, SparkSQL)
- **Data Warehouse:** Azure SQL Database
- **Visualization:** Power BI Desktop, Power BI Service
- **CI/CD Tools:** Azure Repos, Azure DevOps

---

## Solution Architecture
### Overview
The architecture involves:
1. **Data Ingestion:** Ingesting data from ECDC and Azure Blob Storage into Azure Data Lake Gen2.
2. **Data Transformation:** Transforming data using ADF Data Flows and Databricks.
3. **Data Loading:** Loading transformed data into Azure SQL Database.
4. **Reporting:** Visualizing data using Power BI.

<img width="818" alt="image" src="https://github.com/user-attachments/assets/35fc119f-1e65-467b-acae-20aabc064187" />

---

## Project Execution Flow
### 1. Data Extraction/Ingestion
- **Datasets Ingested:**
  - Cases and Deaths Data
  - Hospital Admissions Data
  - Population Data
  - Testing Data
- **Tools Used:** ADF Pipeline Activities (Validation, Get Metadata, Copy Activity).

### 2. Data Transformation
- **Tools Used:** ADF Data Flows, Databricks.
- **Transformations Applied:**
  - Select, Lookup, Filter, Join, Sort, Conditional Split, Derived Columns, Sink.

### 3. Data Loading
- **Destination:** Azure SQL Database.
- **Process:** Copy transformed data into SQL tables for reporting.

### 4. Reporting
- **Tools Used:** Power BI Desktop and Power BI Service.
- **Key Reports:**
  - COVID-19 Trends in the EU/EEA & UK (Cases, Deaths, Hospital Occupancy, ICU Occupancy).
  - Cases and Deaths Breakdown by Population (UK, France, Germany).
  - Total Number of COVID Tests vs Confirmed Cases.

---

## Optional Reading: Pipeline Implementation Details

### Approach

### Environment Setup
- Azure Subscription
- Data Factory
- Azure Blob Storage Account
- Data Lake Storage Gen2
- Azure SQL Database
- Azure Databricks Cluster
- Azure DevOps Account
- Azure Git Repository

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



## 2. DATA TRANSFORMATION

The Cases and Deaths data together with the Hospital admissions data was transformed using ADF Data flows. The Data Flows transformation used on both dataset include

- Select transformation
- Lookup transformation
- Filter transformation
- Join transformation
- Sort transformation
- Conditional split transformation
- Derived columns transformation
- Sink transformation

## Data Flows (1) Cases & Deaths Data:

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





## Data Flows (2) Hospital Admissions Data:

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



## Databricks Activity (3) -- Population File:


<img width="1030" alt="Screenshot at Aug 03 01-09-11" src="https://github.com/user-attachments/assets/09a53d9f-5df1-453e-b203-1963beef46e3">




## 3. Copy Data to Azure SQL
1- Copy Cases and Deaths
2- Copy hospital admissions data
3- Copy testing data


<img width="1267" alt="Screenshot at Aug 03 01-10-24" src="https://github.com/user-attachments/assets/edaf42fe-b88a-47c0-8eef-68f4c852d439">


## 4. Reporting via Power BI

1. Create a Connection from Azure SQL to Power Bi and load the data
2. Analyze the data to get the total confirmed cases and deaths count
3. Identify the trends in data based on reporting date
4. Publish the report to Power BI Server
5. Publish to web

---
