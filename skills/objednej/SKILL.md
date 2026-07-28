---
name: objednej
description: Order items via Ježek bot in one command — searches Rohlík for the requested items and writes them into the shared Google Sheet. Use when the user runs /jezek-bot:objednej or asks to order a list of items.
---

# /jezek-bot:objednej — one-shot order

The user's shopping request is: **$ARGUMENTS**

If `$ARGUMENTS` is empty, ask (in Czech) what they want to order.

Execute the full Ježek bot workflow, conversing in Czech:

1. Parse the request into items and quantities (default quantity 1).
2. Follow the `rohlik-search` skill: find each item on Rohlík via the Rohlík
   MCP and collect product URLs (`https://www.rohlik.cz/<productId>`).
3. Present the matches (name, package size, price, URL) and confirm with the
   user; ask about ambiguous items instead of guessing.
4. Ask whether the order is **erár** (`ano`/`ne`, per-item overrides allowed).
5. Follow the `order-to-sheet` skill: append one row per item to the shared
   Google Sheet (columns: `erár?`, `Kdo objednal?`, množství, odkaz).
6. Report back in Czech what was written.

Rules:
- Never place a real Rohlík order (no cart/checkout tools) — the deliverable
  is rows in the sheet.
- Only append to the sheet; never modify existing rows.
- If configuration (`~/.claude/jezek-bot.json`) is missing, run the `setup`
  skill flow first.
