---
title: "Optimizing VEGFR2 Inhibitor Discovery: <em>Leveraging Molecular Descriptors for Streamlined Drug Development</em>"
excerpt: The research focuses on Vascular Endothelial Growth Factor Receptor 2 (VEGFR2), a pivotal receptor in angiogenesis, with implications in diseases like cancer and macular degeneration. Current FDA-approved drugs face limitations, prompting the exploration of new VEGFR2 inhibitors using the Quantitative Structure-Activity Relationship (QSAR) method. Building upon these findings, the proposed project aims to identify molecular descriptors that aid in the discovery of novel, effective VEGFR2 inhibitors, ultimately constructing a QSAR model for predicting inhibition activity.
collection: Projects
---

# <span style="color:blue"> Introduction <\span>

Vascular Endothelial Growth Factor Receptor 2 (VEGFR2) emerges as a pivotal receptor in angiogenesis, the process of forming new blood vessels. This mechanism holds significance in various diseases such as cancer, age-related macular degeneration, and inflammatory conditions like rheumatoid arthritis. In the context of cancer and macular degeneration, the overproduction of angiogenic factors, notably VEGF, hyperstimulates VEGFR2, leading to excessive angiogenesis. Addressing this, the use of VEGFR2 inhibitors becomes imperative to curtail nutrient supply to cancerous cells, thereby offering potential therapeutic avenues for these conditions.

Although FDA-approved drugs like sorafenib and pazopanib target VEGFR2, their efficacy is hindered by side effects and resistance issues. To enhance treatment outcomes, the exploration of new drugs becomes paramount. While this quest poses challenges, the Quantitative Structure-Activity Relationship (QSAR) method offers a strategic link between molecular structures and effects, enabling the <em>in silico</em> screening of potential bioactive compounds. This approach minimizes the need for costly experiments in drug discovery.

Previous research successfully identified potential inhibitory molecules through QSAR models. Aligning with these achievements, our proposal takes a step further by concentrating on the identification of key molecular descriptors crucial for recognizing effective VEGFR2 inhibitors. The approach aims to deepen the understanding of specific features essential for inhibitory efficacy.

The primary objective of our project is to construct a QSAR model for predicting the inhibition activity of VEGFR2 inhibitors sourced from the ChEMBL database. The resulting QSAR model will hold the potential to assess the efficacy of novel compounds in VEGFR2 inhibition. This approach may have the potential to streamline the early phases of drug discovery for a spectrum of therapeutic applications, including anti-cancer, retinal diseases, and inflammatory diseases therapies. By saving valuable time and resources, we hope that our research contributes to the advancement of innovative and targeted medical solutions.

# Data

![Figure 1: Data Acquisition and Preprocessing Workflow](/images/dataSection.jpg)

We employed the ChEMBL database for its comprehensive collection of compounds inhibiting VEGFR2. ChEMBL, being a robust bioactivity database, provides valuable information on molecular targets, including VEGFR2, making it a suitable resource for our investigation. Our specific query string was "VEGFR2," and we identified the Target ID as CHEMBL279 [5].

Our primary objective was to focus on compounds with reported IC50 bioactivity values for inhibiting VEGFR2. The IC50 serves as a metric for a compound’s potency, indicating the concentration at which the compound inhibits the angiogenesis process by 50%. A lower IC50 signifies higher potency. Our resulting dataset encompasses 14,087 compounds recognized for their VEGFR2 inhibitory properties, featuring 46 relevant attributes (14,087 rows x 46 columns).

Subsequently, to enhance data quality, we eliminated the rows with missing values. The dataset was then categorized into active, intermediate, and inactive groups based on standard IC50 values.
Recognizing the exponential scale of IC50 values, we converted them to pIC50, representing the nega- tive logarithm of IC50. This transformation facilitates more accessible comparison and interpretation of compound potency, leading to the addition of a pIC50 column in our dataframe. Consequently, our final dataframe after data preprocessing comprises 13,699 compounds x 5 features.

# Methods

![Figure 2: A Detailed Workflow of Exploratory Data Analysis and QSAR Analysis](/images/methodfinal.jpg)

## Exploratory Data Analysis

After completing the data preprocessing phase, we meticulously examined the data to evaluate its quality and determine the distinguishability between the active and inactive classes of inhibitors. Consequently, we excluded rows associated with the intermediate class, focusing solely on the active and inactive classes, resulting in a dataset of 11,592 compounds.

To derive Lipinski descriptors, we developed a custom function and utilized the Python package "rdkit" [6] to generate four descriptors: molecular weight, the number of hydrogen bond acceptors and donors, and the octanol-water partition coefficient (logP). An orally active drug adheres to the Lipinski Rule of 5. As a result, our dataframe comprised 11,592 compounds with nine features, including the four Lipinski criteria.
Subsequently, we conducted the Mann-Whitney U Test based on the different Lipinski criteria to assess the distinguishability between the two bioactivity classes (active and inactive) in our dataset (11,592 compounds × 9 features). The Mann-Whitney U test is a non-parametric statistical test used to determine whether there is a significant difference between two independent groups in terms of their distributions. It produces a p-value, which is then compared to a predetermined significance level α to make decisions about the null hypothesis.

The null hypothesis (H0) posits that there is no difference between the two groups (active and inactive bioactivity classes, in our case). If the p-value is less than or equal to the significance level, typically set at 0.05, the null hypothesis is rejected, suggesting a significant difference between the groups. Conversely, a p-value greater than the significance level leads to the acceptance of the null hypothesis, indicating no significant difference.

## QSAR Analysis

Following the insightful findings from our exploratory data analysis, we started with the Quantitative Structure-Activity Relationship (QSAR) analysis. Armed with a comprehensive dataframe containing 13,699 compounds, complete with essential features like chembl_id and canonical_smiles, the next step was creating PaDeL descriptors from the data.

PaDeL descriptors, serving as numerical representations of chemical compounds, offer a rich tapestry of structural and physicochemical properties vital for rigorous QSAR investigations. These descriptors serve as a fingerprint for each compound, capturing nuanced details that contribute to their biological activity.
The meticulous application of PaDeL descriptors involves the assignment of binary values in a dedicated column. A value of 1 signifies the presence of the specific chemical substructure described by the PaDeL descriptor, while 0 denotes its absence. This meticulous approach allows us to translate complex chemical structures into a format conducive to quantitative analysis, laying the foundation for a comprehensive exploration of structure-activity relationships within our compound dataset.

The PaDeL descriptors, totaling 880 molecular descriptors, were generated using a shell script [7], predominantly representing the chemical substructures of the compounds. Employing the SMILES representation and ChEMBL ID of inhibitory compounds as inputs, we obtained a dataframe comprising 13,699 compounds and 880 molecular descriptors, serving as our independent variables for subsequent machine learning analysis. Additionally, dependent variable comprised the target variable, namely pIC50 values (13,699 compounds x 1 column).

Subsequently, we divided our data into train and test sets through an 80-20 train-test split. Leveraging the XGBoost Regressor, a supervised machine learning algorithm renowned for its high performance in regression tasks, we aimed to predict pIC50 values based on the molecular descriptors generated through PaDeL. XGBRegressor, belonging to the gradient boosting family, employs a decision tree ensemble approach that learns sequentially and adaptively, correcting errors and optimizing a loss function. Its regularization terms control tree complexity, preventing overfitting by using shallow trees, while the "boosting" technique combines predictions of multiple weak learners for a robust model.

Post training our XGBoostRegressor model, we fitted it to the data and predicted the pIC50 values. To assess model performance, we employed the R2 (coefficient of determination), quantifying the proportion of variance in the dependent variable explained by the independent variables.

For a nuanced interpretation, we categorized true and predicted pIC50 values into three bioactivity classes—active, intermediate, and inactive—based on threshold values determined through Mann- Whitney U Test results. Values below 4 were deemed inactive, those above 6 as active, and those between 4 and 6 as intermediate. Subsequently, we evaluated classification accuracy using the accuracy_score function from sklearn and visualized the results through a confusion matrix, providing deeper insights into the classification outcomes.

## Identifying the Top 20 Molecular Descriptors

In our pursuit to pinpoint the crucial molecular descriptors influencing VEGFR2 inhibitors, we leveraged the feature*importances* attribute within the XGBoostRegressor Model. Our focus was on extracting the top 20 molecular descriptors and delineating their significance in predicting pIC50 values. To enhance interpretability, we crafted a visually informative bar plot encapsulating the importance of each feature (Figure A1).

Subsequently, we explored the correlation between the presence ("1") of specific PaDeL descriptors (representing chemical substructures) and inhibitor activity. We exclusively focused on inhibitors categorized as "active" in the bioactivity class, filtering this subset for the presence ("1") of individual PaDeL descriptors. This process was iteratively applied to all PaDeL descriptors under consideration.
To convey our findings effectively, we visualized the correlation outcomes through a comprehensive bar plot, shedding light on the interplay between the presence of each PaDeL descriptor and the bioactivity class "active" (Figure 3). In parallel, we consulted the [8] document to glean deeper insights into the nuanced meanings behind the identified top 20 molecular descriptors.

This holistic methodology not only strengthens the robustness of our analysis but also ensures clarity in presenting the pivotal role played by these molecular descriptors in predicting VEGFR2 inhibitor activity.

# Results

## Exploratory Data Analysis

The Mann-Whitney U Test results reveal significant differences between inhibitors belonging to the two bioactivity classes (active and inactive) (Figure A2). All five cases, including pIC50 values and four Lipinski descriptors (MW value, Log P value, NumDonors value, and NumAcceptors value), rejected the null hypothesis at the 0.05 significance level. Notably, the median values of Lipinski descriptors in our dataset adhere to Lipinski’s rule of five guidelines, reinforcing their compliance. Impressively, over 6000 inhibitors from our dataset are in accordance with Lipinski’s rule of five.

## Model Evaluation

We assessed the performance of our XGBoost Regressor using the coefficient of determination metric and achieved R2 = 0.513. This indicates that our descriptors can account for 51% of the variability in the dependent variable, namely the pIC50 values. To enhance interpretability, we reclassified the pIC50 values into three classes, leading to the computation of a confusion matrix and accuracy metrics. Impressively, we attained a commendable classification accuracy of 78%. Further analysis of the confusion matrix (Figure A3) highlights the model’s efficacy, particularly in accurately predicting pIC50 values for a substantial portion of active inhibitors (86% of true active inhibitors).

## Top Molecular Descriptors

![Chemical Substructures and Inhibitor Activity; Table 1: Chemical substructures with highest correlation to inhibitor activity.](/images/correlPresenceAbs.png)

Figure 3 emphasizes key chemical substructures, identified from the top molecular descriptors by the XGBRegressor, exhibiting a strong correlation with inhibitor activity. The analysis presented in Figure 3 allows us to draw the inference that compounds harboring any of these highlighted substructures hold promise as potential VEGFR2 inhibitors. This insight provides a targeted and strategic approach for further exploration and validation in the pursuit of novel inhibitors with enhanced efficacy in inhibiting VEGFR2 activity.

# Conclusion

In summary, our exploration into the application of XGBRegressor for regression tasks yielded a modest performance, as evidenced by an R2 score of only 0.513. However, upon transforming the data to categorize compounds into three distinct activity classes based on pIC50 value thresholds, the model demonstrated commendable accuracy in classifying compounds as ’active,’ ’intermediate,’ and ’inactive.’ This shift in perspective allowed us to leverage the model effectively for classification tasks, highlighting its adaptability and revealing its strengths in different facets of the analysis.

A noteworthy observation stems from our successful identification of chemical substructures that play a pivotal role in discerning between active and inactive inhibitors of VEGFR2. This insight not only enhances our understanding of the underlying mechanisms but also provides valuable guidance for future research and drug discovery endeavors.

Despite these achievements, certain lacunae include the presence of class imbalance within the dataset and the utilization of default hyperparameters. To address these challenges and further enhance model performance, future work should include implementing oversampling techniques to balance class representation and conducting hyperparameter tuning to optimize the model’s predictive capabilities.
