**Layman's Terms**: Looking deeper into the auction - seeing not just the front person, but the next 9 people in line on both buy and sell sides.

**Technical Detail**:
- Uses levels 0-9: all the `bid_px_00` through `bid_px_09` columns
- You calculate OFI for each level separately
- **In your code**: You'll create 10 different OFI calculations, one for each depth level
- The paper shows deeper levels often contain more predictive information than just the best level

**Why it matters**: A stock might look stable at the best level, but have massive orders building up deeper in the book, signaling a big move coming.

# Step 3: Calculating Multi-Level OFI

Now we'll extend our OFI calculation to use all 10 levels of the order book instead of just the best level. This captures deeper market dynamics.

## Understanding Multi-Level OFI

**Why go deeper?**
- Best level only shows surface activity
- Real institutional orders often sit deeper in the book
- Multi-level reveals hidden liquidity and pressure
- More predictive power for price movements

**What this step accomplished:**
1. **Analyzed depth structure** - showed how order sizes vary across the 10 levels
2. **Calculated flows for all levels** - extended the best-level logic to every depth level
3. **Individual level OFIs** - computed separate OFI for each of the 10 levels
4. **Combined approaches** - tested simple sum, weighted sum, and distance-weighted methods
5. **Comparison analysis** - showed how multi-level differs from best-level OFI
6. **Saved results** - prepared data for the next step

**Key insights you should see:**
- Deeper levels have lower activity but still contain information
- Multi-level OFI is typically larger in magnitude than best-level
- Different weighting schemes can emphasize different aspects
- The correlation between best-level and multi-level shows how much additional info you get