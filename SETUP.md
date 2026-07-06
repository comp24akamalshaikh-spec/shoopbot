# Telegram Deposit/Balance Bot — Standalone Setup

This bot must run on **always-on** hosting (Replit **Reserved VM**). Follow these steps
to run it in its own Python project.

## 1. Create a new Python Repl
- On Replit, click **Create Repl** → choose the **Python** template → give it a name.

## 2. Add these files to the new Repl
Upload/drag in everything from this bundle:
- `bot.py` — the whole bot
- `requirements.txt` — dependencies
- `upi_qr.png` — the UPI QR image (admin config)
- `wallet_bot.db` — existing balances/data (skip this if you want a fresh start)

## 3. Install dependencies
In the new Repl's **Shell**, run:
```
pip install -r requirements.txt
```

## 4. Add your secrets (Tools → Secrets)
- `TELEGRAM_BOT_TOKEN` — **required** (your bot token from @BotFather)
- `TRONGRID_API_KEY` — optional, raises Tron rate limits (free from trongrid.io)
- `SOLANA_RPC_URL` — optional, a private Solana RPC endpoint
- `PREMIUM_EMOJI_PACK` / `PREMIUM_EMOJI_MAP` — optional, only if using custom emoji

(API_ID and API_HASH are already set inside `bot.py`.)

## 5. Set the Run command
Set the Repl to run:
```
python bot.py
```

## 6. Publish as Reserved VM
- Click **Publish** → choose **Reserved VM** (always-on) → smallest machine is fine
- Run command: `python bot.py`
- Leave the Build command empty

## Important
- **Only run ONE copy of the bot at a time.** Telegram allows a single connection per
  token, so stop any other instance using the same `TELEGRAM_BOT_TOKEN`.
- Balances are stored in `wallet_bot.db` (a local file). It persists while running, but a
  re-publish can reset it. For real money, move storage to a hosted database.
