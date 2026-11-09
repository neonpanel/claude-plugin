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

## forecasting_check_sales_forecast_job_status

Check the status of a previously started sales-forecast Glue job run.

Use this after calling forecasting_run_sales_forecast_job to monitor progress.
Use forecasting_list_sales_forecasts or forecasting_get_sales_forecast_details to review results after completion.

IMPORTANT: Jobs typically take 3-5 minutes. Do NOT check status earlier than 3 minutes after starting the job.

Possible states:
- STARTING / RUNNING / WAITING: Job is still in progress. Check again in 1-2 minutes.
- SUCCEEDED: Job completed successfully. Forecast data is now available.
- FAILED / ERROR / TIMEOUT / STOPPED: Job did not complete. Check error_message for details.

Side effect level: `read`

**Use when:**
- check the status of a previously started sales-forecast Glue job run

**Required context:**
- job_run_id

**Required inputs:**
- - **job_run_id** (`string` (required)): Required. The JobRunId returned by forecasting_run_sales_forecast_job.

## forecasting_compare_sales_forecast_scenarios

Deep-dive forecasting comparison for multiple items. Supports selectors by inventory_ids, sku+country_code, parent_asin, or product_family (pick one). Compares forecast outputs across scenarios and/or forecast runs, with actuals overlay (include_actuals=true by default). Read-only; no side effects.

Latest forecast evaluation:
- The 'actual' scenario/dataset is always excluded when selecting forecast runs
- Actuals are overlaid separately (same periods) when include_actuals=true, enabling actual-vs-forecast comparison

Aggregation:
- query.aggregation.group_by: optional array of dimensions to aggregate by
- Supported: company, brand, product_family, parent_asin, asin
- company_id is always included for authorization
- When aggregated, numeric fields (units_sold, sales_amount) are summed per (group, period, scenario)
- If group_by is omitted, returns per-item detail rows

Company selection:
- Use query.filters.company_id (preferred) or query.filters.company (numeric string)

Actual-vs-forecast comparison:
- Set include_actuals=true (default) and optionally filter scenario_names to compare specific forecasts against actuals
- Output rows have series_type='actual' or series_type='forecast' to distinguish them

Side effect level: `read`

**Use when:**
- deep-dive forecasting comparison for multiple items

**Required context:**
- filters

**Required inputs:**
- - **filters** (`object` (required)): Item selector and optional filters. Provide exactly one selector path: inventory_ids OR sku + country_code OR parent_asin OR product_family.
- **aggregation** (`object`): Optional aggregation. If group_by is provided, returns aggregated rows instead of per-item detail.
- **limit** (`integer`): Max rows returned (each row = one period x scenario x run). If limit exceeded, most recent periods are prioritized.
- **tool_specific** (`object`):

## forecasting_generate_sales_forecast

Generate sales forecasts using multiple statistical methods on user-provided historical data. This tool applies 6+ forecasting algorithms to your time series data and returns predictions for future periods.

FORECASTING METHODS AVAILABLE:

1. rwlt_monthly_plan (DEFAULT - Recommended)
   - Recency-Weighted Linear Trend (monthly)
   - Best for: Items with recent trends, 6+ months history
   - Uses exponential time decay (recent data weighted more)
   - Softens negative trends by 50% to prevent over-pessimism

2. seasonal_naive
   - Last month's units × seasonality factor
   - Best for: Stable seasonal patterns
   - Simple baseline for comparison

3. moving_avg_12
   - 12-month moving average × seasonality
   - Best for: Smoothing volatility
   - Requires 12+ months history

4. trend_seasonal
   - Linear regression trend × seasonality
   - Best for: Clear growth/decline patterns
   - Uses all historical data equally

5. robust_low (Conservative Ensemble)
   - 70% MA12 + 30% seasonal naive
   - Best for: Risk-averse planning
   - Produces lower-bound estimates

6. availability_plan
   - Compound growth model (default 30% annual)
   - Best for: High-growth items
   - Uses last 3-month average as anchor

7. adgp_rwdr_plan
   - Recency-Weighted Daily Rate
   - Best for: Items with high variance
   - Winsorizes outliers at 97th percentile

SEASONALITY:
- Automatically learned from historical patterns
- Can override with manual 12-month pattern (e.g., "1.0;1.2;1.5;...")
- Applied to all forecast methods

INPUT REQUIREMENTS:
- Minimum: 3 months of history (6-12 months recommended)
- Required fields: period (YYYY-MM), units_sold, sales_amount
- Optional: price, seasonality override

OUTPUT:
- Monthly forecasts for specified horizon (default 24 months)
- Separate series for each selected method
- Includes: forecast_period, units_sold, sales_amount, method_name
- Price projections based on historical averages

NEXT STEPS AFTER GENERATING FORECAST:
1. Use 'forecasting_write_sales_forecast' tool to save generated forecasts to the database
2. After writing, verify your data in the NeonPanel Sales Forecast Live report:
   https://my.neonpanel.com/app/reports/forecasting/sales-forecast-live?filterSet=current

Side effect level: `write`

**Use when:**
- generate sales forecasts using multiple statistical methods on user-provided historical data

**Required context:**
- historical_data

**Required inputs:**
- - **historical_data** (`array` (required)): Time series of historical sales data. Each item represents one month of data.
- **forecast_config** (`object`): Configuration for forecast generation
- **item_metadata** (`object`): Optional metadata about the item being forecasted (for context/labeling)

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

## forecasting_get_sales_forecast_details

Get forecast plan details per item (SKU-level) with rich inventory attributes. Read-only; no side effects.

By default returns the LATEST forecast run. To view a specific forecast, provide tool_specific.scenario_uuid and/or tool_specific.calc_period (get these from forecasting_list_sales_forecasts).

Use cases:
- Show forecast plan per SKU/inventory item (latest or a specific run)
- Filter by brand/product_family/ASIN/SKU and sort by sales or forecast KPIs
- Optionally aggregate results by company/brand/product_family/parent_asin/asin
- Optionally include historical actuals as reference (include_actuals=true)

Forecast run selection (tool_specific):
- scenario_uuid + calc_period: pin to that exact forecast run
- scenario_uuid only: latest calc_period for that scenario
- calc_period only: latest scenario for that calc_period
- Neither: auto-picks the latest forecast run (default)

Input uses the query envelope:
- query: shared filters/limit/sort/projection/grouping
- tool_specific: forecasting-specific knobs (run selection, horizon, which plan fields to include)

Important (company selection):
- Use query.filters.company_id (preferred, integer) or query.filters.company (numeric string only; NOT freeform names)
- If both provided, company_id takes precedence
- At least one must be provided for authorization

Filter logic (selector filters):
- query.filters.asin (child ASIN), query.filters.parent_asin, query.filters.brand, query.filters.product_family, query.filters.sku
- Array filters use OR logic: filter.brand=[\"Nike\",\"Adidas\"] returns items with EITHER brand
- Multiple filter types use AND: (brand=Nike) AND (sku=ABC123)

Supported classification filters:
- query.filters.revenue_abcd_class: [\"A\", \"B\"] returns A+B items (OR logic)

Aggregation:
- query.aggregation.group_by=[\"brand\", \"product_family\"]: returns one row per unique dimension combination
- Supported dimensions: company, brand, product_family, parent_asin, asin
- company_id is always included as a grouping key for authorization
- In aggregated mode, numeric fields (sales, units) are summed; forecast/actuals series are summed per period
- Aggregated rows include inventory_count, sku_count
- If group_by is omitted or empty, returns SKU-level detail rows

Latest forecast evaluation:
- The 'actual' scenario/dataset is always excluded when determining forecast runs

Actual data (include_actuals):
- When include_actuals=true, rows include actuals_series_json with historical actual data (dataset='actual')
- Works in both detail and aggregated modes

Field selection:
- query.select_fields: optional output projection (array of field names)

Unsupported filters:
- query.filters.tags is ignored (no tags column in snapshot); meta.warnings will explain.

Side effect level: `read`

**Use when:**
- get forecast plan details per item (SKU-level) with rich inventory attributes

**Do not use when:**
- not for create/update/delete workflows

**Required inputs:**
- - **filters** (`object`): Standard filters. Tools should support a meaningful subset and warn on unsupported keys.
- **aggregation** (`object`): Optional aggregation. If group_by is provided, returns aggregated rows instead of SKU-level detail.
- **sort** (`object`): Sort by any returned output field/KPI.
- **select_fields** (`array`): Optional output field selection (projection). If omitted, tool returns defaults.
- **limit** (`integer`):
- **cursor** (`string`): Opaque cursor for pagination (if supported).
- **tool_specific** (`object`): Forecasting-specific knobs: run selection + which plan fields to include.

**Related tools:**
- list/search tool for entity lookup

## forecasting_list_sales_forecasts

List all available forecast runs for a company. Returns one row per distinct forecast run (unique calc_period + updated_at + dataset combination) with summary statistics.

Use this tool FIRST to discover what forecasts exist before comparing or reviewing them.

Each row represents a forecast run and includes:
- calc_period: the calculation period (YYYY-MM) that groups the run
- updated_at: timestamp when the run was written
- dataset: scenario/method name (e.g. 'rwlt_monthly_plan', 'manual', 'actual')
- item_count: how many distinct SKUs/inventory_ids are in this run
- period_count: how many forecast months are covered
- period_start / period_end: the date range of forecast periods
- total_units / total_sales_amount: aggregate totals across all items and periods

Default sort: calc_period DESC, updated_at DESC (most recent first).
Tip: use limit=1 to get just the latest forecast run.

Important (company selection):
- query.filters.company_id (required, integer)

Optional filters:
- query.filters.scenario: filter by dataset/scenario name(s)
- query.filters.marketplace: filter by marketplace (US, UK)
- query.filters.calc_period: filter to specific calc_period(s) (YYYY-MM format)

Side effect level: `read`

**Use when:**
- list all available forecast runs for a company

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)):
- **limit** (`integer`): Max rows returned. Use limit=1 to get only the latest forecast run.

## forecasting_run_sales_forecast_job

Trigger the sales-forecast Glue ETL job that re-computes forecasts and writes the results to the Iceberg table.

The job is idempotent: it deletes existing rows for the target calc_period + scenario_uuid + company_id before appending fresh results.

The tool returns immediately with a JobRunId. Use forecasting_check_sales_forecast_job_status to monitor progress.

Jobs typically take 3-5 minutes to complete.

IMPORTANT:
- Before calling, use get_forecasting_settings for the company to obtain the correct scenario_uuid(s).
- company_id, calc_period, and scenario_uuids are all REQUIRED to prevent unintentional broad recalculations.
- After starting, use forecasting_check_sales_forecast_job_status — do NOT check earlier than 3 minutes.

Side effect level: `write`

**Use when:**
- trigger the sales-forecast Glue ETL job that re-computes forecasts and writes the results to the Iceberg table

**Required context:**
- calc_period
- company_id
- scenario_uuids

**Required inputs:**
- - **company_id** (`integer` (required)): Required. The company to run the forecast for. Use get_forecasting_settings to look up the company's forecasting configuration.
- **calc_period** (`string` (required)): Required. Calculation period in YYYY-MM format (e.g. '2026-03').
- **scenario_uuids** (`array` (required)): Required. Scenario UUID(s) to run. Use get_forecasting_settings for the company to get the default scenario_uuid.
- **inventory_id** (`integer`): Optional. Run forecast for a single inventory item only. Omit to run for ALL items in the company.
- **horizon** (`integer`): Forecast horizon in months. Default: 24.

**Clarifying questions:**
- Which value should I use for calc_period?

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

## forecasting_write_sales_forecast

Write user/AI-adjusted sales forecast values into an Iceberg table for auditability and downstream planning. Consequential (writes data).

Use cases:
- Persist manual overrides to a forecast for specific item(s) and period(s)
- Persist AI-recommended adjustments with an explainable reason
- Store sales history (actuals) data for actual-vs-forecast comparison

Safety / audit:
- Always include company_id and an explicit item selector (inventory_id or sku+marketplace)
- Always include a reason and an author/source
- Prefer dry_run=true for validation before writing

Author (important):
- If author.type=ai, author.name MUST be provided. Ask the user what name to record for audit (e.g., their name or team).

Sales amount (required):
- sales_amount is REQUIRED for each write. Derive it as: sales_amount = units_sold * unit_price.
- If unit_price is unknown, either (a) compute it from the latest existing forecast (sales_amount / units_sold) for the same SKU+marketplace, or (b) ask the user to provide an assumed unit price.

Data type (IMPORTANT):
- data_type='forecast' (default): writes forecast data. dataset is set to 'manual', scenario_uuid stores the user-provided scenario name.
- data_type='actual': writes sales history / actuals data. dataset is FORCED to 'actual' and scenario_uuid is FORCED to 'actual', regardless of the scenario_uuid provided in writes[]. Use this when the user is storing historical sales data, importing actuals, or uploading sales history.

Scenario handling (for forecast writes):
- scenario_uuid is REQUIRED and should contain the user-provided scenario name (slug). It is used for idempotency in replace mode.
- Allowed format: lowercase letters, numbers, hyphen, underscore. Length 1–64 (regex: ^[a-z0-9][a-z0-9_-]{0,63}$).

Important (company selection):
- Use company_id (preferred) or query.filters.company (numeric string). Do NOT pass a freeform company name.

Marketplace input note:
- marketplace accepts a country code like US/UK (preferred) and will be mapped to the correct amazon_marketplace_id when writing. country_code is derived automatically from marketplace.
- sales_channel defaults to 'Amazon' for Amazon marketplace forecasts. Always written to the table for downstream filtering.

Example (forecast dry run):
{
  "company_id": 103,
  "reason": "Velocity adjustment based on recent sales",
  "author": { "type": "ai", "name": "Kio" },
  "dry_run": true,
  "writes": [
    { "inventory_id": 29972, "scenario_uuid": "override", "forecast_period": "2026-01", "units_sold": 151, "sales_amount": 1234.56, "currency": "USD" }
  ]
}

Example (actuals import):
{
  "company_id": 103,
  "data_type": "actual",
  "reason": "Importing sales history for Jan 2026",
  "author": { "type": "user", "name": "Mike" },
  "dry_run": true,
  "writes": [
    { "sku": "KEM-ASC-LG-BL1520-XL", "marketplace": "US", "scenario_uuid": "actual", "forecast_period": "2026-01", "units_sold": 200, "sales_amount": 1600.00, "currency": "USD" }
  ]
}

Side effect level: `write`

**Use when:**
- write user/AI-adjusted sales forecast values into an Iceberg table for auditability and downstream planning

**Do not use when:**
- not for read-only information requests

**Required context:**
- company_id
- reason
- writes

**Required inputs:**
- - **company_id** (`integer` (required)): Company id the write applies to. Must be authorized.
- **author** (`object`): Who/what is making this change (for audit). If type='ai', name MUST be provided (ask the user for their name or team).
- **reason** (`string` (required)): Human-readable reason for the adjustment (stored for audit). Be specific (minimum 10 characters).
- **dry_run** (`boolean`): If true, validate and preview writes without persisting.
- **write_mode** (`string`): append (default) inserts new rows; replace deletes existing rows for the same inventory_id (or sku+marketplace) + forecast_period + scenario_uuid before inserting.
- **idempotency_key** (`string`): Optional idempotency key to prevent duplicate writes on retries.
- **debug_sql** (`boolean`): If true during dry_run, include rendered Athena SQL in meta.debug for troubleshooting. (Ignored when dry_run=false.)
- **data_type** (`string`): Type of data being written. 'forecast' (default) writes manual forecast adjustments (dataset='manual'). 'actual' writes sales history/actuals data — forces dataset='actual' and scenario_uuid='actual' regardless of per-write scenario_uuid values. Use 'actual' when importing historical sales data.
- **writes** (`array` (required)): One or more forecast write operations (period-level).

**Clarifying questions:**
- Which value should I use for company_id?

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
