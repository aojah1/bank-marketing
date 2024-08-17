# bank-marketing


## 1) Business Understanding
### Objective:
The goal is to predict whether a customer will subscribe to a term deposit (target variable) based on various features (e.g., age, job, balance). The bank wants to optimize its marketing strategy by focusing on customers most likely to subscribe.

### Key Questions:
What are the target market segments for the Portuguese banking institution, and what shoud be customer retention policies

### Success Criteria:
Provide options to the Portuguese banking institution to optimize its marketing strategies, reduce costs, and increase the conversion rates of its marketing campaigns, thereby improving overall profitability.

## 2. Data Understanding
### Objective: 
Familiarize with the dataset, understand the features, and assess the target variable.

### 2.1 Data Collection
The data is related with direct marketing campaigns of a Portuguese banking institution. The marketing campaigns were based on phone calls. Often, more than one contact to the same client was required, in order to access if the product (bank term deposit) would be (or not) subscribed.

Total number of dataset : 45,211

Total number of features : 16

Target variable : has the client subscribed a term deposit? (binary: "yes","no")

### 2.2 Exploratory Data Analysis (EDA)
To make data exploration easy for the first iteration, I am just loading 10,00 random rows into a dataframe
#### bank client data:
   1 - age (numeric)
   2 - job : type of job (categorical: "admin.","unknown","unemployed","management","housemaid","entrepreneur","student",
                                       "blue-collar","self-employed","retired","technician","services") 
   3 - marital : marital status (categorical: "married","divorced","single"; note: "divorced" means divorced or widowed)
   4 - education (categorical: "unknown","secondary","primary","tertiary")
   5 - default: has credit in default? (binary: "yes","no")
   6 - balance: average yearly balance, in euros (numeric) 
   7 - housing: has housing loan? (binary: "yes","no")
   8 - loan: has personal loan? (binary: "yes","no")
#### related with the last contact of the current campaign:
   9 - contact: contact communication type (categorical: "unknown","telephone","cellular") 
  10 - day: last contact day of the month (numeric)
  11 - month: last contact month of year (categorical: "jan", "feb", "mar", ..., "nov", "dec")
  12 - duration: last contact duration, in seconds (numeric)
#### other attributes:
  13 - campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)
  14 - pdays: number of days that passed by after the client was last contacted from a previous campaign (numeric, -1 means client was not previously contacted)
  15 - previous: number of contacts performed before this campaign and for this client (numeric)
  16 - poutcome: outcome of the previous marketing campaign (categorical: "unknown","other","failure","success")

  Output variable (desired target):
  17 - y - has the client subscribed a term deposit? (binary: "yes","no")
  
### 2.3 Features Exploration

Plot categorical and numerical features data distribution.

## 3. Data Preparation
### Objective: 
Prepare the data for modeling. This involves handling categorical variables, scaling numerical features, and splitting the data.
As part of the process, I first did a check for Collinearity among Categorical Variables. Hare is an output --> 

![Correlation_Matrix](https://github.com/user-attachments/assets/d9237683-ff59-4373-a1be-7ccc868e2d49)


Based on tha above data, I decided to continue with the following categorical featuers
['loan','housing','default','education','job', 'marital', 'education']


### 4. Modeling
#### Objective: 
Build and evaluate models using different classifiers within pipelines using the following :

['num__age' 'num__balance' 'num__day_of_week' 'num__duration'
 'num__campaign' 'num__pdays' 'num__previous' 'cat__loan_yes'
 'cat__housing_yes' 'cat__default_yes' 'cat__education_secondary'
 'cat__education_tertiary' 'cat__education_nan' 'cat__job_blue-collar'
 'cat__job_entrepreneur' 'cat__job_housemaid' 'cat__job_management'
 'cat__job_retired' 'cat__job_self-employed' 'cat__job_services'
 'cat__job_student' 'cat__job_technician' 'cat__job_unemployed'
 'cat__job_nan' 'cat__marital_married' 'cat__marital_single'
 'cat__education_secondary' 'cat__education_tertiary' 'cat__education_nan']

classifiers = {
    'K-Nearest Neighbors': KNeighborsClassifier(),
    'Logistic Regression': LogisticRegression(max_iter=1000),
    'Decision Tree': DecisionTreeClassifier(),
    'Support Vector Machine': SVC(probability=True)
}

Here are the results -->

<img width="515" alt="model_accuracy_comparision" src="https://github.com/user-attachments/assets/a000be3d-6e10-4c9b-9add-4457929823d1">


### 5. Evaluation
### Objective: 

Compare the performance of the models using various metrics and below is a plot for the accuracy of each model -->

#### 5.1 Cross-Validation
Using cross-validation, we can further assess the model's generalization performance. Cross-validation provides a more reliable estimate of a model's performance on unseen data by averaging results over multiple folds.

I did a 5 fold  cross validations using the following rules - 

If the training accuracy is significantly higher than the test accuracy (e.g., by more than 10%), the model might be overfitting.
If the test accuracy is higher than the training accuracy, the model might be underfitting (not capturing enough complexity).

Here are the results for 5 fold CV -->

Evaluating K-Nearest Neighbors...
Training Accuracy: 0.9153
Test Accuracy: 0.8822
Cross-Validation Accuracy (mean of 5 folds): 0.8870
Model is performing consistently between training and test sets.

Evaluating Logistic Regression...
Training Accuracy: 0.8922
Test Accuracy: 0.8881
Cross-Validation Accuracy (mean of 5 folds): 0.8919
Model is performing consistently between training and test sets.

Evaluating Decision Tree...
Training Accuracy: 1.0000
Test Accuracy: 0.8550
Cross-Validation Accuracy (mean of 5 folds): 0.8553
Potential overfitting detected: Training accuracy significantly higher than test accuracy.

Evaluating Support Vector Machine...
Training Accuracy: 0.9020
Test Accuracy: 0.8900
Cross-Validation Accuracy (mean of 5 folds): 0.8928
Model is performing consistently between training and test sets.

### 6 Interpretation of Results
Based on the comparision done above, Vector Machine Support is found to be the best as it came out on the top while model evaluation and cross validation.
See data below for evidence -->

For Vector Machine Support, Cross-Validation Accuracy (mean of 5 folds): 0.9020
For Vector Machine Support - Accuracy: 0.8919

#### 6.1 Visualize the classification methods

We will only focus on Vector Machine Support for further analysis.

<img width="551" alt="svm_age_duration" src="https://github.com/user-attachments/assets/ac86f9eb-0af7-46c9-8e11-23efb2ce7c9a">
<img width="872" alt="svm_age_balance" src="https://github.com/user-attachments/assets/1a67c9b6-abd6-40bc-8a5b-43cbf8a090f6">

### 7 Features with significant impact
To calculate feature importance using permutation importance with the Support Vector Machine (SVM) classifier, I will apply permutation importance to the trained SVM model to identify the most important features.

Here are the results -->

<img width="395" alt="Screenshot 2024-08-17 at 6 46 11 PM" src="https://github.com/user-attachments/assets/9e8adb44-937b-41f9-a954-bf1fe72e675f">

### Conclusion

The goal was to build and evaluate several classification models to predict customer responses (e.g., whether a customer would subscribe to a term deposit) based on various features in the Bank Marketing dataset.

My analysis found Support Vector Machine as the most accurate and efficient clasification model.

Factors such as day of the week you are calling in, age, job status, and defaulter has to be the target personas while calling a prospect and convincing him/her for a term deposit in the first call.

But the top three features that has Portuguese banking institution should target are - 

1) has housing a loan
2) type of job
3) has credit in default








