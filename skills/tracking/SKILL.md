---
name: google-ads-tracking
description: Google Ads conversion tracking and measurement guide. Use when setting up tracking, GTM, enhanced conversions, offline import, phone call tracking, or troubleshooting measurement issues.
---

# Google Ads Tracking Guide — Multi-Vertical Conversion Tracking and Measurement

How to set up conversion tracking, implement Google Tag Manager, configure enhanced conversions, import offline conversions via gclid, integrate with GA4, assign conversion values, select attribution models, handle phone call tracking, and troubleshoot common tracking issues across B2B, e-commerce, healthcare, and local services verticals.

---

## 1. Google Ads Conversion Tracking Setup

### 1.1 Create Conversion Actions

Go to **Google Ads - Goals - Conversions - Summary - New Conversion Action**.

**B2B SaaS/Tech — Recommended conversion actions:**

| Conversion Action | Category | Counting | Value | Priority |
|---|---|---|---|---|
| Demo request submitted | Lead/Submit form | One per click | Dynamic or fixed | Primary |
| Contact form submitted | Lead/Submit form | One per click | Dynamic or fixed | Primary |
| Free trial signup | Sign-up | One per click | Dynamic or fixed | Primary |
| Content download | Lead/Submit form | One per click | Fixed (low) | Secondary |
| Webinar registration | Lead/Submit form | One per click | Fixed (low) | Secondary |
| Phone call (60s+) | Phone call | One per click | Fixed | Secondary |
| Offline - MQL | Import | One per click | Dynamic | Primary (after setup) |
| Offline - SQL | Import | One per click | Dynamic | Primary (after setup) |
| Offline - Closed-Won | Import | One per click | Dynamic | Primary (after setup) |

**Healthcare/Medical — Recommended conversion actions:**

| Conversion Action | Category | Counting | Value | Priority |
|---|---|---|---|---|
| Appointment request | Lead/Submit form | One per click | Dynamic (procedure value) | Primary |
| Phone call (120s+) | Phone call | One per click | Fixed ($200-500) | Primary |
| Direction request | Other | One per click | Fixed (low) | Secondary |
| Insurance inquiry form | Lead/Submit form | One per click | Fixed | Secondary |

**E-commerce/Retail — Recommended conversion actions:**

| Conversion Action | Category | Counting | Value | Priority |
|---|---|---|---|---|
| Purchase | Purchase | Every (real transactions) | Dynamic (transaction value) | Primary |
| Add to cart | Other | One per click | Fixed (low) | Secondary |
| Checkout started | Other | One per click | Fixed | Secondary |
| Newsletter signup | Lead | One per click | Fixed (low) | Secondary |

**Local Services — Recommended conversion actions:**

| Conversion Action | Category | Counting | Value | Priority |
|---|---|---|---|---|
| Phone call (60s+) | Phone call | One per click | Fixed ($50-200) | Primary |
| Contact form / quote request | Lead/Submit form | One per click | Fixed ($50-200) | Primary |
| Direction click | Other | One per click | Fixed (low) | Secondary |
| Chat started | Other | One per click | Fixed (low) | Secondary |

### 1.2 Primary vs Secondary Conversions

| Classification | Effect on Bidding | When to Use |
|---|---|---|
| **Primary** | Included in Smart Bidding optimization | High-value actions you want to optimize toward |
| **Secondary** | Tracked but NOT used for bidding | Informational metrics, micro-conversions |

**B2B best practice:** Start with form submissions as primary. Once you have offline conversion import working with sufficient volume (15+ conversions per month per campaign), switch to offline MQL or SQL as primary and demote form fills to secondary.

**E-commerce best practice:** Purchase is your only primary conversion. Add to cart and checkout started are secondary — useful for diagnostics but should not drive bidding.

**Healthcare best practice:** Appointment request and phone calls are both primary. If phone calls drive the majority of appointments, weight them accordingly with higher fixed values.

**Local services best practice:** Phone calls and contact forms are both primary. If one consistently drives more revenue, assign it a higher value.

### 1.3 Conversion Window Settings

| Setting | E-commerce | B2B | Healthcare | Local Services |
|---|---|---|---|---|
| Click-through window | 30 days | 90 days | 30 days | 30 days |
| View-through window | 1 day | 1 day | 1 day | 1 day |
| Engaged-view window | 10 seconds | 10 seconds | 10 seconds | 10 seconds |

**Rationale:** B2B sales cycles are long — a click today may convert in 60 days. E-commerce, healthcare, and local services typically convert within days, so 30 days is sufficient.

---

## 2. Common Conversion Tracking Anti-Patterns

Based on auditing 43+ client accounts, these are the most common tracking mistakes:

### Anti-pattern 1: Excessive conversion actions (21-95 per account)

- **Symptom:** Account has dozens of conversion actions, many redundant or outdated
- **Impact:** Confuses Smart Bidding, inflates conversion metrics, makes reporting unreliable
- **Fix:** Audit all conversion actions. Keep 2-5 meaningful actions. Archive or remove the rest.

### Anti-pattern 2: Many/Click counting for lead generation

- **Symptom:** Lead gen conversion actions (form fills, calls) set to "Every" instead of "One"
- **Impact:** One person submitting the form twice counts as 2 conversions, inflating numbers
- **Fix:** Change ALL lead gen actions to "One per click" counting. Only "Purchase" should use "Every."

### Anti-pattern 3: Hidden conversion actions still in "Conversions" column

- **Symptom:** HIDDEN conversion actions with "Include in Conversions: Yes"
- **Impact:** Ghost conversions that inflate your numbers and corrupt Smart Bidding
- **Fix:** Filter Goals > Conversions by Status: Hidden. Set any hidden actions to "Secondary" or remove.

### Anti-pattern 4: No conversion tracking at all

- **Symptom:** Campaign spending with 0 conversions reported (but leads ARE coming in)
- **Impact:** Smart Bidding has no signal. You're flying blind.
- **Fix:** Implement at minimum: primary form submission + phone call tracking. Test end-to-end.

### Anti-pattern 5: Auto-created conversion actions from GA4 import

- **Symptom:** Google auto-imports GA4 events as conversion actions without your knowledge
- **Impact:** Random micro-events (page_view, scroll) become "conversions"
- **Fix:** Review Goals > Conversions regularly. Remove any auto-imported actions you didn't intentionally set up.

---

## 3. Google Tag Manager Implementation

**What to tag (minimum set):**
- Conversion Linker (all pages — required for gclid)
- Google Ads Remarketing (all pages)
- Google Ads Conversion — Primary (thank-you page or form success event)
- Google Ads Conversion — Phone Call (website call conversion)
- GA4 Configuration + key event tags (form_submit, purchase)

**Install:** GTM container snippet in `<head>`, `<noscript>` fallback after `<body>`. Use Preview mode to verify before publishing. See Google's GTM docs for step-by-step setup.

**Data layer keys to standardize with dev:** `form_type`, `conversion_value`, `user_email` (for enhanced conversions), `transaction_id` (e-commerce dedup).

---

## 4. Enhanced Conversions Setup

Enhanced conversions send hashed first-party data (email, phone, name, address) alongside the conversion tag so Google can match conversions to clicks when cookies are blocked. Expect 5-15% match-rate lift, higher for B2B and healthcare. Critical as cookie deprecation, ITP, and ad blockers erode standard tracking.

### 4.1 Field Mapping

| Field | Data Layer Variable | Required? |
|---|---|---|
| Email | `user_email` or auto-detected | Required (minimum) |
| Phone | `user_phone` | Recommended |
| First name | `user_first_name` | Recommended |
| Last name | `user_last_name` | Recommended |
| Street address | `user_address_street` | Optional |
| City | `user_address_city` | Optional |
| Country | `user_address_country` | Optional |

Setup methods: Google Tag auto-detect (simplest), GTM manual mapping, or API (server-side). Verify via Conversions - Diagnostics tab.

### 4.2 Enhanced Conversions for Leads

Variant for B2B/healthcare/local: form fill fires enhanced conversion with hashed email → lead progresses in CRM → offline import uploads conversion with same email → Google matches to original click → Smart Bidding optimizes for downstream revenue. Recommended for any B2B account over $5K/mo and healthcare practices tracking procedure revenue.

---

## 5. Offline Conversion Import — The gclid Pipeline

Offline conversion import sends CRM or back-office data (pipeline stage, revenue, appointment outcome) back to Google so Smart Bidding can optimize for business outcomes.

### 5.1 When You Need Offline Conversion Import

| Vertical | Offline Conversion Import Needed? | Why |
|---|---|---|
| B2B | Yes (critical) | Real value happens in CRM 30-180 days later |
| Healthcare | Optional | Phone calls + appointment forms capture most value on-site |
| E-commerce | No | Transaction tracking captures value immediately |
| Local Services | No | Phone calls + forms capture most value on-site |
| Financial | Optional | For long-cycle products (mortgages, loans) |

### 5.2 How It Works

1. **User clicks ad** — Google appends a gclid (Google Click ID) to the URL
2. **User fills out form** — Your form captures the gclid from the URL and stores it in CRM
3. **Lead progresses in CRM** — Sales qualifies, creates opportunity, closes deal
4. **You import conversions** — Upload gclid + conversion action + value + timestamp to Google Ads
5. **Smart Bidding learns** — Google now knows which clicks led to revenue, not just form fills

### 5.3 Capturing the gclid

**Step 1: Auto-tagging must be enabled** — Google Ads - Account Settings - Auto-tagging - ON.

**Step 2: Capture gclid from URL on your website** — When a user clicks an ad, the URL contains `?gclid=abc123`. JS extracts it and stores in a cookie:

```javascript
var m = window.location.href.match(/[?&]gclid=([^&#]*)/);
if (m) document.cookie = 'gclid=' + decodeURIComponent(m[1]) + '; max-age=7776000; path=/';
```

**Step 3: Pass gclid into form submission** — Hidden field populated from cookie/URL on page load. CRM receives gclid with the lead record.

### 5.4 CRM Pipeline to Google Ads Import

| CRM Stage | Google Ads Conversion Action | Conversion Value | Import Frequency |
|---|---|---|---|
| MQL (Marketing Qualified) | Offline - MQL | Fixed ($50-200) or lead score-based | Weekly |
| SQL (Sales Qualified) | Offline - SQL | Fixed ($200-1,000) or estimated deal value x probability | Weekly |
| Opportunity Created | Offline - Opportunity | Estimated deal value x stage probability | Weekly |
| Closed-Won | Offline - Closed Won | Actual revenue | Weekly |

### 5.5 Import Methods

| Method | Best For | Setup Effort |
|---|---|---|
| **Manual CSV upload** | Testing, low volume | Low - upload CSV in Google Ads UI |
| **Scheduled upload** | Regular imports via Google Sheets or SFTP | Medium |
| **Google Ads API** | Automated imports from CRM | High - requires development |
| **CRM native integration** | Salesforce, HubSpot with Google Ads connector | Medium - use platform tools |
| **Zapier/Make** | Automated triggers from CRM stage changes | Low-medium |

### 5.6 Offline Conversion Requirements

- **Minimum volume:** 15+ offline conversions per campaign per month for Smart Bidding to use effectively
- **Time lag:** Upload conversions within 90 days of the click (the sooner the better)
- **gclid format:** Must be the exact gclid captured at click time — no modifications
- **Deduplication:** Each gclid + conversion action combination should only be imported once
- **Conversion time:** Use the actual CRM stage change timestamp, not the upload time

---

## 6. GA4 Integration

Link in **GA4 - Admin - Google Ads Links**; enable auto-tagging and conversion import if desired.

| What GA4 Adds | Why It Matters |
|---|---|
| Cross-channel attribution | See Google Ads vs organic, email, social, direct |
| Multi-session user journey | Full path from first click to conversion |
| GA4 audiences → Google Ads | Behavioral RLSA (pricing visitors, cart abandoners, high-engagement) |
| Micro-event tracking | Scroll, video, engagement signals unavailable in Google Ads alone |
| Exploration reports | Custom conversion-path analysis |

---

## 7. Conversion Value Assignment

### 7.1 Static vs Dynamic Values

| Approach | How It Works | When to Use |
|---|---|---|
| **Static value** | Every conversion of a type gets the same value (e.g., demo = $500) | Starting out, single product, uniform deal size |
| **Dynamic value** | Value varies per conversion based on lead or transaction data | Multiple products, variable deal sizes, e-commerce |
| **Probability-weighted** | Value = estimated deal size x stage probability | Offline import with pipeline data (B2B) |

### 7.2 Static Value Formula

`Value per lead = Average deal/procedure value × Lead-to-close rate`, then adjust per form type by its individual close rate (e.g., demo close rate > content download close rate).

### 7.3 Value-Based Bidding

| Bidding Strategy | When to Use | Minimum Data |
|---|---|---|
| Target CPA | Optimizing for volume, uniform value | 15+ conversions/month |
| Maximize Conversion Value | Optimizing for total value, variable values | 15+ conversions with values/month |
| Target ROAS | Specific return target, mature account | 50+ conversions with values/month |

---

## 8. Attribution Model Selection

Data-driven attribution (DDA) is the default and recommended model for most accounts — Google has deprecated linear, time-decay, and position-based. Last-click remains available for simple/brand measurement; first-click for demand-gen analysis.

**Caveats:** DDA needs volume — accounts with <300 conversions / 30 days get limited accuracy. B2B's multi-touch paths mean last-click undervalues awareness campaigns. Google Ads attribution only sees Google touches — use GA4 for cross-channel. Importing offline conversions recalculates attribution and shifts credit earlier in the funnel.

---

## 9. Phone Call Tracking

### 9.1 Google Ads Call Tracking Options

| Option | What It Tracks | Setup |
|---|---|---|
| **Call extensions** | Calls from the ad directly | Add call extension with your phone number |
| **Call-only ads** | Calls from ads (no website visit) | Create call-only ad format |
| **Website call conversions** | Calls from your website after ad click | Requires Google forwarding number on your site |

### 9.2 Website Call Conversion Setup

1. Create a **Phone Call** conversion action in Google Ads
2. Set minimum call duration (see thresholds by vertical below)
3. Install the **Google forwarding number snippet** on your website
4. The snippet dynamically replaces your phone number with a Google tracking number for ad-click visitors
5. Google tracks the call, duration, and attributes it to the click

### 9.3 Call Duration Thresholds by Vertical

| Vertical | Recommended Minimum | Rationale |
|---|---|---|
| Healthcare | 120 seconds | Appointment scheduling takes time |
| Local Services | 60 seconds | Quick qualification + booking |
| B2B | 60 seconds | Initial interest qualification |
| Financial | 90 seconds | Product discussion |
| E-commerce | 30 seconds (rare) | Order status/support |

### 9.4 Healthcare Phone Call Tracking

Phone calls are often the #1 conversion for healthcare practices — many patients prefer calling over filling out forms.

- Set minimum duration to **120 seconds** (filters out short inquiry calls and wrong numbers)
- Each call may be worth **$1K-$50K** depending on the procedure
- Consider third-party call tracking (CallRail, CallTrackingMetrics) for:
  - **Call recording** — training, quality assurance, compliance review
  - **Caller identification** — match callers to patients
  - **CRM integration** — automatically log calls in practice management software
  - **DNI (dynamic number insertion)** per campaign/keyword — know which ad drove which call
- Track new patient vs. existing patient calls separately if possible

### 9.5 Local Services Phone Call Tracking

Phone calls are often more valuable than form fills for local service businesses.

- Set minimum duration to **60 seconds**
- Enable **call extensions on all campaigns** — many local searches happen on mobile
- Use Google forwarding numbers or third-party tracking
- Track calls by time of day — adjust ad schedule accordingly (e.g., if calls after 6pm never convert, reduce bids)
- Consider call-only ads for mobile campaigns — skip the website entirely

### 9.6 Third-Party Call Tracking

Use third-party platforms (CallRail, CallTrackingMetrics) when you need call recording, caller identification, CRM integration, or advanced DNI — Google native handles keyword attribution but not these.

---

## 10. HIPAA Compliance for Healthcare Tracking

Healthcare advertisers must ensure that no Protected Health Information (PHI) flows into Google Ads or any third-party tracking platform.

### 10.1 Core HIPAA Tracking Rules

- **No PHI in conversion data** — do NOT pass patient names, conditions, diagnoses, or treatment details in conversion tags, URLs, or data layer variables
- **Anonymized conversion values** — use estimated procedure category values (e.g., "dental implant = $3,000"), never actual patient billing amounts
- **No medical data in remarketing** — do not create audiences based on health conditions, treatments, or specific service pages that reveal conditions
- **Consent management** — patients must explicitly opt in before remarketing pixels fire; implement a consent management platform (CMP)

### 10.2 Google's HIPAA Position

Google Ads is **NOT** a HIPAA-covered entity. Google does not sign BAAs (Business Associate Agreements) for Google Ads. **You are responsible** for ensuring no PHI flows into Google's advertising products.

### 10.3 Enhanced Conversions and HIPAA

- **Only use hashed email** for enhanced conversions — never medical data
- Do not include any health-related information in the data layer alongside enhanced conversion fields
- Consider **server-side tracking** (server-side GTM) for additional privacy control — you can filter and sanitize data before it reaches Google

### 10.4 Practical Implementation

| What to Do | What NOT to Do |
|---|---|
| Track "appointment request submitted" as conversion | Track "knee replacement consultation requested" as conversion |
| Assign fixed value by service category | Pass actual patient billing amount |
| Remarket to "visited website" audience | Remarket to "visited diabetes treatment page" audience |
| Use hashed email for enhanced conversions | Include diagnosis codes in conversion data |
| Implement consent banner before tracking fires | Fire tracking pixels without consent |

---

## 11. Cross-Device Tracking

- Google stitches cross-device journeys via signed-in Google accounts — mobile→desktop is captured when the user is signed in on both; work-laptop→personal-device or phone→meeting-room-demo is typically lost.
- Accept that cross-device captures a portion, not all. Use GA4 cross-device reports for extra visibility; don't over-optimize on device-level data alone.

---

## 12. Troubleshooting Common Tracking Issues

### Issue Reference Table

| Issue | Symptoms | Diagnosis | Fix |
|---|---|---|---|
| No conversions tracking | Zero conversions in Google Ads despite form submissions | Check tag fires in GTM Preview; check Events in GA4 | Verify tag trigger, check conversion linker tag exists |
| Conversion count mismatch | Google Ads shows different count than CRM | Compare time ranges, check counting method (one vs every) | Align windows; use "one" counting for leads |
| gclid not captured | Offline imports fail or show zero matches | Check form hidden field; test URL parameter passing | Verify auto-tagging ON; fix JavaScript gclid capture |
| Enhanced conversions not matching | Diagnostics show low match rate | Check hashed data format; verify email field mapping | Ensure SHA-256 hashing; check field CSS selectors |
| Duplicate conversions | Same lead counted twice | Check if tag fires on page reload; check import dedup | Add one-time trigger condition; deduplicate import file |
| Conversion lag | Conversions appear days after the click | Normal for B2B — check conversion window | Extend window to 90 days; wait before evaluating |
| Tag not firing | GTM Preview shows tag did not fire | Check trigger conditions; check tag pausing | Fix trigger URL/event match; unpause tag |
| Cross-domain tracking broken | Users lost between domains | Check cross-domain config in GA4; check linker | Configure GA4 cross-domain; add all domains |
| Phone calls not tracking | Call extension shows clicks but no conversions | Check minimum duration setting; verify forwarding number | Lower duration threshold; reinstall forwarding snippet |

### Tracking Audit Checklist

Run this checklist monthly:

- [ ] Google Ads auto-tagging is ON
- [ ] Conversion linker tag fires on all pages
- [ ] All conversion actions show "Recording" status (not "No recent conversions" or "Inactive")
- [ ] No HIDDEN conversion actions with "Include in Conversions: Yes" (anti-pattern 3)
- [ ] Total conversion action count is 2-5 meaningful actions, not 20+ (anti-pattern 1)
- [ ] All lead gen actions use "One per click" counting (anti-pattern 2)
- [ ] No auto-imported GA4 events acting as primary conversions (anti-pattern 5)
- [ ] Enhanced conversions diagnostics show healthy match rate (above 50%)
- [ ] Offline import is running on schedule with zero errors (if applicable)
- [ ] GA4 link is active and importing data
- [ ] Conversion windows are set appropriately for vertical (30-90 days)
- [ ] Primary vs secondary conversion classification is correct
- [ ] Test conversion submitted and verified end-to-end in the last 30 days
- [ ] No duplicate conversion tags firing on the same page
- [ ] Phone call tracking verified with test call (if applicable)

---

## Related Files

- **google-ads-strategy-guide.md** — Campaign types, account structure, keyword strategy, bidding
- **google-ads-audience-strategy.md** — RLSA, Customer Match, audience layering, bid adjustments

---

> **Built by [Xtropy](https://www.xtropy.net).** Multi-vertical tracking and measurement guide for agency management.
