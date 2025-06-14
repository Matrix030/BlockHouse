**Layman's Terms**: Looking deeper into the auction - seeing not just the front person, but the next 9 people in line on both buy and sell sides.

**Technical Detail**:
- Uses levels 0-9: all the `bid_px_00` through `bid_px_09` columns
- You calculate OFI for each level separately
- **In your code**: You'll create 10 different OFI calculations, one for each depth level
- The paper shows deeper levels often contain more predictive information than just the best level

**Why it matters**: A stock might look stable at the best level, but have massive orders building up deeper in the book, signaling a big move coming.