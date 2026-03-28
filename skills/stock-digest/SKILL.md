# Stock Digest Skill

## Overview
Aggregates stock market news and data into a clean, actionable morning digest. Supports a general market overview or a focused watchlist for specific tickers.

## Trigger
Use this skill when the user asks for:
- A morning market or stock digest
- Stock news summary or briefing
- Pre-market overview
- Watchlist updates for specific tickers (e.g., AAPL, TSLA, NVDA)

**Example invocations:**
```
/stock-digest
/stock-digest AAPL TSLA SPY
/stock-digest NVDA MSFT
```

## Parameters
- **tickers** *(optional)*: One or more stock symbols (e.g., `AAPL TSLA SPY`). If omitted, produce a general market digest.
- **date** *(optional)*: Target date in `YYYY-MM-DD` format. Defaults to today.

---

## Instructions

### Step 1 — Gather Data in Parallel

Run ALL of the following searches simultaneously using `WebSearch` and `WebFetch`:

1. **Market Overview**
   - Search: `"stock market today" OR "pre-market futures" {date}`
   - Fetch: `https://finance.yahoo.com/` for index levels (S&P 500, Nasdaq, Dow, Russell 2000)

2. **Top Financial News**
   - Search: `"top financial news today" {date}`
   - Search: `"Wall Street news" {date}`

3. **Economic Calendar**
   - Search: `"economic calendar today" {date}` — look for Fed speakers, CPI, PCE, jobs data, earnings

4. **Sector Pulse**
   - Search: `"sector performance today" OR "best performing sectors" {date}`

5. **Per-Ticker Research** *(only if tickers provided)*
   - For each ticker, search: `"{TICKER} stock news today" {date}`
   - Fetch: `https://finance.yahoo.com/quote/{TICKER}/news/`

### Step 2 — Compile the Digest

Format the output as a morning briefing using this exact structure:

---

```
╔══════════════════════════════════════════════════════════╗
║           📈 MORNING STOCK DIGEST — {DATE}              ║
╚══════════════════════════════════════════════════════════╝

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 🌍 MARKET OVERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  S&P 500  ......  {price}  ({change}%)
  Nasdaq   ......  {price}  ({change}%)
  Dow      ......  {price}  ({change}%)
  Russell  ......  {price}  ({change}%)
  VIX      ......  {value}  — {sentiment: calm/elevated/fearful}

  Futures: {pre-market direction and key levels}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 📰 TOP STORIES  (3–5 headlines)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  1. {Headline} — {Source}
     {1–2 sentence summary and market implication}

  2. ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 🏭 SECTOR PULSE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  Leading  : {sectors} ↑
  Lagging  : {sectors} ↓
  Theme    : {1-line macro theme e.g. "risk-off, rotation to defensives"}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 👀 WATCHLIST  (only if tickers provided)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  {TICKER}
    Price   : {price} ({change}%)
    News    : {top 1–2 headlines}
    Watch   : {key catalyst or level to watch today}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 📅 ECONOMIC CALENDAR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  {Time} — {Event} (Forecast: {X} | Prior: {Y})
  Earnings today: {TICKER}, {TICKER}, ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 ⚡ QUICK TAKES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  • {Bullet 1 — key risk or opportunity}
  • {Bullet 2}
  • {Bullet 3}

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 ⚠️  DISCLAIMER
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  This digest is for informational purposes only.
  Not financial advice. Verify all data before trading.
```

---

### Step 3 — Quality Rules

- **Be honest about missing data**: If live prices are unavailable, mark them `[unavailable]` and link to Yahoo Finance / MarketWatch. Never fabricate numbers.
- **Be concise**: Each section should be scannable in under 30 seconds.
- **Prioritize relevance**: If tickers are provided, make the Watchlist section the most detailed part.
- **Use real sources**: Cite the source (e.g., Reuters, Bloomberg, WSJ) next to each headline.
- **Market sentiment**: Always include a one-line overall sentiment (bullish / bearish / mixed / cautious).

---

## Data Sources (in priority order)
1. `https://finance.yahoo.com/` — prices, news
2. `https://www.marketwatch.com/` — market overview
3. `https://www.investing.com/economic-calendar/` — economic calendar
4. `https://finviz.com/` — sector heatmap
5. Web search for breaking news

---

## Example Output (General Digest)

```
╔══════════════════════════════════════════════════════════╗
║        📈 MORNING STOCK DIGEST — March 27, 2026         ║
╚══════════════════════════════════════════════════════════╝

 🌍 MARKET OVERVIEW
  S&P 500  ......  5,612  (-0.4%)
  Nasdaq   ......  17,840 (-0.7%)
  Dow      ......  41,200 (+0.1%)
  VIX      ......  19.2   — elevated

  Futures: Mixed. S&P futures -0.3% ahead of PCE data.

 📰 TOP STORIES
  1. Fed's Powell signals no rush on rate cuts — Reuters
     Powell reiterated a data-dependent stance; markets now
     price only 1 cut by year-end, pressuring growth stocks.

  2. Tariff concerns weigh on tech sector — WSJ
     New proposed tariffs on semiconductor imports sparked
     a selloff in chip stocks late Wednesday.

 🏭 SECTOR PULSE
  Leading  : Energy ↑, Financials ↑
  Lagging  : Technology ↓, Consumer Discretionary ↓
  Theme    : Defensive rotation amid rate uncertainty

 📅 ECONOMIC CALENDAR
  8:30 AM — PCE Price Index (Forecast: 0.3% | Prior: 0.3%)
  Earnings: LULU, NKE after close

 ⚡ QUICK TAKES
  • Watch PCE at 8:30 AM — hotter than expected = more selling
  • Energy outperforming; oil above $80 again
  • End-of-quarter rebalancing may add volatility Friday

 ⚠️  DISCLAIMER
  Informational only. Not financial advice.
```

---

## Notes for Skill Developers
- This skill works best when `WebSearch` and `WebFetch` are enabled.
- If web tools are unavailable, produce the digest skeleton with `[unavailable]` placeholders and direct links to live sources.
- The skill can be scheduled via `/schedule` for automatic 7 AM delivery.
