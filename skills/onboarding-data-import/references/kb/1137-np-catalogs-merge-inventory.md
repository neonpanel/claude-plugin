# NP Catalogs: Merge Inventory

Category: Catalogs · Article 1137 · Updated 2026-02-25

This article explains what inventory parent functionality is, and how to use it.

## Introduction

NeonPanel automatically creates inventory items based on several reports we import from Amazon on your behalf via your Amazon SPI Integration.

However, since we are often importing historical data, we might automatically create inventories for old items which will often have old SKUs and FnSKUs. In order to keep your catalogs clean, you can create parent-child relationships between your inventory items. You can think of this as 'merging' of the inventory items.

Note: this can always be reversed by deleting the relation between the two items.

Once you do that, any time either version is reported by Amazon, we will attribute that information to the parent item. This will allow you to keep continuity in your analytics, even if you change packaging or create a different listing and re-list your item on Amazon.

## How to assign a parent to an inventory item

In your Inventory catalog you need to locate the two inventories you want to 'merge'. In our example below, I want to assign a parent to 'Old Red Socks'.

Next, click on the 'child' item first. In the popup window, the SKU of the item you want to add as a parent to this item. I will use the SKU of the 'Red Socks' item in the search bar of the dropdown window and choose that item.

After that, just scroll down and press Save. In the The system will warn you that by merging these items, all the transactions from a child will be transferred to a parent. Press Yes -- and you're done!
