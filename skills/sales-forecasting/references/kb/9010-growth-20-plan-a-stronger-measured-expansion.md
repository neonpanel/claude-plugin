# Growth 20: Plan a Stronger, Measured Expansion

Category: Forecast Scenarios · Article 9010 · Updated 2026-08-24

Plans for about 20% growth over the year ahead.

## Introduction

Growth 20 is for a product you intend to grow substantially over the next year or two. It takes what the product sold last month, sets that month's seasonality aside, raises that underlying level month by month along a fixed upward curve, and then puts seasonality back on for each future month. It suits an operator who has an approved growth plan behind the product and enough cash, storage, and supplier capacity to carry the stock that plan implies.
Growth 20 sits between the gentle Growth 10 curve and the aggressive Growth 30 curve. Everything in it is a target you have chosen, not a prediction NeonPanel worked out from your sales history.

## The short version

| What you want to know | How Growth 20 behaves |
|---|---|
| Where does the plan start? | From the units your product sold in the previous completed month, with that month's seasonality divided out |
| What makes it grow? | A fixed percentage curve held in your NeonPanel settings. Nothing about the curve is learned from your sales |
| How steep is it? | The reference curve starts at 101% of that underlying level in the first month and reaches 121% by the twelfth |
| Why 121% and not exactly 120%? | The curve is shaped so a year of growth builds on itself, and it lands slightly above 120%. The name is a label for the curve, not a promise about any single month |
| How far ahead does it run? | 24 months |
| Can the plan fall from one month to the next? | Yes. A weak seasonal month can more than cancel out the small step up in the curve |
| Does it read your ad spend, promotions, or prices? | No |
| Does it check whether you can supply or afford the units? | No |
| What happens every month? | The whole curve restarts from the newest completed month |
| What does it need from your history? | Sales in the immediately previous calendar month. Without them you get no automatic forecast for that product |
| Can you replace individual months? | Yes. A number you save for a month wins for that month in inventory planning |

## How Growth 20 builds your plan

There are three moving parts: the month you start from, the curve you have chosen, and your seasonality numbers. Judge them separately, because they fail separately. A sensible curve on a distorted starting month gives you a distorted plan, and a fair starting month with a fair curve can still produce an unbuyable November if your seasonality numbers are too strong.
It starts from last month, not from a year of averages
Growth 20 uses the units your product sold in the previous completed month as its starting point. It does not average your last twelve months, and it does not measure how fast you have been growing. Whatever last month did is the foundation for the whole two-year plan.
Your seasonality number for that month comes out first
Before any growth is applied, NeonPanel divides last month's units by your seasonality number for that calendar month. A product that sold 100 units in December, in an account where December is set to 2.0 because December normally runs at double a normal month, gives an underlying level of 50 units rather than 100. If December were set to 1.0, the underlying level would stay at 100. If the starting month were a quiet one set to 0.8, 100 units would become an underlying 125.
This step exists so a seasonal spike is not mistaken for your new everyday sales level and then multiplied a second time in every future month. It matters most when the month you are starting from is either the biggest or the quietest month of your year.
The curve percent
Once NeonPanel has that underlying level, it multiplies it by a stored percentage for each of the next 24 months. The percentages for Growth 20 climb from about 101% in the first forecast month to about 121% in month twelve, then continue on to roughly 145% by month twenty-four. These are reference points — the exact monthly values live in configuration and can be tuned — but the shape is a steady, compounding-style climb rather than a straight line.
Because the percentages compound, the growth accelerates slightly as the plan goes on. The distance from month eleven to month twelve is larger than the distance from month one to month two, even though both represent one month of the same curve.
Seasonality goes back on last
With the growth curve applied to the underlying level, NeonPanel multiplies that result by the seasonality number for each specific future month. A future December, if it is also set to 2.0, gets doubled again — this time on top of the growth curve, not instead of it. This is how a product can show a forecast that looks flat or even lower than a recent month: a strong step up in the growth curve landing on a weak seasonal month can still add up to fewer units than a modest curve landing on a strong one.
What it needs to run at all
Growth 20 needs the immediately previous calendar month to have recorded sales. If that month has no sales at all, NeonPanel does not produce an automatic forecast for the product — there is no fallback to an older month or to a company-wide average. A new listing with no completed month of sales, or a product that had a total gap last month, will show no automatic Growth 20 forecast until a completed month of sales exists.

## How seasonality changes the result

Seasonality is not a footnote to Growth 20; it is one of the three numbers multiplied together to get your final forecast, alongside the underlying level and the growth percentage. Because it is applied twice — divided out of the starting month, then multiplied back into every future month — an inaccurate seasonality setting distorts the plan twice over.
If your seasonality pattern is flat (every month set to 1.0), Growth 20 behaves exactly like its curve: a steady climb from about 101% to 121% over the first year with no monthly bumps. If your pattern has real peaks and troughs, those peaks and troughs will dominate what you actually see month to month, and the underlying growth curve becomes a slower-moving current beneath more visible tides.
This is also why two products on the same Growth 20 curve can look completely different in their monthly forecast: one with a flat seasonality pattern will show a smooth ramp, while one with a sharp Q4 peak will show a forecast that jumps in November and December regardless of what the growth curve alone would suggest.

## Things that surprise people

The curve restarts every month, it does not compound on itself. Each month, NeonPanel takes the newest completed month as the new starting point and reapplies the full Growth 20 curve from month one. It does not remember that last month's forecast was, say, 108% of some older baseline. This is why a single unusually strong or weak month can shift the entire two-year plan, not just the month right after it.
The name is a label, not a guarantee. "Growth 20" describes the shape of the curve NeonPanel applies, not a promise that the product will grow exactly 20% in the next twelve months. The realized growth depends on how the actual starting month compares with history, and on how seasonality plays out.
A weak starting month can make Growth 20 look like a decline. If the previous completed month was unusually low — because of a stockout, a listing issue, or a genuine drop in demand — Growth 20 will still apply its curve to that low number. The result can be a forecast that is lower than what you sold two or three months ago, even though the scenario is technically planning for growth from where it started.
Growth 20 does not know about promotions, ad spend, or price changes. If you are planning a deliberate push — a Prime Day event, a new ad campaign, a price drop meant to drive volume — the scenario will not factor that in on its own. You would need to model that separately and, if appropriate, override specific months.
It does not check feasibility. Growth 20 will happily forecast more units than your supplier can produce, more than your warehouse can hold, or more than your cash flow can fund. The scenario answers "what would a 20%-style growth plan look like" — it does not answer "can we actually do this."

## Common questions

Will my product actually grow 20% if I set this scenario?
Not necessarily. The scenario applies a fixed growth curve to your starting month, but the realized year-over-year growth depends on how that starting month compares to your history and on your seasonality pattern. Treat "Growth 20" as the name of the curve, not a forecast guarantee.
Why did my forecast for next month go down after I switched to Growth 20?
This usually means the previous completed month — the one the curve starts from — was low relative to your seasonal expectations, or the target month has a lower seasonality number than the starting month. The growth curve is climbing, but seasonality can still pull a specific month's number down.
Does Growth 20 use my full sales history, or just recent months?
Just the single most recent completed calendar month. That month, with seasonality removed, is the entire starting point for the curve. Longer-run patterns in your sales are not factored in beyond that.
Can I override a specific month if I know something Growth 20 does not?
Yes. You can save a specific value for a specific month, and that saved value takes priority over the automatic Growth 20 number for that product and month in inventory planning.
What happens if last month had zero sales?
Growth 20 needs a completed month with sales to use as its starting point. Without one, there is no automatic forecast for that product until a completed month with sales exists.
How is Growth 20 different from just multiplying my sales by 1.20?
A flat 20% multiplier would apply the same factor to every future month regardless of timing. Growth 20 instead climbs gradually across the year — starting just above 100% and reaching roughly 120–121% by month twelve — and then continues to compound in year two, while seasonality is applied separately on top of that curve for each specific month.
