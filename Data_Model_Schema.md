# Business Insights 360 - Data Model Schema

Comprehensive documentation of the star schema data model powering the Business Insights 360 Power BI dashboard.

## Data Model Architecture

**Schema Type:** Star Schema (Dimensional Model)  
**Design Pattern:** Fact-Dimension architecture optimized for analytical queries  
**Key Features:** Denormalized dimensions, optimized for Power BI DirectQuery and Import modes

### Model Overview
```
                    ┌─────────────────┐
                    │   Sales (Fact)  │
                    └─────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
    ┌───────────┐     ┌──────────────┐    ┌──────────────┐
    │   Date    │     │   Product    │    │   Customer   │
    │(Dimension)│     │ (Dimension)  │    │ (Dimension)  │
    └───────────┘     └──────────────┘    └──────────────┘
        │                   │                   │
    ┌────────────┐    ┌───────────┐       ┌──────────────┐
    │Sales Rep   │    │ Category  │       │   Territory  │
    │(Dimension) │    │(Dimension)│       │ (Dimension)  │
    └────────────┘    └───────────┘       └──────────────┘
```

---

## Fact Tables

### Sales (Main Fact Table)

**Purpose:** Core transaction-level sales data  
**Granularity:** One row per sale transaction  
**Record Count:** ~500,000 rows  
**Update Frequency:** Daily  

#### Columns:

| Column Name | Data Type | Description | Business Use |
|---|---|---|---|
| **Sales ID** | Integer | Primary Key | Unique transaction identifier |
| **Order ID** | Integer | Foreign Key | Links to orders |
| **Customer ID** | Integer | Foreign Key | Links to Customer dimension |
| **Product ID** | Integer | Foreign Key | Links to Product dimension |
| **Sales Rep ID** | Integer | Foreign Key | Links to Sales Rep dimension |
| **Territory ID** | Integer | Foreign Key | Links to Territory dimension |
| **Date ID** | Integer | Foreign Key | Links to Date dimension |
| **Quantity** | Integer | Measure | Units sold |
| **Unit Price** | Decimal | Measure | Price per unit (before discounts) |
| **Unit Cost** | Decimal | Measure | Cost per unit (COGS) |
| **Discount %** | Decimal | Measure | Discount percentage applied |
| **Net Sales Amount** | Decimal | Measure | Revenue after discounts |
| **Gross Profit** | Decimal | Measure | Revenue minus COGS |
| **Sales Channel** | Text | Attribute | Direct, Distribution, Online |
| **Transaction Type** | Text | Attribute | Sale, Return, Exchange |
| **Load Date** | Date | Metadata | Data load timestamp |

#### Key Relationships:
- Sales.Customer ID → Customer.Customer ID (M:1)
- Sales.Product ID → Product.Product ID (M:1)
- Sales.Sales Rep ID → Sales Rep.Sales Rep ID (M:1)
- Sales.Territory ID → Territory.Territory ID (M:1)
- Sales.Date ID → Date.Date ID (M:1)

#### Aggregations:
- Sum: Quantity, Unit Price, Net Sales Amount, Gross Profit
- Count: Sales ID
- Distinct Count: Customer ID, Product ID

---

### Budget (Secondary Fact Table)

**Purpose:** Budget vs. Actual variance analysis  
**Granularity:** Monthly budget by product/region  
**Record Count:** ~50,000 rows  
**Update Frequency:** Monthly  

#### Columns:

| Column Name | Data Type | Description |
|---|---|---|
| **Budget ID** | Integer | Primary Key |
| **Product ID** | Integer | Foreign Key |
| **Territory ID** | Integer | Foreign Key |
| **Date ID** | Integer | Foreign Key (Month) |
| **Budget Amount** | Decimal | Budgeted revenue |
| **Forecast Amount** | Decimal | Updated forecast |
| **Budget vs Actual** | Decimal | Variance (calculated) |
| **Budget Status** | Text | On-Track, At-Risk, Exceeded |

#### Relationships:
- Budget.Product ID → Product.Product ID (M:1)
- Budget.Territory ID → Territory.Territory ID (M:1)
- Budget.Date ID → Date.Date ID (M:1)

---

## Dimension Tables

### Date (Time Dimension)

**Purpose:** Enable time intelligence and temporal analysis  
**Granularity:** Daily  
**Date Range:** 2020-01-01 to 2026-12-31  
**Record Count:** ~2,500 rows  

#### Columns:

| Column Name | Data Type | Description | Example |
|---|---|---|---|
| **Date ID** | Integer | Primary Key | 20260626 |
| **Date** | Date | Full date | 2026-06-26 |
| **Year** | Integer | Calendar year | 2026 |
| **Quarter** | Integer | Quarter number | 2 |
| **Month** | Integer | Month number | 6 |
| **Month Name** | Text | Month full name | June |
| **Month Short** | Text | Month abbreviation | Jun |
| **Day of Month** | Integer | Day (1-31) | 26 |
| **Day of Week** | Integer | Weekday number | 5 |
| **Day Name** | Text | Weekday full name | Friday |
| **Week Number** | Integer | ISO week number | 26 |
| **Is Weekend** | Boolean | Weekend flag | FALSE |
| **Fiscal Year** | Integer | Fiscal year | 2026 |
| **Fiscal Quarter** | Integer | Fiscal quarter | 3 |
| **Days in Month** | Integer | Total days in month | 30 |

#### Usage:
- **Time Intelligence:** SAMEPERIODLASTYEAR, DATESYTD, DATESMTD
- **Filtering:** Year, Quarter, Month slicers
- **Aggregation:** Monthly, quarterly, annual summaries

---

### Product (Product Dimension)

**Purpose:** Product hierarchy and attributes  
**Granularity:** Individual SKU  
**Record Count:** ~1,200 rows  

#### Columns:

| Column Name | Data Type | Description | Example |
|---|---|---|---|
| **Product ID** | Integer | Primary Key | 1001 |
| **Product Name** | Text | Full product name | "Laptop Pro 15" |
| **Category** | Text | Product category | "Electronics" |
| **Subcategory** | Text | Product subcategory | "Computers" |
| **Brand** | Text | Manufacturer brand | "TechBrand" |
| **Product Line** | Text | Product family | "Professional" |
| **Cost** | Decimal | Standard cost | 1200.00 |
| **List Price** | Decimal | Standard list price | 1999.99 |
| **Margin %** | Decimal | Standard margin | 40.0 |
| **Product Status** | Text | Active, Discontinued, New | "Active" |
| **Launch Date** | Date | Product launch date | 2025-01-15 |
| **Color** | Text | Product color | "Silver" |
| **Size** | Text | Product size | "15-inch" |
| **Supplier ID** | Integer | Supplier reference | 501 |

#### Hierarchy:
```
Category → Subcategory → Brand → Product Line → Product Name
```

#### Usage:
- **Drill-down Analysis:** Category to individual products
- **Profitability Analysis:** Cost, margin tracking
- **Product Mix:** Sales by category, subcategory

---

### Customer (Customer Dimension)

**Purpose:** Customer attributes and segmentation  
**Granularity:** Individual customer  
**Record Count:** ~25,000 rows  

#### Columns:

| Column Name | Data Type | Description | Example |
|---|---|---|---|
| **Customer ID** | Integer | Primary Key | 10001 |
| **Customer Name** | Text | Full customer name | "Acme Corp" |
| **Segment** | Text | Customer segment | "Enterprise" |
| **Industry** | Text | Customer industry | "Manufacturing" |
| **Country** | Text | Customer country | "United States" |
| **State/Province** | Text | State/province | "California" |
| **City** | Text | City | "San Francisco" |
| **Postal Code** | Text | Postal code | "94105" |
| **Customer Since** | Date | First purchase date | 2020-03-15 |
| **Account Status** | Text | Active, Inactive, Dormant | "Active" |
| **Annual Revenue** | Decimal | Customer annual revenue | 5000000 |
| **Employee Count** | Integer | Customer employees | 500 |
| **Credit Limit** | Decimal | Credit ceiling | 100000 |
| **Territory ID** | Integer | Primary territory | 15 |

#### Segments:
- **Enterprise:** Large accounts, $10M+ annual revenue
- **Mid-Market:** Medium accounts, $1M-$10M
- **Small Business:** Small accounts, <$1M
- **Startup:** New/emerging customers

#### Usage:
- **Customer Analytics:** RFM analysis, churn prediction
- **Segmentation:** Segment-based reporting
- **Geographic Analysis:** Maps, regional performance

---

### Sales Rep (Employee Dimension)

**Purpose:** Sales team attributes and management hierarchy  
**Granularity:** Individual sales representative  
**Record Count:** ~500 rows  

#### Columns:

| Column Name | Data Type | Description | Example |
|---|---|---|---|
| **Sales Rep ID** | Integer | Primary Key | 20001 |
| **Sales Rep Name** | Text | Full name | "John Smith" |
| **Title** | Text | Job title | "Senior Account Executive" |
| **Department** | Text | Department | "Sales" |
| **Manager ID** | Integer | Direct manager | 20000 |
| **Manager Name** | Text | Manager name | "Sarah Johnson" |
| **Territory ID** | Integer | Primary territory | 15 |
| **Hire Date** | Date | Hire date | 2019-06-01 |
| **Commission Rate** | Decimal | Commission percentage | 5.0 |
| **Status** | Text | Active, Inactive, On Leave | "Active" |
| **Email** | Text | Email address | "john.smith@company.com" |
| **Phone** | Text | Phone number | "+1-555-0123" |

#### Hierarchy:
```
Manager → Sales Rep → Territory → Customers
```

#### Usage:
- **Sales Performance:** Individual rep metrics
- **Commission Tracking:** Earned commissions
- **Management Reporting:** Team hierarchies

---

### Territory (Geography Dimension)

**Purpose:** Geographic segmentation and regional analysis  
**Granularity:** Sales territory  
**Record Count:** ~80 rows  

#### Columns:

| Column Name | Data Type | Description | Example |
|---|---|---|---|
| **Territory ID** | Integer | Primary Key | 15 |
| **Territory Name** | Text | Territory name | "California North" |
| **Region** | Text | Large geographic region | "West Coast" |
| **Sub-region** | Text | Sub-region | "Bay Area" |
| **Country** | Text | Country | "United States" |
| **State/Province** | Text | State/province | "California" |
| **Territory Manager ID** | Integer | Manager reference | 20050 |
| **Territory Sales Target** | Decimal | Annual sales target | 2000000 |

#### Hierarchy:
```
Country → Region → Sub-region → Territory
```

#### Usage:
- **Geographic Reporting:** Sales by region
- **Competitive Analysis:** Territory performance
- **Expansion Planning:** Regional growth

---

## Data Quality & Integrity

### Primary Keys
- All dimension tables have surrogate keys (integer IDs)
- Natural keys mapped in documentation
- No NULL values in primary keys

### Foreign Keys
- All facts reference dimensions via foreign keys
- Referential integrity enforced
- No orphaned fact rows

### Data Validation
- **Fact Table:** Row count = 500K transactions
- **Date Range:** 2020-2026 (complete years)
- **Customer Deduplication:** No duplicate records
- **Product Validation:** Active products verified

---

## Relationships & Cardinality

| From | To | Type | Cardinality |
|---|---|---|---|
| Sales | Date | FK | M:1 |
| Sales | Customer | FK | M:1 |
| Sales | Product | FK | M:1 |
| Sales | Sales Rep | FK | M:1 |
| Sales | Territory | FK | M:1 |
| Budget | Date | FK | M:1 |
| Budget | Product | FK | M:1 |
| Budget | Territory | FK | M:1 |
| Customer | Territory | FK | M:1 |

---

## Performance Considerations

✅ **Fact Table:** Indexed on all foreign keys  
✅ **Dimensions:** Indexed on all primary keys  
✅ **Date Dimension:** Optimized for time intelligence  
✅ **Aggregations:** Pre-calculated for large datasets  
✅ **Compression:** Columnar storage for efficient querying  

### Query Optimization Tips:
- Filter by Date first to reduce fact table volume
- Use CALCULATE with specific dimension filters
- Avoid unnecessary cross-dimension joins
- Leverage aggregation tables for large measures

---

## Data Dictionary Summary

**Total Tables:** 7 (2 Fact, 5 Dimension)  
**Total Columns:** 85+  
**Total Relationships:** 8  
**Record Count:** ~550,000 rows  
**Last Updated:** June 2026

---

**Schema Version:** 2.0  
**Design Pattern:** Star Schema  
**Optimization Level:** Production-Ready
