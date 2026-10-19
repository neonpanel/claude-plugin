# Empty Results Diagnosis — Knowledge

## Diagnose Empty Results

**Clarifying questions**

- Which tool returned empty results?
- What SKU/ASIN and company were you querying?

**How to help**

- Follow the 5-check diagnostic tree in strict order:
- Check 1: Does the SKU exist in the inventory snapshot? If NO → report 'SKU not found' with possible reasons (not set up, wrong marketplace, inactive/archived)
- Check 2: Does it have lead_time_days configured? If NO → report missing lead time, state company default if one exists
- Check 3: Does it have safety_stock_days configured? If NO → report missing safety stock, state company default and class multiplier if applicable
- Check 4: Is it using the correct forecast scenario? If NO → report the mismatch (current vs. expected scenario) and provide NeonPanel steps to fix
- Check 5: Is it flagged as 'actively sold' or included in planning_base? If NO → report the status and explain how to change planning_base
- If all 5 checks pass → report 'All checks pass, tool returned empty for unknown reason' and escalate to support
- Never re-run the same tool with the same parameters — always identify root cause first

**Don't claim**

- Do not re-run the same tool hoping for different results
- Do not ask the user vague questions like 'Is it the right ASIN?' — run the checks yourself
- Do not skip checks in the diagnostic tree
- Do not claim the issue is fixed without verifying

**Escalate when**

- All 5 checks pass but tool still returns empty
- Tool returns error instead of empty results
- SKU exists in inventory items but not in snapshot (data integrity)

## After identifying the root cause via the diagnostic tree, providing clear resolution steps — what the user needs to configure in NeonPanel and what the agent should re-run after

**What it is**

After identifying the root cause via the diagnostic tree, providing clear resolution steps — what the user needs to configure in NeonPanel and what the agent should re-run after.

**Key objects**

- Resolution steps
- NeonPanel configuration path
- Re-run instructions
- Escalation path

**How to help**

- How do I fix this?
- What do I need to configure?
- Can you fix it for me?

**Clarifying questions**

- Thinking the agent can fix NeonPanel configuration directly
- Not providing specific NeonPanel navigation steps
- Not offering to re-run after the fix

## Verified Facts

**Facts**

- When a planning tool returns empty results, follow the 5-check diagnostic tree in strict order: (1) SKU in snapshot, (2) lead_time_days, (3) safety_stock_days, (4) forecast scenario, (5) actively sold flag.
- Never re-run the same tool with the same parameters when it returns empty — always identify the root cause first.
- If all 5 diagnostic checks pass but the tool still returns empty, escalate to support — do not keep retrying.
- Each diagnostic check has a specific resolution path that includes NeonPanel navigation steps the user must follow.
- After the user applies a configuration fix, always verify the fix worked by re-running the specific check that failed before re-running the full PO workflow.
- The agent cannot make NeonPanel configuration changes directly — it can only diagnose the issue and provide step-by-step instructions for the user to fix it.
- The planning_base parameter controls which SKUs are included in PO placement: 'actively_sold_only' includes only active SKUs, 'all' includes all SKUs regardless of status.

## The 5-check diagnostic decision tree for when supply_chain_list_po_placement_candidates or other planning tools return empty results

**What it is**

The 5-check diagnostic decision tree for when supply_chain_list_po_placement_candidates or other planning tools return empty results. Checks: SKU existence → lead time → safety stock → forecast scenario → planning status.

**Key objects**

- Check 1: SKU in snapshot
- Check 2: lead_time_days
- Check 3: safety_stock_days
- Check 4: forecast scenario
- Check 5: actively sold flag
- Diagnostic output
- Root cause

**How to help**

- Why did the tool return empty?
- No results came back
- PO placement returned nothing
- Debug empty results

**Clarifying questions**

- Re-running the same tool hoping for different results
- Asking the user vague questions instead of running checks
- Not following the tree in order

## 5-Check Diagnostic Tree

**Procedure**

When a planning tool (especially `supply_chain_list_po_placement_candidates`) returns empty results, follow these 5 checks in strict order. Do NOT skip checks or re-run the original tool.

### Check 1: Does the SKU exist in the snapshot?

1. Run `supply_chain_inspect_inventory_sku_snapshot` for the SKU.
2. If the SKU is found → proceed to Check 2.
3. If the SKU is NOT found → report:
   ```
   ❌ SKU not found in company [ID] ([marketplace]).
   Possible reasons:
   - Not set up in NeonPanel yet
   - Set up for a different marketplace (check UK, EU, etc.)
   - Inactive/archived
   - Spelling or format mismatch
   Action: Confirm SKU code and marketplace, then check NeonPanel inventory list.
   ```
4. Use `neonpanel_listInventoryItems` to search for similar identifiers if the exact match fails.

### Check 2: Does it have lead_time_days configured?

1. Run `supply_chain_list_product_logistics_parameters` for the SKU.
2. If lead_time_days is set → proceed to Check 3.
3. If lead_time_days is NOT set → check for company default.
4. If company default exists → report:
   ```
   ⚠️ Lead time not set for this SKU.
   Company default: [X] days.
   Action: Set lead_time_days in NeonPanel for this SKU, or confirm using company default.
   ```
5. If NO company default → report:
   ```
   ❌ Lead time not set for this SKU AND no company default exists.
   This is a critical configuration gap — PO planning cannot proceed.
   Action: Go to NeonPanel → Company Settings → Supply Chain → Set lead_time_days.
   ```

### Check 3: Does it have safety_stock_days configured?

1. Check safety_stock_days from the logistics parameters.
2. If set → proceed to Check 4.
3. If NOT set → check for company default.
4. Report with class multiplier info if applicable:
   ```
   ⚠️ Safety stock not set for this SKU.
   Company default: [X] days (Class A: 1.2x multiplier = [Y] effective days).
   Action: Set safety_stock_days in NeonPanel or confirm using default.
   ```

### Check 4: Is it using the correct forecast scenario?

1. Check the scenario assignment from the inventory snapshot.
2. If the assigned scenario matches the user's requested scenario → proceed to Check 5.
3. If there's a mismatch → report:
   ```
   ❌ SKU is using '[current scenario]' (scenario [ID]),
   not '[requested scenario]'.
   Action: Apply [requested scenario] in NeonPanel:
   1. Go to Inventory Planning
   2. Select SKU [code]
   3. Change forecast scenario
   4. Save and refresh
   Then I'll re-run the PO analysis.
   ```

### Check 5: Is it flagged as "actively sold" or "planned"?

1. Check the planning_base status for the SKU.
2. If flagged as actively sold → report:
   ```
   ✅ All 5 checks pass. Tool returned empty for unknown reason.
   Escalating to support with diagnostic summary.
   ```
3. If NOT flagged → report:
   ```
   ❌ SKU not flagged for PO planning.
   Current planning_base: [value]
   Action: Set planning_base to 'actively_sold_only' or 'all' in NeonPanel.
   ```

## Post-Fix Verification

**Procedure**

1. After the user reports they've made a configuration change, re-run ONLY the specific check that failed.
2. If the check now passes, offer to re-run the full PO planning workflow from Phase 3 (tool execution).
3. If the check still fails, report the persistent issue and escalate.
4. Do NOT re-run all 5 checks — only verify the one that was fixed.

## Resolve Root Cause

**Clarifying questions**

- Have you made the configuration change in NeonPanel yet?

**How to help**

- Based on the diagnostic result, provide specific NeonPanel navigation steps to fix the issue
- For missing lead time: 'Go to NeonPanel → Company Settings → Supply Chain → Set lead_time_days for this SKU (or company default)'
- For missing safety stock: 'Go to NeonPanel → Company Settings → Supply Chain → Set safety_stock_days'
- For wrong scenario: 'Go to NeonPanel → Inventory Planning → Select SKU → Change forecast scenario → Save'
- For inactive SKU: 'Go to NeonPanel → Inventory → Find SKU → Check planning_base setting'
- After user confirms the fix, offer to re-run the PO planning workflow from the tool execution phase
- Verify the fix worked by re-running the specific check that failed

**Don't claim**

- Do not claim the agent can make NeonPanel configuration changes directly
- Do not skip verification after the user applies a fix

**Escalate when**

- User cannot access the NeonPanel configuration page
- Configuration change doesn't resolve the empty results
