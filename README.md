# Bank Churn Prediction using Neural Networks

## Project Summary

This project focuses on building and optimizing a neural network to predict customer churn for a bank. The goal is to identify customers who are likely to leave the bank in the next six months, enabling the bank to take proactive measures to retain them. The analysis involves comprehensive data exploration, preprocessing, and a systematic approach to model building and hyperparameter tuning to achieve high performance, with a specific focus on the recall metric.

## Business Problem

The primary business objective is to address customer churn. By accurately predicting which customers are at risk of leaving, the bank can strategically implement retention campaigns and improve services to enhance customer loyalty and reduce revenue loss associated with churn.

## Dataset

The analysis uses a dataset containing information about bank customers. Key attributes include:

-   **CustomerId, Surname:** Customer identifiers.
-   **CreditScore, Age, Tenure, Balance, NumOfProducts, EstimatedSalary:** Numerical features describing the customer's financial status and relationship with the bank.
-   **Geography, Gender:** Categorical features for customer demographics.
-   **HasCrCard, IsActiveMember:** Boolean flags indicating product ownership and activity.
-   **Exited:** The target variable, indicating whether the customer has churned (1) or not (0).

The dataset is imbalanced, with approximately 20.4% of customers having churned.

## Key Analysis Steps

### 1. Exploratory Data Analysis (EDA)

Initial analysis revealed several key insights:
-   Customers from Germany have a higher churn rate (32.4%) compared to France (16.15%) and Spain (16.67%).
-   Female customers have a higher churn rate (25%) than male customers (16.45%).
-   Customers with 3 or 4 products have extremely high churn rates (82.7% and 100%, respectively), while those with 2 products are the most loyal.
-   Inactive members are more likely to leave than active members.

### 2. Feature Engineering & Data Preprocessing

-   Irrelevant columns (`RowNumber`, `CustomerId`, `Surname`) were dropped.
-   Categorical features like `Geography` and `Gender` were numerically encoded.
-   The data was scaled for use in the neural network.

### 3. Base Model & Performance Improvement

A sequential neural network was built, and its performance was iteratively improved through several key stages:

-   **Base Model:** An initial model was established, which showed signs of overfitting.
-   **Optimizers:** The `Adam` optimizer was found to provide better results than the default `SGD`.
-   **Regularization:** `Dropout` layers (with a rate of 0.5) were introduced to combat overfitting and improve the model's ability to generalize to unseen data.
-   **Callbacks:** `EarlyStopping` was used to monitor validation loss and restore the best model weights, preventing overfitting and finding a well-generalized model with a validation recall of 73.2%.
-   **Class Imbalance:** The significant class imbalance was addressed using two primary techniques:
    1.  **Class Weights:** Assigning `class_weight='balanced'` in the model improved validation recall to 71.5%. Further tuning of the learning rate (reducing to 0.0001) and adding Dropout pushed validation recall to 78.6%.
    2.  **Oversampling:** The minority class was oversampled. This technique, combined with a lower learning rate and Dropout, resulted in a validation recall of 73.7%.

### 4. Hyperparameter Tuning with Keras Tuner

To find the optimal model architecture and hyperparameters, Keras Tuner was employed. It systematically searched through different combinations of:
-   Number of hidden layers and neurons
-   Dropout rates
-   Optimizers (`sgd_momentum`, `adam`)
-   Learning rates

### 5. Model Selection and Final Conclusion

The Keras Tuner search identified several high-performing models. The top two models, **0522** and **0585**, both achieved the best performance. After being retrained for 100 epochs, they demonstrated:

-   **Excellent Generalization:** The training and validation loss curves closely followed each other, indicating minimal overfitting.
-   **High Recall:** Both models achieved a final **validation recall of 86.8%**.

The final selected model (0522 or 0585) provides a reliable tool for the bank to predict customer churn, allowing for targeted interventions to retain valuable customers.
