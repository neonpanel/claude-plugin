# Diagnosing Lost Batches Lost Cogs — Knowledge

## Reference for which NeonPanel tools and reports to use during diagnosis: FIFO Transactions report (COGS Quality, Lost Batches, Transaction List tabs), cogs_analyze_fifo_cogs tool, Statement Analytics, Landed Cost Analysis, Inventory Audit, and search_neonpanel_project_url

**What it is**

Reference for which NeonPanel tools and reports to use during diagnosis: FIFO Transactions report (COGS Quality, Lost Batches, Transaction List tabs), cogs_analyze_fifo_cogs tool, Statement Analytics, Landed Cost Analysis, Inventory Audit, and search_neonpanel_project_url.

**Key objects**

- FIFO Transactions report
- COGS Quality tab
- Lost Batches tab
- Transaction List
- cogs_analyze_fifo_cogs
- Statement Analytics
- Landed Cost Analysis
- Inventory Audit
- search_neonpanel_project_url

**How to help**

- Which report should I use?
- How do I pull lost batch data?
- Where do I find COGS quality?
- How do I check Statement Analytics?

**Clarifying questions**

- Using the wrong analysis_mode in cogs_analyze_fifo_cogs (lost_batches vs lost_cogs)
- Not knowing that Statement Analytics shows liquidations, removals, and adjustments

## Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works

**What it is**

Core concepts: what lost batches are (FIFO engine couldn't assign a source batch for outbound units), what lost COGS is (lost batches specifically on invoice/sale transactions resulting in $0 COGS), why FIFO is per-warehouse, and how same-date transaction priority works.

**Key objects**

- lost batch
- lost COGS
- FIFO queue
- outbound transaction
- invoice
- shipment
- warehouse
- same-date priority

**How to help**

- What are lost batches?
- What is lost COGS?
- Why do I have $0 COGS?
- How does FIFO work per warehouse?

**Clarifying questions**

- Confusing lost batches with lost COGS — all lost COGS are lost batches but not vice versa
- Assuming company-wide stock covers warehouse-specific FIFO — FIFO is per-warehouse
- Not understanding same-date transaction priority — outbound processes before inbound on the same date

## Formulating hypotheses after investigation (date ordering issues, missing POs, missing shipments, wrong warehouse assignments, warehouse clones, onboarding residue, anomalous Amazon adjustments, missing bill connections) and guiding the user toward resolution — prescribing only when evidence is unambiguous

**What it is**

Formulating hypotheses after investigation (date ordering issues, missing POs, missing shipments, wrong warehouse assignments, warehouse clones, onboarding residue, anomalous Amazon adjustments, missing bill connections) and guiding the user toward resolution — prescribing only when evidence is unambiguous.

**Key objects**

- date ordering issue
- missing PO
- missing shipment
- wrong warehouse
- warehouse clones
- onboarding residue
- Amazon adjustments
- missing bill connections
- locked period

**How to help**

- What's causing my lost batches?
- How do I fix this?
- Should I change the dates?
- Is there a missing PO?

**Clarifying questions**

- Prescribing fixes without sufficient evidence
- Assuming recalculation will fix everything without checking if the period is locked
- Not recognizing the hard limit — you cannot detect data that was never entered

## Phase 2 diagnostic: triaging lost batches by aggregate impact, identifying shipment/PO-level patterns, analyzing surrounding transactions (backwards and forwards in time), checking warehouse IDs vs names, spotting volume anomalies, and detecting onboarding residue

**What it is**

Phase 2 diagnostic: triaging lost batches by aggregate impact, identifying shipment/PO-level patterns, analyzing surrounding transactions (backwards and forwards in time), checking warehouse IDs vs names, spotting volume anomalies, and detecting onboarding residue.

**Key objects**

- Lost Batches tab
- shipment-level patterns
- PO-level patterns
- date ordering
- warehouse clones
- acquisition batches (ACQ)
- in-transit POs
- locked periods
- Statement Analytics

**How to help**

- I have lost batches
- Why are my batches lost?
- How do I fix lost batches?
- Which shipments are causing lost batches?

**Clarifying questions**

- Starting with individual SKUs instead of aggregate view
- Assuming warehouse names are unique — warehouse clones can exist
- Not checking backwards in time for earlier smaller problems that accumulated
- Confusing AWD and Amazon FBA as the same warehouse

## Lost Batch Tools Reference

**Clarifying questions**

- What specifically are you trying to investigate — lost COGS, lost batches, or both?

**How to help**

- For lost COGS: use FIFO Transactions report → COGS Quality tab, or cogs_analyze_fifo_cogs with analysis_mode 'lost_cogs'
- For all lost batches: use FIFO Transactions report → Lost Batches tab, or cogs_analyze_fifo_cogs with analysis_mode 'lost_batches'
- For individual transaction tracing: use FIFO Transactions report → Transaction List
- For unusual Amazon outflows: use Statement Analytics report
- For cost propagation verification: use Landed Cost Analysis report
- For balance discrepancies: use Inventory Audit report
- For finding specific POs/shipments: use search_neonpanel_project_url tool

**Don't claim**

- Do not fabricate report data or tool output

## Verified Facts

**Facts**

- Lost batches are transactions where the FIFO engine could not find or assign a source batch for units being moved — the FIFO queue was empty or no matching inventory existed in the expected warehouse.
- Lost batches can occur on any outbound transaction type — invoices (sales), shipments (stock transfers), or any other outbound movement.
- Lost COGS is a specific subset of lost batches that occurs on invoice (sale) transactions — a sale happened but no batch with a cost could be assigned, resulting in $0 COGS for those units.
- All lost COGS are lost batches, but not all lost batches are lost COGS.
- Lost COGS directly impacts the P&L — COGS is undercounted, gross profit is overstated, and financial decisions are being made on bad numbers.
- Lost batches on shipments deliver inventory to destination warehouses with $0 cost; when that zero-cost inventory eventually sells via FIFO, it becomes lost COGS.
- FIFO is per-warehouse — stock in a 3PL warehouse does not cover sales from Amazon FBA, even if total company-wide quantity appears sufficient.
- When an inbound and outbound transaction occur on the same date, NeonPanel gives priority to the outbound during FIFO recalculation, which can result in a lost batch even though both transactions exist.
- Data sources are mixed — Amazon shipments and Amazon/Shopify invoices are automatic; purchase orders and some shipments are manual/user-provided. Most data gaps originate at the boundary between automatic and manual sources.
- Lost batches appear in the FIFO Transactions report → Lost Batches tab.
- Lost COGS appears in the FIFO Transactions report → COGS Quality tab.
- The Warehouse Fork is the key diagnostic signal for lost COGS: if the warehouse is Lost & Found, stock never arrived at the selling warehouse; if it's an actual warehouse, stock arrived but with $0 cost.
- When lost COGS shows Lost & Found as the warehouse, the shipment that should have brought inventory either never happened, had wrong dates, went to the wrong warehouse, or had another upstream issue.
- When lost COGS shows an actual warehouse, the inbound shipment itself had a lost batch — the problem is further upstream in the chain, not at the invoice level.
- Lost batches triage should start with the aggregate view sorted by impact, not individual SKUs — shipment-level or PO-level patterns can resolve many SKU-level problems at once.
- Warehouse clones — two warehouses can look identical by name but be different backend entities. Always verify warehouse IDs, not just names.
- AWD (Amazon Warehousing and Distribution) and Amazon FBA are separate warehouses and should not be confused.
- Amazon can have approximately a 9-month lag between actually disposing of inventory (reflected in the ledger) and including it in a settlement statement.
- Acquisition batches (ACQ) represent opening balance stock from onboarding and may be inaccurate or in the wrong warehouses.
- If an affected period is locked, recalculation is not possible and adjustments are needed instead.
- The agent cannot detect the absence of data that was never entered — if a PO was never created, there is no trace of it in the system.
- The cogs_analyze_fifo_cogs tool supports analysis_mode 'lost_batches' for all lost batches and 'lost_cogs' for lost COGS specifically with quality metrics.
- Statement Analytics report in NeonPanel shows liquidations, removals, and adjustments for investigating unusual Amazon outflows.
- The agent should guide the user toward resolution rather than prescribe fixes, except when evidence is unambiguous.

## Phase 2: Lost Batches General Approach

**Procedure**

1. **Triage — Aggregate view first.** Do NOT start with individual SKUs. Pull lost batches data sorted by impact (units or dollar amount, descending).
2. Look for **shipment-level or PO-level patterns**: if the same shipment causes lost batches across many products, that's one shipment-level problem, not many SKU problems. Identifying these patterns early is critical.
3. Prioritize by materiality — tackle the biggest impact first.
4. **Select individual SKUs/inventory IDs** from the top contributors for detailed investigation.
5. **Analyze surrounding transactions** for each affected SKU:
   - Check Lost & Found warehouse transactions for this item — how many, over what period?
   - Identify origin and destination warehouses of the problematic shipment(s).
   - **Look backwards in time:** Were there smaller problems earlier? Are there POs around or before the lost batch dates that were received into the right warehouse? Is there a date ordering issue (outbound before inbound)?
   - **Same-date variant:** Both transactions on the same date, but NeonPanel's processing priority causes the outbound to run first.
   - **Different-date variant:** The outbound shipment has an earlier date than the inbound.
   - **Look forwards in time:** Are there subsequent shipments with date errors? Did stock eventually arrive but too late?
   - **Check warehouse IDs vs names:** Verify IDs, not just names — warehouse clones exist. AWD and Amazon FBA are separate warehouses.
   - **Check for volume anomalies:** Unusual outflows may indicate liquidations, removals, or adjustments. Check Statement Analytics. Note: Amazon can have ~9 month lag between disposal and settlement.
   - **Check for onboarding residue:** Acquisition batches (ACQ) accuracy, in-transit POs from onboarding, whether the affected period is locked.
6. **Formulate hypotheses** only after examining the full transaction context. Possible causes include:
   - Date ordering issue — inbound and outbound dates need correcting
   - Missing PO — a purchase order was never entered
   - Missing shipment — a stock transfer was never recorded
   - Wrong warehouse assignment — shipment pulling from or delivering to the wrong warehouse
   - Warehouse clones — stock split across what looks like the same warehouse but isn't
   - Onboarding residue — stale acquisition batches or unresolved in-transit POs
   - Anomalous Amazon adjustments — liquidations, removals, or statement items with long lags
   - Missing bill connections — PO price exists but logistics or landed cost bills aren't connected
7. **Guide, don't prescribe** — the user must make the final call in most cases. Prescribe only when evidence is unambiguous.
8. **Hard limit:** You cannot detect the absence of data that was never entered. When you've traced back and can't find a source, tell the user: "I've traced back through the transactions and can't find a source for this stock. This likely means there's a document — possibly a PO or a shipment — that was never entered into the system. Can you check your records for any orders around [date range] that might not have been recorded?"

## Phase 1 diagnostic: assessing COGS quality, identifying worst offenders by SKU/brand, and applying the Warehouse Fork — checking whether the lost COGS transaction is on Lost & Found (stock never arrived) vs

**What it is**

Phase 1 diagnostic: assessing COGS quality, identifying worst offenders by SKU/brand, and applying the Warehouse Fork — checking whether the lost COGS transaction is on Lost & Found (stock never arrived) vs. an actual warehouse (stock arrived with $0 cost).

**Key objects**

- COGS quality
- COGS Quality tab
- Lost & Found warehouse
- warehouse fork
- investigation threshold
- materiality

**How to help**

- I have lost COGS
- My COGS quality is bad
- Why is my COGS $0?
- Which SKUs have the worst COGS quality?

**Clarifying questions**

- Skipping the warehouse fork and assuming all lost COGS means stock never arrived
- Not checking whether stock arrived with $0 cost vs. never arrived at all

## Lost Batch Hypotheses

**Clarifying questions**

- Based on the transaction analysis, which hypothesis seems most likely to you?
- Do you have access to external records (e.g., supplier POs, shipping confirmations) that could confirm or rule out a missing document?

**How to help**

- Present the possible hypotheses based on evidence: date ordering, missing PO, missing shipment, wrong warehouse, warehouse clones, onboarding residue, Amazon adjustments, missing bill connections
- Guide the user toward the most likely cause based on available evidence
- Prescribe a fix ONLY when evidence is unambiguous
- When data that was never entered is suspected, tell the user to check their records for orders around the relevant date range
- Check if the period is locked before suggesting recalculation

**Don't claim**

- Do not prescribe fixes without sufficient evidence
- Do not claim to know what data is missing — only narrow down where the gap likely is
- Do not promise recalculation will fix the issue without checking locked periods

**Escalate when**

- Multiple hypotheses are equally plausible and user cannot determine the cause
- Fix requires bulk updates across many shipments or POs
- Locked period prevents recalculation

## Warehouse Fork Analysis

**Clarifying questions**

- What warehouse is shown on the lost COGS transaction?
- Is it showing Lost & Found or an actual warehouse name?

**How to help**

- If Lost & Found: the stock was not on the selling warehouse at all — trace backwards to find why stock never arrived
- If actual warehouse: the stock was there but with $0 cost — find the inbound shipment that delivered zero-cost stock
- For actual warehouse cases, trace the inbound shipment's source backwards through the lost batches investigation

**Don't claim**

- Do not skip the warehouse fork — it prevents misdiagnosis
- Do not assume all lost COGS means stock never arrived

**Escalate when**

- Both fork paths lead to the same dead end across multiple SKUs

## Lost Batches Diagnosis

**Clarifying questions**

- Which company and time period are you looking at?
- Are you seeing lost batches on shipments, invoices, or both?
- Have you already identified specific SKUs or is this a general concern?

**How to help**

- Pull lost batches data using cogs_analyze_fifo_cogs with analysis_mode 'lost_batches'
- Sort by impact (units or dollar amount, descending)
- Look for shipment-level or PO-level patterns — one shipment issue can cause lost batches across many SKUs
- Prioritize by materiality — tackle biggest impact first
- Select individual SKUs from top contributors for detailed investigation
- Analyze surrounding transactions for each affected SKU

**Don't claim**

- Do not claim to know what data is missing — only state where the gap likely is
- Do not promise recalculation will fix lost batches without checking if the period is locked
- Do not assume lost batches are always errors
- Do not prescribe fixes without sufficient evidence — guide instead

**Escalate when**

- Lost batches span a locked accounting period
- Large volume across multiple SKUs suggests a systematic issue
- Customer has already attempted fixes that didn't resolve the problem
- Bulk warehouse updates are needed across many shipments

## Transaction Tracing

**Clarifying questions**

- Which SKU/inventory ID are you investigating?
- What date range should I look at for surrounding transactions?

**How to help**

- Check Lost & Found warehouse transactions for this item
- Identify origin and destination warehouses of the problematic shipment(s)
- Look backwards: check for POs around or before lost batch dates, earlier smaller problems, date ordering issues
- Look forwards: check for subsequent shipments with date errors or delayed arrivals
- Verify warehouse IDs vs names — check for warehouse clones
- Check for volume anomalies — unusual outflows may indicate liquidations or removals (check Statement Analytics)
- Check for onboarding residue: acquisition batches (ACQ), in-transit POs, locked periods

**Don't claim**

- Do not fabricate transaction data
- Do not assume warehouse names are unique without checking IDs

**Escalate when**

- Investigation reveals the affected period is locked
- Volume anomalies suggest Amazon-side adjustments with long lag times

## Phase 1: Lost COGS Triage

**Procedure**

1. Assess overall COGS quality using `cogs_analyze_fifo_cogs` with analysis_mode 'lost_cogs'.
2. Ask the user for scope: specific SKU, brand, or entire company. Ask for time period and investigation threshold.
3. Sort results by quality percentage ascending to surface worst offenders first.
4. Identify materiality: count affected units and estimate lost COGS in dollar terms.
5. **Apply the Warehouse Fork** for each lost COGS entry — check the warehouse on the transaction:
   - **Fork A (Lost & Found):** Stock was not on the selling warehouse at all. The shipment that should have brought inventory either never happened, had wrong dates, went to the wrong warehouse, or had another upstream issue. Proceed to Phase 2.
   - **Fork B (Actual warehouse):** Stock was on the warehouse but delivered with $0 cost. The inbound shipment itself had a lost batch. Find that inbound shipment, then trace its source backwards via Phase 2.
6. The Warehouse Fork prevents misdiagnosis — without it, you might look for why stock didn't arrive when it actually did arrive, just without costs.

## Lost Cogs Diagnosis

**Clarifying questions**

- Do you want to check a specific SKU, brand, or the entire company?
- What time period are you concerned about?
- Do you have an investigation threshold — a minimum unit count or dollar amount below which you don't want to investigate?

**How to help**

- Pull COGS quality data using cogs_analyze_fifo_cogs with analysis_mode 'lost_cogs'
- Sort by quality percentage ascending to surface worst offenders first
- Identify materiality: how many units, how much estimated lost COGS in dollar terms
- Apply the Warehouse Fork: check the warehouse on each lost COGS transaction
- If warehouse = Lost & Found → stock never arrived, proceed to lost batches investigation
- If warehouse = actual warehouse → stock arrived with $0 cost, find the inbound shipment and trace upstream

**Don't claim**

- Do not claim to know what data is missing — only state where the gap likely is
- Do not promise recalculation will fix lost COGS without checking if the period is locked
- Do not assume lost batches are always errors — they may result from legitimate business scenarios
- Do not fabricate quality scores or percentages without pulling actual data

**Escalate when**

- Lost COGS spans a locked accounting period
- Large volume across multiple SKUs suggests a systematic issue
- Customer has already attempted fixes that didn't resolve the problem

## Quick-Reference Decision Flow

**Procedure**

1. User reports lost COGS → Start Phase 1 (Warehouse Fork).
   - Lost & Found → Stock never arrived → Phase 2.
   - Actual warehouse → Stock arrived with $0 cost → Find inbound shipment → Phase 2 for that shipment.
2. User reports lost batches → Start Phase 2 directly.
   - Aggregate view → Top contributors → Shipment/PO patterns.
   - Select individual SKUs → Analyze surrounding transactions.
   - Formulate hypotheses → Guide user → Acknowledge hard limits.
