---
name: Tech hot-news digest
description: >-
  Use when producing a Chinese tech hot-news brief covering AI, robotics,
  crypto, embedded systems, and LLM from X, Reddit, and major outlets.
---
# Tech hot-news digest

Produce a Chinese tech hot-news brief. Keep this recipe generic: language, domains, item count, sources, destination, and cadence come from the caller (routine or chat). Defaults below match a Taipei evening digest unless overridden.

## Defaults (override if the caller says otherwise)
- Language: Chinese
- Domains (group by these; skip empty): AI · 机器人 · 数字货币 · 嵌入式系统 · LLM
- Count: 8–12 items total
- Freshness: prefer roughly the last 24–48 hours
- Destination: the user in this chat
- If almost nothing new: still send a short “今天没什么特别新” note — do not stay silent

## Gather
Discover live tools each run (do not bake frozen tool schemas into this skill).

1. **X (public tools)** — Search news and posts for the active domains (AI/LLM, robotics, crypto, embedded/MCU/ESP32). Optionally check trends. Prefer high-signal items; respect rate limits (~25 results per call).
2. **Reddit** — Try public hot/top JSON for relevant subs (examples: MachineLearning, LocalLLaMA, artificial, robotics, CryptoCurrency, Bitcoin, embedded, esp32, Hardware) with a normal browser User-Agent. If responses are 403 or blocked, skip Reddit and note it in the source footnote.
3. **Major outlets** — Web search/fetch high-signal sites that day (TechCrunch, The Verge, Ars Technica, Wired, MIT Technology Review, Reuters/Bloomberg tech or crypto, plus domain sites such as company blogs, CoinDesk, CNX Software, Adafruit when relevant).

## Known fallbacks
- Reddit public JSON may return 403 — run on X + news only and say so in 信源说明.
- Do not depend on a box browser session on x.com for X Articles or login; long-form X content may only have a title via API. Prefer primary/outlet URLs when the article body is unavailable.

## Select
- Pick 8–12 non-duplicate, genuinely hot items.
- Prefer official or primary URLs over trend-summary posts when both exist.
- Dedupe near-duplicates; never pad with fluff to hit the count.

## Write
- Optional lead **今日看点** with 1–3 overall top items, then domain sections (skip empty domains).
- Each item: **bold short headline** + 1–2 sentences on why it matters + markdown link(s) with real URLs.
- End with one-line **信源说明**: which channels worked and what failed.

## Deliver
- Send the full Chinese brief to the destination the caller named (default: user in this chat).
- Do not email or Slack unless the caller asked.
- Practical pattern: gather and draft in background work if needed; the parent turn delivers the final Chinese text.

## Who gets the result
The user in this chat, unless the routine or request names a different destination. When there is almost nothing new, still send the short note above.
