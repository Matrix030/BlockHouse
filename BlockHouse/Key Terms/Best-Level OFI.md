**Layman's Terms**: Only looking at the very front of the auction line - just the highest bidder and lowest seller.

**Technical Detail**:
- Uses only level 0: `bid_px_00`, `ask_px_00`, `bid_sz_00`, `ask_sz_00`
- Formula: `OFI = (changes in bid flow) - (changes in ask flow)`
- This is what most basic trading algorithms use
- **In your code**: You'll calculate this using only the `_00` columns

**Why it matters**: Simple but misses a lot of information. Like judging a restaurant's popularity by only looking at the first person in line.

## Understanding Best-Level OFI

**What it measures:** The imbalance between buy and sell pressure at the best bid/ask prices over time.

**The logic:**

1. Track changes in bid and ask sizes at level 00
2. When bid size increases = buying pressure
3. When ask size decreases = buying pressure
4. When ask size increases = selling pressure
5. When bid size decreases = selling pressure

## Step 1: Understanding the Input Variables

From your CSV data, we use these key columns:

- `bid_px_00`: Best bid price (highest price buyers are willing to pay)
- `ask_px_00`: Best ask price (lowest price sellers are willing to accept)
- `bid_sz_00`: Size (number of shares) at the best bid price
- `ask_sz_00`: Size (number of shares) at the best ask price
## Step 2: The Order Flow Logic

The code calculates two flows for each market update:

### Bid Flow Calculation (`bid_flow`)

```python
# For row i, comparing with previous row (i-1):
current = df_copy.iloc[i]
previous = df_copy.iloc[i-1]

if current['bid_px_00'] > previous['bid_px_00']:
    # Price improved (went up), count full size
    df_copy.loc[i, 'bid_flow'] = current['bid_sz_00']
elif current['bid_px_00'] == previous['bid_px_00']:
    # Same price, count size change
    df_copy.loc[i, 'bid_flow'] = current['bid_sz_00'] - previous['bid_sz_00']
elif current['bid_px_00'] < previous['bid_px_00']:
    # Price got worse (went down), count negative full size
    df_copy.loc[i, 'bid_flow'] = -previous['bid_sz_00']
```
**What this means:**

- **Price went up** (`bid_px_00` increased): Someone placed a better bid, so `bid_flow` = new `bid_sz_00`
- **Same price** (`bid_px_00` unchanged): Size changed at same price, so `bid_flow` = change in `bid_sz_00`
- **Price went down** (`bid_px_00` decreased): Best bid got worse, so `bid_flow` = negative of old `bid_sz_00`

### Ask Flow Calculation (`ask_flow`)
```python
if current['ask_px_00'] < previous['ask_px_00']:
    # Price improved (went down), count full size
    df_copy.loc[i, 'ask_flow'] = current['ask_sz_00']
elif current['ask_px_00'] == previous['ask_px_00']:
    # Same price, count size change
    df_copy.loc[i, 'ask_flow'] = current['ask_sz_00'] - previous['ask_sz_00']
elif current['ask_px_00'] > previous['ask_px_00']:
    # Price got worse (went up), count negative full size
    df_copy.loc[i, 'ask_flow'] = -previous['ask_sz_00']
```

**What this means:**

- **Price went down** (`ask_px_00` decreased): Someone placed a better ask, so `ask_flow` = new `ask_sz_00`
- **Same price** (`ask_px_00` unchanged): Size changed at same price, so `ask_flow` = change in `ask_sz_00`
- **Price went up** (`ask_px_00` increased): Best ask got worse, so `ask_flow` = negative of old `ask_sz_00`

## Step 3: The OFI Formula
```python
df_flows['ofi_instant'] = df_flows['bid_flow'] - df_flows['ask_flow']
```

**OFI = bid_flow - ask_flow**
**Interpretation:**
- **Positive OFI**: More buying pressure than selling pressure
- **Negative OFI**: More selling pressure than buying pressure
- **Zero OFI**: Balanced pressure

Step 4: Real Example from Your Data

**For Row 1:**
- `current['bid_px_00']` = 233.67
- `previous['bid_px_00']` = 233.67
- Since prices are equal: `bid_flow = 141 - 139 = 2`
- `current['ask_px_00']` = 233.74
- `previous['ask_px_00']` = 233.74
- Since prices are equal: `ask_flow = 200 - 200 = 0`
- `ofi_instant = bid_flow - ask_flow = 2 - 0 = 2`

**Meaning**: 2 more shares of buying interest appeared at the best bid, creating slight buying pressure.

## Step 5: Time Aggregation
```python
ofi_aggregated = df_flows_indexed['ofi_instant'].resample(f'{time_window_seconds}S').sum()
```

This sums up all the `ofi_instant` values within each time window (e.g., 60 seconds).

**Example**: If in one minute we had:
- `ofi_instant` values: [2, -5, 10, 0, -3, 8]
- `best_level_ofi` for that minute = 2 + (-5) + 10 + 0 + (-3) + 8 = 12

## Why This Logic Makes Sense
1. **Bid improvements** (higher `bid_px_00`) show aggressive buying → positive flow
2. **Ask improvements** (lower `ask_px_00`) show aggressive selling → positive flow
3. **Size increases** at same price show passive interest → positive change
4. **Size decreases** at same price show orders being removed → negative change

The final OFI tells you the **net order flow imbalance** - whether there was more buying or selling pressure in that time period, and by how much.

**Key insight**: We're not just counting trades, but tracking all the "pressure" from orders being added, removed, and improved at the best bid/ask levels.

