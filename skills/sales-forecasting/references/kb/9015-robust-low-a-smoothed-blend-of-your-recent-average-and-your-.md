# Robust Low: A Smoothed Blend of Your Recent Average and Your Latest Month

Category: Forecast Scenarios · Article 9015 · Updated 2026-08-24

Blends your twelve-month average with your latest month to smooth out swings.

## Introduction

Robust Low adds 70% of what the 12-Month Moving Average scenario would forecast to 30% of what Seasonal Naive would forecast. That gives you a plan tied mostly to your longer-term average while still moving when your latest month moves. Despite the name it is not a floor, a minimum, a worst case or a guarantee of the lowest number — and if your latest month came in below your yearly average, Robust Low is actually higher than Seasonal Naive.

## The short version

The mix is fixed: 70% of the 12-Month Moving Average result plus 30% of the Seasonal Naive result. Those weights never change — not for volatile products, not for far-out months, not for any category, and not according to whether your latest month was strong or weak.
The Moving Average part is the average of up to twelve months that had sales, with your seasonality taken out. The Seasonal Naive part is your latest month that had sales, with its seasonality taken out.
Months whose total sales are zero or negative after returns are skipped entirely, not counted as zero and not filled in. The last twelve selling months can span more than a year, but they always come from the latest 18 calendar months; your latest selling month may therefore not be last month.
The result always lands between the two scenarios it is built from: above the average and below the latest month when the latest month is high, above the latest month and below the average when it is low, and equal to both when they agree.
"Low" is a name, not a promise. It is not a lower bound, a confidence interval, a minimum-demand estimate or a cautious purchasing plan. A 70/30 blend does not mean 70% confidence in anything.
There is no trend in it. The underlying level is flat until the next monthly refresh, and month-to-month movement comes entirely from your seasonality pattern.
Final unit numbers are rounded to whole units before revenue is calculated. The forecast runs 24 months ahead by default.
Every month in your seasonality pattern needs a number greater than zero. A zero erases that month from the forecast.

## How it works

First, NeonPanel works out your recent average. It takes up to the last twelve months that had sales and divides each one by its seasonality number to get the underlying level for that month: a November that sold 1,470 units with a seasonality number of 1.50 has an underlying level of 980. Those figures are then averaged — if twelve of them add up to 12,000, the average is 1,000 units. If you only have seven months with sales, it averages those seven; the arithmetic does not wait for twelve.
Second, it works out your latest level. It takes the most recent month that had sales and divides it by that month's seasonality number the same way. A June that sold 1,440 units with a June number of 1.20 has an underlying level of 1,200.
Third, it mixes them 70/30. Seventy percent of 1,000 is 700. Thirty percent of 1,200 is 360. Added together, that is 1,060 units — the blended underlying level, and the whole calculation.
From there the blended level is shaped by your seasonality pattern, one month at a time. In a December with a seasonality number of 2.00 the forecast is 1,060 times 2.00, which is 2,120 units; in a January with 0.80 it is 848. That number is rounded to whole units, and revenue is those units multiplied by your price — the average unit price across up to the last three months that had sales, or the average across your whole history if fewer than three are available.
You can get to the same answer the other way round, by blending 70% of the Moving Average forecast for a month with 30% of the Seasonal Naive forecast for the same month. Both components use the same seasonality number, so the two routes give identical results — and this is the easier one to follow if you are checking NeonPanel against the component scenarios side by side.
Two things this calculation does not do. It does not put a trend in: the blended level sits flat across all 24 months, and any movement between forecast months came from your seasonality pattern, not momentum. And it does not measure uncertainty — the 70 and the 30 are not derived from your product's volatility, your forecast error, a service level or a confidence calculation.

## A worked example

You sell the Harbor 32 oz insulated bottle. June came in strong — well above the product's recent underlying level — and you are not sure how much of it to believe. You want a plan that gives June some credit without letting one month set your purchasing for two years. Robust Low is built for exactly that judgment call. It is July, twelve months of sales are available, and June sold 1,440 bottles against a June seasonality number of 1.20.

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

The twelve months behind the average

| Completed month | Bottles sold | Seasonality number | Underlying level |
|---|---|---|---|
| July | 900 | 1.00 | 900 |
| August | 920 | 1.00 | 920 |
| September | 940 | 1.00 | 940 |
| October | 1,056 | 1.10 | 960 |
| November | 1,470 | 1.50 | 980 |
| December | 2,000 | 2.00 | 1,000 |
| January | 816 | 0.80 | 1,020 |
| February | 832 | 0.80 | 1,040 |
| March | 954 | 0.90 | 1,060 |
| April | 1,000 | 1.00 | 1,000 |
| May | 980 | 1.00 | 980 |
| June | 1,440 | 1.20 | 1,200 |

The underlying levels in the last column add up to 12,000 bottles, so the average is 1,000. Notice that December sold four times what February sold, yet their underlying levels are within 4% of each other. That is seasonality doing its job.

The latest month and the blend
June is the latest month that had sales, and its underlying level is 1,440 divided by 1.20, which is 1,200 bottles — what Seasonal Naive would carry forward on its own. The blend is 70% of 1,000 plus 30% of 1,200: that is 700 plus 360, or 1,060 bottles. So for the same product in the same month, the three scenarios sit at 1,000 for 12-Month Moving Average, 1,060 for Robust Low and 1,200 for Seasonal Naive — Robust Low has moved 30% of the distance from the average toward June.

What the forecast looks like

| Forecast month | Moving Average | Seasonal Naive | The 70/30 mix | Robust Low |
|---|---|---|---|---|
| July | 1,000 | 1,200 | 0.70 × 1,000 + 0.30 × 1,200 | 1,060 bottles |
| August | 1,000 | 1,200 | 0.70 × 1,000 + 0.30 × 1,200 | 1,060 bottles |
| September | 1,000 | 1,200 | 0.70 × 1,000 + 0.30 × 1,200 | 1,060 bottles |
| October | 1,100 | 1,320 | 0.70 × 1,100 + 0.30 × 1,320 | 1,166 bottles |
| November | 1,500 | 1,800 | 0.70 × 1,500 + 0.30 × 1,800 | 1,590 bottles |
| December | 2,000 | 2,400 | 0.70 × 2,000 + 0.30 × 2,400 | 2,120 bottles |
| January | 800 | 960 | 0.70 × 800 + 0.30 × 960 | 848 bottles |

You can reproduce every row from the 1,060-bottle blended level instead: November is 1,060 times 1.50, or 1,590, and December is 1,060 times 2.00, or 2,120. Same answers.
June's strength lifted the plan above the flat average, but only part of the way, and the blend stayed below Seasonal Naive because June was above the average. Underneath, the level is flat at 1,060 bottles for all 24 months — every difference between forecast months comes from your seasonality pattern. This is a smoothed compromise, not a low plan.
The same arithmetic, running the other way
Now take a product where the recent average is still 1,000 units but the latest month's underlying level is only 600. The blend is 70% of 1,000 plus 30% of 600, which is 700 plus 180, or 880 units.
With a neutral July number, the three scenarios line up as Seasonal Naive 600, Robust Low 880, Moving Average 1,000. Robust Low is 280 units higher than Seasonal Naive — not the lowest of the three, and not close. It moved only 30% of the way from the average toward the weak month, exactly as designed.
This is the case people get wrong, so it is worth remembering the shape of it: when your latest month is weak, Robust Low is the more optimistic of the two.
Why "Low" does not mean "lowest"
The name says nothing about a minimum and nothing about where this scenario ranks against your other options. It has exactly one guaranteed property: it sits between the two scenarios it is built from.

When Seasonal Naive is above Moving Average, Robust Low is above Moving Average and below Seasonal Naive.
When Seasonal Naive is below Moving Average, Robust Low is above Seasonal Naive and below Moving Average.
When the two are equal, Robust Low equals both.

That is the whole guarantee. It has no guaranteed relationship to Trend and Seasonality, to any planned growth scenario, to any recency-weighted scenario, to a value you saved, or to what another product is forecasting. Any of those can come out lower in any given month.
It is also not a statistical lower bound, a confidence interval, a service-level quantile or a worst-case demand estimate, and it is not calculated from uncertainty of any kind. If your safety-stock policy needs a downside number, this scenario does not supply one. If you need the lowest available plan, compare the actual numbers — do not infer it from a name.

## When to choose it

Robust Low fits when your latest month tells you something worth acting on but you are not ready to bet the plan on it, and the product's longer-term level is broadly stable underneath. You get more responsiveness than a pure moving average and more smoothing than a pure latest-month forecast — a genuinely useful middle ground.
Reach for it when:

Your latest month contains real new information, but not enough to fully rebase your plan.
A strong latest month might be partly temporary and you want it to count for something rather than everything.
You have up to twelve recent months that give a credible reference level.
You trust your seasonality pattern, since both components run through it.
You do not need a trend or a planned growth curve — a stable level shaped by seasonality is the right shape for this product.
Everyone reading the number understands that "Low" is a name and not a guarantee.

Typical cases: a mature product whose latest month improved after a listing change, where the lift may not stick; a steady consumable that occasionally catches a large marketplace order; a seasonal item where one strong month should influence holiday buying without dictating it; an established item drifting gradually to a new level.

## When to avoid it

Pick something else when:

You need a guaranteed lower bound or a service-level quantile. This is not one.
You need the mathematically lowest available plan. Compare the numbers instead.
Your latest month deserves full weight because it reflects a confirmed structural change — a permanent price move, a new channel going live, a major listing consolidation.
There is a clear growth or decline you expect to continue. Robust Low has no trend and will lag it.
Your historical average is no longer representative of the product.
History is very short or highly intermittent, so neither component has much to stand on.
Both the latest month and the average are distorted by stockouts. Blending two biased numbers does not remove the bias.
Promotions, prices, distribution or advertising are about to change demand materially. None of that is visible to this calculation.
Management needs a predefined target curve. Use a planned growth scenario.
The product is being launched or discontinued on a specific schedule.

One to be firm about: do not select Robust Low because its name sounds cautious. Select it because the 70/30 mix matches the judgment you want to make.

## What it does to your buying plan

You get a steadier purchase plan than a latest-month forecast, and a slower reaction to genuine change. Both halves cost you something.
Because 70% comes from the average, it lags real change. When a product genuinely steps up to a new level or starts declining, Robust Low travels only 30% of the way there in the first month, then closes the gap gradually as the average moves. On a rising product that means under-ordering and risking stockouts while the average catches up; on a declining product it means carrying stock and cash against demand that has already gone. If you know the change is structural, do not wait for the blend.
The latest month still moves things more than you expect. It counts twice — once as 30% of the Seasonal Naive part, and again as one of the months inside the average. It is far less dominant than in pure Seasonal Naive, but "70% average" does not mean the noise is gone. When you see a big shift between refreshes, look at the latest month rather than assuming the smoothing absorbed it.
Stockouts push your plan down, quietly. Recorded sales are what you sold, not what customers wanted, and this calculation does not estimate lost sales. Repeated stockouts drag the average down, and a stockout in your latest month drags the latest-month component down too. Order to that number and you can lock in the shortage for another cycle. Promotions do the same thing upwards: a promotion in your latest month lifts both components, and promotions further back sit inside the average until they age out of the window, keeping your plan elevated long after the spend stopped.
Cash sits flat rather than ramping. Because the underlying level does not trend, your plan for the next 24 months is essentially "this level, seasonally shaped." Simple to budget against, and predictably wrong for any product that is genuinely growing or shrinking.
Each refresh recalculates both halves when new sales arrive. The forecast is rebuilt on the 10th of each month at 08:00 UTC. A newly completed month enters the average and becomes the latest-month component only if it had positive sales after returns. A month with zero or negative sales is skipped, so the previous latest selling month and the existing average can remain in place. When a new selling month does enter, the oldest leaves if the average already held twelve, and both components are recalculated before the 70/30 mix and seasonality are applied again.
Values you save stay in control. In inventory planning, a saved value takes priority over the automatic forecast for that product and month. If automatic Robust Low for November is 1,590 bottles and a confirmed campaign supports 2,300, you model and save 2,300; November plans at 2,300, while December stays at its automatic 2,120 unless you save that too.
At the next monthly refresh, NeonPanel recalculates the automatic Robust Low number underneath, but it does not recalculate or overwrite your saved 2,300. The value you approved remains unchanged and continues to be the one inventory planning uses for November. It never enters either automatic component, so the weights stay exactly 70 and 30.
One practical note. Robust Low is assembled from the results of the other two scenarios rather than calculated directly from your history. If you ever see it come back with no numbers for a product while other scenarios have numbers, that is not something to fix in your data — contact support.

## How seasonality changes the result

Your seasonality pattern does two jobs, and both components use the same pattern. On the way in it divides your historical months, so both the average and the latest level describe underlying demand rather than calendar shape. On the way out it multiplies the blended level, so each forecast month reflects when your customers actually buy. In the worked example the blended level was flat at 1,060 bottles for all 24 months, while the forecast ranged from 848 bottles in January to 2,120 in December — all of that swing from twelve numbers you configured.
Put a number greater than zero in all twelve months. A zero erases that month: the blended level is multiplied by nothing and the plan becomes zero units. A negative number can produce a negative forecast for that month.
If your pattern is blank or does not have exactly twelve entries, NeonPanel replaces the entire pattern with twelve 1.00 values. If one entry is blank or not a number, only that month becomes 1.00 while the rest are kept. The first case quietly removes all seasonality; the second flattens only the affected month.
Wrong numbers distort both ends. Because the same pattern divides your history and shapes your future, a mis-set month is wrong twice over. If your December number is too low, December's underlying level comes out too high, which lifts the average and can lift the latest-month component too — and the future December is then shaped by that same wrong number.
With a neutral pattern, all twelve numbers are 1.00. The average becomes a plain average of up to twelve months that had sales, the latest-month component becomes that month's plain unit total, and Robust Low blends the two 70/30 into one number that repeats flat across all 24 months. That is also what you get if you have not set a usable pattern. NeonPanel does not work out a seasonal curve from your history on its own.

## What history it needs

Robust Low needs three things: your latest month that had positive sales after returns, up to twelve such months for the average, and a seasonality number for every historical and future calendar month.
Months whose sales total is zero or negative after returns are skipped rather than counted as zero, and the gaps are not filled in. The calculation looks back no more than 18 calendar months. Within that limit, "the last twelve months" means the latest twelve months with positive sales, and "your latest month" means the latest such month — if last month sold nothing, an older month becomes the latest one.
If you have fewer than twelve months with sales, the average uses what is there, and the 70/30 weighting does not change to compensate. That affects how much smoothing you actually get:

With one month of history, the average and the latest month are the same number, so the blend adds no smoothing at all.
With a few months, the average becomes a distinct reference point and the blend starts doing something.
With twelve months, the average uses the full window the scenario is named after.

The arithmetic is exact either way, but exact arithmetic on unrepresentative data still gives you an unrepresentative plan. So look through the months feeding both components for anything that makes them not comparable: one-time promotions, stockouts or periods when the listing was unavailable, listing suspensions, partial months at either end, channel launches or closures, product or pack-size changes, and unusual wholesale or bulk orders. The average smooths these months but has no idea what caused them, and whichever one happens to be your latest month gets 30% direct influence on the plan.

## Things that surprise people

It is not the lowest, and it is not trying to be. When your latest month is weak, Robust Low sits 30% of the way up from that month toward the average, which makes it the higher of the two. No floor, no minimum, no worst case.
The weights are not a confidence level. 70/30 is a mixing ratio, not "70% confident." It does not come from your forecast error, your product's volatility or any service level, and it does not change by product, category or how far ahead the month is.
The forecast is flat underneath. People see a Robust Low forecast rising into November and December and read momentum into it. There is none — the underlying level is a single flat number, the whole shape is your seasonality pattern, and it stays flat until the next refresh.
Your latest month counts twice. It is 30% of the Seasonal Naive component and also one of the months inside the average, so an unusual month reaches the result through two paths.
"Twelve months" may not be twelve calendar months. Because months with no sales are skipped, those twelve months can be spread across a longer stretch of calendar. On an intermittent product the average may reach back much further than you assume.
A short history removes the smoothing without warning you. With very little history the average and the latest month converge, and the blend stops doing the job you selected it for.
It does not know about anything you have planned. Campaign calendars, planned prices, advertising budgets, inbound inventory, supply constraints and production capacity are all invisible to it, and it cannot reconstruct demand you lost to a stockout.
It cannot go negative on its own, but it can with a bad seasonality number. The components work from recorded sales, so the blended level is never negative. A negative number in your pattern can still produce a negative forecast for that month.

## How it compares to the nearby scenarios

Against Seasonal Naive. Seasonal Naive gives your latest month full weight; Robust Low gives it 30% and the average 70%, so it reacts less to any single month. If your latest month is high, Robust Low comes in lower than Seasonal Naive; if it is low, Robust Low comes in higher. That second half is what catches people out — Robust Low is not always the safer or lower choice.
Against 12-Month Moving Average. Moving Average stays fully on the average, while Robust Low moves 30% of the way from it toward your latest month. If your latest month is high, Robust Low is higher; if it is low, Robust Low is lower. Both produce a flat underlying level shaped by seasonality, so the only real difference is responsiveness. Choose Robust Low when some latest-month movement is useful information, and Moving Average when you want stability to win outright.
Against Trend and Seasonality. Trend and Seasonality draws a straight line through your history and keeps extending it, so its plan moves further every month. Robust Low blends two flat-level methods and assumes nothing about direction. Across 24 months, Trend and Seasonality can run far above or far below Robust Low depending on which way its line points, and neither is guaranteed to be lower. Choose on the demand pattern — a stable level versus a continuing direction — not on which name sounds more careful.

## Common questions

Is Robust Low always the lowest forecast?
No. It is exactly 70% Moving Average plus 30% Seasonal Naive, so whenever Seasonal Naive is below Moving Average, Robust Low comes out higher than Seasonal Naive.
Is it a lower confidence bound?
No. It is not calculated from uncertainty, error ranges, confidence intervals or service levels. It is a fixed weighted average of two other forecasts.
Then why is it called Robust Low?
The honest description of what the calculation does is "a smoothed blend." Treat the name as a label and read the numbers.
Can I change the 70/30 weights?
No. They are fixed at 70% Moving Average and 30% Seasonal Naive.
Do the weights change for months further out?
No. The same weights apply to every month across the 24-month forecast.
Does it forecast a trend?
No. The underlying level is flat, and any difference between forecast months comes from your seasonality pattern.
What happens with neutral seasonality?
Every future month gets the same blended unit number, flat across the whole forecast. Nothing seasonal is learned automatically from history.
What if I have fewer than twelve months of sales?
The average uses the months available and the weights stay at 70/30. Months with no sales are not inserted as zeros. With very little history the average and the latest month can be nearly the same, which removes the smoothing.
Can Robust Low be higher than 12-Month Moving Average?
Yes, whenever your latest month's underlying level is above the average.
Can Robust Low be higher than Trend and Seasonality?
Yes. There is no guaranteed ordering, and a declining trend line will drop below the blended level.
Does it account for stockouts?
No. It uses recorded unit sales and does not reconstruct demand you could not fulfil.
Does a value I save change the 70/30 calculation?
No. Saved values do not change either component, their histories or the weights. In inventory planning the saved value applies to the month you saved, and adjacent months stay automatic.
Should I pick this whenever I want a cautious plan?
No. Compare the actual numbers first. Depending on your history, Seasonal Naive, 12-Month Moving Average, Trend and Seasonality or another scenario may come out lower.
