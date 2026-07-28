# Ježek Bot 🦔

Claude Code plugin pro letní kempy: najde zboží na [Rohlíku](https://www.rohlik.cz)
a zapíše objednávky do sdílené Google tabulky. Nic nenakupuje — jen plní
objednávkový list, jeden řádek na položku.

*(English summary at the bottom.)*

## Co umí

- **Ježek bot** (agent) — řekneš mu, co chceš objednat, on to najde na
  Rohlíku, ukáže ti, co našel, a po potvrzení zapíše do tabulky.
- `/jezek-bot:objednej mléko 2x, máslo, 10 rohlíků` — totéž jedním příkazem.
- `/jezek-bot:setup` — uloží URL tabulky a tvoji přezdívku.

Formát tabulky (jeden řádek = jedna položka):

| erár? | Kdo objednal? | množství | odkaz |
|-------|---------------|----------|-------|
| ano/ne | přezdívka | číslo | https://www.rohlik.cz/… |

## Instalace

V Claude Code CLI:

```
/plugin marketplace add metjuperry/Letni-kempy-AI
/plugin install jezek-bot@letni-kempy
```

## Nastavení

1. **Rohlík MCP** — plugin ho přináší s sebou (server `rohlik`,
   `https://mcp.rohlik.cz/mcp`). Při instalaci ho schval, pak spusť `/mcp`,
   vyber `rohlik` a přihlas se svým Rohlík účtem (OAuth v prohlížeči).
2. **Google Sheets** — připoj si konektor, který umí zapisovat do Google
   tabulek (např. oficiální Google Drive/Sheets konektor na claude.ai →
   Settings → Connectors, nebo libovolný Google Sheets MCP server přes
   `claude mcp add`). Tabulka musí být sdílená s účtem, kterým se konektor
   přihlašuje.
3. Spusť `/jezek-bot:setup` — zadáš URL sdílené tabulky a svoji přezdívku
   (uloží se do `~/.claude/jezek-bot.json`).

## Použití

```
/jezek-bot:objednej 2x mléko, máslo, toaletní papír
```

nebo prostě napiš: *„Ježku, objednej mi 2 mléka a máslo, je to erár."*

Bot vždy ukáže, co na Rohlíku našel, zeptá se na nejasnosti a na „erár?",
a do tabulky zapíše až po tvém potvrzení. Nikdy nemaže ani nepřepisuje
existující řádky.

---

## English summary

Ježek Bot is a Claude Code plugin that turns shopping requests into rows in a
shared Google Sheet. It searches Rohlík (Czech grocery delivery) via the
bundled official Rohlík MCP server, confirms the matched products with the
user, and appends one row per item (`erár?` = shared funds yes/no, orderer's
nickname, quantity, product URL). It never places real orders.

Install: `/plugin marketplace add metjuperry/Letni-kempy-AI`, then
`/plugin install jezek-bot@letni-kempy`. Authenticate the `rohlik` MCP via
`/mcp` (OAuth), connect a Google Sheets-capable connector, and run
`/jezek-bot:setup`.
