# NP Catalogs: Inventory

Category: Catalogs · Article 1050 · Updated 2026-02-25

NeonPanel's inventory catalog is essential for accurate analytics. It provides an overview of all inventory items with specific parameters for easy identification, management, and improvement.

## Introduction

Inventory catalog in NeonPanel is one of the most important catalogs there is, alongside the Products catalog. In fact, understanding the difference between the two is the first thing we’ll focus on.

## Inventory vs Product

Products in NeonPanel represent specific SKUs that are offered for sale by specific vendors. Blue socks offered by XYZ corporation could be a Product in your Products catalog. However, Products don’t have any analytics attached to them, they are abstract entities.

To sell a specific product on Amazon, you need to create an inventory item and then an Inventory Order to account for the ordered inventory. To create an inventory item, you must first choose a product on which the item will be based.

## Why do you need both?

At first, NeonPanel had only the Products catalog. The thought behind it was that using SKUs (which is the primary identifier of products in NeonPanel) would allow us to uniquely identify products sold on Amazon.

However, we quickly realised that the same SKU can exist for two different listings and/or marketplaces. Because we needed a way to always uniquely identify an item based on Amazon reports, we created an inventory item to do just that.

When two different items and the same SKU are sold on two different marketplaces, and/or by different companies, having an inventory catalog (and clever algorithms) allows NeonPanel to pinpoint exact inventory items for you, so that your analytics are as accurate as possible.

Items sold on different marketplaces will have different associated transportation and other costs. This is important, because NeonPanel helps you keep track of your costs, as well as your expenses, and pinpointing items accurately every time is essential to accurate analytics.

Note: when you activate your Amazon Seller Central Integration, both products and inventory catalogs will be populated automatically. You can always add products and inventory items manually, if you need to.

## Inventory Catalog Overview

The inventory catalog provides an essential overview of all inventory items, marked with specific parameters for easy identification and management. These include:

Product Name: This refers to the name of the product upon which the inventory item is based.

Verified: This column indicates whether or not NeonPanel has been able to verify this inventory on Amazon.

Planning: Offers quick access to inventory planning information associated with that particular item.

Inventory ID: Unique identifier of an inventory item within NeonPanel.

SKU, Listing, ASIN, FNSKU: Amazon identifiers of an inventory item.

Tags and Manager: Classification parameters that can be used to group and filter reports and dashboards, facilitating data analysis in a way that aligns with your specific needs. Detailed information on the usage of these Catalog classifiers, including steps on how to create and assign them, can be found in this article.

Active: Indicates the status of the inventory item.

## Enhancing Inventory Catalog

Upon the initial setup of your NeonPanel account and subsequent linkage with your Amazon account, your product listings, inventory, and other catalogs will be seamlessly imported. However, Amazon doesn't provide key details like product-associated costs and vendor information.

To fully exploit your analytics, it's crucial to supplement this missing data. This can be achieved by utilizing the Inventory List Parameters Export. This process includes downloading the template, populating missing data, and re-uploading the document to NeonPanel.

For more detailed information on enhancing your Inventory Catalog, please refer to this article.
