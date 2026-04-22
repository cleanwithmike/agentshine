# Agent Shine

A minimalist landing page for **Agent Shine** — an AI agent that verifies cleaning jobs from before/after photos and pays your team instantly in USDC on Solana via x402.

## Agent Wallet

Solana mainnet address used by the agent for verifications, payouts and on-chain receipts:

```
DpQh8c66TvbEuBpfWLbBPi5xvPo9mKLHNxUiRgWy14vn
```

Solscan: https://solscan.io/account/DpQh8c66TvbEuBpfWLbBPi5xvPo9mKLHNxUiRgWy14vn

## Run it locally

Just open `index.html` in a browser. That's it. Zero build step.

```bash
# or serve it via a quick static server
python -m http.server 5173
# then visit http://localhost:5173
```

## Deploy in 60 seconds

- **Vercel**: `vercel --prod` from this folder
- **Netlify**: drag the folder into netlify.com/drop
- **Cloudflare Pages**: connect the repo, no build command needed
- Point `agentshine.app` (or whatever domain you buy) at the deploy

## Tech on the page (nothing heavy)

- Single `index.html`
- Tailwind via CDN
- Google Fonts (Inter + Instrument Serif)
- Inline SVG logo
- Vanilla JS for the waitlist form

---

## APIs you'll actually need to build the product

Below is the real stack to turn this landing page into working software. All of these have free tiers or pay-per-use pricing, so you can ship an MVP cheap.

### 1. AI photo verification (the core)
Pick one:
- **OpenAI GPT-4o Vision** (`api.openai.com`) — easiest, great general vision, ~$0.005 per image. Prompt it with a cleaning rubric (kitchen, bathroom, floors, trash, surfaces).
- **Anthropic Claude 3.5 Sonnet Vision** — comparable quality, good at structured JSON output for scores.
- **Google Gemini 2.0 Flash** — cheapest per image, very fast.
- **Roboflow / custom YOLO model** — if you want to train on actual cleaning photos later for higher accuracy.

**Recommendation**: start with GPT-4o Vision. Prompt returns `{score, missed_spots, approved: bool}`.

### 2. Solana payments + x402
- **Helius RPC** (`helius.xyz`) — best Solana RPC with free tier. Use for sending transactions and reading balances.
- **Solana Web3.js** (`@solana/web3.js`) — the core SDK.
- **x402 reference implementation** (`github.com/coinbase/x402`) — the HTTP 402 payment standard. The agent triggers payment when it approves a job.
- **USDC on Solana** mint: `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`
- **Solana Pay** (optional) — for client-side payment links.

### 3. File uploads (cleaner photos)
- **Cloudflare R2** or **AWS S3** — cheap object storage. R2 has no egress fees.
- **Uploadthing** or **Uploadcare** — if you want a hosted upload widget with no setup.

### 4. Database + auth
- **Supabase** — one dashboard for Postgres, auth, storage. Free tier is generous.
- **Clerk** — if you want prettier auth with Google/phone sign-in.

### 5. Notifications (so operators know when jobs finish)
- **Resend** — transactional email, 3k/month free.
- **Twilio** — SMS to cleaners and clients.
- **Telegram Bot API** — free, perfect for a "your agent is working" feed.

### 6. Optional but useful
- **Mapbox** or **Google Maps Platform** — routing and scheduling.
- **Stripe** — for clients who want to pay with card (Stripe → USDC on-ramp).
- **PostHog** — free analytics so you can see who's using the dashboard.

### 7. Hosting
- **Vercel** or **Cloudflare Workers** — frontend and API routes.
- **Fly.io** or **Railway** — if the agent needs a long-running worker.

---

## MVP build order (realistic)

1. **Landing + waitlist** → this page, wired to Supabase or a Google Sheet. Ship today.
2. **Photo upload + AI scoring** → a single endpoint that takes a photo, returns a score. 1–2 days.
3. **Solana payout on approval** → wire up x402 + Helius, test on devnet first. 2–3 days.
4. **Dashboard** → Supabase + Next.js, show jobs, scores, paid amounts. 1 week.
5. **Invite your own cleaners first.** Dogfood it. Then open access.

That's the whole stack. Keep it boring. Ship it.
