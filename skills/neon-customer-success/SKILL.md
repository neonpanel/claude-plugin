---
name: neon-customer-success
description: >
  This skill should be used when the user asks about "NeonPanel onboarding",
  "data import", "upload template", "QuickBooks/Xero setup", "Chart of Accounts mapping",
  "dry run", "go-live readiness", "service-account mapping", "FIFO COGS setup",
  "landed cost configuration", "Amazon connection", "import errors", "data validation",
  "PO invoice mapping", or needs help with NeonPanel customer success workflows.
  Covers the full 5-phase onboarding lifecycle, data import playbooks, accounting
  system integration, and post-go-live stabilization.
metadata:
  version: "1.0.0"
  source: "NeonPanel Agent Training (DynamoDB production)"
---

# Neon — Customer Success & Onboarding

## NeonPanel MCP Tools

### Primary tools (use these first)
- `import_get_instructions`
- `import_create_documents`
- `import_create_documents_by_pdf`
- `import_check_status`
- `financials_analyze_service_coa_mapping`
- `financials_list_chart_of_accounts`
- `inventory_list_items`
- `inventory_list_warehouses`
- `search_neonpanel_project_url`
- `account_list_companies`
- `account_get_report_details`
- `cogs_get_inventory_landed_cost`
- `cogs_get_inventory_cogs`
- `inventory_get_details`

### Supporting tools (use when needed)
- `financials_analyze_general_ledger`
- `financials_analyze_profit_and_loss`
- `forecasting_get_company_settings`

## Domain Coverage

This agent is trained on the following knowledge domains. Detailed playbooks, module maps, and routing indexes for each domain are in the `references/` folder.

- **Accounting** — see `references/accounting-playbook.md`
- **Catalog Management** — see `references/catalog_management-playbook.md`
- **Creating Projects In Np** — see `references/creating_projects_in_np-playbook.md`
- **Creating Updating Projects In Neonpanel** — see `references/creating_updating_projects_in_neonpanel-playbook.md`
- **Data Import** — see `references/data_import-playbook.md`
- **Logistics** — see `references/logistics-playbook.md`
- **Manage Neonpanel Documents** — see `references/manage_neonpanel_documents-playbook.md`
- **Manage Neonpanel Projects** — see `references/manage_neonpanel_projects-playbook.md`
- **Neonpanel Onboarding** — see `references/neonpanel_onboarding-playbook.md`
- **Neonpanel Support** — see `references/neonpanel_support-playbook.md`
- **Po Invoice Mapping** — see `references/po_invoice_mapping-playbook.md`
- **Product Catalog** — see `references/product_catalog-playbook.md`
- **Support** — see `references/support-playbook.md`
