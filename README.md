# CreditScore-and-DataDrift
Note that all of the content here is present in the documentation which is downloadable. <br><br>
Problem Statement : <br>
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
• credit_history: Credit history<br>
• purpose: Purpose of loan<br>
• amount: Credit amount<br>
• savings_balance: Savings accounts/bonds<br>
• employment_length: Present employment since<br>
• installment_rate: installation rate in percentage of disposable income<br>
• personal_status: Personal status and sex<br>
• other_debtors: Other debtors <br>
• residence_history: Present residence since<br>
• property: Property<br>
• age: Age in years<br>
• instalment_plan: Other insatllment plans<br>
• housing: Housing<br>
• existing_credits: Number of existing credits in this bank<br>
• job: Job<br>
• dependents: Number of people being liable to provide maintenance for<br>
• telephone: Telephone<br>
• foreign worker: Foreign worker<br>
• default: Whether default or not<br><br>
The dataset had 22 features including the loan status and various attributes related to both borrowers and their 
payment behavior like checking_balance, credit_history, amount, employment_length, property, age etc.<br>
Initial data exploration revealed the following<br>
• There were 9 features that had continuous values and 13 features which had discrete values. <br>
• Our target variable was the column named default.<br>
• There were no missing values.<br><br>
Feature engineering<br>
There is huge amount of data. However not all the features available to us are useful. Features are selected 
based on the predictive strength of the feature. We can quantify the predictive power of a feature using the 
concept of Information Value. It measures the strength/ ability of the independent variables to separate two 
categorical (dependent) variables.<br>
Logistic regression requires all features to be numerical. So, the categorical variables should be transformed 
using one hot encoding or other techniques. WOE is another such method. Under each feature for every
category, we find the WOE.<br>
This concept is used in the preprocessing step. We use this to calculate the score.<br>
We calculate WOE for each unique value of a categorical variable and transform a continuous variable into 
discrete bins to calculate the WOE values for those bins.<br><br>
Benefits of using WOE transformation are <br>
1. It can handle missing values as these can be binned together.<br>
2. Outliers can be handled. The value that feeds into the model fitting is the WOE transformed value and 
not the raw value.<br>
3. It also handles categorical value so no need for dummy variables.<br>
4. It helps to build a strict linear relationship with log-odds used in logistic regression.<br><br>
Dummy example to explain WOE and IV<br>
![Screenshot 2023-11-14 171352](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/12b1036a-24f0-4fe5-8151-5c261eb32cb2)<br><br>
The weight of evidence tells the predictive power of a single feature concerning its independent feature. If any 
of the categories/bins of a feature has a large proportion of events compared to the proportion of non-events, 
we will get a high value of Woe which in turn says that that class of the feature separates the events from nonevents.<br>
For example, consider category C of the feature X in the above example, the proportion of events (0.16) is very 
small compared to the proportion of non-events (0.37). This implies that if the value of the feature X is C, it is 
more likely that the target value will be 0 (non-event). The Woe value only tells us how confident we are that 
the feature will help us predict the probability of an event correctly.<br><br>
Therefore<br>
![Screenshot 2023-11-14 171541](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/041b9e2c-7a90-4f11-86c2-29a92b44678d)<br><br>

Feature Selection using Information Value<br>
It measures the predictive power of independent variables which is useful in feature selection.<br>
![image](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/22396408-69f0-4b1a-aa8a-94eea125dd76)<br><br>
Model Fitting & Interpreting Results<br>
Now we fit a logistic regression model using our newly transformed WOE of the training dataset. When scaling 
the model into a scorecard, we will need both the Logistic Regression coefficients from model fitting as well as 
the transformed Woe values. <br><br>
Logistic regression<br>
Linear regression is not a good classication algorithm, that is where logistic regression comes in. Here there are 
two possible outputs and the independent variables are non collinear. The output is a probability and we 
consider a threshold to conclude the result. Unlike linear regression it fits a S shaped cuve on the dataset.<br><br>
![Screenshot 2023-11-14 171718](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/5dcfec12-56d1-4314-990d-1c19ba81985d)<br><br>
Thus the logistic regression model looks like this <br><br>
![image](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/01a5f3c9-323d-4370-aa65-fe0902ab9f2e)<br><br>
Here we don’t use the squared error cost function because otherwise we end up in local minimas if we use that.
Instead we use the special type of cost function that is derived using maximum likehood and is called the logistic 
loss function.<br>
![Screenshot 2023-11-14 171830](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/fe75a8ab-8941-4194-87e6-6723868f17ad)<br><br>
We minimize the cost function using gradient descent and hence we get the coefficient and the intercept.<br>
Coming back to our problem<br>
For each independent variable Xi, its corresponding score is :<br>
![Screenshot 2023-11-14 171917](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/b6a63b84-62bf-4abb-8b33-312bcac0fba6)<br><br>
![Screenshot 2023-11-14 171944](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/859e6e88-a21c-4f08-9b21-6a2890ad814d)<br><br><br>
PDO (Points to double the odds)<br>
How many points does the score change if the odds double i.e., from 100:1 to 200:1 to 400:1 and so on. <br>
20 pdo means odds will double with each 20 points<br><br>
LOC (line of credit)<br>
A line of credit refers to the maximum amount of credit that a lender or financial institution is willing to extend 
to a borrower.<br><br>
oddTODO<br>
Good:bad ratio. Note that in credit risk monitoring odds means good:bad (because we want higher scores to be 
given to customers having good risk and bad scores to be given to customer having bad risk).<br><br>
Want Odds<br>
A particular odds ratio of Good and Bad e.g., 10:1 or 50:1<br><br>
Our work was fully done using R program.<br>
Libraries used<br>
• R shiny<br>
Shiny is an R library that enables building interactive web applications that can execute R code on the 
backend.<br>
• DT<br>
The library (DT) statement loads the DT package which provides functionalities for creating interactive 
data tables in the shiny application.<br>
• Data.table<br>
R package provides an enhanced version of data. frame that allows us to do blazing fast data 
manipulations.<br><br>
This is how the dataset looked like<br><br>
![Screenshot 2023-11-14 172432](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/8b827272-5ad8-403b-9171-6fc0627b9bef)<br><br>
Out target or the dependent variable was ‘default’ which had values Good and Bad. This refers whether the 
person has returned the loan or not.<br><br>
To get the descriptive statistics about the data we created a function get_summary_table then those data were 
stored in a table called info.<br>
We ended up with something like this<br>
![Screenshot 2023-11-14 172526](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/0382ca78-72bd-441d-a84c-4fa683ce0833)<br><br><br>
Next, we created a function called get_cuts which helped us to bin up the continuous variables and the discrete 
variables. Then using those we created a list called get_cuts_list which helped us to store all the bins for each 
variable.<br><br>
![Screenshot 2023-11-14 172601](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/bb003b73-e988-4acf-a99e-031b15d1b60a)<br><br>
Where the bins for each variable looks like<br>
![Screenshot 2023-11-14 172632](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/b73dab52-776c-4a89-8a0c-16327dcd27e9)<br><br>
We now create a function called woe_infoval which helped us to calculate the woe’s along with that another 
function get_data_cut which helped us to convert out initial table into one containing only bins.<br><br>
![Screenshot 2023-11-14 172707](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/4b52aef7-94e8-486b-a3ce-ca7e2a255f66)<br><br>
Hence, we get the data table df_woe which contains all the woe values <br>
![Screenshot 2023-11-14 172750](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/33ceff95-3d1b-487c-ba09-b3c69c73a55c)<br><br>
Then I created a list called rv1 that contained DFCUT, DFWOE and WOE_IV for future use that contained the bins 
we created for each feature, the WOE values for each category and the iv values.<br>
![Screenshot 2023-11-14 172835](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/8ea77a1b-3c17-47b4-b502-2f50a154f2bb)<br><br>
Finally, we will fit a logistic regression model using our newly transformed WoE of the training dataset. When 
scaling the model into a scorecard, we will need both the Logistic Regression coefficients from model fitting as 
well as the transformed WoE values. We use the glm function (general linear model function) to train the model 
with the data from the WOE table. The family is binomial which indicates that it is a logistic regression problem. 
We store the coefficients of the features from the model_summary in a variable coef.<br>
![Screenshot 2023-11-14 172909](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/73285683-b818-42e5-8d14-aacf9b6c48f6)<br><br>
The final piece of our puzzle is creating a simple, easy-to-use, and implement credit risk scorecard that can be 
used by any layperson to calculate an individual’s credit score given certain required information about him and 
his credit history. For each attribute, its Weight of Evidence (WoE) and the regression coefficient of its 
characteristic now could be multiplied to give the score points of the attribute. Then scaling is done with the PDO, 
LOC and want odds ratio. <br>
We choose to scale the points such that a total score of 600 points corresponds to good/bad odds of 50 to 1 and 
an increase of the score of 20 points corresponds to a doubling of the good/bad odds.<br>
Then we created a function called scard to store the correspond score for each category for all of the variables.<br>
![Screenshot 2023-11-14 173017](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/a0343f9a-3ade-43a6-9348-e3563dd5d891)<br><br>
The score is just the summation from the score card for each person.<br>
Min_sum is the sum of the minimum scores in each feature and maxsum is the sum of the maximum scores in 
each feature for the observation, this helps us to visualize the score range that we want to set.<br>
So, the scorecard is generated. Now I built an app using R shiny named the Scorecard scaling. Here I change the 
inputs to get a desired minimum and maximum score. Once set, this scorecard will be the final one with the help 
of which we will create the new person’s score which will be in our desired range.<br><br>
![Screenshot 2023-11-14 173109](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/fb08f1dd-e78c-4930-accd-72b149dede6c)<br><br>
Ui and server<br>
A simple shiny app is a directory containing two R scripts/files, one is ui. R , which controls the layout and 
appearance of our app, the other is server. R , which contains the instructions that our computer needs to build 
our app.<br><br>
In ui, we have the siderbar panel and the mainpanel. In the siderbar, for specifying numerical values in ‘Points 
to double the odds’,’at the score of’ and ‘want odds ratio to be’ I used numericInput function. I used action 
button for ‘View Minimum-Maximum Score’. In the mainpanel I used htmlOutput to view the maximum score 
and the minimum score calculated in the server. Also, I gave an action button to view the score card using 
DTOutput, a download button to download the desired scorecard.<br>
In the server using previous functions I calculated the minimum and the maximum score and used observeEvent 
to display the calculated value only after the action tab was used. Again, another observeEvent was added to 
show the modified score card only after the button ViewScore card was used. I used the downloadhandler to 
specify what to download and in which format. <br><br>
PSI (Population stability Index)<br>
In machine learning, data drift refers to a change in the distribution of input data that can impact the 
performance of a trained model. It occurs when the statistical properties of the incoming data change over time, 
leading to a discrepancy between the data the model was trained on and the data it encounters during inference.<br>
Data drift can occur due to various reasons, such as changes in user behavior, changes in the underlying 
population, shifts in data sources or changes in the data collection process. When data drift happens, the model 
may become less accurate or even produce incorrect predictions.<br>
Detecting data drift is crucial for maintaining model performance.<br>
Thus, it measures to what extent the population that was used to construct the rating system is similar to the 
population that is currently being observed. The lower, the better stability of the model.<br><br>
PSI = Sum [ (A-D)*Log(A/D) ]<br>
Where,<br>
A= Population distribution, actual sample (observed data, at time t)<br>
D= Population distribution, development or training sample (when developed the model (t0))<br>
General convention used in the industry:<br>
PSI < 0.1 means no significant<br>
0.10<=PSI<0.25 means moderate shift<br>
PSI >= 0.25 means significant shift<br>
Created the PSI function that could calculate the PSI for the scores as well as for individual variables. Used the 
previously built functions and finally using the rshiny we got this.<br>
![Screenshot 2023-11-14 173357](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/724bbec1-e8a4-4625-88e2-613c639e9390)<br><br>
![Screenshot 2023-11-14 173421](https://github.com/Atrayeedgupta1/CreditScore-and-DataDrift/assets/109009826/15e940ab-6bf7-4ea2-bb6c-69eedfc85a97)<br><br>
In the siderbarPanel I used inputid to upload the train and the test data also to mention the id column and the 
target column based on our problem statement, selectInput to select between score and variable and an action 
button to display the PSI table. In the server the function that we created is called and is used to calculate the PSI 
and is based on what we selected on the check stability select input box.<br>
































