# Audience Analysis

Analyze audience segment performance across RLSA, Customer Match, in-market, and remarketing audiences.

## Target Account

$ARGUMENTS — account name or customer ID (e.g., "Client A" or "1112223333")

## Workflow

1. **Resolve account**: Call `list_accounts` and resolve to customer_id
2. **Get currency**: Call `get_account_currency`

3. **Fetch audience data**: Call `get_audience_performance` with days=30
4. **Fetch campaign context** (in parallel): Call `get_campaign_performance` for overall account benchmarks

5. **Analyze audience segments**:

   **Audience Performance Table** (sorted by conversions):
   | Audience Segment | Type | Campaigns | Impr | Clicks | CTR | Cost | Conv | Conv Rate | CPA |
   |---|---|---|---|---|---|---|---|---|---|

   **Segment Type Breakdown**:
   - **Remarketing (RLSA)**: How do returning visitors perform vs. new?
   - **Customer Match**: Are existing customer lists driving conversions?
   - **In-Market**: Which purchase-intent audiences convert best?
   - **Affinity**: Are interest-based audiences adding value?
   - **Similar/Lookalike**: How do expanded audiences compare?

6. **Benchmark comparison**:
   - Compare audience CTR and conversion rate against account averages
   - Flag audiences performing 2x+ above average (scale opportunity)
   - Flag audiences performing 50%+ below average (reduce or exclude)
   - Calculate incremental value of each audience segment

7. **Recommendations** based on the audiences skill methodology:
   - Audiences to increase bids on (high conversion rate, low impression share)
   - Audiences to decrease bids on or exclude (high cost, low conversion)
   - New audience suggestions based on converting segments
   - RLSA strategy: which search campaigns benefit most from audience layering
   - Customer Match: recommendations for list refresh frequency
