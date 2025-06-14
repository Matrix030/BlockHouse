**Layman's Terms**: Only looking at the very front of the auction line - just the highest bidder and lowest seller.

**Technical Detail**:
- Uses only level 0: `bid_px_00`, `ask_px_00`, `bid_sz_00`, `ask_sz_00`
- Formula: `OFI = (changes in bid flow) - (changes in ask flow)`
- This is what most basic trading algorithms use
- **In your code**: You'll calculate this using only the `_00` columns

**Why it matters**: Simple but misses a lot of information. Like judging a restaurant's popularity by only looking at the first person in line.