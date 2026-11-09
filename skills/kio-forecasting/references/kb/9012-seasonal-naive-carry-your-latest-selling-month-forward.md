# Seasonal Naive: Carry Your Latest Selling Month Forward

Category: Forecast Scenarios · Article 9012 · Updated 2026-08-24

Carries your most recent selling month forward and reshapes it with your seasonality.

## Introduction

Seasonal Naive takes the most recent month in which your product actually sold something, takes that month's seasonal effect out of the number, and treats what is left as your product's normal monthly sales right now. It then reshapes every future month with your seasonality numbers. It suits a product that has settled at a level you believe in, and it suits anyone who wants a forecast they can rebuild in a spreadsheet in under a minute.
One thing to clear up before anything else: despite the name, Seasonal Naive does not copy the same month from last year. Your December forecast does not come from last December.

## The short version

It uses one month as its starting point: the most recent month that had sales. Nothing older feeds the number.
It divides that month's units by that month's seasonality number to get your underlying monthly sales, then multiplies each future month by that month's seasonality number.
It does not smooth history and it does not add growth or decline. The same underlying level repeats across all 24 months NeonPanel forecasts by default.
Everything that moves up or down comes from your seasonality numbers. With all twelve set to 1, every future month is identical to the latest selling month.
Months with no sales at all are skipped, not counted as zero. "The latest month" therefore means the latest month that sold something, which is not always last month.
It reacts immediately. One new month of sales can move the whole forecast at the next monthly refresh.
Its strength is that it is transparent and fast to react. Its risk is the mirror image: one strange month sets the level for everything ahead.
It knows nothing about your future promotions, price changes, advertising, or inbound stock. Use Kio to model those effects and save the months you want to adjust.

## How it works

NeonPanel reads your completed monthly unit sales for that product, in that marketplace and channel, going back as far as 18 months. Months where the product sold nothing are not in that history at all, and neither are months that come out at zero or below after returns. Nothing is filled in for them, so a gap is simply a gap rather than a zero.
From that history the scenario takes exactly one month: the most recent one with sales. Say that is June, and June sold 1,200 units. Your seasonality pattern says June is 1.20, meaning June normally runs 20% above a normal month for this product. Divide 1,200 by 1.20 and you get 1,000. That 1,000 is your underlying monthly sales — what the month would have looked like with the summer lift taken out.
That division is the step people skip when they try to check the number by hand. If NeonPanel treated the raw 1,200 as your normal level and then multiplied December by 2.00, June's summer lift would be baked in twice and December would come out 20% too high. Taking the seasonal effect out first means each future month gets its own seasonal shape and only its own.
Then the forecast is straightforward multiplication. October's seasonality number is 1.10, so October forecasts 1,000 × 1.10 = 1,100 units. November at 1.50 gives 1,500, December at 2.00 gives 2,000, January at 0.80 gives 800. The 1,000 never changes across the whole forecast, and it stays in place until the next monthly forecast run finds a newer month with sales — at which point that newer month becomes the starting point and every future month is recalculated from it.
NeonPanel rounds each month's final unit number to a whole unit before working out forecast revenue. So if June had sold 1,205 units instead, the underlying level would be 1,004.17, and October would come out at 1,104.58 and be stored as 1,105 units. Revenue is those whole units multiplied by a price: the average price over up to your last three months with sales, or your overall historical average price if three are not available. The scenario does not predict how a price change would move demand — price only turns units into money.
That is also what "naive" means here: a deliberate, simple rule where your current level continues. It is not a comment on your data, and it does not mean the scenario ignores seasonality or reuses last year's calendar.

## A worked example

Meet the Harbor 20 oz insulated bottle. It sells steadily most of the year, lifts through the summer, and peaks hard at holiday gifting. Its planner runs the July purchase order review and wants to know what Seasonal Naive is telling her. At the July refresh, June is the latest month with sales, and June sold 1,200 bottles. The company has a real seasonality pattern configured for this product — not the neutral one — and nothing about future promotions, price changes, or advertising is part of this calculation.

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

Step one: take June's summer lift out. June's 1,200 bottles already include the 20% June lift. Divide 1,200 by 1.20 and the underlying level is 1,000 bottles a month.
Step two: give each future month its own shape.

| Forecast month | Underlying monthly sales | Month's seasonality number | The arithmetic | Forecast |
|---|---|---|---|---|
| July | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| August | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| September | 1,000 | 1.00 | 1,000 × 1.00 | 1,000 bottles |
| October | 1,000 | 1.10 | 1,000 × 1.10 | 1,100 bottles |
| November | 1,000 | 1.50 | 1,000 × 1.50 | 1,500 bottles |
| December | 1,000 | 2.00 | 1,000 × 2.00 | 2,000 bottles |
| January | 1,000 | 0.80 | 1,000 × 0.80 | 800 bottles |

Four things in that table are worth pausing on. June's 1,200 bottles are not copied into every month — they became 1,000 first. December's 2,000 comes from the current level and December's own number, not from what last December sold. The climb from October to December is your seasonality pattern and nothing else. And there is no trend in it anywhere: set December to 1.00 and December forecasts 1,000 bottles like everything else.
Now run the same product with a neutral pattern, all twelve months set to 1. The underlying level becomes 1,200 ÷ 1.00 = 1,200 bottles, and every future month forecasts 1,200 — a flat line at last month's sales. NeonPanel does not go through the Harbor bottle's history and rebuild the summer and holiday shape for you. A neutral pattern gives you a neutral forecast.

## When to choose it

Seasonal Naive earns its place when the latest month with sales genuinely is your best single answer to "where is this product right now?" That is often true for a mature replenishable product with a known holiday shape, and true right after something changed for good — a distribution win that has settled, a repackaging, a price move that has bedded in.
It is also the right pick when older history would actively mislead you. If your product spent last spring out of stock, or was a different bundle nine months ago, an average across that history describes a business you no longer run. And it is the right pick when you need speed over smoothness: a twelve-month average takes most of a year to fully acknowledge that a product has doubled, while Seasonal Naive does it at the next refresh.
Concrete cases where it fits well:

A steady replenishable product with a well-understood holiday peak, where last month is a fair read on the current level.
A product that has just finished a lasting distribution expansion and has settled at a new, higher level.
A recently launched item with only a few months of history, where the latest completed month says more than an average of an incomplete ramp.
A replacement SKU whose current sales are representative but whose predecessor's older history is not.

All of those cases assume three things: a credible twelve-month seasonality pattern, a product mature enough that you do not need to assume growth or decline, and someone who will check whether anything odd happened in the starting month. It also helps that the forecast is easy to verify by hand — two arithmetic steps, one number in, one number out.

## When to avoid it

Avoid Seasonal Naive when the latest month with sales was not a normal month. A flash sale, an influencer spike, a bulk wholesale order that landed through your ecommerce channel — any of these makes the starting point too high, and that inflated level then sits under every month ahead of you.
Avoid it just as firmly when the latest month was held down. A stockout, a listing suspension, a shipping interruption, a lost buy box: the month recorded low sales, and Seasonal Naive treats that low number as your current demand.
Look elsewhere too when:

Sales are clearly climbing or falling and you want that direction carried into the plan. Seasonal Naive will not do it.
The product is intermittent, with many months that sold nothing. The starting point can end up being a month from a while ago, and it may not represent much.
One month is simply too noisy for this product to stand in for normal demand.
Your seasonality pattern is missing or neutral but the product genuinely has a seasonal shape you need reflected.
Leadership wants a planned growth target in the numbers rather than a mechanical continuation.
Future demand depends on launches, channel expansion, price changes, or a campaign calendar. None of that is part of this calculation.
The latest completed month is only partly recorded, or has data-quality problems you have not resolved.

And a framing point worth saying out loud: Seasonal Naive is not a target. It is a mechanical continuation of your current level. If you need the forecast to express an ambition, use a planned growth scenario and own the assumption.

## What it does to your buying plan

The practical consequence of a one-month starting point is that your forecast moves in steps rather than drifting, and when it moves it moves everywhere. If June's underlying level was 1,000 bottles and July closes at an underlying 850, the next refresh replaces 1,000 with 850 across the entire automatic forecast — not just next month, but every month for the next two years. Your suggested purchase quantities move with it.
That makes reorder quantities responsive but jumpy. A single strong month can lift your recommended buy for a whole season and pull cash forward into inventory you may not need. A single weak month can cut it and leave you short in your peak. The scenario has no memory to lean on, so it will not argue with either.
The two errors do not cost the same. Overstating your level ties up cash and warehouse space and bleeds slowly through storage, aged stock, and markdowns. Understating it costs you fast, especially in November and December, where an 800-unit gap in a 2,000-unit month is a month you cannot get back. On a long lead time, the starting month you are forecasting from may be four or five months old before the goods land.
So the habit worth building is a quick review of the starting month before you commit a purchase order. Was the product in stock all month, was there a promotion, was there a large one-off order, did a listing go down? If any of those is true, either move the product to a scenario that smooths or save your own monthly numbers.
What changes each month
NeonPanel recalculates the automatic forecast on the 10th of each month at 08:00 UTC. At each run the newest month with sales becomes the starting point, its seasonal effect is taken out, and the resulting level is applied across all future months with each month's own seasonality number. That is why the whole forecast can look different after a refresh even though only one month of history is new.
When you adjust and save months with Kio
Values you model and save with Kio sit on top of the automatic forecast rather than inside it. In inventory planning, a saved value wins for that product and month; the months either side stay automatic unless you save them too. If a month has more than one saved value, the most recent one is used. The automatic Seasonal Naive figure is still stored underneath.
So: the automatic November forecast is 1,500 bottles, you have a confirmed retail promotion, and you use Kio to model and save November at 2,200. The operational November plan uses 2,200. December stays at its automatic 2,000 unless you save December too.
At the next monthly refresh, NeonPanel recalculates the automatic Seasonal Naive forecast underneath, but it does not recalculate or overwrite your saved 2,200. The value you approved remains unchanged and continues to be the one inventory planning uses for November. It also never becomes an input to the automatic calculation, so one saved month does not reshape the rest of the year; if an event spans several months, save every month it touches.

## How seasonality changes the result

Seasonality is the only thing that gives a Seasonal Naive forecast any shape at all, which makes your twelve numbers unusually important here. A number of 1.00 leaves the month alone, 1.20 puts it 20% above your normal level, 0.80 puts it 20% below, and 2.00 doubles it. The pattern is twelve values in calendar order, January through December.
Each number is used twice, in opposite directions, and that is the part that catches people. Your starting month is divided by its number, and each future month is multiplied by its own. An overstated June makes your underlying level too low for the next two years; an overstated December then makes December too high on top of that.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast entirely, and a negative number can produce a negative forecast. Neither is caught for you, and neither is what you want.
If the pattern is missing, empty, has the wrong number of entries, or contains a blank or non-numeric entry, NeonPanel falls back to neutral behavior — 1.00 for the affected value or for all twelve. A neutral pattern gives you a completely flat forecast equal to your latest selling month. It does not mean NeonPanel will work out your seasonal shape from your sales history; that does not happen.
Changing your seasonality pattern never rewrites your recorded actual sales. It changes how history is read on the way into the forecast and how future months are shaped on the way out. Worth reviewing the pattern when your peak season shifts, when you enter a country with a different holiday calendar, when your category mix changes, when a one-off promotion got mistaken for a recurring season, or simply when the annual shape the numbers describe no longer matches the business you see.

## What history it needs

The minimum is modest: one completed month with recorded unit sales for that product in that marketplace and channel, plus a seasonality number for that month and for each month you are forecasting. That is all the arithmetic requires.
NeonPanel loads up to 18 months of history, but Seasonal Naive only uses the latest month that had sales. The other months are not averaged in and do not soften the result, so having eighteen months available does not make the forecast any more stable.
Because months with no sales are skipped rather than stored as zeros, the starting point can be older than you expect. If your product sold nothing in May and June, the July forecast starts from April — a month that is a season out of date. For intermittent products that is one of the strongest reasons to choose a different scenario.
This scenario also reads recorded sales as evidence of demand. If your recorded units were limited by what was on the shelf, the forecast inherits that limit. It does not reconstruct the demand you could not serve.

## Things that surprise people

"Latest month" is not always last month. If the previous calendar month sold nothing, it is not in the history at all and an older month becomes the starting point. Nothing in the forecast flags that for you.
One month decides everything. A 25% error in the starting month is a 25% error across the entire two-year forecast, every month of it, until the next refresh.
A stockout becomes your new normal. If the product sold 500 units only because it was unavailable for half the month, Seasonal Naive starts from 500 units after seasonal adjustment. It does not know about the half-month it missed.
A promotion does the same thing upward. A campaign that doubles the latest month makes that doubled level your baseline until a later refresh replaces it or you save your own numbers over it.
It cannot tell a real trend from noise. A product climbing 5% a month is under-forecast for as long as it keeps climbing, and a product in genuine decline is over-forecast. Neither improves on its own.
It knows nothing about your calendar. Promotion schedules, advertising budgets, planned price changes, inbound purchase orders, production capacity, marketplace events — none of it is read.
A long forecast is not a confident one. Getting 24 months of numbers does not make month 20 as trustworthy as month 2. It is one current level, repeated, wearing twelve different seasonal hats.
The forecast can dip without declining. Months below 1.00 come out lower, so the line falls through January and February while the underlying level has not moved at all.
A zero in your seasonality wipes out a month. It is the fastest way to get a forecast month of zero units, and it looks like a bug rather than a setting.

## How it compares to the nearby scenarios

Against 12-Month Moving Average
Seasonal Naive uses one month — the latest with sales — after taking the seasonal effect out. 12-Month Moving Average averages up to twelve months that had sales, each one adjusted the same way. Seasonal Naive reacts faster when your product has genuinely moved to a new level; Moving Average is far less bothered by one odd month.
Neither adds a trend. Both produce a flat underlying level, and in both the month-to-month movement comes from your seasonality numbers. Choose Seasonal Naive when the latest month is the truest picture of now, and Moving Average when a longer stretch of history gives you a more reliable read on normal.
Picture the Harbor bottle with a strong June. If June's adjusted level is 1,200 bottles while the latest twelve months that had sales — including June — average 1,000, Seasonal Naive plans the year off 1,200 and Moving Average plans it off 1,000. Same seasonality pattern in both — the 200-bottle gap is smoothing, nothing else.
Against Trend and Seasonality
Seasonal Naive carries your current level forward flat. Trend and Seasonality draws a straight line through your months that had sales and extends it, so it can express sustained growth or sustained decline that Seasonal Naive simply ignores.
Seasonal Naive is easier to audit and it will not run away from you two years out, which a straight line extended far enough eventually does. Trend and Seasonality is the better fit when the direction is real and you expect it to continue. Neither is reliably more cautious.
Against Robust Low
Robust Low is a blend: 70% of the 12-Month Moving Average forecast plus 30% of the Seasonal Naive forecast. Compared with Seasonal Naive alone, it pulls an unusually high latest month back toward the longer-run average.
Do not read the name as a guarantee. If your latest month is below your twelve-month average, Robust Low comes out higher than Seasonal Naive, not lower. It is a smoothed blend of two forecasts, not a floor, not a lower bound, and not a worst case.

## Common questions

Does Seasonal Naive use the same month from last year?
No. It uses the latest month that had sales, takes that month's seasonal effect out, and applies each future month's own seasonality number. December's forecast has no direct connection to last December.
Why call it seasonal if the underlying level is flat?
Because the level is only half of it. Each future month is multiplied by its own seasonality number, so the forecast you see rises and falls across the year.
What happens if all twelve seasonality numbers are 1.00?
Every future month forecasts the same units as the latest month that had sales. NeonPanel does not learn a seasonal curve from your sales history to fill the gap.
Does it average several recent months?
No, that is 12-Month Moving Average. Seasonal Naive uses exactly one month.
Does it forecast promotions or holiday events on its own?
Only to the extent that a recurring monthly shape is already in your seasonality numbers. It does not read a promotion calendar and it does not know a specific future event is coming.
What if the latest month was unusually high?
That high level carries into all 24 forecast months. Review what caused it, then either move the product to 12-Month Moving Average or Robust Low, or save your own numbers over the affected months.
What if the latest month was unusually low because of a stockout?
The forecast runs low, because it reads units sold rather than demand you could not serve. Nothing corrects for it, so this one needs your judgement.
Does changing my seasonality pattern change my past sales?
No. Your recorded actual units are untouched. The pattern changes how history is read into the forecast and how future months are shaped.
Can the forecast decline over time?
Not from a trend — there is no trend in this scenario. It can come out lower in months whose seasonality number is below 1.00, but the underlying level stays exactly where it is until the next refresh.
Is it suitable for a new product?
It can be, if the latest completed month is representative. It is risky while a launch is still ramping, or when that month was driven by launch promotions, or when the month is not fully recorded yet.
Does a number I save change next month's automatic forecast?
No. Saved values are not inputs to the calculation. In inventory planning they replace the automatic figure for that product and that month only, and the automatic forecast stays stored underneath.
Is this always more cautious than Trend and Seasonality?
No. It depends entirely on your history and your seasonality numbers. A declining product can forecast lower under Trend and Seasonality, while a recent spike can make Seasonal Naive the higher of the two.
