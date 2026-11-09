# Recency-Weighted Conservative: Follow Your Recent Momentum Up or Down

Category: Forecast Scenarios · Article 9018 · Updated 2026-08-24

Follows your recent momentum, up or down.

## Introduction

Recency-Weighted Conservative looks at roughly your last four months of sales, gives the most recent days far more say than the older ones, draws a straight line through them, and extends that line forward. It suits an established product whose sales have been moving steadily in one direction and whose forecast you intend to review every month.
Despite the name, it is not a cautious forecast. It follows whatever direction your recent sales are pointing — up or down — at full speed.

## The short version

This scenario is a momentum model. If your last few months are climbing, it projects the climb; if they are falling, it projects the fall. There is no cap on the upside, no floor under the downside beyond zero, no discount, and no blend with a safer number. The word "Conservative" in the name does not describe anything the calculation does.
It works from your completed monthly sales totals, not from individual orders. Each month is turned into an average daily rate, that rate is repeated for every day of the month, and the most recent 120 or so of those daily figures become the model's working history. Newer days count much more: a day about 23 days older counts half as much as the newest day.
Two consequences matter most for planning. First, a small daily change becomes a large annual one, because the line is extended across hundreds of future days. Second, months with no sales at all are skipped entirely rather than counted as zero, so a quiet month does not drag the line down — it simply disappears, and the months on either side of it sit next to each other in the calculation.

## How it works

It only uses months that had sales
NeonPanel loads up to 18 months of your sales history for the product, kept separate by marketplace, sales channel, and country. Any month where the product sold nothing is not in that history at all — it is skipped, not recorded as a zero.
This scenario then builds its own much shorter window from what remains. So "the last four months" here means the last four months that had sales, which may stretch back further than four months on the calendar.
It takes your seasonality out first
If you have set a custom seasonality pattern, NeonPanel divides each historical month by that month's seasonality number before doing anything else. A product that sold 1,500 units in a month you have marked at 1.25 is treated as a 1,200-unit month.
The point is to stop a seasonal peak from being read as permanent growth. The model works with the 1,200-unit underlying level, and your seasonality gets added back at the very end.
It turns each month into a daily rate
Every month is divided by the number of days it contains, which puts long and short months on the same footing:

| Sales that month, after seasonality is removed | Days in month | Daily rate |
|---|---|---|
| 310 units | 31 | 10 units a day |
| 300 units | 30 | 10 units a day |
| 280 units | 28 | 10 units a day |

All three months represent the same underlying demand, and the calculation now treats them that way. This is a calendar adjustment, not a reading of real order dates. Nothing about when inside the month those units sold survives this step.
It spreads each month's rate across that month's days
The model then repeats each month's daily rate once for every day in that month. A month that averaged 10 units a day becomes 31 copies of 10. The following month at 12 units a day becomes 30 copies of 12.
The result looks like a daily sales history but is not one. It is a staircase: flat inside each month, with a step at every month boundary. Those steps are what the line is drawn through, so a single sharp month-to-month change is the strongest thing in the data.
It keeps about the last 120 days, weighting recent days far more
Older daily figures are dropped so the model concentrates on roughly four recent months' worth of days. The exact mix depends on the calendar: four 30-day months come to exactly 120, a 31-day month shifts which part of the oldest month survives, and February contributes 28 or 29. So the window is approximately 120 daily figures, not necessarily four whole calendar months — and because months with no sales are missing, the surviving blocks are simply laid end to end. Calendar gaps get squeezed out.
Each day back in time counts 97% as much as the day after it:

| How old the day is | How much it counts |
|---|---|
| Newest day | 1.00 |
| 1 day older | 0.97 |
| 7 days older | about 0.81 |
| 30 days older | about 0.40 |
| 60 days older | about 0.16 |
| 90 days older | about 0.06 |
| 119 days older | about 0.03 |

The practical reading of that table: a day about 23 days older counts half as much as the newest day. So although the window is nominally four months long, the newest month does most of the work. Do not treat this as an even average of four months.
It finds a starting level and a direction
Two numbers come out of the weighted history. The first is the starting daily level: the weighted average of all those daily figures, which lands much closer to your most recent month's rate than to the oldest one in the window.
The second is the direction — how fast the daily rate is climbing or falling. NeonPanel measures every day in the window against the newest day and works out the single straight line that best fits the weighted staircase. The answer is a change in units per day, for each further day into the future. A direction of 0.027 units a day means the projected rate rises by about 0.8 units a day over a 30-day month.
Nothing is done to that number afterwards. A rising direction is projected at full rate, and so is a falling one. This is the point where most other scenarios apply some kind of adjustment, and this one does not.
It adds the days back into months and puts seasonality back
The line is extended day by day across the whole forecast, which by default runs 24 months. Each future month's total is the sum of its own days, so 31-day months naturally come out larger than 30-day or 28-day months at the same rate.
A month's total is never allowed to go below zero. That is arithmetic, not caution: a falling line keeps walking the forecast down month after month until it reaches zero and stays there.
Finally, each future month is multiplied by that month's seasonality number. Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast, and a negative number can produce a negative forecast even though the underlying rate is never negative.
Revenue comes last
Forecast revenue is your forecast units multiplied by the most recent price NeonPanel has for the product. Price plays no part in working out the units — a planned price change will not raise or lower the unit forecast, and the scenario does not model how customers respond to price.

## A worked example

Take a home-goods product that sells continuously. To keep the arithmetic readable, assume its last four months all had 30 days, and that seasonality has already been taken out:

| Block of days | Sales that month | Days | Daily rate |
|---|---|---|---|
| Oldest 30 days | 300 units | 30 | 10 units a day |
| Next 30 days | 360 units | 30 | 12 units a day |
| Next 30 days | 420 units | 30 | 14 units a day |
| Most recent 30 days | 480 units | 30 | 16 units a day |

The 120-day working history is therefore 30 days at 10, then 30 at 12, then 30 at 14, then 30 at 16. Because each day back counts 97% of the day after it, the 16-unit block dominates and the 10-unit block barely registers.
Out of that come two numbers: a starting level of about 14.9 units a day, and a rise of about 0.027 units a day for each further day — roughly 0.8 units a day more each month. That level sits nearer the latest block's 16 than the four-block average of 13, which is the recency weighting doing its job.
Now project it. Suppose the first three forecast months have 31 days at seasonality 1.0, 30 days at 1.1, and 31 days at 0.9:

| Forecast month | Days | Total before seasonality | Seasonality number | Forecast units |
|---|---|---|---|---|
| Month 1 | 31 | about 474 | 1.0 | about 474 |
| Month 2 | 30 | about 483 | 1.1 | about 532 |
| Month 3 | 31 | about 525 | 0.9 | about 472 |

Month 3 has the strongest underlying demand of the three and still forecasts the fewest units, because its 0.9 seasonality number pulls it back. That is normal and is worth remembering when someone asks why a growing forecast dipped.
Carry the same line further and you see how a small daily number builds up. At 0.027 units a day, the projected rate is roughly 9.7 units a day higher after a year and roughly 19.5 units a day higher after two — more than doubling the starting level by the end of the default 24-month forecast, before seasonality.
This example shows the sequence rather than reproducing a live result exactly. Your real numbers will differ because of real calendar boundaries, the exact 120-day slice NeonPanel takes, your product's complete set of months, date indexing, and rounding.
The same product, falling instead
Reverse those blocks — 16, then 14, then 12, then 10 units a day — and the direction mirrors exactly. The starting level comes out at about 11.1 units a day and the daily rate falls by about 0.027 units for each further day, or roughly 0.8 units a day less each month.
That decline is not softened, not halved, and has nothing holding it up above zero. Month 1 comes to about 332 units before seasonality, Month 2 about 297, and the projected daily rate reaches zero about 418 days out — a little under 14 months. Every month after that forecasts zero.
This is a real difference from Recency-Weighted Growth, which cuts a decline in half when all of its measured directions are falling. Recency-Weighted Conservative does no such thing, despite having the more cautious-sounding name.

## When to choose it

Choose Recency-Weighted Conservative when the recent direction of a product is genuinely what you want your plan to follow, and you have the time to check it every month.
It fits best when the last four months or so represent how the product is actually trading now, when it sells often enough that a monthly average is meaningful, when a straight-line continuation is a reasonable planning assumption, and when your team understands that the line goes down as readily as up. It suits an established product whose run rate has moved steadily in one direction for reasons you can name — expanded distribution, a ranking improvement, a category shifting — rather than one that jumps around.
It also requires a deliberate custom seasonality pattern, since NeonPanel takes seasonality out before measuring direction and adds it back afterwards.

## When to avoid it

Avoid it if you need a forecast that is guaranteed to be cautious. Nothing here provides that, and choosing it because of its name is the most common way to be caught out.
Avoid it too when your recent months contain something other than ordinary demand: a stockout, a launch, a promotion, or a one-time bulk or wholesale order. Any of those becomes a direction the model projects for two years. The same applies to intermittent or mostly-zero demand, where months without sales are skipped and the few that remain can produce a direction that means nothing.
Reconsider it when your business has strong annual seasonality but no verified custom pattern, when a straight line is implausible over 12 or 24 months, when older history ought to balance a short-term move, when the product has too little history to support a direction, or when day-of-week and intra-month order patterns matter to your planning — this model cannot see them.
Finally, watch the name itself. If a stakeholder is likely to approve a plan labelled "Conservative" without inspecting it, the label does more harm than the model does good. If you need a deliberately cautious plan, define what cautious means for your business — a service-level target, a percentage discount, a cash ceiling — and build it into your buying rules instead.

## What it does to your buying plan

The upside risk is quantities. A daily rise that looks trivial in isolation is applied to every day of a 24-month forecast, so the second year can carry purchase quantities far above anything you have actually sold. Nothing caps that. Buy against the tail of this forecast without looking at it and you commit cash to inventory that may sit for a long time.
The downside risk is stockouts. Because a fall is projected at full speed and can reach zero within about a year from a modest decline, this scenario can quietly tell you to stop buying a product that is still selling. If a stockout caused those low months in the first place, the model reads your own supply problem as collapsing demand and plans for less — which causes the next stockout.
Month length moves your numbers in ways that are not demand. At an unchanged rate, a 31-day month forecasts about 11% more units than a 28-day February. Read that as a calendar effect, not a trend.
So look at the endpoints, not just next month. Check the 12-month and 24-month figures before you approve anything, against what you can realistically sell, store, and fund. Compare the same product under Recency-Weighted Growth and Recency-Weighted Daily Rate as a sanity check, and flag any month in the recent window that involved a stockout, promotion, launch, or one-time order.
Your plan can change a lot at each refresh
NeonPanel rebuilds the forecast automatically on the 10th of each month at 08:00 UTC. When a newly completed month arrives, a new block of daily figures joins the window, the window slides forward, part or all of the oldest block drops out, and every remaining day's weight changes. Both the starting level and the direction can change as a result.
Because the memory is short, a refresh is rarely a small extension of last month's plan. One new month can change the direction and reshape the entire 24-month forecast, so treat each refresh as a fresh forecast to review. NeonPanel calculates 24 months by default, though your planning view may show a shorter series than that.
Saved values stay yours, but only for the months you save
If you model and save a value with Kio for a specific month, that value takes priority for the product and month in inventory planning. Months either side stay automatic unless you save them too, and if more than one saved value exists for the same product and month, the most recent wins. The automatic forecast is still stored underneath.
At the next monthly refresh, NeonPanel redraws the automatic line underneath, but it does not recalculate or overwrite the value you saved. Your value remains unchanged and continues to be the one inventory planning uses for that product and month.
Saved values never become history. They do not enter the 120-day window, and the next month's direction is measured from completed actual sales rather than from the forecast you edited. Adjust one promotional month upward and the following months do not continue from that higher figure — they carry on from the automatic line unless you save them too.

## How seasonality changes the result

Seasonality wraps around the calculation rather than sitting inside it. Your historical months are divided by their seasonality numbers, the model measures level and direction on what is left, future months are built from the projected line, and each future month is then multiplied by its own seasonality number.
Here is what that does to a strong December. Suppose December sold 620 units, has 31 days, and you have set December to 2.0:
620 units ÷ 2.0 ÷ 31 days = 10 units a day.
The model sees a 10-unit-a-day underlying business, not a 20-unit one, so it will not read your holiday peak as a permanent step up. When a future December arrives, that month's projected days are added up and then multiplied by 2.0, and the peak returns exactly once.
The rules for your seasonality numbers are strict. There are twelve, January through December, and every one must be greater than zero. A zero wipes that month out of your forecast completely, and a negative number can turn a month's forecast negative even though the model's own underlying rate never goes below zero. Check the order as well as the values — a pattern entered out of sequence applies your December uplift to some other month.
If you have not set a custom pattern, every month is treated as normal: your history and your forecast are both left alone, though month length and the projected direction can still make consecutive months differ.
When a forecast looks wrong, compare your recent monthly sales with the same months after seasonality is removed. The second set is what the model actually measures, and the gap between the two often explains the result.

## What history it needs

This scenario needs completed monthly unit totals, the number of days in each of those months, your twelve seasonality numbers, a stable mapping of the product across marketplace, channel and country, and a recent unit price for the revenue figure. It does not need order-level data, and the daily figures it builds are never observed daily sales.
The window is about 120 daily figures, drawn from whichever months had sales, and it can take part of a month's block to fill out. History older than the window does not steady the line, so an 18-month record does not make this scenario more stable than a four-month one — the extra months are simply not used here.
One month of sales gives you almost no evidence of direction. Two or three give you a direction largely decided by a single step between blocks. Even with a full window, the roughly 23-day half weighting means the newest block dominates, so treat "four months of data" as the window's length, not as how evenly those four months are used.
Before you commit, check whether each month in the window is complete, whether stockouts made any of them artificially low, whether a promotion or wholesale order made any artificially high, whether the newest rate is a lasting change or a blip, whether returns and cancellations were recorded consistently, whether your SKU, channel, marketplace, or country mapping changed mid-history, whether your seasonality numbers are valid and in the right order, and whether a straight-line trend is plausible for as far ahead as you plan.
The scenario reads none of the following: stock on hand, inbound stock, in-stock percentage or lost-sales estimates, promotion calendars, PPC budgets, traffic or conversion forecasts, planned prices, supplier capacity, lead times, or competitor behavior. It sees units sold and nothing else, which is why it cannot tell a genuine demand shift from an availability problem, a data correction, or a one-off event.

## Things that surprise people

The name promises something the model does not do. There is no cap, discount, lower bound, blend, or reduction of either a rise or a fall, and it is not guaranteed to come out lower than any other scenario.
Months with no sales vanish rather than count as zero. If your product sold nothing in March, February's block sits directly against April's. The gap is squeezed out, so a quiet period does not pull the line down the way you would expect.
The daily figures are not daily sales. They are one monthly average repeated across a month's days. The staircase this creates has sharp steps at month boundaries, and those steps are exactly what the line is fitted through.
The four-month window behaves like a one-month window. With each day counting 97% of the next, a day 23 days older counts half as much. One recent event can dominate a window that is nominally four times longer.
A stockout looks identical to a decline. Sales you could not make are recorded as sales you did not make, and no availability correction is applied anywhere in this scenario.
A promotion looks identical to growth. Unusually high months are not trimmed back here — that belongs to Recency-Weighted Daily Rate — so one strong promotional month can set a rising direction for two years.
The zero floor is arithmetic, not a business rule. It stops a month going negative. It does nothing to stop the forecast walking down to zero and staying there.
A straight line assumes the same absolute change forever. The model does not detect saturation, a lifecycle stage, a trend break, or growth that tapers. It knows one shape and keeps drawing it.

## How it compares to the nearby scenarios

Recency-Weighted Growth builds its level from up to six months that had sales and measures direction over the latest 2, 6, and 12 such months, weighted 50%, 30%, and 20%. It is deliberately tilted toward growth: when any of those directions is flat or rising, the falling ones are treated as flat, and when all of them are falling, the combined decline is cut in half. Conservative does neither — it follows one recent direction at full strength. Growth is also what NeonPanel uses when it cannot resolve a valid supported scenario from the product or company default.
Recency-Weighted Daily Rate looks back up to 365 days rather than 120, trims unusually high months back before averaging them, and weights history so a month 45 days older counts half as much. It adds a gentle 5% annual growth floor by default and fits no direction at all. Choose Daily Rate for a smoothed recent level; choose Conservative when you want recent direction projected forward.
Availability-Based starts from your latest month that had sales and compounds a set annual growth rate, 30% by default, regardless of which way your recent months have moved. Despite its name it does not look at stock.
Trend and Seasonality draws one straight line through all of your loaded history, up to 18 months, treating every month equally. Conservative draws its line through about 120 recent daily figures and weights the newest far more heavily. Use Trend and Seasonality for a longer, steadier read.
Robust Low blends 70% of the 12-Month Moving Average with 30% of Seasonal Naive. It is a smoothed blend rather than a low bound, and Conservative is not guaranteed to sit below it — or above it.

## Common questions

Is this scenario guaranteed to be conservative?
No. There is no cap, discount, lower bound, blend, or reduction of the fitted direction anywhere in it.
What happens if my sales are rising?
The rise is projected at full rate for as long as the forecast runs, so a strong recent climb can produce a very large second-year plan.
What happens if my sales are falling?
The fall is projected at full rate too. Monthly totals never go below zero, but they can reach zero and stay there. A negative seasonality number can still produce negative units, which is why every month's number must be greater than zero.
Does it use my actual daily orders?
No. It converts each monthly total into an average daily rate and repeats that rate across the month's days.
Then why work in days at all?
Days give the calculation a timeline to weight by recency, a scale to measure direction on, and a natural way to handle 28-, 30-, and 31-day months. They add no information that was not already in your monthly totals.
Is the window exactly four months?
No. It is about 120 daily figures. Calendar boundaries decide which parts of which months are included, and months with no sales are not there at all.
Does it trim promotional spikes?
No. That belongs to Recency-Weighted Daily Rate.
Does it account for stockouts?
No. A stockout shows up as a lower daily rate and can set a falling direction.
Why did a month go down when the trend is up?
A lower seasonality number, a shorter month, or both. The underlying rate can rise while forecast units fall.
How is revenue worked out?
Forecast units multiplied by the most recent price NeonPanel has. Price never affects the unit forecast.
Does a value I save change next month's forecast?
No. Saved values are not history. In inventory planning they take priority for the exact product and month you saved, and the next automatic forecast is still built from your completed actual sales.
What happens when a product has no scenario set?
It can be, if Recency-Weighted Conservative is your company default. When a product has no scenario set, NeonPanel tries the company default first. Recency-Weighted Growth is used only when NeonPanel cannot resolve a valid supported scenario.
