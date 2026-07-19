| Document | Business Requirements Document |
|----------|-------------------------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.0 |
| Status | Draft |
| Author | Hoa Duong |
| Last Updated | 2026-07-18 |

# Business Requirements Document (BRD) — VanArsdel Retail Dashboard

## 1. Business Background

VanArsdel stores sales, product, customer, marketing, and planning data across multiple source files. Although the data is available, there is currently no centralized reporting solution that enables executives and business managers to monitor business performance consistently.

Business users need a single dashboard to:

- Monitor sales performance over time.
- Compare actual performance against business plans.
- Identify high- and low-performing products, regions, and marketing channels.
- Understand which marketing channels contribute the most revenue and customer acquisition.
- Track customer growth and purchasing activity.
- Support faster, data-driven operational and strategic decision-making.

This project aims to deliver a Dashboard that consolidates Actual Sales, Budget, and Forecast data into a single reporting solution for management across multiple business functions.

---

## 2. Business Objectives

The dashboard should enable users to:

- Monitor overall business performance using key sales KPIs.
- Compare Actual performance against Budget where comparable data exists.
- Review Forecast values for future planning.
- Analyze revenue contribution by product, geography, and marketing channel.
- Identify the best-performing marketing channels based on revenue attribution.
- Monitor customer acquisition and purchasing trends.
- Support strategic and operational decision-making.

---

## 3. Stakeholders & Business Decisions

| Stakeholder | Business Need | Business Decision Supported |
|-------------|---------------|-----------------------------|
| **CEO / COO** | Executive overview of overall business performance | Business strategy and long-term growth planning |
| **Sales Director** | Monitor revenue, profitability, product performance, and regional performance | Sales strategy, target setting, and resource allocation |
| **Sales Manager** | Analyze sales trends by product, category, region, and time | Sales execution, territory management, and performance improvement |
| **Finance Manager** | Monitor Budget performance and profitability | Budget control and financial planning |
| **Marketing Manager** | Evaluate revenue contribution by marketing channel and device | Marketing channel optimization and campaign planning |
| **Regional Sales Manager** | Monitor regional sales performance | Regional planning and resource allocation |

---

## 4. Key Business Questions

The dashboard should answer the following questions:

1. How are Revenue, Units Sold, Gross Profit, and Gross Margin changing over time?
2. Which Regions, Categories, Segments, and Products contribute the most revenue?
3. How does Actual performance compare with Budget during the comparable reporting period?
4. What is the expected business performance according to the 2021 Forecast?
5. Which Traffic Channel and Device combinations contribute the most revenue and customer acquisition?
6. How many customers purchased during the selected period?
7. How many customers are newly acquired during the reporting period?
8. Which products and regions are the highest and lowest performers?
9. How does business performance change compared with the previous month and the same period last year?

---

## 5. Business Requirements

### BR01 – Executive Overview

The dashboard shall display the following KPIs:

- Total Revenue
- Units Sold
- Total COGS
- Gross Profit
- Gross Margin %
- Total Customers
- New Customers

---

### BR02 – Sales Analysis

Users shall be able to analyze sales by:

- Year
- Quarter
- Month
- Region
- Category
- Segment
- Product

---

### BR03 – Budget Analysis

The dashboard shall allow users to:

- Compare Actual Revenue against Budget.
- View Budget Variance.
- View Budget Attainment %.

---

### BR04 – Forecast Analysis

The dashboard shall display Forecast values separately from Actual performance for future planning.

---

### BR05 – Marketing Analysis

The dashboard shall allow users to:

- Analyze Revenue by Traffic Channel.
- Analyze Revenue by Device.
- Compare revenue contribution across marketing channels.
- Identify the highest revenue-generating Traffic Channel × Device combinations.

---

### BR06 – Customer Analysis

The dashboard shall display:

- Total Customers.
- New Customers.
- Customer distribution by Region.

---

### BR07 – Interactive Reporting

The dashboard shall support:

- Filtering
- Cross-filtering
- Drill-down
- Drill-through (if implemented)
- Export (subject to user permissions)

---

## 6. KPI Definitions

> Business formulas only. Technical implementation (e.g., DAX) is documented separately in the Metric Dictionary.

| KPI | Business Formula | Description |
|------|------------------|-------------|
| Revenue | Units × Unit Price | Total sales value generated from products sold. |
| Units Sold | Sum of Units | Total quantity of products sold. |
| Total COGS | Units × Unit Cost | Total cost of goods sold. |
| Gross Profit | Revenue − Total COGS | Profit after deducting product costs. |
| Gross Margin % | Gross Profit ÷ Revenue | Gross profitability ratio. |
| Budget Amount | Budget Revenue | Planned revenue for the reporting period. |
| Forecast Amount | Forecast Revenue | Expected revenue for future periods. |
| Budget Attainment % | Actual Revenue ÷ Budget Revenue | Percentage of budget achieved. |
| Budget Variance | Actual Revenue − Budget Revenue | Difference between Actual and Budget revenue. |
| Total Customers | Distinct CustomerID | Number of unique purchasing customers. |
| New Customers | First recorded purchase within the reporting period | Customers making their first recorded purchase during the selected period. |
| MoM Revenue Growth % | Revenue change compared with previous month | Month-over-month revenue growth. |
| YoY Revenue Growth % | Revenue change compared with the same period last year | Year-over-year revenue growth. |

---

## 7. Business Rules

| ID | Business Rule |
|----|---------------|
| **BR01** | Revenue = Units × Unit Price. |
| **BR02** | Total COGS = Units × Unit Cost. |
| **BR03** | Gross Profit = Revenue − Total COGS. |
| **BR04** | Gross Margin % = Gross Profit / Revenue. |
| **BR05** | Budget Attainment % = Actual Revenue / Budget Revenue for the same reporting period. |
| **BR06** | A New Customer is a customer whose first recorded purchase occurs within the selected reporting period. |
| **BR07** | MoM and YoY calculations use the Date dimension. |

---

## 8. Assumptions

- All monetary values are assumed to be reported in **USD**.
- Unit Price and Unit Cost remain constant throughout the available historical period.
- Each Sales record represents one sales transaction line.
- Sales data contains completed transactions only.
- New Customer is derived from the customer's first recorded purchase date.
- Marketing performance is evaluated using **revenue attribution only**, as campaign cost data is unavailable.

---

## 9. Scope

### In Scope

- Revenue, profitability, and customer KPIs.
- Actual vs Budget analysis.
- Forecast reporting.
- Product, Region, Category, and Segment analysis.
- Marketing attribution analysis by Traffic Channel and Device.
- Interactive filtering and drill-down.

### Out of Scope

- Marketing ROI, ROAS, CAC, or campaign cost analysis.
- Inventory analysis.
- Discount and return analysis.
- Customer Lifetime Value (CLV).
- Customer churn analysis.
- Order-level analysis.
- Competitor comparison.

---

## 10. Acceptance Criteria

The dashboard will be considered complete when:

- All approved KPIs are calculated according to the agreed business definitions.
- KPI values are validated against the source data.
- Filters, drill-down, and interactive features function correctly.
- Budget and Forecast are displayed according to the approved business rules.
- All key business questions defined in this document can be answered using the dashboard.
- Stakeholders successfully complete User Acceptance Testing (UAT) and approve the solution.

---

## 11. Related Documents

The following documents provide additional business and technical details:

- Business Glossary
- Data Dictionary
- Metric Dictionary
- Data Gaps & Stakeholder Questions
