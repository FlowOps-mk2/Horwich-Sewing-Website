# Horwich Sewing SK — Website

Static marketing site for Horwich Sewing SK — European contract manufacturer of workwear, protective and technical garments (since 1988).

## Structure

Plain HTML/CSS, no build step. 12 pages sharing `styles.css`.

- `index.html` — home
- `about.html` — our story
- `what-we-do.html` — process & capabilities
- `products.html` — products & industries
- `defence.html` — defence & military
- `compliance.html` — compliance & accreditations
- `certifications.html` — ISO certificate detail
- `social-value.html`, `responsible-sourcing.html`, `privacy.html` — governance/policies
- `crafter.html` — Crafter sub-brand
- `contact.html` — enquiry / request a quote
- `styles.css` — shared stylesheet (brand: deep green / gold / cream)
- `assets/` — logo, favicon, OG image, Defence Capability Statement PDF
- `robots.txt`, `sitemap.xml`, `llms.txt` — SEO / AI-SEO

## View locally

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy

Any static host works. Two easy options:

- **GitHub Pages** — Settings → Pages → deploy from `main` / root.
- **Netlify / Cloudflare Pages** — point at this repo; no build command, publish directory `/`.

Set the production domain, then update the absolute URLs in the `<link rel="canonical">` tags, `sitemap.xml`, `robots.txt` and the JSON-LD if the final URL structure differs (e.g. extensionless URLs).

## Status — prototype

This is a working prototype. Before go-live:

- Remove the "DRAFT PROTOTYPE" notice banner from each page.
- Replace placeholder image boxes with real photography.
- Wire the contact form to an enquiry inbox.
- Have the directors review/approve the policy pages (privacy, responsible sourcing) and fill the `[bracketed]` placeholders.
- Add company registration / VAT numbers to the footer.

© 1988–2026 Horwich Sewing SK. ISO 9001:2015 & ISO 14001:2015 certified by SGS.
