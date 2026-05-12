# Mopp Quote Tool (Internal)

A single-file phone-friendly quote calculator for Mopp Cleaning. Built for the owners to use on-call, no recurring cost, no customer-facing exposure.

## What it does

- Quote a job in 3 taps: service → sqft → done.
- Toggle bi-weekly / weekly to compare recurring revenue.
- Optional add-ons + per-room baseboards/blinds.
- Discount slider 0–25%.
- Owner-only profit panel (payroll, profit per job, profit/hr, margin) — hidden by default.
- Save quotes locally to the device (last 20, viewable under "Recent").
- Copy quote text or native share to SMS/email.
- Works offline once loaded.

## Two URLs, two views

| URL | Who uses it | Shows |
|-----|-------------|-------|
| `…/index.html` | Default | Customer-facing math only |
| `…/index.html?o=1` | Owners | Adds the "Show profit & margin" toggle |

**Bookmark the `?o=1` URL on your phone.** When you "Add to Home Screen", iOS/Android will remember the query param. Anyone who finds the bare URL never sees profit data.

## Privacy posture

Because GitHub Pages requires a public repo on the free plan, the file is technically accessible to anyone who knows the URL. Mitigations baked in:

- `<meta name="robots" content="noindex, nofollow">` — search engines won't index it
- Owner panel hidden without `?o=1`
- No customer data leaves the device (localStorage only)
- No external API calls, no analytics, no fonts loaded from the web
- Recommended: name the repo something non-obvious (e.g., `mq-internal-x7k2` rather than `mopp-cleaning-quotes`)

The pricing math itself is in the JS. If a competitor finds the URL and reads the source, they can see the tier rates. That's an acceptable tradeoff vs. paying for private hosting at this stage.

## Setup (one-time, ~10 minutes)

### 1. Create a GitHub account
If you don't have one: [github.com/signup](https://github.com/signup). Free.

### 2. Create a new repository
- Click the `+` in the top-right of GitHub → **New repository**
- Repository name: pick something non-obvious. Suggested format: `mq-` followed by a short random string. Example: `mq-internal-x7k2`
- Set visibility to **Public** (required for free Pages)
- Check "Add a README file"
- Click **Create repository**

### 3. Upload `index.html`
- On the repo page, click **Add file** → **Upload files**
- Drag `index.html` into the upload area
- Commit message: `Initial deploy`
- Click **Commit changes**

### 4. Enable GitHub Pages
- In the repo, go to **Settings** (top nav of the repo) → **Pages** (left sidebar)
- Under **Source**, select **Deploy from a branch**
- Branch: `main`, folder: `/ (root)`
- Click **Save**
- Wait ~1–2 minutes. The Pages section will show: *"Your site is live at https://<username>.github.io/<repo-name>/"*

### 5. Bookmark on phone with owner mode
Open this URL on the owners' phones (substitute the actual URL):

```
https://<username>.github.io/<repo-name>/?o=1
```

**iPhone (Safari):**
1. Open the URL in Safari
2. Tap the Share button (square with arrow)
3. Scroll down → **Add to Home Screen**
4. Name it "Mopp Quote" → **Add**

**Android (Chrome):**
1. Open the URL in Chrome
2. Tap the three-dot menu (top right)
3. Tap **Add to Home screen** or **Install app**
4. Confirm

The icon appears on the home screen like a native app. Tapping it opens directly into the calculator with owner mode unlocked.

## Updating the tool

Three options, in order of ease:

### Option A — Edit directly on GitHub (no tools needed)
1. Go to the repo on github.com
2. Click `index.html`
3. Click the pencil icon (top right) to edit
4. Paste in the new version
5. Scroll down, commit message: `v2 - <what changed>`, click **Commit changes**
6. New version is live within ~60 seconds. Refresh the phone bookmark.

### Option B — Replace via upload
1. On the repo home, click `index.html` → **Delete file** → commit
2. **Add file** → **Upload files** → drop in new `index.html`
3. Commit. Live in ~60 sec.

### Option C — Git CLI (for version control habits)
```bash
git clone https://github.com/<username>/<repo-name>.git
cd <repo-name>
# edit index.html
git add index.html
git commit -m "v2 - added X"
git push
```

## File structure

```
<repo-root>/
├── README.md        ← this file
└── index.html       ← the entire tool (single file, no dependencies)
```

That's the whole project. The HTML file contains all CSS, all JS, all pricing data, and inline SVG icons. No build step. No npm. No backend.

## Future versions

Things that fit cleanly into this architecture without adding cost:

- Pricing changes → edit the `RATES` and `HOURS` objects in the script, commit.
- New add-ons → add an entry to the `ADDONS` array.
- New service type → add to `RATES`, `HOURS`, `SVC_NAMES`, `SVC_INFO`, plus one new pill in the service grid.
- Custom cleaner pay rate → change `CLEANER_RATE` constant near the top of the script.
- Export saved history → button that downloads localStorage as JSON or CSV.

Things that would require leaving this architecture (and possibly adding cost):

- Syncing quotes between multiple devices (needs a backend or shared DB)
- Address-based sqft auto-lookup (needs an API)
- Sending the quote from this tool directly to a customer (needs an email service)

Each of those can be added later if/when needed.
