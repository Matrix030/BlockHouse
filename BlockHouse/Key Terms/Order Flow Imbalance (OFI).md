   **What is OFI?** Imagine a stock market like an auction house where people place bids (offers to buy) and asks (offers to sell). OFI measures the **imbalance** between buying and selling pressure at different price levels.
   
- **Analogy**:: A tug-of-war rope. OFI measures which side is pulling harder
- **Positive OFI** = More buying pressure (price likely to go up)
- **Negative OFI** = More selling pressure (price likely to go down)

**Layman's Terms**: Imagine a stock as an auction house. At any moment, there are people wanting to buy (bidders) and people wanting to sell (sellers). OFI measures whether there are more eager buyers or sellers at that moment.

**Technical Detail for This Project**: OFI is calculated by tracking how the "order book" changes. The order book shows:

- **Bid side**: People wanting to buy, with their prices and quantities
- **Ask side**: People wanting to sell, with their prices and quantities

The formula tracks when:

- New buy orders arrive → positive OFI
- Buy orders get canceled → negative OFI
- New sell orders arrive → negative OFI
- Sell orders get canceled → positive OFI

**In your CSV data**: You have columns like `bid_px_00`, `bid_sz_00` (best bid price/size) through `bid_px_09`, `bid_sz_09` (10 levels deep).

[[Best-Level OFI]]
[[Cross-Asset OFI]]
[[Integrated OFI]]
[[Multi-Level OFI]]
