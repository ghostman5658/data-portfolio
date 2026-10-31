# Telecom Customer Churn Prediction

A model that flags telecommunications customers who are likely to cancel, and names the account patterns a retention team can actually change. The recommended use is a pilot outreach list, not an automatic discount.

## Business question

Can customer demographics, service subscriptions, and billing data predict which customers will cancel, and what should the company do about the customers who are most at risk?

Keeping a customer is usually cheaper than replacing one. Churn is also uneven: a few account patterns carry much more of the risk. A useful model turns that pattern into a list of people to contact before they decide to leave.

## Who this is for

Marketing and retention teams who design the offer, product managers who bundle add-on services, and customer-service leads who decide where to spend outreach time. The model is only worth deploying if the intervention is cheap enough to survive a false alarm.

## Data

The source is the IBM Telco Customer Churn dataset, a public teaching file of a fictional telecommunications company:

https://github.com/IBM/telco-customer-churn-on-icp4d

It contains 7,043 customers and 21 columns.

- Demographics: gender, senior-citizen status, partner, and dependents.
- Services: phone, multiple lines, internet type, security, backup, device protection, tech support, and streaming.
- Account: tenure (0 to 72 months), contract, paperless billing, payment method, monthly charges, and total charges.
- Target: whether the customer churned.

About 26.5 percent of customers churned and 73.5 percent did not. A model that always predicts "stayed" is about 73.5 percent accurate and catches none of the cancellations, so accuracy alone is the wrong score.

## Preparation

1. Drop `customerID`. It identifies a row and does not predict churn.
2. Convert `TotalCharges` from text to a number. Eleven values are blank, and all eleven are customers with tenure 0 who have not finished a billing cycle. Those blanks are filled with `MonthlyCharges`, which is the only charge those customers would owe. Dropping them would remove the newest customers, who are also the most likely to leave.
3. One-hot encode the categorical fields with the first level dropped, producing 30 binary features from 16 categorical columns.
4. Split 80/20 with stratification, seed 42: 5,634 training rows and 1,409 test rows.
5. Standardize tenure, monthly charges, and total charges for logistic regression.
6. Handle the class imbalance with `class_weight='balanced'` rather than by copying or deleting rows.

## Methods

Four models answer different questions:

| Model | Role | Settings |
| --- | --- | --- |
| Majority-class baseline | Shows why accuracy misleads | Always predicts no churn |
| Logistic regression | Shows which factors push toward or away from churn | Balanced weights, `max_iter=1000` |
| Decision tree | Shows a short set of rules | Depth 5, balanced weights |
| Random forest | Primary model | 200 trees, depth 10, balanced weights |

Success on the holdout was defined in advance: recall at least 0.70, AUC-ROC at least 0.80, precision at least 0.50, and F1 at least 0.60. Recall is first because a missed cancellation costs more than an extra phone call.

## Validation and results

All three trained models cleared those bars. The random forest is the one to pilot.

| Model | Accuracy | Precision | Recall | F1 | AUC-ROC |
| --- | --- | --- | --- | --- | --- |
| Majority-class baseline | 0.735 | 0.000 | 0.000 | 0.000 | 0.500 |
| Logistic regression | 0.738 | 0.504 | 0.783 | 0.614 | 0.842 |
| Decision tree | 0.735 | 0.500 | 0.808 | 0.618 | 0.831 |
| Random forest | 0.757 | 0.528 | 0.778 | 0.629 | 0.841 |

The forest's feature importances and the logistic coefficients point at the same levers:

1. Contract type is the strongest pattern. Month-to-month customers churn at about 43 percent, one-year contracts at about 11 percent, and two-year contracts at about 3 percent.
2. The first 12 months are the highest-risk window. Risk falls as tenure grows.
3. Fiber customers churn more than DSL customers.
4. Customers without security, tech support, backup, or device protection churn more. Those add-ons are a retention lever.
5. Electronic-check payers churn more than customers on automatic bank or card payments.
6. Higher monthly bills line up with higher churn, especially on month-to-month contracts.

Precision of about 0.53 means roughly half of the flagged customers would not have churned. That is acceptable for email or a service call. It is a weak basis for an expensive contract discount unless the threshold is tuned to the cost of the offer.

Gender shows little difference in recall or precision. Senior-citizen status shows a modest recall gap and should be checked again if the model is used on live customers. This file is fictional billing data, so those checks are a rehearsal for a real deployment, not a finding about a real company.

## Key visuals

The source notebook includes the class balance, churn rate by contract and internet type, the model-comparison table, ROC curves, random-forest feature importance, logistic coefficients, a fairness split, and the random-forest confusion matrix. Those figures are the ones to put on a project page.

## Ethics and limitations

- The data are a public fictional teaching set. The rates above describe that file. They are not a claim about a named carrier.
- Using real billing data to target retention offers should be disclosed in the privacy notice. Customers provided those fields to receive service.
- Offers should not quietly differ by gender, age, or another protected trait. A quarterly fairness check, human review of high-impact offers, and a scheduled retrain belong in any live version.
- The split is random, not by time. A pilot should confirm the score still works on later customers before it drives a campaign.
- The forest was not grid-searched, and the project did not test gradient boosting. Both are reasonable next steps. They are not required to see the contract and tenure pattern.

## How to reproduce

Clone the public repository and run this project from its folder:

https://github.com/ghostman5658/data-portfolio/tree/main/projects/telecom-customer-churn

```bash
git clone https://github.com/ghostman5658/data-portfolio.git
cd data-portfolio
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Open `projects/telecom-customer-churn/analysis.ipynb` and run all cells. The notebook reads `Telco-Customer-Churn.csv` from that folder. The split seed is 42.

The write-up is [`paper.pdf`](paper.pdf). The presentation is [`presentation.pdf`](presentation.pdf).

## Recommended pilot

1. Offer a contract upgrade to month-to-month customers.
2. Contact new customers during the first 12 months.
3. Discount a security or support add-on for customers who have none.
4. Treat fiber subscribers as a competitive-risk segment, not only as a high-bill segment.
5. Nudge electronic-check customers toward automatic payment.

Start with the low-cost actions. Tune the score threshold before spending real discount dollars.
