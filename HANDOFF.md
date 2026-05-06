# Biviano Modular Builders — Project Handoff
**Last updated:** 2026-05-06
**Status:** Site live + indexed. GA4 deployed. GHL fully wired (custom fields + source attribution + calculator dollar capture). Pipeline stage creation still on Spencer (UI-only, ~3 min). Monthly scorecard + real photos are next.

---

## ▶️ Pick up here next session

1. **Spencer: create the Modular Pipeline in GHL UI** (API can't do this — PIT scope blocked). Still outstanding from last session.
   Path: GHL → Biviano sub-account → Settings → Pipelines → + Create New Pipeline
   Name: `Modular Pipeline`
   Stages (in order): New Lead → Discovery Call Booked → Site Visit Done → Budget Range Agreed → LOI Signed → Contract Signed
2. **Spencer: confirm GA4 is firing** in the Realtime dashboard. If still empty, run the GA4 troubleshooting checklist at bottom.
3. **Build the monthly scorecard** (the Fri 5/8 deliverable per the Project-Update-2026-05-05 timeline).
4. **Mike: send real photos** — set-day crane shots, finished exteriors, Facebook social-proof screenshots. Drop into `/content/` and swap into `index.html` social-proof + `gallery/index.html`.
5. **Mike: confirm calculator pricing** is still accurate ($250/sqft module + $70K foundation + $25K deck) before scaling traffic.

---

## Live Site
- **URL:** https://www.bivianomodularbuilders.com
- **GitHub repo:** https://github.com/morejobcalls/bmb-website (branch: `main`)
- **Deploys via:** GitHub Actions → `.github/workflows/deploy.yml` (auto on push to main, ~20 sec)
- **DNS:** GoDaddy → GitHub Pages (CNAME: `morejobcalls.github.io`)
- **HTTPS:** Not yet enforced — SSL cert not yet provisioned by GitHub. Once `protected_domain_state: verified`, run: `gh api --method PUT repos/morejobcalls/bmb-website/pages --field https_enforced=true`

---

## Key Contacts & Credentials
| Field | Value |
|-------|-------|
| Client | Mike Biviano Jr. |
| Client email | mbiviano@hotmail.com |
| Client phone | (617) 678-6446 |
| GHL account | Biviano sub-account in SPG GHL (`zPo4vLlEjjXCflgDSXlI`) |
| Consultation webhook | `https://services.leadconnectorhq.com/hooks/zPo4vLlEjjXCflgDSXlI/webhook-trigger/0eba93d2-0fb2-4770-9af2-4e912dc84502` |
| Calculator webhook | `https://services.leadconnectorhq.com/hooks/zPo4vLlEjjXCflgDSXlI/webhook-trigger/008d0857-3a29-435c-9f1d-09abac2ca68a` |
| GHL API key (PIT) | `BMB_GHL_API_KEY` in `/Volumes/T7/SPG/Claude Code/.env` |
| GA4 Measurement ID | `G-GW5Z0VVDEC` |

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
- **Homepage** (`index.html`) — hero w/ updated H1 "Built Faster. 25% Less." + prominent "Try Our Estimator" CTA, why BMB, **timeline UI** (animated rail with dots that activate on scroll + confetti burst at "keys in hand"), social proof, towns grid, testimonials, FAQ, footer
- **Pricing** (`pricing/index.html`) — stat band, **3-step estimate calculator** (town picker auto-advances → sqft → contact → confirm), what's included, FAQ
- **Why Modular** — full education + Mike bio + credential tiles
- **Process** — 6-step build process, comparison table, FAQ
- **Gallery** — photo grid (placeholder Unsplash, needs real photos)
- **8 Town pages** — Marshfield, Hanover, Pembroke, Duxbury, Norwell, Hingham, Humarock, Scituate
- **404, robots.txt, sitemap.xml**

### Copy / pricing locked in
- Per-sqft anchor is **$250 across the entire site** (was inconsistent before — some $300, some $250). Derived dollar amounts and savings ranges updated to match (e.g. 3,000 sqft @ $250 = $750K; "25% less than stick-built" → "38% less").
- Planning Board references stripped from every page (Mike doesn't want this in marketing).
- Replaced with: "40+ years local experience", "knows every inspector personally", "lifelong Marshfield resident", "4th-generation contractor".
- Foundation language: "wood-driven piles in flood zones or poured concrete on standard lots".
- Service area footers: Duxbury · Norwell · Hingham · Humarock (Kingston + Hull removed).
- **"BMB Guarantee — we pay your rent" language removed** from pricing/, why-modular/, CLAUDE.md, and the workbook (Mike does NOT guarantee that).
- Calculator pricing: $250/sqft module + $70K foundation + $25K deck.
- Hero CTA: prominent boxed "Try Our Estimator → See your ballpark in 30 seconds" replaces the old wimpy ghost link.

### Photos
All Unsplash placeholders swapped to **traditional New England residential** photos (white-clapboard colonials, Cape cod, cedar shingle, coastal shingle-style). Original photos were dark architect-cube modernist — wrong vernacular for South Shore MA. 19 verified photo IDs across 56 image references in 12 files.

### Mobile menu — fixed 2026-05-05
Hamburger overlay was getting cut off on iOS Safari (URL bar was hiding bottom items). Replaced `top:72px; bottom:0` with `top:72px; height:calc(100dvh - 72px)` + safe-area padding + scroll containment in 6 files (index.html, towns.css, why-modular, process, pricing, gallery).

### GHL Custom Fields ✅ (all required + 2 numeric for calculator dollar capture)
| Field key | Type | Field ID | Purpose |
|---|---|---|---|
| `contact.bmb_home_size` | TEXT | `dliGEdhCSZPoiGV4CGdy` | sqft from any form |
| `contact.bmb_town` | TEXT | `H9fIf0fuTvkUcR0D0Ovd` | build location |
| `contact.bmb_project_description` | TEXT | `f2VhJGBdPfu6vAPQPCpr` | free text |
| `contact.bmb_lot_status` | RADIO | `FQ4ZYfw19TqcaeayKEkz` | Lot owned · Searching · Have land |
| `contact.bmb_budget_range` | RADIO | `EbOE90WYWExcm3vxswCr` | Under $500K · $500–700K · $700K+ |
| `contact.bmb_target_start` | RADIO | `mbfUjfDN0bAIuIlwTIwj` | ASAP · 3–6mo · 6–12mo · Planning |
| `contact.bmb_plant_partner` | TEXT | `IDxJOueBklhmcQfVM7kt` | factory partner |
| `contact.bmb_source` | RADIO | `mA9Sd89sgT221rJcziHF` | Calculator · Consultation form · Town page · Direct |
| `contact.bmb_estimate_total` | NUMERICAL | `gahPKmcEx94iDaBEZEjF` | full ballpark from calculator (NEW 2026-05-06) |
| `contact.bmb_home_cost_calc` | NUMERICAL | `AFurlpPLr5LwaDGTTFLm` | sqft × $250 from calculator (NEW 2026-05-06) |

**API gotcha:** RADIO fields require `"options": ["string","string"]` (NOT array of `{key,label}` objects — that errors with `"v.trim is not a function"`). Endpoint: `POST https://services.leadconnectorhq.com/locations/{LOC}/customFields` + header `Version: 2021-07-28`.

### Webhook wiring ✅ (DONE 2026-05-06)
All 12 form/calculator submissions now tag the source:
- Homepage consult form (`index.html`) → `bmb_source: "Consultation form"`
- Pricing consult form (`pricing/index.html`) → `bmb_source: "Consultation form"`
- Process consult form (`process/index.html`) → `bmb_source: "Consultation form"`
- 8 town consult forms → `bmb_source: "Town page"`
- Pricing calculator → `bmb_source: "Calculator"` + `bmb_estimate_total` + `bmb_home_cost_calc` (sqft × $250 + $70K + $25K)

Mike can now sort/filter pipeline by entry point AND see exact deal size on the contact record without having to redo the math.

### Calculator UX (DONE 2026-05-06)
- Town selection is now **Step 1 of 3** (was just sqft → contact). Captures location even if user bails before contact step.
- 8 named-town buttons **auto-advance** after selection (220ms beat to show selected state).
- "Another Plymouth County town" reveals a free-text input + requires manual continue (Enter key also advances).
- Webhook payload includes the typed town value when "Other" is used.

### Analytics
GA4 deployed across all 14 HTML files. Measurement ID `G-GW5Z0VVDEC`, Stream ID `14819136283`. Saved to `.env` as `BMB_GA4_MEASUREMENT_ID` / `BMB_GA4_STREAM_ID`. Snippet confirmed in deployed HTML via raw GitHub.

---

## What's Left ⬜

### 1. Modular Pipeline in GHL — BLOCKED ON UI (Spencer)
Cannot create via API — PIT scope blocks pipeline write. ~3 min in GHL UI. See "Pick up here next session" #1.

### 2. GA4 verification (Spencer to confirm)
Spencer reported GA4 Realtime tab was empty on 2026-05-05 EOD. If still empty, run the GA4 troubleshooting checklist at bottom of this file. After confirmed firing → Search Console at `https://search.google.com/search-console`, verify via GA, submit `https://www.bivianomodularbuilders.com/sitemap.xml`.

### 3. Monthly scorecard
Friday 5/8 deliverable per the Project-Update timeline. One-page report: # leads, # discovery calls, # site visits, # contracts, revenue. Pull automatically from GHL — could be a Google Sheet via GHL Webhooks or a GHL dashboard. Custom fields are now in place to support this.

### 4. Nurture sequence
8–12 week email/SMS sequence for modular leads. Deferred per Spencer until 30 days of real lead flow so messaging matches actual buyer behavior.

### 5. Real photos (waiting on Mike)
- Set-day crane photos (the moment modules drop)
- Finished exterior shots of completed builds
- Screenshots of 3 Facebook posts for social proof: Joanne Keogh Humarock + 2 Marshfield spec homes
Drop into `/content/`, update `src` in `index.html` social-proof section + `gallery/index.html`.

### 6. HTTPS enforcement
Waiting on GitHub SSL provisioning. Check status: `gh api repos/morejobcalls/bmb-website/pages | jq '.protected_domain_state, .https_enforced'`. When `protected_domain_state` = `"verified"`, run: `gh api --method PUT repos/morejobcalls/bmb-website/pages --field https_enforced=true`.

### 7. bivianocontracting.com updates
Project scope calls for adding drone video + calculator to the existing GC site. Separate project — don't touch until modular site is fully producing. Low priority.

---

## Recent commits (2026-05-05 → 2026-05-06)
```
7aec6e8  Wire bmb_source on every form + send calculator dollar fields to GHL
e0e75d5  Auto-advance calculator after town pick (except "Other")
b6fdff1  Site fixes: $250 anchor, mobile menu, estimator CTA, timeline, town step
fa4ee08  Add "25% Less" to hero H1; remove rent guarantee from context
c68aef1  Add GA4 (G-GW5Z0VVDEC) tracking to all pages
88e18d3  Remove all Planning Board references across site
```

---

## How to Pick Up With Claude

### Resuming where we left off (2026-05-06 EOD)
```
I'm picking up the BMB project from where we left off on May 6.
Working directory: /Volumes/T7/SPG/3. FULFILLMENT/Clients/BIVIANO/07 - Websites/Modular
Repo: morejobcalls/bmb-website, branch main, deploys via GitHub Actions on push.
Read HANDOFF.md — focus on "Pick up here next session" at the top.

Status updates:
- Pipeline created in GHL UI? [yes/no — if yes, paste pipeline ID from URL]
- GA4 Realtime confirmed firing? [yes/no]
- Real photos from Mike? [yes/no — list files if yes]

Today's task: [describe what you want]
```

### Building the monthly scorecard (Fri 5/8 task)
```
I'm continuing the BMB project. Read HANDOFF.md.
Build the monthly scorecard for Mike — one-page report with:
- # leads (broken down by bmb_source)
- # discovery calls booked
- # site visits done
- # contracts signed
- revenue (from contact.bmb_estimate_total field on closed contacts)
Should pull from GHL automatically. Recommend: GHL custom dashboard if possible, or Google Sheet w/ Apps Script + GHL API as fallback.
```

---

## GA4 troubleshooting (open as of 2026-05-05 EOD — may be resolved)

GA4 snippet is verifiably in the live HTML. If Realtime is empty:

1. **Hard-refresh** https://www.bivianomodularbuilders.com (Cmd+Shift+R) — CDN may have cached pre-GA4 HTML.
2. **DevTools → Network → filter `google`** — should see `gtag/js?id=G-GW5Z0VVDEC` (loader) and `g/collect?...` (the hit). If neither fires → ad blocker (uBlock, Brave Shields, Privacy Badger, Ghostery).
3. **Try incognito** with all extensions off, OR test from phone on cellular.
4. **Realtime can lag 30–90 seconds** on the very first hit ever.
5. **Verify property selection** — top-left dropdown must say "Biviano Modular Builders," not "Biviano Contracting."

If still empty after all five:
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
