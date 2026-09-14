# Wisloop Technology — Static Site (Vercel-ready)

This is a plain static `index.html` (no build step, no server, no Python needed)
generated from the original Cloudflare Python Worker version of the site.
The company logo is embedded directly in the HTML as a base64 data URI, so
there are no external assets to worry about.

## Why the original repo "wasn't running"

The zip you uploaded (`wisloop-technology-cloudflare-ready.zip`) is written
specifically for **Cloudflare Workers** using `workers-py` (a Python-on-Pyodide
runtime) — see `wrangler.jsonc` and `src/worker.py`. Vercel does not run that
runtime, so pushing those files as-is to a Vercel project will fail or show
nothing. That's not a bug in your code — it's just built for a different host.

The actual page has no real backend logic (only one Flask route rendering a
static HTML template with one variable — the logo), so the simplest fix is to
export it as a single static `index.html`, which is what's in this folder.

## Deploy to Vercel

**Option A — Vercel dashboard (easiest, no CLI):**
1. Create a new GitHub repo and push just the contents of this folder
   (`index.html` + `vercel.json`) to it — replacing whatever you pushed before.
2. Go to https://vercel.com/new, import that repo.
3. Framework preset: choose "Other" (or leave auto-detect — a lone `index.html`
   is picked up as a static site automatically).
4. Click Deploy. Done — no build command, no install command needed.

**Option B — Vercel CLI:**
```bash
npm i -g vercel
cd wisloop-vercel-site
vercel        # first deploy / preview
vercel --prod # promote to production
```

## Run it locally first (optional)

```bash
cd wisloop-vercel-site
python3 -m http.server 8000
# open http://localhost:8000
```

## If you actually want to keep it on Cloudflare Workers instead

The original zip already works for that purpose — follow its own `README.md`:
```bash
uv run pywrangler dev      # local preview
uv run pywrangler deploy   # deploy to Cloudflare
```
That path has nothing to do with Vercel, though — Cloudflare Workers and
Vercel are separate hosting platforms, and a Cloudflare Worker project isn't
deployable on Vercel without rewriting it (which is what this static export
does).
