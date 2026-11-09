# NeonPanel MCP Tool Guidance

## account_get_companies_with_permission

Check which companies the user has a specific permission for (NeonPanel: GET /api/v1/permissions/{permission}/companies).

Use this tool before calling Athena-based tools that require specific permissions. For example, before calling financials_analyze_general_ledger, check which companies the user can access with permission "view:quicksight_group.bookkeeping".

Returns an array of Company objects (id, uuid, name, short_name, currency, timezone) where the user holds the given permission.

Optionally filter to specific companies by passing companyUuids.

Side effect level: `read`

**Use when:**
- check which companies the user has a specific permission for (NeonPanel: GET /api/v1/permissions/{permission}/companies)

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- permission

**Required inputs:**
- - **permission** (`string` (required)):
- **companyUuids** (`array`): Optional: limit results to a set of company UUIDs.

**Related tools:**
- list/search tool for entity lookup

## account_get_report_details

Retrieve full report details including sheets and URL query parameters (NeonPanel: GET /api/v1/reports/{reportId}).

Returns: id, title, group, description, link (base URL), and sheets[] — each sheet has slug, title, description, and parameters[].

URL building pattern:
  {link}?sheet={sheets[].slug}&{parameter.slug}={value}&...

URL rules:
- sheet is optional — if omitted, the first sheet opens by default
- company parameter: comma-separated short_name values from account_list_companies (e.g., ?company=5SU,KEM)
- Parameter order does not matter
- Parameter types determine formatting:
  • DateTimePicker → YYYY-MM-DD (e.g., 2026-02-01)
  • DateTimeRangePicker → usually two params like start-date / end-date
  • Dropdown with multiselect=true → comma-separated (e.g., brand=Nike,Adidas)

Example URL:
https://my.neonpanel.com/app/reports/basic_suite_new/v3-inventory-transactions-bas-user?company=5SU,KEM&sheet=transaction-list&start-date=2026-02-01&end-date=2026-02-13

Workflow: Call account_list_reports first to get reportId values, then call this tool for the full sheet/parameter details.

Side effect level: `read`

**Use when:**
- retrieve full report details including sheets and URL query parameters (NeonPanel: GET /api/v1/reports/{reportId})

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- reportId

**Required inputs:**
- - **reportId** (`integer` (required)): Report ID obtained from account_list_reports.

**Related tools:**
- list/search tool for entity lookup

## account_list_companies

Retrieve companies the authenticated user can access (NeonPanel: GET /api/v1/companies).

Returns paginated list of companies with: id (numeric), uuid (string for REST API calls), name, short_name (used as company identifier in report URLs), currency (base currency), timezone.

Use this tool to:
- Resolve company_id → uuid for other NeonPanel REST API tools
- Get short_name values for building report URLs (e.g., ?company=5SU,KEM)
- List all companies the user has access to

Pagination: page (default 1), per_page (10–60, default 30).

Note: Most Athena-based tools accept company_id (numeric). NeonPanel REST tools accept either company_id or companyUuid — the server resolves automatically.

Side effect level: `read`

**Use when:**
- retrieve companies the authenticated user can access (NeonPanel: GET /api/v1/companies)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **page** (`integer`):
- **perPage** (`integer`):

## account_list_reports

Retrieve the list of available reports with groups, descriptions, and direct URLs (NeonPanel: GET /api/v1/reports).

Each report has: id (use with account_get_report_details for sheets and parameters), title, group (report category), description, link (base URL to open the report).

Workflow:
1. Call this tool to browse available reports
2. Call account_get_report_details with reportId to get sheets and URL parameters
3. Build the report URL: {link}?company={short_name}&sheet={slug}&{param}={value}

The company parameter uses short_name values from account_list_companies (e.g., ?company=5SU,KEM).

No input parameters required — returns all reports accessible to the authenticated user.

Side effect level: `read`

**Use when:**
- retrieve the list of available reports with groups, descriptions, and direct URLs (NeonPanel: GET /api/v1/reports)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

## inventory_list_items

List inventory items for a company with optional filters (NeonPanel: GET /api/v1/companies/{uuid}/inventory-items).

Returns paginated list of inventory items with: id, name, fnsku, asin, sku, image, country_code, weight/length/height/depth (imperial units: pounds/inches).

Search: The search field matches by SKU, ASIN, FnSKU, ID, or Name. Alternatively use the specific fnsku, asin, or sku filters for exact matching. Filter by country_code (2-letter ISO, e.g., "US", "DE") to narrow by marketplace.

Pagination: page (default 1), per_page (10–60, default 30).

Related tools: Use inventory_get_details for restock data and warehouse balances, cogs_get_inventory_cogs for COGS breakdown, cogs_get_inventory_landed_cost for manufacturing costs.

Side effect level: `read`

**Use when:**
- list inventory items for a company with optional filters (NeonPanel: GET /api/v1/companies/{uuid}/inventory-items)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **page** (`integer`):
- **perPage** (`integer`):
- **countryCode** (`string`):
- **search** (`string`):
- **fnsku** (`string`):
- **asin** (`string`):
- **sku** (`string`):

## inventory_list_warehouses

Retrieve warehouses for a company (NeonPanel: GET /api/v1/companies/{uuid}/warehouses).

Returns paginated list of warehouses with: uuid, name, type (e.g., "Asset"), country_code (e.g., "US").

Use the search parameter to filter by warehouse name. Pagination: page (default 1), per_page (10–60, default 30).

Related: Use warehouse uuid with inventory_get_warehouse_balances to see inventory balances per warehouse, or with cogs_get_inventory_landed_cost as the warehouse_uuid filter.

Side effect level: `read`

**Use when:**
- retrieve warehouses for a company (NeonPanel: GET /api/v1/companies/{uuid}/warehouses)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **page** (`integer`):
- **perPage** (`integer`):
- **search** (`string`):

## project_management_list_payment_requests

List payment request installments for a company across document types such as Inventory Orders and Bills (NeonPanel: GET /api/v1/companies/{uuid}/payment-requests). Supports due-date range filters start_date and end_date. Returns payment request fields with parent document context.

Side effect level: `read`

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **start_date** (`string`):
- **end_date** (`any`):

## project_management_list_payment_terms

List global NeonPanel payment terms available for project/document creation (NeonPanel: GET /api/v1/payment-terms). Terms are not company-scoped and include the document type they apply to, such as InventoryOrder or Bill.

Side effect level: `read`

**Use when:**
- list global NeonPanel payment terms available for project/document creation (NeonPanel: GET /api/v1/payment-terms)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

## project_management_list_shipments

Side effect level: `unknown`

## project_management_list_vendors

Side effect level: `unknown`

## projects_list_recent_activity

Lists recent project-related activity in a readable support-friendly format. Use this when the user asks what is new in the last N days.

Current event coverage:
- task_status_changed
- field_value_changed

Typical use:
- "What is new in the last 3 days for company 103?"
- "Show me recent field changes for the last 7 days"

Results are ordered newest first and include a prebuilt `summary` field for readable output.

Side effect level: `read`

**Use when:**
- lists recent project-related activity in a readable support-friendly format

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)): Company ID filter. Required.
- **days_back** (`integer`): How many days back to look.
- **event_types** (`array`): Optional event type filter. Omit to include all supported activity types.
- **limit** (`integer`): Maximum rows to return.

## search_neonpanel_project_url

Search for NeonPanel projects (Purchase Orders, FBA Shipments, Assembly Orders, etc.) and get their URLs.

SEARCH OPTIONS:
1. GENERAL SEARCH (search): Searches across project name, reference number, AND project key - USE THIS FOR EASIEST SEARCH
   - Example: "12345" will find any project with "12345" in name, ref, or key
   - Example: "FBA" will find all FBA shipments
   - Searches are case-insensitive partial matches

2. SPECIFIC FIELD SEARCH (project_name/project_ref_number/project_key): Search only one specific field
   - project_name: Batch/project name (e.g., "12/11/2025 DP_KP_HmP_SC KAI Sea-GYR3")
   - project_ref_number: Reference/order number (e.g., "4024523804758")
   - project_key: System key format ProjectType-ID (e.g., "FbaInboundShipment-40358")

RECOMMENDATION: Use 'search' parameter for flexibility unless you need exact field matching.

Side effect level: `read`

**Use when:**
- search for NeonPanel projects (Purchase Orders, FBA Shipments, Assembly Orders, etc.) and get their URLs

**Do not use when:**
- not for irreversible changes

**Required context:**
- company_id

**Required inputs:**
- - **search** (`string`): RECOMMENDED: Search across all fields (project name, reference number, project key). Partial match, case-insensitive. Example: '12345' finds projects with '12345' anywhere.
- **project_name** (`string`): Search ONLY project name field. Partial match. Example: 'DP_KP' finds batches with that in the name.
- **project_ref_number** (`string`): Search ONLY reference number field. Exact match. Example: '4024523804758' finds that specific order.
- **project_key** (`string`): Search ONLY project key field. Exact match. Format: ProjectType-ID. Example: 'FbaInboundShipment-40358'.
- **company_id** (`array` (required)): Filter by company ID(s). Required.

## shopify_list_inventory_balances

List Shopify inventory balance records with warehouse, seller, and company details. Read-only; no side effects.

Use cases:
- View current Shopify stock levels per SKU/warehouse/seller
- Filter by warehouse, seller, balance type, SKU, barcode
- Aggregate totals by warehouse, seller, SKU, balance type, or snapshot date
- Track inventory snapshots over a date range

Input uses the Option-1 query envelope:
- query: shared filters/limit/sort/projection/grouping

Important (company selection):
- Use query.filters.company_id (preferred, integer) or query.filters.company (numeric string only)
- At least one must be provided for authorization

Data sources:
- neonpanel_iceberg.shopify_balances (quantity, sku, barcode, date, type)
- neonpanel_iceberg.shopify_sellers (seller name, domain, status, connection state)
- neonpanel_iceberg.inventory_warehouses (warehouse name)
- neonpanel_iceberg.app_companies (company name)

Filter logic:
- Array filters use OR logic within the same filter type
- Multiple filter types use AND
- All string filters are case-insensitive

Date filters:
- query.filters.snapshot_date: exact date match (YYYY-MM-DD). Highest priority.
- query.filters.date_from / date_to: date range (only applied when snapshot_date is not set)

Aggregation:
- query.aggregation.group_by: returns one row per unique dimension combination
- Supported dimensions: company, seller, warehouse, balance_type, sku, barcode, snapshot_date
- company_id is always included in GROUP BY for authorization
- Aggregated rows include: record_count, distinct_inventory_ids, distinct_skus, total_stock_quantity, avg_stock_quantity, min_stock_quantity, max_stock_quantity, earliest/latest_snapshot_date
- If group_by is omitted or empty, returns detail rows

Side effect level: `read`

**Use when:**
- list Shopify inventory balance records with warehouse, seller, and company details

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **filters** (`object`): Standard filters. All string filters are case-insensitive.
- **aggregation** (`object`): Optional aggregation. If group_by is provided, returns aggregated rows.
- **sort** (`object`): Sort specification (not yet implemented server-side).
- **select_fields** (`array`): Optional output field selection (projection).
- **limit** (`integer`):
- **cursor** (`string`): Opaque cursor for pagination (not supported yet).

## supply_chain_analyze_sales_velocity

Analyze sales velocity for selected items. Computes realized velocity signals, derives plan-horizon velocity for PO decisions, detects inconsistencies, and returns explainable recommendations.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

Required: sku (array, 1-10 values).
Company selection: prefer company_id; company (numeric string) and company_ids (single value) are accepted aliases.

Side effect level: `read`

**Use when:**
- analyze sales velocity for selected items

**Required context:**
- sku

**Required inputs:**
- - **company** (`string`):
- **company_id** (`integer`):
- **company_ids** (`array`):
- **brand** (`array`):
- **marketplace** (`array`):
- **marketplaces** (`array`):
- **product_family** (`array`):
- **parent_asin** (`array`):
- **asin** (`array`):
- **sku** (`array` (required)): Required. 1-10 SKUs to analyze.
- **revenue_abcd_class** (`array`):
- **output_mode** (`string`):
- **traffic_weight_3d** (`number`):
- **traffic_weight_7d** (`number`):
- **traffic_weight_30d** (`number`):
- **coverage_days_override** (`integer`):
- **months_ahead** (`integer`):
- **limit** (`integer`):

## supply_chain_inspect_inventory_sku_snapshot

Inventory SKU deep dive (raw snapshot inspection). Returns raw inventory_planning_snapshot row(s) from the latest snapshot partition for the authorized company_id(s).

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

How to use:
- Best: provide sku + marketplace (US/UK) to inspect a specific item.
- If you omit sku and/or marketplace, the tool will return a small sample of rows (useful to confirm snapshot data exists and to see exact country labels / SKU formatting).

Available Data (includes all snapshot columns):
- Standard inventory metrics: total_units, available, inbound, total_balance_quantity, etc.
- Work-in-progress (WIP) orders: wip_total_ordered_quantity (quantity from POs with status 'Created' or pending), wip_orders_json (detailed WIP order information)
- Confirmed orders: total_ordered_quantity, orders_json
- Sales metrics: sales_last_30_days, units_sold_last_30_days, avg_units_30d/7d/3d
- Planning parameters: lead_time_days, safety_stock_days, seasonality_pattern, sales_forecast_scenario_id
- Warehouse balances: warehouse_balance_details_json (note: Amazon FBA warehouses excluded to prevent double-counting with 'available' field)

CRITICAL: Understanding inventory in-transit data:
- 'inbound' column: Amazon-reported data for items in transit TO FBA (from Amazon Restock Report)
- 'fc_transfer' column: Amazon-reported data for items transferring BETWEEN FBA fulfillment centers
- 'fba_quantity_in_transit' (from neonpanel_calculated): NeonPanel control metric to verify Amazon's 'inbound' data. Compare these two values to identify discrepancies or missing shipments in Amazon's reporting.
- 'Transfer Warehouse' data (from neonpanel_calculated warehouse transfers): ALL NeonPanel-calculated items in transit between warehouses. This is independent of Amazon's data and shows your complete warehouse transfer activity.

Naming note (important):
- The metric previously labeled "Average item price" should be treated as "Average purchase price". In this tool output, prefer snapshot.average_purchase_price.

Before calling this tool:
1) Ask the user if they want a specific SKU + marketplace or just a sample row for validation.
2) Optionally ask if they want to restrict to a specific company_id (recommended if they have access to multiple companies).

Knowledge base: https://help.neonpanel.com/en/articles/60
Inventory Planning report (UI): https://my.neonpanel.com/app/reports/forecasting/inventory-planning

When presenting results (important):
- Render item_ref.item_icon_url as an image when possible.
- Present a clean summary first (SKU, marketplace, inventory_id, company_id, snapshot date).
- Then show the raw snapshot fields grouped logically (inventory, sales, inbound, WIP orders, confirmed orders, planning, lead time/safety stock).

Side effect level: `read`

**Use when:**
- inventory SKU deep dive (raw snapshot inspection)

**Required inputs:**
- - **sku** (`string`): Optional. The SKU to inspect. If omitted, returns sample rows.
- **marketplace** (`string`): Optional. Marketplace/country code (maps to snapshot column pil.country_code). If omitted, returns sample rows and lets you see available country labels.
- **company_id** (`integer`): Optional: restrict to a specific company_id (must be permitted). Recommended if you have access to multiple companies.
- **limit** (`integer`): Optional. Max rows to return if multiple rows match the SKU+marketplace. Recommended default: 5.
- **debug** (`boolean`): Optional. If true, include diagnostics to explain why rows might be missing (authorized company_ids, chosen snapshot partition, available countries for the SKU).

## supply_chain_list_fba_replenishment_candidates

List items needing FBA replenishment based on projected stockout risk and inbound coverage.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

Required: company_id (integer). Calls without company_id are rejected by schema.

CRITICAL: FBA inventory columns:
- inbound: Amazon-reported units in transit TO FBA.
- fc_transfer: Amazon-reported units transferring BETWEEN FBA centers.
- available: Amazon-reported sellable FBA units.
- fba_quantity_in_transit: NeonPanel control metric to validate Amazon inbound reporting.

Defaults and behavior:
- sales_velocity default is 'current'.
- planning_base is auto-defaulted if omitted based on selector fields and sales_velocity.
- fba_on_hand includes inbound + available + fc_transfer + fc_processing for replenishment planning.

Side effect level: `read`

**Use when:**
- list items needing FBA replenishment based on projected stockout risk and inbound coverage

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_id

**Required inputs:**
- - **company_id** (`integer` (required)): REQUIRED. Numeric company identifier.
- **planning_base** (`string`): Which items to include in planning. Auto-defaulted if omitted.
- **target_skus** (`array`):
- **target_inventory_ids** (`array`):
- **target_asins** (`array`):
- **parent_asins** (`array`):
- **brand** (`array`):
- **product_family** (`array`):
- **category** (`array`):
- **marketplaces** (`array`): Marketplace filter. Values: US, UK, ALL.
- **countries** (`array`):
- **revenue_abcd_class** (`array`): Filter by revenue class.
- **time_window** (`object`):
- **sales_velocity** (`string`):
- **use_seasonality** (`boolean`):
- **override_default** (`boolean`):
- **fba_lead_time_days_override** (`integer`):
- **fba_safety_stock_days_override** (`integer`):
- **days_between_shipments** (`integer`):
- **limit** (`integer`):
- **stockout_threshold_days** (`integer`):
- **active_sold_min_units_per_day** (`number`):

## supply_chain_list_po_placement_candidates

List items needing PO placement based on lead time + safety stock + PO cadence coverage.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

Required: company_id (integer). Calls without company_id are rejected by schema.

VELOCITY MODES (sales_velocity, default: 'planned'):
  • 'planned': Uses forecast units for the arrival month — forward-looking, accounts for seasonal demand during lead time.
  • 'current': Uses trailing 30-day realized average — backward-looking.
  • 'target': Uses daily_unit_sales_target from sales plan — goal-based.

Why 'planned' may differ from 30-day average:
  The tool uses FORECAST velocity for the arrival month to prevent under-ordering before high-demand periods.
  Example: 60-day lead time → March arrival → March forecast 1,928 units → velocity = 64.3 u/day even if trailing 30d = 46.1 u/day.

Inventory calculation:
  available = warehouse balances + FBA available + confirmed orders + WIP orders (if include_work_in_progress=true).
  WIP orders are POs with status 'Created'/pending not yet received. Including WIP (default) prevents double-ordering.

planning_base (auto-defaulted if omitted):
  • Omit it — the tool picks automatically based on selector fields and sales_velocity.
  • If target_skus/target_asins/parent_asins are set → 'targeted_only'.
  • If sales_velocity='planned' → 'planned_only'; 'target' → 'targeted_only'; 'current' → 'actively_sold_only'.
  • Set explicitly only when you need to override (e.g., 'all' to include zero-velocity items).

company_id: pass directly (integer). Do NOT pass a freeform company name — use account_list_companies first to find the numeric id.

Side effect level: `read`

**Use when:**
- list items needing PO placement based on lead time + safety stock + PO cadence coverage

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_id

**Required inputs:**
- - **company_id** (`integer` (required)): REQUIRED. Numeric company identifier.
- **planning_base** (`string`): Which items to include in planning. Auto-defaulted if omitted — see tool description.
- **target_skus** (`array`): Filter to specific SKUs (sets planning_base='targeted_only' automatically).
- **target_inventory_ids** (`array`): Filter to specific inventory IDs.
- **target_asins** (`array`): Filter to specific child ASINs.
- **parent_asins** (`array`): Filter to specific parent ASINs.
- **brand** (`array`):
- **product_family** (`array`):
- **category** (`array`):
- **marketplaces** (`array`): Marketplace filter. Values: US, UK, ALL. Default: ALL.
- **countries** (`array`):
- **revenue_abcd_class** (`array`): Filter by revenue class (A=top 20%, B=next 30%, C=next 30%, D=tail 20%).
- **time_window** (`object`):
- **sales_velocity** (`string`): Velocity calculation mode. See tool description for details.
- **use_seasonality** (`boolean`):
- **override_default** (`boolean`):
- **lead_time_days_override** (`integer`): Lead time in days for PO arrival estimate.
- **safety_stock_days_override** (`integer`): Target safety stock buffer in days.
- **days_between_pos** (`integer`): Desired days between purchase orders.
- **include_work_in_progress** (`boolean`): Include WIP purchase orders (status 'Created'/pending) in available inventory. Default true prevents double-ordering. Set false for conservative planning.
- **limit** (`integer`): Max items to return.
- **stockout_threshold_days** (`integer`):
- **active_sold_min_units_per_day** (`number`):

## supply_chain_list_product_logistics_parameters

List product logistics parameters (vendor, product spec/name for POs, dimensions, box params, MOQ) from the latest inventory planning snapshot.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

Inputs (choose one):
- Provide inventory_ids (best when you already know the inventory IDs), OR
- Provide sku + company_id (best when you only know the SKU).

Notes:
- Data is read from the latest available inventory_planning_snapshot partition for the caller’s permitted companies.

Side effect level: `read`

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required inputs:**
- - **inventory_ids** (`array`): Provide a list of inventory_id values to look up logistics parameters for. Use this OR sku+company_id.
- **sku** (`string`): Provide a SKU to look up logistics parameters for. Requires company_id. Use this OR inventory_ids.
- **company_id** (`integer`): Required when using sku. Must be a company_id permitted for the caller.
- **limit** (`integer`): Safety limit for returned rows. If sku matches multiple marketplaces/rows, this caps the result size. Default: 200.

## supply_chain_list_stock_replenishment_risk_items

Identify inventory items at risk of stockout or insufficient days-of-supply.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

Required: company_id (integer). Calls without company_id are rejected by schema.

Use cases:
- Surface items approaching or at stockout risk
- Identify products with insufficient days-of-supply buffer
- Understand inbound supply timing and p80 arrival dates
- Prioritize replenishment orders or expedited actions

Risk dimensions (two independent analyses):
1. Stockout Risk — will physical inventory run out?
   high: days_of_supply(p50) < 0 | moderate: p50≥0 but p80<0 | low: p80≥0 but p95<0 | ok: p95≥0
2. Supply Buffer Risk — is buffer below min_days_of_supply target?
   high: p50 < threshold | moderate: p50≥threshold but p80<threshold | low: p80≥threshold but p95<threshold | ok: p95≥threshold

Velocity preset modes: balanced (0.5/0.3/0.2), conservative (0.7/0.2/0.1), aggressive (0.2/0.3/0.5)

Side effect level: `read`

**Use when:**
- identify inventory items at risk of stockout or insufficient days-of-supply

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_id

**Required inputs:**
- - **company_id** (`integer` (required)): REQUIRED. Company ID (integer). Calls without this field are rejected by schema.
- **brand** (`array`):
- **marketplace** (`array`):
- **marketplaces** (`array`): Alias of marketplace.
- **product_family** (`array`):
- **parent_asin** (`array`):
- **asin** (`array`):
- **sku** (`array`):
- **inventory_id** (`array`):
- **revenue_abcd_class** (`array`): Filter by revenue class (A=top 20%, B=next 30%, C=next 30%, D=tail 20%).
- **tags** (`array`):
- **sort** (`object`): Sort by any output field. Default: stockout_risk_tier asc.
- **select_fields** (`array`): Optional output field projection. Omit to return all default fields.
- **limit** (`integer`): Max items to return.
- **cursor** (`string`): Opaque pagination cursor.
- **min_days_of_supply** (`integer`): Target days-of-supply buffer. Items below this threshold are flagged as supply_buffer_risk.
- **velocity_weighting_mode** (`string`): Velocity preset. balanced=(0.5/0.3/0.2), conservative=(0.7/0.2/0.1), aggressive=(0.2/0.3/0.5). Default: balanced.
- **velocity_weighting** (`object`): Custom velocity weights (overridden by velocity_weighting_mode if both provided).
- **include_warehouse_stock** (`boolean`): Include non-FBA warehouse/3PL stock in available supply. Default: true.
- **include_inbound_details** (`boolean`): Include FBA inbound shipment details (count, units, arrival dates). Default: true.
- **p80_arrival_buffer_days** (`integer`): Safety buffer subtracted from p80 inbound arrival estimates (accounts for receiving delays).
- **stockout_risk_filter** (`array`): Return only items matching these stockout risk tiers (OR logic). Omit to include all tiers.
- **supply_buffer_risk_filter** (`array`): Return only items matching these supply buffer risk tiers (OR logic). Omit to include all tiers.

## supply_chain_shipment_arrival_oracle

Monitor in-transit shipments with predictive ETAs based on historical warehouse-to-warehouse performance.

IMPORTANT — flat input: pass all parameters at top level. Do NOT use nested query/filters/tool_specific wrappers.

This tool provides real-time visibility into shipment status with:
- Statistical ETA predictions: P50 (median), P80 (conservative), P95 (worst-case) arrival dates
- Delay detection: Compare carrier-provided ETAs against historical route performance
- Ghost shipment identification: Detect shipments physically arrived but not ledger-received
- Route performance insights: Identify consistently slow/fast shipping lanes
- Urgency scoring: Prioritize which shipments need immediate attention
- Route aggregation mode: Analyze performance by shipping lane

MODES:
1. SHIPMENT MODE (default): Returns individual shipment details
2. ROUTE AGGREGATION MODE: Set aggregate_by_route=true to get performance by origin→destination pairs

DATA SOURCE:
- Table: neonpanel_iceberg.inventory_shipments_snapshot
- Coverage: All shipment types (warehouse-to-warehouse, FBA inbound, AWD inbound)
- Updates: Daily refresh with latest tracking data

DEFAULT FILTERS (applied automatically):
- Excludes terminal statuses: CLOSED, CANCELLED, DELETED, ERROR, ABANDONED (use include_terminal_status=true to override)
- Excludes received shipments: Only shows in-transit (use include_received=true to override)
- When using search or ref_number: Terminal status filter is automatically disabled to find any shipment

FILTERING:
- company_id (REQUIRED): Company owning the shipments
- search: Search by shipment name or ref number (partial match, searches both fields)
- ref_number: Find specific shipment by exact reference number
- signal: Filter by actionable alert type (Ghost Shipment, Delayed, etc.)
- shipment_type: Filter by logistics model type (REGULAR, FBA INBOUND, AWD INBOUND)
- include_received: Show received shipments (default: false)
- include_terminal_status: Show CLOSED/CANCELLED/DELETED/ERROR shipments (default: false)
- destination_warehouse_name: Find shipments to specific warehouse (partial match)
- original_warehouse_name: Find shipments from specific origin (partial match)
- delay_threshold_days: Show only shipments delayed by X+ days beyond P80
- min_days_in_transit: Show only shipments in transit for X+ days
- shipped_after/shipped_before: Date range for ship date
- eta_before: Find shipments expected to arrive by specific date

ROUTE AGGREGATION MODE (aggregate_by_route=true):
Returns route-level performance metrics:
- avg_days_in_transit, median_days_in_transit
- reliability_pct: % of shipments arriving within P80
- delayed_count, ghost_shipment_count per route

Side effect level: `read`

**Use when:**
- monitor in-transit shipments with predictive ETAs based on historical warehouse-to-warehouse performance

**Required context:**
- company_id

**Required inputs:**
- - **company_id** (`integer` (required)): Company identifier (required)
- **signal** (`array`):
- **shipment_type** (`array`):
- **destination_warehouse_name** (`string`):
- **original_warehouse_name** (`string`):
- **delay_threshold_days** (`integer`):
- **min_days_in_transit** (`integer`):
- **origin_country_code** (`array`):
- **destination_country_code** (`array`):
- **include_received** (`boolean`):
- **include_terminal_status** (`boolean`):
- **shipped_after** (`string`):
- **shipped_before** (`string`):
- **eta_before** (`string`):
- **search** (`string`):
- **ref_number** (`string`):
- **sort_field** (`string`):
- **sort_direction** (`string`):
- **limit** (`integer`):
- **aggregate_by_route** (`boolean`):
