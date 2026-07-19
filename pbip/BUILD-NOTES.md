# VanArsdel Sales — PBIP build notes

Generated PBIP (TMDL semantic model + PBIR report) from `report-spec.md` / `wireframe.md`,
built on the **real** dataset in `dataset.rar` (extracted to `../dataset/dataset/`).

## How to open
1. Open `VanArsdel Sales.pbip` in **Power BI Desktop** (enable *Preview features → Power BI Project (.pbip) save format* if not already on).
2. On first open the model loads the prepared CSVs in `pbip/data/` via the **`DataFolder`** query parameter
   (currently an absolute path). If you move the folder, update `DataFolder` in *Transform data → Manage parameters*.
3. Refresh. Report has 3 pages.

## What was built
- **Semantic model** (`VanArsdel Sales.SemanticModel`, TMDL): star schema — fact `Sales` → `Product`, `Customer`, `Campaign`, `Date`; `Customer`→`Geo`; disconnected `BudgetForecast` and `P&L Stage`; measure table `Key Measures` (26 measures across 4 folders).
- **Report** (`VanArsdel Sales.Report`, PBIR): 3 pages, 30 visuals, theme `Datapot.json`. Validates clean (`powerbi-report-author validate` → 0 errors / 0 warnings).

## Data-prep decisions applied (per data-quality.md remediation + stakeholder answers)
| Decision | Action |
|---|---|
| Currency | USD (labelled) |
| Date dim | Regenerated continuous daily **2015-01-01 → 2021-12-31** (fixes 2015 = 16.6% of sales dropping) |
| Duplicates | 133 fully-duplicate Sales rows (0.02%) removed |
| OrderID | Surrogate added, **1 row = 1 order** (`Total Orders = DISTINCTCOUNT(Order ID)`) |
| ZIP | Kept as text (leading zeros preserved) |
| Campaign | TRIM; `Affliliate→Affiliate`, `Deskop→Desktop`; **`Mail`+`Paper` kept distinct** from `Email` (postal channel) |
| Budget/Forecast | Unpivoted long, split Category + Segment, MonthID built |
| New Customer | `MIN(OrderDate)` per customer within dataset + Jan-2015 footnote (left-censoring) |
| Customer tiering | `Customer GP` + `Customer Tier` precomputed (lifetime/historical, per R-10) |

Verified totals: Revenue **$65,534,436** · COGS $47,840,138 · Gross Profit $17,694,298 · GM% **27.0%** · 2020 Budget $11,170,059.

## Page → visual map (matches wireframe.md)
- **P1 Overview:** KPI card (Revenue/COGS/GP/Attainment%) · line Actual/Budget/Forecast · combo GP+Margin · **funnel Revenue→COGS→GP** (via `P&L Stage`) · combo Attainment% by Category · table Actual-vs-Budget by Cat-Seg (2020 filter).
- **P2 Performance:** KPI card (Units/Revenue/ASP) · bar Revenue by Region · bar Top Products · **matrix Region→District→City** (Revenue, GP, GM%, Prior-Year).
- **P3 Customer:** KPI card (Customers/New/Repeat%/Freq) · column New vs Returning · combo Revenue & Orders by Channel · bar Customers by Segment · column Profitability Tiering + footnote.

## Known limitations / pending (require the real dataset the stakeholder will supply, or Desktop)
1. **Budget Attainment & 3-series trend** are only meaningful **Jan–Jun 2020** (Actuals ≤ Jun-2020; Budget 2020–21; Forecast 2021 never overlaps Actuals). Not fabricated — awaiting the supplemental data you mentioned.
2. **Rural-Productivity** has no budget → blank in the Attainment table (expected, G02).
3. **Rolling 13/12-month anchor** is implemented as the **Date-range slicer** window (a true disconnected-anchor rolling window is a follow-up).
4. **Top 10 Products** is sorted desc by Revenue; apply the *Top N = 10* visual filter in Desktop (measure-based Top-N can't be hand-authored safely in PBIR).
5. **Drill-through** entity page (spec §Interactivity) — designed, not yet built.
6. **TMDL compile + visual render** not verified here (no Power BI Desktop / Node bridge in this environment) — open the `.pbip` in Desktop to confirm.

## Gap-questions resolved by the real data (good news)
- **G11** Geo has Region→District→City (39 districts, 28,575 cities) → drill-down matrix fully buildable.
- **G13** 100% of Sales rows have a CampaignID → channel/device coverage is complete (no "Unknown/Direct" bucket).
