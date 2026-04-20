# Competitive SERP Audit

Analyze how your ads and competitors appear on Google Search for target queries across locations and devices.

## Audit Target

$ARGUMENTS — target queries and optional location/device settings (e.g., "energy assistance programs, bill help california" or "solar rebates location:San Francisco device:mobile")

## Workflow

1. **Resolve account** (if provided): Call `list_accounts` and resolve to customer_id
2. **Get impression share data** (if account resolved): Call `get_competitive_position` to understand current competitive standing

3. **Parse audit parameters** from $ARGUMENTS:
   - **Queries**: comma-separated search terms to audit
   - **Location**: geographic target (default: United States)
   - **Device**: mobile or desktop (default: desktop)

4. **Run SERP audits** (choose method based on available tools):

   **Option A — Browser-based audit** (detailed, slower):
   - For 1-5 queries: Call `audit_serp` for each query
   - For 6+ queries: Call `audit_serp_multi` with all queries and locations
   - Returns: actual ad placements, competitor ad copy, organic results

   **Option B — Serper API** (fast, if API key configured):
   - Call `serp_search_batch` with all queries
   - Returns: organic results, paid ads, People Also Ask, related searches

5. **Analyze competitive landscape**:

   **Ad Presence Table** (per query):
   | Query | Position | Your Ad? | Top Competitors | Ad Copy Theme |
   |---|---|---|---|---|

   **Competitor Analysis**:
   - Which competitors appear most frequently?
   - What messaging themes do they use?
   - What offers/CTAs are common? (%, free, call now)
   - Are competitors using ad extensions effectively?

   **Organic vs. Paid Overlap**:
   - Queries where you rank organically AND run ads (potential savings)
   - Queries where competitors dominate both organic and paid

6. **Strategic recommendations**:
   - Queries where you're not showing but competitors are
   - Ad copy improvements based on competitor messaging gaps
   - Extension opportunities competitors are using that you aren't
   - Budget allocation suggestions based on competitive intensity
   - Queries to consider pausing (dominated by competitors, high CPC, low conversion potential)

## Example Usage

```
/google-ads:competitive-audit "energy assistance" "bill help" "CARE program"
/google-ads:competitive-audit "solar panel rebates" location:California device:mobile
```
