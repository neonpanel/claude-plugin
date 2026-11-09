# Amazon Inventory Ledger report explanation

Category: Inventory Management · Article 1401 · Updated 2026-02-25

The article explains how to reconcile inventory balances by breaking them into key components—starting/ending warehouse counts, items in transit, and transfer adjustments. It details the formulas for tracking changes (e.g., through shipments) and emphasizes that the  "In Transit Between Warehouses" column shows items still in transit at day’s end.

## Introduction

NeonPanel automatically downloads Amazon Inventory Ledger report in a daily basis to provide user possibility to analyse this historical data in a readable, easy to operate format. Below you can see documentation described report options. In NeonPanel -> Reports sections we can find this info in FBA Inventory report, FBA Starting balances report, FBA In Transition report. Also this data is used to built Inventory Audit report.

To manually review your inventory or download adjustments online, follow these steps:

Open the Inventory Ledger report and click View Online.

Select either Summary or Detailed view, then choose your desired date range from the Date Range drop-down menu.

Filter your results by entering Merchant SKU, Fulfillment Network SKU, ASIN, or any combination of these identifiers.

Click Generate Report.

Note: If there are more than 50 adjustments, the report will span multiple pages.

https://developer-docs.amazon.com/sp-api/docs/report-type-values-fba

In NeonPanel all good sent to FBA are considered belonging to Amazon FBA US (or other marketplace) warehouse. So, goods with is "in transit  between fulfilment center" are included in the adjusted balances.

## Inventory Balance Breakdown Explanation

Below is an explanation of how the various balance columns relate to one another and to the event columns. In this dataset, the overall inventory balance is broken down into components representing the location of items (either in the warehouse or in transit) and adjustments due to transfers and other events. The relationships are described below.

## Starting Balance

The overall Starting Balance consists of three parts:

Starting Warehouse Balance:The count of items physically present in the warehouse at the beginning of the period.

In Transit Between Warehouses:Items that are in the process of being transferred between warehouses.

Warehouse Transfer In/Out:Adjustments due to items being transferred that are not yet reflected in the warehouse count.

Mathematically:

Starting Balance = Starting Warehouse Balance + In Transit Between Warehouses + Warehouse Transfer In/Out

For example, in the first row:

Starting Warehouse Balance = 2427

In Transit = 34

Warehouse Transfer In/Out = 100

Thus, 2427 + 34 + 100 = 2561, which matches the starting balance.

## Ending Balance

The overall Ending Balance consists of:

Ending Warehouse Balance:The count of items physically present in the warehouse at the end of the period.

In Transit Between Warehouses:This value represents the number of items that are in transit at the end of the day.

Mathematically:

Ending Balance = Ending Warehouse Balance + In Transit Between Warehouses

For example, in the first row:

Ending Warehouse Balance = 2527

In Transit = 34

Thus, 2527 + 34 = 2561, which matches the ending balance.

## 2. How the Components Change

Note: The value for In Transit Between Warehouses represents the items in transit at the end of the day and can vary from day to day depending on completed or pending transfers. The warehouse balance is the primary value that changes (and, where applicable, adjustments due to warehouse transfers are applied at the beginning). For a better understanding, consider the “adjusted” starting warehouse balance as:

Adjusted Starting Warehouse Balance = Starting Warehouse Balance + Warehouse Transfer In/Out

The difference between this adjusted starting warehouse balance and the Ending Warehouse Balance reflects the net change in the warehouse during the period.

For example, in the second row:

Adjusted Starting Warehouse Balance = 2527 + 0 = 2527

Ending Warehouse Balance = 2522

Net Change = 2522 - 2527 = -5

This difference (-5) corresponds exactly to the value in the Customer Shipments column, indicating that shipments out of the warehouse resulted in a decrease in inventory.

## 3. Event Columns and Net Change

Additional columns (such as Receipts, Found, Disposed, Lost, Vendor Returns, Customer Returns, Customer Shipments, Damaged, Unknown Events, and Other Events) record various transactions that impact the inventory.  We omit it here.

## 4. Summary of Relationships

Overall Starting Balance:Starting Balance = Starting Warehouse Balance + Warehouse Transfer In/Out + In Transit Between Warehouses

Overall Starting Balance:Starting Balance = Starting Warehouse Balance + Warehouse Transfer In/Out + In Transit Between Warehouses

Overall Ending Balance:Ending Balance = Ending Warehouse Balance + In Transit Between Warehouses

Overall Ending Balance:Ending Balance = Ending Warehouse Balance + In Transit Between Warehouses

Net Change in Inventory:Net Change = Ending Balance - Starting Balance

Net Change in Inventory:Net Change = Ending Balance - Starting Balance

In these records, the net change is determined by the difference in the warehouse balances (after accounting for transfers) and aligns with the recorded event values (such as Customer Shipments etc.).

## Useful links:

1.  Detailed View column explanation

2. Summary view column explanation

3. Inventory adjustment reason codes and their type.
