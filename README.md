# Dashboard Link : https://app.powerbi.com/groups/me/reports/51fa2994-80be-45ad-b5e6-72c66341e580?ctid=e14e73eb-5251-4388-8d67-8f9f2e2d5a46&pbi_source=linkShare

# Problem Statement
In the current competitive landscape, customer retention has emerged as a pivotal component of business sustainability. Churn analysis leverages advanced data analytics and machine learning methodologies to identify, understand, and mitigate customer attrition. By dissecting customer data, organizations can uncover patterns and key factors driving churn, enabling them to proactively address customer dissatisfaction and foster long term loyalty. The insights derived empower organizations to implement data driven strategies, enhancing overall customer satisfaction and retention metrics.

# Data and Resources Utilized

Resources:
Downloadable dataset
Color  Palette:  
  `#4A44F2`, `#9B9FF2`, `#F2F2F2`, `#A0D1FF`

# Target Audience
This project specifically addresses churn analysis for the telecommunications sector, yet its methodologies and outcomes have broad applicability across industries such as retail, finance, and healthcare. Businesses that prioritize customer retention can harness these techniques to derive actionable insights and drive strategic improvements in loyalty and satisfaction.

# Project Objectives
The primary goals of this project include:  

1. Establishing an ETL process within a robust database system to ensure seamless data management.  
2. Creating a comprehensive Power BI dashboard for multidimensional visualization of customer data across:  
    Demographics  
    Geography  
    Payment and account details  
    Service usage  
3. Profiling churners to identify actionable areas for targeted marketing campaigns.  
4. Implementing predictive modelling to identify at risk customers and mitigate future churn.



# Key Metrics
The following metrics will be explored and visualized:
 Total Customers
 Churn Count and Churn Rate
 New Customer Acquisitions  



# Step 1: ETL Process Using SQL Server

 Database Setup and Data Loading
Environment Setup:  
Microsoft SQL Server is employed for its scalability, efficiency in handling large datasets, and integrity management during recurrent ETL processes.

Actions:  
 Install SQL Server Management Studio (SSMS) [Download here](https://learn.microsoft.com/enus/sql/ssms/downloadsqlservermanagementstudiossms?view=sqlserverver16).  
 Create a database `db_Churn` using the following SQL query:  
sql
CREATE DATABASE db_Churn;


Data Import:  
Utilize the SSMS Import Wizard to load the source CSV into a staging table `stg_Churn`. Ensure:
 `Customer_ID` is set as the primary key.  
 Data types such as `Bit` are adjusted to `Varchar(50)` to avoid compatibility issues.

 Data Exploration
Key insights are generated using SQL queries to analyze distribution and null value presence in the data:  
1. Gender Distribution:
   sql
   SELECT Gender, COUNT(Gender) AS TotalCount,
   COUNT(Gender) * 1.0 / (SELECT COUNT(*) FROM stg_Churn) AS Percentage
   FROM stg_Churn
   GROUP BY Gender;
   
2. Customer Status and Revenue Contribution:
   sql
   SELECT Customer_Status, COUNT(Customer_Status) AS TotalCount, 
   SUM(Total_Revenue) AS TotalRev, 
   SUM(Total_Revenue) / (SELECT SUM(Total_Revenue) FROM stg_Churn) * 100 AS RevPercentage
   FROM stg_Churn
   GROUP BY Customer_Status;
   

 Handling Missing Values
Missing data is replaced with meaningful defaults before migrating the cleansed data into the production table `prod_Churn`.  
sql
SELECT 
    ISNULL(Value_Deal, 'None') AS Value_Deal,
    ISNULL(Multiple_Lines, 'No') AS Multiple_Lines,
     Additional fields processed similarly
INTO [db_Churn].[dbo].[prod_Churn]
FROM [db_Churn].[dbo].[stg_Churn];




# Step 2: Data Transformation in Power BI

 Key Transformations
 Derived Columns:  
   *Churn Status*  
    Power query
    Churn Status = IF([Customer_Status] = "Churned", 1, 0)
    
   *Monthly Charge Range*:  
    Categorize monthly charges into bands such as `<20`, `2050`, `50100`, and `>100`.

 Reference Tables for Mapping:  
  1. Age Groups (`<20`, `2035`, `3650`, `>50`)  
  2. Tenure Groups (`<6 months`, `612 months`, ...)

 Service Usage Profiling:  
  Unpivot service related columns for granular analysis.


# Step 3: Power BI Measures

 Calculated Metrics
 Total Customers:  
   power query
   Total Customers = COUNT(prod_Churn[Customer_ID])
   
 Total Churn and Churn Rate:  
   Power query
   Total Churn = SUM(prod_Churn[Churn Status])
   Churn Rate = [Total Churn] / [Total Customers]
   

# Step 4: Power BI Visualization

 Key Insights
1. Summary Metrics:  
   Display cards summarizing total customers, new joiners, total churn, and churn rate.  
2. Demographic Analysis:  
   Examine churn rates across age groups and gender.  
3. Geographical and Service Insights:  
    Top 5 states by churn rate  
    Internet type and service usage distribution  

 Churn Reason Analysis  
Tooltips highlight churn reasons with corresponding totals for detailed investigation.

# Step 5: Predictive Modelling Using Random Forest

 Overview
Random Forest, a powerful ensemble machine learning algorithm, is utilized to predict churn. Its ability to aggregate results from multiple decision trees ensures robust and accurate predictions, mitigating overfitting risks.  

 Data Preparation
 Export views from SQL Server for preprocessing and model training.  
 Implement Random Forest for classification, predicting churn likelihood based on customer attributes.


This project delivers a comprehensive framework for customer churn analysis, integrating SQL driven ETL, dynamic Power BI dashboards, and predictive modelling to enable actionable insights for improved customer retention strategies.  

# Snapshot of Dashboard(Power BI Service)
![Image](https://github.com/user-attachments/assets/e28f7a58-8918-40de-80c3-2602518e6121)

![Image](https://github.com/user-attachments/assets/ab6d3aef-5af1-4600-913c-fb7a51acf928)

