# SAMA Website — Historical Context for New Agents

This file exists to onboard a new Claude agent (or developer) who is picking up work on this project. It captures the full history, decisions, and workflow established in prior sessions so you don't need to re-derive everything from scratch.

---

## What This Project Is

A single-page static website for **SAMA (Service & Mercy Alliance)**, a Muslim community organization based on Grand Island, NY. The site serves as a public-facing home for the musallah (prayer space), listing prayer times, programs, announcements, and community links.

**Live site:** Deployed via Vercel (check vercel.com dashboard under project `masjid-site`)
**GitHub repo:** https://github.com/tahirinavid-hue/masjid-site

---

## Tech Stack

- **Single file:** The entire site is `index.html` — no framework, no build step, no dependencies
- **Styling:** Tailwind CSS (loaded via CDN)
- **Hosting:** Vercel (auto-deploys from GitHub)
- **Version control:** Git → GitHub

---

## Deployment Workflow

```
Edit index.html locally
        ↓
git commit + push to master
        ↓
GitHub (master + main both updated)
        ↓
Vercel auto-deploys to production
```

**Critical rule:** Always push to **both** `master` and `main` on every change:
```bash
git push origin master
git push origin master:main
```
This is because Vercel watches `master` (set as production branch) but `main` also needs to stay in sync. This was set up intentionally — do not push to one without the other.

---

## Branch Strategy

| Branch | Purpose |
|--------|---------|
| `master` | Production — always deployable, always matches live site |
| `main` | Mirror of master — kept in sync manually on every push |
| `staging` | Preview/test branch — Vercel auto-generates a preview URL for it |

**To reset staging back to master** (drop all staging changes):
```bash
git checkout staging
git reset --hard master
git push origin staging --force
git checkout master
```

---

## GitHub Authentication

The repo owner is `tahirinavid-hue`. Pushes require a Personal Access Token (PAT). The PAT has been used in this session but **should be regenerated** as it was shared in plain text in the chat. Go to:
> github.com → Settings → Developer settings → Personal access tokens

When pushing with a PAT, set the remote temporarily:
```bash
git remote set-url origin https://<username>:<PAT>@github.com/tahirinavid-hue/masjid-site.git
git push ...
git remote set-url origin https://github.com/tahirinavid-hue/masjid-site.git  # remove token after
```

---

## Git Identity (already configured globally)

```
user.name  = Navid Tahiri
user.email = tahirinavid@gmail.com
```

---

## Prayer Times — How They Work

Prayer times are **dynamic** — the site automatically shows today's correct times.

- **Source:** `Grandyle_Village_New_York_USA_Prayer_Times (2).xlsx` (in the project folder) — full year schedule from salahtimes.com
- **Implementation:** The full year of data is embedded directly in `index.html` as a JavaScript object (`PT`). A small inline script runs on page load, reads today's date, and populates the prayer table.
- **Element IDs used:** `pt-fajr-adhan`, `pt-fajr-iqamah`, `pt-dhuhr-adhan`, etc. (pattern: `pt-{prayer}-{adhan|iqamah}`)
- **Footnote ID:** `pt-footnote` — auto-updates to show "Times shown for [Month] [Day], 2026 · Source: salahtimes.com"
- **Jumu'ah times** are hardcoded separately (not from the spreadsheet) — Khutbah 1:45 PM, Salah 2:15 PM
- To update prayer times for a new year: re-run the Node.js parsing script against the new Excel file and re-embed the data

---

## Site Structure (key sections in order)

1. **Navbar** — logo, nav links, mobile menu
2. **Hero** — SAMA heading, tagline, CTA buttons (Join Community, Donate)
3. **Prayer Times** — dynamic daily table + Jumu'ah card
4. **Announcements** — card grid (currently: Neighbors Foundation news, Eid celebration, General Assembly meeting Apr 12)
5. **Programs & Services** — Weekend Islamic School (RSVP link), Youth Mentorship (RSVP link Apr 4)
6. **Join the Community** — WhatsApp, Facebook, Instagram links
7. **Donate** — donation CTA
8. **Map** — embedded Google Map
9. **Footer** — About blurb, Accessing the Musallah section, Contact (address, phone, email), Quick Links

---

## Key Content Details

- **Address:** 1822 Huth Rd, Grand Island, NY 14072
- **Phone:** (716) 830-6559
- **Email:** Admin@samagi.org
- **WhatsApp group:** https://chat.whatsapp.com/IYvETlqZOOv6JA7ERe3qq5
- **Facebook:** https://www.facebook.com/share/g/1CNTQbqgkn/
- **Instagram:** @SAMA.GRANDISLAND — https://www.instagram.com/sama.grandisland
- **Musallah access:** Smart lock — visitors need entry code, call/text (716) 830-6559

---

## Design Decisions & Preferences

- **SAMA header text:** Deep green `#1a5c2e`, non-italic, large (`text-7xl/8xl`)
- **Color palette:** Moss green (`#4a5e4a`), gold (`#b89a5a`), cream background
- **Font:** Display font for headings, body font for text
- Keep the site as a **single `index.html` file** — no splitting into components, no build tools
- The owner prefers **direct, quick edits** pushed immediately to production

---

## Workflow Notes for New Agents

- After every change: commit to `master`, push to both `master` and `main`
- Staging is available for testing before going to prod — reset it from master when starting a new staging experiment
- The PAT needs to be embedded in the remote URL temporarily for each push (see authentication section above)
- Node.js + the `xlsx` package are installed locally and were used to parse the prayer times spreadsheet
- There is no CI, no tests, no linting — just edit and push
