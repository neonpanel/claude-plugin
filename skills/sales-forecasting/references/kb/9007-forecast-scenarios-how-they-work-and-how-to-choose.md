# Forecast Scenarios: How They Work and How to Choose

Category: Forecast Scenarios · Article 9007 · Updated 2026-08-24

Understand how NeonPanel's twelve forecast scenarios use recent sales, longer-term averages, trends, growth targets, and seasonality—and how to choose the right approach for each product.

## Introduction

A forecast scenario is the method NeonPanel uses to turn a product's sales history into a forward plan. It is the setting that decides whether next quarter's forecast follows your recent run rate, your long-run average, a trend, or a growth target your leadership has already committed to.
Choosing one is an operating decision, not a chart preference. The scenario on a product feeds the forecast that feeds your purchase orders, so it shows up in what you buy and how much cash you tie up.
There is no scenario that is right for everything. A mature replenishment item, a fast-growing hero SKU, a Q4 gift product, and a new launch usually should not behave the same way.

## The one thing to understand first

Every scenario works from sales history or from a fixed planning curve. None of them knows about a promotion you have planned, an advertising push, a new marketplace, a supplier problem, or a price change. When you know something the history does not, you can model the effect, review the adjusted months, and save those values on top of the automatic forecast.

## How your forecast actually gets made

On the 10th of each month at 08:00 UTC, NeonPanel rebuilds the automatic forecast for every active product. Here is what happens to each one.
First it works out which scenario to use. If you have set a scenario on the product, that one is used. If you have not, NeonPanel uses your company default. If the setting is blank in both places, or holds something NeonPanel cannot recognize, it uses Recency-Weighted Growth.
There is one wrinkle worth knowing. NeonPanel only checks the company default when the product's own setting is empty. If a product has a scenario set but that value is invalid or unsupported, NeonPanel goes straight to Recency-Weighted Growth rather than falling back to your default.
Next it loads sales history — up to eighteen months of it — and keeps only the months where the product actually sold something. Months with no sales are skipped entirely, not counted as zero. This matters more than it sounds, and it comes up again throughout this guide.
Then it takes the seasonal effect out of that history, so the calculation works on the underlying level of demand rather than on peaks and troughs you already expect. It runs the scenario on that cleaned-up history, puts the seasonality back onto each future month, and produces a forecast of units and revenue, normally twenty-four months ahead.
Finally the result goes to the Live forecast, which is what your replenishment planning reads.
Only one automatic scenario is kept per product per run. You cannot have two running side by side.

## Planned growth: you set the target

Flat Scenario, Growth 10, Growth 20, and Growth 30 take your most recent month and apply a fixed curve to it. Growth 10 works toward roughly 10% growth over a year, Growth 20 toward roughly 20%, Growth 30 toward roughly 30%, and Flat Scenario holds steady.
These are targets, not predictions. NeonPanel does not check your ad budget or promotion calendar to see whether the target is realistic. Your team owns that judgment, and owns making sure marketing, pricing, and supply can actually deliver it.
All four need the product to have sold something in the immediately previous month. If that month is missing or came in at zero, NeonPanel produces no automatic forecast for that product at all — it does not quietly fall back to a longer-term average.

## Stable and seasonal: the history decides

Seasonal Naive, 12-Month Moving Average, Trend and Seasonality, and Robust Low all read the sales history and work out a level from it. What separates them is how much weight they put on the most recent month versus the longer-run average, and whether they follow a trend.
Use this family when the product's past sales are a fair picture of how it should behave going forward.

## Recency-weighted: recent months count more

Availability-Based, Recency-Weighted Daily Rate, Recency-Weighted Conservative, and Recency-Weighted Growth all lean on recent performance rather than treating the whole year equally. They suit products where what happened last month tells you more than what happened last spring.
One warning up front: despite the name, Availability-Based does not look at your stock. Not on-hand units, not inbound shipments, not supplier capacity. It takes recent sales and grows them by a set annual rate. Read its guide before you turn it on.

## What each scenario does, in one line

| Scenario | What it does | Good fit for |
|---|---|---|
| Flat Scenario | Holds your latest month's level steady going forward | Mature products with a deliberate no-growth plan |
| Growth 10 | Works the latest level up toward about 10% growth over a year | Established brands planning steady growth |
| Growth 20 | A stronger planned ramp, about 20% over the first year | Funded growth with inventory and cash to back it |
| Growth 30 | An aggressive planned ramp, about 30% over the first year | Committed expansion with real upside evidence |
| Seasonal Naive | Carries your most recent selling month forward, reshaped by seasonality | Steady run rate where the calendar is the main variable |
| 12-Month Moving Average | Averages up to twelve selling months, then adds seasonality | Mature, noisy products that are broadly stable |
| Trend and Seasonality | Draws a straight line through your history and extends it | Products with a real sustained direction plus seasonal peaks |
| Robust Low | Blends 70% moving average with 30% latest month | Damping down a recent swing you do not fully trust |
| Availability-Based | Grows recent sales by a set annual rate, currently 30% | Recent demand plus explicit built-in growth |
| Recency-Weighted Daily Rate | Recent sales per day, with extreme months trimmed back | Fast-moving demand where one spike should not take over |
| Recency-Weighted Conservative | Follows recent momentum up or down | Products with clear recent direction |
| Recency-Weighted Growth | Blends several trend windows and leans optimistic | The system fallback when no valid supported scenario can be found |

Two of these names are misleading and worth flagging again. Seasonal Naive does not copy the same month from last year — it carries your latest level forward and reshapes it with your seasonality settings. Robust Low is not a floor or a worst case — it is a fixed 70/30 blend, and when your latest month is running below the yearly average it can come out higher than Seasonal Naive.

## How seasonality changes the answer

The scenario decides the underlying level. Seasonality decides how that level is spread across the calendar.
Your seasonality setting is twelve numbers, one per month. A month set to 1.0 is normal. A month set to 1.25 runs 25% above normal. A month set to 0.8 runs 20% below. A month set to 2.0 doubles.
So if a scenario works out an underlying level of 1,000 units a month, a 0.8 month becomes 800 units, a 1.0 month stays at 1,000, and a 1.5 month becomes 1,500.
When you have set a real seasonal pattern, NeonPanel takes that pattern out of your history before running the scenario, and puts it back on afterwards. That is what stops a known December peak from being counted twice — once because it happened, and again because you told the system December is big. If all twelve of your numbers are 1.0, seasonality is treated as neutral and nothing is adjusted.
Every one of those twelve numbers needs to be greater than zero. A zero wipes that month out of your forecast completely. A negative number can push your forecast below zero, even for scenarios that otherwise never produce a negative result. Nothing currently stops you from entering either one, so check your seasonality settings before you trust a forecast that looks wrong.
You also need all twelve, in January-to-December order. If the pattern is blank or does not have exactly twelve entries, NeonPanel treats the whole thing as neutral, and a single blank or non-numeric entry becomes 1.0 for that month while the rest of the pattern is kept. An invalid pattern therefore turns into "no seasonality" quietly rather than showing you an error, which is worth knowing if a forecast comes back flatter than you expected.

## The same product under four different scenarios

Take a product where the most recent month sold 1,000 units once seasonality is stripped out, the twelve-month average is 900 units, the trend is pointing up, and December is set to 1.5.
Here is what December looks like under each approach.

| Scenario | Underlying level | December forecast |
|---|---|---|
| Seasonal Naive | 1,000, the latest month | 1,500 units |
| Flat Scenario | 1,000, held steady | 1,500 units |
| 12-Month Moving Average | 900, the yearly average | 1,350 units |
| Robust Low | 930, being 70% of 900 plus 30% of 1,000 | 1,395 units |
| Trend and Seasonality | 1,100, if the line projects there | 1,650 units |
| Growth 10 | 1,100, at a point on the curve late in year one | 1,650 units |

That is a spread of 300 units on one product in one month, from the same history. The differences are deliberate — each scenario reflects a different planning philosophy — but they are exactly why the setting deserves attention.
Seasonal Naive, 12-Month Moving Average, Trend and Seasonality, and Robust Low round to whole units before revenue is worked out, so you will not see fractional units from those four.

## Picking a scenario

Start by asking whether you are forecasting or committing
If your leadership has already agreed on a growth number and the business is being run to hit it, use a planned growth scenario. You are recording a decision, not making a prediction.
If you want the forecast to work out the answer from what has actually been selling, use one of the other eight.
Then check whether your history is any good
Before trusting any history-based scenario, ask whether the product has been selling long enough to have a meaningful record, whether it went out of stock for a stretch, whether a promotion inflated the most recent month, and whether anything permanent has changed — the listing, the price, the channel mix, the ad spend.
Also look for gaps. Because months with no sales are dropped rather than recorded as zero, a product that went quiet for three months does not show a hole in the data. It shows a shorter history, and some scenarios will treat those skipped months as if they never happened.
A more sophisticated scenario does not make poor history reliable. It usually just projects the same problem more confidently.
Match the scenario to the behavior

| If the product... | Start with |
|---|---|
| Sells steadily and you plan no growth | Flat Scenario |
| Is on an approved moderate growth plan | Growth 10 |
| Is on an approved stronger growth plan | Growth 20 |
| Is part of a funded aggressive expansion | Growth 30 |
| Has a steady run rate with calendar peaks | Seasonal Naive |
| Is mature and bounces around but is broadly flat | 12-Month Moving Average |
| Has a real direction plus seasonal peaks | Trend and Seasonality |
| Just swung and you want to damp it down | Robust Low |
| Should grow at a set annual rate off recent sales | Availability-Based |
| Moves fast and needs spike protection | Recency-Weighted Daily Rate |
| Has clear recent momentum to follow | Recency-Weighted Conservative |
| Needs a sensible general-purpose method | Recency-Weighted Growth |

Think about what it costs you
A bigger forecast is not the safe choice. It lowers stockout risk, but it also raises your purchase quantities, ties up more cash, adds storage cost, increases your aged-inventory exposure, and multiplies the damage if one of your seasonality numbers is wrong.
A smaller forecast protects cash but can leave you short and cost you sales you would have made.
Pick the scenario alongside your lead times, safety stock, order frequency, cash position, and what your suppliers can actually deliver. The scenario is one input to that decision, not the whole of it.

## Review it every month

After each month closes, compare what you forecast with what you sold. Try to separate two different errors: the scenario getting the level wrong, and your seasonality settings getting the shape wrong. They need different fixes.
Look into stockouts and one-off events before concluding the scenario is at fault. Then change the setting only when you have evidence that a different method fits the product better. Switching scenarios every month because last month missed will make your plan worse, not better.

## Automatic forecasts and forecasts you save

Automatic scenarios handle the repeatable monthly work. You can model the things history cannot know: a Prime Day promotion, an advertising expansion, a new marketplace, a wholesale order you have already booked, a price change, a deliberate sell-through, or a supply restriction. You can review the result and save the adjusted values for the months affected.
In inventory planning, a value you save takes priority for that product and month. The other months stay automatic unless you save values for them too, so you can adjust December without disturbing the rest of the year.
The monthly refresh does not recalculate or overwrite the values you saved. NeonPanel recalculates the automatic baseline underneath them, but your saved values remain unchanged and continue to be the values used for those products and months. They also remain separate from the calculation: saving December does not teach the scenario or change what it calculates for January.
The Sales Forecast Live report behaves differently right now: a saved forecast can hide the automatic numbers for months you did not save. Check the report after saving to confirm you are seeing what you expect.

## Mistakes we see most often

Treating Growth 10 as a forecast. It is a target you have chosen. It tells you what the business intends to support, not what demand is independently expected to do.
Using Trend and Seasonality right after a spike. A straight line drawn through an abnormal period extends that abnormality for two years. Check for promotions, listing changes, and stockouts before you trust the direction.
Assuming Robust Low gives you the lowest number. It is a fixed blend, and it can land above Seasonal Naive whenever your latest month is below the yearly average.
Assuming Availability-Based respects your stock. It does not look at inventory at all. Availability is handled downstream in replenishment planning.
Expecting a scenario to know your promotion calendar. It only knows history and your settings. Model known events, review their effect, and save the months you want to adjust.
Setting a company default and never revisiting it. A good default saves a lot of maintenance, but seasonal heroes, new launches, products you are deliberately selling through, and fast-moving SKUs often need their own setting.

## A sensible way to run this

For most established catalogs, pick a company default that fits the bulk of the catalog, set up a deliberate seasonality pattern where products genuinely have one, and then add product-level exceptions only where the demand behavior actually justifies it. Model known future events and save the affected months. Review plan against actual each month, and revisit the default when your catalog or growth strategy shifts.
Growth 10 is often a practical business default when a company genuinely plans to support moderate annual growth. Recency-Weighted Growth is what the system falls back to, but that is a technical default rather than a recommendation — it is not automatically the best commercial choice for your brand.

## Common questions

Does the scenario control what I reorder?
Not directly. It controls the demand forecast. Replenishment combines that with your stock on hand, inbound units, lead times, safety stock, and ordering rules.
Can different products use different scenarios?
Yes. Set a company default, then override individual products where it makes sense.
Does the forecast take my ad budget into account?
No. None of the operational scenarios read planned PPC spend. If you are planning an advertising push, you can model the lift and save the adjusted monthly values.
Does the forecast know I was out of stock?
Only indirectly, and not helpfully. The history will show the reduced sales, but the scenario cannot reconstruct the demand you missed. Review stockout periods before choosing a method, and model the demand the recorded sales did not capture when an adjustment is needed.
Which scenario applies if I have not chosen one?
If the product has no scenario set, your company default applies. If neither is set, or the setting is not one NeonPanel recognizes, it uses Recency-Weighted Growth.
How often does the forecast update?
It runs automatically on the 10th of each month at 08:00 UTC, and can also be generated on demand. See Monthly Forecast Refresh.
Can I change the scenario later?
Yes, any time. The next run uses the new one. Review the resulting forecast before you place an order against it.
If I save an override, does it replace everything?
In inventory planning it replaces only the months you saved. In the Sales Forecast Live report it can currently hide unsaved automatic months, so check the report after saving.
Will the monthly refresh change values I saved?
No. The automatic baseline is recalculated underneath, but your saved values stay unchanged and continue to take precedence for those products and months. The values you approved remain the ones inventory planning uses.
