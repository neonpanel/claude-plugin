# Po Invoice Mapping — Knowledge

## Looking up a purchase order by its reference number using NeonPanel tools

**What it is**

Looking up a purchase order by its reference number using NeonPanel tools. Involves finding the company, searching for the PO project, and retrieving linked records (shipments, bills).

**Key objects**

- Purchase Order
- PO reference number
- Company ID
- Project record
- Linked shipment
- Linked bill

**How to help**

- Look up PO 320250006
- Find a purchase order
- Get details for this PO number
- What's on this order?

**Clarifying questions**

- Confusing PO reference numbers with internal project IDs
- Not realizing a PO may have multiple linked shipments or bills

## Verified Facts

**Facts**

- The PO-to-invoice mapping produces a formatted markdown document, not a legally binding invoice.
- PO lookup requires a reference number (e.g., 320250006) and uses the search_neonpanel_project_url tool to find matching projects.
- A NeonPanel PO project may have linked records: shipments, bills, and inventory items.
- The invoice template includes: header (vendor, PO ref, date, status), line items table (SKU, description, qty, unit price, line total), and totals section.
- Landed costs (freight, duties, other charges) can be added to the invoice using neonpanel_getInventoryLandedCost.
- COGS data can be pulled via neonpanel_getInventoryCogs to show cost-of-goods-sold per item.
- All amounts on the generated invoice must come directly from NeonPanel tool responses — never estimate or fabricate figures.
- The invoice output is markdown-formatted text suitable for copying, sharing, or converting to PDF externally.

## Lookup Po Details

**Clarifying questions**

- What's the PO reference number?
- Which company should I search in?

**How to help**

- Search for the PO using search_neonpanel_project_url
- Present the PO summary with linked records (shipments, bills)
- Offer to generate an invoice from the results

**Don't claim**

- Do not fabricate PO details not returned by the tool

**Escalate when**

- PO not found

## Line Items

**Procedure**

| # | SKU | Description | Qty | Unit Price | Line Total |
|---|-----|-------------|-----|------------|------------|
| 1 | {sku} | {description} | {qty} | ${unit_price} | ${line_total} |
| 2 | ... | ... | ... | ... | ... |

---

## Totals

**Procedure**

| | |
|---|---|
| **Subtotal** | ${subtotal} |
| **Freight** | ${freight} |
| **Duties** | ${duties} |
| **Other Charges** | ${other} |
| **Grand Total** | ${grand_total} |

---

## Invoice Markdown Template

**Procedure**

Use this template structure when generating the invoice output. Adapt field names based on what the NeonPanel tools actually return — do NOT fabricate fields that weren't in the response.

```
# PURCHASE ORDER INVOICE

| | |
|---|---|
| **Vendor** | {vendor_name} |
| **PO Reference** | {po_reference_number} |
| **PO Date** | {po_date} |
| **Status** | {po_status} |
| **Shipment Status** | {shipment_status} |
| **Shipment Date** | {shipment_date} |

---

## Mapping purchase order data from NeonPanel into a structured invoice/document format

**What it is**

Mapping purchase order data from NeonPanel into a structured invoice/document format. Covers field mapping (PO fields → invoice fields), line item formatting, totals calculation, and markdown output.

**Key objects**

- Invoice header
- Line items table
- Subtotal
- Invoice template
- Vendor info
- PO-to-invoice field map

**How to help**

- Generate an invoice from this PO
- Create a text invoice for PO 320250006
- Format this order as an invoice
- Map this PO to an invoice

**Clarifying questions**

- Expecting PDF output (only markdown/text is supported)
- Confusing the PO total with invoice total when landed costs differ

## Handle Missing Data

**Procedure**

1. If the PO is not found, inform the user and ask them to verify the reference number and company.
2. If line-item details are unavailable (tool returns only the PO summary), output a **summary invoice** with just the header and total amount — note that line items couldn't be retrieved.
3. If landed cost data is unavailable, output the basic invoice and note that landed costs are not yet available for this PO.
4. Never fill in placeholder values — if a field is missing, omit it or mark it as "N/A".

## Generate Invoice with Landed Costs

**Procedure**

1. Follow steps 1–6 from "Generate Invoice from Purchase Order" above.
2. Call `neonpanel_getInventoryLandedCost` to retrieve landed cost data for the items.
3. Optionally call `neonpanel_getInventoryCogs` for COGS breakdown.
4. Add a **Cost Breakdown** section to the invoice showing:
   - Product cost (from PO)
   - Freight charges
   - Duties / tariffs
   - Other landed cost components
   - Total landed cost
5. Output the enriched invoice with the cost breakdown section appended.

## Enriching the invoice with additional cost data — landed costs, COGS, unit costs — pulled from NeonPanel's costing tools

**What it is**

Enriching the invoice with additional cost data — landed costs, COGS, unit costs — pulled from NeonPanel's costing tools.

**Key objects**

- Landed cost
- COGS
- Unit cost
- Freight
- Duties
- Cost breakdown

**How to help**

- Include landed costs on the invoice
- Show COGS per item
- Add freight and duties to the invoice
- What's the true cost per unit?

**Clarifying questions**

- Confusing PO price with landed cost
- Not realizing landed cost includes freight, duties, and other charges

## Po Invoice With Costs

**Clarifying questions**

- What's the PO reference number?
- Should I include freight, duties, or all landed cost components?

**How to help**

- Look up the PO via search_neonpanel_project_url
- Pull inventory details via neonpanel_getInventoryDetails
- Pull landed costs via neonpanel_getInventoryLandedCost
- Optionally pull COGS via neonpanel_getInventoryCogs
- Map all data onto the enriched invoice template
- Output the formatted markdown invoice with cost breakdown

**Don't claim**

- Do not fabricate cost figures not returned by the tools
- Do not present landed cost estimates as exact figures unless the tool confirms them

**Escalate when**

- Landed cost data unavailable for this PO
- Significant discrepancy between PO total and landed cost total

## Linked Records

**Procedure**

| Type | Reference | Status | Link |
|------|-----------|--------|------|
| Shipment | {shipment_ref} | {status} | [Open]({url}) |
| Bill | {bill_ref} | {status} | [Open]({url}) |

---

*Generated from NeonPanel PO data. This is a formatted summary, not a legal invoice.*
```

## Generate Invoice from Purchase Order

**Procedure**

1. Get the PO reference number from the user (e.g., 320250006).
2. If the company is ambiguous, ask which company to search in. Otherwise use the default/known company.
3. Call `search_neonpanel_project_url` with the PO reference number to find the project.
4. From the results, identify the **Inventory Order (PO)** record and note its details (date, amount, status, linked records).
5. Call `neonpanel_getInventoryDetails` to retrieve line-item details for the PO (SKUs, descriptions, quantities, unit prices).
6. Map the retrieved data onto the invoice template below.
7. Calculate line totals (qty × unit price) and the grand total.
8. Output the formatted markdown invoice.
9. Offer to enrich with landed costs or COGS if the user wants more detail.

## Generate Po Invoice

**Clarifying questions**

- What's the PO reference number?
- Which company is this PO from (if you have multiple)?
- Do you want just the basic invoice or should I include landed costs?

**How to help**

- Search for the PO using search_neonpanel_project_url
- Retrieve inventory details with neonpanel_getInventoryDetails
- Map PO data onto the invoice template
- Output the formatted markdown invoice

**Don't claim**

- Do not fabricate line items or amounts not returned by the tools
- Do not claim this is a legally valid invoice — it is a formatted summary document
- Do not guess unit prices if the tool doesn't return them

**Escalate when**

- PO not found in NeonPanel
- PO data is incomplete or missing line items
