# MapleFreight Delivery Delay Prediction & Slack Alerts

**Course:** AML 3303  
**Student Name:** Narinder Kaur  
**Student ID:** c0965335  
**Project:** Assessment 1 – Delivery Delay Prediction & Slack Alerts

## 1. Project Overview

MapleFreight Logistics delivers approximately 8,000 shipments every week across Ontario, Quebec, and the Prairies.

The company has experienced a decrease in on-time delivery performance from 94% to 88%. Late deliveries create additional costs, customer complaints, and operational problems.

The purpose of this project is to develop a machine learning model that predicts which shipments are likely to arrive late and sends automatic notifications to the dispatch team using Slack.

## 2. Problem Statement

Currently, MapleFreight often identifies delivery delays only after the promised delivery time has passed.

This project aims to identify high-risk shipments earlier so the dispatch team can take action before customers experience delays.

## 3. Project Objectives

The main objectives are:

1. Understand and explore the shipment dataset.
2. Identify important factors contributing to delivery delays.
3. Clean and preprocess the dataset.
4. Build and compare machine learning models.
5. Evaluate the model using precision, recall, F1-score, and a confusion matrix.
6. Identify high-risk shipments using a probability threshold.
7. Send automatic Slack alerts.
8. Provide business recommendations to improve delivery performance.

## 4. Dataset Description

**Dataset:** `maplefreight_delivery_delay_dataset.csv`

The original dataset contains:

- 6,035 shipment records
- 23 columns
- Shipment, carrier, route, traffic, weather, and delivery information

**Target variable:** `delivered_late`

- `0` = Shipment delivered on time
- `1` = Shipment delivered late

Approximately 21% of shipments are late, creating a class imbalance.

The column `actual_transit_hours` was excluded from model training because actual delivery time is not available when making predictions.

## 5. Data Cleaning and Preprocessing

The dataset required cleaning before machine learning.

The main preprocessing steps included:

- Checking missing values
- Removing duplicate and ambiguous shipment records
- Standardizing inconsistent category names
- Correcting suspected unit errors
- Handling invalid numerical values
- Encoding categorical variables
- Scaling numerical features where required

These steps helped prepare the dataset for model training.

## 6. Exploratory Data Analysis

Exploratory Data Analysis (EDA) was performed to understand the dataset and identify possible reasons for late deliveries.

The analysis examined:

- Delivery delay distribution
- Weather conditions
- Carrier performance
- Service levels
- Customer priority tiers
- Numerical feature relationships

Graphs and visualizations were created using Python libraries such as Matplotlib and Seaborn.

The EDA helped identify patterns that could be useful for predicting delivery delays.

## 7. Machine Learning Models

A baseline model was developed first to provide a reference for comparison.

Additional machine learning modelling was performed to identify a better approach for predicting late deliveries.

**Selected final model: Logistic Regression**

Logistic Regression was selected because it provides understandable probability predictions and supports risk-based classification.

The model was evaluated using classification metrics rather than accuracy alone because the dataset contains more on-time shipments than late shipments.

## 8. Final Model Performance

The final Logistic Regression model produced the following reported results:

| Evaluation Metric | Result |
|---|---|
| Accuracy | 77.7% |
| Precision | 47.0% |
| Recall | 65.6% |
| F1-score | 54.7% |
| Selected Risk Threshold | 0.55 |

**Interpretation:**

- Accuracy shows the percentage of correct predictions.
- Precision shows how many shipments predicted as late were actually late.
- Recall shows how many actual late shipments were correctly identified.
- F1-score balances precision and recall.

Recall is particularly important because missing a shipment that will arrive late could prevent dispatch from taking timely action.

The notebook contains the baseline comparison, final evaluation, and confusion matrix.

## 9. Risk Threshold Selection

The selected classification threshold is **0.55**.

This means shipments with a predicted probability of late delivery of 55% or higher are classified as high risk.

The threshold was selected to balance identifying late shipments with limiting unnecessary alerts.

Using this threshold, the model identified **345 high-risk shipments in the test dataset**.

This threshold can be adjusted in the future depending on dispatch capacity and business requirements.

## 10. Slack Alert Integration

Slack was integrated into the project using an Incoming Webhook.

A Slack application named **MapleFreight Alerts** was created and connected to the `#dispatch-alerts` channel.

The integration uses Python's `requests` library to send HTTP POST requests.

The notification includes:

- Total number of high-risk shipments
- Shipment IDs
- Destination cities
- Carrier names
- Predicted risk scores

**Slack integration result:** Successful.

The webhook test returned HTTP status `200` and response `ok`.

The real Slack notification displayed 345 high-risk shipments and details of the five highest-risk shipments.

**Screenshot:** `screenshots/slack_alert.png`

The webhook URL is stored privately in `.env` and is not included in the public repository.

## 11. Business Recommendations

Based on the analysis and model predictions, I recommend:

1. **Monitor high-risk shipments:** Dispatch should review shipments with high predicted delay probabilities.
2. **Improve carrier performance:** Regularly evaluate carriers with repeated delivery delays.
3. **Plan for weather conditions:** Use weather forecasts when planning delivery routes.
4. **Reduce traffic-related delays:** Consider congestion when scheduling deliveries.
5. **Prioritize important customers:** Pay special attention to high-priority shipments.
6. **Use Slack notifications:** Provide early warnings so dispatch can investigate delays and communicate with customers.

These recommendations may help improve on-time delivery performance and customer satisfaction.

## 12. Project Files

```text
MapleFreight_Delivery_Delay_Prediction/
│
├── MapleFreight_Delivery_Delay_Prediction_c0965335.ipynb
├── maplefreight_delivery_delay_dataset.csv
├── README.md
├── requirements.txt
├── .gitignore
├── .env.example
└── screenshots/
    └── slack_alert.png
```

The local `.env` file is excluded from GitHub for security reasons.

## 13. How to Run the Project

**Step 1:** Download or clone the GitHub repository.

**Step 2:** Open the project folder in VS Code or Jupyter Notebook.

**Step 3:** Install the required libraries:

```bash
python -m pip install -r requirements.txt
```

**Step 4:** Open:

`MapleFreight_Delivery_Delay_Prediction_c0965335.ipynb`

**Step 5:** Select the correct Python kernel.

**Step 6:** Run the notebook cells from top to bottom.

**Step 7:** To enable Slack alerts, create a `.env` file using `.env.example` and add a valid Slack Incoming Webhook URL:

```text
SLACK_WEBHOOK_URL=YOUR_PRIVATE_WEBHOOK_URL
```

Set `SEND_SLACK_ALERT = True` when testing the integration with your own Slack workspace.

Without a configured webhook, leave Slack sending disabled.

## 14. Technologies Used

- Python
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Requests
- Python-dotenv
- Slack Incoming Webhooks
- GitHub
- Visual Studio Code

## 15. AI Usage

## 15. AI Usage

**Tool used:** ChatGPT (OpenAI), to help me understand and complete my assignment.

**What I used it for:** I used ChatGPT to help me plan my project, understand and debug Python code, clean the dataset, build the Logistic Regression model, create graphs, and set up Slack notifications. It also helped me organize my Jupyter Notebook and write my README in simple language. I checked the model results using the actual outputs from my notebook.

**A case where the output was wrong:** During Slack integration, ChatGPT initially told me that receiving HTTP status code 200 meant my Slack notification was sent successfully. However, when I checked my Slack channel, no message appeared. I also noticed that the response showed HTML instead of the expected `ok` message.

I checked the webhook URL and found that I was not using the correct Slack Incoming Webhook address. After correcting the URL and running the code again, I received `Status: 200` and `Response: ok`. This time, the notification appeared successfully in my `#dispatch-alerts` channel, showing 345 high-risk shipments and details of the five highest-risk shipments.

**What I learned:** I learned that AI can be helpful for understanding assignments and solving coding problems, but it does not always give correct answers. I should always check the results, test the code myself, and make sure everything works before submitting my assignment.
## 16. Conclusion

This project demonstrates how machine learning can help a logistics company predict delivery delays before they occur.

The final Logistic Regression model was used to identify shipments at risk of arriving late.

By integrating the model with Slack, high-risk shipment predictions were converted into notifications that the dispatch team can review.

The project shows how data analysis, machine learning, and automated communication can work together to support better operational decisions.

---

**Prepared by:** Narinder Kaur  
**Student ID:** c0965335  
**Course:** AML 3303

