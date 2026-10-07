# Base44 Dev Environment

## Project Overview
Next.js 15.5.9 App Router project (Netlify platform starter) using Tailwind CSS v4, `bright` (CodeHike WASM syntax highlighter), `@netlify/blobs`, server actions, and edge functions.

## Running the app
```bash
docker compose -f docker-compose.base44.yml up -d
```
The app runs on port 3000 via `next dev` with live reload. Dependencies install from the lockfile on container startup (`npm ci`).

## Key findings
- **Build regression in Next.js 15.5.3**: `next build` fails with `Cannot find module for page: /_not-found` during "Collecting page data". Fixed by upgrading to 15.5.9.
- **Security**: 15.5.0–15.5.6 have CVE-2025-66478 (RCE, CVSS 10.0). 15.5.7–15.5.8 have CVE-2025-55184 (DoS) + CVE-2025-55183 (source code exposure). 15.5.9 patches all known advisories in the 15.5.x line.
- The `@netlify/blobs` and edge function features require Netlify infrastructure; without `CONTEXT` env var set, those pages degrade gracefully (show an alert instead of the editor).
- `next.config.js` includes `allowedDevOrigins` derived from `BASE44_PUBLIC_HOST_SUFFIX` for the preview proxy.

## Verifying
- All routes (`/`, `/revalidation`, `/image-cdn`, `/edge`, `/blobs`, `/classics`, `/quotes/random`) should return 200.
- Run `npx next build` inside the container to verify the production build succeeds.
