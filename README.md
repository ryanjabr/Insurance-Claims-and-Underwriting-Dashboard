# Insurance Performance Dashboard - Havencrest Life & General
A Power BI dashboard analyzing insurance KPIs across Claims, Underwriting & Profitability, and Renewals & Customer Profiles. Covers years 2020–2022 with multiple product types, regions, and channels.

## Objectives
- Track key insurance KPIs for decision-making.
- Identify underperforming and high-performing product lines.
- Provide actionable insights on claims, profitability, and customer retention.

## Dashboard Features
- Claims Performance → Total Claim Amount, CSR, CRR, Pending Claims, TAT Compliance, Claim Frequency, Claim Severity.
- Underwriting & Profitability → GWP, Loss Ratio, Expense Ratio, Combined Ratio, Hit Ratio, Underwriting Profit/Loss.
- Renewals & Customer Profiles → Total Proposals, Policies Issued, Hit Ratio, Renewal Ratio, Churn Rate, Avg. Policy Tenure.
- Slicers for filtering: Product, Claim Type, Region, Channel, Year.

## Tools & Methods Used
- Power BI: Dashboard creation, KPI cards, DAX measures (YoY, ratios).
- Excel: Data preparation.
- DAX: To create metrics needed for insights such as loss ratio , hit ratio , expense ratio etc.

## Data Collection & Preparation
- Imported Excel dataset with Policies, Claims, Customers as sheets.
- Removed duplicates, handled missing values.
- Created extra columns (e.g., Policy End Date, TAT(Days), TAT(Status), Renewal etc).
- Standardized formats for dates and categories.
- Added a Date Table to support time intelligence functions (YoY, trends).
- Cleaned inconsistent entries in Region/Channel/Product columns.
- Validated totals.
- Used Renewal column (Yes/No) for Renewal Ratio and Churn Rate.

## Data Modeling (Power BI)
- Built a separate Date Table covering 01-01-2020 to 31-12-2022.
- Marked it as the official Date Table and established relationships with fact tables.

#### Relationships created:
- DateTable[Date] ↔ Policies[Policy Start Date] (1-to-many).
- Customers[CustomerID] ↔ Policies[CustomerID] (1-to-many).
- Policies[PolicyID] ↔ Claims[PolicyID] (1-to-many).

  
## Dashboard Development (Power BI)
- Designed 5 pages:
  - Home Page 
  - Claims Performance – Total Claim Amount, CSR, CRR, TAT Compliance, Claim Frequency, Claim Severity.
  - Underwriting & Profitability – GWP, Loss Ratio, Expense Ratio, Combined Ratio, Hit Ratio, Profit/Loss.
  - Renewals & Customer Profiles – Total Proposals, Policies Issued, Renewal Ratio, Churn Rate, Avg Policy Tenure (yrs).
  - Executive Summary
- Used smart KPI cards with conditional formatting for YoY % changes (+/-).
- Added slicers for Product, Region, Channel, Claim Type, and Year for interactivity.
- Applied icons for major KPIs (only where it added clarity).


### Home Page
![image alt](https://github.com/ryanjabr/Insurance-Claims-and-Underwriting-Dashboard/blob/653e5a0e11ddd445abe652516df7160426b05a99/Insurance%20Project_pages-to-jpg-0001.jpg)


### Claim Performance
![image alt](https://github.com/ryanjabr/Insurance-Claims-and-Underwriting-Dashboard/blob/dee6cc2e5349e64f8d580b181af864483011ed30/Insurance%20Project_pages-to-jpg-0002.jpg)


### Underwriting & Profitability
![image alt](https://github.com/ryanjabr/Insurance-Claims-and-Underwriting-Dashboard/blob/cdc714f431640ea910967b6ffbf06349b6dc0db1/Insurance%20Project_pages-to-jpg-0003.jpg)


### Renewals & Customer Profile
![image alt](https://github.com/ryanjabr/Insurance-Claims-and-Underwriting-Dashboard/blob/f4b725bce5de8d642e9140d8d8e024ebcba5f144/Insurance%20Project_pages-to-jpg-0004.jpg)



## Insights & Storytelling

### Motor Insurance – Slight Underwriting Loss
- Claims: Frequency remained stable (~16%), but severity spiked in 2021 and stayed high. CSR fell from 90% to 80%, while CRR increased from 0% to 10%, weakening customer trust.

- Underwriting: From a narrow profit in 2020, Motor slipped into losses in 2021 and 2022. Expense ratios crept upward, pushing combined ratios above 100%.

- Renewals: Renewal ratios slid from 80% → 78% → 76%, with churn rate worsening. Average policy tenure peaked in 2021 but dropped sharply in 2022.

👉 Motor started as a stable performer but is now under strain: rising severity, falling settlement quality, and worsening retention (renewal ratio) make it a weak segment needing corrective action.

- For detailed year-by-year analysis, see [Motor Insurance (2020 - 2022)](#Detailed Analysis-motor-insurance-2020---2022)

------------------------------------------------------------------------------------------------------------------------------------------

### Health Insurance - Problem Product
- Claims: Health has consistently carried the highest claim frequency (25–27%) and high claim severity, pressuring loss ratios year after year. CSR fell steadily (80% → 76.9% → 75%), while pending claims and TAT compliance worsened.

- Underwriting: Combined Ratios remained well above 110%, ensuring continuous underwriting losses. Losses grew in 2021 before slightly easing in 2022, but profitability never recovered.

- Renewals: Retention deteriorated from 75% to below 70%, with churn rate increasing from 25% to 31%. Policy tenure peaked in 2021 but dropped sharply in 2022.
  
👉 Across three years, Health is clearly the insurer’s most problematic product: high claims frequency, weak claims handling efficiency, and worsening renewals make it unsustainable without corrective action. 

- For detailed year-by-year analysis, see 

------------------------------------------------------------------------------------------------------------------------------------------

### Life Insurance - Best Performing Product
- Claims: Life consistently reported very low claim frequency (6.7% → 6.1% → 5.6%), offsetting rising claim severity. CSR improved steadily from 90% to 92.6%, while CRR fell from 5% to 3.7%. TAT compliance remained the best among products (94–96%).

- Underwriting: Across three years, Life maintained the strongest profitability with Combined Ratios of 70–73%, driven by controlled Loss and Expense Ratios. Profits grew from $3.7M to $7.7M.

- Renewals: Retention remained exceptional (93–95%), churn rate consistently low (5–7%), and average tenure stable at ~9 years.
  
👉 Life is the insurer’s strongest and most reliable product: profitable, efficient, and highly stable, providing the backbone of portfolio performance.

- For detailed year-by-year analysis, see 




