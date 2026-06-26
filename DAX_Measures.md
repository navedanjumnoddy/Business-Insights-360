# Business Insights 360 - DAX Measures Reference

Complete documentation of all DAX formulas used in the Business Insights 360 Power BI dashboard.

## Revenue & Sales Measures

### Total Revenue
```dax
Total Revenue = SUMX(Sales, Sales[Quantity] * Sales[Price])
```
- **Purpose:** Calculate total sales revenue
- **Use Cases:** Revenue trending, sales performance analysis
- **Dependencies:** Sales[Quantity], Sales[Price]

### Revenue YoY Growth
```dax
Revenue YoY Growth = 
CALCULATE([Total Revenue], 
  SAMEPERIODLASTYEAR(Date[Date])) - [Total Revenue]
```
- **Purpose:** Year-over-year revenue comparison
- **Use Cases:** Growth analysis, trend identification
- **Format:** Percentage or absolute value

### Revenue YoY Growth %
```dax
Revenue YoY Growth % = 
DIVIDE([Revenue YoY Growth], 
  CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date])), 0)
```
- **Purpose:** Percentage growth rate
- **Format:** Percentage formatting recommended

## Profit Measures

### Gross Profit
```dax
Gross Profit = [Total Revenue] - [Total COGS]
```
- **Purpose:** Calculate profit before operating expenses
- **Components:** Revenue minus Cost of Goods Sold

### Net Profit
```dax
Net Profit = [Gross Profit] - [Operating Expenses] - [Other Expenses]
```
- **Purpose:** Bottom-line profit calculation
- **Use Cases:** Profitability analysis, financial reporting

### Profit Margin %
```dax
Profit Margin % = DIVIDE([Net Profit], [Total Revenue], 0)
```
- **Purpose:** Profitability as percentage of revenue
- **Target:** Typically 15-25% depending on industry

## Cost Measures

### Total COGS
```dax
Total COGS = SUMX(Sales, Sales[Unit Cost] * Sales[Quantity])
```
- **Purpose:** Total cost of goods sold
- **Use Cases:** Margin analysis, product profitability

### Operating Expenses
```dax
Operating Expenses = 
SUMX(Expenses, Expenses[Amount])
```
- **Purpose:** Calculate operating costs
- **Includes:** Salaries, utilities, marketing, R&D

## Forecast & Accuracy Measures

### Total Forecast
```dax
Total Forecast = SUMX(Forecast, Forecast[Forecast Amount])
```
- **Purpose:** Sum of all forecast values
- **Dimension:** By product, region, or time period

### Forecast Variance
```dax
Forecast Variance = [Total Revenue] - [Total Forecast]
```
- **Purpose:** Actual vs. Forecast difference
- **Interpretation:** Positive = better than forecast

### Forecast Accuracy %
```dax
Forecast Accuracy % = 
DIVIDE(
  [Total Forecast], 
  [Total Revenue], 
  0
)
```
- **Purpose:** Forecast vs. Actual accuracy ratio
- **Target:** 90-95% accuracy threshold

## Market Share Measures

### Total Market Sales
```dax
Total Market Sales = 
CALCULATE([Total Revenue], 
  ALL(Customer[Customer ID]))
```
- **Purpose:** Entire market revenue
- **Use Cases:** Competitive positioning

### Market Share %
```dax
Market Share % = 
DIVIDE([Total Revenue], [Total Market Sales], 0)
```
- **Purpose:** Company's market share percentage
- **Visualization:** Trend analysis, competitive comparison

## Customer Measures

### Total Customers
```dax
Total Customers = DISTINCTCOUNT(Sales[Customer ID])
```
- **Purpose:** Count of unique customers
- **Use Cases:** Customer base analysis

### Avg Revenue per Customer
```dax
Avg Revenue per Customer = 
DIVIDE([Total Revenue], [Total Customers], 0)
```
- **Purpose:** ARPU metric
- **Use Cases:** Customer value analysis

### Customer Retention Rate
```dax
Customer Retention Rate = 
DIVIDE(
  CALCULATE([Total Customers], Date[Year] = MAX(Date[Year])),
  CALCULATE([Total Customers], Date[Year] = MAX(Date[Year]) - 1),
  0
)
```
- **Purpose:** Measure customer loyalty
- **Target:** >80% retention rate

## Product Measures

### Total Products Sold
```dax
Total Products Sold = SUM(Sales[Quantity])
```
- **Purpose:** Total unit sales volume
- **Use Cases:** Volume analysis, capacity planning

### Avg Price
```dax
Avg Price = DIVIDE([Total Revenue], [Total Products Sold], 0)
```
- **Purpose:** Average selling price per unit
- **Use Cases:** Price trend analysis

### Product Ranking
```dax
Product Ranking = 
RANKX(
  ALL(Product[Product Name]), 
  [Total Revenue],,,DESC
)
```
- **Purpose:** Rank products by revenue
- **Use Cases:** Top product identification

## Time Intelligence Measures

### MTD Revenue (Month-to-Date)
```dax
MTD Revenue = 
CALCULATE([Total Revenue], 
  DATESMTD(Date[Date]))
```
- **Purpose:** Revenue from start of month to today
- **Use Cases:** Monthly performance tracking

### QTD Revenue (Quarter-to-Date)
```dax
QTD Revenue = 
CALCULATE([Total Revenue], 
  DATESQTD(Date[Date]))
```
- **Purpose:** Revenue from start of quarter to today

### YTD Revenue (Year-to-Date)
```dax
YTD Revenue = 
CALCULATE([Total Revenue], 
  DATESYTD(Date[Date]))
```
- **Purpose:** Revenue from start of year to today
- **Common KPI:** Annual performance indicator

### Prior Period Revenue
```dax
Prior Period Revenue = 
CALCULATE([Total Revenue], 
  PREVIOUSMONTH(Date[Date]))
```
- **Purpose:** Previous period comparison
- **Use Cases:** Sequential trend analysis

## Comparative Measures

### Revenue vs Budget
```dax
Revenue vs Budget = [Total Revenue] - [Total Budget]
```
- **Purpose:** Actual vs. planned variance
- **Interpretation:** Positive = over-performance

### Revenue vs Budget %
```dax
Revenue vs Budget % = 
DIVIDE([Revenue vs Budget], [Total Budget], 0)
```
- **Purpose:** Budget variance percentage
- **Common Threshold:** ±10% acceptable range

## Advanced Measures

### Running Total (Cumulative)
```dax
Running Total = 
CALCULATE([Total Revenue],
  FILTER(
    ALL(Date[Date]),
    Date[Date] <= MAX(Date[Date])
  )
)
```
- **Purpose:** Cumulative sum over time
- **Use Cases:** Trend analysis, waterfall charts

### Moving Average (3-Month)
```dax
Moving Average 3M = 
CALCULATE([Total Revenue],
  DATESBETWEEN(
    Date[Date],
    TODAY()-90,
    TODAY()
  )
) / 3
```
- **Purpose:** Smoothed trend analysis
- **Parameter:** Adjust 90 days for different periods

### Variance from Target
```dax
Variance from Target = 
DIVIDE(
  [Total Revenue] - [Target Revenue],
  [Target Revenue],
  0
)
```
- **Purpose:** Performance vs. goal
- **Status:** Conditional formatting recommended

## Filter-Safe Measures

### Total Revenue (All)
```dax
Total Revenue All = 
CALCULATE([Total Revenue], 
  REMOVEFILTERS())
```
- **Purpose:** Revenue ignoring all filters
- **Use Cases:** Benchmark comparisons, percentage calculations

## Debugging Measures

### Row Count
```dax
Row Count = COUNTROWS(Sales)
```
- **Purpose:** Verify data row counts
- **Use Cases:** Data validation, audit

### Last Refresh Time
```dax
Last Refresh Time = MAX(Sales[Load Date])
```
- **Purpose:** Track data freshness
- **Visibility:** Display in footer or tooltip

---

## Best Practices Used

✅ **Implicit vs. Explicit Measures:** Most measures are explicit (dashboard-specific)  
✅ **CALCULATE Function:** Heavy use for filtering and context modification  
✅ **Time Intelligence:** SAMEPERIODLASTYEAR, DATESYTD for temporal analysis  
✅ **Error Handling:** DIVIDE with 0 parameter to avoid errors  
✅ **Performance:** Measures preferred over calculated columns  
✅ **Naming Convention:** Clear, descriptive measure names with context

## Common Modifications

### Change Time Period
Replace `SAMEPERIODLASTYEAR()` with:
- `PREVIOUSMONTH()` - Prior month comparison
- `PREVIOUSQUARTER()` - Prior quarter
- `DATEADD(Date[Date], -1, YEAR)` - Custom period

### Add Filters
Example: Filter by region
```dax
Regional Revenue = 
CALCULATE([Total Revenue],
  Sales[Region] = "North America")
```

### Combine Measures
```dax
Profit per Customer = 
DIVIDE([Total Net Profit], [Total Customers], 0)
```

---

**Last Updated:** June 2026  
**Total Measures:** 50+  
**Performance Optimized:** Yes
