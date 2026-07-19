| Document | Data Quality Assessment |
|----------|-------------------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.0 |
| Status | Draft |
| Author | Hoa Duong |
| Last Updated | 2026-07-19 |

# Data Quality Assessment — VanArsdel

## 1. Purpose

This document evaluates the VanArsdel source data against standard data-quality dimensions, rates each issue by severity, provides evidence and business impact, and defines the remediation required before the dashboard is built. It complements the **Data Profiling Report** (facts) and the **BRD** (requirements).

---

## 2. Quality Dimensions Used

| Dimension | Question answered |
|-----------|-------------------|
| Completeness | Are values missing? |
| Validity / Conformity | Do values follow the expected type, format, and allowed set? |
| Consistency | Are values consistent across fields/tables? |
| Uniqueness | Are records / keys free of unintended duplication? |
| Integrity | Do relationships between tables hold (no orphans / gaps)? |
| Accuracy | Do values reflect reality? |
| Timeliness / Coverage | Does the data cover the required periods? |
| Relevance | Are all fields useful for analysis? |

---

## 3. Quality Scorecard (summary)

| Dimension | Status | Comment |
|-----------|--------|---------|
| Completeness | 🟢 Good | 0 missing values across all tables |
| Validity / Conformity | 🟠 Attention | dirty Campaign values; ZIP-as-number risk |
| Consistency | 🟠 Attention | ID types mixed (text vs number) |
| Uniqueness | 🔴 Action | 133 duplicate sales rows; undefined fact grain |
| Integrity | 🔴 Action | Date dimension missing 2015 → 16.6% of sales unlinked |
| Accuracy | 🟢 Good* | Price ≥ Cost for all products (*subject to synthetic-data caveat) |
| Timeliness / Coverage | 🟠 Attention | Actual/Budget/Date periods misaligned |
| Relevance | 🟡 Minor | constant columns add no analytical value |

**Overall:** usable after remediation. 3 High-severity issues must be fixed before build.

---

## 4. Findings Register

Severity: 🔴 High · 🟠 Medium · 🟡 Low

| ID | Dimension | Finding | Severity | Evidence | Business Impact | Recommendation |
|----|-----------|---------|:--------:|----------|-----------------|----------------|
| **DQ-01** | Integrity / Timeliness | Date dimension does not cover 2015; Sales spans 2015→Jun 2020 but Date dim is 2016→2021. | 🔴 | 364 sales dates absent from Date dim = **112,202 rows (16.6%)** lost on join; 554 Date-dim dates unused. | Time-based analysis silently drops 2015 (16.6% of sales). | Extend Date dimension back to 2015-01-01 (continuous). |
| **DQ-02** | Uniqueness | 133 fully-duplicated rows in Sales. | 🔴 | `duplicated()` = 133 (265 rows involved). | Double-counts revenue/orders. | Investigate source; de-duplicate or add a transaction/line ID. |
| **DQ-03** | Validity / Conformity | ZIP stored as text with leading zeros — will corrupt if cast to number. | 🔴 | 1,921 Geo ZIPs and many Customer ZIPs start with "0" (e.g. 08872, 06010). | Casting to number breaks 1,900+ ZIPs → wrong Customer↔Geo joins & geography. | Keep Zip/ZipCode as **text** at every load step. |
| **DQ-04** | Uniqueness | Sales fact grain is undefined — no unique key. | 🟠 | No combination of the 4 keys is unique; duplicates per DQ-02. | Cannot validate "1 row = 1 event"; weak dedupe control. | Agree grain (e.g. 1 row = 1 order line) + add surrogate/transaction key. |
| **DQ-05** | Validity / Conformity | `TrafficChannel` contains dirty values. | 🟠 | `'Organic Search  '`, `'Affliliate  '` (trailing spaces), `'Affliliate'` (typo), `'Mail'` vs `'Email'`. | Group-by creates false duplicate channels; fragmented marketing analysis. | TRIM; fix `Affliliate→Affiliate`; confirm/merge `Mail`↔`Email`. |
| **DQ-06** | Validity / Conformity | `Device` contains dirty/invalid values. | 🟠 | `'Deskop'` (typo of Desktop); `'Paper'` (not a device). | Desktop split into two groups; invalid "Paper" in device analysis. | Fix `Deskop→Desktop`; resolve `Paper` (map or exclude). |
| **DQ-07** | Consistency | ID data types are inconsistent. | 🟠 | ProductID/CustomerID/ManufacturerID stored as text; CampaignID as number. | Join risk if the two sides differ; no numeric ops on IDs. | Standardize each ID type consistently on both fact and dimension sides. |
| **DQ-08** | Conformity / Atomicity | `Email Name` combines several fields. | 🟠 | Format `(email): Last, First ` with trailing space; 100% start with "(". | Hard to filter/group by Email, Last, First; trailing space breaks matching. | Split into Email / LastName / FirstName; TRIM. |
| **DQ-09** | Relevance | Constant (single-value) columns. | 🟡 | `Units`=1, `Manufacturer`=VanArsdel, `ManufacturerID`=7, `Country`=USA. | No analytical value; add model weight. | Hide/exclude or document as constants; keep Units numeric if used in sums. |
| **DQ-10** | Uniqueness | Possible duplicate customers. | 🟡 | 250,857 distinct Email Names vs 282,597 CustomerIDs (~31,740 gap). | May inflate unique-customer counts. | Investigate; agree a "unique customer" definition. |
| **DQ-11** | Integrity (info) | Dimension rows with no fact activity. | 🟡 | 554 Date dates, 1 customer, 10,758 ZIPs unused. | Normal for lookups; no data loss. | No action; be aware in default filters. |

### Budget / Forecast — additional findings

| ID | Dimension | Finding | Severity | Impact / Recommendation |
|----|-----------|---------|:--------:|--------------------------|
| **DQ-12** | Conformity (structure) | Budget/Forecast in wide layout with combined `Category-Segment` field. | 🟠 | Must unpivot to long and **split into Category + Segment** before use (enables Category/Segment filtering & Actual-vs-Budget by Category). |
| **DQ-13** | Completeness (plan) | Budget covers only 2020–2021; Forecast only 2021. | 🟠 | No plan for 2015–2019 → Actual-vs-Budget only comparable in **2020**. Confirm whether historical budgets exist. |
| **DQ-14** | Completeness (plan) | Budget contains **Revenue only** (no Cost). | 🟠 | Cannot compute planned Gross Profit/Margin → those visuals are Actual-only. Request Budget cost if plan comparison of profit is required. |
| **DQ-15** | Coverage (plan grain) | Budget grain = Category × Segment × Month. | 🟠 | No Budget by Region / Product / Channel → plan comparison unavailable on Pages 2 & 3. Confirm if finer budgets exist. |
| **DQ-16** | Completeness (plan) | Missing budget line for `Rural-Productivity`. | 🟡 | This Actual combo has no target → excluded from attainment. Confirm expected. |

---

## 5. Remediation Plan (before dashboard build)

| Priority | Action | Resolves |
|:--------:|--------|----------|
| 1 | Extend Date dimension to 2015 (continuous daily calendar) | DQ-01 |
| 2 | Investigate & remove 133 duplicate Sales rows | DQ-02, DQ-04 |
| 3 | Enforce Zip/ZipCode as **text** at load | DQ-03 |
| 4 | Standardize Campaign values (TRIM, Affiliate, Desktop; resolve Paper/Mail) | DQ-05, DQ-06 |
| 5 | Unpivot Budget/Forecast → long; split Category + Segment; build month key | DQ-12 |
| 6 | Standardize ID data types across fact & dimensions | DQ-07 |
| 7 | Split `Email Name`; TRIM | DQ-08 |
| 8 | Derive `New/Returning` flag + first-purchase date | (enables BR06) |
| 9 | Document/hide constant columns | DQ-09 |

---

## 6. Data-Quality Questions for Stakeholders

1. Revenue formula — is `Revenue = Units × Unit Price` correct? Any discount/tax? (currency = USD?)
2. Are the 133 duplicate rows genuine repeat transactions or ingestion errors? Is there a source Order/Transaction ID?
3. What is the official reporting period — include 2015, or start 2016?
4. Definition of a "unique customer" (by CustomerID, Email, or name)? (DQ-10)
5. Standard allowed values for TrafficChannel & Device? Is `Mail` = `Email`? What is `Paper`? (DQ-05/06)
6. Do historical budgets (2015–2019) exist? (DQ-13)
7. Is Budget Cost / Profit available, to compare profit vs plan? (DQ-14)
8. Are finer budgets (by Region / Product / Channel) available? (DQ-15)
9. Is `Rural-Productivity` expected to have a budget target? (DQ-16)
10. Are the customer emails real PII, and what are the security/masking requirements?
11. Note: extremely even customer-value distribution and "0 customers with ≥5 orders" suggest the data may be sampled/synthetic — please confirm before drawing business conclusions.

---

## 7. Related Documents
- Data Profiling Report (`VanArsdel_Data_Profiling.md`)
- Business Requirements Document (`VanArsdel_BRD.md`)
- Data Gap Register (`VanArsdel_DataGap.xlsx`)
