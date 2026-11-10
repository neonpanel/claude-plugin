---
name: inventory-supply-chain
description: >
  Inventory operations and supply chain: stock location, warehouse balances, inbound FBA, shipments, lost batches, inventory transactions, landed-cost or COGS diagnosis, sales velocity, and arrival prediction. Not for reorder quantities or purchase-order timing (use the replenishment-planning skill).
metadata:
  version: "1.0.0"
  expertise: "supply_chain"
  source: "NeonPanel expertise packs (DynamoDB production)"
---

# Inventory & supply chain

## Scope and boundaries

Inventory operations and supply chain: stock location, warehouse balances, inbound FBA, shipments, lost batches, inventory transactions, landed-cost or COGS diagnosis, sales velocity, and arrival prediction. Not for reorder quantities or purchase-order timing (use the replenishment-planning skill).

Neighboring expertise areas are separate skills in this plugin. When a question belongs to one of them, say which skill applies instead of approximating the answer:

- `onboarding-data-import` — Onboarding & data import
- `sales-forecasting` — Sales forecasting
- `replenishment-planning` — Replenishment planning
- `listing-research` — Listing & marketing research
- `market-intelligence` — Market intelligence
- `accounting-history` — Accounting history
- `financial-planning` — Financial planning
- `neonpanel-plugin-setup` — installing and connecting this plugin and its MCP connectors

## How to use this skill

Before answering, read `references/index.md` to find the matching knowledge area or KB article, then open the referenced file. Load only what the question needs.

KB articles are the official product documentation. If a KB article conflicts with a knowledge-area topic, follow the KB article and mention the discrepancy.

Apply the company context in `references/brand-neonpanel_brand_pack.md`.

## Playbook

### Goals

- Help with inventory, fulfillment, and planning questions.
- Identify data gaps in stock, orders, or shipments.

### Boundaries

- Avoid operational promises (SLAs) not in canonical claims.
- Do not assume carrier or warehouse behavior.

### Success criteria

- Clear data requests (SKU, location, order/shipment IDs).
- Escalate when fulfillment risk is high.

### Tone

- Practical and action-oriented.

## NeonPanel MCP Tools

**Read:** `account_get_companies_with_permission`, `account_get_report_details`, `account_list_companies`, `account_list_permissions`, `account_list_reports`, `account_lookup_asin_catalog`, `cogs_analyze_fifo_cogs`, `cogs_export_unit_costs`, `cogs_get_inventory_cogs`, `cogs_get_inventory_landed_cost`, `cogs_list_lost_batches`, `financials_analyze_amazon_statement`, `financials_get_revenue_and_cogs`, `financials_list_amazon_statements`, `inventory_get_details`, `inventory_get_listing_details_by_asin`, `inventory_get_warehouse_balances`, `inventory_list_items`, `inventory_list_warehouses`, `project_management_list_payment_requests`, `project_management_list_payment_terms`, `projects_list_recent_activity`, `search_neonpanel_project_url`, `shopify_list_inventory_balances`, `shopify_list_orders`, `supply_chain_list_fba_replenishment_candidates`

**Changes data:** `inventory_valuation_analyze_3pl_inventory`, `inventory_valuation_analyze_inventory_value`, `inventory_valuation_audit_inventory_balances`

**Unclassified:** `cogs_list_fifo_transactions`, `inventory_valuation_analyze_shipment_discrepancy`, `project_management_list_adjustments`, `project_management_list_assembly_orders`, `project_management_list_bills`, `project_management_list_inventory_orders`, `project_management_list_invoices`, `project_management_list_services`, `project_management_list_shipments`, `project_management_list_vendors`

Detailed usage guidance: `references/tools.md`

## Knowledge map

Use `references/index.md` for the complete topic and article index.

- **Inventory Ops**: 8 topics — `references/inventory_ops.md`
  - **Diagnose Lost Batches Cogs** (skill): 19 topics — `references/diagnose_lost_batches_cogs.md`
  - **Diagnosing Lost Batches Lost Cogs** (skill): 15 topics — `references/diagnosing_lost_batches_lost_cogs.md`
  - **Empty Results Diagnosis** (skill): 7 topics — `references/empty_results_diagnosis.md`
  - **Inventory Transactions** (skill): 10 topics — `references/inventory_transactions.md`
  - **Inventory Validation** (skill): 12 topics — `references/inventory_validation.md`
  - **Lost Batches Cogs Diagnosis** (skill): 17 topics — `references/lost_batches_cogs_diagnosis.md`
