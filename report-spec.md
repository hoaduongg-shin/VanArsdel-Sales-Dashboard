| Document | Report Spec |
|----------|-------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.1 |
| Status | Draft |
| Last Updated | 2026-07-19 |

# Report spec — VanArsdel Retail Dashboard

## Purpose & audience
- **Audience:** CEO/COO, Finance Manager, Sales Director, Sales Manager, Regional Sales Manager, Marketing Manager, Product Development (per BRD §3)
- **Decisions supported:** see BRD §3 — one row per stakeholder
- **Refresh / latency:** **Monthly.** (Not daily — source is a periodic export of Actuals/Budget/Forecast files, not a live transactional feed; daily refresh would show no new data most days.)
- **Default view on open:** Anchor month = Jun 2020 (latest month present in Actuals; 2020 is partial —
  Jan–Jun only). Page 1 opens on the rolling 13-month window ending at the anchor; all
  Categories/Segments selected. Chosen because it's the most current picture available; document this
  as a BA default pending stakeholder confirmation.

---

## Key questions
| # | Question | Answered on |
|---|----------|-------------|
| 1 | Revenue, Units, Gross Profit, Gross Margin trend over time | Page 1, Page 2 |
| 2 | Which Region/Category/Segment/Product contribute most revenue | Page 1, Page 2 |
| 3 | Actual vs Budget for the comparable period (overall & by Category) | Page 1 |
| 4 | Expected performance per 2021 Forecast | Page 1 |
| 5 | Which Traffic Channels contribute most revenue and orders | Page 3 |
| 6 | How many customers purchased in the period | Page 3 |
| 7 | How many customers are newly acquired | Page 3 |
| 8 | Best/worst products and regions (with prior-year comparison) | Page 2 |
| 9 | MoM / YoY change | Page 1, Page 2 |
| 10 | Which customer tiers contribute the most gross profit | Page 3 |

---

## KPIs
| KPI | Measure | Notes |
|-----|---------|-------|
| Total Revenue | `[Total Revenue]` | vs. prior period, no fixed target |
| Units Sold | `[Total Units]` | vs. prior period; Units = 1 per record, so equals transaction lines |
| Total COGS | `[Total COGS]` | — |
| Gross Profit | `[Gross Profit]` | Actual only — Budget has no cost |
| Gross Margin % | `[Gross Margin %]` | ≥30% = healthy (proposed); baseline observed ≈27% |
| Average Selling Price | `[ASP]` | Revenue ÷ Units Sold (Page 2) |
| Budget Attainment % | `[Budget Attainment %]` | ≥100% On Track · 90–99% Watch · <90% At Risk (proposed); valid Jan–Jun 2020 only |
| Budget Variance | `[Budget Variance]` | 0 = on plan |
| Forecast Amount | `[Forecast Amount]` | reference only, no target (2021, no Actuals to compare) |
| Total Customers | `[Total Customers]` | vs. prior period |
| New Customers | `[New Customers]` | vs. prior period; Jan-2015 overstates "new" (no pre-2015 purchase history to check against) |
| Repeat Purchase Rate % | `[Repeat Purchase Rate %]` | customers with ≥2 purchases ÷ total customers (Page 3) |
| Average Purchase Frequency | `[Avg Purchase Frequency]` | total orders ÷ total customers (Page 3) |
| Customer Profitability Tier | `[Customer Gross Profit]` ranked into tiers | Top 10% / next 20% / remaining, by cumulative gross profit (Page 3) |
| MoM Revenue Growth % | `[MoM Revenue Growth %]` | > 0% |
| YoY Revenue Growth % | `[YoY Revenue Growth %]` | > 0% |

## Grain & dimensions
- **Fact grain:** one Sales row = one product sold to one customer on one date with one campaign attribution (no Order/Basket ID — order-level analysis out of scope).
- **Slicers / dimensions:** Date (Year/Quarter/Month), Region, District, Category, Segment, Product, Traffic Channel, Device, Customer.
- **Budget/Forecast grain:** Category × Segment × Month, Revenue only — no Region/Product/Channel/Cost. Actual-vs-Budget is therefore limited to Revenue at Category/Segment/Month.

## Pages

### - Page 1 — Overview: Sales & Finance *(spec)*
KPI cards: Total Revenue, Total COGS, Gross Profit, Budget Attainment % (H1 2020).

Trend: Actual vs Budget vs Forecast revenue — rolling 13 months, anchored on selected period (line); Gross Profit + Gross Margin % — rolling 12 months, Actual only (line, dual axis).

Breakdown: Revenue → COGS → Gross Profit (funnel); Budget Attainment % by Category (column = Revenue, line = Attainment %).

Detail table: Actual vs Budget by Category-Segment (Actual, Budget, Variance, Attainment %) — restricted to the comparable period (2020).

Notes: rolling window adapts to grain (Month → rolling months, Quarter → rolling quarters). Actual (solid) vs Budget (dashed) vs Forecast (dashed, separate) — never blended into one series. "Budget by Region" not buildable (Budget source has no Region field). Region removed from Page 1 filters so Budget Attainment stays comparable.

### - Page 2 — Performance & Geography *(spec)*
KPI cards: Total Units Sold, Total Revenue, Average Selling Price per Unit.

Trend: Monthly Revenue; MoM / YoY revenue growth.

Breakdown: Revenue by Region (bar); Top 10 Products by Revenue (bar; tie-break: Units desc, then name A–Z).

Detail: Regional performance matrix — drill-down Region → District (→ City); columns = Revenue, Gross Profit, Gross Margin %, Prior-Year Revenue.

Notes: geography reflects customer location (Sales → Customer.ZipCode → Geo). Layout: detail matrix on one half, the two bar charts on the other half.

### - Page 3 — Customer & Behavior *(spec)*
KPI cards: Total Customers, New Customers, Repeat Purchase Rate %, Average Purchase Frequency.

Trend: New vs Returning customers over time (clustered column, monthly).

Breakdown: Revenue & Orders by Traffic Channel (clustered column, dual axis); Customers by Segment (bar).

Detail table: Customer Profitability Tiering — customers ranked by cumulative gross profit into tiers (Top 10% / next 20% / remaining) with % of gross profit contributed (column or ranked table).

Notes: Device is a filter, not a chart. Customers-by-Segment counts a customer in every Segment purchased (may appear in multiple). Jan-2015 shows an artificial spike in "new customers" (no pre-2015 history) — label with a footnote rather than removing it.

## Filters
- Report-level: Date (Year/Quarter/Month + range).
- Page-level (Overview): Category, Segment. (Performance & Geography): Region, District, Category, Segment, Product. (Customer & Behavior): Traffic Channel, Device.

## Interactivity
- Filtering / cross-filtering: standard, all pages.
- Drill-down: Category → Segment → Product hierarchy; Region → District (→ City) hierarchy.
- Drill-through: from any Product/Region/Channel visual to a single-entity detail page — required (Sales Manager and Regional Sales Manager personas both need entity-level drill-through to act on the dashboard).
- Rolling window (Page 1): selecting an anchor period drives the rolling 12/13-month trend; KPI cards reflect the selected period only.
- Export: enabled, subject to user role permissions (apply Power BI workspace-level role security, not a data model concern).

## Design notes
- Theme `theme.json` (registered). Page 1280×720. Format via theme, not per-visual.
- Currency: USD, explicit label on every monetary visual.
- Percentages: 1 decimal place. Numbers: thousands separator.
- Color meaning: positive variance/growth = positive indicator color; negative = warning indicator color — consistent across all pages.
- Any visual mixing Actual with Budget or Forecast uses visibly distinct series styling (solid vs. dashed) — they never share a continuous time window in this dataset.
- Rolling 12/13-month trends (Page 1) anchor on the selected period and adapt to the chosen time grain; the Revenue → COGS → Gross Profit funnel replaces a plain profit breakdown to show value conversion.
