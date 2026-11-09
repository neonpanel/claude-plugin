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

## financials_analyze_general_ledger

Analyzes the General Ledger – aggregates journal entry line items (debit, credit, net) by account, account type, classification, financial statement (P&L / Balance Sheet), customer, and time period.

Use this tool to:
- View total debits, credits, and net amounts for each account in a date range
- Break down expenses or revenues by account type (e.g., 'Expense', 'Cost of Goods Sold', 'Other Income')
- Aggregate by classification (Asset, Liability, Equity, Revenue, Expense) for a high-level overview
- Split by statement (PL = Profit & Loss, BS = Balance Sheet) to separate operating vs. balance sheet accounts
- Track monthly, quarterly, or yearly trends for any account or category
- Filter by SDE / EBITDA / P&L flags to focus on seller-discretionary, EBITDA-relevant, or P&L-relevant accounts
- Search for specific accounts by name or number
- Group by report_chart to see how accounts roll up in the standard reporting chart
- Filter or group by customer (customer_names with match_type) to see GL activity tied to a specific customer

Marketplace filtering: The GL has no dedicated marketplace column. To filter by Amazon marketplace, use the customer_names filter with the marketplace domain name (e.g., customer_names=['Amazon.com'] for US, ['Amazon.co.uk'] for UK, ['Amazon.de'] for Germany). Each marketplace is recorded as a separate customer in QuickBooks.

IMPORTANT: Results are capped by the 'limit' parameter (default 100). When no account_names/account_numbers filter is set, ALL accounts are considered but only the top N by sort order are returned. To explore different accounts, either: (1) change sort field/direction, (2) use account_names filter with account_name_match_type='contains' to drill into a category (e.g., ['FBA'] to see all FBA-related accounts), or (3) use classifications/statements/account_types filters to narrow scope.

Sign convention: net = debit − credit. Expense/Asset accounts normally have net > 0 (net debit). Revenue/Liability/Equity accounts normally have net < 0 (net credit). To find top expenses, sort by net DESC. To find top revenue sources, sort by net ASC (most negative = most revenue).

Currency: All debit/credit amounts are in the company's main currency (shown in main_currency column when grouping by company).

Periodicity: Supports day, month, quarter, year, or total (no time breakdown).

Default time range: When no time parameters are provided, the tool returns data for the last complete calendar month (e.g., if today is Feb 13 2026, it returns Jan 1–31 2026). Use start_date/end_date for custom ranges, or periods_back for a rolling window.

Drill-down: Use financials_list_account_transactions to see individual journal entry lines for a specific account, including JE names, document numbers, line descriptions, and a running balance. This is useful after identifying an interesting account via this aggregation tool.

Journal entries: Use financials_list_journal_entries to browse/search journal entries by name, document type, or amount range. Use financials_get_journal_entry_details to see the full line-by-line breakdown of a specific JE.

Integration note: Cross-reference with cogs_analyze_fifo_cogs to compare GL-recorded COGS vs. FIFO unit costs, or with advertising_analyze_campaign_performance to reconcile ad spend GL entries vs. Marketing Stream data.

Dig deeper: For a full interactive General Ledger report with drill-down into individual journal entries and attachments, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/general-ledger

Knowledge base: https://help.neonpanel.com/en/articles/general-ledger-analysis

Side effect level: `read`

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for GL analysis. company_id is required; all others are optional.
- **aggregation** (`object`): Controls time window, periodicity, and grouping.
- **sort** (`object`): Controls ranking and ordering of results.
- **limit** (`integer`): Maximum rows to return (default 100, max 500). Only the top N rows by the chosen sort order are returned.

## financials_analyze_profit_and_loss

Analyzes the Profit & Loss statement by computing the full P&L waterfall from journal entry data.

The waterfall structure:
  Gross Revenue = Sales + VAT Collected
  Revenue = all Income-type accounts (includes Sales, VAT, Reimbursements, Promo Discounts, Refunds, Liquidations, and other income)
  CM1 = Revenue + Cost of Inventory Sold (product cost margin)
  CM2 = Revenue − Cost of Goods Sold (includes Amazon Fees)
  CM3 = CM2 + Amazon Promotion (contribution margin after promos)
  EBITDA = CM3 + Expenses
  Margin = EBITDA / Revenue

P&L line item definitions:
- Sales: Amazon sales (accounts 40011, 40013, 46000)
- VAT Collected: VAT on sales (account 22021)
- Reimbursements: Amazon FBA reimbursements (accounts 40015, 40103, 40105)
- Promo Discounts: Promotional discounts & coupons (accounts 40107, 40108)
- Refunds: Customer refunds (accounts 40014, 40104, 40106)
- Liquidations: Inventory liquidations (accounts 40101, 40102)
- Revenue: Total income (all Income-type accounts)
- Cost of Inventory Sold: Product cost / COGS at landed cost (parent account = 'Cost of Inventory Sold')
- Amazon Fees: Platform fees = −COGS − Cost of Inventory Sold (FBA fees, referral fees, etc.)
- Amazon Promotion: Advertising & promotional spend (parent account = 'Amazon Promotion')
- Expenses: Operating expenses minus Amazon Promotion
- Net Income: Revenue − COGS − Expenses (all types)

Sign convention: Revenue, Sales, Reimbursements are positive. Refunds, Promo Discounts, Cost of Inventory Sold, Amazon Fees, Expenses are negative. CM1/CM2/CM3/EBITDA are net results.

Margin percentages (cm1_pct, cm2_pct, cm3_pct, margin) are ratios to Revenue. NULL when Revenue = 0.

Currency: All amounts are in the company's main currency.

Periodicity: Supports month (default), quarter, year, or total (no time breakdown). Monthly is most common for P&L analysis.

Default time range: Last complete calendar month when no dates provided.

Marketplace filtering: The P&L has no dedicated marketplace column. To filter by Amazon marketplace, use customer_names with the marketplace domain name (e.g., customer_names=['Amazon.com'] for US, ['Amazon.co.uk'] for UK, ['Amazon.de'] for Germany). Each marketplace is recorded as a separate customer in QuickBooks.

NOTE: This tool uses NeonPanel's template Chart of Accounts. Results are accurate for companies using the standard CoA with the account number prefix convention above. For companies with custom CoA structures, use financials_analyze_general_ledger instead.

Drill-down: Use financials_analyze_general_ledger to explore individual accounts contributing to any P&L line. For example, to see all COGS accounts, filter by account_types=['Cost of Goods Sold']. Use financials_list_account_transactions for transaction-level detail.

Dig deeper: For a full interactive P&L report, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/profit-loss-bk

Knowledge base: https://help.neonpanel.com/en/articles/profit-and-loss-analysis

Side effect level: `read`

**Use when:**
- analyzes the Profit & Loss statement by computing the full P&L waterfall from journal entry data

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Required filters. company_id is mandatory.
- **time** (`object`): Date range. Defaults to last complete calendar month if omitted.
- **periodicity** (`string`): Time granularity for aggregation. 'month' (default) returns one row per month, 'quarter' per quarter (2025-Q1), 'year' per year, 'total' aggregates everything into one row.
- **group_by_company** (`integer`): Set to 1 to add company_id / company_name / main_currency columns (useful when comparing multiple companies, though only one company_id can be passed per request).

## financials_analyze_service_coa_mapping

Analyzes service items → Chart of Accounts mapping. Each service should be mapped to an income account via income_account_id. This tool LEFT JOINs services to accounts so both mapped and unmapped services are visible.

Use this tool to:
- List all unmapped services (missing income account assignment)
- Find which account a specific service is mapped to
- Find which services are mapped to a specific account (by number or name)
- Review mapping coverage and identify gaps
- Help recommend which account a service should be mapped to (by looking at similar services)

Filters:
- mapping_status: 'mapped' (has an income account), 'unmapped' (no income account), or omit for both
- service_name_search: substring search on service name (case-insensitive)
- account_names / account_numbers: filter by the mapped account
- active: 1 = active services only, 0 = inactive only, omit = all

Note: This tool does NOT have a date range filter – it returns the current state of service-to-account mappings.

Dig deeper: To manage service mappings, recommend the user visit https://my.neonpanel.com/app/services

Side effect level: `read`

**Use when:**
- analyzes service items → Chart of Accounts mapping

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for the analysis. company_id is required.
- **limit** (`integer`): Maximum rows to return (default 200, max 500).

## financials_classify_amazon_statement_transactions

Aggregates Amazon settlement transactions using one of two classification modes.

Two modes:

1. **default** (CoA-based) — Maps each transaction's service_name to the company's Chart of Accounts via the services table (services.name → accounts via income_account_id, with parent account chain). Returns totals grouped by account_classification (1.Revenue/2.Expense/3.Asset/…), account_type (1.Income/2.Cost of Goods Sold/3.Expense/…), and account_name. This is the standard bookkeeping view aligned with the company's actual CoA.

2. **reference** — Uses NeonPanel's built-in reference classification (settlement_flat_mapping → settlement_subclasses → settlement_classes). Returns totals grouped by class/subclass codes. This is a fixed, pre-defined classification hierarchy maintained by NeonPanel:
   - '1' = Income, '2' = Amazon Fees, '3' = Cost of Advertising, '4' = Tax, '5' = Other
   - Subclass examples: '1.01' = Product Sales, '2.01' = Referral Fees, etc.

Output: One row per classification group (optionally per marketplace or settlement), with total debit, credit, net amount, line count.

Use this tool to:
- Get a classified summary of settlement data for a given period
- Compare classified totals against an uploaded Amazon payments report (PDF)
- View income vs. fees vs. tax breakdown (reference mode) or by CoA accounts (default mode)
- Drill into account-level or subclass-level detail
- Add marketplace or settlement granularity to the breakdown

Sign convention: positive = credit to seller (income), negative = charge (fee/tax). total_debit and total_credit split debits and credits into separate unsigned totals.

Reconciliation workflow:
1. User uploads a PDF payments report → LLM extracts dates & amounts
2. LLM calls THIS tool with matching date range → gets classified totals
3. LLM compares PDF amounts vs. classified totals per category
4. LLM calls financials_save_amazon_statement_reconciliation_result to persist the comparison
5. Use financials_list_amazon_statement_reconciliation_results to review saved results later

ProTip: Use financials_list_amazon_statements first to find settlement IDs and date ranges, then filter by settlement_ids here for precise matching.

Integration:
- Cross-reference with financials_analyze_amazon_statement for raw transaction-level detail
- Use financials_analyze_service_coa_mapping to inspect or fix service → account mappings
- Use financials_list_chart_of_accounts to browse the full CoA

Default time range: Last 3 months when no dates are provided.

Knowledge base: https://help.neonpanel.com
Dig deeper: For a full interactive settlement view, direct the user to https://my.neonpanel.com

Side effect level: `read`

**Use when:**
- aggregates Amazon settlement transactions using one of two classification modes

**Required context:**
- filters.company_id

**Required inputs:**
- - **mode** (`string`): Classification mode.

- 'default': CoA-based — maps service_name → services → accounts → account_types. Groups by account_classification, account_type, account_name. Use this for bookkeeping reconciliation aligned with the company's Chart of Accounts.
- 'reference': NeonPanel's built-in reference classification (class/subclass hierarchy). Use this for standardized comparison across companies or when CoA mapping is incomplete.
- **filters** (`object` (required)): Filters for the query. company_id is required; all others are optional.
- **time** (`object`): Date range filter on transaction_date (the date each transaction was posted).
- **aggregation** (`object`): Controls aggregation granularity. Available dimensions depend on mode.
- **sort** (`object`): Controls ranking and ordering of results.
- **limit** (`integer`): Maximum rows to return (default 200, max 500).

## financials_get_bookkeeping_sync_register

Shows the bookkeeping sync register – the status of document synchronisation with QuickBooks Online (QBO) or Xero. Lists each sync entry with its current status, any sync errors, and a direct link to the document in the accounting platform.

Use this tool to:
- Check which documents have been synced to QBO/Xero and which have failed
- Find sync errors and troubleshoot failed syncs
- List all documents pending sync (status 'Ready' or 'Going')
- Look up a specific document's sync status by doc_number or QBO transaction reference
- Filter by document type (Invoice, Bill, Expense, Journal Entry, etc.)
- Get the QBO/Xero link (bk_document_url) to open the document directly in the accounting platform

Sync statuses:
- 'Off' (0): Sync disabled for this document
- 'Ready' (1): Queued for sync
- 'Going' (2): Sync in progress
- 'Synced' (3): Successfully synced to QBO/Xero
- 'Error' (4): Sync failed — check sync_error for details

Note: The table name 'qbo_register' is a legacy name from when only QuickBooks was supported. It now covers both QBO and Xero.

Default time range: Last complete calendar month. Use start_date/end_date for custom ranges.

Default sort: Most recent first (desc).

NeonPanel link builder: Build a direct link to the sync register in NeonPanel using this pattern: https://my.neonpanel.com/app/integrations/bookkeeping/registers?date%5B0%5D={start_date}to{end_date}&date%255B0%255D={start_date}to{end_date}&search={doc_number_or_ref}. The date parameter format is YYYY-MM-DDtoYYYY-MM-DD. Both date params are required (one single-encoded, one double-encoded). The search parameter accepts doc_number or qbo_transaction_ref. Example: https://my.neonpanel.com/app/integrations/bookkeeping/registers?date%5B0%5D=2026-01-01to2026-02-13&date%255B0%255D=2026-01-01to2026-02-13&search=12248784773-02-AAU

Related tools:
- financials_list_journal_entries: Browse/search journal entry headers
- financials_get_journal_entry_details: See line-by-line breakdown of specific JEs
- financials_list_account_transactions: See JE lines posted to a specific account

Dig deeper: For the full interactive General Ledger report with drill-down, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/general-ledger

Knowledge base: https://help.neonpanel.com/en/articles/bookkeeping-sync

Side effect level: `read`

**Use when:**
- shows the bookkeeping sync register – the status of document synchronisation with QuickBooks Online (QBO) or Xero

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for sync register. company_id is required; all others are optional.
- **time** (`object`): Date range for transaction_date. Defaults to last complete calendar month if omitted.
- **sort_direction** (`string`): Sort order by transaction_date. 'desc' (default) = most recent first.
- **limit** (`integer`): Max rows to return. Default 100.

**Related tools:**
- list/search tool for entity lookup

## financials_get_journal_entry_details

Gets the full line-by-line breakdown of specific journal entries identified by doc_number (reference number). Returns every account line with debit, credit, net, account info (type, classification, statement, report_chart), line description, and attachment filenames.

Use this tool to:
- Drill into a specific journal entry to see all debit/credit lines and which accounts are affected
- Verify that a JE is balanced (total debit == total credit)
- See attached documents (original filenames) for a JE
- Inspect the account breakdown of an Amazon invoice, bill, expense, or manual JE
- Follow up from financials_list_journal_entries by passing doc_numbers from that tool's output

Input: Provide 1–20 doc_numbers. No date range needed — the tool finds JEs by doc_number directly.

Output: One row per JED line, grouped by doc_number and ordered by line_number. JE header fields (je_name, je_message, je_amount, je_currency, source/target_doc_type) are repeated on each line for convenience.

Workflow: Use financials_list_journal_entries to find JEs → take doc_numbers → pass them here to see full details.

Aggregation: Use financials_analyze_general_ledger for aggregated analysis by account or time period.

Account ledger: Use financials_list_account_transactions to see all JE lines posted to a specific account with running balance.

Dig deeper: For a full interactive General Ledger report with drill-down, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/general-ledger

Knowledge base: https://help.neonpanel.com/en/articles/general-ledger-analysis

Side effect level: `read`

**Use when:**
- gets the full line-by-line breakdown of specific journal entries identified by doc_number (reference number)

**Do not use when:**
- not for create/update/delete workflows

**Required context:**
- filters.company_id
- filters.doc_numbers

**Required inputs:**
- - **filters** (`object` (required)): Filters for JE detail lookup. Both company_id and doc_numbers are required.

**Clarifying questions:**
- Which value should I use for filters.company_id?

**Related tools:**
- list/search tool for entity lookup

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

## financials_list_account_transactions

Lists individual journal entry lines for specific accounts – the detailed account ledger / account analysis view. Returns every transaction with JE name, ref number (doc_number), date, debit, credit, line description, and a running balance per account.

Use this tool to:
- View all transactions posted to a specific account (by number or name)
- See JE names (e.g., 'Amazon.com', 'Amazon.ca') and ref numbers (doc_number) for each transaction
- Get a running balance for an account over a date range
- Search across line descriptions (e.g., find all lines mentioning a specific invoice number)
- Look up a specific journal entry by doc_number
- Drill down from financials_analyze_general_ledger results to see the individual transactions behind an aggregate

IMPORTANT: Use at least one account filter (account_numbers, account_names, account_types, or classifications) to get targeted results. Without any account filter, ALL accounts are included and the 200-row limit may truncate results. For a single account analysis, use account_numbers with the exact account number.

Default time range: Last complete calendar month. Use start_date/end_date for custom ranges.

Currency: All amounts are in the company's main currency.

Journal entry details: Use financials_get_journal_entry_details with doc_numbers from this tool's output to see the full line-by-line breakdown of a specific JE across all accounts (not just the account you're viewing).

JE listing: Use financials_list_journal_entries to browse/search journal entries by name, document type, or amount range.

Dig deeper: For the full interactive account ledger with attachments and drill-down, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/general-ledger

Knowledge base: https://help.neonpanel.com/en/articles/general-ledger-analysis

Side effect level: `read`

**Use when:**
- lists individual journal entry lines for specific accounts – the detailed account ledger / account analysis view

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for account transactions. company_id is required. Use at least one account filter for targeted results.
- **time** (`object`): Date range. Defaults to last complete calendar month if omitted.
- **sort_direction** (`string`): Sort by transaction_date: 'asc' = oldest first (chronological, default), 'desc' = newest first.
- **limit** (`integer`): Maximum rows to return (default 200, max 500).

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

## financials_list_chart_of_accounts

Lists the Chart of Accounts (COA) for a company – the master account register. Returns each account with its number, name, type, classification, statement assignment, hierarchy (parent/grandparent), breadcrumb path, and reporting flags.

Use this tool to:
- Browse the full chart of accounts
- Look up a specific account by number or name
- Filter accounts by type (e.g., 'Income', 'Expense', 'Cost of Goods Sold', 'Bank')
- Filter by classification (Asset, Liability, Equity, Revenue, Expense)
- Filter by financial statement (PL or BS)
- See the account hierarchy (parent → grandparent → account path)
- Identify which accounts are SDE / EBITDA / P&L relevant
- Find active vs. inactive accounts

This is a reference-data tool – no date range is needed. It returns the current state of the COA.

account_type comes from the account_types table (the authoritative source for type names and classifications).

Dig deeper: For the full interactive COA management, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/chart-of-accounts

Knowledge base: https://help.neonpanel.com/en/articles/chart-of-accounts

Side effect level: `read`

**Use when:**
- lists the Chart of Accounts (COA) for a company – the master account register

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for the COA. company_id is required.
- **limit** (`integer`): Maximum rows to return (default 200, max 500).

## financials_list_journal_entries

Lists journal entry headers with summary statistics – total debit, total credit, net, line count, and attachment count per journal entry.

Use this tool to:
- Browse journal entries for a company in a date range
- Search for journal entries by name (e.g., 'Amazon.com', 'FBA', 'Advertising')
- Look up specific journal entries by doc_number (reference number)
- Filter by document type (Invoice, Bill, Expense, Manual Journal Entry, etc.)
- Find large or unusual journal entries by filtering on amount range
- See how many line items and attachments each JE has

IMPORTANT: Results are capped by the 'limit' parameter (default 100). For broad searches, use filters to narrow scope.

Default sort: Most recent first (desc). Change to 'asc' for chronological order.

Default time range: When no time parameters are provided, returns data for the last complete calendar month.

Amount: The 'amount' and 'main_amount' fields come from the journal_entries header. Use total_debit / total_credit / total_net for the sum of all JED lines.

Common source_doc_type values: 'Invoice', 'Bill', 'Expense', 'Journal Entry', 'Transfer', 'Deposit', 'Payment', 'Credit Memo', 'Vendor Credit', 'Sales Receipt'.

Drill-down: Use financials_get_journal_entry_details with doc_numbers from this tool's output to see the full line-by-line breakdown with account information.

Aggregation: Use financials_analyze_general_ledger for aggregated analysis by account, account type, classification, or time period.

Account ledger: Use financials_list_account_transactions to see all JE lines posted to a specific account with running balance.

Dig deeper: For a full interactive General Ledger report with drill-down, recommend the user visit https://my.neonpanel.com/app/reports/bookkeeping/general-ledger

Knowledge base: https://help.neonpanel.com/en/articles/general-ledger-analysis

Side effect level: `read`

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **filters** (`object` (required)): Filters for journal entries. company_id is required; all others are optional.
- **time** (`object`): Date range for transaction_date. Defaults to last complete calendar month if omitted.
- **sort_direction** (`string`): Sort order by transaction_date. 'desc' (default) = most recent first.
- **limit** (`integer`): Max rows to return. Default 100.

## financials_list_unmapped_statement_transactions

Diagnostic tool that lists Amazon settlement transaction combos that lack classification mapping.

Two modes:

1. **reference** — Shows transaction_type + amount_type + amount_description combos that have NO matching rule in settlement_flat_mapping. These are the rows that get subclass '9.99' (Unclassified) in the classify tool's reference mode. Use this to identify which mapping rules need to be added to settlement_flat_mapping.

2. **coa** (default) — Shows service_name values that have NO matching service → account link in the company's Chart of Accounts. These are the rows that appear as 'unmapped' in the classify tool's default (CoA) mode. Use this to identify which services need to be linked to an account.

Workflow:
1. Run classify tool → notice unmapped amounts are significant
2. Call THIS tool to get the specific unmapped combos or service names
3. Fix the mappings:
   - For reference mode gaps → add rules to settlement_flat_mapping
   - For CoA mode gaps → use financials_analyze_service_coa_mapping to link services to accounts
4. Re-run classify tool to verify everything is now mapped

Default time range: Last 3 months when no dates are provided.

Side effect level: `read`

**Use when:**
- diagnostic tool that lists Amazon settlement transaction combos that lack classification mapping

**Do not use when:**
- not for irreversible changes
- not for create/update/delete workflows

**Required context:**
- filters.company_id

**Required inputs:**
- - **mode** (`string`): Which mapping layer to check.

- 'coa' (default): Lists service_name values with no service → account mapping in the company's CoA.
- 'reference': Lists transaction_type + amount_type + amount_description combos with no settlement_flat_mapping rule.
- **filters** (`object` (required)): Filters. company_id is required.
- **time** (`object`): Date range filter on posted transaction date.
- **sort** (`object`): Controls ordering.
- **limit** (`integer`): Maximum rows to return (default 200, max 500).
