# Construction & Waterproofing Company Website

## Overview

A responsive, multi-page business website for a construction and waterproofing company based in Hyderabad, India. The site serves as the company's primary online presence — showcasing services, portfolio, team capabilities, and providing lead capture through contact forms and callback requests. Company name has been removed from all visible text; only the logo image is used for branding.

**Tech Stack:** React + Vite + TailwindCSS (frontend) · Express.js (backend) · Gmail API (email notifications)

**Live Pages:** Home · About · Services · Portfolio · Brochure · Contact

---

## Project Architecture

### Frontend (`client/`)

- **Framework:** React 18 with Vite build tooling
- **Routing:** `wouter` — lightweight client-side router
- **Styling:** TailwindCSS with custom CSS variables in `index.css`
- **UI Components:** shadcn/ui component library (`client/src/components/ui/`)
- **State Management:** TanStack React Query for server state; React `useState` for local form state
- **Icons:** `lucide-react` for UI icons; `react-icons/si` for brand logos (Instagram, LinkedIn, Facebook)
- **Form Handling:** `react-hook-form` with `zod` validation via `@hookform/resolvers/zod`
- **Logger:** `client/src/lib/logger.ts` — structured browser console logging with log levels (debug suppressed in production)

### Backend (`server/`)

- **Framework:** Express.js running on Node.js
- **Email:** Gmail API via Replit Google Mail connector — sends notification emails for inquiries and callback requests
- **Logger:** `server/logger.ts` — structured, timestamped server logging with configurable log levels via `LOG_LEVEL` env var
- **Static Serving:** In production, serves the Vite build from `server/public/`; in development, Vite dev server with HMR

### Theme & Branding

- **Primary dark:** `hsl(200, 80%, 12%)` — deep blue-teal (backgrounds, headers)
- **Accent:** `hsl(174, 72%, 46%)` — bright teal (buttons, highlights, icons)
- **Brand hex:** `#003d4d` (dark) / `#1db894` (teal) — used in email templates
- **Concept:** Water/protection branding — reflects waterproofing as the company's core service

---

## File Structure

```
├── client/
│   ├── public/
│   │   ├── images/                  # AI-generated + logo images
│   │   │   ├── hydroprotek-logo.png
│   │   │   ├── brochure-*.png       # 6 brochure page images
│   └── src/
│       ├── App.tsx                   # Root component — routing, layout, providers
│       ├── main.tsx                  # Entry point — mounts React app
│       ├── index.css                 # Global styles, CSS variables, theme
│       ├── components/
│       │   ├── Navbar.tsx            # Sticky header — desktop nav, mobile sheet menu
│       │   ├── Footer.tsx            # Footer — links, contact, social icons
│       │   ├── WhatsAppButton.tsx    # Floating WhatsApp CTA (bottom-right)
│       │   ├── SEO.tsx               # Per-page meta tag management
│       │   └── ui/                   # shadcn/ui component library
│       ├── hooks/
│       │   ├── use-toast.ts          # Toast notification hook
│       │   └── use-mobile.tsx        # Mobile breakpoint detection hook
│       ├── lib/
│       │   ├── logger.ts            # Client-side structured logger
│       │   ├── queryClient.ts       # TanStack React Query config + API client
│       │   └── utils.ts             # Tailwind class merge utility (cn)
│       └── pages/
│           ├── Home.tsx              # Hero, services grid, why-us, process, CTA
│           ├── About.tsx             # Company story, mission/vision/values, PDF downloads
│           ├── Services.tsx          # All 8 services with images and details
│           ├── Portfolio.tsx         # Project experience, materials, team capabilities
│           ├── Brochure.tsx          # Company brochure — stats, overview, services, CTA
│           ├── Contact.tsx           # Inquiry form, callback form, map, contact cards
│           └── not-found.tsx         # 404 page
├── server/
│   ├── index.ts                     # Express server bootstrap, middleware, error handling
│   ├── routes.ts                    # API endpoints (/api/inquiry, /api/callback)
│   ├── gmail.ts                     # Gmail API integration for notification emails
│   ├── logger.ts                    # Server-side structured logger
│   ├── storage.ts                   # Storage interface (template boilerplate, unused)
│   ├── static.ts                    # Production static file serving
│   └── vite.ts                      # Vite dev server setup (DO NOT MODIFY)
├── shared/
│   └── schema.ts                    # Drizzle ORM schema (template boilerplate, unused)
├── attached_assets/                 # Original uploaded construction photos
└── replit.md                        # This file — project documentation
```

---

## Pages

| Route | Page | Description |
|-------|------|-------------|
| `/` | Home | Hero banner, 8 services grid, why-choose-us, 6-step process, CTA |
| `/about` | About | Company story, mission/vision/values cards, links to Portfolio & Brochure |
| `/services` | Services | Detailed view of all 8 services with images, checklists, best-for tags |
| `/portfolio` | Portfolio | Project experience (airports, data centres), materials, team capabilities, tools |
| `/brochure` | Brochure | Company brochure — stats, about section, services, projects, why-us, materials |
| `/contact` | Contact | Contact cards, inquiry form, Google Maps embed, callback request form |

---

## Services (8 total)

1. **Waterproofing Solutions** — Terrace, basement, bathroom, structural waterproofing
2. **Civil & General Constructions** — Foundation to finishing, RCC, masonry, flooring
3. **Manpower Solutions** — Skilled/semi-skilled workforce supply across all trades
4. **Estimation & Project Planning** — BOQ, cost estimation, scheduling, tender documentation
5. **Inspection, Consulting & Maintenance** — NDT, moisture mapping, maintenance contracts
6. **Material Supply** — Cement, TMT steel, RMC, chemicals from top brands
7. **Structural Steel Fabrication** — Beams, PEB components, trusses, on-site erection
8. **Precasting** — Factory-manufactured precast concrete elements

---

## Backend API

### `POST /api/inquiry`

Receives contact form submissions. Validates name, phone, and message fields. Sends formatted HTML email to `hydroprotek.in@gmail.com` via Gmail API.

**Request body:**
```json
{
  "name": "string (required, min 2 chars)",
  "phone": "string (required, min 10 chars)",
  "email": "string (optional)",
  "service": "string (optional)",
  "message": "string (required, min 10 chars)"
}
```

### `GET /downloads/Company-Profile.pdf`

Generates and serves the Company Profile as a downloadable PDF using PDFKit. Includes company overview, project experience, team expertise, materials, and contact info.

### `GET /downloads/Company-Brochure.pdf`

Generates and serves the Company Brochure as a downloadable PDF using PDFKit. Includes services overview, project experience, why-us section, materials, and contact info.

### `POST /api/callback`

Receives callback phone number requests. Validates phone number. Sends notification email and returns WhatsApp deep link.

**Request body:**
```json
{
  "phone": "string (required, min 10 chars)"
}
```

---

## Logging System

### Server Logger (`server/logger.ts`)

- Structured, timestamped log output: `12:45:07 AM [INFO] [server] Starting...`
- Log levels: `debug`, `info`, `warn`, `error`
- Configurable via `LOG_LEVEL` environment variable (defaults to `debug` in dev, `info` in prod)
- Usage: `const logger = createLogger("moduleName");` then `logger.info("message", { key: "value" })`
- Error-level logs go to `stderr`; all others go to `stdout`

### Client Logger (`client/src/lib/logger.ts`)

- Prefixed browser console output: `[INFO] [ComponentName] message`
- Log levels: `debug`, `info`, `warn`, `error`
- Debug messages automatically suppressed in production builds (`import.meta.env.PROD`)
- Usage: `const logger = createLogger("ComponentName");` then `logger.info("action completed")`

---

## Contact & Social Media

- **Phone:** +91 99634 21217
- **Email:** hydroprotek.in@gmail.com
- **WhatsApp:** +91 99634 21217
- **Instagram:** [@hydroprotek.in](https://www.instagram.com/hydroprotek.in)
- **LinkedIn:** [hydroprotek-construction](https://www.linkedin.com/in/hydroprotek-construction-0425213b2)
- **Facebook:** [facebook.com](https://www.facebook.com/)
- **Address:** Plot 27, Road no.3, Nagaram, Hyderabad, 500083

---

## Environment & Configuration

- **Port:** 5000 (Express serves both API and frontend)
- **NODE_ENV:** `development` (Vite HMR) or `production` (static serving)
- **LOG_LEVEL:** Optional — controls server log verbosity (`debug` | `info` | `warn` | `error`)
- **Gmail:** Configured via Replit Google Mail connector (automatic OAuth token management)
- **SESSION_SECRET:** Available as a secret (currently used by template, not by active features)

---

## Running the Project

The workflow named "Start application" runs `npm run dev`, which starts the Express server with Vite middleware on port 5000. The server handles both API requests and frontend serving.

In production, the Vite build output is served as static files from `server/public/`.

---

## Recent Changes

- Feb 2026: Updated logo to excavator/construction worker design; location changed to "across India"; experience to "6+"; workforce to "50+"; removed monetary project values
- Feb 2026: Added server-side PDF generation for Company Profile and Brochure downloads (PDFKit)
- Feb 2026: Removed company name from all visible text; About page now links to Portfolio & Brochure instead of PDF downloads
- Feb 2026: Added structured logging system (server + client) with configurable log levels
- Feb 2026: Added inline code comments and function signatures to all source files
- Feb 2026: Cleaned up old navy/amber colors in email templates to match teal theme
- Feb 2026: Documented unused template boilerplate (storage.ts, shared/schema.ts)
- Feb 2026: Theme updated to deep blue-teal color scheme (water/protection branding)
- Feb 2026: Brochure page at /brochure with 6 AI-generated images
- Feb 2026: LinkedIn and Facebook social links added to footer
- Feb 2026: Company logo generated and integrated into Navbar and Footer
- Feb 2026: Downloadable PDF profiles added to About page
- Feb 2026: Portfolio page at /portfolio with project experience, materials, team capabilities
- Feb 2026: Services expanded to 8 with dedicated images
- Feb 2026: Gmail notification system for contact form and callback requests
- Feb 2026: Initial build with all pages, attached images integrated
