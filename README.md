# fallfind

> **The searchable sovereign-estate browser.** Live URL: <https://sjgant80-hub.github.io/fallfind/>

`fallfind` is Simon Gant's live, searchable, documented portfolio of the AI Native Solutions sovereign-tool estate. One URL. Every build. Documented per entry. Paste-your-stack matching. Receipts-first.

It's the link you hand a buyer.

---

## For buyers · "I'm tired of paying for SaaS that watches me"

1. Go to <https://sjgant80-hub.github.io/fallfind/>
2. Type the SaaS you want to leave into the search box (e.g. `xero`, `hubspot`, `calendly`)
3. Or paste your whole stack into the **Pain → Tool** box at the bottom — fallfind names the 3 best-fit sovereign replacements
4. Click **Open live** — the actual sovereign tool opens. No marketing page between you and the product.

Every entry shows:
- **What it does** (1–2 sentences)
- **What it replaces** (the SaaS name)
- **What grift it exposes** (their actual monthly price, what they monetise)
- **The live URL** (clickable, top of card)
- **The source URL** (GitHub, MIT-licensed, fork it)

If a build is offline, fallfind shows that honestly — it doesn't hide dead links.

---

## For developers · the architecture

| | |
|---|---|
| **Single file** | `index.html` · ~32 KB · vanilla JS, no build step |
| **Runs from** | `file://` · GitHub Pages · any static host |
| **Data source** | <https://raw.githubusercontent.com/sjgant80-hub/fall-registry/main/index.json> (live) |
| **Cache** | IndexedDB with localStorage fallback · 5-minute TTL · stale-while-revalidate |
| **Match engine** | pure client-side token-scoring against registry fields · no LLM call |
| **Signal hook** | `BroadcastChannel('fall-signal')` · prime **433** · emits `fallfind_search`, `fallfind_pain`, `fallfind_boot` |
| **Licence shim** | `window.Konomi` (sovereign tier, no gate) |
| **PWA** | data-URL manifest baked in |

### Pain → Tool matching

```js
// pseudo-code (real code in index.html)
const tokens = userText.toLowerCase().match(/[a-z][a-z0-9]+/g).filter(stopwords);
for (const item of registry.items) {
  for (const t of tokens) {
    if (item.haystack.includes(t)) score += 1;
    if (item.obsoletes.some(o => o.toLowerCase().includes(t))) score += 3; // BIG bonus
    if (item.category === t) score += 2;
  }
}
return top 3 by score;
```

The **`obsoletes`** field in the registry is the leverage. When a buyer types "I'm using HubSpot", every sovereign tool whose `obsoletes` array contains "HubSpot" scores +3. That's how `fallsalescrm` beats every generic match for `hubspot`.

### Registry shape

Each entry in `fall-registry/index.json` looks like:

```json
{
  "name": "fallaccount-trades",
  "kind": "app",
  "category": "accounting",
  "purpose": "Sovereign accounts for UK builders/plumbers/sparkies · CIS · MTD · receipt-photo scan",
  "url": "https://sjgant80-hub.github.io/fallaccount-trades/",
  "source": "https://github.com/sjgant80-hub/fallaccount-trades",
  "obsoletes": ["Xero", "QuickBooks", "FreeAgent"],
  "grift_exposed": "Xero £36/mo · QuickBooks £17/mo · per-feature gating",
  "tier": "C",
  "prime": 31,
  "status": "live",
  "shipped": "2026-05-28"
}
```

If you ship a new sovereign tool, add it to fall-registry — it shows up in fallfind on the next 5-minute cache cycle.

### Hotkeys

- `/` — focus search
- `Ctrl/Cmd + Enter` (in the Pain box) — run the matcher

### Run locally

```bash
git clone https://github.com/sjgant80-hub/fallfind.git
cd fallfind
# any static server works — even file://
python3 -m http.server 8000
# or just double-click index.html
```

---

## Doctrine

`fallfind` is part of the AI Native Solutions sovereign estate.

- Single HTML file · vanilla JS · no framework · no build · no tracking
- Pulls live from the canonical registry · zero hardcoded inventory
- Receipts-first: the buyer is ≤1 click from the actual working tool
- MIT licensed · fork it · brand it · ship your own

The portfolio is the proof. The code is the receipt.

— [Simon Gant](https://www.ai-nativesolutions.com) · [LinkedIn](https://www.linkedin.com/in/simon-gant-295b56180/) · prime 433
