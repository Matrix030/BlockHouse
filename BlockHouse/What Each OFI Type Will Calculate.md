Now that you understand the data structure:

1. **Best-Level OFI**: Only uses `bid_px_00`, `ask_px_00`, `bid_sz_00`, `ask_sz_00`
2. **Multi-Level OFI**: Uses all levels (`_00` through `_09`)
3. **Integrated OFI**: Combines all levels using smart weighting
4. **Cross-Asset OFI**: Looks at how other stocks' OFI affects this stock