# Wattwise · Smart Energy Dashboard

React, Vite, Tailwind, React Router, Recharts and an Express simulated IoT API. Node 22 and pnpm 10.34.3 are pinned in `.mise.toml` and `packageManager`.

## Run and verify

`pnpm install --frozen-lockfile`, then `pnpm dev`. Validate with `pnpm typecheck`. Sign in at `/login`; the verified demo account remains `alex@demo.com` / `energy123`. Routes: `/`, `/login`, `/appliances`, `/connect-device`, `/analytics`, `/alerts`, `/recommendations`, `/goals`, `/settings`.

## Deployment

Vite dev and `pnpm preview` both serve `/api`; preview requires an existing build. Figma publishing uses `.figma/make/deploy`. On static hosting without an API, missing responses (404/405 or HTML) activate a clearly labelled local-storage demo; goals/settings survive browser reloads. Real 401/500 errors do not silently fall back. Configure static hosting to rewrite application routes to `index.html`.

For a Node-backed deployment, `pnpm server` starts the API (`API_PORT`, default 3001). Reverse-proxy `/api` to it alongside the static frontend. SQLite persists accounts, alerts, notification settings, connected-device metadata, appliance state, schedules and goals in `wattwise.db`; login sessions and the live power simulation reset on restart.

Email verification and password reset use six-digit, hashed, 10-minute OTP codes. Copy `.env.example` to `.env`, enable Google 2-Step Verification, and provide a Gmail App Password. Without Gmail configuration, development codes are printed to the API console. Users may register with any valid email address.

The device hub uses Web Bluetooth where supported (Chrome or Edge over HTTPS or localhost). Wi-Fi setup is explicitly simulated because browsers cannot scan or join networks, and no Wi-Fi credentials are collected. Matter products are stored as setup metadata. Siri Shortcuts can call `POST /api/assistant/command` with an `x-api-key` header and `{ "appliance": "ac", "action": "on" }`; this requires a publicly reachable HTTPS deployment and does not register the web app with HomeKit.

## API

Public auth routes: `POST /api/login`, plus `/api/auth/signup`, `/api/auth/verify`, `/api/auth/resend`, `/api/auth/forgot-password`, and `/api/auth/reset-password`. Authenticated sessions use Bearer tokens. Protected endpoints include:

- `POST /api/logout`
- `GET /api/consumption?period=day|week|month`
- `GET /api/appliances`, `PATCH /api/appliances/:id`
- `GET /api/alerts`, `DELETE /api/alerts/:id`
- `GET /api/recommendations`, `POST /api/anomaly`
- `GET /api/goals`, `POST /api/goals`, `PATCH /api/goals/:id`, `DELETE /api/goals/:id`
- `GET/POST /api/devices`, `DELETE /api/devices/:id`
- `POST /api/assistant/api-key`

Goal body: `{"applianceId":"ac","targetPercent":15,"month":"2026-10"}`. Creation: 201; deletion: 204; invalid data: 400; unknown goal: 404. Responses include baselineKWh, currentKWh, reductionPercent and clamped progress (0–100).

## Simulation assumptions

Time-of-use: ₹5/kWh 21:00–06:00, ₹8 06:00–16:00, ₹12 16:00–21:00. Cost sums hourly kWh × rate; returned tariff is weighted effective rate. These are illustrative, not live utility rates.

CO₂ = kWh × 0.71 kg/kWh (illustrative grid factor). Peak kW is maximum one-hour average, not instantaneous peak. Week/month charts repeat simulated days; comparisons use deterministic prior-period baselines. Green score is a bounded reduction-based demo score, not a certification.

Goals compare projected 30-day device usage against fixed simulated prior-month consumption (10% higher). Month selection labels a projection, not measured historical data. AC anomalies affect energy/cost/carbon and goal progress. Device toggles affect live power, not energy already consumed. Schedules are metadata only. Recommendation savings are illustrative, not tariff-based forecasts. CSV exports include peak and sustainability metrics.

## Design deliverables

- [Persona and user flow](docs/product.md)
- [Design system](docs/design-system.md)
- [Prototype handoff and demo script](docs/prototype.md)

The app is a coded interactive prototype. No Figma prototype URL has been supplied or created; the handoff specifies frames/connections rather than inventing a link. Pitch slides and the external Figma prototype still need authoring.

Accessibility improvements include chart tables, keyboard/dialog focus, skip links, reduced motion, 44px controls and high-contrast text. This is not a complete WCAG 2.1 AAA audit or certification.
