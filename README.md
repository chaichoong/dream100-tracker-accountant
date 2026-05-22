# Dream 100 Accountants Tracker

A single-page web app for the Operations Director partner programme. Three things in one tool:

1. Outreach tracker for the Dream 100 firms (tier scoring, four-touch sequence, status pipeline).
2. Affiliate plumbing (Partners table, referral codes, UTM links, referral logging).
3. Commission ledger (per-partner totals, quarterly due, year-to-date).

Built for [Operations Director](https://operationsdirector.co.uk) by Kevin Brittain.

**v3.0** is browser-only (localStorage). No backend, no logins, no tracking. Data lives in your browser. Use Export CSV to back up.

## What it does

### Firms tab

- 100 real UK SME-focused accountancy firms seeded across every major niche and region.
- Tier scoring (A, B, C, unscored), filterable.
- Status pipeline: To source, Researched, Touch 1 to 4 sent, Replied, Booked, Signed, Dead, Paused.
- Inline editing of status, owner, last-touch date, notes.
- Filters by tier, status, owner. Live text search.
- Funnel chart, tier doughnut, next-actions panel.
- Add new firms via the bottom row form.
- Import or export CSV.
- **Stale Firms alert:** any firm in Touch 1 to 4 sent status with a last-touch date 5+ days old appears as a clickable pill at the top of the firms list. Click jumps to the firm.
- **Signed → Partner:** when you mark a firm as Signed, a gold "→ Partner" button appears on its row. Click to create a partner record pre-filled with the firm's details.

### Partners tab

- One card per partner accountancy firm.
- Status (Active, Paused, Terminated) and rate model (Pilot 30/20 or Standard 20/15).
- Auto-generated referral code (firm letters + 3 digits) or set your own.
- UTM-tagged referral link to the launch page, copy to clipboard in one click.
- Click a card to open partner detail with their referrals and total commission earned.
- Each referral logs: client name, status (Lead → Discovery → Building → Active → Cancelled), setup amount, setup paid date, MRR, monthly start date, optional cancelled date.

### Commission Ledger tab

- Summary cards: active partners, total referrals, setup commission, recurring to date, total earned, quarterly due (since the start of the current calendar quarter).
- Full referrals table across all partners.
- By-partner totals with quarterly due column.

## How commission is calculated

For each referral:

- **Setup commission** = setup amount (£1,997 standard, £998.50 pilot) × partner setup rate (30% pilot, 20% standard), paid once when setup is paid.
- **Recurring commission** = months active × MRR (£247 default) × partner recurring rate (20% pilot, 15% standard). Months active counts from Monthly start date until Cancelled date (or today if still active).

Quarterly due = setup commissions where setup paid date falls in the current quarter, plus recurring commission accrued in the current quarter for active referrals.

## Data and privacy

- Everything lives in your browser (`localStorage`).
- No server. No login. No tracking. No cookies.
- Two browsers (e.g. your laptop and Erica's) will NOT sync automatically.
- Use Export CSV to back up. Use Reset edits to clear firm edits (partners and referrals are kept).

To share data between people, two options:

1. Export CSV from one browser, share, the other person imports.
2. Switch to an Airtable-backed version. See Roadmap below.

## Local preview

No build step. Open `index.html` in a browser.

```
git clone https://github.com/YOUR-USERNAME/dream100-tracker.git
cd dream100-tracker
open index.html
```

Or serve it locally:

```
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

1. Create a public repository on GitHub called `dream100-tracker`.
2. Push the contents of this folder:

   ```
   git init
   git add .
   git commit -m "Dream 100 tracker v3.0 with Partners + Ledger"
   git branch -M main
   git remote add origin https://github.com/YOUR-USERNAME/dream100-tracker.git
   git push -u origin main
   ```

3. In the repo, Settings → Pages → Source → "GitHub Actions".
4. The workflow at `.github/workflows/pages.yml` publishes on every push to `main`.

### Custom domain

To use `dream100.operationsdirector.co.uk`:

1. Add a `CNAME` file in the repo root containing the line `dream100.operationsdirector.co.uk`.
2. Add a DNS CNAME record from `dream100` to `YOUR-USERNAME.github.io`.
3. After propagation, enable HTTPS in Settings → Pages.

## Adding firms

Two routes.

**Route A. Via the UI (browser-local).** Use the Add row at the bottom of the Firms table.

**Route B. Via the seed file (shared in repo).** Edit `seed.js`, push to `main`, GitHub Pages redeploys.

## Adding partners and referrals

1. Mark a firm as Signed in the Firms tab. A gold "→ Partner" button appears. Click it.
2. The Add Partner modal opens pre-filled. Confirm rate model (Pilot or Standard) and signed date.
3. Save. The partner now appears in the Partners tab with an auto-generated referral code.
4. Click the partner card. Use the Copy link button to grab the UTM-tagged URL for outreach.
5. When the partner refers a client, click "+ Add referral" in the partner detail.
6. As the deal progresses, edit the referral status and add setup paid date / monthly start date.
7. Commission updates automatically in the Ledger tab.

## Touch reminders (no external tools)

The app flags stale firms in real time. A firm is stale if:

- Status is Touch 1, 2, 3, or 4 sent, **and**
- Last touch date is 5 or more days ago (or missing).

Stale firms appear as orange pills at the top of the Firms tab. Click a pill to jump to that firm. Move them forward or mark Dead / Paused.

No Make.com, no Slack notifications, no API. Pure browser logic that runs on every render.

## File structure

```
dream100-tracker/
├── index.html              # The app (single file)
├── seed.js                 # 64 firm seed (editable)
├── README.md               # This file
├── LICENSE                 # MIT
├── .gitignore
└── .github/
    └── workflows/
        └── pages.yml       # Auto-deploy on push to main
```

## Browser support

Chrome, Safari, Edge, Firefox. Uses standard `localStorage`, `fetch` (Chart.js CDN only), ES2017+.

## Roadmap

- **When 10+ partners active:** click-through analytics on the launch page. Track which UTM-tagged referral link drove which conversion. Trigger to build, not yet.

## Source material

- Sales page: [launch.operationsdirector.co.uk/launch](https://launch.operationsdirector.co.uk/launch)
- Main site: [operationsdirector.co.uk](https://operationsdirector.co.uk)
- Companion documents in the outputs folder:
  - `Dream100_Accountants.xlsx` (spreadsheet with tier criteria, sourcing methodology)
  - `Dream100_Sequence_Templates.docx` (four-touch templates)
  - `Accountant_Partner_Offer.docx` (standard partner offer)
  - `Pilot_Offer.docx` (first-3 partners pilot terms)
  - `Case_Study_Pack.docx` (interview script + template)
  - `Loom_Scripts.docx` (2/5/10 min walkthrough scripts)
  - `Discovery_Call_Kit.docx` (Calendly questions + call script + follow-up emails)
  - `accountants-landing.html` (landing page for operationsdirector.co.uk/accountants)

## Note on the Airtable table

A `🎯 Dream 100 - Accountants` table was created in your `⚙️ Operations Director` Airtable base during the v2.0 build (Table ID `tblXPuSvClo6nF4gz`). It is no longer used by the app and can be deleted from Airtable manually if you want a clean base. Or leave it as a snapshot.

## Licence

MIT. See `LICENSE`.
