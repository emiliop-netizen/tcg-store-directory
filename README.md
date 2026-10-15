# TCG store directory

Game stores that buy trading cards from collectors, by country, for TCG Seller (a Minds agent).
Facts about stores only - **no prices are stored here**. Prices are always read live from each store.

Files: `countries/<ISO country code>.json` (for example `countries/AU.json`).

## Store groups

| group | meaning |
|---|---|
| `feed` | Store has a public buylist price feed - price cards in seconds (see `feed`) |
| `public_buylist` | Public per-card buylist page - browse it |
| `login_required` | Buylist needs a free store account (e.g. BinderPOS) - collector checks it |
| `quote_only` | Send a list or photos, get an offer |
| `blocked` | Site blocks automated browsers - collector checks it |
| `in_store` | Buys in person / no online buylist |
| `not_buying_singles` | Says it does not buy singles |
| `unknown` | Nothing about buying found online - walk in or phone |

## Feed platforms

- **StorePass**: `GET https://<host>/saas/search?store_id=<store_id>&product_line=<Game>&mongo=true&sort=Relevance&buylist_products=true&name=<card number or name>` -> `products[].offer_price` (cash), `offer_price_credit` (credit; empty at some stores). `price` and `variant_info` are SELL prices - ignore.
- **CardCompanion** (Shopify app): `GET https://<shop>/tools/cc_buylist/buylist/search?shopify_domain=<x>.myshopify.com&game=<slug>&search=<card number>&page=1&page_size=24&include_prices=true` -> `results[].offers[].buylist_price` / `store_credit_price`.
- **SortSwift**: `GET https://buylist-api.sortswift-services.com/api/buylist/search?query=<q>&page=1&limit=24&sort=relevance&storeId=<key>`, then `POST .../api/buylist/get-price/bulk` -> `NMFinalCash` / `NMFinalCredit`.

## Sources

Official store locators (Wizards, Bandai TCG+), certificate-transparency search for buylist sites, OpenStreetMap, store websites, and targeted research. Each entry has `checked_at`. Store details change; verify at submission.
