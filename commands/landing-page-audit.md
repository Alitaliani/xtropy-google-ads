# Landing Page Audit

Crawl and analyze all landing pages used across a Google Ads account for health, speed, and ad relevance.

## Target Account

$ARGUMENTS — account name or customer ID (e.g., "Client A" or "1112223333")

## Workflow

1. **Resolve account**: Call `list_accounts` and resolve to customer_id
2. **Get currency**: Call `get_account_currency`

3. **Analyze landing pages**: Call `analyze_landing_pages` with the customer_id
   - Crawls all final URLs from active ads
   - Checks HTTP status codes (flags 404s, redirects, errors)
   - Analyzes content relevance to ad copy
   - Identifies missing elements (CTAs, forms, phone numbers)

4. **Check page speed** (run in parallel if PageSpeed API key is configured):
   - Call `check_page_speed` for the top landing pages by ad spend
   - Returns Core Web Vitals: LCP, FID, CLS
   - Returns overall performance score (0-100)
   - Returns mobile vs. desktop scores

5. **Cross-reference with campaign data**: Call `get_campaign_performance` to link landing page health to spend and conversions

6. **Present findings**:

   **Landing Page Health Summary**:
   | URL | Status | Speed Score | Mobile | LCP | CLS | Ad Spend | Conv Rate |
   |---|---|---|---|---|---|---|---|

   **Critical Issues** (fix immediately):
   - Pages returning 404 or 5xx errors
   - Pages with speed score < 50
   - Pages with LCP > 4 seconds

   **Optimization Opportunities**:
   - Pages with poor mobile experience
   - Pages missing clear CTAs
   - High-spend pages with below-average conversion rates
   - Pages with redirect chains slowing load time

7. **Recommendations** based on the strategy skill:
   - Prioritize fixes by ad spend impact
   - Suggest A/B test candidates for high-traffic pages
   - Flag pages that need mobile-specific optimization
   - Estimate potential conversion lift from speed improvements
