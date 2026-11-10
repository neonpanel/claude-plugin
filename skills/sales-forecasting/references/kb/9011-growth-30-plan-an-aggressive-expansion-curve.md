# Growth 30: Plan an Aggressive Expansion Curve

Category: Forecast Scenarios · Article 9011 · Updated 2026-08-24

An aggressive plan targeting about 30% growth over the year ahead.

## Introduction

Growth 30 is the steepest of NeonPanel's planned-growth curves. It takes what your product sold in the previous completed month, sets that month's seasonality aside, lifts that underlying level fast along a fixed upward curve, and then puts seasonality back on for each future month. Choose it when the business has approved and funded an expansion well above where the product sells today.
It is the right tool for covering stock on a deliberate, aggressive growth plan. It also carries the most overstock, cash, and long-range exposure of the four planned curves — Flat, Growth 10, Growth 20, and Growth 30 — so treat assigning it as a purchasing decision rather than a reporting preference.

## The short version

| What you want to know | How Growth 30 behaves |
|---|---|
| Where does the plan start? | From the units your product sold in the previous completed month, with that month's seasonality divided out |
| What creates the ramp? | A fixed percentage curve held in your NeonPanel settings. Nothing about it is learned from your sales |
| How steep is it? | The reference curve starts at 102% of that underlying level in the first month and reaches 130% by the twelfth |
| And after the first year? | Roughly 170% of your underlying level by the end of the second year |
| Does "30" mean 30% more every month? | No. The curve rises gradually across the year |
| Is it based on your sales trend? | No |
| Does it know about your expansion plans? | No. People choose the curve. Ad budgets, campaign calendars, promotions, and planned prices are not inputs |
| Does it enforce supply or cash limits? | No |
| Can the plan fall from one month to the next? | Yes. A weak seasonal month can outweigh the step up in the curve |
| What happens every month? | The whole curve restarts from the newest completed month |
| What does it need from your history? | Sales in the immediately previous calendar month. Without them you get no automatic forecast for that product |
| Can you replace individual months? | Yes. A number you save for a month wins for that month in inventory planning |

## How Growth 30 builds your plan

Growth 30 states an intent: plan inventory for a substantial increase over the level your product is selling at now. It does not check whether that intent is achievable. The curve is applied even when last month was an unusual promotional high, the product is nearing saturation, your supply cannot cover the result, cash makes the plan impractical, no marketing or channel expansion is actually planned, or you simply picked the wrong scenario for this product. None of those conditions stop the calculation — they are checks you have to make yourself before trusting the output.
It starts from one month, not a trend
Growth 30 looks at exactly one number to begin: the units your product sold in the previous completed calendar month. It does not average several months and does not measure how fast you have been growing recently. Whatever that one month shows is the entire foundation for the next 24 months of forecast.
Seasonality comes out of that starting month first
Before any growth is applied, NeonPanel divides the starting month's units by your seasonality number for that calendar month, to find the underlying level the growth curve will build from. A product that sold 100 units in a December set to 2.0 — double a normal month — gives an underlying level of 50 units. The same 100 units in a December set to 1.0 stays at 100. In a quiet month set to 0.8, 100 units becomes an underlying 125.
This step matters more for Growth 30 than for a gentler curve, because whatever distortion is baked into the underlying level gets amplified by the steepest growth curve NeonPanel offers.
The curve climbs fast, and it compounds
Once the underlying level is set, NeonPanel multiplies it by a stored percentage for each of the next 24 months. Growth 30's percentages climb from about 102% in the first forecast month to roughly 130% by month twelve, and continue on to about 170% by month twenty-four. The exact monthly values live in configuration rather than in this guide, but the shape is a steady, accelerating climb rather than a straight line — the distance from month eleven to month twelve is larger than the distance from month one to month two.
That acceleration is the point of choosing Growth 30 over Growth 10 or Growth 20: it is meant for a product where the business expects the pace of growth itself to be picking up, not just the raw numbers.
Seasonality goes back on last
After the growth curve is applied to the underlying level, NeonPanel multiplies that result by the seasonality number for each specific future month. A future December set to 2.0 gets doubled again on top of the growth curve, not instead of it. This double effect is why a strong step in the growth curve landing on a weak seasonal month can still add up to fewer forecast units than an earlier, smaller step landing on a strong seasonal month.
What it needs to produce anything at all
Growth 30 requires the immediately previous calendar month to have recorded sales. If there is no sales history for that month — a brand-new listing, or a product with a total gap last month — NeonPanel does not fall back to an older month or to a company average. There is simply no automatic forecast for that product until a completed month with sales exists.

## How seasonality changes the result

Seasonality is not a minor adjustment on top of Growth 30; it is one of three multiplied factors that produce your final number, alongside the underlying level and the growth percentage. Because it is applied twice — divided out of the starting month and multiplied back into every future month — a seasonality setting that is wrong in either direction distorts the plan twice.
With a flat seasonality pattern (every month at 1.0), Growth 30 shows exactly its curve: roughly 102% climbing to 130% across the first year with no monthly bumps. With a real seasonal pattern, those peaks and troughs usually dominate the month-to-month picture, and the aggressive growth curve becomes a fast-moving current running beneath more visible tides.
Because Growth 30's curve is the steepest of the four planned-growth options, seasonality errors compound with more consequence here than on Flat, Growth 10, or Growth 20. A seasonality number set too high on a peak month, combined with a curve already pushing toward 130–170%, can produce a purchase recommendation far beyond what the business can actually use.

## Things that surprise people

The curve restarts every month rather than compounding on a memory of its own forecasts. Each rebuild takes the newest completed month as the new starting point and reapplies the full Growth 30 curve from month one. A single unusually strong or weak month can therefore shift the entire two-year plan, not just the following month.
The name is a label for the curve's shape, not a promise about outcome. "Growth 30" describes roughly where the curve lands by month twelve, not a guarantee that the product will grow exactly 30% over the next year. Realized growth depends on how the actual starting month compares with history and on how seasonality plays out across the months that follow.
A weak starting month can produce a forecast that looks like decline. If the previous completed month was unusually low — a stockout, a suppressed listing, a genuine drop — Growth 30 still applies its steep curve to that low number. The result can land below what the product sold two or three months earlier, even though the scenario is, by design, planning for aggressive growth from wherever it started.
Growth 30 has no knowledge of promotions, ad spend, or pricing plans. A Prime Day push, a new advertising campaign, or a price cut meant to drive volume are not read by the calculation. If you are modeling one of these deliberately, you need to account for it separately and override the affected months.
It does not check feasibility of any kind. Growth 30 will forecast more units than your supplier can produce, more than your warehouse can store, or more than your cash flow can fund, without any warning. The scenario answers "what would an aggressive growth plan look like," not "can the business actually execute this." Because it is the steepest curve available, this gap between forecast and feasibility tends to be largest here.

## Common questions

Will my product actually grow 30% if I use this scenario?
Not necessarily. Growth 30 applies a fixed, steep growth curve to your starting month, but the realized year-over-year change depends on how that starting month compares to your broader history and on your seasonality pattern. Treat "Growth 30" as the name of the curve rather than a forecast guarantee.
Why would I choose Growth 30 over Growth 20?
Growth 30 is meant for products on a confirmed, funded, aggressive expansion plan — not simply for products that recently sold well. If the business has approved a stronger growth trajectory than Growth 20 reflects and can support the resulting inventory and cash commitment, Growth 30 matches that intent more closely.
Why did my forecast go down for a month even though I'm on Growth 30?
This usually means the previous completed month, which the curve starts from, was low relative to seasonal expectations, or the target month carries a lower seasonality number than the starting month. The curve itself is always climbing; seasonality can still pull a specific month's number down relative to another.
Does Growth 30 look at my full sales history?
No. It uses only the single most recent completed calendar month, with seasonality removed, as its entire starting point. Longer-run patterns in your sales history are not factored in beyond that one month.
What happens if I know about a promotion or ad push that should boost sales further?
Growth 30 will not factor that in on its own. You can model the expected effect and save specific values for the affected months, which then take priority over the automatic Growth 30 numbers for those months in inventory planning.
What happens if there were no sales last month?
Growth 30 needs a completed month with sales to use as its starting point. Without one, there is no automatic forecast for that product until a completed month with sales exists — NeonPanel does not substitute an older month or a company average.
How risky is it to leave a product on Growth 30 by default?
More risky than the gentler curves, because it produces the largest gap between forecast and actual demand if the underlying assumption turns out to be wrong. A product left on Growth 30 without an active, funded expansion plan behind it is likely to be over-forecast, which can lead to excess inventory and tied-up cash.
