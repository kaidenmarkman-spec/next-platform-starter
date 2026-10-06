# Base44 Dev Environment

## Project Overview
Next.js 15 (App Router) + Tailwind CSS v4 — a Netlify platform starter.
No external credentials required to run locally.

## Running the App
```
docker compose -f docker-compose.base44.yml up -d --build
```
The app serves on port 3000. Healthcheck probes `http://localhost:3000`.

## Key Notes
- **Netlify features are gated on `process.env.CONTEXT`.** In plain `next dev` (no `CONTEXT` set), the blobs editor, runtime-context card, and edge-function pages render their fallback/empty state. This is expected — do not set `CONTEXT=dev` unless Netlify Blobs credentials are also configured, or the blobs page will crash trying to reach the Netlify Blob store.
- `next.config.js` includes `allowedDevOrigins` derived from `BASE44_PUBLIC_HOST_SUFFIX` so the preview origin can access dev assets/HMR.
- File-watch polling (`CHOKIDAR_USEPOLLING`, `WATCHPACK_POLLING`) is enabled for reliable hot reload under the Docker bind mount.
- The `/revalidation` page fetches the Wikipedia API at request time; no credentials needed.
- The `/quotes/random` route reads from `data/quotes.json` (local file, no external dependency).

## Tech Stack
- Node 22, Next.js 15.5.3, React 18.3.1, Tailwind CSS v4
- Package manager: npm (lockfile present — uses `npm ci`)
