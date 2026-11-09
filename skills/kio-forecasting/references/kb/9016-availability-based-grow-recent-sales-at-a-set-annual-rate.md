# Availability-Based: Grow Recent Sales at a Set Annual Rate

Category: Forecast Scenarios · Article 9016 · Updated 2026-08-24

Takes recent sales and grows them by a set annual rate.

## Introduction

Availability-Based takes the most recent month in which your product actually sold something and grows that number into the future at a set annual rate — 30% a year by default. It suits a product where the latest month is a fair picture of current demand and where your team has deliberately decided to plan for growth rather than let the numbers argue about direction.
Before anything else: the name is misleading. Availability-Based does not look at your stock on hand, your inbound shipments, your purchase orders, your warehouse or production capacity, or how many days your listing was actually in stock. Nothing about availability enters the calculation. It is a recent sales figure plus a growth plan, and you should read every number it produces with that in mind.

## The short version

The scenario finds the most recent month that had sales, strips out the seasonal effect you configured for that month, and treats what is left as your current monthly sales level. It then grows that level at 30% a year, spread evenly across the months, and finally reapplies your seasonality month by month so the calendar shape comes back.
Because the growth rate is a setting rather than something learned from your history, the forecast rises whether your recent sales have been climbing, flat, or falling. There is no trend detection and no attempt to spot unusual months. One month sets the level; the setting sets the direction.
A few facts worth carrying into your planning:

The default growth is 30% a year, about 2.2104% a month. Before seasonality is applied, the underlying level is 30% above your starting level after twelve months and 69% above it after twenty-four months. Final forecast units can be higher or lower depending on each month's seasonality number.
There is a 5% floor on the annual growth rate. With the standard 30% setting the floor never does anything.
Months with no sales at all are skipped entirely rather than counted as zero, so "the most recent month that had sales" is not always last month.
Nothing about your inventory, your supply, or your marketing plans is an input. The forecast can comfortably exceed what you can buy or ship.
This is not the setting NeonPanel falls back to. When no scenario is set on a product and no valid company default exists, NeonPanel uses Recency-Weighted Growth.

## How it works

Everything below is about units. Revenue comes afterwards, by multiplying forecast units by a price, and the price never changes the unit forecast.
It reads completed months, per product, per marketplace. NeonPanel loads up to 18 completed months of sales history, already totalled by calendar month, for each product in each marketplace and channel — down to the company, product, SKU, marketplace, sales channel, country and month. A product selling on two marketplaces gets two separate calculations rather than one blended company total. This scenario normally needs only the newest of those months, though the wider history is loaded.
It takes out the seasonal effect before choosing a level. Each historical month is divided by the seasonality number you configured for that month. If November sold 1,800 units and your November seasonality number is 1.5, the underlying level is 1,800 ÷ 1.5 = 1,200 units, and the scenario plans from 1,200. If your product has no custom pattern, or the pattern is neutral, every month's number is 1 and the underlying level is simply the units you sold.
It uses the most recent month that had sales as the starting point. No averaging, no smoothing, no blending with earlier months: whatever the newest month with sales worked out to after seasonality was removed becomes the level the whole growth curve is built on. Months in which the product sold nothing are not in the history at all — they are skipped, not counted as zero, so they neither drag the level down nor leave a gap to fill. If last month sold nothing, the starting point is the month before that, and NeonPanel does not work out whether the empty month was a stockout, a suppressed listing, or genuine silence in the market.
It checks whether your latest month was weak, then grows anyway. NeonPanel compares the latest month with the average of up to your last three months that had sales. When the latest month is below that average it uses whichever is larger, your configured growth rate or the 5% floor. When the latest month is at or above the average it uses your configured growth rate directly. With the standard settings both paths arrive at 30%, so that comparison changes nothing you can see. It would only matter if your growth rate were set below 5%, and even then the floor would lift the number on the weak-latest-month path only.
It converts the annual rate into a monthly step. Growing 30% a year is not 2.5% a month. Growth builds on itself here — each month grows off a bigger number than the one before — so the correct monthly step is smaller: about 2.2104%. Apply it twelve times and you land exactly 30% higher. Using 2.5% a month would overshoot the annual target noticeably.
It walks the level forward, month by month. The first future month is your starting level times 1.022104, the second is that result grown again, and so on. By the twelfth future month the level is your starting point times 1.30; by the twenty-fourth it is times 1.69. No straight line is drawn through your recent months and no direction is taken from them. The rate you configured is the direction.
It puts seasonality back. Each future month's level is multiplied by the seasonality number for that calendar month. The growth curve decides how high the underlying level sits; your twelve seasonality numbers decide how that level is spread around the year. Put a number greater than zero in every one of those twelve months — a zero erases that month from the forecast, and a negative number can produce a negative forecast.
It never goes below zero, then it becomes revenue. The underlying monthly level is held at zero or above before seasonality is applied, so with valid seasonality numbers your final units are never negative. NeonPanel normally writes 24 future months and stores your actual sales history alongside the forecast. Revenue is then forecast units multiplied by a price: the average unit price across up to your last three months that had sales, or your overall historical average price if those are not available. The scenario has no view on how a price change would affect what customers buy.

## A worked example

Take a kitchen product — call it the Pour-Over Kettle. Its most recent completed month sold 1,200 units, and the seasonality number for that month is 1.0, so the underlying level is 1,200 ÷ 1.0 = 1,200 units. Annual growth is set to 30%, the floor is 5%, and the effective rate is therefore 30%. The next three months have seasonality numbers of 1.0, 1.1 and 0.9.

Because the latest month had sales, the Kettle's starting level is 1,200 units. It is not averaged with anything earlier. Growing at about 2.2104% a month, the underlying level for the first three months looks like this:

| Future month | Underlying level, before seasonality |
|---|---|
| Month 1 | 1,226.5 units |
| Month 2 | 1,253.6 units |
| Month 3 | 1,281.3 units |

Now apply the seasonality numbers for those calendar months:

| Future month | Underlying level | Seasonality number | Forecast units |
|---|---|---|---|
| Month 1 | 1,226.5 | 1.0 | 1,226.5 |
| Month 2 | 1,253.6 | 1.1 | 1,379.0 |
| Month 3 | 1,281.3 | 0.9 | 1,153.2 |

Month 3 comes out lower than Month 2, and this is the single most common thing people misread. Nothing reversed. The underlying level kept climbing, from 1,253.6 to 1,281.3 units. The 0.9 seasonality number is what pulled the final figure down. When you are trying to work out why a month dipped, always look at the level before seasonality first.
With neutral seasonality — every month set to 1 — the same Kettle gives you roughly 1,368.2 units in Month 6, exactly 1,560 units in Month 12 (1,200 × 1.30), and 2,028 units in Month 24 (1,200 × 1.69). Two years out, the plan is buying 69% more Kettles a month than you sell today, purely because a setting said 30% a year.
Now suppose the Kettle sold nothing at all last month. That month is not in the history, so the forecast is built from the last month that did sell — say two months back at 1,150 units. The empty month leaves no trace, and NeonPanel does not decide for you whether it was a stockout, a listing problem, or a real collapse in demand.

## When to choose it

Availability-Based earns its place when the latest month is a credible read on your current run rate and your team genuinely wants a smooth growth plan rather than a statistical guess. It fits best when the configured annual rate reflects an approved commercial target, when working out a direction from history would be less useful than simply starting from where you are now, and when the product is reasonably stable outside the seasonality you have already configured.
It also works for a product with thin history, as long as you have at least one representative completed month, and it is a reasonable pick when someone else on the team will check supply feasibility separately — because this scenario will never do that for you.
Everyone looking at the output needs to understand they are looking at a target-shaped curve, not a demand model. If your stakeholders will read a rising line as a prediction, choose something else or label the chart carefully. Used deliberately, it answers a useful planning question: what inventory would we need if our current run rate grew 30% a year?

## When to avoid it

Skip this scenario if you expect the forecast to be limited by what you can actually stock — it will not be. Skip it too if the latest month was distorted, because a stockout, a suppressed listing, a launch ramp, a promotion, or a single bulk wholesale order will all become the foundation of a two-year curve.
Avoid it when recent sales are falling and you want the forecast to reflect that, when you would rather growth responded to several windows of your history instead of one fixed annual number, and when demand is intermittent enough that a single month with sales is not representative of anything.
It is also wrong when the annual growth assumption has not been agreed commercially, when you specifically want a level baseline, when the important movements happen inside a month rather than between months, or when a 30% annual curve would produce second-year quantities nobody would approve. And if your team expects a forecast to react to advertising, price, promotions, or supply plans, none of those are inputs here.
If you need a stock-constrained plan, build one after the forecast exists by comparing it against inventory, inbound supply, lead times, and capacity.

## What it does to your buying plan

Overstock is the main risk. A 30% annual rate reaches 69% growth after two years. If the latest month happened to be strong, or the growth target is simply not met, purchase orders written straight off this forecast produce excess stock, storage fees, and cash sitting on a shelf.
Stockouts are the quieter risk. If the latest month was held down by a stockout, the level you are planning from represents what you managed to sell rather than what customers wanted. The scenario has no way to repair that: it grows the constrained number, and your plan inherits the shortfall for two years.
One month carries the whole plan. Because the starting point is a single month, the result is unusually sensitive to a partial period, a promotion, a data correction, a one-off order, or a skipped zero-sales month. Every forecast in the 24-month series traces back to that one number.
Small assumption differences become large quantities. Because growth builds on itself, a rate that looks close enough in the first quarter diverges badly by the second year. Review the second-year numbers before you approve anything.
A wrong seasonality number damages both ends. The same twelve numbers pull your historical month apart and reshape the future month, so one wrong value distorts both the level you plan from and the month you plan for. Check all twelve, and check they are in January-to-December order.
Practical safeguards: compare the latest month against at least the previous three to twelve months before trusting it; note known stockouts, promotions, and part-months during review; look at the underlying level separately from the seasonally adjusted numbers; test higher and lower annual growth assumptions outside the automatic scenario; check the forecast against the supply and cash you have; save explicit month-level numbers where the automatic result is wrong; and recheck those saved numbers after every refresh.
Every refresh can move the whole plan
The automatic forecast currently runs on the 10th of each month at 08:00 UTC. When it runs, a newly completed month can change your starting level, and a new month with sales replaces the previous starting point. Months with no sales stay absent, so an older month can remain the starting point for longer than you would expect. Seasonality is then applied across the new forecast calendar.
The consequence: the entire curve can change, not only the month that was just added. NeonPanel writes 24 future months, and the planning view may show you a shorter 12-month series drawn from them.
When you adjust and save a month with Kio
A forecast you model and save with Kio is not one of the automatic scenarios — it sits on top. In inventory planning, a saved value replaces the automatic result for that product and month, and the neighbouring months keep their automatic values unless they have saved values of their own. If more than one saved value exists for the same month, the most recent one wins. The Availability-Based result stays stored underneath.
At the next monthly refresh, NeonPanel recalculates the automatic Availability-Based curve underneath, but it does not recalculate or overwrite the value you saved. Your saved value remains unchanged and continues to be the one inventory planning uses for that product and month.
Saved values are not training data and do not become the starting point for the next calculation. If you save 1,500 units for September, October does not grow from 1,500 — October still comes from the automatic calculation unless you save a value for October too. You can compare your saved value with the refreshed baseline, but the refresh itself will not change the value you approved.

## How seasonality changes the result

Seasonality wraps around the growth calculation rather than being part of it. For a custom pattern, the order is: each historical month is divided by its own seasonality number; the newest of those months becomes the level; the 30% annual curve grows that level; and each future month is multiplied by its own seasonality number.
That order is what stops a peak being counted twice. Say your latest month is December with 2,400 units sold and a December seasonality number of 2.0. The level you plan from is 1,200 units, not 2,400. Next December then gets its own 2.0 applied after the growth curve. If the raw 2,400 had been used as the level and December seasonality applied on top, you would be planning for roughly double a real December.
When your pattern is neutral or missing. If no custom pattern is set, or the pattern has the wrong number of entries, or an entry is blank or not a number, NeonPanel uses 1 for those months. Nothing is removed from history and nothing is added to the future, so the forecast has no peaks or troughs of its own.
The one thing you must get right. Put a number greater than zero in every month of the pattern. A zero erases that month from the forecast — it does not leave it unchanged. A negative number can produce a negative forecast even though the underlying calculation never goes below zero. These two values are not caught for you, so the check has to happen when you configure the pattern.

## What history it needs

The minimum is genuinely one month. A single month with sales is enough to establish a starting point. Missing and zero-sales months are not filled in and they do not trigger any alternative behavior.
Thin history is not an error, but it does mean the whole 24-month plan rests on very little evidence. A launch month, a part-month, a month with a suppressed listing, a one-time wholesale order, or an unusual promotion will all happily become the foundation of the curve.
It reads months, not days. A month where you sold hard in the first week and went quiet for three weeks looks identical to an evenly-paced month with the same total.
Worth checking before you assign this scenario: whether the latest completed month covers a full period of selling; whether the product was in stock and buyable throughout it; whether a large promotion or a one-time order inflated it; whether returns, cancellations, or data corrections distorted the units; whether your SKU, marketplace, channel, or country mappings changed mid-history; whether the seasonality pattern is valid and attached to the right product; and whether 30% a year is a number your business can actually stand behind. None of these checks happen automatically.
What it never reads. Current stock on hand, available-to-promise inventory, inbound shipments or purchase orders, historical in-stock percentage, lost-sales estimates, warehouse or production capacity, supplier lead times, reorder points or safety stock, planned promotions or campaign budgets, planned price changes, or competitor activity. Several of those are decisive for a real buying plan, so handle them outside this scenario or reflect them by saving your own forecast values.

## Things that surprise people

The name promises something it does not deliver. People assume a scenario called Availability-Based has been checked against stock. It has not. Always run a separate supply-feasibility review.
It does not average your last three months. You may hear otherwise. The starting point is always the single most recent month that had sales, with no blending.
A weak latest month does not slow the growth. The comparison against your recent average exists, but with the standard 30% setting and 5% floor both outcomes are 30%. A falling product still gets a rising forecast.
Zero-sales months disappear rather than counting as zero. This flatters the starting level for an intermittent product: the empty months never pull it down, because they are not in the calculation at all.
A month can drop while growth is positive. Seasonality can more than cancel out the underlying monthly growth, so compare underlying levels rather than final units when diagnosing direction.
"30% a year" is not "2.5% a month." The monthly step is about 2.2104%, because each month grows off a bigger number than the last.
The second year is where the damage lives. Approving the first three months of a 30% curve tells you almost nothing about the quantities it will ask for in month twenty-four.

## How it compares to the nearby scenarios

Recency-Weighted Daily Rate. Availability-Based starts from one month's total and applies your configured annual rate. Daily Rate turns several recent months into sales-per-day figures, trims unusually high ones back, weights recent months much more heavily than older ones, uses a gentler 5% annual floor, and adjusts for how many days each future month has. Choose Daily Rate when several recent months should set the level and extreme months should be damped; choose Availability-Based when the latest month should be the starting point and your growth plan should dominate.
Recency-Weighted Conservative. That scenario spreads each month's sales evenly across its days, keeps roughly the most recent 120 of those days, and follows the direction it finds in them, weighting recent days more. Calendar gaps end up compressed together, and despite the name there is nothing in it that softens a decline. Availability-Based works out no direction at all from your history.
Recency-Weighted Growth. That one builds a level from up to six recent months that had sales and works out a direction from windows of 2, 6 and 12 such months. It leans toward growth: it ignores falling signals when any flat or rising signal exists, and halves the decline when everything is falling. It is also what NeonPanel uses when nothing valid is set. Choose it when the forecast should read direction from your sales; choose Availability-Based when you want a fixed growth assumption instead.
Seasonal Naive. Seasonal Naive carries your latest underlying monthly level forward without growing it, so with neutral seasonality it stays flat. Availability-Based uses much the same starting point but grows it at the effective annual rate.
Growth 10, Growth 20, Growth 30. Those planned scenarios apply a stored percentage curve to your previous completed month, and the percentages live in operational data that can change independently of the calculation code. Availability-Based works out its monthly step mathematically from an annual setting. Both are plan-shaped curves, but the shape comes from different machinery, so a Growth 30 forecast and a 30%-a-year Availability-Based forecast will not match month for month.

## Common questions

Does it read our current inventory? No. Not stock on hand, not inbound units, not availability, not capacity, not in-stock history.
Does it limit demand to what we can fulfil? No. The forecast can sit well above anything you are able to supply.
Then why is it called Availability-Based? The calculation does not support what the name implies. Treat it as a recent-month starting point with a growth plan on top.
Does it average our last three months? No. The most recent month that had sales is used on its own.
What happens when last month sold nothing? That month is not in the history. The forecast continues from an older month that did have sales, and NeonPanel does not work out whether the gap came from no demand, no stock, or a data problem.
What is the default growth rate? 30% a year, which is about 2.2104% a month.
Is 30% a year the same as 2.5% a month? No. Twelve months of 2.5% would overshoot 30%, because each month grows off a bigger number. The scenario uses about 2.2104%.
Can it learn that our sales are declining? No. It works out no direction from history. With the default setting the underlying level grows 30% a year regardless.
If the latest month is below our recent average, does growth slow down? Not with the standard settings. The comparison happens, but the rate ends up as the larger of your configured growth and the 5% floor, and 30% wins either way.
Does it account for promotions, ads, or price changes? No. None of them are inputs to the unit forecast.
How is revenue worked out? Forecast units times the average unit price across up to your last three months that had sales, or your historical average price when those are not available. Price converts units into money; it does not shape demand.
Why does a month drop when annual growth is positive? Seasonality. Check the underlying level before seasonality to see the real direction.
Does saving a forecast change future automatic numbers? In inventory planning, a saved value replaces only the matching product and month. It does not train the scenario and it does not reset the starting point.
Is Availability-Based the default? It can be your company default, but it is not the system fallback. Recency-Weighted Growth is used when NeonPanel cannot resolve a valid supported scenario.
