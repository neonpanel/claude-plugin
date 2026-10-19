# Manage Neonpanel Projects — Knowledge

## The mandatory first steps before creating or updating any NeonPanel project document

**What it is**

The mandatory first steps before creating or updating any NeonPanel project document. ALWAYS resolve all entity IDs (vendor_id, customer_id, warehouse_id, inventory_id, service_id, sales_channel_id) using the appropriate list/search tools BEFORE calling any create or update tool. Never pass user-provided strings directly as IDs — they must be resolved to actual system IDs first.

**Key objects**

- warehouses
- inventories
- vendors
- services
- customers
- sales_channels
- payment_terms
- IDs

**How to help**

- create a document
- update a document
- make an invoice
- add a shipment
- create a bill
- create a PO
- create an adjustment
- create a project
- help me with a new project

**Clarifying questions**

- Guessing IDs instead of listing them first
- Passing user-provided strings directly as entity IDs
- Trying to create a document without resolving vendor_id, customer_id, or warehouse_id first

**Clarifying questions**

- What type of document do you need to create? (Bill, Invoice, Inventory Order, Shipment, or Adjustment)
- Do you have the vendor or customer name?
- Which warehouse should this be associated with?

**How to help**

- ALWAYS resolve all entity IDs (vendor_id, customer_id, warehouse_id, inventory_id, service_id, sales_channel_id) using list/search tools BEFORE calling any create tool.
- Never pass user-provided name strings directly as IDs — always look up the system ID first.
- If any required ID cannot be found, ask the user for clarification before proceeding.

**Don't claim**

- Do not guess IDs or use placeholder IDs for entities.
- Do not attempt to create a document without first resolving all required entity IDs.
- Do not pass user-provided strings (like vendor names) directly into ID fields.

**Escalate when**

- Entity cannot be found in the system after searching
- User reports that a created document has wrong associations

## Payload Formatting Rules

**Procedure**

1. **Nested arrays** (`items`, `documents`, `details`) must be proper JSON arrays of objects, NOT stringified JSON. Passing a string will trigger an `invalid_type` error.
2. **Nested array formatting**: Always pass arrays directly, never as strings. This applies to all document types.

## Creating a Shipment

### Procedure

1. Complete the Pre-Creation Checklist above.
2. Resolve `origin_warehouse_id` and `destination_warehouse_id` using `inventory_list_warehouses`.
3. Resolve `origin_inventory_id` for each line item using `inventory_list_items`.
4. Build the payload:
   - `name`: descriptive title (e.g., "Origin Warehouse - Destination Warehouse - Product Transfer - <Month Year>")
   - `ref_number`: user-provided code
   - `date_shipment_sent`: current date if not specified
   - `shipment_method`: strictly "LTL", "FTL", or "SPD" only
   - `items`: array of `{origin_inventory_id, quantity_shipped, quantity_received}` objects
5. Call `project_management_create_shipment`.

## Creating an Invoice

### Procedure

1. Complete the Pre-Creation Checklist above.
2. Resolve `customer_id`, `warehouse_id`, and `sales_channel_id`.
3. Resolve `inventory_id` and/or `service_id` for each line item.
4. Build the payload:
   - `name`: descriptive title (e.g., "Customer Name - Monthly Services - <Month Year>")
   - `ref_number`: user-provided code
   - `date`: current date if not specified
   - `currency`: "USD" (default)
   - `market`: "US" (default)
   - `customer_id`: resolved ID
   - `warehouse_id`: resolved ID
   - `sales_channel_id`: resolved ID
   - `details`: array of `{inventory_id, service_id, quantity, amount}` objects
5. Call `project_management_create_invoice`.

## Creating an Adjustment

### Procedure

1. Complete the Pre-Creation Checklist above.
2. Resolve `warehouse_id` using `inventory_list_warehouses`.
3. Resolve `inventory_id` and/or `service_id` for each line item using `inventory_list_items` / `project_management_list_services`.
4. Build the payload:
   - `name`: descriptive title (e.g., "Warehouse Name - Damaged Goods Write-Off - <Month Year>")
   - `ref_number`: user-provided code
   - `date`: current date if not specified
   - `currency`: "USD" (default)
   - `market`: "US" (default)
   - `warehouse_id`: resolved ID
   - `details`: array of `{inventory_id, service_id, quantity, rate}` objects
   - Note: positive quantity = stock removed; negative quantity = stock added
5. Call `project_management_create_adjustment`.

## Strict formatting and validation rules for NeonPanel project document payloads, including JSON array formatting, shipment method constraints, and proper payload structure

**What it is**

Strict formatting and validation rules for NeonPanel project document payloads, including JSON array formatting, shipment method constraints, and proper payload structure.

**Key objects**

- shipment_method
- JSON arrays
- customer
- sales_channel

**How to help**

- format document payload
- fix validation error
- why is my document failing

**Clarifying questions**

- Passing stringified JSON instead of proper arrays
- Using invalid shipment methods like Ground

**Clarifying questions**

- What error message are you seeing?
- Which document type were you trying to create?

**How to help**

- Ensure nested lists (items, documents, details) are proper JSON arrays of objects.
- For shipments, only use 'LTL', 'FTL', or 'SPD' as shipment_method values.

**Don't claim**

- Do not use custom strings like 'Ocean Freight' or 'Ground' for shipment_method.
- Do not pass stringified JSON arrays — always use proper JSON array objects.

**Escalate when**

- Validation errors persist after following all formatting rules

## Verified Facts

### Facts

- When creating or updating shipments, the shipment_method field strictly accepts only 'LTL', 'FTL', or 'SPD'.
- For shipments (items), bills (documents), and invoices (details), the payload must be a proper JSON array of objects, not a stringified JSON array.
- The 'name' field must NEVER be left blank on any document. Always generate a descriptive, human-readable title combining the entity/vendor/customer, purpose, and date (e.g., 'Vendor Name - Service Description - <Month Year>').
- Date fields (date, date_order_placed, date_shipment_sent) must NEVER be left blank. If the user does not specify a date, default to the current date.
- Unless the user specifies otherwise, always explicitly set currency to 'USD' and market to 'US' on every document.
- When creating Bills or Inventory Orders, payment_term_id is REQUIRED. If the user does not provide one, look it up using project_management_list_payment_terms or ask the user. Never create a Bill or PO without payment_term_id.
- Line item pricing keys differ by document type: Bills and Adjustments use 'rate', Inventory Orders use 'price', Invoices use 'amount'. Using the wrong key will cause errors.
- Line item quantity must always be included. Default to 1 for flat-fee services if the user does not specify a quantity.
- Before creating any document, ALL entity IDs (vendor_id, customer_id, warehouse_id, inventory_id, service_id, sales_channel_id) must be resolved using the appropriate list/search tools. Never pass user-provided name strings directly — always resolve to system IDs first.
- The ref_number field should use the short code provided by the user (e.g., 'INV-100', 'PO-250'). The name field is separate and must contain a descriptive title.

## Field Population Rules (ALL Document Types)

### Procedure

When populating the document payload, follow these rules strictly:

1. **`name` field**: NEVER leave blank. Generate a descriptive, human-readable title
   combining the entity/vendor/customer name, purpose, and period. Example:
   `"Vendor Name - Service Description - <Month Year>"`.
2. **`ref_number` field**: Use the short code the user provides (e.g. `"INV-100"`,
   `"PO-250"`). This is separate from `name`. On Inventory Orders it is accepted
   **only on create** and ignored on update, so confirm it before creating.
3. **Issue and placement dates** (`date`, `date_order_placed`, `date_shipment_sent`):
   NEVER leave blank. If the user does not specify one, default it.
4. **Completion dates that assert an event happened** — on Inventory Orders,
   `date_manufacturing_completed`: **NEVER default this.** See the separate rule below.
5. **`currency`**: Always set explicitly. ISO 4217, three letters. Default `"USD"`
   unless the user specifies otherwise.
6. **`market`**: Always set explicitly. ISO 3166-1 alpha-2, two letters. Default
   `"US"` unless the user specifies otherwise. The server resolves the code to a
   marketplace, so it must be a code the platform knows.
7. **`quantity`**: Always include on every line item. Default to 1 for flat-fee services.
8. **Pricing key per document type** (CRITICAL — using the wrong key causes errors):
   - **Bills**: use `rate`
   - **Adjustments**: use `rate`
   - **Inventory Orders**: use `price`
   - **Invoices**: use `amount`

### The manufacturing-completed date

On an Inventory Order, `date_manufacturing_completed` (stored as
`date_order_received`) is the date manufacturing finished and the goods were
received. Setting it tells NeonPanel the stock on that order has been
**manufactured** and is real, available inventory.

- Leaving it off a completed order means planning, valuation and landed cost all
  behave as though the goods were never made. Date-range filters also key on this
  field, so the order disappears from period queries.
- **Never populate it by default or assumption.** Ask: *"Has manufacturing been
  completed on this order yet?"*
- If the answer is no or unknown, leave it null deliberately and tell the user, so
  they know to set it once production finishes.
- A `400` on create or update means the date is earlier than the company's Entry
  Locked Date. Report it; do not retry blindly.

Its sibling `date_order_placed` (stored as `po_date`) is the date the purchase order
was placed with the vendor, and may be defaulted.

### Don't claim

- Do not describe a document as created or updated unless the tool confirmed it.
- Do not treat a date appearing in an example as a real value.

## Mandatory field-population rules for all NeonPanel project document types

**What it is**

Mandatory field-population rules for all NeonPanel project document types. When creating any document (Bill, Invoice, Inventory Order, Shipment, Adjustment), ALL highly recommended fields must be populated even if technically optional in the API schema. This includes: name (always generate a descriptive title), dates (default to current date if not specified), currency (default USD), market (default US), payment_term_id (required for Bills and Inventory Orders), and correct pricing keys per document type.

**Key objects**

- name
- ref_number
- date
- date_order_placed
- date_shipment_sent
- currency
- market
- payment_term_id
- quantity
- rate
- price
- amount
- vendor_id
- customer_id
- warehouse_id
- sales_channel_id
- details
- items

**How to help**

- create a bill
- create an invoice
- create a PO
- create a shipment
- create an adjustment
- make a purchase order
- add a new bill
- help me fill out this document

**Clarifying questions**

- Leaving name blank and only filling ref_number
- Skipping date fields when user doesn't specify a date
- Forgetting to set currency and market
- Using wrong pricing key (e.g. amount instead of rate for Bills)
- Creating Bills or POs without payment_term_id
- Defaulting quantity to 0 instead of 1 for flat-fee services

**Clarifying questions**

- What payment terms should I use for this Bill/PO?
- Do you have a specific reference number for this document?
- Which currency and market should I use? (defaults: USD / US)

**How to help**

- ALWAYS populate the 'name' field with a descriptive title — never leave it blank.
- ALWAYS set date fields — default to current date if user doesn't specify.
- ALWAYS set currency (default USD) and market (default US) explicitly.
- For Bills and Inventory Orders, ALWAYS include payment_term_id — look it up via project_management_list_payment_terms if not provided.
- Use the correct pricing key: Bills/Adjustments use 'rate', Inventory Orders use 'price', Invoices use 'amount'.
- Always include quantity on line items — default to 1 for flat-fee services.

**Don't claim**

- Do not leave the 'name' field blank on any document.
- Do not leave date fields blank — always default to current date.
- Do not create Bills or Inventory Orders without a payment_term_id.
- Do not use 'amount' as the pricing key for Bills (use 'rate') or 'rate' for Invoices (use 'amount').

**Escalate when**

- User reports that a created document is missing fields or has incorrect data
- Payment requests were not generated because payment_term_id was missing

## Creating a Bill

### Procedure

1. Complete the Pre-Creation Checklist above.
2. Resolve `vendor_id` using `project_management_list_vendors`.
3. Resolve `service_id` for each line item using `project_management_list_services`.
4. Resolve `payment_term_id` using `project_management_list_payment_terms`. This is REQUIRED — do not skip it.
5. If linking to source documents (Inventory Orders, Shipments, Assembly Orders), include them in the `documents` array as `{type, id}` objects.
6. Build the payload:
   - `name`: descriptive title (e.g., "Vendor Name - Q2 Freight Services - <Month Year>")
   - `ref_number`: user-provided code
   - `date`: current date if not specified
   - `currency`: "USD" (default)
   - `market`: "US" (default)
   - `vendor_id`: resolved ID
   - `payment_term_id`: resolved ID
   - `details`: array of `{service_id, quantity, rate}` objects
7. Call `project_management_create_bill`.

## Creating an Inventory Order (Purchase Order)

### Procedure

1. Complete the Pre-Creation Checklist: identify the document type and resolve every
   entity ID before building the payload.
2. Resolve `vendor_id` using `project_management_list_vendors`.
3. Resolve `warehouse_id` using `inventory_list_warehouses` (the destination warehouse).
4. Resolve `inventory_id` for each line item using `inventory_list_items`. Each item
   must reference an inventory record belonging to the same company.
5. Resolve `payment_term_id` using `project_management_list_payment_terms` if payment
   terms apply. This is optional and nullable — do not block creation on it.
6. Build the payload:
   - `name`: descriptive title, never blank (e.g. `"Vendor Name - Inventory Restock - <Month Year>"`)
   - `ref_number`: the user-provided PO reference. **Create-only** — it is ignored on
     update, so there is no correcting it later. Confirm it before creating.
   - `date_order_placed`: the date the PO was placed with the vendor (stored as
     `po_date`). Default it if the user does not specify one.
   - `date_manufacturing_completed`: the date manufacturing finished and goods were
     received (stored as `date_order_received`). **Never default this** — see below.
   - `currency`: ISO 4217, three letters. Default `"USD"`.
   - `market`: ISO 3166-1 alpha-2, two letters. Default `"US"`.
   - `vendor_id`, `warehouse_id`, `payment_term_id`: resolved IDs
   - `details`: array of `{inventory_id, quantity, price}` objects
7. Read the payload back to the user for confirmation before calling the tool. Show
   the manufacturing-completed line explicitly, including when it is empty.
8. Call `project_management_create_inventory_order`.
9. Report what the tool returned, including the order ID and `link`. Never describe
   the order as created unless the tool confirmed it.

### The two dates mean different things

- **`date_order_placed`** — when the purchase order was placed. Administrative. Safe
  to default to the current date.
- **`date_manufacturing_completed`** — when manufacturing finished and the goods were
  received. Setting it tells NeonPanel the stock on this order has been
  **manufactured** and is real, available inventory. Leaving it off a completed order
  means planning, valuation and landed cost behave as though the goods were never
  made, and the order drops out of date-range queries, which filter on this field.

Ask the user directly: *"Has manufacturing been completed on this order yet? Setting
that date is what tells NeonPanel the stock has been manufactured."* If the answer is
no or unknown, leave it null deliberately and say so, so they know to set it later.

### Facts

- `payment_term_id` is optional. When set, the server auto-generates payment request
  installments from the term's schema.
- `ref_number` is accepted on create only; it is ignored on update.
- `market` is resolved server-side from the country code to a marketplace the
  platform knows.

### Escalate when

- A `400` is returned — the manufacturing-completed date is earlier than the
  company's Entry Locked Date. Report it and ask whether the date is wrong or the
  lock needs moving. Do not retry blindly.
- A `422` is returned — input validation failed. Name the offending field.

## Updating a document — the three replace traps

### What it is

Updating a NeonPanel document is a sparse patch — fields you omit keep their current
values. Three fields break that expectation, and two of them destroy data silently.

### 1. `details` is a full replace, not a merge

Sending the `details` array **deletes every existing line item and re-creates it**
from exactly what you sent. Sending only the line you changed deletes all the others.

**Procedure for any line-item change:**

1. Read the current document first (`project_management_get_inventory_order`, or the
   equivalent getter for the document type).
2. Take the complete existing `details` array.
3. Apply the change to that full list — edit, add or remove within it.
4. Send the **entire** array back.

Tell the user you are resending the full line-item set, so a partial list is never
mistaken for a partial update.

### 2. `ref_number` is create-only

On Inventory Orders, `ref_number` is accepted during create and **silently ignored on
update**. There is no correcting it afterwards through the update call. Confirm it
with the user before creating, and if they ask to change it later, tell them plainly
that the field cannot be patched rather than sending an update that appears to
succeed and changes nothing.

### 3. `payment_term_id` re-syncs the installment schedule

Sending `payment_term_id` on update re-syncs the payment requests against that term
rather than rebuilding them:

- Installments the new term still needs are left untouched, **including any edits
  made to them**.
- Installments the new term no longer needs are deleted.
- Sending the term the document already has changes nothing.

**To change a single installment** — its paid amount, payment date, transaction
number or memo — use the payment request update tool. Do not resend the payment term
to try to fix one installment; it will not do what you want and may delete others.

### Procedure

1. Read the document before updating it.
2. State which fields you are changing, and get confirmation.
3. If `details` is involved, say that the full line-item set is being resent.
4. Report what the tool returned. Never describe an update as applied unless the tool
   confirmed it.

### Escalate when

- The user asks to change a `ref_number` after creation.
- An update returns `400` (a date precedes the company's Entry Locked Date) or `422`
  (validation failure) — name the field rather than retrying.

## Pre-Creation Checklist (ALL Document Types)

### Procedure

Before creating ANY NeonPanel project document (Bill, Invoice, Inventory Order,
Shipment, or Adjustment), complete these steps in order:

1. **Identify the document type** the user needs (Bill, Invoice, Inventory Order,
   Shipment, Adjustment).
2. **Resolve ALL entity IDs** using list/search tools:
   - `project_management_list_vendors` → `vendor_id` (for Bills, Inventory Orders)
   - `project_management_list_services` → `service_id` (for Bills)
   - `inventory_list_items` → `inventory_id` (for Inventory Orders, Invoices, Adjustments, Shipments)
   - `inventory_list_warehouses` → `warehouse_id` (for all document types except Bills)
   - `project_management_list_payment_terms` → `payment_term_id` (Bills and Inventory Orders)
   - For Invoices: resolve `customer_id` and `sales_channel_id`
3. **Never skip ID resolution.** If the user provides a name string (e.g.
   "Vendor: Example Co"), you MUST look up the actual system ID before creating the
   document. Passing raw strings will fail.
4. **If any required ID cannot be found**, tell the user and ask for clarification.
   Do not create the document with missing IDs.

### Payment terms are optional, not required

`payment_term_id` is nullable on Inventory Orders — a document can be created
without one. Do not block creation waiting for it, and do not invent one.

When it **is** set, the server auto-generates the payment request installments from
that term's schema. So ask whether payment terms apply rather than omitting it
silently, and tell the user that setting it will generate the installment schedule.

### Don't claim

- Do not claim payment terms are mandatory.
- Do not create a document with unresolved IDs in the hope the server will match a name.
