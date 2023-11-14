# CreditScore-and-DataDrift
Utilized logistic regression and utilized WOE to transform and analyze data to generate score cards. Also developed an app using R shiny for deployment to streamline scorecard range setting using PDO, LOC , wantOdds and detected data drift using PSI.<br><br>
The problem aims to show the maximum score and the minimum score along with the Score card after we enter 
the PDO, LOC and Want Odds. After adjusting the values, one can set the desired score range and get the 
corresponding score card and download it. Now when a new individual comes, with the help of this score card 
we can generate his credit score. If this score lying in the range is more than a cutoff the person gets the loan.
Also view the PSI table to check whether there is a data drift.<br><br>
Credit score:<br>
A credit score is a numeric representation of an individual’s creditworthiness or the ability to repay the debt on 
time. It is the result of a statistical model, based on information about the borrower. Credit scores usually lies in 
a range which may vary on the company. Higher the score the individual gets, the chances of getting the loan 
increases. They are also used to determine the interest rate and the credit limit one receives. <br><br>
About the dataset<br>
• Customer: Customer ID<br>
• checking_balance: Status of existing checking account<br>
• months_loan_duration: Duration in month <br>
• credit_history: Credit history
• purpose: Purpose of loan
• amount: Credit amount
• savings_balance: Savings accounts/bonds
• employment_length: Present employment since
• installment_rate: installation rate in percentage of disposable income
• personal_status: Personal status and sex
• other_debtors: Other debtors 
• residence_history: Present residence since
• property: Property
• age: Age in years
• instalment_plan: Other insatllment plans
• housing: Housing
• existing_credits: Number of existing credits in this bank
• job: Job
• dependents: Number of people being liable to provide maintenance for
• telephone: Telephone
• foreign worker: Foreign worker
• default: Whether default or not


