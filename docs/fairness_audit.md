# SmartLend Fairness Audit

Complete this template during the Topic 3 lab using results from `notebooks/03_fairness_audit.ipynb`.

## Model Under Review

Logistic Regression model with no class weighting

## Dataset

Give Me Some Credit (Kaggle) data/processed/cs-processed.csv Test rows: 29336

## Groups Examined

Under 35     3662
35-60       16635
Over 60      9039

## Metrics Computed

=== Group-Level Metrics ===
                n  default_rate  accuracy  predicted_default_rate  false_positive_rate  false_negative_rate
Under 35   3662.0         0.102     0.897                   0.016                0.009                0.928
35-60     16635.0         0.069     0.931                   0.005                0.002                0.967
Over 60    9039.0         0.027     0.973                   0.000                0.000                0.996

## Fairness Metric Selected

False negative rate gap was selected as the criterion, so false negatives means that actual defaulters whom the model identified as non-risk related, which will directly translate into SmartLend losing money and harm people financially getting credit that they cannot afford.

## Gap Value and Interpretation

So with FNR values across the demographics, the model misses 99.6% of defaults in Over 60s, 96.7% in 35-60s and 92.8% in Under 35s, the model is essentially blind when it comes to default risk ESPECIALLY in older demographics.

## Likely Causes

The initial dataset already has a big imbalance of 94-6 towards "No Default", splitting this into group level metrics the actual default rates of U35s was much higher than other groups at 10.2%, so the model has a bias to the fact that O60s have the lowest actual default rate at just 2.7% of any demographic due to understanding the historical pattern of Over 60s representing safe borrowing, this is why FNR is essentially 0 for Over 60s as it is never predicting a default due to the low default rate. On the opposite side, over 35s have the highest default rate so the model makes more default predictions meaning they have the highest predicted default rate at 0.016, due to the higher default prediction, FPR is the highest with U35s. The disparities in metrics show an imbalanced model relying on baseline risks that vary to try and maximise accuracy, instead of an actual bias in the code against a certain demographic.

## Recommended Action

Choose one:

- Do not deploy pending further investigation

Justification:

The model in its current form has a 96.4% average FNR, raising to 99.6% depending on age demographic that is applying, this is an unacceptable financial risk for SmartLend. Coupled wit the Equal Opportunities Gap that negatively impacts under 35s.

## Known Limitations of This Audit

This model didn't explore intersectional groups, specifically relating to the low default rate among over 60s, what were the incomes of the over 60s, 35-60s and under 35s and were default rates of low income/age related in any way. Given that money is ultimately going to be related to default rate, income should've been explored a little more.

