# 📊 Enterprise Financial Performance Dashboard  
### CFO-Level Business Intelligence | Power BI | DAX | FP&A Analytics

---

## ❓ Business Questions This Dashboard Answers (CFO First)

This project is designed around **real questions CFOs and FP&A teams ask every month**, not around charts or visuals.

### Profitability & Efficiency
- Are we growing **profitably**, or just growing revenue?
- How much does it cost us to earn $1 of revenue?
- Which departments consume the highest share of operating costs?

### Growth & Momentum
- Is revenue accelerating or slowing down month-over-month?
- Are expenses growing faster than revenue (cost inflation risk)?

### Cash & Sustainability
- What is our average monthly burn rate?
- How predictable and stable are our profits over time?

### Risk & Concentration
- Are we overly dependent on a single revenue stream?
- How volatile is our profit performance?

### Capital Allocation
- Are we spreading investments too thin across projects?
- Which initiatives deliver the best financial outcomes?

> **CFO principle:**  
> *Dashboards should answer questions first — visuals come second.*

---

## 🎯 Business Objective

The objective of this project is to simulate a **CFO-grade enterprise financial intelligence system** built on transaction-level data.

The dashboard enables finance leaders to:
- Monitor profitability, cost efficiency, and financial risk
- Track revenue and expense momentum
- Support board-level and investor reporting
- Make informed capital allocation decisions

Unlike static P&L statements, this solution enables **dynamic drill-down**, trend analysis, and KPI-based decision-making.

---

## 🗂️ Dataset Overview

**Source:**  
Power BI Financial Dataset by SlideScope  
https://github.com/slidescope/Power-BI-Financial-Dataset-by-SlideScope-for-Data-Science-Projects

### Why This Dataset Is CFO-Grade
- Transaction-level granularity (300 rows)
- Each row = one financial event
- Signed amount logic (Revenue = positive, Expense = negative)
- Mirrors real ERP and accounting system extracts

This structure closely resembles data pulled from:
- SAP, Oracle, NetSuite, Tally
- QuickBooks, Zoho Books
- Enterprise finance data warehouses

---

## 🔍 Why Transaction-Level Data Matters

Transaction-level data allows:
- Drill-down from company KPIs → department → project → transaction
- Accurate cash flow timing
- Audit-friendly and explainable reporting
- ROI analysis at initiative level

> CFOs prefer granular data because **summaries hide inefficiencies**.

---

## 🧾 Core Dimensions & Business Meaning

| Dimension | Business Use |
|--------|-------------|
| Date | Time intelligence (MoM, YoY, rolling metrics) |
| Department | Cost accountability & ownership |
| Cost Center | Budget control & compliance |
| Revenue Stream | Revenue mix & dependency analysis |
| Project | ROI & capital allocation |
| Region | Geographic performance |
| Payment Mode | Liquidity & treasury planning |

---

## 🛠️ Tech Stack

- **Power BI** – Enterprise dashboards
- **DAX** – Financial KPI logic
- **Star schema modeling**
- **Custom Date Table** for reliable time intelligence

---

## 📈 Key CFO & FP&A KPIs with DAX

### 1️⃣ Gross Margin %
**Business Question:**  
Are we earning efficiently?

```DAX
Gross Margin % =
DIVIDE(
    [Net Profit],
    [Total Revenue],
    BLANK()
)


2️⃣ Operating Cost Ratio

Business Question: 
How much does it cost to earn $1 of revenue?
Operating Cost Ratio % =
DIVIDE(
    [Total Expense],
    [Total Revenue],
    BLANK()
)

3️⃣ Revenue Growth % (Month-over-Month)

Business Question:
Is revenue accelerating or slowing?
Revenue MoM % =
VAR CurrentMonth =
    MAX ( 'Financial_Transactions'[Date] )
VAR PrevMonthRevenue =
    CALCULATE (
        [Total Revenue],
        FILTER (
            ALL ( 'Financial_Transactions'[Date] ),
            'Financial_Transactions'[Date]
                >= EOMONTH ( CurrentMonth, -2 ) + 1
                && 'Financial_Transactions'[Date]
                <= EOMONTH ( CurrentMonth, -1 )
        )
    )
RETURN
DIVIDE ( [Total Revenue] - PrevMonthRevenue, PrevMonthRevenue )

4️⃣ Expense Growth % (Cost Inflation Indicator)

Business Question:
Are expenses rising faster than revenue?
Expense MoM % =
VAR CurrentMonth =
    MAX ( 'Financial_Transactions'[Date] )
VAR PrevMonthExpense =
    CALCULATE (
        [Total Expense],
        FILTER (
            ALL ( 'Financial_Transactions'[Date] ),
            'Financial_Transactions'[Date]
                >= EOMONTH ( CurrentMonth, -2 ) + 1
                && 'Financial_Transactions'[Date]
                <= EOMONTH ( CurrentMonth, -1 )
        )
    )
RETURN
DIVIDE ( [Total Expense] - PrevMonthExpense, PrevMonthExpense )

5️⃣ EBITDA (Executive View)

Business Question:
How do investors evaluate our operating performance?
EBITDA =
[Net Profit]

6️⃣ Department Cost Contribution %

Business Question:
Which departments consume the most company resources?
Department Cost % =
DIVIDE(
    [Total Expense],
    CALCULATE(
        [Total Expense],
        ALL('Financial_Transactions'[Department])
    ),
    BLANK()
)

7️⃣ Revenue Concentration %

Business Question:
Are we overly dependent on one revenue stream?
Revenue Concentration % =
DIVIDE(
    [Total Revenue],
    CALCULATE(
        [Total Revenue],
        ALL('Financial_Transactions'[Revenue_Stream])
    ),
    BLANK()
)

8️⃣ Average Monthly Burn Rate

Business Question:
How fast are we consuming cash?
Average Burn Rate =
AVERAGEX(
    VALUES('Financial_Transactions'[Month]),
    [Total Expense]
)

9️⃣ Cost per Project

Business Question:
Are we spreading capital too thin across initiatives?
Cost per Project =
DIVIDE(
    [Total Expense],
    DISTINCTCOUNT('Financial_Transactions'[Project]),
    BLANK()
)
🔟 Profit Volatility (Risk Indicator)

Business Question:
How predictable and stable are our profits?
Profit Volatility =
STDEVX.P(
    VALUES('Financial_Transactions'[Month]),
    [Net Profit]
)

