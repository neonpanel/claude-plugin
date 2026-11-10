---
name: neonpanel-plugin-setup
description: >
  Use this skill when the user has just installed the NeonPanel Claude plugin,
  asks how to set it up, asks what accounts or data sources to connect, wants
  to connect NeonPanel or NeonaSphera to Claude, asks about MCP setup, or needs
  help getting value from the NeonPanel expertise skills in Claude. Not for
  onboarding a NeonPanel account, importing data or creating NeonPanel
  documents (use the onboarding-data-import skill).
metadata:
  version: "1.0.0"
  source: "NeonPanel plugin setup"
---

# NeonPanel Plugin Setup

This skill covers installing and connecting the NeonPanel Claude plugin. It
takes the user from "plugin installed" to "Claude can use the right
NeonPanel and NeonaSphera context." Questions about NeonPanel itself belong
to the expertise skills listed at the end of this file.

Start with a short readiness interview. Do not dump every setup path at once
unless the user explicitly asks for a complete checklist.

## Readiness Interview

Ask the user:

1. Do you already have a NeonPanel account?
2. Do you already have a NeonaSphera account?
3. Are you trying to use Amazon seller data, Shopify data, shared workspace
   memory, file memory, or all of these?
4. Are you setting this up for yourself, or for a team/shared workspace?

## NeonPanel Path

NeonPanel is the commerce operations/data side of the plugin. It is what
powers Amazon seller workflows, Shopify context, inventory, supply chain,
forecasting, accounting, COGS, advertising, and Brand Analytics questions.

If the user does not have a NeonPanel account, direct them to:

https://my.neonpanel.com/register

If the user already has a NeonPanel account, guide them to connect the
relevant data sources before expecting Claude to answer with live data:

- Amazon integration: https://my.neonpanel.com/app/integrations/amazon
- Shopify integration, if they use Shopify: https://my.neonpanel.com/app/integrations/shopify

After account/data setup, tell them to add the NeonPanel MCP server in Claude
via Add Custom Connector:

https://mcp.neonpanel.com/mcp

## NeonaSphera Path

NeonaSphera is the shared workspace and memory side. It enables team/workspace
context, shared memory, and file-memory workflows.

If the user does not have a NeonaSphera account, direct them to:

https://my.neonasphera.com

After signup/login, tell them to create a workspace in the interface. If they
want Claude to use Shared Workspace or file memory, they should connect Claude
to that workspace through the NeonaSphera MCP server via Add Custom Connector:

https://admin.neonasphera.com/mcp

## Connector Guidance

Use "Add Custom Connector in Claude" as the reliable setup instruction. If the
user's Claude environment supports opening MCP connector links directly, they
can try the MCP URLs, but do not promise automatic installation.

## Verification Prompts

After the user connects NeonPanel MCP, suggest:

- "Check whether my NeonPanel account is connected."
- "What Amazon data can you see for my account?"
- "Which NeonPanel expertise skill should I use first?"

After the user connects NeonaSphera MCP, suggest:

- "Check whether you can access my NeonaSphera workspace."
- "What shared workspace memory is available?"
- "Help me use file memory with this workspace."

## Expertise skills in this plugin

Once setup is complete, questions about NeonPanel load one of these
expertise skills. Each one carries its own scope, boundaries and knowledge:

- `onboarding-data-import` — Onboarding & data import: NeonPanel onboarding and platform operations: connecting Amazon/QuickBooks/Xero, data imports and templates, Chart of Accounts and service mapping, dry runs, go-live readiness, import or validation errors, and project creation or editing.
- `sales-forecasting` — Sales forecasting: Forecasting and demand planning: sales forecasts, scenario comparison, forecast accuracy, statistical or seasonal forecasting, overrides, and projections.
- `replenishment-planning` — Replenishment planning: Replenishment and purchase-order planning: reorder timing and quantities, days of supply, safety stock, lead times, stockout risk, FBA replenishment, and purchase-order candidates.
- `listing-research` — Listing & marketing research: Amazon listing and marketing research: listing audits, title, bullet and image review, A+ content, compliance and quality, keyword research, competitor analysis, and listing conversion.
- `market-intelligence` — Market intelligence: Market intelligence and Brand Analytics: keyword gaps, search-term momentum, competitive landscape, click or conversion share, keyword mining, category trends, BSR, and advertising performance.
- `inventory-supply-chain` — Inventory & supply chain: Inventory operations and supply chain: stock location, warehouse balances, inbound FBA, shipments, lost batches, inventory transactions, landed-cost or COGS diagnosis, sales velocity, and arrival prediction.
- `accounting-history` — Accounting history: Historical accounting: P&L, general ledger, journal entries, Amazon statement or settlement reconciliation, revenue and COGS, inventory valuation, unit costs, and bookkeeping sync.
- `financial-planning` — Financial planning: Forward-looking finance: cash-flow planning, financial forecasts, budgets, spending strategy, profitability or margin improvement, cost optimization, and projections.
