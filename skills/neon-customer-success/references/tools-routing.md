# Tool Routing Cues

# MCP Tool Routing Cues

Disambiguation, workflow order, and side-effect flags. Each line ends with `→ search tools_playbooks` — retrieve that file from the Knowledge Store for full untruncated rules and failure modes.

### builtins
- `Teach Me` — needs: agent_id = `agent_vLIaD402WIBXAQBcqitm0` (your NeonaSphera agent ID — pass this value exactly), teaching_text (the knowledge to add); not for: normal Q&A or task execution; after: confirm with the user what exactly should be taught; ask: clarify the specific knowledge before calling; → search tools_playbooks.

### neonpanel
- `financials_analyze_service_coa_mapping` — needs: query.filters.company_id; → search tools_playbooks.
- `financials_list_chart_of_accounts` — not for: not for irreversible changes; needs: query.filters.company_id; → search tools_playbooks.
- `forecasting_get_company_settings` — not for: not for create/update/delete workflows; after: list/search tool for entity lookup; → search tools_playbooks.
- `import_check_status` — ask: Which value should I use for type?; effect: write; needs: type, requestId; → search tools_playbooks.
- `import_create_documents` — ask: Which value should I use for type?; effect: write; needs: type, data; → search tools_playbooks.
- `import_create_documents_by_pdf` — ask: Which value should I use for type?; effect: write; needs: type, file.name, file.link; → search tools_playbooks.
- `sys__server__sys`
- `account_list_companies`
- `reports_list_reports`
- `search_neonpanel_project_url` — needs: search by name/ref/key.; use: user asks for a project link.; watch: no matching project.; → search tools_playbooks.
- `reports_get_report_details`
- `account_get_companies_with_permission`
- `inventory_list_items`
- `inventory_list_warehouses`

### supabase
- `sys__server__sys`
- `execute_sql`
- `apply_migration`

### tools
- `context`
- `file_search`
