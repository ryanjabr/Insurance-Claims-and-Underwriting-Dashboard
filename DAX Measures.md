# Insurance Performance Dashboard - Havencrest Life & General

## DAX Measures Created (Power BI)

#### Total Claims 
```
total claims = COUNTROWS(Claims2)
```

#### Settled Claim Amount 
```
Settled Claim Amount = calculate(sum(Claims2[Claim Amount]),filter(Claims2,Claims2[Claim Status]="Settled"))
```

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

#### Pending Claims No. 
```
Pending Claims No. = calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Pending"))
```

#### Pending Claims
``` 
Pending Claims = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[Claim Status]="Pending")),countrows(Claims2))
```

#### TAT Compliance
``` 
TAT Compliance = divide(calculate(countrows(Claims2),filter(Claims2,Claims2[TAT Status]="Within TAT")),[Claims Settled])
```

#### Accepted Policies 
```
Accepted Policies = 
CALCULATE(
    DISTINCTCOUNT(Policies1[Policy ID]),
    Policies1[Proposal Status (Insurer)] = "Accepted"
)
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

#### Renewals (Yes)
```
Renewals (Yes) = calculate(countrows(Policies1),filter(Policies1,Policies1[Proposal Status (Insurer)]="Accepted"&&Policies1[Renewal]="Yes"))
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

#### Current Loss Ratio
```
Current loss ratio = [Loss Ratio]
```

#### Previous Loss Ratio
```
Prev Loss Ratio = 
CALCULATE (
    DIVIDE ( [Total Claim Amount], [GWP] ),
    SAMEPERIODLASTYEAR ( DateTable[Date] )
)
```

#### YOY Loss Ratio 
```
YOY Loss Ratio = divide([Current loss ratio] - [Prev Loss Ratio],[Prev Loss Ratio],0)
```

#### YOY Expense Ratio 
```
YoY Expense Ratio = 
var curr_expenseratio = [Expense Ratio]
var prev_expenseratio = calculate([Expense Ratio],SAMEPERIODLASTYEAR(DateTable[Date]))
return
if(isblank(prev_expenseratio),blank(),divide(curr_expenseratio - prev_expenseratio, prev_expenseratio, 0))
```

#### YOY Combined Ratio
```
YOY Combined Ratio = 
VAR CURRENT_COMBINED_RATIO = [Loss Ratio]+[Expense Ratio]
VAR PREVIOUS_COMBINED_RATIO = CALCULATE(([Loss Ratio]+[Expense Ratio]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREVIOUS_COMBINED_RATIO),BLANK(),DIVIDE(CURRENT_COMBINED_RATIO - PREVIOUS_COMBINED_RATIO , PREVIOUS_COMBINED_RATIO , 0))
```

#### YOY Hit Ratio
```
YOY Hit Ratio = 
VAR CURRENT_HIT_RATIO = [Hit Ratio]
VAR PREVIOUS_HIT_RATIO = calculate(divide([Policies Issued],[Total Proposals]),SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREVIOUS_HIT_RATIO),BLANK(),DIVIDE(CURRENT_HIT_RATIO - PREVIOUS_HIT_RATIO , PREVIOUS_HIT_RATIO , 0))
```

#### YOY Profit / Loss 
```
YOY Underwriting Profit/Loss = 
VAR CURR_UWPL = [Profit/Loss]
VAR PREV_UWPL = CALCULATE([Profit/Loss],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_UWPL),BLANK(),DIVIDE(CURR_UWPL - PREV_UWPL, PREV_UWPL, 0))
```

#### YOY Total Proposals 
```
YOY Total Proposals = 
VAR CURR_TOTAL_PROPOSALS = [Total Proposals]
VAR PREV_TOTAL_PROPOSALS = CALCULATE([Total Proposals],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_TOTAL_PROPOSALS),BLANK(),DIVIDE(CURR_TOTAL_PROPOSALS - PREV_TOTAL_PROPOSALS, PREV_TOTAL_PROPOSALS, 0))
```

#### YOY Policies Issued
```
YOY Policies Issued = 
VAR CURR_POLICIES_ISSUED = [Policies Issued]
VAR PREV_POLICIES_ISSUED = CALCULATE([Policies Issued],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_POLICIES_ISSUED),BLANK(),DIVIDE(CURR_POLICIES_ISSUED - PREV_POLICIES_ISSUED,PREV_POLICIES_ISSUED,0))
```

#### YOY Renewal Ratio
```
YOY Renewal Ratio = 
VAR CURR_RENEWALRATIO = [Renewal Ratio]
VAR PREV_RENEWALRATIO = CALCULATE([Renewal Ratio],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_RENEWALRATIO),BLANK(),DIVIDE(CURR_RENEWALRATIO-PREV_RENEWALRATIO, PREV_RENEWALRATIO,0))
```

#### YOY Churn Rate 
```
YOY Churn Rate = 
VAR CURR_CHURNRATE = [Churn Rate]
VAR PREV_CHURNRATE = CALCULATE([Churn Rate],SAMEPERIODLASTYEAR(DateTable[Date]))
RETURN
IF(ISBLANK(PREV_CHURNRATE),BLANK(),DIVIDE(CURR_CHURNRATE - PREV_CHURNRATE, PREV_CHURNRATE, 0))
```

#### Total Underwriting Expense
```
total underwriting expense = sum(Policies1[Undewriting Expense])
```

