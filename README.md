# 💼 SavingBox Campaign & Financial Behavior Dashboard

**Created by:** Fidel Umukoro  
📧 [fidelumukoro@gmail.com](mailto:fidelumukoro@gmail.com)  

A comprehensive 4-page Power BI dashboard designed to provide deep insights into **user engagement**, **campaign performance**, **savings behavior**, and **loan risk** for a mock fintech platform—**SavingBox**. This project simulates real-world stakeholder questions and delivers **actionable analytics** to guide strategic decisions.

---

## 🔗 Project Resources

- 📊 [Live Power BI Dashboard](https://app.powerbi.com/view?r=eyJrIjoiOWJlYTYyYTQtNDAwNi00MDA5LWE3ODgtMzU3NTI4N2E0ODZlIiwidCI6ImY0MzFkYTgxLWIyYmQtNGRiNC1iNjk3LTNhOTUzYzA5MDk0NSJ9)  
- 📄 [PDF Report Summary](https://shorturl.at/dWpoG)  
- 📁 [Download Dataset (Excel/CSV)](https://shorturl.at/nf1IV)

---

## 🧾 Project Overview

This end-to-end BI solution replicates a data-driven environment for a digital finance product. Key use cases:

- Track user activity and monthly retention
- Analyze campaign conversion across demographics and regions
- Segment users by savings behavior and frequency
- Assess loan performance and default risk

---

## 🧼 Excel Data Cleaning

The dataset was cleaned in Excel before loading into Power BI. Key preprocessing steps:

1. **Fill missing demographic fields:**

```excel
=IF(A2="", "Unknown", A2)

🧠 Power BI Data Modeling
Fact and dimension tables were connected using the following relationships:

users[User_id] → savings[User_id], campaign_participation[User_id], loans[User_id]

savings[Date], activity_log[Timestamp] → DateTable[Date]

Date Table (DAX):

DAX
Copy
Edit
DateTable = 
ADDCOLUMNS(
    CALENDAR(MIN(savings[Date]), MAX(savings[Date])),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMMM"),
    "MonthNumber", MONTH([Date]),
    "Week", WEEKNUM([Date]),
    "Day", FORMAT([Date], "dddd")
)
🧮 Key DAX Measures
🟢 Engagement Metrics
Active Users:

DAX
Copy
Edit
Active Users = 
CALCULATE(
    DISTINCTCOUNT(activity_log[User_id]),
    FILTER(activity_log, activity_log[Activity_type] = "login")
)
Monthly Active Users (MAU):

DAX
Copy
Edit
MAU = 
CALCULATE(
    DISTINCTCOUNT(activity_log[User_id]),
    ALLEXCEPT(DateTable, DateTable[Month], DateTable[Year])
)
🎯 Campaign Metrics
Conversion Rate:

DAX
Copy
Edit
Campaign Conversion = 
DIVIDE(
    DISTINCTCOUNT(campaign_participation[User_id]),
    DISTINCTCOUNT(users[User_id])
)
Female Participation:

DAX
Copy
Edit
Participants (Female) = 
CALCULATE(
    COUNTROWS(campaign_participation),
    users[Gender] = "Female"
)
💰 Savings Insights
Avg Savings Per User:

DAX
Copy
Edit
Avg Savings Per User = 
AVERAGEX(
    SUMMARIZE(savings, savings[User_id], "Total", SUM(savings[Amount])),
    [Total]
)
Consistent Savers (3+ months):

DAX
Copy
Edit
Consistent Savers = 
COUNTROWS(
    FILTER(
        SUMMARIZE(savings, savings[User_id], "MonthCount", DISTINCTCOUNT(savings[Month])),
        [MonthCount] >= 3
    )
)
Savings Segment Logic:

DAX
Copy
Edit
Savings Segment = 
SWITCH(TRUE(),
    [Save Count] >= 10, "Power Saver",
    [Save Count] >= 3, "Casual Saver",
    "Low Activity"
)
💳 Loan Analysis
Default Rate:

DAX
Copy
Edit
Loan Default Rate = 
DIVIDE(
    CALCULATE(COUNTROWS(loans), loans[Loan_Status] = "Defaulted"),
    COUNTROWS(loans)
)
Avg Loan (Completed):

DAX
Copy
Edit
Avg Loan (Completed) = 
CALCULATE(
    AVERAGE(loans[Loan_Amount]),
    loans[Loan_Status] = "Completed"
)
🧩 Dashboard Pages
1. Overview
→ User base, engagement rate, MAU trends, churn rate

2. Campaign Analysis
→ Funnel, demographics, region and gender participation

3. Savings Behavior
→ Saving patterns by age/region, segmentation, frequency heatmaps

4. Loan & Risk
→ Default rates, loan performance, credit segmentation

Includes interactive slicers for: gender, region, age group, date, and product type.

❓ Key Business Questions Answered
Which user segments are most engaged with savings?

What campaigns drive the highest participation?

How do savings behaviors correlate with loan performance?

Where is user activity declining, and why?

✅ Strategic Recommendations
Encourage Locked Savings during onboarding to boost retention

Use nudges and auto-save prompts for low-frequency users

Incorporate savings history into credit scoring models

Localize campaigns by region and age for higher conversion
