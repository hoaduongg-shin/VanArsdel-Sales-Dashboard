# Business glossary (sale)

Shared definitions used across reports. Report-specific field definitions live in each report's
`dictionary/data-dictionary.md`; this file holds cross-report business concepts.

## Company & Product

| Term | Definition |
|------|------------|
| **VanArsdel** | The retail/manufacturing company that owns this dataset. |
| **Product** | An individual sellable SKU (e.g. "Maximus RP-01"). Identified by `ProductID`. |
| **Category** | A top-level grouping used to classify similar products - Rural, Urban, Mix, Youth, Accessory. |
| **Segment** | A subdivision within a category that groups products with similar characteristics or target customers  (e.g. Select, Productivity, Convenience, Extreme, Moderation, Regular, All Season, Youth, Accessory). |
| **Unit cost** | The cost incurred to acquire or produce one unit of a product. |
| **Unit price** | The selling price charged to customers for one unit of a product. |

## Sale & Revenue

| Term | Definition |
|------|------------|
| **Sales transaction/Sales line** | One row in the `Sales` fact = one product sold to one customer, on one date, attributed to one campaign touchpoint. |
| **Units** | Quantity of a product sold in one Sales row. |
| **Revenue** | Total sales value generated from products sold ( = Unit x Unit Price (from `Product`)). |
| **COGS** | Cost of Goods Sold - Direct production cost associated with products sold (*= Unit x Unit Cost (from `Product`)*). |
| **Gross Profit** | Earnings remaining after direct production costs are deducted (*= Revenue − COGS*). |
| **Gross Margin %** | Percentage of sales retained after covering direct production costs (*= Gross Profit/Revenue*). |

## Budget & Forecast

| Term | Definition |
|------|------------|
| **Budget** | Approved sales target used to evaluate business performance. |
| **Forecast** | Updated projection of expected business performance. |
| **Attainment %** | Percentage of target achieved during the reporting period (*= Actual/Budget*). |
| **Variance** | Difference between actual performance and target (*= Actual − Budget*). |

## Marketing/campaign

| Term | Definition |
|------|------------|
| **Campaign** | A marketing or sales initiative used to promote products or drive sales during a specific period. |
| **Traffic Channel** | The source through which customers visit a website, app, or store (e.g. "Affiliate","Organic Search"). |
| **Device** | The source or channel through which a customer accessed the platform (e.g. "Desktop","Mobile","Tablet"). |

## Customer & Geography

| Term | Definition |
|------|------------|
| **Customer** | An individual buyer whose purchasing activity is tracked over time. |
| **State** | The state, province, or equivalent administrative division of a country. |
| **Region** | A geographic area used to group multiple locations. |
| **District** | A local administrative division within a state, province, or city. |
| **Country** | The country where the customer or transaction is located. |

## Time

| Term | Definition |
|------|------------|
| **MoM** | Month over month (vs the previous month). |
| **YTD** | Year to date (Jan 1 → current period). |
| **Grain** | The level of detail of one fact row (e.g. *one sales row = 1 product × 1 customer × 1 date × 1 campaign touchpoint*). |
