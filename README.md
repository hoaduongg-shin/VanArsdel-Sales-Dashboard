# VanArsdel Retail Dashboard

A Business Analyst case study: turning raw multi-file sales data from **VanArsdel** into a
consolidated, decision-ready **3-page dashboard** (Power BI). This repository holds the full
analysis and design trail — from understanding the data to the final report specification.

---

## Approach

The work follows a standard BA workflow. Each stage produced one document, and each feeds the next:

1. **Understand the data** — profile every table (structure, types, keys, relationships).
2. **Assess quality** — evaluate the data against quality dimensions and log every issue.
3. **Establish shared vocabulary** — define business terms (glossary) and every source field (data dictionary).
4. **Clarify gaps** — turn unknowns and blockers into questions for stakeholders.
5. **Define requirements** — capture what the business needs (KPIs, questions, scope).
6. **Specify & design the report** — pin down pages, KPIs, visuals, filters, then wireframe them.

**Source data:** `VanArsdel_Actuals.xlsx` (star schema — 1 fact `Sales` + 5 dimensions) and
`VanArsdel_Budget_Forecast.xlsx` (planning data), packaged in `dataset.rar`.

---

## Documents & recommended reading order

| # | File | What it covers |
|---|------|----------------|
| 0 | **README.md** | You are here — approach and reading order. |
| 1 | **data-profile.md** | What the data looks like: tables, fields, types, cardinality, ranges, relationships. Descriptive, no judgement. |
| 2 | **data-quality.md** | Issues found, rated by severity across quality dimensions, with evidence, impact, and remediation. |
| 3 | **business-glossary.md** | Shared business terms and their agreed definitions. |
| 4 | **data-dictionary.md** | Field-level definitions of every source column. |
| 5 | **data-gap-questions.md** | Open items and blockers turned into questions for stakeholders. |
| 6 | **brd.md** | Business Requirements Document — objectives, stakeholders, KPIs, business rules, scope. |
| 7 | **report-spec.md** | Report specification — pages, KPI measures, grain, filters, interactivity, design notes. |
| 8 | **wireframe.md** | Low-fidelity layout of the 3 pages, with X/Y axis annotations per visual. |
| 9 | **pbip/BUILD-NOTES.md** | The built Power BI project — how to open, data-prep decisions, verified totals, limitations. |

> Read 1 → 8 to follow the reasoning from raw data to final design; see 9 and the `pbip/` folder for the implementation.

---

## The dashboard at a glance

| Page | Audience | Focus |
|------|----------|-------|
| **1 — Overview: Sales & Finance** | CEO/COO, Finance | Revenue, cost, profit, and **Actual vs Budget/Forecast** (rolling 13/12-month trend, funnel, attainment by category). |
| **2 — Performance & Geography** | Sales & Regional managers | Product and regional performance with drill-down and prior-year comparison. |
| **3 — Customer & Behavior** | Marketing, Product Dev | Acquisition, repeat-purchase behavior, channel mix, and customer profitability tiering. |

---

## Key decisions

- **Budget comparison is Revenue-only, at Category × Segment × Month.** The plan data has no
  Region/Product/Channel/Cost, so Actual-vs-Budget lives only on Page 1; profit/margin are Actual-only.
- **Time grain adapts to audience:** Page 1 uses a rolling 13/12-month window; Pages 2–3 default to Month.
- **Geography reflects customer location** (Sales → Customer.ZipCode → Geo), not store/shipping location.
- **Data preparation required before build:** extend the Date dimension to 2015, remove duplicate sales
  rows, standardize campaign values, keep ZIP as text, and derive a New/Returning flag —
  see `data-quality.md` for the full remediation plan.

---

## Power BI build (`pbip/`)

The dashboard is implemented as a **Power BI Project (PBIP)** — a file-based semantic model
(TMDL) plus report (PBIR), built on the real dataset per `report-spec.md` and `wireframe.md`.

- **`pbip/VanArsdel Sales.SemanticModel`** — star-schema model in TMDL: fact `Sales` +
  dimensions, disconnected `BudgetForecast` and `P&L Stage`, and a `Key Measures` table
  (~26 DAX measures). Reads the cleaned data in `pbip/data/`.
- **`pbip/VanArsdel Sales.Report`** — 3-page PBIR report (theme `theme.json`), validated
  with the `powerbi-report-author` CLI (0 errors).
- **`pbip/data/`** — cleaned CSVs after the `data-quality.md` remediation (Date extended to
  2015, 133 duplicate rows removed, surrogate `OrderID`, campaign values standardized,
  Budget/Forecast unpivoted, New/Returning + customer tier derived).
- **`pbip/BUILD-NOTES.md`** — how to open, decision log, and verified totals
  (Revenue ≈ $65.5M · Gross Margin 27.0%).

**Open it:** open `pbip/VanArsdel Sales.pbip` in Power BI Desktop (enable *Preview features →
Power BI Project (.pbip) save format*), then **Refresh**.

### Known limitations & how to improve

| Limitation | Cause | How to improve |
|---|---|---|
| Budget Attainment & the Actual/Budget/Forecast trend are only meaningful for **Jan–Jun 2020** | Actuals end Jun-2020; Budget covers 2020–21; Forecast (2021) never overlaps Actuals | Obtain post-Jun-2020 Actuals and/or historical (2015–19) budgets to widen the comparable window |
| **Rolling 13/12-month anchor** is approximated by the Date-range slicer | A true anchored rolling window needs a disconnected date/anchor parameter | Add a disconnected date table + rolling DAX measures driven by a selected anchor month |
| **Top 10 Products** currently shows all products sorted by revenue | Measure-based Top-N can't be authored safely in raw PBIR | Apply a *Top N = 10 by Revenue* visual filter in Power BI Desktop |
| **Drill-through** to a single-entity page is not built yet | Time-boxed first build | Add a Product/Region drill-through detail page |
| Customer profitability tiering is near-even (weak VIP concentration) | Data appears **synthetic** (`data-quality.md` Q11) | Re-validate tiers once real customer-level data is available |

---

## Repository contents

| File | Type |
|------|------|
| `README.md` | This overview |
| `data-profile.md`, `data-quality.md` | Data understanding & quality |
| `business-glossary.md`, `data-dictionary.md` | Reference definitions |
| `data-gap-questions.md` | Stakeholder questions |
| `brd.md`, `report-spec.md`, `wireframe.md` | Requirements & design |
| `theme.json` | Power BI report theme |
| `dataset.rar` | Source data (Actuals + Budget/Forecast) |
| `pbip/` | Power BI Project — TMDL semantic model, PBIR report, cleaned `data/`, `BUILD-NOTES.md` |

---

## Status

**Build complete** — the PBIP (semantic model + 3-page report) is implemented on the real
dataset and validates with 0 errors. Still **Draft**: pending stakeholder confirmation on the
open items in `data-gap-questions.md` (revenue formula, historical/finer budgets, customer
definition, data currency/PII) and the enhancements listed under
[Power BI build → Known limitations](#known-limitations--how-to-improve).
