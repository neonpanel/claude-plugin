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

## advertising_analyze_campaign_performance

Analyzes Amazon Advertising Marketing Stream data across Sponsored Products (SP), Sponsored Brands (SB), and Sponsored Display (SD), enriched with ASIN attributes.

Use this tool to:
- Compare campaign-level or ad-group-level ROAS, ACoS, and spend efficiency
- Analyze performance by placement (Top of Search, Detail Page, Other, Off Amazon)
- Slice spend by ad type (SP vs SB vs SD) to find the most efficient channel
- Track monthly or yearly trends in advertising efficiency
- Identify top/bottom campaigns by any KPI
- Filter by product dimensions (product_family, brand, pareto class) to see ad spend allocation by portfolio segment
- Drill into targeting keywords to see which targeting expressions drive conversions

Data enrichment: Each row is enriched with ASIN attributes (product_family, brand, pareto_abc_class, revenue_abcd_class, revenue_share) via the purchased/advertised ASIN.

Periodicity: Data is daily granularity. Group by day, month, year, or total (no time breakdown).

Integration note: Use with advertising_analyze_search_terms for search-query-level drill-down (SP only), or brand_analytics_get_search_term_momentum to correlate ad spend with organic momentum.

Knowledge base: https://help.neonpanel.com/en/articles/advertising-campaign-analysis

Side effect level: `read`

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for campaign performance analysis. company_id is required; all others are optional.
- **aggregation** (`object`): Controls time window, periodicity, and grouping.
- **sort** (`object`): Controls ranking and ordering of results. Rows are returned with a 'rank' column (1 = best).
- **limit** (`integer`): Maximum rows to return (default 100, max 500). Only the top N rows by the chosen sort order are returned. Increase to 500 for broader coverage, or use filters to focus on specific segments.

## advertising_analyze_search_terms

Analyzes Amazon Sponsored Products Search Term Report data enriched with product attributes via campaign→ASIN mapping.

Use this tool to:
- Harvest 'Proven Winners': high-converting search terms to promote to exact match or inject into organic SEO (Title/Bullets)
- Identify 'Bleeders': terms with high spend but no/low sales for Negative Exact targeting
- Compare match type efficiency (broad vs phrase vs exact)
- Slice PPC performance by product dimensions: product_family, brand, pareto_abc_class, revenue_abcd_class
- Find which product families or brands drive the most efficient ad spend
- Bridge paid and organic strategy by cross-referencing with Brand Analytics tools

IMPORTANT: Results are capped by the 'limit' parameter (default 100). When search_terms is omitted, ALL search terms are considered but only the top N by sort order are returned. To explore different keyword groups, either: (1) change sort field/direction to surface different keywords, (2) use search_terms filter with match_type='contains' to drill into a keyword category (e.g., ['ring'] to see all ring-related terms), or (3) use product_families/brands filters to narrow by product line.

Data enrichment: Each search term row is joined to its campaign's ASIN via campaign_asin_map, then enriched with ASIN attributes (product_family, brand, pareto_abc_class, revenue_abcd_class, revenue_share) for filtering and grouping.

Attribution: Uses the longest available attribution window (30d → 14d → 7d → 1d) for sales/purchases/units.

Integration note: Cross-reference results with brand_analytics_get_keyword_funnel_metrics to compare paid efficiency vs organic share, or with brand_analytics_get_search_term_momentum to see organic momentum for the same terms.

Knowledge base: https://help.neonpanel.com/en/articles/advertising-search-term-analysis

Side effect level: `read`

**Use when:**
- analyzes Amazon Sponsored Products Search Term Report data enriched with product attributes via campaign→ASIN mapping

**Do not use when:**
- not for irreversible changes

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for search term analysis. company_id is required; all others are optional.
- **aggregation** (`object`): Controls time window and grouping.
- **sort** (`object`): Controls ranking and ordering of results. Rows are returned with a 'rank' column (1 = best).
- **limit** (`integer`): Maximum rows to return (default 100, max 500). Only the top N rows by the chosen sort order are returned. Increase to 500 for broader coverage, or use filters to focus on specific keyword segments.

## brand_analytics_analyze_repeat_purchases

Analyzes repeat purchase behaviour per ASIN from Amazon Brand Analytics Repeat Purchase Report.

Use this tool to:
- Identify ASINs with the highest repeat customer rates (loyalty signals)
- Find products driving recurring revenue (Subscribe & Save candidates)
- Compare repeat purchase rates across the portfolio to spot consumables vs one-time buys
- Track week-over-week trends in repeat purchase metrics
- Detect ASINs with declining repeat rates (quality/satisfaction issues)

Key metrics:
- repeat_customers_pct: Fraction of customers who purchased the ASIN more than once (0-1 scale). Higher values indicate strong product loyalty or consumable nature.
- repeat_revenue_pct: Fraction of total ASIN revenue from repeat buyers (0-1 scale). High repeat_revenue_pct with low repeat_customers_pct suggests a few heavy buyers.
- orders vs unique_customers: When orders >> unique_customers, customers are re-ordering.

WoW trends: The latest-week snapshot includes week-over-week deltas for repeat_customers_pct, repeat_revenue_pct, and orders. Positive repeat_customers_pct_wow = growing loyalty.

IMPORTANT: Results are capped by the 'limit' parameter (default 50). When 'asin' filter is omitted, ALL ASINs are considered but only the top N by sort order are returned. A seller may have hundreds of ASINs. To explore different segments: (1) filter by specific ASINs, (2) adjust sort field, or (3) increase limit up to 200.

Default time window: Last 4 complete weeks. Use periods_back or explicit start_date/end_date for custom ranges.

Tip: Sort by avg_repeat_customers_pct DESC to find the most loyal products. Use min_orders to filter out low-volume ASINs with noisy percentages.

Integration note: Cross-reference with brand_analytics_get_cross_sell_opportunities to find bundle candidates among high-repeat ASINs. Use brand_analytics_get_keyword_funnel_metrics to understand acquisition funnels for top repeaters.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-repeat-purchase

Side effect level: `write`

**Use when:**
- analyzes repeat purchase behaviour per ASIN from Amazon Brand Analytics Repeat Purchase Report

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): Filters for repeat purchase analysis. company_id is required; all others are optional.
- **aggregation** (`object`):
- **sort** (`object`): Controls ranking and ordering of results. Rows are returned with a 'rank' column (1 = best).
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — reduces response size. IMPORTANT: Use exact field names from the enum — e.g., 'avg_repeat_customers_pct' not 'repeat_customer_pct'.
- **limit** (`integer`): Maximum rows to return (default 50, max 200). Only the top N ASINs by the chosen sort order are returned.
- **tool_specific** (`object`): Repeat purchase analysis thresholds.

## brand_analytics_analyze_search_catalog_performance

Analyzes Search Catalog Performance using the Brand Analytics catalog performance report. Returns KPI metrics, trend deltas, and 4 high-level RYG signals (strength, weakness, opportunity, threshold) with explanations. Supports filtering by classification and signal colors.

Side effect level: `read`

**Use when:**
- analyzes Search Catalog Performance using the Brand Analytics catalog performance report

**Do not use when:**
- not for irreversible changes

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): WHERE clause filters. company_ids is required.
- **aggregation** (`object`):
- **sort** (`object`):
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — significantly reduces response size. If omitted, returns all columns (~90 fields). IMPORTANT: Use exact field names from the enum.
- **ryg_company_id** (`integer`): Optional: company_id whose custom RYG thresholds to apply. If omitted, uses the first company_id from filters. If the company has no custom thresholds, system defaults are used.
- **limit** (`integer`):

## brand_analytics_analyze_search_query_performance

Analyzes Search Query Performance (SQP) using a precomputed snapshot. Returns KPI metrics, deltas, efficiency ratios (click-through and conversion), diagnostic scenario classification (A: visibility, B: creative, C: conversion, D: protect), term type (branded/generic/long_tail), priority tier (1-4), and RYG signals. Supports filtering by all computed dimensions.

Side effect level: `read`

**Use when:**
- analyzes Search Query Performance (SQP) using a precomputed snapshot

**Do not use when:**
- not for irreversible changes

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): WHERE clause filters. company_ids is required.
- **aggregation** (`object`):
- **sort** (`object`):
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — significantly reduces response size. If omitted, returns all columns (~100 fields). IMPORTANT: Use exact field names from the enum — e.g., 'searchquerydata_searchquery' not 'search_query'.
- **ryg_company_id** (`integer`): Optional: company_id whose custom RYG thresholds to apply. If omitted, uses the first company_id from filters. If the company has no custom thresholds, system defaults are used.
- **limit** (`integer`):

## brand_analytics_get_competitive_landscape

Identifies the Top 3 clicked products for search terms and their relative click/conversion shares from Amazon Brand Analytics Top Search Terms report.

Use this tool to:
- Find 'Weak Leader' keywords where top-ranking products have low conversion shares (opportunity to displace)
- Identify keywords where competitors dominate (defensive priority)
- Discover share-of-voice gaps between your products and competitors
- Analyze competitive positioning trends over time

IMPORTANT — Data enrichment opportunity: Amazon does NOT expose Search Query Details (total impressions, total clicks, click rate, per-ASIN impressions, price, and up to 10 competing ASINs) via API. Each result row includes a `seller_central_query_detail_url` deep link. Ask the user to open that URL in Seller Central and upload a screenshot for significantly richer competitive analysis. The screenshot contains data unavailable anywhere else.

Integration note: Use with brand_analytics_get_keyword_funnel_metrics to understand why competitors may be winning or losing.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-competitive

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): Standard filters for competitive analysis.
- **aggregation** (`object`):
- **sort** (`object`):
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — reduces response size. IMPORTANT: Use exact field names from the enum.
- **limit** (`integer`):
- **tool_specific** (`object`): Competitive landscape analysis specific parameters.

**Related tools:**
- list/search tool for entity lookup

## brand_analytics_get_conversion_leak_analysis

Provides ASIN-level funnel metrics to identify where potential buyers drop off between discovery and purchase.

Use this tool to diagnose:
- High impressions but low clicks: Visibility problem (main image, title, price positioning)
- High clicks but low cart-adds: Content friction (bullets, A+ content, reviews, pricing)
- High cart-adds but low purchases: Checkout friction (shipping, coupons, Buy Box issues)

Integration note: Cross-reference with brand_analytics_get_ad_efficiency to see if paid traffic converts differently than organic.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-asin-funnel

Side effect level: `read`

**Use when:**
- provides ASIN-level funnel metrics to identify where potential buyers drop off between discovery and purchase

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_ids
- filters.marketplaces

**Required inputs:**
- - **filters** (`object` (required)): Standard filters for ASIN selection.
- **aggregation** (`object`): Time-based aggregation options.
- **sort** (`object`):
- **limit** (`integer`):
- **tool_specific** (`object`): Conversion leak analysis specific parameters.

**Clarifying questions:**
- Which value should I use for filters.company_ids?

**Related tools:**
- list/search tool for entity lookup

## brand_analytics_get_cross_sell_opportunities

Retrieves products frequently purchased together from Amazon Brand Analytics Market Basket Analysis report.

Use this tool to:
- Discover complementary products for Virtual Bundle creation
- Identify A+ Content cross-sell table candidates
- Find Product Targeting ad campaign opportunities (target ASINs frequently co-purchased with yours)
- Detect competitor products commonly bought alongside yours (defensive/offensive targeting)
- Validate bundle ideas with real co-purchase data

Data source: Weekly Market Basket report. Each primary ASIN has up to 3 co-purchased ASINs per week.
- combination_pct: Percentage of the primary ASIN's orders that also included the co-purchased ASIN (0-100 scale)
- best_rank: Amazon's co-purchase rank (1 = most frequently bought together)
- consistency_score: How reliably this pair appears across weeks (1.0 = every week in the window)

Ownership detection: co_purchased_is_own = true when the co-purchased ASIN also appears as a primary ASIN (i.e., it belongs to the seller). Use this to distinguish own-portfolio cross-sell from competitor product insights.

IMPORTANT: Results are capped by the 'limit' parameter (default 50). When 'asin' filter is omitted, ALL ASINs are considered but only the top N pairs by sort order are returned. A seller may have hundreds of ASIN pairs. To explore different segments: (1) filter by specific ASINs, (2) adjust sort field, or (3) increase limit up to 200.

Default time window: Last 4 complete weeks. Use periods_back or explicit start_date/end_date for custom ranges.

Tip: Filter to max_rank=1 to see only the #1 co-purchased product per ASIN. Raise min_combination_pct to focus on strong signals.

Integration note: Cross-reference with advertising_analyze_search_terms to validate co-purchase patterns with paid traffic behaviour. Use brand_analytics_get_keyword_funnel_metrics to check if co-purchased ASINs share keywords.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-market-basket

Side effect level: `read`

**Use when:**
- retrieves products frequently purchased together from Amazon Brand Analytics Market Basket Analysis report

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): Filters for cross-sell analysis. company_id is required; all others are optional.
- **aggregation** (`object`):
- **sort** (`object`): Controls ranking and ordering of results. Rows are returned with a 'rank' column (1 = best).
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — reduces response size. IMPORTANT: Use exact field names from the enum — e.g., 'primary_asin' not 'purchased_asin', 'avg_combination_pct' not 'combination_pct'.
- **limit** (`integer`): Maximum rows to return (default 50, max 200). Only the top N pairs by the chosen sort order are returned.
- **tool_specific** (`object`): Cross-sell analysis thresholds.

**Related tools:**
- list/search tool for entity lookup

## brand_analytics_get_keyword_funnel_metrics

Returns search funnel data (Impressions, Clicks, Cart Adds, Purchases) for keywords from Amazon Brand Analytics Search Query Performance (SQP) report.

Compares 'Brand Share' against 'Total Market' to determine if the product is a conversion leader or needs SEO/PPC adjustments.

Use this tool to:
- Identify organic growth potential vs. conversion efficiency
- Find keywords where you have high impressions but low click share (visibility gap)
- Discover keywords where you have high clicks but low cart-add share (content/pricing issue)
- Analyze conversion funnel drop-off points at each stage

IMPORTANT: Results are capped by the 'limit' parameter (default 100). When 'keywords' is omitted, ALL keywords are considered but only the top N by sort order are returned. A company may have thousands of tracked keywords. To explore different keyword groups: (1) change sort field/direction to surface different keywords, (2) use keywords filter with match_type='contains' to drill into a keyword theme (e.g., ['ring'] returns all ring-related terms), or (3) use brand filters to narrow by product line.

Integration note: Cross-reference results with advertising_analyze_search_terms to compare organic funnel share vs paid PPC efficiency on the same keywords, or with brand_analytics_get_search_term_momentum to track click share trends over time.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-sqp

Side effect level: `read`

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_ids
- filters.marketplaces

**Required inputs:**
- - **filters** (`object` (required)): Standard filters for data selection.
- **aggregation** (`object`): Time-based aggregation for funnel metrics.
- **sort** (`object`): Sort results by any output metric.
- **select_fields** (`array`): Optional output field selection. If omitted, returns all available metrics. IMPORTANT: Use exact field names from the enum.
- **limit** (`integer`): Maximum rows to return (default 100, max 200). Only the top N rows by the chosen sort order are returned. Use keywords filter to focus on specific keyword segments.
- **tool_specific** (`object`): Brand Analytics keyword funnel specific parameters.

**Clarifying questions:**
- Which value should I use for filters.company_ids?

**Related tools:**
- list/search tool for entity lookup

## brand_analytics_get_search_term_momentum

Returns weekly search term momentum data from the Brand Analytics smart snapshot: your click share, WoW delta, 4-week and 12-week rolling averages, top-3 competitor ASINs with click/conversion shares, and a computed momentum signal.

Use this tool to:
- Track how your click share on a search term is trending (accelerating, growing, stable, declining, collapsing)
- Identify search terms where you are losing or gaining momentum vs competitors
- See which competitor ASINs hold the top 3 positions and their conversion shares
- Find weak leader opportunities where rank-1 has low conversion share
- Spot defensive priorities where competitors are gaining share on your keywords

IMPORTANT: Results are capped by the 'limit' parameter (default 100). When search_terms is omitted, ALL search terms are considered but only the top N by sort order are returned. A company may have thousands of tracked search terms. To explore different keyword groups: (1) change sort field/direction to surface different terms, (2) use search_terms filter with match_type='contains' to drill into a keyword theme (e.g., ['ring'] returns all ring-related terms), or (3) use brand/product_family/category filters to narrow by product line.

Momentum signals explained:
- accelerating: WoW up AND 4-week avg above 12-week avg (sustained upward trend)
- growing: WoW up (recent positive movement)
- stable: no significant WoW change
- declining: WoW down (recent negative movement)
- collapsing: WoW down AND 4-week avg below 12-week avg (sustained downward trend)
- new: first appearance (no prior week data)

Integration note: Use with brand_analytics_get_keyword_funnel_metrics to understand the full funnel behind a search term, or with brand_analytics_get_competitive_landscape for raw report-level detail.

Knowledge base: https://help.neonpanel.com/en/articles/brand-analytics-competitive

Side effect level: `read`

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_ids

**Required inputs:**
- - **filters** (`object` (required)): Standard filters for momentum analysis.
- **aggregation** (`object`):
- **sort** (`object`):
- **select_fields** (`array`): Optional output field selection. When provided, only these fields are returned per row — reduces response size. If omitted, returns all columns. IMPORTANT: Use exact field names from the enum.
- **limit** (`integer`): Maximum rows to return (default 100, max 200). Only the top N rows by the chosen sort order are returned. Use filters to focus on specific keyword segments.
- **tool_specific** (`object`): Momentum analysis specific parameters.

**Related tools:**
- list/search tool for entity lookup

## brand_analytics_growth_machine_diagnosis

Fuses Search Query Performance (SQP) + Search Catalog Performance (SCP) + Amazon Ads (PPC) at the (keyword × ASIN × period) grain and emits one locked prescription per row. This is THE core tool for executing the 'Growth Machine' playbook (SQP tells you where demand is, SCP tells you why you do/don't convert it, PPC tells you what happens when you pay for it).

Prescription enum (one of):
- FIX_CART_LEAK_CUT_PPC   — cart_to_purchase_rate < threshold AND PPC spend ≥ min → plug final-mile leak BEFORE scaling ads.
- NEGATIVE_EXACT          — PPC sales = 0 on ≥ ppc_clicks_min clicks → bleeder, negative-exact and remove from backend.
- INJECT_INTO_SEO         — PPC CVR ≥ green threshold AND organic brand_purchase_share < red threshold → proven winner invisible organically; move into Title/Bullets.
- DEFEND_ORGANIC          — Organic brand_purchase_share ≥ green (cannibalization or defend). Cut PPC bid or hold defensive ad support.
- DISPLACE_WEAK_LEADER    — Top-1 competitor converts < red threshold AND we are already close → allocate budget to displace.
- EVALUATE_OR_SKIP        — No clear signal; gather more data or deprioritize.

All thresholds come from brand_analytics_iceberg.ryg_thresholds with tool IN ('growth_machine','sqp','scp','global') and per-company override precedence.

When `use_tracked_search_terms=true` (default), the keyword universe is auto-scoped to rows in brand_analytics_iceberg.tracked_search_terms. When `use_competitor_registry=true` (default), competitor_asins are exposed for keywords they share. When screenshot data exists in sqp_query_details_uploads for a (keyword, period), `screenshot_data_available=true` and leader/my_share_gap signals are enriched; otherwise leader-based prescriptions are best-effort from SCP market-share approximations.

Each row includes `seller_central_query_detail_url` — a deep link the user can open in Seller Central to upload a screenshot via brand_analytics_upload_sqp_query_details when screenshot_data_available=false.

The response also includes a header with `normalization_match_rate` (share of PPC keywords that matched an SQP/SCP keyword after LOWER(TRIM(...)) normalization) so the agent can reason about drift.

Side effect level: `read`

**Required context:**
- company_id
- period_end
- period_start

**Required inputs:**
- - **company_id** (`integer` (required)):
- **marketplace** (`string`):
- **period_start** (`string` (required)): Inclusive start (UTC date).
- **period_end** (`string` (required)): Inclusive end (UTC date).
- **grain** (`string`):
- **entity_ids** (`array`): Optional. ASINs (for child_asin/parent_asin), family names (product_family), or brand names (brand). Empty array = all company catalog.
- **keywords** (`array`): Optional explicit keyword filter. Overrides use_tracked_search_terms when non-empty.
- **use_tracked_search_terms** (`boolean`):
- **use_competitor_registry** (`boolean`):
- **focus** (`string`):
- **limit** (`integer`):

**Clarifying questions:**
- Which value should I use for company_id?

## brand_analytics_list_analytics_watchlist

Lists the company's saved analytics watchlists from brand_analytics_iceberg.analytics_watchlist. Read-only.

Each row is a named bundle of entities + cadence + focus used by brand_analytics_run_watchlist to drive a recurring diagnosis via brand_analytics_growth_machine_diagnosis.

Upsert key: (company_id, marketplace, LOWER(watchlist_name)).

Side effect level: `read`

**Use when:**
- lists the company's saved analytics watchlists from brand_analytics_iceberg.analytics_watchlist

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)):
- **marketplaces** (`array`):
- **watchlist_names** (`array`):
- **grain** (`array`):
- **cadence** (`array`):
- **focus** (`array`):
- **owner** (`array`):
- **include_inactive** (`boolean`):
- **limit** (`integer`):

## brand_analytics_list_competitor_asins

Lists the company's registered competitor ASINs from brand_analytics_iceberg.competitor_asins.

Use this tool to:
- See which ASINs the company tracks as competitors
- Filter by rivalry scope: against a specific ASIN, parent, or product family
- Audit `is_active` rows before running competitive analysis

Written by brand_analytics_write_competitor_asins. Consumed by brand_analytics_growth_machine_diagnosis (flags is_competitor, competitor_won_keyword) and optionally by brand_analytics_get_competitive_landscape.

Side effect level: `read`

**Use when:**
- lists the company's registered competitor ASINs from brand_analytics_iceberg.competitor_asins

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)): Company identifier(s) — required for multi-tenant isolation.
- **marketplaces** (`array`): Optional: filter by Amazon marketplace code(s) (e.g., 'US').
- **against_my_asin** (`array`): Optional: restrict to rivalries scoped against these of my ASINs.
- **against_my_product_family** (`array`): Optional: restrict to rivalries scoped against these of my product families.
- **competitor_asins** (`array`): Optional: filter to specific competitor ASIN(s).
- **include_inactive** (`boolean`): If true, include soft-deleted rows (is_active = false).
- **limit** (`integer`):

## brand_analytics_list_ryg_thresholds

Lists the RYG (Red/Yellow/Green) signal thresholds used by Brand Analytics analysis tools. Shows both system defaults and any company-specific overrides.

Use cases:
- Inspect current threshold values before customising them
- Verify company-specific overrides are applied correctly
- Understand how strength, weakness, opportunity, and trend signals are classified

Each row defines one threshold boundary for a (tool, signal_group, metric, color) combination.
Company overrides (company_ids entry) take priority over defaults (company_id IS NULL in the DB) in analysis queries.

Side effect level: `read`

**Use when:**
- lists the RYG (Red/Yellow/Green) signal thresholds used by Brand Analytics analysis tools

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)): Company identifier(s) - required for multi-tenant isolation.
- **tool** (`string`): Optional: filter by tool name (sqp = Search Query Performance, scp = Search Catalog Performance, global = shared trend thresholds, growth_machine = prescription-rule thresholds for the Growth Machine diagnostic tool).
- **include_defaults** (`boolean`): If true (default), include system default thresholds alongside company overrides. Set false to see only company-specific overrides.

## brand_analytics_list_sqp_query_details_uploads

Lists persisted Seller Central Search Query Details uploads (screenshot extractions). Read-only.

One row per (company_id, marketplace, LOWER(keyword), period_start). Re-uploads overwrite the previous row (upsert). Use this tool to verify which keyword/period combinations already have screenshot data before asking the user for another upload.

Filters: company_ids (required), optional marketplaces, keywords (case-insensitive), uploaded_by, period range (any upload whose [period_start, period_end] overlaps the supplied window).

Side effect level: `read`

**Use when:**
- lists persisted Seller Central Search Query Details uploads (screenshot extractions)

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)):
- **marketplaces** (`array`):
- **keywords** (`array`):
- **uploaded_by** (`array`):
- **period_overlap_start** (`string`):
- **period_overlap_end** (`string`):
- **limit** (`integer`):

## brand_analytics_list_tracked_search_terms

Lists the company's tracked search terms (keyword cores) from brand_analytics_iceberg.tracked_search_terms.

Use this tool to:
- See which keywords the company is tracking for a given ASIN, parent, or product family
- Audit priority and intent assignments before running diagnostics
- Retrieve the keyword list that brand_analytics_growth_machine_diagnosis auto-scopes to when use_tracked_search_terms=true

Scope: one row per (company_id, marketplace, keyword, asin, parent_asin, product_family). Null scope means the keyword applies company-wide.

Side effect level: `read`

**Use when:**
- lists the company's tracked search terms (keyword cores) from brand_analytics_iceberg.tracked_search_terms

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- company_ids

**Required inputs:**
- - **company_ids** (`array` (required)): Company identifier(s) — required.
- **marketplaces** (`array`): Optional: filter by Amazon marketplace code(s).
- **asin** (`array`): Optional: filter to keywords scoped to these child ASINs.
- **parent_asin** (`array`): Optional: filter to keywords scoped to these parent ASINs.
- **product_family** (`array`): Optional: filter to keywords scoped to these product families.
- **keywords** (`array`): Optional: filter to specific keywords (exact match).
- **intent** (`array`): Optional: filter by intent tag.
- **include_inactive** (`boolean`): If true, include soft-deleted rows (is_active = false).
- **limit** (`integer`):

## brand_analytics_run_watchlist

Executes a saved analytics watchlist: reads its config from brand_analytics_iceberg.analytics_watchlist, expands it into the parameters you would pass to brand_analytics_growth_machine_diagnosis (grain + entity_ids + focus + cadence-derived time window), and bumps last_run_at.

In v1 this tool is MANUAL-TRIGGER ONLY (no scheduler). The caller then invokes brand_analytics_growth_machine_diagnosis with the returned `diagnosis_parameters` (or a future orchestrator can chain them automatically).

Cadence -> default time window:
- weekly    -> last 7 days
- monthly   -> last 30 days
- quarterly -> last 90 days

Upserts last_run_at to current_timestamp if dry_run=false.

Side effect level: `read`

**Required context:**
- company_id
- marketplace
- watchlist_name

**Required inputs:**
- - **company_id** (`integer` (required)):
- **marketplace** (`string` (required)):
- **watchlist_name** (`string` (required)):
- **dry_run** (`boolean`): If true (default), returns the diagnosis_parameters WITHOUT updating last_run_at.

**Clarifying questions:**
- Which value should I use for company_id?

## brand_analytics_upload_sqp_query_details

Persists Search Query Details data extracted from a Seller Central screenshot. Consequential (writes data).

Why this tool exists:
Amazon does NOT expose Search Query Details (total impressions, total clicks, click rate, per-ASIN impressions, price, and up to 10 competing ASINs) via API — only in the Seller Central UI. Other Brand Analytics tools surface a `seller_central_query_detail_url` deep link per keyword. The flow is:
  1. Call a diagnostic tool (e.g., brand_analytics_growth_machine_diagnosis) — it tells you which keywords have `screenshot_data_available = false`.
  2. Ask the user to open `seller_central_query_detail_url` in Seller Central and upload a screenshot.
  3. Your agent extracts the fields from the screenshot into the JSON payload required by this tool.
  4. Call this tool to persist. Subsequent diagnoses will read the persisted data from Iceberg instead of re-asking the user.

Upsert key: (company_id, marketplace, LOWER(keyword), period_start). Re-uploads for the same key overwrite the previous entry and bump `uploaded_at`.

Example payload (agent-extracted from screenshot):
{
  "company_id": 103,
  "reason": "User uploaded screenshot for priority Yellow keyword.",
  "extraction": {
    "marketplace": "US",
    "keyword": "rose gold ring",
    "period_start": "2026-03-01",
    "period_end": "2026-03-31",
    "total_impressions": 1200000,
    "total_clicks": 48000,
    "total_click_rate": 0.04,
    "competitors": [
      { "asin": "B0AAAA0001", "brand": "Rival A", "impressions": 180000, "clicks": 9000, "click_rate": 0.05, "price_median": 29.99, "rank": 1 },
      { "asin": "B0BBBB0002", "brand": "Rival B", "impressions": 150000, "clicks": 6500, "click_rate": 0.043, "price_median": 24.99, "rank": 2 }
    ],
    "source_screenshot_s3_uri": null
  }
}

Side effect level: `write`

**Use when:**
- persists Search Query Details data extracted from a Seller Central screenshot

**Required context:**
- company_id
- extraction.keyword
- extraction.marketplace
- extraction.period_end

**Required inputs:**
- - **company_id** (`integer` (required)):
- **reason** (`string` (required)): Audit trail (e.g., which keyword/user triggered the upload).
- **dry_run** (`boolean`):
- **extraction** (`object` (required)): Structured data extracted by the agent from the Seller Central screenshot.

**Clarifying questions:**
- Which value should I use for company_id?

## brand_analytics_write_analytics_watchlist

Upserts, deactivates, or resets the company's saved analytics watchlists. Consequential.

Upsert key: (company_id, marketplace, LOWER(watchlist_name)). Each row bundles a grain + entity_ids + cadence + focus that brand_analytics_run_watchlist expands into parameters for brand_analytics_growth_machine_diagnosis.

Actions:
- write        (default) — insert or replace watchlist rows with is_active=true
- deactivate   — same upsert but mark is_active=false (soft delete)
- reset        — deactivate ALL watchlists for the company (writes[] ignored)

Always dry_run=true by default. Pass dry_run=false to persist.

Side effect level: `read`

**Use when:**
- upserts, deactivates, or resets the company's saved analytics watchlists

**Do not use when:**
- not for read-only information requests

**Required context:**
- company_id
- reason

**Required inputs:**
- - **company_id** (`integer` (required)):
- **reason** (`string` (required)):
- **action** (`string`):
- **dry_run** (`boolean`):
- **writes** (`array`):

**Clarifying questions:**
- Which value should I use for company_id?

**Related tools:**
- get tool to confirm target state

## brand_analytics_write_competitor_asins

Writes competitor ASIN entries for a company. Consequential.

Use cases:
- Register known competitor ASINs for a brand/product family
- Update priority, label, or brand of an existing competitor
- Soft-delete (set is_active=false) via action='deactivate'
- Hard reset a scope via action='reset'

Upsert key: (company_id, marketplace, competitor_asin, against_my_asin, against_my_product_family).

Example (register competitors):
{
  "company_id": 103,
  "reason": "Seeding known competitors for product family Widgets.",
  "writes": [
    { "marketplace": "US", "competitor_asin": "B0AAAA1111", "competitor_brand": "Rival Co", "competitor_label": "Main rival", "against_my_product_family": "Widgets", "priority": 1 },
    { "marketplace": "US", "competitor_asin": "B0BBBB2222", "competitor_label": "Low-price attacker", "against_my_asin": "B0CCCC3333", "priority": 2 }
  ]
}

Side effect level: `write`

**Use when:**
- writes competitor ASIN entries for a company

**Do not use when:**
- not for read-only information requests

**Required context:**
- company_id
- reason

**Required inputs:**
- - **company_id** (`integer` (required)): Company identifier.
- **reason** (`string` (required)): Audit trail.
- **action** (`string`): write (default): upsert entries. deactivate: set is_active=false for the given slots (soft delete). reset: deactivate ALL competitor entries for this company.
- **dry_run** (`boolean`): If true, validate and preview without persisting.
- **writes** (`array`): Competitor entries to upsert or deactivate. Required when action='write' or action='deactivate'.

**Clarifying questions:**
- Which value should I use for company_id?

**Related tools:**
- get tool to confirm target state

## brand_analytics_write_ryg_thresholds

Write company-specific RYG (Red/Yellow/Green) signal threshold overrides for Brand Analytics analysis tools. Consequential (writes data).

Use cases:
- Customise strength/weakness/opportunity/trend thresholds per company
- Override system defaults with company-specific values
- Reset company overrides to fall back to system defaults

Each write creates one threshold row for a (tool, signal_group, metric, color) slot.
If a row already exists for the same company + slot, it is replaced (upsert semantics).

To reset (delete all company overrides), use action='reset'.

Available tools: sqp (Search Query Performance), scp (Search Catalog Performance), global (shared trend thresholds), growth_machine (prescription-rule thresholds for brand_analytics_growth_machine_diagnosis — signal_code names the prescription label, e.g., inject_into_seo, negative_exact, fix_cart_leak_cut_ppc, defend_organic, displace_weak_leader, evaluate_or_skip).

Example (set custom SQP strength thresholds):
{
  "company_id": 103,
  "reason": "Company 103 has higher baseline metrics; raising green bar.",
  "writes": [
    { "tool": "sqp", "signal_group": "strength", "metric": "click_share", "color": "green", "threshold_value": 0.15, "signal_code": "market_leader", "signal_description": "Top click share (>15%)." },
    { "tool": "sqp", "signal_group": "strength", "metric": "purchase_rate", "color": "green", "threshold_value": 0.12, "signal_code": "high_intent_win", "signal_description": "Elite conversion (>12%)." }
  ]
}

Example (reset all overrides for a company):
{
  "company_id": 103,
  "reason": "Reverting to system defaults.",
  "action": "reset"
}

Side effect level: `write`

**Use when:**
- write company-specific RYG (Red/Yellow/Green) signal threshold overrides for Brand Analytics analysis tools

**Do not use when:**
- not for read-only information requests

**Required context:**
- company_id
- reason

**Required inputs:**
- - **company_id** (`integer` (required)): Company identifier to set overrides for.
- **reason** (`string` (required)): Human-readable reason for the change (audit trail).
- **action** (`string`): write (default): upsert threshold overrides. reset: delete ALL company overrides (reverts to system defaults).
- **dry_run** (`boolean`): If true, validate and preview without persisting.
- **writes** (`array`): Threshold overrides to write. Required when action='write'.

**Clarifying questions:**
- Which value should I use for company_id?

**Related tools:**
- get tool to confirm target state

## brand_analytics_write_tracked_search_terms

Writes tracked search term entries for a company. Consequential.

Use cases:
- Seed the company's keyword cores per ASIN / parent / product family
- Tag keywords with intent (defend | attack | evaluate | branded) and priority
- Soft-delete entries via action='deactivate'
- Reset the full tracking set via action='reset'

Upsert key: (company_id, marketplace, LOWER(keyword), asin, parent_asin, product_family).

Example (seed a per-ASIN core and a family-wide keyword):
{
  "company_id": 103,
  "reason": "Day-1 onboarding: seed keyword cores.",
  "writes": [
    { "marketplace": "US", "asin": "B0AAAA1111", "keyword": "moisturizing face cream", "priority": 1, "intent": "defend" },
    { "marketplace": "US", "product_family": "Widgets", "keyword": "blue widget",             "priority": 2, "intent": "attack" }
  ]
}

Side effect level: `write`

**Use when:**
- writes tracked search term entries for a company

**Do not use when:**
- not for irreversible changes
- not for read-only information requests

**Required context:**
- company_id
- reason

**Required inputs:**
- - **company_id** (`integer` (required)):
- **reason** (`string` (required)):
- **action** (`string`): write (default): upsert entries. deactivate: set is_active=false for the given slots. reset: deactivate ALL tracked-term entries for this company.
- **dry_run** (`boolean`):
- **writes** (`array`):

**Clarifying questions:**
- Which value should I use for company_id?

**Related tools:**
- get tool to confirm target state

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
