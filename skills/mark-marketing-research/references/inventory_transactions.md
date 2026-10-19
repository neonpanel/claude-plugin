# Inventory Transactions — Knowledge

## When and how to correct erroneous transactions — covering reversals, adjustments, and re-entry, with guidance on which method is appropriate for each scenario

**What it is**

When and how to correct erroneous transactions — covering reversals, adjustments, and re-entry, with guidance on which method is appropriate for each scenario.

**Key objects**

- reversal
- adjustment
- re-entry
- transaction date
- FIFO impact
- correction

**How to help**

- How do I fix a transaction entered with the wrong date?
- How do I undo a transaction?
- When should I use a reversal vs. an adjustment?
- Can I just delete a transaction and re-enter it?
- How do I correct a bill that was entered with the wrong amount?

**Clarifying questions**

- Deleting and re-entering transactions instead of using reversals — this can corrupt FIFO sequencing
- Using adjustments to fix date errors — adjustments change quantity/cost but not sequencing
- Not understanding that a reversal must mirror the original transaction date to correctly unwind FIFO

**Clarifying questions**

- What type of error needs correcting — wrong date, wrong quantity, wrong cost, or wrong warehouse?
- Has the transaction already affected downstream COGS or been included in a closed period?
- Do you know the original transaction date?

**How to help**

- For wrong date errors: create a reversal mirroring the original transaction on the original date, then re-enter with the correct date
- For wrong quantity or cost: use a reversal if the transaction has already affected FIFO; use an adjustment only for physical count corrections
- Never delete and re-enter a transaction without first reversing it — this can corrupt FIFO sequencing

**Don't claim**

- You can fix a date error by editing the transaction directly
- Deleting and re-entering a transaction is equivalent to reversing it
- Adjustments are the correct tool for fixing sequencing errors

**Escalate when**

- The error occurred in a closed accounting period
- The correction would affect a large number of downstream COGS calculations

## Tool Execute Sql

**Clarifying questions**

- Is this a read query or a write query?
- Which table or dataset are you targeting?

**How to help**

- Use execute_sql for ad-hoc queries and data inspection
- For schema changes or bulk data operations, use apply_migration instead

**Escalate when**

- Query would modify or delete production data without a confirmed backup

## Triage: User-Generated vs. Integration-Sourced Documents

**Procedure**

Run this check **first** whenever a sales document (invoice) or shipment is implicated in a discrepancy.

1. Determine the **source** of the document:
   - **Integration-sourced** (Amazon, Shopify, or other platform integration) → go to step 2
   - **User-generated** (manually entered by the user) → go to step 3

2. **Integration-sourced document:**
   - Treat the document itself as reliable — platform integrations rarely produce erroneous transactions.
   - The issue almost certainly lies **upstream**: missing inventory, an unrecorded inbound, a timing gap, or a prior transaction that never posted.
   - Proceed directly to the **How to Read a Transaction Timeline for a SKU** playbook and look for upstream gaps.

3. **User-generated document:**
   - Check for **obvious user error first** before investigating upstream:
     - **Date inversion**: shipment sent date is *after* the shipment received date — this is a common entry mistake.
     - **Approximate dates**: users sometimes enter rounded or estimated dates (e.g., "first of the month"). Because the engine processes transactions with precise sequencing, an approximate date can cause a shipment to attempt to pick up goods that haven't arrived yet (outbound date lands before the inbound date).
     - **Wrong warehouse**: document entered against the wrong location.
   - If a date inversion or sequencing error is found, use the **How to Correct a Transaction Error** playbook to reverse and re-enter with the correct dates.
   - If no user error is found, treat the document as reliable and look upstream as in step 2.

---

## How to Correct a Transaction Error

**Procedure**

Use this playbook to fix wrong dates, quantities, costs, or warehouse assignments without corrupting FIFO sequencing.

1. Identify the **error type**:
   - **Wrong date** → reversal + re-entry required
   - **Wrong quantity or cost** → reversal if FIFO-affecting; adjustment only for physical count corrections
   - **Wrong warehouse** → reversal on original warehouse + new transaction on correct warehouse
2. Confirm the **original transaction date** — the reversal must mirror this date exactly to correctly unwind FIFO.
3. Create the **reversal**: enter a transaction of the same type, same date, same warehouse, with the quantity/cost negated.
4. Verify the running balance returns to the pre-error state after the reversal posts.
5. Re-enter the transaction with the **correct values**.
6. Re-check the transaction timeline to confirm the running balance is now correct downstream.
7. **Never delete and re-enter** without reversing first — deletion without reversal leaves FIFO in a corrupted state.

---

## Tool Apply Migration

**Clarifying questions**

- What migration are you trying to apply?
- Has the migration been tested in a staging environment first?

**How to help**

- Confirm the migration script and target environment before executing
- Use apply_migration for schema or data migrations; use execute_sql for ad-hoc queries

**Escalate when**

- Migration affects live production data without a backup

## Transaction Type Quick Reference

**Procedure**

| Transaction Type | Affects Quantity | Affects Cost | Generates COGS | Notes |
|---|---|---|---|---|
| Inbound Shipment | ✅ | ✅ (base cost) | ❌ | Brings units in at base cost from PO or prior warehouse |
| Bill | ❌ | ✅ (adds to shipment) | ❌ | Adds freight, duties, etc. to an existing shipment's base cost |
| Outbound Shipment / Invoice | ✅ | ✅ (depletes FIFO) | ✅ | Consumes oldest batches first; generates COGS |
| Transfer | ✅ | ❌ | ❌ | Moves quantity between warehouses; does not affect unit cost |
| Adjustment | ✅ or ✅ | ✅ or ❌ | ❌ | For physical count corrections only; does not fix sequencing errors |
| Reversal | Mirrors original | Mirrors original | Mirrors original | Must use original transaction date to correctly unwind FIFO |

## Verified Facts

**Facts**

- An inbound shipment brings units into a warehouse at a base cost sourced from the originating PO or prior warehouse; a bill does not transfer cost independently — it adds capitalizable costs (freight, duties, etc.) to that shipment's existing base cost to form the final Landed Cost.
- Transfers move quantity between warehouses only — they do not generate COGS and do not affect unit cost.
- Adjustments change quantity or cost directly and are appropriate for physical count corrections; they are not the correct tool for fixing sequencing or date errors.
- A reversal unwinds a transaction by mirroring it with the same date — it must use the original transaction date to correctly unwind FIFO sequencing.
- The FIFO engine uses transaction date (not posting date) to determine batch sequence; a transaction entered late but dated correctly will be sequenced correctly.
- Transaction date is when the event occurred; posting date is when it was entered into NeonPanel. These can differ and the distinction matters for FIFO sequencing audits.
- Outbound shipments and invoices consume inventory and generate COGS; inbound shipments, transfers, and bills do not generate COGS on their own.

## A taxonomy of all transaction types in NeonPanel — what each one does, whether it affects quantity, cost, or both, and which warehouses are involved

**What it is**

A taxonomy of all transaction types in NeonPanel — what each one does, whether it affects quantity, cost, or both, and which warehouses are involved.

**Key objects**

- inbound shipment
- outbound shipment
- transfer
- adjustment
- bill
- reversal
- COGS
- landed cost
- warehouse

**How to help**

- What is the difference between a shipment and a bill?
- Which transaction types generate COGS?
- What does a transfer do vs. a shipment?
- When should I use an adjustment vs. a reversal?
- Why did my cost change after I added a bill?

**Clarifying questions**

- Assuming a bill transfers cost independently — it only adds capitalizable costs to an existing shipment's base cost
- Confusing inbound shipment (brings units in) with a bill (adds cost to an existing shipment)
- Using adjustments to fix sequencing errors instead of reversals
- Not knowing that transfers move quantity only — they do not generate COGS

**Clarifying questions**

- Which transaction type are you working with — inbound shipment, bill, transfer, adjustment, or outbound?
- Are you seeing an unexpected cost change, or a quantity discrepancy?
- Which warehouse and SKU is affected?

**How to help**

- Identify the transaction type and confirm what it should affect (quantity, cost, or both)
- If cost changed unexpectedly after a bill, verify the bill was applied to the correct shipment
- If a transfer was used but cost changed, investigate whether an inbound shipment was accidentally created instead

**Don't claim**

- Bills transfer the full cost of goods independently
- Transfers generate COGS
- Adjustments can fix FIFO sequencing errors caused by wrong transaction dates

**Escalate when**

- Transaction type is not one of the known types and behavior is unexpected
- Cost is changing on transactions that should not affect cost

## How to Read a Transaction Timeline for a SKU

**Procedure**

Use this playbook whenever investigating an L&F pull, unexpected COGS figure, or inventory discrepancy.

1. Identify the **SKU** and **intended warehouse** under investigation.
2. Export the full transaction timeline for that SKU and warehouse — include all transaction types (inbound, outbound, transfer, bill, adjustment).
3. Sort the timeline by **transaction date** (not posting date). FIFO sequencing is driven by transaction date.
4. Add a running balance column. Recalculate the balance at each row: prior balance + inbound quantity − outbound quantity.
5. Scan for the first point where the running balance goes **negative or drops unexpectedly** — this is the L&F trigger point.
6. Look **upstream** from the trigger point for:
   - Missing inflows (PO not checked in, transfer never recorded)
   - Date sequencing errors (outbound dated before the corresponding inbound)
   - Over-depleting earlier outbound transactions
7. If no structural gap is found, escalate to the Offsetting Balances Cross-Check playbook (in the Lost Batches & COGS Diagnosis skill).

---

## How to read and interpret a full transaction timeline for a SKU in a given warehouse — the foundational skill used in all L&F and COGS investigations

**What it is**

How to read and interpret a full transaction timeline for a SKU in a given warehouse — the foundational skill used in all L&F and COGS investigations.

**Key objects**

- transaction timeline
- SKU
- warehouse
- running balance
- transaction date
- posting date
- FIFO sequence
- inbound
- outbound

**How to help**

- How do I read a transaction timeline?
- How do I find where inventory went missing?
- How do I check if transactions are in the right order?
- What does a healthy transaction timeline look like?
- How do I spot a sequencing error in the timeline?

**Clarifying questions**

- Confusing transaction date (when it happened) with posting date (when it was entered) — FIFO uses transaction date
- Looking only at recent transactions when the root cause is an old sequencing error
- Assuming a negative running balance is always a data entry error — it may be an L&F signal

**Clarifying questions**

- What SKU and warehouse are you investigating?
- What date range should the timeline cover?
- Are you seeing a negative running balance, an L&F pull, or an unexpected COGS figure?

**How to help**

- Export the full transaction timeline for the SKU and warehouse
- Sort by transaction date (not posting date) and verify the running balance at each step
- Flag any point where the running balance goes negative or drops unexpectedly — this is the L&F trigger point
- Look upstream from the trigger point for missing inflows, wrong dates, or over-depleting outbounds

**Don't claim**

- A negative running balance always means a data entry error
- Looking only at recent transactions is sufficient to find the root cause

**Escalate when**

- Running balance goes negative before any outbound transactions exist
- Timeline shows correct inflows and outflows but balance is still wrong
