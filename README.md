# HUMAN RESOURCE ANALYSIS
# Workforce Retention, Performance & Turnover Analysis

A data-driven analysis of a 311 employee HR dataset (2006–2018), built to answer the questions HR management couldn't answer with raw data alone: *why do people leave, which departments are highest-risk, and which levers actually move retention?*

The project spans data cleaning, feature engineering, exploratory analysis, and an interactive Power BI dashboard, backed by a full report and presentation deck built to defend the findings to stakeholders.

---

## Table of Contents

- [Overview](#overview)
- [Business Problem](#business-problem)
- [Aims & Objectives](#aims--objectives)
- [Tech Stack](#tech-stack)
- [Repository Structure](#repository-structure)
- [Dataset](#dataset)
- [Data Cleaning](#data-cleaning)
- [Feature Engineering](#feature-engineering)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Dashboard](#dashboard)
- [Recommendations](#recommendations)
- [How to Explore This Project](#how-to-explore-this-project)
- [Limitations](#limitations)
- [License & Data Attribution](#license--data-attribution)

---

## Overview

| | |
|---|---|
| **Dataset** | 311 employees, 2006–2018 |
| **Tools** | Microsoft Power BI, DAX, Power Query (M) |
| **Deliverables** | Word report, PowerPoint deck, Power BI dashboard, cleaned dataset |
| **Headline metrics** | 33.4% attrition · 66.6% retention · 4.11/5 avg. engagement |

**Three findings drive the whole analysis:**
1. **Tenure is the strongest attrition signal** — 90% attrition among employees with under 1 year of service, falling to 0% at 10+ years.
2. **Turnover is concentrated, not evenly spread** — Production (67% of headcount) carries the highest departmental turnover at 39.7%.
3. **Recruitment source predicts retention, not performance** — Website hires retain at 92.3% vs. 38.8% for Google Search hires, despite near-identical performance scores across all channels.

---

## Business Problem

HR management collects extensive employee data, demographics, roles, salaries, performance, recruitment source, tenure but lacked a structured analysis of what that data actually says about workforce stability. Specifically, the organization could not confidently answer:

- Why employees leave
- Which departments experience the highest turnover
- Whether compensation and engagement are competitive
- How recruitment sources affect employee quality and retention
- Which workforce trends need immediate managerial attention

This project closes that gap with quantified, reproducible evidence.

---

## Aims & Objectives

**Aim:** Analyze the HR dataset to evaluate workforce trends, performance, and retention patterns, and generate actionable insight for talent management and workforce planning.

**Objectives:**
1. Clean and prepare employee records for consistent analysis
2. Analyze workforce demographics (age, gender, department, position)
3. Evaluate retention and turnover patterns
4. Assess performance across departments, positions, and managers
5. Examine salary distribution and compensation equity
6. Determine recruitment source effectiveness
7. Explore the relationship between engagement/satisfaction and retention
8. Generate strategic, actionable recommendations

---

## Tech Stack

- **Power BI Desktop** — data modeling, DAX measures, interactive dashboard
- **Power Query (M)** — data cleaning and transformation
- **DAX** — KPI measures, calculated columns, tenure and attrition logic

---

## Repository Structure

```
hr-workforce-analytics/
│
├── data/
│   └── HRDataset_v14_cleaned.csv        # cleaned dataset (26 cols, 311 rows)
│
├── dashboard/
│   └── HR_DASHBOARD.pbix                # Power BI report (2 pages, DAX measures)
│
├── reports/
│   ├── HR_Analytics_Report.docx         # full written report (15 pages)
│   └── HR_Analytics_Presentation.pptx   # 16-slide defense deck
│
├── charts/
│   └── *.png                            # exported dashboard screenshots
│
└── README.md
```

---

## Dataset

Source: [Human Resources Data Set](https://www.kaggle.com/datasets/rhuebner/human-resources-data-set) (Kaggle, rhuebner) a widely used HR analytics dataset representing a fictitious mid-sized organization.

| Attribute | Detail |
|---|---|
| Original size | 311 rows × 36 columns |
| Cleaned size | 311 rows × 30 columns |
| Grain | One row per employee |
| Time span | Hire dates 2006–2018; terminations through late 2018 |
| Active employees | 207 (66.6%) |
| Terminated employees | 104 (33.4%) |
| Departments | Production, IT/IS, Sales, Software Engineering, Admin Offices, Executive Office |

---

## Data Cleaning

Performed in Power Query. Reduced the dataset from 36 → 26 columns.

- **Removed redundant ID columns** (MaritalStatusID, GenderID, EmpStatusID, DeptID, PerfScoreID duplicate, FromDiversityJobFairID duplicate, HispanicLatino) where a cleaner field already existed
- **Standardized text fields** — trimmed duplicated/whitespace-inconsistent `Position` values (e.g. "Data Analyst" appearing twice due to trailing spaces)
- **Fixed DOB parsing errors** — two-digit years (e.g. `07/10/83`) were misread after date-type conversion; split and reconstructed the century (`07/10/1983`)
- **Preserved legitimate blanks** — 207 blank `DateofTermination` values represent active employees, not missing data
- **Back-filled a missing Manager ID** — 8 rows under manager "Webster Butler" had a blank `ManagerID`; recovered from other rows referencing the same manager name
- **Cleaned anomalous `TermReason` entries** — trimmed whitespace, standardized categories, removed placeholder/joke text values
- **De-duplicated records** and validated row counts

---

## Feature Engineering

### Calculated Columns
| Feature | Logic |
|---|---|
| `Termd` | 1 if `DateofTermination` populated, else 0 |
| `Employee Age` | Derived from `DOB` |
| `Employee Age (groups)` | Banded age ranges |
| `Years Worked` | Tenure in years (see fix below) |
| `Years Worked (groups)` | Tenure bands: 0–1, 1–3, 3–5, 5–10, 10+ years |

### DAX Measures
| Measure | Purpose |
|---|---|
| `Attrition Rate` | `DIVIDE(terminated, total, 0)` |
| `Retention Rate` | `DIVIDE(active, total, 0)` |
| `Average Employee Tenure` | Average `Years Worked`, filtered to active employees |
| `Average Salary` / `Salary Gap` | Compensation monitoring by gender |
| `Average Performance` | Average `PerfScoreID` |

---

## Methodology

1. **Data acquisition** — sourced from Kaggle (rhuebner)
2. **Data cleaning** — Power Query, 36 → 30 columns
3. **Feature engineering** — calculated columns and DAX measures built directly in the Power BI data model
4. **Exploratory analysis** — cross-tabulated turnover, performance, salary, and satisfaction across department, recruitment source, manager, tenure band, and year using Power BI visuals
5. **Dashboard construction** — 2-page interactive Power BI report, slicers on Department / Gender / Recruitment Source
6. **Insight synthesis** — mapped each finding to a business implication and recommendation

---

## Key Findings

| # | Question | Finding |
|---|---|---|
| 1 | Which departments have the highest turnover? | Production: 39.7% (n=209); Software Engineering: 36.4% (n=11, small sample) |
| 2 | Why do people leave? | ~40 of 104 exits are voluntary/career-driven (another position, more money, career change) |
| 3 | Which recruitment sources retain best? | Website 92.3% retention vs. Google Search 38.8%, at similar performance levels |
| 4 | Does performance vary by department? | Narrow spread (2.84–3.09 / 4); Sales is the lowest, corroborated by highest absenteeism |
| 5 | Are there salary gaps? | 4.0% gender gap overall (5.5% ex-Executive Office), varies by department |
| 6 | Highest absenteeism/conduct issues? | Sales: 11.6 avg. absence days/year, highest lateness |
| 7 | Does satisfaction predict retention? | No — correlation with attrition ≈ −0.005 (statistically negligible) |
| 8/10 | Does tenure predict attrition? | Yes — 90% (< 1yr) → 65.4% (1–3yr) → 32.4% (3–5yr) → 19.4% (5–10yr) → 0% (10+yr) |
| 9 | Manager effectiveness? | Jennifer Zamora: 3.14 perf / 14.3% turnover (best); Webster Butler & Amy Dunn: 61.9% turnover despite average performance |
| 11 | Priority trends? | Onboarding, Production retention, sourcing mix, survey redesign, pay audit |

Full detail, business implications, and defensible caveats for each finding are in [`reports/HR_Analytics_Report.docx`](reports/HR_Analytics_Report.docx). 

---

## Dashboard

A 2-page interactive Power BI report (`HR_DASHBOARD.pbix`):

- **Page 1 — HR Analysis:** turnover by department, termination drivers, recruitment source quality, performance, absenteeism
- **Page 2 — Workforce Performance:** tenure-based attrition risk, manager effectiveness, salary trends, satisfaction/engagement vs. retention

Cross-filterable by Department, Gender, and Recruitment Source slicers.

![HR ANALYSIS]
<img width="528" height="357" alt="HR Dashboard 1" src="https://github.com/user-attachments/assets/52c83c94-306e-48fc-b84a-a021a301e7dc" />

WORKFORCE PERFORMANCE
<img width="527" height="356" alt="HR Dashboard 2" src="https://github.com/user-attachments/assets/18573b3c-7d10-4095-85cf-31f2ffc2ce5e" />

---

## Recommendations

Ordered by impact vs. effort:

1. **Fix the first year first** — structured onboarding, 30/60/90-day check-ins, assigned mentors
2. **Launch a Production retention plan** — largest headcount + highest turnover
3. **Rebalance recruiting spend** toward Employee Referral / Website; audit Google Search & Diversity Job Fair funnels
4. **Coach high-turnover managers** — pair with mentors from low-turnover, high-performance managers
5. **Commission a controlled pay-equity audit** — control for role, tenure, performance
6. **Redesign the engagement/satisfaction survey** — current scores don't predict attrition
7. **Review Sales workload & incentive structure**
8. **Operationalize the dashboard** — assign ownership of KPI cards for ongoing monitoring

---

## How to Explore This Project

1. Download and install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
2. Clone or download this repository
3. Open `dashboard/HR_DASHBOARD.pbix` in Power BI Desktop
4. Use the slicers (Department, Gender, Recruitment Source) to filter the report interactively
5. Refer to `reports/HR_Analytics_Report.docx` for the full write-up, or `reports/HR_Analytics_Presentation.pptx` for the summary deck

```bash
git clone https://github.com/chinonyeonwuka2003-collab/hr-workforce-analytics.git
cd hr-workforce-analytics
```

---

## Limitations

- Dataset is a well-known public/fictitious HR dataset — findings are illustrative of technique, not necessarily generalizable to other organizations
- Small-sample departments (Software Engineering n=11, Executive Office n=1) and recruitment sources (n<5) should be read as directional, not statistically robust
- Salary gap analysis is descriptive only — no causal claim is made; a controlled audit (Recommendation 5) would be needed before drawing conclusions about pay equity
- Tenure calculations use a fixed anchor date (28 Feb 2019, the latest verified activity in the dataset) rather than the current date, to keep results stable and reproducible regardless of when the project is viewed

---

## License & Data Attribution

- **Dataset:** [Human Resources Data Set](https://www.kaggle.com/datasets/rhuebner/human-resources-data-set) by rhuebner, used under its original Kaggle license terms
- **Analysis, dashboard, and report:** © 2026 — feel free to fork and adapt for learning purposes with attribution

---

*Built entirely in Microsoft Power BI using Power Query and DAX. Full methodology, defended findings, and recommendations available in the accompanying report and presentation deck.*
