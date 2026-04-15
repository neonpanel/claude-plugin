# Tool Routing Cues

# MCP Tool Routing Cues

Disambiguation, workflow order, and side-effect flags. Each line ends with `→ search tools_playbooks` — retrieve that file from the Knowledge Store for full untruncated rules and failure modes.

### builtins
- `PDF Generator` — needs: title, body content (Markdown string or list of {heading, body} sections); not for: summaries or answers the user will read inline; after: fully compose the content first, then call once; ask: confirm title and sections before generating; → search tools_playbooks.

### neonpanel
- `inventory_get_details` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: inventoryId; → search tools_playbooks.
- `shopify_list_inventory_balances` — not for: not for irreversible changes; needs: query; → search tools_playbooks.
- `shopify_list_orders` — not for: not for irreversible changes; needs: query; → search tools_playbooks.
- `sys__server__sys`
- `cogs_analyze_fifo_cogs` — needs: `query.filters.company_id[]`.; use: COGS quality checks and reconciliation.; watch: missing company_id.; → search tools_playbooks.
- `cogs_list_lost_batches`
- `cogs_export_unit_costs` — needs: `query.filters.company_id[]`.; use: Sellerboard uploads.; watch: missing dates or company_id.; → search tools_playbooks.
- `search_neonpanel_project_url` — needs: search by name/ref/key.; use: user asks for a project link.; watch: no matching project.; → search tools_playbooks.
- `reports_list_reports`
- `account_get_companies_with_permission`
- `account_list_companies`
- `inventory_list_items`
- `cogs_get_inventory_cogs`
- `inventory_list_warehouses`
- `inventory_get_warehouse_balances`
- `cogs_get_inventory_landed_cost`
- `inventory_valuation_analyze_inventory_value` — needs: `query.filters.company_id[]`.; use: valuation snapshots and reporting.; watch: missing company_id.; → search tools_playbooks.
- `reports_get_report_details`

### tools
- `context`
- `file_search`
