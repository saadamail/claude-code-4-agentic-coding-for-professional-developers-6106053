# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is the exercise-files repository for the LinkedIn Learning course "Claude Code 4: Agentic Coding for Professional Developers." It contains a small React app ("Stargazers") used as the course's running example. The repo has one branch per course video (naming convention `CHAPTER#_MOVIE#`, e.g. `02_03`), with `b`/`e` suffixes for a branch's beginning/end state; `main` holds the final state of the code covered in the course.

## Commands

- `npm run dev` — start the Vite dev server
- `npm run build` — production build, output goes to `docs/` (not `dist/`, configured in `vite.config.js` so the site can be served via GitHub Pages)
- `npm run preview` — preview the production build
- `npm run lint` — run ESLint (flat config, `@eslint/js` + React/React Hooks/React Refresh plugins)

There is no test suite/runner configured in this repo.

## Architecture

Single-page React 19 app built with Vite, styled with Pico CSS (`@picocss/pico`).

- `src/main.jsx` — entry point, mounts `App` in `StrictMode`.
- `src/App.jsx` — root component. Fetches cast data from `public/cast.json` on mount and holds two pieces of state: the full `cast` array and the currently-selected `memberInfo` (drives the modal).
- `src/components/Nav.jsx` — top nav bar; renders `ToggleTheme` and a dropdown listing all cast members by name.
- `src/components/ListCast.jsx` — grid of cast thumbnail images; clicking one selects that member.
- `src/components/Modals.jsx` — detail dialog for the selected cast member, with prev/next navigation by numeric `id`.
- `src/components/ToggleTheme.jsx` — cycles theme through `auto` → `light` → `dark` → `auto`, applying `data-theme` on `document.documentElement` and persisting the choice to `localStorage`.
- `src/components/icons/Arrow.jsx`, `src/components/InterfaceStyles.jsx` — small shared icon/style helpers used by `Modals`.

Data flow is intentionally simple and prop-driven: `App` owns state, passes `cast` and an `onChoice`/`handleChange` callback down to `Nav`/`ListCast`/`Modals`; there is no state management library or router.

Content and assets live under `public/`:
- `public/cast.json` — the cast member records (id, name, slug, bio, origin, favorites, etc.) fetched at runtime.
- `public/images/` — SVG/PNG art referenced by slug (e.g. `images/{slug}.svg`, `images/{slug}_tn.svg` for thumbnails).

The Vite build compiles JSX through `babel-plugin-react-compiler` (React 19 compiler) via the `@vitejs/plugin-react` config in `vite.config.js`.

## CI

`.github/workflows/` contains `main.yml` (project CI) plus `claude.yml` and `claude-code-review.yml` (Claude Code GitHub Actions integration for automated review/assistance on PRs and issues).
