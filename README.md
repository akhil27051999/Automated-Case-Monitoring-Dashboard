# Automated Case Monitoring Dashboard with Predictive Volume Forecasting

## 🚀 Overview

The **Automated Case Monitoring Dashboard** is a custom-built internal tool that provides real-time visibility into unassigned and SLA-sensitive support cases while proactively predicting future case volumes. Designed for high-scale operational environments, the dashboard ensures SLA adherence, optimizes workforce planning, and enables proactive workload distribution.

![Screenshot 2025-06-29 222901](https://github.com/user-attachments/assets/2c38f908-648e-44d1-ad64-e693f0191ec8)

## ❓ Problem Statement

Amazon’s transportation and logistics operations often handle large volumes of support cases. Prior to this dashboard, teams faced several key challenges:

- Manual tracking of **unassigned and at-risk cases** often led to missed SLAs.  
- **Lack of visibility** into aging or stagnant case queues delayed escalations.  
- No proactive mechanism to **forecast future case volumes** based on historical data.  
- Difficult for operations teams to **plan staffing or prioritize tasks** during peak loads.

These gaps resulted in SLA breaches, reactive firefighting, and sub-optimal workforce allocation.


## 🎯 Objective

Build a comprehensive and intelligent dashboard that:

- Tracks unassigned, aging, and SLA-sensitive cases in real-time.  
- Surfaces high-risk or stagnant tickets for timely triaging.  
- Predicts case volume trends based on historical patterns for the upcoming week.  
- Improves escalation management and operational decision-making.  
- Reduces SLA breaches and improves shift planning accuracy.


## 🛠️ Features

- ⚡ **Live Case Monitoring**: Displays real-time status of open, unassigned, and pending cases.  
- ⚠️ **SLA Violation Detection**: Flags aging tickets approaching SLA thresholds.  
- 📈 **Predictive Forecasting**: Uses historical weekly volume trends to forecast upcoming case loads.  
- 🧠 **GenAI-Powered Development**: JavaScript logic, UI scripting, and alert workflows were built using **ChatGPT** to optimize code quality and reduce development time.  
- 📊 **Dynamic UI**: Interactive frontend interface with filters, condition-based highlights, and visual prioritization.


## 🧱 Tech Stack

| Component         | Description                                |
|------------------|--------------------------------------------|
| **Frontend**      | JavaScript (with AmazonQ/GenAI assistance), HTML, CSS |
| **Backend Data**  | Internal Amazon ticketing APIs             |
| **Automation**    | JavaScript logic for case rules & polling  |
| **Forecasting**   | JavaScript logic for pattern detection using historical data |
| **Alerting**      | Integrated with internal notification systems |
| **Hosting**       | Deployed internally on Amazon ops tools    |


## 📈 Impact

- ✅ Reduced **SLA breaches by 25%** by surfacing high-risk cases early.  
- ✅ Enabled **better shift planning** through case volume forecasting.  
- ✅ Replaced manual case checks with an automated, self-refreshing dashboard.  
- ✅ Improved case assignment efficiency and reduced escalation backlog.  
- ✅ Recognized as a high-impact observability tool within the ops team.

## 📌 Outputs



## 🏆 Recognition

- Internally acknowledged for its innovation and impact on operational efficiency.  
![Screenshot 2025-06-29 222926](https://github.com/user-attachments/assets/46d8d908-ea7a-406e-84c8-b8691b4b993d)


## 🧠 How GenAI Was Used

- JavaScript development for DOM manipulation, rule-based filtering, and API integration was **accelerated using AmazonQ**.  
- Prompt-engineered solutions were used to design case logic, SLA thresholds, and dynamic alerts.  
- This AI-assisted development approach reduced implementation time and improved code accuracy.


## 🙋 Author

**[Akhil Thyadi]**  
AWS Cloud TransOps Specialist | Amazon  
LinkedIn: [https://www.linkedin.com/in/akhil-thyadi/]  

