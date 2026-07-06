# Railway Deployment Guide

This Telegram bot is now configured for Railway deployment. Follow these steps:

## Prerequisites
- Railway account (https://railway.app)
- GitHub account with this repo pushed
- Your Telegram bot token from @BotFather

## Deployment Steps

### 1. Push to GitHub
```bash
git init
git add .
git commit -m "Initial commit - Railway ready"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

### 2. Create a Railway Project
1. Go to https://railway.app and sign in
2. Click **"Create New Project"** → **"Deploy from GitHub repo"**
3. Select your repository
4. Railway will automatically detect the Python app and use the `Procfile`

### 3. Add Environment Variables
In your Railway project dashboard:
1. Go to **Variables**
2. Add the following variables:
   - `TELEGRAM_BOT_TOKEN` = Your bot token from @BotFather
   - `PREMIUM_EMOJI_PACK` = (optional) Your premium emoji pack name
   - `PREMIUM_EMOJI_MAP` = (optional) JSON of custom emoji IDs
   - `TRONGRID_API_KEY` = (optional) For Tron rate limiting
   - `SOLANA_RPC_URL` = (optional) Private Solana RPC endpoint

### 4. Configure Resource Allocation
- **Memory**: 512 MB (sufficient for this bot)
- **CPU**: Shared is fine
- **Storage**: Note that SQLite data is ephemeral; data resets on redeploy

### 5. Deploy
1. Click **"Deploy"** button
2. Watch logs to ensure startup completes: `Bot is running.`
3. Your bot should now be live!

## Important Notes

### Database Persistence
⚠️ **WARNING**: The SQLite database (`wallet_bot.db`) is NOT persistent on Railway. Every redeploy will reset your data.

**Options:**
1. **Keep current setup** (ephemeral): Data resets on each deploy (good for testing)
2. **Add PostgreSQL** (recommended for production):
   - Add a PostgreSQL service in Railway dashboard
   - Update `main.py` to use PostgreSQL instead of SQLite
   - Your data will persist across redeployments

### Bot Uptime
- Railway keeps web services running 24/7
- Your bot will be always-on
- Only one instance of the bot can run per token (Telegram API limit)

### Monitoring
1. Go to **Logs** tab to see real-time output
2. Errors appear in red
3. Use `/menu` command in Telegram to test if bot is alive

### Updating the Bot
1. Push changes to GitHub: `git push`
2. Railway automatically redeploys (takes ~1-2 minutes)
3. Watch the **Deployments** tab for status

### Stopping the Bot
- In Railway dashboard, go to **Settings** → click **"Delete Service"** or toggle **Off**
- Or disconnect your GitHub repo

## Database Migration (Optional, for Production)

To use PostgreSQL for persistent data:

1. **Add PostgreSQL service in Railway**
   - In your project dashboard, click **"Create"** → **"Database"** → **PostgreSQL**
   - Note the connection string

2. **Update main.py** to use PostgreSQL
   - Replace `sqlite3`/`aiosqlite` with `asyncpg` or `psycopg`
   - Update connection strings to use `DATABASE_URL` from Railway environment

3. **Redeploy** - Railway will auto-detect and redeploy

## Troubleshooting

### Bot won't start
- Check **Variables** tab - ensure `TELEGRAM_BOT_TOKEN` is set
- Check **Logs** for error messages
- Make sure token format is correct (no spaces)

### Bot not responding
- Send `/start` to the bot in Telegram
- Check logs for any error messages
- Restart the service via Railway dashboard

### Data lost after redeploy
- This is expected with SQLite on Railway
- Consider adding PostgreSQL for persistence (see Database Migration above)

### Memory/CPU issues
- Check resource usage in Railway dashboard
- Increase allocated memory if needed

## Support
- Railway docs: https://docs.railway.app
- Telethon docs: https://docs.telethon.dev
- Visit Railway community: https://railway.app/community
