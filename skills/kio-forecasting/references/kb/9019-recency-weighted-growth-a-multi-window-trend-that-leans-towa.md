# Recency-Weighted Growth: A Multi-Window Trend That Leans Toward Growth

Category: Forecast Scenarios · Article 9019 · Updated 2026-08-24

Combines your recent level with several trend windows and leans toward growth.

## Introduction

Recency-Weighted Growth works out how many units a day your product is selling now, measures which way sales have been moving over three different stretches of history, and projects that direction forward. It is the scenario NeonPanel uses when it cannot resolve a valid supported scenario from the product or your company default, so you may be running it without having chosen it.
It is tilted toward growth on purpose. If any measured direction is flat or rising, every falling direction counts as flat instead. When all measured directions point down, the combined decline is halved before it reaches your forecast.

## The short version

The forecast is built from a current level and a direction. The level is a weighted average of the daily sales rate across up to your last six months that had sales, with recent months counting slightly more. The direction is measured three times — over your last 2 months that had sales, your last 6, and your last 12 — and combined at 50%, 30%, and 20%, with the shortest and most recent carrying half the weight.
Then the tilt applies. If at least one direction is flat or rising, any falling direction is treated as flat, and the weight of that window is not handed to the others, so a single rising 2-month signal contributes its 50% and no more. If all three are falling, they are combined normally and the whole decline is cut in half. This is not a neutral trend estimator and should not be read as one.
Two more things shape the result. Months where the product sold nothing are skipped rather than counted as zero, so a quiet month never drags the level or the direction down. And the projection steps forward in calendar months from your most recent month that had sales, so if that month is a while back, the first forecast month already sits several steps along the line.

## How it works

How a product ends up on this scenario
NeonPanel picks a scenario for each product in a fixed order. If a scenario is set on the product, it uses that one; if nothing is set, it uses your company default. Either way, the chosen scenario then has to be one of the twelve NeonPanel supports.
If it is not, the forecast falls back to Recency-Weighted Growth. One trap is worth knowing: if the setting on a product holds something invalid rather than being blank, NeonPanel does not go back and try your company default — it goes straight to Recency-Weighted Growth. The company default is only ever consulted when the product's own setting is blank. There is no product-family fallback anywhere in this chain, and the fallback is not Growth 10.
It uses months that had sales, in order
NeonPanel loads up to 18 months of your sales history for the product, kept separate by marketplace, sales channel, and country, already totalled by month. Any month where the product sold nothing is not in that history — it is skipped, not stored as a zero.
Everything that follows counts positions in that list, not calendar months. "Your last 12 months" means your last 12 months that had sales, which might span 15 calendar months if three were empty. Calendar gaps get squeezed out.
It takes your seasonality out and works in units a day
If you have set a custom seasonality pattern, each historical month is divided by its own seasonality number first. A December that sold 1,800 units against a December setting of 1.5 is treated as a 1,200-unit month, so a seasonal peak is never mistaken for permanent growth.
Each month is then divided by its number of days, which puts long and short months on the same footing:

| Sales that month, after seasonality is removed | Days in month | Daily rate |
|---|---|---|
| 310 units | 31 | 10 units a day |
| 300 units | 30 | 10 units a day |
| 280 units | 28 | 10 units a day |

These are monthly averages, not real daily sales. Nothing about when inside the month those units sold survives this step.

It sets a current level from up to six months
The starting level is a weighted average of the daily rates from your last six months that had sales. Each step further back counts 97% as much as the step in front of it:

| Positions back | How much it counts |
|---|---|
| Most recent month with sales | 1.00 |
| 1 back | 0.97 |
| 2 back | 0.94 |
| 3 back | 0.91 |
| 4 back | 0.89 |
| 5 back | 0.86 |

That is a gentle tilt: the oldest of the six still counts about 86% as much as the newest, so all six genuinely contribute — unlike Recency-Weighted Conservative, where the newest weeks swamp everything else. The result is neither your latest month nor a plain six-month average; it sits between them, nearer the recent end. With fewer than six months that had sales, the level uses what exists, and the fewer there are the more each one moves the answer.
It measures three directions
NeonPanel draws a straight line through the daily rates in three stretches of history — your last 2 months that had sales, your last 6, and your last 12 — and reads off how fast each climbs or falls.
Each answer is a change in units per day for every further month, so +0.6 means selling 0.6 more units a day each month than the month before. A window is only measured if you have enough months to fill it. When a window is unavailable, NeonPanel redistributes its planned share across the windows that do exist: with only the 2- and 6-month windows, their usual 50-to-30 balance becomes 62.5% and 37.5%.
That is different from a window that exists but points down while another is flat or rising. The falling direction becomes zero, but its share is not redistributed; it stays in the blend and contributes nothing.
The growth tilt decides which directions count
This is the part that most affects your numbers, and it works one of two ways.
When at least one direction is flat or rising, every falling direction is set to zero and drops out. The rest are combined at their original weights, and the weights of the zeroed windows are not shared out. So if your 2-month direction rises while your 6- and 12-month directions fall, the forecast is built from 50% of the rising signal and nothing else — not 100% of it. It climbs, but more gently than the recent signal alone suggests.
Note that a flat direction counts as "not falling", so a dead-level 12-month window switches off two falling windows even when nothing is actually rising. The older evidence is discarded, not balanced against the newer signal.
When every available direction is falling, they are combined at their normal weights and the result is multiplied by 0.5. A combined decline of 0.40 units a day per month becomes 0.20. The forecast still declines — at half the speed your history shows.
When all three are rising, none of this applies; they combine at 50%, 30%, and 20%.
It steps forward from your last month with sales
For each forecast month, NeonPanel counts the whole calendar months between your most recent month that had sales and that forecast month, then adds that many steps of the combined direction to the starting level.
Normally the most recent month with sales is the month just gone, so the first forecast month is one step along. But if recent months had no sales and were skipped, the first forecast month can be two or three steps along — which, on a rising direction, means the forecast opens higher than you would expect from a product that has just been quiet. The rate is never allowed to fall below zero.
It turns the rate back into months and puts seasonality back
Each future month's units are the projected daily rate multiplied by the days in that month, so 31-day months come out larger than February at the same rate. Each month is then multiplied by its own seasonality number.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast, and a negative number can produce a negative forecast even though the underlying rate is never negative.
Revenue comes last
Forecast revenue is your forecast units multiplied by the most recent price NeonPanel has for the product. Price plays no part in working out the units, and the scenario does not model how customers respond to a price change.

## A worked example

Take a replenishable accessory with twelve months that had sales. After seasonality is removed and each month is converted to a daily rate, the history runs, oldest to newest:
7.0, 7.5, 8.0, 8.5, 9.0, 9.5, 10.0, 10.5, 11.0, 11.5, 12.0, 13.0 units a day.
The level. The last six — 10.0 through 13.0 — averaged with the 0.97-per-step weights give about 11.4 units a day. A plain average would be 11.3 and the latest month alone is 13.0, so the weighting lifts the level slightly without letting the newest month take over.
The three directions. Drawing a line through each window gives:

| Window | How fast the daily rate is moving |
|---|---|
| Last 2 months that had sales | +1.00 units a day, each month |
| Last 6 months that had sales | +0.57 units a day, each month |
| Last 12 months that had sales | +0.52 units a day, each month |

All three are rising, so nothing is zeroed and nothing is halved. Combined at 50%, 30% and 20%, they come to about +0.78 units a day, each month. Notice how much the 2-month window pulls the answer up: both longer windows say roughly half a unit, yet the combined figure is nearly 0.8, because the shortest carries half the weight.

The first three months. Suppose they have 31 days at seasonality 1.0, 30 days at 1.1, and 31 days at 0.9:

| Forecast month | Daily rate | Days | Before seasonality | Seasonality number | Forecast units |
|---|---|---|---|---|---|
| Month 1 | 12.2 | 31 | about 377 | 1.0 | about 377 |
| Month 2 | 12.9 | 30 | about 388 | 1.1 | about 427 |
| Month 3 | 13.7 | 31 | about 425 | 0.9 | about 383 |

Month 3 has the strongest underlying demand and still forecasts fewer units than Month 2, because its 0.9 seasonality number pulls it back. Worth remembering when someone asks why a growing forecast dipped.
A year out. If that direction holds, Month 12's rate is about 20.7 units a day — nearly double the 11.4 it started from. In a 31-day month with a seasonality number of 1.2, that is about 770 units in a single month. This is exactly why you check the far end of the forecast: a recent 2-month uptick with 50% of the weight can lift your second-year plan enormously.
These figures are rounded for readability. Your live numbers will differ slightly because NeonPanel uses your product's full history, exact month lengths, and unrounded arithmetic.
When the directions disagree
Now suppose the same product's short-term direction is rising while its 6-month and 12-month directions both fall.
Because one direction is rising, both falling ones are set to zero. The forecast is built from the rising short-term signal at its original 50% weight; the 30% and 20% belonging to the falling windows are not handed over. With all three windows available, the forecast therefore climbs at exactly half the rate the recent uptick alone implies — but it still climbs, despite six and twelve months of evidence pointing down.
When everything is falling
Suppose all available directions fall and combine to −0.40 units a day per month. The scenario halves that to −0.20 and projects the gentler line.
From a level of 10 units a day, that gives about 9.8 units a day in Month 1, 8.8 in Month 6, and 7.6 in Month 12. The decline is real in the forecast, but it arrives at half the speed your history actually showed.

## When to choose it

Choose Recency-Weighted Growth when you have a reasonable run of history — ideally 6 to 12 months that had sales — and you want the forecast to react quickly to growth without reacting equally quickly to decline. That lopsidedness is a deliberate planning stance, and if it matches how you buy, this scenario expresses it directly.
It fits products that sell continuously enough for a monthly average to mean something, where recent, medium, and longer-term direction all tell you something useful, and where you would rather infer a trend from your own data than commit to a fixed target like Growth 10 or Growth 20. It assumes you maintain seasonality as a separate custom pattern, since the model strips seasonality out before measuring anything and adds it back at the end. And it suits teams that will actually look at the three directions and the 12- and 24-month endpoints before approving a purchase plan.

## When to avoid it

Avoid it whenever you need an even-handed read on where sales are heading. A scenario that discards long-term decline the moment one short window turns flat is not that, and no amount of review changes what it is built to do. Avoid it too if you believe declines should be planned at full speed: halving a genuine end-of-lifecycle drop keeps buying quantities high for products on their way out, and that inventory is hard to clear.
Be careful when your last two months that had sales contain anything unusual — a promotion, a recovery from a stockout, a data correction, a one-off wholesale order. That window carries 50% of the direction, so one soft month followed by a normal one looks like sharp growth and gets projected for two years. The same applies to intermittent demand, where months with no sales are skipped and the few that remain produce unstable directions.
Reconsider it as well when you want a fixed target curve, a flat plan, or a forecast constrained by stock, capacity, or cash — none of which this scenario provides. And if it turned up on a product automatically, do not assume it was chosen because it fits: it is the fallback, which means NeonPanel had something dependable to use, not that this method was tested against your product.

## What it does to your buying plan

The growth tilt shows up as a persistent lean toward buying more. In a mixed picture it plans for growth; in a declining picture it plans for half the decline. Neither is wrong as a business stance, but both mean that when demand really is softening, this scenario is the last of your options to notice.
The upside risk is quantities and cash. The combined direction is applied month after month across a 24-month forecast, so a recent uptick with 50% of the weight can nearly double your daily rate within a year. Buy against the far end without checking it and you tie up cash in stock that arrives after the trend has ended.
The downside risk is subtler. Because the decline is halved, a falling product still shows respectable quantities for a long time: you are less likely to stock out and more likely to be left holding units. And if a stockout caused the low months, the recovery afterwards reads as a rising short-term direction and pushes the plan higher still.
Month length also moves your numbers in ways that are not demand. At an unchanged rate, a 31-day month forecasts about 11% more units than a 28-day February. Read that as a calendar effect, not a trend.
The habit that catches most problems is to compare all three directions rather than looking only at the final number. If they disagree in sign, find out why, and check that the latest two months that had sales are complete and representative. Look at both the projected daily rate and the final monthly units, since seasonality and month length can make them tell different stories. Then check the 12- and 24-month figures against what you can realistically sell, store, and fund, and cross-check the product under Recency-Weighted Daily Rate, Recency-Weighted Conservative, and a stable baseline like the 12-Month Moving Average.
Your plan can change sharply at each refresh
NeonPanel rebuilds the forecast automatically on the 10th of each month at 08:00 UTC. When a newly completed month arrives, the six-month level window moves and every weight in it shifts, and all three direction windows move too. A direction can change sign, a window can become available for the first time, and an all-falling product can flip into the mixed case or back.
That last point is the one to watch. A slightly rising new 2-month direction switches off falling 6- and 12-month directions altogether, turning a declining forecast into a rising one on a single month's data. The change in your plan can be far larger than the change in your sales, so treat each refresh as a fresh forecast to review. NeonPanel calculates 24 months by default, though your planning view may show a shorter series than that.
Saved values stay yours, but only for the months you save
If you model and save a value with Kio for a specific month, it takes priority for the product and month in inventory planning. Months either side stay automatic unless you save them too, and if more than one saved value exists for the same product and month, the most recent wins. The automatic forecast is still stored underneath.
At the next monthly refresh, NeonPanel recalculates the automatic forecast underneath, but it does not recalculate or overwrite the value you saved. Your value remains unchanged and continues to be the one inventory planning uses for that product and month.
Saved values never become history. They do not enter the six-month level or any direction window, and the next forecast is measured from completed actual sales rather than from the forecast you edited. Save a high number for a planned promotion and the month after does not continue from it — it carries on from the automatic line unless you save that month too.
If this scenario appears and you did not choose it
Check whether the product has a scenario set on it at all, whether that setting — or your company default, if the product's is blank — points at a scenario NeonPanel supports, and whether an invalid setting on the product blocked your company default. That last case surprises people, because the company default looks correct in your settings and is never reached.

## How seasonality changes the result

Seasonality wraps around the calculation rather than sitting inside it. Historical months are divided by their seasonality numbers, then by their days, to give the daily rates the model measures. The level and the three directions are worked out on those rates. The projected rate is then multiplied by the days in each future month, and finally by that month's seasonality number.
Here is what that does to a strong December. Suppose December sold 930 units, has 31 days, and you have set December to 1.5:
930 units ÷ 1.5 ÷ 31 days = 20 units a day.
The model measures direction against a 20-unit-a-day December, not the 30 units a day the raw total implies, so your holiday peak never registers as a surge in the underlying trend. When a future December comes around at a projected 22 units a day:
22 × 31 days × 1.5 = 1,023 forecast units.
The peak returns exactly once, at the end, where it belongs.
The rules for your seasonality numbers are strict. There are twelve, January through December, and every one must be greater than zero. A zero wipes that month out of your forecast completely, and a negative number can turn a month's forecast negative even though the underlying rate never goes below zero. Check the order as well as the values — a pattern entered out of sequence applies your December uplift to some other month.
If you have not set a custom pattern, every month is treated as normal: nothing is removed from your history and nothing added to your forecast, though month length and the projected direction still make consecutive months differ.

## What history it needs

Different parts of this scenario use different amounts of history. The level uses up to your last 6 months that had sales, the three directions use your last 2, 6, and 12, and the wider forecast process loads up to 18 calendar months — with months that had no sales dropped from all of it.
Twelve months that had sales let all three directions contribute; with fewer, only the windows that can be filled are measured, and their 50/30/20 shares are rescaled across the available windows. Two months are enough for a short-term direction, but a forecast resting on two months is extremely sensitive — one unusual month is the entire signal. Six give you a full level and a medium-term direction. Twelve give the tilt rule the broadest context it can have. A short history does not turn this scenario into Growth 10 or any other fixed curve; it stays Recency-Weighted Growth, using whatever signals it can calculate.
It needs completed monthly unit totals, the number of days in each of those months, twelve valid seasonality numbers, a stable mapping of the product across marketplace, channel and country, and a recent unit price for the revenue figure. Because it works from monthly averages, it cannot see demand accelerating within a month, weekday patterns, or a promotion concentrated into a few days.
Before you commit, check that the latest two months that had sales are complete and representative — they deserve the most scrutiny, because they carry half the weight of the direction. Then check whether stockouts created false declines, whether promotions or distribution changes created false growth, whether there was a launch, relaunch, or listing suppression in the window, whether your SKU, marketplace, channel or country mappings changed mid-history, whether returns, cancellations and bulk orders are recorded consistently, whether all twelve seasonality numbers are valid and in the right order, and whether a steady change in daily rate is plausible 24 months out.
The scenario reads none of the following: stock on hand, inbound units, or capacity; in-stock percentage or lost-sales estimates; promotion calendars or PPC plans; traffic, conversion rate, or market-share forecasts; planned price changes; supplier lead times; cash constraints; or competitor activity. It infers direction from units sold and cannot know why they changed.

## Things that surprise people

It is the fallback, so you may not have chosen it. If it shows up on a product, check the settings before assuming someone picked it.
An invalid setting does not fall back to your company default. A blank setting does. Something NeonPanel cannot recognize goes straight to this scenario, and your company default is never consulted.
A flat window discards a genuine long-term decline. The rule is "not falling", not "rising", so a dead-level 12-month window has the same effect as a strongly rising one — and the falling windows contribute zero rather than balancing the newer signal. Their weight is not handed over either, so when only the 2-month direction survives it supplies 50% of the answer, not 100%.
The last two months decide half the direction. One soft month followed by a normal one looks like rapid growth to this scenario.
Months with no sales vanish rather than count as zero. A skipped month never pulls the level or the direction down, and the windows count positions in the list of months that had sales, not calendar months.
A gap pushes the first forecast month further along the line. The projection counts calendar months from your most recent month with sales, so after a quiet stretch it can open two or three steps along the direction rather than one — higher on a rising line and lower on a falling line.
Unusually high months are not trimmed back. That belongs to Recency-Weighted Daily Rate, so one exceptional month can lift both the level and one or more directions.
A stockout looks identical to a decline, and the recovery looks like growth. No inventory or availability information enters this calculation at any point.
The zero floor is arithmetic, not caution. It stops the rate going negative and does nothing about the growth tilt above zero.
A straight line assumes the same change forever. The model does not detect saturation, a lifecycle stage, or a trend break. A rising direction produces very high second-year rates; a falling one eventually reaches zero.

## How it compares to the nearby scenarios

Recency-Weighted Conservative builds about 120 daily figures by spreading each month's sales across its days, fits one recent direction, and follows it at full speed either way — no cap, no softening, no tilt. The names mislead in both directions: Growth carries the decline-softening logic and Conservative carries none.
Recency-Weighted Daily Rate looks back up to 365 days, trims unusually high months back before averaging them, weights history so a month 45 days older counts half as much, and adds a gentle 5% annual growth floor. It fits no direction at all. Choose it for a smoothed and capped recent level; choose Growth for direction inferred from several windows.
Availability-Based starts from your latest month that had sales and compounds a set annual growth rate, 30% by default, whichever way your history has been moving. It measures no direction, and despite its name it does not look at stock.
Trend and Seasonality draws one straight line through all of your loaded history, up to 18 months, and extends it wherever it points — no windows, no weights, no tilt.
Growth 10 follows a fixed percentage curve from your previous completed month and infers nothing from your history's direction. It is also not the fallback, whatever its name suggests.

## Common questions

What happens when a product has no scenario set?
Only when NeonPanel cannot resolve a valid supported scenario. If the product's setting is blank, NeonPanel tries your company default first. If the product holds an unrecognized value, it goes straight to Recency-Weighted Growth without trying the company default.
Is Growth 10 the fallback?
No.
How is the current level worked out?
A weighted average of the daily rates from up to your last six months that had sales, each step back counting 97% as much as the step in front of it.
Which stretches of history are used for direction, and how much does each count?
Your last 2, 6, and 12 months that had sales, weighted 50%, 30%, and 20%. They may not be consecutive calendar months, and a window is only used if you have enough months to fill it.
What if one direction is rising and two are falling?
The falling ones are set to zero and the rising one contributes at its original weight. The zeroed weights are not transferred to it. A flat direction has the same effect as a rising one, because the rule is "not falling".
What if every direction is falling?
They are combined at their normal weights and the result is halved.
Is this an unbiased view of my trend?
No. It favours flat and rising signals and softens an all-falling decline.
Does it trim unusually high months?
No. That belongs to Recency-Weighted Daily Rate.
Does it use my actual daily orders, or know about my stock?
Neither. It converts each monthly total into an average daily rate, and a stockout simply lowers those rates and can distort every direction.
Why did a month go down when the direction is up?
A lower seasonality number, fewer days in the month, or both.
How is revenue worked out?
Forecast units multiplied by the most recent price NeonPanel has. Price never affects the unit forecast.
Does a value I save change the next forecast?
No. Saved values are not history. In inventory planning they take priority for the exact product and month you saved, and the next forecast is still built from your completed actual sales.
If NeonPanel picked this scenario, is it the best one for my product?
Not necessarily. It is what NeonPanel uses when nothing valid is set, not a per-product recommendation.
