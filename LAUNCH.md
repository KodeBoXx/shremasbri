# Shrem Asbri BG Limited — Website Launch Checklist

## Current State
- **V1** (frozen, dummy logo): https://master.shremasbri.pages.dev
- **V2** (dual co-branded logos): https://shremasbri-v2.pages.dev
- **Repo**: https://github.com/KodeBoXx/shremasbri.git
- **Cloudflare account**: subscription.yralmedia@gmail.com

---

## Pending from Client
- [ ] Director photos — Hitesh Chhatwal, Piyush Jain, Bhuvneshwari Ramadoss (G.P. Subash done)
- [ ] Final SABL logo artwork (V1 uses dummy 3-circle, V2 uses Shrem + Asbri logos)
- [ ] Confirm V1 or V2 as the go-live version
- [ ] Domain name (GoDaddy) — need domain + DNS access
- [ ] Email address for contact form (e.g. info@shremasbri.com)

---

## Go-Live Tasks

### 1. Domain Mapping (GoDaddy → Cloudflare Pages)
**What you need from client:** Domain name + GoDaddy DNS access

**Steps:**
1. Cloudflare Pages → project → **Custom Domains** → Add domain
2. Cloudflare gives a CNAME record, e.g.:
   ```
   shremasbri.com  →  shremasbri.pages.dev
   www             →  shremasbri.pages.dev
   ```
3. Client logs into GoDaddy → DNS Settings → add those two CNAME records
4. Done — SSL is automatic, propagates in 10–30 minutes, completely free

---

### 2. Contact Form (Web3Forms)
**Free tier:** 250 submissions/month — sufficient for this site
**What you need:** An email address to receive enquiries

**Steps:**
1. Sign up at https://web3forms.com with the client email
2. Get the **Access Key** from the dashboard
3. Set **allowed domain** to `shremasbri.com` in Web3Forms dashboard (prevents misuse of key)
4. In `index.html`, add inside `<form id="contact-form">`:
   ```html
   <input type="hidden" name="access_key" value="YOUR_ACCESS_KEY_HERE">
   <input type="hidden" name="subject" value="New enquiry — Shrem Asbri BG">
   <input type="hidden" name="from_name" value="SABL Website">
   ```
5. Change form tag to: `<form id="contact-form" action="https://api.web3forms.com/submit" method="POST">`
6. Remove the `e.preventDefault()` fake submit handler from the JS (or adapt it for real submission)

**Security note:** The access key is intentionally public-facing — Web3Forms keys are not secret. Domain restriction in the dashboard locks it to your site only.

---

### 3. Email Routing (Cloudflare — Free, No Limits)
Sets up `info@shremasbri.com` as a real inbox that forwards to any Gmail.

**Steps:**
1. Cloudflare Dashboard → **Email Routing** → Enable
2. Add route: `info@shremasbri.com` → forward to personal Gmail
3. Done — use this address as the Web3Forms destination

---

### 4. Security Headers
Add a `_headers` file to the project root. Cloudflare Pages reads it automatically.

**Create `_headers`:**
```
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  X-XSS-Protection: 1; mode=block
```

Then deploy — covers clickjacking, MIME sniffing, XSS in one shot. Free, no backend needed.

---

## What Is Already Secured (Free via Cloudflare)
- SSL/HTTPS — auto-managed, auto-renews
- DDoS protection — Cloudflare network
- CDN — served from edge nodes globally

## Not Applicable (Static Site — No Backend)
- SQL injection — no database
- Auth vulnerabilities — no login system
- Server-side exploits — no server

---

## Design Notes
- Design system: SABL — tokens in `colors_and_type.css`
- Font: DM Sans (Google Fonts)
- Hero image: `assets/hero-cbg-plant.jpg` (real CBG plant, Moldova)
- Director photo: G.P. Subash — `assets/director-subash.png`
- JV card logos: `assets/logo-shrem-actual.png`, `assets/logo-asbri-actual.png`
- V2 nav logos: both company logos with pipe divider, white-inverted for dark nav
- Mobile hero: dark navy-blue overlay applied only on `max-width: 768px`
