# antitech.eu

Static site (no build step) listing tech features, products and services Europeans are left out of because of European regulation (EU, EEA, UK, Switzerland). Deployed on Vercel from `main`. Every push goes live.

- `index.html`: layout, styles and rendering. Don't touch it during routine data updates.
- `item.html`: the detail page for one entry, served at `/item/<slug>` (rewrite in `vercel.json`). Don't touch it during routine data updates either.
- `data/items.json`: all content. `updated` is the date the data was last checked (YYYY-MM-DD).
- `data/laws.json`: one entry per `rule` value: a summary paragraph, official links and, for EU laws, the lead MEPs still in office with emails verified on their europarl.europa.eu profile.

## Item schema

```json
{
  "slug": "company-feature",       // URL of the detail page, /item/<slug>; never change it once live
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
  "src": [["Outlet", "https://..."]],   // shown on the tile
  "impact": "One paragraph, 3-5 sentences: what this company did because of the rule, what it said, what happened since.",
  "impact_src": [["Outlet", "https://..."]]  // optional: extra sources shown only on the detail page
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
5. Keep `what` short and factual, in the same tone as the existing items. Every new item also needs a `slug` and an `impact` paragraph. When an item's status changes, update its `impact` paragraph too.
   If an item uses a `rule` that isn't in `data/laws.json` yet, add it there (summary paragraph and official links; set `eu` and `contacts` only for EU laws, with emails checked on the MEP's official profile).
6. Always bump `updated` to today's date, even if nothing else changed.
7. Validate before committing: `python3 -c "import json;[json.load(open(f)) for f in ('data/items.json','data/laws.json')]"`.
8. Commit message: `data: weekly update YYYY-MM-DD`, with a one-line summary of what was added or changed.
