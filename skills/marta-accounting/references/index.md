# Agent Knowledge Index

## Other references

- Brand and company context: `references/brand-neonpanel_brand_pack.md`
- MCP tool guidance: `references/tools.md`

## Knowledge areas

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
