### **A. Event Information (What Happened)**
- **`ts_recv`/`ts_event`**: Timestamps (when the market event occurred)
- **`action`**: What happened (A=Add order, C=Cancel order, M=Modify, T=Trade)
- **`side`**: B=Bid (buy order), A=Ask (sell order)
- **`price`**: The price of this specific order
- **`size`**: How many shares in this order
- **`symbol`**: Stock ticker (AAPL = Apple)

### **B. Order Book Snapshot (The Current State)**
This is the **key part** for OFI calculations! You have 10 levels of depth:

**For each level (00 through 09):**
- **`bid_px_XX`**: Best bid prices (what buyers are willing to pay)
- **`ask_px_XX`**: Best ask prices (what sellers want to receive)
- **`bid_sz_XX`**: Number of shares buyers want at that price
- **`ask_sz_XX`**: Number of shares sellers want at that price
- **`bid_ct_XX`**: Count of orders at that bid level
- **`ask_ct_XX`**: Count of orders at that ask level

### **C. Technical Metadata**
- **`rtype`**, **`publisher_id`**, **`instrument_id`**: System identifiers
- **`sequence`**: Order of events
- **`flags`**: Technical flags