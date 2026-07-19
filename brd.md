| Document | Business Requirements Document |
|----------|-------------------------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.1 |
| Status | Draft |
| Author | Hoa Duong |
| Last Updated | 2026-07-19 |

# Business Requirements Document (BRD) — VanArsdel Retail Dashboard

## 1. Business Background

VanArsdel stores sales, product, customer, marketing, and planning data across multiple source files. Although the data is available, there is currently no centralized reporting solution that enables executives and business managers to monitor business performance consistently.

Business users need a single dashboard to:

- Monitor sales performance over time.
- Compare actual performance against business plans (Budget/Forcast).
- Identify high- and low-performing products, regions, and marketing channels.
- Understand which marketing channels contribute the most revenue and customer acquisition.
- Track customer growth, repeat-purchase behavior, and customer profitability.
- Support faster, data-driven operational and strategic decision-making.

This project delivers a **three-page dashboard** that consolidates Actual Sales, Budget, and Forecast data into a single reporting solution serving management across multiple business functions:

1. **Overview – Sales & Finance** (executive & finance)
2. **Performance & Geography** (sales & regional management)
3. **Customer & Behavior** (marketing & product development)

---

## 2. Business Objectives

The dashboard should enable users to:

- Monitor overall business performance using key sales and financial KPIs.
- Compare Actual performance against Budget where comparable data exists (Revenue, at Category/Segment/Month level).
- Review Forecast values (2021) for future planning.
- Analyze revenue contribution by product, geography, category, and segment.
- Identify the best-performing marketing channels based on revenue attribution.
- Monitor customer acquisition, repeat-purchase behavior, and customer profitability.
- Support strategic and operational decision-making.

---

## 3. Stakeholders & Business Decisions

| Stakeholder | Dashboard Page | Business Need | Business Decision Supported |
|-------------|----------------|---------------|-----------------------------|
| **CEO / COO** | Page 1 | Executive overview of overall performance & plan attainment | Business strategy and long-term growth planning |
| **Finance Manager** | Page 1 | Monitor Budget attainment, cost, and profitability | Budget control and financial planning |
| **Sales Director** | Page 2 | Monitor revenue, product performance, and regional performance | Sales strategy, target setting, resource allocation |
| **Sales Manager** | Page 2 | Analyze sales by product, category, region, and time | Sales execution and territory management |
| **Regional Sales Manager** | Page 2 | Monitor regional sales performance (drill-down) | Regional planning and resource allocation |
| **Marketing Manager** | Page 3 | Evaluate revenue/acquisition by channel and customer behavior | Marketing channel optimization and targeting |
| **Product Development** | Page 3 | Understand purchasing behavior by segment and customer profitability | Product and segment strategy |

---

## 4. Dashboard Structure

> This section defines the agreed page layout, target audience, KPIs, and visuals. Detailed axis and interaction specifications are maintained in the Dashboard Wireframe.

### Page 1 — Overview: Sales & Finance
**Audience:** CEO / COO, Finance

**KPIs (4):** Total Revenue (Actual) · Total COGS · Gross Profit · Budget Attainment %

**Visuals:**
| # | Visual | Type | X axis | Y axis |
|---|--------|------|--------|--------|
| 1 | Actual vs Budget vs Forecast over time | Line | Time (**rolling 13 months**) | Revenue (3 series) |
| 2 | Gross Profit vs Gross Margin over time (Actual only) | Line | Time (**rolling 12 months**) | Gross Profit & Margin % |
| 3 | Revenue → COGS → Gross Profit | **Funnel** | 3 stages | Value |
| 4 | Budget Attainment % by Category | Column + line | Category | Column: Revenue · Line: Attainment % |

**Rolling window** adapts to the selected time grain (Month → rolling months, Quarter → rolling quarters). The time filter is hierarchical (Year–Quarter–Month).
**Filters:** Date range · Category · Segment

### Page 2 — Performance & Geography
**Audience:** Sales Director, Sales/Regional Manager

**KPIs (3):** Total Units Sold · Total Revenue (Actual) · Average Selling Price per Unit

**Visuals:**
| # | Visual | Type | Layout |
|---|--------|------|--------|
| 1 | Revenue by Region | Bar | ¼ page |
| 2 | Top 10 best-selling products | Bar | ¼ page |
| 3 | Regional performance detail (drill-down): Geo level · Revenue · Gross Profit · Gross Margin % · Prior-Year Revenue | **Matrix** | ½ page |

**Filters:** Date range · Category · Segment

### Page 3 — Customer & Behavior
**Audience:** Marketing, Product Development

**KPIs (4):** Total Customers · New Customers · Repeat Purchase Rate % · Average Purchase Frequency

**Visuals:**
| # | Visual | Type | X axis | Y axis |
|---|--------|------|--------|--------|
| 1 | New vs Returning customers over time | Clustered column | Month | Customers (2 groups) |
| 2 | Revenue & Orders by Traffic Channel | Clustered column, dual Y | Traffic Channel | Y-left: Revenue · Y-right: Orders |
| 3 | Customers by Segment | Bar | Segment | Customer count |
| 4 | Customer Profitability Tiering | Column / ranked table | Tier (Top 10% / next 20% / low) | % of Gross Profit contributed |

**Filters:** Date range · Traffic Channel · Device

---

## 5. Key Business Questions

The dashboard should answer the following questions:

1. How are Revenue, Units Sold, Gross Profit, and Gross Margin changing over time (rolling 12/13-month trend)?
2. How does Actual Revenue compare with Budget during the comparable reporting period, overall and by Category?
3. What is the expected business performance according to the 2021 Forecast?
4. How is total value converted from Revenue to Gross Profit after COGS?
5. Which Regions, Categories, Segments, and Products contribute the most revenue?
6. Which products and regions are the highest and lowest performers, and how do they compare with the previous year?
7. Which Traffic Channels contribute the most revenue and orders?
8. How many customers purchased, and how many are newly acquired, during the selected period?
9. What share of Gross Profit is contributed by the most profitable customer tiers?
10. How does performance change versus the previous month and the same period last year?

---

## 6. Business Requirements

### BR01 – Executive Overview (Page 1)
The dashboard shall display Total Revenue, Total COGS, Gross Profit, and Budget Attainment %, with a rolling 12/13-month trend of Revenue (Actual/Budget/Forecast) and Gross Profit/Margin, and a Revenue → COGS → Gross Profit funnel.

### BR02 – Sales & Geography Analysis (Page 2)
Users shall be able to analyze sales by Year, Quarter, Month, Region, Category, Segment, and Product, including Top-N product ranking and a drill-down regional performance matrix with prior-year comparison.

### BR03 – Budget Analysis
The dashboard shall compare Actual Revenue against Budget and display Budget Variance and Budget Attainment %, **at Category / Segment / Month level** (the level at which Budget data exists).

### BR04 – Forecast Analysis
The dashboard shall display Forecast values (2021) alongside Actual and Budget in the trend visual for future planning.

### BR05 – Marketing Analysis (Page 3)
The dashboard shall analyze Revenue and Orders by Traffic Channel and allow filtering by Device, based on **revenue attribution only** (campaign cost is unavailable).

### BR06 – Customer Analysis (Page 3)
The dashboard shall display Total Customers, New Customers, Repeat Purchase Rate, Average Purchase Frequency, New-vs-Returning trend, customers by Segment, and Customer Profitability Tiering.

### BR07 – Interactive Reporting
The dashboard shall support filtering, cross-filtering, drill-down, drill-through (if implemented), and export (subject to user permissions).

---

## 7. KPI Definitions

> Business formulas only. Technical implementation (e.g., DAX) is documented separately in the Metric Dictionary.

| KPI | Business Formula | Description |
|------|------------------|-------------|
| Revenue | Units × Unit Price | Total sales value generated from products sold. |
| Units Sold | Sum of Units | Total quantity sold. *(Note: Units = 1 per record, so this equals the number of transaction lines.)* |
| Total COGS | Units × Unit Cost | Total cost of goods sold. |
| Gross Profit | Revenue − Total COGS | Profit after deducting product costs. |
| Gross Margin % | Gross Profit ÷ Revenue | Gross profitability ratio. |
| Average Selling Price (ASP) | Revenue ÷ Units Sold | Average selling value per unit sold. |
| Budget Amount | Budget Revenue | Planned revenue for the reporting period. |
| Forecast Amount | Forecast Revenue | Expected revenue for future periods. |
| Budget Attainment % | Actual Revenue ÷ Budget Revenue | Percentage of budget achieved (same period, comparable level). |
| Budget Variance | Actual Revenue − Budget Revenue | Difference between Actual and Budget revenue. |
| Total Customers | Distinct CustomerID | Number of unique purchasing customers. |
| New Customers | First recorded purchase within the reporting period | Customers making their first recorded purchase during the selected period. |
| Repeat Purchase Rate % | Customers with ≥ 2 purchases ÷ Total Customers | Share of customers who purchased more than once. |
| Average Purchase Frequency | Total Orders ÷ Total Customers | Average number of purchases per customer. |
| Customer Gross Profit | Σ [Units × (Unit Price − Unit Cost)] per CustomerID | Lifetime historical gross profit contributed by a customer. |
| Customer Profitability Tier | Customers ranked by cumulative Customer Gross Profit, grouped into tiers (Top 10%, next 20%, remaining) | Identifies the most profitable ("VIP/Gold") customers. |
| MoM Revenue Growth % | Revenue change vs previous month | Month-over-month revenue growth. |
| YoY Revenue Growth % | Revenue change vs same period last year | Year-over-year revenue growth. |

---

## 8. Business Rules

| ID | Business Rule |
|----|---------------|
| **R-01** | Revenue = Units × Unit Price. |
| **R-02** | Total COGS = Units × Unit Cost. |
| **R-03** | Gross Profit = Revenue − Total COGS. |
| **R-04** | Gross Margin % = Gross Profit / Revenue. |
| **R-05** | Budget Attainment % = Actual Revenue / Budget Revenue for the same reporting period. |
| **R-06** | Budget and Forecast data exist at **Category × Segment × Month** granularity and contain **Revenue only**. Therefore Budget/Forecast comparisons are limited to Revenue at Category/Segment/Month/Year. No Budget exists for Region, Product, Traffic Channel, COGS, or profit — so Gross Profit and Gross Margin are Actual-only, and plan comparison is unavailable on Pages 2 and 3. |
| **R-07** | Actual-vs-Budget comparison is valid only where the two periods overlap (2020). |
| **R-08** | Page 1 time-trend visuals display a rolling window (13 months for Revenue, 12 months for Profit/Margin) anchored on the selected period, adapting to the chosen time grain. |
| **R-09** | A New Customer is a customer whose first recorded purchase occurs within the selected reporting period; Returning = purchasing after the first-purchase period. |
| **R-10** | Customer Profitability Tiering ranks all customers by cumulative historical Gross Profit and splits them into tiers. |
| **R-11** | Customers-by-Segment counts a customer in **every** Segment they purchased (a customer may appear in multiple segments). |
| **R-12** | MoM and YoY calculations use the Date dimension. |

---

## 9. Assumptions

- All monetary values are assumed to be reported in **USD** (to be confirmed with stakeholders).
- Unit Price and Unit Cost remain constant throughout the available historical period.
- Each Sales record represents one sales transaction line; Units = 1 per record.
- Sales data contains completed transactions only.
- **Data coverage:** Actual sales span Jan 2015 – Jun 2020; Budget covers 2020–2021; Forecast covers 2021. The comparable Actual-vs-Budget overlap is 2020 (Jan–Jun).
- **Budget/Forecast structure:** provided at Category-Segment × Month; the combined "Category-Segment" field is **split into separate Category and Segment fields** during data preparation so that Page 1 can filter Budget by Category/Segment.
- Marketing performance is evaluated using **revenue attribution only**, as campaign cost data is unavailable.
- **Required data preparation:** extend the Date dimension back to 2015; remove 133 fully-duplicated sales rows; standardize Campaign values (fix "Affliliate"→"Affiliate", "Deskop"→"Desktop", resolve "Paper"/"Mail"); keep Zip/ZipCode as text (preserve leading zeros); derive a New/Returning flag. *(See Data Gaps & Stakeholder Questions.)*

---

## 10. Scope

### In Scope
- Revenue, profitability, and customer KPIs.
- Actual vs Budget analysis (Revenue, at Category/Segment/Month level).
- Forecast reporting (2021).
- Product, Region, Category, and Segment analysis, with prior-year comparison and drill-down.
- Revenue-to-Gross-Profit funnel.
- Rolling 12/13-month trend analysis.
- Marketing attribution analysis by Traffic Channel (Device as a filter).
- Customer acquisition, repeat-purchase behavior, and **customer profitability tiering** (historical gross-profit contribution).
- Interactive filtering, cross-filtering, and drill-down.

### Out of Scope
- Marketing ROI, ROAS, CAC, or campaign cost analysis.
- Predictive Customer Lifetime Value (CLV) and churn analysis.
- Inventory analysis.
- Discount and return analysis.
- Order-level analysis.
- Competitor comparison.

---

## 11. Acceptance Criteria

The dashboard will be considered complete when:

- All approved KPIs are calculated according to the agreed business definitions.
- KPI values are validated against the source data.
- Filters, drill-down, and interactive features function correctly.
- Budget and Forecast are displayed according to the approved business rules, respecting the Category/Segment/Month granularity constraint.
- The rolling-window trend and Customer Profitability Tiering behave as defined.
- All key business questions defined in this document can be answered using the dashboard.
- Stakeholders successfully complete User Acceptance Testing (UAT) and approve the solution.

---

## 12. Related Documents

- Business Glossary
- Data Dictionary
- Metric Dictionary
- **Data Gaps & Stakeholder Questions** (`VanArsdel_DataGap.xlsx`)
- **Dashboard Wireframe** (3-page layout with axis specifications)
