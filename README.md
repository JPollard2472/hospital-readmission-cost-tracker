# Hospital Readmission Cost Tracker
**An automated Python + SQL pipeline that turns public Medicare data into an estimate of avoidable readmission costs, and shows where hospitals should focus to reduce them.**

## Key findings
Using CMS data on 2,833 U.S. hospitals (performance period July 2021 – June 2024):
- **$186M in estimated avoidable readmission costs** over the 3-year window, about $62M a year
- **12,245 readmissions** above what CMS expects, given each hospital's patient mix
- **Half the cost sits in 219 hospitals**, under 8% of those measured
- **Heart failure and pneumonia drive 77%** of the excess cost ($142.6M)
- **1-star hospitals average 2.5x the excess cost** of 5-star hospitals ($107K vs. $43K)
- **Closing half the gap** would avoid about 6,100 readmissions and $93M

## The business problem

When a Medicare patient is readmitted within 30 days, the hospital absorbs extra care costs. Under the Hospital Readmissions Reduction Program (HRRP), CMS can also cut up to 3% of a hospital's Medicare base payments. This project answers three questions: which hospitals and conditions drive excess readmissions, what that costs, and where improvement efforts should go first.

## How it works

1. **Extract (Python):** pulls 18,330 readmission records and 5,419 hospital records from the CMS Provider Data API, with paging and automatic retries
2. **Transform (SQL):** 7 SQL models clean the raw data, calculate excess readmissions and cost, and rank hospitals and states using CTEs and window functions
3. **Validate:** automated data-quality checks for duplicates, hospital match rate, and totals that reconcile across tables
4. **Deliver:** Tableau-ready files, an Excel model with what-if inputs, and an executive presentation

## What's in this repository

| File | What it is |
|---|---|
| Pipeline notebook (.ipynb) | The full Python + SQL pipeline, runnable in Google Colab |
| Hospital_Readmission_Cost_Tracker.pptx | 9-slide executive presentation with recommendations |
| Hospital_Readmission_Analysis.xlsx | Excel model: dashboard, what-if inputs, savings scenarios |
| CSV files | Pipeline outputs used for the Tableau dashboard |

## Method and assumptions

- **Excess readmissions** = discharges × (predicted rate − expected rate), counted only where CMS's excess readmission ratio is above 1.0
- **Estimated cost** = excess readmissions × $15,200, the average readmission cost reported by AHRQ HCUP (Statistical Brief #248, 2018 data)

**Limitations:** the cost figure sizes avoidable spending; it is not each hospital's actual HRRP penalty, which CMS calculates separately. The $15,200 figure is from 2018, so current costs are likely higher. CMS's ratio compares each hospital with a national average, so about half of hospitals land above 1.0 on any condition by design.

## Data sources

- CMS Hospital Readmissions Reduction Program (dataset 9n3s-kdb3)
- CMS Hospital General Information (dataset xubh-q36u)

## Tools

Python (pandas, requests) · SQL (SQLite) · Google Colab · Excel · Tableau · PowerPoint

---
*Built by Jadon Pollard · September 2026*
