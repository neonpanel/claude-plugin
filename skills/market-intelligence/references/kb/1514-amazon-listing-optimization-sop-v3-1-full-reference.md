# Amazon Listing Optimization SOP v3.1 — Full Reference

Category: Amazon Listing Optimization · Article 1514 · Updated 2026-07-21

Complete Amazon Listing Optimization SOP v3.1 — two-stage architecture (Stage 1 data collection + Stage 2 strategic analysis), 23 phases, NeonPanel + BrightData MCP, Supabase DB-backed execution. Covers execution architecture, all phase specifications with tool calls and structured_data schemas, critical rules, cost estimates, and decision frameworks.

## Execution Architecture — Two-Stage Model

This SOP is executed in **two stages** with a **human checkpoint** in between.

| Stage | Model tier | What It Does | Phases |
|---|---|---|---|
| **Stage 1 — Data collection** | Cheap (Haiku/Flash/4o-mini) | Data collection: scrapes, analytics pulls, tool calls → writes to DB | 1–6 (16 sub-phases) |
| **Stage 2 — Strategic analysis** | Expensive (Sonnet/Opus) | Strategic analysis: reads DB, synthesizes, diagnoses, writes actions | 7–10 (7 sub-phases) |

Both stages share the same Supabase database as working memory. Both read this SOP from `sop_versions WHERE is_active = true`.

### How It Works

```
YOU → Start Stage 1 (data collection, cheap model tier)
YOU: "Run audit for [ASIN], company [ID], marketplace [US]"

STAGE 1:
  1. ⚠️ COST GATE: Confirm cheap model
  2. Reads SOP from DB
  3. Calls start_audit() → gets audit_run_id
  4. Runs phases 1a → 6b (sequential, ~33-55 tool calls, ~25-40 min)
  5. Writes each phase to phase_results
  6. STOPS at the checkpoint and delivers the checkpoint message

YOU → Start Stage 2 (strategic analysis, expensive model tier)
YOU: "Execute phases 7-10 for audit_run_id = [X]"

STAGE 2:
  1. Reads all phase_results from DB
  2. Verifies all 16 Stage 1 phases = completed
  3. Loads QA insights (carry_to_final = true)
  4. Runs phases 7a → 10
  5. Writes findings, actions, baselines
  6. Marks audit as completed
```

### Why This Works
1. **The database IS the checkpoint.** Stage 1 writes. Stage 2 reads.
2. **Cost-optimal.** ~80% of tool calls on cheap model (10-20x less).
3. **Failure-safe.** Completed phases are saved. Restart picks up from next incomplete.
4. **Human in the loop.** YOU decide when to switch.

### Cost Estimate
| Component | Model | Est. Cost |
|---|---|---|
| Stage 1 (16 phases, ~33-55 tool calls) | Cheap | ~$0.30–0.80 |
| Stage 2 (7 phases, ~0-5 tool calls) | Expensive | ~$2.00–4.00 |
| **TOTAL** | | **~$2.30–4.80** |

vs. v2.x single-stage execution on an expensive model: ~$8–15 → **65-75% savings**

## Parallel Execution Option

Stage 1 can be launched in **multiple parallel chats** for speed:

| Chat | Assigned Phases | Est. Time |
|---|---|---|
| Chat 1 | 1a, 1b, 1c, 1d | ~15 min |
| Chat 2 | 2a, 2b, 2c, 3a, 3b | ~12 min |
| Chat 3 | 4a, 4b | ~5 min |
| Chat 4 | 5a, 5b | ~5 min |
| Chat 5 (after Chat 1 finishes 1a) | 1e, 6a, 6b | ~8 min |

Each chat only writes to its assigned phases. When ALL chats report done, start Stage 2.

For most audits, **sequential (1 chat) is fine** — the ~15-minute time saving rarely justifies managing 5 windows.

## Database Schema

**Supabase Project:** 5SU NP (wdhyodubtxgsdeopmlkg) | Region: us-east-2

### Tables
| Table | Purpose | Written By |
|---|---|---|
| `sop_versions` | This SOP document, versioned | Human / Stage 2 |
| `audit_runs` | Each audit execution with params, status, schedule | Stage 1 (create), Stage 2 (complete) |
| `phase_results` | **Shared working memory.** Each phase saves here. | Stage 1 (1-6), Stage 2 (7-10) |
| `audit_findings` | Final per-child diagnostic findings | Stage 2 |
| `audit_actions` | Final prioritized action items with scoring | Stage 2 |
| `audit_baselines` | Pre/post metric snapshots for 30-day measurement | Stage 2 |
| `audit_qa_insights` | Mid-audit Q&A captures with carry_to_final flag | Stage 2 |
| `sop_pending_updates` | Operational findings queued for next SOP version | Stage 2 |

### Key Functions & Views
| Name | Type | Purpose |
|---|---|---|
| `start_audit(...)` | Function | Creates audit_run + initializes all 23 phase rows |
| `initialize_audit_phases(audit_run_id)` | Function | Called by start_audit |
| `audit_progress` | View | Dashboard: completion %, next phase, findings count |
| `phase_details` | View | Per-phase status, timing, summary preview |
| `audits_due_for_iteration` | View | Audits due or overdue for 30-day re-run |

## Stage 1 Protocol (Data Collection)

### STEP 0 — Cost Gate
The FIRST thing said in EVERY new Stage 1 session:

> ⚠️ **COST CHECK** — This audit involves ~33-55 tool calls of pure data collection. I should be running on a **cheap model** (Haiku, 4o-mini, Flash). Using an expensive model wastes ~$5-10 with zero quality benefit. **Are you running me on a cheap model?** Type OK to proceed.

**Do NOT proceed until confirmed.**

### STEP 1 — Session Start
1. Read active SOP: `SELECT content FROM sop_versions WHERE is_active = true;`
2. Check due iterations: `SELECT * FROM audits_due_for_iteration;`
3. If continuing: `SELECT * FROM audit_progress WHERE audit_run_id = [ID];` Resume from next incomplete phase.

### STEP 2 — Create or Continue Audit
```sql
SELECT * FROM start_audit(
  p_neon_company_id := [ID],
  p_marketplace := '[MKT]',
  p_parent_asin := '[ASIN]',
  p_brand_name := '[BRAND]',
  p_child_asins := '[...]'::jsonb,
  p_competitor_asins := '[...]'::jsonb,
  p_core_keywords := '[...]'::jsonb
);
```

### STEP 3 — Execute Phases 1-6
For each phase:
1. Update status to 'in_progress'
2. Call specified tool(s)
3. Extract data per structured_data schema
4. Write to phase_results (status, timing, tools_used, summary, structured_data, key_findings)
5. On failure: record error, increment retry_count, move to next phase

### STEP 4 — Checkpoint
Deliver the exact checkpoint message with audit_run_id. **STOP. Do NOT attempt phases 7-10.**

### Stage 1 Rules
1. Extract and save. Do NOT analyze or interpret.
2. Follow the phase recipe exactly.
3. Capture EVERYTHING.
4. Flag anomalies, don't resolve them.
5. Never attempt phases 7-10.
6. RULE_VARIATION_PARENTING_ACCURACY: Only act on child ASINs from audit_run.
7. RULE_INSUFFICIENT_SIGNAL: Tag signal_quality on all keyword metrics.

## Stage 2 Protocol (Strategic Analysis)

### Trigger
User sends: "Execute phases 7-10 for audit_run_id = [X]"

### Session Start Protocol
1. Check due iterations: `SELECT * FROM audits_due_for_iteration;`
2. Check progress: `SELECT * FROM audit_progress WHERE audit_run_id = [ID];`
3. Verify all Stage 1 phases completed. If any failed → STOP, advise re-run.
4. Load all Stage 1 output from phase_results.
5. Load carry-forward QA insights (carry_to_final = true).
6. Load pending SOP updates.

### RULE_MID_AUDIT_QA_CAPTURE
Whenever the user asks a question and Stage 2 responds with strategic guidance:
```sql
INSERT INTO audit_qa_insights (audit_run_id, phase_code, question_summary, answer_summary, actionable_takeaway, carry_to_final)
VALUES ([ID], '[current_phase]', '...', '...', '...', true/false);
```

## Prerequisites

| Item | Required? | Notes |
|---|---|---|
| Company ID | ✅ Required | Numeric ID from NeonPanel |
| Marketplace | ✅ Required | Country code: US, UK, DE, etc. |
| Parent ASIN | ✅ Required | Or the main listing URL |
| Child ASINs | Preferred | If not provided, Stage 1 discovers from scrape |
| Brand Name | ✅ Required | For SQP/SCP brand filtering |
| Top 3-5 Competitor ASINs | Preferred | If not provided, auto-detect from Brand Analytics |
| Core keyword themes | Preferred | 5-10 main search terms |
| Known pain points | Optional | Anything client suspects is wrong |

## Phase 1: Product Intelligence Gathering

### Phase 1A — Listing Deep Scrape
**Code:** `1a_listing_scrape` | **Tool:** `web_data_amazon_product` (per child) | **Calls:** 3-8

Extract per child: Title (text + char count + keyword placement), Bullets (all 5), Description/A+, Price (current + was-price + deals), Rating + review count, BSR + category, Variation attributes, Fulfillment method, Image count + type classification (hero, lifestyle, infographic, dimensions, comparison, social proof, packaging, close-up), Video count + types, A+ modules used, Brand Story presence, Cross-sell modules, Virtual Bundle, S&S enrollment, Active coupons/deals, FBT widget ASINs, Brand Store link.

### Phase 1B — Reviews Scrape
**Code:** `1b_reviews_scrape` | **Tool:** `web_data_amazon_product_reviews` (per child) | **Calls:** 1-8

Extract: Review distribution (5★-1★), Positive/negative themes, Common questions, Competitor mentions, Photo/video review presence.

### Phase 1C — Competitor Scrape
**Code:** `1c_competitor_scrape` | **Tools:** `web_data_amazon_product` + `web_data_amazon_product_reviews` (per competitor) | **Calls:** 4-15

Identical data pull as 1A + 1B for each competitor.

### Phase 1D — Live SERP Snapshot
**Code:** `1d_serp_snapshot` | **Tool:** `web_data_amazon_product_search` (per keyword) | **Calls:** 3-10

Capture: Your organic positions, Competitor positions, Sponsored density, Badge visibility, Price context. **RULE_VARIATION_PARENTING_ACCURACY CHECKPOINT.**

### Phase 1E — FBT & Bundle Intelligence
**Code:** `1e_fbt_bundle_intel` | **Tools:** `brand_analytics_get_cross_sell_opportunities` + `web_data_amazon_product` | **Calls:** 3-10

Pull Market Basket data, cross-reference FBT widget ASINs. **RULE_VARIATION_PARENTING_ACCURACY** and **RULE_BUNDLE_COMPLIANCE** checkpoints.

## Phase 2: Search Query Performance (SQP)

### Phase 2A — Full Funnel Metrics (ASIN-Level)
**Code:** `2a_sqp_funnel_broad` | **Tool:** `brand_analytics_get_keyword_funnel_metrics` | **Calls:** 5-8

**IMPORTANT: Use ASIN filter, NOT brand filter.** Brand-level includes ALL products under brand.

Pulls per child ASIN:
1. `search_frequency_rank` ASC, Limit 200 → highest-volume keywords
2. `brand_purchase_share` DESC, Limit 200 → money keywords
3. `brand_click_share` DESC, Limit 200 → visibility keywords
4. `brand_impression_share` DESC filtered to impression > 0 but click_share < 3% → CTR problem candidates

Enable funnel_analysis, benchmark against top competitor. **RULE_INSUFFICIENT_SIGNAL** on every row.

### Phase 2B — Per-Child Cannibalization Check
**Code:** `2b_sqp_per_child` | **Tool:** `brand_analytics_get_keyword_funnel_metrics` (per child) | **Calls:** 3-8

Pull top-100 keywords per child independently. Build overlap matrix.

### Phase 2C — SQP Signal Dashboard
**Code:** `2c_sqp_ryg_signals` | **Tool:** `brand_analytics_analyze_search_query_performance` | **Calls:** 2-4

Pull RYG signals filtered by child ASINs. Filter for pareto_abc_class A, then A+B.

## Phase 3: Search Catalog Performance (SCP)

### Phase 3A — Catalog KPIs Per ASIN
**Code:** `3a_scp_catalog_kpis` | **Tool:** `brand_analytics_analyze_search_catalog_performance` | **Calls:** 1-2

Per-child: impressions, clicks, cart adds, purchases, revenue, CTR, cart add rate, CVR, WoW deltas.

### Phase 3B — Problem Child Identification
**Code:** `3b_scp_problem_children` | **Tool:** `brand_analytics_analyze_search_catalog_performance` (red filter) | **Calls:** 1-2

Filter for `weakness_colors: ["red"]`, `click_trend_colors: ["red"]`, `purchase_trend_colors: ["red"]`.

## Phase 4: Competitive Landscape & Momentum

### Phase 4A — Head-to-Head Competitive Positioning
**Code:** `4a_competitive_landscape` | **Tool:** `brand_analytics_get_competitive_landscape` | **Calls:** 1-3

Configure: `analysis_mode = "head_to_head"`, `weak_leader_detection.enabled = true`, `share_calculation = "combined"`, `include_position_history = true`.

### Phase 4B — Search Term Momentum
**Code:** `4b_search_term_momentum` | **Tool:** `brand_analytics_get_search_term_momentum` | **Calls:** 3-5

Pull: collapsing keywords, accelerating keywords, biggest WoW losers, displacement opportunities, competitor-specific threats.

## Phase 5: Advertising Analysis

### Phase 5A — PPC Search Term Performance
**Code:** `5a_ppc_search_terms` | **Tool:** `advertising_analyze_search_terms` | **Calls:** 4-6

Pulls: top spend, proven winners (high CVR), bleeders (high spend zero sales), match type efficiency.

**Try 4-week window first; if empty, try 12-week, then 52-week lookback.**

If ALL queries return empty → record `ppc_data_available: false`.

### Phase 5B — Campaign Structure & Placement
**Code:** `5b_campaign_structure` | **Tool:** `advertising_analyze_campaign_performance` | **Calls:** 2-5

Pulls: SP vs SB vs SD comparison, placement efficiency, per-ASIN ad spend allocation.

## Phase 6: Cross-Sell & Repeat Purchase

### Phase 6A — Market Basket Analysis
**Code:** `6a_market_basket` | **Tool:** `brand_analytics_get_cross_sell_opportunities` | **Calls:** 1-3

Cross-reference with Phase 1E FBT data. **RULE_VARIATION_PARENTING_ACCURACY** checkpoint.

### Phase 6B — Repeat Purchase Analysis
**Code:** `6b_repeat_purchases` | **Tool:** `brand_analytics_analyze_repeat_purchases` | **Calls:** 1-2

Per-child: repeat customer %, repeat revenue %, WoW trends.

**RULE_SUBSCRIBE_AND_SAVE:** If repeat_customer_pct < 2% across all children AND product is durable/one-time-install → tag `sns_fit: "contraindicated"`.

---
**⬆️ STAGE 1 STOPS HERE — CHECKPOINT BEFORE STAGE 2 ⬆️**

## Phase 7: Content, Image & Rufus Deep Audit (Stage 2)

### Phase 7A — Content Gap Matrix
**Code:** `7a_content_gap_matrix` | **Tool:** None (reads phase_results) | **Calls:** 0

Build comparison table: Title, Bullets, A+, Enhanced — You vs. competitors.
**RULE_NO_IRRELEVANT_TITLE_KEYWORDS:** Validate every keyword against Phase 2A SQP data.

### Phase 7B — Image Audit (Per Child)
**Code:** `7b_image_audit` | **Tool:** May use `scrape_as_markdown` for image URLs | **Calls:** 0-5

### Phase 7C — Video Audit
**Code:** `7c_video_audit` | **Tool:** None | **Calls:** 0

### Phase 7D — Rufus / AI-Readiness Audit
**Code:** `7d_rufus_readiness` | **Tool:** None | **Calls:** 0

Score across 6 dimensions (each 1-10):
1. Intent Coverage
2. Q&A Structure
3. Entity Completeness
4. Objection Handling
5. "Not For" Scoping
6. Comparison Readiness

## Phase 8: Synthesis & Diagnosis (Stage 2)

**Code:** `8_synthesis` | **Tool:** None (reads ALL phase_results + QA insights)

### Load Sequence
1. All completed phase_results ordered by phase_number
2. Carry-forward QA insights (carry_to_final = true)
3. Pending SOP updates

### Traffic Diagnosis Framework (Per Child)
| Signal Pattern | Diagnosis | Fix Domain |
|---|---|---|
| High CVR + Low CTR + Low Impression Share | **Packaging Problem** | Main image, title, price, badges |
| Normal CTR + Low Purchase Share | **PDP Problem** | Bullets, A+, reviews, pricing |
| High ACOS + Low Organic Share | **Relevance Problem** | Keyword-content alignment |
| Collapsing momentum + Competitor gaining | **Competitive Displacement** | Full audit needed |
| Multiple children on same keywords | **Cannibalization** | PPC restructure, title differentiation |
| Low repeat rate for consumable | **Retention Problem** | Product quality, S&S optimization |
| No FBT/bundle presence | **Ecosystem Gap** | Virtual Bundle, A+ cross-sell |

Save root cause table to `audit_findings`. Save cannibalization summary to `audit_runs`.

## Phase 9: Prioritized Action Plan (Stage 2)

### Scoring: Impact × Ease × Revenue Exposure (each 1–5, max 125)

### Tiers
- 🔥 **TIER 1** (60–125): Do first
- ⚙️ **TIER 2** (25–59): Schedule within 2–4 weeks
- 🧪 **TIER 3** (<25): Test / monitor
- 🛡️ **DEFENSIVE**: Protect, don't change without A/B test

### Content Quality Rules
- **RULE_NO_ALL_CAPS_BULLETS:** No ALL CAPS or emoji in bullets. Title Case headers + sentence case body.
- **RULE_NO_IRRELEVANT_TITLE_KEYWORDS:** SQP-validate every title keyword.
- **RULE_SUBSCRIBE_AND_SAVE:** Only when repeat_customer_pct > 5%, natural replenishment cycle, AND competitor adoption.
- **RULE_BUNDLE_COMPLIANCE:** No reciprocal duplicate bundles.

Save all actions to `audit_actions` table.

## Phase 10: Baseline Capture (Stage 2)

### Metrics to Capture
| Metric | Source Phase | Cadence |
|---|---|---|
| Click Share (core keywords) | 2A | Weekly |
| Purchase Share (core keywords) | 2A | Weekly |
| Catalog CTR / CVR | 3A | Weekly |
| Weekly Search Revenue | 3A | Weekly |
| Momentum signals | 4B | Weekly |
| PPC ACoS / ROAS | 5A (if available) | Weekly |
| Organic SERP rank | 1D | Bi-weekly |
| Total weekly orders | 3A | Weekly |
| Impression share | 2A | Weekly |
| Family revenue | 3A | Weekly |

Save to `audit_baselines`. Set `next_iteration_due` = today + 30 days. Update status = 'completed'. Write executive_summary.

### 30-Day Decision Framework
| Outcome | Action |
|---|---|
| Click share ↑, CVR ↑ | ✅ Working. Proceed to Tier 2. |
| Click share ↑, CVR ↓ | ⚠️ PDP needs work. |
| Click share ↓, CVR ↑ | ⚠️ Evaluate net revenue. |
| Click share ↓, CVR ↓ | 🔴 Revert. Re-diagnose. |
| No change | 🟡 Escalate optimization. |

## Critical Rules — Non-Negotiable

### Operational Rules (Both Stages)
1. Never optimize one child without analyzing parent and all siblings
2. Never change a title without checking cannibalization data first
3. Never improve CVR at the expense of CTR
4. Never optimize without SQP data
5. Never freeze PPC after listing changes
6. Always document baseline before changes
7. Always check FBT/Bundle/S&S status
8. Treat Rufus readiness as a content dimension
9. Always save phase results to DB before ending a session
10. Always check audit_progress at start of every session

### Data Quality Rules (Stage 1)
11. **RULE_VARIATION_PARENTING_ACCURACY:** Only act on verified child ASINs; tag others as [OTHER_PARENT]
12. **RULE_INSUFFICIENT_SIGNAL:** Tag signal quality on all keyword metrics (<15 clicks = noise, 15-49 = directional, 50+ = high)

### Content & Recommendation Rules (Stage 2)
13. **RULE_NO_ALL_CAPS_BULLETS:** No ALL CAPS or emoji in bullet copy
14. **RULE_NO_IRRELEVANT_TITLE_KEYWORDS:** SQP-validate every title keyword
15. **RULE_SUBSCRIBE_AND_SAVE:** S&S only when data supports it
16. **RULE_BUNDLE_COMPLIANCE:** No reciprocal duplicate bundles
17. **RULE_MID_AUDIT_QA_CAPTURE:** Capture mid-audit Q&A; incorporate in Phase 8/9

## Structured Data Schemas — Phase 1

### Phase 1A Schema
```json
{
  "children": [{
    "asin": "B0xxx1",
    "title": "...", "title_length": 150,
    "price": 29.99, "was_price": 39.99,
    "rating": 4.5, "review_count": 1234,
    "bsr": 5678, "bsr_category": "Category Name",
    "variation_attribute": "Blue / Large",
    "fulfillment": "FBA",
    "image_count": 7,
    "image_types": ["hero", "lifestyle", "infographic", "dimensions", "comparison", "social_proof", "packaging"],
    "video_count": 1, "video_types": ["brand"],
    "has_aplus": true,
    "aplus_modules": ["comparison_table", "image_text", "brand_story"],
    "has_brand_story": true, "has_cross_sell_module": true,
    "has_virtual_bundle": false,
    "has_subscribe_and_save": true, "sns_discount": "10%",
    "has_coupon": true, "coupon_value": "15% off",
    "fbt_asins": ["B0yyy1", "B0yyy2"],
    "bullets": ["bullet 1 text", "bullet 2 text"]
  }]
}
```

### Phase 1B Schema
```json
{
  "children": [{
    "asin": "B0xxx1",
    "total_reviews": 1234, "rating": 4.5,
    "reviews_pooled": true,
    "distribution": {"5": 60, "4": 20, "3": 10, "2": 5, "1": 5},
    "positive_themes": ["durability", "value for money"],
    "negative_themes": ["sizing runs small"],
    "common_questions": ["Does it work with X?"],
    "competitor_mentions": ["Brand X"],
    "has_photo_reviews": true, "has_video_reviews": false
  }]
}
```

### Phase 1C Schema
```json
{
  "competitors": [{
    "asin": "COMP1", "title": "...", "brand": "...",
    "price": 34.99, "price_per_unit": 0.05,
    "rating": 4.3, "review_count": 5678,
    "bsr": 1234, "bsr_category": "...",
    "bought_past_month": "10K+",
    "image_count": 7, "video_count": 2,
    "has_aplus": true, "has_brand_story": false,
    "has_virtual_bundle": true, "has_subscribe_and_save": false,
    "has_coupon": false, "fulfillment": "FBA",
    "bullets": ["..."], "fbt_asins": ["..."],
    "positive_review_themes": ["..."],
    "negative_review_themes": ["..."]
  }]
}
```

### Phase 1D Schema
```json
{
  "keywords": [{
    "keyword": "cabinet door bumpers",
    "our_positions": [{"asin": "B09LVWN39P", "organic_rank": 3, "page": 1, "is_sponsored": false, "badges": ["amazons_choice"]}],
    "competitor_positions": [{"asin": "B088HGQSZT", "organic_rank": 1, "page": 1, "badges": ["amazons_choice"], "brand": "FATLODA"}],
    "sponsored_count_page_1": 8,
    "price_range_page_1": {"min": 4.99, "max": 14.99}
  }]
}
```

### Phase 1E Schema
```json
{
  "children": [{
    "asin": "B09LVWN39P",
    "fbt_partners": [{
      "asin": "B0CS6G8VGH", "title": "...", "brand": "Vellax",
      "relationship": "same_brand_different_parent",
      "combination_pct": 8.79, "consistency_score": 1.0,
      "best_rank": 1, "bundle_opportunity": true
    }],
    "market_basket_partners": [{"asin": "...", "combination_pct": 5.2, "consistency_score": 0.8}]
  }]
}
```

## Structured Data Schemas — Phases 2-6

### Phase 2A Schema
```json
{
  "children": [{
    "asin": "B09LVWN39P",
    "keyword_count": 100,
    "top_keywords": [{
      "keyword": "cabinet door bumpers",
      "sfr": 1234,
      "total_impressions": 45000, "brand_impression_share": 7.2,
      "total_clicks": 3200, "brand_click_share": 12.6,
      "total_cart_adds": 800, "brand_cart_add_share": 15.1,
      "total_purchases": 400, "brand_purchase_share": 12.1,
      "signal_quality": "high"
    }],
    "ctr_gap_keywords": [],
    "cvr_gap_keywords": [],
    "weekly_search_revenue_estimate": 5265
  }]
}
```

### Phase 2B Schema
```json
{
  "cannibalization_matrix": [{
    "keyword": "cabinet door bumpers",
    "children": {
      "B09LVWN39P": {"click_share": 12.6, "purchase_share": 12.1},
      "B0CV4CMBWM": {"click_share": 0, "purchase_share": 0}
    },
    "cannibalization_risk": "none"
  }],
  "summary": {"total_overlapping_keywords": 0, "cannibalization_detected": false}
}
```

### Phase 2C Schema
```json
{
  "signals": [{
    "asin": "B09LVWN39P",
    "strength_color": "green", "strength_explanation": "...",
    "weakness_color": "red", "weakness_explanation": "...",
    "opportunity_color": "green", "opportunity_explanation": "...",
    "threshold_color": "yellow", "threshold_explanation": "..."
  }]
}
```

## Resource Usage & Version Comparison

### Estimated Resource Usage Per Full Audit (v3.1)
| Stage | Phases | Est. Tool Calls | Model Tier | Est. Cost |
|---|---|---|---|---|
| Stage 1 | 1a–6b (16 phases) | 33–55 | Cheap | ~$0.30–0.80 |
| Stage 2 | 7a–10 (7 phases) | 0–5 | Expensive | ~$2.00–4.00 |
| **TOTAL** | **23 phases** | **~33–60** | **Mixed** | **~$2.30–4.80** |

### v2.x vs v3.1 Comparison
| Metric | v2.x | v3.1 | Improvement |
|---|---|---|---|
| Cost per audit | ~$8–15 | ~$2.30–4.80 | **65–75% savings** |
| Wall-clock time (sequential) | ~90 min | ~40–50 min | **45% faster** |
| Wall-clock time (parallel) | ~90 min | ~20–25 min | **75% faster** |
| Stages to configure | 1 | 2 | Simple |
| Failure recovery | Full restart | Resume from last phase | Granular |
