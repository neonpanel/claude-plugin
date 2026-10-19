# Planning And Forecasting — Knowledge

The Planning and Forecasting knowledge area covers how NeonPanel combines historical sales data with promotional goals to forecast sales velocity and inventory needs. It describes the inventory data streams (CALCULATED vs. 3PL/Amazon Ledger), the four required inputs for PO planning (identifier, scenario, parameters, scope), and the metrics used for risk assessment (days of supply, safety stock, velocity).

This area provides the foundational concepts an agent needs to understand before engaging in PO planning workflows. The procedural steps for running analysis are covered in the separate PO Planning Workflow skill pack.

## Planning and Forecasting module overview

### What it is

The Planning and Forecasting module combines historical sales data with promotional goals and operational plans to forecast sales velocity and determine inventory needs. It is the foundation for all PO planning and replenishment decisions.

### Key objects

- **Historical Data** — past sales used to establish baseline velocity
- **Promotional Goals** — planned sales lifts from promotions, deals, or ranking targets
- **Sales Velocity** — how fast a SKU sells per day, calculated in two modes
- **Inventory Needs** — the quantity required to cover demand until the next replenishment

### Facts

- Historical data alone cannot predict increased velocity from promotional activities.
- Forecast scenarios control which sales projections are used for calculations.
- The module supports two velocity modes: current (weighted trailing averages) and planned (arrival-anchored multi-month average).

## FBA replenishment suggestions

### What it is

FBA replenishment suggestions are system-generated recommendations for how much to send to Amazon fulfillment centers. They are based on Amazon Ledger data, not NeonPanel calculated inventory.

### How suggestions are generated

- Amazon Ledger provides the current FBA inventory picture (available + inbound)
- Replenishment parameters (min/max levels, lead time, sell-through rate) are applied
- The suggestion shows recommended quantity to send to FBA

### Facts

- The FBA replenishment tool always counts inbound units in total_fba_available_units.
- This differs from PO planning tools which do not count inbound.
- Amazon Ledger data must be imported for suggestions to be generated.
- The user must provide a SKU or ASIN and marketplace to get suggestions.

## Parent ASIN lookup: trust and resolve

### What it is

When a user provides a parent ASIN, it should be trusted immediately and passed directly to supply chain tools. The tools handle parent-to-child resolution natively.

### Why this matters

- Pre-validating via `neonpanel_listInventoryItems` is unnecessary and may introduce errors
- Supply chain tools accept `parent_asin` and auto-resolve child SKUs
- This saves time and avoids redundant API calls

### Facts

- Product family (product_family field) is more reliable than parent ASIN for finding all variants because it is manually curated.
- Parent ASIN mappings can be stale, null, or missing for new variants.
- The marketplace should be confirmed alongside the parent ASIN.

## FBA replenishment queries vs. PO planning

### What it is

A user asking about FBA replenishment may need clarification on how it differs from PO planning. The two workflows use different data sources and serve different purposes.

### Key distinction

| Aspect | FBA Replenishment | PO Planning |
|---|---|---|
| **Data source** | Amazon Ledger | NeonPanel calculated inventory |
| **Purpose** | Restock Amazon FBA | Replenish total supply chain |
| **Inbound counting** | Counts inbound | Does not count inbound |

### Facts

- The user may not know which workflow they need — clarify whether they're replenishing FBA inventory specifically.
- FBA replenishment suggestions are only available if Amazon Ledger data has been imported.

## Inventory data streams: when a user asks about them

### What it is

Users may ask about inventory data streams without specifying which one. The two streams — CALCULATED and 3PL — have different sources and purposes.

### How to clarify

- Determine if the user is asking about a specific SKU's data or the general system architecture
- Explain the two streams and their sources
- Help the user understand which stream is relevant

### Facts

- CALCULATED data comes from NeonPanel's own transactions.
- 3PL data comes from external sources like Amazon Ledger.
- Discrepancies between the two streams may indicate data integrity issues.

## PO planning request: starting the workflow

### What it is

A PO planning request is a user asking for purchase order recommendations. It begins the 7-phase PO planning workflow.

### What's needed to start

- Company and marketplace
- Product identifier (ASIN, SKU, or product family)
- The PO Planning Workflow skill handles the full 7-phase procedure

### Facts

- The PO Planning Workflow skill is a separate skill pack that covers the complete procedure.
- The agent should confirm the company and marketplace before initiating the workflow.
- The 7-phase workflow covers: intake, validation, assessment, recommendation, presentation, review, and execution.

## PO timing: recommended order dates and arrival windows

### What it is

PO timing determines when a purchase order should be placed and when inventory is expected to arrive. Three factors drive the calculation.

### Key factors

- **Lead time** — days from PO placement to receipt. The recommended order date = target arrival date − lead time.
- **PO cadence** — how often POs are placed (e.g., every 30 days). This sets the minimum gap between orders.
- **Arrival window** — the period the new inventory is expected to cover. The window shows the demand range the shipment will serve.

### Facts

- The arrival window should be shown alongside the demand it covers so the user can assess whether the timing aligns with sales patterns.
- Urgent timing (e.g., lead time extends beyond the stockout date) is a critical signal that alternative fulfillment methods may be needed.
- PO cadence prevents placing orders too close together — respect the configured cadence when recommending.

## Understanding inventory data streams

### What it is

NeonPanel maintains two parallel streams of inventory quantity data. Understanding the difference explains why the same SKU may show different values depending on which stream is queried.

### The two streams

| Stream | Source | Used for |
|---|---|---|
| **CALCULATED** | NeonPanel's own transactions | PO planning, internal tracking |
| **3PL** | External sources (Amazon Ledger, etc.) | FBA replenishment suggestions |

### Facts

- The two streams may show different quantities for the same SKU because they originate from different sources.
- 3PL data must be imported manually via NeonPanel's Import feature.
- FBA replenishment uses Amazon Ledger data, not NeonPanel calculated data.

## Forecast scenario management in NeonPanel

### What it is

Forecast scenarios are the statistical models NeonPanel uses to project sales for inventory items. They can be managed both by Kio (via NeonPanel forecasting tools) and by the user in the NeonPanel UI.

### What the agent can do

- Read the current company forecast settings, including the default scenario, with `forecasting_get_company_settings`.
- Inspect current forecasts for an item with `forecasting_get_sales_forecast_details` or `forecasting_list_sales_forecasts`.
- Compare how different scenarios would project an item with `forecasting_compare_sales_forecast_scenarios`.
- Update the company default scenario/seasonality with `forecasting_update_company_settings`.
- Generate or recalculate a forecast (`forecasting_generate_sales_forecast`, `forecasting_run_sales_forecast_job`), then write it to Live with `forecasting_write_sales_forecast`.

### Facts

- Kio can update settings, recalculate, and write forecasts to Live — this is a core reason it's useful.
- After changing a scenario, a recalculation must run and be written to Live before projections update.
- A modeled or compared scenario does not affect replenishment until written to Live.
- Different SKUs under the same parent may have different scenario assignments.
- Per-item scenarios (set in the Inventory Catalog UI) override the company default.

### Don't claim

- Don't claim a compared/modeled scenario is live until it has been written with `forecasting_write_sales_forecast`.

### Escalate when

- A settings update or forecast write fails repeatedly, or the wrong scenario persists after a successful recalculation.

## Promotional planning: accounting for demand lifts

### What it is

Promotional planning accounts for the increased sales velocity that comes from promotions, deals, or ranking targets. Historical data alone cannot predict promo-driven demand.

### Key considerations

- Promotional goals (e.g., achieving top-3 organic ranking on Amazon) can significantly increase velocity
- Historical data reflects past demand, not future promotional lifts
- The user should specify the expected increase in sales velocity from the promotion

### Facts

- Historical data alone cannot predict increased sales velocity when promotional goals are in play.
- The user's promotional targets should be factored into the replenishment recommendation.
- Without promotional input, the recommendation will be based on baseline velocity only.

## Planning parameters explained

### What it is

Planning parameters are the operational settings that control PO recommendations. Each parameter has a specific meaning and can be set at the SKU level or inherited from company defaults.

### The four parameters

| Parameter | What it controls | Default source |
|---|---|---|
| **Lead time** | Days from PO placement to receipt | SKU or company |
| **Safety stock** | Buffer days against demand variability | SKU or company (with class multiplier) |
| **PO cadence** | Minimum gap between POs (e.g., 30 days) | SKU or company |
| **Planning horizon** | How far out to plan | Company default |

### Facts

- Safety stock uses class-based multipliers (e.g., Class A × 1.2) applied to the base safety stock days.
- Missing lead time or safety stock at both SKU and company level is a critical configuration gap.
- Different SKUs under the same parent may have different parameter configurations.

## Identifier validation and resolution

### What it is

Identifier validation is the process of confirming that a product identifier (parent ASIN, child ASIN, SKU, or product family) resolves to the correct SKUs in the system.

### Resolution behavior by identifier type

| Type | Resolution method |
|---|---|
| **Parent ASIN** | Pass directly to supply chain tools — they auto-resolve children |
| **Child ASIN** | Resolve directly to the specific variant |
| **SKU code** | Resolve directly to one record |
| **Product family** | Two-step: parent ASIN lookup first, then product family cross-check |

### Facts

- Parent ASINs should not be pre-validated via neonpanel_listInventoryItems — supply chain tools handle resolution natively.
- Product family is more reliable than parent ASIN for finding all variants because it is manually curated.
- The number of expected variants should be confirmed with the user before proceeding.

## Presentation rules for product variants

### What it is

When displaying product variants, each variant must be shown individually in its own row. This ensures the user can see the full picture.

### Presentation rules

- Every product variant appears in its own row
- The total variant count is stated and the user is asked to confirm
- Variants are never collapsed into combined columns or "other" buckets without explicit user consent
- This applies to inventory position, PO recommendations, and any multi-variant display

### Facts

- Collapsing variants hides important differences in lead time, safety stock, and risk status between variants.
- The user should always see the full list and confirm the count before proceeding.

## Selecting a forecast scenario for a SKU

### What it is

Selecting a forecast scenario for a SKU determines which statistical model builds its sales baseline for replenishment and downstream planning. Kio can help choose and apply the scenario; the user can also set it per-SKU in the NeonPanel UI.

### What the agent can do

- Compare candidate scenarios for the SKU with `forecasting_compare_sales_forecast_scenarios` to show which fits recent demand best.
- Update the company default scenario with `forecasting_update_company_settings` when the choice should apply broadly (read first with `forecasting_get_company_settings`).
- Recalculate the forecast (`forecasting_generate_sales_forecast` / `forecasting_run_sales_forecast_job`) and write it to Live (`forecasting_write_sales_forecast`).
- Explain the six scenarios so the user can decide, and note when a per-SKU override in the Inventory Catalog is the better path.

### Facts

- Kio can apply a scenario by recalculating and writing to Live; the user can also set a per-SKU scenario in the Inventory Catalog.
- Per-inventory scenario settings override the company default for that SKU and marketplace.
- A scenario choice only affects replenishment once the forecast is recalculated and written to Live.
- Item-level assignment wins over product family, which wins over the company default.

### Don't claim

- Don't claim a compared scenario is in effect until the forecast has been written to Live.

### Escalate when

- A per-SKU override set in the UI doesn't take effect after the next recalculation, or a write to Live fails.

## Sales velocity calculation modes and weighting

### What it is

Sales velocity — how fast a SKU sells per day — is calculated differently depending on the mode selected. Understanding these modes explains why the same SKU may show different velocity values in different contexts.

### Current velocity mode

Uses a weighted blend of trailing averages:
- Default weights: 0.5 for 30-day, 0.3 for 7-day, 0.2 for 3-day
- Custom weights can be supplied via the velocity_weighting parameter
- The resolved weights are returned in meta.velocity_weighting_used

Realized daily averages (avg_units_30d, avg_units_7d, avg_units_3d) average only over **in-stock days** (days where Amazon restock report shows available > 0). This means out-of-stock periods do not deflate the demand rate — the averages reflect actual selling velocity, not calendar-day averages.

### Planned velocity mode

Uses an arrival-anchored multi-month window average:
- Sum of forecast months from arrival month through the coverage window
- Divided by (window_months × 30.41)
- This smooths forecast noise and prices in seasonality for the period new inventory will actually be selling

### Tool vs. dashboard differences

MCP supply chain tools may flag items as due approximately 2–4 weeks earlier than the QuickSight "60.0 Inventory Planning" dashboard. The tools additionally subtract reorder cadence (days_between_pos default 30 for PO, days_between_shipments default 14 for FBA). Setting the cadence parameter to 0 produces dashboard-comparable output.

### Facts

- Custom velocity weights can be supplied and the agent should always tell the user which weights were applied.
- The planned velocity mode uses a multi-month average, not a single forecast month — this prevents forecast noise from driving erratic recommendations.
- In-stock-day averaging (current mode) means velocity does not drop during stockouts, which more accurately reflects true demand.
- Historical data alone cannot predict increased sales velocity when promotional goals (e.g., achieving top-3 organic ranking on Amazon) are in play.

## SKU lookup resolution by identifier type

### What it is

SKU lookup resolution depends on the identifier type provided. Each type has a different resolution path.

### Resolution by type

| Identifier type | Resolution path |
|---|---|
| **Parent ASIN** | Pass directly to supply chain tools — they auto-resolve children |
| **Child ASIN** | Resolve directly to the specific variant |
| **SKU code** | Resolve directly to one record |

### Facts

- Parent ASINs should not be pre-validated via neonpanel_listInventoryItems.
- A marketplace is needed alongside the identifier to resolve correctly.
- Product family is a separate lookup path that may span multiple parent ASINs.

## Product family lookup: two-step resolution

### What it is

Product family lookup is a two-step resolution process that finds all variants belonging to a product family. It is more reliable than parent ASIN alone.

### The two-step process

1. **Query by parent ASIN** first to find initial variants
2. **Cross-check by product_family** field to find any additional variants not in the parent ASIN group

### Why this matters

- Product family is manually curated by the team, not dependent on Amazon catalog sync
- Parent ASIN mappings can be stale, null, or missing for new variants
- The two-step approach catches variants that would otherwise be missed

### Facts

- Orphaned variants (products with a product_family but no parent ASIN) should be flagged.
- The marketplace should be confirmed alongside the lookup.

## Diagnosing an inactive SKU

### What it is

An inactive SKU is one that is not flagged as actively sold or planned in NeonPanel. Whether it should be included in PO planning depends on the `planning_base` parameter.

### Planning base parameter

| Value | Behavior |
|---|---|
| `actively_sold_only` | Only includes SKUs flagged as actively sold — excludes inactive SKUs |
| `all` | Includes all SKUs regardless of status |

### Facts

- If a SKU should be included but is not showing up, the user may need to update its status in NeonPanel.
- Inactive SKUs may still have inventory data and can be queried individually, but they won't appear in bulk planning runs with `actively_sold_only`.
- The `planning_base` parameter choice affects the scope of the PO analysis and should be communicated to the user.

## Four required inputs for PO planning

### What it is

Every PO planning request requires four categories of input, all of which must be confirmed before any analysis tools are called.

### 1. Identifier type

The product identifier determines how tools are called. The identifier may be:
- **Parent ASIN** — preferred path; supply chain tools accept parent_asin and auto-resolve child SKUs
- **Child ASIN** — a specific variant under a parent
- **SKU code** — the most granular identifier
- **Product family** — a manually curated grouping that may span multiple parent ASINs

When the user provides a parent ASIN, trust it immediately and pass it directly to supply chain tools. Do NOT pre-resolve it via neonpanel_listInventoryItems — the supply chain tools handle parent-to-child resolution natively.

Product family (product_family field) is more reliable than parent ASIN for finding all variants because it is manually curated by the team, not dependent on Amazon catalog sync. Parent ASIN mappings can be stale, null, or missing for new variants.

### 2. Forecast scenario

The forecast scenario (e.g., "promo_shift_128_growth_v2") determines which sales projections are used. Scenarios are configured in NeonPanel, not by the agent. A scenario mismatch occurs when the user requests a specific scenario but the SKU is assigned a different one.

### 3. Planning parameters

Each SKU may have its own settings or fall back to company defaults:
- **Lead time** — days from PO placement to receipt
- **Safety stock** — buffer days, with class-based multipliers (e.g., Class A × 1.2)
- **PO cadence** — how often POs are placed (e.g., every 30 days)
- **Planning horizon** — how far out to plan

When neither SKU-level nor company-default values exist for lead time or safety stock, this is a critical configuration gap that blocks PO planning.

### 4. Scope

Which SKUs and which marketplace(s) are being planned. When a parent ASIN is provided, confirm the marketplace and whether all child SKUs under that parent should be included.

### Facts

- All four inputs must be collected in a single structured request, not spread across multiple conversational turns.
- Different SKUs under the same parent may have different forecast scenarios, lead times, and safety stock configurations. Each SKU must be validated independently.
- The agent cannot change forecast scenarios directly — scenario changes must be made by the user in NeonPanel.

## FBA replenishment shipments and Amazon Ledger data

### What it is

FBA replenishment shipments are shipments sent to Amazon fulfillment centers. Unlike PO planning, FBA replenishment uses **Amazon Ledger data** to calculate suggestions — not NeonPanel's own calculated inventory.

### Key difference: PO vs. FBA replenishment

| Aspect | PO Planning | FBA Replenishment |
|---|---|---|
| **Data source** | NeonPanel calculated inventory | Amazon Ledger data |
| **Tool** | PO planning workflow | FBA replenishment tools |
| **Inventory scope** | Own + 3PL warehouse | Amazon FBA network |

### How FBA replenishment suggestions are calculated

- Based on Amazon Ledger balances (FBA available + inbound)
- Uses configured replenishment parameters (min/max, lead time, etc.)
- Accounts for sell-through rate and pending orders

### Facts

- Amazon Ledger data must be imported into NeonPanel before FBA replenishment suggestions can be calculated.
- The FBA replenishment tool always counts inbound units in total_fba_available_units.
- The PO placement tool does NOT count inbound/in-transit units in available inventory — this is a key difference between the two workflows.

## Checking inventory position for a SKU

### What it is

Inventory position is the current stock state of a SKU across all locations. It is retrieved via the `supplychaininspectinventorysku_snapshot` tool, which returns four components: FBA available, inbound, warehouse stock, and WIP orders.

### Key information to present

- Days of supply — both FBA-only and with inbound calculations
- Risk status derived from the days-of-supply thresholds
- The four breakdown components

### Facts

- The snapshot reflects a point-in-time view. Inbound quantities change as shipments are received.
- A company and marketplace context are needed before calling the tool.
- The snapshot is the starting point for any replenishment recommendation.

## FBA replenishment planning workflow

### What it is

FBA replenishment planning is the process of determining when and how much to send to Amazon fulfillment centers. It differs from PO planning in its data source and workflow.

### FBA vs. PO planning

- **FBA replenishment**: Uses Amazon Ledger data, focuses on FBA inventory levels, accounts for inbound shipments to FBA
- **PO planning**: Uses NeonPanel calculated inventory, covers warehouse + 3PL stock, broader scope

### Facts

- Amazon Ledger data must be imported for the system to generate FBA replenishment suggestions.
- FBA replenishment accounts for inbound FBA shipments in available inventory, unlike PO planning.
- The user should be guided to the correct workflow based on whether they're replenishing FBA or planning purchase orders.

## Missing safety stock configuration

### What it is

Safety stock — the buffer days held to protect against demand variability — may not be configured for a given SKU. The system checks for a company-level default and applies a class-based multiplier.

### Resolution

- If safety stock is not configured for the SKU, check if a company default exists and report it with the class multiplier information
- If no company default exists either, this is a **critical configuration gap** that blocks PO planning
- Class multipliers (e.g., Class A × 1.2) are applied to the base safety stock days

### Facts

- Safety stock is per-SKU with inheritance from company defaults.
- The class multiplier reflects the SKU's importance — higher-class SKUs get larger buffers.
- The user can configure safety stock in NeonPanel: **Inventory Planning → Select SKU → Planning Parameters → Safety Stock**.

## FBA replenishment calculation logic

### What it is

FBA replenishment logic determines how NeonPanel calculates replenishment suggestions for FBA inventory. The key distinction is that it uses **Amazon Ledger data** rather than NeonPanel's calculated inventory.

### How it works

- Amazon Ledger data provides the current FBA inventory picture
- Replenishment parameters (min/max levels, lead time, sell-through rate) determine when and how much to replenish
- Suggestions are calculated based on the gap between current levels and target levels

### Facts

- The agent explains the data source distinction when asked about FBA replenishment vs. PO planning.
- A user asking about FBA replenishment may need to understand why the numbers differ from NeonPanel's calculated inventory.
- Amazon Ledger data must be imported for the system to generate FBA replenishment suggestions.

## Missing lead time configuration

### What it is

Lead time — the days from PO placement to receipt — may not be configured for a given SKU. In that case, the system checks for a company-level default.

### Resolution

- If lead time is not configured for the SKU, check if a company default exists and report it
- If no company default exists either, this is a **critical configuration gap** that blocks PO planning
- The user can configure lead time in NeonPanel: **Inventory Planning → Select SKU → Planning Parameters → Lead Time**

### Facts

- Lead time is per-SKU but can inherit from company defaults.
- A missing lead time means the system cannot calculate the recommended order date.
- Different SKUs under the same parent may have different lead time configurations.

## Inventory position components and risk metrics

### What it is

Inventory position is the current stock state of a SKU across all locations and stages. It breaks down into four components, and risk is assessed through days-of-supply calculations.

### Four components of inventory position

| Component | Description | Counted as available? |
|---|---|---|
| **FBA available** | Units currently sellable on Amazon | Yes |
| **Inbound** | Units in transit to FBA warehouses | Yes (for supply calculations) |
| **Warehouse stock** | Units in own or 3PL warehouses | Only if shipped to FBA |
| **WIP orders** | Purchase orders placed but not yet received | No — not available stock |

### Days of supply — two calculations

Days of supply must always be calculated two ways:
- **FBA-only**: FBA available ÷ daily velocity — reflects immediate sell-through risk
- **With inbound**: (FBA available + inbound) ÷ daily velocity — reflects forward coverage with pipeline inventory

Both values are needed for risk assessment. The gap between them matters most during restock windows.

### Risk thresholds

- **FBA-only < 7 days**: Critical — stockout imminent
- **FBA-only < 14 days**: At risk — restock needed soon
- **FBA-only ≥ 14 days and with inbound ≥ 30 days**: Healthy

### Safety stock multipliers

Safety stock uses class-based multipliers applied to base safety stock days:
- Class A: 1.2x
- Other classes use their configured multiplier (varies by company)

### Facts

- WIP orders are part of total coverage but must not be counted as available stock.
- AWD (Amazon Warehousing and Distribution) inventory is distinct from FBA inventory. AWD stock must be shipped to FBA before it becomes available for sale.
- Inventory snapshots reflect a point-in-time view. Inbound quantities change as shipments are received.
- The PO placement tool does NOT count inbound/in-transit units in available inventory. The FBA replenishment tool always counts inbound units in total_fba_available_units.
- When inventory data appears inconsistent across sources (e.g., CALCULATED vs. 3PL), this is an escalation-worthy data integrity issue.
- The planning_base parameter controls which SKUs are included: 'actively_sold_only' (active SKUs only) or 'all' (all SKUs regardless of status).

## Planning parameters: lead time, safety stock, PO cadence, and horizon

### What it is

Planning parameters define the operational constraints for PO recommendations. Each SKU may have its own settings, or fall back to company-level defaults.

### The four parameters

| Parameter | Description | Source |
|---|---|---|
| **Lead time** | Days from PO placement to receipt | SKU-level or company default |
| **Safety stock** | Buffer days with class-based multipliers | SKU-level or company default |
| **PO cadence** | Minimum gap between POs (e.g., 30 days) | SKU-level or company default |
| **Planning horizon** | How far out to plan | Typically company default |

### Facts

- When neither SKU-level nor company-default values exist for lead time or safety stock, this is a critical configuration gap that blocks PO planning.
- Safety stock uses class-based multipliers (e.g., Class A × 1.2) applied to the base safety stock days.
- Different SKUs under the same parent may have different parameter configurations.

## Forecast scenarios: checking and resolving assignments

### What it is

Forecast scenarios determine which statistical model builds a SKU's sales baseline for replenishment. Kio can check the current state and resolve issues directly.

### How to check

- Read the company default and settings with `forecasting_get_company_settings`.
- Inspect the item's current forecast with `forecasting_get_sales_forecast_details` or list forecasts with `forecasting_list_sales_forecasts`.
- Look up inventory context with `inventory_get_details` / `inventory_list_items` when you need SKU/marketplace specifics.
- Compare scenarios with `forecasting_compare_sales_forecast_scenarios`.

### How to resolve

- Update the default scenario/seasonality with `forecasting_update_company_settings`.
- Recalculate with `forecasting_generate_sales_forecast` or `forecasting_run_sales_forecast_job`; track progress via `forecasting_check_sales_forecast_job_status`.
- Write the result to Live with `forecasting_write_sales_forecast`.

### Facts

- Kio can both check and change scenario/forecast state — including recalculating and writing to Live.
- A change affects replenishment only after the forecast is written to Live.
- Per-item scenarios (set in the Inventory Catalog) override the company default.

### Don't claim

- Don't reference tools Kio doesn't have; use the NeonPanel forecasting/inventory tools listed above.
- Don't claim a comparison result is live until it has been written to Live.

### Escalate when

- A settings update or forecast write fails repeatedly, or the wrong scenario persists after a completed recalculation.

## Product family resolution: two-step lookup

### What it is

Product family resolution is a two-step lookup that finds all variants belonging to a product family. It is more reliable than parent ASIN alone.

### The two-step process

1. **Parent ASIN lookup** — find variants under the parent ASIN
2. **Product family cross-check** — query the `product_family` field to find any additional variants not in the parent ASIN group

### Why this is needed

- Product family is manually curated and not dependent on Amazon catalog sync
- Parent ASIN mappings can be stale, null, or missing for new variants
- Orphaned variants (products with a product_family but no parent ASIN) should be flagged

### Facts

- The two-step approach catches variants that the parent ASIN lookup alone would miss.
- Orphaned variants may need manual review to resolve their parent ASIN mapping.

## Confirming planning scope

### What it is

Before running PO planning, the scope must be confirmed with the user. This prevents running analysis on the wrong SKUs or marketplaces.

### Scope confirmation

- List all SKUs matching the identifier so the user can verify the count
- If multiple marketplaces are available, confirm which one(s) to include
- A parent ASIN may resolve to 1–50+ child SKUs — the user should confirm the count before proceeding

### Facts

- Scope confirmation is a user-facing step, not an internal validation — the user must explicitly approve the SKU list.
- Scope may differ between planning runs (e.g., "all children" vs. "just the top seller").

## Planning scope: SKUs and marketplaces

### What it is

Planning scope defines the boundaries of a PO planning request — which SKUs and which marketplaces should be included.

### Scope dimensions

- **SKU coverage**: All SKUs under a parent ASIN, a specific subset, or a product family
- **Marketplace**: US, UK, CA, EU, or multiple marketplaces

### Facts

- Different SKUs under the same parent may have different forecast scenarios, lead times, and safety stock — each SKU must be validated independently.
- A product family may span multiple parent ASINs, so the scope may be broader than a single parent lookup.
- Marketplace determines which Amazon fulfillment network and currency apply.

## SKU not found: causes and resolution

### What it is

A SKU not found response means the identifier could not be resolved in the system. This can happen for several reasons.

### Common causes

- The identifier is misspelled or incorrect
- The SKU is not set up for the specified marketplace
- The product family mapping is wrong or incomplete
- The SKU has been discontinued or archived

### Facts

- Double-check the identifier spelling and marketplace before concluding the SKU doesn't exist.
- Product family resolution is more reliable than parent ASIN for finding all variants — if parent ASIN fails, try the product family.
- A SKU that exists in one marketplace may not exist in another.

## Inventory data streams: user queries

### What it is

Users may ask about inventory data without specifying which stream. The two streams — CALCULATED and 3PL — serve different purposes and may show different values.

### How to clarify

- Determine if the user is asking about a specific SKU's data or the general system architecture
- Explain the two data streams and their sources
- Help the user understand which stream is relevant to their question

### Facts

- CALCULATED data comes from NeonPanel's own transactions.
- 3PL data comes from external sources like Amazon Ledger.
- The streams may diverge for the same SKU — understanding why is key to data integrity.

## Empty results from supply chain tools

### What it is

Empty results mean the tool ran successfully but returned no data. This is different from an error — the tool worked, but the parameters or data produced nothing.

### 5-check diagnostic checklist

1. **Identifier accuracy** — is the SKU/ASIN correct for this marketplace?
2. **Planning base** — is the SKU actively sold, or excluded by `actively_sold_only`?
3. **Scenario assignment** — is the correct forecast scenario assigned?
4. **Parameter configuration** — are lead time and safety stock configured?
5. **Data stream** — are you querying the correct data stream (CALCULATED vs. 3PL)?

### Facts

- Do not re-run the same tool with the same parameters — the result will be the same.
- Empty results for one tool may indicate a different tool or approach is needed.
- The Empty Results Diagnosis skill covers this procedure in detail.

## Inventory position: components and retrieval

### What it is

Inventory position is the current stock state of a SKU across all locations. It is retrieved via the `supplychaininspectinventorysku_snapshot` tool.

### Four components

| Component | Description |
|---|---|
| **FBA available** | Units currently sellable on Amazon |
| **Inbound** | Units in transit to FBA warehouses |
| **Warehouse stock** | Units in own or 3PL warehouses |
| **WIP orders** | Purchase orders placed but not yet received |

### Facts

- The snapshot reflects a point-in-time view — inbound quantities change as shipments are received.
- A company and marketplace context are needed before calling the tool.
- The user may want FBA-only or total inventory across all warehouses — clarify this.

## Applying a forecast scenario in NeonPanel

### What it is

A forecast scenario is the statistical model NeonPanel uses to build a sales baseline for an inventory item, before seasonality multipliers and any manual overrides are applied. Every active inventory item uses one scenario.

NeonPanel offers six built-in scenarios:

- **Recency-weighted growth (ADGP–RWDR)** — weights recent daily sales (roughly the last 12 months) more heavily than distant history. The default-style choice for hero SKUs, launches, and cases where recent velocity matters more than a flat annual average.
- **Availability-driven growth (ADGP)** — controlled upward bias from recent months (often 1–3 months of unit sales), smoothing old spikes and dips. Optimizes for staying in stock with a predictable replenishment cadence.
- **Moving average (12 months)** — averages the last twelve months so single spikes don't dominate. Best for mature SKUs with flat year-over-year demand.
- **Seasonal repeat** — projects last year's monthly shape forward. Strong when Prime Day, Q4, or category seasonality repeats reliably and history is clean.
- **Trend with seasonality** — statistical trend plus seasonal pattern from history, for when the level is rising or falling and calendar peaks still matter.
- **Robust low** — conservative baseline that dampens anomalies and promo spikes. For cash-tight buying, noisy data, or new listings, accepting slight under-forecast risk over overstock.

Most brands use Recency-weighted growth (ADGP–RWDR) as the company default, then assign simpler scenarios only where history or strategy calls for it.

### How to apply a scenario

Kio can apply and recalculate scenarios directly, and the user can also set them in NeonPanel's UI.

**Kio can:**

- Compare scenario outputs for an item before committing (`forecasting_compare_sales_forecast_scenarios`).
- Update the company default scenario/seasonality via `forecasting_update_company_settings` (read current values first with `forecasting_get_company_settings`).
- Generate/recalculate the forecast under the chosen scenario (`forecasting_generate_sales_forecast` or `forecasting_run_sales_forecast_job`), checking progress with `forecasting_check_sales_forecast_job_status`.
- Write the result to the Live forecast so it drives replenishment and downstream planning (`forecasting_write_sales_forecast`).

**In the UI, the user can:**

- **Company default** — Setup & Settings → Forecasting & Planning → Default Forecasting Values → Scenario, then Save Changes. Applies to every item with no override.
- **Per inventory item** — Inventory Catalog → select the row → item modal → Forecast Scenario. Item-level settings always win. If an item has no scenario, NeonPanel falls back to the Product Family value, then the company default.

The scenario combines with the item's seasonality pattern and catalog parameters to produce the baseline that flows to Sales Forecast Live. On the 15th of each month NeonPanel recalculates active inventory automatically — unless a manual forecast is saved for that item, in which case statistical generation is skipped for that SKU.

### Clarifying questions

- Do you want me to compare scenarios first, or apply a specific one you've already chosen?
- Are we changing a single SKU or the company default?
- Should I write the result to the Live forecast, or just model it for review?

### Facts

- Kio can compare scenarios, update company forecast settings, recalculate, and write the forecast to Live.
- There are six built-in scenarios; Recency-weighted growth (ADGP–RWDR) is the common company default.
- Override hierarchy: per-inventory setting → product family → company default.
- Scenario output is a baseline; seasonality multipliers and manual overrides apply on top.
- Writing to Live is what makes a forecast drive replenishment; unsaved/modeled scenarios do not.
- The monthly recalculation runs on the 15th and is skipped for any SKU with a saved manual forecast.

### Don't claim

- Don't invent scenario names outside the six listed above.
- Don't claim a modeled/compared scenario is driving replenishment until it has been written to Live.

### Escalate when

- A scenario or settings update won't save, or an item keeps forecasting with the wrong model after a recalculation — this points to a NeonPanel app/config issue.

## Replenishment risk assessment: days of supply thresholds

### What it is

Replenishment risk measures how urgently a SKU needs restocking. It is determined by two days-of-supply calculations and their relationship to risk thresholds.

### Risk thresholds

| Category | FBA-only DOS | With inbound DOS | Meaning |
|---|---|---|---|
| **Critical** | < 14 days | Any | Stockout imminent — immediate action needed |
| **Warning** | 14–30 days | Any | Restock needed soon |
| **Healthy** | ≥ 30 days | ≥ 30 days | Adequate coverage |

### Facts

- Inbound shipments and pipeline inventory improve the "with inbound" days of supply, potentially moving a SKU from Critical to Warning.
- Risk status applies per SKU, not per product family — different variants under the same parent may have different risk levels.
- A critical SKU with a long lead time may be unrecoverable by standard PO — this is an escalation scenario.

## Checking inventory position for a specific SKU

### What it is

Checking inventory position for a specific SKU involves running the `supplychaininspectinventorysku_snapshot` tool and presenting the results.

### What to present

- FBA available, inbound, warehouse stock, WIP orders, and total coverage
- Days of supply — both FBA-only and with inbound
- Risk status derived from days-of-supply thresholds

### Facts

- The company and SKU are required before running the check.
- The snapshot is the starting point for any replenishment recommendation.
- Multiple SKUs under the same parent should each be checked independently.

## Two streams of inventory quantity data in NeonPanel

### What it is

NeonPanel maintains two parallel streams of inventory quantity data: **CALCULATED** data and **3PL** data.

### CALCULATED data

Derived from NeonPanel's own inventory management transactions: Inventory Orders, FBA Shipments, AWD Shipments, Non-Amazon Shipments, Invoices, and Inventory Adjustments.

### 3PL data

Sourced from external systems such as Amazon Ledger. These balances can be imported into NeonPanel via the Import feature.

### Key objects

- **CALCULATED Data**: Internal inventory tracking
- **3PL Data**: External warehouse/fulfillment data
- **Amazon Ledger**: One type of 3PL data source

### Facts

- The two streams may show different quantities for the same SKU because they originate from different sources.
- Discrepancies between CALCULATED and 3PL data may indicate a data integrity issue.
- 3PL data import is a manual process.

## PO planning intake: collecting the four required inputs

### What it is

PO planning intake is the process of collecting all required inputs before any analysis tools are called. Four categories must be confirmed.

### The four inputs

1. **Product identifier** — parent ASIN, child ASIN, SKU, or product family
2. **Forecast scenario** — which sales projection to use
3. **Planning parameters** — lead time, safety stock, PO cadence, horizon (or use company defaults)
4. **Scope** — which SKUs and marketplaces

### Facts

- All four inputs must be collected in a single structured request, not spread across multiple turns.
- Different SKUs under the same parent may have different configurations for each input.
- The agent should present the four inputs clearly and ask the user to confirm before proceeding.

## Two inventory data streams: CALCULATED vs. 3PL

### What it is

NeonPanel maintains two parallel streams of inventory quantity data: **CALCULATED** data and **3PL** data.

### CALCULATED data

Derived from NeonPanel's own inventory management transactions: Inventory Orders, FBA Shipments, AWD Shipments, Non-Amazon Shipments, Invoices, and Inventory Adjustments. This stream reflects inventory as tracked within NeonPanel's internal system.

### 3PL data

Sourced from external systems such as Amazon Ledger. These balances can be imported into NeonPanel via the Import feature. This stream reflects inventory as reported by the external warehouse or fulfillment provider.

### FBA replenishment distinction

For FBA replenishment shipments, NeonPanel uses **Amazon Ledger data** to calculate replenishment suggestions — not NeonPanel's own calculated inventory data.

### Facts

- The two streams may show different quantities for the same SKU because they originate from different sources and update at different cadences.
- When a significant discrepancy exists between CALCULATED and 3PL data for the same SKU, it may indicate a data integrity issue worth investigating.
- 3PL data import is a manual process initiated through NeonPanel's Import feature.
- Amazon Ledger is one type of 3PL data source; other external warehouse systems may also supply 3PL data.

## Forecast scenario mismatch

### What it is

A forecast scenario mismatch means a SKU is projecting on a different scenario than the user intended — often causing over- or under-forecasting. Kio can diagnose and correct this.

### How to resolve

- Confirm the current scenario via `forecasting_get_company_settings` (default) and `forecasting_get_sales_forecast_details` (what the item is actually using).
- Compare the current vs. intended scenario with `forecasting_compare_sales_forecast_scenarios` to quantify the difference.
- If a change is warranted, update settings (`forecasting_update_company_settings`) and/or recalculate (`forecasting_generate_sales_forecast`, `forecasting_run_sales_forecast_job`), then write to Live (`forecasting_write_sales_forecast`).
- For a single SKU that should differ from the company default, the user sets a per-item scenario in the Inventory Catalog; Kio can then recalculate and write it.

### Facts

- Kio can correct a mismatch by recalculating and writing the forecast to Live — it is not limited to reporting the mismatch.
- A corrected scenario only affects replenishment once written to Live.
- Different SKUs under the same parent may legitimately use different scenarios.
- Per-item scenario settings override the company default.

### Don't claim

- Don't claim the mismatch is fixed until the recalculated forecast has been written to Live.

### Escalate when

- The intended scenario won't persist after a successful write, or projections stay wrong after a completed recalculation.

## FBA replenishment: when a user asks about it

### What it is

When a user asks about FBA replenishment, they need to know how it works and what information is needed to get suggestions.

### What's needed

- Company and marketplace
- Specific SKU or ASIN (or "all SKUs")
- The key distinction: FBA replenishment uses Amazon Ledger data, not NeonPanel calculated inventory

### Facts

- The FBA replenishment tool always counts inbound units in total_fba_available_units.
- Amazon Ledger data must be imported for suggestions to be generated.
- The user may need to distinguish between FBA replenishment and PO planning.

## Identifier types: parent ASIN, child ASIN, SKU, and product family

### What it is

Product identifiers determine how supply chain tools and inventory lookups work. Four types exist, each with different resolution behavior.

### Identifier types

| Type | Description | Tool behavior |
|---|---|---|
| **Parent ASIN** | Groups all child variants | Supply chain tools accept parent_asin and auto-resolve children |
| **Child ASIN** | A specific variant | Resolved directly — no parent expansion |
| **SKU code** | Most granular identifier | Resolved directly to one record |
| **Product family** | Manually curated grouping | May span multiple parent ASINs — most reliable for completeness |

### Facts

- Parent ASINs should be passed directly to supply chain tools without pre-validation via neonpanel_listInventoryItems — the tools handle resolution natively.
- Product family is more reliable than parent ASIN for finding all variants because it is manually curated by the team, not dependent on Amazon catalog sync. Parent ASIN mappings can be stale or null for new variants.
- A marketplace (US, UK, etc.) is needed alongside the identifier to resolve correctly.

## PO quantity recommendation: the five-section format

### What it is

A PO quantity recommendation is presented in a structured five-section format. This ensures the user has all the information needed to make a decision.

### The five sections

| Section | Content |
|---|---|
| **Current Inventory** | FBA available, inbound, warehouse stock, WIP orders, days of supply |
| **Sales Forecast** | Velocity, projected demand, promotional adjustments |
| **Risk Assessment** | Risk status, stockout window, urgency |
| **PO Recommendation** | Recommended quantity, arrival window, supplier |
| **Next Steps** | What the user needs to do next |

### Facts

- Upcoming promotions or seasonal spikes should be factored into the recommendation.
- Each product variant appears in its own row with its own recommendation.
- The user should confirm the quantity before the PO is placed.
