# Po Planning Workflow — Knowledge

## Po Data Validation

**How to help**

- Check company exists and user has access
- Check SKU/ASIN exists in the specified company and marketplace
- Check forecast scenario exists and is assigned to the SKU
- Check lead time and safety stock are configured (SKU-level or company default)
- Report validation results with status indicators: ✅ confirmed, ❌ missing/failed, ⚠️ using defaults
- If any check fails, stop and report — do not proceed to tool execution

**Don't claim**

- Do not skip validation and go straight to analysis tools
- Do not assume configuration exists without checking
- Do not proceed with ❌ items unresolved

**Escalate when**

- SKU exists in one system but not another (data integrity issue)
- No lead time set AND no company default exists
- Forecast scenario referenced by user doesn't exist in NeonPanel

## Phase 1 of PO planning: collecting all required inputs (identifier type, forecast scenario, planning parameters, scope) in a single structured request before making any tool calls

**What it is**

Phase 1 of PO planning: collecting all required inputs (identifier type, forecast scenario, planning parameters, scope) in a single structured request before making any tool calls.

**Key objects**

- Identifier (parent ASIN, child ASIN, SKU, product family)
- Forecast scenario
- Lead time days
- Safety stock days
- PO cadence
- Planning horizon
- Marketplace
- SKU scope

**How to help**

- Plan POs for company X
- Let's do replenishment planning
- I need to place a purchase order

**Clarifying questions**

- Assuming a parent ASIN is a single SKU
- Not specifying which forecast scenario to use
- Expecting the agent to know default parameters without checking

## Phase 2: Data Validation

**Procedure**

1. Once inputs are confirmed, validate each before running analysis tools.
2. Check company exists and user has access.
3. **If user provided a parent ASIN**: Trust their input. Do NOT call `neonpanel_listInventoryItems` to validate it. Go directly to the supply chain tools in Phase 3 — they accept `parent_asin` as a parameter and resolve all children automatically.
4. **If user provided a SKU or child ASIN**: Confirm it exists using `neonpanel_listInventoryItems`.
5. Check forecast scenario assignment using `supply_chain_inspect_inventory_sku_snapshot`.
6. Check lead time and safety stock using `supply_chain_list_product_logistics_parameters`.
7. **Two-step family completeness check**: After the initial parent ASIN query returns results, identify the `product_family` value from those results (e.g., "Bumpers - Mix"). Re-query by `product_family` + `marketplace` to catch any orphaned variants whose `parent_asin` field is null or mismatched. If the product_family query returns more SKUs than the parent ASIN query, flag the extras as data integrity issues (missing parent ASIN mapping).
8. Report findings with clear status indicators:
   - ✅ Found 3 SKUs under parent ASIN B0D45BYWQR
   - ⚠️ Found 2 additional SKUs via product family "Bumpers - Mix" with null parent ASIN — flagged for catalog fix
   - ❌ Scenario "promo_shift_128_growth_v2" not assigned to SKU VLX-SLCNPDS-104-MIX-VLX
   - ⚠️ Lead time not set; using company default (30 days)
9. If any check returns ❌, STOP and report the issue. Do not proceed to tool execution with unresolved failures.
10. For multi-SKU families, validate each SKU independently.

## Phase 5: Forecast Scenario Mismatch Handling

**Procedure**

1. When the user requests a specific scenario but the SKU is using a different one, report the mismatch immediately:

```
🔄 FORECAST SCENARIO MISMATCH DETECTED

Current scenario:   [Current scenario name] (scenario [ID])
Requested scenario: [Requested scenario name]

Impact on PO planning:
- Current forecast: [X] units in [month] → PO of ~[Y] units
- Requested scenario: [UNKNOWN — need to apply it first]
```

2. Provide step-by-step NeonPanel instructions:
   - Go to NeonPanel → Inventory Planning
   - Select SKU [SKU code]
   - Change forecast scenario to [requested scenario]
   - Save and refresh
3. Ask: "Want me to wait for you to apply the scenario, or proceed with the current forecast?"
4. If user chooses to wait, confirm when they're ready and re-run from Phase 3 (tool execution).
5. If user chooses to proceed, clearly disclose which scenario is being used in the result presentation.

## Po Planning Intake

**Clarifying questions**

- Is that identifier a parent ASIN, child ASIN, SKU, or product family code?
- Which forecast scenario should I use? (e.g., promo_shift_128_growth_v2, default, or current?)
- Should I use company default lead time and safety stock, or do you have specific values?
- Planning horizon — 30, 60, or 90 days?
- All SKUs in this family, or specific ones?

**How to help**

- Collect all four input categories in a single structured request: identifier, scenario, parameters, scope
- Confirm marketplace (US, UK, etc.)
- Do NOT make any tool calls until all inputs are confirmed
- Proceed to data validation phase once inputs are collected

**Don't claim**

- Do not assume a parent ASIN maps to a single SKU
- Do not guess the forecast scenario — always ask
- Do not skip intake and go straight to tool calls
- Do not spread clarifications across multiple exchanges — ask everything in one request

**Escalate when**

- User cannot provide a product identifier
- User is unsure which forecast scenario exists

## Phase 1: Intake & Clarification

**Procedure**

1. When a user says "plan POs for company X, marketplace Y, product Z", immediately ask for ALL of the following in a single structured request:
2. **Identifier clarity**: Is the identifier a parent ASIN, child ASIN, SKU, or product family code? If parent ASIN, confirm it — do NOT ask for child ASINs (the supply chain tools resolve children automatically).
3. **Forecast scenario**: Which scenario to use? (e.g., "promo_shift_128_growth_v2", default, or current?) Is it already created in NeonPanel?
4. **Planning parameters**: Lead time days, safety stock days, PO cadence, planning horizon (30/60/90 days) — or confirm using company defaults.
5. **Scope**: All SKUs in this family, or specific ones? Which marketplace?
6. Do NOT make any tool calls until all four categories are confirmed.
7. If the user provides partial info, ask for the remaining items in one follow-up — never spread across multiple exchanges.

## Phase 6 of PO planning: detecting and handling forecast scenario mismatches — when the user requests a specific scenario but the SKU is assigned a different one

**What it is**

Phase 6 of PO planning: detecting and handling forecast scenario mismatches — when the user requests a specific scenario but the SKU is assigned a different one.

**Key objects**

- Current scenario
- Requested scenario
- Impact analysis
- NeonPanel scenario change steps
- Re-run trigger

**How to help**

- Use this forecast scenario
- Why is it using the wrong scenario?
- Switch to promo scenario

**Clarifying questions**

- Thinking the agent can change scenarios directly
- Ignoring the mismatch and proceeding with wrong scenario
- Not explaining the impact difference between scenarios

## Verified Facts

**Facts**

- PO planning requires four validated inputs before any tool calls: identifier type, forecast scenario, planning parameters (lead time, safety stock, PO cadence, horizon), and scope.
- All intake clarifications must be collected in a single structured request — not spread across multiple back-and-forth exchanges.
- The correct tool sequence for PO planning is: (1) neonpanel_listInventoryItems, (2) supply_chain_inspect_inventory_sku_snapshot, (3) supply_chain_list_product_logistics_parameters, (4) supply_chain_list_po_placement_candidates.
- When a tool returns empty results, never re-run the same tool with the same parameters. Switch to the Empty Results Diagnosis skill to identify the root cause.
- PO recommendations must include five sections: Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps.
- Days of supply must be calculated two ways: FBA-only (available stock ÷ daily velocity) and with inbound (available + inbound ÷ daily velocity).
- Safety stock calculations use class-based multipliers (e.g., Class A items use a 1.2x multiplier on base safety stock days).
- When presenting PO recommendations, always state a clear recommendation with rationale. Never end with 'Option 1 or Option 2?' without providing a recommended choice.
- The agent cannot change forecast scenarios directly — scenario changes must be made by the user in NeonPanel.
- A forecast scenario mismatch must be reported with impact analysis before proceeding. The user must choose: wait to apply the correct scenario, or proceed with the current one.
- Data validation results must use clear status indicators: ✅ for confirmed, ❌ for missing/failed, ⚠️ for using defaults or requiring attention.
- Escalation is required when: data exists in one system but not another, no lead time AND no company default exists, the same tool query returns different results on retry, or multiple valid PO quantities exist with significantly different trade-offs.
- PO cadence defines how frequently purchase orders are placed and affects recommended PO timing and quantity calculations.
- When multiple SKUs exist under a parent ASIN, each SKU must be validated independently — they may have different forecast scenarios, lead times, and safety stock configurations.
- When the user provides a parent ASIN, trust their input immediately — do not run validation calls to neonpanel_listInventoryItems to confirm it. Go straight to the supply chain analysis tools.
- The supply chain tools (supply_chain_analyze_sales_velocity, supply_chain_list_po_placement_candidates, supply_chain_inspect_inventory_sku_snapshot) are parent-ASIN-aware. Pass the parent ASIN directly using the parent_asin parameter — these tools return all child SKUs automatically.
- Never pre-resolve a parent ASIN to child ASINs manually. Let the supply chain tools handle parent-to-child resolution — that is what they are built for.
- When the user provides an additional ASIN mid-conversation, immediately add it to the parent_asin filter array — do not treat it as a separate lookup requiring new validation.
- Product family (product_family field) is more reliable than parent ASIN for finding all variants because: (1) it is manually curated by the team, not dependent on Amazon catalog sync, (2) parent ASIN mappings can be stale, null, or missing for new variants, and (3) product family catches cross-parent items if ASINs are split or merged.
- When analyzing a product family, always use the two-step lookup: first query by parent_asin to find initial items, then identify the product_family from those results and re-query by product_family to catch any orphaned variants with null or mismatched parent ASIN mappings.
- Any variant found via product_family lookup that has a null or mismatched parent_asin should be flagged to the user as a data integrity issue requiring catalog correction.
- When a parent ASIN query returns fewer SKUs than expected, always cross-check by product_family before concluding the family is complete.

## Phase 3 of PO planning: executing tools in the correct order — (1) listInventoryItems, (2) inspect_inventory_sku_snapshot, (3) list_product_logistics_parameters, (4) list_po_placement_candidates

**What it is**

Phase 3 of PO planning: executing tools in the correct order — (1) listInventoryItems, (2) inspect_inventory_sku_snapshot, (3) list_product_logistics_parameters, (4) list_po_placement_candidates.

**Key objects**

- neonpanel_listInventoryItems
- supply_chain_inspect_inventory_sku_snapshot
- supply_chain_list_product_logistics_parameters
- supply_chain_list_po_placement_candidates

**How to help**

- Run the PO analysis
- Get PO recommendations
- What tools do I need for planning?

**Clarifying questions**

- Running PO placement before getting inventory snapshot
- Skipping logistics parameters check
- Re-running the same tool with same params when it returns empty

## Phase 7 of PO planning: knowing when to escalate — data integrity issues, missing critical configuration, unexpected tool results, or user decisions required

**What it is**

Phase 7 of PO planning: knowing when to escalate — data integrity issues, missing critical configuration, unexpected tool results, or user decisions required.

**Key objects**

- Data integrity issue
- Missing critical config
- Unexpected tool results
- User decision required
- Escalation summary format

**How to help**

- Something isn't working
- The data looks wrong
- I need help deciding

**Clarifying questions**

- Asking vague follow-ups instead of escalating
- Not summarizing findings before escalating
- Escalating too early without running diagnostics

## Po Escalation

**Clarifying questions**

- Can you describe what you're seeing that looks wrong?

**How to help**

- Summarize all findings collected so far
- Explain the specific gap or issue clearly
- Present options with trade-offs: 'This requires a decision from you. Should I: A) [Option with trade-offs] B) [Option with trade-offs] Or escalate to [specialist]?'
- If data integrity issue: document which system has the data and which doesn't
- If tool failure: document the tool, parameters, and error

**Don't claim**

- Do not ask vague follow-up questions instead of escalating
- Do not escalate without first summarizing what was tried and found
- Do not escalate too early — run the diagnostic tree first

**Escalate when**

- Data exists in one system but not another
- Tool returns error 3+ times on same query
- User explicitly requests human help

## Phase 3: Tool Sequencing

**Procedure**

1. Execute tools in this order — adapting based on identifier type:

**If parent ASIN provided (preferred path):**
   - Step 1: `supply_chain_inspect_inventory_sku_snapshot` with `parent_asin: ["B0D45BYWQR"]` — Returns all child SKUs with inventory + forecast data
   - Step 1b: Identify `product_family` from Step 1 results. Re-query with `product_family` filter to catch orphaned variants with null parent ASIN. Merge results.
   - Step 2: `supply_chain_analyze_sales_velocity` with `parent_asin: ["B0D45BYWQR"]` — Returns velocity data for all children
   - Step 3: `supply_chain_list_product_logistics_parameters` — Get lead time, MOQ, vendor info per SKU
   - Step 4: `supply_chain_list_po_placement_candidates` with `parent_asin: ["B0D45BYWQR"]` — Get PO recommendations for all children

**If SKU/child ASIN provided:**
   - Step 1: `neonpanel_listInventoryItems` — Confirm SKU exists
   - Step 2: `supply_chain_inspect_inventory_sku_snapshot` — Get inventory + forecast
   - Step 3: `supply_chain_list_product_logistics_parameters` — Get lead time, MOQ, vendor info
   - Step 4: `supply_chain_list_po_placement_candidates` — Get PO recommendations

2. **When user provides an additional ASIN mid-conversation**: Add it to the `parent_asin` array immediately (e.g., `parent_asin: ["B0D45BYWQR", "B0FGQH688S"]`). Do NOT treat it as a separate lookup.
3. If Step 4 returns empty results, do NOT re-run it. Switch to the **Empty Results Diagnosis** skill.
4. Collect results from all steps before presenting to the user.
5. If any step fails with an error (not empty, but error), retry once. If it fails again, escalate.

## Phase 4: Result Presentation

**Procedure**

1. Present results in exactly 5 sections using this template:

```
📊 PO PLANNING ANALYSIS: [SKU] ([Child ASIN])
Company [ID] | [Marketplace] Market | Parent ASIN: [Parent]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1️⃣ CURRENT INVENTORY (as of [date])
   Available (FBA):        [X] units
   Inbound (FBA):          [X] units
   Warehouse stock:        [X] units
   WIP orders:             [X] units ([PO reference])
   ─────────────────────────────────
   Total coverage:         [X] units

2️⃣ SALES FORECAST (Using: [scenario name])
   Current velocity:       [X] u/day ([period] avg)
   Peak velocity:          [X] u/day ([month])
   Next 30 days:          ~[X] units
   Next 60 days:          ~[X] units

3️⃣ RISK ASSESSMENT
   [⚠️/✅] Days of supply (FBA only):     [X] days
   [⚠️/✅] Days of supply (with inbound): [X] days
   Status: [CRITICAL/AT RISK/HEALTHY] ([explanation])

4️⃣ PO RECOMMENDATION
   Recommended quantity:   [X]–[Y] units
   Recommended timing:     Place by [date] ([lead time]-day lead time)
   Arrival window:         [date range]
   Rationale:
   - [Demand coverage explanation]
   - [Safety stock calculation with class multiplier]
   - [PO cadence alignment]

5️⃣ NEXT STEPS
   ☐ [Action item 1]
   ☐ [Action item 2]
   ☐ [Action item 3]
   ☐ (Optional) [Action item 4]
```

2. Always state a clear recommendation — never end with "Option 1 or Option 2?" without recommending one.
3. Calculate days of supply two ways: FBA-only AND with all inbound.
4. Include the safety stock class multiplier in the rationale (e.g., "Class A: 1.2x").
5. Show every product variant individually in its own row. Never collapse SKUs into combined columns or "other" buckets without explicit user permission.
6. State how many variants were found (including any discovered via product family cross-check) and ask the user to confirm the count before proceeding.

## Po Scenario Mismatch

**Clarifying questions**

- Do you want me to proceed with the current scenario, or wait for you to apply the requested one?

**How to help**

- Report the mismatch clearly: current scenario vs. requested scenario
- Explain the potential impact on PO recommendations
- Provide step-by-step NeonPanel instructions to change the scenario: Go to Inventory Planning → Select SKU → Change forecast scenario → Save and refresh
- Offer two paths: wait for user to apply scenario then re-run, or proceed with current scenario
- If user chooses to wait, confirm when they're ready and re-run from tool execution phase

**Don't claim**

- Do not change forecast scenarios directly — the agent cannot do this
- Do not ignore the mismatch and proceed silently with the wrong scenario
- Do not present results without disclosing which scenario was actually used

**Escalate when**

- Forecast scenario referenced by user doesn't exist in NeonPanel at all

## Po Result Presentation

**How to help**

- Present results in exactly 5 sections: (1) Current Inventory — available FBA, inbound, warehouse stock, WIP orders, total coverage; (2) Sales Forecast — current velocity, peak velocity, 30/60 day projections, scenario used; (3) Risk Assessment — days of supply FBA-only AND with inbound, status label; (4) PO Recommendation — quantity, timing, arrival window, rationale including safety stock multiplier and PO cadence; (5) Next Steps — actionable checklist
- Always state a clear recommendation with rationale
- Calculate days of supply two ways: FBA-only and with all inbound
- Use class-based safety stock multipliers (e.g., Class A: 1.2x)

**Don't claim**

- Never end with 'Option 1 or Option 2?' without providing a recommended choice
- Do not present raw tool output without structuring it
- Do not omit the rationale for the recommendation

**Escalate when**

- Multiple valid PO quantities with significantly different trade-offs
- Conflicting signals (e.g., minimize inventory vs. prevent stockouts)

## Po Tool Execution

**How to help**

- Execute tools in strict order: (1) neonpanel_listInventoryItems, (2) supply_chain_inspect_inventory_sku_snapshot, (3) supply_chain_list_product_logistics_parameters, (4) supply_chain_list_po_placement_candidates
- If step 4 returns empty, switch to the Empty Results Diagnosis skill — do NOT re-run the same tool
- Collect results from all steps before presenting to user
- Proceed to result presentation phase

**Don't claim**

- Never re-run the same tool with the same parameters when it returns empty
- Do not skip steps in the tool sequence
- Do not present partial results — complete all steps first

**Escalate when**

- Tool times out or returns error 3+ times
- Same query returns different results on retry

## Phase 2 of PO planning: validating all inputs exist and are correctly configured before running analysis tools

**What it is**

Phase 2 of PO planning: validating all inputs exist and are correctly configured before running analysis tools. Checks SKU existence, scenario assignment, lead time, and safety stock.

**Key objects**

- Validation report
- Status indicators (✅ ❌ ⚠️)
- Company access
- SKU existence
- Scenario assignment
- Lead time config
- Safety stock config

**How to help**

- Check if my data is ready for planning
- Validate these inputs
- Why did the tool return empty?

**Clarifying questions**

- Skipping validation and going straight to analysis
- Assuming company defaults always exist
- Not checking each SKU independently in multi-SKU families

## Phase 5 of PO planning: presenting results in a structured 5-section format — Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps

**What it is**

Phase 5 of PO planning: presenting results in a structured 5-section format — Current Inventory, Sales Forecast, Risk Assessment, PO Recommendation, and Next Steps.

**Key objects**

- Current Inventory section
- Sales Forecast section
- Risk Assessment section
- PO Recommendation section
- Next Steps checklist
- Days of supply
- Safety stock multiplier

**How to help**

- Show me the PO recommendation
- What should I order?
- Give me the analysis

**Clarifying questions**

- Presenting raw tool output without structure
- Ending with 'Option 1 or 2?' without a recommendation
- Not calculating days of supply both ways (FBA-only and with inbound)

## Phase 6: Escalation

**Procedure**

1. Escalate when any of these conditions are met:
   - **Data integrity**: SKU exists in one system but not another
   - **Missing critical config**: No lead time AND no company default
   - **Unexpected results**: Same tool query returns different results on retry, or tool returns data with null/zero values for key fields
   - **User decision required**: Multiple valid PO quantities with different trade-offs, or conflicting parameters
   - **Orphaned variants**: Product family cross-check reveals SKUs with null parent ASIN — flag as catalog data integrity issue
2. When escalating, always:
   - Summarize all findings collected so far
   - Explain the specific gap or issue
   - Present options with trade-offs:
     "This requires a decision from you. Should I: A) [Option with trade-offs] B) [Option with trade-offs] Or escalate to [specialist]?"
3. Never ask vague follow-up questions instead of escalating.
4. Never escalate without first running the diagnostic tree (if applicable).
