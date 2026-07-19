# VanArsdel Retail Dashboard

A Business Analyst case study: turning raw multi-file sales data from **VanArsdel** into a
consolidated, decision-ready **3-page dashboard** (Power BI). This repository holds the full
analysis and design trail — from understanding the data to the final report specification.

---

## Approach

The work follows a standard BA workflow. Each stage produced one document, and each feeds the next:

1. **Understand the data** — profile every table (structure, types, keys, relationships).
2. **Assess quality** — evaluate the data against quality dimensions and log every issue.
3. **Clarify gaps** — turn unknowns and blockers into questions for stakeholders.
4. **Define requirements** — capture what the business needs (KPIs, questions, scope).
5. **Specify & design the report** — pin down pages, KPIs, visuals, filters, then wireframe them.
6. **Reference throughout** — a shared glossary and data dictionary keep terms and fields consistent.

**Source data:** `VanArsdel_Actuals.xlsx` (star schema — 1 fact `Sales` + 5 dimensions) and
`VanArsdel_Budget_Forecast.xlsx` (planning data), packaged in `dataset.rar`.

---

## Documents & recommended reading order

| # | File | What it covers |
|---|------|----------------|
| 0 | **README.md** | You are here — approach and reading order. |
| 1 | **data-profile.md** | What the data looks like: tables, fields, types, cardinality, ranges, relationships. Descriptive, no judgement. |
| 2 | **data-quality.md** | Issues found, rated by severity across quality dimensions, with evidence, impact, and remediation. |
| 3 | **data-gap-questions.md** | Open items and blockers turned into questions for stakeholders. |
| 4 | **brd.md** | Business Requirements Document — objectives, stakeholders, KPIs, business rules, scope. |
| 5 | **report-spec.md** | Report specification — pages, KPI measures, grain, filters, interactivity, design notes. |
| 6 | **wireframe.md** | Low-fidelity layout of the 3 pages, with X/Y axis annotations per visual. |
| — | **business-glossary.md** | Shared business terms and definitions (reference, used throughout). |
| — | **data-dictionary.md** | Field-level definitions of every source column (reference, used throughout). |

> Read 1 → 6 in order to follow the reasoning. Use the glossary and data dictionary as reference at any point.

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

## Repository contents

| File | Type |
|------|------|
| `README.md` | This overview |
| `data-profile.md`, `data-quality.md`, `data-gap-questions.md` | Data understanding & quality |
| `brd.md`, `report-spec.md`, `wireframe.md` | Requirements & design |
| `business-glossary.md`, `data-dictionary.md` | Reference |
| `theme.json` | Power BI report theme |
| `dataset.rar` | Source data (Actuals + Budget/Forecast) |

---

## Status

Draft — pending stakeholder confirmation on the open items in `data-gap-questions.md`
(revenue formula, historical/finer budgets, customer definition, and data currency/PII).
