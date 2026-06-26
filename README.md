# Business Insights 360

A comprehensive Power BI dashboard project designed to deliver multi-dimensional business analytics across Finance, Sales, Marketing, Supply Chain, and Executive functions.

## Project Overview

**Business Insights 360** is a professional-grade analytics dashboard built as part of the Codebasics Power BI course. It demonstrates advanced Power BI capabilities including complex DAX measures, interactive reporting, and multi-functional business intelligence.

### Key Features

- **10 Report Pages** covering diverse business functions
- **Advanced DAX Measures** for financial analysis, forecasting, and KPI tracking
- **Star Schema Data Model** optimized for performance
- **Custom Visuals** including Deneb and native Power BI visuals
- **P&L Waterfall Analysis** with drill-down capabilities
- **Forecast Accuracy Tracking** with variance analysis
- **Market Share Monitoring** and competitive analysis
- **Executive Summary Dashboard** with key metrics

## Report Pages

1. **Finance View** - P&L statement, variance analysis, budget vs. actual
2. **Sales View** - Revenue trends, product performance, regional analysis
3. **Marketing View** - Campaign ROI, customer acquisition, retention metrics
4. **Supply Chain View** - Inventory levels, supplier performance, logistics
5. **Executive Summary** - KPI dashboard with key business metrics
6. **Forecast Accuracy** - Actual vs. projected performance
7. **Market Share** - Competitive positioning and trends
8. **Customer Analytics** - Segmentation and lifetime value
9. **Product Performance** - Sales mix and profitability
10. **Detailed Analytics** - Drill-down exploration page

## Technical Specifications

### Data Model
- **Dimension Tables:** 5 (Date, Product, Customer, Sales Rep, Territory)
- **Fact Tables:** 2 (Sales Transactions, Budget)
- **Total Records:** 500K+ transactions
- **Architecture:** Star Schema with optimized relationships

### DAX Measures
- **50+ Calculated Measures** including:
  - Revenue, Gross Profit, Net Profit
  - Forecast vs. Actual variance
  - Market Share percentage
  - YoY growth calculations
  - Rolling averages
  - Forecast accuracy metrics

### Custom Visuals & Features
- Deneb for advanced custom visualizations
- Conditional formatting with drill-through actions
- Dynamic titles and descriptions
- Performance optimization with aggregations

## Data Sources

- **Sales Database:** AtliQ Motors sales transactions
- **Budget Data:** Financial planning database
- **Market Data:** Competitive intelligence feeds
- **Supplementary Data:** Product catalog, customer master

## How to Use

### Opening the Dashboard
1. Download the `.pbix` file from this repository
2. Open in Power BI Desktop (latest version recommended)
3. Refresh data connections to your data sources
4. Navigate through report tabs using the page navigation

### Key Interactions
- **Slicers:** Use date, product, and region filters for segmentation
- **Drill-through:** Click on visuals to explore detailed data
- **Bookmarks:** Save and switch between different report views
- **Tooltips:** Hover over visuals for additional context

## Performance Tips

- **Filter strategically** to reduce data load
- **Use bookmarks** for saved report states
- **Disable auto-refresh** if working with large datasets
- **Archive historical data** to maintain performance

## Lessons Learned

This project demonstrates:
- Complex DAX formula composition
- Efficient data modeling for multi-dimensional analysis
- User experience design in BI dashboards
- Performance optimization techniques
- Professional report layout and design

## Files Included

- `Business_Insights_360.pbix` - Main Power BI workbook
- `DAX_Measures.md` - Complete DAX measure documentation
- `Data_Model_Schema.md` - Detailed data model specification
- `README.md` - This file
- `.gitignore` - Git ignore configuration

## Future Enhancements

- [ ] Real-time data refresh integration
- [ ] Mobile-optimized views
- [ ] Advanced AI features (Q&A, decomposition tree)
- [ ] Paginated reports for print distribution
- [ ] Integration with Power Automate workflows

## Technical Skills Demonstrated

✅ Advanced DAX (CALCULATE, FILTER, SUMX, RANKX)  
✅ Data Modeling (Star schema, relationships, cardinality)  
✅ Performance Optimization (Aggregations, columnar storage)  
✅ Power BI Features (Bookmarks, drill-through, conditional formatting)  
✅ Report Design (Color theory, typography, user experience)  
✅ Business Logic (Financial analysis, forecasting, market analysis)

## Installation & Setup

### Prerequisites
- Power BI Desktop (version 2.100 or later)
- Familiarity with Power BI concepts
- Access to data sources (or sample data for demo)

### Steps
1. Clone this repository
2. Download `Business_Insights_360.pbix`
3. Open in Power BI Desktop
4. Configure data source connections
5. Refresh all tables
6. Navigate through reports

## Author

**Naved Anjum (Noddy)**  
Senior MIS Reporting Manager  
Power BI Developer | Business Intelligence Analyst  
[LinkedIn](#) | [GitHub](https://github.com/navedanjumnoddy)

## Acknowledgments

- **Course:** Codebasics Power BI Bootcamp
- **Instructor:** Dhaval Patel
- **Dataset:** AtliQ Motors (fictitious company for learning)

## License

This project is shared for portfolio and educational purposes. Commercial use requires explicit permission.

---

**Last Updated:** June 2026  
**Status:** Production Ready
