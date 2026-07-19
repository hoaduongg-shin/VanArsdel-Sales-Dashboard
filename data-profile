| Document | Data Profiling Report |
|----------|-----------------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.0 |
| Status | Draft |
| Author | Hoa Duong |
| Last Updated | 2026-07-19 |

# Data Profiling Report — VanArsdel

## 1. Purpose & Scope

This document describes the structure and content of the VanArsdel source data **as it is** — tables, fields, data types, cardinality, value ranges, key candidates, and relationships. It is descriptive only; issues and remediation are assessed separately in the **Data Quality Assessment**.

**Sources profiled:**
- `VanArsdel_Actuals.xlsx` — star schema: 1 fact table (`Sales`) + 5 dimensions (`Date`, `Product`, `Customer`, `Geo`, `Campaign`).
- `VanArsdel_Budget_Forecast.xlsx` — planning data (Budget & Forecast).

---

## 2. Data Sources Overview

| Table | Role | Rows | Cols | Grain (1 row = ) | Primary Key |
|-------|------|-----:|-----:|------------------|-------------|
| Sales | Fact | 675,368 | 5 | one sales transaction line | none (no unique key) |
| Customer | Dimension | 282,597 | 3 | one customer | CustomerID |
| Geo | Dimension | 39,948 | 6 | one ZIP code | Zip |
| Date | Dimension | 2,192 | 7 | one calendar day | Date |
| Product | Dimension | 212 | 8 | one product | ProductID |
| Campaign | Dimension | 22 | 3 | one campaign | CampaignID |
| Budget/Forecast | Planning | 36* | 12 (wide) | one Type × Year × Month row | Type + Year + Month |

\* 36 data rows in wide layout → 324 rows after unpivot to long (Type · Year · Month · Category · Segment · Amount).

---

## 3. Table-by-Table Profile (Actuals)

### 3.1 Sales (fact)
Grain: one transaction line. **No single or composite column uniquely identifies a row.**

| Column | Type (stored) | Nulls | Distinct | Range / Sample |
|--------|---------------|------:|---------:|----------------|
| ProductID | text | 0 | 212 | 392–691 |
| Date | date | 0 | 2,002 | 2015-01-01 → 2020-06-30 |
| CustomerID | text | 0 | 282,596 | 1–282,597 |
| CampaignID | number | 0 | 22 | 1–22 |
| Units | text | 0 | 1 | constant = 1 |

### 3.2 Customer
| Column | Type | Nulls | Distinct | Notes |
|--------|------|------:|---------:|-------|
| CustomerID | text | 0 | 282,597 | Primary key |
| ZipCode | text | 0 | 29,190 | leading zeros present (e.g. 06010) |
| Email Name | text | 0 | 250,857 | composite: `(email): Last, First ` |

### 3.3 Geo
| Column | Type | Nulls | Distinct | Notes |
|--------|------|------:|---------:|-------|
| Zip | text | 0 | 39,948 | Primary key; leading zeros (e.g. 08872) |
| City | text | 0 | 28,575 | "City, ST, USA" |
| State | text | 0 | 49 | |
| Region | text | 0 | 3 | East, Central, West |
| District | text | 0 | 39 | "District #NN" |
| Country | text | 0 | 1 | constant = USA |

### 3.4 Date
| Column | Type | Nulls | Distinct | Range |
|--------|------|------:|---------:|-------|
| Date | date | 0 | 2,192 | 2016-01-01 → 2021-12-31 |
| MonthNo | number | 0 | 12 | 1–12 |
| MonthName | text | 0 | 12 | Jan–Dec |
| MonthID | number | 0 | 72 | 201601–202112 |
| Month | text | 0 | 72 | Jan-21 … |
| Quarter | text | 0 | 4 | Q1–Q4 |
| Year | number | 0 | 6 | 2016–2021 |

### 3.5 Product
| Column | Type | Nulls | Distinct | Range / Notes |
|--------|------|------:|---------:|---------------|
| ProductID | text | 0 | 212 | 392–691 (PK) |
| Product | text | 0 | 173 | product names |
| Category | text | 0 | 5 | Urban, Mix, Accessory, Youth, Rural |
| Segment | text | 0 | 9 | Productivity, Select, … |
| ManufacturerID | text | 0 | 1 | constant = 7 |
| Manufacturer | text | 0 | 1 | constant = VanArsdel |
| Unit Cost | number | 0 | 162 | 15.31 – 149.46 |
| Unit Price | number | 0 | 162 | 20.97 – 204.74 |

### 3.6 Campaign
| Column | Type | Nulls | Distinct | Values |
|--------|------|------:|---------:|--------|
| CampaignID | number | 0 | 22 | 1–22 (PK) |
| TrafficChannel | text | 0 | 8 | Organic Search, SEO, SEM, SMO, Banner, Affiliate, Email, Mail |
| Device | text | 0 | 5 | Desktop, Mobile, Tablet, (Deskop), (Paper) |

---

## 4. Relationships & Referential Integrity

| Relationship | Join path | Orphan keys (fact→dim) | Unused dim keys |
|--------------|-----------|------------------------|-----------------|
| Sales → Product | ProductID | 0 | 0 |
| Sales → Customer | CustomerID | 0 | 1 customer |
| Sales → Campaign | CampaignID | 0 | 0 |
| Sales → Date | Date | **364 dates → 112,202 rows (16.6%)** | 554 dates |
| Customer → Geo | ZipCode = Zip | 0 | 10,758 ZIPs |

**Notes**
- Geo is **not** linked directly to Sales; it is reached via `Sales → Customer.ZipCode → Geo.Zip`. Geographic analysis therefore reflects **customer location**.
- The Sales↔Date mismatch is caused by the Date dimension covering 2016–2021 while Sales spans 2015–mid-2020.

---

## 5. Budget / Forecast Profile

**Layout:** wide — one row per `Type × Year × Month`, with 9 columns each holding a `Category-Segment` revenue value. Requires unpivot (melt) into long form.

| Attribute | Value |
|-----------|-------|
| Types | Budget, Forecast |
| Coverage | Budget: 2020, 2021 (12 months each) · Forecast: 2021 (12 months) |
| Measure columns | 9 combined `Category-Segment` fields |
| Nulls / Duplicates | 0 / 0 |
| Category-Segment combos | 9 — all present in Actuals (Actuals has 1 extra: `Rural-Productivity`) |
| Dimensions available | Category, Segment, Year, Month — **Revenue only** |
| Dimensions NOT available | Region, Product, Traffic Channel, Device, Cost/Profit, Customer |

**Totals (after unpivot):**
| Type | Year | Total Amount |
|------|------|-------------:|
| Budget | 2020 | 11,170,059 |
| Budget | 2021 | 10,986,388 |
| Forecast | 2021 | 10,991,884 |

---

## 6. Key Observations

- **Completeness:** no missing values in any column of any table.
- **Constant (single-value) columns:** `Sales.Units` (=1), `Product.Manufacturer` (=VanArsdel), `Product.ManufacturerID` (=7), `Geo.Country` (=USA).
- **Data types (as stored):** most ID fields and ZIP codes are stored as **text**; `CampaignID` and numeric measures are stored as numbers; dates are proper date types.
- **Cardinality highlights:** Customer 282,597 (250,857 distinct Email Names); Geo 39,948 ZIPs; Product 212; Category 5; Segment 9; Region 3.
- **Time coverage:** Actual sales Jan 2015 – Jun 2020 (5.5 years; 2015–2019 full, 2020 partial through June); Date dimension 2016–2021; Budget/Forecast 2020–2021. Comparable Actual-vs-Budget overlap: **2020 (Jan–Jun)**.
- **Value integrity:** `Unit Price ≥ Unit Cost` for all 212 products (no negative-margin products).

*Issues arising from these observations are evaluated in the Data Quality Assessment.*
