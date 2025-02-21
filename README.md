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
This section provides a deeper dive into the **pipelines** and **Databricks** implementation used in this project.

### Pipelines Created in ADF
1. **pl_ingest_population_data:**
   - Ingests population data from Azure Blob Storage to Data Lake Gen2.
   - Uses Copy Activity and Schedule Trigger.
     
<img width="826" alt="image" src="https://github.com/user-attachments/assets/8cfd4f18-1115-47ac-8ccc-88c04a72014c" />


2. **pl_ingest_ecdc_data:**
   - Ingests ECDC data (cases, deaths, hospital admissions, testing) from the web to Data Lake Gen2.
   - Uses Lookup, ForEach, and Schedule Trigger.

<img width="822" alt="image" src="https://github.com/user-attachments/assets/77f7ba56-12ec-4d03-9cac-504f7b885c2d" />


3. **pl_transform_cases_deaths:**
   - Transforms cases and deaths data using ADF Data Flows.
   - Applies filters, pivots, and lookups.

<img width="824" alt="image" src="https://github.com/user-attachments/assets/ed1be4a3-359d-4229-8d29-13659e2d099b" />


4. **pl_transform_hospital_data:**
   - Transforms hospital admissions data using ADF Data Flows.
   - Splits data into weekly and daily paths.

<img width="825" alt="image" src="https://github.com/user-attachments/assets/65735917-6912-4550-9c20-3d5342c16613" />


5. **pl_copy_to_sql:**
   - Copies transformed data into Azure SQL Database.

<img width="823" alt="image" src="https://github.com/user-attachments/assets/798fe79c-7e3a-49c0-a330-131c2b596f13" />

---
