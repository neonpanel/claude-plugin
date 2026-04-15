# Tool Routing Cues

# MCP Tool Routing Cues

Disambiguation, workflow order, and side-effect flags. Each line ends with `→ search tools_playbooks` — retrieve that file from the Knowledge Store for full untruncated rules and failure modes.

### neonpanel
- `advertising_analyze_campaign_performance` — needs: query.filters.company_id; → search tools_playbooks.
- `advertising_analyze_search_terms` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_repeat_purchases` — effect: write; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_search_catalog_performance` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_search_query_performance` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_get_competitive_landscape` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_get_cross_sell_opportunities` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: query.filters.company_; → search tools_playbooks.
- `brand_analytics_get_keyword_funnel_metrics` — not for: not for create/update/delete workflows; ask: Which value should I use for query.filters.company_id?; after: list/s; → search tools_playbooks.
- `brand_analytics_get_search_term_momentum` — not for: not for irreversible changes; after: list/search tool for entity lookup; needs: query.filters.company_id; → search tools_playbooks.
- `inventory_get_details` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: inventoryId; → search tools_playbooks.
- `sys__server__sys`
- `account_list_companies`
- `reports_list_reports`
- `account_get_companies_with_permission`
- `inventory_get_listing_details_by_asin`
- `inventory_list_items`
- `reports_get_report_details`

### tools
- `context`
- `file_search`
