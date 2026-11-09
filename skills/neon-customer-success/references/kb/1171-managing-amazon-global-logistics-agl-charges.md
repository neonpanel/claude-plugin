# Managing Amazon Global Logistics (AGL) Charges

Category: Accounting · Article 1171 · Updated 2026-02-25

Accurately handling FBA International Freight charges is crucial for precise financial reporting and inventory management in NeonPanel. By following these steps, you ensure that all associated costs are appropriately recorded and reflected in your financial statements, maintaining the integrity of your financial data.

## Introduction

This guide explains the step-by-step process for effectively managing and recording FBA International Freight Duties, Taxes, and Shipping charges within NeonPanel to ensure accurate cost allocation and reporting in QuickBooks Online (QBO).

We suppose that you include this charges in inventory landed cost (what we recommend to do). Otherwise just ignore this information.

## Step 1. Setup of AGL Clearing Account and Vendor in QBO.

Create AGL Clearing Account:Go to the Chart of Accounts in QBO.Create a new account named 'AGL Clearing Account' under the 'Inventory Assets' group.

Create AGL Clearing Account:

Go to the Chart of Accounts in QBO.

Create a new account named 'AGL Clearing Account' under the 'Inventory Assets' group.

Add Vendor - AGL Services:Navigate to the Vendors section in QBO.Add a new vendor named 'AGL Services'.

Add Vendor - AGL Services:

Navigate to the Vendors section in QBO.

Add a new vendor named 'AGL Services'.

Wait 1-5 min, this new items should be synced with NeonPanel automatically.

If it wasn't synced automatically, you have possibility to force it manually here: Menu -> Quickbooks -> Settings

## Step 2. Set Up Custom FBA Freight Services.

In NeonPanel, Amazon FBA International Freight Duties and Taxes Charge other-transaction and Amazon FBA International Freight Shipping Charge other-transaction services are created automatically, as soon as they appear in your statement first time, to specifically categorise various expenses related to FBA International Freight, such as duties, taxes, and shipping costs. This will help in accurately tracking and categorising these specific costs.

Assign the 'AGL Clearing Account' as the default account for all FBA Freight services, which you are going to enter in a landed cost Bill . This ensures that all entries related to FBA International Freight are consistently recorded under this sub-account.

## Step 3. Set Up Default account for  'AGL Services' vendor.

In NeonPanel open Menu -> Catalog -> Vendors, find 'AGL Services' vendor and chose 'AGL Clearing Account' in account column. That's it.

You don't need set up account for each vendor. If you left it blank, NeonPanel uses Account Payable (A/P) account from Transaction Map

## Record AGL Transactions in Bills

Anytime you receive invoices from 'AGL Services' with amount specified by FBA Inbound Shipment, you have to enter one or more Bills and connect it to relevant shipment. You can connect one Bill to multiple shipments or one Bill to each shipment. You decided how you want it. NeonPanel is capable to allocate cost accurate among multiple shipments.

Apply the landed cost service you normally use to allocate additional costs: freight, duties and taxes etc.

## Example Entries

Amazon Statement Journal Entry:Debit AGL Clearing Account: $2,500Credit Account Receivable (A/R): $2,500This entry reflects the initial recognition of the landed cost related to the asset purchase.

Amazon Statement Journal Entry:

Debit AGL Clearing Account: $2,500

Credit Account Receivable (A/R): $2,500

This entry reflects the initial recognition of the landed cost related to the asset purchase.

Bill Entering Landed Cost :Debit Inventory (or relevant Asset Account): $2,500Credit AGL Clearing Account: $2,500This entry allocates the landed cost to the appropriate asset account, clearing the temporary holding.

Bill Entering Landed Cost :

Debit Inventory (or relevant Asset Account): $2,500

Credit AGL Clearing Account: $2,500

This entry allocates the landed cost to the appropriate asset account, clearing the temporary holding.

## Importance of Accurate Transaction Recording

Control Point:The 'AGL Clearing Account' serves as a control point to ensure all FBA Freight bills are recorded.Without the creation of these bills, NeonPanel lacks the data necessary for accurate SKU-level cost allocation.

Control Point:

The 'AGL Clearing Account' serves as a control point to ensure all FBA Freight bills are recorded.

Without the creation of these bills, NeonPanel lacks the data necessary for accurate SKU-level cost allocation.

Without the creation of these bills, NeonPanel lacks the data necessary for accurate SKU-level cost allocation.

Impact on Financial Statements:Cost of Goods Sold (COGS): Incorrect recording of FBA Freight charges can lead to inaccuracies in COGS.Inventory Asset Discrepancies: There may be discrepancies between the Inventory Assets values reported in QBO and NeonPanel if FBA Freight costs are not included.

Impact on Financial Statements:

Cost of Goods Sold (COGS): Incorrect recording of FBA Freight charges can lead to inaccuracies in COGS.

Inventory Asset Discrepancies: There may be discrepancies between the Inventory Assets values reported in QBO and NeonPanel if FBA Freight costs are not included.

By following these steps, you ensure that your FBA International Freight costs are accurately tracked, allocated, and reported in QuickBooks Online, supporting precise financial management and reporting.
