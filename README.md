# Nexasat

## Project Overview: Enhancing Revenue Strategies with CLV Segmentation

!(assets/images/NEXASAT.png)

### Business Challenge
NexaSat, a prominent player in the telecommunications industry, faces a critical dilemma: how to optimize marketing efforts and resource allocation. Their diverse customer base—ranging from occasional users to loyal enthusiasts—requires a tailored approach. As competition intensifies, NexaSat aims to maximize revenue from existing customers while identifying high-potential prospects. The challenge lies in pinpointing receptive customers and crafting personalized offers.

### Untapped Potential
NexaSat recognizes untapped opportunities within their customer base. Personalized offers and bundled services could boost Average Revenue Per User (ARPU). However, without a structured approach, their up-selling and cross-selling efforts yield suboptimal results.

### The Solution: CLV Segmentation
- What is CLV Segmentation?
  - A data-driven strategy categorizing customers based on long-term value.
  - Enables targeted marketing and tailored services.
  - Optimizes resource allocation and strengthens customer relationships.

- NexaSat's Goal
  - Unlock up-selling and cross-selling opportunities.
  - Design personalized offers for each segment.
  - Focus on high-opportunity segments for maximum ROI.
  - Strengthen loyalty in a competitive telecom landscape.

### Project Aim
Implement CLV segmentation to strategically boost revenue, enhance ARPU, and foster lasting customer satisfaction. NexaSat aims to secure a competitive edge by delivering tailored solutions aligned with customer preferences.

### Data Description
- Customer Data (July 2023):
  - Includes unique customer IDs, gender, partner status, dependents, senior citizen status, call duration, data usage, plan type (prepaid/postpaid), and plan level (basic/premium).

### Tech Stack
- PostgreSQL:
  - Used for data manipulation and analysis.

### Project Scope
1. Exploratory Data Analysis (EDA):
   - Examine customer data to uncover insights into behavior and preferences.
   - Understand demographic patterns and usage trends.

2. Feature Engineering:
   - Create relevant features like Customer Lifetime Value (CLV) and CLV scores.
   - These features will be crucial for segmentation.

3. Segmentation:
   - Categorize customers based on CLV scores.
   - Further segment based on demographics, usage, and service plans.

4. Segment Profiling and Strategy Formulation:
   - Understand each segment's unique traits.
   - Develop personalized marketing strategies (special offers, targeted promotions) aligned with customer preferences.

### Solution
```SQL

-- Query 1: Create Schema
CREATE SCHEMA Nexa.Sat;

-- Query 2: Create a table in the schema
CREATE TABLE "Nexa_Sat".nexa_sat (
    Customer_ID VARCHAR(50),
    gender VARCHAR(10),
    Partner VARCHAR(3),
    Dependents VARCHAR(3),
    Senior_Citizen INT,
    Call_Duration FLOAT,
    Data_Usage FLOAT,
    Plan_Type VARCHAR(20),
    Plan_Level VARCHAR(20),
    Monthly_Bill_Amount FLOAT,
    Tenure_Months INT,
    Multiple_Lines VARCHAR(3),
    Tech_Support VARCHAR(3),
    Churn INT);

```
```SQL
-- Import data

-- Set search path for queries
SET search_path TO "Nexa_Sat"

-- Confirm current schema
SELECT current_schema();

SELECT * FROM 
nexa_sat

-- Data Cleaning
-- Check for Duplicates

SELECT Customer_ID, gender, Partner, Dependents,
  Senior_Citizen, Call_Duration, Data_Usage,
  Plan_Type, Plan_Level, Monthly_Bill_Amount,
  Tenure_Months, Multiple_Lines, Tech_Support, 
  Churn
  FROM nexa_sat
  GROUP BY
  Customer_ID, gender, Partner, Dependents,
  Senior_Citizen, Call_Duration, Data_Usage,
  Plan_Type, Plan_Level, Monthly_Bill_Amount,
  Tenure_Months, Multiple_Lines, Tech_Support, 
  Churn
  HAVING COUNT(*) > 1 -- This will filter out duplicate rows

--Check for null values
SELECT *
FROM nexa_sat
WHERE Customer_ID IS NULL
OR gender IS NULL
OR Partner IS NULL
OR Dependents IS NULL
OR Senior_Citizen IS NULL
OR Call_Duration IS NULL
OR Data_Usage IS NULL
OR Plan_Type IS NULL
OR Plan_Level IS NULL
OR Monthly_Bill_Amount IS NULL
OR Tenure_Months IS NULL
OR Multiple_Lines IS NULL
OR Tech_Support IS NULL
OR Churn IS NULL;

---EDA Exploratory Data Analysis
-- Total Users

SELECT COUNT(Customer_ID) AS current_Users
FROM  nexa_sat
WHERE Churn = 0;

-- Total Users by Level
SELECT Plan_Level, COUNT(Customer_ID) AS current_Users
FROM  nexa_sat
WHERE Churn = 0
GROUP BY Plan_Level;

-- Total Revenue
SELECT CAST(SUM(Monthly_Bill_Amount) AS DECIMAL(18, 2)) AS Total_Revenue
FROM nexa_sat;

-- Revenue by plan level
SELECT plan_level, CAST(SUM(Monthly_Bill_Amount) AS DECIMAL(18, 2)) AS Revenue
FROM nexa_sat
GROUP BY 1
ORDER BY 2;

-- Churn count by plan type and plan level
SELECT plan_level,
	   plan_type,
	COUNT(*) AS Total_Customers,
	SUM(Churn) AS Churn_Count
FROM nexa_sat
GROUP BY 1,2
ORDER BY 1;

-- Average Tenure by plan level
SELECT plan_level, ROUND(AVG(tenure_months),2) AS Average_Tenure
FROM nexa_sat
GROUP BY 1;
