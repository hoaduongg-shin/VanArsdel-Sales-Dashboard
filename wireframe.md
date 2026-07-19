# Mockup / wireframe — VanArsdel Retail Executive Dashboard

Layout sketches for the 3 pages defined in `docs/report-spec.md`. Each visual is annotated with
the metric/dimension it shows so a DA can map it 1:1 to a measure in `dictionary/data-dictionary.md`.

## Page 1 — Executive Overview
![Page 1 wireframe](../../../docs/assets/page1-executive-overview.svg)

| Visual | Metric / dimension shown |
|--------|----------------------------|
| KPI card — Total Revenue | `[Total Revenue]`, current filter context |
| KPI card — Total Units | `[Total Units]` |
| KPI card — Gross Margin % | `[Gross Margin %]` |
| KPI card — Budget Attainment % | `[Budget Attainment %]`, forced to Jan–Jun 2020 (only comparable window) |
| Trend line | `[Total Revenue]` by `Date[Month]`, 2015 → H1 2020; dashed segment = `[Budget Amount]` reference, 2020 only |
| Bar — Revenue by Region | `[Total Revenue]` by `Geo[Region]` |
| Bar — Revenue by Category | `[Total Revenue]` by `Product[Category]` |
| Slicers | Year, Region, Category |

## Page 2 — Budget & Category Performance
![Page 2 wireframe](../../../docs/assets/page2-budget-category.svg)

| Visual | Metric / dimension shown |
|--------|----------------------------|
| KPI cards | Actual Revenue, Budget Revenue, Attainment %, Variance — all Jan–Jun 2020 |
| Grouped bar | `[Total Revenue]` vs `[Budget Amount]` by `Product[Category-Segment]`, 9 groups (Rural-Productivity excluded — no Budget column, data gap #4) |
| Table | Category-Segment × Actual / Budget / Variance / Attainment % |
| Reference trend (greyed, dashed) | `[Forecast Amount]` by month, 2021 — labeled "no Actuals available for this period yet" so it's never misread as a real comparison |
| Slicers | Category, Segment |

## Page 3 — Marketing & Customer Detail
![Page 3 wireframe](../../../docs/assets/page3-marketing-customer.svg)

| Visual | Metric / dimension shown |
|--------|----------------------------|
| KPI cards | Total Customers, New Customers, Top Channel by Revenue, Revenue/Customer |
| Stacked bar | `[Total Revenue]` by `Campaign[TrafficChannel]` × `Campaign[Device]` |
| Table — Top 10 Products | `Product[Product]`, `Product[Category-Segment]`, `[Total Revenue]`, `[Total Units]`, sorted desc by Revenue |
| Table — Top 10 Customers | `Customer[Customer Name]`, `Geo[Region]`, `[Total Revenue]`, `# purchases` (`COUNTROWS(Sales)`), sorted desc by Revenue |
| Slicers | Traffic Channel, Device, Date range |

## Notes for the DA
- All three pages share the same Date/Region/Category filter model — cross-filtering should work
  identically on Page 1 & Page 2; Page 3 filters independently by Channel/Device since Campaign is
  not related to Category.
- Any visual that mixes Actual with Budget or Forecast must use visibly distinct series styling
  (solid vs. dashed, as sketched) — Actual, Budget, and Forecast never share a continuous time
  window in this dataset (see `docs/data-gaps.md` #1), so blending them into one unbroken line
  would misrepresent the data.
