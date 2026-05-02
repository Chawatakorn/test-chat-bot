# 🧠 Data Intelligence AI Chatbot

> Education BI Project Assistant — Gemini 1.5 Flash (ฟรี) + GitHub Pages + Cloudflare Workers

---

## Architecture

```
GitHub Pages (index.html)
        │
        │ POST /chat
        ▼
Cloudflare Worker (worker/index.js)   ← GEMINI_API_KEY เก็บที่นี่ (ปลอดภัย)
        │
        │ Gemini API
        ▼
Gemini 1.5 Flash (Google AI)
```

---

## Deploy — 3 ขั้นตอน

### Step 1 — Deploy Cloudflare Worker

1. ไปที่ [dash.cloudflare.com](https://dash.cloudflare.com) → สมัครฟรี
2. ไปที่ **Workers & Pages** → **Create** → **Create Worker**
3. คลิก **Edit Code** → วางโค้ดจาก `worker/index.js` ทั้งหมด → **Deploy**
4. ไปที่ **Settings** → **Variables** → **Add variable**
   - Variable name: `GEMINI_API_KEY`
   - Value: API Key จาก [aistudio.google.com/apikey](https://aistudio.google.com/apikey)
   - คลิก **Encrypt** → **Save**
5. Copy Worker URL (รูปแบบ: `https://xxx.xxx.workers.dev`)

### Step 2 — ใส่ Worker URL ใน index.html

เปิดไฟล์ `index.html` แก้บรรทัด:

```js
const WORKER_URL = "https://YOUR-WORKER.YOUR-SUBDOMAIN.workers.dev";
```

เปลี่ยนเป็น URL จริงของ Worker ที่ได้จาก Step 1

### Step 3 — Deploy GitHub Pages

1. สร้าง repo ใหม่ใน GitHub (public หรือ private ก็ได้)
2. Push ไฟล์ทั้งหมดขึ้น repo
3. ไปที่ **Settings** → **Pages** → Source: **Deploy from branch** → branch: `main` → folder: `/ (root)`
4. รอ 1–2 นาที → ได้ URL แบบ `https://username.github.io/repo-name`

---

## Files

```
├── index.html          ← Frontend (GitHub Pages)
├── worker/
│   ├── index.js        ← Cloudflare Worker (API proxy)
│   └── wrangler.toml   ← Worker config (optional, สำหรับ CLI deploy)
└── README.md
```

---

## Free Tier Limits

| Service | Free Limit |
|---------|-----------|
| Gemini 1.5 Flash | 15 req/min · 1,500 req/day |
| Cloudflare Workers | 100,000 req/day |
| GitHub Pages | Unlimited |

---

## วิธีแก้ปัญหา

**CORS Error** → ตรวจสอบว่า Worker deploy แล้วและ URL ถูกต้องใน `index.html`

**Gemini Error** → ตรวจสอบ `GEMINI_API_KEY` ใน Cloudflare Worker Variables

**Worker ไม่ response** → เปิด Cloudflare Dashboard → Workers → Logs ดู error
