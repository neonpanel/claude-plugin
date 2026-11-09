# Transaction Map : Use Cases

Category: Accounting · Article 1384 · Updated 2026-02-25

By standardizing and automating transaction processes, the Transaction Map simplifies complex accounting tasks and enhances operational efficiency for businesses with advanced inventory management needs. Check detailed Use Cases here.

## Introduction

Introduction

This case study showcases the implementation of an advanced inventory management approach designed to streamline the tracking, allocation, and accounting of inventory across multiple warehouse types and shipment processes. The approach integrates two distinct workflows for receiving inventory, enabling businesses to optimize their operations and automate complex accounting tasks.

Approach 1: Direct Receipt into Asset Warehouse

In this workflow, inventory orders (IOs) are received directly into an Asset Warehouse upon arrival. Any associated costs, such as landed costs or inspection fees, are linked to the IO itself via Bills. This simplifies the process for businesses that do not require intermediary steps, as the goods are immediately allocated to the final inventory location.

Approach 2: Receipt into In Transit Warehouse and Transfer to Asset Warehouses

In cases where inventory requires additional handling or is initially stored in a temporary location, IOs are received into an In Transit Warehouse (typically a China Forwarding warehouse). From there, goods are transferred to Asset Warehouses through various shipment types, including:

•    Shipments: Standard transfers between warehouses.

•    FBA Inbound Shipments: Transfers to Amazon Fulfilment Centers.

•    AWD Shipments: Transfers to Amazon Warehousing & Distribution (AWD) facilities.

## We use this default Transaction Map:

The Transaction Map is a structured framework that defines the automated accounting logic for inventory and financial operations within NeonPanel. It maps specific inventory events and document triggers to corresponding journal entries, ensuring accurate financial tracking and reporting.

Key Features:

1.    Event-Based Automation: Transactions are triggered by predefined events, such as receiving inventory, creating shipments, or recalculating costs (e.g., COGS).

2.    Flexible Warehouse Types: Supports multiple warehouse types, including Asset, In Transit, FBA, and AWD, to align with diverse operational workflows.

3.    Landed Cost Allocation: Automatically distributes additional costs (e.g., inspection, freight, customs) to inventory batches, enabling precise cost tracking.

4.    Integrated Accounting: Journal entries are generated for each transaction, maintaining alignment with accounting standards and integration with platforms like QuickBooks Online (QBO) and Xero.

5.    Customizable Mapping: Allows mapping of accounts, warehouses, and service-related costs to fit unique business needs.

Transaction Triggers:

•    Inventory Receiving: Handles goods received into both In Transit and Asset warehouses.

•    Shipment Transfers: Tracks inventory movements between warehouses, including FBA and AWD shipments.

•    Expense Allocation: Links bills for landed costs to inventory and shipments.

•    COGS Recalculation: Updates inventory costs dynamically based on sales data.

Here’s a detailed Use Case for a customer creating an Inventory Order (IO) that directly places goods into an Asset Warehouse and connects a Landed Cost Bill to the IO. The use case outlines all events, documents, and records generated in NeonPanel.

## Inventory Order Received to Asset Warehouse with Landed Cost Bill connected (Part 1)

Scenario: A customer places an Inventory Order (IO) with goods shipped directly to an Asset Warehouse. After receiving the goods, they connect a Landed Cost Bill to the IO for additional costs (e.g., shipping, duties, etc.). Process Flow 1. Inventory Order Creation • Event: Customer creates an Inventory Order (IO) for purchasing goods. • Document Created: • Inventory Order (IO): Details purchase information, including supplier, items, quantities, purchase price, and destination warehouse (Asset Warehouse). 2. Goods Shipped and Received • Event: Goods are delivered directly to the Asset Warehouse. • Trigger: Customer checks the task Inventory Received in IO as Received. • Documents and Records Created: • Receiving Event (Transaction Map Trigger: Receive Order Task Checked): Debit: 12010 Inventory Asset Credit: 20000 Accounts Payable (A/P) TransactionDate : IO Order received Date Document Type: Journal Entry Warehouse Type: Asset Description: Warehouse Name 3. Landed Cost Bill Creation - Transaction 1 Event: Customer receives and connects a Landed Cost Bill to the IO. Documents and Records Created: Expense Info Task Triggered (Transaction Map Trigger: Expense Info Task Checked): Debit: [Account from Service Map] (e.g., Freight Cost, Customs, etc. clearing accounts) Credit: 20000 Accounts Payable (A/P) Transaction Date: Bill Date Document Type: Bill Description: Bill Name, Service Name In NeonPanel, each Service item is mapped to a specific account. For every transaction involving a service, NeonPanel automatically uses the corresponding mapped account. 4. Landed Cost Bill Creation - Transaction 2 (as soon as the goods are received by connected IO directly to the

## Inventory Order Received to Asset Warehouse with Landed Cost Bill connected (Part 2)

Asset warehouse, we need to create a 2nd transaction according Transaction Map) Event: Customer receives and connects a Landed Cost Bill to the IO. Documents and Records Created: Expense Info Task Triggered (Transaction Map Trigger: Expense Info Task Checked): Debit: 12010 Inventory Asset Credit: [Account from Service Map] (e.g., Freight Cost, Customs, etc. clearing accounts) Transaction Date: Bill Date Document Type: Bill Description: Bill Name, Service Name Use Case Summary: as soon as goods are received directly to asset warehouse and are not going to be transferred by shipments for sales any more, we have to count all amounts on the final asset account.

## Inventory Order received to In Transit Warehouse with Shipment and Multiple Bills (Part 1)

Scenario: A customer creates an Inventory Order (IO) with goods first received into an In Transit Warehouse. An Inspection Cost Bill (Bill 1) is connected to the IO. Later, the goods are shipped to an Asset Warehouse, and a second Freight and Customs Bill (Bill 2) is connected to the shipment. Process Flow 1. Inventory Order Creation • Event: Customer creates an Inventory Order (IO) for purchasing goods. • Document Created: • Inventory Order (IO): Details purchase information, including supplier, items, quantities, purchase price, and the destination warehouse (In Transit). 2. Goods Received to In Transit Warehouse Event: Customer checks the task Inventory Received in IO as Received. Documents and Records Created: Receiving Event (Transaction Map Trigger: Receive Order Task Checked): Debit: 12020 Inventory In Transit Credit: 20000 Accounts Payable (A/P) Date: Date inventory received Document Type: Journal Entry Warehouse Type: In Transit Description: Warehouse Name 3. Bill 1 (Inspection Cost) Connected to IO Event: Bill for Inspection Costs is received and connected to the IO. Documents and Records Created: Inspection Cost Event (Transaction Map Trigger: Expense Info Task Checked): Debit: 12040 Inspection Cost Credit: 20000 Accounts Payable (A/P) Date: Bill Date Document Type: Bill Description: Bill Name, Service Name 4. Bill 2 (Freight and Customs Costs) Connected to Shipment Event: Bill for Freight and Customs Costs is received and connected to the shipment. Documents and Records Created: Freight Cost Event (Transaction Map Trigger: Expense Info Task Checked): Debit: 12050 Freight Cost Credit: 20000 Accounts Payable (A/P) Date: Bill Date

## Inventory Order received to In Transit Warehouse with Shipment and Multiple Bills (Part 2)

Document Type: Bill Description: Bill Name, Service Name Customs Cost Event (Transaction Map Trigger: Expense Info Task Checked): Debit: 12060 Customs Credit: 20000 Accounts Payable (A/P) Date: Bill Date Document Type: Bill Description: Bill Name, Service Name 5. Shipment Created to Move Goods from In Transit to Asset Warehouse • Event: Task Check Up & Fix checked in Shipment Document transferring goods from In Transit Warehouse to the Asset Warehouse. Documents and Records Created: Shipment Event (Transaction Map Trigger: Inventory Check Up & Fix Issue Task Checked): Debit: 12010 Inventory Asset Credit: 12020 Inventory In Transit Credit: 12040 Inspection Cost Credit: 12050 Freight Cost Credit: 12060 Customs Date: Shipment Arrived At Document Type: Journal Entry Warehouse Type: Asset Description: Shipment Name
