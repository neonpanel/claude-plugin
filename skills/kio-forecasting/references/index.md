# Agent Knowledge Index

## Other references

- Brand and company context: `references/brand-neonpanel_brand_pack.md`
- MCP tool guidance: `references/tools.md`

## Knowledge areas

### Planning And Forecasting

File: `references/planning_and_forecasting.md`

- **Applying a forecast scenario in NeonPanel** — The statistical model NeonPanel uses to build a baseline. Kio can compare scenarios, update settings, recalculate, and write the forecast to Live — or the user can set it in the UI.
- **PO timing: recommended order dates and arrival windows** — PO timing is determined by lead time, target arrival, and PO cadence. The recommended order date is calculated backward from the desired arrival window.
- **Replenishment risk assessment: days of supply thresholds** — Replenishment risk is assessed through days of supply. Critical (<14 days FBA-only), Warning (14-30), and Healthy (>30 with inbound) thresholds guide urgency.
- **Checking inventory position for a SKU** — Inventory position is retrieved via supplychaininspectinventorysku_snapshot, showing FBA available, inbound, warehouse stock, and WIP orders.
- **Identifier types: parent ASIN, child ASIN, SKU, and product family** — Product identifiers come in four types. Parent ASIN is preferred for supply chain tools; product family is most reliable for cross-variant lookup.
- **Planning parameters: lead time, safety stock, PO cadence, and horizon** — Planning parameters (lead time, safety stock, PO cadence, horizon) may be SKU-specific or fall back to company defaults. Missing values are configuration gaps.
- **Planning scope: SKUs and marketplaces** — Planning scope defines which SKUs under a parent/family and which marketplaces are included. Each SKU is validated independently.
- **Confirming planning scope** — Planning scope is confirmed by listing all SKUs matching the identifier and asking the user to verify the count and marketplaces.
- **Two inventory data streams: CALCULATED vs. 3PL** — NeonPanel maintains two inventory data streams: CALCULATED (from NP transactions) and 3PL (from external sources like Amazon Ledger).
- **Diagnosing an inactive SKU** — An inactive SKU is not flagged as actively sold or planned. The planning_base parameter controls inclusion: 'actively_sold_only' or 'all'.
- **Missing lead time configuration** — Lead time may be missing at the SKU level. A company default may exist. If neither is configured, it is a critical configuration gap.
- **Missing safety stock configuration** — Safety stock may be missing at the SKU level. A company default with class multiplier may exist. If neither, it's a critical configuration gap.
- **SKU not found: causes and resolution** — A SKU may not be found if it is not set up for the marketplace, the identifier is incorrect, or the product family mapping is wrong.
- **Forecast scenario mismatch** — A mismatch is when a SKU is forecasting on a different scenario than intended. Kio can confirm the current scenario, compare options, and correct it by recalculating and writing to Live.
- **Presentation rules for product variants** — Product variants must be displayed individually, one per row. Never collapse variants into combined columns or 'other' buckets without user consent.
- **FBA replenishment shipments and Amazon Ledger data** — FBA replenishment uses Amazon Ledger data, not NeonPanel calculated inventory. Suggestions are calculated from Ledger balances and configured parameters.
- **FBA replenishment calculation logic** — FBA replenishment suggestions are calculated from Amazon Ledger data using configured parameters, not from NeonPanel calculated inventory.
- **FBA replenishment planning workflow** — FBA replenishment planning uses Amazon Ledger data. The user must have Ledger data imported for the system to generate suggestions.
- **FBA replenishment queries vs. PO planning** — FBA replenishment queries use Amazon Ledger data. The key distinction from PO planning is the data source and inventory scope.
- **FBA replenishment suggestions** — FBA replenishment suggestions are calculated from Amazon Ledger data using configured parameters. They account for FBA available + inbound inventory.
- **Forecast scenario management in NeonPanel** — Kio can read current scenario settings, compare scenarios, update company defaults, recalculate, and write the forecast to Live. Users can also set scenarios in the UI.
- **Selecting a forecast scenario for a SKU** — Kio can compare scenarios for a SKU and apply the best one by updating settings, recalculating, and writing to Live. The user can also select per-SKU scenarios in the Inventory Catalog.
- **Identifier validation and resolution** — Identifiers are validated by looking up SKUs via the product family or parent ASIN. Parent ASINs go directly to supply chain tools without pre-validation.
- **Two streams of inventory quantity data in NeonPanel** — NeonPanel has two data streams: CALCULATED (from NP transactions) and 3PL (from external sources like Amazon Ledger).
- **Inventory data streams: user queries** — When a user asks about inventory data, clarify whether they mean CALCULATED (NP transactions) or 3PL (external sources) data.
- **Inventory position components and risk metrics** — Inventory position has four components. Days of supply is calculated two ways: FBA-only and with inbound. Risk thresholds determine criticality.
- **Inventory position: components and retrieval** — Inventory position has four components: FBA available, inbound, warehouse stock, and WIP orders. Retrieved via supplychaininspectinventorysku_snapshot.
- **Checking inventory position for a specific SKU** — Inventory position is checked via supplychaininspectinventorysku_snapshot, showing FBA available, inbound, warehouse stock, and WIP orders with DOS.
- **Empty results from supply chain tools** — Empty results from a supply chain tool indicate the tool ran successfully but returned no data. The Empty Results Diagnosis skill provides a 5-step checklist.
- **FBA replenishment: when a user asks about it** — FBA replenishment uses Amazon Ledger data. The user needs a company, marketplace, and SKU/ASIN to get suggestions.
- **Forecast scenarios: checking and resolving assignments** — Kio checks scenario/forecast state with the forecasting tools, compares options, and resolves issues by updating settings, recalculating, and writing to Live.
- **Inventory data streams: when a user asks about them** — When a user asks about inventory data streams, clarify whether they mean CALCULATED (NP transactions) or 3PL (external sources) data.
- **Parent ASIN lookup: trust and resolve** — Parent ASINs are trusted immediately and passed directly to supply chain tools. Do not pre-validate with neonpanel_listInventoryItems.
- **PO planning intake: collecting the four required inputs** — PO planning intake requires four inputs: identifier, forecast scenario, planning parameters, and scope. All four are collected in a single structured request.
- **Planning and Forecasting module overview** — The Planning and Forecasting module combines historical data with promotional goals and plans to forecast sales velocity and inventory needs.
- **Planning parameters explained** — Planning parameters (lead time, safety stock, PO cadence, horizon) are SKU-level or company-default settings that control PO recommendations.
- **Four required inputs for PO planning** — PO planning requires four validated inputs: identifier type, forecast scenario, planning parameters, and scope. All four must be confirmed before any tool calls.
- **PO planning request: starting the workflow** — A PO planning request starts with the company, marketplace, and product identifier. The full 7-phase procedure is handled by the PO Planning Workflow skill.
- **Product family lookup: two-step resolution** — Product family lookup uses a two-step process: query by parent ASIN first, then cross-check by product_family field for completeness.
- **Product family resolution: two-step lookup** — Product family resolution uses a two-step lookup: parent ASIN first, then product_family cross-check. Orphaned variants with null parent ASIN are flagged.
- **Promotional planning: accounting for demand lifts** — Promotional planning accounts for demand lifts from promotions, deals, or ranking targets. Historical data alone cannot predict promo-driven velocity.
- **PO quantity recommendation: the five-section format** — PO quantity recommendations are presented in five sections: Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps.
- **SKU lookup resolution by identifier type** — SKU lookup resolution depends on the identifier type: parent ASINs go to supply chain tools, while child ASINs and SKU codes resolve directly.
- **Understanding inventory data streams** — NeonPanel has two data streams: CALCULATED (from NP transactions) and 3PL (from external sources like Amazon Ledger). They may show different values.
- **Sales velocity calculation modes and weighting** — Velocity can use 'current' mode (weighted trailing averages) or 'planned' mode (arrival-anchored multi-month average). Custom velocity weighting is configurable.

### Forecast Workflow Discipline (skill under planning_and_forecasting)

File: `references/forecast_workflow_discipline.md`

- **Rules for handling user-provided scenario names, labels, and formatting conventions** — What it is Rules for handling user-provided scenario names, labels, and formatting conventions. The agent must use the user's exact naming — never rename, relabel, or 'improve' user-provided identifie
- **Commit Forecast Data** — Clarifying questions What scenario name should this be saved under? Can you confirm the SKUs and date range included in this write? Should this overwrite the existing scenario or create a new version?
- **The step-by-step discipline for writing forecast data: audit current plan against actuals, show the gap, dry run the proposed write, get explicit user approval, commit to the live database, and summarize the audit trail** — What it is The step-by-step discipline for writing forecast data: audit current plan against actuals, show the gap, dry run the proposed write, get explicit user approval, commit to the live database,
- **Audit Forecast Changes** — Clarifying questions Which scenario or time period do you want to review? Should I compare against actuals or a previous forecast version? How to help Retrieve the committed scenario data Show what wa
- **Audit Trail Review** — Procedure Identify the scope. Ask which scenario, time period, or SKU set the user wants to review. Retrieve committed data. Pull the saved scenario data including scenario name, save date, and attrib
- **Verified Facts** — Facts Always compare proposed forecast adjustments against the current plan and actuals so the user sees the gap before approving changes. When saving forecast scenarios, use the exact name and format

### Po Planning Workflow (skill under planning_and_forecasting)

File: `references/po_planning_workflow.md`

- **Po Data Validation** — How to help Check company exists and user has access Check SKU/ASIN exists in the specified company and marketplace Check forecast scenario exists and is assigned to the SKU Check lead time and safety
- **Phase 1 of PO planning: collecting all required inputs (identifier type, forecast scenario, planning parameters, scope) in a single structured request before making any tool calls** — What it is Phase 1 of PO planning: collecting all required inputs (identifier type, forecast scenario, planning parameters, scope) in a single structured request before making any tool calls. Key obje
- **Phase 2: Data Validation** — Procedure Once inputs are confirmed, validate each before running analysis tools. Check company exists and user has access. If user provided a parent ASIN: Trust their input. Do NOT call neonpanellist
- **Phase 5: Forecast Scenario Mismatch Handling** — Procedure When the user requests a specific scenario but the SKU is using a different one, report the mismatch immediately: Provide step-by-step NeonPanel instructions: Go to NeonPanel → Inventory Pla
- **Po Planning Intake** — Clarifying questions Is that identifier a parent ASIN, child ASIN, SKU, or product family code? Which forecast scenario should I use? (e.g., promoshift128growthv2, default, or current?) Should I use c
- **Phase 1: Intake & Clarification** — Procedure When a user says "plan POs for company X, marketplace Y, product Z", immediately ask for ALL of the following in a single structured request: Identifier clarity: Is the identifier a parent A
- **Phase 6 of PO planning: detecting and handling forecast scenario mismatches — when the user requests a specific scenario but the SKU is assigned a different one** — What it is Phase 6 of PO planning: detecting and handling forecast scenario mismatches — when the user requests a specific scenario but the SKU is assigned a different one. Key objects Current scenari
- **Verified Facts** — Facts PO planning requires four validated inputs before any tool calls: identifier type, forecast scenario, planning parameters (lead time, safety stock, PO cadence, horizon), and scope. All intake cl
- **Phase 3 of PO planning: executing tools in the correct order — (1) listInventoryItems, (2) inspect_inventory_sku_snapshot, (3) list_product_logistics_parameters, (4) list_po_placement_candidates** — What it is Phase 3 of PO planning: executing tools in the correct order — (1) listInventoryItems, (2) inspectinventoryskusnapshot, (3) listproductlogisticsparameters, (4) listpoplacementcandidates. Ke
- **Phase 7 of PO planning: knowing when to escalate — data integrity issues, missing critical configuration, unexpected tool results, or user decisions required** — What it is Phase 7 of PO planning: knowing when to escalate — data integrity issues, missing critical configuration, unexpected tool results, or user decisions required. Key objects Data integrity iss
- **Po Escalation** — Clarifying questions Can you describe what you're seeing that looks wrong? How to help Summarize all findings collected so far Explain the specific gap or issue clearly Present options with trade-offs
- **Phase 3: Tool Sequencing** — Procedure Execute tools in this order — adapting based on identifier type: If parent ASIN provided (preferred path): Step 1: supplychaininspectinventoryskusnapshot with parentasin: ["B0D45BYWQR"] — Re
- **Phase 4: Result Presentation** — Procedure Present results in exactly 5 sections using this template: Always state a clear recommendation — never end with "Option 1 or Option 2?" without recommending one. Calculate days of supply two
- **Po Scenario Mismatch** — Clarifying questions Do you want me to proceed with the current scenario, or wait for you to apply the requested one? How to help Report the mismatch clearly: current scenario vs. requested scenario E
- **Po Result Presentation** — How to help Present results in exactly 5 sections: (1) Current Inventory — available FBA, inbound, warehouse stock, WIP orders, total coverage; (2) Sales Forecast — current velocity, peak velocity, 30
- **Po Tool Execution** — How to help Execute tools in strict order: (1) neonpanellistInventoryItems, (2) supplychaininspectinventoryskusnapshot, (3) supplychainlistproductlogisticsparameters, (4) supplychainlistpoplacementcan
- **Phase 2 of PO planning: validating all inputs exist and are correctly configured before running analysis tools** — What it is Phase 2 of PO planning: validating all inputs exist and are correctly configured before running analysis tools. Checks SKU existence, scenario assignment, lead time, and safety stock. Key o
- **Phase 5 of PO planning: presenting results in a structured 5-section format — Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps** — What it is Phase 5 of PO planning: presenting results in a structured 5-section format — Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps. Key objects Current Inve
- **Phase 6: Escalation** — Procedure Escalate when any of these conditions are met: Data integrity: SKU exists in one system but not another Missing critical config: No lead time AND no company default Unexpected results: Same

## Knowledge base

### Forecast Scenarios: How They Work and How to Choose

Article 9007 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9007-forecast-scenarios-how-they-work-and-how-to-choose.md`
Understand how NeonPanel's twelve forecast scenarios use recent sales, longer-term averages, trends, growth targets, and seasonality—and how to choose the right approach for each product.

### Flat Scenario: Hold Your Current Sales Level Steady

Article 9008 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9008-flat-scenario-hold-your-current-sales-level-steady.md`
Holds demand at your latest level, with no planned growth or decline.

### Growth 10: Plan a Modest, Steady Increase

Article 9009 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9009-growth-10-plan-a-modest-steady-increase.md`
Works your latest level up toward about 10% growth over a year.

### Growth 20: Plan a Stronger, Measured Expansion

Article 9010 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9010-growth-20-plan-a-stronger-measured-expansion.md`
Plans for about 20% growth over the year ahead.

### Growth 30: Plan an Aggressive Expansion Curve

Article 9011 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9011-growth-30-plan-an-aggressive-expansion-curve.md`
An aggressive plan targeting about 30% growth over the year ahead.

### Seasonal Naive: Carry Your Latest Selling Month Forward

Article 9012 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9012-seasonal-naive-carry-your-latest-selling-month-forward.md`
Carries your most recent selling month forward and reshapes it with your seasonality.

### 12-Month Moving Average: A Steady Baseline From Your Last Twelve Selling Months

Article 9013 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9013-12-month-moving-average-a-steady-baseline-from-your-last-twe.md`
Averages up to twelve selling months, then applies your seasonality.

### Trend and Seasonality: For Products That Climb or Fall at a Steady Pace

Article 9014 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9014-trend-and-seasonality-for-products-that-climb-or-fall-at-a-s.md`
Extends the direction your sales have been heading, then applies your seasonality.

### Robust Low: A Smoothed Blend of Your Recent Average and Your Latest Month

Article 9015 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9015-robust-low-a-smoothed-blend-of-your-recent-average-and-your-.md`
Blends your twelve-month average with your latest month to smooth out swings.

### Availability-Based: Grow Recent Sales at a Set Annual Rate

Article 9016 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9016-availability-based-grow-recent-sales-at-a-set-annual-rate.md`
Takes recent sales and grows them by a set annual rate.

### Recency-Weighted Daily Rate: Follow Your Recent Sales Per Day

Article 9017 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9017-recency-weighted-daily-rate-follow-your-recent-sales-per-day.md`
Follows your recent sales per day, with unusually high months trimmed back.

### Recency-Weighted Conservative: Follow Your Recent Momentum Up or Down

Article 9018 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9018-recency-weighted-conservative-follow-your-recent-momentum-up.md`
Follows your recent momentum, up or down.

### Recency-Weighted Growth: A Multi-Window Trend That Leans Toward Growth

Article 9019 · Forecast Scenarios · Updated 2026-08-24
File: `references/kb/9019-recency-weighted-growth-a-multi-window-trend-that-leans-towa.md`
Combines your recent level with several trend windows and leans toward growth.
