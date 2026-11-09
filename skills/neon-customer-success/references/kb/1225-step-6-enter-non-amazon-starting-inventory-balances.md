# Step 6: Enter Non-Amazon Starting Inventory Balances

Category: Initial Setup · Article 1225 · Updated 2026-02-25

This article guides users through providing starting inventory balances and associated landed costs for non-Amazon warehouses to ensure accurate data migration and inventory management in NeonPanel.

## Introduction

In this step, you will provide the starting inventory balances for all your non-Amazon (also referred to as 3PL) warehouses, along with the associated landed costs. This step is crucial for correctly migrating the quantities and costs into NeonPanel from your previous inventory management system.

## Purpose and Next Steps

This step is a vital part of the Initial Data Migration process, which works best when done holistically. Accurate data regarding the starting inventory balances and landed costs for all your non-Amazon warehouses is essential for maintaining accurate inventory records and financials.

By ensuring that all data is accurately entered and uploaded, you enable NeonPanel to calculate transactions effectively from the Start Date, providing a seamless transition and reliable inventory management.

## Process Overview

1. Download the Template: Navigate to the Setup & Settings page in your NeonPanel account, then to Data Uploads section, and find the relevant template under 'Inventory Orders,' and click the 'Download Template' blue button.

2. Fill Out the Document: Enter the required data into the template. This includes:

- Inventory balances for all non-Amazon warehouses as of the Start Date.

- Landed costs associated with these inventory items.

- Inventory orders that are still in production, with the 'date-order-received' column left empty.

Click the 'Download Instructions' button to download a file with upload instructions.

3. Upload the Completed Template: Re-upload the completed document back into NeonPanel

## Important Considerations

Date Order Received: This field specifies when transactions are created. For the migration process, use the chosen Start Date (yyyy-mm-dd format). For inventory orders still in production, leave this column empty.

For inventory already transported to a different country (e.g., from China to a 3PL warehouse in the US), include all landed costs. However, for inventory still in warehouses at your suppliers' warehouses, include only the manufacturing costs. NeonPanel will calculate the landed costs for these products as they are incurred (when invoices for shipping are issued and customers enter them into NeonPanel using the Bills process).
