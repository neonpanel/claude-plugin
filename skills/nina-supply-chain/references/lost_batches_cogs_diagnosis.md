# Lost Batches Cogs Diagnosis — Knowledge

## Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process

**What it is**

Reference mapping of which NeonPanel tools and reports to use at each stage of the diagnostic process. Phase 1 uses cogs_analyze_fifo_cogs (lost_cogs mode) for COGS quality assessment. Phase 2 uses cogs_list_lost_batches for comprehensive lost batch investigation across ALL transaction types including shipments. Per-SKU deep dives use neonpanel_getInventoryCogs and neonpanel_getInventoryDetails. Also covers Statement Analytics for Amazon anomalies, Landed Cost Analysis for cost propagation, Inventory Audit for balance verification and the Offsetting Balances Cross-Check, and search_neonpanel_project_url for finding specific documents.

**Key objects**

- FIFO Transactions report (COGS Quality tab, Lost Batches tab, Transaction List)
- cogs_analyze_fifo_cogs tool (lost_cogs mode — Phase 1)
- cogs_list_lost_batches tool (Phase 2 — all transaction types)
- neonpanel_getInventoryCogs (per-SKU COGS deep dive)
- neonpanel_getInventoryDetails (per-SKU transaction history)
- Statement Analytics report
- Landed Cost Analysis report
- Inventory Audit report (Offsetting Balances Cross-Check)
- search_neonpanel_project_url tool

**How to help**

- Which report should I look at?
- How do I pull lost batch data?
- Where do I check for Amazon adjustments?
- How do I verify costs?
- Which tool for lost COGS vs lost batches?
- How do I check if inventory exists in another warehouse?

**Clarifying questions**

- Using cogs_analyze_fifo_cogs (lost_batches mode) for comprehensive lost batch investigation — this only covers invoice/sale transactions. Use cogs_list_lost_batches instead for all transaction types.
- Not knowing Statement Analytics exists for Amazon anomaly investigation
- Confusing the Landed Cost Analysis report with shipment-level landed cost
- Not understanding that cogs_list_lost_batches captures shipment-level lost batches that cogs_analyze_fifo_cogs does not surface
- Not knowing the Inventory Audit report can be used to cross-reference warehouse surpluses against deficits (Offsetting Balances Cross-Check)

## Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works

**What it is**

Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works. Also covers: how the FIFO engine tracks batch quantities (partial batch depletion creating blended COGS), how inbound shipments and Bills combine to form Landed Cost, and the L&F warehouse as a designed $0-cost signal rather than a system error.

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
- inbound shipment
- Bill
- Landed Cost
- blended COGS
- batch quantity ticking

**How to help**

- What are lost batches?
- What is lost COGS?
- Why do I have $0 COGS?
- How does FIFO work per warehouse?
- What's the difference between lost batches and lost COGS?
- How does Landed Cost work?
- What does the Lost & Found warehouse mean?
- How does the FIFO engine assign costs across multiple batches?

**Clarifying questions**

- Confusing lost batches with lost COGS — all lost COGS are lost batches but not vice versa
- Assuming company-wide stock covers warehouse-specific FIFO — FIFO is per-warehouse
- Not understanding same-date transaction priority — outbound processes before inbound on the same date
- Thinking lost batches mean inventory is physically lost — it means the FIFO engine couldn't find a source batch to assign
- Thinking a Bill transfers the full cost of goods — it only adds capitalizable costs (freight, duties) on top of the shipment's base cost
- Assuming the FIFO engine always uses one batch per invoice — it depletes partial batches and blends across multiple chronological batches
- Treating L&F as a system error — it is a designed signal that quantity was insufficient in the intended warehouse at the time of the transaction

**Clarifying questions**

- Are you asking about lost batches in general, or specifically about lost COGS on sales?
- Are you trying to understand how Landed Cost is built up from a shipment and a bill?
- Are you asking about how the FIFO engine assigns costs when a sale spans multiple batches?

**How to help**

- Explain the distinction: lost batches (any outbound transaction) vs lost COGS (specifically on invoices/sales)
- Explain FIFO per-warehouse mechanics
- Explain same-date transaction priority
- Explain FIFO quantity ticking: if an invoice needs 100 units and the oldest batch has 40, the engine depletes 40 from that batch then moves to the next chronological batch for the remaining 60, creating blended COGS
- Explain Landed Cost: inbound shipment carries the base cost; a Bill adds capitalizable costs (freight, duties) on top — the combination is the final Landed Cost
- Explain L&F as a designed signal: it fires when quantity in the intended warehouse was insufficient at the exact time of the transaction, and the $0 cost is intentional to flag the issue
- If the user has a specific issue, route to Phase 1 (lost COGS) or Phase 2 (lost batches)

**Don't claim**

- Do not say lost batches and lost COGS are the same thing
- Do not claim FIFO works company-wide — it is per-warehouse
- Do not say a Bill transfers the full cost of goods — it only adds capitalizable costs on top of the shipment's base cost
- Do not say the FIFO engine always uses a single batch per invoice — it depletes partial batches and blends across multiple chronological batches
- Do not treat the Lost & Found warehouse as a system error — it is a designed $0-cost signal

## Warehouse Fork Assessment

**Clarifying questions**

- What warehouse is shown on the lost COGS transaction?
- Is it Lost & Found or an actual warehouse name like Amazon FBA US?

**How to help**

- If Lost & Found: stock was not on the selling warehouse at all at the time of the transaction — this is a designed $0-cost signal indicating insufficient quantity in the intended warehouse
- When L&F fires: (1) determine the intended warehouse for that transaction, (2) pull the full contextual transaction timeline for the SKU in that warehouse, (3) look for structural gaps — date sequencing errors, missing inflows (PO not checked in), or over-depleting earlier outbound shipments
- Do NOT jump to 'missing bills' as the first hypothesis when L&F fires
- If actual warehouse: stock was on the warehouse but delivered with $0 cost — the inbound shipment itself had a lost batch
- For actual warehouse cases: find the inbound shipment that delivered zero-cost stock, then trace that shipment's source backwards
- This fork prevents misdiagnosis — without it you might look for why stock didn't arrive when it did arrive, just without costs

**Don't claim**

- Do not skip the warehouse check — it's the most important first diagnostic signal for lost COGS
- Do not assume Lost & Found means the same thing as an actual warehouse showing $0 cost
- Do not assume missing bills are the cause when L&F fires — check the transaction timeline for structural gaps first

## Advanced Pattern A: Offsetting Balances Cross-Check

**Procedure**

*Use this when the transaction timeline investigation hits a dead end and the root cause cannot be identified.*

1. **Trigger condition:** You've meticulously checked the transaction timeline for the SKU and warehouse but cannot find the reason for the L&F deficit. This often occurs due to pure user error (e.g., a PO was never entered at all, leaving no trace in the transaction history).
2. **Open the Inventory Audit report.** This report compares Calculated FIFO balances against Actual 3rd-party balances (e.g., the Amazon FBA ledger).
3. **Identify the deficit warehouse.** Look at the destination warehouse suffering from large L&F deficits.
4. **Cross-reference other warehouses.** Scan the Inventory Audit report for a corresponding large **Calculated Surplus** (excess calculated stock vs. actual balance) in another warehouse.
5. **Interpret the finding.** A surplus in Warehouse A paired with a deficit in Warehouse B indicates:
   - The inventory physically exists (or existed), but a transfer or shipment between the surplus warehouse and the deficit warehouse was **missed or misdated** just prior to the period when the lost batches began.
6. **Investigate the transfer history** between the two warehouses around that date range.
7. **Communicate clearly:** "The inventory appears to exist in [Warehouse A] but was never recorded as transferred to [Warehouse B]. This is likely a missing or misdated shipment between these two locations around [date range]."

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
- An inbound shipment picks up goods at the base cost it found them at (e.g., from a PO or previous warehouse). A Bill does not transfer the sole cost — it adds capitalizable costs (freight, duties, etc.) to the shipment's existing base cost. The combination of the shipment's base cost and the bill's added costs equals the final Landed Cost.
- The FIFO engine strictly tracks batch quantities, not just batch identity. If an invoice requires 100 units and the oldest batch has only 40 remaining, the engine depletes those 40 units from that batch, then moves to the next chronological batch for the remaining 60 units — creating a blended COGS.
- When Lost & Found is triggered, it means that at the exact time of the transaction (invoice or outbound transfer), there was insufficient quantity in the intended warehouse. The difference comes off the L&F warehouse at $0 cost intentionally — it is a designed signal to the supply chain manager that something must be fixed.
- When L&F fires, do NOT assume missing bills as the first hypothesis. Instead: (1) determine the intended warehouse for that transaction, (2) pull the full contextual transaction timeline for the SKU in that warehouse, (3) look for structural gaps — date sequencing errors, missing inflows, or over-depleting earlier outbound shipments.
- The Offsetting Balances Cross-Check: when a transaction timeline investigation hits a dead end, open the Inventory Audit report and look for a large Calculated Surplus in another warehouse that corresponds to the deficit warehouse's L&F shortfall. This indicates inventory exists but a transfer or shipment between the surplus and deficit warehouses was missed or misdated just prior to when the lost batches began.

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

Phase 2 of the diagnostic skill: the aggregate-first investigation approach for all lost batches. Uses cogs_list_lost_batches to pull ALL lost batches across all transaction types (invoices, shipments, adjustments). Starts with the big picture (shipment-level and PO-level patterns), then drills into individual SKUs. For each SKU, analyzes surrounding transactions — Lost & Found activity, origin/destination warehouses, backwards timeline (POs, earlier problems, date ordering), forwards timeline (subsequent shipments, delayed arrivals), warehouse ID verification, volume anomalies, and onboarding residue. Includes two advanced dead-end patterns: Pattern A (Offsetting Balances Cross-Check using the Inventory Audit report to find a surplus warehouse corresponding to the deficit) and Pattern B (Safety Stock Timebomb — Amazon posts historical liquidations onto a settlement statement months after onboarding, safety stock absorbs the hit silently, then real stock depletes and massive L&F fires suddenly).

**Key objects**

- Lost Batches tab
- Aggregate view (by shipment/PO pattern)
- Surrounding transactions
- Date ordering (same-date priority, different-date variant)
- Volume anomalies
- Statement Analytics report
- cogs_list_lost_batches tool
- Inventory Audit report
- Offsetting Balances Cross-Check
- Safety Stock Timebomb
- Amazon asynchronous ledger
- safety stock cushion

**How to help**

- I have lost batches
- Trace why this batch is lost
- Find the root cause of lost batches
- Which shipments are causing the most lost batches?
- Why is stock missing from this warehouse?
- I've checked everything and can't find the cause
- Amazon FBA suddenly has massive lost batches
- My investigation hit a dead end

**Clarifying questions**

- Starting with individual SKUs instead of aggregate patterns — one shipment can cause lost batches across many SKUs
- Only looking at the lost batch itself without examining surrounding transactions
- Not checking for same-date transaction priority issues
- Confusing AWD with Amazon FBA warehouses
- Using cogs_analyze_fifo_cogs (lost_batches mode) instead of cogs_list_lost_batches — the former only covers invoice/sale transactions, the latter covers ALL transaction types including shipments
- Assuming missing bills when L&F fires — the first step is to check the transaction timeline for structural gaps, not bill connections
- Looking only at recent transactions when Amazon FBA suddenly shows massive L&F — the root cause may be historical liquidations posted months after onboarding
- Not cross-referencing other warehouses in the Inventory Audit report when a dead-end is reached — a surplus elsewhere may reveal a missed transfer

## Offsetting Balances Cross Check

**Clarifying questions**

- Have you already traced the full transaction timeline for this SKU and hit a dead end?
- Which warehouse is showing the L&F deficit?
- Do you have access to the Inventory Audit report?

**How to help**

- Open the Inventory Audit report
- Identify the destination warehouse suffering from large L&F deficits
- Cross-reference other warehouses in the report to spot a corresponding large Calculated Surplus (excess stock vs. actual 3rd-party balance)
- If a surplus warehouse is found: the root cause is likely a missed or misdated transfer/shipment between the surplus warehouse and the deficit warehouse, occurring just prior to when the lost batches began
- Investigate the transfer history between the two warehouses around that date
- This pattern typically surfaces when a user never entered a PO or transfer — the inventory exists but was never moved in the system

**Don't claim**

- Do not use this pattern as the first step — it is for dead-end investigations after the transaction timeline has been exhausted
- Do not assume a surplus in another warehouse always means a missed transfer — confirm the timing relative to when lost batches began

**Escalate when**

- No corresponding surplus found in any warehouse — may indicate inventory was never entered at all
- Surplus warehouse is locked — adjustments may be needed instead of corrections

## Core Mechanics Reference

**Procedure**

### Shipments vs. Bills — How Landed Cost Is Built

- An **inbound shipment** picks up goods at the **base cost** it found them at (e.g., from a PO or previous warehouse). It physically brings the units into the system.
- A **Bill** does *not* transfer the sole cost. It *adds capitalizable costs* (freight, duties, etc.) to that shipment's existing base cost.
- The combination of the shipment's base cost + the bill's added costs = the final **Landed Cost**.
- Implication: if a bill is missing, the landed cost is incomplete, but the shipment still carries its base cost. Do not assume a missing bill means $0 cost on the shipment.

### FIFO Engine — Quantity Ticking and Blended COGS

- The FIFO engine does not merely pick the "oldest batch" and assign its cost universally to an invoice.
- It **strictly tracks batch quantities**. Example: an invoice requires 100 units; the oldest chronological batch has only 40 units remaining. The engine:
  1. Depletes those 40 units from the oldest batch (at that batch's cost).
  2. Moves to the *next* chronological batch to fulfill the remaining 60 units (at that batch's cost).
  3. The result is a **blended COGS** — a weighted average of the two (or more) batches consumed.
- Implication: COGS on a single invoice can reflect multiple cost layers if the oldest batch was partially depleted.

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
| Statement Analytics report | Investigate unusual Amazon outflows — liquidations, removals, adjustments. **Required for Pattern B (Safety Stock Timebomb).** |
| Landed Cost Analysis report | Verify cost propagation — PO price + logistics + landed cost bills |
| Inventory Audit report | Compare actual vs. FIFO-calculated balances, spot discrepancies. **Required for Pattern A (Offsetting Balances Cross-Check).** |
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

## Safety Stock Timebomb

**Clarifying questions**

- Is this an Amazon FBA warehouse showing sudden, massive L&F deficits?
- Does the recent supply chain data look clean — no obvious recent failures?
- When did the client onboard to NeonPanel?
- Have you checked the Statement Analytics report going back to the onboarding date?

**How to help**

- Go directly to the Statement Analytics report — do NOT start with recent transaction analysis
- Pull the full statement history dating back to the client's NeonPanel onboarding date
- Look for anomalous, massive liquidation or deduction clusters posted on a single day
- Explain the mechanism: Amazon posted historical liquidations (from months ago, pre-onboarding) onto a settlement statement after the client onboarded; NeonPanel correctly processed the statement and deducted the stock
- Explain the timebomb delay: the client had healthy safety stock that absorbed the deduction silently — the balance didn't drop below zero immediately; months later, regular sales depleted the last real stock, hit the missing cushion, and suddenly automated sales triggered massive L&F
- Confirm the anomalous statement cluster date and cross-reference it with when the safety stock would have been exhausted given normal sales velocity

**Don't claim**

- Do not look only at recent transactions when this pattern is suspected — the root cause is historical
- Do not assume the current transaction data is the source of the problem
- Do not conclude the client's recent supply chain is broken — the issue originates from pre-onboarding Amazon ledger actions

**Escalate when**

- Massive L&F deficit cannot be explained by any statement cluster — may indicate a different root cause
- Client disputes the Amazon liquidation events — may require Amazon seller support escalation

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
   - **Do NOT default to "missing bills" when L&F fires** — first check the transaction timeline for structural gaps (date sequencing errors, missing inflows, over-depleting outbound shipments)
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
   - **Fork A — Warehouse = Lost & Found:** At the exact time of the transaction, there was insufficient quantity in the intended warehouse. The $0 cost is intentional — it is a designed signal that something must be fixed. → Proceed to Phase 2 to trace backwards.
   - **Fork B — Warehouse = An actual warehouse (e.g., Amazon FBA US):** Stock was on the warehouse when the sale occurred, but was delivered with $0 cost. The inbound shipment itself had a lost batch. The problem is upstream. → Find the inbound shipment that delivered zero-cost stock, then trace that shipment's source via Phase 2.
5. **This fork prevents misdiagnosis.** Without it, you might waste time looking for why stock didn't arrive when it did arrive — just without costs.

## Advanced Pattern B: Safety Stock Timebomb

**Procedure**

*Use this when Amazon FBA suddenly shows massive L&F deficits but recent supply chain data looks flawless.*

1. **Trigger condition:** A client using automated Amazon FBA fulfillment gets sudden, massive L&F deficits in the FBA warehouse. Recent transactions look clean. No obvious recent failures.
2. **Do NOT investigate recent transactions first.** The root cause is historical, not current.
3. **Go directly to the Statement Analytics report.** Pull the full statement history dating back to the client's NeonPanel onboarding date.
4. **Look for anomalous, massive liquidation or deduction clusters** posted on a single day or within a short window.
5. **Explain the mechanism to the user:**
   - Amazon posted massive liquidations (or similar deductions) all on one day.
   - These events actually pertained to ledger actions from months ago (pre-NeonPanel onboarding), but Amazon dropped them onto a settlement statement *after* the client onboarded.
   - NeonPanel correctly processed the statement and deducted the stock.
6. **Explain the timebomb delay:**
   - Because the client had healthy safety stock, this massive deduction simply ate the cushion. The balance didn't drop below zero immediately.
   - Months later, regular sales depleted the last "real" stock, hit the missing cushion, and suddenly automated sales triggered massive L&F.
7. **Confirm the timeline:** Cross-reference the anomalous statement cluster date with the client's normal sales velocity to estimate when the safety stock would have been exhausted — this should align with when the L&F deficits began appearing.
8. **Communicate clearly:** "The root cause is not in your recent supply chain. Amazon posted historical liquidations from before your onboarding onto a later settlement statement. NeonPanel processed it correctly, but the deduction was absorbed by your safety stock. Once that cushion was depleted by normal sales, the missing inventory started triggering Lost & Found."

## Decision Flow Summary

**Procedure**

```
User reports lost COGS or lost batches
  │
  ├── Lost COGS? → Phase 1: Check warehouse on the transaction
  │     ├── Lost & Found → Insufficient qty in intended warehouse at time of transaction ($0 is intentional signal) → Phase 2
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
        ├── Step 4: Formulate hypotheses (do NOT default to missing bills for L&F)
        ├── Step 5: Guide the user (prescribe only when unambiguous)
        ├── Step 6: Hard limit — can't detect missing data that was never entered
        │
        └── Dead end? → Advanced Patterns
              ├── Pattern A: Offsetting Balances Cross-Check → Inventory Audit report
              └── Pattern B: Safety Stock Timebomb → Statement Analytics back to onboarding
```
