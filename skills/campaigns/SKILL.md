---
name: google-ads-campaigns
description: Google Ads campaign structure framework. Use when building campaigns, naming conventions, ad group structure, RSA strategy, Shopping campaigns, or extensions setup.
---

# Google Ads Campaign Structure Framework — Multi-Vertical

Campaign naming conventions, ad group structure, RSA strategy, Shopping setup, LSAs, and extensions across B2B, healthcare, e-commerce, local services, and financial services verticals.

---

## Campaign Naming Convention

Consistent naming lets you filter, report, and optimize without guessing what each campaign does.

### Standard Format

```
{Region}_{Vertical}_{Theme}_{CampaignType}_{Language}_{MatchType}_{Device}
```

### Components

| Element | Options | Examples |
|---|---|---|
| **Region** | US, EU, UK, APAC, Global | US |
| **Vertical** | B2B, Healthcare, Ecomm, Local, Financial | B2B |
| **Theme** | Brand, NonBrand, Competitor, RLSA, Content, DSA, Shopping | NonBrand |
| **Campaign Type** | Search, PMax, Display, Video, DemandGen, Shopping, LSA | Search |
| **Language** | EN, FR, DE, ES, Multi | EN |
| **Match Type** | Exact, Phrase, Broad, Mixed, N/A (for Shopping/PMax/LSA) | Exact |
| **Device** | AllDevices, Desktop, Mobile | AllDevices |

### Examples

```
US_B2B_NonBrand_Search_EN_Exact_AllDevices
US_Ecomm_Shopping_PMax_EN_NA_AllDevices
US_Local_Services_LSA_EN_NA_AllDevices
US_Healthcare_Brand_Search_EN_Phrase_AllDevices
```

### Ad Group Naming

```
{keyword theme}
```

Describe the keyword cluster it contains. Examples: `data enrichment software`, `orthopedic surgeon near me`, `emergency plumber`, `running shoes sale`.

### Ad Label Naming

```
{angle}-{start date}
```

Label every ad with messaging angle and launch date for performance analysis and creative rotation. Examples: `pain-callout-15Jan2025`, `social-proof-01Feb2025`, `competitor-contrast-01Apr2025`.

---

## Ad Group Structure

### SKAG vs Themed Ad Groups

**SKAGs (Single Keyword Ad Groups) are outdated.** Close variant matching and RSA combinations make them unnecessary and create management overhead.

**Use themed ad groups instead:**

| Approach | Keywords Per Ad Group | When to Use |
|---|---|---|
| **Themed Ad Groups** | 3-5 keywords sharing the same intent | Default approach for all campaigns |
| **Single Theme, Multiple Variations** | 3-5 variations of the same core concept | When keyword volume is high enough |
| **SKAG** | 1 keyword | Only for extremely high-value keywords where you need full control |

### How to Theme Ad Groups

Group keywords by **shared intent** - all keywords in one ad group should trigger the same ad and send users to the same landing page.

**Good Ad Group Theme:** `data enrichment software`
```
[data enrichment software]
[data enrichment platform]
[data enrichment tool]
"data enrichment software"
"data enrichment platform"
```
All these keywords have the same intent. One set of ads and one landing page works for all.

**Bad Ad Group Theme:** `data tools` (too broad)
```
[data enrichment software]
[data visualization tool]
[data warehouse platform]
```
These have completely different intents. You can't write one ad that matches all three.

### Ad Group Composition

| Element | Quantity | Notes |
|---|---|---|
| **Keywords** | 3-5 per ad group | Same intent, different variations |
| **RSAs** | 2-3 per ad group | Different messaging angles (pain, benefit, proof) |
| **Extensions** | All types at campaign level | Sitelinks, callouts, snippets, images |
| **Landing Page** | 1 per ad group | Must match keyword intent |

---

## Shopping Campaign Structure (E-commerce)

### Product Group Strategy

Organize product groups in a hierarchy that gives you granular bidding control:

```
All Products
  └─ Brand
       └─ Category
            └─ Product Type
                 └─ Item ID (optional, for top sellers)
```

| Level | Purpose | Example |
|---|---|---|
| **Brand** | Separate owned vs third-party brands | Nike, Adidas, Generic |
| **Category** | Google product taxonomy | Apparel > Shoes > Athletic |
| **Product Type** | Your internal categorization | Running Shoes, Trail Shoes, Casual Shoes |
| **Item ID** | Individual SKU control | SKU-12345 (use only for top 10-20% sellers) |

**Rules:**
- Always subdivide "Everything else" at each level to avoid catch-all groups eating budget
- Exclude low-margin or out-of-stock products at the product group level
- Use custom labels in your feed to create segments like "Best Sellers", "Clearance", "High Margin"

### Feed Optimization Basics

Your Shopping performance is only as good as your product feed. Optimize these fields:

| Field | Best Practice |
|---|---|
| **Title** | Front-load the most important keywords. Format: Brand + Product Type + Key Attributes (size, color, material). Max 150 chars, first 70 are most visible. |
| **Description** | Include relevant search terms naturally. Highlight key features, materials, use cases. Max 5,000 chars. |
| **Images** | White background, high resolution (800x800+), show the product clearly. Add lifestyle images as additional_image_link. |
| **Price** | Must match landing page price exactly. Use sale_price for promotions. |
| **GTIN/MPN** | Always include when available. Products with GTINs get priority in auctions. |
| **Product Type** | Use your own category taxonomy (separate from google_product_category). Be specific. |
| **Availability** | Keep in sync with actual inventory. Out-of-stock items waste crawl budget. |

### Standard Shopping vs Performance Max Shopping

| Factor | Standard Shopping | PMax Shopping |
|---|---|---|
| **Control** | Full control over bids, negatives, product groups | Limited control, Google AI optimizes |
| **Query visibility** | Full search term reports | Limited search term visibility |
| **When to use** | When you need granular control, new accounts, tight budgets | When you have 50+ conversions/month and trust Google's AI |
| **Negatives** | Full negative keyword support | Account-level negatives only |
| **Best for** | Experienced advertisers, complex catalogs | Scaling proven products, broad catalogs |

### Priority Campaign Strategy (Query Funneling)

Use campaign priorities to funnel search queries through campaigns with different bids:

| Campaign | Priority | Bid | Negatives | Purpose |
|---|---|---|---|---|
| **Catch-All (Generic)** | High | Low bid ($0.30-0.50) | None | Catches all queries first at low cost |
| **Category (Mid-Intent)** | Medium | Medium bid ($0.50-1.00) | Generic terms from catch-all | Catches category-specific queries |
| **Product (High-Intent)** | Low | High bid ($1.00+) | Generic + category terms | Only fires for specific product queries |

**How it works:** Google checks priority first, then negatives. A high-priority campaign fires first, but if the query matches a negative keyword, it falls through to the next priority level. This lets you bid low on generic queries and high on specific product searches.

---

## Local Services Ads (LSA) — Summary

Pay-per-lead format for service businesses; appears above Search ads. Badge is essentially required for visibility.

| Category | Service Areas | Lead Types | Typical CPL / Budget | Badge |
|---|---|---|---|---|
| **Home Services** (plumber, electrician, HVAC, roofer) | ZIP / city / radius — start tight | Calls, Messages, Bookings | $5-30 CPL; moderate weekly budget, scale on quality | Google Guaranteed (background + license + insurance; up to $2K customer guarantee) |
| **Legal / Financial / Real Estate** | City / metro | Calls, Messages, Bookings | $50-150+ CPL; higher weekly cap | Google Screened (background + license + credentials) |
| **Healthcare / Wellness** (select categories) | City / metro | Calls, Bookings | Varies; often $20-60 | Google Screened |

Rules: select ALL relevant service types you actually serve; location option by ZIP/city/radius; respond within 5 minutes (impacts rank); dispute invalid leads within 30 days; reviews directly affect ranking.

---

## RSA Best Practices

### Headline Strategy (15 Slots Available)

Use all 15 headline slots. Give Google enough options to test while maintaining control over key positions.

**Headline Allocation:**

| Position | Purpose | Pinning | Example |
|---|---|---|---|
| **Headline 1** | Keyword relevance | Pin to Position 1 | "Enterprise Data Enrichment Platform" |
| **Headline 2** | Value proposition | Pin to Position 2 | "Enrich CRM Data in Real-Time" |
| **Headline 3** | CTA | Pin to Position 3 | "Book a Demo Today" |
| **Headlines 4-15** | Testing pool | Unpinned | Various benefits, features, social proof |

### Headline Categories to Cover

Fill your 15 headlines across these categories:

| Category | # of Headlines | Examples |
|---|---|---|
| **Keyword Match** | 2-3 | Include the primary keyword and close variants |
| **Value Proposition** | 2-3 | Main benefit, unique differentiator |
| **Social Proof** | 2-3 | "Trusted by 500+ Companies", "4.8/5 on G2" |
| **CTA** | 2-3 | "Get a Demo", "Start Free Trial", "See Pricing" |
| **Pain/Problem** | 1-2 | "Stop Wasting Time on Manual Data Entry" |
| **Feature** | 1-2 | "Real-Time API, 99.9% Uptime" |
| **Urgency/Specificity** | 1-2 | "Results in 30 Days", "Set Up in Under 1 Hour" |

### Vertical-Specific Headline Guidance

**Healthcare RSA Headlines:**

| Category | Examples |
|---|---|
| **Credentials** | "Board Certified [Specialty]", "Fellowship-Trained Surgeon" |
| **Patient Outcomes** | "98% Patient Satisfaction", "50,000+ Patients Treated" |
| **Insurance** | "Most Insurance Accepted", "We Accept Medicare" |
| **Access** | "Same-Day Appointments Available", "Telehealth Visits Offered" |
| **Compliance** | No health guarantees, no "cure" language, no before/after claims without disclaimers |

**E-commerce RSA Headlines:**

| Category | Examples |
|---|---|
| **Price/Value** | "Starting at $X", "Save X% Today", "Best Price Guaranteed" |
| **Shipping** | "Free Shipping Over $X", "Ships Today", "2-Day Delivery" |
| **Reviews** | "4.8/5 Stars", "10,000+ Reviews", "Award-Winning Products" |
| **Urgency** | "Limited Stock", "Sale Ends [Date]", "While Supplies Last" |
| **Returns** | "Free 30-Day Returns", "Hassle-Free Exchanges" |

**Local Services RSA Headlines:**

| Category | Examples |
|---|---|
| **Service Area** | "Serving [City/Region]", "Your Local [Service] Experts" |
| **Response Time** | "Same-Day Service Available", "24/7 Emergency Service" |
| **Trust** | "Licensed & Insured", "20+ Years Experience", "Family-Owned" |
| **Reviews** | "5-Star Rated on Google", "500+ Happy Customers" |
| **Guarantee** | "Satisfaction Guaranteed", "Free Estimates" |

**Financial Services RSA Headlines:**

| Category | Examples |
|---|---|
| **Credentials** | "Certified Financial Planner", "SEC Registered Advisor" |
| **Trust** | "Fiduciary Standard", "Fee-Only Advisor", "No Hidden Fees" |
| **Results** | "Helping Clients Since [Year]", "Managing $X+ in Assets" |
| **Access** | "Free Initial Consultation", "Virtual Meetings Available" |
| **Compliance** | No guaranteed returns, no "risk-free" claims, include required disclaimers |

### Description Strategy (4 Slots Available)

| Slot | Purpose | Example |
|---|---|---|
| **Description 1** | Expand on the primary value prop | "Automatically enrich every lead in your CRM with 50+ data points. No manual work required." |
| **Description 2** | Social proof or feature support | "Trusted by 500+ B2B companies. Integrates with Salesforce, HubSpot, and 30+ tools." |
| **Description 3** | Handle objections or add urgency | "Start with a free pilot. No credit card required. See results in your first week." |
| **Description 4** | Alternative angle or CTA | "Replace manual research with automated enrichment. Book a 15-minute demo today." |

### The "Fake ETA" Strategy

Pin headlines 1, 2, 3 AND descriptions 1, 2 to create a fully controlled ad that behaves like the old Expanded Text Ad format.

Use this as your **control ad** in each ad group. Run it alongside a true RSA (minimal pinning) as your **test ad**.

| Ad | Pinning | Purpose |
|---|---|---|
| **RSA 1 (Control - "Fake ETA")** | Pin H1, H2, H3 + D1, D2 | Predictable, controlled messaging |
| **RSA 2 (Test - Open)** | Pin H1 only, rest unpinned | Let Google optimize combinations |
| **RSA 3 (Angle B)** | Pin H1, H2 only | Different messaging theme |

### RSA Copy Rules

- **Never repeat the same message** across pinned positions (H1 and H2 should say different things)
- **Each RSA should have a unique messaging THEME** - don't create 3 identical RSAs with slightly different wording
- **Include numbers and stats** when possible - they increase CTR ("2x pipeline", "50+ integrations", "$0 setup fee")
- **Use Dynamic Keyword Insertion** {KeyWord:Data Enrichment} in one headline as a test variant
- **Write headlines that work independently** - any combination of H1 + H2 + H3 should make sense together
- **Refresh RSAs every 8-12 weeks** or when CTR declines 20%+ from baseline

---

## Ad Extensions (Assets) — Required by Type

Minimums to ship on every campaign:

- **Sitelinks:** 4 min / 6-8 best (Pricing, Product, Case Studies, About, Blog, Trial, Contact)
- **Callouts:** 4 min / 6-8 best (short benefit phrases)
- **Structured Snippets:** 1 min / 2-3 best
- **Image Extensions:** 1 min / 3-5 best
- **Call Extensions:** critical for Healthcare and Local; optional elsewhere
- **Location Extensions:** critical for Local; important for Healthcare
- **Price Extensions:** always for E-commerce; when transparent elsewhere
- **Promotion Extensions:** always during E-commerce sales; seasonal for Local
- **Lead Form Extensions:** optional test

Apply sitelinks/callouts/snippets at account level when cross-campaign; campaign level when theme-specific; ad-group level only when strictly necessary.

---

## Campaign Settings Checklist

Before launching any new Google Ads campaign, verify every setting:

### Network Settings
- [ ] Search Network: ON
- [ ] Display Network: OFF (create separate Display campaigns)
- [ ] Search Partners: OFF (turn on only after 60+ days if you want to test)

### Location Settings
- [ ] Correct geographic targeting set
- [ ] Location option: "Presence: People in or regularly in your targeted locations"
- [ ] NOT "Presence or Interest" (this is the default and it's wrong for most verticals)

### Language Settings
- [ ] Correct language selected (usually English for US campaigns)
- [ ] Separate campaigns for each language if targeting multiple

### Bidding Settings
- [ ] Appropriate bid strategy for account maturity
- [ ] Bid cap set if using Maximize Clicks
- [ ] Target CPA set if using Maximize Conversions (based on real data)

### Budget Settings
- [ ] Daily budget set and realistic for keyword CPCs
- [ ] Monthly budget calculated (daily x 30.4)
- [ ] Delivery method: Standard (not accelerated)

### Ad Rotation
- [ ] "Do not optimize" during testing phase (first 4-6 weeks)
- [ ] "Optimize: Prefer best performing ads" after testing phase

### Ad Schedule
- [ ] Business hours set (M-F, 7am-7pm as default for B2B; 7 days for healthcare/local/e-commerce)
- [ ] Weekend/night bid adjustments applied after data collection

### Conversion Tracking
- [ ] Primary conversion action selected
- [ ] Conversion tracking verified and firing
- [ ] Attribution model set correctly
- [ ] Conversion window appropriate for sales cycle

### Vertical-Specific Settings
- [ ] Google Business Profile linked (healthcare, local services)
- [ ] Call tracking configured with appropriate duration threshold
- [ ] Shopping feed approved and healthy (e-commerce)
- [ ] Service areas defined (local services)

---

> **Built by [Xtropy](https://www.xtropy.net).** Multi-vertical campaign structure framework for agency management.
