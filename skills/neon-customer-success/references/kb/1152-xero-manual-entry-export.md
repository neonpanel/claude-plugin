# Xero Manual Entry export

Category: Xero Integration · Article 1152 · Updated 2026-02-25

By aligning your data with Xero's specific import requirements, NeonPanel provide a seamless, straightforward import process that maximizes efficiency and reduces manual data entry errors.

## Introduction

In NeonPanel we have report XERO Manual Entry Export. You can use this report to export your data in XERO.

## Detailed Mapping Process (Part 1)

Narration: This field is compiled by combining the "NP" - "Document Number" from your data. This creates a unique identifier for each transaction, which helps in tracking and referencing transactions in Xero. Narration: This field is compiled by combining the "NP" - "Document Number" from your data. This creates a unique identifier for each transaction, which helps in tracking and referencing transactions in Xero. Date: The "Transaction Date" from your data is used here, formatted to meet Xero's date requirement (12/25/2024), ensuring that the data aligns with your financial timeline in Xero. Date: The "Transaction Date" from your data is used here, formatted to meet Xero's date requirement (12/25/2024), ensuring that the data aligns with your financial timeline in Xero. Description: We combined "Account Classification" and "Account" from you current Chart of Accounts to provide a detailed description of each transaction, giving context to the entries in your Xero account. Description: We combined "Account Classification" and "Account" from you current Chart of Accounts to provide a detailed description of each transaction, giving context to the entries in your Xero account. AccountCode: Directly taken from the "Account" field in your data, this includes the specific account codes used in your chart of accounts, ensuring transactions are recorded against the correct accounts. AccountCode: Directly taken from the "Account" field in your data, this includes the specific account codes used in your chart of accounts, ensuring transactions are recorded against the correct accounts. TaxRate: We set this based on the nature of each

## Detailed Mapping Process (Part 2)

transaction. Where no tax is applicable, we've set the rate to "Tax Exempt", ensuring compliance with tax recording requirements. TaxRate: We set this based on the nature of each transaction. Where no tax is applicable, we've set the rate to "Tax Exempt", ensuring compliance with tax recording requirements. Amount: Depending on whether the original entry was a debit or a credit, we used the corresponding value. We ensured to maintain the correct signage (positive for credits and negative for debits) to reflect the true nature of each transaction. Amount: Depending on whether the original entry was a debit or a credit, we used the corresponding value. We ensured to maintain the correct signage (positive for credits and negative for debits) to reflect the true nature of each transaction. Tracking Fields: "Customer" was used as the TrackingName1 to enable you to categorize and track transactions by customer. We use amazon marketplace name, like "Amazon.ca", or the respective customer name for non-amazon sales was used as TrackingOption1, allowing for detailed tracking and analysis. Tracking Fields: "Customer" was used as the TrackingName1 to enable you to categorize and track transactions by customer. We use amazon marketplace name, like "Amazon.ca", or the respective customer name for non-amazon sales was used as TrackingOption1, allowing for detailed tracking and analysis.

## Xero Import instructions

Even so XERO instruction tell you do not modify template file column names, you still need to delete '*' sign from them. We have it checked ) .

This mapping and preparation process has been designed to streamline your financial data integration into Xero, making it as effortless and error-free as possible.https://go.xero.com/Journal/Import.aspx

IMPORTANT: Some accounts in XERO could be locked by the system. To record manual journal you have to add a secondary accounts to the chart of accounts in XERO. You are unable to import records for locked accounts (in these cases XERO just left you AccountCode column blank).

You can read more about this in Xero Central
