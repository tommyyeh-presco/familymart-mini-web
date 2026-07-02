# familymart-mini-web

The **WKWebView mini-app** for the FamilyMart / PrestoPay iOS mockup — a Bibian-style
(比比昂×Amazon) shopping page. This is deliberately a **separate repo from the iOS app**: it
demonstrates the "embed a remote website via WKWebView" pattern, where the mini-app's code
lives on a server, not in the app bundle.

- **Live:** https://tommyyeh-presco.github.io/familymart-mini-web/
- **Hosting:** GitHub Pages, `main` branch, root (`/`). Just a single `index.html`.
- **Loaded by:** the iOS app's WKWebView button (see the `ios mockup` repo).

## What it does (all client-side, no backend)

- Bibian-style landing: category grid, 免空運費 promo, 限時搶購 flash sale
- **熱門商品** — 6 hardcoded products
- Product detail → quantity → **加入購物車**
- **Cart** (🛒 badge) — edit quantities, remove, subtotal; persisted in `localStorage`
- **Checkout** — recipient + My Fami Pay + line items, free shipping over NT$1,499
- **Order success** with a generated order number
- **Order history** (👤 icon) — past orders persisted in `localStorage`

Hardcoded items, in-memory/`localStorage` state — **no database**.

## Deploy

```bash
git add -A && git commit -m "..." && git push origin main
# GitHub Pages does not reliably auto-build on push — trigger it:
gh api --method POST repos/tommyyeh-presco/familymart-mini-web/pages/builds
# check status:
gh api repos/tommyyeh-presco/familymart-mini-web/pages/builds/latest --jq '.status + " " + .commit'
```

The iOS app's WKWebView ignores local cache, so a rebuilt page shows on the next open of the
WebView button — no app rebuild needed. To preview without the app, open the live URL in any
browser.

## Structure

Single `index.html` — a tiny SPA: inline CSS + vanilla JS with view-switching
(`show(view)`), a `PRODUCTS` array, and `cart` / `orders` arrays persisted to `localStorage`
under keys `bibian_cart` and `bibian_orders`.
