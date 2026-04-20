# Bidding Strategy Audit

Evaluate bidding strategy performance and identify optimization opportunities across campaigns.

## Target Account

$ARGUMENTS — account name or customer ID (e.g., "Client A" or "1112223333")

## Workflow

1. **Resolve account**: Call `list_accounts` and resolve to customer_id
2. **Get currency**: Call `get_account_currency`

3. **Fetch bidding data** (run in parallel):
   a. Call `get_bidding_strategies` for strategy-level performance
   b. Call `get_campaign_performance` with days=30 for campaign-level metrics
   c. Call `get_device_performance` with days=30 for device bid adjustment analysis
   d. Call `get_hourly_performance` for time-of-day patterns

4. **Analyze bidding strategies**:

   **Strategy Performance Table**:
   | Strategy | Type | Campaigns | Spend | Conv | CPA | ROAS | Target Met? |
   |---|---|---|---|---|---|---|---|

   **Strategy Type Assessment**:
   - **Target CPA**: Is actual CPA within 20% of target? Enough conversion volume (30+/month)?
   - **Target ROAS**: Is actual ROAS meeting target? Sufficient conversion value data?
   - **Maximize Conversions**: Is spend efficiency declining? Should a target be added?
   - **Maximize Clicks**: Are clicks converting? Should this upgrade to conversion-based bidding?
   - **Manual CPC**: Is there enough data to switch to automated bidding?

5. **Device analysis** from the optimization skill:
   - Compare CPA by device (desktop vs. mobile vs. tablet)
   - Flag devices with CPA 2x+ above average
   - Suggest device bid adjustments

6. **Time-of-day analysis**:
   - Identify peak conversion hours
   - Flag hours with high spend but zero conversions
   - Suggest ad scheduling adjustments

7. **Recommendations** based on the optimization skill bid management rules:
   - Campaigns that should change bidding strategy (with reasoning)
   - Target CPA/ROAS adjustments based on recent performance
   - Device bid modifier suggestions with expected impact
   - Ad schedule recommendations (increase/decrease bids by hour)
   - Campaigns with insufficient conversion volume for automated bidding (< 30/month)
