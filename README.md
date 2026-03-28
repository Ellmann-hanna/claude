# Claude Skills

A collection of custom Claude Code skills.

## Available Skills

### 📈 stock-digest
Aggregates stock market news and data into a clean morning briefing.

**Usage:**
```
/stock-digest                    # General market digest
/stock-digest AAPL TSLA SPY     # Focused watchlist digest
```

**Features:**
- Real-time index levels (S&P 500, Nasdaq, Dow, Russell 2000, VIX)
- Top 3–5 financial headlines with summaries
- Sector performance pulse
- Per-ticker watchlist with news and key levels
- Economic calendar (Fed events, earnings, data releases)
- Quick-take bullets for the day ahead

See [`skills/stock-digest/SKILL.md`](skills/stock-digest/SKILL.md) for full documentation.

---

> Skills are invoked via slash commands in Claude Code. Install by placing the `skills/` folder in your project or `~/.claude/` directory.
