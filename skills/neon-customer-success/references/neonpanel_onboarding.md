# Neonpanel Onboarding — Knowledge

## Client's responsibility to finalize Chart of Accounts, define accounting policy, connect accounting systems, and approve mappings and financial outputs

**What it is**

Client's responsibility to finalize Chart of Accounts, define accounting policy, connect accounting systems, and approve mappings and financial outputs.

**Key objects**

- CoA finalization
- Accounting policy definition
- System connection
- Mapping approval
- Financial output approval

**How to help**

- What accounting prep do I need to do?
- Who approves the mappings?
- When do I connect my accounting system?
- What if I don't have credentials?

**Clarifying questions**

- Thinking NeonPanel handles all accounting decisions
- Not having accounting system credentials ready

## Client's ongoing responsibility to maintain accurate operational and financial data after go-live, including POs, freight invoices, inventory records, and structural changes

**What it is**

Client's ongoing responsibility to maintain accurate operational and financial data after go-live, including POs, freight invoices, inventory records, and structural changes.

**Key objects**

- Ongoing PO entry
- Freight invoice upload
- Inventory record maintenance
- Structural changes
- New warehouse addition
- New channel addition

**How to help**

- What do I maintain after go-live?
- When do I upload new invoices?
- How do I add a new warehouse?
- What about new sales channels?

**Clarifying questions**

- Thinking data maintenance is NeonPanel's responsibility after go-live
- Not uploading freight invoices promptly

## Client's responsibility to enter or confirm per-unit costs for Amazon inventory in the Initial Amazon Inventory Project prepared by NeonPanel

**What it is**

Client's responsibility to enter or confirm per-unit costs for Amazon inventory in the Initial Amazon Inventory Project prepared by NeonPanel.

**Key objects**

- Initial Amazon Inventory Project
- Per-unit cost
- Cost confirmation
- Pre-filled inventory data

**How to help**

- Who enters Amazon costs?
- What is the Initial Amazon Inventory Project?
- Do I need to confirm costs?
- What if I don't know the costs?

**Clarifying questions**

- Thinking NeonPanel knows the purchase costs for Amazon inventory
- Not understanding that cost accuracy affects all downstream FIFO calculations

## Post-Go-Live Support

**Procedure**

1. Inform the client that NeonPanel provides post-go-live monitoring and support.
2. NeonPanel conducts health checks to ensure data is flowing correctly.
3. Offer optimization guidance for:
   - Forecasting setup
   - Profitability analysis configuration
   - New channel or warehouse additions
4. Remind the client that new channels and warehouses require additional setup — they are not automatically configured.
5. Explain ongoing client responsibilities (see Data Maintenance playbook).

## Verified Facts

**Facts**

- NeonPanel processes, validates, and prepares Amazon-sourced data after connection — including warehouses, inventory, orders, fees, and shipments.
- NeonPanel handles duplicate SKU cleanup and catalog validation for Amazon data during onboarding.
- NeonPanel pre-fills inventory data from Amazon; the client must enter or confirm per-unit costs in the Initial Amazon Inventory Project.
- The client is responsible for providing all non-Amazon data: warehouse balances, PO details, shipment information, and landed cost inputs.
- NeonPanel technically validates data (correct format, no duplicates, proper references); the client validates business accuracy (correct quantities, costs, vendors).
- The client must finalize the Chart of Accounts, define accounting policy, connect their accounting system, and approve service-account mappings.
- Using a custom Chart of Accounts requires reconfiguring service-account mappings in the Services catalog.
- NeonPanel can propose a default Chart of Accounts and default service-account mappings, but the client must approve them.
- Dry-run testing requires active client review and approval — it is not automatic.
- Discrepancies found during dry-run must be resolved before go-live.
- Starting balances must be entered and verified before go-live to ensure accurate inventory and financial reporting.
- NeonPanel provides post-go-live support including health checks, optimization guidance, and help with forecasting, profitability analysis, and new channel/warehouse setup.
- After go-live, the client is responsible for ongoing data maintenance: PO entry, freight invoice uploads, inventory records, and structural changes.
- Cost accuracy in the Initial Amazon Inventory Project affects all downstream FIFO calculations.
- New channels and warehouses require additional setup after go-live — they are not automatically configured.

## Chart of Accounts and Accounting Setup

**Procedure**

1. Ask the client whether they want to use NeonPanel's default Chart of Accounts or their own custom CoA.
2. If using a custom CoA: explain that this requires reconfiguring service-account mappings in the Services catalog.
3. NeonPanel can propose default service-account mappings, but the client must review and approve them.
4. Guide the client through connecting their accounting system (QuickBooks Online or Xero).
5. Ensure the client has accounting system credentials ready before attempting connection.
6. Have the client define their accounting policy preferences (e.g., how to handle specific transaction types).
7. Review and finalize all mappings before proceeding to dry-run.

## Client and NeonPanel responsibilities for Chart of Accounts finalization, service-account mapping, and accounting policy decisions during onboarding

**What it is**

Client and NeonPanel responsibilities for Chart of Accounts finalization, service-account mapping, and accounting policy decisions during onboarding.

**Key objects**

- Chart of Accounts (CoA)
- Service-account mapping
- Accounting policy
- QBO/Xero connection
- Default CoA
- Custom CoA

**How to help**

- Who decides the Chart of Accounts?
- Can NeonPanel propose a CoA?
- What accounts do I need?
- How does CoA affect mappings?

**Clarifying questions**

- Not understanding that custom CoA requires reconfiguring service-account mappings
- Thinking NeonPanel decides accounting policy — client must define it

## Client and NeonPanel responsibilities for validating non-Amazon warehouse balances, in-production POs, on-the-water shipments, and inbound Amazon receiving data

**What it is**

Client and NeonPanel responsibilities for validating non-Amazon warehouse balances, in-production POs, on-the-water shipments, and inbound Amazon receiving data.

**Key objects**

- Warehouse balance validation
- In-production PO
- On-the-water shipment
- Inbound receiving
- Technical validation
- Data accuracy check

**How to help**

- Who validates warehouse balances?
- What about in-production POs?
- Who checks on-the-water shipments?
- What does technical validation mean?

**Clarifying questions**

- Thinking NeonPanel validates data accuracy — NeonPanel validates technical correctness, client validates business accuracy
- Not providing non-Amazon data on time

## Dry-Run Testing

**Procedure**

1. Explain that dry-run is a test run of the full data flow — it is NOT automatic and requires active review.
2. NeonPanel runs the dry-run and generates a validation report.
3. Review the validation report with the client, checking for:
   - Data accuracy (do the numbers look right?)
   - Missing records (are all expected transactions present?)
   - Mapping correctness (are transactions hitting the right accounts?)
4. If discrepancies are found, they MUST be resolved before go-live.
5. Document all issues found and assign owners (NeonPanel vs. client) for resolution.
6. Re-run the dry-run after fixes are applied to confirm resolution.

## Amazon Inventory Cost Entry

**Procedure**

1. Direct the client to the Initial Amazon Inventory Project in NeonPanel.
2. Explain that NeonPanel has pre-filled inventory quantities from Amazon, but per-unit costs must be entered by the client.
3. Emphasize that cost accuracy is critical — it affects all downstream FIFO calculations, COGS, and profitability reporting.
4. If the client doesn't know exact costs, suggest using supplier invoices or purchase records as reference.
5. Have the client review and confirm all costs before proceeding.

## Go-Live Approval

**Procedure**

1. Confirm all dry-run discrepancies have been resolved.
2. Confirm starting balances have been entered and verified.
3. Present the go-live checklist to the client:
   - Amazon data imported and validated ✓
   - Non-Amazon data provided and validated ✓
   - Costs entered in Initial Amazon Inventory Project ✓
   - Chart of Accounts finalized ✓
   - Service-account mappings approved ✓
   - Dry-run passed with no unresolved issues ✓
   - Starting balances entered and verified ✓
4. Get explicit client approval to proceed with go-live.
5. Set the go-live date and communicate the monitoring period.
6. Transition to live sync.

## Service-Account Mapping

**Procedure**

1. Navigate to the Services catalog in NeonPanel.
2. Review the auto-generated service list (Amazon services, Shopify services, landed cost services).
3. For each service, verify the mapped Chart of Accounts entry is correct.
4. If using a custom CoA, reconfigure each service-account mapping to match the client's accounts.
5. Have the client approve all mappings before proceeding.
6. Document any services that don't have a clear account mapping — these need client decision.

## Ongoing Data Maintenance (Client Responsibility)

### Procedure

1. Explain that after go-live, the client is responsible for maintaining accurate data:
   - Enter new POs as they are placed
   - Upload freight invoices promptly for accurate landed cost tracking
   - Maintain inventory records (adjustments, transfers)
   - Report structural changes (new warehouses, new sales channels)
2. Emphasize that delayed freight invoice uploads will result in incomplete landed cost data.
3. For new warehouse additions: guide the client through the setup process or escalate to NeonPanel support.
4. For new sales channel additions: explain that additional configuration is required and coordinate with NeonPanel.

## Client Data Collection (Non-Amazon)

### Procedure

1. Explain that the client is responsible for providing all non-Amazon data.
2. Request the following from the client:
   - 3PL warehouse balance reports (current stock levels at each non-Amazon warehouse)
   - Open PO spreadsheets (in-production purchase orders)
   - In-transit shipment details (on-the-water shipments)
   - Freight and customs invoices (for landed cost inputs)
3. Set a clear deadline for data submission — delays here delay the entire onboarding.
4. Remind the client that NeonPanel does not have access to non-Amazon data sources.
5. If the client is unsure what data to provide, walk through each category with examples.

## Responsibility Clarification (Who Does What)

### Procedure

1. When a client asks "who does what?", use this framework:
   - **NeonPanel handles**: Amazon data import/processing, technical validation, duplicate cleanup, catalog validation, dry-run execution, post-go-live health checks, optimization guidance.
   - **Client handles**: Non-Amazon data provision, cost entry, business accuracy validation, CoA finalization, accounting policy decisions, mapping approval, go-live approval, ongoing data maintenance.
   - **Shared**: Dry-run review (NeonPanel runs it, client validates results), discrepancy resolution (depends on the issue source).
2. If a specific task is ambiguous, check the module definitions for the relevant onboarding phase.
3. When in doubt, clarify with the client rather than assuming responsibility.

### Note

This framework divides work between NeonPanel and the client. It is not the agent
roster — for which NeonaSphera agent owns a given request, follow the Agent Roster
and Handoff Rules.

## Client's responsibility to provide all non-Amazon data: warehouse balances, PO details, shipment information, and landed cost inputs required for accurate onboarding

**What it is**

Client's responsibility to provide all non-Amazon data: warehouse balances, PO details, shipment information, and landed cost inputs required for accurate onboarding.

**Key objects**

- Non-Amazon data
- Warehouse balances
- PO details
- Shipment information
- Landed cost inputs
- Data deadline

**How to help**

- What data do I need to provide?
- What about 3PL balances?
- Do I need PO details?
- What about freight and customs?
- When do I provide this?

**Clarifying questions**

- Thinking NeonPanel has access to non-Amazon data
- Underestimating the amount of data needed
- Missing data deadlines

## Amazon Data Connection and Processing

**Procedure**

1. Confirm the client has Amazon Seller Central access and knows which marketplaces to connect.
2. Guide the client through connecting their Amazon account in NeonPanel.
3. Explain that NeonPanel will automatically import warehouses, inventory, orders, fees, and shipments from Amazon.
4. Inform the client that NeonPanel processes and validates this data — it is not immediately ready after connection.
5. NeonPanel handles duplicate SKU cleanup and catalog validation during this phase.
6. NeonPanel pre-fills inventory data from Amazon into the Initial Amazon Inventory Project.
7. Inform the client they will need to enter or confirm per-unit costs in the next phase.

## NeonPanel's responsibility for processing, validating, and preparing Amazon-sourced data after connection, including warehouses, inventory, orders, fees, and shipments

**What it is**

NeonPanel's responsibility for processing, validating, and preparing Amazon-sourced data after connection, including warehouses, inventory, orders, fees, and shipments.

**Key objects**

- Amazon connection
- Warehouse import
- Inventory sync
- Order import
- Fee processing
- Shipment import
- Duplicate SKU cleanup
- Catalog validation

**How to help**

- What does NeonPanel do with Amazon data?
- Who validates Amazon imports?
- Who cleans up Amazon catalogs?
- What about duplicate SKUs?
- Who pre-fills inventory?

**Clarifying questions**

- Thinking Amazon data is immediately ready after connection
- Not understanding that NeonPanel processes and validates before presenting data

## NeonPanel's ongoing support after go-live, including health checks, optimization, and guidance on forecasting, profitability analysis, and new channel/warehouse setup

**What it is**

NeonPanel's ongoing support after go-live, including health checks, optimization, and guidance on forecasting, profitability analysis, and new channel/warehouse setup.

**Key objects**

- Health check
- Post-go-live monitoring
- Optimization guidance
- Forecasting setup
- Profitability analysis
- New channel setup
- New warehouse setup

**How to help**

- What happens after go-live?
- How long does NeonPanel monitor?
- What is a health check?
- Can NeonPanel help with forecasting?
- How do I add new channels?

**Clarifying questions**

- Thinking support ends at go-live
- Not understanding that new channels/warehouses require additional setup

## Operational Data Validation

**Procedure**

1. Explain the split responsibility: NeonPanel validates technical correctness (format, duplicates, references); the client validates business accuracy (correct quantities, costs, vendors).
2. For warehouse balances: NeonPanel checks data format; client confirms actual stock levels match.
3. For in-production POs: NeonPanel validates structure; client confirms PO details are current and accurate.
4. For on-the-water shipments: NeonPanel validates shipment records; client confirms shipment status and expected arrival.
5. Flag any discrepancies found during technical validation and ask the client to resolve business-side issues.

## NeonPanel and client responsibilities during dry-run testing, validation, approval, and the transition to live sync

**What it is**

NeonPanel and client responsibilities during dry-run testing, validation, approval, and the transition to live sync.

**Key objects**

- Dry-run test
- Validation report
- Approval gate
- Go-live date
- Monitoring period
- Discrepancy resolution

**How to help**

- What happens in dry-run?
- Who runs the dry-run?
- What if there are discrepancies?
- When do we go live?
- What do I need to approve?

**Clarifying questions**

- Thinking dry-run is automatic — it requires active review and approval
- Not understanding that discrepancies found in dry-run must be resolved before go-live
