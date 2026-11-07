# antitech.eu

Static site (no build step) listing tech features, products and services Europeans are left out of because of European regulation (EU, EEA, UK, Switzerland). Deployed on Vercel from `main`. Every push goes live.

- `index.html`: layout, styles and rendering. Don't touch it during routine data updates.
- `data/items.json`: all content. `updated` is the date the data was last checked (YYYY-MM-DD).

## Item schema

```json
{
  "co": "Company",                 // join several with " · "
  "name": "Feature name",
  "what": "One or two plain sentences: what it does and what Europe is missing.",
  "launched": "YYYY-MM-DD",        // first availability anywhere outside Europe
  "arrived": "YYYY-MM-DD" | null,  // first availability in Europe; null if still missing
  "removed": "YYYY-MM-DD",         // only for status "removed"
  "approx": true,                  // optional: launched date is an estimate
  "approx2": true,                 // optional: arrived date is an estimate
  "status": "missing | removed | partial | late",
  "regions": ["EU", "UK", "CH"],   // where it's unavailable
  "rule": "DMA | GDPR | AI Act | DSA | ... | Unstated",
  "cite": "stated | inferred",     // stated = the company itself named the rule
  "src": [["Outlet", "https://..."]]
}
```

## Weekly update rules

1. Search for news from the past ~10 days on:
   - new launches that skip Europe or the EU/EEA, UK or Switzerland
   - existing "missing" or "partial" items that have since arrived in Europe (set `arrived`, change `status` to `late`, or `partial` if only some countries or features)
   - items whose regulatory cause has become clearer
2. Only add an item if a credible outlet reports it AND regulation is plausibly the cause. Delays explained by localisation, language support or commercial reasons don't belong here.
3. Never claim "the EU banned X" unless that is literally true. Use `cite: "stated"` only when the company publicly named the rule.
4. Every item needs at least one working source URL. Prefer primary or major outlets.
5. Keep `what` short and factual, in the same tone as the existing items.
6. Always bump `updated` to today's date, even if nothing else changed.
7. Validate before committing: `python3 -c "import json;json.load(open('data/items.json'))"`.
8. Commit message: `data: weekly update YYYY-MM-DD`, with a one-line summary of what was added or changed.
