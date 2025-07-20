# Credit-Scoring-Task
This document analyzes the results of credit scoring for all wallets using a deep autoencoder-based approach. Each wallet was scored on a scale of 0–1000, where higher scores indicate behavior more similar to responsible, low-risk DeFi usage, and lower scores indicate potential risk or anomalies.


## 1. Introduction
This project implements a credit scoring system for DeFi wallets using transaction-level data from the Aave V2 protocol. Each wallet is assigned a credit score (0–1000), with higher scores indicating more trustworthy and stable behavior. The method leverages an unsupervised deep autoencoder neural network to assess behavioral normality and risk, without requiring any pre-labeled data.


## 2. Methodology: Why Deep Autoencoder?
Autoencoders are neural networks trained to reconstruct their input. When trained on unlabeled data, they learn the structure of typical wallet behavior.


Key idea: Most “normal” wallets are reconstructed well (low error), while risky or unusual wallets aren’t (high error). The reconstruction error is used as a proxy for credit risk.

Advantages:

Captures complex, nonlinear transaction patterns.

Scales to large user bases.

Requires no prior risk labels (fully unsupervised).

Robust to variations in user behavior.

## 3. Data Processing Flow
 
### Step 1: Data Ingestion
The input is a JSON file where each row is a wallet transaction (fields: action, amount, assetSymbol, timestamp, etc.).

Data is loaded using pandas.

### Step 2: Feature Engineering
Transactions are grouped by userWallet to compute wallet-level behavioral features:

Total and count of deposits, withdrawals, borrows, repayments, liquidations.

Withdrawal-to-deposit ratio.

These features form a comprehensive profile of each wallet’s behavior on Aave.

### Step 3: Preprocessing
Features are standardized using StandardScaler for stable, balanced neural network training.

### Step 4: Deep Autoencoder Design
Architecture:

Input layer: Dimensionality equals number of engineered features (e.g., 11).

Encoder:

8 neurons, ReLU activation

4 neurons, ReLU activation

2-neuron bottleneck (learns compact representation)

Decoder:

4 neurons, ReLU activation

8 neurons, ReLU activation

Output layer: reconstructs original features

Loss: Mean squared error (MSE).

Optimizer: Adam.

Early stopping: Stops training when validation loss doesn’t improve for 5 epochs.

Benefits: The bottleneck forces the network to capture only essential “normal” behavioral patterns for accurate reconstruction.

### Step 5: Training
Model is trained on all wallets’ features (80% train / 20% validation split).

Trains to minimize the ability to distinguish “normal” behavior from “unusual” through reconstruction.

### Step 6: Credit Score Generation
Each wallet’s features are reconstructed by the trained autoencoder.

The mean squared reconstruction error (mse) is computed.

Credit Score Scaling:
creditScore = 1000 * (1 - (mse - mse.min()) / (mse.max() - mse.min()))

Low reconstruction error → high score (trusted)

High reconstruction error → low score (risky)

### Step 7: Output
Generates a CSV file:

userWallet, creditScore_autoencoder

Highest scores represent DeFi users with the most typical and trustworthy Aave usage.

## 4. How to Run
Upload your user_transactions.json in the same directory or through Google Colab.

Run all cells in the provided notebook/script.

The output file wallet_credit_scores_autoencoder.csv will be generated.

## 5. Customization/Extension
Feature engineering: Add time-based or ratio features for greater nuance.

Model tuning: Adjust layer sizes or add dropout for larger, more complex datasets.

Validation: If labeled risk outcomes later become available, further validate or refine the cutoff thresholds for scores.

## 6. Justification and Value
This method automatically discovers subtle, nonlinear signals of creditworthiness from raw behavior.

Using a deep autoencoder is both state-of-the-art in unsupervised anomaly and risk detection, and well-suited to dynamic, label-sparse domains like DeFi credit markets.

Project by [Daniel Y]
Date: [20/07/2025]
