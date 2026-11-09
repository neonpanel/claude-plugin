# Step 2: Optimize Warehouses & Vendors Catalogs

Category: Initial Setup · Article 423 · Updated 2026-02-25

Next up, you will need to optimize your warehouses and vendors catalogs in NeonPanel

## Introduction

When setting up NeonPanel for the first time, it is important to optimize your warehouses and vendors catalogs. This will ensure that inventory management and analytics work to their full potential.

## Merging Warehouses

When you first set up your Amazon integration, NeonPanel will import your FBA Shipments data and automatically create warehouses based on that information.

It is important to note that sometimes Amazon has variations of addresses for the same warehouse, and NeonPanel will create separate warehouses for each address (even an extra comma or a number will result in multiple warehouses).

You need to merge these warehouses before migrating your other information, as warehouses are crucial to our FIFO system which helps calculate landed costs and COGS automatically.

After adding all of your warehouses and merging 'duplicates', you might want to import 3PL inventory balances and set up a procedure to capture them every once in a while. Our customers often opt for an upload once every week or month.

Note: at this stage it will be useful for you to decide whether you want to simplify your inventory management process with a Supplier Forwarding Warehouse. If you do, create a 'Supplier Forwarding' warehouse, and merge your individual supplier warehouses to it. This process is irreversible.

## Vendors Catalog

Vendors are important for managing Inventory Orders and Bills in NeonPanel. When you first create your company in NeonPanel, your vendor catalog will be empty. Therefore, you need to populate it first.

Note: if you activate your QBO or Xero integration, NeonPanel will automatically import vendors from your records.

To start, add your vendors' profiles by navigating to the Vendors catalog and clicking Add vendor.

There are 3 tabs to fill out, but only the main one is required. The rest are for your convenience. Adding vendors will enable you to select them in the appropriate fields in Inventory Order and Bill processes.
