# Biviano Modular Builders — Project Handoff
**Last updated:** 2026-05-05
**Status:** Website live. GA4 deployed. GHL custom fields created. Pipeline + webhook wiring are next.

---

## ▶️ Pick up here next session

1. **Spencer creates the Modular Pipeline in GHL UI** (API can't do this — PIT scope blocked).
   Path: GHL → Biviano sub-account → Settings → Pipelines → + Create New Pipeline
   Name: `Modular Pipeline`
   Stages (in order): New Lead → Discovery Call Booked → Site Visit Done → Budget Range Agreed → LOI Signed → Contract Signed
2. **Wire `bmb_source` into the form webhooks** (15 min, code-only, see "Webhook wiring" below).
3. **Confirm GA4 is firing** in the Realtime dashboard. Spencer reported it wasn't showing as of 2026-05-05 EOD — see "GA4 troubleshooting" below.
4. **Build the monthly scorecard** (Fri 5/8 per timeline).

---

## Live Site
- **URL:** https://www.bivianomodularbuilders.com
- **GitHub repo:** https://github.com/morejobcalls/bmb-website (branch: `main`)
- **Deploys via:** GitHub Actions → `.github/workflows/deploy.yml` (auto on push to main, ~20 sec)
- **DNS:** GoDaddy → GitHub Pages (CNAME: `morejobcalls.github.io`)
- **HTTPS:** Not yet enforced — SSL cert not yet provisioned by GitHub (status: errored). Once it shows `protected_domain_state: verified`, run: `gh api --method PUT repos/morejobcalls/bmb-website/pages --field https_enforced=true`

---

## Key Contacts & Credentials
| Field | Value |
|-------|-------|
| Client | Mike Biviano Jr. |
| Client email | mbiviano@hotmail.com |
| Client phone | (617) 678-6446 |
| GHL account | Biviano sub-account in SPG GHL |
| Consultation webhook | `https://services.leadconnectorhq.com/hooks/zPo4vLlEjjXCflgDSXlI/webhook-trigger/0eba93d2-0fb2-4770-9af2-4e912dc84502` |
| Calculator webhook | `https://services.leadconnectorhq.com/hooks/zPo4vLlEjjXCflgDSXlI/webhook-trigger/008d0857-3a29-435c-9f1d-09abac2ca68a` |

---

## Design System
| Token | Value |
|-------|-------|
| `--coal` | `#111110` |
| `--coal-mid` | `#1C1C1A` |
| `--coal-light` | `#2A2A27` |
| `--blue` | `#4A90C4` |
| `--stone` | `#8C7B6B` |
| Fonts | Bebas Neue (headings) · Libre Baskerville (serif body) · Outfit (UI/body) |

---

## What's Done ✅

### Website (all built, committed, live)
- **Homepage** (`index.html`) — hero, why BMB, process overview, social proof (Facebook cards), testimonials, service areas grid (8 towns), FAQ, footer
- **Pricing** (`pricing/index.html`) — stat band, 3-step estimate calculator (lead capture → GHL), what's included section, FAQ
- **Why Modular** (`why-modular/index.html`) — full education + Mike bio + credential tiles
- **Process** (`process/index.html`) — 6-step build process, comparison table, FAQ
- **Gallery** (`gallery/index.html`) — photo grid (placeholder Unsplash, needs real photos)
- **8 Town pages** — Marshfield, Hanover, Pembroke, Duxbury, Norwell, Hingham, Humarock, Scituate
- **404** (`404.html`) — branded error page with nav links
- **robots.txt** — allows all, points to sitemap
- **sitemap.xml** — all pages listed

### Content / Copy decisions
- Planning Board references removed from ALL pages (Mike explicitly doesn't want this in marketing)
- Replaced with: "40+ years local experience", "knows every inspector personally", "lifelong Marshfield resident", "4th-generation contractor"
- Foundation language: "wood-driven piles in flood zones or poured concrete on standard lots"
- Hero H1: "Custom Modular / Homes. / Built Faster."
- Calculator pricing: $250/sqft (module) + $70K foundation + $25K deck ≈ $297/sqft all-in (~$300/sqft as stated on site) — **confirm exact numbers with Mike**
- Service area footers: Duxbury · Norwell · Hingham · Humarock (Kingston + Hull removed)

---

## What's Left ⬜

### 3. Analytics — DEPLOYED 2026-05-05 ✅ (verify firing)
**Status:** GA4 snippet pushed to all 14 HTML files; deploy succeeded (commit `c68aef1`).

- Measurement ID: **G-GW5Z0VVDEC**
- Stream ID: **14819136283**
- Saved to `.env` as `BMB_GA4_MEASUREMENT_ID` + `BMB_GA4_STREAM_ID`
- Confirmed live in deployed HTML via raw GitHub content

**Open question:** Spencer reported GA4 Realtime tab was empty on 2026-05-05. See "GA4 troubleshooting" at bottom — likely browser cache, ad blocker, or 30–90s lag on first-ever hit.

**After GA4 is confirmed firing:** Set up Google Search Console at https://search.google.com/search-console → verify via Google Analytics → submit `https://www.bivianomodularbuilders.com/sitemap.xml`.

---

### 4. GHL Custom Fields — DONE 2026-05-05 ✅
All required fields exist in the Biviano sub-account.

| Field key | Type | Field ID |
|---|---|---|
| `contact.bmb_home_size` | TEXT | `dliGEdhCSZPoiGV4CGdy` (pre-existing) |
| `contact.bmb_town` | TEXT | `H9fIf0fuTvkUcR0D0Ovd` (pre-existing) |
| `contact.bmb_project_description` | TEXT | `f2VhJGBdPfu6vAPQPCpr` (pre-existing) |
| `contact.bmb_lot_status` | RADIO (Lot owned · Searching for lot · Have land) | `FQ4ZYfw19TqcaeayKEkz` |
| `contact.bmb_budget_range` | RADIO (Under $500K · $500K – $700K · $700K+) | `EbOE90WYWExcm3vxswCr` |
| `contact.bmb_target_start` | RADIO (ASAP · 3–6 months · 6–12 months · Planning phase) | `mbfUjfDN0bAIuIlwTIwj` |
| `contact.bmb_plant_partner` | TEXT | `IDxJOueBklhmcQfVM7kt` |
| `contact.bmb_source` | RADIO (Calculator · Consultation form · Town page · Direct) | `mA9Sd89sgT221rJcziHF` |

**API gotcha for next time:** RADIO fields require `"options": ["string","string"]` (array of strings, NOT array of `{key,label}` objects — that returns `"v.trim is not a function"`). Endpoint: `POST https://services.leadconnectorhq.com/locations/{LOC}/customFields` with header `Version: 2021-07-28`.

---

### 5. GHL Modular Pipeline — BLOCKED ON UI (Spencer to do)
**Status:** Cannot create via API — PIT scope blocks pipeline write (`401: token is not authorized for this scope`).

**Spencer to do (3 min):** GHL → Biviano sub-account → Settings → Pipelines → + Create New Pipeline
- **Name:** `Modular Pipeline`
- **Stages (in order):**
  1. New Lead
  2. Discovery Call Booked
  3. Site Visit Done
  4. Budget Range Agreed
  5. LOI Signed
  6. Contract Signed

After creation, capture the pipeline ID from the URL or via API list call so it can be referenced when wiring webhook automations.

---

### 5b. Webhook wiring — NOT DONE (next coding task)
**Status:** Pending. Blocks proper lead source attribution.

**Problem:** Today, two webhooks fire from the site, but neither tags the source. GHL can't tell a Calculator lead from a Town-page consultation lead.

| Form | Location in code | Webhook trigger ID | What it sends today |
|---|---|---|---|
| **Pricing calculator** | `pricing/index.html` (CALC_WEBHOOK) | `008d0857-3a29-435c-9f1d-09abac2ca68a` | `lead{}, calculator{inputs.squareFootage}, meta{}` — **NO `customField` block** |
| **Consultation form** | `pricing/index.html` + `index.html` + `duxbury/`, `hanover/`, `hingham/`, `humarock/` town pages | `0eba93d2-0fb2-4770-9af2-4e912dc84502` | `firstName, lastName, email, phone, customField{bmb_home_size, bmb_town, bmb_project_description}` |

**Fix:**
1. **Calculator** — add a `customField` block to the payload:
   ```js
   customField: {
     'contact.bmb_source': 'Calculator',
     'contact.bmb_home_size': String(sf)  // lift sqft out of calculator.inputs
   }
   ```
2. **Homepage consultation form** — add `'contact.bmb_source': 'Consultation form'` to the existing `customField` block.
3. **Town-page consultation forms** (`duxbury`, `hanover`, `hingham`, `humarock`) — add `'contact.bmb_source': 'Town page'` to each.
4. Audit `marshfield`, `pembroke`, `norwell`, `scituate` town pages — they may not yet have webhook forms; if they do, same treatment.

**Effort:** ~15 min, single commit, auto-deploys.

**Two more fields referenced in `.env` that don't yet exist in GHL:**
- `BMB_GHL_FIELD_ESTIMATE_TOTAL=contact.bmb_estimate_total`
- `BMB_GHL_FIELD_HOME_COST_CALC=contact.bmb_home_cost_calc`

These need to be created in GHL (TEXT or NUMERICAL) before the calculator webhook can populate them. When wiring the calculator, also push `bmb_estimate_total` (totalEstimate) and `bmb_home_cost_calc` (homeCost) into the `customField` block so the full pricing breakdown lives on the contact record, not buried in a JSON payload.

---

### 6. Nurture Sequence
**Status:** Deferred per Spencer's instruction — skip for now.

When ready: 8–12 week email/SMS sequence for modular leads. Touchpoints: education (what is modular), case studies (Joanne Keogh Humarock, Marshfield spec homes), re-engagement.

---

### 7. Modular Scorecard
**Status:** Not done.

Monthly report for Mike: # leads, # discovery calls, # site visits, # contracts, revenue. Can be built as a Google Sheet pulled from GHL data or a GHL dashboard.

---

### 8. Real Photos
**Status:** Not done — needs assets from Mike.

Placeholders currently using Unsplash. Replace with:
- Set-day crane photos
- Exterior shots of finished builds
- Facebook post thumbnails for social proof section (3 cards on homepage — Joanne Keogh Humarock, 2 Marshfield spec homes)

**When Mike sends photos:** Drop into `/content/` folder, update `src` attributes in `index.html` (social proof section) and `gallery/index.html`.

---

### 9. HTTPS Enforcement
**Status:** Blocked — waiting on GitHub SSL provisioning.

Check status: `gh api repos/morejobcalls/bmb-website/pages | jq '.protected_domain_state, .https_enforced'`

When `protected_domain_state` = `"verified"`, run:
`gh api --method PUT repos/morejobcalls/bmb-website/pages --field https_enforced=true`

---

### 10. bivianocontracting.com Updates
**Status:** Not done. Low priority.

Project scope calls for adding drone video + calculator to the existing GC site. Separate project — don't touch until modular site is fully dialed in.

---

## How to Pick Up With Claude

### Adding GA4 (immediate next step)
```
I'm continuing the Biviano Modular Builders website project. 
The site is live at bivianomodularbuilders.com, repo is morejobcalls/bmb-website on GitHub.
Working directory: /Volumes/T7/SPG/3. FULFILLMENT/Clients/BIVIANO/07 - Websites/Modular

I need you to add Google Analytics 4 to all 13 HTML pages.
The Measurement ID is: G-XXXXXXXXXX  ← replace with real ID

Add the standard gtag.js snippet to the <head> of every index.html file 
and 404.html. Then commit and push to main.
```

### Any other task
Start with:
```
I'm continuing the Biviano Modular Builders website project.
Working directory: /Volumes/T7/SPG/3. FULFILLMENT/Clients/BIVIANO/07 - Websites/Modular
Repo: morejobcalls/bmb-website, branch main, deploys via GitHub Actions on push.
Read HANDOFF.md for full context on what's done and what's left.
Task: [describe what you want]
```

### Resuming where we left off (2026-05-05 EOD)
```
I'm picking up the BMB project from where we left off on May 5.
Read HANDOFF.md — focus on "Pick up here next session" at the top.

Pipeline status: [did you create it in GHL UI? — yes/no]
GA4 status: [is Realtime now showing hits? — yes/no, and what DevTools showed]

Next coding task ready to go: webhook wiring (#5b in HANDOFF) — adding
bmb_source + bmb_estimate_total + bmb_home_cost_calc to the calculator
and consultation form payloads. ~15 min.
```

---

## GA4 troubleshooting (open as of 2026-05-05 EOD)

GA4 snippet is verifiably in the live HTML (raw GitHub + production both confirmed). If Realtime is empty, run through:

1. **Hard-refresh** https://www.bivianomodularbuilders.com (Cmd+Shift+R) — GitHub Pages CDN may have served pre-GA4 cached HTML on first load.
2. **Open DevTools → Network → filter `google`**. Should see two requests on every page load:
   - `gtag/js?id=G-GW5Z0VVDEC` (the loader script)
   - `g/collect?...` or `g/s/collect?...` (the actual hit)
   If neither fires → ad blocker (uBlock, Brave Shields, Privacy Badger, Ghostery all kill GA by default).
3. **Try incognito** with all extensions off, OR test from phone on cellular.
4. **Realtime can lag 30–90 seconds** on the very first hit ever — not instant.
5. **Verify property selection** — top-left dropdown must say "Biviano Modular Builders," not "Biviano Contracting."

If still empty after all five, run from terminal to prove the snippet is on the served page:
```bash
curl -skL https://www.bivianomodularbuilders.com/ | grep -c "G-GW5Z0VVDEC"
# Should return 2 (one for src URL, one for gtag('config', ...))
```

---

## Deployment Reference
```bash
# Push changes live
cd "/Volumes/T7/SPG/3. FULFILLMENT/Clients/BIVIANO/07 - Websites/Modular"
git add [files]
git commit -m "your message"
git push origin main
# GitHub Actions deploys automatically in ~20 seconds

# Check deploy status
gh run list --repo morejobcalls/bmb-website --limit 3

# Cancel stuck deploy
gh run cancel [run-id] --repo morejobcalls/bmb-website
```
