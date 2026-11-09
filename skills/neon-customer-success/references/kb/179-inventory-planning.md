# Inventory Planning

Category: Reports · Article 179 · Updated 2026-02-25

Inventory Planning report provides Amazon sellers with a comprehensive tool to plan their inventory levels and avoid stockouts.

## Introduction

At NeonPanel, we don't want to forecast inventory levels that will be required at some point in the future. We would rather plan, based on what we know about our marketing efforts, seasonality and market conditions.

That's why we offer an Inventory Planning report to help ecommerce sellers plan their inventory levels and never run out of stock. This dashboard not only helps you maximize the potential of each listing to reach its highest organic position, but also to increase the effectiveness and profitability of your business as a whole.

In order to use this report, you first need to fill out Order Cycle and Min Qty to Order fields in your inventory catalog for all inventory items. Read how to Enhance your Inventory Catalog here.

Let's take a closer look at the Inventory Planning Dashboard and how it can help you.

There are 3 tabs in this dashboard:

KPI & Filters

FBA Inbound Shipment Schedule

Inventory Order Schedule

## Purpose of the Tab

The first tab provides a high-level overview of your inventory information. It is useful to understand the overall inventory levels your business has as well as which products or product families have the highest inventory turnover. The dotted line delineates our benchmark for an optimal turnover rate: six times annually.

The visual analytics are accompanied by a table that allows you to dive deeper into specific metrics.

## Controls (Part 1)

Apart from choosing relevant dates and, if applicable, specific companies, you can customize your view based on particular inventory orders or inventory IDs. Key controls to note: Lead Time: Adjust between 1 and 360 days to reflect the average time a product takes to move through the entire supply chain. Total Safety Stock: Can be set from 30 days to 270 days, this acts as a buffer against unforeseen demand changes, and determines the absolute minimum quantity that must be held on hand at all times to avoid stock-outs. Note: these are 'fall-back' settings, and will be applied to those inventory items that don't have specific values set in the inventory catalog. Read more in our Inventory Parameters Explained article. Planning Variant:You have two options for guiding NeonPanel's inventory calculations: By Actual Sales Data: Choose this option for NeonPanel to base calculations on your Amazon account's historical sales data. For instance, when determining the quantity of inventory needed for a 30-day safety stock. By Actual Sales Data: Choose this option for NeonPanel to base calculations on your Amazon account's historical sales data. For instance, when determining the quantity of inventory needed for a 30-day safety stock. By Target Sales Data: Opt for this if you prefer NeonPanel to use the 'target sales' figures you've specified in the inventory catalog for each item. By Target Sales Data: Opt for this if you prefer NeonPanel to use the 'target sales' figures you've specified in the inventory catalog for each item. Include Inbound

## Controls (Part 2)

Quantity in Available Balance? This control allows you to decide whether to include the stock quantities from inbound shipments, created in your Amazon account, that Amazon hasn't yet received, in your available balance. In some cases, you might have initiated an inbound shipment but not dispatched the inventory from a third-party warehouse. This can lead to NeonPanel counting the same stock twice. Pro Tip: To avoid the double-counting scenario mentioned above, consider importing inventory balances as if the stock from the inbound shipment has already been dispatched from your third-party warehouse. You can be creative here. For example, if you have 3 different SKUs sold on a single ASIN (parent ASIN with many variations), you may choose SKU as a Planning Unit, but filter by a specific ASIN, you will get detailed information about every SKU sold under that ASIN.

## FBA Inbound Shipment Schedule

This tab shows you when FBA shipments should be sent to Amazon from your 3rd party warehouses in order to avoid running out of stock. Each box on the graph represents a shipment that should be created in Amazon.

FBA Shipments - once created in Amazon - get automatically synced with NeonPanel, capturing all the relevant information. You can check this information in Amazon on in the relevant FBA Shipment project.

You can also view the FBA Out of Stock Date in the table below, which reflects when the inventory for the planning unit will run out given the current sales velocity and inventory levels in the Amazon Warehouses.

Daily Unit Sales Target comes from the Inventory Catalog → Inventory profile.

## Inventory Order Schedule

Finally, the Inventory Order Schedule tab helps you plan your future inventory orders.

It takes into account factors such as current sales velocity, inventory levels in Amazon warehouses, and lead time for each product to suggest the optimal dates and quantities for reordering. This ensures that you have enough inventory to meet customer demand without overstocking.

Note: the Lead Time (Order Cycle) comes from the Inventory Catalog → Inventory profile

## Important Setting: Show Items

The "Show Items" setting allows you to determine which planning units are displayed in the tabs. You have three options:

Show only items for which the Sales Target metric is provided in the catalog

Show actively sold items as well: this shows all the products as the previous setting, but also those that are sold actively (20+ sales per week)

Show all items
