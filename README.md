# Classifcation Models: Predicting Company Bankruptcy

The Taiwan Economic Journal bankruptcy dataset consists of 6819 rows of company data with 96 attributes that represent aspects of a company’s financial health. The classification modeling exercise will predict whether the company is likely to go bankrupt, with the response variable as “Bankrupt?” The paper "Financial Ratios and Corporate Governance Indicators in Bankruptcy Prediction" reviewed the same dataset and found that attributes related to profitability and solvency were the most important predictors of whether a company would go bankrupt. The model exploration below will focus on this subset of 26 features.  

Profitability Features:  
* ROA C before interest and depreciation before interest
-	ROA (B) before interest and depreciation after tax
-	ROA (A) before interest and % after tax
-	Realized Sales Gross Margin
-	Gross Profit to Sales
-	Operating Profit Rate
-	Persistent EPS in the Last Four Seasons

Solvency Features  
-	Interest-bearing debt interest rate
-	Cash Reinvestment %
-	Current Ratio
-	Quick Ratio
-	Interest Expense Ratio
-	Current Liability to Assets
-	Operating Funds to Liability
-	Inventory/Working Capital
-	Current Liabilities/Liability
-	Working Capital/Equity
-	Current Liabilities/Equity
-	Long-term Liability to Current Assets
-	Quick Assets/Total Assets
-	Current Assets/Total Assets
-	Cash/Total Assets
-	Quick Assets/Current Liability
-	Cash/Current Liability
-	Equity to Liability

### Exploratory Data Analysis

Missing Values/Outliers  
No missing values were found in the data. However, an initial glance at the data shows the potential of outliers, since most of the attributes are ratios. A ratio attribute would indicate that values should fall between 0-1 or 0-100, however, the following features contain values in the billions:
* Interest-bearing interest rate, Current Ratio, Quick Ratio, Quick Assets/Current Liability, Cash/Current Liability, Long-term Liability to Current Assets

Countplot of Bankrupt?  
The countplot shows that there is a high occurrence of non-bankruptcy within the dataset, with only 220 rows (3% of the data) occurring as Bankrupt.

![Countplot bankrupt](https://user-images.githubusercontent.com/49419673/151727048-fc4ca527-55eb-4b88-bf21-f249c31a29a0.png)

Correlation of Features to Bankrupt?  
Below are the features with the highest and lowest correlation to Bankrupt?, and several from the Profitability and Solvency groups have strong positive and negative correlations to Bankrupt?. Notable positive correlation to the response variable includes Current Liability to Assets and Current Liabilities/Equity. Features with strong negative correlation include ROA A before interest and % after tax, ROA B before interest and depreciation after tax, ROA C before interest and depreciation before interest, and Persistent EPS in the Last Four Seasons.

Positive correlation to Bankrupt?  
![Correlation to Bankrupt top](https://user-images.githubusercontent.com/49419673/151727133-202e4707-f214-4421-9987-5fa37d6e5938.png)

Negative correlation to Bankrupt?  
![Correlation to Bankrupt bottom](https://user-images.githubusercontent.com/49419673/151727159-ee00f82d-2f45-4ea0-8e77-f1d46267b695.png)

Exploring Features with Potential Outliers  
-	Interest-bearing interest rate, Current Ratio, Quick Ratio, Quick Assets/Current Liability, Cash/Current Liability, Long-term Liability to Current Assets  

Each feature in the above list was reviewed for occurences where values in the feature were greater than 1 since they reflect ratios. Out of this list, only Cash/Current Liability had any occurrences of bankruptcy, with 15 occurrences out of 41 total rows where Cash/Current Liability was greater than 1. Since the majority of these rows had Bankruptcy equal to 0, these outlier rows were dropped.  
Total outlier rows dropped: 382  
Total rows remaining in the dataset: 6437  

Exploring Features for Relationship to Average Likelihood of Bankruptcy  
The histograms below show that features Current Ratio, Quick Ratio, Persistent EPS in the Last Four Seasons, and Current Liability to Assets show differentiation amongst values in the feature and the average occurrence of bankruptcy.  
-	Current Ratio values greater than 0.1 have a higher likelihood of bankruptcy at 50%
-	Quick Ratio values greater than 0.1 also have a higher likelihood of bankruptcy
-	Current Liability to Assets values greater than 0.3 have higher likelihood of bankruptcy
-	Persistent EPS in the Last Four Seasons values less than 0.17 have higher likelihood of bankruptcy

![Current Ratio under 1 histogram](https://user-images.githubusercontent.com/49419673/151727339-9b21310e-a11c-4048-9098-6a51d915b192.png)
![Quick Ratio_under 1 histogram](https://user-images.githubusercontent.com/49419673/151727347-061eab96-4f39-4827-bdf0-52e666075f65.png)
![Histogram Current Liability to Assets](https://user-images.githubusercontent.com/49419673/151727357-510b868a-6e54-400d-977a-39b1a63fd1a1.png)
![Histogram Persistent EPS](https://user-images.githubusercontent.com/49419673/151727367-89076406-ff47-43d4-a3ba-bdbb63a188b4.png)

Conversely, the histograms of the other features did not show differentiation with relationship to bankruptcy. The histogram for Interest-bearing debt interest rate below shows that all of the values for Interest-bearing debt interest rate produce an average bankruptcy less than 0.07.  
![Histogram interest bearing](https://user-images.githubusercontent.com/49419673/151727548-0f90605f-2d78-4c14-996f-ceaeaa58d371.png)

### Model Evaluation

Training and Validation  
The dataset was split into train and validation sets, allocating 20% of the dataset to the validation set.

Classification models explored include Naïve Bayes, Logistic Regression, SVM Support Vector Classification model with linear kernel and hyperparameters tuned. After using Randomized Search CV to find the optimal values for C and Gamma, the optimal values obtained were C: 6.488, Gamma: 0.027).  
Goodness of fit metrics produced for the training and validation sets: True Positive Rate, False Positive Rate, Precision, Recall, Accuracy. ROC and Precision/Recall graphs were created for the validation sets. The F1 score was used to assess model performance on the validation set.  

A dummy classifier model using the “most frequent” strategy was run to review the occurrence of Bankrupt vs. Not Bankrupt within the edited dataset. The accuracy score produced 96.3%, indicating that 96.3% of the rows in the dataset were Not Bankrupt.  

Goodness of Fit Metrics  
Accuracy was the lowest in the Naïve Bayes model, achieving 81.8% accuracy in the validation set, while the other three models achieved the highest accuracy possible at 96.3% in the validation set. Since accuracy is higher in the training sets, this exhibits overfitting in the models.  
The Naïve Bayes model also produced the highest True Positive and False Positive rates. The Logistic Regression and SVM models predicted 0 or 1 instances of bankruptcy, so the True Positive and False Positive rates were very low, ranging from 0 – 7%. Precision in the Logistic Regression and SVM models ranged from 0 – 100% due to 0 or 1 instances of bankruptcy predicted.  

Goodness of Fit Metrics on Train Set  
| Model                                       | Accuracy | True Positive Rate/Recall | False Positive Rate | Precision |
| --------------------------------------------| -------- | ------------------------- | ------------------- | --------- |
| Naive Bayes                                 | 82.5%    | 79.5%                     | 17.4%               | 12.1%     |
| Logistic Regression                         | 97.2%    | 7.3%                      | 0.1%                | 68.7%     |
| SVM - Linear Kernel default                 | 97.1%    | 0%                        | 0%                  | 0%        |
| SVM - Linear Kernel (C: 6.488, Gamma 0.027) | 97.1%    | 0.7%                      | 0%                  | 100%      |

Goodness of Fit Metrics on Validation Set
| Model                                       | Accuracy | True Positive Rate/Recall | False Positive Rate | Precision |
| --------------------------------------------| -------- | ------------------------- | ------------------- | --------- |
| Naive Bayes                                 | 81.8%    | 81.3%                     | 18.1%               | 14.8%     |
| Logistic Regression                         | 96.3%    | 2.1%                      | 0.08%               | 50%       |
| SVM - Linear Kernel default                 | 96.3%    | 0%                        | 0%                  | 0%        |
| SVM - Linear Kernel (C: 6.488, Gamma 0.027) | 96.3%    | 0%                        | 0%                  | 0%        |

ROC Curves  
ROC curves were not produced for both SVM models on the validation sets because 0 instances of bankruptcy were predicted.

![Bayes_ROC](https://user-images.githubusercontent.com/49419673/151728445-1f0ff736-1b1b-4a57-895c-e1c80efc4a92.png)
![Logistic_ROC](https://user-images.githubusercontent.com/49419673/151728460-beeab547-f82d-4869-9c6a-66267408ee8f.png)

Precision-Recall Tradeoff  
The Naïve Bayes model performs the worst in optimizing Precision and Recall, as the highest Precision achievable reaches only 30%, when Recall hits 25%. The SVM models have similar Precision-Recall Curves, and the tuned SVM model has the highest achievable balance of precision and recall with precision at under 60% and recall at 30%.   

![Bayes_precisionrecall](https://user-images.githubusercontent.com/49419673/151728479-ad441e4b-0493-43bc-ae4e-8f615d9f1e29.png)
![Logistic_precisionrecall](https://user-images.githubusercontent.com/49419673/151728494-74cc4aa1-d6c3-42cd-b1fc-6d17a96c26dc.png)

![svc_no tuning precision recall](https://user-images.githubusercontent.com/49419673/151728508-6ed6c0e1-ff63-4cad-ae4b-2e7c17a45ef6.png)
![svc2_precision recall](https://user-images.githubusercontent.com/49419673/151728513-79fd3f2a-b682-42db-9908-23ac85c7a753.png)

Performance Metric on Validation set: F1 score  
The Naïve Bayes model produced the highest F1 score due to the Logistic Regression and SVM models producing 0 or 1 occurrences of predicted positive values.  
| Model                                       | F1 Score | 
| --------------------------------------------| -------- |
| Naive Bayes                                 | 0.25     | 
| Logistic Regression                         | 0.04     | 
| SVM - Linear Kernel default                 | 0.0      | 
| SVM - Linear Kernel (C: 6.488, Gamma 0.027) | 0.0      |

Suggested Future Work  
Due to the imbalanced sample with only 3% of the sample experiencing bankruptcy, future modeling work might include resampling strategies to mitigate the data imbalance.  



References  
Deron Liang, Chia-Chi Lu, Chih-Fong Tsai, Guan-An Shi. “Financial Ratios and Corporate Governance Indicators in Bankruptcy Prediction: A Comprehensive Study.” *European Journal of Operational Research* 252 (2016) 561-572


