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

- Slicers for filtering: Product, Claim Type, Region, Channel, Year

## Tools & Methods Used

Power BI: Dashboard creation, KPI cards, DAX measures (YoY, ratios).
Excel: Data preparation.
DAX: To create metrics needed for insights such as loss ratio , hit ratio , expense ratio etc.

## Data Preparation
- Removed duplicates, handled missing values.
- Created extra columns (e.g., Policy End Date, TAT(Days), TAT(Status), Renewal etc)
- Standardized formats for dates and categories.
- Added a Date Table to support time intelligence functions (YoY, trends).
- Cleaned inconsistent entries in Region/Channel/Product columns.
- Validated totals
- Used Renewal column (Yes/No) for Renewal Ratio and Churn Rate.

## DAX Measures Created (Power BI)

#### Total Claim Amount
``` Total Claim Amount = calculate(SUM(Claims2[Claim Amount]),filter(Claims2,Claims2[Claim Status]="Settled")) ```

#### Claims Settled
``` Claims Settled = calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled")) ```

#### Claim Settlement Ratio (CSR)
``` CSR = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled")),countrows(Claims2)) ```

#### Claims Rejected
``` Claims Rejected = calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Rejected")) ```

#### Claim Rejection Ratio (CRR)
``` CRR = DIVIDE([Claims Rejected],COUNTROWS(Claims2)) ```

#### Pending Claims
``` Pending Claims = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Pending")),countrows(Claims2)) ```

#### TAT Compliance
``` TAT Compliance = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[TAT Status]="Within TAT")),[Claims Settled]) ```

#### Claim Frequency
``` Claim Frequency = divide(COUNTROWS(Claims2), calculate(COUNTROWS(Policies1),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted"))) ```

#### Claim Severity
``` Claim Severity = divide([Total Claim Amount],CALCULATE(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled"))) ```

#### Gross Written Premium (GWP)
``` GWP = calculate(SUM(Policies1[Premium]),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted")) ```

#### Loss Ratio 
``` Loss Ratio = divide([Total Claim Amount],[GWP]) ```

#### Expense Ratio 
``` Expense Ratio = divide(SUM(Policies1[Undewriting Expense]),[GWP]) ```

#### Combined Ratio
``` Combined Ratio = ([Loss Ratio] + [Expense Ratio]) ```




