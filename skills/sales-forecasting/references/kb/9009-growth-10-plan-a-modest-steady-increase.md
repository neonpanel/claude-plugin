# Growth 10: Plan a Modest, Steady Increase

Category: Forecast Scenarios · Article 9009 · Updated 2026-08-24

Works your latest level up toward about 10% growth over a year.

## Introduction

Growth 10 takes the units you sold in the last completed month, removes the seasonal effect of that month, and then lifts that level a little more each month along a fixed curve that reaches 110% by the twelfth month. It suits a product you have decided to grow gently — more ambitious than holding steady, far less aggressive than Growth 20 or Growth 30. It is a target you choose, not a prediction NeonPanel makes about what your sales will do.

## The short version

| What you want to know | What Growth 10 does |
|---|---|
| Where does the starting number come from? | Units sold in the last completed calendar month, divided by that month's seasonality number. |
| What shapes the growth? | A fixed percentage curve in NeonPanel's settings, rising from 100% to 110% over the first year. |
| Does it mean 10% growth every month? | No. About 10% higher by the twelfth month than where you started. |
| Is the first forecast month 10% up? | No. Month one is 100%, the same as your starting level. |
| Is every month higher than the last? | Not on the chart. Seasonality easily outweighs a one-point step in the curve. |
| Does it learn a trend from your sales? | No. The curve is the same regardless of your history. |
| Does it read PPC spend, promotions, or price plans? | No. None are inputs to the units. |
| Does it check stock on hand or inbound shipments? | No. It plans demand without checking you can supply it. |
| What happens next month? | It rebuilds from the newest completed month and restarts the curve at month one. |
| Can a month be adjusted? | Yes. A value you save wins for that product and month in inventory planning. |
| What if last month had no sales? | No automatic forecast for that product. NeonPanel does not substitute an average. |
| How far ahead does it run? | 24 months, reaching roughly 122% by the second-year endpoint. |
| The name causes more confusion than anything else here. Growth 10 is not 10% growth every month, not an immediate 10% jump in the first forecast month, not a prediction that you will grow 10%, and not a guarantee that your twelfth month lands exactly 10% above the raw sales of your starting month. It is a gentle ramp: 100% in month one, stepping up about a point at a time, 110% in month twelve. | It is also not calculated from your marketing. Your ad budget, campaign calendar, promotions, and pricing may all be good reasons to choose this target, but none are read by the calculation. You choose Growth 10 because your plans justify it; the scenario just applies percentages. |

## How it works

It starts from one month, not from your trend
Growth 10 looks at a single number: the units you sold in the calendar month immediately before the forecast. It does not average your history or draw a line through recent months to find a direction. The direction is already decided; only the starting height comes from your data.
That rule is stricter than it sounds. Your sales history in NeonPanel keeps only the months that actually had sales — a month with no sales at all is skipped entirely rather than stored as a zero. For most scenarios that just means an empty month is passed over. For Growth 10 it decides everything, because the one month it needs is the month immediately before the forecast.
It takes the season out of that month
Sales from a single month carry that month's seasonal effect, so NeonPanel removes it before applying the curve: it divides that month's units by that month's seasonality number.
A seasonality number of 1.4 means the month usually runs 40% above your normal pace; 0.8 means 20% below. So 100 units in a neutral 1.0 month gives a starting level of 100. The same 100 units in a peak 2.0 month gives 50, because half of that was the season. And 100 units in a quiet 0.8 month gives 125, because the month worked against you.
This keeps a holiday peak from becoming the base of your growth plan. It also means the seasonality number on your starting month has to be right — if it is wrong, your starting level is wrong, and the whole curve sits on it.
It applies a rising percentage to that level
Each future month gets its own percentage from the Growth 10 curve, applied to the starting level. In the first year the curve runs 100, 101, 102, 103, 104, 105, 105, 106, 107, 108, 109, and 110 percent, and by the end of the second year it reaches roughly 122%.
Every percentage is measured against your original starting level, not the month before it. Month four at 103% means your starting level times 1.03, not month three plus another 3%. That is why a curve ending at 110% really does end about 10% up rather than snowballing into something larger.
Those percentages live in NeonPanel's settings data rather than the application code, so the curve can be adjusted without a software release.
It puts the season back in
For each future month, NeonPanel multiplies the planned units by that calendar month's seasonality number. That is the number you see in the forecast and the number that drives your purchase planning, before any value you model and save. The curve tells you the ambition; the seasonality tells you when the units land. NeonPanel writes 24 months this way.
When there is nothing to start from
Growth 10 needs the month immediately before the forecast to have sales above zero. If that month is missing, its sales are zero, or removing its seasonality leaves zero or less, NeonPanel produces no automatic forecast rows at all for that product. It does not fall back to a recent average and does not reach back for an older month that did have sales.
An empty forecast is a signal to investigate, not a system error. Check whether the product was out of stock, the listing was suppressed or paused, channel data arrived late, or demand genuinely stopped. If you still need a plan, move the product to a scenario that uses a longer stretch of history, or model and save the monthly values. One other case does the same thing: if the percentage curve cannot be loaded, the scenario produces no rows rather than guessing at a default.

## A worked example

Take the Harbor Mug, a ceramic mug that sells all year and spikes in the gift season. The last completed month is December, which sold 100 units. Its saved seasonality pattern, January through December, is 0.8, 0.8, 0.9, 1.0, 1.0, 1.2, 1.0, 1.0, 1.0, 1.1, 1.5, 2.0 — June runs 20% above its usual pace, December at double.
Find the starting level. December's seasonality number is 2.0, so 100 units divided by 2.0 gives a starting level of 50 units a month. The curve builds on that 50, not on December's seasonal 100.
Apply month one. The curve starts at 100%, so month one is 50 times 1.00, or 50 units before seasonality. January's number is 0.8, so January's forecast is 50 times 0.8, or 40.00 units.
Jump to month twelve. The curve reaches 110%, so the planned level is 50 times 1.10, or 55 units. Next December's number is 2.0, so December is 55 times 2.0, or 110.00 units.
Here is the whole first year. Every row is the same arithmetic: 50 units, times that month's curve percentage, times that month's seasonality number.

| Forecast month | Curve | Seasonality number | Units the plan calls for |
|---|---|---|---|
| January | 100% | 0.8 | 40.00 |
| February | 101% | 0.8 | 40.40 |
| March | 102% | 0.9 | 45.90 |
| April | 103% | 1.0 | 51.50 |
| May | 104% | 1.0 | 52.00 |
| June | 105% | 1.2 | 63.00 |
| July | 105% | 1.0 | 52.50 |
| August | 106% | 1.0 | 53.00 |
| September | 107% | 1.0 | 53.50 |
| October | 108% | 1.1 | 59.40 |
| November | 109% | 1.5 | 81.75 |
| December | 110% | 2.0 | 110.00 |

The planned level moves gently, from 50 units to 55 over the year. The visible forecast swings far more, from 40 to 110 units, because the seasonality numbers range from 0.8 to 2.0. Growth is the small movement; season is the big one.
Three numbers answer three different questions: the curve percentage says how ambitious a month is compared with where you started, the planned level before seasonality says what pace you are buying toward, and the final units say what to have in stock.
With that in mind the odd-looking rows make sense. February's 40.40 units barely clear January's 40.00 because both carry a 0.8, so a one-point step moves almost nothing. July's 52.50 sits below June's 63.00 even though July is further along the curve, purely because June carries a 1.2 and July a 1.0. A month can be more ambitious and still smaller.
Across the twelve months the plan totals about 703 units. The same product on Flat Scenario totals 665, so this curve asks you to buy roughly 38 more units over the year — about 6% more inventory and 6% more cash committed.
The table keeps decimals so you can check the arithmetic; Growth 10 stores fractional units and your screens may round them.

## When to choose it

Growth 10 fits when a modest increase is a real, agreed objective rather than optimism. It works when your team has approved a gentle growth target, when recent demand is a reliable place to start from, and when you want a transparent target instead of a trend inferred from history.
It also fits a mature product expected to expand gradually, or one whose channel reach, distribution, or repeat-purchase activity supports a cautious increase. Choose it when your inventory team can absorb a small ramp without uncomfortable exposure, and when Flat looks too cautious but Growth 20 is more than you can defend. Two more situations suit it:

Your history is too short or too disrupted for a statistical scenario, while the last completed month is usable.
You want one repeatable growth assumption to apply across many products without tuning each one.

The business case can rest on marketing plans, distribution deals, or new channels, but the algorithm reads none of that. You choose Growth 10 because your plans justify the target, not because it calculated the target from your plans.

## When to avoid it

Avoid Growth 10 when nothing concrete supports an upward target. A curve that rises on its own is easy to accept and expensive to be wrong about. Skip it, or model a more suitable plan, when:

The last completed month is inflated by a one-time promotion or launch spike, so the curve starts too high.
The last completed month is artificially low after a stockout or suppressed listing, so it starts too low.
The product is declining, being discontinued, or close to saturation.
A stronger approved target makes Growth 20 or Growth 30 the honest choice.
You need an evidence-based estimate from your actual sales rather than a chosen target.
Known events call for irregular, month-specific numbers that no smooth curve can express.
Your seasonality pattern is unverified or changing materially, because curve and pattern multiply together.
Inventory, cash, or production constraints cannot support the units the curve implies.
You expect an advertising or pricing plan to be modeled for you.

Growth 10 is not automatically the safe middle option. It over-forecasts if the starting month or the target is too high, and under-forecasts if your real momentum is stronger than the curve.

## What it does to your buying plan

Growth 10 asks you to buy slightly ahead of where you are, every month. In the Harbor Mug example that is 38 extra units over a year against a flat plan — small in isolation, but it is inventory paid for before the demand exists, and it repeats on every product you assign it to. Six percent across a catalog is a real cash decision, not a rounding difference.
The peak months are where the risk concentrates. November and December together call for about 192 units, more than a quarter of the year's plan, and those are the months where the growth percentage and the seasonal peak stack up. On a long lead time you commit that cash months ahead, on a plan built from a single December.
Growth 10 never caps its plan against what you can supply. It does not read stock on hand, inbound shipments, factory capacity, or your budget, so feasibility stays a separate check. It also does not correct for demand you lost: if a stockout held the starting month below what customers wanted, the curve grows from that depressed number, and a modest target on an artificially low base can still leave you short.
The reverse risk is just as real. On a product that is genuinely accelerating, a curve that only reaches 110% in a year leaves you under-bought and stocking out — which looks like a demand problem but is a plan that was too gentle. On a mature or declining product the same gentle rise piles up stock you never sell through, so watch sell-through and lifecycle stage alongside the forecast.

## Adjusting a month with a saved value

A forecast you model and save is not an automatic scenario; it is an adjustment on top of one. In inventory planning, a saved value takes priority for that product and forecast month: unsaved months keep using Growth 10, and saving one month does not touch its neighbors. If more than one value exists for the same month, the most recent is used.
Say Growth 10 plans 53.50 units for September and a confirmed order makes 70 the right number. You model and save 70; September uses 70, and October stays on Growth 10 unless you save it too. At the next monthly refresh, NeonPanel recalculates the automatic curve underneath, but it does not recalculate or overwrite your saved 70. That value remains unchanged and continues to be the one inventory planning uses for September.
Your saved value does not alter the curve or become the next starting point. The next automatic run still starts from actual sales in the newest completed month.
One caveat: that month-by-month behavior is what inventory planning does. The Sales Forecast Live view picks one active run first and gives saved values priority, so there a saved value can hide the automatic numbers for unsaved months. If the two screens disagree, that is why.

## How seasonality changes the result

Growth 10 has two layers. The bottom layer is your planned level, lifted gently from 100% to 110% over a year. The top layer is your twelve seasonality numbers, which decide when those units are needed. The forecast is the two multiplied together, and on a seasonal product the top layer dominates.
Watch that multiplication at the peaks, because the effects stack. A 110% month landing on a 2.0 December is 2.2 times your starting level — 110 units against a starting level of 50. Test the peak months in units before accepting the plan; the endpoint of the curve tells you little about the biggest purchase order of the year.
It also explains why a later month can be smaller than an earlier one: the curve only rises, but a low-season number wipes out several points of growth, as July's 52.50 does against June's 63.00.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast, because the planned units get multiplied by nothing. A negative number is worse: it produces a negative forecast, which no scenario is designed to handle. Large numbers are accepted as written, so a stray 15 where you meant 1.5 produces a plan ten times too big for that month.
NeonPanel expects exactly twelve values, January to December. If the list is blank or has the wrong number of entries, every month falls back to 1.0. A single blank or non-numeric entry becomes 1.0 for that month while the rest of your pattern is kept.
If you leave seasonality neutral, your forecast rises smoothly along the curve and does nothing else. Growth 10 will not detect a seasonal shape from your history and apply it for you.

## What history it needs

Growth 10 needs one month: the calendar month immediately before the forecast, with sales above zero. That is the whole requirement. A long, clean history does not improve the result and will not compensate for a missing final month. NeonPanel loads up to 18 months of history for the monthly run, and other scenarios use much of it, but Growth 10 reads only that last completed month.
Remember that months with no sales are not stored as zeros; they are absent. A product that sold nothing last month has no month for Growth 10 to work with, and the result is an empty forecast. Intermittent sellers, seasonal items that go quiet out of season, and anything out of stock for a full month all hit this.
History is tracked separately for each product in each marketplace and channel, so a product can produce a forecast in one marketplace and nothing in another. Check that combination rather than the product overall.

## Things that surprise people

The curve restarts every month. Growth 10 does not carry on from where it was. Each rebuild takes the newest completed month as the new starting point and begins the curve again at 100%. If January closes at 42 units against a 0.8 seasonality number, the new starting level is 42 divided by 0.8, or 52.5. February then gets month one at 100% and February's own 0.8: 52.5 times 1.00 times 0.8 is 42.00 units, against the 40.40 the earlier run planned for February. Nothing was learned; the starting point moved.
The restart cuts both ways. A strong month lifts the whole two-year plan, a weak one lowers it, a one-off anomaly spreads into every future month until the next rebuild, and the seasonality landing on each step of the curve shifts as the calendar moves on. Compare each refreshed plan with the previous one rather than assuming the old curve slid along.
A quiet month can erase the forecast. One month with no sales means no automatic rows at the next run, which bites hardest on slow movers and new marketplaces where sales are sporadic.
"10" is an endpoint, not a monthly rate. The curve gains about a point a month and ends the first year at 110%. Whether that shows up as 10% year-over-year growth depends on the monthly restart, your seasonality, the live curve, and which months you compare. Judge it by the monthly units, not the name.
A smooth curve looks more authoritative than it is. The line is predefined, identical for every product using it, and independent of your momentum. Write down why you expect to hit the target, and compare actual sales against the plan after every refresh.
It cannot see the reason behind a number. A promotion, a competitor going out of stock, a viral week, a warehouse fire — all look identical to it. It reads units, nothing else.
It is not the default. If no scenario is set and no company default applies, or if the scenario on a product is something NeonPanel does not recognize, the forecast uses Recency-Weighted Growth. An unrecognized setting does not fall back to your company default either; it goes straight to Recency-Weighted Growth.
Revenue follows units. Growth 10 plans units and then applies a price, normally holding price at the previous month's level and calculating revenue as forecast units multiplied by that price. Planned price changes do not change the units.
It rebuilds on a schedule. Automatic forecasts refresh on the 10th of each month at 08:00 UTC, so a correction to seasonality or to a product's scenario reaches the automatic numbers at the next run rather than immediately.

## How it compares to the nearby scenarios

Against Flat Scenario. Identical machinery, different curve. Flat holds at 100% every month; Growth 10 lifts the same starting level to 110% by month twelve. Both start from the last completed month, remove its seasonality, and reapply future seasonality. Choose Growth 10 when a modest increase has been approved and you can fund it; choose Flat when holding your level is the honest plan.
Against Growth 20. The same idea with a steeper ramp: Growth 20 starts above your current level in month one, at 101%, and reaches 121% by month twelve, which means more inventory exposure and more cover if demand accelerates. Do not move to it just because recent sales went up — move when the business has accepted the stronger plan and can carry the stock.
Against Trend and Seasonality. Growth 10 states an intention; Trend and Seasonality measures what your sales have been doing and extends that straight line forward. Use Growth 10 when the plan should reflect a decision and Trend and Seasonality when it should reflect your history — remembering that a straight line extends a trend past the point where it is believable.
Against Recency-Weighted Growth. That one works from your recent daily sales pace and the directions visible in recent months rather than a stored curve. It is what NeonPanel uses when it cannot resolve a valid supported scenario after checking the product and, when the product setting is blank, your company default. Growth 10 is not the system fallback, though it can be your company default.
Against saved adjustments. Growth 10 gives one reusable rule for every future month; values you save give exact numbers where the plan is irregular or event-driven. Many teams use both.

## Common questions

Is Growth 10 a statistical forecast, or a prediction that I will grow 10%?
Neither. It is a predefined target curve applied to your last completed month after that month's seasonality is removed. It applies configured percentages; deciding whether the target is realistic is your job.
Does it add 10% every month, or 10% in the first month?
Neither. Month one is 100% of your starting level, and the curve climbs gradually to 110% by month twelve.
Does Growth 10 use all my history?
No. The starting point is the last completed month with its seasonality removed. The monthly run loads up to 18 months for other purposes, but the curve is not fitted to any of it.
What happens after an unusually strong month?
The whole curve starts from a higher level. Confirm the month is representative before accepting the purchase plan.
Does it know about stockouts or my inventory limits?
No. Stock availability is not an input, so a stockout in your starting month quietly lowers the whole plan, and the curve can call for more units than you could ever source. Feasibility is a separate review.
Does my PPC spend or planned pricing change the units?
No. Ad budgets, campaign calendars, and price plans are not read by the calculation, and the scenario does not model how price affects demand. Model those effects and save the adjusted months when you approve the target.
Can Growth 10 produce more than 10% year-over-year growth?
It can land above or below that. The monthly restart, your seasonality, the live curve, and which months you compare all affect the reported figure. Evaluate the actual monthly units.
What does neutral seasonality do?
Every month gets a 1.0, so you see the growth curve on its own. It does not learn a seasonal curve from your history.
Can only one month be changed?
In inventory planning, yes. A saved value overlays that month while unsaved months continue on Growth 10. The Sales Forecast Live view can hide the automatic numbers for unsaved months.
Will a saved value influence the next automatic run?
No. It is not training data and does not change the curve or the starting point.
Is Growth 10 the default scenario?
It can be your company default, but it is not the system fallback. When a product has no scenario set, NeonPanel tries your company default first. Recency-Weighted Growth is used only when NeonPanel cannot resolve a valid supported scenario.
How far ahead does the curve run?
24 months, reaching roughly 122% by the second-year endpoint.
