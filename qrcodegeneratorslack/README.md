# Slack QR Code Generator

A Slack slash command that generates branded QR codes with your logo in the centre.

**Usage:** `/qrcode https://example.com`

---

## Setup

### 1. Deploy to Vercel

```bash
npm install
vercel deploy
```

Note your deployment URL — you'll need it for the Slack app setup (e.g. `https://your-app.vercel.app`).

### 2. Create a Slack App

1. Go to [api.slack.com/apps](https://api.slack.com/apps) → **Create New App** → **From scratch**
2. Name it (e.g. "QR Generator") and pick your workspace

### 3. Add a Slash Command

1. In your app settings, go to **Slash Commands** → **Create New Command**
2. Fill in:
   - Command: `/qrcode`
   - Request URL: `https://your-app.vercel.app/api/qrcode`
   - Short description: `Generate a branded QR code`
   - Usage hint: `https://example.com`
3. Save

### 4. Add Bot Permissions

1. Go to **OAuth & Permissions** → **Scopes** → **Bot Token Scopes**
2. Add: `files:write`, `chat:write`
3. Click **Install to Workspace**
4. Copy the **Bot User OAuth Token** (starts with `xoxb-`)

### 5. Set Environment Variables in Vercel

In your Vercel project settings → Environment Variables, add:

| Key | Value |
|-----|-------|
| `SLACK_SIGNING_SECRET` | From app settings → Basic Information |
| `SLACK_BOT_TOKEN` | The `xoxb-` token from OAuth & Permissions |
| `LOGO_URL` | Public URL to your logo image (square PNG works best) |

### 6. Redeploy

```bash
vercel deploy --prod
```

---

## How it works

1. User types `/qrcode https://resolve.ai` in any Slack channel
2. Slack sends the request to your Vercel endpoint
3. The app verifies the request is genuinely from Slack
4. Generates a 500×500 QR code with `H` level error correction (supports logo overlay)
5. Fetches your logo, resizes it to 20% of the QR size, adds white padding
6. Composites the logo onto the centre of the QR code using Sharp
7. Uploads the final PNG to the Slack channel

---

## Logo tips

- Square logos work best
- PNG with transparent background is ideal
- The logo covers ~20% of the QR code — this is within the safe limit for error correction
- Host it on a CDN or your existing asset host — it just needs to be a public URL
