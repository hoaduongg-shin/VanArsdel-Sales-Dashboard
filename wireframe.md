| Document | Dashboard Wireframe |
|----------|---------------------|
| Project | VanArsdel Retail Dashboard |
| Version | 1.1 |
| Status | Draft |
| Author | Hoa Duong |
| Last Updated | 2026-07-19 |

# Dashboard Wireframe — VanArsdel Retail Dashboard

Low-fidelity layout for the three-page dashboard. Each visual is annotated with its **type**, **X axis**, and **Y axis**. Companion to the BRD and Report Spec.

**Legend**
- `X:` horizontal axis (analysis dimension) · `Y:` vertical axis (value)
- `[vs B/F]` visual compares against Budget/Forecast (only where Budget grain allows: Category / Segment / Month)
- Time grain on the X axis is set per audience (see each page header)

---

## Page 1 — Overview: Sales & Finance
**Audience:** CEO / COO · Finance — **X-axis time grain: rolling 13/12 months** (anchored on selected month; adapts to Month/Quarter)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ FILTERS: [Date range] [Anchor month] [Rolling 13m ✓] [Category] [Segment]    |
├──────────────────────────────────────────────────────────────────────────────┤
│ KPI CARDS (value = selected period):                                         │
│ ┌───────────┐ ┌───────────┐ ┌───────────┐ ┌──────────────────┐               │
│ │Total      │ │Total      │ │Gross      │ │Budget            │               │
│ │Revenue    │ │COGS       │ │Profit     │ │Attainment %      │               │
│ └───────────┘ └───────────┘ └───────────┘ └──────────────────┘               │
├───────────────────────────────────┬──────────────────────────────────────────┤
│ (1) Actual vs Budget vs Forecast  │ (2) Gross Profit + Gross Margin %        │
│     LINE                 [vs B/F] │     LINE, dual axis (Actual only)        │
│     ╱╲╱╲╱╲╱╲ (13 points)          │     bars ▂▅▃▆  ─── margin line        │
│     X: time, rolling 13 months    │     X: time, rolling 12 months           │
│     Y: Revenue · 3 series         │     Y-left: Gross Profit                 │
│        (Actual/Budget/Forecast)   │     Y-right: Gross Margin %              │
├───────────────────────────────────┼──────────────────────────────────────────┤
│ (3) Revenue → COGS → Gross Profit │ (4) Budget Attainment % by Category      │
│     FUNNEL                        │     COLUMN + LINE            [vs B/F]    │
│     ▇▇▇▇▇▇  Revenue          │     █ █ ▄ █  ····· 100% target line      │
│     ▅▅▅▅    COGS               │     X: Category                          │
│     ▃▃▃     Gross Profit        │     Y: column = Revenue,                 │
│     X: stages · Y: value          │        line = Attainment %               │
├───────────────────────────────────┴──────────────────────────────────────────┤
│ TABLE: Actual vs Budget by Category-Segment                    [vs B/F]      │
│   Category-Segment | Actual | Budget | Variance | Attainment %               │
│   (restricted to the comparable period — 2020)                               │
└──────────────────────────────────────────────────────────────────────────────┘
```
Notes: Actual = solid, Budget/Forecast = dashed — never blended into one series. Region is not a
filter here (Budget has no Region, would break Attainment). Chart (2) is Actual-only (Budget has no cost).

---

## Page 2 — Performance & Geography
**Audience:** Sales Director · Sales / Regional Manager — **X-axis time grain: Month** (drill to Day)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ FILTERS: [Date range] [Region ▸ District] [Category] [Segment] [Product]    │
├─────────────────────────────────────────────────────────────────────────────┤
│ KPI CARDS:                                                                  │
│ ┌───────────────┐ ┌───────────────┐ ┌────────────────────┐                  │
│ │Total Units    │ │Total Revenue  │ │Avg Selling Price   │                  │
│ │Sold           │ │               │ │per Unit (ASP)      │                  │
│ └───────────────┘ └───────────────┘ └────────────────────┘                  │
├─────────────────────────────────┬───────────────────────────────────────────┤
│ (1) Revenue by Region           │ TABLE: Regional performance (drill-down)  │
│     BAR                         │   Level (Region▸District▸City)            │
│     ████████ East               │   | Revenue | Gross Profit | Margin %     │
│     ██████   Central            │   | Prior-Year Revenue                    │
│     ████     West               │                                           │
│     X: Revenue · Y: Region      │   ▸ Region                                │
├─────────────────────────────────┤     ▸ District                            │
│ (2) Top 10 Products by Revenue  │       ▸ City                              │
│     BAR                         │                                           │
│     ███████ Product A           │   (geography = customer location:         │
│     █████   Product B           │    Sales → Customer.ZipCode → Geo)        │
│     X: Revenue · Y: Product     │                                           │
│     (tie-break: Units, name)    │                                           │
└─────────────────────────────────┴───────────────────────────────────────────┘
```
Layout: detail matrix on one half, the two bar charts stacked on the other half.

---

## Page 3 — Customer & Behavior
**Audience:** Marketing · Product Development — **X-axis time grain: Month**

```
┌────────────────────────────────────────────────────────────────────────────┐
│ FILTERS: [Date range] [Traffic Channel] [Device]                           │
├────────────────────────────────────────────────────────────────────────────┤
│ KPI CARDS:                                                                 │
│ ┌────────────┐ ┌────────────┐ ┌────────────────┐ ┌────────────────────┐    │
│ │Total       │ │New         │ │Repeat Purchase │ │Avg Purchase        │    │
│ │Customers   │ │Customers   │ │Rate %          │ │Frequency           │    │
│ └────────────┘ └────────────┘ └────────────────┘ └────────────────────┘    │
├───────────────────────────────────┬────────────────────────────────────────┤
│ (1) New vs Returning over time    │ (2) Revenue & Orders by Traffic Channel│
│     CLUSTERED COLUMN              │     CLUSTERED COLUMN, dual axis        │
│     ▐▌▐▌ ▐▌▐▌ ▐▌▐▌ (New/Ret)      │     █▆▄▂  ─── orders line            │
│     X: Month                      │     X: Traffic Channel (desc)          │
│     Y: Customers · 2 groups       │     Y-left: Revenue, Y-right: Orders   │
├───────────────────────────────────┼────────────────────────────────────────┤
│ (3) Customers by Segment          │ (4) Customer Profitability Tiering     │
│     BAR                           │     COLUMN / RANKED TABLE              │
│     ██████ Segment A              │     ████ Top 10%                       │
│     ████   Segment B              │     ██   Next 20%                      │
│     X: Customers · Y: Segment     │     ▃    Remaining                    │
│     (counted per Segment bought)  │     X: tier · Y: % of Gross Profit     │
└───────────────────────────────────┴────────────────────────────────────────┘
```
Notes: Device is a filter, not a chart. A customer who buys multiple Segments is counted in each.
Jan-2015 shows an artificial "new customers" spike (no pre-2015 history) — footnote it, do not remove.

---

## Cross-page conventions
- **Global filter:** Date range applies to all pages; the time-grain (Month / Quarter / Year) drives the X axis and defaults per audience (P1 rolling 13/12 months, P2 Month, P3 Month).
- **`[vs B/F]` visuals:** only Page 1 (charts 1, 4 and the table). Budget grain is Category × Segment × Month, Revenue only — no plan comparison by Region, Product, Channel, or profit.
- **Interactions:** cross-filter within page; drill-down Category→Segment→Product and Region→District→City; drill-through from a Product/Region/Channel to an entity detail.
- **Formatting:** USD labelled on monetary visuals; percentages 1 decimal; Actual = solid, Budget/Forecast = dashed.

## Related documents
- Business Requirements Document (`VanArsdel_BRD.md`)
- Report Spec (`VanArsdel_Report_Spec.md`)
- Data Profiling Report (`VanArsdel_Data_Profiling.md`)
- Data Quality Assessment (`VanArsdel_Data_Quality.md`)
