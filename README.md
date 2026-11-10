# NeonPanel — Claude Cowork Plugin

E-commerce intelligence for Amazon sellers: one skill per NeonPanel expertise
area, connected to NeonPanel's live data via MCP.

## Expertise skills

| Skill | Expertise | Covers |
|-------|-----------|--------|
| `onboarding-data-import` | Onboarding & data import | NeonPanel onboarding and platform operations: connecting Amazon/QuickBooks/Xero, data imports and templates, Chart of Accounts and service mapping, dry runs, go-live readiness, import or validation errors, and project creation or editing |
| `sales-forecasting` | Sales forecasting | Forecasting and demand planning: sales forecasts, scenario comparison, forecast accuracy, statistical or seasonal forecasting, overrides, and projections |
| `replenishment-planning` | Replenishment planning | Replenishment and purchase-order planning: reorder timing and quantities, days of supply, safety stock, lead times, stockout risk, FBA replenishment, and purchase-order candidates |
| `listing-research` | Listing & marketing research | Amazon listing and marketing research: listing audits, title, bullet and image review, A+ content, compliance and quality, keyword research, competitor analysis, and listing conversion |
| `market-intelligence` | Market intelligence | Market intelligence and Brand Analytics: keyword gaps, search-term momentum, competitive landscape, click or conversion share, keyword mining, category trends, BSR, and advertising performance |
| `inventory-supply-chain` | Inventory & supply chain | Inventory operations and supply chain: stock location, warehouse balances, inbound FBA, shipments, lost batches, inventory transactions, landed-cost or COGS diagnosis, sales velocity, and arrival prediction |
| `accounting-history` | Accounting history | Historical accounting: P&L, general ledger, journal entries, Amazon statement or settlement reconciliation, revenue and COGS, inventory valuation, unit costs, and bookkeeping sync |
| `financial-planning` | Financial planning | Forward-looking finance: cash-flow planning, financial forecasts, budgets, spending strategy, profitability or margin improvement, cost optimization, and projections |
| `neonpanel-plugin-setup` | Plugin setup | Installing the plugin and connecting the NeonPanel and NeonaSphera MCP servers |

## MCP Connection

Connected to NeonPanel's MCP server at `https://mcp.neonpanel.com/mcp` (OAuth2).
Provides 66 tools across: brand analytics, financials, forecasting, supply chain,
inventory, COGS, advertising, imports, Shopify, and account management.

## Install

This plugin is available three ways. All of them install the same package.

### Claude Code (CLI or the Code tab in Desktop)

Install from the public community marketplace:

```shell
/plugin marketplace add anthropics/claude-plugins-community
/plugin install neonpanel@claude-community
```

Or from this repository directly (branded marketplace):

```shell
/plugin marketplace add neonpanel/claude-plugin
/plugin install neonpanel@neonpanel
```

### Claude Cowork (Desktop)

Cowork has its own plugin manager and does not read Claude Code installs.
Open **Customize → Plugins → Add marketplace**, enter `neonpanel/claude-plugin`
(or `anthropics/claude-plugins-community`), then install **neonpanel**.

### For organizations (admins)

Claude's org marketplace ("Sync from GitHub") only accepts private/internal
repositories, so the plugin is also mirrored to a private repo. In Claude
Desktop org settings → **Plugins → Create marketplace → Sync from GitHub**,
select `neonpanel/claude-plugin-private`, enable **Sync automatically**, and
set **Default access**. Members then get the plugin (and every update) without
adding a marketplace themselves. The private mirror is published from the same
release as the public repo, so both always carry the same version.

### Then set up

Connect your NeonPanel account when Claude prompts for OAuth on first tool use.
Skills load automatically when your question matches an expertise area. Ask Claude:

```text
Help me set up NeonPanel.
```

Claude will guide you through account readiness, Amazon/Shopify integrations, and
the two MCP connectors, which are set up differently:

- NeonPanel MCP (`https://mcp.neonpanel.com/mcp`) — bundled with this plugin and
  configured automatically on install.
- NeonaSphera MCP (`https://admin.neonasphera.com/mcp`) — optional; added manually
  via Add Custom Connector (the setup skill walks you through it) for Shared
  Workspace, memory, and file-memory workflows. It is not auto-installed.

> Note: plugins are supported in Claude Code and Cowork, but not in Claude Chat.
> To use NeonPanel from Chat (claude.ai), add the NeonPanel MCP server as a custom
> connector instead.

## How It Works

Each skill is one NeonPanel expertise area: its scope and boundaries, an operating
playbook, and the knowledge-base articles, topics and tool guidance for that area.
When you ask a question, Claude loads the matching expertise and uses its knowledge
plus the NeonPanel MCP tools to answer with live data. Claude stays Claude; the
skills add expertise, not a persona.

Detailed knowledge lives in each skill's `references/` folder and is indexed in
`references/index.md`, so only what a question needs is read.
