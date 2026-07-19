# Data Gaps & Stakeholder Questions

This document records data gaps, ambiguities, and assumptions identified during data profiling.

Its purpose is to:
- Highlight missing or incomplete information that may affect reporting.
- Capture questions requiring stakeholder clarification.
- Record proposed handling before the semantic model is finalized.

> **Scope**
>
> This document focuses on business-related data gaps and assumptions.
> Data quality issues (e.g. typos, invalid data types, formatting inconsistencies) are documented separately in `data-quality.md`.

| ID | Table | Column | Observation | Evidence | Business Impact | Question for Stakeholder | Proposed Resolution |
|----|-------|--------|-------------|----------|-----------------|--------------------------|---------------------|
| G01 | Date | Date | The Date dimension does not cover the full Sales transaction period. | Date table starts on 2016-01-01; Sales contains transactions from 2015-01-01. | Time Intelligence calculations for 2015 may be incomplete or inaccurate. | — | Extend the Date dimension to include the complete transaction history. |
| G02 | BudgetForecast | Category-Segment | No Budget exists for the **Rural–Productivity** product group. | No corresponding Budget record exists after unpivoting the source file. | Budget KPIs cannot be calculated for this product group. | Is this intentionally excluded, or is Budget data missing? | Leave Budget as NULL and exclude from Budget comparisons until confirmed. |
| G03 | Product | Unit Price, Unit Cost | Only one Unit Price and Unit Cost exist for each product across all years. | No historical pricing table is provided. | Historical Revenue and Gross Profit assume prices remained constant over time. | Should prices be treated as constant throughout the reporting period? | Use the available values and document the assumption. |
| G04 | Sales | Order ID | Sales records do not contain an Order ID. | No OrderID field exists in the Sales dataset. | Order-level KPIs (e.g. Orders, Average Order Value) cannot be calculated reliably. | Does each Sales row represent an Order or an Order Line? | Treat each Sales row as one sales line until confirmed otherwise. |
| G05 | Customer | Customer | Customer information is limited to ID, ZIP Code and Email Name. | No registration date, customer segment or demographic attributes are available. | Customer segmentation and lifecycle analysis cannot be performed. | Is a Customer Master dataset available? | Limit customer reporting to transaction-based metrics. |
| G06 | Campaign | Marketing Cost | Marketing attribution is available, but campaign cost data is not. | No spend, impressions or clicks are included in the dataset. | Marketing efficiency metrics such as ROI, ROAS, CAC and CPA cannot be calculated. | Is marketing spend data available from another source? | Limit reporting to revenue attribution by marketing channel. |
| G08 | Sales | Currency | The reporting currency is not explicitly provided. | No Currency field exists in the source files. | Financial metrics may be interpreted incorrectly if the assumed currency is wrong. | Should all financial values be treated as USD? | Confirm the reporting currency and document it in the Business Glossary. |
| G09 | BudgetForecast | Budget/Forecast | The dataset includes both a Budget and a Forecast. | There are two types of planning data. | Different KPIs can be developed. | Should performance evaluation KPIs be compared to Budget, Forecast, or both? | Design KPIs based on business requirements. |
