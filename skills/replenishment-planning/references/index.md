# Knowledge index

## Other references

- Brand and company context: `references/brand-neonpanel_brand_pack.md`
- MCP tool guidance: `references/tools.md`

## Knowledge areas

### Inventory Ops

File: `references/inventory_ops.md`

- **Sku Lookup** — Clarifying questions Is that a parent ASIN, child ASIN, SKU code, or product family name? Which marketplace — US, UK, or another? How to help Use neonpanel_listInventoryItems to search for the identif
- **Check Inventory Position** — Clarifying questions Which SKU or ASIN are you checking? Which company and marketplace? How to help Run supplychaininspectinventorysku_snapshot to get current position Calculate days of supply FBA-onl
- **Inventory Position** — Clarifying questions Which SKU or ASIN do you want inventory levels for? Do you want FBA-only or total inventory across all warehouses? How to help Use supplychaininspectinventorysku_snapshot to get c
- **Warehouse Check** — Clarifying questions Which company should I check? How to help Run neonpanel_listWarehouses for the company Check for clones (same name, different IDs) Identify AWD warehouses and flag that inventory
- **Verified Facts** — Facts A parent ASIN may map to multiple child ASINs and SKUs. Always resolve to specific SKUs before running inventory analysis. SKU lookup must specify both company ID and marketplace — the same SKU
- **Sku Lookup Resolution** — Clarifying questions What identifier do you have — parent ASIN, child ASIN, or SKU code? Which marketplace? How to help If parent ASIN: go directly to supply chain tools (they resolve children automat
- **Inventory Position Check** — Clarifying questions Which company and SKU? How to help Run supplychaininspectinventorysku_snapshot for the SKU Break down: FBA available, inbound, warehouse stock, WIP orders, total coverage Calculat
- **Warehouse Types And Clones** — Clarifying questions Which company are you checking warehouses for? How to help Run neonpanel_listWarehouses to list all warehouses Group by name to detect clones (same name, different IDs) Identify A

### Diagnose Lost Batches Cogs (skill under inventory_ops)

File: `references/diagnose_lost_batches_cogs.md`

- **Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process** — What it is Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process. Phase 1 uses cogsanalyzefifocogs (lostcogs mode) for COGS quality assessment. Phase 2
- **Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works** — What it is Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in
- **Warehouse Fork Assessment** — Clarifying questions What warehouse is shown on the lost COGS transaction? Is it Lost & Found or an actual warehouse name like Amazon FBA US? How to help If Lost & Found: stock was not on the selling
- **Aggregate Pattern Analysis** — Clarifying questions Do you want to look at all lost batches or filter by transaction type (invoices vs. shipments)? Is there a specific date range to focus on? How to help Pull lost batches sorted by
- **Verified Facts** — Facts Lost COGS diagnosis has two entry points: 'I have lost COGS' starts at Phase 1 (COGS triage with the Warehouse Fork), while 'I have lost batches' starts directly at Phase 2 (general lost batches
- **Tool Selection Lost Cogs Vs Batches** — Clarifying questions Are you looking at lost COGS (sales with $0 cost) or lost batches in general (across all transaction types)? Do you need to include shipment-level lost batches in your investigati
- **Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal that determines the nature of the problem** — What it is Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal
- **Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches** — What it is Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches. Uses cogslistlostbatches to pull ALL lost batches across all transaction types (invoices, s
- **Diagnostic Tool Selection** — Clarifying questions Are you looking for lost COGS specifically or all lost batches? Do you need to investigate programmatically or via the UI reports? How to help For lost COGS (Phase 1): use FIFO Tr
- **Tools & Reports Quick Reference** — Procedure | Tool / Report | When to Use | |---|---| | FIFO Transactions → COGS Quality tab | Identify lost COGS — sales with $0 cost assignment | | FIFO Transactions → Lost Batches tab | Identify all
- **Phase 2: Lost Batches General Investigation** — Procedure Start with the aggregate view — never individual SKUs first. Pull lost batches data using cogslistlost_batches. This tool captures ALL transaction types — invoices, shipments, adjustments, a
- **Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for unambiguous cases, the hard limit (cannot detect absence of data never entered), and escalation rules for locked periods, systematic issues, and bulk operations** — What it is Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for
- **Lost Batches Diagnosis** — Clarifying questions Which SKU(s) or inventory IDs are affected? What time period are the lost batches occurring in? Have you already identified any patterns (specific shipments, POs, warehouses)? How
- **Surrounding Transaction Analysis** — Clarifying questions Which specific SKU or inventory ID are we investigating? Do you have the transaction dates for the lost batch? How to help Check Lost & Found warehouse transactions for this item
- **Phase 2 Continued: Formulate Hypotheses & Guide the User** — Procedure Formulate hypotheses — with care. Only after examining the full transaction context. Possibilities include: Date ordering issue — inbound and outbound dates need correcting so inbound preced
- **Phase 1: Lost COGS Triage** — Procedure Assess overall COGS quality. Use cogsanalyzefifocogs (analysismode: lost_cogs) to pull COGS quality data. Aggregate by SKU, brand, or company-wide depending on the user's scope. Surface the
- **Diagnostic Guidance Limits** — Clarifying questions Have you already attempted any fixes? If so, what did you try? Is the affected period locked? How to help Surface data, explain mechanics, and point the user in the right directio
- **Decision Flow Summary** — Procedure
- **Lost Cogs Diagnosis** — Clarifying questions Do you want to check a specific SKU, brand, or the entire company? What time period are you concerned about? Do you have an investigation threshold — a minimum unit count or dolla

### Diagnosing Lost Batches Lost Cogs (skill under inventory_ops)

File: `references/diagnosing_lost_batches_lost_cogs.md`

- **Reference for which NeonPanel tools and reports to use during diagnosis: FIFO Transactions report (COGS Quality, Lost Batches, Transaction List tabs), cogs_analyze_fifo_cogs tool, Statement Analytics, Landed Cost Analysis, Inventory Audit, and search_neonpanel_project_url** — What it is Reference for which NeonPanel tools and reports to use during diagnosis: FIFO Transactions report (COGS Quality, Lost Batches, Transaction List tabs), cogsanalyzefifocogs tool, Statement An
- **Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works** — What it is Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in
- **Formulating hypotheses after investigation (date ordering issues, missing POs, missing shipments, wrong warehouse assignments, warehouse clones, onboarding residue, anomalous Amazon adjustments, missing bill connections) and guiding the user toward resolution — prescribing only when evidence is unambiguous** — What it is Formulating hypotheses after investigation (date ordering issues, missing POs, missing shipments, wrong warehouse assignments, warehouse clones, onboarding residue, anomalous Amazon adjustm
- **Phase 2 diagnostic: triaging lost batches by aggregate impact, identifying shipment/PO-level patterns, analyzing surrounding transactions (backwards and forwards in time), checking warehouse IDs vs names, spotting volume anomalies, and detecting onboarding residue** — What it is Phase 2 diagnostic: triaging lost batches by aggregate impact, identifying shipment/PO-level patterns, analyzing surrounding transactions (backwards and forwards in time), checking warehous
- **Lost Batch Tools Reference** — Clarifying questions What specifically are you trying to investigate — lost COGS, lost batches, or both? How to help For lost COGS: use FIFO Transactions report → COGS Quality tab, or cogsanalyzefifoc
- **Verified Facts** — Facts Lost batches are transactions where the FIFO engine could not find or assign a source batch for units being moved — the FIFO queue was empty or no matching inventory existed in the expected ware
- **Phase 2: Lost Batches General Approach** — Procedure Triage — Aggregate view first. Do NOT start with individual SKUs. Pull lost batches data sorted by impact (units or dollar amount, descending). Look for shipment-level or PO-level patterns:
- **Phase 1 diagnostic: assessing COGS quality, identifying worst offenders by SKU/brand, and applying the Warehouse Fork — checking whether the lost COGS transaction is on Lost & Found (stock never arrived) vs** — What it is Phase 1 diagnostic: assessing COGS quality, identifying worst offenders by SKU/brand, and applying the Warehouse Fork — checking whether the lost COGS transaction is on Lost & Found (stock
- **Lost Batch Hypotheses** — Clarifying questions Based on the transaction analysis, which hypothesis seems most likely to you? Do you have access to external records (e.g., supplier POs, shipping confirmations) that could confir
- **Warehouse Fork Analysis** — Clarifying questions What warehouse is shown on the lost COGS transaction? Is it showing Lost & Found or an actual warehouse name? How to help If Lost & Found: the stock was not on the selling warehou
- **Lost Batches Diagnosis** — Clarifying questions Which company and time period are you looking at? Are you seeing lost batches on shipments, invoices, or both? Have you already identified specific SKUs or is this a general conce
- **Transaction Tracing** — Clarifying questions Which SKU/inventory ID are you investigating? What date range should I look at for surrounding transactions? How to help Check Lost & Found warehouse transactions for this item Id
- **Phase 1: Lost COGS Triage** — Procedure Assess overall COGS quality using cogsanalyzefifocogs with analysismode 'lost_cogs'. Ask the user for scope: specific SKU, brand, or entire company. Ask for time period and investigation thr
- **Lost Cogs Diagnosis** — Clarifying questions Do you want to check a specific SKU, brand, or the entire company? What time period are you concerned about? Do you have an investigation threshold — a minimum unit count or dolla
- **Quick-Reference Decision Flow** — Procedure User reports lost COGS → Start Phase 1 (Warehouse Fork). Lost & Found → Stock never arrived → Phase 2. Actual warehouse → Stock arrived with $0 cost → Find inbound shipment → Phase 2 for tha

### Empty Results Diagnosis (skill under inventory_ops)

File: `references/empty_results_diagnosis.md`

- **Diagnose Empty Results** — Clarifying questions Which tool returned empty results? What SKU/ASIN and company were you querying? How to help Follow the 5-check diagnostic tree in strict order: Check 1: Does the SKU exist in the
- **After identifying the root cause via the diagnostic tree, providing clear resolution steps — what the user needs to configure in NeonPanel and what the agent should re-run after** — What it is After identifying the root cause via the diagnostic tree, providing clear resolution steps — what the user needs to configure in NeonPanel and what the agent should re-run after. Key object
- **Verified Facts** — Facts When a planning tool returns empty results, follow the 5-check diagnostic tree in strict order: (1) SKU in snapshot, (2) leadtimedays, (3) safetystockdays, (4) forecast scenario, (5) actively so
- **The 5-check diagnostic decision tree for when supply_chain_list_po_placement_candidates or other planning tools return empty results** — What it is The 5-check diagnostic decision tree for when supplychainlistpoplacementcandidates or other planning tools return empty results. Checks: SKU existence → lead time → safety stock → forecast
- **5-Check Diagnostic Tree** — Procedure When a planning tool (especially supplychainlistpoplacementcandidates) returns empty results, follow these 5 checks in strict order. Do NOT skip checks or re-run the original tool. Check 1:
- **Post-Fix Verification** — Procedure After the user reports they've made a configuration change, re-run ONLY the specific check that failed. If the check now passes, offer to re-run the full PO planning workflow from Phase 3 (t
- **Resolve Root Cause** — Clarifying questions Have you made the configuration change in NeonPanel yet? How to help Based on the diagnostic result, provide specific NeonPanel navigation steps to fix the issue For missing lead

### Inventory Transactions (skill under inventory_ops)

File: `references/inventory_transactions.md`

- **When and how to correct erroneous transactions — covering reversals, adjustments, and re-entry, with guidance on which method is appropriate for each scenario** — What it is When and how to correct erroneous transactions — covering reversals, adjustments, and re-entry, with guidance on which method is appropriate for each scenario. Key objects reversal adjustme
- **Tool Execute Sql** — Clarifying questions Is this a read query or a write query? Which table or dataset are you targeting? How to help Use executesql for ad-hoc queries and data inspection For schema changes or bulk data
- **Triage: User-Generated vs. Integration-Sourced Documents** — Procedure Run this check first whenever a sales document (invoice) or shipment is implicated in a discrepancy. Determine the source of the document: Integration-sourced (Amazon, Shopify, or other plat
- **How to Correct a Transaction Error** — Procedure Use this playbook to fix wrong dates, quantities, costs, or warehouse assignments without corrupting FIFO sequencing. Identify the error type: Wrong date → reversal + re-entry required Wrong
- **Tool Apply Migration** — Clarifying questions What migration are you trying to apply? Has the migration been tested in a staging environment first? How to help Confirm the migration script and target environment before execut
- **Transaction Type Quick Reference** — Procedure | Transaction Type | Affects Quantity | Affects Cost | Generates COGS | Notes | |---|---|---|---|---| | Inbound Shipment | ✅ | ✅ (base cost) | ❌ | Brings units in at base cost from PO or pri
- **Verified Facts** — Facts An inbound shipment brings units into a warehouse at a base cost sourced from the originating PO or prior warehouse; a bill does not transfer cost independently — it adds capitalizable costs (fr
- **A taxonomy of all transaction types in NeonPanel — what each one does, whether it affects quantity, cost, or both, and which warehouses are involved** — What it is A taxonomy of all transaction types in NeonPanel — what each one does, whether it affects quantity, cost, or both, and which warehouses are involved. Key objects inbound shipment outbound s
- **How to Read a Transaction Timeline for a SKU** — Procedure Use this playbook whenever investigating an L&F pull, unexpected COGS figure, or inventory discrepancy. Identify the SKU and intended warehouse under investigation. Export the full transacti
- **How to read and interpret a full transaction timeline for a SKU in a given warehouse — the foundational skill used in all L&F and COGS investigations** — What it is How to read and interpret a full transaction timeline for a SKU in a given warehouse — the foundational skill used in all L&F and COGS investigations. Key objects transaction timeline SKU w

### Inventory Validation (skill under inventory_ops)

File: `references/inventory_validation.md`

- **Verifying warehouse identity and type: detecting warehouse clones (same name, different IDs), distinguishing AWD from FBA, and confirming warehouse types (FBA, AWD, 3PL, own)** — What it is Verifying warehouse identity and type: detecting warehouse clones (same name, different IDs), distinguishing AWD from FBA, and confirming warehouse types (FBA, AWD, 3PL, own). Key objects W
- **Verify Planning Config** — How to help For each SKU, check: leadtimedays (SKU-level or company default), safetystockdays (SKU-level or company default), forecast scenario assignment, planningbase status (actively sold) Use supp
- **Warehouse Clone Detection** — Procedure Run neonpanel_listWarehouses for the company. Group warehouses by name. If any name appears more than once with different IDs, flag as potential clone: Identify all AWD warehouses and flag t
- **Inventory Position Analysis** — Procedure For each validated SKU (including orphaned variants found via product family), compile the full inventory position: FBA Available: Current sellable stock in FBA warehouses. Inbound: Units in
- **Verified Facts** — Facts A parent ASIN may map to multiple child ASINs and SKUs. Always resolve to specific SKUs before running any inventory or planning analysis. SKU lookup must specify both company ID and marketplace
- **Verifying that all required planning configuration exists for a SKU: lead time, safety stock, forecast scenario assignment, planning status (actively sold), and vendor/MOQ settings** — What it is Verifying that all required planning configuration exists for a SKU: lead time, safety stock, forecast scenario assignment, planning status (actively sold), and vendor/MOQ settings. Key obj
- **Planning Configuration Verification** — Procedure For each SKU in scope (including any orphaned variants found via product family cross-check), check these four configuration items: Lead time: Run supplychainlistproductlogisticsparameters.
- **Resolve Sku Identifier** — Clarifying questions Is that a parent ASIN, child ASIN, SKU code, or product family name? Which marketplace — US, UK, or another? How to help Use neonpanel_listInventoryItems with the identifier and c
- **Resolving product identifiers to specific SKUs: handling parent ASINs (which map to multiple children), child ASINs, SKU codes, and product family names** — What it is Resolving product identifiers to specific SKUs: handling parent ASINs (which map to multiple children), child ASINs, SKU codes, and product family names. Confirming the correct marketplace.
- **Parent Asin Direct Lookup** — Clarifying questions Which marketplace should I use for this parent ASIN? Should I include all child SKUs or specific ones? How to help Trust the user's identifier — do NOT call neonpanellistInventory
- **Verify Warehouses** — Clarifying questions Which company should I check warehouses for? How to help Use neonpanel_listWarehouses to get full warehouse list Check for warehouse clones: same name but different IDs Identify w
- **SKU Identifier Resolution** — Procedure Determine the identifier type: parent ASIN, child ASIN, SKU code, or product family name. Confirm the marketplace (US, UK, EU, etc.) and company ID. If parent ASIN: Do NOT call neonpanellist

### Lost Batches Cogs Diagnosis (skill under inventory_ops)

File: `references/lost_batches_cogs_diagnosis.md`

- **Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process** — What it is Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process. Phase 1 uses cogsanalyzefifocogs (lostcogs mode) for COGS quality assessment. Phase 2
- **Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works** — What it is Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in
- **Warehouse Fork Assessment** — Clarifying questions What warehouse is shown on the lost COGS transaction? Is it Lost & Found or an actual warehouse name like Amazon FBA US? How to help If Lost & Found: stock was not on the selling
- **Advanced Pattern A: Offsetting Balances Cross-Check** — Procedure Use this when the transaction timeline investigation hits a dead end and the root cause cannot be identified. Trigger condition: You've meticulously checked the transaction timeline for the
- **Verified Facts** — Facts Lost COGS diagnosis has two entry points: 'I have lost COGS' starts at Phase 1 (COGS triage with the Warehouse Fork), while 'I have lost batches' starts directly at Phase 2 (general lost batches
- **Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal that determines the nature of the problem** — What it is Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal
- **Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches** — What it is Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches. Uses cogslistlostbatches to pull ALL lost batches across all transaction types (invoices, s
- **Offsetting Balances Cross Check** — Clarifying questions Have you already traced the full transaction timeline for this SKU and hit a dead end? Which warehouse is showing the L&F deficit? Do you have access to the Inventory Audit report
- **Core Mechanics Reference** — Procedure Shipments vs. Bills — How Landed Cost Is Built An inbound shipment picks up goods at the base cost it found them at (e.g., from a PO or previous warehouse). It physically brings the units in
- **Tools & Reports Quick Reference** — Procedure | Tool / Report | When to Use | |---|---| | FIFO Transactions → COGS Quality tab | Identify lost COGS — sales with $0 cost assignment | | FIFO Transactions → Lost Batches tab | Identify all
- **Phase 2: Lost Batches General Investigation** — Procedure Start with the aggregate view — never individual SKUs first. Pull lost batches data using cogslistlost_batches. This tool captures ALL transaction types — invoices, shipments, adjustments, a
- **Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for unambiguous cases, the hard limit (cannot detect absence of data never entered), and escalation rules for locked periods, systematic issues, and bulk operations** — What it is Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for
- **Safety Stock Timebomb** — Clarifying questions Is this an Amazon FBA warehouse showing sudden, massive L&F deficits? Does the recent supply chain data look clean — no obvious recent failures? When did the client onboard to Neo
- **Phase 2 Continued: Formulate Hypotheses & Guide the User** — Procedure Formulate hypotheses — with care. Only after examining the full transaction context. Possibilities include: Date ordering issue — inbound and outbound dates need correcting so inbound preced
- **Phase 1: Lost COGS Triage** — Procedure Assess overall COGS quality. Use cogsanalyzefifocogs (analysismode: lost_cogs) to pull COGS quality data. Aggregate by SKU, brand, or company-wide depending on the user's scope. Surface the
- **Advanced Pattern B: Safety Stock Timebomb** — Procedure Use this when Amazon FBA suddenly shows massive L&F deficits but recent supply chain data looks flawless. Trigger condition: A client using automated Amazon FBA fulfillment gets sudden, mass
- **Decision Flow Summary** — Procedure

### Planning And Forecasting

File: `references/planning_and_forecasting.md`

- **Applying a forecast scenario in NeonPanel** — The statistical model NeonPanel uses to build a baseline. The forecasting tools can compare scenarios, update settings, recalculate, and write the forecast to Live — or the user can set it in the UI.
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
- **Forecast scenario mismatch** — A mismatch is when a SKU is forecasting on a different scenario than intended. The forecasting tools can confirm the current scenario, compare options, and correct it by recalculating and writing to Live.
- **Presentation rules for product variants** — Product variants must be displayed individually, one per row. Never collapse variants into combined columns or 'other' buckets without user consent.
- **FBA replenishment shipments and Amazon Ledger data** — FBA replenishment uses Amazon Ledger data, not NeonPanel calculated inventory. Suggestions are calculated from Ledger balances and configured parameters.
- **FBA replenishment calculation logic** — FBA replenishment suggestions are calculated from Amazon Ledger data using configured parameters, not from NeonPanel calculated inventory.
- **FBA replenishment planning workflow** — FBA replenishment planning uses Amazon Ledger data. The user must have Ledger data imported for the system to generate suggestions.
- **FBA replenishment queries vs. PO planning** — FBA replenishment queries use Amazon Ledger data. The key distinction from PO planning is the data source and inventory scope.
- **FBA replenishment suggestions** — FBA replenishment suggestions are calculated from Amazon Ledger data using configured parameters. They account for FBA available + inbound inventory.
- **Forecast scenario management in NeonPanel** — The forecasting tools can read current scenario settings, compare scenarios, update company defaults, recalculate, and write the forecast to Live. Users can also set scenarios in the UI.
- **Selecting a forecast scenario for a SKU** — The forecasting tools can compare scenarios for a SKU and apply the best one by updating settings, recalculating, and writing to Live. The user can also select per-SKU scenarios in the Inventory Catalog.
- **Identifier validation and resolution** — Identifiers are validated by looking up SKUs via the product family or parent ASIN. Parent ASINs go directly to supply chain tools without pre-validation.
- **Two streams of inventory quantity data in NeonPanel** — NeonPanel has two data streams: CALCULATED (from NP transactions) and 3PL (from external sources like Amazon Ledger).
- **Inventory data streams: user queries** — When a user asks about inventory data, clarify whether they mean CALCULATED (NP transactions) or 3PL (external sources) data.
- **Inventory position components and risk metrics** — Inventory position has four components. Days of supply is calculated two ways: FBA-only and with inbound. Risk thresholds determine criticality.
- **Inventory position: components and retrieval** — Inventory position has four components: FBA available, inbound, warehouse stock, and WIP orders. Retrieved via supplychaininspectinventorysku_snapshot.
- **Checking inventory position for a specific SKU** — Inventory position is checked via supplychaininspectinventorysku_snapshot, showing FBA available, inbound, warehouse stock, and WIP orders with DOS.
- **Empty results from supply chain tools** — Empty results from a supply chain tool indicate the tool ran successfully but returned no data. The Empty Results Diagnosis skill provides a 5-step checklist.
- **FBA replenishment: when a user asks about it** — FBA replenishment uses Amazon Ledger data. The user needs a company, marketplace, and SKU/ASIN to get suggestions.
- **Forecast scenarios: checking and resolving assignments** — Scenario/forecast state can be checked with the forecasting tools, options compared, and issues resolved by updating settings, recalculating, and writing to Live.
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

### Accounting

File: `references/accounting.md`

- **Stock Risk** — Clarifying questions Which warehouse or company? What is your risk threshold? How to help use supplychainliststockreplenishmentriskitems tool identify high-risk items provide mitigation recommendation
- **Replenishment Analysis** — Clarifying questions Which warehouse or company? What is your target service level? How to help use supplychainlistfbareplenishment_candidates tool identify at-risk items provide reorder recommendatio
- **Metric Drilldown** — Clarifying questions Which metric or report are you looking at? Do you want the breakdown by SKU, category, or warehouse? What date range should I analyze? How to help ALWAYS drill down to the most gr
- **Landed cost tracking, freight and duty allocation, and landed cost analytics** — What it is Landed cost tracking, freight and duty allocation, and landed cost analytics. Covers how additional costs (shipping, customs, duties) are allocated to inventory items. Key objects Landed co
- **Velocity Analysis** — Clarifying questions Which product or warehouse? What time period? How to help use supplychainanalyzesalesvelocity tool explain velocity trends provide demand insights reference velocity dashboard Don
- **Artifact Requests** — Accounting Artifact Requests Integration name Sync job ID and timestamp Bill/Invoice IDs Vendor name(s) Mapping export or chart of accounts export Screenshots: sync status, error message Date range an
- **Landed Cost Analytics** — Clarifying questions Which warehouse or period? What metrics are you interested in? How to help explain landed cost dashboard guide on health metrics provide interpretation guidance reference analytic
- **Account and category mapping** — What it is Account and category mapping. Key objects Account Category Mapping How to help map accounts category mismatch Clarifying questions Not understanding that mapping changes affect historical d
- **Transaction Mapping** — Clarifying questions Which transaction type? (Inventory, Sales, Purchases) Is this for understanding or troubleshooting? How to help explain transaction mapping reference transaction map guide clarify
- **Agl Setup** — Clarifying questions Are you setting up AGL for the first time? Do you have the AGL vendor configured? How to help provide AGL setup steps confirm custom FBA freight services validate default AGL acco
- **Starting Balances** — Clarifying questions Which warehouse or company? Do you have the balance data? How to help guide on balance entry confirm data verification validate totals provide review checklist Don't claim startin
- **Sync Status** — Clarifying questions Which integration? (QBO or Xero) Do you have a sync job ID? What is the error message? How to help collect error message confirm last successful sync check integration status iden
- **User Memory** — Clarifying questions What data do you want to store or retrieve? Is this for customer data or internal tracking? How to help use execute_sql tool for queries explain User Memory schema guide on data p
- **Profit Analytics Beta** — Clarifying questions Are you using the beta feature? Which dashboard or metric? How to help explain beta feature scope guide on filters and settings reference KPI dashboard provide interpretation guid
- **Bill Missing** — Clarifying questions What is the bill/invoice ID? Which vendor? Which warehouse or company? How to help confirm status in source system check Bill Info attachments verify shipment/order link identify
- **Fifo Analytics** — Clarifying questions Which warehouse or period? What is the specific concern? How to help explain FIFO transaction tracking provide COGS quality metrics reference analytics dashboard guide on interpre
- **Bills, invoices, and payables** — What it is Bills, invoices, and payables. Key objects Bill Invoice Vendor How to help bill missing invoice missing Clarifying questions Confusing bills (payables) with invoices (receivables) bill vs i
- **Accounting system connection and synchronization** — What it is Accounting system connection and synchronization. Key objects Connection Sync Job Ledger Snapshot How to help connect accounting sync accounting Clarifying questions Confusing sync errors w
- **Inventory Audit** — Clarifying questions Which warehouse or period? Are you auditing against 3PL or internal records? How to help explain audit process guide on reconciliation provide audit checklist reference audit guid
- **Metric Contributor / Breakdown Drilldown** — Procedure When a user asks about contributors, breakdowns, or what's driving a metric (e.g. "what are the biggest contributors to lost COGS?"): Identify the metric and date range. Confirm which metric
- **FIFO inventory valuation, COGS calculations, unit cost analysis, and inventory audits** — What it is FIFO inventory valuation, COGS calculations, unit cost analysis, and inventory audits. Covers how inventory costs flow using first-in-first-out methodology and how to audit inventory values
- **Mapping Errors** — Clarifying questions Which account or service? Which category is affected? Is this for Bills or another document? How to help request mapping export identify mismatch confirm correct mapping validate
- **Financial reconciliation, revenue and COGS reporting, and verifying that totals match across systems** — What it is Financial reconciliation, revenue and COGS reporting, and verifying that totals match across systems. Covers profit analysis and cross-system data validation. Key objects Reconciliation rep
- **Chart Of Accounts** — Clarifying questions Are you importing from QBO/Xero or using NeonPanel default? How to help confirm CoA source explain service mapping requirements provide setup guidance Don't claim CoA changes don'
- **Po Placement** — Clarifying questions Which vendor or supplier? What is your lead time? How to help use supplychainlistpoplacement_candidates tool identify candidates provide ordering guidance confirm quantities Don't
- **Revenue Reporting** — Clarifying questions Which period or company? What level of detail? How to help use neonpanel_getRevenueAndCogs tool provide revenue/COGS summary explain variance analysis guide on reporting Don't cla
- **Integration Setup** — Clarifying questions Which accounting system? (QBO or Xero) Is this first-time setup? How to help provide integration steps confirm credentials validate connection test sync Don't claim automatic inte
- **Auth** — Clarifying questions Which integration? (QBO or Xero) What is the exact error message? How to help collect timestamp request re-authentication confirm credentials validate token status Don't claim tok
- **Verified Facts** — Facts NeonPanel integrates with QuickBooks Online and Xero for accounting workflows. QuickBooks Online integration exports data from NeonPanel to QBO; it does not sync journal entries back into NeonPa
- **Unit Costs** — Clarifying questions Which period or product? What is the intended use? How to help use cogsexportunit_costs tool provide cost export explain cost methodology guide on cost analysis Don't claim costs
- **Project Search** — Clarifying questions What is the project name or reference? How to help use searchneonpanelproject_url tool provide project link explain project scope

### Po Invoice Mapping (skill under accounting)

File: `references/po_invoice_mapping.md`

- **Looking up a purchase order by its reference number using NeonPanel tools** — What it is Looking up a purchase order by its reference number using NeonPanel tools. Involves finding the company, searching for the PO project, and retrieving linked records (shipments, bills). Key
- **Verified Facts** — Facts The PO-to-invoice mapping produces a formatted markdown document, not a legally binding invoice. PO lookup requires a reference number (e.g., 320250006) and uses the searchneonpanelprojecturl to
- **Lookup Po Details** — Clarifying questions What's the PO reference number? Which company should I search in? How to help Search for the PO using searchneonpanelproject_url Present the PO summary with linked records (shipme
- **Line Items** — Procedure | # | SKU | Description | Qty | Unit Price | Line Total | |---|-----|-------------|-----|------------|------------| | 1 | {sku} | {description} | {qty} | ${unitprice} | ${linetotal} | | 2 |
- **Totals** — Procedure | | | |---|---| | Subtotal | ${subtotal} | | Freight | ${freight} | | Duties | ${duties} | | Other Charges | ${other} | | Grand Total | ${grand_total} | ---
- **Invoice Markdown Template** — Procedure Use this template structure when generating the invoice output. Adapt field names based on what the NeonPanel tools actually return — do NOT fabricate fields that weren't in the response. ``
- **Mapping purchase order data from NeonPanel into a structured invoice/document format** — What it is Mapping purchase order data from NeonPanel into a structured invoice/document format. Covers field mapping (PO fields → invoice fields), line item formatting, totals calculation, and markdo
- **Handle Missing Data** — Procedure If the PO is not found, inform the user and ask them to verify the reference number and company. If line-item details are unavailable (tool returns only the PO summary), output a summary inv
- **Generate Invoice with Landed Costs** — Procedure Follow steps 1–6 from "Generate Invoice from Purchase Order" above. Call neonpanelgetInventoryLandedCost to retrieve landed cost data for the items. Optionally call neonpanelgetInventoryCogs
- **Enriching the invoice with additional cost data — landed costs, COGS, unit costs — pulled from NeonPanel's costing tools** — What it is Enriching the invoice with additional cost data — landed costs, COGS, unit costs — pulled from NeonPanel's costing tools. Key objects Landed cost COGS Unit cost Freight Duties Cost breakdow
- **Po Invoice With Costs** — Clarifying questions What's the PO reference number? Should I include freight, duties, or all landed cost components? How to help Look up the PO via searchneonpanelprojecturl Pull inventory details vi
- **Linked Records** — Procedure | Type | Reference | Status | Link | |------|-----------|--------|------| | Shipment | {shipmentref} | {status} | [Open]({url}) | | Bill | {billref} | {status} | [Open]({url}) | --- Generate
- **Generate Invoice from Purchase Order** — Procedure Get the PO reference number from the user (e.g., 320250006). If the company is ambiguous, ask which company to search in. Otherwise use the default/known company. Call searchneonpanelproject
- **Generate Po Invoice** — Clarifying questions What's the PO reference number? Which company is this PO from (if you have multiple)? Do you want just the basic invoice or should I include landed costs? How to help Search for t

## Knowledge base

- No KB articles assigned
