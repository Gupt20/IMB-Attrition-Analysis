# IBM HR Attrition Analysis | Power BI Dashboard
### Identifying why employees leave — and what the business can do to stop it

[Dataset Source](https://www.kaggle.com/datasets/rohitsahoo/employee-attrition)

---

## Business Problem

A mid-sized company is losing **1 in 5 employees every year** — a 20% annual attrition rate that is costing the business in recruitment fees, lost productivity, and institutional knowledge walking out the door.

The HR Manager has been asked by the CEO to answer three specific questions before the next board meeting:

- **Where** is attrition concentrated — which departments and roles are most affected?
- **Who** is most at risk — what does the profile of an employee likely to leave look like?
- **Why** are they leaving — what are the root causes the business can actually act on?

This dashboard was built to answer those three questions clearly, so the HR team can make a targeted retention investment rather than applying a generic fix across the whole company.

---

## Dataset Overview

**Source:** IBM HR Analytics Employee Attrition & Performance (via Kaggle)  
**Size:** 1,470 employee records | 35 features  
**Type:** Fictional but realistic corporate HR dataset created by IBM data scientists

The dataset includes demographic information (age, gender, marital status), job details (department, role, salary, overtime), satisfaction scores (job satisfaction, work-life balance, environment satisfaction), and a binary attrition outcome (Yes/No) for each employee.

**One important note:** This dataset has no time dimension — it is a snapshot, not a trend. This means the analysis identifies risk profiles and patterns but cannot show whether attrition is getting better or worse over time. I have noted this as a limitation below.

---

## Tools and Approach

| Tool | Purpose |
|---|---|
| Power Query | Data cleaning, type conversion, removing zero-variance columns |
| DAX | Calculated measures (attrition rate, salary bands, risk scoring) |
| Power BI Desktop | Report building across 3 pages |
| Power BI Service | Publishing and sharing the live dashboard |

The raw data was loaded into Power BI and cleaned in Power Query — three columns (`EmployeeCount`, `StandardHours`, `Over18`) were removed as they held the same value for every row and added no analytical value. A numeric attrition column was created from the Yes/No field to enable rate calculations. Salary bands were created as a calculated column using DAX SWITCH logic.

**DAX measures created:**
```
Total Employees = COUNT(HR_Data[EmployeeNumber])
Attrited Employees = SUM(HR_Data[Attrition_Numeric])
Attrition Rate = DIVIDE([Attrited Employees], [Total Employees], 0)
Avg Salary (Leavers) = CALCULATE(AVERAGE(HR_Data[MonthlyIncome]), HR_Data[Attrition] = "Yes")
```

---

## Key Findings

> Each finding below is a specific, data-backed insight. Numbers are drawn directly from the dataset.

**Finding 1 — Overall attrition rate is 16.1%, heavily concentrated in two departments**

Of 1,470 employees, 237 have left — a 16.1% attrition rate. This is not evenly distributed. Sales accounts for 20.6% attrition and HR accounts for 19.0%, while Research & Development sits at 13.8%. The problem is not company-wide — it is a Sales and HR problem first.

**Finding 2 — Sales Representatives have the highest attrition of any role at 39.8%**

Nearly 4 in 10 Sales Representatives leave. This is the single most alarming number in the dataset. The next highest role is Laboratory Technician at 23.9%. Both roles share a common characteristic — they are the lowest paid roles in their respective departments, suggesting a compensation issue rather than a culture issue.

**Finding 3 — Employees earning under $3,000/month leave at 3× the rate of those earning above $6,000**

Employees in the lowest salary band (under $3K/month) have a 26.5% attrition rate. This drops to 16.2% in the $3K–$6K band and falls to 8.9% for those earning above $6K. The relationship between salary and retention is linear and clear — this is the strongest single predictor of attrition in the dataset.

**Finding 4 — Overtime is a critical attrition driver: 30.5% vs 10.4%**

Employees required to work overtime leave at a 30.5% rate — nearly three times the 10.4% rate of employees who do not work overtime. Overtime is disproportionately assigned in Sales and HR, which directly explains Finding 1. Overwork and low pay are compounding each other in those departments.

**Finding 5 — The highest attrition risk is at Year 1 of tenure (34.5% attrition)**

Employees in their first year are the most likely to leave — 34.5% attrition. This drops sharply after Year 2 and stabilises around 10% from Year 5 onwards. This pattern strongly suggests an onboarding or expectation-setting problem rather than a long-term engagement problem. Employees are leaving before the company has had a chance to retain them.

**Finding 6 — Single employees leave at nearly double the rate of married employees**

Single employees have a 25.5% attrition rate compared to 12.5% for married employees. While marital status itself is not something a business can influence, it is a useful risk signal — single employees in low-salary, high-overtime roles represent the highest-risk segment in this dataset.

---

## Recommendations

> One actionable recommendation per finding. Each one is specific enough to bring to a management meeting.

**Rec 1 — Focus retention budget on Sales and HR first, not the whole company**
A blanket retention programme wastes money on departments that do not have a problem. R&D at 13.8% attrition is closer to industry average. Concentrate intervention resources on Sales and HR where the problem is acute.

**Rec 2 — Audit Sales Representative compensation immediately**
A 39.8% attrition rate in a single role is a business emergency — the company is essentially cycling through its entire Sales Rep headcount every 2–3 years. Benchmark Sales Rep salaries against market rates. Even a modest salary increase for the bottom quartile is likely cheaper than continuous recruitment and training costs.

**Rec 3 — Model the cost of attrition to make the salary case to leadership**
The business case for a salary increase is stronger when it is expressed in money, not percentages. If average recruitment + training cost per Sales Rep is $15,000, and the company loses 40 Sales Reps per year, that is $600,000 annually. A 10% salary increase for 100 Sales Reps at $2,500/month costs $300,000. The numbers speak for themselves.

**Rec 4 — Investigate overtime assignment practices in Sales and HR**
Before implementing an overtime policy, understand why it is happening. Is it under-resourcing? Inefficient processes? Unrealistic targets? The data shows overtime is the problem — but fixing it requires understanding the cause, not just mandating fewer hours.

**Rec 5 — Redesign the onboarding programme for new hires**
A 34.5% Year 1 attrition rate means the company is losing a third of all new employees before they become productive. Conduct exit interviews specifically with employees who leave within 12 months. The most common answers will point directly to what needs to change — whether that is role clarity, manager quality, salary expectation mismatch, or culture fit.

**Rec 6 — Use the high-risk profile for proactive HR check-ins**
The highest-risk employee in this dataset is: single, under 30, in Sales or HR, earning under $3K/month, working overtime, in their first year. HR should identify current employees matching this profile and schedule proactive retention conversations — not wait for them to resign.

---

## Limitations

**1. No time dimension**
This is a single snapshot — there is no way to know if attrition improved or worsened over time. A real analysis would need at least 2–3 years of monthly data to identify trends and measure whether interventions worked.

**2. No exit interview data**
The dataset tells us *who* left and *what their profile was* — but not *why they said they left* in their own words. Salary and overtime are strong statistical correlates, but they may not be the reasons employees would cite themselves. Real retention strategy needs both quantitative and qualitative data.

**3. Dataset is fictional**
The IBM dataset is synthetically generated, which means patterns are cleaner and more linear than real-world HR data usually is. In a real dataset, findings would have more noise and require more careful interpretation.

---

## What I Would Analyse Next

If I had access to additional data, the next questions I would ask are:

- **Cost of attrition by role** — calculate the true financial cost (recruitment + training + lost productivity) per role to build a business case for retention investment
- **Manager-level attrition analysis** — do certain managers have consistently higher attrition on their teams? This is often the most actionable finding in real HR data
- **Attrition prediction model** — using logistic regression in Python to score current employees by attrition probability, enabling truly proactive HR intervention

---

## Skills Demonstrated

`Power BI` · `DAX` · `Power Query` · `Data Modelling` · `KPI Design` · `Business Storytelling` · `HR Domain Knowledge` · `Drill-Through Reports` · `Calculated Columns` · `Data Cleaning`

---

## Dashboard Preview

*(Add 2–3 screenshot images of your Power BI report here)*

![Executive Summary Page](#)
![Risk Factors Page](#)
![Drill-Through Detail Page](#)

---

## Contact

**Your Name** · [LinkedIn](#) · [GitHub](#) · your.email@gmail.com
