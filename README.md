# Level Agency — Skills Marketplace

Internal demo tool for the Agentic Operations Skills Library.

## Deploy to Vercel (3 steps)

### Option A — Vercel CLI (fastest)

```bash
# 1. Install Vercel CLI
npm install -g vercel

# 2. From this folder, deploy
vercel

# 3. Follow the prompts:
#    - Link to existing project? No
#    - Project name: level-skills-marketplace
#    - Which directory is your code? ./
#    - Override settings? No
```

Your URL appears immediately. Runs at `https://level-skills-marketplace-[hash].vercel.app`

To get a cleaner URL, set a custom domain in the Vercel dashboard.

### Option B — GitHub + Vercel (recommended for team use)

```bash
# 1. Create a GitHub repo
git init
git add .
git commit -m "Initial deploy: Level Skills Marketplace"
git remote add origin https://github.com/YOUR_ORG/level-skills-marketplace.git
git push -u origin main
```

Then:
1. Go to vercel.com → New Project
2. Import your GitHub repo
3. Deploy — done. Every push to main auto-deploys.

### Option C — Drag and drop

1. Go to vercel.com/new
2. Drag the `public/` folder onto the page
3. Done — no account needed for a preview URL

---

## Live API (optional)

The demo runs with pre-generated output by default.

To enable live Claude generation, you need a server-side proxy (the Anthropic API key cannot be exposed client-side).

### Quick proxy setup (Vercel serverless function)

Create `api/generate.js`:

```js
export default async function handler(req, res) {
  if (req.method !== 'POST') return res.status(405).end();
  
  const response = await fetch('https://api.anthropic.com/v1/messages', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'x-api-key': process.env.ANTHROPIC_API_KEY,
      'anthropic-version': '2023-06-01',
    },
    body: JSON.stringify(req.body),
  });

  const data = await response.json();
  res.status(response.status).json(data);
}
```

Add `ANTHROPIC_API_KEY` in Vercel dashboard → Project → Settings → Environment Variables.

Update the fetch URL in `index.html` from:
```
https://api.anthropic.com/v1/messages
```
to:
```
/api/generate
```

---

## What's in the demo

- **Catalog** — full skills library browser with tier/domain/status filters
- **Run a skill** — live agent run with streaming output (XYZ Co pacing narrative)
- **Approval flow** — interactive approve/reject with Slack delivery mock
- **Health** — quality score and override rate rankings
- **Activity** — library changelog feed
- **Ledger** — append-only audit log of demo runs

---

*Level Agency Agentic Operations — v0.5 March 2026*
