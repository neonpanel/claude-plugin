# Agent Knowledge Index

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

## Knowledge base

- No KB articles assigned
