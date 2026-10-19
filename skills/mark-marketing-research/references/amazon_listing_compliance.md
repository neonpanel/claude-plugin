# Amazon Listing Compliance — Knowledge

## Generating Compliant Listing Copy from Scratch

**Procedure**

1. Confirm the target marketplace (US, UK, EU, etc.) to determine unit and formatting standards.
2. **Run the Tools-First Data Retrieval playbook** — pull all available product data, competitor info, and market data from NeonPanel and BrightData BEFORE asking the user any questions.
3. Gather ONLY genuinely missing info from the user — typically limited to: target positioning, preferred tone, specific marketing angles, or specs not available in tools.
4. Draft the title: Brand + Key Feature + Product Type + Size/Quantity. No promotional language.
5. Draft bullet points using sentence case or title case for soft headers. Focus on benefits and features.
6. **Run the Prohibited Content Quick-Check** — scan draft for any prices, reviews, or cross-product references. Remove all violations before proceeding.
7. For any measurements, run the **Unit Conversion Validation** playbook to verify all conversions.
8. Run the full **Pre-Submission Compliance Checklist** against the draft.
9. Fix any violations found.
10. Present the validated copy to the user with a note confirming compliance checks passed and unit conversions verified.

## Unit Conversions

**Clarifying questions**

- Which unit types are you converting (weight, volume, dimensions)?
- Which marketplace are you targeting — this affects whether metric or imperial is primary?

**How to help**

- Provide the correct unit conversion for the requested values
- Specify which unit format is required for the target marketplace
- Validate that converted values are displayed in the correct Amazon format

**Don't claim**

- Do not round conversions in ways that misrepresent the product's actual measurements
- Do not claim that unit display requirements are the same across all Amazon marketplaces
- Do not provide conversions for units Amazon does not support in listing fields

**Escalate when**

- Conversion discrepancy could lead to customer complaints or returns (e.g., significant size/weight difference)

## Tools-First Data Retrieval (MANDATORY — Run Before Asking Any Questions)

### Procedure

Before composing ANY question to the user, run through this checklist to determine if the data can be retrieved via tools instead.

### Step 1: Identify What You Need
- List every data point required to complete the user's request (e.g., product dimensions, PPC data, competitor listings, return reasons, variation details, inventory levels).

### Step 2: Check Tool Availability for Each Data Point

| Data needed | Tool to use | Ask user? |
|---|---|---|
| Product details, specs, dimensions | NeonPanel `getInventoryDetails` | NO — pull from tool |
| Inventory levels, warehouse data | NeonPanel `listInventoryItems`, `listWarehouses` | NO — pull from tool |
| COGS, unit costs, landed costs | NeonPanel `getInventoryCogs`, `getInventoryLandedCost` | NO — pull from tool |
| Competitor listings, pricing, reviews | BrightData product scraping | NO — pull from tool |
| Competitor bullet points, titles | BrightData product scraping | NO — pull from tool |
| Search volume, keyword data | Brand Analytics / available tools | NO — pull from tool |
| Return reasons, customer complaints | NeonPanel reports if available | Check tool first, ask only if unavailable |
| PPC performance data | NeonPanel reports if available | Check tool first, ask only if unavailable |
| Internal business strategy or goals | No tool available | YES — ask the user |
| Future product plans or unreleased items | No tool available | YES — ask the user |
| Subjective preferences (tone, angle, positioning) | No tool available | YES — ask the user |
| Brand guidelines or voice requirements | No tool available | YES — ask the user |
| Cost targets or margin goals not in NeonPanel | No tool available | YES — ask the user |

### Step 3: Retrieve First, Then Ask
1. Call all available tools in parallel to gather retrievable data.
2. Review what you received — identify any genuine gaps.
3. Only THEN ask the user for the remaining items (should typically be 1-3 questions, not 14).

### Step 4: Self-Check
- If you are about to ask more than 3 questions, STOP.
- Re-read your question list and cross-check each item against the tool availability table above.
- Remove any question where a tool could provide the answer.
- Proceed only with genuinely un-retrievable items.

## The self-validation checklist the agent must run against all generated listing copy before presenting it to the user

**What it is**

The self-validation checklist the agent must run against all generated listing copy before presenting it to the user. Covers every known compliance rule including a mandatory unit conversion verification step to catch errors before the user sees them.

**Key objects**

- Compliance checklist
- Pre-submission review
- Violation detection
- Copy validation steps
- Listing quality gate
- Unit conversion verification gate

**How to help**

- Check my listing copy for compliance
- Validate these bullet points
- Does this title meet Amazon requirements?
- Review my listing before submission
- Are my measurements accurate?

**Clarifying questions**

- Skipping validation because the copy 'looks fine'
- Only checking some rules instead of the full checklist
- Assuming compliance rules are suggestions rather than requirements
- Not verifying unit conversions because the numbers 'seem right'

## Formatting Rules

**Clarifying questions**

- Which listing field are you formatting — title, bullets, or description?
- Are you listing on a specific Amazon marketplace (US, UK, EU)?
- Is this for a new listing or updating an existing one?

**How to help**

- Provide the formatting rules for the specific field requested
- Highlight character limits and restricted characters
- Show examples of compliant vs non-compliant formatting

**Don't claim**

- Do not claim formatting rules are the same across all marketplaces
- Do not invent character limits not documented in Amazon's style guides

**Escalate when**

- User reports that a correctly formatted listing was still rejected by Amazon

## Handling Compliance Violations Found During Review

**Procedure**

1. Identify the specific violation and which rule it breaks.
2. Flag it clearly to the user with the original text and the rule violated.
3. Provide a corrected alternative that preserves the marketing intent while being compliant.
4. If multiple violations are found, list all of them — do not fix one and miss others.
5. After corrections, re-run the full checklist to confirm no new violations were introduced.

## Unit Conversion Validation

**Clarifying questions**

- Which listing fields contain unit values you'd like validated?
- Are you checking conversions between specific unit systems (metric ↔ imperial)?
- Is this for a single product or a bulk validation across multiple listings?

**How to help**

- Validate that unit conversions in the listing are mathematically accurate
- Check that units are displayed in the format required by the target marketplace
- Flag any rounding errors or inconsistencies between related fields
- Confirm that dual-unit displays (if used) are consistent

**Don't claim**

- Do not approve unit values without verifying the conversion math
- Do not claim that approximate conversions are acceptable when Amazon requires exact values
- Do not validate units for product categories you don't have rules for
- Do not assume the source unit value is correct — flag if it seems implausible

**Escalate when**

- Unit errors could cause safety issues (e.g., medication dosage, chemical concentrations)
- Bulk validation reveals systematic conversion errors across many listings
- User disputes a conversion that appears mathematically correct

**Procedure**

Run this procedure for EVERY unit conversion that appears in listing copy. Never skip this step.

### Reference Conversion Factors

| Conversion | Factor | Formula |
|---|---|---|
| Inches → Millimeters | 1 in = 25.4 mm | inches × 25.4 = mm |
| Millimeters → Inches | 1 mm = 0.03937 in | mm ÷ 25.4 = inches |
| Feet → Millimeters | 1 ft = 304.8 mm | feet × 304.8 = mm |
| Ounces (fluid) → Milliliters | 1 fl oz = 29.5735 ml | fl oz × 29.5735 = ml |
| Milliliters → Fluid Ounces | 1 ml = 0.03381 fl oz | ml ÷ 29.5735 = fl oz |
| Pounds → Kilograms | 1 lb = 0.4536 kg | lbs × 0.4536 = kg |
| Kilograms → Pounds | 1 kg = 2.2046 lbs | kg × 2.2046 = lbs |
| Ounces (weight) → Grams | 1 oz = 28.3495 g | oz × 28.3495 = g |
| Grams → Ounces | 1 g = 0.03527 oz | g ÷ 28.3495 = oz |

### Known Error Patterns

- ✘ 0.2 inches = 2 mm → ✓ 0.2 inches = 5.08 mm (0.2 × 25.4 = 5.08)
- ✘ 2 mm = 0.2 inches → ✓ 2 mm = 0.079 inches (2 ÷ 25.4 = 0.0787)
- Watch for "convenient" round numbers — if a conversion looks suspiciously clean, recalculate.

### Validation Steps

1. **Identify** every measurement in the copy that includes a unit conversion (e.g., "X inches (Y mm)").
2. **Look up** the correct conversion factor from the reference table above.
3. **Calculate** using the exact formula. Show your work: e.g., 0.2 in × 25.4 = 5.08 mm.
4. **Reverse-check** — convert the result back to the original unit to confirm: 5.08 mm ÷ 25.4 = 0.2 in ✓.
5. **Compare** your calculated value against what's in the copy. If they don't match, correct it.
6. **Flag suspiciously round numbers** — if a converted value is a clean round number (e.g., 2mm, 5mm, 10mm), double-check it. Real conversions are rarely perfectly round.
7. **Round appropriately** — round to 2 decimal places for inches, whole numbers for mm. Never round in a way that changes the measurement by more than 1%.
8. **Document** — when presenting the copy, note that unit conversions have been verified.

## Prohibited Content

**Clarifying questions**

- What type of content are you unsure about — health claims, competitor mentions, or something else?
- Which product category does this apply to?

**How to help**

- Identify whether the content falls under Amazon's prohibited content rules
- Explain which specific policy the content violates
- Suggest compliant alternative phrasing
- Flag any content that requires substantiation or certification

**Don't claim**

- Do not provide an exhaustive list of all prohibited content — Amazon's policies are extensive and change
- Do not advise ways to disguise prohibited claims
- Do not claim that content allowed on a seller's own website is automatically allowed on Amazon
- Do not approve pesticide, drug, or medical device claims without proper EPA/FDA substantiation

**Escalate when**

- Content involves potential legal liability (health/safety claims, regulated substances)

## Verified Facts

**Facts**

- ALL CAPS text is prohibited in Amazon bullet points — even for headers or emphasis. Use sentence case or title case instead.
- Price mentions are prohibited in Amazon bullet points because prices vary by seller, promotions, and time. Never include specific prices like '$0.08/pad' in listing copy.
- Review counts and star ratings must never appear in Amazon listing copy. Values like '4.6-star rating from 2,600+ reviews' are dynamic data that cannot be embedded in static listing content.
- Cross-product references are prohibited in Amazon bullet points. Do not reference other ASINs, competitor products, or other items in the seller's catalog within a single listing's copy.
- Promotional claims such as 'Save 10%', 'Best Deal', 'Limited Time Offer', or 'Save X% vs Buying Separately' are prohibited in Amazon product titles.
- Amazon product titles must not contain promotional language, subjective claims, or pricing information.
- Unit conversions in listing copy must be mathematically accurate. For example, 2mm equals approximately 0.079 inches, NOT 0.2 inches. Always verify conversions before including them.
- US Amazon marketplace listings should use imperial units (inches, pounds, ounces, Fahrenheit) as the primary measurement system. Metric equivalents may be included in parentheses.
- Violating Amazon Content Policy rules can result in listing suppression, meaning the product becomes unsearchable and unbuyable until the violation is corrected.
- Every piece of generated listing copy must be validated against the full Amazon compliance checklist before being presented to the user.
- Amazon bullet points should use sentence case or title case for the first few words as a soft header, followed by the descriptive content — never use ALL CAPS words as bullet headers.
- Do not include time-sensitive or variable information in listing copy — this includes prices, stock levels, review counts, ratings, and promotional offers.
- Factual product claims in listing copy must be verifiable and accurate. Do not exaggerate dimensions, quantities, or performance metrics.
- Common unit conversion reference: 1 inch = 25.4 mm | 1 mm = 0.03937 inches | 1 foot = 304.8 mm | 1 oz = 29.5735 ml | 1 fl oz = 29.5735 ml | 1 ml = 0.03381 fl oz | 1 lb = 0.4536 kg | 1 kg = 2.2046 lbs | 1 oz (weight) = 28.3495 g | 1 g = 0.03527 oz.
- Every unit conversion in listing copy must be independently calculated and verified before output — never estimate, round prematurely, or rely on memory. Use the formula: mm ÷ 25.4 = inches, inches × 25.4 = mm, oz × 29.5735 = ml, lbs × 0.4536 = kg.
- 0.2 inches = 5.08 mm (NOT 2 mm). This is a known error pattern — always multiply inches by 25.4 to get mm, or divide mm by 25.4 to get inches.
- Per-unit pricing (e.g., '$0.08/pad', '$0.05/pad', 'only $X per piece') is prohibited in Amazon listing copy. Prices are variable and must never appear in static bullet points or descriptions.
- Specific review metrics (e.g., '4.6-star rating', '2,600+ reviews', 'thousands of 5-star reviews') are prohibited in Amazon listing copy. Review data is dynamic and controlled by Amazon — it cannot be embedded in seller-authored content.
- Cross-referencing other products in the seller's catalog (e.g., 'use our Felt Pads for heavy furniture', 'pair with our X product') is prohibited in Amazon bullet points. Each listing must describe only the product being sold.
- The agent must run a Prohibited Content Quick-Check on all generated listing copy before presenting it to the user, scanning for: (1) any prices or per-unit costs, (2) any review counts or star ratings, (3) any cross-product references. All matches must be removed before output.
- Promotional language is strictly prohibited in Amazon product titles. This includes but is not limited to: discount percentages ('Save 10%', '20% Off'), savings comparisons ('Save X% vs Buying Separately', 'Better Value Than'), superlative value claims ('Best Value', 'Best Deal', 'Lowest Price', 'Bargain', 'Great Savings'), and urgency or time-limited language ('Limited Time', 'Special Offer', 'Act Now', 'While Supplies Last', 'Exclusive Deal'). This applies to all listing types including Virtual Bundles.
- The agent must always attempt to retrieve data using available MCP tools (NeonPanel, BrightData) before asking the user for information. Users should only be asked for data that genuinely cannot be retrieved via tools, such as internal business decisions, strategy preferences, or data not tracked in connected systems.

## Listing Copy Compliance

**Clarifying questions**

- Which marketplace is this listing for (US, UK, EU, etc.)?
- What product category does this listing belong to?
- Are you writing a new listing or reviewing an existing one for compliance?

**How to help**

- Review the listing copy against Amazon's content policy rules
- Check for prohibited terms, unsupported claims, and restricted language
- Validate title, bullet points, and description against marketplace-specific guidelines
- Flag any compliance violations with specific rule references
- Suggest compliant alternatives for any flagged content

**Don't claim**

- Do not guarantee that a listing will pass Amazon's automated review
- Do not claim compliance rules are identical across all Amazon marketplaces
- Do not approve health, safety, or efficacy claims without proper substantiation
- Do not advise using competitor brand names in listing copy
- Do not suggest circumventing Amazon's content policies
- Do not claim that compliant copy guarantees listing approval — Amazon may have additional automated checks
- Do not provide legal advice on product claims or certifications
- Do not assert that formatting rules never change — Amazon updates policies periodically
- Do not recommend keyword stuffing as a valid optimization strategy
- Do not claim that all special characters are prohibited — some are allowed in specific contexts

**Escalate when**

- Listing involves regulated product categories (supplements, medical devices, pesticides)
- User needs legal guidance on product claims or certifications
- Listing was rejected by Amazon and the reason is unclear

## Ensuring factual accuracy in listing copy for the target marketplace — correct unit conversions (metric vs imperial), marketplace-appropriate measurements, and avoiding cross-product references

**What it is**

Ensuring factual accuracy in listing copy for the target marketplace — correct unit conversions (metric vs imperial), marketplace-appropriate measurements, and avoiding cross-product references. Includes a mandatory unit conversion validation step with reference conversion factors and reverse-check verification to prevent errors like stating 0.2 inches = 2mm.

**Key objects**

- Unit conversion reference table
- Conversion validation step
- Reverse-check verification
- US marketplace standards (inches, ounces, Fahrenheit)
- Metric to imperial conversion factors (1 in = 25.4 mm, 1 oz = 29.5735 ml, 1 lb = 0.4536 kg)
- Cross-product reference policy
- Factual accuracy requirements
- Known error patterns (e.g., 0.2 in ≠ 2 mm)

**How to help**

- What units should I use for US Amazon listings?
- How do I convert mm to inches correctly?
- Can I reference other products in my listing?
- What measurement format does Amazon US expect?
- Is this conversion correct?
- Validate my product dimensions

**Clarifying questions**

- Incorrectly converting metric to imperial (e.g., claiming 0.2 inch = 2mm when 0.2 inch = 5.08mm)
- Relying on memory or estimation instead of calculating conversions with exact factors
- Rounding conversions too aggressively, changing the actual measurement
- Using metric units on US marketplace listings where imperial is standard
- Referencing competitor products or other ASINs in bullet copy
- Assuming product specs from one marketplace apply to another

## Amazon's Content Policy rules that govern what can and cannot appear in product listing copy — titles, bullet points, and descriptions

**What it is**

Amazon's Content Policy rules that govern what can and cannot appear in product listing copy — titles, bullet points, and descriptions. Covers prohibited content types such as pricing, reviews, promotional claims, ALL CAPS formatting, and cross-product references.

**Key objects**

- Bullet point formatting rules
- Title formatting rules
- Prohibited content types
- Amazon Content Policy
- Listing suppression triggers

**How to help**

- What formatting rules apply to Amazon bullets?
- Can I mention price in bullet points?
- What gets a listing suppressed?
- Are ALL CAPS allowed in bullets?
- Can I reference reviews or ratings in copy?

**Clarifying questions**

- Thinking ALL CAPS headers in bullets are acceptable for emphasis — they violate Amazon policy
- Including price mentions in bullets — prices differ by seller and change over time
- Referencing star ratings or review counts — these are dynamic and not allowed in static copy
- Adding promotional claims like 'Save 10%' in titles — Amazon prohibits promotional language in titles

## Prohibited Content Quick-Check (Mandatory Pre-Output Gate)

### Procedure

Run this check on ALL generated listing copy BEFORE presenting it to the user. This is a mandatory gate — copy must not be shown until it passes.

### 1. Price Scan
- Search for: dollar signs ($), cent symbols (¢), "cost per", "per unit", "per pad", "per piece", "per count", "only $", "just $", "for $", any specific monetary value
- If found: Remove the entire price reference. Replace with benefit-focused language that doesn't mention cost.
- Examples of violations: "$0.08/pad", "pack is $0.05/pad", "only $12.99", "less than $1 per piece"
- Correct approach: Focus on value proposition without specific numbers (e.g., "economical bulk pack" instead of "$0.05/pad")

### 2. Review & Rating Scan
- Search for: star ratings (any number + "star"), review counts (any number + "reviews"), "rated", "rating", "customer favorite", "best seller", "top rated", "highly rated", "thousands of reviews"
- If found: Remove the entire review/rating reference. Do not rephrase — remove completely.
- Examples of violations: "4.6-star rating from 2,600+ reviews", "thousands of 5-star reviews", "top-rated product"
- Correct approach: Describe product qualities directly without referencing customer feedback metrics

### 3. Cross-Product Reference Scan
- Search for: "use our", "pair with our", "try our", "see our", "our [other product name]", "works great with our", references to other ASINs, other product names from the seller's catalog
- If found: Remove the cross-reference entirely. Each listing must stand alone.
- Examples of violations: "use our Felt Pads for heavy furniture", "pair with our furniture sliders", "see our full line of floor protectors"
- Correct approach: Describe only the product being listed. Do not reference the seller's other products.

### 4. Final Confirmation
- After removing all violations, re-read the copy to ensure it still makes sense and flows naturally.
- Confirm: "Prohibited content check passed — no prices, reviews, or cross-product references found."

## Pre-Submission Compliance Checklist

**Procedure**

Run this checklist against ALL generated listing copy before presenting it to the user. Every item must pass.

1. **Prohibited Content Quick-Check (MANDATORY FIRST STEP)** — Run the Prohibited Content Quick-Check playbook above. Copy must pass all three scans (price, review/rating, cross-product) before proceeding.
2. **No ALL CAPS** — Scan every bullet point for fully capitalized words used as headers or emphasis. Replace with sentence case or title case. Example: ✘ "ULTRA THIN DESIGN" → ✓ "Ultra Thin Design" or "Ultra thin design — ..."
3. **No price mentions** — Confirmed by Quick-Check step 1 above. Double-check: search for dollar signs ($), cent symbols (¢), and phrases like "cost per", "only $", "just $", "per unit", "per pad", or any specific monetary value.
4. **No review or rating references** — Confirmed by Quick-Check step 2 above. Double-check: search for star ratings, review counts, or phrases like "rated", "reviews", "stars", "customer favorite".
5. **No cross-product references** — Confirmed by Quick-Check step 3 above. Double-check: check for mentions of other products, ASINs, competitor names, or phrases like "use our", "pair with", "compared to", "unlike other", "vs", "better than".
6. **No promotional claims in titles** — Check the title for discount language, savings claims, urgency language, or subjective superlatives. Example: ✘ "Save 10% vs Buying Separately" → ✓ Remove from title entirely.
7. **Verify all unit conversions (MANDATORY)** — For every measurement that includes a conversion, calculate it using exact factors. Do NOT rely on memory. Use the Unit Conversion Validation playbook below. Example: ✘ "0.2 inch (2mm)" [WRONG: 0.2 × 25.4 = 5.08mm] → ✓ "0.2 inches (5.08mm)" or "2mm (0.08 in)".
8. **Correct units for marketplace** — US listings use inches, pounds, ounces, Fahrenheit as primary units. Metric may appear in parentheses.
9. **No time-sensitive or variable data** — Check for stock levels, shipping times, seasonal references, or any data that changes over time.
10. **Factual accuracy** — Verify that all product claims (dimensions, quantities, materials, performance) are accurate and match the provided specs.
11. **Title format** — Confirm the title contains: Brand + Key Feature + Product Type + Size/Quantity. No promotional language, no ALL CAPS (except brand names that are officially capitalized).

## Compliance Check

**Clarifying questions**

- Would you like me to check a specific listing field (title, bullets, description) or the full listing?
- Do you have the ASIN or product category for context?

**How to help**

- Run a compliance check against known Amazon content policy rules
- Identify any prohibited content, unsupported claims, or formatting violations
- Provide a pass/fail summary with specific issues highlighted
- Suggest corrective actions for each violation found

**Don't claim**

- Do not guarantee a clean compliance check means Amazon will approve the listing
- Do not claim the check covers every possible Amazon policy — it covers known documented rules

**Escalate when**

- Multiple serious violations found that suggest the listing needs a full rewrite
