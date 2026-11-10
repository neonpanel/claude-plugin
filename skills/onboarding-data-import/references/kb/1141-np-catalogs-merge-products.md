# NP Catalogs: Merge Products

Category: Catalogs · Article 1141 · Updated 2026-02-25

This article explains what mering of products means, and how to do it.

## Introduction

NeonPanel automatically creates product items based on several reports we import from Amazon on your behalf via your Amazon SPI Integration.

Sometimes, Amazon creates strange-looking SKUs like 'FBAID.Missing' or 'amazon.gr.SKU..' and so on. They often report these separately in your Statements or in the Ledger, which triggers the creation of such products (and sometimes inventory) in your NP catalog.

You may not want to see sales or inventory movements separately for these items anywhere in NeonPanel. In order to achieve this, you can 'merge' products by creating parent-child relations between them.

Once you do that, whenever an SKU of the child product appears in the system, NP will treat it as if it had the parent's SKU.

## How to create parent products

In your Product catalog, find the SKUs you want to merge. In the example below, I want to merge the two SKUs together, so I click on the top one which was clearly automatically created by Amazon.

After that, I provide the SKU of a parent product in the parent product search field, and select the product I want to assign as a parent.
