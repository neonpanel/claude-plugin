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

## cogs_analyze_fifo_cogs

Analyze FIFO (First-In-First-Out) Cost of Goods Sold from invoice transactions.

This tool provides flexible COGS analysis with:
- Time aggregation: monthly, annual, or total period
- Multiple grouping dimensions: SKU, brand, product family, ASIN, marketplace, etc.
- Comprehensive filtering by product attributes, geography, and classifications
- Sign-corrected amounts (invoice quantities are negative, amounts are reversed for proper COGS calculation)
- Data quality metrics: track missing costs and estimate lost COGS
- **Transaction-level detail**: Use detail_level='transactions' to see individual records

DATA SOURCE:
- Table: neonpanel_iceberg.fifo_transactions_snapshot
- Filter: document_type = 'Invoice' (sales transactions only)
- COGS Calculation: ABS(transaction_amount) - absolute value to convert negative invoice amounts to positive COGS
- Transactions: Each row represents a unit sold from a specific inventory batch (FIFO cost assignment)

QUERY MODES:

1. AGGREGATED MODE (default):
   Use for summary reports with GROUP BY dimensions.
   Example: {filters: {company_id: [92], detail_level: 'aggregated'}, aggregation: {group_by: ['brand']}}

2. TRANSACTION MODE:
   Use for investigating specific transactions, batches, or data quality issues.
   Example: {filters: {company_id: [92], inventory_id: [38423], batch_id: [38967], start_date: '2025-11-01', end_date: '2025-11-30', detail_level: 'transactions'}, limit: 50}
   Returns: transaction_id, document_date, sku, batch_id, inventory_id, warehouse, costs, etc.

3. DATA QUALITY MODES:
   - analysis_mode: 'lost_batches' - Transactions without batch assignment
   - analysis_mode: 'lost_cogs' - Transactions with batch but zero cost
   Combine with detail_level: 'transactions' to see specific problematic records.

TIME FILTERING:
Date filters can be specified in two ways (filters take precedence):
- filters.start_date / filters.end_date - Simple, works with any mode
- aggregation.time.start_date / aggregation.time.end_date - For time-grouped aggregations

GROUPING DIMENSIONS (query.aggregation.group_by):
Supported dimensions (order matters for hierarchical grouping):
- 'sku': SKU identifier
- 'brand': Brand name
- 'product_family': Product family/category
- 'child_asin': Child ASIN (variation)
- 'parent_asin': Parent ASIN (product group)
- 'marketplace': Marketplace name (e.g., Amazon.com, Amazon.ca)
- 'country': Country name (e.g., United States, Canada)
- 'marketplace_currency': Currency code (e.g., USD, CAD)
- 'revenue_abcd_class': Revenue classification (A=top 70%, B=next 20%, C=next 8%, D=bottom 2%)
- 'pareto_abc_class': Pareto classification (A=top 80%, B=next 15%, C=bottom 5%)
- 'inventory_id': Specific inventory item ID
- 'batch_id': Batch document ID
- 'vendor': Supplier/vendor name

OUTPUT METRICS:
- cogs_amount: Total cost of goods sold (sum of item_landed_cost × quantity, sign-corrected)
- units_sold: Total units sold (absolute value of quantity)
- units_with_cost: Units that have cost data (item_landed_cost > 0)
- units_missing_cost: Units sold without cost data (item_landed_cost = 0 or NULL)
- cogs_quality_pct: Data quality percentage (units_with_cost ÷ units_sold × 100)
- cogs_quality_status: Visual indicator - 🔴 Red: <90% (critical), 🟡 Yellow: 90-98.99% (warning), 🟢 Green: ≥99% (excellent)
- estimated_lost_cogs: Estimated missing COGS (avg_unit_cogs × units_missing_cost)
- transactions_count: Number of invoice transactions
- avg_unit_cogs: Average COGS per unit (cogs_amount ÷ units_sold)
- purchase_price_amount: Total purchase price component (sum of item_purchase_price × quantity)

EXAMPLE USE CASES:

1. Get 20 transactions for a specific batch:
   - filters: {company_id: [92], inventory_id: [38423], batch_id: [38967], start_date: '2025-11-01', detail_level: 'transactions'}
   - limit: 20

2. Find transactions with missing costs:
   - filters: {company_id: [92], analysis_mode: 'lost_cogs', detail_level: 'transactions'}
   - limit: 50

3. Monthly COGS by brand:
   - filters: {company_id: [103]}
   - aggregation: {group_by: ['brand'], time: {periodicity: 'month', start_date: '2024-01-01', end_date: '2024-12-31'}}

4. Quality check - brands with missing cost data:
   - filters: {company_id: [103]}
   - aggregation: {group_by: ['brand']}
   - sort: {field: 'cogs_quality_pct', direction: 'asc'}

IMPORTANT NOTES:
- company_id filter is REQUIRED (multi-tenant data)
- detail_level='transactions' ignores aggregation.group_by and returns individual records
- All filters work with both aggregated and transaction modes
- Results sorted by cogs_amount DESC by default

Side effect level: `read`

**Use when:**
- analyze FIFO (First-In-First-Out) Cost of Goods Sold from invoice transactions

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): WHERE clause filters. company_id is required.
- **aggregation** (`object`): GROUP BY dimensions and time bucketing
- **sort** (`object`): Sort order for results
- **limit** (`integer`): Maximum number of rows to return

## cogs_export_unit_costs

Export monthly unit costs for uploading to Sellerboard or other profitability tools. Returns a flat table with purchase price, logistics cost, and landed cost per SKU per month.

Side effect level: `read`

**Use when:**
- export monthly unit costs for uploading to Sellerboard or other profitability tools

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **sort** (`object`):
- **limit** (`integer`): Maximum number of rows to return (default: 10000)

## cogs_get_inventory_cogs

Retrieve cost of goods sold (COGS) for an inventory item (NeonPanel: GET /api/v1/companies/{uuid}/inventory-items/{id}/cogs).

Returns: company, inventory, currency, amount (total COGS), lost_amount, total_amount, quantity, and paginated details[] of sales documents. Each sale has: document (type/link/status/date/ref_number), currency, amount, quantity, and details[] of cost batches written off (type: "Purchase Price" etc., batch document, amount, quantity).

Required: startDate (YYYY-MM-DD). endDate defaults to today.

Pagination: page (default 1), per_page (10–60, default 30).

COGS = cost of inventory consumed by sales. For manufacturing/purchase costs independent of sales, use cogs_get_inventory_landed_cost. For FIFO COGS analysis via Athena, use cogs_analyze_fifo_cogs.

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- inventoryId
- startDate

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **inventoryId** (`integer` (required)):
- **startDate** (`string` (required)):
- **endDate** (`string`):
- **page** (`integer`):
- **perPage** (`integer`):

**Clarifying questions:**
- Which value should I use for inventoryId?

**Related tools:**
- list/search tool for entity lookup

## cogs_get_inventory_landed_cost

Retrieve landed cost (manufacturing expenses) for an inventory item (NeonPanel: GET /api/v1/companies/{uuid}/inventory-items/{id}/landed-cost).

Returns: company, inventory, warehouse, currency, total amount, total quantity, and paginated data[] of cost batches. Each batch has: batch document, currency, amount, quantity, and details[] (type: "Purchase Price" etc., document, amount, quantity).

Required: warehouseUuid (from inventory_list_warehouses), startDate (YYYY-MM-DD). endDate defaults to today.

Pagination: page (default 1), per_page (10–60, default 30).

Related: For COGS (cost of goods sold = cost allocated to sales), use cogs_get_inventory_cogs instead. Landed cost covers manufacturing/purchase expenses regardless of sales.

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- inventoryId
- startDate
- warehouseUuid

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **inventoryId** (`integer` (required)):
- **warehouseUuid** (`string` (required)):
- **startDate** (`string` (required)):
- **endDate** (`string`):
- **page** (`integer`):
- **perPage** (`integer`):

**Clarifying questions:**
- Which value should I use for inventoryId?

**Related tools:**
- list/search tool for entity lookup

## cogs_list_fifo_transactions

Side effect level: `unknown`

## cogs_list_lost_batches

List transactions where batch tracking failed (batch is NULL). Shows transactions with lost cost tracking, sorted by amount. Useful for identifying data quality issues.

Side effect level: `read`

**Use when:**
- list transactions where batch tracking failed (batch is NULL)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **limit** (`integer`): Maximum number of transactions to return (default: 5)

## financials_analyze_amazon_statement

Analyzes Amazon settlement transaction details – filters and aggregates individual line items (revenue, fees, refunds, taxes) with currency conversion to the company's main currency.

Use this tool to:
- Break down a settlement by amount_type and amount_description (revenue vs. fees vs. taxes)
- Track monthly/quarterly fee trends across marketplaces
- Find all charges for a specific order or SKU
- Compare marketplace profitability in a single base currency (amount_main)
- Aggregate refunds by time period or marketplace
- Analyze FBA fulfillment fees vs. commission breakdown

ProTip: Use financials_list_amazon_statements first to find statement IDs, then drill into specific statements here.

Currency conversion: Each transaction's local-currency amount is converted to the company's main currency (amount_main) using the currency_rate table. Marketplace currency (from amazon_marketplaces) is used as fallback when the detail row's currency is NULL.

Common transaction_type values: 'Order', 'Refund', 'Service Fee', 'Adjustment', 'Transfer', 'FBA Inventory Fee', 'Debt', 'Lightning Deals Fee'.
Common amount_type values: 'ItemPrice', 'ItemFees', 'Promotion', 'ItemWithheldTax', 'Other'.
Common amount_description values: 'Principal', 'Tax', 'Commission', 'FBAPerUnitFulfillmentFee', 'ShippingChargeback', 'GiftWrap', 'PromotionShipping', etc.

Sign convention: positive = credit to seller (e.g., sale revenue), negative = charge (e.g., Amazon fee).

Default time range: Last 3 months when no dates are provided.

Marketplace: Derived from amazon_sellers + amazon_marketplaces via amazon_seller_id. Filter by marketplace_codes (country codes: 'US', 'DE', 'SE', 'GB', etc.).

Integration: Cross-reference with financials_analyze_general_ledger to reconcile settlement amounts vs. GL entries, or with cogs_analyze_fifo_cogs to compare settlement revenue against product costs.

Dig deeper: For a full interactive settlement view, recommend the user visit https://my.neonpanel.com

Side effect level: `read`

**Required context:**
- filters

**Required inputs:**
- - **filters** (`object` (required)): Compatibility alias for query.filters. Prefer query.filters.
- **time** (`object`): Compatibility alias for query.time. Prefer query.time.
- **start_date** (`string`): Compatibility alias for query.time.start_date. Prefer query.time.start_date.
- **end_date** (`string`): Compatibility alias for query.time.end_date. Prefer query.time.end_date.
- **aggregation** (`object`): Compatibility alias for query.aggregation. Prefer query.aggregation.
- **sort** (`object`): Compatibility alias for query.sort. Prefer query.sort.
- **limit** (`integer`): Compatibility alias for query.limit. Prefer query.limit.

## financials_get_revenue_and_cogs

Retrieve Revenue & COGS summary broken down by period and grouping (NeonPanel: GET /api/v1/revenue-and-cogs).

Returns filters (echo of applied filters) and data[] with: start_date, end_date, revenue_amount, cogs_amount, currency, and optional company/country_code/invoice depending on grouping.

Grouping options: company (revenue/COGS per company), country (per marketplace country), invoice (per invoice document). Multiple groupings can be combined.

Periodicity: total (default), yearly, quarterly, monthly.

startDate is required (YYYY-MM-DD). endDate defaults to today.

Filter by companyUuids and/or countryCodes. If no companyUuids are given, returns data for all accessible companies.

For deeper COGS analysis at the inventory level, use cogs_get_inventory_cogs or cogs_analyze_fifo_cogs.

Side effect level: `read`

**Use when:**
- retrieve Revenue & COGS summary broken down by period and grouping (NeonPanel: GET /api/v1/revenue-and-cogs)

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- startDate

**Required inputs:**
- - **startDate** (`string` (required)):
- **endDate** (`string`):
- **companyUuids** (`array`):
- **countryCodes** (`array`):
- **grouping** (`array`):
- **periodicity** (`string`):

**Related tools:**
- list/search tool for entity lookup

## financials_list_amazon_statements

Lists Amazon statements – one row per statement showing: statement ID, date range, deposit date, total payout amount, currency, seller ID, marketplace (name, country code, country), and company.

Use this tool to:
- Browse recent statements for a company
- Search for a specific statement by ID
- Find statements in a date range
- Filter by marketplace country code (e.g., 'US', 'DE', 'SE', 'GB')
- Filter by currency or seller ID
- Filter by total amount range

Each statement covers a ~2-week period. The deposit_date is when Amazon transfers funds. total_amount is the net payout in the statement currency.

Drill-down: After finding a statement, use financials_analyze_amazon_statement with the statement_id filter to see the full transaction breakdown (fees, revenue, refunds, etc.).

Default time range: When no dates are provided, returns statements from the last 3 months.

Knowledge base: https://help.neonpanel.com

Side effect level: `read`

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for the query. company_id is required; all others are optional.
- **time** (`object`): Date range filter on deposit_date.
- **sort_direction** (`string`): Sort by deposit_date. 'desc' = newest first (default), 'asc' = oldest first.
- **limit** (`integer`): Maximum rows to return (default 50, max 500).

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

## inventory_get_warehouse_balances

Retrieve inventory balances for a warehouse (NeonPanel: GET /api/v1/companies/{uuid}/warehouses/{uuid}/balances).

Returns paginated list of items with: inventory (InventoryItem object), balance (quantity in stock).

Use balancesDate (YYYY-MM-DD) to retrieve historical balances at a past date. Defaults to current date if omitted.

Pagination: page (default 1), per_page (10–60, default 30).

Related: Get the warehouseUuid from inventory_list_warehouses first.

Side effect level: `read`

**Use when:**
- retrieve inventory balances for a warehouse (NeonPanel: GET /api/v1/companies/{uuid}/warehouses/{uuid}/balances)

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- warehouseUuid

**Required inputs:**
- - **company_id** (`integer`): Numeric company ID (preferred; same as in Athena-based tools). Provide this OR companyUuid.
- **companyUuid** (`string`): Company UUID string. Use company_id instead when possible.
- **warehouseUuid** (`string` (required)):
- **page** (`integer`):
- **perPage** (`integer`):
- **balancesDate** (`string`):

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

## inventory_valuation_analyze_3pl_inventory

Analyze actual inventory balances from neonpanel_iceberg.inventory_balances. This table stores point-in-time inventory quantities imported from external operational sources such as Amazon, AWD, Shopify, 3PL feeds, and manually uploaded audit counts. Use this tool when the user asks what external/source systems reported for inventory quantity by date, source, status, SKU, inventory item, seller, or warehouse. For NeonPanel-calculated FIFO inventory value and quantity, use inventory_valuation_analyze_inventory_value. For direct calculated-vs-actual reconciliation, use inventory_valuation_audit_inventory_balances.

DATA RELATIONSHIP:
- inventory_balances = actual/source-side stock snapshots, one row per company/date/source/status/inventory/SKU/warehouse grain.
- fifo_transactions_snapshot = NeonPanel calculated inventory quantity and value derived from transaction history.
- The audit workflow compares inventory_balances.quantity to FIFO calculated quantity at the same date and warehouse.

DATE BEHAVIOR:
- filters.snapshot_date returns balances for exactly that date.
- filters.date_from/date_to returns a date range, useful with aggregation.time.periodicity day/month/year.
- If no date filter is provided, the tool uses the latest available inventory_balances date per company after other filters.

COMMON USE CASES:
- Total actual inventory by warehouse for the latest uploaded Amazon/AWD/Shopify/manual snapshot.
- Actual inventory trend by source and month.
- Source-system balance by SKU or inventory_id for a specific audit date.

Side effect level: `write`

**Use when:**
- analyze actual inventory balances from neonpanel_iceberg.inventory_balances

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **aggregation** (`object`):
- **sort** (`object`):
- **limit** (`integer`):

## inventory_valuation_analyze_inventory_value

Analyze current inventory value using FIFO batch balances. Calculates inventory quantity and value by ranking transactions to find the latest balance for each warehouse/batch. Supports flexible grouping by product, location, and time dimensions with date-based snapshots.

DATA SOURCE:
- Table: neonpanel_iceberg.fifo_transactions_snapshot
- Method: Window function (PARTITION BY inventory_id, warehouse, batch ORDER BY transaction_id DESC) to get last balance
- Metrics: batch_balance (quantity), item_landed_cost (unit cost), calculated balance_amount

GROUPING DIMENSIONS (group_by):
- 'sku': Stock Keeping Unit
- 'brand': Brand name
- 'product_family': Product family/category
- 'child_asin': Child ASIN (specific variant)
- 'parent_asin': Parent ASIN (product family)
- 'marketplace': Amazon marketplace (e.g., Amazon.com)
- 'country': Country name
- 'marketplace_currency': Currency code (USD, EUR, etc.)
- 'revenue_abcd_class': Revenue classification
- 'pareto_abc_class': Pareto classification (A=top 80%, B=next 15%, C=bottom 5%)
- 'inventory_id': Specific inventory item ID
- 'io_batch_id': Inventory Order batch ID
- 'vendor': Supplier/vendor name
- 'origin_warehouse': Source warehouse
- 'destination_warehouse': Destination warehouse (current location)

FILTERS (filters object):
- company_id (required): Array of company IDs for multi-company analysis
- sku: Filter by specific SKUs
- brand: Filter by brand names
- product_family: Filter by product families
- child_asin: Filter by child ASINs
- parent_asin: Filter by parent ASINs
- marketplace: Filter by marketplaces
- country: Filter by countries
- marketplace_currency: Filter by currencies
- revenue_abcd_class: Filter by revenue classes ['A', 'B', 'C', 'D']
- pareto_abc_class: Filter by Pareto classes ['A', 'B', 'C']
- inventory_id: Filter by inventory IDs
- vendor: Filter by vendors
- origin_warehouse: Filter by source warehouses
- destination_warehouse: Filter by destination warehouses

TIME AGGREGATION:
- periodicity: 'total' (default) = single snapshot, 'month' = monthly snapshots, 'year' = yearly snapshots
- snapshot_date: Specific date for valuation (YYYY-MM-DD). If not provided, uses latest available data.
- When periodicity is 'month' or 'year' with snapshot_date, creates historical time series up to that date

OUTPUT METRICS:
- balance_quantity: Total inventory units at snapshot date
- balance_amount: Total inventory value (quantity × item_landed_cost)
- batches_count: Number of distinct inventory batches
- avg_unit_cost: Average cost per unit (balance_amount ÷ balance_quantity)
- time_period: Time period label (if periodicity != 'total')

SORT OPTIONS:
- balance_amount (default)
- balance_quantity
- batches_count
- avg_unit_cost
- time_period

USE CASES:
1. Current inventory value by warehouse: group_by=['destination_warehouse'], no snapshot_date
2. Inventory value by brand at year-end: group_by=['brand'], snapshot_date='2024-12-31'
3. Monthly inventory trends: group_by=['brand'], periodicity='month', snapshot_date='2024-12-31'
4. SKU-level valuation: group_by=['sku'], sort by balance_amount DESC

COMPANY ACCESS:
- Requires permission: view:quicksight_group.inventory_management_new OR view:quicksight_group.finance-new
- Returns only data for companies user has access to

Side effect level: `write`

**Use when:**
- analyze current inventory value using FIFO batch balances

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **aggregation** (`object`):
- **sort** (`object`):
- **limit** (`integer`): Maximum number of rows to return

## inventory_valuation_analyze_shipment_discrepancy

Side effect level: `unknown`

## inventory_valuation_audit_inventory_balances

Audit actual inventory counts against NeonPanel-calculated inventory. This tool compares neonpanel_iceberg.inventory_balances, which stores source-side quantities imported from Amazon, AWD, Shopify, 3PL feeds, or manual audit uploads, against NeonPanel calculated FIFO balances from neonpanel_iceberg.fifo_transactions_snapshot. Use it when the user asks to compare calculated inventory to actual warehouse/3PL/source data, find mismatches, identify missing source rows, or validate manually uploaded audit counts.

DATA RELATIONSHIP:
- inventory_balances is actual/source inventory by company, date, source, status, inventory_id/SKU, and warehouse_id/warehouse.
- fifo_transactions_snapshot is NeonPanel calculated inventory from transaction history; this tool sums the latest positive batch balances as of the audit date.
- Rows are matched by company + audit date + inventory_id or SKU + warehouse name.

FLAGS:
- audit_status='matched_within_tolerance': actual and calculated quantities differ by at most quantity_tolerance.
- audit_status='quantity_discrepancy': both sides exist but quantity difference exceeds tolerance.
- audit_status='missing_calculated_data': actual/source count exists but no matching calculated FIFO balance exists.
- audit_status='missing_actual_data': calculated FIFO balance exists but no matching actual/source count exists.
- needs_review is true for discrepancies and missing data.

DATE BEHAVIOR:
- filters.snapshot_date compares exactly that inventory_balances date to calculated FIFO quantity as of that date.
- If snapshot_date is omitted, the latest available inventory_balances date per company after filters is used.

Side effect level: `write`

**Use when:**
- audit actual inventory counts against NeonPanel-calculated inventory

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **audit** (`object`):
- **sort** (`object`):
- **limit** (`integer`):

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

## shopify_list_orders

List Shopify orders with line items and fulfillment/order events. Read-only; no side effects.

Use cases:
- View Shopify orders with their line items and event history (fulfillment, refund, etc.)
- Filter by financial status, fulfillment status, event type/status, SKU, seller, warehouse, currency
- Aggregate orders by seller, status, event type, warehouse, SKU, or month
- Track order lifecycle from placement through fulfillment events

Input uses the Option-1 query envelope:
- query: shared filters/limit/sort/projection/grouping

Important (company selection):
- Use query.filters.company_id (preferred, integer) or query.filters.company (numeric string only)
- At least one must be provided for authorization
- Company is resolved through shopify_sellers.company_id

Data sources:
- neonpanel_iceberg.shopify_orders (order header: amounts, statuses, timestamps)
- neonpanel_iceberg.shopify_order_items (line items: sku, quantity, amount, title)
- neonpanel_iceberg.shopify_order_events (events: fulfillment, refund, etc. with warehouse, quantity, amount)
- neonpanel_iceberg.shopify_sellers (seller name, domain, status, connection state)
- neonpanel_iceberg.inventory_warehouses (warehouse name)
- neonpanel_iceberg.app_companies (company name)

Detail mode (no group_by):
- Returns one row per order × line-item × event combination
- Includes full order header, item details, and event details
- Sorted by order_created_at DESC

Filter logic:
- Array filters use OR logic within the same filter type
- Multiple filter types use AND
- All string filters are case-insensitive

Date filters:
- query.filters.date_from / date_to: filter on order creation date (shopify_created_at)

Aggregation:
- query.aggregation.group_by: returns one row per unique dimension combination
- Supported dimensions: company, seller, financial_status, fulfillment_status, currency, event_type, event_status, warehouse, sku, order_month
- company_id is always included in GROUP BY for authorization
- Aggregated rows include: order_count, line_item_count, distinct_skus, total_item_quantity, total_item_amount, total_event_quantity, total_event_amount, total_order_amount, total_order_subtotal, earliest/latest_order_date

Side effect level: `read`

**Use when:**
- list Shopify orders with line items and fulfillment/order events

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
