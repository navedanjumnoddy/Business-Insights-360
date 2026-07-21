# Business Insights 360 — AtliQ Hardware
### Multi-Domain Analytics Dashboard | Power BI | DAX | SQL | Data Optimization

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-CC2927?style=for-the-badge&logo=mysql&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 🔗 Live Dashboard

**[▶ View Live Report on Power BI Service](https://app.powerbi.com/view?r=eyJrIjoiNzkxNDNjOWItYzMyMC00NTlhLTg5ZDYtMTkwNTIzNzliY2IzIiwidCI6Ijg3NTIyNjVhLTMwYTctNDk0MS05YmFhLTQ3NmE3MzE4MGZlMSJ9)**

> ℹ️ **Project Note:** This is a **personal portfolio project** built while completing the Codebasics Power BI course, using their fictional AtliQ Hardware case study. It is not a client engagement, and no real company, dataset, or proprietary information is involved. The goal was to practice enterprise-style, multi-department BI development end-to-end — from data modeling through DAX to performance tuning.

---

## 📌 Project Overview

**Scenario (Course Case Study):** AtliQ Hardware is a fictional global electronics manufacturer, used by Codebasics as a teaching scenario to simulate a company operating through retail, distribution, and e-commerce channels, where Sales, Finance, Marketing, Supply Chain, and Executive teams historically relied on fragmented Excel-based reporting.

**What I Built:** A unified Power BI dashboard spanning **6 departments** (Sales, Finance, Supply Chain, Executive, Marketing, Products), modeled on data integrated from **2 source types** (MySQL-style database + Excel/CSV files) with **1M+ records**, designed to give each department real-time visibility into sales trends, profitability, forecast accuracy, and market performance.

**Project Highlights:**
- 📈 Practiced translating scattered departmental reporting into a single connected data model
- 🧮 Built a full P&L waterfall (Gross Sales → Net Profit) to practice financial reporting logic in DAX
- 🔍 Applied DAX Studio to review query plans and tidy up the model (column cardinality, data types, redundant columns) as a performance-tuning exercise
- 🎯 Designed department-specific views so each audience (Sales, Finance, Supply Chain, Marketing, Executive) sees the metrics relevant to them

---

## 🎯 Business Questions Answered by Each Department

| Department | Key Questions Answered | Why It Matters |
|------------|------------------------|--------|
| **Sales** | Which customers/regions drive profitability? How do actual sales compare to targets? | Practice identifying top performers and at-risk accounts |
| **Finance** | What is the full P&L flow from gross sales to net profit? Where are margin leaks? | Practice tracking profitability across products, customers, and time periods |
| **Supply Chain** | How accurate are forecasts? Which categories have inventory risk? | Practice reducing stockout/excess-inventory style risk analysis |
| **Marketing** | Which product categories generate highest margins? How do segments compare? | Practice optimizing product mix analysis |
| **Executive** | What is company-wide market share? Which divisions/products are strategic priorities? | Practice building C-suite-style visibility into business health |
| **Products** | How does each product perform on profitability and growth vs. targets? | Practice identifying underperformers for improvement or discontinuation |

---

## 📊 Dashboard Views & Functionality

| Page | Audience | Key Metrics | Use Case |
|------|----------|------------|----------|
| **Home** | All Users | Data freshness timestamps, navigation | Entry point with last-refresh visibility |
| **Finance View** | Finance, CFO | P&L pivot, Net Sales, GM%, Period trends | Full profitability analysis with drill-down capability |
| **Sales View** | Sales Leadership | Customer performance, GM% vs Revenue scatter, segment analysis | Identify top/bottom customers and profitability drivers |
| **Marketing View** | Marketing Team | Product performance, GM% waterfall, market scatter | Optimize product portfolio and category focus |
| **Supply Chain View** | Operations, SCM | Forecast Accuracy %, Net Error, Risk profiles | Monitor forecast reliability and inventory health |
| **Executive View** | C-Suite | Market share %, top 5 customers/products, division trends | High-level KPIs for strategic decision-making |
| **Product Level View** | Product Managers | Product-level P&L, YoY growth, GM% trends | Detailed performance view for individual SKUs |
| **Sales Trend (Tooltip)** | Context Users | NS$ and GM% inline trend (400×300px) | Quick performance reference without leaving current page |

---

## 💰 P&L Waterfall Logic — Full Financial Transparency

The dashboard implements a complete profit-to-loss waterfall, enabling stakeholders to understand where margin is created and lost:

```
Gross Sales (List Price × Quantity)
  ↓ minus Pre-Invoice Deductions
Net Invoice Sales
  ↓ minus Post-Invoice Deductions (Trade Discounts, Promotions)
= Net Sales (NS$) — Revenue available for operations
  ↓ minus Cost of Goods Sold (COGS)
= Gross Margin (GM$) — Production profit before operations
  ÷ Net Sales
= Gross Margin % (GM%) — Profitability efficiency metric
  ↓ minus Operational Expenses (Marketing, Distribution, Admin)
= Net Profit — Bottom-line profitability
```

**Why This Matters:** Each step can be drilled by customer, product, region, or time period, which is a useful pattern for pinpointing where profit leaks occur in any real P&L.

---

## 🧮 Key DAX Measures & Business Logic

| Measure | Formula | Business Use |
|---------|---------|--------------|
| **Net Sales (NS$)** | SUM(fact_sales[net_sales_amount]) | Revenue after all deductions; core KPI |
| **Gross Margin %** | DIVIDE([GM $], [NS $]) | Profitability efficiency; target tracking |
| **Forecast Accuracy %** | 1 - [ABS Error %] | Supply chain reliability metric |
| **Net Error** | [Forecast NS $] - [Actual NS $] | Bias in forecast (positive = overforecast) |
| **AtliQ Market Share %** | DIVIDE([NS $], [Total Market Sales $]) | Competitive position tracking |
| **GM% Growth YoY** | DIVIDE([GM% Current] - [GM% LY], ABS([GM% LY])) | Profitability trend vs. prior year |
| **Forecast Risk Profile** | IF([ABS Error %] > threshold, "High Risk", "Healthy") | Supply chain risk flagging |

---

## ⚙️ Technical Implementation Highlights

### Data Integration (2 Source Types → 1M+ Records)
- **MySQL-style Database:** Sales transactions, forecasts, customer master, product master
- **Excel/CSV Files:** Operational costs, targets, market share benchmarks, reference data
- **Power Query (M):** Extracted, cleaned, and transformed data from both sources with consistency checks

### Data Model (Star Schema)
```
Dimension Tables:
  • dim_product (product details, hierarchy)
  • dim_customer (customer master, segments)
  • dim_market (geography, territories)
  • dim_date (calendar, fiscal periods)

Fact Tables:
  • fact_sales_monthly (actual sales, quantities, pricing)
  • fact_forecast_monthly (forecast sales, forecast quantities)
  • Supporting tables: freight_cost, manufacturing_cost, discounts, targets
```

### Performance Optimization Practice (DAX Studio)
- Reviewed query execution plans in DAX Studio and removed redundant columns, optimized data types, and reordered column cardinality on the fact tables
- Purpose was to practice the workflow of profiling a model and tightening it up — a habit worth carrying into any real production model, where the actual gains would depend on the dataset and hardware

### Interactivity Features
- **Bookmark Toggles:** Marketing/Product views switch between different metric comparisons
- **Info Panel Controls:** Hide/Show bookmarks for clean interface
- **Period Benchmark Selector:** Compare current period vs Last Year or vs Forecast Target
- **Scatter Chart Quadrants:** Identify high-margin/high-volume vs at-risk products/customers
- **Inline Tooltips:** 400×300px Sales Trend chart appears without navigation

---

## 📸 Screenshots & Visual Guides

> **How to add:** In Power BI Desktop → File → Export → Export to PNG per page. Save to `Screenshots/` folder with names below. GIFs of bookmark toggles and the period selector can also be dropped into the same folder.

| Page | Filename | Key Visual |
|------|---------|-----------|
| Home | `01_home.png` | *[screenshot / GIF placeholder — add here]* |
| Finance View | `02_finance.png` | *[screenshot / GIF placeholder — add here]* |
| Sales View | `03_sales.png` | *[screenshot / GIF placeholder — add here]* |
| Marketing View | `04_marketing.png` | *[screenshot / GIF placeholder — add here]* |
| Supply Chain View | `05_supply_chain.png` | *[screenshot / GIF placeholder — add here]* |
| Executive View | `06_executive.png` | *[screenshot / GIF placeholder — add here]* |
| Product Level View | `07_product_level.png` | *[screenshot / GIF placeholder — add here]* |

> 💡 **Tip:** Record a 30-second GIF showing the bookmark toggles and period selector in action — this shows off Power BI interactivity far better than static images.

---

## 🛠️ Tools & Technologies Used

| Tool | Purpose | Key Feature |
|------|---------|------------|
| **Power BI Desktop** | Report authoring & data modeling | 6-department dashboard, custom visuals, bookmarks |
| **Power Query (M)** | ETL & data transformation | 2-source integration, 1M+ record processing |
| **DAX** | Advanced calculations & KPIs | Financial measures, forecast logic, period comparisons |
| **SQL** | Data extraction & validation | MySQL-style queries for sales/forecast/master data |
| **DAX Studio** | Performance optimization practice | Query plan review, model tidy-up |
| **Excel** | Reference & master data | Targets, operational costs, market benchmarks |

---

## 💡 What This Project Demonstrates

1. **End-to-End Analytics Workflow** — Requirements → data integration → modeling → DAX development → optimization → deployment, across 6 simulated departments
2. **Multi-Source Data Integration** — Practiced ETL orchestration across 2 different data source types
3. **Scale Awareness** — Modeled 1M+ records in a star schema with performance in mind
4. **Optimization Habits** — Used DAX Studio to profile and tidy the model rather than leaving it as-is
5. **Business Acumen** — Translated finance, supply chain, and sales concepts into working DAX measures
6. **Multi-Audience Design** — Built distinct pages for different functional audiences (Sales, Finance, Marketing, Supply Chain, Executive)

### Practice Interview Questions

**Q: "Walk me through the P&L waterfall logic. Why structure it this way?"**
*A: The waterfall breaks down profit generation step-by-step so it's clear where margin is created (Gross Margin) and where it's consumed (Operations). This enables drill-down by customer/product/region to find where profitability leaks would occur in a real P&L.*

**Q: "Explain the Forecast Accuracy measure. Why ABS Error?"**
*A: Forecast Accuracy = 1 - ABS(Error%), which penalizes both overforecasting and underforecasting equally. If forecast is $100 but actual is $90, the error is 10% whether you overforecast or underforecast — both are problems for supply chain planning.*

**Q: "How did you approach performance optimization with DAX Studio?"**
*A: I reviewed query execution plans, removed redundant columns, checked data types on the fact tables, and reordered column cardinality to improve compression — a habit I'd apply to any model, scaled to whatever the actual dataset and refresh cadence demand in a real production setting.*

**Q: "What would you do differently if building this for a real enterprise?"**
*A: I'd implement row-level security (RLS) by region/market, add data quality monitoring for source systems, implement incremental refresh for large fact tables, and add version control for PBIX changes using Power BI Projects.*

---

## 📁 Repository Structure

```
business-insights-360/
│
├── README.md (this file)
├── Live Dashboard Link (https://app.powerbi.com/...)
├── DAX/
│   └── measures.dax                    ← Key DAX measures with business logic
├── DataModel/
│   └── schema.md                       ← Table structure, relationships, design rationale
├── Screenshots/
│   ├── 01_home.png
│   ├── 02_finance.png
│   ├── 03_sales.png
│   ├── 04_marketing.png
│   ├── 05_supply_chain.png
│   ├── 06_executive.png
│   └── 07_product_level.png
└── Theme/
    └── AtliQ_Professional_Theme.json   ← Import-ready Power BI theme
```

---

## 👤 Author & Contact

**Naved Anjum** — Senior MIS Reporting Manager | Power BI Developer

📧 **Email:** navedanjum1989@gmail.com
🔗 **LinkedIn:** [linkedin.com/in/navedanjum1989](https://linkedin.com/in/navedanjum1989)
🔗 **GitHub:** [github.com/navedanjumnoddy](https://github.com/navedanjumnoddy)
💼 **Portfolio:** [GitHub Projects](https://github.com/navedanjumnoddy)

---

## 📚 References & Acknowledgments

- **Course Foundation:** Codebasics Power BI Course (this project follows their AtliQ Hardware "Business Insights 360" case study, built independently as a personal learning project)
- **Data Source:** Fictional AtliQ Hardware scenario — no real company data involved
- **Architecture:** Star schema design following Kimball methodology

---

**Last Updated:** July 2026
**Status:** Complete | Personal Portfolio Project
