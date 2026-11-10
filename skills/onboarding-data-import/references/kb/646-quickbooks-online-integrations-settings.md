# QuickBooks Online Integrations Settings

Category: QuickBooks Integration · Article 646 · Updated 2026-02-25

In NeonPanel, you can control how data is synchronised with QuickBooks Online. In this article we will cover the settings you have and what they do.

## Introduction

In this article we will go through the settings you can toggle on your QuickBooks Online integration page, and what they mean.

If you have an active QuickBooks Online integration with another provider, we recommend you turn it off to avoid 'double counting'. Browse our accounting section to learn more about  how it works in NeonPanel.

Toggling attachment status button in any setting will determine whether csv, xlsx or any other files from NP Processes are attached in QuickBooks Online.

## Sync Options

There are 3 options for each of the subsequent settings: Manual, On, and Off.

## Manual

When this option is chosen, somebody in your team with the 'bookkeeper' role will need to manually initiate syncronization.

To initiate a syncronisation, an accountant first needs to mark the task associated with the payment as completed in the relevant Bill or Inventory Order.

Tasks with QuickBooks Online Logs and Transactions are only accessible by users with a role of 'accountant' or 'bookkeeper'.

Once the transaction task is checked, the bill's relevant journal entries will be synced with QuickBooks Online. This way, an accountant has to approve syncronisations before they are made.

## Auto

If you instead choose the Auto (On) setting, the synchronisation will be initiated once the person responsible for the order (like a product manager) fills out and completes the steps above the QuickBooks Online transactions ones.

## Off

This option turns off synchronization completely.

## Control Which Data Gets Synced

There are also multiple types of accounting data NeonPanel can sync with QuickBooks Online. You can control the sync options on each of these separately.

## Keep in Mind

NeonPanel gives you detailed control over the synchronization process between QuickBooks Online and NP. However, to take advantage of all the functionality provided, you need to have at least the Essentials QuickBooks Online subscription.

If you use the base-level QuickBooks Online subscription called Simple Start, NeonPanel will only be able to synchronize invoices and COGS with your QuickBooks Online account. The Simple Start QuickBooks Online subscription does not include a Bills feature, which is essential for enabling the synchronization of Inventory Orders and Bills between your NeonPanel and QuickBooks Online accounts.

We encourage you to upgrade to the Essentials QuickBooks Online subscription to unlock NeonPanel's full synchronization capabilities.

## Export Amazon Invoices

This setting determines whether invoices from Amazon are uploaded to your QuickBooks Online account.

If you turn it on, all invoices you get from Amazon will be imported to NeonPanel, and then to QuickBooks.

Available for all QuickBooks Online subscriptions.

## Export Inventory Orders

This setting determines whether the data from NeonPanel Inventory Orders is exported to your QuickBooks Online account or not.

Turning this section on will lead to inventory quantity information that you capture in NeonPanel to be synced with QuickBooks Online.

Available for the Essentials QuickBooks Online subscription and above.

However, this won't lead to COGS being synced automatically - please read until the end.

## Export & Import Bills & Expenses

This setting determines whether the data from NeonPanel Bills is exported to your QuickBooks Online account or not. This includes both landed cost bills and non-landed cost bills.

It is important to note that not only will be data captured in a NeonPanel process be synced to QuickBooks Online, but also the expenses that you capture on QuickBooks Online will be synced with NeonPanel. Either way, you will get a Single Source of Truth for your accounting data.

By using NP processes to capture data, however, you get the added benefit of getting automatic COGS calculations - more on that at the end.

Available for the Essentials QuickBooks Online subscription and above.

## Export & Import Other Invoices

Currently, NeonPanel only integrates with Amazon as a sales channel. It is in our plans to create integrations with other platforms such as Shopify, WooCommerce, Walmart etc.

By turning this option on, you can enter non-Amazon invoices into QuickBooks Online, and NeonPanel will pick up the data for those platforms, and can even calculate your COGS automatically, if you want. This will allow you to quickly see the information from your other channels in NP Analytics.

Available for all QuickBooks Online subscriptions.

## Export Cost of Goods Sold (COGS)

Calculating and properly accounting for your COGS in a challenge many Amazon Sellers face. This setting controls whether the COGS information NeonPanel has is synced with QuickBooks Online.

QuickBooks doesn't offer a comprehensive inventory management module, which would calculate COGS, although you can find some software on their marketplace to help you with this.

By turning this option on - using the landed costs information you capture using NP Processes - NeonPanel will calculate and make relevant journal entries for the amount of your COGS, when an invoice from Amazon is processed. This way, you automatically account for both your revenue ,and your costs and expenses.
