# 🌹 Rose Bot – Render Deployment Guide

## Overview
This bot uses **PostgreSQL** (via SQLAlchemy) for its database — not MongoDB.
Render provides a free PostgreSQL service that works perfectly with this bot.

---

## Step 1: Get Your Telegram Bot Token

1. Open Telegram and message **@BotFather**
2. Send `/newbot` and follow the prompts
3. Copy the **token** BotFather gives you (e.g. `123456:ABC-DEF...`)

---

## Step 2: Get Your Telegram User ID

1. Message **@userinfobot** on Telegram
2. It will reply with your numeric user ID (e.g. `987654321`)

---

## Step 3: Push Code to GitHub

```bash
cd rose-render
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/rose-bot.git
git push -u origin main
```

---

## Step 4: Create a Render Account & Deploy

1. Go to **https://render.com** and sign up (free)
2. Click **"New +"** → **"Blueprint"**
3. Connect your GitHub repo
4. Render will detect `render.yaml` and auto-create:
   - A **Web Service** (the bot)
   - A **PostgreSQL database**

---

## Step 5: Set Environment Variables in Render

After the blueprint is created, go to your **Web Service** → **Environment** tab and fill in:

| Variable | Value | Required |
|---|---|---|
| `TOKEN` | Your BotFather token | ✅ Yes |
| `OWNER_ID` | Your numeric Telegram ID | ✅ Yes |
| `OWNER_USERNAME` | Your username without @ | ✅ Yes |
| `URL` | `https://your-service-name.onrender.com` | ✅ Yes |
| `MESSAGE_DUMP` | A private Telegram chat/channel ID | Optional |
| `SUDO_USERS` | Space-separated list of admin user IDs | Optional |
| `SUPPORT_USERS` | Space-separated list of support user IDs | Optional |

> **Note:** `DATABASE_URL` is set automatically by Render from the PostgreSQL database.

---

## Step 6: Get Your Service URL

1. After first deploy, go to your **Web Service** page on Render
2. Copy the URL shown at the top (e.g. `https://rose-telegram-bot.onrender.com`)
3. Go to **Environment** → set `URL` to that value
4. Click **"Save Changes"** → Render will redeploy

---

## Step 7: Verify It's Working

- Open Telegram and send `/start` to your bot
- If the bot replies — it's live! 🎉

---

## Common Issues

### Bot not responding?
- Check **Logs** on Render for error messages
- Make sure `TOKEN` is set correctly
- Make sure `URL` matches your Render service URL exactly (no trailing slash)

### Database errors?
- Wait 1-2 minutes after first deploy — the DB needs time to initialize
- Render auto-runs `BASE.metadata.create_all()` on first start — tables are created automatically

### Free tier goes to sleep?
- Render free services sleep after 15 minutes of inactivity
- Use a service like **UptimeRobot** (free) to ping your URL every 5 minutes:
  - Go to https://uptimerobot.com → Add Monitor → HTTP(s)
  - URL: `https://your-service.onrender.com`
  - Interval: every 5 minutes

---

## Environment Variables Reference

| Variable | Default | Description |
|---|---|---|
| `ENV` | `True` | Must be `True` to use env vars |
| `TOKEN` | — | Telegram bot token |
| `OWNER_ID` | — | Your Telegram user ID |
| `OWNER_USERNAME` | — | Your Telegram username |
| `DATABASE_URL` | auto | PostgreSQL connection string |
| `WEBHOOK` | `True` | Use webhook (required for Render) |
| `URL` | — | Your Render service URL |
| `PORT` | `10000` | Port Render listens on |
| `WORKERS` | `4` | Number of worker threads |
| `NO_LOAD` | `translation rss` | Modules to disable |
| `DEL_CMDS` | `False` | Delete command messages |
| `STRICT_GBAN` | `False` | Strict global ban enforcement |
| `SUDO_USERS` | — | Space-separated sudo user IDs |
| `SUPPORT_USERS` | — | Space-separated support user IDs |
| `WHITELIST_USERS` | — | Space-separated whitelist user IDs |
| `MESSAGE_DUMP` | — | Chat ID to dump saved messages |
| `BAN_STICKER` | default sticker | Sticker file ID for bans |
| `ALLOW_EXCL` | `False` | Allow `!` as command prefix |
