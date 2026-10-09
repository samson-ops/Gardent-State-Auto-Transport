# Garden State Auto Transport — website

Deploy-ready static site with the Ship Guy pricing widget wired to the
**Garden State Auto Transport** lead source (key `6ac7f4c5b54e0`).

## Files
- `index.html` — the full one-page site (hero + instant quote widget, reviews, how it works, services, vehicles, coverage, why us, about, FAQ, contact, footer)
- `assets/logo.png`, `assets/favicon.png`
- `functions/api/quote/[[path]].js` — Cloudflare Pages Function that proxies `/api/quote/*` to BeRocker with the lead key (key never appears in the page)

## Deploy (Cloudflare Pages)
1. Create a Pages project → upload this folder (or connect a repo with it at the root).
2. Optional but recommended: Settings → Environment variables → `BEROCKER_LEAD_KEY = 6ac7f4c5b54e0` (overrides the default in the function).
3. Point `gardenstateautotransport.com` / `www` at the project.
4. Google Maps: add `gardenstateautotransport.com` to the referrer restrictions of the key in `PQ_CONFIG` (ZIP suggestions only — the tool works without it).

## Before going live — edit these values
- Phone numbers are set per office; the home page uses the Toms River number.
- Footer email `info@gardenstateautotransport.com` — change if different.
- Testimonials are placeholder copy — swap for real reviews.

## Test after deploy
Submit a quote on the live site; the lead should appear in BeRocker under **Garden State Auto Transport**. If it lands under Ship Guy, the key is wrong.

## City pages
11 pages in `locations/`: toms-river, asbury-park, paterson, red-bank, princeton, bayonne, edison, woodbridge, cherry-hill, brick, middletown
— each ~1,500 words of local copy, own phone/address/map, quote widget (site code `GSAT-<CITY>` so you can see which office a lead came from), LocalBusiness + FAQ + Breadcrumb schema, canonical/OG/geo meta. `sitemap.xml` and `robots.txt` are included.
Cloudflare Pages serves `locations/red-bank-nj.html` at `/locations/red-bank-nj` automatically; the canonicals already use that form.
Main site number is (732) 724-0725 on all non-location pages; each office page uses its own local number. The site is positioned as a lead-generation/referral service: no MC/USDOT numbers, no broker claims, and a disclaimer in the footer of every page (`#disclaimer`).

## Full page list (31 URLs, all in sitemap.xml)
- `index.html` — home
- `how-it-works.html`, `services.html`, `car-shipping-cost.html`, `preparation-checklist.html`, `cross-country-shipping.html`, `faq.html`, `reviews.html`, `locations.html`, `contact.html`
- Service pages: `open-car-transport`, `enclosed-auto-transport`, `door-to-door-shipping`, `state-to-state-shipping`, `expedited-shipping`, `snowbird-car-shipping`, `military-car-shipping`, `auction-car-transport`
- `locations/<city>-nj.html` × 11
- `privacy.html`, `terms.html` (lead-gen terms), `404.html` (Cloudflare Pages serves it automatically for missing routes)

Every page has the quote widget (except locations, privacy, terms, 404), unique title/description/canonical/OG, and JSON-LD (Service + FAQ on service pages; FAQPage on FAQ/cost/prep/cross-country; Organization on locations; LocalBusiness on city pages).
Regenerate everything with `python3 gen_cities.py && python3 gen_pages.py` (scripts not included in this zip — ask and I'll add them).
