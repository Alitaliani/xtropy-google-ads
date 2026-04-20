---
name: google-ads-audiences
description: Google Ads audience targeting strategy across verticals. Use when setting up RLSA, Customer Match, in-market segments, remarketing, or audience layering.
---

# Google Ads Audience Strategy — Multi-Vertical Targeting, Layering, and Segmentation

How to build, layer, and manage audiences in Google Ads across verticals (B2B, healthcare, e-commerce, local services). Covers RLSA, Customer Match, in-market segments, custom audiences, observation vs targeting mode, bid adjustments, exclusions, and dynamic remarketing.

---

## Audience Types Available in Google Ads

| Audience Type | Data Source | Best For | Minimum Size |
|---|---|---|---|
| **RLSA (Remarketing Lists)** | Website visitors via Google tag | Re-engaging known visitors in Search | 1,000 users |
| **Customer Match** | CRM uploads (email, phone, address) | Targeting existing contacts and building Similar Audiences | 1,000 matched users |
| **In-Market Segments** | Google's behavioral signals | Reaching people actively researching categories | N/A (Google-managed) |
| **Affinity Segments** | Google's interest-based signals | Broad awareness | N/A (Google-managed) |
| **Custom Segments** | Your keyword/URL/app definitions | Intent-based targeting beyond standard segments | N/A (Google-managed) |
| **Similar Audiences** | Modeled from your first-party lists | Expanding reach from proven audiences | Source list needs 1,000+ users |
| **Combined Segments** | Boolean logic across multiple audiences | Precise intersection targeting | Varies |

---

## RLSA - Remarketing Lists for Search Ads

RLSA is the highest-ROI audience strategy in Google Ads. It lets you show Search ads only to (or bid higher for) people who have already visited your website.

### Why RLSA Is Critical

- Buyers research before converting — B2B buyers for weeks or months, healthcare patients for days to weeks, e-commerce shoppers for hours to days
- First-visit conversion rates are typically 1-3% — most visitors leave without converting
- RLSA recaptures that research traffic when they search again
- You can bid on broader, more expensive keywords — but only for known visitors (controlling cost)

### RLSA Setup

1. **Install the Google tag** on all website pages (or use Google Tag Manager)
2. **Create remarketing lists** in Google Ads (Audience Manager - Your Data Segments)
3. **Define segments by behavior:**

| Segment | Definition | Membership Duration | Use Case |
|---|---|---|---|
| All visitors | All website traffic | 540 days (max) | Broad RLSA, baseline |
| High-intent pages | Visited pricing, demo, or contact page | 90 days | Aggressive bid adjustments |
| Content consumers | Visited 3+ pages or spent 2+ minutes | 180 days | Medium-intent RLSA |
| Converters | Completed a form submission or demo request | 540 days | Exclude from prospecting, upsell |
| Blog/resource visitors | Visited blog or resource pages only | 30 days | Low-intent, use for awareness only |

4. **Apply lists to Search campaigns** (see Observation vs Targeting below)

### RLSA Strategy by Intent Level

| Scenario | Approach | Bid Adjustment |
|---|---|---|
| High-intent keywords + known visitors | RLSA in observation mode, bid +30-50% | Aggressive - these convert well |
| Broad keywords + known visitors only | RLSA in targeting mode, standard bids | Controls cost on expensive broad terms |
| Competitor keywords + known visitors | RLSA in targeting mode, bid +20% | Only show competitor ads to people who already know you |
| Brand keywords | No RLSA needed | Already high-intent by definition |

### RLSA Considerations by Vertical

| Vertical | RLSA Window | Strategy |
|---|---|---|
| B2B | 540 days | Long sales cycle, bid +30-50% on returning visitors |
| Healthcare | 30-90 days | Shorter decision window, procedure-based segments |
| E-commerce | 30-180 days | Cart abandoners (7 days), browsers (30 days), past buyers (180 days) |
| Local | 30 days | Short decision cycle, may not reach 1,000 list minimum |

**Note on local services:** Local service businesses often have audience lists under 1,000 users. Workarounds: extend membership duration to 540 days, combine with in-market segments, focus on geo targeting instead of RLSA. If you cannot reach the 1,000-user minimum even with maximum membership duration, RLSA will not be viable — prioritize geo targeting and in-market segments.

---

## Customer Match - CRM Audience Uploads

Customer Match lets you upload first-party CRM data (emails, phone numbers, mailing addresses) to Google Ads, which then matches it against Google accounts.

### What You Can Upload

| Data Type | Match Quality | Notes |
|---|---|---|
| Email addresses | Highest match rate | Use all available emails - work and personal |
| Phone numbers | Good | Include country code, E.164 format preferred |
| Mailing addresses | Lower | Requires first name, last name, country, zip minimum |
| Combined | Best | Upload all available data points for each contact |

### Match Rate Expectations

| Vertical | Data Type | Expected Match Rate | Notes |
|---|---|---|---|
| B2B | Business emails only | 30-50% | Better than Meta because Google has work accounts |
| B2B | Personal emails included | 60-80% | Significantly improves match rate |
| E-commerce | Personal emails | 50-70% | Good match rates with consumer email data |
| Healthcare | Limited applicability | N/A | HIPAA restricts patient data uploads (see warning below) |
| Local services | Mixed data quality | 20-40% | Often incomplete email data, small lists |
| Any | Combined data (email + phone + address) | 70-90% | Upload all available data points for best results |

### Healthcare HIPAA Warning

- Do **NOT** upload patient email lists to Customer Match — this violates HIPAA
- Only upload marketing consent lists (newsletter subscribers, event attendees, webinar registrants)
- Patient data stays in the EHR/practice management system
- Use website remarketing instead of Customer Match for patient audiences
- When in doubt, consult your compliance officer before uploading any healthcare-related list

### E-commerce Customer Match

| Segment | Source | Purpose |
|---|---|---|
| Past purchasers | Order data | Upsell/cross-sell campaigns |
| High-value customers | Top 20% by LTV | Build Similar Audiences from best buyers |
| Cart abandoners | Email capture at checkout | Win-back campaigns — 60-80% match rate with personal emails |
| Seasonal buyers | Holiday/back-to-school purchases | Pre-season reactivation campaigns |
| Lapsed customers | No purchase in 6-12 months | Win-back with promotions or new products |

### Local Services Customer Match

- Often too small (<1,000 emails) to be usable
- Focus on remarketing and geo targeting instead
- If list is large enough: past customers for reviews/referral campaigns
- Consider combining with broader audiences to reach minimum thresholds

### CRM Segments to Upload (B2B)

| Segment | Source | Purpose |
|---|---|---|
| Closed-won customers | CRM - won deals | Build Similar Audiences, exclude from prospecting |
| High-value customers | CRM - top 20% by revenue | Build Similar Audiences from best customers |
| Pipeline contacts | CRM - active opportunities | Reinforce during sales cycle (acceleration) |
| Marketing qualified leads | CRM - MQL stage | Re-engage leads that have not progressed |
| Lost deals | CRM - closed-lost | Win-back campaigns with new messaging |
| Target account contacts | ABM list | Direct targeting of named accounts |

### Customer Match Best Practices

- **Refresh lists weekly** - stale lists miss new CRM entries and include converted contacts
- **Segment aggressively** - a list of "all CRM contacts" is less useful than targeted segments
- **Hash before upload** - Google accepts SHA-256 hashed data for privacy compliance
- **Minimum 1,000 matched users** required for the list to be usable
- **Use Customer Match for Similar Audiences** - your best customer list is the strongest seed

---

## Dynamic Remarketing for E-commerce

Dynamic remarketing shows users the exact products they viewed on your site, making it the most personalized and highest-converting remarketing strategy for e-commerce.

### Requirements

- **Product feed** linked to Google Ads via Google Merchant Center
- **Remarketing tag** with custom parameters that pass product IDs (or use Google Tag Manager with the data layer)
- **Feed-to-tag mapping**: product IDs in your tag must match product IDs in your Merchant Center feed

### Dynamic Remarketing Segments

| Segment | Window | Value | Strategy |
|---|---|---|---|
| Cart abandoners | 1-7 days | Highest — they were ready to buy | Aggressive bids, urgency messaging, potential discount |
| Product viewers | 7-30 days | High — active shopping intent | Show viewed products, highlight reviews/ratings |
| Category browsers | 30-90 days | Medium — interest but not product-specific | Show category best sellers, new arrivals |
| Past purchasers | 90-180 days | Varies | Exclude from acquisition campaigns OR target for upsell/cross-sell |

### Dynamic Remarketing Best Practices

- **Separate campaigns by funnel stage** — cart abandoners and product viewers should have different budgets and bids
- **Exclude recent purchasers** from the product they just bought (show complementary products instead)
- **Use responsive display ads** — Google auto-generates ad variations from your product feed
- **Set frequency caps** — showing the same product 50 times annoys users; cap at 5-7 impressions per day
- **Monitor ROAS by segment** — cart abandoners should deliver 5-10x ROAS; category browsers may deliver 2-3x

---

## In-Market, Affinity, and Custom Segments

In-market segments capture active research intent; affinity captures long-term interests; custom segments let you define intent via search terms or URLs. Use observation mode first, then move to targeting where it outperforms. In-market/custom segments are most effective on Display and YouTube — on Search, keywords already carry the intent signal.

### In-Market Segments by Vertical

| Vertical | Segment Examples | Expected Lift (observation + bid adj) |
|---|---|---|
| B2B | CRM software, cloud computing, ERP systems, project management, cybersecurity | +10-20% conv rate on relevant keywords |
| Healthcare | Medical services, dental, vision care, health insurance (limited by policy) | +5-15%, availability varies by region |
| E-commerce | Apparel, consumer electronics, home and garden, beauty | +15-25% on mid-funnel keywords |
| Local | Home services (plumbing, HVAC, roofing), automotive | +10-20%, often limited by geo overlap |
| Financial | Business lending, commercial insurance, payment processing | +10-15% |

**Affinity segments** are long-term interest signals. Use only for YouTube/Display awareness campaigns — never for Search or conversion-optimized campaigns (too broad, low signal).

### Custom Segments — Combined URL + Search Term Definitions

| Use Case | Search Terms OR URLs to Include |
|---|---|
| Competitor researchers | Competitor brand names, "[competitor] alternative", "[competitor] vs"; competitor websites and blogs |
| Category buyers | "best [category] software", "[category] comparison"; G2, Capterra, TrustRadius category pages |
| Problem-aware | "[problem] solution", "how to fix [problem]"; industry publications, analyst sites |
| Local service seekers | "[service] near me", "[service] in [city]"; Yelp, Angi, HomeAdvisor category pages |
| E-commerce shoppers | "[product] reviews", "buy [product] online"; Amazon product pages, category review sites |
| Healthcare research | "[procedure] cost", "[condition] treatment"; WebMD, Healthline, specialty info sites |

**Best practices:** one intent per segment (don't mix competitor + category + problem); 10-15 terms/URLs per segment; test in observation before targeting; strongest on Display/YouTube/Discovery, limited impact on Search.

---

## Observation vs Targeting Mode

This is the most important audience setting in Google Ads and one of the most misunderstood.

| Mode | What It Does | When to Use |
|---|---|---|
| **Observation** | Ads show to everyone; audience data is collected for bid adjustments and reporting | Default for Search campaigns - learn before restricting |
| **Targeting** | Ads ONLY show to people in the selected audience | RLSA-only campaigns, Customer Match campaigns, Display/YouTube prospecting |

### Decision Framework

| Campaign Type | Recommended Mode | Rationale |
|---|---|---|
| Search - core keywords | Observation | Don't restrict reach; use bid adjustments instead |
| Search - broad keywords | Targeting (RLSA only) | Control cost by limiting to known visitors |
| Search - competitor keywords | Targeting (RLSA) or Observation | Targeting saves budget; observation gives data |
| Display - prospecting | Targeting | Must restrict or Display shows to everyone |
| Display - dynamic remarketing | Targeting | Must target specific product viewer segments |
| YouTube - awareness | Targeting | Must define audience or spend is wasted |
| Performance Max | Neither (PMax manages this) | Audience signals guide PMax but don't restrict |

### Common Mistake

Adding audiences in **targeting mode** to Search campaigns when you mean **observation mode**. This silently restricts your reach to only people in those audiences — you lose all other Search traffic. Always double-check the mode after adding audiences.

---

## Audience Bid Adjustments

Bid adjustments let you increase or decrease bids for specific audiences without restricting reach (when using observation mode).

### Recommended Bid Adjustments

| Audience | Bid Adjustment | Rationale |
|---|---|---|
| Pricing/demo page visitors (RLSA) | +40% to +60% | Highest intent - they were ready to buy |
| Cart abandoners — e-commerce (RLSA) | +50% to +80% | Were about to purchase, highest recovery value |
| Multi-page visitors (RLSA) | +20% to +30% | Research behavior indicates interest |
| Customer Match - pipeline contacts | +30% to +50% | Reinforcing during active sales cycle |
| Customer Match - past purchasers | +20% to +40% | Upsell/cross-sell opportunity |
| Customer Match - MQLs | +20% to +30% | Re-engaging known qualified leads |
| In-market - relevant segments | +10% to +20% | Mild uplift for behavioral signal |
| Converters (RLSA) | -100% (exclude) | Already converted - don't pay again |
| Customer Match - closed-won | -100% (exclude) | Already a customer (unless upsell campaign) |

### How to Set Bid Adjustments

1. Go to the campaign or ad group
2. Click **Audiences** in the left menu
3. Add audiences in **Observation** mode
4. After 2-4 weeks of data, review performance by audience
5. Set bid adjustments based on conversion rate and CPA differences
6. Re-evaluate monthly - audience performance shifts over time

---

## Negative Audiences and Exclusions

Exclusions prevent wasted spend on people who should never see your ads.

### Must-Have Exclusions

| Exclusion | How to Build | Apply To | Verticals |
|---|---|---|---|
| **Existing customers** | Customer Match list of current customers | All prospecting campaigns | All |
| **Recent converters** | RLSA list of thank-you page visitors (7-30 days) | All prospecting campaigns | All |
| **Job applicants** | RLSA list of careers page visitors | All campaigns | B2B |
| **Competitor employees** | Customer Match list (if you have competitor employee emails) | All campaigns | B2B |
| **Internal employees** | Customer Match list of employee emails | All campaigns | All |
| **Disqualified leads** | Customer Match list of DQ contacts from CRM | All prospecting campaigns | B2B |
| **Recent purchasers** | RLSA list of order confirmation page (7-30 days) | Acquisition campaigns | E-commerce |
| **Out-of-area visitors** | Geo exclusions (not audience-based) | All campaigns | Local |

### Exclusion Setup

1. Build the exclusion list in **Audience Manager** (Your Data Segments or Customer Segments)
2. Go to the campaign - **Audiences** - **Exclusions**
3. Add the relevant exclusion list
4. Verify the exclusion is active (check audience insights to confirm list size)

**Important:** Exclusion lists need the same minimum 1,000 users as targeting lists. If your exclusion list is too small, Google may not apply it consistently.

---

## Audience Sizing

| Audience Type | Minimum | Recommended for Optimization |
|---|---|---|
| RLSA (Search) / YouTube remarketing | 1,000 users | 5,000+ |
| Customer Match | 1,000 matched users | 3,000+ (for Similar Audiences) |
| Display remarketing | 100 users | 1,000+ |

**Small-audience workarounds (local services):**
- Extend membership duration to 540 days and lean on geo targeting as the primary restriction.
- Use in-market/custom segments (no minimum size) to supplement or replace RLSA.
- Combine RLSA with in-market via OR logic to clear the 1,000-user threshold; if still short, accept audiences are non-viable and optimize keywords + geo.

---

## Audience Strategy Checklist

- [ ] Google tag installed; RLSA segments created (all visitors, high-intent, converters)
- [ ] Customer Match lists uploaded and refreshed on cadence (weekly for pipeline, monthly for customers)
- [ ] All Search campaigns have RLSA + Customer Match in **observation mode** (verify mode)
- [ ] Bid adjustments set after 2-4 week learning period; re-evaluated monthly
- [ ] Exclusions active: existing customers, recent converters, employees, DQ leads
- [ ] Dynamic remarketing configured (e-commerce): product feed + tag with product IDs
- [ ] PMax has audience signals (best-customer Customer Match is the top signal)
- [ ] Display/YouTube use targeting mode with defined audiences and frequency caps
- [ ] Healthcare: HIPAA verified — no patient data in any list
- [ ] Local: list sizes checked vs 1,000-user minimum; geo is primary restriction
