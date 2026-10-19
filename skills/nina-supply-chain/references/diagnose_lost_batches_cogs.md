# Diagnose Lost Batches Cogs — Knowledge

## Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process

**What it is**

Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process. Phase 1 uses cogs_analyze_fifo_cogs (lost_cogs mode) for COGS quality assessment. Phase 2 uses cogs_list_lost_batches for comprehensive lost batch investigation across ALL transaction types including shipments. Per-SKU deep dives use neonpanel_getInventoryCogs and neonpanel_getInventoryDetails. Also covers Statement Analytics for Amazon anomalies, Landed Cost Analysis for cost propagation, Inventory Audit for balance verification, and search_neonpanel_project_url for finding specific documents.

**Key objects**

- FIFO Transactions report (COGS Quality tab, Lost Batches tab, Transaction List)
- cogs_analyze_fifo_cogs tool (lost_cogs mode — Phase 1)
- cogs_list_lost_batches tool (Phase 2 — all transaction types)
- neonpanel_getInventoryCogs (per-SKU COGS deep dive)
- neonpanel_getInventoryDetails (per-SKU transaction history)
- Statement Analytics report
- Landed Cost Analysis report
- Inventory Audit report
- search_neonpanel_project_url tool

**How to help**

- Which report should I look at?
- How do I pull lost batch data?
- Where do I check for Amazon adjustments?
- How do I verify costs?
- Which tool for lost COGS vs lost batches?

**Clarifying questions**

- Using cogs_analyze_fifo_cogs (lost_batches mode) for comprehensive lost batch investigation — this only covers invoice/sale transactions. Use cogs_list_lost_batches instead for all transaction types.
- Not knowing Statement Analytics exists for Amazon anomaly investigation
- Confusing the Landed Cost Analysis report with shipment-level landed cost
- Not understanding that cogs_list_lost_batches captures shipment-level lost batches that cogs_analyze_fifo_cogs does not surface

## Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works

**What it is**

Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works. All lost COGS are lost batches, but not all lost batches are lost COGS.

**Key objects**

- lost batch
- lost COGS
- FIFO queue
- outbound transaction
- invoice
- shipment
- warehouse
- same-date priority
- Lost & Found warehouse

**How to help**

- What are lost batches?
- What is lost COGS?
- Why do I have $0 COGS?
- How does FIFO work per warehouse?
- What's the difference between lost batches and lost COGS?

**Clarifying questions**

- Confusing lost batches with lost COGS — all lost COGS are lost batches but not vice versa
- Assuming company-wide stock covers warehouse-specific FIFO — FIFO is per-warehouse
- Not understanding same-date transaction priority — outbound processes before inbound on the same date
- Thinking lost batches mean inventory is physically lost — it means the FIFO engine couldn't find a source batch to assign

**Clarifying questions**

- Are you asking about lost batches in general, or specifically about lost COGS on sales?

**How to help**

- Explain the distinction: lost batches (any outbound transaction) vs lost COGS (specifically on invoices/sales)
- Explain FIFO per-warehouse mechanics
- Explain same-date transaction priority
- If the user has a specific issue, route to Phase 1 (lost COGS) or Phase 2 (lost batches)

**Don't claim**

- Do not say lost batches and lost COGS are the same thing
- Do not claim FIFO works company-wide — it is per-warehouse

## Warehouse Fork Assessment

**Clarifying questions**

- What warehouse is shown on the lost COGS transaction?
- Is it Lost & Found or an actual warehouse name like Amazon FBA US?

**How to help**

- If Lost & Found: stock was not on the selling warehouse at all — the shipment that should have brought inventory either never happened, had wrong dates, went to the wrong warehouse, or had another upstream issue
- If actual warehouse: stock was on the warehouse but delivered with $0 cost — the inbound shipment itself had a lost batch
- For actual warehouse cases: find the inbound shipment that delivered zero-cost stock, then trace that shipment's source backwards
- This fork prevents misdiagnosis — without it you might look for why stock didn't arrive when it did arrive, just without costs

**Don't claim**

- Do not skip the warehouse check — it's the most important first diagnostic signal for lost COGS
- Do not assume Lost & Found means the same thing as an actual warehouse showing $0 cost

## Aggregate Pattern Analysis

**Clarifying questions**

- Do you want to look at all lost batches or filter by transaction type (invoices vs. shipments)?
- Is there a specific date range to focus on?

**How to help**

- Pull lost batches sorted by impact descending
- Group by shipment or PO to find patterns — one shipment causing lost batches across 15 products is one problem, not 15
- Identify the top 3-5 contributors by unit or dollar impact
- For each top contributor, determine if it's a shipment-level, PO-level, or warehouse-level issue
- Build an investigation list of individual SKUs from the top contributors

**Don't claim**

- Do not investigate SKU by SKU without first checking for aggregate patterns
- Do not treat each lost batch as an independent problem without checking for shared causes

## Verified Facts

**Facts**

- Lost COGS diagnosis has two entry points: 'I have lost COGS' starts at Phase 1 (COGS triage with the Warehouse Fork), while 'I have lost batches' starts directly at Phase 2 (general lost batches investigation).
- The Warehouse Fork is the critical first diagnostic signal for lost COGS: if the warehouse on the transaction is Lost & Found, stock never arrived at the selling warehouse; if it's an actual warehouse, stock arrived but with $0 cost because the inbound shipment had a lost batch.
- Do NOT start lost batch investigation with individual SKUs — start with the aggregate view sorted by impact. Look for shipment-level or PO-level patterns: if the same shipment causes lost batches across 15 products, that's one shipment-level problem, not 15 separate SKU problems.
- When an inbound and outbound transaction occur on the same date, NeonPanel gives priority to the outbound during FIFO recalculation — the inbound processes after, which can result in a lost batch even though both transactions exist.
- Amazon can have an approximately 9-month lag between actually disposing of inventory (reflected in the ledger) and including it in a settlement statement.
- Solving lost batches proactively on shipments prevents future lost COGS — shipment lost batches deliver inventory to destination warehouses with $0 cost, and when that zero-cost inventory sells via FIFO, it becomes lost COGS.
- Lost COGS directly impacts the P&L — COGS is undercounted, gross profit is overstated, and financial decisions are being made on bad numbers.
- The agent should guide, not prescribe: surface data, explain mechanics, and point the user in the right direction. Only prescribe a fix when evidence is unambiguous.
- The agent cannot detect the absence of data that was never entered — it can only narrow down where the gap likely is based on everything else visible, then ask the user to check their records.
- Use cogs_analyze_fifo_cogs (analysis_mode: lost_cogs) for Phase 1 — lost COGS triage focusing on invoice/sale transactions with COGS quality metrics.
- Use cogs_list_lost_batches for Phase 2 — comprehensive lost batch investigation across ALL transaction types including invoices, shipments, adjustments, and any other outbound transactions.
- cogs_analyze_fifo_cogs (analysis_mode: lost_batches) is legacy/limited — it only covers invoice/sale transaction lost batches. For comprehensive lost batch investigation, always use cogs_list_lost_batches instead.
- cogs_list_lost_batches captures shipment-level lost batches that cogs_analyze_fifo_cogs does not surface. Shipment lost batches are critical because they deliver inventory to destination warehouses with $0 cost, which becomes lost COGS when that inventory sells.
- The COGS Quality tab shows lost COGS (sales with $0 cost assignment); the Lost Batches tab shows all lost batches regardless of transaction type; the Transaction List shows individual transactions for per-SKU tracing.
- Statement Analytics report should be used to investigate unusual Amazon outflows — liquidations, removals, and adjustments that may explain volume anomalies.
- Lost batches are not always errors — they may result from legitimate business scenarios. Do not assume every lost batch requires a fix.
- Do not fabricate COGS quality scores or percentages — always pull actual data using the appropriate tools before stating any numbers.
- Lost batches are transactions where the FIFO engine could not find or assign a source batch for units being moved — the FIFO queue was empty or no matching inventory existed in the expected warehouse.
- Lost batches can occur on any outbound transaction type — invoices (sales), shipments (stock transfers), or any other outbound movement.
- Lost COGS is a specific subset of lost batches that occurs on invoice (sale) transactions — a sale happened but no batch with a cost could be assigned, resulting in $0 COGS for those units.
- All lost COGS are lost batches, but not all lost batches are lost COGS.
- FIFO is per-warehouse — stock in a 3PL warehouse does not cover sales from Amazon FBA, even if total company-wide quantity appears sufficient.
- Data sources are mixed — Amazon shipments and Amazon/Shopify invoices are automatic; purchase orders and some shipments are manual/user-provided. Most data gaps originate at the boundary between automatic and manual sources.
- Acquisition batches (ACQ) represent opening balance stock from onboarding and may be inaccurate or in the wrong warehouses.
- If an affected period is locked, recalculation is not possible and adjustments are needed instead.
- Warehouse clones — two warehouses can look identical by name but be different backend entities. Always verify warehouse IDs, not just names.
- AWD (Amazon Warehousing and Distribution) and Amazon FBA are separate warehouses and should not be confused.

## Tool Selection Lost Cogs Vs Batches

**Clarifying questions**

- Are you looking at lost COGS (sales with $0 cost) or lost batches in general (across all transaction types)?
- Do you need to include shipment-level lost batches in your investigation?

**How to help**

- Explain the two-tool distinction: cogs_analyze_fifo_cogs (Phase 1, invoice/sale COGS quality) vs cogs_list_lost_batches (Phase 2, all transaction types)
- If user needs Phase 1 (COGS quality triage), guide to cogs_analyze_fifo_cogs with analysis_mode: lost_cogs
- If user needs Phase 2 (comprehensive lost batches), guide to cogs_list_lost_batches
- Explain that cogs_analyze_fifo_cogs lost_batches mode is legacy/limited — only invoice/sale transactions
- Explain that cogs_list_lost_batches captures shipment lost batches that the other tool misses

**Don't claim**

- Do not tell users to use cogs_analyze_fifo_cogs (lost_batches mode) for comprehensive lost batch investigation — it only covers invoice/sale transactions
- Do not say both tools return the same data — cogs_list_lost_batches covers all transaction types including shipments, while cogs_analyze_fifo_cogs focuses on invoice/sale transactions

**Escalate when**

- User is confused about which tool to use after explanation
- User reports that cogs_list_lost_batches is not returning expected data

## Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal that determines the nature of the problem

**What it is**

Phase 1 of the diagnostic skill: assessing overall COGS quality, identifying the worst offenders by SKU/brand/company, and applying the Warehouse Fork — the critical first diagnostic signal that determines the nature of the problem. If the warehouse on the lost COGS transaction is Lost & Found, stock never arrived at the selling warehouse. If it's an actual warehouse (e.g., Amazon FBA US), stock arrived but with $0 cost — the problem is upstream in the inbound shipment.

**Key objects**

- COGS Quality tab
- Lost COGS entry
- Warehouse Fork (Lost & Found vs. actual warehouse)
- COGS quality percentage
- Investigation threshold
- cogs_analyze_fifo_cogs tool (analysis_mode: lost_cogs)

**How to help**

- I have lost COGS
- My COGS quality is bad
- Sales have $0 cost
- Check my COGS quality
- Which SKUs have the worst COGS?

**Clarifying questions**

- Not checking the warehouse on the transaction first — jumping straight to tracing backwards without the fork
- Assuming all lost COGS means stock never arrived — it may have arrived with $0 cost
- Starting with individual SKUs instead of the aggregate view

## Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches

**What it is**

Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches. Uses cogs_list_lost_batches to pull ALL lost batches across all transaction types (invoices, shipments, adjustments). Starts with the big picture (shipment-level and PO-level patterns), then drills into individual SKUs. For each SKU, analyzes surrounding transactions — Lost & Found activity, origin/destination warehouses, backwards timeline (POs, earlier problems, date ordering), forwards timeline (subsequent shipments, delayed arrivals), warehouse ID verification, volume anomalies, and onboarding residue.

**Key objects**

- Lost Batches tab
- Aggregate view (by shipment/PO pattern)
- Surrounding transactions
- Date ordering (same-date priority, different-date variant)
- Volume anomalies
- Statement Analytics report
- cogs_list_lost_batches tool

**How to help**

- I have lost batches
- Trace why this batch is lost
- Find the root cause of lost batches
- Which shipments are causing the most lost batches?
- Why is stock missing from this warehouse?

**Clarifying questions**

- Starting with individual SKUs instead of aggregate patterns — one shipment can cause lost batches across many SKUs
- Only looking at the lost batch itself without examining surrounding transactions
- Not checking for same-date transaction priority issues
- Confusing AWD with Amazon FBA warehouses
- Using cogs_analyze_fifo_cogs (lost_batches mode) instead of cogs_list_lost_batches — the former only covers invoice/sale transactions, the latter covers ALL transaction types including shipments

## Diagnostic Tool Selection

**Clarifying questions**

- Are you looking for lost COGS specifically or all lost batches?
- Do you need to investigate programmatically or via the UI reports?

**How to help**

- For lost COGS (Phase 1): use FIFO Transactions report → COGS Quality tab, or cogs_analyze_fifo_cogs with analysis_mode: lost_cogs
- For all lost batches (Phase 2): use FIFO Transactions report → Lost Batches tab, or cogs_list_lost_batches for ALL transaction types including shipments
- For individual transaction tracing: use FIFO Transactions report → Transaction List, or neonpanel_getInventoryCogs and neonpanel_getInventoryDetails per SKU
- For Amazon anomalies (liquidations, removals): use Statement Analytics report
- For cost verification: use Landed Cost Analysis report or neonpanel_getInventoryLandedCost
- For balance discrepancies: use Inventory Audit report
- For finding specific documents (POs, shipments): use search_neonpanel_project_url tool
- Note: cogs_analyze_fifo_cogs (analysis_mode: lost_batches) is legacy/limited — only covers invoice/sale transactions. Always use cogs_list_lost_batches for comprehensive investigation.

**Don't claim**

- Do not confuse the COGS Quality tab (lost COGS only) with the Lost Batches tab (all lost batches)
- Do not use the wrong analysis_mode on cogs_analyze_fifo_cogs

## Tools & Reports Quick Reference

**Procedure**

| Tool / Report | When to Use |
|---|---|
| FIFO Transactions → COGS Quality tab | Identify lost COGS — sales with $0 cost assignment |
| FIFO Transactions → Lost Batches tab | Identify all lost batches regardless of transaction type |
| FIFO Transactions → Transaction List | Examine individual transactions, trace stock flow per SKU |
| `cogs_analyze_fifo_cogs` (analysis_mode: 'lost_cogs') | **Phase 1:** Pull lost COGS data programmatically with quality metrics. Focuses on invoice/sale transactions. |
| `cogs_list_lost_batches` | **Phase 2:** Pull ALL lost batches across all transaction types — invoices, shipments, adjustments. The comprehensive tool for lost batch investigation. Captures shipment-level lost batches that `cogs_analyze_fifo_cogs` does not surface. |
| `cogs_analyze_fifo_cogs` (analysis_mode: 'lost_batches') | **Legacy/limited:** Lost batches from invoice/sale transactions only. Use `cogs_list_lost_batches` instead for comprehensive investigation. |
| `neonpanel_getInventoryCogs` | Per-SKU COGS deep dive — trace which batches were assigned to each sale |
| `neonpanel_getInventoryDetails` | Per-SKU surrounding transaction analysis — stock levels, transaction history, batch details |
| `neonpanel_getInventoryLandedCost` | Cost propagation verification — PO price + logistics + landed cost bills |
| Statement Analytics report | Investigate unusual Amazon outflows — liquidations, removals, adjustments |
| Landed Cost Analysis report | Verify cost propagation — PO price + logistics + landed cost bills |
| Inventory Audit report | Compare actual vs. FIFO-calculated balances, spot discrepancies |
| `search_neonpanel_project_url` | Find specific POs, shipments, FBA shipments by name or reference number |

## Phase 2: Lost Batches General Investigation

**Procedure**

1. **Start with the aggregate view — never individual SKUs first.** Pull lost batches data using `cogs_list_lost_batches`. This tool captures ALL transaction types — invoices, shipments, adjustments, and any other outbound transactions. Sort by impact (units or dollar amount, descending).
2. **Look for shipment-level or PO-level patterns.** Shipments, POs, and invoices typically involve many SKUs at once. If the same shipment is causing lost batches across 15 different products, that's one shipment-level problem, not 15 separate SKU problems. Identifying these patterns early is critical.
3. **Prioritize by materiality.** Tackle the biggest impact first.
4. **Select individual SKUs/inventory IDs** from the top contributors. Build your investigation list.
5. **For each SKU, analyze surrounding transactions:**
   a. **Lost & Found activity** — what transactions for this item involve Lost & Found? How many? Over what time period?
   b. **Origin and destination warehouses** of the specific shipment(s) causing the issue.
   c. **Look backwards in time:**
      - Were there smaller problems earlier for the same inventory ID? The current lost batch might be the culmination of an issue that started building.
      - Are there POs around or before the lost batch dates? Were they received? Into the right warehouse?
      - Is there a timing/date ordering issue? Did an outbound shipment process before an inbound that should have preceded it?
      - **Same-date variant:** Both transactions on the same date, but NeonPanel's processing priority causes the outbound to run first.
      - **Different-date variant:** The outbound shipment has an earlier date than the inbound — same problem, more obvious.
   d. **Look forwards in time:**
      - Are there subsequent shipments that might have date errors or other irregularities?
      - Did stock eventually arrive but too late?
   e. **Verify warehouse IDs vs. names:**
      - Check for warehouse clones — two warehouses can look identical by name but be different backend entities. Verify IDs, not just names.
      - AWD and Amazon FBA are separate warehouses — don't confuse them.
   f. **Check for volume anomalies:**
      - An Amazon invoice suddenly writing off hundreds of units when the SKU normally sells 50/month is suspicious.
      - Check the Statement Analytics report for liquidations, removals, or adjustments.
      - Note: Amazon can have a ~9 month lag between disposing of inventory and including it in a settlement statement.
   g. **Check for onboarding residue:**
      - Acquisition batches (ACQ) representing opening balance stock — are they accurate? In the right warehouses?
      - In-transit POs from onboarding that were never resolved — stock never "arrived."
      - Check if the affected period is locked — if locked, recalculation is not possible and adjustments are needed instead.

## Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for unambiguous cases, the hard limit (cannot detect absence of data never entered), and escalation rules for locked periods, systematic issues, and bulk operations

**What it is**

Behavioral guardrails for the diagnostic process: the guide-don't-prescribe principle (surface data and explain mechanics, but let the user make the final call on fixes), the exception for unambiguous cases, the hard limit (cannot detect absence of data never entered), and escalation rules for locked periods, systematic issues, and bulk operations.

**Key objects**

- Guide vs. prescribe boundary
- Unambiguous evidence exception
- Hard limit (missing data never entered)
- Locked period constraint
- Escalation triggers

**How to help**

- What should I do to fix this?
- Can you just fix it?
- Why can't you tell me what's missing?
- Should I recalculate?

**Clarifying questions**

- Expecting the agent to prescribe fixes without sufficient evidence
- Thinking recalculation always fixes lost batches — locked periods prevent it
- Assuming the system can detect data that was never entered

## Lost Batches Diagnosis

**Clarifying questions**

- Which SKU(s) or inventory IDs are affected?
- What time period are the lost batches occurring in?
- Have you already identified any patterns (specific shipments, POs, warehouses)?

**How to help**

- Pull ALL lost batches data using cogs_list_lost_batches — this captures invoices, shipments, adjustments, and all other outbound transaction types
- Start with aggregate view — sort by impact (units or dollars descending)
- Look for shipment-level or PO-level patterns: if the same shipment causes lost batches across many SKUs, that's one problem not many
- Prioritize by materiality — tackle biggest impact first
- Select individual SKUs from top contributors for detailed investigation
- For each SKU, run surrounding transaction analysis

**Don't claim**

- Do not start with individual SKUs — always start with the aggregate view
- Do not assume lost batches are always errors — they may result from legitimate business scenarios
- Do not prescribe fixes without sufficient evidence — guide instead

**Escalate when**

- Bulk warehouse updates needed across many shipments
- Lost batches span a locked accounting period
- Pattern suggests systematic data entry gap across multiple POs or shipments

## Surrounding Transaction Analysis

**Clarifying questions**

- Which specific SKU or inventory ID are we investigating?
- Do you have the transaction dates for the lost batch?

**How to help**

- Check Lost & Found warehouse transactions for this item — how many, over what period?
- Identify origin and destination warehouses of the problematic shipment(s)
- Look backwards: earlier problems for same inventory ID, POs around the lost batch dates, date ordering issues (same-date priority or different-date variant)
- Look forwards: subsequent shipments with date errors, stock that arrived too late
- Verify warehouse IDs vs names — check for warehouse clones (same name, different backend entity), confirm AWD vs FBA distinction
- Check for volume anomalies — unusual outflows should be investigated via Statement Analytics for liquidations, removals, adjustments (note: Amazon can have ~9 month lag)
- Check for onboarding residue — ACQ batches in wrong warehouses, unresolved in-transit POs, locked periods

**Don't claim**

- Do not confuse AWD with Amazon FBA — they are separate warehouses
- Do not rely on warehouse names alone — always verify warehouse IDs
- Do not assume unusual Amazon outflows are errors without checking Statement Analytics

**Escalate when**

- Volume anomalies suggesting Amazon liquidations or removals with long settlement lags
- Onboarding residue in locked periods requiring adjustments

## Phase 2 Continued: Formulate Hypotheses & Guide the User

**Procedure**

6. **Formulate hypotheses — with care.** Only after examining the full transaction context. Possibilities include:
   - Date ordering issue — inbound and outbound dates need correcting so inbound precedes outbound
   - Missing PO — a purchase order was never entered
   - Missing shipment — a stock transfer was never recorded
   - Wrong warehouse assignment — shipment pulling from or delivering to the wrong warehouse
   - Warehouse clones — stock split across what looks like the same warehouse but isn't
   - Onboarding residue — stale acquisition batches or unresolved in-transit POs
   - Anomalous Amazon adjustments — liquidations, removals, or statement items with long lags
   - Missing bill connections — PO price exists but logistics or landed cost bills aren't connected
7. **Guide, don't prescribe (usually).** Surface data, explain mechanics, point the user in the right direction. The user must make the final call because:
   - You may not have access to data that verifies what actually happened in the real world
   - What looks like a simple date swap might mask a deeper missing-document issue
   - The user knows their business chronology — you don't
8. **Exception — when you CAN prescribe:** When evidence is unambiguous — a clear data entry error, a pattern that can't reasonably be anything else. But this is the exception, not the default.
9. **The hard limit.** You cannot detect the absence of data that was never entered. If a PO was never created, there's no trace. Narrow down where the gap likely is, then tell the user: "I've traced back through the transactions and can't find a source for this stock. This likely means there's a document — possibly a PO or a shipment — that was never entered into the system. Can you check your records for any orders around [date range] that might not have been recorded?"

## Phase 1: Lost COGS Triage

**Procedure**

1. **Assess overall COGS quality.** Use `cogs_analyze_fifo_cogs` (analysis_mode: `lost_cogs`) to pull COGS quality data. Aggregate by SKU, brand, or company-wide depending on the user's scope.
2. **Surface the worst offenders.** Sort by quality percentage ascending. Identify materiality: how many units, how much estimated lost COGS in dollar terms.
3. **Ask about scope and threshold.** Confirm: specific SKU, brand, or entire company? Time period? Investigation threshold (minimum units or dollars)?
4. **Apply the Warehouse Fork** for each lost COGS entry — check the warehouse on the transaction:
   - **Fork A — Warehouse = Lost & Found:** Stock was not on the selling warehouse at all when the sale occurred. The shipment that should have brought inventory either never happened, had wrong dates, went to the wrong warehouse, or had another upstream issue. → Proceed to Phase 2 to trace backwards.
   - **Fork B — Warehouse = An actual warehouse (e.g., Amazon FBA US):** Stock was on the warehouse when the sale occurred, but was delivered with $0 cost. The inbound shipment itself had a lost batch. The problem is upstream. → Find the inbound shipment that delivered zero-cost stock, then trace that shipment's source via Phase 2.
5. **This fork prevents misdiagnosis.** Without it, you might waste time looking for why stock didn't arrive when it did arrive — just without costs.

## Diagnostic Guidance Limits

**Clarifying questions**

- Have you already attempted any fixes? If so, what did you try?
- Is the affected period locked?

**How to help**

- Surface data, explain mechanics, and point the user in the right direction
- Only prescribe a fix when evidence is unambiguous — a clear data entry error or a pattern that can't reasonably be anything else
- If data was never entered, communicate clearly: state where the gap likely is, explain what type of document would fill it, acknowledge you cannot confirm what the missing data contains
- Suggest the user check their own records (supplier invoices, shipping documents, bank statements)
- If the period is locked, explain that recalculation is not possible and adjustments are needed instead

**Don't claim**

- Do not prescribe fixes without sufficient evidence — guide instead
- Do not claim to know what data is missing — only state where the gap likely is
- Do not promise recalculation will fix the issue without checking if the period is locked
- Do not assume lost batches are always errors

**Escalate when**

- Lost batches span a locked accounting period
- Large volume across multiple SKUs suggests a systematic issue
- Customer has already attempted fixes that didn't resolve the problem
- Bulk warehouse updates are needed across many shipments

## Decision Flow Summary

**Procedure**

```
User reports lost COGS or lost batches
  │
  ├── Lost COGS? → Phase 1: Check warehouse on the transaction
  │     ├── Lost & Found → Stock never arrived → Phase 2
  │     └── Actual warehouse → Stock arrived with $0 cost
  │           → Find the inbound shipment → Phase 2 (for that shipment)
  │
  └── Lost Batches? → Phase 2 directly
        │
        ├── Step 1: Aggregate view using cogs_list_lost_batches — top contributors, shipment/PO-level patterns
        ├── Step 2: Select individual SKUs to investigate
        ├── Step 3: Analyze surrounding transactions
        │     ├── Lost & Found transactions for this item
        │     ├── Origin/destination warehouses
        │     ├── Backwards: POs, earlier problems, date ordering
        │     ├── Forwards: subsequent shipments, delayed arrivals
        │     ├── Warehouse IDs vs names (clones?)
        │     ├── Volume anomalies → Statement Analytics
        │     └── Onboarding residue (ACQ, in-transit POs, locked periods)
        ├── Step 4: Formulate hypotheses
        ├── Step 5: Guide the user (prescribe only when unambiguous)
        └── Step 6: Hard limit — can't detect missing data that was never entered
```

## Lost Cogs Diagnosis

**Clarifying questions**

- Do you want to check a specific SKU, brand, or the entire company?
- What time period are you concerned about?
- Do you have an investigation threshold — a minimum unit count or dollar amount below which you don't want to investigate?

**How to help**

- Pull COGS quality data using cogs_analyze_fifo_cogs (analysis_mode: lost_cogs)
- Sort by quality percentage ascending to surface worst offenders
- Identify materiality: unit count and estimated lost COGS in dollars
- Apply the Warehouse Fork: check the warehouse on each lost COGS transaction
- If Lost & Found → stock never arrived → proceed to lost-batches-diagnosis
- If actual warehouse → stock arrived with $0 cost → find the inbound shipment → proceed to lost-batches-diagnosis for that shipment

**Don't claim**

- Do not claim to know what data is missing — only state where the gap likely is
- Do not promise recalculation will fix lost COGS without checking if the period is locked
- Do not fabricate quality scores or percentages without pulling actual data
- Do not skip the Warehouse Fork — it prevents misdiagnosis

**Escalate when**

- Lost COGS spans a locked accounting period
- Large volume across multiple SKUs suggests a systematic issue
- Customer has already attempted fixes that didn't resolve the problem
