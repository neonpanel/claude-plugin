# Accounting Data Flow

Category: Accounting · Article 1166 · Updated 2026-02-25

NeonPanel automates the integration of Amazon accounting data, creating invoices, journal entries, and managing inventory transactions. This ensures accurate and efficient financial record-keeping for Amazon sellers.

## Introduction

Managing Amazon accounting data can be challenging due to the complex and voluminous nature of transactions. NeonPanel (NP) simplifies this process by offering a comprehensive solution for integrating Amazon data into your accounting system. Here’s a detailed look at how NP handles Amazon accounting data and the flexibility it offers clients.

## Importing and Processing Data

1. Importing Statements from Amazon

When a client connects their Amazon integration to NeonPanel, the platform imports statements directly from Amazon. These statements provide detailed records of sales, expenses, and other related financial activities.

2. Creating Invoices

Based on the imported statements, NeonPanel creates invoices. These invoices serve as comprehensive records of sales and expenses, capturing all data connected to the services provided to sellers. The creation process involves generating service names using a specific formula:

Service Name = 'Amazon' + "amount description" + "transaction type"

This standardized naming convention ensures clarity and consistency in the records.

3. Generating Journal Entries

From these invoices, NeonPanel creates Journal Entries (JEs) in the client's register. These JEs are essential for accurate financial record-keeping and can be integrated into the client's accounting software:

QuickBooks Online (QBO): Via direct integration.

Xero: Via file upload.

This flexibility ensures that clients can maintain their preferred accounting workflows.

## Service Mapping and Customization

4. Mapping Services to Service Accounts

Services in NeonPanel must be mapped to Service Accounts from the client's Chart of Accounts (CoA). This mapping process is crucial as it allows JEs to be created in accordance with the client's specific accounting policies. The flexibility in service mapping ensures that the financial data aligns accurately with the client's accounting structure.

5. Comprehensive Data Integration

NeonPanel integrates more than just Amazon data into your accounting software. It also handles:

Purchasing Transactions: Includes deposit payments, installment payments, and corresponding asset transactions.

Bills: All bills entered in NeonPanel are integrated into the client's accounting software.

This comprehensive data management ensures that all relevant financial transactions are accurately recorded and integrated.

## Inventory Management Transactions

6. Managing Inventory Transactions with Transaction Map

NeonPanel facilitates the management of inventory transactions and associated expenses through its Transaction Map. Specific events in the workflow trigger certain JE transactions. For example:

When a payment is made for an inventory order or a landed cost bill, NeonPanel creates a JE with a Debit to Stock Asset and a Credit to Accounts Payable.

This automated process ensures that inventory management is seamlessly integrated into the accounting system, maintaining accuracy and efficiency.

## Important Notes

7. Handling Cash vs Accounts Payable Transactions

NeonPanel will not create Cash vs Accounts Payable transactions. These transactions should be entered directly through the client's accounting software, sourced from their bank or payment system. This approach ensures that cash flow and accounts payable records remain accurate and aligned with actual bank transactions.
