# Inventory Parameters Explained

Category: Catalogs · Article 1060 · Updated 2026-02-25

This article lists  the critical inventory catalog parameters that you need to fill out, and explains why they are so important.

## Introduction

Importing key parameters into the inventory catalog is crucial, not just during the initial setup, but also as you introduce new products to your portfolio. Crucial parameters to update:

Lead Time

FBA Lead Time

Safety Stock

FBA Safety Stock

Daily Sales Target

Duty and Extra Duty

Current Price

Unit Landed Cost

## Lead Time

Definition: The span required for a product to traverse the entire supply chain—from the moment of order placement to its arrival at a 3rd party (non-Amazon) destination warehouse. This cycle includes steps like order processing, manufacturing, transportation, among other relevant processes.

Importance: This metric is pivotal in gauging when the inventory in your 3rd party (non-Amazon) warehouse should be restocked. It influences the 'Inventory Order Schedule' tab in the Inventory Planning dashboard and is vital for proficient inventory planning.

## FBA Lead Time

Definition: The time necessary for a product to be shipped to an Amazon Fulfillment Center and readied for sale.

Importance: This measure helps pinpoint when the stock at the Amazon fulfillment center requires replenishment. It impacts the 'FBA Inbound Shipment Schedule' tab in the Inventory Planning dashboard, rendering it crucial for adept inventory management.

## Safety Stock

Definition: An extra quantity of products stored across all warehouses to counter potential stock-outs due to unpredictable demand, disruptions in the supply chain, or variations in lead times. Represented in days, this metric acts as a buffer, ensuring a consistent supply to meet demand, even amidst unexpected challenges.

Importance: Understanding the requisite amount of safety stock is integral to the algorithm guiding your planning process. It's crucial to note that this value signifies days, not individual inventory units. Given that NeonPanel accesses both historical and the latest daily sales figures, it enables dynamic adjustments on the dashboard based on the present sales pace.

## FBA Safety Stock

Definition: The specific stock quantity held within Amazon's fulfilment centers for those utilizing the FBA service. This ensures a ready supply for shipping, even during surge periods or unforeseen events, and is measured in days.

Importance: Recognizing the essential safety stock volume is a cornerstone of the planning algorithm. Just as with the general safety stock, this value denotes days, not stock units. With NeonPanel's ability to tap into past and recent daily sales data, the dashboard can nimbly adjust based on the ongoing sales velocity.

## Daily Sales Target

Definition: The sales volume you aspire to achieve each day.

Importance: This metric becomes especially relevant in the "Planning" phase when the "target" is activated in the 'Planning Variant' settings. If you're anticipating substantial shifts in daily sales, perhaps due to enhanced marketing initiatives, this parameter aids in more strategic planning. For products you don't intend to repurchase, setting this value to 0 is advisable. During the planning stage, this facilitates focusing solely on products with a specified target, thus weeding out superfluous items.

## Duty and Extra Duty

Definition: This pertains to the standard customs duty and an added customs duty. Their presence markedly affects the Landed Cost allocation.

Importance: The "Allocation Base" (the distribution methodology for a singular customs duty payment across individual inventory items) hinges on the "Customs Duty" and "Extra Duty" values. Rather than apportioning based on product weight or volume, the allocation hinges on the "Amount," adjusted by the combined "Duty" and "Customs Duty". Specifically, the formula is: Amount * (1 + (Duty + Customs Duty)/100).

## Current Price

Definition: The most recent pricing from the supplier, which is paired with a Landed Cost.

Importance: In the ‘Profit Analytics’ dashboard and others, when the "COGS = Current" scenario is chosen, the derived Landed Cost from the 'Current Price' (and its corresponding 'Landed Cost') takes precedence in calculations over those values sourced from Inventory Order and Bills & Expenses data.

## Unit Landed Cost

Definition: Total cost of producing and shipping the product to the point of sale. In our case - to Amazon.

Importance: This is a fall-back option in a case where NeonPanel isn't able to calculate the accurate landed cost from purchase orders and landed cost bills for a specific unit.
