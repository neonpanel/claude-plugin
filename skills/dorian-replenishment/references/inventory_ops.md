# Inventory Ops — Knowledge

## Sku Lookup

**Clarifying questions**

- Is that a parent ASIN, child ASIN, SKU code, or product family name?
- Which marketplace — US, UK, or another?

**How to help**

- Use neonpanel_listInventoryItems to search for the identifier
- If parent ASIN, list all child SKUs and ask user to confirm scope
- If no results, check marketplace, spelling, and active/archived status
- For detailed validation, use the Inventory Validation skill

**Don't claim**

- Do not assume a parent ASIN is a single product

**Escalate when**

- Identifier doesn't match any SKU in the company

## Check Inventory Position

**Clarifying questions**

- Which SKU or ASIN are you checking?
- Which company and marketplace?

**How to help**

- Run supply_chain_inspect_inventory_sku_snapshot to get current position
- Calculate days of supply FBA-only and with inbound
- Apply risk thresholds (Critical <7d, At Risk <14d, Healthy ≥14d)

**Don't claim**

- Do not count WIP orders as available stock
- Do not present only one days-of-supply calculation — always show both

**Escalate when**

- Inventory snapshot data appears stale or contradictory

## Inventory Position

**Clarifying questions**

- Which SKU or ASIN do you want inventory levels for?
- Do you want FBA-only or total inventory across all warehouses?

**How to help**

- Use supply_chain_inspect_inventory_sku_snapshot to get current inventory
- Break down: FBA available, inbound, warehouse stock, WIP orders, total coverage
- Calculate days of supply: FBA-only and with all inbound
- Flag risk levels: <14 days FBA-only = critical, <30 days = at risk

**Don't claim**

- Do not confuse FBA available stock with total inventory
- Do not report WIP orders as available stock
- Do not fabricate inventory numbers — only report what the tool returns

**Escalate when**

- Inventory snapshot shows zero across all locations for an active SKU
- Significant discrepancy between calculated and 3PL inventory

## Warehouse Check

**Clarifying questions**

- Which company should I check?

**How to help**

- Run neonpanel_listWarehouses for the company
- Check for clones (same name, different IDs)
- Identify AWD warehouses and flag that inventory there is not FBA-available
- Report warehouse types: FBA, AWD, 3PL, own

**Don't claim**

- Do not match warehouses by name alone — always verify IDs

**Escalate when**

- Warehouse clones detected that may affect inventory calculations

## Verified Facts

**Facts**

- A parent ASIN may map to multiple child ASINs and SKUs. Always resolve to specific SKUs before running inventory analysis.
- SKU lookup must specify both company ID and marketplace — the same SKU code may exist in different marketplaces with different inventory.
- Inventory position has four components: FBA available (sellable now), Inbound (in transit to FBA), Warehouse stock (in own/3PL warehouses), and WIP orders (purchase orders placed but not yet received).
- Days of supply should be calculated two ways: FBA-only (available stock ÷ daily velocity) and with inbound (available + inbound ÷ daily velocity). Both are needed for risk assessment.
- Two warehouses can have identical names but different backend IDs (warehouse clones). Always verify warehouse IDs, not just names.
- AWD (Amazon Warehousing and Distribution) warehouses are distinct from Amazon FBA warehouses. Inventory in AWD must be shipped to FBA before it becomes available for sale.
- WIP orders (Work In Progress) represent purchase orders that have been placed but not yet received. They should not be counted as available stock but are part of total coverage calculations.
- When a SKU is not found in a company, check: correct marketplace, active vs archived status, spelling/format of the identifier, and whether it's set up in NeonPanel at all.
- Inventory snapshots from supply_chain_inspect_inventory_sku_snapshot reflect a point-in-time view. Inbound quantities change as shipments are received.
- Warehouse type (FBA, AWD, 3PL, own) determines how inventory is managed and which replenishment rules apply.

## Sku Lookup Resolution

**Clarifying questions**

- What identifier do you have — parent ASIN, child ASIN, or SKU code?
- Which marketplace?

**How to help**

- If parent ASIN: go directly to supply chain tools (they resolve children automatically)
- If SKU/child ASIN: run neonpanel_listInventoryItems to confirm existence
- If not found: check marketplace, active/archived status, spelling

**Don't claim**

- Do not pre-resolve parent ASINs via neonpanel_listInventoryItems

**Escalate when**

- SKU exists in one system but not another

## Inventory Position Check

**Clarifying questions**

- Which company and SKU?

**How to help**

- Run supply_chain_inspect_inventory_sku_snapshot for the SKU
- Break down: FBA available, inbound, warehouse stock, WIP orders, total coverage
- Calculate days of supply two ways: FBA-only and with inbound
- Apply risk thresholds: 🔴 <7 days FBA, 🟡 <14 days FBA, 🟢 ≥14 days FBA and ≥30 days with inbound
- Note WIP orders are not available stock but part of total coverage

**Don't claim**

- Do not count WIP orders as available stock
- Do not treat AWD inventory as FBA-available

**Escalate when**

- Inventory data appears inconsistent across sources

## Warehouse Types And Clones

**Clarifying questions**

- Which company are you checking warehouses for?

**How to help**

- Run neonpanel_listWarehouses to list all warehouses
- Group by name to detect clones (same name, different IDs)
- Identify AWD warehouses and flag that inventory is not FBA-available

**Don't claim**

- Do not match warehouses by name alone — always verify IDs
- Do not treat AWD inventory as available for FBA sale

**Escalate when**

- Warehouse clones detected that may be splitting inventory data
