# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website for Carley, a personal trainer based in Milton Keynes and Hereford (UK). The site handles marketing, consultation booking, a blog, and a client portal with progress dashboard.

## No Build System

This is plain HTML/CSS/JS — no npm, no bundler, no compilation step. To preview locally, use any static file server:

```bash
npx serve .
# or
python -m http.server 8080
```

## Architecture

All pages share a single `styles.css`. Each page embeds its own `<script type="module">` or links to `script.js` for Firebase logic.

| File | Purpose |
|---|---|
| `index.html` | Main landing page — hero, about, services/pricing, booking calendar, social, testimonials, contact form |
| `blog.html` | Blog posts |
| `login.html` | Client portal login (Firebase Auth) |
| `signup.html` | Client account creation (Firebase Auth + Firestore) |
| `dashboard.html` | Client fitness dashboard — shows coach notes, workout plan, progress (Chart.js, GLightbox) |
| `admin.html` | Coach-only page to create client accounts and set their data in Firestore |
| `script.js` | Main JS for `index.html`: booking calendar, contact form, slideshow, mobile nav |

## Firebase Backend

Firebase is loaded via CDN (`https://www.gstatic.com/firebasejs/9.6.10/`). The config (API key etc.) is embedded directly in `script.js` and in inline `<script type="module">` blocks in the HTML files. Project: `carleynfitnesscalendar`.

Firestore collections:
- `bookings` — consultation booking requests (written from `index.html` calendar)
- `contactMessages` — contact form submissions
- `clients/{uid}` — client profile data (goal, weight, workout plan, coach notes)

## Key Toggles & Patterns

**Fully booked mode** (`script.js:10`): Set `const FULLY_BOOKED = true` to disable all calendar date selection and show a "Fully booked" badge. Currently set per-location (the MK calendar is in `script.js`; Hereford status is shown as a static notice in the HTML).

**Disabled calendar days** (`script.js:113-119`): Days 2, 3, 4 (Tue/Wed/Thu) are blocked. Edit this array to change available days.

**AOS animations**: All sections use `data-aos="fade-up"` attributes. AOS is initialised once at the bottom of `index.html` and `blog.html`.

## External Libraries (all via CDN)

- **AOS 2.3.4** — scroll animations
- **FullCalendar 5.11.2** (CSS) + **3.0.0** (JS) — included but the booking calendar is custom-built with plain DOM, not FullCalendar
- **Chart.js 4.4.1** + **chartjs-adapter-date-fns** — dashboard graphs (currently commented out, kept for future use)
- **GLightbox 3.2.0** — image lightbox on the dashboard
- **Google Analytics** — gtag ID `G-VJSQ9P5NLW` present on all pages
