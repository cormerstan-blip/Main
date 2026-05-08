# Claude Context - MT5 Demo Bot
*Auto-generated | Last updated: 2026-05-09 02:05 (Asia/Kuala_Lumpur)*

---

## How to Continue This Session

1. Share this repo with Claude: **https://github.com/cormerstan-blip/Main**
2. Say: *"Continue my MT5 demo trading. Read CLAUDE_CONTEXT.md and mt5_status.json for full context."*

---

## What Is Running

Autonomous Python bot (`mt5_autonomous.py`) on a Windows PC connected to MetaTrader 5.
Polls every 30 s, evaluates signals, opens/closes trades, pushes status here after each poll.

| Field | Value |
|-------|-------|
| Account name | Cormers Tan |
| Server | OctaFX-Demo |
| Login | 213859343 |
| Balance baseline | $5,000 USD |
| Leverage | 1:500 |
| Mode | DEMO (paper money) |

---

## File Map (Windows PC)

| Path | Purpose |
|------|---------|
| `C:\Users\PC\mt5_autonomous.py` | **Main bot - restart to resume** |
| `C:\Users\PC\mt5_config.json` | Live config - bot reloads every poll |
| `C:\Users\PC\mt5_status.json` | Live snapshot pushed to GitHub every 30 s |
| `C:\Users\PC\mt5_autonomous.log` | Console log |
| `C:\Users\PC\ledger.json` | All trade records (JSON array) |
| `C:\Users\PC\mt5_perf.json` | Cumulative performance state |
| `C:\Users\PC\mt5_git.json` | GitHub PAT + repo path |
| `C:\Users\PC\mt5-repo\` | Local git clone |

---

## Restart Bot (PowerShell)

```powershell
Get-Process python -ErrorAction SilentlyContinue | Stop-Process -Force
Start-Process -FilePath "C:\Python314\python.exe" `
  -ArgumentList "-u", "C:\Users\PC\mt5_autonomous.py" `
  -RedirectStandardOutput "C:\Users\PC\mt5_autonomous.log" `
  -RedirectStandardError  "C:\Users\PC\mt5_autonomous_err.log" `
  -WindowStyle Hidden
```

---

## Strategy (v2)

| Parameter | Value |
|-----------|-------|
| Symbols | BTCUSD, ETHUSD, XAUUSD, EURUSD |
| Entry TF | M5 candles |
| HTF filter | M15 EMA 9/21 must be bullish |
| Signal | EMA 9/21 cross-up |
| RSI | 30-70 entry, exit > 78 |
| MACD | (12,26,9) line > signal |
| Min ATR | >= 0.1% of price |
| TP / SL | ATR x 3.0 / ATR x 1.5 (2:1 R:R) |
| Trailing | Breakeven @ 1.0x ATR, trail @ 1.8x ATR |
| Risk | 1.5% normal -> 0.8% after >=2 consec losses |
| Corr guard | No simultaneous BTC+ETH |
| Circuit breaker | Pause 30 min after 4 consecutive losses |
| Direction | Long only |

---

## Monitoring

| Layer | What | Cadence |
|-------|------|---------|
| This repo - `mt5_status.json` | Live JSON snapshot | Every 30 s |
| This repo - `MT5_DAILY.md` | Daily markdown summary | Daily |
| Cloud routine `trig_01Vtad8Uzu8jUjZmzf2BCvzg` | Hourly report on claude.ai | Every 1 h |

---

## PAT Rotation

1. Create new classic PAT at github.com/settings/tokens (scope: `repo`)
2. Edit `C:\Users\PC\mt5_git.json` - update the `"pat"` field
3. Restart bot - `sync_git_auth()` syncs it to `.git/config` automatically

---

## Environment

- Python 3.14 at `C:\Python314\`
- MetaTrader5 lib v5.0.5735
- MT5 terminal at `C:\Program Files\MetaTrader 5\terminal64.exe`
- OctaFX: ORDER_FILLING_FOK only (baked in)
