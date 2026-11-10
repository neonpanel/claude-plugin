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

## account_list_permissions

Retrieve all possible permission names (NeonPanel: GET /api/v1/permissions).

Returns { permissions: string[] } — the full list of permission strings defined in the system.

Use this tool to discover valid permission names before calling account_get_companies_with_permission.

Side effect level: `read`

**Use when:**
- retrieve all possible permission names (NeonPanel: GET /api/v1/permissions)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

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

## account_lookup_asin_catalog

ASIN catalog lookup & parent-child mapping. Resolves parent ↔ child ASIN relationships, SKU ↔ ASIN mapping, and returns product attributes from the latest inventory planning snapshot.

Use cases:
- Look up a child ASIN → find its parent ASIN and all siblings
- Look up a parent ASIN → list all child variations (colors, sizes)
- Map SKU → ASIN or ASIN → SKU
- Identify the 'Hero' variant (highest revenue_share among siblings)
- Get product attributes: brand, product_family, revenue class, Pareto ABC class

This is a cross-domain utility tool. Use it before diving into domain-specific tools (brand analytics, supply chain, COGS, advertising) to understand the product catalog structure.

Typical workflow:
1. Call this tool with parent_asin to list all child variations and their revenue shares.
2. Identify the Hero variant (is_hero=true) vs. underperformers.
3. Pass specific child ASINs to domain tools (conversion leak analysis, search query performance, repeat purchases, etc.).

Data source: inventory_planning.last_snapshot_inventory_planning (latest snapshot per company+marketplace+SKU).

Lookup modes (provide at least one filter):
- asin: look up by child ASIN(s) → returns their parent, siblings, and attributes
- parent_asin: look up by parent ASIN(s) → returns all child variations
- sku: look up by SKU(s) → returns ASIN mapping and attributes
- brand / product_family: browse catalog by brand or product family

When presenting results:
- Render item_icon_url as an image when possible.
- Highlight the Hero variant (highest revenue_share) in each parent group.
- Show variation attributes (color, size) to help users identify specific children.

Side effect level: `read`

**Use when:**
- aSIN catalog lookup & parent-child mapping

**Required inputs:**
- - **company_id** (`integer`): Optional: restrict to a specific company_id (must be permitted). Recommended if you have access to multiple companies.
- **marketplace** (`string`): Optional. Marketplace filter (maps to country_code). Recommended to avoid cross-marketplace duplicates.
- **asin** (`array`): Optional. Child ASIN(s) to look up. Returns their parent ASIN, sibling variants, and attributes.
- **parent_asin** (`array`): Optional. Parent ASIN(s) to look up. Returns all child variations under each parent.
- **sku** (`array`): Optional. SKU(s) to look up. Returns ASIN mapping and product attributes.
- **brand** (`string`): Optional. Filter by brand name (case-insensitive contains match).
- **product_family** (`string`): Optional. Filter by product family (case-insensitive contains match).
- **include_siblings** (`boolean`): When true (default), expands results to include all sibling child ASINs under the same parent(s). Set to false to return only the exact matches.
- **limit** (`integer`): Max rows to return. Default: 50.
- **debug** (`boolean`): If true, include diagnostics (authorized company_ids, rendered SQL).

## forecasting_get_company_settings

Retrieve forecasting settings for a company (NeonPanel: GET /api/v1/companies/{uuid}/settings/forecasts).

Returns: status (active/trial/inactive), default_seasonality (12 monthly multipliers separated by semicolons, e.g., "1;1;1;1;1;1;1;1;1;0.8;1.5;2.5"), default_scenario_id, and default planning parameters: lead_time_days, safety_stock_days, fba_lead_time, fba_safety_stock, planned_po_frequency, fba_replenishment_frequency, and ABC classification thresholds (safety_stock_multiplicator / revenue_class / pareto_class for A/B/C/D).

Related: Use forecasting_update_company_settings to modify these values. Use forecasting_generate_sales_forecast and forecasting_write_sales_forecast for actual forecast generation.

Side effect level: `read`

**Use when:**
- retrieve forecasting settings for a company (NeonPanel: GET /api/v1/companies/{uuid}/settings/forecasts)

**Do not use when:**
- not for create/update/delete workflows

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.

**Related tools:**
- list/search tool for entity lookup

## forecasting_update_company_settings

Update forecasting settings for a company (NeonPanel: PUT /api/v1/companies/{uuid}/settings/forecasts).

Pass only the fields you want to change — all are optional. Returns { success: true } on success.

Key fields: status (active/trial/inactive), default_seasonality (12 semicolon-separated monthly multipliers), default_lead_time_days, default_safety_stock_days, default_fba_lead_time, default_fba_safety_stock, default_planned_po_frequency, default_fba_replenishment_frequency, and ABC classification parameters.

Use forecasting_get_company_settings first to see current values before updating.

Side effect level: `read`

**Use when:**
- update forecasting settings for a company (NeonPanel: PUT /api/v1/companies/{uuid}/settings/forecasts)

**Do not use when:**
- not for read-only information requests

**Required inputs:**
- - **status** (`string`):
- **default_seasonality** (`string`):
- **default_scenario_id** (`integer`):
- **default_lead_time_days** (`integer`):
- **default_safety_stock_days** (`integer`):
- **default_fba_lead_time** (`integer`):
- **default_fba_safety_stock** (`integer`):
- **default_planned_po_frequency** (`integer`):
- **default_fba_replenishment_frequency** (`integer`):
- **default_safety_stock_multiplicator_class_a** (`number`):
- **default_safety_stock_multiplicator_class_b** (`number`):
- **default_safety_stock_multiplicator_class_c** (`number`):
- **default_safety_stock_multiplicator_class_d** (`number`):
- **default_revenue_class_a** (`number`):
- **default_revenue_class_b** (`number`):
- **default_revenue_class_c** (`number`):
- **default_pareto_class_a** (`number`):
- **default_pareto_class_b** (`number`):
- **default_pareto_class_c** (`number`):
- **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.

**Related tools:**
- get tool to confirm target state

## inventory_get_details

Retrieve full inventory item details with restock data and warehouse balances (NeonPanel: GET /api/v1/companies/{uuid}/inventory-items/{id}/balances).

Returns:
- inventory: item details (id, name, fnsku, asin, sku, image, country_code, dimensions)
- restock_data: daily_sales_target, unit_vendor_price, unit_landed_cost, estimated_daily_sales, lead_time_days, safety_stock_days, fba_lead_time_days, fba_safety_stock_days, target_fba_fee, target_price, target_referral_percentage
- balances_data: balances_date + balances_list (warehouse + balance per warehouse)

Use balancesDate (YYYY-MM-DD) for historical balance snapshots. Defaults to today.

Related: Use cogs_get_inventory_cogs for COGS detail, cogs_get_inventory_landed_cost for manufacturing costs, supply_chain_inspect_inventory_sku_snapshot for Athena-based snapshot data.

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- inventoryId

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **inventoryId** (`integer` (required)):
- **balancesDate** (`string`):

**Related tools:**
- list/search tool for entity lookup

## inventory_get_listing_details_by_asin

Find listing by ASIN and return the latest listing details, optionally syncing from Amazon (NeonPanel: GET+POST /api/v1/companies/{uuid}/listings).

Returns: listingId, listing object (id, asin, parent_asin, title, brand, color, image, bullet_points grouped by language), listings array (all matches), synced (boolean).

Bullet points are keyed by BCP 47 language tag (e.g., en_US, de_DE) with ordered arrays of strings.

When sync=true (default), triggers a live sync from Amazon to get the freshest data. Set sync=false to skip the sync and return cached data only.

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- asin

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **asin** (`string` (required)):
- **sync** (`boolean`):

**Related tools:**
- list/search tool for entity lookup

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

## project_management_create_adjustment

Side effect level: `unknown`

## project_management_create_bill

Side effect level: `unknown`

## project_management_create_inventory_order

Side effect level: `unknown`

## project_management_create_invoice

Side effect level: `unknown`

## project_management_create_project

Create a NeonPanel project record. Initial support is project_type="inventory_order" / Purchase Orders (NeonPanel: POST /api/v1/companies/{uuid}/inventory-orders). Supply project fields such as ref_number, dates, market, currency, vendor_id, warehouse_id, payment_term_id, and details[]. Payment request mutation is handled by direct payment request tools as those endpoints are delivered.

Side effect level: `write`

**Use when:**
- create a NeonPanel project record

**Required context:**
- project

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **project_type** (`string`):
- **project** (`object` (required)):

## project_management_create_shipment

Side effect level: `unknown`

## project_management_get_adjustment

Side effect level: `unknown`

## project_management_get_bill

Side effect level: `unknown`

## project_management_get_inventory_order

Side effect level: `unknown`

## project_management_get_invoice

Side effect level: `unknown`

## project_management_get_shipment

Side effect level: `unknown`

## project_management_list_adjustments

Side effect level: `unknown`

## project_management_list_assembly_orders

Side effect level: `unknown`

## project_management_list_bills

Side effect level: `unknown`

## project_management_list_inventory_orders

Side effect level: `unknown`

## project_management_list_invoices

Side effect level: `unknown`

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

## project_management_list_services

Side effect level: `unknown`

## project_management_list_shipments

Side effect level: `unknown`

## project_management_list_vendors

Side effect level: `unknown`

## project_management_record_payment

Side effect level: `unknown`

## project_management_update_adjustment

Side effect level: `unknown`

## project_management_update_bill

Side effect level: `unknown`

## project_management_update_inventory_order

Side effect level: `unknown`

## project_management_update_invoice

Side effect level: `unknown`

## project_management_update_payment_request

Side effect level: `unknown`

## project_management_update_project

Side effect level: `unknown`

## project_management_update_shipment

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
