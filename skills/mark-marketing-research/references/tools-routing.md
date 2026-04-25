# Tool Routing Cues

# MCP Tool Routing Cues

Disambiguation, workflow order, and side-effect flags. Each line ends with `→ search tools_playbooks` — retrieve that file from the Knowledge Store for full untruncated rules and failure modes.

### neonpanel
- `account_get_report_details` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: reportId; → search tools_playbooks.
- `account_list_reports` — not for: not for irreversible changes; → search tools_playbooks.
- `advertising_analyze_campaign_performance` — needs: query.filters.company_id; → search tools_playbooks.
- `advertising_analyze_search_terms` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_repeat_purchases` — effect: write; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_search_catalog_performance` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_analyze_search_query_performance` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_get_competitive_landscape` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_get_conversion_leak_analysis` — not for: not for create/update/delete workflows; ask: Which value should I use for query.filters.company_id?; after: list; → search tools_playbooks.
- `brand_analytics_get_cross_sell_opportunities` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; needs: query.filters.company_; → search tools_playbooks.
- `brand_analytics_get_keyword_funnel_metrics` — not for: not for create/update/delete workflows; ask: Which value should I use for query.filters.company_id?; after: list/s; → search tools_playbooks.
- `brand_analytics_get_search_term_momentum` — not for: not for irreversible changes; after: list/search tool for entity lookup; needs: query.filters.company_id; → search tools_playbooks.
- `brand_analytics_list_ryg_thresholds` — not for: not for irreversible changes; needs: company_id; → search tools_playbooks.
- `brand_analytics_write_ryg_thresholds` — not for: not for read-only information requests; ask: Which value should I use for company_id?; after: get tool to confirm target; → search tools_playbooks.
- `account_get_companies_with_permission`
- `account_list_companies`
