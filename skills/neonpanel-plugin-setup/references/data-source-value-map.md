# Data Source Value Map

This map explains why each account, integration, and connector matters.

## NeonPanel Account

A NeonPanel account gives Claude access to commerce operations workflows
through the NeonPanel MCP server.

## Amazon Integration

Connect Amazon here:

https://my.neonpanel.com/app/integrations/amazon

Enables:

- Catalog and ASIN context.
- Seller operations workflows.
- Inventory and replenishment analysis.
- Brand Analytics and keyword workflows where available.
- Advertising and search-term analysis where available.
- Financial, COGS, and margin workflows when supporting data is present.

Most impacted skills:

- `sales-forecasting` — Sales forecasting: Forecasting and demand planning: sales forecasts, scenario comparison, forecast accuracy, statistical or seasonal forecasting, overrides, and projections.
- `replenishment-planning` — Replenishment planning: Replenishment and purchase-order planning: reorder timing and quantities, days of supply, safety stock, lead times, stockout risk, FBA replenishment, and purchase-order candidates.
- `listing-research` — Listing & marketing research: Amazon listing and marketing research: listing audits, title, bullet and image review, A+ content, compliance and quality, keyword research, competitor analysis, and listing conversion.
- `market-intelligence` — Market intelligence: Market intelligence and Brand Analytics: keyword gaps, search-term momentum, competitive landscape, click or conversion share, keyword mining, category trends, BSR, and advertising performance.
- `inventory-supply-chain` — Inventory & supply chain: Inventory operations and supply chain: stock location, warehouse balances, inbound FBA, shipments, lost batches, inventory transactions, landed-cost or COGS diagnosis, sales velocity, and arrival prediction.
- `accounting-history` — Accounting history: Historical accounting: P&L, general ledger, journal entries, Amazon statement or settlement reconciliation, revenue and COGS, inventory valuation, unit costs, and bookkeeping sync.
- `financial-planning` — Financial planning: Forward-looking finance: cash-flow planning, financial forecasts, budgets, spending strategy, profitability or margin improvement, cost optimization, and projections.

## Shopify Integration

Connect Shopify here if applicable:

https://my.neonpanel.com/app/integrations/shopify

Enables:

- DTC/channel context.
- Customer/order context where available.
- Better cross-channel operational analysis.

## NeonaSphera Account And Workspace

Create or access NeonaSphera here:

https://my.neonasphera.com

Create a workspace in the interface, then connect:

https://admin.neonasphera.com/mcp

Enables:

- Shared Workspace collaboration.
- Workspace memory.
- File-memory workflows.
- Team context that Claude can use across sessions where authorized.

## Recommended Setup Order

1. Create/log into NeonPanel.
2. Connect Amazon.
3. Connect Shopify if relevant.
4. Add the NeonPanel MCP server in Claude.
5. Create/log into NeonaSphera if Shared Workspace or file memory matters.
6. Create a workspace.
7. Add the NeonaSphera MCP server in Claude.
8. Ask: "Help me decide which NeonPanel expertise skill to use first."
