# Tool Routing Cues

# MCP Tool Routing Cues

Disambiguation, workflow order, and side-effect flags. Each line ends with `→ search tools_playbooks` — retrieve that file from the Knowledge Store for full untruncated rules and failure modes.

### neonpanel
- `forecasting_compare_sales_forecast_scenarios` — needs: query.filters; → search tools_playbooks.
- `forecasting_get_company_settings` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; → search tools_playbooks.
- `forecasting_list_latest_sales_forecast` — not for: not for irreversible changes; needs: query; → search tools_playbooks.
- `inventory_get_details` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: inventoryId; → search tools_playbooks.
- `sys__server__sys`
- `account_get_companies_with_permission`
- `account_list_companies`
- `supply_chain_analyze_sales_velocity` — needs: `query.filters` (company_id), optional `tool_specific` weights.; use: “why is velocity changing?”.; watch: missing company_id.; → search tools_playbooks.
- `supply_chain_inspect_inventory_sku_snapshot` — needs: `company_id` or `sku` (recommended); optional `debug`.; use: forensic analysis of a specific SKU.; watch: missing identifiers; limited results if `limit` too low.; → search tools_playbooks.
- `supply_chain_list_po_placement_candidates` — needs: `query.filters` (company_id), optional overrides.; use: “what should I reorder?”.; watch: missing company_id.; → search tools_playbooks.
- `supply_chain_list_fba_replenishment_candidates` — needs: `query.filters` (company_id), optional `tool_specific` overrides.; use: “which ASINs need replenishment?”.; watch: missing company_id or conflicting overrides.; → search tools_playbooks.
- `supply_chain_list_product_logistics_parameters` — needs: `inventory_ids[]` or `sku` + `company_id`.; use: lead time or MOQ discussions.; watch: missing inventory identifiers.; → search tools_playbooks.
- `supply_chain_list_stock_replenishment_risk_items` — needs: `query.filters` (company_id).; use: “what’s at risk this week?”.; watch: empty if no risk items.; → search tools_playbooks.
- `supply_chain_shipment_arrival_oracle` — needs: `query.filters.company_id` (required).; use: “what’s delayed?” and logistics risk checks.; watch: missing company_id; no in-transit results.; → search tools_playbooks.
- `search_neonpanel_project_url` — needs: search by name/ref/key.; use: user asks for a project link.; watch: no matching project.; → search tools_playbooks.
- `reports_list_reports`
- `reports_get_report_details`
- `inventory_list_warehouses`
- `inventory_get_warehouse_balances`
- `inventory_list_items`

### tools
- `context`
- `file_search`
