# NP Catalogs: Services

Category: Catalogs · Article 1035 · Updated 2026-02-25

The Services Catalog in NeonPanel is a fundamental tool for managing and categorizing service expenses, that ensures precise accounting.

## Introduction

The Services Catalog in NeonPanel is crucial for managing various expenses used in the Bills & Expenses process. It ensures precise accounting and supports strategic business decisions.

## Integration with Accounting Software

NeonPanel integrates with QuickBooks Online (QBO) for seamless data exchange. NeonPanel can import from QBO into and export from NeonPanel to QBO, with options to toggle synchronization and set a start date. More details are available in our QBO Settings article.

## Auto-Generated Service Items

During onboarding, NeonPanel creates a Service Item list in the Services Catalog, including all necessary Amazon services and landed cost services.

## Amazon Services

NeonPanel creates Amazon services from line items in client Statements issued by Amazon. The service name formula is:

Service Name = 'Amazon' + "amount description" + "transaction type"

For more on service names and data flow, see the Accounting Data Flow article.

## Landed Cost Services

Landed cost services are tied to shipments in NeonPanel, causing inventory items in those shipments to appreciate. The bill amount is distributed based on the allocation base of the Service Group linked to the chosen service (e.g., weight, volume, amount, count, customs duty). For detailed instructions, see Create Landed Cost Bills.

## Service Account Connections

Each service is linked to a specific account from the user's chart of accounts. When chosen, the associated account creates Journal Entries.

## Managing Amazon Global Logistics (AGL) Expenses

For managing Amazon Global Logistics (AGL) expenses, refer to the Managing Amazon Global Logistics (AGL) Charges article.

## Adding Custom Services

You may at some point want to add a Custom Service, which isn't included in the default services provided by NeonPanel.

To add custom services:

1. Navigate to the Catalogs tab in the main navigation menu.

2. Select Services Catalog.

3. Click "Add service".

For landed cost services with granular analytics, select the appropriate parent account in the “Landed Cost” field: Customs, Freight, Brokerage, Inspections, or Others. For non-landed cost services, leave the “Landed Cost” field empty. Fill out the remaining fields and click “Save”.

These services will now be available in the Bills & Expenses projects for selection.
