# Inventory Validation — Knowledge

## Verifying warehouse identity and type: detecting warehouse clones (same name, different IDs), distinguishing AWD from FBA, and confirming warehouse types (FBA, AWD, 3PL, own)

**What it is**

Verifying warehouse identity and type: detecting warehouse clones (same name, different IDs), distinguishing AWD from FBA, and confirming warehouse types (FBA, AWD, 3PL, own).

**Key objects**

- Warehouse ID
- Warehouse name
- Warehouse type
- Warehouse clone
- AWD vs FBA
- neonpanel_listWarehouses

**How to help**

- Check the warehouses
- Are there duplicate warehouses?
- Is this AWD or FBA?

**Clarifying questions**

- Matching warehouses by name instead of ID
- Treating AWD inventory as FBA-available
- Not checking for clones

## Verify Planning Config

**How to help**

- For each SKU, check: lead_time_days (SKU-level or company default), safety_stock_days (SKU-level or company default), forecast scenario assignment, planning_base status (actively sold)
- Use supply_chain_list_product_logistics_parameters to get logistics config
- Use supply_chain_inspect_inventory_sku_snapshot to check scenario assignment
- Report with status indicators: ✅ configured, ❌ missing, ⚠️ using company default
- If multiple SKUs: validate each independently — they may have different configs
- Flag any SKU that is not ready for planning and explain what's missing

**Don't claim**

- Do not assume all SKUs in a family have the same configuration
- Do not skip validation for any SKU in the scope
- Do not proceed to PO planning with ❌ items unresolved

**Escalate when**

- No lead time AND no company default for a SKU
- Forecast scenario doesn't exist in NeonPanel

## Warehouse Clone Detection

**Procedure**

1. Run `neonpanel_listWarehouses` for the company.
2. Group warehouses by name.
3. If any name appears more than once with different IDs, flag as potential clone:
   ```
   ⚠️ WAREHOUSE CLONE DETECTED
   Name: "Amazon FBA US"
   - ID: wh_001 (type: FBA, status: active)
   - ID: wh_047 (type: FBA, status: active)
   These may cause inventory data to split across two entities.
   Action: Verify which ID is correct in your shipments and inventory data.
   ```
4. Identify all AWD warehouses and flag them:
   ```
   ℹ️ AWD Warehouses (inventory NOT available for FBA sale):
   - "Amazon AWD US" (ID: wh_012)
   Inventory here must be shipped to FBA before it becomes sellable.
   ```
5. List warehouse types for reference: FBA, AWD, 3PL, own.

## Inventory Position Analysis

**Procedure**

1. For each validated SKU (including orphaned variants found via product family), compile the full inventory position:
2. **FBA Available**: Current sellable stock in FBA warehouses.
3. **Inbound**: Units in transit to FBA (from shipments not yet received).
4. **Warehouse Stock**: Units in own/3PL warehouses (not yet shipped to FBA).
5. **WIP Orders**: Purchase orders placed but not yet received (include PO reference numbers).
6. Calculate total coverage = FBA Available + Inbound + Warehouse Stock + WIP.
7. Calculate days of supply two ways:
   - FBA-only: Available ÷ daily velocity
   - With inbound: (Available + Inbound) ÷ daily velocity
8. Apply risk thresholds:
   - 🔴 CRITICAL: FBA-only < 7 days
   - 🟡 AT RISK: FBA-only < 14 days
   - 🟢 HEALTHY: FBA-only ≥ 14 days AND with-inbound ≥ 30 days
9. Note: WIP orders should NOT be counted as available stock but are part of total coverage for planning purposes.

## Verified Facts

**Facts**

- A parent ASIN may map to multiple child ASINs and SKUs. Always resolve to specific SKUs before running any inventory or planning analysis.
- SKU lookup must specify both company ID and marketplace — the same SKU code may exist in different marketplaces with different inventory data.
- Inventory position has four components: FBA available (sellable now), Inbound (in transit to FBA), Warehouse stock (in own/3PL warehouses), and WIP orders (purchase orders placed but not yet received).
- Two warehouses can have identical names but different backend IDs (warehouse clones). Always verify warehouse IDs, not just names.
- AWD (Amazon Warehousing and Distribution) warehouses are distinct from Amazon FBA warehouses. Inventory in AWD must be shipped to FBA before it becomes available for sale.
- WIP orders represent purchase orders placed but not yet received. They should not be counted as available stock but are part of total coverage calculations.
- When a SKU is not found in a company, check: correct marketplace, active vs. archived status, spelling/format of the identifier, and whether it's set up in NeonPanel.
- Lead time and safety stock can be configured at the SKU level or fall back to company defaults. When neither is set, this is a critical configuration gap.
- For multi-SKU families, each SKU must be validated independently — they may have different lead times, safety stock, forecast scenarios, and planning status.
- Warehouse type (FBA, AWD, 3PL, own) determines how inventory is managed and which replenishment rules apply.
- When the user states an identifier is a parent ASIN, trust their input immediately. Do not run neonpanel_listInventoryItems to validate it — go directly to the parent-ASIN-aware supply chain tools.
- The supply chain tools (supply_chain_analyze_sales_velocity, supply_chain_list_po_placement_candidates, supply_chain_inspect_inventory_sku_snapshot) accept a parent_asin parameter and automatically resolve to all child SKUs. Use them directly instead of pre-resolving.
- Never pre-resolve a parent ASIN to child ASINs using neonpanel_listInventoryItems. The supply chain tools handle parent-to-child resolution natively.
- When the user provides an additional ASIN during a conversation, add it to the existing parent_asin filter array immediately — do not start a new validation flow.
- Product family (product_family field) is more reliable than parent ASIN for finding all variants because: (1) it is manually curated by the team, not dependent on Amazon catalog sync, (2) parent ASIN mappings can be stale, null, or missing for new variants, and (3) product family catches cross-parent items if ASINs are split or merged.
- When resolving a product family, always use the two-step lookup: first query by parent_asin to find initial items, then identify the product_family from those results and re-query by product_family to catch orphaned variants with null or mismatched parent ASIN mappings.
- Any variant found via product_family lookup that has a null or mismatched parent_asin should be flagged to the user as a data integrity issue — the parent ASIN mapping needs to be fixed in the catalog.
- When a parent ASIN query returns fewer SKUs than expected, always cross-check by product_family before concluding the family is complete.

## Verifying that all required planning configuration exists for a SKU: lead time, safety stock, forecast scenario assignment, planning status (actively sold), and vendor/MOQ settings

**What it is**

Verifying that all required planning configuration exists for a SKU: lead time, safety stock, forecast scenario assignment, planning status (actively sold), and vendor/MOQ settings.

**Key objects**

- Lead time days
- Safety stock days
- Forecast scenario assignment
- Planning base status
- Vendor info
- MOQ
- Company defaults

**How to help**

- Is this SKU ready for planning?
- Check the configuration
- What's missing?

**Clarifying questions**

- Assuming company defaults always exist
- Not checking each SKU independently
- Confusing SKU-level config with company-level defaults

## Planning Configuration Verification

**Procedure**

1. For each SKU in scope (including any orphaned variants found via product family cross-check), check these four configuration items:
2. **Lead time**: Run `supply_chain_list_product_logistics_parameters`. Check for SKU-level lead_time_days. If not set, check company default.
3. **Safety stock**: Check safety_stock_days from same tool. Note the item class and applicable multiplier (e.g., Class A: 1.2x).
4. **Forecast scenario**: Run `supply_chain_inspect_inventory_sku_snapshot`. Check which scenario is assigned. Compare to user's requested scenario.
5. **Planning status**: Check if SKU is flagged as actively sold / included in planning_base.
6. Report results per SKU with status indicators:
   ```
   SKU: VLX-SLCNPDS-104-MIX-VLX
   ✅ Lead time: 60 days (SKU-level)
   ✅ Safety stock: 14 days (company default, Class A: 1.2x = 16.8 effective)
   ❌ Forecast scenario: Using 'Recency-Weighted Conservative', not 'promo_shift_128_growth_v2'
   ✅ Planning status: Actively sold
   ⚠️ Parent ASIN: NULL — found via product family cross-check only
   ```
7. If any SKU has ❌ items, flag it as not ready for planning and provide resolution steps.

## Resolve Sku Identifier

**Clarifying questions**

- Is that a parent ASIN, child ASIN, SKU code, or product family name?
- Which marketplace — US, UK, or another?

**How to help**

- Use neonpanel_listInventoryItems with the identifier and company ID to find matching SKUs
- If parent ASIN: list all child ASINs/SKUs under it and ask user to confirm scope
- If product family: search by family name and list all matching SKUs
- Report findings clearly: 'Found X SKUs under parent ASIN Y: [list with SKU codes and names]'
- If no results: check marketplace, spelling, active vs. archived status

**Don't claim**

- Do not assume a parent ASIN is a single product — always resolve to specific SKUs
- Do not proceed with analysis until the user confirms which SKUs to include

**Escalate when**

- Identifier doesn't match any SKU in the specified company and marketplace
- Same SKU code exists in multiple marketplaces with different data

## Resolving product identifiers to specific SKUs: handling parent ASINs (which map to multiple children), child ASINs, SKU codes, and product family names

**What it is**

Resolving product identifiers to specific SKUs: handling parent ASINs (which map to multiple children), child ASINs, SKU codes, and product family names. Confirming the correct marketplace.

**Key objects**

- Parent ASIN
- Child ASIN
- SKU code
- Product family
- Marketplace
- neonpanel_listInventoryItems

**How to help**

- Find this product
- Look up this ASIN
- Which SKUs are under this parent?
- Resolve this identifier

**Clarifying questions**

- Treating a parent ASIN as a single product
- Not specifying marketplace
- Assuming SKU codes are unique across marketplaces

## Parent Asin Direct Lookup

**Clarifying questions**

- Which marketplace should I use for this parent ASIN?
- Should I include all child SKUs or specific ones?

**How to help**

- Trust the user's identifier — do NOT call neonpanel_listInventoryItems to validate a parent ASIN
- Go directly to supply chain tools with parent_asin parameter: supply_chain_inspect_inventory_sku_snapshot, supply_chain_analyze_sales_velocity, supply_chain_list_po_placement_candidates
- These tools resolve parent-to-child automatically and return all children
- If user provides additional ASINs mid-conversation, add them to the parent_asin array immediately

**Don't claim**

- Do not call neonpanel_listInventoryItems to pre-resolve a parent ASIN to children
- Do not ask the user to provide child ASINs when they already gave a parent ASIN
- Do not treat an additional ASIN as a separate lookup requiring new validation

**Escalate when**

- Supply chain tools return no results for a confirmed parent ASIN

## Verify Warehouses

**Clarifying questions**

- Which company should I check warehouses for?

**How to help**

- Use neonpanel_listWarehouses to get full warehouse list
- Check for warehouse clones: same name but different IDs
- Identify warehouse types: FBA, AWD, 3PL, own
- Flag AWD warehouses — inventory there is NOT available for FBA sale until shipped
- Report: 'Found X warehouses. Y are FBA, Z are AWD. [Clone warnings if any]'
- If clones detected: warn user and recommend verifying which ID is correct in their data

**Don't claim**

- Do not match warehouses by name alone — always use IDs
- Do not treat AWD inventory as FBA-available
- Do not ignore warehouse clones — they cause data integrity issues

**Escalate when**

- Warehouse clones detected that may affect inventory calculations
- Warehouse type is ambiguous or unlabeled

## SKU Identifier Resolution

**Procedure**

1. Determine the identifier type: parent ASIN, child ASIN, SKU code, or product family name.
2. Confirm the marketplace (US, UK, EU, etc.) and company ID.
3. **If parent ASIN**: Do NOT call `neonpanel_listInventoryItems` to pre-resolve children. Trust the user's input and go directly to the supply chain tools, which accept `parent_asin` as a parameter and resolve all children automatically.
   - Use `supply_chain_inspect_inventory_sku_snapshot` with `parent_asin: ["B0D45BYWQR"]`
   - Use `supply_chain_list_po_placement_candidates` with `parent_asin: ["B0D45BYWQR"]`
   - Use `supply_chain_analyze_sales_velocity` with `parent_asin: ["B0D45BYWQR"]`
   - These tools return all child ASINs/SKUs automatically.
4. **Two-step family completeness check (mandatory for parent ASIN lookups)**:
   - After the initial parent ASIN query returns results, identify the `product_family` value from those results (e.g., "Bumpers - Mix").
   - Re-query by `product_family` + `marketplace` to catch any orphaned variants whose `parent_asin` field is null or mismatched.
   - If the product_family query returns more SKUs than the parent ASIN query, flag the extras:
     ```
     ⚠️ ORPHANED VARIANTS DETECTED
     Found via product family "Bumpers - Mix" but missing parent ASIN mapping:
     - SKU: [sku_code] (child ASIN: B0FGQH688S) — parent_asin is NULL
     Action: Fix parent ASIN mapping in catalog for complete family coverage.
     ```
   - Include the orphaned variants in the analysis — do not exclude them just because parent ASIN is null.
5. **If child ASIN or SKU code**: Run `neonpanel_listInventoryItems` to confirm it exists in the company and marketplace.
6. **If product family name**: Run `neonpanel_listInventoryItems` with search_query to find matching SKUs, then confirm with user.
7. **When user provides an additional ASIN mid-conversation**: Add it to the existing `parent_asin` array immediately (e.g., `parent_asin: ["B0D45BYWQR", "B0FGQH688S"]`). Do NOT start a new validation flow or treat it as a separate lookup.
8. If no results found from supply chain tools: check marketplace, try alternate identifier formats, check active vs. archived.
9. If the same SKU exists in multiple marketplaces, flag this and confirm which marketplace to use.
