| Document | Report Spec |
|----------|-------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.0 |
| Status | Draft |
| Last Updated | 2026-07-18 |

# Report spec — VanArsdel Retail Dashboard

## Purpose & audience
- **Audience:** CEO/COO, Sales Director, Sales Manager, Finance Manager, Marketing Manager, Regional Sales Manager (per BRD §3)
- **Decisions supported:** see BRD §3 — one row per stakeholder
- **Refresh / latency:** **Monthly.** (Not daily — source is a periodic export of Actuals/Budget/Forecast files, not a live transactional feed; daily refresh would show no new data most days.)
- **Default view on open:** Year = 2020 (most recent year present in Actuals, though partial —
  Jan–Jun only), all Regions/Categories selected. Chosen because it's the most current complete
  picture available; document this as a BA default pending stakeholder confirmation.

---

## Key questions
| # | Question | Answered on |
|---|----------|-------------|
| 1 | Revenue, Units, Gross Profit, Gross Margin trend over time | Page 1, Page 2 |
| 2 | Which Region/Category/Segment/Product contribute most revenue | Page 1, Page 2 |
| 3 | Actual vs Budget for the comparable period | Page 3 |
| 4 | Expected performance per 2021 Forecast | Page 3 |
| 5 | Best Traffic Channel × Device combinations | Page 4 |
| 6 | How many customers purchased in the period | Page 5 |
| 7 | How many customers are newly acquired | Page 5 |
| 8 | Best/worst products and regions | Page 1, Page 2 |
| 9 | MoM / YoY change | Page 2 |

---

## KPIs
| KPI | Measure | Notes |
|-----|---------|-------|
| Total Revenue | `[Total Revenue]` | vs. prior period, no fixed target |
| Units Sold | `[Total Units]` | vs. prior period |
| Total COGS | `[Total COGS]` | — |
| Gross Profit | `[Gross Profit]` | — |
| Gross Margin % | `[Gross Margin %]` | ≥30% = healthy (proposed) |
| Budget Attainment % | `[Budget Attainment %]` | ≥100% On Track · 90–99% Watch · <90% At Risk (proposed); valid Jan–Jun 2020 only |
| Budget Variance | `[Budget Variance]` | 0 = on plan |
| Forecast Amount | `[Forecast Amount]` | reference only, no target (2021, no Actuals to compare) |
| Total Customers | `[Total Customers]` | vs. prior period |
| New Customers | `[New Customers]` | vs. prior period; Jan-2015 overstates "new" (no pre-2015 purchase history to check against) |
| MoM Revenue Growth % | `[MoM Revenue Growth %]` | > 0% |
| YoY Revenue Growth % | `[YoY Revenue Growth %]` | > 0% |

## Grain & dimensions
- **Fact grain:** one Sales row = one product sold to one customer on one date with one campaign attribution (no Order/Basket ID — order-level analysis out of scope).
- **Slicers / dimensions:** Date (Year/Quarter/Month), Region, District, Category, Segment, Product, Traffic Channel, Device, Customer.

## Pages

### Page 1 — Executive Overview *(built)*
KPI cards: Total Revenue, Gross Profit, Gross Margin %, Total Units Sold, Total Customers, Budget Attainment % (H1 2020).
Trend: Revenue by month, full history 2015 → H1 2020 (line); Gross Margin % by month.
Breakdown: Revenue by Region; Revenue by Category; Revenue by Traffic Channel.
Detail table: Top 10 Products by Revenue (tie-break: Units desc, then name A–Z); Top 10 Regions by Revenue (only 3 regions exist — East/Central/West).

### Page 2 — Sales Performance *(built)*
KPI cards: Revenue, Units Sold, Gross Profit, Gross Margin %, MoM Revenue Growth %, YoY Revenue Growth %.
Trend: Monthly Revenue; Monthly Units.
Breakdown: Revenue by Region; Revenue by Category-Segment; Revenue by Product (Top 10, same tie-break as Page 1).
Detail: Product performance ranking table (Revenue, Units, Gross Margin % by Product); Category-Segment matrix (rows = Category, columns = Segment, values = Revenue).

### Page 3 — Budget & Forecast Analysis *(built)*
KPI cards: Actual Revenue (H1 2020), Budget Amount (H1 2020), Budget Variance, Budget Attainment %.
Trend: Actual vs. Budget, monthly, Jan–Jun 2020 only — Actual (solid) vs Budget (dashed), never blended into one series.
Trend (reference only): Forecast 2021, monthly — separate chart, labeled "No Actuals available for this period — reference only."
Breakdown: Budget Attainment % by Category-Segment (bar) — excludes Rural-Productivity (no Budget column exists for this group in source data; shown as a one-line callout).
Detail table: Category-Segment performance — Actual, Budget, Variance, Attainment %, restricted to Jan–Jun 2020.
Notes: "Budget Variance by Region" removed — not buildable (Budget/Forecast source has no Region field, only Type/Year/Month/Category-Segment). Forecast never plotted on the same axis/series as Actual — no overlapping period exists.

### Page 4 — Marketing Performance *(built)*
KPI cards: Revenue from Campaign Attribution (= `[Total Revenue]` filtered by Campaign context), Customers Acquired (= `[Total Customers]` in same context).
Breakdown: Revenue by Traffic Channel; Revenue by Device; Revenue by Traffic Channel × Device (matrix or stacked bar).
Detail table: Channel performance ranking (Traffic Channel × Device, Revenue, Units, Customers, sorted desc by Revenue).
Notes: revenue attribution only — Marketing ROI/CAC out of scope (no campaign cost data).

### Page 5 — Customer Analysis *(built)*
KPI cards: Total Customers, New Customers.
Breakdown: Customers by Region (distinct CustomerID count, by Geo.Region); Revenue by Customer (Top 10, same tie-break as Page 1).
Trend: Customer growth over time (distinct CustomerID by month).
Notes: Jan-2015 will show an artificial spike in "new customers" — every active customer that month reads as new, since there's no pre-2015 data to check against. Label with a footnote rather than removing it.

## Filters
- Report-level: Date, Region, Category, Segment.
- Page-level (Sales Performance): Product, Category, Segment. (Marketing Performance): Traffic Channel, Device. (Customer Analysis): Region, District.

## Interactivity
- Filtering / cross-filtering: standard, all pages.
- Drill-down: Category → Segment → Product hierarchy; Region → District hierarchy.
- Drill-through: from any Product/Region/Channel visual to a single-entity detail page — required (Sales Manager and Regional Sales Manager personas both need entity-level drill-through to act on the dashboard).
- Export: enabled, subject to user role permissions (apply Power BI workspace-level role security, not a data model concern).

## Design notes
- Theme `shared/themes/datapot-theme.json` (registered). Page 1280×720. Format via theme, not per-visual.
- Currency: USD, explicit label on every monetary visual.
- Percentages: 1 decimal place. Numbers: thousands separator.
- Color meaning: positive variance/growth = positive indicator color; negative = warning indicator color — consistent across all pages.
- Any visual mixing Actual with Budget or Forecast uses visibly distinct series styling (solid vs. dashed) — they never share a continuous time window in this dataset.
