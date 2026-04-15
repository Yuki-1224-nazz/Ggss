# 🛡️ Turnstile Solver

A high-performance Cloudflare Turnstile bypass API built with Node.js + Playwright. Features a live-stats dashboard, GoLogin account harvester, cookie extraction, and proxy support.

## 🚀 Quick Start

### Local Development

```bash
# 1. Install dependencies
npm install

# 2. Install Playwright browsers
npx playwright install chromium

# 3. Copy env file
cp .env.example .env

# 4. Start server
npm start
```

Open `http://localhost:3000` in your browser.

---

## 🌐 API Reference

### `GET /turnstile`
Solve a Cloudflare Turnstile challenge.

**Query Parameters:**
| Parameter | Required | Description |
|-----------|----------|-------------|
| `url`     | ✅ Yes  | Target URL where Turnstile is rendered |
| `sitekey` | ✅ Yes  | Cloudflare site key |
| `action`  | ❌ No   | Optional Turnstile action string |
| `cdata`   | ❌ No   | Optional custom data string |
| `proxy`   | ❌ No   | Set `true` to use a proxy from proxies.txt |

**Example:**
```
GET /turnstile?url=https://example.com&sitekey=0x4AAAAAAAQn-wN8S1gi-nJa
```

**Response:**
```json
{
  "status": "success",
  "token": "0.abc123...",
  "elapsed_time": "3.412",
  "cookies": {
    "cf_clearance": "...",
    "__cf_bm": "..."
  }
}
```

---

### `POST /api/go`
Solve GoLogin Turnstile → create free account → harvest proxies → save to `proxies.txt`.

**Response:**
```json
{
  "status": "success",
  "email": "user_abc123@ixcyon.top",
  "password": "tg@ixcynigga1234",
  "proxies_saved": 12
}
```

---

### `GET /api/proxies/download`
Download the harvested `proxies.txt` file.

### `GET /api/accounts/download`
Download the harvested `accounts.txt` file.

### `GET /api/stats`
Get real-time statistics as JSON.

### `GET /api/stats/stream`
Server-Sent Events (SSE) stream for live statistics updates.

---

## 🔧 Proxy Format (`proxies.txt`)
One proxy per line. Supported formats:
```
host:port
http://host:port
user:pass:host:port
http://user:pass@host:port
scheme://user:pass@host:port
```

---

## ⚙️ Environment Variables

| Variable     | Default   | Description |
|--------------|-----------|-------------|
| `PORT`       | `3000`    | Server port |
| `POOL_SIZE`  | `3`       | Number of concurrent browser instances |
| `HEADLESS`   | `true`    | Run browsers headlessly |
| `USER_AGENT` | Chrome 124| Custom user agent string |

---

## ☁️ Vercel Deployment

> ⚠️ **Note:** Playwright (browser automation) does **not** run on Vercel's serverless platform due to binary size and execution limits. For full functionality, deploy to a **VPS**, **Railway**, **Render**, or **Fly.io** instead.

### Deploy to Railway (Recommended)
```bash
npm install -g @railway/cli
railway login
railway init
railway up
```

### Deploy to Render
1. Connect your GitHub repo to [render.com](https://render.com)
2. Set Build Command: `npm install && npx playwright install chromium`  
3. Set Start Command: `npm start`
4. Add environment variables from `.env.example`

### Deploy to Fly.io
```bash
npm install -g flyctl
flyctl launch
flyctl deploy
```

---

## 📁 File Structure
```
turnstile-solver/
├── server.js          # Main Express server + solver logic
├── package.json
├── vercel.json        # Vercel config
├── .env.example       # Environment variables template
├── proxies.txt        # Auto-created by GoLogin harvester
├── accounts.txt       # Auto-created by GoLogin harvester
└── public/
    ├── index.html     # Dashboard UI
    ├── style.css      # Styles
    └── app.js         # Frontend JavaScript
```

---

## ⚡ Features
- ✅ Cloudflare Turnstile bypass via headless Playwright
- ✅ Cookie extraction (`cf_clearance`, `__cf_bm`, etc.)
- ✅ Live statistics dashboard with SSE
- ✅ Browser pool for concurrent solves
- ✅ Proxy support with multiple formats
- ✅ GoLogin account creator + proxy harvester
- ✅ Download proxies.txt / accounts.txt from UI
- ✅ Beautiful dark UI with real-time charts

---

## Credits
Inspired by [Turnaround](https://github.com/Body-Alhoha/turnaround), [Theyka](https://github.com/Theyka) & [Sexfrance](https://github.com/sexfrance).