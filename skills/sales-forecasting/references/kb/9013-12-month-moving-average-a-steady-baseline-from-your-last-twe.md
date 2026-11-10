# 12-Month Moving Average: A Steady Baseline From Your Last Twelve Selling Months

Category: Forecast Scenarios · Article 9013 · Updated 2026-08-24

Averages up to twelve selling months, then applies your seasonality.

## Introduction

The 12-Month Moving Average works out what a normal month looks like for your product by averaging up to twelve months that had sales, with each month's seasonal effect taken out first. It then reshapes each future month with your seasonality numbers. It suits mature, replenishable products where you want one month of strange sales to matter a little rather than a lot.
The trade-off is right there in the design. It is much calmer than a forecast built on your latest month alone, and it is correspondingly slower to notice when your product has genuinely moved to a new level.

## The short version

It averages up to twelve months that had sales, giving each of them equal weight.
Those twelve are not necessarily the last twelve calendar months. Months with no sales are skipped rather than counted as zero, so the window can reach back further than a year.
It divides each of those months by that month's seasonality number before averaging, then multiplies each future month by that month's seasonality number.
If you have fewer than twelve months with sales, it averages the ones you have and divides by that smaller count. It does not divide by twelve and it does not assume sales for the missing months.
It contains no trend. The underlying level is flat across all 24 months NeonPanel forecasts by default, and every rise and fall you see is your seasonality pattern.
It moves gradually. Each refresh that adds a new month with sales swaps that month in and, in a full window, one month out.
Its strength is stability. Its risk is lag: after a lasting change in demand, the old months keep pulling the average toward where you used to be.
It knows nothing about your future promotions, advertising, pricing, purchase orders, or stock. Model those effects and save the months you want to adjust.

## How it works

NeonPanel reads your completed monthly unit sales for that product, in that marketplace and channel, going back as far as 18 months. Months in which the product sold nothing are not in that history at all, and neither are months that come out at zero or below after returns. Nothing is filled in for them.
From what is left, the scenario takes up to the twelve most recent months and gives every one of them the same weight. This is the point where the name misleads people: "12-month" means twelve months that had sales, not twelve consecutive squares on the calendar. If your product sold nothing last February and nothing last May, your twelve selling months stretch back fourteen calendar months to find twelve of them.
Before averaging, each month is put on a level footing by dividing out its seasonal effect. Take two months that look nothing alike: December sold 2,000 units with a December seasonality number of 2.00, and January sold 800 units with a January number of 0.80. Divide each by its own number — 2,000 ÷ 2.00 = 1,000 and 800 ÷ 0.80 = 1,000 — and both months turn out to describe exactly the same underlying level of 1,000 units. Averaging the raw 2,000 and 800 would have told you the product was in freefall when nothing had changed.
Add up the adjusted numbers, divide by how many months you have, and that is your underlying monthly sales. Then each future month gets its own seasonality number applied: at an underlying 1,000 units, October at 1.10 forecasts 1,100 units, November at 1.50 forecasts 1,500, December at 2.00 forecasts 2,000, and January at 0.80 forecasts 800.
That underlying average stays flat across the whole forecast until the next monthly run recalculates it. Nothing in this scenario draws a line through your months or projects a direction — every difference between one future month and the next comes from your seasonality pattern.
NeonPanel rounds each month's final unit number to a whole unit before working out forecast revenue. Note the order: future months are calculated from the unrounded average and then each is rounded, rather than the average being rounded first. Revenue is those whole units multiplied by a price — the average price across up to your last three months with sales, or your overall historical average price if three are not available. The scenario does not predict how a price change would move demand; price only turns units into money.

## A worked example

Meet the Harbor 20 oz insulated bottle. It lifts through summer, peaks hard at holiday gifting, and its planner has been burned before by a forecast that swung on one strange month. She wants a baseline that is calmer than Seasonal Naive. It is the July refresh. June is the latest month with sales, twelve months with sales are available, the company has a real seasonality pattern configured, and everything below is in bottles.

Here is the pattern:

| Month | Your seasonality number | What that means for the Harbor bottle |
|---|---|---|
| January | 0.80 | Runs 20% below a normal month |
| February | 0.80 | Runs 20% below |
| March | 0.90 | Runs 10% below |
| April | 1.00 | A normal month |
| May | 1.00 | A normal month |
| June | 1.20 | Runs 20% above |
| July | 1.00 | A normal month |
| August | 1.00 | A normal month |
| September | 1.00 | A normal month |
| October | 1.10 | Runs 10% above |
| November | 1.50 | Runs 50% above |
| December | 2.00 | Runs at double a normal month |

Step one: put every month on the same footing. Each month's recorded sales are divided by that month's seasonality number.

| Month with sales | What it actually sold | Its seasonality number | The arithmetic | Underlying sales for that month |
|---|---|---|---|---|
| July | 900 | 1.00 | 900 ÷ 1.00 | 900 |
| August | 920 | 1.00 | 920 ÷ 1.00 | 920 |
| September | 940 | 1.00 | 940 ÷ 1.00 | 940 |
| October | 1,056 | 1.10 | 1,056 ÷ 1.10 | 960 |
| November | 1,470 | 1.50 | 1,470 ÷ 1.50 | 980 |
| December | 2,000 | 2.00 | 2,000 ÷ 2.00 | 1,000 |
| January | 816 | 0.80 | 816 ÷ 0.80 | 1,020 |
| February | 832 | 0.80 | 832 ÷ 0.80 | 1,040 |
| March | 954 | 0.90 | 954 ÷ 0.90 | 1,060 |
| April | 1,000 | 1.00 | 1,000 ÷ 1.00 | 1,000 |
| May | 980 | 1.00 | 980 ÷ 1.00 | 980 |
| June | 1,440 | 1.20 | 1,440 ÷ 1.20 | 1,200 |

Step two: add them up and divide. 900 + 920 + 940 + 960 + 980 + 1,000 + 1,020 + 1,040 + 1,060 + 1,000 + 980 + 1,200 = 12,000 bottles. Divide 12,000 by the twelve months and the underlying level is 1,000 bottles a month.
June is the interesting row. It sold 1,440 bottles, the biggest non-holiday month in the table, and after adjustment it is the strongest underlying month at 1,200. But it counts for one twelfth of the average, no more. Had June come in at an ordinary 1,000 underlying bottles instead of 1,200, the baseline would be 983 rather than 1,000 — June's strength is worth 17 bottles here, not 200.
Step three: give each future month its own shape.

| Forecast month | Underlying monthly sales | Month's seasonality number | The arithmetic | Forecast |
|---|---|---|---|---|
| July | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| August | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| September | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| October | 1,000 | 1.10 | 1,000 × 1.10 | 1,100 bottles |
| November | 1,000 | 1.50 | 1,000 × 1.50 | 1,500 bottles |
| December | 1,000 | 2.00 | 1,000 × 2.00 | 2,000 bottles |
| January | 1,000 | 0.80 | 1,000 × 0.80 | 800 bottles |

The underlying level is 1,000 bottles for every future month. The whole holiday peak comes from the November and December numbers you configured, and there is no growth or decline anywhere in the calculation.

What happens at the next refresh: say July closes at 1,100 bottles. July's seasonality number is 1.00, so its underlying value is also 1,100. It joins the window, and because the window already held twelve months, the oldest one — last July at 900 — drops out.
The arithmetic is a swap: 12,000 − 900 + 1,100 = 12,200, and 12,200 ÷ 12 = 1,016.67 bottles. Every future month is then recalculated from that 1,016.67 and rounded, so a neutral month is stored as 1,017 units and an October at 1.10 becomes 1,118.
That is the whole personality of this scenario in one calculation. A month that arrived 200 bottles above the month it displaced moved the baseline by about 17 bottles. Seasonal Naive, given the same July, would have jumped its baseline straight to 1,100.

## When to choose it

Choose the 12-Month Moving Average when your product is mature, demand is broadly stable, and your individual months are noisier than the business behind them. Order timing, a lumpy wholesale order, a two-week ad test, a short stockout — this scenario lets all of that average out instead of letting the most recent instance set your buying plan.
It fits when the whole stretch of history represents the product you sell today: same pricing, same listing quality, same channels. It is also the natural pick when you want a transparent baseline with no growth assumption inside it, so nobody has to defend a projected trend in a purchasing meeting.
Products where it usually works well:

Established evergreen products with recurring holiday peaks.
Household consumables with steady annual demand and noisy individual months.
Replenishable accessories where you do not expect a strong direction either way.
Products whose latest month is regularly distorted by order timing or shipment cut-offs.
Stable products whose recent selling months reflect current pricing, listing quality, and channel coverage.

Two more conditions matter. Your seasonality pattern needs to be credible, because this scenario leans on it twice. And you should genuinely want every month in the window to count equally — if you believe the last three months matter more than the nine before them, this is not the scenario that expresses that.

## When to avoid it

Avoid it when something lasting has changed. A channel launch, a distribution loss, a price repositioning, a packaging change that doubled your unit velocity — in all of these, older months in the window describe a business that no longer exists, and averaging them in holds your forecast back.
Avoid it too when the product has a real, sustained direction you want carried into the plan, when the product is early in launch and its short history is still moving fast, or when demand is so intermittent that "an average month" is not a meaningful idea.
Look elsewhere as well when:

Your seasonality has shifted and the configured numbers no longer describe the year.
Leadership needs a specific planned growth target in the numbers.
Known future campaigns, pricing changes, or retail events will materially change demand.
Reacting fast to the newest month matters more to you than smoothing.

And one caution that is easy to miss: a smooth forecast is not automatically a safe one. A stable-looking average built from an unrepresentative window is confidently wrong, and it is wrong in a way that looks reassuring on a chart.

## What it does to your buying plan

The practical effect is that your suggested purchase quantities drift rather than jump. In a full window, the change is the difference between the new month and the month that leaves, divided by twelve. In the worked example, 1,100 replaces 900, so the baseline rises by about 17 bottles. Month-to-month revisions are usually smaller than under a one-month scenario, though a large gap between the incoming and outgoing months can still matter.
That predictability is worth real money in planning. You can commit to longer lead times with less fear that the plan underneath will have changed shape by the time you place the next order, and your reorder quantities line up more comfortably with case packs and container fills.
The cost is delay, and it cuts both ways. After a genuine step up, the average sits below your true demand for months, so you under-buy and risk stockouts precisely when the product is winning. After a genuine step down, older strong months keep the average elevated, so you over-buy and carry stock you will be discounting later. The forecast only catches up as each stale month leaves the window.
One more thing to plan around: an unusual period remains until twelve later months that had sales have entered the window. If every calendar month has sales, that is roughly twelve refreshes. If some months have no sales, the unusual month can remain through more than twelve monthly runs, though the source history never reaches beyond the latest 18 calendar months. Before a large commitment, look at what is inside the window rather than only at the number it produces.
What changes each month
NeonPanel recalculates the automatic forecast on the 10th of each month at 08:00 UTC. At each run, a newly completed month with sales can enter the window, the oldest month leaves if twelve were already there, every month in the window is adjusted by its own seasonality number, the average is recalculated, and every future month is rebuilt from it.
So one new month can still change your entire forecast — it is just that it changes it gently, because that month is only one twelfth of the answer.
When you adjust and save months
Values you model and save stay separate from the automatic forecast. In inventory planning, a saved value wins for that product and month; neighbouring months stay automatic unless you save them too, and if a month has several saved values, the most recent one is used. The automatic Moving Average figure remains stored underneath.
At the next monthly refresh, NeonPanel recalculates the automatic Moving Average underneath, but it does not recalculate or overwrite your saved value. The value you approved remains unchanged and continues to be the one inventory planning uses for that product and month. Saved values never enter the averaging window: they are not sales history or training data, and they do not influence the automatic calculation.
Worked through: automatic November comes out at 1,500 units, you have a confirmed holiday campaign and expect 2,100, so you model and save November at 2,100. The operational November plan keeps using 2,100 after the refresh. December stays automatic unless you save December as well, and the next average is still built from recorded sales. If an event spans several months, save every month it touches.

## How seasonality changes the result

Your twelve seasonality numbers do double duty in this scenario, and it is worth understanding both jobs. On the way in, each historical month is divided by its own number so that a big December and a small January can be compared fairly. On the way out, each future month is multiplied by its number to put the seasonal shape back.
That means an error in one number distorts the result twice — once in the baseline, once in the shape. If your December number is overstated, December history looks weaker than it was, which drags the average down, and future Decembers get inflated on top of that lower base. Reviewing the pattern is usually a better use of an hour than switching scenarios.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast entirely, and a negative number can produce a negative forecast. Neither is caught for you, and neither is what you want.
If the pattern is missing, empty, has the wrong number of entries, or contains a blank or non-numeric entry, NeonPanel falls back to neutral behavior — 1.00 for the affected value or for all twelve. With a neutral pattern, dividing by 1.00 changes nothing, so your baseline is simply the ordinary average of your recent selling months, and every future month gets that same average. Nothing is learned from your history to fill the gap: NeonPanel does not detect your seasonal shape on its own.
Changing your pattern never rewrites your recorded actual sales. It changes how history is read into the forecast and how future months are shaped coming out. Worth reviewing when your peak shifts, when you start selling in a country with a different holiday calendar, when your category mix changes, or when a one-off promotion got mistaken for a recurring season.

## What history it needs

The scenario uses up to twelve months that had sales, drawn from up to 18 months of history. It runs happily with fewer, but how much you should trust it changes with how many you have.

| Months with sales in the window | How to read the result |
|---|---|
| 1–3 | Technically an average, but still highly sensitive to each individual month. |
| 4–11 | Smoothing is working, but the window may not cover a full seasonal cycle. |
| 12 | The full window. Note that twelve selling months can span more than a calendar year when there are gaps. |
| More than 12 available | Only the latest twelve count toward the current baseline. |

Those ranges are guidance for how to interpret the number, not extra rules in the calculation. With six months of sales, the maths is simply the six adjusted values added up and divided by six — never divided by twelve, and never topped up with assumed sales for the missing positions. A shorter average is more sensitive to each month in it, which is worth knowing before you buy against it.
Because every month in the window carries equal weight, it pays to know what is in there. Look for stockouts or listing interruptions, one-time promotions, partially recorded or delayed sales, channel launches and closures, bundle changes that redefined what a unit means, discontinued variations, and large wholesale orders sitting alongside ordinary ecommerce demand. Any of those is being treated as a normal month right now.
And as with every scenario here, the units are what you sold, not what customers wanted. If availability limited your sales, the average inherits that limit; nothing reconstructs the demand you could not serve.

## Things that surprise people

Twelve months is not twelve calendar months. Months with no sales are skipped, not stored as zero, so twelve selling months can reach back fourteen or more months. For an intermittent product, part of your "recent" average can be over a year old.
A zero-sales month does not drag the average down. It is not in the history at all, so it neither lowers the average nor counts as a data point. Whether that is what you want depends on why the month was empty.
Every month counts the same. The newest and oldest months inside the twelve-selling-month window carry identical weight. With twelve consecutive selling months, the oldest is eleven calendar months before the newest; with gaps, it can be further back. This scenario has no recency preference at all; if you want one, look at the recency-weighted scenarios.
Lag is the price of stability. If your product permanently doubles its run rate, the average climbs toward the new level over the following months rather than getting there at once, and you may be short of stock in the meantime. In reverse, older high months keep the average up and quietly build overstock.
A promotion can stick around for more than twelve refreshes. A month inflated by a campaign remains one equally weighted member until twelve later months that had sales push it out, or until it falls outside the 18-calendar-month history limit. Months with no sales do not move it along. Your seasonality pattern will not separate promotional sales from ordinary demand for you.
Smoothing does not recover lost sales. Several months held down by stockouts lower the average, and the average has no idea why. A calmer forecast can still be a systematically low one.
Rising demand comes out under-forecast. If your adjusted monthly sales have climbed steadily from 600 to 1,200 units, the average lands somewhere in the middle — well below where you actually are — because a flat average is the whole design.
It knows nothing about your calendar. Promotion schedules, advertising plans, planned price changes, purchase orders, inventory availability, production capacity — none of it is read.
A number like 1,016.67 is arithmetic, not a promise. The precision comes from dividing by twelve, not from confidence. Real plans still need case packs, lead times, service levels, and a margin for being wrong.
A zero in your seasonality wipes out a month. It is the fastest way to see a forecast month of zero units, and it looks like a bug rather than a setting.

## How it compares to the nearby scenarios

Against Seasonal Naive
Moving Average uses up to twelve adjusted selling months; Seasonal Naive uses only the latest one. Moving Average shrugs off a single spike or dip, and Seasonal Naive reacts to a new level immediately. Neither estimates a trend, and in both the month-to-month movement comes from your seasonality numbers.
In the worked example the difference is visible: Moving Average sets the underlying level at 1,000 bottles, while Seasonal Naive would use June's adjusted 1,200. Same seasonality pattern, same history — the 200-bottle gap is smoothing and nothing else. Choose Moving Average when you want the average of your recent past to lead. Choose Seasonal Naive when the latest month is the truest picture of now.
Against Trend and Seasonality
Moving Average produces a flat underlying level. Trend and Seasonality draws a straight line through your months that had sales and extends it forward, so it can express sustained growth or decline that a flat average cannot.
Moving Average is easier to interpret and defend when demand has no durable direction. Trend and Seasonality is the better tool when the direction is real and expected to hold, with the caveat that a straight line extended far enough can overshoot badly if what you fitted was a temporary run rather than a trend.
Against Robust Low
Robust Low is a blend: 70% of the Moving Average forecast plus 30% of the Seasonal Naive forecast. It therefore always sits somewhere between the two, leaning toward the average but conceding some ground to your latest month.
Practically, when your latest month is above the average, Robust Low comes out above Moving Average; when it is below, Robust Low comes out below. Do not read the name as a guarantee — it is a weighted blend of two specific forecasts, not a floor, not a lower bound, and not automatically the lowest number available to you.

## Common questions

Do I need twelve months of sales?
No. The calculation averages the selling months you have and divides by that count. A shorter set is less representative and may not cover a full season. Equally, twelve selling months can span more than twelve calendar months when there are gaps.
Are recent months weighted more heavily?
No. Every month in the window carries equal weight after seasonal adjustment.
Does it forecast a trend?
No. The underlying level is flat. Only your seasonality numbers create differences between future months.
Does it average my raw unit sales?
With a real seasonality pattern, it divides each month by that month's number first and averages the results. With a neutral pattern, dividing by 1.00 changes nothing, so raw and adjusted are the same.
What happens when seasonality is neutral?
You get an ordinary average of your recent selling months, repeated across every future month. NeonPanel does not learn a seasonal curve from your history.
Why did my forecast change this month?
A newly completed month with sales entered the window and, in a full window, the oldest month left. The average and every future month were recalculated.
Can one promotion still distort a twelve-month average?
Yes. Its impact is much smaller than under Seasonal Naive, but it remains one equally weighted month until twelve later months with sales push it out, or until it falls outside the 18-calendar-month history limit.
Does it ignore months where nothing sold?
Yes. Months with zero or negative units are removed from the history, and missing calendar months are not inserted as zeros. Neither contributes a zero to the average, which is why twelve selling months can span more than twelve calendar months.
Will it account for stockouts?
No. It averages units you actually sold. It does not estimate what demand would have been with full availability.
Is it always more cautious than Seasonal Naive?
No. If your latest adjusted month is below the twelve-month average, Moving Average forecasts higher than Seasonal Naive.
Is it the same as Robust Low?
No. Robust Low combines 70% of the Moving Average forecast with 30% of the Seasonal Naive forecast.
Do numbers I save change the next average?
No. Saved values are month-specific operational overrides. They are not sales history and they never feed the next calculation.
