# MapleFreight Delivery Delay Prediction & Slack Alerts

**Student Name:** Diksha Sharma  
**Student ID:** c0965344  
**Course:** AML 3303  

## Project Overview

MapleFreight Logistics handles approximately 8,000 shipments per week across Ontario, Quebec, and the Prairies. Its on-time delivery performance has decreased from 94% to 88%.

This project develops a machine learning solution to identify in-transit shipments that are at high risk of arriving late. High-risk shipments are automatically communicated to the dispatch team through Slack so that corrective action can be taken before the promised delivery window is missed.

## Dataset

The dataset contains shipment, route, carrier, service-level, weather, traffic, vehicle, driver, and operational information.

The target variable is `delivered_late`, where:

- `0` = shipment delivered on time
- `1` = shipment delivered late

The post-delivery variable `actual_transit_hours` was excluded from the predictive features to prevent data leakage.

## Data Preparation

The project includes:

- Missing-value handling
- Duplicate removal
- Category standardization
- Data-quality checks
- Numerical and categorical preprocessing
- One-hot encoding of categorical variables
- Train/test splitting
- Class-imbalance handling

## Exploratory Data Analysis

EDA identified several factors associated with late deliveries.

Key observations included:

- Economy shipments had the highest late-delivery rate at approximately 26.56%.
- Late shipments had a higher average pickup delay.
- Late shipments experienced higher average traffic levels.
- Route congestion was higher among late shipments.
- Shipments with more prior late deliveries also showed increased risk.

## Machine Learning Models

Two classification approaches were evaluated:

### Logistic Regression

The Logistic Regression baseline produced approximately:

- Accuracy: 0.741
- Precision: 0.425
- Recall: 0.737
- F1-score: 0.539

The relatively high recall makes this model useful for identifying shipments that may require intervention.

### Random Forest

The Random Forest model produced approximately:

- Accuracy: 0.807
- Precision: 0.857
- Recall: 0.073
- F1-score: 0.134

Although Random Forest achieved higher overall accuracy and precision at the default threshold, its recall was very low. This demonstrates why accuracy alone is not an appropriate metric for this imbalanced business problem.

## Model Explainability

Random Forest feature importance was used to investigate the variables influencing predictions.

Important features included:

- Scheduled transit hours
- Traffic index
- Distance
- Pickup delay
- Route congestion
- Driver experience
- Shipment weight
- Fuel cost
- Vehicle age
- Number of stops

These findings can help dispatch teams understand operational conditions associated with delivery risk.

## Slack Alert Integration

A Slack Incoming Webhook was integrated with the prediction workflow.

High-risk shipments are sent to the private `#dispatch-alerts` channel. The alert includes:

- Number of high-risk shipments
- Shipment ID
- Destination
- Carrier
- Predicted late-delivery risk

The system identified 428 high-risk shipments in the evaluated set and sent the five highest-risk shipments to Slack for dispatch review.

Evidence of the working integration is available at:

`screenshots/slack_alert.png`

The Slack webhook URL is stored locally in a `.env` file and is not committed to the repository.

## Security

The project uses environment variables to protect the Slack webhook credential.

The repository includes:

- `.gitignore` to exclude `.env`
- `.env.example` showing the required variable name
- No Slack webhook URL in the notebook or repository

## Running the Project

Install the required packages using:

`pip install -r requirements.txt`

Create a local `.env` file containing:

`SLACK_WEBHOOK_URL=your_actual_slack_webhook_url`

Then run the notebook from top to bottom.

## Business Recommendations

MapleFreight should prioritize shipments with high predicted late-delivery risk for proactive dispatch intervention. Particular attention should be given to pickup delays, heavy traffic, congested routes, service level, and shipments with a history of late deliveries.

Potential interventions include rerouting shipments, contacting carriers, adjusting dispatch priorities, and proactively notifying customers.

## AI Assistant Disclosure

An AI assistant (ChatGPT) was used during the project for guidance with code structure, debugging, interpretation of model results, documentation, and Slack integration.

One AI-generated suggestion incorrectly referred to the Random Forest pipeline using the variable name `model_rf`, while the actual pipeline in the notebook was named `rf_model`. This caused a `NameError`. The issue was identified by checking the previously defined model variable and correcting the code to use `rf_model`.

AI-generated suggestions were reviewed and tested before being incorporated into the final project.

## Project Files

- `MapleFreight_Delivery_Delay_Prediction_c0965344.ipynb` - complete analysis and modelling notebook
- `README.md` - project documentation
- `requirements.txt` - Python dependencies
- `.env.example` - environment-variable template
- `.gitignore` - excludes private environment files
- `screenshots/slack_alert.png` - evidence of successful Slack alert
