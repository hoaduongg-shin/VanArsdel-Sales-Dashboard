# Data dictionary — VanArsdel Retail Dashboard

Keep in lock-step with the TMDL semantic model. One section per table, then a measures section.

## Conventions
- **Model Name** = user-facing name in the model. **Source Column** = physical `snake_case` column pasted in `dataset/raw/`.
- Types: `int64`, `double`, `string`, `dateTime`, `boolean`.
- Source workbook 1: `VanArsdel_Actuals.xlsx` (sheets: Date, Campaign, Customer, Product, Geo, Sales).
- Source workbook 2: `VanArsdel_Budget_Forecast.xlsx` (sheet: Sheet1, wide format — must be **unpivoted** before load; see `BudgetForecast` table below).

---

## Table: `Date`  *(dimension)*
- **Grain:** one row per calendar day
- **Source file:** `VanArsdel_Actuals.xlsx` (sheets: Date)

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Date | Date | dateTime | PK | no | Calendar date, one row per day | 1/1/2016 |
| Month Number | MonthNo | int64 | – | yes | Numeric month (1–12), used for sorting Month | 1 |
| Month Name | MonthName | string | – | yes | Short month name | Jan |
| Month ID | MonthID | int64 | – | yes | Sortable YYYYMM key | 202101 |
| Month | Month | string | – | no | Display label "Mon-YY" | Jan-21 |
| Quarter | Quarter | string | – | no | Calendar quarter | Q1 |
| Year | Year | int64 | – | no | Calendar year | 2021 |

---

## Table: `Campaign`  *(dimension)*
- **Grain:** one row per (TrafficChannel × Device) combination
- **Source file:** `VanArsdel_Actuals.xlsx` (sheets: Campaign)`

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Campaign ID | CampaignID | int64 | PK | yes | Unique identifier of a campaign touchpoint | 22 |
| Traffic Channel | TrafficChannel | string | – | no | Marketing channel associated with a purchase | Affiliate |
| Device | Device | string | – | no | Customer interaction platform associated with a purchase | Mobile |

---

## Table: `Customer`  *(dimension)*
- **Grain:** one row per customer
- **Source file:** VanArsdel_Actuals.xlsx` (sheets: Customer) — **282,597 rows**

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Customer ID | CustomerID | string | PK | yes | Unique customer identifier | 000001 |
| Zip Code | ZipCode | string | FK → Geo.Zip | yes | Customer postal code | 90250 |
| Email | *(parsed from Email Name)* | string | – | no | Extracted email address | Meghan.Alexander@xyza.com |
| Customer Name | *(parsed from Email Name)* | string | – | no | Extracted "First Last" | Meghan Alexander |
| Email Name (raw) | Email Name | string | – | yes | Original customer identification string from the source — kept hidden for lineage/debug | (Meghan.Alexander@xyza.com): Alexander, Meghan |

---

## Table: `Product`  *(dimension)*
- **Grain:** one row per SKU
- **Source file:** VanArsdel_Actuals.xlsx` (sheets: Product) — **212 rows**

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Product ID | ProductID | string | PK | yes | SKU key | 392 |
| Product | Product | string | – | no | SKU display name | Maximus RP-01 |
| Category | Category | string | – | no | Top-level product grouping (5 values) | Rural |
| Segment | Segment | string | – | no | Sub-classification within Category (10 real combos with Category) | Productivity |
| Manufacturer ID | ManufacturerID | string | – | yes | Manufacturer identifier - Constant = 7 for all rows | 7 |
| Manufacturer | Manufacturer | string | – | yes | Manufacturer name - Constant = "VanArsdel" for all rows | VanArsdel |
| Unit Cost | Unit Cost | double | – | no | Static production cost per unit (USD, assumed) | 37.27 |
| Unit Price | Unit Price | double | – | no | Static selling price per unit (USD, assumed) | 51.06 |

---

## Table: `Geo`  *(dimension)*
- **Grain:** one row per zip code
- **Source file:** VanArsdel_Actuals.xlsx` (sheets: Geo) — **39,948 rows**

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Zip | Zip | string | PK | yes | Zip code key | 22654 |
| State | State | string | – | no | State code | VA |
| Region | Region | string | – | no | Geographic region: East / Central / West | East |
| District | District | string | – | no | Sub-region code | District #07 |
| Country | Country | string | – | yes | Country - aAways "USA" in this dataset | USA |

---

## Table: `Sales`  *(fact)*
- **Grain:** one row per (product × customer × date × campaign touchpoint) sale event — **no separate order/basket ID**, so 1 row = 1 sellable unit-event, not necessarily 1 "order"
- **Source file:** VanArsdel_Actuals.xlsx` (sheets: Sales) — **675,368 rows**

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Product ID | ProductID | string | FK → Product | yes | Product sold | 676 |
| Date | Date | dateTime | FK → Date | yes | Transaction date | 2015-09-25 |
| Customer ID | CustomerID | string | FK → Customer | yes | Purchasing customer | 070283 |
| Campaign ID | CampaignID | int64 | FK → Campaign | yes | Marketing touchpoint associated with the sale | 22 |
| Units | Units | int64 | – | no | Quantity sold | 1 |

---

## Table: `BudgetForecast`  *(fact — requires unpivot)*
- **Grain (after transform):** one row per (Type × Year × Month × Category-Segment)
- **Source file:** `VanArsdel_Budget_Forecast.xlsx`

| Model Name | Source Column | Type | Key | Hidden | Description | Example |
|------------|---------------|------|-----|--------|-------------|---------|
| Type | Type | string | – | no | Planning scenario - `Budget` or `Forecast` | Budget |
| Year | Year | int64 | – | no | Plan year | 2020 |
| Month | Month | string | – | no | Calendar month | Dec |
| Category-Segment | (derived) | string | FK → Product | yes | Product group covered by the plan | Urban-Convenience |
| Amount | (derived) | double | – | no | Planned value for the reporting period | 120710.44 |

---

## Measures  *(all on `Key Measures`)*

> DAX below is a **starting suggestion for the DA**, not a required exact syntax — the BA's job is the business formula; the DA should validate/optimize the implementation (e.g. filter context on the disconnected `Category-Segment` key).

| Measure | Folder | Format | DAX (draft) | Definition |
|---------|--------|--------|-----|------------|
| Total Revenue | 01 Volume & Revenue | Currency | `SUMX(Sales, Sales[Units] * RELATED(Product[Unit Price]))` | Units × Unit Price, Total sales value generated from products sold |
| Total Units | 01 Volume & Revenue | #,##0 | `SUM(Sales[Units])` | Total quantity sold |
| Total COGS | 01 Volume & Revenue | Currency | `SUMX(Sales, Sales[Units] * RELATED(Product[Unit Cost]))` | Units × Unit Cost, Total direct production cost of products sold |
| Gross Profit | 01 Volume & Revenue | Currency | `[Total Revenue] - [Total COGS]` | Revenue minus cost |
| Gross Margin % | 01 Volume & Revenue | Percent | `DIVIDE([Gross Profit], [Total Revenue])` | Profitability ratio |
| Budget Amount | 02 Budget & Forecast | Currency | `CALCULATE(SUM(BudgetForecast[Amount]), BudgetForecast[Type] = "Budget")` | Planned revenue for the current filter context |
| Forecast Amount | 02 Budget & Forecast | Currency | `CALCULATE(SUM(BudgetForecast[Amount]), BudgetForecast[Type] = "Forecast")` | Latest projected revenue for the reporting period |
| Budget Attainment % | 02 Budget & Forecast | Percent | `DIVIDE([Total Revenue], [Budget Amount])` | Actual ÷ Budget — Percentage of the planned revenue target achieved |
| Budget Variance | 02 Budget & Forecast | Currency | `[Total Revenue] - [Budget Amount]` | Difference between actual revenue and the planned target |
| Total Customers | 03 Customers | #,##0 | `DISTINCTCOUNT(Sales[CustomerID])` | Number of unique customers who made a purchase |
| MoM Revenue Growth % | 04 Time Intelligence | Percent | `DIVIDE([Total Revenue] - CALCULATE([Total Revenue], DATEADD(Date[Date], -1, MONTH)), CALCULATE([Total Revenue], DATEADD(Date[Date], -1, MONTH)))` | Percentage change in revenue compared with the previous month |
| YoY Revenue Growth % | 04 Time Intelligence | Percent | `DIVIDE([Total Revenue] - CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date])), CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Date[Date])))` | Revenue vs. same period last year |
