# Talabaty — merged project

This is the three uploaded project snapshots (`talabatyH1`, `talabaty_test`, `usedFiles`) combined
into a single, complete, working full-stack app. Nothing from any of the three source folders was
deleted — see **"What happened to each source folder"** at the bottom for exactly where every file
ended up.

## Structure

```
talabaty-merged/
├── backend/     Express + TypeScript + MongoDB API
├── frontend/    React + TypeScript + Vite app
└── archive/     Original files that weren't safe to wire into the live app (see below)
```

## Running it

**Backend**
```bash
cd backend
npm install
cp .env.example .env      # fill in MONGO_URI / JWT_SECRET
npm run dev                # http://localhost:5000
```

**Frontend**
```bash
cd frontend
npm install
cp .env.example .env      # defaults to http://localhost:5000/api
npm run dev                 # http://localhost:5173
```

`npm run build` runs `tsc` first on both, so a failing type check fails the build — that's your
zero-errors guardrail going forward too.

## What was merged from where

- **Base app (backend + frontend):** `talabatyH1`. It was the most complete, working snapshot —
  full customer flow (browse → cart → checkout → order history), partner dashboard, admin basics,
  and the original design system in `frontend/src/styles/index.css` (brand colors, `.rounded-xl2`,
  `.shadow-card`, `.btn-brand`, etc.) — **that stylesheet's rules are all still there, byte-for-byte,
  as "Section 1"** of the new `index.css`.
- **Newer public site + admin panel (frontend + backend):** `usedFiles`. This was clearly the most
  recent work in progress — a redesigned marketing homepage (`Header`, `Footer`, and the
  `sections/` folder: `Hero`, `Categories`, `HowitWorks`, `Partner`, `Rating`), a rebuilt Login/
  Register flow, and a full admin dashboard (stats, customers, partners, orders, freeze/unfreeze,
  partner approval) with a matching backend `adminController`/`adminRoutes`. All of that is now
  wired in, and its stylesheets are "Section 2" of the merged `index.css`, plus their own files
  (`Header.css`, `Hero.css`, `Login.css`, `AdminDahboard.css`, etc.) — nothing about that design was
  altered.
- **`talabaty_test`:** this snapshot's backend source had already lost most of its own code (its
  `authController.ts`, `adminController.ts`, and `cartController.ts` were empty 0-byte files in the
  zip you uploaded — only the old compiled `dist/` output still had real logic, and that logic was
  already superseded by the other two folders). Its live ideas — a Tailwind-based partner dashboard
  redesign, a `Category` product model with no way to actually create categories — were unfinished
  and not wired to anything, so integrating them as-is would have reintroduced real errors. Rather
  than lose them, the **entire original `talabaty_test` folder is preserved untouched** in
  `archive/talabaty_test-original-experimental/` so you still have every file to pull ideas from
  later; it's just not part of the live build.

## What I added on top (to make the merged pieces actually work together)

- `POST /api/auth/forgot-password`, `POST /api/auth/reset-password`, `GET /api/auth/google` on the
  backend — the new Login page already called these; they didn't exist anywhere yet. Google sign-in
  has no OAuth credentials configured, so that route redirects back to login with a clear
  `?googleError=not_configured` instead of a broken link or a raw 404.
- `resetPasswordToken` / `resetPasswordExpire` fields on the `User` model to support the above.
- Fixed the real bug in `usedFiles`' admin "products" endpoint, which was a stub that always
  returned an empty list — it now actually queries products, like the rest of the admin panel does.
- The header's account dropdown now reflects whether you're actually logged in (shows your name,
  role, "My dashboard", and a working Log out) instead of always saying "Sign in to continue".
- Added a few missing responsive breakpoints (admin sidebar on tablet/phone, hero section on
  mobile) — everything else in the two active designs was already responsive via Bootstrap's grid.
- `bootstrap-icons` added as a real dependency (the new Header/AdminDashboard use `bi bi-*` icon
  classes that weren't available before); the original Tabler icon set some pages already used is
  still loaded too, so nothing already using it breaks.

## Notes

- Both designs coexist because they don't collide: the original app's utility classes
  (`.text-navy-900`, `.bg-surface`, …) and the new site's classes (`.dashboard-wrapper`,
  `.partner-sidebar`, …) use entirely different naming, so nothing needed to be rewritten or
  overridden — everything was additive.
- I couldn't run `npm install` / `npm run build` in this environment (no network access), so I
  manually verified every relative import across both `frontend/src` and `backend/src` resolves to
  a real file, and checked every place the merged pieces call into each other. Please run
  `npm install && npm run build` in both folders as your first step — if anything unexpected turns
  up, it'll be fast to point me at the exact error.
