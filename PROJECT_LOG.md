# PROJECT_LOG.md — decentralizedpm.com

Internal reference document covering project context, design decisions, and work history. See `README.md` for public-facing info.

---

## 1. Project Overview

**What it is:** Marketing landing page for Decentralized Property Management (DPM) — full-service residential and commercial property management for owners and tenants in Houston, TX.

| Field | Value |
|---|---|
| **Tagline** | Modern Property Management, Decentralized. |
| **Contact** | info@decentralizedpm.com · 713-299-2339 |
| **Domain** | decentralizedpm.com |
| **GitHub** | github.com/jescobarDMA/decentralizedpm.com (public) |
| **Hosting** | Hostinger (Git-connected, auto-deploy from `main`) |
| **Positioning** | Engineering-led property management. Transparent systems for owners; responsive experience for tenants. |
| **Audience** | Two audiences served side-by-side: property owners/investors and tenants. |

---

## 2. Tech Stack

- **Plain static site** — hand-written HTML + CSS + vanilla JS. No framework, no build step, no dependencies.
- **Single page** — sections: Hero, Problem, Services (two-column split), Process, About, Proof, CTA Split, Footer.
- **Brand tokens** defined as CSS custom properties in `styles.css` `:root`:
  - Navy `#2C3E50` · Slate `#506D8A` · Sky `#8FAFC6` · Highlight `#D4956B` · Neutral `#F5F7FA`
- **Fonts** self-hosted (TTF in `assets/fonts/`):
  - Instrument Sans — headings (same as DMA sister site)
  - Lora — body text (serif, distinguishes DPM from DMA)

---

## 3. Architecture Notes

| File | Purpose |
|---|---|
| `index.html` | Full single-page markup. Inline `<script>` at bottom handles: mobile nav toggle, scroll-state nav transition. |
| `styles.css` | All styles. CSS custom properties at `:root`. BEM-ish naming (`.hero__pre`, `.tier-card--highlight`, etc.). |
| `assets/fonts/` | 8 TTF files — Instrument Sans (4 weights) + Lora (4 weights) |
| `assets/images/` | `logo.png` (full wordmark, nav + footer), `icon.png` (keyhole icon, favicon) |
| `assets/video/` | `.gitkeep` placeholder — real hero MP4 to be dropped in here when available |
| `.htaccess` | Apache config: disables HTML cache, aggressive cache for static assets, sets MP4 MIME type |
| `.gitignore` | `.DS_Store`, `*.log`, `.env`, `.vscode/`, `.idea/` |

---

## 4. Key Design Decisions

### Hero — Gradient Fallback (no video yet)
- **Blueprint grid overlay:** CSS `background-image` crosshatch pattern at `rgba(143,175,198,0.07)` on a 56px grid. Evokes property floor plans; purely CSS, no images, zero overhead.
- **Gradient:** `radial-gradient(ellipse 80% 70% at 75% 55%, rgba(80,109,138,0.5) 0%, transparent 65%)` + `linear-gradient(145deg, #1a2535 0%, #2C3E50 55%)`. Creates a "light source from the right" effect — architectural feel.
- **When video arrives:** Insert `<video class="hero__video" autoplay muted loop playsinline preload="auto" aria-hidden="true">` in the marked TODO block in `index.html`. No CSS changes needed — `.hero__video` rules are already defined. Remove the `::before` blueprint grid or keep it as a subtle overlay.
- **Full-bleed video method (pre-wired):** Same `transform: translate(-50%, -50%)` + `min-width/min-height: 100%` approach used on the DMA sister site (documented there as more reliable on Hostinger/LiteSpeed than `object-fit: cover`).

### Navigation
- `position: fixed` so the nav overlays the hero.
- **Transparent state:** Uses a `linear-gradient(to bottom, rgba(26,37,53,0.55) 0%, transparent)` background — this anchors the logo (which has a white background) on the dark hero without needing `filter: brightness(0) invert(1)`.
- **Why not `filter: brightness(0) invert(1)`?** DMA's logo had a transparent background so the filter inverted it cleanly to white. DPM's logo has a white background — applying that filter would render the entire image as a white rectangle. The dark gradient approach solves this without touching the logo asset.
- **Solid state (post-scroll):** `background: white`, standard border and shadow. Logo shown at natural colors (slate-on-white = correct contrast).

### Brand Color Strategy
- `--highlight: #D4956B` (warm tan) used ONLY for: stat card numbers, "Most Popular" / "Available Now" badge backgrounds, and `border-top` accent on highlighted tier cards. Never used as a CTA button color. This keeps it as editorial punctuation against the blue palette rather than a competing primary.
- `--navy: #2C3E50` — dark sections (hero, process, proof card headers, CTA panels, footer).
- `--slate: #506D8A` — primary CTA buttons, section labels, tier card borders, icon backgrounds.
- `--sky: #8FAFC6` — hover states, nav links over dark hero, process connector line, footer column titles.

### Typography
- **InstrumentSans** (headings): same as DMA — geometric sans, strong hierarchy.
- **Lora** (body): serif, replaces Work Sans from DMA. Line-height bumped to `1.85` (serif needs more leading than sans). Hero sub-headline and founder bio set in Lora italic — adds editorial warmth.
- Body font strategy: the Lora / InstrumentSans pairing signals professional real estate without feeling corporate-generic. Lora at 0.88–1rem / 1.65–1.85 leading reads cleanly at all section widths.

### Services Section — Two-Column Split
- Left column: "For Property Owners" — three owner-tier cards (Full-Service, Leasing Only, Portfolio Partner).
- Right column: "For Tenants" — three tenant-tier cards (Browse Listings, Resident Portal, Maintenance).
- Below both columns: shared six-card service grid (Tenant Screening, Lease Admin, Maintenance Coordination, Owner Reporting, Rent Collection, Inspections & Compliance).
- At 1024px: columns stack vertically (Owner on top, Tenant below).
- If stacked layout feels too tall in production, consider a tab switcher: two `<button>` tabs toggle `.services-col` visibility via `aria-selected` + a two-line JS toggle in the existing inline script.

### CTA Split Panel
- Full-width two-panel layout: left panel navy (owners), right panel slate (tenants). Both `href="#"` with `<!-- TODO -->` comments for easy link swap without a redeploy.
- A dark `#1a2535` strip below both panels shows the shared contact info (phone + email). Grid uses `grid-column: 1 / -1` to span the strip across both columns.

### Placeholder Links
All booking/listing/portal links use `href="#"` with an HTML comment marking the TODO. Before launch: swap in owner consultation link, tenant listings link, resident portal link, and maintenance request link.

---

## 5. Work Log (Chronological)

### Git Commits
| Commit | Date | Description |
|---|---|---|
| Initial | 2026-04-16 | Initial commit — DPM marketing site, full 8-section layout, DPM brand tokens, Lora body font, blueprint-grid hero, two-column services split, CTA split panel |
| 16APR26_HeroVideo | 2026-04-16 | Wire hero video — rename to `dpm-hero.mp4`, drop `<video>` element, remove blueprint-grid placeholder, set 0.5× playback speed |

---

## 6. Deployment Setup

**GitHub** → `github.com/jescobarDMA/decentralizedpm.com`
- Branch: `main`
- Visibility: Public

**Hostinger**
- Connect to GitHub repo via Hostinger's Git integration:
  - hPanel → Hosting → Git → Add Repository → paste GitHub URL → Deploy
- Redeploy after push: hPanel → Hosting → Git → Deploy / Pull
- Cache purge after every deployment: hPanel → Cache → LiteSpeed Cache → Purge All
  - **Important:** LiteSpeed caches aggressively — always purge cache + hard-refresh (`Cmd+Shift+R`) after pushing.
- DNS: managed in Hostinger DNS panel for `decentralizedpm.com`

**Commit message convention (matching DMA):**
```
git add <files>
git commit -m "16APR26_Description"
git push
```

---

## 7. Known Issues / Open Items

- ~~**Hero video pending**~~ — resolved. `assets/video/dpm-hero.mp4` live. Plays at 0.5× speed, blueprint-grid placeholder removed.
- **Placeholder links:** 4 `href="#"` links need real destinations before launch:
  - Owner consultation / calendar booking link (in Process section CTA + owner CTA panel)
  - Tenant listings link (in "Browse Listings" tier card + tenant CTA panel)
  - Resident portal link (in "Resident Portal" tier card)
  - Maintenance request link (in "Maintenance & Repairs" tier card)
- **Email alias:** `info@decentralizedpm.com` must be provisioned in Hostinger before launch. This is the hard blocker for going live.

**Future considerations:**
- Compress video when available: produce WebM + smaller MP4 variant, add `poster` frame
- Set up Netlify/Vercel deploy preview for fast iteration without touching production
- Custom 404 page for Hostinger
- If services section stacked layout is too tall: add tab switcher JS

---

## 8. Quick Reference

```bash
# Local preview
open /Users/joseescobar/Desktop/JESCOBAR/Programming/VS_Code/decentralizedpm.com/index.html

# Push a change
cd /Users/joseescobar/Desktop/JESCOBAR/Programming/VS_Code/decentralizedpm.com
git add <files>
git commit -m "DDMMMYY_Description"
git push
# Then: Hostinger → Git → Deploy + Cache → Purge All

# DPM brand source of truth (canonical colors/fonts)
/Users/joseescobar/Desktop/JESCOBAR/Programming/VS_Code/agenticDPM_ExVA/brands/decentralized-property-management/colors.json
/Users/joseescobar/Desktop/JESCOBAR/Programming/VS_Code/agenticDPM_ExVA/brands/decentralized-property-management/brand-style-guide.pdf

# Sister site reference (DMA) — same architecture
/Users/joseescobar/Desktop/JESCOBAR/Programming/VS_Code/decentralizedma.com/
```
