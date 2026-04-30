# Workspace

## Overview

pnpm workspace monorepo using TypeScript + a standalone Python Flask Travel SEO Engine.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5 (Node.js) + Flask 3 (Python)
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run Node.js API server locally
- `cd artifacts/travel-seo && python app.py` — run Flask Travel SEO Engine

## Travel SEO Engine (artifacts/travel-seo/)

Python Flask application with AI-powered travel guide generation.

### Architecture
- `app.py` — Flask app, routes, caching config, sitemap
- `services/ai_logic.py` — Gemini AI integration (server-side only, key never exposed to frontend)
- `templates/base.html` — shared dark-mode layout with Tailwind CSS
- `templates/index.html` — homepage with city search and popular destinations
- `templates/guide.html` — full guide page (itinerary, budget, FAQ, insider tips)
- `templates/sitemap.xml` — dynamic XML sitemap template

### URL Structure
- Homepage: `/` or `/?lang=XX`
- Guide pages: `/<lang>/guide/<city-slug>` (e.g. `/ar/guide/marrakech`, `/de/guide/berlin`)
- Legacy redirect: `/guide/<city>` → 301 → `/en/guide/<city>`
- Sitemap: `/sitemap.xml` (506 URLs: 1 home + 100 cities × 5 languages)
- Robots: `/robots.txt`
- API: `POST /api/generate`

### Features
- Gemini 1.5 Flash AI guide generation for any city in 5 languages
- 24-hour Flask-Caching per city/language combo
- Fallback cached templates when API fails
- Autocomplete search on homepage (50-city list)
- Quick-city chips for instant navigation
- SEO: auto-generated titles & meta descriptions per city/language, hreflang for all 5 langs, canonical tags
- 506-URL dynamic sitemap (100 cities × 5 languages)
- Full RTL support for Arabic (dir="rtl", mirrored layout)
- Cyber-travel dark mode UI with Tailwind CSS
- Sticky mobile booking bar with live bookers ticker
- Urgency strip on every guide page
- Internal linking to 3 nearby/related destinations per guide (spider trap)
- Footer with top 10 destination links in current language
- Booking.com dynamic affiliate links with city + language params
- Share buttons (Twitter + copy link)

### Environment Variables
- `GEMINI_API_KEY` — Gemini AI API key (set as environment variable)
- `FLASK_SECRET_KEY` — Flask session secret
- `PORT` — Server port (default: 5000)

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.
