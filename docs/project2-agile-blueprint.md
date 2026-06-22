# Agile Systems Blueprint: Subscription Revenue Optimization Engine

## 1. Business Problem Statement
The executive leadership team has identified an unmitigated 4.2% month-over-month increase in customer churn within our premium subscription tiers. Currently, revenue metrics are siloed, calculations for Monthly Recurring Revenue (MRR) are handled manually in spreadsheets, and there is zero visibility into Customer Lifetime Value (CLV) lifecycle patterns. 

**Objective:** Engineer an automated subscription analytics platform to track recurring revenue metrics, run cohort retention analysis, and serve a visual executive dashboard for strategic retention intervention.

## 2. Product Backlog & Jira Epic Breakdown

### EPIC 1: Systems Analysis & Requirements Scoping (BA/PM Layer)
* **User Story 1 (As a VP of Finance):** I want an automated system to calculate our Monthly Recurring Revenue (MRR) based on daily active subscription states so that I do not have to rely on manual, error-prone spreadsheets.
  * *Acceptance Criteria:* Must aggregate total active contract values dynamically, accounting for downgrades and cancellations.
* **User Story 2 (As a VP of Customer Success):** I want to view a monthly cohort retention matrix so that I can isolate exactly which month's sign-ups are canceling their subscriptions early.
  * *Acceptance Criteria:* System must group users by their signup month (cohort) and calculate percentage retention across a 12-month lifecycle.

### EPIC 2: Data Architecture & Analytics Engineering (Technical Layer)
* **Task 1:** Design a normalized schema capable of handling subscription life-cycle states (Active, Upgraded, Cancelled, Churned).
* **Task 2:** Develop complex SQL analytical scripts utilizing Window Functions and Common Table Expressions (CTEs) to isolate customer retention cohorts.

### EPIC 3: Business Intelligence & Executive Delivery (Visualization Layer)
* **Task 1:** Map out a semantic data layer to connect the data warehouse models directly to an interactive BI workspace.
* **Task 2:** Construct an executive dashboard layout visualizing core North-Star metrics: MRR, Churn Rate, LTV, and LTV-to-CAC ratios.
