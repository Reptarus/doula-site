# [Name] Doula — Business Website

A minimal, fast, beautiful static website for a doula business. Built with [Astro](https://astro.build), deployed to [Cloudflare Pages](https://pages.cloudflare.com) for free.

**Estimated annual cost: ~$10-15/year** (just the domain name)

---

## Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) 18+ installed
- A [GitHub](https://github.com) account
- A [Cloudflare](https://cloudflare.com) account (free)

### Local Development

```bash
# Install dependencies
npm install

# Start dev server (http://localhost:4321)
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## Project Structure

```
doula-site/
├── public/
│   └── favicon.svg              # Site favicon (replace with real one)
├── src/
│   ├── components/
│   │   ├── Header.astro         # Site header + mobile nav
│   │   └── Footer.astro         # Site footer
│   ├── layouts/
│   │   └── BaseLayout.astro     # Page wrapper (head, fonts, meta)
│   ├── pages/
│   │   ├── index.astro          # Home page
│   │   ├── about.astro          # About page
│   │   ├── services.astro       # Services & pricing
│   │   ├── book.astro           # Booking calendar (Cal.com embed)
│   │   ├── pay.astro            # Payment portal (Stripe links)
│   │   └── contact.astro        # Contact form
│   └── styles/
│       └── global.css           # Design tokens + global styles
├── astro.config.mjs             # Astro configuration
├── package.json
└── README.md
```

---

## Deployment to Cloudflare Pages

### First-Time Setup

1. Push this repo to GitHub
2. Go to [Cloudflare Dashboard](https://dash.cloudflare.com) → **Pages** → **Create a project**
3. Select **Connect to Git** and choose your repo
4. Configure build settings:
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
   - **Node.js version:** `18` (set in Environment Variables as `NODE_VERSION=18`)
5. Click **Save and Deploy**

Your site will be live at `your-project.pages.dev` within ~30 seconds.

### Connecting Your Custom Domain

1. In Cloudflare Pages, go to your project → **Custom domains**
2. Click **Set up a custom domain**
3. Enter your domain (e.g., `nameDoula.com`)
4. If your domain is registered on Cloudflare, DNS is automatic
5. If registered elsewhere, point your nameservers to Cloudflare

### Automatic Deployments

Every time you push to `main`, Cloudflare automatically rebuilds and deploys. No manual steps needed.

---

## Content Editing Guide

### Changing Text & Business Info

All content is in the `.astro` files under `src/pages/`. Open any file and look for:

- **`[Name]`** — Replace with business name everywhere it appears
- **`$X,XXX`** — Replace with actual pricing
- **`(360) XXX-XXXX`** — Replace with phone number
- **`hello@yourdomain.com`** — Replace with business email
- **`@yourbusiness`** — Replace with Instagram handle

You'll also want to update `src/components/Header.astro` and `src/components/Footer.astro` since they contain the business name too.

**Quick find-and-replace across all files:**
```bash
# From the project root, find all instances of placeholder text
grep -r "\[Name\]" src/
grep -r "XXX" src/
grep -r "yourdomain" src/
grep -r "yourbusiness" src/
```

### Adding Photos

1. Put image files in the `public/images/` directory (create it if needed)
2. Reference them in pages as `/images/your-photo.jpg`

For the hero image on the home page, open `src/pages/index.astro` and replace the placeholder block:
```html
<!-- Replace the placeholder div with: -->
<img src="/images/hero.jpg" alt="Description of photo" style="width: 100%; height: 100%; object-fit: cover;" />
```

**Recommended image sizes:**
- Hero image: 1200x800px minimum
- Portrait/about photo: 800x1000px minimum
- Format: WebP preferred (smaller files), JPG as fallback

### Changing Colors

All colors are defined as CSS variables in `src/styles/global.css`:

```css
:root {
  --color-cream: #F7F3EE;        /* Page background */
  --color-sage: #8B9E7E;          /* Primary accent */
  --color-sage-dark: #5C7050;     /* Darker accent (text, hover) */
  --color-terracotta: #C48B6C;    /* Secondary accent (eyebrow text) */
  --color-charcoal: #2D2D2D;      /* Main text color */
}
```

Change these values and the entire site updates automatically.

---

## Third-Party Service Setup

### Cal.com (Appointment Scheduling) — FREE

1. Sign up at [cal.com](https://cal.com)
2. Create a new **Event Type** (e.g., "Free Consultation — 30 min")
3. Go to **Settings → General** and connect your Google Calendar
4. Go to **Event Types → [Your Event] → Embed** and copy the embed code
5. Open `src/pages/book.astro` and replace the `<div class="cal-placeholder">` block with the Cal.com embed code

**What this gives your wife:** Clients pick a time from available slots, fill in their info, and it automatically appears on her Google Calendar with confirmation emails sent to both parties.

### Stripe (Payments) — FREE to set up, 2.9% + $0.30 per transaction

1. Sign up at [stripe.com](https://stripe.com)
2. Complete account verification
3. Go to **Payment Links** in the Stripe dashboard
4. Create payment links for:
   - **Invoice payment** (adjustable amount)
   - **Payment plan installment** (fixed amounts)
   - **Gift certificate** (fixed amount)
5. Open `src/pages/pay.astro` and replace `href="#"` with your Stripe payment link URLs

**What this gives your wife:** Clients click a link, enter their card on Stripe's hosted page, and money goes to your bank account. Stripe handles receipts, refunds, and PCI compliance.

### Contact Form (Formspree) — FREE for 50 submissions/month

1. Sign up at [formspree.io](https://formspree.io)
2. Create a new form
3. Copy your form endpoint (looks like `https://formspree.io/f/xyzabc`)
4. Open `src/pages/contact.astro` and replace `YOUR_FORM_ID` in the form action

**What this gives your wife:** Form submissions arrive as emails in her inbox. No server code needed.

### Email Forwarding (Cloudflare) — FREE

1. In Cloudflare Dashboard, go to your domain → **Email** → **Email Routing**
2. Click **Create address**
3. Set `hello@yourdomain.com` → forwards to her personal Gmail
4. Verify the destination email

**What this gives your wife:** Professional email address that forwards to her existing Gmail. She replies from Gmail and clients see the business domain.

---

## Maintenance

### Updating the Site

```bash
# Make your changes to any files
# Then:
git add .
git commit -m "describe your change"
git push
# Cloudflare auto-deploys in ~30 seconds
```

### Updating Dependencies

```bash
# Check for outdated packages
npm outdated

# Update Astro
npm update astro
```

### Adding a New Page

1. Create a new file in `src/pages/` (e.g., `testimonials.astro`)
2. Use the same pattern as existing pages:
```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout title="Testimonials | [Name] Doula">
  <!-- Your content here -->
</BaseLayout>
```
3. Add a link in `src/components/Header.astro` and `Footer.astro`

---

## Estimated Costs

| Service           | Cost              | What It Does                        |
|--------------------|-------------------|-------------------------------------|
| Cloudflare Pages   | Free              | Hosting, CDN, SSL, auto-deploy      |
| Domain name        | ~$10-15/year      | Your custom .com address             |
| Cal.com            | Free              | Appointment scheduling               |
| Stripe             | 2.9% + $0.30/txn  | Payment processing                   |
| Formspree          | Free (50/month)   | Contact form submissions             |
| Cloudflare Email   | Free              | Business email forwarding            |
| **Total fixed**    | **~$10-15/year**  |                                      |

---

## Tech Stack

- **[Astro](https://astro.build)** — Static site generator, zero JS shipped to client
- **[Cloudflare Pages](https://pages.cloudflare.com)** — Global CDN hosting with auto-deploy
- **[Cal.com](https://cal.com)** — Open-source appointment scheduling
- **[Stripe](https://stripe.com)** — Payment processing
- **[Formspree](https://formspree.io)** — Form-to-email service

---

## Help / Troubleshooting

**Build fails on Cloudflare?**
- Make sure `NODE_VERSION` environment variable is set to `18` in Cloudflare Pages settings
- Check build logs in Cloudflare dashboard for specific errors

**Cal.com widget not loading?**
- Verify the embed script is placed correctly (not inside a comment block)
- Check browser console for errors
- Make sure your Cal.com event type is set to "public"

**Contact form not sending?**
- Verify Formspree form ID is correct
- Check Formspree dashboard for submissions
- Make sure you confirmed the destination email in Formspree

**Site not updating after push?**
- Check Cloudflare Pages dashboard for build status
- Cloudflare caches aggressively — try clearing browser cache or waiting a few minutes
