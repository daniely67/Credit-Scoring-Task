# DeFi Wallet Credit Scoring with Rule-Based Approach

## 1. Introduction
This project implements a rule-based credit scoring system for DeFi user wallets, using transaction-level data from the Aave V2 protocol. The system assigns a credit score (0–1000) to each wallet, representing the user's financial reliability and risk profile, based on explicit, transparent rules derived from their on-chain behaviors.

## 2. Methodology: Why Rule-Based Scoring?
Rule-based approaches use domain knowledge and simple if-then logic to assign risk or trust points to user behaviors.

Key idea: Specific patterns—such as frequent liquidations or large withdrawals—are penalized, while regular deposits and timely repayments are rewarded.

Advantages:

Maximum transparency: Rules are explicit and easily explainable.

Immediate interpretability: Stakeholders and auditors can trace every score.

Adaptable: Rules can be refined as understanding of risky behaviors evolves.

## 3. Data Processing Flow
Step 1: Data Ingestion
Input data is a JSON file where each row is a transaction (e.g., action, amount, asset, timestamp).

Data is loaded with pandas.

## Step 2: Feature Engineering
Transactions are grouped by userWallet to calculate wallet-level metrics:

Total and count of deposits, withdrawals, borrows, repayments, liquidations.

Withdrawal-to-deposit ratio.

## Step 3: Scoring Rules
Sample rules applied:

Start with a base score: e.g., 1000 points.

Penalties:

Subtract 200 points for each liquidation event (high risk).

Subtract 100 points if the withdrawal-to-deposit ratio exceeds 0.7 (potentially draining their account).

Subtract 400 points if no deposits were made (inactivity/risk of counterparty).

Rewards:

Add 50 points for more than three deposit events (active, reliable use).

Clamp: Final scores are limited to the 0–1000 range.

All rule thresholds and point values can be easily configured to match protocol or project risk policy.

## Step 4: Output
Results are output as a CSV file:

userWallet, creditScore_rule_based

Highest scores reflect wallets with safe, active, and responsible Aave usage.

## 4. Example Rule-Based Scoring Formula
python
score = 1000
score -= 200 * num_liquidations
if withdrawal_deposit_ratio > 0.7:
    score -= 100
if total_deposits == 0:
    score -= 400
if num_deposits > 3:
    score += 50
score = max(0, min(score, 1000))
5. How to Run
Place your user_transactions.json in the working directory or upload in Colab.

Run the rule-based scoring script or notebook.

The output file wallet_credit_scores_rule_based.csv will be generated.

## 6. Customization/Extension
Editable rules: Adjust criteria or point values to reflect evolving protocol risk factors.

Feature expansion: Introduce new behaviors (e.g., loan longevity, asset diversity, activity recency).

Hybridization: Use rule-based scores as part of a wider model or as a compliance backstop.

## 7. Justification and Value
This approach is transparent, explainable, and auditable, making it ideal for regulated or community-facing credit scoring tasks.

It provides clear, operationalizable insights into user risk and trustworthiness based on actual on-chain actions.

Project by [Daniel Y]
Date: [20/07/2025]
