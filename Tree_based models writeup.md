#  Classification on Taiwan Economic Journal Bankruptcy Dataset with Tree-based Models   

The classification modeling exercise will serve to predict whether the company is likely to go bankrupt, with the response variable as “Bankrupt?” using the following tree-based models: Random Forest, Gradient Boosted Trees, and Extra Trees.  


### EDA and Preprocessing
In previous EDA, there were no missing values in the data, however the dataset did contain imbalanced classes of bankrupt vs. non-bankrupt occurrences. There is a high occurrence of non-bankruptcy, with only 3% of the data occurring as bankrupt. Therefore, the rebalancing SMOTE technique was applied to the train dataset oversample the minority class of bankrupt occurrences. Oversampling will help the models improve on correctly classifying and predicting for bankrupt occurrences.  

Train/test split  
The models were validated on 20% of the sample, with 805 of the sample used to train the models.  



### Model Evaluation
Models were created using default settings for baseline values and later created with hyperparameter tuning. The list below provides the optimized models created with hyperparameter tuning:  
Random Forest:
* n_estimators: 400  
* max_features: square root  
* max_depth: None  

Gradient Boosted Trees  
* learning rate: 0.2
* max_depth: 6  
* max_features: square root
* n_estimators: 200  

Extra Trees
* n_estimators: 150  
* criterion: entropy  
* max_depth: None  
* min_samples_split: 2  



### Comparing Performance Across Models on Validation Set  
The Random Forest model with default settings achieved the highest F1 score of all baseline models.  

Gradient Boosting is known to perform better than other algorithms for imbalanced classes and yielded the best performance out of all models. The model achieved the highest F1 score at 0.66 as well, balancing Precision and Recall with the highest Precision at 67%. The models achieved the same Recall at 65%. Precision saw the highest improvement over the Random Forest baseline, indicating that tuning hyperparameters improved on the models’ ability to predict the likelihood a company would be bankrupt.  


| Model                  | F1 Score | Precision | Recall | Accuracy |
| ---------------------- | -------- | --------- | ------ |--------- |
| **Baseline Model**     |          |           |        |          |
| Random Forest          | 0.55     | 47%       | 65%    | 96%      |
| **Optimized Models**   |          |           |        |          |
| Random Forest          | 0.56     | 49%       | 65%    | 96%      |
| Extra Trees            | 0.58     | 53%       | 65%    | 97%      |       
| Gradient Boosted Trees | 0.66     | 67%       | 65%    | 98%      |



### Feature Importance Comparison  
Feature importance was reviewed from the optimized Random Forest, Gradient Boosted Trees, and Extra Trees models. The Random Forest and Extra Trees models were similar in applying heaviest importance to several features when generating the model. The Gradient Boosted Trees model behaved differently by applying much larger importance on 1 feature (Total debt/total net worth) with minimal importance on all other features.  
The following features were included within the top 10 listed for feature importance:  
* All of the models used 2 out of the 3 ROA features and ROA (C) before interest and depreciation was common to all 3 models
* Net income to total assets
* Net provide before Tax/Paid in Capital

Optimal Random Forest Feature Importance  
![Ranfor_Tun 6 25 features](https://user-images.githubusercontent.com/49419673/152699603-594ee222-8c10-4095-a830-65ae4e148522.png)  

Optimal Extra Trees Feature Importance  
![Extra Trees 25 features](https://user-images.githubusercontent.com/49419673/152699638-121cf119-4a56-412a-a19e-71c4b96aa039.png)  

Optimal Gradient Boosted Trees Feature Importance  
![GB features](https://user-images.githubusercontent.com/49419673/152699682-58f42f91-10d2-4013-905a-ced79a9c182f.png)  



