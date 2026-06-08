# Security configuration — Horwich Sewing SK website

This is a static HTML site (no database, no server-side code), so the attack surface is small.
The hardening below closes the common gaps and signals diligence to security-conscious buyers
(useful for MOD/defence supplier assurance).

## What's implemented

**Response headers** — full set provided for every common host:
- `_headers` — Netlify / Cloudflare Pages
- `.htaccess` — Apache
- `nginx-security.conf` — nginx (include in your `server {}` block)

Headers set: `Content-Security-Policy`, `Strict-Transport-Security` (HSTS), `X-Frame-Options: DENY`
+ CSP `frame-ancestors 'none'` (clickjacking), `X-Content-Type-Options: nosniff`,
`Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy` (locks camera/mic/geo/etc.),
`Cross-Origin-Opener-Policy: same-origin`. Apache/nginx also force HTTP→HTTPS.

**In-page fallback** — every page carries a `Content-Security-Policy` and `referrer` `<meta>` tag, so the
core CSP applies even on hosts that can't set custom headers (e.g. GitHub Pages, used for the preview).

**Contact form** — a hidden honeypot field (`name="website"`) is in place to catch bots.

**security.txt** — `/.well-known/security.txt` (RFC 9116) gives researchers a disclosure contact.

## The CSP, in plain terms
Only first-party content loads, plus Google Fonts. No third-party scripts, frames or objects.
`'unsafe-inline'` is currently allowed for scripts and styles because the page uses inline
styles and small inline scripts (menu toggle, certificate lightbox, draft no-index switch).

## Deploy checklist
1. Enable HTTPS/SSL on the host; confirm HTTP redirects to HTTPS.
2. Confirm headers are live — test at https://securityheaders.com and Mozilla Observatory (aim A/A+).
3. HSTS `preload` is included; only submit the domain to hstspreload.org once you're sure **all**
   subdomains will be HTTPS-only (it's hard to undo).
4. GitHub Pages can't set headers — it relies on the `<meta>` CSP only. A host like Netlify or
   Cloudflare Pages (free) applies the full `_headers` set; recommended for production.

## When you wire the contact form to a backend
- Add the form endpoint to the CSP: `form-action` and `connect-src` (e.g. Formspree/Netlify Forms).
- Reject any submission where the honeypot `website` field is non-empty.
- Add rate-limiting and a CAPTCHA (e.g. hCaptcha/Turnstile) to stop spam.
- Validate and sanitise all fields server-side; never email raw input as HTML.

## Optional, stronger follow-ups
- **Self-host the fonts** (Cormorant Garamond, Cinzel, Inter): removes the Google Fonts requests
  (privacy/GDPR + speed) and lets us drop `fonts.googleapis.com`/`fonts.gstatic.com` from the CSP.
- **Externalise inline JS** and remove inline `on*` handlers: lets us drop `'unsafe-inline'` from
  `script-src` for a much stronger CSP.
- Add **Subresource Integrity** (SRI) hashes to any future CDN `<script>`/`<link>`.
- Review and refresh `/.well-known/security.txt` before its `Expires` date (currently 2027-06-08);
  consider a dedicated `security@horwichsewingsk.com` mailbox.
