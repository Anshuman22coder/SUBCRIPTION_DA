# 📊 SaaS Subscription Retention & Churn Analysis (SQL & Power BI)

An end-to-end data analytics project evaluating subscriber behavior, retention trends, and churn patterns from September 2022 to September 2023.

---

## 📌 Project Overview
* **Objective:** Track customer retention and churn dynamics, evaluate cohort performance over a 13-month timeline, and identify long-tenure subscribers.
* **Tech Stack:** Microsoft SQL Server (T-SQL), Power BI, DAX, Excel.
* **Key Metrics:** Overall Retention Rate, Churn Rate, Month-over-Month Signups, Long-term Retainers ($\ge 150$ days).

---

## 📈 Key Metrics Summary
| Metric | Value | Business Interpretation |
| :--- | :--- | :--- |
| **Total Subscriptions** | 3,069 | Total historical volume analyzed |
| **Active / Retained Subscriptions** | 1,065 | Subscriptions where `canceled_date IS NULL`[cite: 1] |
| **Canceled / Churned Subscriptions** | 2,004 | Subscriptions where `canceled_date IS NOT NULL`[cite: 1] |
| **Overall Retention Rate** | **34.70%** | Retained / Total[cite: 1] |
| **Overall Churn Rate** | **65.30%** | Churned / Total[cite: 1] |
| **Long-Term Retainers ($\ge 5$ Months)** | 1,226 | Users active for at least 150 days[cite: 1] |

---

## 🔍 Key SQL Queries & Logic

### 1. Monthly Cohort Retention Rate
```sql
SELECT 
    FORMAT(created_date, 'yyyy-MM') AS signup_month,
    COUNT(*) AS total_signups,
    CAST(COUNT(IIF(canceled_date IS NULL, 1, NULL)) * 100.0 / COUNT(*) AS DECIMAL(6,2)) AS retention_rate
FROM SubscriptionData  
GROUP BY FORMAT(created_date, 'yyyy-MM')
ORDER BY signup_month;
```

### 2. Identifying Long-Term Retainers ($\ge 150$ Days)
```sql
SELECT COUNT(*) AS manifold_subscribers
FROM SubscriptionData 
WHERE DATEDIFF(DAY, created_date, ISNULL(canceled_date, GETDATE())) >= 150;
```

---

## 💡 Business Findings & Insights
1. **Tenure Maturity vs. Apparent Retention:**
   * Recent signups (e.g., September 2023) show retention rates as high as **96.97%**[cite: 1]. This reflects low exposure to churn opportunities due to short account life, rather than permanent loyalty.
   * Older cohorts (September 2022) sit at **9.22% retention**[cite: 1], reflecting their cumulative churn exposure over 12 full months.
2. **Core Retention Baseline:**
   * Over 1,226 subscribers stayed active for more than 150 days (5 months)[cite: 1], highlighting a loyal core segment that sustains revenue.
3. **Repeat Subscribers:**
   * Identified accounts with multiple subscriptions (`HAVING COUNT(created_date) > 1`)[cite: 1], indicating a reactivation funnel that can be targeted with automated re-engagement campaigns.

---
## 🖥️ Power BI Dashboard

### Page 1: Subscription KPI Overview
*Retention vs. Churn rates, active subscriber counts, and sign-up velocity.*

![Subscription KPI Overview](<Screenshot 2026-09-20 225351.png>)

---

### Page 2: Cohort & Lifecycle Analysis
*Tenure distribution, cohort retention curve, and monthly churn trends.*

![Cohort and Lifecycle Analysis](<Screenshot 2026-09-20 225357.png>)

---

## 🚀 How to Run Locally
1. Clone the repo:
   ```bash
 
git clone https://github.com/Anshuman22coder/SUBCRIPTION_DA.git
   ```
2. Run SQL scripts in `SQL/sqls_for_subscribtion_dataset.docx` in SQL Server Management Studio (SSMS).
3. Open `subscription_.pbix` in Power BI Desktop to view the interactive visuals.
