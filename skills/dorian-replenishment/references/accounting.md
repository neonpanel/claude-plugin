# Accounting — Knowledge

# Accounting Playbooks

1. **Sync failed** (`acct-sync-status`)
   - Symptom: Sync fails
   - Checks: integration, error message
   - Outcome: retry or escalate

2. **Bill missing** (`acct-bill-missing`)
   - Symptom: Bill missing
   - Checks: bill ID, vendor
   - Outcome: confirm status in source system

3. **Mapping mismatch** (`acct-mapping-errors`)
   - Symptom: Mapping mismatch
   - Checks: account names, mapping export
   - Outcome: request mapping export

4. **Totals mismatch** (`acct-reconciliation`)
   - Symptom: Totals mismatch
   - Checks: report period
   - Outcome: request report export

5. **Auth expired** (`acct-auth`)
   - Symptom: Authorization error
   - Checks: error message
   - Outcome: re-auth or escalate

6. **Duplicate entries** (`acct-sync-status`)
   - Symptom: Duplicates
   - Checks: sync job ID
   - Outcome: collect evidence

7. **Vendor mapping** (`acct-bill-missing`)
   - Symptom: Vendor mismatch
   - Checks: vendor name
   - Outcome: request mapping export

8. **Chart mismatch** (`acct-mapping-errors`)
   - Symptom: Chart of accounts differs
   - Checks: chart export
   - Outcome: request export

9. **Reconciliation audit** (`acct-reconciliation`)
   - Symptom: Audit request
   - Checks: period, report type
   - Outcome: collect exports and escalate

10. **Repeated auth failures** (`acct-auth`)
    - Symptom: Auth fails repeatedly
    - Checks: timestamp
    - Outcome: escalate

11. **Landed cost services missing** (`acct-mapping-errors`)
   - Symptom: Only Amazon/non-landed services appear in Bill Item List
   - Checks: whether a shipment or inventory order was attached in Bill Info
   - Outcome: attach shipment/order in Bill Info, then re-open Item List

## Stock Risk

**Clarifying questions**

- Which warehouse or company?
- What is your risk threshold?

**How to help**

- use supply_chain_list_stock_replenishment_risk_items tool
- identify high-risk items
- provide mitigation recommendations
- explain risk scoring

**Don't claim**

- automatic risk prevention

**Escalate when**

- critical stockout risk

## Replenishment Analysis

**Clarifying questions**

- Which warehouse or company?
- What is your target service level?

**How to help**

- use supply_chain_list_fba_replenishment_candidates tool
- identify at-risk items
- provide reorder recommendations
- explain coverage metrics

**Don't claim**

- automatic reordering

## Metric Drilldown

**Clarifying questions**

- Which metric or report are you looking at?
- Do you want the breakdown by SKU, category, or warehouse?
- What date range should I analyze?

**How to help**

- ALWAYS drill down to the most granular level available (SKU or transaction) in the first response
- Query the full dataset — never summarize into catch-all categories like Other or Untracked
- Present all items ranked by impact, not just the top 3
- If the dataset is large, show the top 10-15 with a note about remaining items, but never lump them into a generic Other bucket
- If granular data is unavailable, state that explicitly rather than speculating

**Don't claim**

- Never present an Other or Untracked catch-all category that hides the actual breakdown
- Never speculate about what might be in an aggregated category — run the query instead
- Never provide only a partial top-N list without offering to show the full breakdown

**Escalate when**

- Granular data is genuinely unavailable from the data source
- Query returns errors or incomplete results

## Landed cost tracking, freight and duty allocation, and landed cost analytics

**What it is**

Landed cost tracking, freight and duty allocation, and landed cost analytics. Covers how additional costs (shipping, customs, duties) are allocated to inventory items.

**Key objects**

- Landed cost
- Freight cost
- Duty allocation
- Cost allocation rules
- Landed cost report

**How to help**

- How are landed costs calculated?
- Add freight costs to items
- View landed cost breakdown
- Set up cost allocation rules

**Clarifying questions**

- Confusing landed cost with purchase price
- Not understanding that landed costs are allocated across items in a shipment

**Clarifying questions**

- Is a shipment or inventory order attached in Bill Info?

**How to help**

- confirm Bill Info attachment
- attach shipment/order if missing
- re-open Item List
- verify service mapping

**Don't claim**

- landed cost appears without attachment

## Velocity Analysis

**Clarifying questions**

- Which product or warehouse?
- What time period?

**How to help**

- use supply_chain_analyze_sales_velocity tool
- explain velocity trends
- provide demand insights
- reference velocity dashboard

**Don't claim**

- velocity is static

## Artifact Requests

# Accounting Artifact Requests

- Integration name
- Sync job ID and timestamp
- Bill/Invoice IDs
- Vendor name(s)
- Mapping export or chart of accounts export
- Screenshots: sync status, error message
- Date range and report export

## Landed Cost Analytics

**Clarifying questions**

- Which warehouse or period?
- What metrics are you interested in?

**How to help**

- explain landed cost dashboard
- guide on health metrics
- provide interpretation guidance
- reference analytics guide

**Don't claim**

- landed cost auto-calculates

## Account and category mapping

**What it is**

Account and category mapping.

**Key objects**

- Account
- Category
- Mapping

**How to help**

- map accounts
- category mismatch

**Clarifying questions**

- Not understanding that mapping changes affect historical data
- chart of accounts vs categories
- account_name
- category_name
- mapping_export

## Transaction Mapping

**Clarifying questions**

- Which transaction type? (Inventory, Sales, Purchases)
- Is this for understanding or troubleshooting?

**How to help**

- explain transaction mapping
- reference transaction map guide
- clarify warehouse-bound transactions
- provide examples

**Don't claim**

- transaction mapping is automatic

## Agl Setup

**Clarifying questions**

- Are you setting up AGL for the first time?
- Do you have the AGL vendor configured?

**How to help**

- provide AGL setup steps
- confirm custom FBA freight services
- validate default AGL account
- guide through example entries

**Don't claim**

- AGL auto-configures

## Starting Balances

**Clarifying questions**

- Which warehouse or company?
- Do you have the balance data?

**How to help**

- guide on balance entry
- confirm data verification
- validate totals
- provide review checklist

**Don't claim**

- starting balances auto-populate

**Escalate when**

- balance discrepancies

## Sync Status

**Clarifying questions**

- Which integration? (QBO or Xero)
- Do you have a sync job ID?
- What is the error message?

**How to help**

- collect error message
- confirm last successful sync
- check integration status
- identify failure pattern

**Don't claim**

- exact sync frequency
- automatic retry

**Escalate when**

- repeated sync failure
- financial data at risk
- sync blocked for >24 hours

## User Memory

**Clarifying questions**

- What data do you want to store or retrieve?
- Is this for customer data or internal tracking?

**How to help**

- use execute_sql tool for queries
- explain User Memory schema
- guide on data persistence
- provide query examples

**Don't claim**

- User Memory is automatic

**Escalate when**

- data privacy concerns

## Profit Analytics Beta

**Clarifying questions**

- Are you using the beta feature?
- Which dashboard or metric?

**How to help**

- explain beta feature scope
- guide on filters and settings
- reference KPI dashboard
- provide interpretation guidance

**Don't claim**

- profit analytics is production-ready

## Bill Missing

**Clarifying questions**

- What is the bill/invoice ID?
- Which vendor?
- Which warehouse or company?

**How to help**

- confirm status in source system
- check Bill Info attachments
- verify shipment/order link
- identify creation trigger

**Don't claim**

- status mapping rules
- automatic bill creation

**Escalate when**

- financial reconciliation impacted
- audit required

## Fifo Analytics

**Clarifying questions**

- Which warehouse or period?
- What is the specific concern?

**How to help**

- explain FIFO transaction tracking
- provide COGS quality metrics
- reference analytics dashboard
- guide on interpretation

**Don't claim**

- COGS quality auto-corrects

**Escalate when**

- COGS discrepancies

## Bills, invoices, and payables

**What it is**

Bills, invoices, and payables.

**Key objects**

- Bill
- Invoice
- Vendor

**How to help**

- bill missing
- invoice missing

**Clarifying questions**

- Confusing bills (payables) with invoices (receivables)
- bill vs invoice
- bill_id
- invoice_id
- vendor_name

## Accounting system connection and synchronization

**What it is**

Accounting system connection and synchronization.

**Key objects**

- Connection
- Sync Job
- Ledger Snapshot

**How to help**

- connect accounting
- sync accounting

**Clarifying questions**

- Confusing sync errors with data import errors
- sync vs export
- sync timing
- integration_name
- sync_job_id
- error_message

## Inventory Audit

**Clarifying questions**

- Which warehouse or period?
- Are you auditing against 3PL or internal records?

**How to help**

- explain audit process
- guide on reconciliation
- provide audit checklist
- reference audit guide

**Don't claim**

- audits auto-complete

**Escalate when**

- audit discrepancies

## Metric Contributor / Breakdown Drilldown

**Procedure**

When a user asks about contributors, breakdowns, or what's driving a metric (e.g. "what are the biggest contributors to lost COGS?"):

1. **Identify the metric and date range.** Confirm which metric the user is asking about (COGS, revenue, landed cost, etc.) and the time period.
2. **Query at the most granular level immediately.** Always drill down to SKU-level or transaction-level data in the first response. Do NOT summarize into high-level buckets.
3. **Present all items ranked by impact.** Show results sorted by contribution (highest to lowest). If there are many items, show the top 10–15 with their individual values and percentages.
4. **Never create catch-all categories.** Do not lump remaining items into "Other", "Untracked", or similar buckets. If the list is long, offer to show the next batch — but always with individual line items.
5. **If granular data is unavailable, say so explicitly.** State "I don't have SKU-level data for this metric" rather than speculating or filling in with aggregated placeholders.
6. **Offer deeper analysis.** After presenting the breakdown, proactively offer to drill further (e.g. "Want me to look at the transaction history for any of these SKUs?").

## FIFO inventory valuation, COGS calculations, unit cost analysis, and inventory audits

**What it is**

FIFO inventory valuation, COGS calculations, unit cost analysis, and inventory audits. Covers how inventory costs flow using first-in-first-out methodology and how to audit inventory values.

**Key objects**

- FIFO valuation
- COGS
- Unit cost
- Inventory audit
- Cost layers
- Inventory value

**How to help**

- How is COGS calculated?
- Show me unit costs
- Run an inventory audit
- Explain FIFO valuation
- Why did my inventory value change?

**Clarifying questions**

- Confusing FIFO with average cost methods
- Not understanding that COGS depends on purchase order sequence
- Mixing up inventory value with inventory count

**Clarifying questions**

- Which warehouse or period?
- Which inventory movement?

**How to help**

- explain FIFO method
- reference warehouse-specific calculation
- provide valuation details

**Don't claim**

- FIFO not used
- other valuation methods

## Mapping Errors

**Clarifying questions**

- Which account or service?
- Which category is affected?
- Is this for Bills or another document?

**How to help**

- request mapping export
- identify mismatch
- confirm correct mapping
- validate Chart of Accounts

**Don't claim**

- default mapping rules
- automatic remapping

**Escalate when**

- mapping changes affect financials

## Financial reconciliation, revenue and COGS reporting, and verifying that totals match across systems

**What it is**

Financial reconciliation, revenue and COGS reporting, and verifying that totals match across systems. Covers profit analysis and cross-system data validation.

**Key objects**

- Reconciliation report
- Revenue totals
- COGS totals
- Profit margins
- Discrepancies
- Cross-system validation

**How to help**

- Reconcile my accounts
- Why don't my totals match?
- Show revenue vs COGS
- Check profit margins
- Find discrepancies

**Clarifying questions**

- Expecting automatic reconciliation without manual review
- Confusing reconciliation with sync

**Clarifying questions**

- Which report and period?
- What is the expected total?
- Which warehouse or company?

**How to help**

- request report export
- compare source vs system
- identify discrepancies
- confirm root cause

**Don't claim**

- reconciliation rules
- automatic reconciliation

**Escalate when**

- audit required
- financial impact

## Chart Of Accounts

**Clarifying questions**

- Are you importing from QBO/Xero or using NeonPanel default?

**How to help**

- confirm CoA source
- explain service mapping requirements
- provide setup guidance

**Don't claim**

- CoA changes don't affect mappings

## Po Placement

**Clarifying questions**

- Which vendor or supplier?
- What is your lead time?

**How to help**

- use supply_chain_list_po_placement_candidates tool
- identify candidates
- provide ordering guidance
- confirm quantities

**Don't claim**

- automatic PO creation

## Revenue Reporting

**Clarifying questions**

- Which period or company?
- What level of detail?

**How to help**

- use neonpanel_getRevenueAndCogs tool
- provide revenue/COGS summary
- explain variance analysis
- guide on reporting

**Don't claim**

- revenue is automatic

## Integration Setup

**Clarifying questions**

- Which accounting system? (QBO or Xero)
- Is this first-time setup?

**How to help**

- provide integration steps
- confirm credentials
- validate connection
- test sync

**Don't claim**

- automatic integration
- no setup required

## Auth

**Clarifying questions**

- Which integration? (QBO or Xero)
- What is the exact error message?

**How to help**

- collect timestamp
- request re-authentication
- confirm credentials
- validate token status

**Don't claim**

- token lifetime
- automatic token refresh

**Escalate when**

- repeated auth failures
- credentials compromised

## Verified Facts

**Facts**

- NeonPanel integrates with QuickBooks Online and Xero for accounting workflows.
- QuickBooks Online integration exports data from NeonPanel to QBO; it does not sync journal entries back into NeonPanel.
- Chart of Accounts, Vendors, and Customers can be imported from QBO or Xero into NeonPanel.
- Users can use NeonPanel's default Chart of Accounts or their own Chart of Accounts; using a custom CoA requires reconfiguring service-account mappings in the Services catalog.
- NeonPanel creates a Service Item list that includes Amazon and Shopify services and landed cost services during onboarding.
- In Bills, landed cost services appear in the Item List only after a shipment or inventory order is attached in the Bill Info step; if none is attached, only non-landed services appear.
- NeonPanel calculates COGS using the FIFO method; in general, FIFO is used to make all Inventory Management transactions.
- Services in NeonPanel must be mapped to Service Accounts in the Chart of Accounts for accurate accounting and reporting.
- NeonPanel imports Amazon statements and Shopify orders, and generates invoices and journal entries based on that data.
- NeonPanel does not create Cash vs Accounts Payable transactions; those are recorded directly in the accounting system via their connection to banks.
- QuickBooks Online sync settings support Manual, On (Auto), and Off options.
- Inventory-related accounting transactions are warehouse-bound. Inventory Orders (POs) and Invoices reference exactly one warehouse. Shipments reference an Origin and a Destination warehouse. Bills are linked to orders or shipments which carry the warehouse association.
- FIFO transactions are calculated per warehouse. Moving goods between warehouses (via shipments) creates separate FIFO events at the Origin and Destination.
- Amazon Global Logistics (AGL) services must be configured separately for freight cost tracking and require a custom FBA Freight service and default AGL Services vendor account.
- Starting balances must be entered and verified before go-live to ensure accurate inventory and financial reporting.
- Transaction mapping defines how NeonPanel documents (Orders, Bills, Shipments, Invoices) flow into accounting systems and how warehouse associations are maintained.
- FIFO transaction quality metrics track the health and accuracy of COGS calculations per warehouse.
- Landed cost analytics provide visibility into service costs, freight expenses, and cost distribution across inventory movements.
- Inventory audits reconcile NeonPanel inventory records against 3PL or internal warehouse records to ensure data accuracy.
- Amazon Profit Analytics (beta) provides KPI dashboards, profit and loss analysis, and COGS variation tracking for Amazon sales channels.
- When a user asks about contributors, breakdowns, or causes of a metric, the agent must always drill down to the most granular level available (SKU or transaction level) in the first response — never use catch-all categories like Other or Untracked that hide the actual data.

## Unit Costs

**Clarifying questions**

- Which period or product?
- What is the intended use?

**How to help**

- use cogs_export_unit_costs tool
- provide cost export
- explain cost methodology
- guide on cost analysis

**Don't claim**

- costs are fixed

## Project Search

**Clarifying questions**

- What is the project name or reference?

**How to help**

- use search_neonpanel_project_url tool
- provide project link
- explain project scope
