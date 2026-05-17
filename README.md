# Verus Tracker

A self-contained HTML tracker for VRSC miners. No installation, no server, no dependencies — just open the file in a browser.

**Live demo:** https://blackshirt-crypto.github.io/verus-tracker-html/verus-tracker.html

---

## What it does

### Monitor tab
- Live worker status from the [verus.farm](https://verus.farm) pool API
- Per-worker cards showing current hashrate, 1hr average, 24hr average, and blocks found
- Expand any worker card for detail — invalid share %, difficulty, round shares, and per-coin block history
- Fleet summary — online/offline count, your total hashrate vs pool hashrate
- Per-coin block summary (VRSC, CHIPS, vARRR, vDEX)
- Offline alerts with browser notifications
- Auto-polls every 2 minutes (configurable)
- Works for **any** verus.farm miner — enter any R-address

### Payout History tab
- Reads directly from the Verus blockchain explorer ([insight.verus.io](https://insight.verus.io))
- Shows **all** incoming VRSC to your address — any pool, solo mining, direct transfers
- Date range picker with presets (30d, 90d, 6mo, 1yr, all)
- Dual line charts — 30-day and 6-month daily reward graphs
- Full transaction table with links to each tx on the explorer
- Works for **any** VRSC R-address

---

## Getting started

### Option 1 — Use the live GitHub Pages link
Click the live demo link above. The built-in Setup page will guide you through any browser configuration needed.

### Option 2 — Download and open locally
1. Download `verus-tracker.html`
2. Open it in Chrome, Firefox, or Edge — just double-click the file
3. Follow the Setup tab instructions

---

## The CORS thing (what it is and how to fix it)

The **Monitor tab** (verus.farm data) works in any browser with no setup.

The **Payout History tab** reads from insight.verus.io, which browsers block when opening a local HTML file — this is a browser security feature called CORS. The built-in Setup tab detects this automatically and gives you exact fix instructions for your OS and browser.

**The short version:**

**Windows Chrome** — press Win+R and run:
```
chrome.exe --disable-web-security --user-data-dir="C:\ChromeDev"
```

**Mac Chrome** — open Terminal and run:
```
open -n -a "Google Chrome" --args --disable-web-security --user-data-dir="/tmp/chrome-dev"
```

**Linux Chrome** — open a terminal and run:
```
google-chrome --disable-web-security --user-data-dir="/tmp/chrome-dev"
```

**Firefox** — go to `about:config`, find `security.fileuri.strict_origin_policy`, set it to `false`.

> Only use the Chrome flag when running this tracker. Don't browse the web in that window.

---

## Files

| File | Purpose |
|------|---------|
| `verus-tracker.html` | The tracker — everything in one file |
| `verus-proxy.js` | Optional Node.js proxy for self-hosting (eliminates CORS for everyone) |

---

## Self-hosting the proxy (optional)

If you want to share the tracker publicly without anyone needing the CORS fix, you can run the included proxy on a VPS:

```bash
node verus-proxy.js
```

Then update this line in `verus-tracker.html`:
```javascript
const EXPLORER = 'https://insight.verus.io';
```
to:
```javascript
const EXPLORER = 'http://YOUR_VPS_IP:3535';
```

The proxy forwards requests to insight.verus.io and adds the CORS headers browsers need.

---

## Notes

- The Monitor tab only shows workers currently active on verus.farm. Workers that have been offline long enough may drop from the API.
- The blocks panel shows recent blocks only — the verus.farm `/api/blocks` endpoint is a rolling window, not full history. The Payout History tab covers full history via the blockchain.
- 1hr and 24hr hashrate averages are calculated locally by the browser from poll history. They show "building..." until enough data points accumulate.
- Your address and settings are saved to browser localStorage so the tracker remembers you on next visit.

---

## Contributing

Pull requests welcome. If you mine on a pool with a public API and want to add support for it, open an issue.

---

*Community tool. Not affiliated with the Verus Coin Foundation or verus.farm.*
