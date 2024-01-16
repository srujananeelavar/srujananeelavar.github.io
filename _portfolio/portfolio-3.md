---
title: "Prediction of Diabetes based on Multiple Diagnostic and Lifestyle Features"
excerpt: The study aims to leverage machine learning models, including Random Forest, Multiclass Logistic Regression, and k-Nearest Neighbors, to predict diabetes from a dataset, emphasizing the importance of early detection to reduce complications and enhance the quality of life.
collection: Projects
---

<span style="color:gray"> Machine Learning for Scientists (02620) Course Project (Apr 2023)\_

_This [Google Colab notebook](https://colab.research.google.com/drive/1TEjunvA97hkH1N5KvBcnazY9O0U2EzNR) contains the code pertaining to this project._

# <span style="color:#007ea7"> Introduction

<p style='text-align: justify;'> 
Diabetes, a chronic disorder marked by high blood sugar, impairs the regulation of glucose levels, impacting quality of life. The three types are Type I (autoimmune reaction hindering insulin production), Type II (imbalance causing insulin resistance), and Gestational (during pregnancy, affecting both mother and baby). Complications include blood vessel damage, nerve damage, and more. In 2019, diabetes caused 1.5 million deaths, and the projected cases by 2040 are 650 million. Early detection is crucial. 
</p>

<p style='text-align: justify;'> 
This study employs machine learning models (Random Forest, Multiclass Logistic Regression, k-Nearest Neighbors) on CDC’s BRFSS2015 dataset to predict diabetes, aiming to reduce complications and enhance quality of life.
</p>

# <span style="color:#007ea7"> Data

![Figure 1: Attaining Class Balance using SMOTE](/images/Figure1MLProject.png)

_**Figure 1**: Attaining Class Balance using SMOTE_

<p style='text-align: justify;'> 
The dataset, sourced from [Kaggle](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset?resource=download), comprises 253,680 survey responses to CDC’s BRFSS2015, featuring 21 variables like high blood pressure, BMI, etc. The target variable, Diabetes012, has three classes (0 for no diabetes, 1 for prediabetes, and 2 for diabetes) with class imbalance. Due to computational constraints, 14,000 samples were used, split 75:25 for training and testing. 
</p>

<p style='text-align: justify;'> 
Class imbalances were addressed using Synthetic Minority Oversampling Technique (SMOTE) on the training set, creating a balanced dataset. SMOTE aids in preventing overfitting and improving generalization. Post-SMOTE, the dataset is balanced (8610 samples per class). The project aims to compare model performance on original and oversampled data. Class imbalance is evident before SMOTE (left plot), while balance is achieved after SMOTE (right plot). [Figure 1a & 1b]
</p>

# <span style="color:#007ea7"> Methods

## k-Nearest Neighbor

<p style='text-align: justify;'> 
Implemented from scratch, k-Nearest Neighbor is a non-parametric classification method that assigns labels based on majority votes. The algorithm calculates distances between the query and training set samples, assigns labels based on the k nearest distances, and determines the class by majority voting. This intuitive algorithm works well on both small and large datasets without requiring training.
</p>

## Multiclass Logistic Regression

<p style='text-align: justify;'> 
Implemented independently, Multiclass Logistic Regression predicts probabilities of a categorical variable with more than two categories. With 14,000 observations and 21 features across 3 classes, the algorithm finds the weight matrix W to predict class membership. The workflow involves calculating Z by multiplying X and W, applying the softmax function for probability distribution, and determining the class with the highest probability using argmax. The negative log-likelihood function is used for the loss function, updated iteratively until convergence.
</p>

## Random Forest

<p style='text-align: justify;'> 
Using scikit-learn, Random Forest is implemented with 300 decision trees. The algorithm builds an ensemble of decision trees on bootstrap samples and random feature subsets. To make predictions, it takes a majority vote of the decision trees. The criterion for measuring split quality is entropy.
</p>

## Evaluation Metrics

<p style='text-align: justify;'> 
After fitting and predicting on our dataset, we assessed model performance using scikit-learn's accuracy score and classification report. The accuracy score calculates the proportion of correct predictions, while the classification report provides various metrics for model evaluation. Confusion matrices were constructed before and after SMOTE for a detailed analysis of oversampling impact on accuracy. Key metrics studied include:
</p>

### Precision

<p style='text-align: justify;'> 
The ratio of true positive predictions to all positive predictions (Precision = True Positive / True Positive + False Positive).
</p>

### Recall

<p style='text-align: justify;'> 
The ratio of true positive predictions to all actual positive instances (Recall = True Positive / True Positive + False Negative).
</p>

### F1-Score

<p style='text-align: justify;'>
The harmonic mean of precision and recall, offering a balanced measure of model performance (F1-score = 2 _ Precision _ Recall / (Precision + Recall))
</p>

# Results

![Figure 3: op 20 Important Features from XGBRegressor](/images/top20.png)

_**Figure 3**: op 20 Important Features from XGBRegressor_

<p style='text-align: justify;'> 
In our pursuit to pinpoint the crucial molecular descriptors influencing VEGFR2 inhibitors, we leveraged the feature*importances* attribute within the XGBoostRegressor Model. Our focus was on extracting the top 20 molecular descriptors and delineating their significance in predicting pIC50 values. To enhance interpretability, we crafted a visually informative bar plot encapsulating the importance of each feature (Figure 3).
</p>

<p style='text-align: justify;'> 
Subsequently, we explored the correlation between the presence ("1") of specific PaDeL descriptors (representing chemical substructures) and inhibitor activity. We exclusively focused on inhibitors categorized as "active" in the bioactivity class, filtering this subset for the presence ("1") of individual PaDeL descriptors. This process was iteratively applied to all PaDeL descriptors under consideration.
To convey our findings effectively, we visualized the correlation outcomes through a comprehensive bar plot, shedding light on the interplay between the presence of each PaDeL descriptor and the bioactivity class "active". In parallel, we consulted the document to glean deeper insights into the nuanced meanings behind the identified top 20 molecular descriptors.
</p>

<p style='text-align: justify;'> 
This holistic methodology not only strengthens the robustness of our analysis but also ensures clarity in presenting the pivotal role played by these molecular descriptors in predicting VEGFR2 inhibitor activity.
</p>

# <span style="color:#007ea7"> Results

## Exploratory Data Analysis

![Figure 4: Mann-Whitney U Test Results](/images/exploratorymannu.jpg)

_**Figure 4**: Mann-Whitney U Test Results_

<p style='text-align: justify;'> 
The Mann-Whitney U Test results reveal significant differences between inhibitors belonging to the two bioactivity classes (active and inactive) (Figure 4). All five cases, including pIC50 values and four Lipinski descriptors (MW value, Log P value, NumDonors value, and NumAcceptors value), rejected the null hypothesis at the 0.05 significance level. Notably, the median values of Lipinski descriptors in our dataset adhere to Lipinski’s rule of five guidelines, reinforcing their compliance. Impressively, over 6000 inhibitors from our dataset are in accordance with Lipinski’s rule of five.
</p>

## Model Evaluation

![Figure 5: Confusion Matrix](/images/confusionmatrix.png)

_**Figure 5**: Confusion Matrix_

<p style='text-align: justify;'> 
We assessed the performance of our XGBoost Regressor using the coefficient of determination metric and achieved R2 = 0.513. This indicates that our descriptors can account for 51% of the variability in the dependent variable, namely the pIC50 values. To enhance interpretability, we reclassified the pIC50 values into three classes, leading to the computation of a confusion matrix and accuracy metrics. Impressively, we attained a commendable classification accuracy of 78%. Further analysis of the confusion matrix (Figure 5) highlights the model’s efficacy, particularly in accurately predicting pIC50 values for a substantial portion of active inhibitors (86% of true active inhibitors).
</p>

## Top Molecular Descriptors

![Figure 6: Chemical Substructures and Inhibitor Activity; Table 1: Chemical substructures with highest correlation to inhibitor activity.](/images/figure5.png)

_**Figure 6**: Chemical Substructures and Inhibitor Activity; **Table 1**: Chemical substructures with highest correlation to inhibitor activity._

<p style='text-align: justify;'> 
Figure 6 emphasizes key chemical substructures, identified from the top molecular descriptors by the XGBRegressor, exhibiting a strong correlation with inhibitor activity. The analysis presented in Figure 6 allows us to draw the inference that compounds harboring any of these highlighted substructures hold promise as potential VEGFR2 inhibitors. This insight provides a targeted and strategic approach for further exploration and validation in the pursuit of novel inhibitors with enhanced efficacy in inhibiting VEGFR2 activity.
</p>

# <span style="color:#007ea7"> Conclusion

<p style='text-align: justify;'> 
In summary, our exploration into the application of XGBRegressor for regression tasks yielded a modest performance, as evidenced by an R2 score of only 0.513. However, upon transforming the data to categorize compounds into three distinct activity classes based on pIC50 value thresholds, the model demonstrated commendable accuracy in classifying compounds as ’active,’ ’intermediate,’ and ’inactive.’ This shift in perspective allowed us to leverage the model effectively for classification tasks, highlighting its adaptability and revealing its strengths in different facets of the analysis.
</p>

<p style='text-align: justify;'> 
A noteworthy observation stems from our successful identification of chemical substructures that play a pivotal role in discerning between active and inactive inhibitors of VEGFR2. This insight not only enhances our understanding of the underlying mechanisms but also provides valuable guidance for future research and drug discovery endeavors.
</p>

<p style='text-align: justify;'> 
Despite these achievements, certain lacunae include the presence of class imbalance within the dataset and the utilization of default hyperparameters. To address these challenges and further enhance model performance, future work should include implementing oversampling techniques to balance class representation and conducting hyperparameter tuning to optimize the model’s predictive capabilities.
</p>
