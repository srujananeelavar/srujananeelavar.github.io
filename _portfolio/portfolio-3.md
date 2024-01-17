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
The harmonic mean of precision and recall, offering a balanced measure of model performance (F1-score = 2 * Precision * Recall / (Precision + Recall))
</p>

# <span style="color:#007ea7"> Results

We applied our models to both the imbalanced dataset and the oversampled dataset to compare their performance. This allows us to understand the significance and limitations of oversampling.

## k-Nearest Neighbor

### Before Oversampling

<p style='text-align: justify;'> 
The accuracy of k-Nearest Neighbor before applying SMOTE on our training dataset was 80.0%. Table 1 (see below) presents the classification report. Accuracy scores fluctuate with increasing k values (Table 2).
</p>

![Figure 2: Confusion matrix for k-NN without SMOTE](/images/kNNwithoutSMOTE.png)

_**Figure 1**: Confusion matrix for k-NN without SMOTE_

![Table 2: Table of Different k Values with Corresponding Accuracy Score](/images/kNNAcc.png)

_**Table 2**: Table of Different k Values with Corresponding Accuracy Score_

### After Oversampling

<p style='text-align: justify;'> 
Post-SMOTE, our k-Nearest Neighbor model's accuracy on the training dataset decreased to 61.8%. This drop is attributed to overfitting before SMOTE, which favored the majority class. The accuracy scores drop gradually with increasing k values (Table 4).
</p>

![Figure 3: Confusion matrix for k-NN with SMOTE](/images/kNNSmote.png)

_**Figure 3**: Confusion matrix for k-NN with SMOTE_

![Table 4: Table of Different k Values with Corresponding Accuracy Score (SMOTE)](/images/kNNAccSmote.png)

_**Table 4**: Table of Different k Values with Corresponding Accuracy Score (SMOTE)_

## Multiclass Logistic Regression

### Before Oversampling

<p style='text-align: justify;'> 
The accuracy of k-Nearest Neighbor before applying SMOTE on our training dataset was 80.0%. Table 1 (see below) presents the classification report. Accuracy scores fluctuate with increasing k values (Table 2).
</p>

![Figure 4: Confusion matrix for MLR without SMOTE](/images/kNNwithoutSMOTE.png)

_**Figure 4**: Confusion matrix for k-NN without SMOTE_

![Figure 5: Table of Different k Values with Corresponding Accuracy Score](/images/kNNAcc.png)

_**Figure 5**: Table of Different k Values with Corresponding Accuracy Score_

### After Oversampling

<p style='text-align: justify;'> 
Post-SMOTE, our k-Nearest Neighbor model's accuracy on the training dataset decreased to 61.8%. This drop is attributed to overfitting before SMOTE, which favored the majority class. The accuracy scores drop gradually with increasing k values (Table 4).
</p>

![Figure 6: Confusion matrix for MLR with SMOTE](/images/kNNSmote.png)

_**Figure 6**: Confusion matrix for k-NN with SMOTE_

![Figure 7: Table of Different k Values with Corresponding Accuracy Score (SMOTE)](/images/kNNAccSmote.png)

_**Figure 7**: Table of Different k Values with Corresponding Accuracy Score (SMOTE)_

## Random Forest

### Before Oversampling

<p style='text-align: justify;'> 
The accuracy of k-Nearest Neighbor before applying SMOTE on our training dataset was 80.0%. Table 1 (see below) presents the classification report. Accuracy scores fluctuate with increasing k values (Table 2).
</p>

![Figure 2: Confusion matrix for k-NN without SMOTE](/images/kNNwithoutSMOTE.png)

_**Figure 1**: Confusion matrix for k-NN without SMOTE_

![Table 2: Table of Different k Values with Corresponding Accuracy Score](/images/kNNAcc.png)

_**Table 2**: Table of Different k Values with Corresponding Accuracy Score_

### After Oversampling

<p style='text-align: justify;'> 
Post-SMOTE, our k-Nearest Neighbor model's accuracy on the training dataset decreased to 61.8%. This drop is attributed to overfitting before SMOTE, which favored the majority class. The accuracy scores drop gradually with increasing k values (Table 4).
</p>

![Figure 3: Confusion matrix for k-NN with SMOTE](/images/kNNSmote.png)

_**Figure 3**: Confusion matrix for k-NN with SMOTE_

![Table 4: Table of Different k Values with Corresponding Accuracy Score (SMOTE)](/images/kNNAccSmote.png)

_**Table 4**: Table of Different k Values with Corresponding Accuracy Score (SMOTE)_

# <span style="color:#007ea7"> Conclusions

<p style='text-align: justify;'> 
In conclusion, all our models are performing well, however they are struggling to identify certain classes, which is however seen to improve after performing oversampling using SMOTE. There is an increase in True Positives and False Positives for all models despite a decrease in True Negatives and False Negatives. 
</p>

<p style='text-align: justify;'> 
From our results, we can conclude that Random Forest is able to distinguish between classes better than Multi class Logistic Regression and kNN models. This might be due to the fact that the test data set lack certain classes, i.e., the minority classes do not appear in the test set. 
</p>

<p style='text-align: justify;'> 
A point to note is that we did not perform oversampling on our test data set and hence it explains the reason why the minority classes are being poorly recognized in our confusion matrices. Further, the survey data may lack distinctive features for certain classes, i.e, there can be overlapping classes. Additional features for the minority class that is more distinctive may help in clearly distinguishing between the minority classes.
</p>

<p style='text-align: justify;'> 
In the future, we could accommodate the usage of better computational resources so that we can run our models on our entire data set. This will help us understand if the limitation of our work is due to the lower number of samples. Further, we would like to tune the k-neighbors value for SMOTE in order to analyze how changing this hyper parameter changes the accuracy of our models. It would also be very beneficial to classify the different types of Diabetes by acquiring relevant data sets.
</p>
