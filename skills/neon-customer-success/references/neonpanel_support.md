# Neonpanel Support — Knowledge

## Verified Facts

**Facts**

- Supported bulk upload types are Inventory Orders, Bills, Shipments, Invoices, and Forecasts.
- NeonPanel uploads accept CSV files only.
- Each upload template contains an instruction sheet and an example data sheet.
- Template selection should be determined before validation or transformation.
- If the user provides a non-CSV file, it should be aligned and converted into the NeonPanel CSV template format.
- If the user provides multiple files, only one source file should be processed unless the second file is explicitly a template/reference for mapping.
- Column mappings may be assumed only when ambiguity is low; ambiguous mappings require user confirmation.
- Issue reporting should focus only on missing required fields, invalid formats, duplicates, or mismatched templates.
- Amazon Removal Order Detail reports should be treated as a Shipment upload source when Order Type is Return, and mapped to the Shipment Upload Template.
- For Amazon Removal Order Detail files, confirmation is required for company, receiving warehouse, and country before finalizing the CSV.
- Invoices, Inventory Orders (POs), and Inventory Adjustments each have exactly ONE warehouse — the location where goods are placed or sold from. It is impossible to receive the same order into multiple warehouses; each warehouse requires a separate order.
- Shipments (all types — FBA, inventory shipments, etc.) have exactly TWO warehouses: an Origin (where goods are sent from) and a Destination (where goods are sent to).
- Bills do not have a warehouse column; they are linked to Inventory Orders or Shipments which carry the warehouse association.
- When uploading data, each row in Inventory Orders, Invoices, and Inventory Adjustments must specify one warehouse. If the user mentions multiple warehouses for the same order, they need separate orders — one per warehouse.
- Products are master records that define what you sell; Inventory tracks stock levels by warehouse.
- Product Families group related products together for organizational and reporting purposes.
- Parent Products are created by merging multiple SKUs into a single parent-child hierarchy.
- Services in NeonPanel include auto-generated Amazon services, custom services, and landed cost services.
- Amazon Global Logistics (AGL) services must be configured separately for freight cost tracking and require a custom FBA Freight service and default AGL Services vendor account.
- Service Account Connections map services to Chart of Accounts entries for accurate accounting.
- Brands are organizational units that can be assigned to products for categorization and reporting.
- Vendors are suppliers that can be linked to products for procurement and cost tracking.
- Service Groups organize related services for easier management and reporting.
- Both Products and Inventory are required in NeonPanel: Products define what you sell, Inventory tracks warehouse stock levels.
- Product/SKU merging does NOT require zero inventory on the SKUs being merged. The 'Inventory exists' error during a product merge does not mean inventory must be zeroed out.
- Warehouse merging requires that the warehouse being merged has no transactions. This is a different rule from product/SKU merging — do not conflate the two.
- The 'Inventory exists' error when merging products/SKUs indicates a conflict with existing inventory records on the target or source product, not that inventory quantities must be zero.
- Product merge rules and warehouse merge rules have different prerequisites. Warehouse merges require no transactions on the source warehouse; product merges have their own distinct requirements around inventory record conflicts.
- When uncertain about the exact meaning of a merge error message, the agent should state it does not have verified documentation rather than guess at the cause.
- Common merge-related error messages in NeonPanel include 'Inventory exists', which relates to inventory record conflicts during product/SKU merging.
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
- Starting balances must be entered and verified before go-live to ensure accurate inventory and financial reporting.
- Transaction mapping defines how NeonPanel documents (Orders, Bills, Shipments, Invoices) flow into accounting systems and how warehouse associations are maintained.
- FIFO transaction quality metrics track the health and accuracy of COGS calculations per warehouse.
- Landed cost analytics provide visibility into service costs, freight expenses, and cost distribution across inventory movements.
- Inventory audits reconcile NeonPanel inventory records against 3PL or internal warehouse records to ensure data accuracy.
- Amazon Profit Analytics (beta) provides KPI dashboards, profit and loss analysis, and COGS variation tracking for Amazon sales channels.
- When a user asks about contributors, breakdowns, or causes of a metric, the agent must always drill down to the most granular level available (SKU or transaction level) in the first response — never use catch-all categories like Other or Untracked that hide the actual data.
- Product listings must include required fields (title, description, price, images, SKU) before they can be published to any sales channel.
- SKU data should be validated for completeness, accuracy, and consistency before syncing to inventory or order management systems.
- Bulk catalog updates should be performed during off-peak hours and include validation checks to prevent data corruption or listing errors.
- Product variants (size, color, style) should be managed as child SKUs linked to a parent product to maintain accurate inventory and pricing.
- Catalog synchronization across multiple channels requires mapping of channel-specific attributes and compliance with each marketplace's listing requirements.
- Product images must meet channel-specific requirements (dimensions, format, file size) and be optimized for fast loading and mobile display.

## Acct Agl Setup

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

## Acct Inventory Audit

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

## Acct Auth

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

## Account and category mapping — Chart of Accounts setup, category mapping rules, account codes, fixing mapping errors

**What it is**

Account and category mapping — Chart of Accounts setup, category mapping rules, account codes, fixing mapping errors.

**Key objects**

- Chart of accounts
- Category mapping
- Account codes
- Mapping rules

**How to help**

- Map accounts
- Set up category mapping
- Change account codes
- Fix mapping errors

**Clarifying questions**

- Not understanding that mapping changes affect historical data
- chart of accounts vs categories

## Imp Warehouse Company Confirm

**Clarifying questions**

- Which warehouse or company?
- What is the exact name?
- Is this for import or configuration?

**How to help**

- validate against master list
- request confirmation
- proceed after confirmation

**Don't claim**

- automatic name matching

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

## Management of SKU-level product data including attributes, variants, inventory mapping, and data quality

**What it is**

Management of SKU-level product data including attributes, variants, inventory mapping, and data quality.

**Key objects**

- sku
- product_variant
- attributes
- inventory_mapping
- data_quality
- barcode

**How to help**

- add SKU
- update SKU data
- product variants
- SKU attributes
- barcode lookup

## Imp Removal Order

**Clarifying questions**

- Is the Order Type 'Return'?
- Which warehouse is receiving?
- Do you have the file?

**How to help**

- map to Shipment template
- confirm company and warehouse
- validate country
- provide mapping guidance

**Don't claim**

- automatic mapping

## Service catalog configuration — services, account mapping, and allocation base

### What it is

Service catalog configuration covers the services NeonPanel uses to categorize costs and revenue — auto-generated services, Amazon services, custom services, AGL services — and how each service connects to an account in the Chart of Accounts.

A service also carries an **allocation base**, which is how NeonPanel decides to spread that service's cost across inventory items when the service appears on a bill.

### How it works

When a bill is linked to a document, each service line on that bill spreads its cost across the items in the document. The split is governed by the service's **allocation base**, and because the base lives on the service, a single bill with several services can split each service's portion on different logic.

Supported allocation bases:

- **Volume** — proportional to item volume.
- **Weight** — proportional to item weight.
- **Count** — even split per unit.
- **Duty** — adds a percentage of the customs duty to the item cost.
- **Amount** — proportional to item cost (value-based).

Because the allocation base is a per-service setting, changing how a cost is split means editing the service, not the individual bill.

### How to help

- Add or configure a custom, Amazon, or AGL service.
- Map a service to the correct Chart of Accounts account.
- Set or explain the allocation base on a service and how it affects landed cost splits.

### Clarifying questions

- Which service type — Amazon, custom, or AGL?
- Which account in the Chart of Accounts should it map to?
- Which allocation base should this service use (volume, weight, count, duty, amount)?

### Don't claim

- Do NOT say the allocation base is set per bill — it is a property of the service.
- Do NOT claim services or their account mappings are generated automatically without confirming the service type.

## Bulk catalog operations, data synchronization, import/export workflows, and catalog maintenance tasks

**What it is**

Bulk catalog operations, data synchronization, import/export workflows, and catalog maintenance tasks.

**Key objects**

- bulk_update
- import_job
- export_job
- sync_status
- catalog_version
- change_log

**How to help**

- bulk update products
- import catalog
- export products
- sync catalog
- catalog changes

## Acct Velocity Analysis

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

## Data Import

**Procedure**

### Template Request
1. Symptom: User needs an import template.
2. Checks: Identify the import type (Inventory Orders, Bills, Shipments, Invoices, Forecasts).
3. Outcome: Provide the correct template version and confirm template structure.
4. Reference the instruction sheet and example data sheet included in each template.

### Validation Errors
1. Symptom: Import file rejected or validation errors reported.
2. Checks: Collect import job ID and exact error message.
3. Identify which rows are affected.
4. Request the validation report.
5. Suggest corrections based on error type.
6. Confirm fix before retry.

### Partial Import / Missing Rows
1. Symptom: Missing rows after import.
2. Checks: Collect file name and expected row count.
3. Request reconciliation report.
4. Identify missing rows and investigate cause.
5. Confirm expected count vs actual.

### Format Questions
1. Symptom: User unsure about format requirements.
2. Checks: Identify import type.
3. Reference template instructions for field mapping.
4. Confirm field requirements and provide examples.

### Reconciliation Mismatch
1. Symptom: Totals don't match after import.
2. Checks: Collect report period and expected total.
3. Request reconciliation report.
4. Compare source vs imported data.
5. Identify discrepancies and confirm root cause.

### Repeated Import Failures
1. Symptom: Import errors keep recurring.
2. Checks: Collect multiple job IDs and error patterns.
3. Outcome: Escalate if pattern indicates systemic issue.

### Multi-File Import
1. Symptom: User provides multiple files.
2. Checks: Confirm which file is the source data and which is template/reference.
3. Process only one source file unless second is explicitly a reference.

### Warehouse/Company Confirmation
1. Symptom: User needs to confirm warehouse or company for import.
2. Checks: Validate against master list.
3. Request confirmation before proceeding.

### Amazon Removal Order Import
1. Symptom: User has Amazon Removal Order Detail report.
2. Checks: Confirm Order Type is Return.
3. Map to Shipment Upload Template.
4. Confirm company, receiving warehouse, and country before finalizing CSV.

### Template Field Guidance
1. Symptom: User needs column definitions or field requirements.
2. Checks: Identify import type.
3. Use template_instructions.md to select the correct template file.
4. Explain field requirements and provide examples.

### Data Mismatch After Import
1. Symptom: Totals don't match post-import.
2. Checks: Request reconciliation report.
3. Compare source data with imported records.
4. Identify and resolve discrepancies.

## Acct Sync Status

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

## Acct Project Search

**Clarifying questions**

- What is the project name or reference?

**How to help**

- use search_neonpanel_project_url tool
- provide project link
- explain project scope

## Acct Chart Of Accounts

**Clarifying questions**

- Are you importing from QBO/Xero or using NeonPanel default?

**How to help**

- confirm CoA source
- explain service mapping requirements
- provide setup guidance

**Don't claim**

- CoA changes don't affect mappings

## Creation, editing, and management of product listings across multiple sales channels and marketplaces

**What it is**

Creation, editing, and management of product listings across multiple sales channels and marketplaces.

**Key objects**

- product_listing
- channel
- marketplace
- listing_status
- title
- description
- images

**How to help**

- create listing
- update product
- publish listing
- edit product details
- manage listings

## Service groups configuration — why service groups are essential, how to create and use them for organized reporting

**What it is**

Service groups configuration — why service groups are essential, how to create and use them for organized reporting.

**Key objects**

- Service Group
- Service
- Grouping Logic

**How to help**

- create service group
- add services to group
- understand service groups
- why use service groups

**Clarifying questions**

- service group vs service
- grouping logic

**Clarifying questions**

- What services do you want to group?
- What is the grouping logic?

**How to help**

- explain service group purpose
- guide on creation
- confirm grouping strategy

**Don't claim**

- service groups are optional

## Acct Integration Setup

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

## Cat Product Returns

**Clarifying questions**

- Which metric are you interested in?
- What time period?

**How to help**

- reference Product Returns dashboard
- explain trend graph
- guide on refund rate analysis
- identify top contributors

**Don't claim**

- automatic return prevention

## Import setup and file requirements — template selection, file format, required fields

**What it is**

Import setup and file requirements — template selection, file format, required fields.

**Key objects**

- Import Job
- Template
- File
- CSV Format

**How to help**

- start import
- download template
- which template to use
- file format requirements

**Clarifying questions**

- file format
- required fields
- template version confusion

## Acct Starting Balances

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

## Acct Unit Costs

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

## Imp Multi File

**Clarifying questions**

- Is the second file a template or reference?
- Which file is the source data?

**How to help**

- confirm single source file
- validate template reference
- proceed with one source

**Don't claim**

- multiple source files supported

## Inventory catalog management — the relationship between products and inventory, why both are needed, and how inventory records interact with product merge operations

**What it is**

Inventory catalog management — the relationship between products and inventory, why both are needed, and how inventory records interact with product merge operations.

**Key objects**

- Inventory
- Product-Inventory relationship
- Inventory record conflict
- Merge inventory impact
- Warehouse
- Stock

**How to help**

- product vs inventory
- why both product and inventory
- how does inventory affect merging
- manage inventory
- warehouse inventory

**Clarifying questions**

- Thinking inventory must be zero to merge products (incorrect)
- Confusing inventory-level merge constraints with warehouse-level merge constraints
- product vs inventory levels

**Clarifying questions**

- Are you confused about the distinction?
- Is this for setup or troubleshooting?

**How to help**

- explain product vs inventory relationship
- clarify warehouse inventory
- provide guidance

**Don't claim**

- products and inventory are the same

## Imp Partial

**Clarifying questions**

- Which rows are missing?
- Do you have a reconciliation report?
- What was the expected row count?

**How to help**

- request reconciliation report
- identify missing rows
- confirm expected count
- investigate cause

**Don't claim**

- partial import is normal
- data loss is expected

**Escalate when**

- data loss risk
- financial impact
- audit required

## Imp Validation Errors

**Clarifying questions**

- Do you have the import job ID?
- What is the exact error message?
- Which rows are affected?

**How to help**

- request validation report
- identify error rows
- suggest corrections
- confirm fix before retry

**Don't claim**

- automatic error fixing

**Escalate when**

- repeated failures
- data loss risk
- financial impact

## Landed cost — how bills are distributed to inventory items

### What it is

Landed cost is the true, all-in cost of an inventory item after additional costs — freight, customs, duties, handling — are added on top of the purchase price. In NeonPanel, these extra costs enter the system as **bills**, and NeonPanel spreads them onto the items so that reporting, COGS, and analytics reflect the real cost of goods.

### How it works

A bill in NeonPanel is linked to a document — a shipment, an inventory order, an assembly, and so on. Once that link exists, the money on the bill flows down onto the items in that document automatically.

The important thing to understand is that the distribution is **all-or-nothing across the document**: the bill's amount is spread across **every item in the linked document**, never onto a hand-picked subset. There is no way to point a bill (or part of a bill) at a single ASIN, a single SKU, or only some lines of a shipment. If you need cost to land on a narrower set of items, that set has to be its own document with its own bill.

How much of the cost each item receives is decided by the **allocation base**. A single bill can carry several service lines, and each service has its own allocation base, so different portions of the same bill can be split on different logic. The supported allocation bases are:

- **Volume** — split in proportion to each item's volume.
- **Weight** �� split in proportion to each item's weight.
- **Count** — split evenly by unit count.
- **Duty** — adds a percentage of the customs duty onto the item's cost.
- **Amount** — split in proportion to each item's cost (value-based).

The allocation base is a property of the **service**, not of the bill — so if a customer wants to change how a cost is split, they change it on the service, and it applies wherever that service is used.

### How to help

- Explain that a bill must be attached to a document (check Bill Info) before any cost lands on items.
- Explain that the whole document's items receive the cost, split by each service's allocation base.
- If a user expects cost on only some items, explain the all-or-nothing rule and suggest separating those items into their own document/bill.
- Walk through the landed cost breakdown report to show how a given bill was split.

### Clarifying questions

- Is a shipment or inventory order attached in Bill Info? (No attachment → no landed cost lands on items.)
- Which allocation base is set on the service(s) on this bill?
- Are you expecting the cost to hit only some items, or all items in the document?

### Don't claim

- Do NOT say a bill can be distributed to only some items, a selected ASIN, a single SKU, or part of a shipment/order/assembly. Distribution always covers all items in the linked document.
- Do NOT say the allocation base is chosen per bill — it is set per service.
- Do NOT claim landed cost appears on items without a document attached in Bill Info.

### Escalate when

- A user reports that cost distributed to the wrong set of items despite a correct document attachment and allocation base.
- The landed cost report shows a split that doesn't match the configured allocation base.

## Cat Services Setup

**Clarifying questions**

- Which service type? (Amazon, custom, AGL)
- Is this for landed cost?

**How to help**

- identify service type
- provide setup guidance
- confirm account mapping

**Don't claim**

- services auto-generate without configuration

## Acct Profit Analytics Beta

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

## Imp Template

**Clarifying questions**

- Which import type? (Inventory Orders, Bills, Shipments, Invoices, Forecasts)

**How to help**

- identify import type
- provide template version
- confirm template structure
- explain instruction sheet

**Don't claim**

- file size limits
- row limits

**Escalate when**

- template unavailable
- template corrupted

## Accounting system connection and synchronization — connecting QBO/Xero, sync status, sync errors, retry guidance

**What it is**

Accounting system connection and synchronization — connecting QBO/Xero, sync status, sync errors, retry guidance.

**Key objects**

- Sync status
- Connection settings
- Sync errors
- Accounting platform
- Sync Job
- Ledger Snapshot

**How to help**

- Check sync status
- Fix sync errors
- Connect accounting system
- Re-sync data

**Clarifying questions**

- Confusing sync errors with data import errors
- sync vs export
- sync timing

## Bills, invoices, and payables — creating, viewing, tracking payments, due dates, vendor management

**What it is**

Bills, invoices, and payables — creating, viewing, tracking payments, due dates, vendor management.

**Key objects**

- Bill
- Invoice
- Payment
- Vendor
- Due date

**How to help**

- View bills
- Create an invoice
- Track payments
- Check what's due
- bill missing
- invoice missing

**Clarifying questions**

- Confusing bills (payables) with invoices (receivables)

## Validation and error handling during data imports — error messages, row-level issues, retry guidance

**What it is**

Validation and error handling during data imports — error messages, row-level issues, retry guidance.

**Key objects**

- Validation Report
- Error
- Row
- Error Severity

**How to help**

- import failed
- validation errors
- row errors
- repeated failures

**Clarifying questions**

- error severity
- row mapping

## Acct Mapping Errors

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

## Acct Transaction Mapping

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

## Product catalog management — product introduction, product families, parent products, product merging, merge prerequisites, merge error messages (e.g

**What it is**

Product catalog management — product introduction, product families, parent products, product merging, merge prerequisites, merge error messages (e.g. 'Inventory exists'), and the distinction between product merge rules and warehouse merge rules.

**Key objects**

- Product
- SKU
- Parent Product
- Product Family
- Product Merge
- Merge Error
- Inventory exists error

**How to help**

- how do I add a product
- how do I merge SKUs
- what does Inventory exists error mean
- product merge prerequisites
- difference between warehouse merge and product merge

**Clarifying questions**

- Confusing warehouse merge rules (no transactions required) with product merge rules (different prerequisites)
- Assuming 'Inventory exists' error means inventory must be zeroed out before merging
- Conflating product/SKU merge operations with warehouse merge operations

## Acct User Memory

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

## Acct Metric Drilldown

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

## Imp Format Questions

**Clarifying questions**

- Which import type?
- Which field or column?
- Do you have the template?

**How to help**

- reference template instructions
- confirm field mapping
- validate format
- provide example

**Don't claim**

- non-CSV formats supported

**Escalate when**

- format mismatch blocks import

## Acct Fifo Analytics

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

## Catalog Management

**Procedure**

### Add a New Product
1. Navigate to the Products section in NeonPanel.
2. Click "Add Product" or "New Product".
3. Enter the product name, SKU, and basic details.
4. Assign the product to a Product Family if applicable (optional).
5. Configure pricing and availability settings.
6. Save the product.
7. Verify the product appears in the catalog.

### Create a Product Family
1. Navigate to the Products section.
2. Click "Product Families" or "Manage Families".
3. Click "New Family".
4. Enter the family name and description.
5. Select the parent product (if creating a parent-child hierarchy).
6. Add child SKUs to the family.
7. Save the family.
8. Verify the family structure in the product hierarchy.

### Merge Products into a Parent Product
1. Identify the SKUs to merge.
2. Navigate to the Products section.
3. Select the primary SKU (this becomes the parent).
4. Click "Create Parent Product" or "Merge Products".
5. Select the child SKUs to merge.
6. Confirm the merge operation.
7. Verify the parent-child hierarchy is created.
8. Update any affected inventory or orders.

### Troubleshoot Product/SKU Merge Errors
1. Identify the exact error message displayed (e.g., "Inventory exists").
2. Determine whether the user is merging **products/SKUs** or **warehouses** — the rules are different.
3. If merging **products/SKUs**:
   - The "Inventory exists" error indicates a conflict with existing inventory records, NOT that inventory quantities must be zero.
   - Do NOT advise the user to zero out inventory as a fix — this is incorrect.
   - If the exact cause of the error is not covered in verified facts, state that clearly and escalate rather than guessing.
4. If merging **warehouses**:
   - Warehouse merging requires that the source warehouse has **no transactions**.
   - This is a completely different rule from product/SKU merging — do not conflate them.
5. If the error message is not recognized or documented, tell the user: "I don't have verified documentation on this specific error. Let me escalate to get you an accurate answer."
6. Never fabricate an explanation for a merge error — always ground the answer in verified facts or escalate.

### Configure a Custom Service
1. Navigate to the Services catalog.
2. Click "Add Service" or "New Service".
3. Select the service type (Amazon, Custom, or AGL).
4. Enter the service name and description.
5. Configure service parameters (if applicable).
6. Map the service to a Chart of Accounts entry.
7. Save the service.
8. Verify the service appears in the Services list.

### Map a Service to an Account
1. Navigate to the Services catalog.
2. Find the service to map.
3. Click "Edit" or "Configure Mapping".
4. Select the target account from the Chart of Accounts.
5. Confirm the mapping.
6. Save the changes.
7. Verify the mapping in the Service-Account Mapping report.

### Add a Brand
1. Navigate to the Brands section.
2. Click "Add Brand" or "New Brand".
3. Enter the brand name and details.
4. Save the brand.
5. Verify the brand appears in the Brands list.

### Assign a Brand to a Product
1. Navigate to the Products section.
2. Find the product to assign a brand to.
3. Click "Edit" or "Configure".
4. Select the brand from the Brand dropdown.
5. Save the changes.
6. Verify the brand assignment in the product details.

### Add a Vendor
1. Navigate to the Vendors section.
2. Click "Add Vendor" or "New Vendor".
3. Enter the vendor name, contact details, and terms.
4. Save the vendor.
5. Verify the vendor appears in the Vendors list.

### Create a Service Group
1. Navigate to the Service Groups section.
2. Click "Create Group" or "New Group".
3. Enter the group name and description.
4. Select services to include in the group.
5. Confirm the grouping logic.
6. Save the group.
7. Verify the group appears in the Service Groups list.

### Analyze Product Returns
1. Navigate to the Product Returns dashboard.
2. Review the Trend Graph to see return patterns over time.
3. Check the Average Refund Rates by product or category.
4. Identify the Biggest Contributors to returns.
5. Use filters to drill down by warehouse, time period, or product family.
6. Export the data for further analysis if needed.
7. Document findings and recommendations.

## Acct Stock Risk

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

## Acct Replenishment Analysis

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

## Acct Revenue Reporting

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

## Cat Products Intro

**Clarifying questions**

- Is this a new product or updating existing?
- Do you need product families?

**How to help**

- explain product structure
- reference product introduction guide
- confirm product family needs

**Don't claim**

- products auto-sync to inventory

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

## Acct Bill Missing

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

## Acct Po Placement

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

## Acct Landed Cost Analytics

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

## Cat Product Families

**Clarifying questions**

- Are you creating a new family or merging existing products?
- How many SKUs are involved?

**How to help**

- explain product family purpose
- guide on parent product creation
- reference merge products guide

**Don't claim**

- product families are required

## Post-import reconciliation — verifying totals match, identifying missing rows, data mismatch resolution

**What it is**

Post-import reconciliation — verifying totals match, identifying missing rows, data mismatch resolution.

**Key objects**

- Reconciliation Report
- Totals
- Missing Rows

**How to help**

- data mismatch
- missing rows
- totals don't match after import

**Clarifying questions**

- partial import

**Clarifying questions**

- Which report and period?
- What is the expected total?
- Which warehouse or company?

**How to help**

- request reconciliation report
- compare source vs imported
- identify discrepancies
- confirm root cause

**Don't claim**

- reconciliation logic
- automatic reconciliation

**Escalate when**

- financial impact
- audit required

## Cat Service Account Mapping

**Clarifying questions**

- Which service?
- Which account in Chart of Accounts?

**How to help**

- confirm service and account
- provide mapping guidance
- validate Chart of Accounts

**Don't claim**

- automatic service mapping

## Product Listings

**Procedure**

### Update Product Listing
1. Identify product by SKU, product ID, or name.
2. Determine target sales channel(s) (Amazon, Shopify, eBay, etc.).
3. Check current listing status (draft, active, inactive).
4. Gather listing information — required fields: Title, description, price, SKU, images.
5. Validate data: check required field completeness, price format, SKU uniqueness, image requirements.
6. Update listing and apply changes to target channel(s).
7. Verify update: confirm changes appear correctly on channel.
8. Document changes in catalog change history.

### Bulk Catalog Changes
1. Identify products to update (by SKU, category, brand, or filter).
2. Define changes to apply (price adjustment, attribute update, status change).
3. Prepare update file: export current data, create update spreadsheet/CSV.
4. Validate bulk data: check format, SKU matching, accuracy.
5. Test on small subset (10-20 products) first.
6. Schedule bulk update during off-peak hours if possible.
7. Monitor import job progress and review validation results.
8. Sync to channels and verify changes appear.
9. Spot-check updated products and document operation.

### SKU Troubleshooting
1. Identify the issue type: SKU not found, inventory mismatch, variant error, data quality, or duplicate SKU.
2. Gather SKU information from catalog and inventory system.
3. Review product variant relationships and check for duplicates.
4. Diagnose root cause based on issue type.
5. Resolve: create missing SKU, fix mapping, repair variants, update data, or merge duplicates.
6. Verify resolution: check catalog, inventory mapping, search, and variant relationships.
7. Prevent recurrence: review validation rules and update workflows.

## Cat Merge Rules

**Clarifying questions**

- Are you merging products/SKUs or merging warehouses? The rules are different.
- What is the exact error message you are seeing?
- Which SKUs or products are you trying to merge?

**How to help**

- Clarify whether the user is merging products/SKUs or warehouses
- Explain the specific merge prerequisites for the relevant operation
- Explain the meaning of the error message using verified facts
- If the error is not documented, state that clearly and escalate

**Don't claim**

- Do not claim that product/SKU merging requires zero inventory on the SKUs being merged
- Do not conflate warehouse merge rules with product merge rules
- Do not guess at the meaning of merge error messages without verified documentation

**Escalate when**

- User encounters a merge error not covered in verified facts
- Merge operation causes data loss or unexpected results

## Brands and vendors catalog management — adding brands, assigning brands to products, managing vendor lists

**What it is**

Brands and vendors catalog management — adding brands, assigning brands to products, managing vendor lists.

**Key objects**

- Brand
- Vendor
- Brand Assignment
- Vendor List

**How to help**

- add brand
- assign brand to product
- add vendor
- manage vendor list

**Clarifying questions**

- brand vs product
- vendor vs supplier

**Clarifying questions**

- Are you adding a brand or vendor?
- Is this for product assignment?

**How to help**

- identify brand or vendor
- provide setup guidance
- confirm assignment

**Don't claim**

- brands auto-assign

## Accounting

**Procedure**

### Sync Failed
1. Symptom: Accounting sync fails.
2. Checks: Identify integration (QBO or Xero), collect error message.
3. Confirm last successful sync timestamp.
4. Check integration status and connection settings.
5. Identify failure pattern — retry or escalate.

### Bill Missing
1. Symptom: Bill missing from ledger.
2. Checks: Collect bill ID and vendor name.
3. Confirm status in source accounting system.
4. Check Bill Info attachments and shipment/order links.
5. Identify creation trigger.

### Mapping Mismatch
1. Symptom: Account or category mapping is incorrect.
2. Checks: Identify which account or service is affected.
3. Request mapping export.
4. Identify the mismatch and confirm correct mapping.
5. Validate against Chart of Accounts.

### Totals Mismatch / Reconciliation
1. Symptom: Financial totals don't reconcile.
2. Checks: Collect report period and expected total.
3. Request report export.
4. Compare source vs system totals.
5. Identify discrepancies and confirm root cause.
6. Escalate if audit required.

### Auth Expired
1. Symptom: Authorization error with accounting integration.
2. Checks: Identify integration and collect error message.
3. Collect timestamp of failure.
4. Request re-authentication.
5. Escalate if repeated failures.

### Duplicate Entries
1. Symptom: Duplicate records in accounting.
2. Checks: Collect sync job ID.
3. Collect evidence of duplicates.
4. Investigate sync timing and deduplication.

### Vendor Mapping Issues
1. Symptom: Vendor name mismatch between systems.
2. Checks: Collect vendor name from both systems.
3. Request mapping export.
4. Resolve naming discrepancy.

### Chart of Accounts Mismatch
1. Symptom: Chart of accounts differs between NeonPanel and accounting system.
2. Checks: Request chart export from both systems.
3. Compare and identify differences.
4. Reconcile mappings.

### Reconciliation Audit
1. Symptom: Formal audit request.
2. Checks: Collect period and report type.
3. Gather all exports.
4. Escalate to accounting specialist.

### Repeated Auth Failures
1. Symptom: Authentication fails repeatedly.
2. Checks: Collect timestamps and error patterns.
3. Escalate immediately.

### Landed Cost Services Missing
1. Symptom: Only Amazon/non-landed services appear in Bill Item List.
2. Checks: Verify whether a shipment or inventory order was attached in Bill Info.
3. Outcome: Attach shipment/order in Bill Info, then re-open Item List.

### Metric Contributor / Breakdown Drilldown
When a user asks about contributors, breakdowns, or what's driving a metric:
1. **Identify the metric and date range.** Confirm which metric (COGS, revenue, landed cost, etc.) and time period.
2. **Query at the most granular level immediately.** Always drill down to SKU-level or transaction-level data in the first response. Do NOT summarize into high-level buckets.
3. **Present all items ranked by impact.** Show results sorted by contribution (highest to lowest). If many items, show top 10–15 with individual values and percentages.
4. **Never create catch-all categories.** Do not lump remaining items into "Other", "Untracked", or similar buckets. If the list is long, offer to show the next batch — but always with individual line items.
5. **If granular data is unavailable, say so explicitly.** State "I don't have SKU-level data for this metric" rather than speculating.
6. **Offer deeper analysis.** After presenting the breakdown, proactively offer to drill further.

### AGL Setup
1. Confirm if this is first-time AGL setup.
2. Check if AGL vendor is configured.
3. Provide AGL setup steps.
4. Configure custom FBA freight services.
5. Validate default AGL account.
6. Guide through example entries.

### Starting Balances
1. Identify warehouse or company.
2. Collect balance data.
3. Guide on balance entry process.
4. Confirm data verification.
5. Validate totals.
6. Provide review checklist.

### Integration Setup
1. Identify accounting system (QBO or Xero).
2. Determine if first-time setup.
3. Provide integration steps.
4. Confirm credentials.
5. Validate connection.
6. Test sync.

### Chart of Accounts Setup
1. Determine if importing from QBO/Xero or using NeonPanel default.
2. Confirm CoA source.
3. Explain service mapping requirements.
4. Provide setup guidance.
