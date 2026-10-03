# Wattwise

React + Vite + Tailwind CSS project running inside Figma Make with an Express simulated IoT API.

## Application additions

- `src/App.tsx`: React Router data-mode routes `/`, `/login`, `/appliances`, `/analytics`, `/alerts`, `/recommendations`, `/goals`, `/settings`. Preserve the entrypoint and router. Only the selected recommendations sidebar uses EcoTrack branding.
- `src/Goals.tsx`: goal creation, editing, deletion and projected progress.
- `server/api.js`: Express API, fixed demo login, in-memory sessions and home state. `/api/login` is public; consumption, appliances, alerts, recommendations, anomaly, logout and goals require Bearer authentication. Goals support GET/POST `/api/goals`, PATCH/DELETE `/api/goals/:id`.
- `shared/energy.js`: browser-safe time-of-use, emissions and goal calculations; keep backend and fallback formulas aligned.
- `src/demo-api.ts`: local-storage-backed fallback for static hosting, labelled in the UI. Only missing/HTML API responses activate fallback; do not hide real 401/500 errors.
- `server/index.js`: independent Node API service, `pnpm server`, `API_PORT` defaults to 3001.
- Vite dev and preview both mount `/api`. Static hosts must rewrite page routes to `index.html`.
- `docs/`: persona, user flow, design system and prototype handoff.

Dependencies additionally include express, react-router, recharts and lucide-react. Use Node 22, pnpm 10.34.3, `pnpm typecheck` and `pnpm install --frozen-lockfile`. TypeScript allows JS in server/shared. Canonical `.gitignore`, `.gitattributes`, `.mise.toml` and `vite.config.ts` already exist; do not create underscore variants.

Preserve measured chart wrappers, chart data tables, 44px hit areas, visible focus, dialog focus handling, reduced-motion support and skip links. DM Sans body and Manrope display fonts are wired in `src/index.css`. Do not claim WCAG AAA certification without a complete audit.

## Development Server

A Vite development server is **already running** on `$PORT` (default 8443). You don't need to start it manually.

- Preview URL: The user can access the running app through the preview panel
- Hot reload: Changes to source files are reflected immediately

## Project Structure

This is the canonical project structure. Start with task-relevant files below. Only follow imports or inspect other files when required, when a documented path is missing, or when the repository contradicts this guide.

- `src/main.tsx` - React entrypoint; imports `src/index.css` and mounts `src/App.tsx` into the `#root` element
- `src/App.tsx` - Primary application component and the usual starting point for UI work
- `src/index.css` - Global CSS entrypoint and Tailwind CSS v4 import
- `index.html` - Vite HTML shell containing the `#root` element and loading `src/main.tsx`
- `package.json` - Project dependencies and the Vite build, development, preview, and formatting scripts
- `vite.config.ts` - Vite configuration with React, Tailwind CSS v4, and Figma Make plugins plus the `@` alias for `src`
- `.mise.toml` - Toolchain versions for Node.js and pnpm

## Dependencies

- Runtime: React 19 and React DOM 19
- Styling: Tailwind CSS v4 with the `@tailwindcss/vite` plugin
- Build tooling: Vite 8, TypeScript 5.7, and `@vitejs/plugin-react`
- Formatting: oxfmt

## Styling

This project uses **Tailwind CSS v4** through the `@tailwindcss/vite` plugin configured in `vite.config.ts`. `src/index.css` imports Tailwind with `@import 'tailwindcss';`. Use Tailwind utility classes directly in JSX and put global CSS or Tailwind v4 theme customization in `src/index.css`. This scaffold does not need a Tailwind config file or PostCSS config.

`src/main.tsx` imports `src/index.css`, so global font wiring belongs in `src/index.css`. Keep CSS `@import` statements first, then add any `@font-face` rules and font-family defaults there.

## Code quality

- Use double quotes for strings containing apostrophes (`"We're here to help"`), or escape them in single-quoted strings. An unescaped apostrophe in a single-quoted string breaks the build.
- Ensure JSX tags are closed and braces are balanced.
- Export components as default exports.
