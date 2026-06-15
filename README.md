# NeonPanel — Claude Cowork Plugin

E-commerce intelligence suite for Amazon sellers. 8 specialist agents connected
to NeonPanel's live data via MCP.

## Agents / Skills

| Skill | Agent | Specialty |
|-------|-------|-----------|
| `neon-customer-success` | Neon | Onboarding, data import, accounting setup, go-live |
| `dorian-replenishment` | Dorian | PO planning, inventory replenishment, reorder optimization |
| `nina-supply-chain` | Nina | Supply chain analysis, logistics, inventory tracking |
| `mark-marketing-research` | Mark | Amazon listing audit, optimization, compliance |
| `marta-accounting` | Marta | P&L, general ledger, Amazon statements, reconciliation |
| `kio-forecasting` | Kio | Sales forecasting, demand planning, scenario comparison |
| `april-market-intelligence` | April | Competitive intelligence, keyword analysis, market trends |
| `sanjay-financial-analyst` | Sanjay | Cash flow planning, financial projections, spending strategy |

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

### Then set up

Connect your NeonPanel account when Claude prompts for OAuth on first tool use.
Skills activate automatically when your query matches their specialty. Ask Claude:

```text
Help me set up NeonPanel.
```

Claude will guide you through account readiness, Amazon/Shopify integrations, and
the two MCP connectors, which are set up differently:

- NeonPanel MCP (`https://mcp.neonpanel.com/mcp`) — bundled with this plugin and
  configured automatically on install.
- NeonaSphera MCP (`https://admin.neonasphera.com/mcp`) — optional; added manually
  via Add Custom Connector (the onboarding skill walks you through it) for Shared
  Workspace, memory, and file-memory workflows. It is not auto-installed.

> Note: plugins are supported in Claude Code and Cowork, but not in Claude Chat.
> To use NeonPanel from Chat (claude.ai), add the NeonPanel MCP server as a custom
> connector instead.

## How It Works

Each skill encodes the role definition, domain knowledge, routing logic, and tool
playbooks from a trained NeonPanel agent. When you ask a question, Claude matches
it to the most relevant skill and uses that agent's expertise + NeonPanel MCP tools
to answer with live data.

Skills carry reference documents (playbooks, module maps, canonical claims) that
provide detailed domain knowledge and step-by-step workflows.
