**Layman's Terms**: How the buying/selling pressure in one stock affects the price of other stocks. Like how a run on one bank might cause panic at other banks.

**Technical Detail**:
- Uses OFI from multiple stocks simultaneously
- **In your code**: You'll need to:
    1. Calculate OFI for each stock in your dataset
    2. Use these as features to predict returns of other stocks
    3. The paper uses LASSO regression to find which stocks influence which others
- Particularly important for stocks in the same sector or with economic relationships

**Why it matters**: Modern markets are interconnected. Big institutional traders often buy/sell baskets of related stocks, creating cross-impact effects.