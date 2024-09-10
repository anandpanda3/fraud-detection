# Anomaly Detection in Credit Card Transactions

## Project Report

**https://sugatagh.github.io/dsml/projects/anomaly-detection-in-credit-card-transactions/**
 
## Kaggle Notebook

**https://www.kaggle.com/code/sugataghosh/anomaly-detection-in-credit-card-transactions**

## Anomaly Detection

In [**statistics**](https://en.wikipedia.org/wiki/Statistics) and [**data analysis**](https://en.wikipedia.org/wiki/Data_analysis), an [**anomaly**](https://en.wikipedia.org/wiki/Anomaly) or [**outlier**](https://en.wikipedia.org/wiki/Outlier) refers to a rare observation which deviates significantly from the majority of the data and does not conform to a well-defined notion of normal behaviour. It is possible that such observations may have been generated by a different mechanism or appear inconsistent with the remainder of the dataset. The process of identifying such observations is generally referred to as anomaly detection. In recent days, [**machine learning**](https://en.wikipedia.org/wiki/Machine_learning) is progressively being employed to automate the process of anomaly detection through [**supervised learning**](https://en.wikipedia.org/wiki/Supervised_learning) (when observations are [**labeled**](https://en.wikipedia.org/wiki/Labeled_data) as *normal* or *anomalous*), [**semi-supervised learning**](https://en.wikipedia.org/wiki/Semi-supervised_learning) (when only a small fraction of observations are labeled) and [**unsupervised learning**](https://en.wikipedia.org/wiki/Unsupervised_learning) (when observations are not labeled). Anomaly detection is particularly suitable in the following setup:

- Anomalies are very rare in the dataset
- The features of anomalous observations differ significantly from those of normal observations
- Anomalies may result for different (potentially new) reasons

Anomaly detection can be very useful in credit card fraud detection. Fraudulent transactions are rare compared to authentic transactions. Also, the methods through which fraudulent transactions occur keep evolving, as the old ways get flagged by existing fraud detection systems. In this notebook, we shall develop a basic anomaly detection system that flags transactions with feature values deviating significantly from those of authentic transactions.

## Data

**Source:** **https://www.kaggle.com/mlg-ulb/creditcardfraud**

The dataset contains information on the transactions made using credit cards by European cardholders, in two particular days of September $2013$. It presents a total of $284807$ transactions, of which $492$ were fraudulent. Clearly, the dataset is highly imbalanced, the positive class (fraudulent transactions) accounting for only $0.173\%$ of all transactions. The columns in the dataset are as follows:

- **Time:** The time (in seconds) elapsed between the transaction and the very first transaction
- **V1 to V28:** Obtained from principle component analysis (PCA) transformation on original features that are not available due to confidentiality
- **Amount:** The amount of the transaction
- **Class:** The status of the transaction with respect to authenticity. The class of an authentic (resp. fraudulent) transaction is taken to be $0$ (resp. $1$)

## Project Objective

The objective of the project is to detect anomalies in credit card transactions. To be precise, given the data on `Time`, `Amount` and transformed features `V1` to `V28`, our goal is to fit a [**probability distribution**](https://en.wikipedia.org/wiki/Probability_distribution) based on authentic transactions, and then use it to correctly identify a new transaction as authentic or fraudulent. Note that the target variable plays no role in constructing the probability distribution.

## Overview

- We carry out necessary feature extraction and feature transformation.
- As the anomaly detection algorithm suffers from high-dimensional data, we figure out the most relevant features separating the target classes, and use only those in the modeling purpose.
- Based on the training data, we fit a [**multivariate normal distribution**](https://en.wikipedia.org/wiki/Multivariate_normal_distribution).
- Given a new transaction, if the corresponding density value of the fitted distribution is lower than a pre-specified threshold, then we flag the transaction as fraudulent.
- In this notebook, we focus more on the true positive class (the class of fraudulent transactions) than the true negative class (the class of authentic transactions). This is because a false negative (the algorithm predicts a fraudulent transaction as authentic) is far more dangerous than a false positive (the algorithm predicts an authentic transaction as fraudulent, which can always be cross-verified). For this reason, we use $F_2$-score as the [**evaluation metric**](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers).
- The choice of the threshold is optimised by iterating over a pre-specified set of values, predicting on the validation set, and evaluating the predictions by means of the $F_2$-score.
- In this work, the optimal threshold value comes out to be $0.009^9 \approx 3.87 \times 10^{-19}$.
- The corresponding $F_2$-score for predictions on the validation set is $0.834671$, which is an optimistic projection due to the threshold tuning over the validation set.
- Applying the same model on the test set, we get predictions with an $F_2$-score of $0.816492$.