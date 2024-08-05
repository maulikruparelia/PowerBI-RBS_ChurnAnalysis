# PowerBI-RBS_ChurnAnalysis
Here’s a detailed breakdown of each step to proceed with creating your Power BI report and dashboard for bank customer churn analysis based on the provided requirements. I will guide you through data preparation, modeling, visualization, and analysis.

### Step 1: Data Preparation

#### 1. Gather Data
- **Data Sources**: Collect data from the following tables:
  - **ActiveCustomer**
  - **Bank_Churn**
  - **CreditCard**
  - **CustomerInfo**
  - **ExitCustomer**
  - **Gender**
  - **Geography**

#### 2. Clean and Transform Data
- **Import Data**:
  - Open Power BI Desktop.
  - Go to **Home > Get Data** and choose the appropriate data source (Excel, CSV, SQL Database, etc.).
  
- **Transform Data**:
  - Open the **Power Query Editor** by clicking on **Transform Data**.
  - Review each dataset for any necessary transformations:
    - Remove unnecessary columns like `RowNumber`, `CustomerId`, and `Surname`.
    - Check for and handle any missing values.
    - Convert date formats for the `Bank DOJ` column.

- **Merging Datasets**:
  - Use **Merge Queries** to combine tables based on common fields (e.g., CustomerId).
  - Ensure you keep relevant columns, including churn indicators (Exited, CreditScore, Age, etc.).

### Step 2: Data Modeling

#### 1. Define Relationships
- **Model View**:
  - Switch to the **Model view** in Power BI.
  - Create relationships between tables:
    - **ActiveCustomer** to **Bank_Churn** using `CustomerId`.
    - **CustomerInfo** to **CreditCard**, **Gender**, and **Geography** using `CustomerId`.

### Step 3: Data Analysis

#### 1. Create Calculated Columns
- **Credit Score Categories**:
  - Open the **Data view**.
  - Create a calculated column for credit score categories:
    ```DAX
    CreditScoreCategory = 
    SWITCH(
        TRUE(),
        'CustomerInfo'[CreditScore] >= 800, "Excellent",
        'CustomerInfo'[CreditScore] >= 740, "Very Good",
        'CustomerInfo'[CreditScore] >= 670, "Good",
        'CustomerInfo'[CreditScore] >= 580, "Fair",
        'CustomerInfo'[CreditScore] < 580, "Poor"
    )
    ```

- **Tenure Calculation**:
  - Calculate the tenure based on the `Bank DOJ` column:
    ```DAX
    Tenure = DATEDIFF('CustomerInfo'[Bank DOJ], TODAY(), YEAR)
    ```

### Step 4: Report and Dashboard Creation

#### 1. Design Visuals
- **Page 1: Churn Overview**
  - **Pie Chart**: Churn vs. Retained Customers
    - Values: Count of `CustomerId`, Legend: `Exited`.

- **Page 2: Credit Score Impact**
  - **Bar Chart**: Churn Rate by Credit Score Category
    - Axis: `CreditScoreCategory`, Values: Count of `CustomerId`, Legend: `Exited`.

- **Page 3: Geography Impact**
  - **Map Visual**: Churn Rate by Geography
    - Location: `Geography`, Values: Count of `CustomerId`, Tooltip: `Exited`.

- **Page 4: Demographics**
  - **Bar Chart**: Churn Rate by Age Group
    - Create Age Groups:
      ```DAX
      AgeGroup = 
      SWITCH(
          TRUE(),
          'CustomerInfo'[Age] < 25, "Under 25",
          'CustomerInfo'[Age] >= 25 && 'CustomerInfo'[Age] < 35, "25-34",
          'CustomerInfo'[Age] >= 35 && 'CustomerInfo'[Age] < 45, "35-44",
          'CustomerInfo'[Age] >= 45 && 'CustomerInfo'[Age] < 55, "45-54",
          'CustomerInfo'[Age] >= 55, "55+"
      )
      ```
    - Axis: `AgeGroup`, Values: Count of `CustomerId`, Legend: `Exited`.

#### 2. Add Interactive Filters
- **Slicers**:
  - Add slicers for `Geography`, `Gender`, `CreditScoreCategory`, and `AgeGroup`.
  - This allows users to filter the visuals based on these attributes.

### Step 5: Insights and Recommendations

#### 1. Identify Key Insights
- After creating visuals, analyze the trends:
  - **High churn rates** for specific credit score categories.
  - **Geographical areas** with high churn rates.

#### 2. Actionable Recommendations
- Based on insights:
  - Develop loyalty programs for customers in high-risk categories.
  - Target campaigns for young customers or those with lower balances.

### Final Steps

1. **Review and Test**:
   - Ensure all visuals are functioning as expected.
   - Test slicers to confirm they filter data correctly.

2. **Publish**:
   - Once the report is complete, publish to Power BI Service for sharing with stakeholders.

### Sample Visual Layout

- **Churn Overview** (Pie Chart)
- **Churn by Credit Score** (Bar Chart)
- **Churn by Geography** (Map)
- **Churn by Age Group** (Bar Chart)
- **Churn by Gender** (Bar Chart)
- **Financial Indicators** (Scatter Plot)

### Conclusion

With these steps, you can create a comprehensive Power BI report and dashboard to analyze customer churn at your bank effectively. 
