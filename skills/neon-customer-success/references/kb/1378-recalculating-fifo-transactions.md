# Recalculating FIFO Transactions

Category: Inventory Management · Article 1378 · Updated 2026-02-25

NeonPanel’s Inventory Management (IM) system records all inventory movements and costs using FIFO, with recalculations needed for changes to older documents to maintain accuracy.

## Introduction

NeonPanel’s Inventory Management (IM) system keeps precise records of all inventory movements and cost tracking. Using the First-In, First-Out (FIFO) method, NP ensures accurate costing based on the sequence of inventory receipt and usage.

When a task in a relevant document triggers an IM transaction, only that specific document's data is calculated, excluding surrounding documents. This approach is efficient for managing recent orders, shipments, and bills.

However, if an older document (e.g., from three months ago) is retroactively modified, it’s essential to perform a full recalculation. This recalculation updates subsequent documents that generate transactions after the modified document’s date, preserving accuracy across the system.

To initiate a recalculation:

Navigate to the Set Up and Settings page (under your avatar if you have the required permissions).

Go to locate the Inventory Transactions section, and click Recalculation.

Depending on your needs, you can choose to recalculate from:

The date of the latest modified document.

The Start Date of your account in NeonPanel.
