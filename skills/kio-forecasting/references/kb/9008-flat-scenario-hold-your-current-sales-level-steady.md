# Flat Scenario: Hold Your Current Sales Level Steady

Category: Forecast Scenarios · Article 9008 · Updated 2026-08-24

Holds demand at your latest level, with no planned growth or decline.

## Introduction

Flat Scenario takes the units you sold in the last completed month, removes the seasonal effect of that month, and plans on you selling that same underlying amount every month going forward. It suits a steady product you are not trying to grow or wind down — one you simply want to keep in stock at its current pace. It is a plan you choose rather than a prediction NeonPanel makes, so the number it produces is only as good as the month it starts from.

## The short version

| What you want to know | What Flat Scenario does |
|---|---|
| Where does the starting number come from? | Units sold in the last completed calendar month, divided by that month's seasonality number. |
| Does the plan grow over time? | No. The underlying level stays the same every month. |
| Will every month show the same units? | No. Each month's seasonality is applied afterwards, so peaks and dips still appear. |
| Does it study your sales trend? | No. It does not average your history or draw a line through recent months. |
| Does it use PPC spend, promotions, or price plans? | No. None of those are inputs to the units. |
| Does it look at stock on hand or inbound shipments? | No. It plans demand without checking that you can supply it. |
| What happens next month? | It rebuilds from the newest completed month, so the whole plan moves with it. |
| Can Kio adjust a month? | Yes. A value you save with Kio wins for that product and month in inventory planning. |
| What if last month had no sales? | No automatic forecast at all for that product. NeonPanel does not substitute an average. |
| How far ahead does it run? | 24 months. |

Flat Scenario is the simplest curve NeonPanel offers, and that is the point: last month becomes the level, the level repeats, seasonality reshapes the months, and nothing else in your history changes the answer. It also means the whole plan rests on one month. Before you assign it, ask whether the last completed month is a fair picture of how that product sells, because a month inflated by a promotion or flattened by a stockout carries that distortion through the next two years, until the following monthly rebuild replaces it.

## How it works

It starts from one month, not from your history
Flat Scenario looks at a single number: the units you sold in the calendar month immediately before the forecast. It does not average the last twelve months, does not weight recent months more heavily, and does not draw a line through your recent sales.
That single-month rule is stricter than it sounds. Your sales history in NeonPanel keeps only the months that actually had sales — a month with no sales at all is skipped entirely rather than stored as a zero. For most scenarios that just means an empty month is passed over. For Flat Scenario it decides everything, because the one month it needs is the month immediately before the forecast.
It takes the season out of the starting month
Sales from a single month carry that month's seasonal effect, so NeonPanel removes it before repeating the level: it divides the month's units by that month's seasonality number.
A seasonality number of 1.4 means that month usually runs 40% above your normal pace, and 0.8 means 20% below. If December sold 100 units and December's number is 2.0, the underlying level is 100 divided by 2.0, or 50 units a month. With seasonality switched off — every month set to 1.0 — the underlying level is simply the units you sold.
This step stops a holiday peak from becoming your year-round plan. Without it, a strong December would tell NeonPanel to buy for December-sized demand in February.
It holds that level steady
Flat Scenario then applies a stored percentage to that underlying level for each future month. The flat curve is 100% for every month, so the underlying plan simply repeats: 50 units of underlying demand this month, 50 next month, 50 the month after. Those percentages live in NeonPanel's settings data rather than in the application code, so the curve can be adjusted without a software release.
It puts the season back in
For each future month, NeonPanel multiplies the underlying level by that calendar month's seasonality number. An underlying level of 50 units becomes 40 units in a month with a 0.8 seasonality number, 50 units in a neutral 1.0 month, 75 units in a 1.5 month, and 100 units in a 2.0 month. That is the number you see in the forecast and the number that feeds your purchase planning, before any value you model and save with Kio.
NeonPanel writes 24 months this way, each using the same underlying level and its own calendar-month seasonality number, so the plan two years out is built exactly like next month's.
When there is nothing to start from
Flat Scenario needs the month immediately before the forecast to have sales above zero. If that month is missing, or its sales are zero, or removing its seasonality leaves zero or less, NeonPanel produces no automatic forecast rows at all for that product. It does not fall back to a three-month or twelve-month average, and it does not reach further back for an older month that did have sales.
An empty forecast is a signal to investigate, not a system error. Check whether the product was out of stock, whether the listing was suppressed or paused, whether channel data arrived late, or whether demand genuinely stopped. If you still need a plan, either move the product to a scenario that uses a longer stretch of history or use Kio to model and save the monthly values.
One other case does the same thing: if the flat percentage curve itself cannot be loaded, the scenario produces no rows rather than guessing at a default.

## A worked example

Meet the Harbor Mug, a 12-ounce ceramic mug that sells steadily all year and spikes in the gift season. The last completed month is December, which sold 100 units. Its saved seasonality pattern, January through December, is 0.8, 0.8, 0.9, 1.0, 1.0, 1.2, 1.0, 1.0, 1.0, 1.1, 1.5, 2.0 — so June normally runs 20% above the product's usual pace and December runs at double.
Find the underlying level. December's seasonality number is 2.0, so NeonPanel divides December's 100 units by 2.0 and gets 50. It reads that December as a 50-unit-a-month product having a double-strength December, not as a 100-unit-a-month product.
Hold the level flat. The flat curve is 100% in every month, so the underlying plan is 50 units a month for the next 24 months.
Reapply each month's seasonality. January's number is 0.8, so January is 50 times 0.8, or 40 units. December's number is 2.0, so next December is 50 times 2.0, or 100 units — the same as the December you just finished, which is exactly what "flat" should mean.
Here is the whole first year:

| Forecast month | Seasonality number | The arithmetic | Units the plan calls for |
|---|---|---|---|
| January | 0.8 | 50 × 0.8 | 40 |
| February | 0.8 | 50 × 0.8 | 40 |
| March | 0.9 | 50 × 0.9 | 45 |
| April | 1.0 | 50 × 1.0 | 50 |
| May | 1.0 | 50 × 1.0 | 50 |
| June | 1.2 | 50 × 1.2 | 60 |
| July | 1.0 | 50 × 1.0 | 50 |
| August | 1.0 | 50 × 1.0 | 50 |
| September | 1.0 | 50 × 1.0 | 50 |
| October | 1.1 | 50 × 1.1 | 55 |
| November | 1.5 | 50 × 1.5 | 75 |
| December | 2.0 | 50 × 2.0 | 100 |

The monthly line rises and falls between 40 and 100 units, but the underlying 50-unit target never moves. Over the twelve months the plan comes to 665 units, a total set as much by your twelve seasonality numbers as by the flat curve.
With seasonality switched off, every month would be 1.0, December's 100 units would divide by 1.0 to give an underlying level of 100, and every future month would be forecast at 100 units — including January, which really sells 40. Those twelve numbers are doing real work, and they are worth getting right.
Numbers on screen may be rounded for display. Flat Scenario keeps fractions internally, so a plan of 52.5 units is stored as 52.5 even if the report shows 53.

## When to choose it

Flat Scenario earns its place when holding your current pace is the actual plan, not just the absence of one. It fits when your team has deliberately decided to keep a product at its current level, when a mature product has no approved growth or decline target, and when recent sales fairly reflect the level you intend to support.
It also suits you when you want seasonality to shape your buying without a growth curve on top. Because the underlying level is fixed, everything moving on the chart is your seasonality pattern, which makes the plan easy to explain to a supplier or a finance partner.
Choose it in these situations too:
Your history is too short or too disrupted for a statistical scenario, but the last completed month is trustworthy.
A launch ramp has finished and you want to hold the level the product settled at.
You want a target anyone can audit: one month, one division, one multiplication.
You need a placeholder while a more detailed plan is prepared and approved.
The common thread is the starting month. Flat Scenario is only as sound as that month is representative, so "simple" does not mean "safe in all conditions."

## When to avoid it

Skip Flat Scenario, or use Kio to model a more suitable plan, when the last completed month is not a fair reading of the product: a month inflated by a one-time promotion, a clearance event, or a launch spike, or a month pushed down by a stockout, a suppressed listing, or an operational problem. Flat Scenario does not examine why the month came out as it did — that judgment is yours.
Avoid it as well when:
Your business has an approved growth target that should lift the plan over time. A Growth scenario expresses that directly.
Demand is clearly declining, and a flat plan would keep buying at a level you no longer sell through.
Your history shows a steady, believable trend you would rather have measured than chosen. That is what the statistical scenarios are for.
The seasonality pattern is missing, stale, or wrong for this marketplace, because a flat plan leans on those twelve numbers more than most.
Known events — a confirmed wholesale order, a channel launch, a planned pause — need specific values in specific months rather than one repeating rule.
You want a pricing or advertising decision reflected in the units. Flat Scenario will not model it.

## What it does to your buying plan

Because the underlying level never changes, Flat Scenario gives you a purchase plan with a predictable annual shape. Everything you order above or below your average pace traces your seasonality pattern, so working back to order dates is simple: take the units for each month and add your lead time.
The peaks are where the money sits. In the Harbor Mug example, November and December together call for 175 units against a 50-unit underlying level — more than a quarter of the year's 665 units in two months. With a long lead time you commit cash for those units months ahead, on a plan set by a single December. Size that commitment against your confidence in the starting month, not against how smooth the curve looks.
Flat Scenario never caps its plan against what you can supply. It does not read stock on hand, inbound shipments, factory capacity, or your purchasing budget, and it will call for units you have no way to source, so feasibility stays a separate check in replenishment and cash planning. It also does not correct for demand you lost: if a stockout held last month below what customers wanted to buy, it treats that depressed number as your real level and plans down to it, quietly locking in a lower buying pace for a product that was actually selling out.

## Adjusting a month with Kio

A forecast you model and save with Kio is not one of the automatic scenarios; it is an adjustment on top of them. In inventory planning, a saved value takes priority for that product and forecast month: months without a saved value keep using Flat Scenario, and saving November does not touch October or December. If more than one value exists for the same month, the most recent one is used.
The automatic Flat Scenario result stays stored underneath and is recalculated at the next monthly refresh. The refresh does not recalculate or overwrite your saved value: it remains unchanged and continues to be the value inventory planning uses for that product and month. A saved value never trains the scenario or feeds the next automatic calculation, which still starts from actual sales in the newest completed month.
That means you can keep Flat Scenario as your standing rule and use Kio where you know something it cannot. Leave January through October automatic, then model and save a November promotion and a December retail target. The automatic forecast keeps refreshing underneath, while the November and December values you approved remain in control.
One caveat: the month-by-month behavior above is what inventory planning does. The Sales Forecast Live view picks one active run first and gives saved values priority, so there a saved value can hide the automatic numbers for months you did not save. If the two screens disagree, that is why.

## How seasonality changes the result

Think of Flat Scenario as two layers. The bottom layer is the underlying level, the seasonally adjusted pace you intend to hold. The top layer is your twelve seasonality numbers. Flat Scenario freezes the bottom layer and lets the top layer do all the visible work.
That is why the chart can climb steeply without the plan growing at all. A December far above January is not Flat Scenario adding growth; it is a 2.0 December sitting on the same underlying 50 units as a 0.8 January. It also means your annual total is really a seasonality decision — the example year comes to 665 units, and raising November and December raises the total even though nothing about "flat" changed.
Put a number greater than zero in every one of the twelve months. A zero erases that month from the forecast entirely, because the underlying level gets multiplied by nothing. A negative number is worse: it produces a negative forecast, which no scenario is designed to handle. Numbers are accepted as written, so a stray 15 where you meant 1.5 produces a plan ten times too big for that month, and a number set too low quietly under-buys a peak and leaves you stocking out in it.
NeonPanel expects exactly twelve values, one per month from January to December. If the list is blank or has the wrong number of entries, every month falls back to 1.0. A single blank or non-numeric entry becomes 1.0 for that month while the rest of your pattern is kept.
If you leave seasonality neutral, your forecast shows the same units every month. Flat Scenario will not detect a seasonal shape from your history and apply it for you — neutral means a flat calendar, not an automatically learned one.
The pattern matters at both ends: the starting month is divided by its number and every future month is multiplied by its own. An inaccurate number on the starting month distorts the whole two-year plan, not just that month.

## What history it needs

Flat Scenario needs one month: the calendar month immediately before the forecast, with sales above zero. That is the whole requirement. You do not need a year of clean data, and having one does not change the result.
NeonPanel loads up to 18 months of sales history for the monthly forecast run, and other scenarios use much of it, but Flat Scenario reads only that last completed month. A long, rich history behind a weak or missing final month will not make the plan reliable.
Remember that months with no sales are not stored as zeros; they are absent. A product that sold nothing last month has no month for Flat Scenario to work with, and the result is an empty forecast. Intermittent sellers, seasonal items that go quiet out of season, and anything out of stock for a full month all hit this.
Sales history is tracked separately for each product in each marketplace and channel, so a product can produce a forecast in one marketplace and nothing in another. Check that specific combination rather than the product overall.

## Things that surprise people

The plan moves every month. Flat Scenario is not frozen when you set it. Each monthly rebuild starts from the newest completed month, so a soft month pulls the whole two-year plan down and a strong month pushes it up. If the Harbor Mug's December gave an underlying level of 50, and January then closes at 36 units against a 0.8 seasonality number, the new level is 36 divided by 0.8, or 45. The plan drops 10% — not because NeonPanel detected a downward trend, but because the starting point changed.
A quiet month can erase the forecast. One month with no sales means no automatic rows at the next run. This bites hardest on slow movers and new marketplaces where sales are sporadic.
Flat does not mean identical months. People regularly read a rising forecast as growth. The underlying level stays flat; the visible monthly numbers follow the calendar.
A zero in your seasonality pattern deletes a month. The pattern is taken as entered, so a mistake there flows straight into what you buy.
It cannot see the reason behind a number. A promotion, a competitor going out of stock, a viral week, a warehouse fire — all look identical to it. It reads units, nothing else.
It is not the default. If no scenario is set and no company default applies, or if the scenario on a product is something NeonPanel does not recognize, the forecast uses Recency-Weighted Growth. An unrecognized setting on a product does not fall back to your company default either; it goes straight to Recency-Weighted Growth.
Revenue follows units, not the other way around. Flat Scenario plans units and then applies a price, normally holding price at the previous month's level and calculating revenue as forecast units multiplied by that price. Planned price changes do not change the units it forecasts.
It rebuilds on a schedule, not on demand. Automatic forecasts refresh on the 10th of each month at 08:00 UTC, so a correction to seasonality or to a product's scenario reaches the automatic numbers at the next run rather than immediately.

## How it compares to the nearby scenarios

Against Growth 10. Both work the same way: start from the last completed month, remove that month's seasonality, apply a stored percentage curve, then reapply each future month's seasonality. Only the curve differs. Flat holds at 100% every month; Growth 10 climbs gradually to 110% by the twelfth. Neither derives anything from your PPC or pricing plans. Choose Flat when holding your level is the plan; choose Growth 10 when a modest, approved increase is.
Against Seasonal Naive. These two can produce almost identical charts, because Seasonal Naive also carries a recent seasonally adjusted level forward. What differs is the meaning and the starting point. Flat Scenario is a target you chose, and it strictly requires the immediately previous month. Seasonal Naive is a baseline method that carries forward the most recent month that had sales, which may be older than last month if last month was empty. Pick on whether you want a stated target or an automatic baseline, not on which chart looks better.
Against 12-Month Moving Average. Flat rests on one month; the moving average smooths up to twelve months that had sales. That makes it far less rattled by a single strange month, but also slower to react when your sales level genuinely changes, so a product that has stepped up to a new pace stays under-forecast for a while.
Against adjusting values with Kio. Flat Scenario gives you one reusable rule that keeps refreshing itself; values you save with Kio give you exact numbers for specific months. When your plan is event-driven — a confirmed purchase order, a channel launch, a temporary pause — Kio can express it far better than any curve. Many teams use both.

## Common questions

Is Flat Scenario predicting that my sales will stay flat?
No. It is a target you chose: use last month's underlying level as the pace you intend to hold, then reshape it by season. It makes no claim about how likely that is.
Why does my forecast go up and down if the scenario is flat?
Your seasonality pattern does that. The underlying level is unchanged; the calendar shape sits on top of it.
Does it average my sales history?
No. It uses the last completed month with that month's seasonality removed. The monthly run loads up to 18 months of history for other purposes, but Flat Scenario reads only that one month.
Does it know a stockout held last month down?
No. It uses recorded sales and never looks at stock availability. Check availability yourself before accepting a plan built on a weak month.
Will spending more on ads raise the Flat forecast?
No. Ad budgets and campaign calendars are not inputs to the units. If you expect an advertising-driven lift, pick a growth curve that reflects it or save the monthly numbers you are planning for.
Will a planned price change alter the units?
No. The scenario does not model how price affects demand. Review units and revenue assumptions separately.
If I leave seasonality neutral, will it learn my seasonality?
No. A neutral pattern gives you the same units every month. Enter your twelve numbers if your product is seasonal.
What happens when the starting month has a seasonality number of 2.0?
Its units are halved to find the underlying level. In the example, December's 100 units become an underlying 50.
Are the flat percentages always 100%?
The Flat curve is 100% in every month. Those percentages are stored as settings data rather than fixed in code, so if your numbers ever suggest otherwise, raise it with support.
Does it stop at what I can actually supply?
No. It plans demand regardless of stock on hand, inbound shipments, or production capacity.
What happens if I save a forecast for just one month?
In inventory planning, that month uses your saved value and every other month keeps using Flat Scenario. The Sales Forecast Live view behaves differently and can hide the automatic numbers for unsaved months.
Does my saved forecast change the next automatic calculation?
No. Saved values are overrides. The next run starts from actual sales in the newest completed month.
What happens when a product has no scenario set?
It can be, if Flat Scenario is your company default. When a product has no scenario set, NeonPanel tries the company default first. Recency-Weighted Growth is used only when NeonPanel cannot resolve a valid supported scenario.
How far ahead does it forecast, and how often should I review it?
24 months. Review after every monthly refresh, and whenever the last completed month, your seasonality pattern, or your business plan changes materially.
