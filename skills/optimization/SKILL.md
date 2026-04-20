---
name: google-ads-optimization
description: Google Ads optimization playbook including Account Hygiene Checklist. Use when optimizing campaigns, auditing accounts, managing bids, analyzing search terms, or making scale-vs-cut decisions.
---

# Google Ads Optimization Playbook — Multi-Vertical

Weekly optimization checklists, account hygiene audits, search terms audit workflow, bid management, scale-vs-cut decision framework, and vertical-specific optimization for B2B, e-commerce, healthcare, and local services.

---

## Account Hygiene Checklist

Run this BEFORE any strategy work. These issues corrupt bidding signals and waste budget silently.

### Conversion Action Cleanup

The #1 issue across managed accounts. Most accounts accumulate 20-50+ conversion actions over time, corrupting Smart Bidding.

- [ ] Count total conversion actions — flag if >15 (typical problem accounts have 30-95)
- [ ] Verify counting model: ALL lead gen actions must use "One per click" (not "Many per click")
  - Many/Click inflates conversion numbers and teaches Smart Bidding to optimize for low-quality repeat actions
  - Only use Many/Click for e-commerce purchases where repeat transactions are real
- [ ] Check for HIDDEN conversion actions still included in "Conversions" column
  - Go to Goals > Conversions > filter by Status: Hidden
  - Any HIDDEN action with "Include in Conversions: Yes" is silently inflating your numbers
- [ ] Ensure only 1-2 actions are marked Primary per campaign goal
  - B2B: Demo request OR offline MQL (not both, unless deliberately weighting)
  - Healthcare: Appointment request + Phone call (120s+)
  - E-commerce: Purchase only (add-to-cart is Secondary)
  - Local: Phone call (60s+) + Contact form
- [ ] Remove or archive REMOVED conversion actions that clutter reporting
- [ ] Verify conversion values are set correctly (not $0 or $1 defaults)

### Zero-Conversion Campaign Detection

- [ ] Flag active campaigns with >$100 spend and 0 conversions in last 30 days
- [ ] Before pausing, verify: Is conversion tracking actually working? (check tag firing)
- [ ] Check if it's a new campaign (<60 days old) — allow learning time
- [ ] Check if conversions happen offline with long delay (B2B only)
- [ ] Decision tree:
  - Tracking broken → Fix tracking (same-day priority)
  - Tracking works, no conversions after 60 days at 3x CPA → Pause campaign
  - Tracking works, <30 days data → Continue monitoring

### Quality Score Recovery

- [ ] Pull QS distribution — flag any keyword with QS ≤3 and >$50/mo spend
- [ ] For QS 1-2: Check landing page URL first (404s are immediate fix)
- [ ] For QS 3-4: Check ad relevance (is keyword in any headline?)
- [ ] For thin content pages (<300 words): flag for client content team
- [ ] Track QS trends month-over-month for top 20 keywords by spend

### Device Waste Check

- [ ] Pull device performance breakdown for all active campaigns
- [ ] Flag tablet spend with zero conversions (extremely common across portfolios)
- [ ] If tablet has $0 conversions over 60+ days, recommend -100% bid adjustment
- [ ] Check mobile vs desktop CVR ratio — if mobile CVR is <50% of desktop, apply -30% mobile bid

### Impression Share Baseline

- [ ] Flag campaigns with IS <15% (many accounts show 10% floor — the minimum Google reports)
- [ ] Diagnose cause for each: IS Lost to Budget vs IS Lost to Rank
- [ ] Lost to Rank → QS/bid issue → fix QS and improve ads before adding budget
- [ ] Lost to Budget → if campaign is profitable, increase budget 20-30%
- [ ] Lost to Budget on unprofitable campaign → do NOT increase budget; optimize first

---

## Weekly Optimization Checklist

Run this every Monday morning. Takes 30-45 minutes for a standard account.

### 1. Budget and Pacing (5 min)
- [ ] Check month-to-date spend vs monthly budget target
- [ ] Verify no campaigns are limited by budget unexpectedly (IS Lost Budget > 20%)
- [ ] Confirm all campaigns are delivering (no disapprovals, no zero-impression campaigns)
- [ ] Check for any campaigns that exhausted daily budget before 2pm consistently

### 2. Conversion Performance (10 min)
- [ ] Pull last 7 days vs previous 7 days: leads, CPA, CVR, spend
- [ ] Flag any campaign with CPA 50%+ above target in the last 14 days
- [ ] Check conversion tracking is still firing (sudden drops = broken tags)
- [ ] Review offline conversion uploads - are they current? (MQL, SQL, Opp data)

### 3. Search Terms Audit (10 min)
- [ ] Pull Search Terms Report for last 7 days
- [ ] Add negative keywords for irrelevant queries
- [ ] Flag high-spend/no-conversion search terms for deeper review
- [ ] Identify new keyword opportunities from converting search terms

### 4. Keyword Health (5 min)
- [ ] Pause any keyword that has spent 3x target CPA with zero conversions (all-time data)
- [ ] Review Quality Score changes - flag any keyword that dropped below 5
- [ ] Check for "Low Search Volume" keywords - consolidate or replace
- [ ] Review auction insights for top campaigns - any new competitors?

### 5. Ad Performance (5 min)
- [ ] Check for disapproved ads and fix immediately
- [ ] Review ad-level CTR and CVR - any ad with 50%+ lower CTR than its ad group average after 1,000+ impressions = candidate for pause
- [ ] Verify all ad groups have at least 2 active RSAs

### 6. Action Items (5 min)
- [ ] Document what you changed this week and why
- [ ] List 1-3 experiments to run next week
- [ ] Flag anything that needs escalation (budget increase, landing page changes, conversion tracking issues)

### 7. Hygiene Quick Check (5 min)
- [ ] Any new conversion actions created (accidentally or by Google auto-creation)?
- [ ] Any campaigns that crossed the $100-spend-zero-conversion threshold this week?
- [ ] Any QS drops below 5 on top keywords?
- [ ] Tablet spend accumulating with zero conversions?

---

## Search Terms Audit Workflow

The Search Terms Report is the single highest-ROI optimization activity in Google Ads. It tells you exactly what people typed before clicking your ad.

### Step-by-Step Process

**Step 1: Pull the Report**
- Go to Keywords > Search Terms in Google Ads
- Date range: Last 7 days (weekly review) or Last 14 days (bi-weekly)
- Sort by Cost (highest first)

**Step 2: Classify Each Search Term**

| Classification | Definition | Action |
|---|---|---|
| **Relevant + Converting** | Matches intent, has conversions | Keep. Consider adding as a keyword if not already. |
| **Relevant + Not Converting** | Matches intent, but no conversions yet | Monitor. Only take action after 2-3x CPA spend. |
| **Irrelevant + Any Spend** | Does not match intent | Add as negative keyword immediately. |
| **Ambiguous** | Could be relevant, unclear intent | Monitor for 1-2 more weeks. |

**Step 3: Add Negatives**

For irrelevant terms:
- Add as **exact match negative** if only that specific query is irrelevant
- Add as **phrase match negative** if the theme is irrelevant (e.g., "free data enrichment")
- Add to a **shared negative keyword list** if it applies across multiple campaigns

**Step 4: Mine New Keywords**

For converting search terms that aren't in your keyword list:
- Add them as exact match keywords in the appropriate ad group
- If the term represents a new theme, consider creating a new ad group for it
- Track these additions - they often become your best performers

### Search Terms Red Flags

| Red Flag | What It Means | Action |
|---|---|---|
| High spend, zero conversions | Budget waste on irrelevant or low-intent queries | Negative keyword + investigate why it's matching |
| Competitor name appearing | You're showing for competitor searches (possibly from broad match) | Add as negative or create a dedicated Competitor campaign |
| Job-related queries | "data enrichment jobs", "data analyst salary" | Add full jobs negative list |
| Educational queries | "what is data enrichment", "data enrichment tutorial" | Negative or move to a content campaign with content LP |
| Geographic mismatch | Queries from locations outside your target market | Check location settings (Presence only) |
| Duplicate keywords | Same search term triggering multiple campaigns | Add cross-campaign negatives to prevent cannibalization |

### Search Terms Audit Schedule

| Account Maturity | Frequency | Focus |
|---|---|---|
| **Week 1-2 (new campaign)** | Daily | Aggressive negative keyword addition, validate match type behavior |
| **Week 3-4** | Every 2-3 days | Continue negatives, start mining new keywords |
| **Month 2-3** | Weekly | Standard weekly audit |
| **Month 4+** | Bi-weekly | Mature account, fewer new terms appearing |

---

## Bid Management Rules

### Manual CPC Bid Rules

| Scenario | Rule | Example |
|---|---|---|
| **Keyword converting at target CPA** | Maintain current bid | CPA target: $150, actual CPA: $140 - keep bid |
| **Keyword converting below target CPA** | Increase bid by 15-20% | CPA: $100 vs $150 target - increase bid to capture more volume |
| **Keyword converting above target CPA** | Decrease bid by 15-20% | CPA: $200 vs $150 target - decrease bid |
| **Keyword never converted, spent > 3x CPA** | Pause keyword | Spent $450+ with zero conversions - pause |
| **Keyword never converted, spent < 3x CPA** | Continue monitoring | Spent $200 of $450 threshold - let it run |

### Smart Bidding Management Rules

Smart Bidding (tCPA, tROAS) manages bids automatically, but you still need to manage the targets:

| Scenario | Rule | Example |
|---|---|---|
| **CPA consistently 10%+ below target** | Lower target CPA by 10% | Achieving $120 CPA on $150 target - change to $135 |
| **CPA consistently 10%+ above target** | Raise target CPA by 10% or investigate | Achieving $180 CPA on $150 target - investigate before raising |
| **CPA wildly fluctuating** | Do nothing for 2 weeks | Smart Bidding needs time to calibrate |
| **CPA doubling overnight** | Check conversion tracking first | Broken tags cause Smart Bidding to go haywire |
| **Volume dropped after tCPA decrease** | Raise target back, decrease more gradually | You went too aggressive, Google restricted auctions |

### Bid Adjustment Cadence

| Bidding Type | Adjustment Frequency | Data Threshold |
|---|---|---|
| **Manual CPC** | Weekly | 200+ clicks or 10+ conversions per keyword |
| **Smart Bidding (tCPA)** | Every 2-4 weeks | 30+ conversions in the evaluation window |
| **Smart Bidding (tROAS)** | Monthly | 50+ conversions with value data |

**Critical rule:** Never change Smart Bidding targets more than 15-20% at a time. Drastic changes force the algorithm to re-learn, causing a performance dip.

---

## Performance Max — Quick Reference

**Prerequisites (all must be true before launch):**
- Search campaigns profitable and maxed out (IS Lost Budget < 10%)
- 50+ conversions/month across the account
- Offline conversion tracking set up and uploading regularly
- High-quality creative assets (images, videos, headlines, descriptions)
- Minimum $3,000/month dedicated PMax budget
- Brand exclusions configured at launch to protect Brand Search

### Warning Signs

| Warning Sign | What It Means | Action |
|---|---|---|
| **Brand Search conversions dropping after PMax launch** | PMax cannibalizing Brand Search | Add brand exclusions to PMax |
| **High spend on Display/YouTube with low conversions** | Allocating to low-intent channels | Improve audience signals, consider pausing |
| **"New customer" conversions are actually existing leads** | Customer match list stale | Update customer match lists, review conversion actions |
| **CPA climbing steadily over 30 days** | Expanding to lower-quality audiences | Tighten audience signals, reduce budget |
| **Great overall numbers but pipeline isn't growing** | Optimizing for low-quality conversions | Switch to offline conversion optimization |

---

## When to Scale vs When to Cut

### The Decision Framework

Every campaign, keyword, and ad eventually reaches one of four states:

```
                    PERFORMING
                        |
         +--------------+--------------+
         |                             |
    AT CAPACITY                   ROOM TO GROW
    (IS Lost Budget < 10%)        (IS Lost Budget > 20%)
         |                             |
    Hold or expand to              Scale: increase
    new themes/channels             budget 15-20%/week
         |                             |
         +--------------+--------------+
                        |
                  UNDERPERFORMING
                        |
         +--------------+--------------+
         |                             |
    FIXABLE                        BROKEN
    (Root cause identified)        (No clear fix)
         |                             |
    Fix: QS, LP, ads, targeting    Cut: pause, reallocate
    then re-evaluate in 30 days     budget to winners
```

### Scale Signals (Green Light)

| Signal | Threshold | Scaling Action |
|---|---|---|
| CPA at or below target for 30+ days | Consistent performance | Increase budget 15-20% per week |
| IS Lost (Budget) > 20% on profitable campaign | Consistently budget-limited | Increase daily budget 25-30% |
| Conversion rate improving month-over-month | 3+ months trend | Add more keywords in the same theme |
| New keyword converting from Search Terms | 5+ conversions from a new query | Create dedicated ad group for it |
| Exact match maxed out, profitable | IS Lost < 10% | Add phrase match for proven keywords |
| Phrase match maxed out, profitable | 30+ conversions/month | Add broad match with audience layering |

### Cut Signals (Red Light)

| Signal | Threshold | Cutting Action |
|---|---|---|
| Keyword spent 3x CPA, zero conversions (all-time) | No exceptions | Pause keyword |
| Campaign CPA 2x+ target for 30+ days | After all troubleshooting | Reduce budget 50%, give 14 more days, then pause |
| Quality Score dropped to 3 or below | After landing page fix attempt | Pause keyword, it's costing too much |
| CTR below 1% for Search campaign | After ad copy refresh | Investigate keyword-ad relevance, likely pause |
| Broad match generating 80%+ irrelevant queries | After adding negatives | Pause broad match, revert to phrase/exact |
| PMax spending heavily on Display with no conversions | After 30 days + audience signal improvements | Pause PMax, go back to Search only |

### Scale Decision Checklist

Before scaling any campaign, confirm:
- [ ] CPA has been at or below target for 30+ days (not just 7 days)
- [ ] Conversion tracking is verified and accurate
- [ ] Landing page conversion rate is stable (not declining)
- [ ] You have budget headroom (client approved increase)
- [ ] You've checked that scaling won't cannibalize other campaigns
- [ ] You have a plan to monitor the increased spend daily for the first 2 weeks

### Cut Decision Checklist

Before cutting any campaign, confirm:
- [ ] You've looked at all-time data, not just last 7 days
- [ ] You've verified conversion tracking is working (a campaign might look like it has zero conversions because the tag is broken)
- [ ] You've checked offline conversions (maybe the click converted into an SQL 45 days later)
- [ ] You've tried optimization fixes first (QS improvement, landing page changes, ad copy refresh)
- [ ] The underperformance has persisted for 30+ days
- [ ] You're not cutting during a known seasonal dip

---

## Vertical-Specific Optimization

| Vertical | Top 3 Optimization Moves |
|---|---|
| **E-commerce** | Shopping feed health (disapprovals, attributes, prices); product group ROAS tiering; dynamic remarketing + promotion extensions |
| **Healthcare** | Ad copy compliance (no outcome guarantees); call tracking verification + recording; appointment scheduling integration + provider page freshness |
| **Local Services** | GBP review sync to ads; service area CVR review by zip/city; click-to-call CVR by hour/day + LSA lead quality |
| **B2B** | Offline conversion uploads current (MQL/SQL/Opp); cost per opportunity ranking (not CPL); brand exclusions on PMax |

---

## Beyond Weekly — Monthly + Quarterly Checklist

Run monthly items on the first Monday of the month. Revisit quarterly items every 90 days to reset strategic direction.

**Monthly (first Monday):**
- [ ] Full pipeline review: spend, leads, MQLs, SQLs, pipeline, revenue MoM
- [ ] Calculate true cost per opportunity and cost per customer
- [ ] Update offline conversion data in Google Ads
- [ ] QS distribution audit — flag keywords >$50/mo with QS <5
- [ ] Geographic performance: apply +15-25% bids to top geos, -20-50% to weak geos
- [ ] Device performance: update bid adjustments from actual CVR
- [ ] Match type performance: exact vs phrase vs broad on CPA/CVR/volume
- [ ] Auction Insights + competitor ad review (Transparency Center)
- [ ] Refresh ads running 8+ weeks; plan next test batch with labels
- [ ] Budget reallocation: rank campaigns by cost per opportunity, shift 20% from bottom to top

**Quarterly (every 90 days) — strategic questions:**
- [ ] IS on NonBrand: >90% → expand themes; <60% → capture existing first
- [ ] 90-day CPA trend: up = market shift; down = scale signal
- [ ] Scale-vs-cut review: sunset anything running at loss 90+ days with no fix path
- [ ] Channel expansion: only consider PMax/YouTube/Demand Gen once Search IS > 85%
- [ ] Landing page competitiveness audit + A/B test plan for next quarter
- [ ] Full conversion tracking audit: tags firing, offline imports current, attribution settings correct

---

> **Built by [Xtropy](https://www.xtropy.net).** Multi-vertical optimization playbook for agency management.
