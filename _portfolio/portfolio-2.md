---
title: "A Comparative Analysis of Protein Secondary Structure Prediction Algorithms"
excerpt: The project aims to predict the protein secondary structure using machine learning (HMM & kNN) and deep learning (LSTM) algorithms to forecast the local conformation of amino acids in a protein chain.
collection: Projects
---

<span style="color:gray"> _Computational Methods for Biological Modeling and Simulation (02712) Course Project (Nov 2023)_

_This [Google Drive Link](https://drive.google.com/drive/folders/1UfIrLgRXrXgJZXj9r6R8haiCdiqjVHMv?usp=share_link) contains the data and code pertaining to this project._

# <span style="color:#007ea7"> Introduction

<p style='text-align: justify;'> 
Predicting the secondary structure of proteins is a fundamental endeavor in the field of bioinformatics and structural biology. Understanding the arrangement of amino acids into alpha helices, beta sheets, or random coils within a protein is crucial for elucidating its function and interactions. Secondary structure prediction methods employ computational algorithms and various data sources, including amino acid sequences and experimental data, to infer these structural features.
</p>

<p style='text-align: justify;'> 
Before diving into different secondary structure prediction approaches, it is important to understand the biological significance and details about secondary structures of proteins. We examine only three main secondary structure classes here, namely the alpha helix (H), beta sheet (E) and random coil (C).
</p>

<p style='text-align: justify;'> 
1. Alpha helices are essential for maintaining the structural integrity and stability of a protein. They also play a crucial role in protein-protein interactions and are involved in DNA binding and recognition. An alpha helix is a right-handed coil in which the backbone of the protein forms a spiral, with hydrogen bonds stabilizing the structure. Each turn of the helix typically consists of 3.6 amino acid residues.
</p>

<p style='text-align: justify;'> 
2. Beta sheets are crucial for protein-protein interactions, as well as for providing structural strength to proteins. They are involved in forming the beta-barrel structure in membrane proteins. Beta sheets are formed when neighboring segments of the protein’s polypeptide chain run alongside each other and are held together by hydrogen bonds.
</p>

<p style='text-align: justify;'> 
3. Random coils, or unstructured regions, represent the flexible and less ordered portions of a protein. They often act as flexible linkers between the other secondary structure elements. Random coils lack a regular repeating structure. They are characterized by a lack of stable hydrogen bonding patterns, which gives them their flexible and disordered nature.
</p>

<p style='text-align: justify;'> 
In this project, we aim to delve into the practical aspects of predicting protein secondary structures, focusing on the significance of alpha helices, beta sheets, and random coils. By exploring predictive methods and their biological implications, the objective is to improve our understanding of protein behavior for practical applications in drug discovery, functional annotation, and related fields.

# <span style="color:#007ea7"> Methods

## Data

<p style='text-align: justify;'> 
The dataset was obtained from RCSB PDB, which comprises 477,000 samples, each featuring an amino acid sequence alongside its corresponding secondary structure sequence. To streamline the analysis, we transformed the original eight-category secondary structure labels (’sst’) into a simplified three-category system. This transformation involved grouping ’B’ into ’E’ and combining ’G’ and ’I’ into ’H,’ yielding three distinct categories: ’C’ (Coil), ’H’ (Helix), and ’E’ (Beta).
</p>

<p style='text-align: justify;'> 
Subsequently, to ensure data integrity, we systematically removed samples with duplicate amino acid sequences, followed by the creation of distinct train and test datasets. The focus then shifted to employing three distinct methods for secondary structure prediction: K-Nearest Neighbor (KNN), Deep Learning with Long Short-Term Memory (LSTM) model, and Hidden Markov Model (HMM). Each method brings its unique set of characteristics and assumptions to the predictive task, providing a diverse exploration of the dataset’s underlying patterns.
</p>

## k-Nearest Neighbors (kNN) Classification

### Data Pre-processing

<p style='text-align: justify;'> 
After completing the data preprocessing phase, we meticulously examined the data to evaluate its quality and determine the distinguishability between the active and inactive classes of inhibitors. Consequently, we excluded rows associated with the intermediate class, focusing solely on the active and inactive classes, resulting in a dataset of 11,592 compounds.
</p>

<p style='text-align: justify;'> 
To derive Lipinski descriptors, we developed a custom function and utilized the Python package "rdkit" to generate four descriptors: molecular weight, the number of hydrogen bond acceptors and donors, and the octanol-water partition coefficient (logP). An orally active drug adheres to the Lipinski Rule of 5. As a result, our dataframe comprised 11,592 compounds with nine features, including the four Lipinski criteria.
Subsequently, we conducted the Mann-Whitney U Test based on the different Lipinski criteria to assess the distinguishability between the two bioactivity classes (active and inactive) in our dataset (11,592 compounds × 9 features). The Mann-Whitney U test is a non-parametric statistical test used to determine whether there is a significant difference between two independent groups in terms of their distributions. It produces a p-value, which is then compared to a predetermined significance level α to make decisions about the null hypothesis.
</p>

<p style='text-align: justify;'> 
The null hypothesis (H0) posits that there is no difference between the two groups (active and inactive bioactivity classes, in our case). If the p-value is less than or equal to the significance level, typically set at 0.05, the null hypothesis is rejected, suggesting a significant difference between the groups. Conversely, a p-value greater than the significance level leads to the acceptance of the null hypothesis, indicating no significant difference.
</p>

## QSAR Analysis

<p style='text-align: justify;'> 
Following the insightful findings from our exploratory data analysis, we started with the Quantitative Structure-Activity Relationship (QSAR) analysis. Armed with a comprehensive dataframe containing 13,699 compounds, complete with essential features like chembl_id and canonical_smiles, the next step was creating PaDeL descriptors from the data.
</p>

<p style='text-align: justify;'> 
PaDeL descriptors, serving as numerical representations of chemical compounds, offer a rich tapestry of structural and physicochemical properties vital for rigorous QSAR investigations. These descriptors serve as a fingerprint for each compound, capturing nuanced details that contribute to their biological activity.
The meticulous application of PaDeL descriptors involves the assignment of binary values in a dedicated column. A value of 1 signifies the presence of the specific chemical substructure described by the PaDeL descriptor, while 0 denotes its absence. This meticulous approach allows us to translate complex chemical structures into a format conducive to quantitative analysis, laying the foundation for a comprehensive exploration of structure-activity relationships within our compound dataset.
</p>

<p style='text-align: justify;'> 
The PaDeL descriptors, totaling 880 molecular descriptors, were generated using a shell script [7], predominantly representing the chemical substructures of the compounds. Employing the SMILES representation and ChEMBL ID of inhibitory compounds as inputs, we obtained a dataframe comprising 13,699 compounds and 880 molecular descriptors, serving as our independent variables for subsequent machine learning analysis. Additionally, dependent variable comprised the target variable, namely pIC50 values (13,699 compounds x 1 column).
</p>

<p style='text-align: justify;'> 
Subsequently, we divided our data into train and test sets through an 80-20 train-test split. Leveraging the XGBoost Regressor, a supervised machine learning algorithm renowned for its high performance in regression tasks, we aimed to predict pIC50 values based on the molecular descriptors generated through PaDeL. XGBRegressor, belonging to the gradient boosting family, employs a decision tree ensemble approach that learns sequentially and adaptively, correcting errors and optimizing a loss function. Its regularization terms control tree complexity, preventing overfitting by using shallow trees, while the "boosting" technique combines predictions of multiple weak learners for a robust model.
</p>

<p style='text-align: justify;'> 
Post training our XGBoostRegressor model, we fitted it to the data and predicted the pIC50 values. To assess model performance, we employed the R2 (coefficient of determination), quantifying the proportion of variance in the dependent variable explained by the independent variables.
</p>

<p style='text-align: justify;'> 
For a nuanced interpretation, we categorized true and predicted pIC50 values into three bioactivity classes—active, intermediate, and inactive—based on threshold values determined through Mann- Whitney U Test results. Values below 4 were deemed inactive, those above 6 as active, and those between 4 and 6 as intermediate. Subsequently, we evaluated classification accuracy using the accuracy_score function from sklearn and visualized the results through a confusion matrix, providing deeper insights into the classification outcomes.
</p>

## Identifying the Top 20 Molecular Descriptors

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
