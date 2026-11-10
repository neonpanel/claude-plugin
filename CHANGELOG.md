# Changelog

## 5.0.0 - 2026-11-10
- The plugin now offers expertise, not a cast of specialists. One skill per
  NeonPanel expertise area, defined by `expertise_packs.json`, the same
  registry that drives NeonaSphera's expertise packs.
- Skills renamed (saved prompts that reference the old names need updating):
  `neon-customer-success` → `onboarding-data-import`,
  `kio-forecasting` → `sales-forecasting`,
  `dorian-replenishment` → `replenishment-planning`,
  `mark-marketing-research` → `listing-research`,
  `april-market-intelligence` → `market-intelligence`,
  `nina-supply-chain` → `inventory-supply-chain`,
  `marta-accounting` → `accounting-history`,
  `sanjay-financial-analyst` → `financial-planning`,
  `neonpanel-onboarding` → `neonpanel-plugin-setup`.
- Each skill's always-loaded text is its scope and boundaries, how to navigate
  its knowledge, the expertise playbook and its tools. Descriptions state the
  neighboring area a skill does not own.
- Removed from skills: the constitution, Base OS, personality and the
  specialist roster. No skill asks Claude to be a named specialist or Neona.
- Unchanged: knowledge-area topics, knowledge-base articles and images, brand
  context and tool guidance stay nested under `references/` and indexed.
- The build fails if any specialist name or handoff wording survives.

## 1.0.0 - 2026-04-15
- Initial release
- 8 specialist agents: Neon, Dorian, Nina, Mark, Marta, Kio, April, Sanjay
- MCP connection to NeonPanel API (66 tools, OAuth2)
- Domain playbooks, module maps, and canonical claims as reference content
