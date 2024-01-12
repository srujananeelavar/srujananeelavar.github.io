---
title: "A Comparative Analysis of Protein Secondary Structure Prediction Algorithms"
excerpt: The project aims to predict the protein secondary structure using machine learning (HMM & kNN) and deep learning (LSTM) algorithms to forecast the local conformation of amino acids in a protein chain.
collection: Projects
---

<span style="color:gray"> _Computational Methods for Biological Modeling and Simulation (02712) Course Project_
<span style="color:gray"> _(Nov 2023)_

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
</p>

# <span style="color:#007ea7"> Methods

## <span style="color:#007ea7"> Data

<p style='text-align: justify;'> 
The dataset was obtained from RCSB PDB, which comprises 477,000 samples, each featuring an amino acid sequence alongside its corresponding secondary structure sequence. To streamline the analysis, we transformed the original eight-category secondary structure labels (’sst’) into a simplified three-category system. This transformation involved grouping ’B’ into ’E’ and combining ’G’ and ’I’ into ’H,’ yielding three distinct categories: ’C’ (Coil), ’H’ (Helix), and ’E’ (Beta).
</p>

<p style='text-align: justify;'> 
Subsequently, to ensure data integrity, we systematically removed samples with duplicate amino acid sequences, followed by the creation of distinct train and test datasets. The focus then shifted to employing three distinct methods for secondary structure prediction: K-Nearest Neighbor (KNN), Deep Learning with Long Short-Term Memory (LSTM) model, and Hidden Markov Model (HMM). Each method brings its unique set of characteristics and assumptions to the predictive task, providing a diverse exploration of the dataset’s underlying patterns.
</p>

## <span style="color:#007ea7"> k-Nearest Neighbors (kNN) Classification

### Data Pre-processing

<p style='text-align: justify;'> 
Amino acid sequences were transformed into numerical representations using a one-hot encoding scheme. Each character in the sequences was mapped to a numerical value, calculated as the alphabetical position of the character (e.g., A=1, B=2, ..., Z=25). The transformed structural labels were encoded numerically to facilitate compatibility with machine learning models. The encoding scheme assigned ’C’ to 3, ’H’ to 2, and ’E’ to 1, converting the structural information into a format suitable for classification algorithms.
</p>

<p style='text-align: justify;'> 
Subsequently, all sequences were padded to match the average length using the ’pad_sequences’ function from the TensorFlow Keras library. The sequences were then flattened and reshaped into a two-dimensional matrix to serve as input for machine learning models. This preprocessing step ensured that the data had a consistent format suitable for training and evaluation.
</p>

<p style='text-align: justify;'> 
The dataset was divided into training and testing sets using an 80:20 ratio. This division allowed for model training on a substantial portion of the data while retaining a separate set for unbiased model evaluation.
</p>

### Model Training and Fitting

![Figure 1: kNN Classification](/images/kNNImage.png)

_**Figure 1**: kNN Classification_

<p style='text-align: justify;'> 
kNN, or k-Nearest Neighbors, stands as a noteworthy supervised learning algorithm falling within the realm of instance-based, lazy learning methods. As a non-parametric approach, it refrains from making explicit assumptions about the functional form characterizing the underlying data distribution. The central concept driving kNN involves classifying a data point by leveraging the majority class within its k nearest neighbors within the feature space. These "nearest neighbors" encompass data points sharing similar features or attributes.
</p>

<p style='text-align: justify;'> 
To apply kNN in the context of protein secondary structure prediction, we instantiate the KNeigh- borsClassifier from scikit-learn, specifying the number of neighbors (n_neighbors). This parameter, ’k’, crucially influences the model’s behavior, with a smaller ’k’ enhancing sensitivity to local varia- tions potentially leading to overfitting, while a larger ’k’ may yield a smoother decision boundary but risks oversimplification. kNN models were employed for structural classification with varying values of k (3, 4, 5, 6, 7, 8, 9, 10). The choice of k, the number of nearest neighbors considered during classification, was critical in determining model performance. We used an elbow plot to decide the best ’k’ for this classification task.
</p>

<p style='text-align: justify;'> 
In the training phase of kNN, a departure from conventional machine learning methods occurs. Instead of explicitly training a model, kNN memorizes the entire training dataset (X_train, y_train). The model effectively retains feature vectors and their corresponding labels (secondary structures) for each training instance. When predicting the label for a new, unseen protein sequence (e.g., X_test), the model utilizes the stored training data. For the new instance, the algorithm computes distances, often using metrics like Euclidean distance, between its feature vector and those of all training set instances.
</p>

<p style='text-align: justify;'> 
Identifying the k nearest neighbors involves selecting the training instances with the smallest distances to the new instance. In classification tasks like predicting protein secondary structure, the algorithm conducts a majority vote among the labels of the k nearest neighbors. The label most frequently occurring among these neighbors is then assigned as the predicted label for the new instance. This distinctive approach characterizes kNN’s ability to make predictions based on the immediate context of a data point within the feature space.
</p>

## <span style="color:#007ea7"> Hidden Markov Model (HMM)

### General Model Description

<p style='text-align: justify;'> 
Hidden Markov Models (HMMs) constitute a versatile and influential class of probabilistic models utilized across various disciplines for modeling sequential and temporal data. Originating in the work of L. E. Baum and his colleagues during the 1960s, HMMs have found applications in diverse fields such as speech and handwriting recognition, bioinformatics, finance, and natural language processing.
</p>

<p style='text-align: justify;'> 
The fundamental concept driving HMMs is their ability to model systems where the internal dynamics are not directly observable, making them particularly well-suited for scenarios characterized by underlying hidden processes. In the context of HMMs, a system is conceptualized as a Markov process, implying that the future state of the system is solely dependent on its current state and not influenced by past states—a property known as the Markov property.
</p>

<p style='text-align: justify;'> 
The topology of an HMM involves two crucial components: observable states and hidden states. Observable states are associated with the visible outcomes or emissions at each time step, while hidden states govern the transitions between these observable states. This hierarchical structure allows HMMs to capture the temporal dependencies and intricate patterns inherent in sequential data.
</p>

<p style='text-align: justify;'> 
The generality of the HMM framework lies in its flexibility to model diverse systems and its capability to learn from data to reveal the hidden structure governing the observed outcomes. The inherent adaptability of HMMs, coupled with their ability to encapsulate complex dynamics, renders them invaluable in addressing a myriad of real-world problems that involve sequential data analysis and pattern recognition.
</p>

<p style='text-align: justify;'> 
In the figure below, the aij probabilities are the transition probabilities. The bij probabilities are the emission probabilities.
</p>

![Figure 2: General Topology of HMM](/images/hmm1.png)

_**Figure 2**: General Topology of HMM_

### Protein Secondary Structure Prediction HMM Model

<p style='text-align: justify;'> 
The initial dataset was preprocessed by removing duplicate sequences to ensure data integrity. The first step involved calculating the initial state probabilities and transition probabilities. The initial state probabilities were derived by analyzing the frequency of each hidden state at the beginning of the sequences. Transition probabilities were determined by counting the occurrences of transitions between consecutive hidden states. Emission probabilities were calculated by analyzing the observed sequences in relation to the hidden states. The likelihood of observing a particular amino acid given a hidden state was computed, forming the emission probabilities.
</p>

<p style='text-align: justify;'> 
The dataset was split into training and testing sets using the train_test_split function from the sklearn library. The training set comprised 80% of the data, while the testing set comprised the remaining 20%.
A Categorical Hidden Markov Model (HMM) with three hidden states was implemented using the hmmlearn library. The model parameters, including initial state probabilities, transition probabilities, and emission probabilities, were set based on the calculated probabilities from the training data by maximum likelihood estimation (MLE). Each hidden state had the capacity to emit 25 symbols corresponding to the possible standard and non-standard amino acid characters.
</p>

<p style='text-align: justify;'> 
The trained HMM model was used to predict hidden states for sequences in the testing set using the Viterbi algorithm. The predictions were then compared with the actual hidden states, and the Levenshtein distance was computed to quantify the dissimilarity between predicted and actual sequences.
</p>

<p style='text-align: justify;'> 
A customized function was developed to visualize the predicted and actual sequences, providing insights into the similarity between the two sequences. The accuracy of the model was assessed by calculating the similarity between predicted and actual sequences using the Levenshtein distance. The average similarity across all sequences in the testing set was computed to gauge the overall performance of the HMM.
In the figure below, we observe an amino acid sequence where each observed state corresponds to an amino acid emitted by one of three hidden states. These hidden states represent different structural components of the amino acid sequence.
</p>

<p style='text-align: justify;'> 
In essence, the hidden states reveal the underlying structure and dynamics, illustrating the core principle of Hidden Markov Models in capturing intricate patterns within sequential data.
</p>

![Figure 3: Hidden Markov Models Unveil Amino Acid Structures](/images/hmm2.png)

_**Figure 3**: Hidden Markov Models Unveil Amino Acid Structures_

## <span style="color:#007ea7"> Deep Learning Model

<p style='text-align: justify;'> 
The main motivation of using deep learning in this problem was to treat the task of secondary structure sequence prediction from amino acid sequences as a sequence-to-sequence translation task.
</p>

### Data Pre-processing

<p style='text-align: justify;'> 
The initial phase involved the conversion of amino acid labels and secondary structure labels into integer representations. This crucial preprocessing step transformed the input amino acid sequences and the corresponding expected output of secondary structure sequences into sequences of integers, facilitating their compatibility with the subsequent stages of the model.
</p>

<p style='text-align: justify;'> 
Following the conversion to integer representations, the dataset underwent further organization by being partitioned into batches, each containing 64 instances. To ensure uniformity and streamline the training process, sequences within each batch were padded with zeros, harmonizing their lengths. This step of zero-padding aimed to create homogeneous sequences within a batch, enabling efficient parallelization and computation during the training phase.
</p>

### Model Architecture

![Figure 4: Model Architecture](/images/lstmmodelarchitecture.png)

_**Figure 4**: Model Architecture_

<p style='text-align: justify;'> 
The first layer in our model was an embedding layer to represent amino acids as continuous vectors. This embedding process transforms the categorical nature of amino acids into continuous represen- tations, allowing the subsequent layers of the neural network to operate on a more meaningful and continuous input space.
</p>

![Figure 5: LSTM Architecture](/images/LSTM.png)

_**Figure 5**: LSTM Architecture_

<p style='text-align: justify;'> 
The output of the embedding layer was passed into an bi-directional LSTM layer. LSTM, which stands for Long Short-Term Memory, is a type of recurrent neural network (RNN) architecture. The LSTM architecture consists of: Memory Cell, Forget Gate, Inpute Gate and Output Gate. By incorporating these components, LSTMs are capable of learning and remembering patterns in sequences over extended periods. The gating mechanisms enable them to selectively update and utilize information, making them well-suited for tasks where understanding and retaining long-term dependencies are crucial.
</p>

<p style='text-align: justify;'> 
A key element of our model architecture involved the incorporation of a bidirectional LSTM layer. A bi-directional LSTM consists of two LSTMs: one for the forward direction, and one for the backward direction. The choice of a bidirectional LSTM is particularly significant as it enables the model to capture sequential dependencies in both forward and backward directions. This bidirectional nature enhances the model’s ability to comprehend the contextual information surrounding each amino acid in the sequence, thereby improving the overall representation learning.
</p>

<p style='text-align: justify;'> 
For the specific task of classifying secondary structures, a multi-class classification framework was adopted. This implies that the model aimed to categorize each amino acid into one of the predefined secondary structure classes, such as helix, strand, or coil. To train the model, we employed the categorical cross-entropy loss function. Categorical cross-entropy is well-suited for multi-class classification problems, as it measures the dissimilarity between the predicted probability distribution and the true distribution of class labels.
</p>

<p style='text-align: justify;'> 
The optimization of model parameters during training was facilitated by the Adam optimizer. Adam (short for Adaptive Moment Estimation) is an optimization algorithm that combines the advantages of both momentum-based optimization and root mean square propagation. It adapts the learning rates for each parameter individually, offering a dynamic and efficient approach to gradient descent. This adaptability helps the model converge faster and more reliably, especially in scenarios involving high-dimensional and intricate data such as amino acid sequences.
</p>

## <span style="color:#007ea7"> Evaluation

<p style='text-align: justify;'> 
In order to compare and evaluate the performance of the three approaches on any given amino acid sequence, we computed the similarity score as follows:
</p>

<p style='text-align: justify;'> 
Similarity Score = (1 − Levenshtein Distance/Sequence Length) ∗ 100 
</p>

<p style='text-align: justify;'> 
Here, Levenshtein distance, also known as edit distance, is a measure of the similarity between two strings. It quantifies the minimum number of single-character edits (insertions, deletions, or substitutions) required to transform one string into another.
</p>

<p style='text-align: justify;'> 
Furthermore, he accuracy of each method was computed by taking the average over the similarity scores over all the sequences in the test set.
</p>

# <span style="color:#007ea7"> Results

<p style='text-align: justify;'> 
Upon closely observing the model performance on a few select strings (Figure 6), it is clear that all three models have a decent performance in predicting alpha helices and random coils, compared to the prediction of beta sheets. The observed inefficiency in predicting beta structures is likely influenced by the scarcity of training instances representing beta structures and the inherent challenge of capturing long-range dependencies crucial for accurate predictions.
</p>

![Figure 6: Similarity Scores of Selected Strings](/images/simscore.jpg)

_**Figure 6**: Similarity Scores of Selected Strings_

<p style='text-align: justify;'> 
From the accuracy plot (Figure 7), the kNN method exhibited surprisingly high performance, demon- strating notable effectiveness in discerning the protein’s secondary structure based on its neighbors in the dataset. However, the simplicity of the LSTM model stood out as it claimed the top position, showcasing the best performance among the three methods. The LSTM’s capacity to capture sequen- tial dependencies in the amino acid sequences proved highly effective in the context of secondary structure prediction.
</p>

![Figure 7: Model-wise Accuracy (%)](/images/accuracy.jpg)

_**Figure 7**: Model-wise Accuracy (%)_

<p style='text-align: justify;'> 
On the contrary, the Hidden Markov Model (HMM) exhibited relatively low performance. HMM, relying on a probabilistic framework, faced challenges in capturing intricate patterns within the protein sequences, resulting in less accurate predictions compared to the other methods.
</p>

<p style='text-align: justify;'> 
While Hidden Markov Models (HMMs) offer interpretability and proficiency in modeling local dependencies, they face limitations in addressing long-range interactions crucial for accurate structure prediction. On the other hand, the k-Nearest Neighbors (kNN) method, with its simplicity and intuitive approach to local sequence similarity, falls short due to sensitivity to noise and an inadequate capacity to model intricate long-range dependencies inherent in protein folding.
</p>

<p style='text-align: justify;'> 
In summary, the LSTM’s advanced features in handling these critical aspects make it the preferred choice for protein secondary structure prediction when compared to HMMs and kNN.
</p>
