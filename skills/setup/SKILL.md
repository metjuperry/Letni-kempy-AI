---
name: setup
description: Configure Ježek bot — save the shared Google Sheet URL and the user's nickname, and check that the Rohlík and Google Sheets MCP connections work. Use when the user wants to set up, reconfigure, or troubleshoot Ježek bot.
---

# Ježek bot setup

Converse with the user in Czech. Collect and save the two configuration
values, then verify the MCP connections.

## 1. Collect configuration

Ask the user for:

1. **The shared Google Sheet URL** — must look like
   `https://docs.google.com/spreadsheets/d/<id>/...`. If it does not, ask
   again; do not save an invalid URL.
2. **Their nickname** — the value written into the `Kdo objednal?` column.

## 2. Save

Write both values to `~/.claude/jezek-bot.json` (create or overwrite):

```json
{
  "sheetUrl": "<the sheet URL>",
  "nickname": "<the nickname>"
}
```

If the file already exists, show the current values first and only change
what the user wants changed.

## 3. Verify connections

1. **Rohlík MCP**: try a trivial search (e.g. `batch_search_products` with
   keyword `mléko`). If the tools are missing or unauthenticated, tell the
   user to run `/mcp`, select the `rohlik` server, and sign in via OAuth.
2. **Google Sheets**: try to read the configured spreadsheet with the
   connected Google Sheets/Drive MCP tools. If no such tools exist, point the
   user to the README ("Nastavení" section) to connect a Google Sheets
   connector.

## 4. Confirm

Summarize (in Czech) the saved config and the connection status of both
services, so the user knows whether `objednej` will work end-to-end.
