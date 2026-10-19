# Amazon Listing Audit — Knowledge

## Advertising Analysis

**Clarifying questions**

- Do you have PPC campaign data available?
- What date range for advertising analysis?

**How to help**

- Pull PPC performance data: search term report, campaign structure, ACOS
- Identify high-spend low-conversion terms
- Analyze campaign structure for optimization opportunities
- Save results to phase_results in Supabase

**Don't claim**

- Do not recommend PPC changes without analyzing current performance data first

**Escalate when**

- PPC data is not accessible

## Phase 5 — PPC/Advertising Analysis

**Procedure**

1. Confirm PPC data access with the user.
2. Pull search term report, campaign structure, and ACOS data.
3. Identify high-spend low-conversion terms.
4. Analyze campaign structure for optimization opportunities.
5. Save results to phase_results in Supabase.

## Verified Facts

**Facts**

- The Amazon Listing Audit has 10 phases: Phases 1-6 are data collection, Phases 7-10 are strategic analysis.
- Phase 1 (Product Intelligence): scrape target listing, competitor listings, reviews, and SERP snapshot using BrightData.
- Phase 2 (SQP): pull Search Query Performance data — keyword funnel metrics, click share, conversion share — and identify cannibalization between own ASINs.
- Phase 3 (SCP): pull Search Catalog Performance data, identify underperforming variations, calculate catalog-level KPIs.
- Phase 4 (Competitive Landscape): pull competitor listings, pricing, review data via BrightData; run head-to-head comparison and search term momentum analysis.
- Phase 5 (PPC/Advertising): pull search term report, campaign structure, ACOS; identify high-spend low-conversion terms and optimization opportunities.
- Phase 6 (Cross-sell & Repeat): analyze market basket data, repeat purchase patterns, bundle opportunities, and Subscribe & Save potential.
- Phase 7 (Content Deep Audit): audit title, bullets, description, A+ Content, image stack, video, and Rufus/AI readiness against competitors.
- Phase 8 (Synthesis & Diagnosis): synthesize findings across all phases, apply traffic diagnosis framework, categorize issues by type (content, keyword, competitive, conversion, advertising).
- Phase 9 (Action Plan): score each action using Impact × Ease × Revenue Exposure (max 125), tier actions (Tier 1: 80+, Tier 2: 40-79, Tier 3: <40), apply 30-day decision framework.
- Phase 10 (Baseline Measurement): capture current BSR, keyword rankings, conversion rate, sessions, revenue; define 30-day measurement plan with check-in dates.
- All data collection phases (1-6) must be completed and saved to Supabase before starting strategic analysis phases (7-10).
- Action scoring uses the formula: Impact × Ease × Revenue Exposure, with a maximum score of 125.
- There are 17 critical audit rules that govern how data is interpreted and recommendations are made.
- Signal quality tags must be applied to low-data keywords — do not treat low-confidence signals the same as high-confidence ones.
- Phase results must be saved to Supabase before ending any session — unsaved data is lost.
- Audit data is stored in Supabase tables: audit_runs, phase_results, audit_findings, audit_actions, and audit_baselines.
- The start_audit() function creates a new audit run, and initialize_audit_phases() sets up the phase tracking structure.
- When a Supabase query returns empty results or fails, follow the Supabase Query Fallback Procedure — never retry the same failing query more than twice total.
- Before querying any Supabase table, first call list_tables to confirm the table exists and verify the exact table name.
- If both Supabase query attempts fail, clearly state the specific technical finding and offer concrete alternatives (check different table, check Supabase dashboard, or paste data directly).
- Parallel execution is supported: multiple chats can run different data collection phases simultaneously, but all must save to the same audit_run_id.
- The 30-day decision framework defines check-in points for measuring the impact of implemented audit actions.

## Phase 6 — Cross-sell & Repeat Purchase

### Procedure

1. Analyze market basket data for cross-sell opportunities.
2. Check repeat purchase patterns.
3. Evaluate bundle opportunities (Virtual Bundles, Frequently Bought Together).
4. Assess Subscribe & Save potential.
5. Save results to phase_results in Supabase.

## Supabase Query Fallback

**Clarifying questions**

- What data are you trying to retrieve?
- Do you know which table it should be in?

**How to help**

- Follow the Supabase Query Fallback Procedure: verify table existence with list_tables first
- First query attempt with targeted SQL and explicit columns
- If empty/error: run diagnostic query before retrying
- Second attempt with adjusted query based on diagnostics
- If both fail: clear exit with specific technical finding and alternatives

**Don't claim**

- Do not retry the same failing query more than twice
- Do not say 'let me try again' without explaining what will be different

**Escalate when**

- Both query attempts fail and user needs the data urgently

## Phase 2 — Search Query Performance (SQP)

### Procedure

1. Confirm the user has Brand Analytics access.
2. Pull SQP data: keyword funnel metrics, click share, conversion share.
3. Identify cannibalization between the user's own ASINs.
4. Build the SQP signal dashboard.
5. Save results to phase_results in Supabase.

## Phase 4 — Competitive Landscape

### Procedure

1. Identify top 3-5 competitors with the user.
2. Use BrightData to pull competitor listings, pricing, and review data.
3. Run head-to-head comparison on key metrics.
4. Analyze search term momentum — who's gaining/losing share.
5. Save competitive data to phase_results in Supabase.

## Synthesis Diagnosis

**Clarifying questions**

- Have phases 1-7 been completed?

**How to help**

- Synthesize findings across all phases into a unified diagnosis
- Apply the traffic diagnosis framework to identify root causes
- Categorize issues by type: content, keyword, competitive, conversion, advertising
- Generate the synthesis report with clear cause-effect chains

**Don't claim**

- Do not diagnose without data — every finding must trace to collected evidence

**Escalate when**

- Contradictory signals across phases that can't be resolved

## Baseline Measurement

**Clarifying questions**

- Which metrics are most important for your business?

**How to help**

- Capture current baselines: BSR, keyword rankings, conversion rate, sessions, revenue
- Define the 30-day measurement plan with check-in dates
- Set up tracking for key metrics
- Save baselines to audit_baselines in Supabase

**Don't claim**

- Do not skip baseline capture — it's essential for measuring audit impact

## Phase 8 — Synthesis & Diagnosis

### Procedure

1. Verify phases 1-7 are complete.
2. Synthesize findings across all phases into a unified diagnosis.
3. Apply the traffic diagnosis framework to identify root causes.
4. Categorize issues by type: content, keyword, competitive, conversion, advertising.
5. Generate the synthesis report with clear cause-effect chains.

## Phases 1-6 of the listing audit: Product Intelligence (scrape listing, reviews, competitors, SERP), SQP analysis, SCP analysis, Competitive Landscape, PPC/Advertising analysis, and Cross-sell/Repeat Purchase analysis

**What it is**

Phases 1-6 of the listing audit: Product Intelligence (scrape listing, reviews, competitors, SERP), SQP analysis, SCP analysis, Competitive Landscape, PPC/Advertising analysis, and Cross-sell/Repeat Purchase analysis. Each phase collects raw data and saves to Supabase.

**Key objects**

- Phase 1: Product Intelligence
- Phase 2: SQP
- Phase 3: SCP
- Phase 4: Competitive Landscape
- Phase 5: PPC/Advertising
- Phase 6: Cross-sell & Repeat
- phase_results table
- BrightData scraping

**How to help**

- Run a specific data collection phase
- What tools does each phase use?
- Start a new listing audit
- What data do I need to collect?

**Clarifying questions**

- Analyzing data during collection phases — collect first, analyze later
- Skipping phases and trying to run analysis with incomplete data

## Start a New Listing Audit

**Procedure**

1. Confirm the target ASIN and marketplace with the user.
2. Call `start_audit()` in Supabase to create a new audit_run record.
3. Call `initialize_audit_phases()` to set up phase tracking for all 10 phases.
4. Confirm the audit_run_id with the user for reference.
5. Begin Phase 1: Product Intelligence data collection.

## Competitive Landscape

**Clarifying questions**

- Who are the top 3-5 competitors?
- What primary keywords define your competitive space?

**How to help**

- Use BrightData to pull competitor listings, pricing, and review data
- Run head-to-head comparison on key metrics
- Analyze search term momentum
- Save competitive data to phase_results in Supabase

**Don't claim**

- Do not make competitive claims without data to support them

## Phase 1 — Product Intelligence

### Procedure

1. Use BrightData to scrape the target listing (product details, images, bullets, A+ content).
2. Scrape competitor listings identified from search results (top 3-5).
3. Pull review data for the target ASIN and competitors for sentiment analysis.
4. Run a SERP snapshot for the primary keyword.
5. Save all results to phase_results in Supabase with the audit_run_id.

## Parallel Execution

**Clarifying questions**

- How many parallel chat sessions can you run?
- Which phases do you want to run in parallel?

**How to help**

- Recommend phase groupings for parallel execution
- Ensure all parallel results save to the same audit_run_id in Supabase
- Coordinate: all data collection phases must complete before starting analysis

**Don't claim**

- Do not start analysis phases in parallel with data collection phases

## Product Intelligence

**Clarifying questions**

- What ASIN should I scrape?
- Which marketplace?
- Who are the top 3-5 competitors?

**How to help**

- Use BrightData to scrape the target listing
- Scrape competitor listings
- Pull review data for sentiment analysis
- Run SERP snapshot for the primary keyword
- Save all results to phase_results in Supabase

**Don't claim**

- Do not analyze data during collection — save raw data and move to next phase

**Escalate when**

- BrightData scraping fails for the target ASIN

## Phase 3 — Search Catalog Performance (SCP)

### Procedure

1. Pull Search Catalog Performance data for the target ASIN (and variations if applicable).
2. Identify problem child ASINs (underperforming variations).
3. Calculate catalog-level KPIs.
4. Save results to phase_results in Supabase.

## Phase 9 — Prioritized Action Plan

### Procedure

1. List all recommended actions from the synthesis.
2. Score each action using Impact × Ease × Revenue Exposure (max 125).
3. Tier actions: Tier 1 (score 80+, do immediately), Tier 2 (40-79, schedule soon), Tier 3 (<40, backlog).
4. Apply the 30-day decision framework — assign actions to specific weeks.
5. Present the prioritized action plan with expected outcomes.

## Supabase-backed audit execution: tables (audit_runs, phase_results, audit_findings, audit_actions, audit_baselines), functions (start_audit(), initialize_audit_phases()), and the mandatory Supabase Query Fallback Procedure (2-attempt maximum with diagnostic steps between attempts)

**What it is**

Supabase-backed audit execution: tables (audit_runs, phase_results, audit_findings, audit_actions, audit_baselines), functions (start_audit(), initialize_audit_phases()), and the mandatory Supabase Query Fallback Procedure (2-attempt maximum with diagnostic steps between attempts).

**Key objects**

- audit_runs table
- phase_results table
- audit_findings table
- audit_actions table
- audit_baselines table
- start_audit() function
- initialize_audit_phases() function
- audit_progress view
- Supabase Query Fallback Procedure

**How to help**

- Start a new audit
- Resume a failed audit
- Check audit progress
- Query stored results
- What tables does the audit use?

**Clarifying questions**

- Not saving phase results before ending a session
- Looping through repeated failed queries instead of following the fallback procedure
- Assuming table names without calling list_tables first

## Supabase Query Fallback Procedure

**Procedure**

This procedure is MANDATORY whenever querying Supabase for any audit data.

1. **Verify table existence**: Call `list_tables` to confirm the target table exists (exact name, case-sensitive). If the table does NOT exist, stop immediately and tell the user.
2. **First query attempt**: Write a targeted SQL query with explicit column names (avoid SELECT *). Use LIMIT 10 unless all rows are needed. Execute via `execute_sql`.
3. **If first attempt fails or returns empty**: Run a diagnostic query — `SELECT count(*) FROM table_name;` and `SELECT * FROM table_name LIMIT 5;` to understand why.
4. **Second query attempt**: Based on diagnostic findings, adjust the query (fix column names, broaden WHERE clause, try different filter column). Execute.
5. **If both attempts fail — STOP**: Do not attempt a third query. State the specific technical finding clearly (e.g., "table exists but is empty" or "no rows match criteria"). Offer alternatives: check a different table, check Supabase dashboard directly, or paste the data.

## Resume Failed Audit

**Clarifying questions**

- What's the audit_run_id?
- Which phase did it stop at?

**How to help**

- Check audit_progress view in Supabase
- Identify the last successfully completed phase
- Resume from the next incomplete phase
- If data was lost, re-run only the affected phase

**Don't claim**

- Do not restart the entire audit if only one phase failed

**Escalate when**

- Multiple phases have corrupted or missing data

## Resume a Failed Audit

**Procedure**

1. Get the audit_run_id from the user.
2. Check audit_progress view in Supabase to see completed vs pending phases.
3. Identify the last successfully completed phase.
4. Resume from the next incomplete phase.
5. If data was lost, re-run only the affected phase — do not restart the entire audit.

## Phase 7 — Content Deep Audit

### Procedure

1. Verify all data collection phases (1-6) are complete in Supabase.
2. Audit listing content against competitors: title, bullets, description, A+ Content.
3. Audit image stack: hero image, lifestyle, infographic, video.
4. Assess Rufus/AI readiness — is the listing structured for AI comprehension?
5. Score content gaps and generate the content audit report.
6. Save results to phase_results in Supabase.

## Content Deep Audit

**Clarifying questions**

- Have all data collection phases (1-6) been completed?

**How to help**

- Audit listing content against competitors: title, bullets, description, A+ Content
- Audit image stack: hero, lifestyle, infographic, video
- Assess Rufus/AI readiness
- Score content gaps and generate the content audit report
- Save results to phase_results in Supabase

**Don't claim**

- Do not run content audit without complete data from phases 1-6

**Escalate when**

- Data collection phases are incomplete

## Audit Rules Frameworks

**Clarifying questions**

- Are you looking for the full list of 17 rules or a specific rule?
- Do you need help applying the scoring formula to specific actions?

**How to help**

- Present the relevant audit rules
- Explain the action scoring formula: Impact × Ease × Revenue Exposure (max 125)
- Apply signal quality tags to low-data keywords
- Walk through the 30-day decision framework

**Don't claim**

- Do not ignore signal quality tags on low-data keywords

## Phase 10 — Baseline Measurement

### Procedure

1. Capture current baselines: BSR, keyword rankings, conversion rate, sessions, revenue.
2. Define the 30-day measurement plan with specific check-in dates (Day 7, 14, 21, 30).
3. Set up tracking for key metrics.
4. Save baselines to audit_baselines in Supabase.

## 17 critical audit rules governing data interpretation and recommendations

**What it is**

17 critical audit rules governing data interpretation and recommendations. Action scoring formula: Impact × Ease × Revenue Exposure (max 125). Tiering: Tier 1 (80+, do immediately), Tier 2 (40-79, schedule soon), Tier 3 (<40, backlog). Signal quality tags for low-data keywords. 30-day decision framework with check-in points.

**Key objects**

- 17 critical rules
- Action scoring (max 125)
- Tier 1/2/3 classification
- Signal quality tags
- 30-day decision matrix
- Revenue exposure weighting

**How to help**

- What are the audit rules?
- How do I score actions?
- What tier is this action?
- How do I handle low-data keywords?

**Clarifying questions**

- Ignoring signal quality tags on low-data keywords
- Treating all signals with equal confidence regardless of data volume

## Brand Analytics Sqp

**Clarifying questions**

- Do you have access to Brand Analytics / Search Query Performance data?
- What date range should I analyze?

**How to help**

- Pull SQP data: keyword funnel metrics, click share, conversion share
- Identify cannibalization between own ASINs
- Build the SQP signal dashboard
- Save results to phase_results in Supabase

**Don't claim**

- Do not fabricate SQP metrics — only report data actually retrieved

**Escalate when**

- Brand Analytics access is not available

## Start Audit

**Clarifying questions**

- What ASIN do you want to audit?
- Which marketplace (US, UK, DE, etc.)?
- Do you have an existing audit run to resume, or is this new?

**How to help**

- Call start_audit() in Supabase to create a new audit_run
- Initialize phases with initialize_audit_phases()
- Begin Phase 1: Product Intelligence data collection

**Don't claim**

- Do not skip the audit initialization step
- Do not start analysis phases before completing all data collection phases

**Escalate when**

- Supabase connection fails during audit initialization

## Phases 7-10 of the listing audit: Content Deep Audit (title, bullets, A+, images, video, Rufus/AI readiness), Synthesis & Diagnosis (cross-phase findings, traffic diagnosis framework), Prioritized Action Plan (Impact × Ease × Revenue Exposure scoring, tiering), and Baseline Measurement (BSR, rankings, conversion, sessions, revenue tracking)

**What it is**

Phases 7-10 of the listing audit: Content Deep Audit (title, bullets, A+, images, video, Rufus/AI readiness), Synthesis & Diagnosis (cross-phase findings, traffic diagnosis framework), Prioritized Action Plan (Impact × Ease × Revenue Exposure scoring, tiering), and Baseline Measurement (BSR, rankings, conversion, sessions, revenue tracking).

**Key objects**

- Phase 7: Content Deep Audit
- Phase 8: Synthesis & Diagnosis
- Phase 9: Action Plan
- Phase 10: Baseline Measurement
- Traffic diagnosis framework
- Action scoring formula
- 30-day decision framework

**How to help**

- Run strategic analysis on collected data
- Generate an action plan
- What's the diagnosis framework?
- How do I score and prioritize actions?

**Clarifying questions**

- Running analysis without complete data collection from phases 1-6
- Skipping synthesis and jumping straight to action plan

## Cross Sell Repeat

**Clarifying questions**

- Is the product eligible for Subscribe & Save?
- Do you sell complementary products?

**How to help**

- Analyze market basket data for cross-sell opportunities
- Check repeat purchase patterns
- Evaluate bundle opportunities
- Assess Subscribe & Save potential
- Save results to phase_results in Supabase

**Don't claim**

- Do not confuse Subscribe & Save with Subscribe with Amazon

## Action Plan

**Clarifying questions**

- What's your timeline for implementing changes?
- Are there budget constraints?

**How to help**

- Score each action using Impact × Ease × Revenue Exposure (max 125)
- Tier actions: Tier 1 (80+), Tier 2 (40-79), Tier 3 (<40)
- Apply the 30-day decision framework
- Present the prioritized action plan with expected outcomes

**Don't claim**

- Do not guarantee specific revenue or ranking outcomes from actions

## Catalog Performance Scp

**Clarifying questions**

- Is this a parent ASIN with child variations?
- What date range for SCP data?

**How to help**

- Pull Search Catalog Performance data
- Identify problem child ASINs
- Calculate catalog-level KPIs
- Save results to phase_results in Supabase

**Don't claim**

- Do not skip SCP analysis for products with variations
