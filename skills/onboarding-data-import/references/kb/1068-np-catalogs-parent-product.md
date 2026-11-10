# NP Catalogs: Parent Product

Category: Catalogs · Article 1068 · Updated 2026-02-25

Explore NeonPanel's 'Parent Product' feature: a solution for handling changed SKUs and ensuring consistent analytics.

## Introduction

In NeonPanel's Product catalog, each item has a "Parent Product" feature. When NeonPanel looks up an item by its SKU, it first checks if this SKU has a linked parent product. If there's a parent product, the system uses its SKU instead.

This feature makes it easy for NeonPanel to group data from different SKUs together for streamlined analysis. For example, if you work with a third-party warehouse that changes the SKU, the system might think you have two different products. But, by setting the original product as the parent of the new SKU, both will be seen as the same product in the analytics.

Beyond just warehouse SKU adjustments, this feature offers multiple advantages. For instance, when Amazon generates unique SKUs, such as Uncommingled.MSKU.1631179819318, in their reports, NeonPanel might struggle to associate it with the correct inventory, thereby impacting the accuracy in stock levels, landed costs and COGS. By associating these unique SKUs to a parent product, you ensure data accuracy, effectively sidestepping potential reporting issues.
