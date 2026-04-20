# Keyword Research

Discover new keyword opportunities with search volume, competition, and bid estimates — plus SERP landscape analysis.

## Research Target

$ARGUMENTS — seed keywords, a landing page URL, or both, plus optional targeting (e.g., "energy assistance programs geo:US language:en" or "https://example.com/care-program")

## Workflow

1. **Resolve account** (if an account name/ID is provided):
   - Call `list_accounts` and resolve to customer_id
   - Call `get_account_currency`

2. **Generate keyword ideas**: Call `get_keyword_ideas` with:
   - `keyword_seed`: seed keywords from $ARGUMENTS
   - `url_seed`: URL if provided
   - `geo_target`: geographic target (default: US)
   - `language_id`: language (default: 1000 for English)
   - `include_adult`: false

3. **SERP landscape** (run in parallel with step 2 if Serper API key is configured):
   - Call `serp_search` for the top 3-5 seed keywords to see:
     - Current organic results
     - People Also Ask questions
     - Related searches
     - Paid ad placements (if any)
   - If many keywords, use `serp_search_batch` for efficiency

4. **Analyze and present results**:

   **Keyword Ideas Table** (sorted by search volume):
   | Keyword | Avg. Monthly Searches | Competition | Low Bid | High Bid |
   |---|---|---|---|---|

   **SERP Landscape Insights**:
   - Who's ranking organically for these terms?
   - What ad copy are competitors using?
   - People Also Ask themes (potential ad group ideas)
   - Related searches (expansion opportunities)

5. **Strategic recommendations** based on the google-ads-strategy skill:
   - Group keywords by theme/intent for ad group structure
   - Flag high-volume/low-competition opportunities
   - Identify keywords where competitors are bidding aggressively
   - Suggest match types based on intent signals
   - Estimate required budget based on bid ranges

## Example Usage

```
/google-ads:keyword-research energy bill assistance california
/google-ads:keyword-research https://example.com/savings-program geo:US
/google-ads:keyword-research "solar panel rebates" "ev charging incentives"
```
