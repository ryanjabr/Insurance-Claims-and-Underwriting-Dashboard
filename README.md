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

- Power BI: Dashboard creation, KPI cards, DAX measures (YoY, ratios).
- Excel: Data preparation.
- DAX: To create metrics needed for insights such as loss ratio , hit ratio , expense ratio etc.

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
``` 
Total Claim Amount = calculate(SUM(Claims2[Claim Amount]),filter(Claims2,Claims2[Claim Status]="Settled"))
```

#### Claims Settled
``` 
Claims Settled = calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled"))
```

#### Claim Settlement Ratio (CSR)
``` 
CSR = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled")),countrows(Claims2))
```

#### Claims Rejected
``` 
Claims Rejected = calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Rejected"))
```

#### Claim Rejection Ratio (CRR)
``` 
CRR = DIVIDE([Claims Rejected],COUNTROWS(Claims2))
```

#### Pending Claims
``` 
Pending Claims = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Pending")),countrows(Claims2))
```

#### TAT Compliance
``` 
TAT Compliance = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[TAT Status]="Within TAT")),[Claims Settled])
```

#### Claim Frequency
``` 
Claim Frequency = divide(COUNTROWS(Claims2), calculate(COUNTROWS(Policies1),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted")))
```

#### Claim Severity
``` 
Claim Severity = divide([Total Claim Amount],CALCULATE(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Settled")))
```

#### Gross Written Premium (GWP)
``` 
GWP = calculate(SUM(Policies1[Premium]),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted"))
```

#### Loss Ratio 
``` 
Loss Ratio = divide([Total Claim Amount],[GWP])
```

#### Expense Ratio 
``` 
Expense Ratio = divide(SUM(Policies1[Undewriting Expense]),[GWP])
```

#### Combined Ratio
``` 
Combined Ratio = ([Loss Ratio] + [Expense Ratio])
```

#### Hit Ratio
``` 
Hit Ratio = divide(calculate(COUNTROWS(Policies1),FILTER(Policies1,Policies1[Proposal Status (Insurer)]="Accepted")),countrows(Policies1))
```

#### Profit / Loss
``` 
Profit/Loss = [GWP]-[Total Claim Amount]- SUM(Policies1[Undewriting Expense])
```

#### Total Proposals
``` 
Total Proposals = COUNTROWS(Policies1)
```

#### Policies Issued
``` 
Policies Issued = CALCULATE(COUNTROWS(Policies1),Policies1[Proposal Status (Insurer)] = "Accepted")
```

#### Renewal Ratio
```
Renewal Ratio = divide(calculate(countrows(Policies1),filter(Policies1,Policies1[Renewal]="Yes" && Policies1[Proposal Status (Insurer)]="Accepted")),calculate(countrows(Policies1),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted")))
```

#### Churn Rate
``` 
Churn Rate = divide(calculate(countrows(Policies1),filter(policies1,Policies1[Renewal]="No")),CALCULATE(countrows(Policies1),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted")))
```

#### Year-Over-Year (YOY) Total Claim Amount
```
YOY Total Claim Amount = 
VAR CURRENT_TOTAL_CLAIM_AMOUNT = [Total Claim Amount]
VAR PREVIOUS_TOTAL_CLAIM_AMOUNT = CALCULATE([Total Claim Amount],DATEADD(DateTable[Date], -1, YEAR))
RETURN
IF(ISBLANK(PREVIOUS_TOTAL_CLAIM_AMOUNT),BLANK(),DIVIDE(CURRENT_TOTAL_CLAIM_AMOUNT - PREVIOUS_TOTAL_CLAIM_AMOUNT, PREVIOUS_TOTAL_CLAIM_AMOUNT,0))
```

#### YOY CSR 
```
YOY CSR = 
VAR CURR_csr = [CSR]
VAR PREV_csr = CALCULATE(DIVIDE([Claims Settled],[total claims]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_csr),BLANK(),DIVIDE(CURR_csr - PREV_csr, PREV_csr,0))
```

#### YOY CRR
```
YOY CRR = 
VAR CURR_CRR = [CRR]
VAR PREV_CRR = CALCULATE(DIVIDE([Claims Rejected],[total claims]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_CRR),BLANK(),DIVIDE(CURR_CRR - PREV_CRR, PREV_CRR,0))
```

#### YOY Pending Claims
```
YOY Pending Claims = 
VAR CURR_PENDING = [Pending Claims]
VAR PREV_PENDING = CALCULATE(DIVIDE([Pending Claims No.],[total claims]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_PENDING),BLANK(),DIVIDE(CURR_PENDING - PREV_PENDING, PREV_PENDING,0))
```

### YOY TAT Compliance
```
YOY TAT Compliance = 
VAR CURR_TAT_COMPLIANCE = [TAT Compliance]
VAR PREV_TAT_COMPLIANCE = CALCULATE([TAT Compliance],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_TAT_COMPLIANCE),BLANK(),DIVIDE(CURR_TAT_COMPLIANCE - PREV_TAT_COMPLIANCE, PREV_TAT_COMPLIANCE,0))
```

#### YOY Claims Frequency 
```
YOY Claims Frequency = 
VAR CURR_CLAIMS_FREQ = [Claim Frequency]
VAR PREV_CLAIMS_FREQ = 
CALCULATE(
    DIVIDE([total claims], [Accepted Policies], 0),
    SAMEPERIODLASTYEAR(DateTable[Date])
)
RETURN
IF(ISBLANK(PREV_CLAIMS_FREQ),BLANK(),DIVIDE(CURR_CLAIMS_FREQ - PREV_CLAIMS_FREQ, PREV_CLAIMS_FREQ, 0))
```

#### YOY Claims Severity
```
YOY Claims Severity = 
VAR CURR_CLAIMS_SEVERITY = [Claim Severity]
VAR PREV_CLAIMS_SEVERITY = CALCULATE(DIVIDE([Total Claim Amount],[Claims Settled]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_CLAIMS_SEVERITY),BLANK(),DIVIDE(CURR_CLAIMS_SEVERITY - PREV_CLAIMS_SEVERITY, PREV_CLAIMS_SEVERITY,0))
```

#### YOY GWP
```
YOY GWP = 
var CURRENTGWP = [GWP]
VAR PREVIOUSGWP = CALCULATE([GWP],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREVIOUSGWP),BLANK(), DIVIDE(CURRENTGWP - PREVIOUSGWP , PREVIOUSGWP , 0))
```



