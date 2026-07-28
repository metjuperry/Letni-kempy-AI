---
name: order-to-sheet
description: Write confirmed Rohlík order items into the shared Google Sheet order list, one row per item. Use after products have been found and confirmed by the user.
---

# Writing orders into the shared Google Sheet

## Configuration

Read `~/.claude/jezek-bot.json`:

```json
{
  "sheetUrl": "https://docs.google.com/spreadsheets/d/.../edit",
  "nickname": "Ježek"
}
```

If the file is missing or either value is empty, follow the `setup` skill to
collect and save the values first.

## Row schema

The sheet has four columns; append **one row per ordered item**:

| Column | Header | Value |
|--------|-----------------|-------|
| 1 | `erár?` | `ano` or `ne` — whether the item is paid from shared camp funds. Defaults to `ne`; write `ano` only when the user explicitly said the order or item is erár. |
| 2 | `Kdo objednal?` | The `nickname` from the config, verbatim. |
| 3 | množství | The quantity as a plain number (e.g. `2`). |
| 4 | odkaz | The Rohlík product URL from the `rohlik-search` skill (`https://www.rohlik.cz/<productId>`). |

## How to write

1. Use the connected **Google Sheets / Google Drive MCP** tools to open the
   spreadsheet at `sheetUrl` and read its current contents.
2. Find the first fully empty row after the existing data and append the new
   rows there.
3. **Never** overwrite or delete existing rows, and never touch the header
   row. Append only.
4. Write values exactly in the column order above; leave other columns (if
   any exist) untouched.
5. After writing, read the appended range back to verify, then report to the
   user in Czech what was added (product names, quantities, row numbers).

## Before writing

Only write rows the user has explicitly confirmed (items and quantities).
Include the `erár?` value in the confirmation summary (default `ne`) so the
user can correct it before writing.

## If no Sheets-capable MCP is connected

If there are no tools that can edit Google Sheets, stop and tell the user (in
Czech) that a Google Sheets connector is required — point them to the
"Nastavení" section of this plugin's README (connect the Google Drive/Sheets
connector via `/mcp` or claude.ai connectors). Do not attempt to write via
unauthenticated APIs or the browser.
