---
name: google-ads-strategy
description: Google Ads strategy methodology for multi-vertical agency management. Use when planning campaigns, analyzing account structure, advising on keyword strategy, bidding, Quality Score, or landing page optimization.
---

# Google Ads Strategy Guide — Multi-Vertical

Campaign types, account structure, keyword strategy, bidding, Quality Score, and landing page mapping across verticals: B2B, healthcare, e-commerce, local services, and home services.

---

## Identify the Client's Vertical First

Before applying any framework, classify the client:

| Vertical | Characteristics | Primary Conversion | Typical Budget |
|---|---|---|---|
| **B2B SaaS/Tech** | Long sales cycle, high ACV, CRM pipeline | Demo request, trial signup, offline MQL/SQL | $5K-$30K/mo |
| **Healthcare/Medical** | Procedure-based, phone calls critical, HIPAA compliance | Appointment request, phone call (120s+) | $2K-$32K/mo |
| **E-commerce/Retail** | Transaction-based, ROAS-driven, product feed | Purchase, add to cart | $2K-$15K/mo |
| **Local Services** | Service area, phone calls, seasonal | Phone call (60s+), contact form | $1K-$10K/mo |
| **Home Services** | Project-based, estimate requests, seasonal | Contact form, phone call | $1K-$5K/mo |
| **Financial Services** | Compliance constraints, trust-driven, local/regional | Application, phone call | $4K-$15K/mo |

This classification drives campaign types, keyword strategy, bidding, landing pages, and conversion tracking.

---

## Campaign Types by Vertical

### The Priority Ladder

Start at the bottom (highest intent), prove it works, then climb up. Sequence varies by vertical.

**B2B SaaS/Tech:**
```
LEVEL 1: Brand Search + High-Intent NonBrand Search
LEVEL 2: Medium-Intent NonBrand + Competitor Search
LEVEL 3: RLSA + Display Remarketing
LEVEL 4: Performance Max + YouTube Remarketing
LEVEL 5: YouTube Prospecting + Demand Gen
```

**E-commerce/Retail:**
```
LEVEL 1: Shopping/PMax (product feed) + Brand Search
LEVEL 2: NonBrand Search (category + product keywords)
LEVEL 3: Dynamic Remarketing (product-level retargeting)
LEVEL 4: Display Prospecting + YouTube
LEVEL 5: Demand Gen + Broad Match expansion
```

**Healthcare/Medical:**
```
LEVEL 1: Brand Search + Procedure/Condition Search
LEVEL 2: Symptom Keywords + "Near Me" Search
LEVEL 3: PMax (with geo constraints)
LEVEL 4: Display Remarketing (shorter windows)
LEVEL 5: YouTube Awareness (with compliance review)
```

**Local Services & Home Services:**
```
LEVEL 1: Brand Search + Service + Location Search
LEVEL 2: "Near Me" + Emergency/Urgent Keywords
LEVEL 3: Local Services Ads (if eligible)
LEVEL 4: PMax (tight geo radius)
LEVEL 5: Display Remarketing + Seasonal campaigns
```

**Do NOT jump levels before the previous level is profitable.** Most common Google Ads mistake across all verticals.

### When to Use Each Campaign Type

| Campaign Type | B2B | Healthcare | E-commerce | Local Services | Financial |
|---|---|---|---|---|---|
| **Search** | Always #1 | Always #1 | #2 after Shopping | Always #1 | Always #1 |
| **Shopping** | Rarely | N/A | #1 priority | N/A | N/A |
| **Brand Search** | Run alongside | Run alongside | Run alongside | Run alongside | Run alongside |
| **Competitor Search** | After core Search | After core Search | After Shopping | Rarely | After core Search |
| **RLSA** | After pixel data | After pixel data | After pixel data | If list size allows | After pixel data |
| **Performance Max** | After 50+ conv/mo | After geo setup | After feed ready | After Search proves | After 50+ conv/mo |
| **Display Remarketing** | Supplement | Short windows | Dynamic product | If list size allows | Supplement |
| **Local Services Ads** | N/A | If eligible | N/A | If eligible | N/A |
| **YouTube** | Brand/awareness | Compliance required | Product demos | Rarely | Trust building |
| **Demand Gen** | After Search maxed | Carefully | After Search maxed | Rarely | After Search maxed |

---

## Account Structure by Intent Level

### The Intent Pyramid (Universal)

Structure your account around intent levels, not product lines. Applies across all verticals.

```
Account
├── Brand (Highest Intent)
│   └── Ad Groups: Brand exact, Brand + product/service
├── High-Intent NonBrand (Solution-Aware)
│   └── Ad Groups: [product/service] specific terms
├── Medium-Intent NonBrand (Problem-Aware)
│   └── Ad Groups: [problem/need] related terms
├── Low-Intent NonBrand (Category-Aware)
│   └── Ad Groups: informational, "what is", "how to"
├── Competitor (Variable Intent)
│   └── Ad Groups: [competitor] alternatives, vs
└── RLSA / Remarketing
    └── Ad Groups: Broader keywords, only for known visitors
```

### Expected Metrics by Vertical and Intent Level

**B2B SaaS/Tech:**

| Intent Level | Expected CTR | Expected CPC | Expected CVR | Target CPA Range |
|---|---|---|---|---|
| Brand | 8-15% | $0.50-3 | 15-30% | $5-20 |
| High-Intent NonBrand | 3-8% | $8-40 | 3-8% | $50-200 |
| Medium-Intent NonBrand | 2-5% | $5-25 | 1-4% | $100-400 |
| Competitor | 2-5% | $10-50 | 1-3% | $100-500 |
| RLSA | 4-10% | $3-20 | 5-12% | $30-150 |

**Healthcare/Medical:**

| Intent Level | Expected CTR | Expected CPC | Expected CVR | Target CPA Range |
|---|---|---|---|---|
| Brand | 8-15% | $1-5 | 10-25% | $10-40 |
| Procedure/Condition | 3-7% | $3-25 | 3-8% | $50-300 |
| Symptom Keywords | 2-5% | $2-15 | 1-4% | $80-400 |
| "Near Me" | 4-8% | $3-20 | 5-12% | $30-150 |

**E-commerce/Retail:**

| Intent Level | Expected CTR | Expected CPC | Expected CVR | Target ROAS |
|---|---|---|---|---|
| Brand | 8-15% | $0.30-2 | 5-15% | 800%+ |
| Shopping/PMax | 1-3% | $0.50-5 | 1-4% | 300-600% |
| Product Keywords | 3-7% | $0.50-5 | 2-5% | 300-500% |
| Category Keywords | 2-5% | $0.30-3 | 1-3% | 200-400% |

**Local Services & Home Services:**

| Intent Level | Expected CTR | Expected CPC | Expected CVR | Target CPA Range |
|---|---|---|---|---|
| Brand | 8-15% | $0.50-3 | 10-25% | $5-20 |
| Service + Location | 4-8% | $2-15 | 5-15% | $20-100 |
| "Near Me" / Emergency | 5-10% | $3-20 | 8-20% | $15-80 |
| General Service | 2-5% | $2-10 | 2-6% | $50-200 |

---

## Keyword Strategy

### Match Types — When to Use Each

| Match Type | Symbol | Behavior | When to Use | Risk |
|---|---|---|---|---|
| **Exact** | [keyword] | Only that query + close variants | Always start here | Low |
| **Phrase** | "keyword" | Query must contain keyword in order | After exact proves profitable | Medium |
| **Broad** | keyword | Any query Google considers related | Last phase only. Requires Smart Bidding + 30+ conv/mo | High |

### Match Type Progression

```
Phase 1: Exact Match Only
├── Launch with exact match keywords
├── Collect 2-4 weeks of data
├── Identify converting keywords
└── Build negative keyword list from Search Terms Report

Phase 2: Add Phrase Match
├── Add phrase match for proven exact match winners
├── Continue aggressive Search Terms auditing
└── Expect 20-40% more volume at slightly higher CPA

Phase 3: Broad Match (Conditional)
├── ONLY if: 30+ conversions/month, Smart Bidding active, audiences layered
├── Monitor Search Terms Report DAILY for first 2 weeks
└── Remove audience constraints only with strong 90-day history
```

### Negative Keywords by Vertical

Add these before launching.

**Universal (all verticals):**
```
jobs, careers, salary, hiring, internship, resume, interview
```

**B2B additions:**
```
free, cheap, discount, open source, freemium
training, course, tutorial, certification, bootcamp
template, example, PDF, download, reddit, quora, wiki
personal, consumer, DIY, homemade
```

**Healthcare additions:**
```
home remedy, natural cure, DIY treatment
pictures, images, photos, video of [procedure]
symptoms of (unless targeting symptom keywords)
reddit, quora, forum, wiki
nursing school, medical school, residency
```

**E-commerce additions:**
```
free, DIY, how to make, how to build, plans
used, refurbished (unless you sell these)
repair, fix, troubleshoot (unless you offer services)
recall, lawsuit, dangerous
```

**Local Services additions:**
```
DIY, how to, tutorial, YouTube
jobs, hiring, salary, career
cheap, cheapest, free estimate (unless you offer free estimates)
license, certification, training (unless relevant)
```

### Keyword Optimization Rules

**Non-Performer Rule:** Keyword has NEVER converted + spent 3x target CPA (all-time data) = pause it. No exceptions.

**Ongoing Optimization Rule:** Keyword HAS converted historically but CPA is above target in last 90-day window = reduce bid or pause.

**Don't panic-pause** on a single bad week. Use minimum 30 days of data for new keywords before cut/keep decisions.

---

## Bidding Strategy by Vertical

### Bidding Progression

**B2B SaaS/Tech (low conversion volume):**
```
STAGE 1: Manual CPC or Maximize Clicks (with bid cap) — first 2-4 weeks
STAGE 2: Maximize Conversions (with target CPA) — after 30+ conversions/month
STAGE 3: Maximize Conversion Value (with target ROAS) — after 6+ months of offline data
```

**Healthcare/Medical (moderate volume, phone calls):**
```
STAGE 1: Maximize Conversions — from launch (phone calls provide faster signal)
STAGE 2: Target CPA — after 30+ conversions/month
Note: Phone call conversions accelerate Smart Bidding learning
```

**E-commerce/Retail (high volume, transaction data):**
```
STAGE 1: Maximize Conversion Value or Target ROAS — from launch if history exists
STAGE 2: Target ROAS refinement — ongoing
Note: Revenue data available immediately, so value-based bidding works faster
```

**Local Services & Home Services (moderate volume):**
```
STAGE 1: Maximize Conversions — from launch
STAGE 2: Target CPA — after 15-30 conversions/month
Note: Simpler funnel = faster Smart Bidding learning
```

### Smart Bidding Requirements

| Requirement | B2B | Healthcare | E-commerce | Local |
|---|---|---|---|---|
| Min conversions/month for tCPA | 30+ | 15-30 (calls help) | 30+ | 15-30 |
| Min conversions/month for tROAS | 50+ | N/A (rarely used) | 50+ | N/A |
| Conversion delay tolerance | Up to 90 days | 1-7 days | Immediate | 1-7 days |
| Primary conversion action | Demo/MQL | Appointment/Call | Purchase | Call/Form |

### When to Stay on Manual CPC

- Fewer than 15 conversions per month
- Conversion delay is 30+ days (B2B only)
- Running competitor campaigns (Smart Bidding struggles with low QS)
- Highly seasonal with dramatic volume swings
- Need tight cost control during a test phase
- Micro-budget accounts (<$500/mo) where every click matters

### Bid Adjustments by Vertical

| Dimension | B2B | Healthcare | E-commerce | Local |
|---|---|---|---|---|
| **Desktop** | +10-20% | +0% | +0% | -10% |
| **Mobile** | -20-30% | +0-10% (calls!) | +0% | +10-20% (calls!) |
| **Tablet** | +0% | -30-50% | -20% | -50-100% |
| **Business Hours** | +0% | +0% | N/A | +0% |
| **Nights/Weekends** | -50-100% | -30% (urgent: +0%) | +0% | -30-50% |

*Bid adjustments are mostly ignored under Smart Bidding. Device is the only one partially respected under tCPA.*

---

## Quality Score Optimization

### Quality Score Components (1-10 Scale)

| Component | Weight | What Google Measures | How to Improve |
|---|---|---|---|
| **Expected CTR** | ~35% | How likely users click your ad | Compelling headlines, strong CTAs, ad extensions |
| **Ad Relevance** | ~25% | How well ad matches search query | Keywords in headlines, tight ad group themes |
| **Landing Page Experience** | ~40% | How useful your landing page is | Message match, fast load (<3s), mobile-friendly, clear CTA |

### Why Quality Score Matters

```
Ad Rank = Max Bid x Quality Score x Expected Impact of Extensions
```

QS 10 with a $5 bid beats QS 4 with a $10 bid. At $2K-$15K/mo budgets, a 1-point QS improvement saves hundreds per month.

**QS Impact on CPC:**

| Quality Score | CPC Impact |
|---|---|
| 10 | -50% vs average |
| 8-9 | -25% vs average |
| 7 | Baseline |
| 5-6 | +25% vs average |
| 3-4 | +50-100% vs average |
| 1-2 | +200%+ (often won't serve) |

---

## Landing Page Strategy by Vertical

### Landing Page Mapping

| Campaign Type | B2B | Healthcare | E-commerce | Local Services |
|---|---|---|---|---|
| **Brand** | Homepage | Homepage | Homepage | Homepage |
| **High-Intent** | Dedicated LP, no nav, single CTA | Provider/procedure page, appointment form | Product page, buy button | Service page, contact form, click-to-call |
| **Medium-Intent** | Solution page, softer CTA | Condition info page, "learn more" CTA | Category page, browse | Service area page, free estimate CTA |
| **Low-Intent** | Content/resource page | Educational content, FAQ | Blog/buying guide | Blog, seasonal tips |
| **Competitor** | Comparison LP | "Why Choose Us" page | Price comparison | Reviews/testimonials page |

### Landing Page Essentials (Universal)

- [ ] Headline matches the ad headline and keyword intent
- [ ] Primary CTA visible above the fold
- [ ] Page loads in under 3 seconds on mobile and desktop
- [ ] Mobile responsive
- [ ] Social proof visible (reviews, ratings, logos, certifications)
- [ ] Trust signals near the CTA (privacy, security, credentials)

### Vertical-Specific Requirements

**Healthcare:**
- [ ] Provider credentials displayed (Board Certified, MD, years of experience)
- [ ] Patient testimonials (HIPAA-compliant consent)
- [ ] Insurance accepted list
- [ ] Click-to-call prominently placed
- [ ] Online scheduling integration
- [ ] No health outcome guarantees (compliance)

**E-commerce:**
- [ ] Product images (multiple angles, lifestyle shots)
- [ ] Price clearly displayed
- [ ] Add-to-cart button above the fold
- [ ] Reviews and ratings visible
- [ ] Shipping info (free shipping threshold, delivery time)
- [ ] Trust badges (secure checkout, returns policy)

**Local Services:**
- [ ] Service area clearly stated
- [ ] Click-to-call button prominent
- [ ] Short contact form (name, phone, service needed, zip)
- [ ] Reviews and ratings displayed
- [ ] "Licensed & Insured" badge
- [ ] Response time / availability ("Same-Day Service")
- [ ] Gallery of completed work

---

## Budget Allocation by Budget Tier

Budget allocation depends on total monthly budget, not just vertical.

### Under $3K/month

Run Search only. Don't dilute across channels.

| Campaign | % of Budget |
|---|---|
| Brand Search | 5-10% |
| Core NonBrand Search | 85-90% |
| Reserve | 5% |

### $3K-$8K/month

| Campaign | % of Budget |
|---|---|
| Brand Search | 5-10% |
| High-Intent NonBrand Search | 50-60% |
| Competitor or Medium-Intent Search | 15-20% |
| PMax or Shopping (e-comm) | 10-15% |
| Reserve | 5-10% |

### $8K-$15K/month

| Campaign | % of Budget |
|---|---|
| Brand Search | 3-5% |
| High-Intent NonBrand Search | 35-45% |
| Medium-Intent + Competitor | 15-20% |
| PMax or Shopping | 15-20% |
| Display Remarketing | 5-10% |
| Reserve | 5-10% |

### $15K+/month

| Campaign | % of Budget |
|---|---|
| Brand Search | 3-5% |
| High-Intent NonBrand Search | 30-40% |
| Medium-Intent NonBrand | 10-15% |
| Competitor Search | 5-10% |
| PMax / Shopping | 15-20% |
| Display Remarketing | 5-10% |
| YouTube / Demand Gen | 5-10% |
| RLSA | 5% |
