# Recency-Weighted Daily Rate: Follow Your Recent Sales Per Day

Category: Forecast Scenarios · Article 9017 · Updated 2026-08-24

Follows your recent sales per day, with unusually high months trimmed back.

## Introduction

Recency-Weighted Daily Rate works out how many units a day your product is currently selling, leaning heavily on your most recent months, trims unusually strong months back so one of them cannot run away with the forecast, and adds a gentle 5% a year of growth. It suits a product that sells steadily month after month, where recent demand should matter more than last year's, and where the occasional promotional spike should not set the plan for two years.
One thing to be clear about up front: "daily rate" describes how the calculation thinks, not what it reads. It never looks at individual order dates, weekdays, or weekends. It takes each completed month's total, divides it by the days in that month, and works with one average rate per month.

## The short version

For each of your recent months, NeonPanel removes the seasonal effect you configured, divides by the number of days in the month to get a sales-per-day figure, and trims any unusually high figure back. It then averages those figures with recent months counting far more than older ones, grows the result slightly, and multiplies by the number of days in each future month before putting seasonality back.
The practical effects are worth knowing before you assign it:

Recent months dominate. A month roughly 45 days older counts about half as much as a fresh one, and one 90 days older counts about a quarter.
Unusually high months get trimmed. NeonPanel finds the sales-per-day rate that 97% of this product's months fall at or below, and no month may count for more than that.
The trimming works in one direction only. There is no matching protection for unusually low months, so a stockout month pulls your forecast down at full strength.
Default growth is modest: 5% a year, or about 0.4074% a month.
Month length changes the numbers. A rate of 10 units a day gives 310 units in a 31-day month, 300 in a 30-day month and 280 in February.
Months with no sales at all are skipped rather than counted as zero, so an intermittent product looks stronger here than it is.
Nothing about stock, inbound shipments, ads, promotions, or future prices enters the calculation.
This is not NeonPanel's system fallback. When NeonPanel cannot resolve a valid supported scenario from the product or company default, it uses Recency-Weighted Growth.

## How it works

The whole calculation is about units. Revenue is worked out at the end by multiplying units by a price.
It reads completed months, for one product in one marketplace. The monthly forecast run loads up to 18 completed months of history, and this scenario uses roughly the last 365 days of it. History stays separate by company, product, SKU, marketplace, sales channel, country and month, so you never see a rate blended across unrelated marketplaces or products.
It removes the seasonal effect first. Each month's units are divided by the seasonality number you set for that month. A July that sold 1,550 units with a July number of 1.25 gives an underlying 1,550 ÷ 1.25 = 1,240 units. That way a configured peak is not mistaken for a permanent rise in demand.
It turns each month into a sales-per-day figure. Divide the underlying units by the days in that month: 310 units in a 31-day month, 300 in a 30-day month and 280 in a 28-day February all come to 10 units a day. This stops a long month looking stronger than a short one. It also means everything inside the month is lost — a month with a three-day flash sale and a quiet month with the same total are identical from here on.
It trims the unusually high months. NeonPanel works out the sales-per-day rate that about 97% of this product's months in the lookback fall at or below, and no month is allowed to count for more than that. If the trim line works out at 18 units a day, a month at 14 stays 14, a month at 18 stays 18, and a month at 25 counts as 18. The trimmed month is still in the calculation — only the rate it contributes is limited. Nothing here knows why a month was high, and nothing lifts an unusually low month. Because NeonPanel works out that 97% level approximately and across the product's whole lookback, redoing the sum by hand on a few months will not always land on the same trim line.
It weights recent months far above older ones. Each month gets an age in days, measured from the calculation's recent reference point, and how much it counts falls away smoothly with that age:

| How old the month is | How much it counts |
|---|---|
| Current | Full weight |
| About 15 days | Roughly four-fifths |
| About 45 days | Half |
| About 90 days | A quarter |
| About 135 days | An eighth |
| About 180 days | A sixteenth |

Nothing ever drops to exactly zero; old months simply matter less and less. The age is measured in real calendar days, so a gap in your history makes the months beyond it count less rather than inserting a zero. And the age is only about recency — it does not turn a monthly total into daily sales data.
It averages the trimmed rates using those weights. Multiply each month's trimmed rate by how much it counts, add those up, and divide by the total weight. The result is one current sales-per-day figure carrying both protections: a recent month outweighs an old one, and no month contributes above the trim line.
It adds a small amount of growth. The default floor is 5% a year, a monthly step of about 0.4074% — not 5% ÷ 12, because growth builds on itself and each month grows off a slightly bigger number. Twelve of those steps land exactly 5% higher, so the first future month is about 0.4074% above your current rate and the twelfth is 5% above it, before seasonality.
It applies your uplift setting, scaled by plan confidence. Whatever you put in the manager monthly uplift setting is scaled by plan confidence before it joins the monthly growth, and it stacks on top of the 5%. The standard values are an uplift of 0% and confidence of 80%, and 80% of nothing is still nothing, so by default the floor supplies all the forward growth. Any uplift you set is a planning decision of your own rather than something learned from your sales. Confirm both settings with whoever configured them before trying to reproduce a nonzero-uplift forecast in a spreadsheet.
It converts each future rate back into monthly units. Multiply the projected rate by the number of days in that calendar month. A rate of 10 units a day is 310 units in a 31-day month, 300 in a 30-day month, and 280 in February. That variation is neither seasonality nor a trend — it is just how many selling days the month contains.
It puts seasonality back, then works out revenue. Each future month's units are multiplied by the seasonality number for that calendar month. Put a number greater than zero in every one of the twelve months: a zero erases that month from the forecast, and a negative number can produce a negative forecast. Revenue is then units times your latest available unit price, or your historical average price when no latest price exists. The price only converts units into money.

## A worked example

Take a personal-care product — call it the Daily Cleanser — and a forecast being calculated on 1 August. NeonPanel dates each monthly total at the first of its month, so the four months in the lookback are 122, 92, 61 and 31 days old.

| Month | Sales-per-day figure | Days old | How much it counts |
|---|---|---|---|
| 1 April | 10 units/day | 122 | 0.1527 |
| 1 May | 12 units/day | 92 | 0.2424 |
| 1 June (promotion) | 24 units/day | 61 | 0.3908 |
| 1 July | 14 units/day | 31 | 0.6203 |

The Cleanser's trim line, calculated across its full lookback, is 18 units a day. June's promotional 24 is therefore trimmed to 18. Multiplying each trimmed rate by how much it counts gives:

| Month | Rate used | How much it counts | Contribution |
|---|---|---|---|
| 1 April | 10.0 | 0.1527 | 1.527 |
| 1 May | 12.0 | 0.2424 | 2.909 |
| 1 June | 18.0 | 0.3908 | 7.034 |
| 1 July | 14.0 | 0.6203 | 8.685 |

Add the contributions (20.155), divide by the total weight (1.4062), and the Cleanser's current rate is about 14.3324 units a day. July's 14 units a day carried more influence than any other month, and June's promotion still counted — but at 18 rather than 24.
Now say the next three months have 31 days with a seasonality number of 1.0, then 30 days at 1.1, then 31 days at 0.9. Growing the rate by about 0.4074% a month:

| Future month | Rate | Days | Units before seasonality | Seasonality number | Forecast units |
|---|---|---|---|---|---|
| Month 1 | 14.3908 | 31 | 446.1 | 1.0 | 446.1 |
| Month 2 | 14.4494 | 30 | 433.5 | 1.1 | 476.8 |
| Month 3 | 14.5083 | 31 | 449.8 | 0.9 | 404.8 |

Three things there catch people out. Month 2 is the shortest of the three but ends up highest, because its 1.1 seasonality number more than makes up for the missing day. Month 3 has the strongest underlying rate and still comes out lowest, because of its 0.9 number. And the underlying rate rises every month even though the forecast units do not.
Twelve months out, the Cleanser's rate is 14.3324 × 1.05 = 15.0490 units a day. If that month has 31 days and a seasonality number of 1.2, the forecast is 15.0490 × 31 × 1.2, or about 559.8 units.
These figures are rounded and built from four months. Your real numbers will differ a little, because NeonPanel uses the product's full history, the exact age of every month, its own approximate calculation of the 97% level, and your configured settings.

## When to choose it

This is a strong choice for a product that sells continuously and whose run rate drifts rather than jumps. It fits when you have several recent completed months, when recent demand deserves more weight than older demand, and when you want the occasional strong month damped rather than taken at face value.
It also fits when you would rather have gentle growth than a steep target curve, when you want a level estimate rather than a line extended into the future, and when adjusting for month length helps your planning. Choose it when the latest month matters but should not be the only thing that matters — which describes a lot of repeat-purchase ecommerce: consumables, refills, and replenishable household and personal-care products with a known seasonal shape configured separately.

## When to avoid it

Avoid it when demand is intermittent. Because months with no sales are skipped rather than counted as zero, a product that sells in three months out of twelve gets a rate built only from the months it sold — optimistic in a way that is easy to miss.
Avoid it when the timing inside the month is the whole story: weekday-only demand, or sales concentrated in a flash-sale window, cannot survive a monthly average. Avoid it too when a stockout caused low sales and you need the forecast to reflect the demand you lost rather than the sales you made.
Think twice when your high months are real step changes rather than blips. A permanent distribution expansion, a big ranking improvement, or a successful relaunch is trimmed back like any other high month, and it takes several months of new history before the trim line catches up. A saved forecast value, or a different scenario, usually represents that transition better in the meantime.
It is also wrong when you want recent and long-term direction explicitly combined, when the plan should follow a deliberate 10%, 20%, or 30% target curve, when a decline should be projected as a straight line, when a new product has too little history for the trim line to mean anything, when the 5% annual floor is not appropriate, or when nobody can say what the uplift and confidence settings are.

## What it does to your buying plan

The trim line can make you buy too little. It exists for exceptional spikes, but it cannot tell a spike from a genuine new level. If your product has really stepped up, the forecast lags reality for a few months and your purchase orders lag with it.
Recent strength can make you buy too much. With recent months counting so heavily, a promotion still lifts the rate even after being trimmed — especially if two or three recent months in a row were elevated. A forecast built mostly from a promotional quarter carries that optimism forward for two years.
Stockouts bias you downward. Low sales caused by having nothing to sell are read as a low rate. There is no in-stock correction and no estimate of what demand would have been.
The protection is one-sided. The trim applies to high months only, so bad months land in the average at full weight while good ones may not.
The output looks more precise than the input. A forecast quoted in units per day suggests daily insight you do not have; the underlying resolution is monthly.
A short history weakens the trim line. The 97% level means little when calculated from three or four months. Do not treat it as a firm guard for a new product.
A nonzero uplift multiplies out. An uplift setting is a planning assumption, and combined with the 5% floor and two years of growth, a modest-looking monthly uplift adds a lot of units to what you commit to.
Practical safeguards: compare each month's original and trimmed rate; mark stockout and promotion months outside the model; decide deliberately whether a strong recent month is exceptional or a real shift; review the forecast rates as well as the monthly units; keep month-length effects separate from seasonality; check the uplift and confidence settings; compare second-year quantities against your supply and cash; and save month-specific values where your knowledge beats the automatic result.
Every refresh can move the whole plan
The automatic forecast currently runs on the 10th of each month at 08:00 UTC. When a newly completed month arrives, its sales-per-day figure is calculated, the 365-day window rolls forward, and the trim line can move. Every month's age changes, which changes how much it counts, which changes your current rate. The future sequence of month lengths and seasonality numbers shifts along too.
So the whole forecast can be recalculated, not only the month that was added. NeonPanel writes 24 future months and the planning view may show a shorter 12-month series drawn from them.
When you adjust and save a month
A value you model and save sits on top of the automatic result rather than replacing the scenario. In inventory planning it takes over for that product and month, and the months around it keep their automatic values unless they have saved values too. If more than one saved value exists for the same month, the most recent wins, and the automatic result stays stored underneath.
At the next monthly refresh, NeonPanel recalculates the automatic rate underneath, but it does not recalculate or overwrite the value you saved. If the automatic forecast says 450 units for September and you save 520, September keeps using 520 after the refresh. October remains automatic unless it has a saved value of its own.
Saved values never become inputs. Your saved September number does not become September's sales rate or affect the next automatic calculation. You can compare 520 with the refreshed baseline, but the refresh itself will not change the value you approved.

## How seasonality changes the result

Seasonality wraps around the rate calculation at both ends. Historical units are divided by their own month's number, then converted to a rate; the rates are trimmed and weighted; the projected rate is multiplied by the days in each future month; and each future month is multiplied by its own seasonality number.
An example shows the symmetry. A November that sold 900 units with a November number of 1.5, across 30 days, gives 900 ÷ 1.5 ÷ 30 = 20 underlying units a day. If a future November's projected rate is 22 units a day, the forecast is 22 × 30 × 1.5 = 990 units. The historical peak was not mistaken for a permanent rate, and the future peak was applied once.
When your pattern is neutral or missing. If the pattern is absent, malformed, or all ones, NeonPanel uses 1 for every month. Nothing is removed from history and nothing is added to the future — but your monthly totals can still rise and fall, because February has fewer selling days than January.
The one thing you must get right. Every one of the twelve months needs a number greater than zero. A zero erases that month from the forecast rather than leaving it alone, and a negative number can produce a negative forecast. Neither is caught for you, so check the pattern when you configure it.

## What history it needs

The scenario looks back about 365 days. More months with sales make both the trim line and the current rate steadier — though because recent months count so much more, the newest one or two still do most of the work. The wider forecast run loads up to 18 months, but months older than the 365-day window do not affect this rate.
What it needs is modest: completed monthly unit totals, the number of days in each of those months, a correct product and marketplace mapping, seasonality numbers, and price history for the revenue figures. It does not need a row per order. A 310-unit month is 10 units a day whether those sales trickled in daily or arrived in a weekend.
It can produce a rate from very little history, but a short series weakens both protections at once: the 97% level barely means anything with a handful of months, and the weighted rate can be dominated by one or two of them. For a new product, review the forecast closely until several representative months exist.
Before you assign this scenario, check that month totals are complete and exclude a partial current month; that the day counts match the real months; that known stockouts have not created misleadingly low rates; that a one-time wholesale order or promotion is not dominating the lookback; that returns and cancellations are handled consistently; that SKU and marketplace mappings did not change mid-history; that your seasonality pattern is in January-to-December order; and that the uplift and confidence settings reflect current policy.
It never reads raw daily order timing, weekday or weekend patterns, in-stock percentage, stock on hand or inbound inventory, supplier lead times, planned ad spend, promotion calendars, future price changes, traffic or conversion forecasts, or competitor activity. The trim line limits high monthly rates and nothing more — it is not a promotion detector.

## Things that surprise people

"Daily rate" does not mean daily data. The input is one average rate per completed month, and nothing about the shape of demand within a month reaches the forecast.
Months with no sales vanish instead of counting as zero. They are skipped, not zeroed, which quietly flatters an intermittent product. What a gap does do is push older months further into the past, so they count for less.
A trimmed month is not a discarded month. It still contributes at the trim line, with its own recency weight, so a trimmed promotional month can still pull the forecast up.
Two months can differ with an unchanged rate. Different day counts and different seasonality numbers are enough on their own.
The strongest underlying month can produce the lowest forecast. A high rate multiplied by a 0.9 seasonality number and a 30-day month can land below a weaker month elsewhere in the year.
5% a year is not 0.42% a month. It is about 0.4074%, because each month grows off a slightly bigger number than the last.
Plan confidence does nothing on its own. It only scales the uplift, so with the standard 0% uplift, changing it changes nothing.

## How it compares to the nearby scenarios

Availability-Based. It takes the single most recent month that had sales and grows it 30% a year by default, with no trimming of high months and no averaging across recent history. Choose Daily Rate when several recent months should set the level and extreme months should be damped; choose Availability-Based when you deliberately want one recent month plus an aggressive growth plan.
Recency-Weighted Conservative. It spreads each month's sales evenly across its days, keeps roughly the most recent 120 of those days, and follows the direction it finds in them, weighting recent days more heavily. Calendar gaps get compressed together, and despite its name nothing in it softens a decline. Daily Rate works out no direction from your history; its movement comes from the weighted level plus your growth settings.
Recency-Weighted Growth. It builds a level from up to six recent months that had sales and works out a direction from windows of 2, 6 and 12 such months, weighted 50%, 30% and 20%. It leans toward growth: falling signals are ignored whenever any flat or rising signal exists, and an all-falling picture is halved. Pick Growth when direction matters most; pick Daily Rate when smoothing, trimming, and month length matter more.
12-Month Moving Average. It gives every month in its window equal say and produces a level underlying baseline. Daily Rate leans hard on recent months, trims high ones, and adjusts for days per month.
Trend and Seasonality. It fits one straight line across all the loaded history, up to 18 months, and extends it. Daily Rate extends no line and concentrates on recent months.

## Common questions

Does it use raw daily order history, and why is it called a daily rate? It does not: each completed month becomes one average daily rate. The name describes the level it works with, which is measured in units per day and multiplied by calendar days to build future months.
What does the recency weighting actually do? A month about 45 days older than another counts about half as much. At 90 days it counts about a quarter.
What exactly gets trimmed? Each month's average daily rate is held at or below the rate that about 97% of that product's months in the lookback fall at or below.
Does trimming remove the promotional month? No. The month stays in the calculation; only the rate it contributes is limited.
Are unusually low months protected too? No. There is no matching rule for the low side.
Why do two months differ when the rate barely moved? Different numbers of days, or different seasonality numbers, or both.
What is the default forward growth? 5% a year, about 0.4074% a month.
What does plan confidence do? It scales the manager monthly uplift. With the standard 0% uplift, it changes nothing.
Does it use inventory availability? No. And a stockout lowers your observed sales, which lowers the estimated rate.
Does it know a high month was a promotion? No. Trimming may damp a high month, but nothing knows why it was high.
How is revenue worked out? Forecast units times your latest available unit price, or your historical average price when no latest price exists.
Can both month length and seasonality change one month? Yes. Month length sets the units, then seasonality scales them.
Does a saved value affect next month's rate? In inventory planning, saved values override the matching month only and never feed the next calculation.
Is this the fallback scenario? No. Recency-Weighted Growth is what NeonPanel uses when nothing valid is set.
