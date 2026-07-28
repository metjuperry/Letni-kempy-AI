---
name: rohlik-search
description: Search Rohlík (rohlik.cz) for products the user wants to buy and return their product URLs. Use whenever the user asks to find, price, or order groceries or other items available on Rohlík.
---

# Searching products on Rohlík

Use the **Rohlík MCP** tools (server `rohlik`, bundled with this plugin) to
find products. Do not scrape rohlik.cz with a browser or curl.

## How to search

1. Use `batch_search_products` — it accepts up to 4 keyword queries at once,
   so batch the user's items (call it multiple times for more than 4 items).
   Keywords must be in Czech (e.g. `mléko`, `máslo`, `toaletní papír`).
2. Each query returns up to 5 products with `productId`, `productName`,
   `brand`, `price`, `textualAmount` (package size), and `inStock`.
3. If you already have product IDs (e.g. from a previous conversation), use
   `get_product_details` to refresh price and availability.

## Choosing the right product

- Prefer in-stock products (`inStock: true`).
- Prefer products marked `favourite: true` when the query is generic.
- Otherwise pick a sensible default: reasonable brand and package size for
  the requested amount; prefer better price per unit (`pricePerUnit`).
- If the request is genuinely ambiguous (e.g. "mléko" — fresh vs. long-life,
  fat content), present the top 2–3 candidates with name, size, and price and
  ask the user to pick. Do not guess on ambiguous items.

## Product URLs

The search results do not include URLs. Construct them from the product ID:

```
https://www.rohlik.cz/<productId>
```

Example: productId `1441840` → `https://www.rohlik.cz/1441840`. This URL
resolves directly to the product page (verified — no slug needed). Never
invent IDs; only use IDs returned by the MCP tools.

## Output

Produce a structured list, one entry per requested item:

| item requested | quantity | product name | package | price | URL |

This list is the input for the `order-to-sheet` skill.

## If the Rohlík MCP is unavailable

If no Rohlík MCP tools are available or they fail with authentication errors,
tell the user (in Czech) to run `/mcp`, select the `rohlik` server, and sign
in via OAuth with their Rohlík account. Do not attempt any fallback scraping.
