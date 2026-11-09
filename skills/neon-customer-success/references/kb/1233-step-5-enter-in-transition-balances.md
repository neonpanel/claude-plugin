# Step 5: Enter In Transition Balances

Category: Initial Setup · Article 1233 · Updated 2026-02-25

This article guides users through providing in transition inventory balances and associated landed costs for non-Amazon warehouses to ensure accurate data migration and inventory management in NeonPanel.

## Introduction

In this step, you will import the balances for inventory that was in transition to Amazon at the Start Date. This includes FBA Shipments which had been sent but not yet received at Amazon fully as of the Start Date. This ensures that NeonPanel accurately tracks inventory that was shipped but not yet fully received by Amazon as of the Start Date.

## Purpose and Next Steps

This step is part of the comprehensive Initial Data Migration process, essential for ensuring all your inventory data is accurately represented in NeonPanel. By completing this step, you enable NeonPanel to accurately calculate transactions from the Start Date, ensuring seamless inventory and financial management.

## Process Overview

Open the Inventory In Transition Report: Access the report via this link: Inventory In Transition Report. Open the Export PO tab. Make sure to select the correct Start\Transition Date. Your Shipped At starting from date should be specified at 6 months before the Start Date.

Open the Export PO tab. Make sure to select the correct Start\Transition Date. Your Shipped At starting from date should be specified at 6 months before the Start Date.

Download the table from Export PO Tab: Download the table as a CSV file from the Export PO tab.

Provide Landed Costs: Ensure the landed costs are filled in the 'price' column.

Upload the Completed Template: Re-upload the completed CSV file back into NeonPanel.

## 1. Open the Inventory In Transition Report

Access the report by navigating to the following link: Inventory In Transition Report.

## 2. Review the Main Tab

Navigate to the Main tab within the report.

This tab shows each FBA shipment, detailing how much stock was received prior to the Transition Date (which should be the Start Date) and how much was received after.

The Shipped At Starting From date determines the latest date a shipment was shipped to be considered in this report. We recommend setting this date at least 3 months prior to the Transition/Start Date to ensure all relevant shipments are included.

This report uses extensive Ledger information provided by Amazon and processed by NeonPanel to show the received and in-transit quantities accurately.

## 3. Download the Export PO Tab

Navigate to the Export PO tab within the report.

Download the table as a CSV file. This tab includes only the stock received after the Start Date, since stock received prior to the Start Date would have been included in Step 4.

## 4. Provide Landed Costs

Open the downloaded CSV file.

Review the data and ensure all necessary fields are filled out, particularly the 'price' column.

For inventory in transition, include the full landed costs (manufacturing costs, freight, brokerage, customs duties, etc.).

## Key Fields in the Template:

Date Order Received: This field specifies when transactions are created. Use the chosen Start Date (yyyy-mm-dd format). This will be pre-filled by NP.

Warehouse: This field specifies which warehouse to put this inventory on. In this case the stock needs to be put on the warehosue where the FBA Shipment is taking it. This will be pre-filled by NP.

Price: Enter the comprehensive per unit landed cost for each inventory item received after the Start Date.

## 5. Upload the Completed Template

Save the completed CSV file.

Navigate back to the Setup & Settings page in NeonPanel, then to the Data Uploads section.

Upload the filled-out template using the provided upload functionality under the Inventory Order section.

By completing Step 5, you ensure that NeonPanel accurately reflects your in-transit inventory balances. This step, along with the others, provides a comprehensive approach to inventory data migration, ensuring reliable and efficient inventory management from the Start Date onward.
