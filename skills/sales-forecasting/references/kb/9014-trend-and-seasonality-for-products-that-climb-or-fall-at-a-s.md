# Trend and Seasonality: For Products That Climb or Fall at a Steady Pace

Category: Forecast Scenarios · Article 9014 · Updated 2026-08-24

Extends the direction your sales have been heading, then applies your seasonality.

## Introduction

Trend and Seasonality draws a straight line through your product's past months of sales and extends that line into the future, then reshapes each future month with your seasonality pattern. It suits a product with a clear, steady direction — a listing that adds roughly the same number of units every month, or one sliding down at an even pace. If your sales are flat, erratic, or brand new, one of the other scenarios will serve you better.

## The short version

NeonPanel uses every month that had sales in the last 18 months. Months with no sales are skipped entirely — not counted as zero, not filled in.
Your seasonality comes out of history before the line is drawn, so a December peak that happens every year is not mistaken for growth. It goes back on afterwards, month by month.
The line moves by a fixed number of units per step, not by a percentage. Fifty units a month stays fifty units a month forever; the growth never builds on itself.
One step means one month that had sales, not one month on the calendar. Gaps in your history get squeezed out, so the line climbs faster than the calendar alone would suggest.
The underlying line never goes below zero. A negative seasonality number you enter yourself can still push the final units for a month below zero.
Final unit numbers are rounded to whole units before revenue is calculated.
The forecast runs 24 months ahead by default, and the direction never changes across those 24 months.
It does not know about promotions, price changes, competitors, stockouts, discontinuation dates, or the point where growth flattens out. You do.

## How it works

It starts by collecting the months that had sales. NeonPanel looks back across up to 18 completed calendar months and keeps every month where units sold were greater than zero. A month with no sales is not included and is not replaced with a zero — nothing is invented to fill the hole. This scenario uses all of those months, however many there are; unlike 12-Month Moving Average, it does not stop at twelve.
Then it takes your seasonality out of each of those months. A seasonality number of 1.50 for November means November usually runs 50% above normal, so NeonPanel divides to find the underlying level: 1,650 bottles sold in November divided by 1.50 is an underlying level of 1,100. This is the step that stops your annual holiday spike from being read as growth.
Next it draws one straight line through those underlying figures. It is the same best-fit trendline a spreadsheet draws: the line is placed so the total of the squared gaps between it and your actual months is as small as possible. Your months do not have to sit on the line, and usually they will not.
Two numbers come out of that — where the line starts, and how much it adds or takes away with each step. If it adds 50, the underlying level rises by 50 units for every step in your history and then by another 50 for every future month. If it takes away 30, the level falls by 30 per step. If it adds nothing, the level is flat and the forecast looks like a flat baseline shaped by seasonality.
The step is where people get caught out. A step is one month that had sales, not one month on the calendar. Say you sold in January, nothing in February, then sold again in March and April: those three become steps one, two and three, and February is simply not there. A rise that actually took three calendar months is treated as covering two steps, which makes the per-step climb larger — and every future month then advances one step. The forecast climbs faster per calendar month than your real history did, and the more gaps you have, the more pronounced it gets.
The line then continues into the future. If it starts at 600 bottles and adds 50 per step, the first forecast month is step 18 and comes to 1,500 bottles. Step 24 is the seventh forecast month and comes to 1,800. The twenty-fourth forecast month is step 41 and comes to 2,650 bottles. There is no bend, slowdown or reversal anywhere along those 24 future months.
Wherever the line would go below zero, NeonPanel uses zero instead. A declining product's line eventually crosses into negative numbers on paper. If the arithmetic produces minus 75 units, the underlying level for that month is zero, and it stays zero for every month after that.
Finally seasonality goes back on, month by month. An underlying level of 1,750 bottles in a December with a seasonality number of 2.00 becomes a forecast of 3,500 bottles; the same 1,750 in a January with 0.80 becomes 1,400. That number is rounded to whole units, and revenue is those units multiplied by your price — the average unit price across up to the last three months that had sales, or the average across your whole history if fewer than three are available.

## A worked example

You sell the Harbor 32 oz insulated bottle. Distribution has widened steadily and reviews have piled up, and the product has been adding roughly 50 bottles a month to its underlying level for a year and a half. It also has a real seasonal shape: a soft first quarter, a summer lift in June, and a heavy gifting peak in November and December. It is July, and NeonPanel has 18 completed months to work with, from January of Year 1 through June of Year 2.

Your seasonality pattern

| Month | Seasonality number | What it means |
|---|---|---|
| January | 0.80 | Usually runs 20% below normal |
| February | 0.80 | 20% below |
| March | 0.90 | 10% below |
| April | 1.00 | Normal |
| May | 1.00 | Normal |
| June | 1.20 | 20% above |
| July | 1.00 | Normal |
| August | 1.00 | Normal |
| September | 1.00 | Normal |
| October | 1.10 | 10% above |
| November | 1.50 | 50% above |
| December | 2.00 | Twice normal |

Your history, with seasonality taken out
Every one of these 18 months had sales, so every one of them counts and the steps line up with the calendar.

| Month | Step | Bottles sold | Seasonality number | Division | Underlying level |
|---|---|---|---|---|---|
| Year 1 January | 0 | 480 | 0.80 | 480 ÷ 0.80 | 600 |
| Year 1 February | 1 | 520 | 0.80 | 520 ÷ 0.80 | 650 |
| Year 1 March | 2 | 630 | 0.90 | 630 ÷ 0.90 | 700 |
| Year 1 April | 3 | 750 | 1.00 | 750 ÷ 1.00 | 750 |
| Year 1 May | 4 | 800 | 1.00 | 800 ÷ 1.00 | 800 |
| Year 1 June | 5 | 1,020 | 1.20 | 1,020 ÷ 1.20 | 850 |
| Year 1 July | 6 | 900 | 1.00 | 900 ÷ 1.00 | 900 |
| Year 1 August | 7 | 950 | 1.00 | 950 ÷ 1.00 | 950 |
| Year 1 September | 8 | 1,000 | 1.00 | 1,000 ÷ 1.00 | 1,000 |
| Year 1 October | 9 | 1,155 | 1.10 | 1,155 ÷ 1.10 | 1,050 |
| Year 1 November | 10 | 1,650 | 1.50 | 1,650 ÷ 1.50 | 1,100 |
| Year 1 December | 11 | 2,300 | 2.00 | 2,300 ÷ 2.00 | 1,150 |
| Year 2 January | 12 | 960 | 0.80 | 960 ÷ 0.80 | 1,200 |
| Year 2 February | 13 | 1,000 | 0.80 | 1,000 ÷ 0.80 | 1,250 |
| Year 2 March | 14 | 1,170 | 0.90 | 1,170 ÷ 0.90 | 1,300 |
| Year 2 April | 15 | 1,350 | 1.00 | 1,350 ÷ 1.00 | 1,350 |
| Year 2 May | 16 | 1,400 | 1.00 | 1,400 ÷ 1.00 | 1,400 |
| Year 2 June | 17 | 1,740 | 1.20 | 1,740 ÷ 1.20 | 1,450 |

Look at the last column: 600, 650, 700, 750, climbing by exactly 50. This example is deliberately tidy, so the best-fit line lands on those points exactly — it starts at 600 bottles and adds 50 per step. Real data is messier and the line runs through the middle of the scatter rather than touching every point, but the two numbers that come out are the same two numbers.
What the forecast looks like
July of Year 2 is step 18. The underlying level is 600 plus 50 times 18, which is 1,500 bottles. July's seasonality number is 1.00, so the forecast is 1,500 bottles.

| Forecast month | Step | Arithmetic | Underlying level | Seasonality number | Forecast |
|---|---|---|---|---|---|
| Year 2 July | 18 | 600 + 50 × 18 | 1,500 | 1.00 | 1,500 bottles |
| Year 2 August | 19 | 600 + 50 × 19 | 1,550 | 1.00 | 1,550 bottles |
| Year 2 September | 20 | 600 + 50 × 20 | 1,600 | 1.00 | 1,600 bottles |
| Year 2 October | 21 | 600 + 50 × 21 | 1,650 | 1.10 | 1,815 bottles |
| Year 2 November | 22 | 600 + 50 × 22 | 1,700 | 1.50 | 2,550 bottles |
| Year 2 December | 23 | 600 + 50 × 23 | 1,750 | 2.00 | 3,500 bottles |
| Year 3 January | 24 | 600 + 50 × 24 | 1,800 | 0.80 | 1,440 bottles |

Three things in that table are worth pausing on. December's 3,500 bottles is not a wild forecast — it is a 1,750-bottle underlying level times a 2.00 holiday number. January then drops to 1,440 even though the underlying level rose to 1,800, purely because January's number is 0.80. And that December-to-January fall does not mean the trend reversed; underneath, it is still adding 50 bottles a month. Because the line was drawn on history with seasonality already removed, the annual holiday peak did not create the climb.
The same product, but declining
Now take an item whose line starts at 300 units and loses 75 per step. Step four puts the arithmetic at zero. Step five would put it at minus 75, so NeonPanel uses zero instead, and every month after that is zero too. Multiplying that zero by any finite seasonality number — positive or negative — still gives zero. A negative final forecast can happen earlier, while the underlying line is still above zero, if that positive level is multiplied by a negative seasonality number.

## When to choose it

Trend and Seasonality is the right pick when your product's underlying monthly sales — once you mentally strip out the seasonal shape — have been moving in one reasonably consistent direction, and you have a business reason to expect that to continue. It also needs enough comparable monthly history behind it and a seasonality pattern you actually believe. One more condition: the growth has to be roughly steady in units rather than a percentage that builds on a bigger number each month, because this scenario adds a fixed number of units per month. If your product is doubling, it is the wrong tool.
It earns its keep in situations like these:

A mature product picking up a fairly consistent number of units each month after you widened distribution or added a marketplace.
A product in an orderly decline, drifting down month by month, with no cliff-edge discontinuation planned.
A subscription or refill item whose subscriber base grows or shrinks by roughly the same absolute number every month.
An established listing where a flat average would visibly lag the product — if underlying sales rose from 600 to 1,450 in 50-unit steps, the latest twelve months average 1,175, which still sits well below the current direction.

## When to avoid it

Pick something else when:

The product is in a volatile launch period, where a few early months can produce a steep line out of ordinary noise.
Growth is exponential, arrives in steps, or is expected to level off. The line cannot bend.
History contains a one-time jump — a channel launch, a store closure, an acquisition, a listing merge — that the line will read as a trend.
The recent direction came from a promotion that will not repeat.
The product sells intermittently, with many months at no sales. Those gaps get squeezed out and steepen the line.
You have a discontinuation date or a planned stock exit and need a deliberate cutoff. The line glides toward zero on its own schedule, not yours.
Older history belongs to a different version of the business — different price, pack size or channel.
The direction is really about stock rather than demand: sales fell because you were out of stock, and the line reads that as customers leaving.
Management wants a specific percentage growth target. Use a planned growth scenario.
Your seasonality pattern is neutral but the product genuinely has a seasonal shape.
Nobody will review the far end of the forecast and an overextended line would be expensive.

## What it does to your buying plan

Every future month is a little different from the one before it, so your purchase orders keep moving in one direction. That is exactly what you want on a genuinely growing product, and it is how you get burned on a product whose growth was borrowed.
On a rising product, your risk is overstock and tied-up cash. The line adds the same number of units every month for 24 months, so the forecast twenty months out sits far above today's run rate. If the rise was temporary — a viral month, a competitor briefly out of stock, a coupon campaign — you will be ordering against a level the product never reaches, and because the arithmetic looks precise, an inflated far-future number can pass review unquestioned.
On a falling product, your risk is stockout and lost sales. If recent low months came from being out of stock rather than from customers losing interest, the line reads the dip as demand leaving and keeps walking down. Order to that and you stay short, which produces another low month, which steepens the decline at the next refresh. You can model the demand the stockout hid and save the adjusted months, breaking that loop.
The zero floor is not a phase-out plan. A declining line reaching zero produces a run of zero-unit months. That keeps the number from going negative, but it is not a considered end-of-life plan.
Watch where cash is committed against the far months. Uncertainty grows with distance and the line does not widen to reflect it. Treat months beyond your lead time plus a buffer as a planning signal rather than a commitment.
Every refresh redraws the whole line. The forecast is recalculated on the 10th of each month at 08:00 UTC. At each run, newly completed months with sales join the history, months older than the 18-month window drop out, seasonality is taken out again, the line is drawn from scratch, and the future months are recalculated. A completed month with no sales is skipped rather than added as zero. One new selling month can change both where the line starts and how steeply it moves, so the entire 24 months can shift.
Values you save stay in control. In inventory planning, a saved value takes priority over the automatic forecast for that product and month. If the automatic November forecast is 2,550 bottles but a confirmed campaign supports 3,100, you model and save 3,100; November plans at 3,100, while December stays on its automatic 3,500 unless you save that too.
At the next monthly refresh, NeonPanel redraws the automatic line underneath, but it does not recalculate or overwrite your saved 3,100. The value you approved remains unchanged and continues to be the one inventory planning uses for November. It does not feed back into the line — it is a planning decision, not a sales record. If you are adjusting month after month, a different scenario may be the better standing rule; saving one month does not rebase the line.

## How seasonality changes the result

Seasonality does two separate jobs here. On the way in, it divides your history so the line describes an underlying level rather than a calendar shape. On the way out, it multiplies each forecast month so the plan reflects when your customers actually buy. That means one pattern moves the forecast twice — once by changing the direction of the line and once by changing the shape of the output. In the worked example the underlying level climbed straight through the year while the forecast swung from 1,440 bottles in January to 3,500 in December, both from the same twelve numbers.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast: the underlying level is multiplied by nothing and the plan becomes zero units. A negative number can produce a negative forecast while the underlying level is still positive; once the underlying line has reached its zero floor, multiplying it by a negative still gives zero.
If your pattern is blank or does not have exactly twelve entries, NeonPanel replaces the entire pattern with twelve 1.00 values. If one entry is blank or not a number, only that month becomes 1.00 while the rest are kept. The first case quietly removes all seasonality; the second flattens only the affected month.
Wrong numbers can invent a trend that does not exist. If your December number is set at 1.20 when the real holiday lift is closer to 2.00, December's underlying level comes out far higher than the months around it, and the line drawn through those figures gets pulled upward. You end up with a growth trend that is really a mis-set holiday number, repeating twice across 18 months of history.
With a neutral pattern, all twelve numbers are 1.00. Dividing by 1.00 changes nothing, so the line is drawn straight through your recorded monthly units, and multiplying by 1.00 changes nothing either, so the forecast is a plain straight line with no seasonal shape. That is also what you get if you have not set a usable pattern. NeonPanel does not work out your seasonal curve from history on its own.

## What history it needs

The line is drawn using every month that had sales within the last 18 calendar months. Months with no sales are skipped, and calendar gaps are squeezed out rather than filled with zeros.
More important than the count is whether those months are comparable to each other and to the future you are planning. A number the arithmetic can produce is not automatically a trend you can bank on: a meaningful line needs several comparable months behind it, and more history helps separate a persistent direction from a couple of noisy ones.
Treat short histories with real suspicion:

A handful of months can produce a steep line out of ordinary launch variation.
Less than a year of history has never seen a full seasonal cycle, so you are trusting a pattern that has not been tested against your own data.
One unusual month can move both where the line starts and how steeply it climbs, and it keeps doing so until it ages out of the window.

Before you rely on the direction, look through the months in the window for anything that makes them not comparable: stockouts or periods when the listing was unavailable, one-time campaigns, a relaunch, a listing or channel migration, a change in unit of measure, bundled and unbundled sales mixed together, partial months at either end, and bulk or wholesale orders that are not ordinary ecommerce demand. Any of these can be the real reason behind a line that looks convincing.
And remember what the calculation actually knows: it found a numerical relationship between units and time. It does not know why units changed, and a line that fits your history beautifully is no evidence that the cause will still be there next year.

## Things that surprise people

The trend is units, not a percentage. Adding 50 units a month means the forecast for month 24 is 1,200 units above today. It is not 50% higher, and each month's growth does not build on the last.
A step is a month with sales, not a calendar month. This is the most surprising behavior in the scenario. A product that sold in six of the last twelve months has six steps of history, not twelve, and each future calendar month still advances one step. The line is therefore steeper against the calendar than the same data would suggest if the empty months were counted as zeros. They are not counted at all.
One unusual month can carry the whole line. A best-fit line has no defense against outliers. A promotional spike or a stockout trough is not capped, trimmed or removed — it sits in the data and pulls the line, and it keeps pulling until that month falls out of the 18-month window.
The line never notices that the world changed. A price increase, a competitor entering, losing a ranking position, cutting ad spend, running out of new markets, the product maturing, a newer version cannibalizing it — every one of these changes the direction of real sales, and none of them changes the direction of the line.
A drop from December to January is not a reversal. Two adjacent forecast months can move against the trend purely because of their seasonality numbers. Read the underlying direction before concluding the product turned.
It does not soften a decline or cap a rise. Some scenarios treat growth and decline differently on purpose. This one applies whatever direction it found at full strength, both ways, for the full 24 months.
It cannot see anything you have planned. Promotions, advertising budgets, price changes, inbound stock, on-hand inventory and production capacity are all invisible to it. If you know something the history does not, that belongs in a saved value or a different scenario.
The forecast can go negative, in one specific way. The underlying line is held at zero or above, so the projection itself is never negative. The final number is that projection multiplied by the month's seasonality number, and a negative seasonality number produces a negative result. Keep every month in your pattern above zero.

## How it compares to the nearby scenarios

Against Seasonal Naive. Seasonal Naive takes your latest month that had sales, strips the seasonality out of it, and carries that one level forward unchanged — flat, where Trend and Seasonality moves every month. It avoids the risk of extending a line too far, but it can be dominated by a single odd latest month. Choose Trend and Seasonality when the direction is real and you expect it to hold; choose Seasonal Naive when where you are now matters more than where you have been heading.
Against 12-Month Moving Average. Moving Average takes one flat average of up to twelve months and shapes it with seasonality, deliberately assuming nothing about direction. In the worked example, the latest twelve underlying values run from 900 to 1,450 and average 1,175. That still lags a product ending at 1,450 and continuing upward, but by less than a full-history average would. The line can capture the rise only if you have reason to believe it continues.
Against Robust Low. Robust Low blends a recent average with the latest month and has no trend at all, so its underlying level stays flat until the next refresh and it can absorb a spike without extending anything. Trend and Seasonality may sit above or below it depending on which direction the line found and how far out you look. Neither is guaranteed to be the lower plan — despite its name, Robust Low is not a floor. Choose on which assumption fits the product.

## Common questions

Is the trend a percentage growth rate?
No. A trend of 50 adds 50 units to the underlying level every future month. It does not grow by 50%, and each month's gain does not build on the last.
How much history does it use?
Every month that had sales within the last 18 calendar months. Months with no sales are not inserted as zeros and do not widen the gap between steps.
Does it weight recent months more heavily?
No. Every month in the window counts the same. If you want recent months to dominate, look at the recency-weighted scenarios.
Can the forecast go below zero?
The underlying line reaches zero and stops there, so the projection never goes negative. The final number can still come out negative if that month's seasonality number is negative.
Does it spot the point where growth flattens out?
No. It extends one straight line for the full 24 months. A launch curve levelling off, a market saturating or a sudden decline all need you to notice and act.
What happens if my seasonality is neutral?
The line is drawn straight through your recorded monthly units and projected forward with no seasonal shape. Nothing seasonal is learned automatically from history.
Can December be higher and January lower even though the trend is up?
Yes, and that is normal. The underlying level rises while the final numbers fall, because the two months carry different seasonality numbers.
Does it ignore promotional spikes?
No. A promotion month stays in the data and pulls the line like any other month. The only way to remove it is to correct the sales history itself through the proper process.
Is more history always better?
No. More months usually steady the direction, but months belonging to an older version of the product or channel make it misleading. Length is not relevance.
Is this always a more aggressive forecast than Moving Average?
No. A downward line produces lower numbers than a flat average, and even an upward line can start below the average depending on where it begins.
Does a value I save change the trend next month?
No. Saved values are operational decisions, not sales history, and never become inputs to the line. In inventory planning the saved value applies to the month you saved, and adjacent months stay automatic.
Can I use this to plan a discontinuation?
Only loosely. It can extend a decline and eventually reach zero, but it has no idea when you intend to stop selling. A real phase-out should be planned explicitly.
