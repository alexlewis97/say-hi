# Base44 Dev Notes

## What this is
`say-hi` — a Node.js chat experiment that gets TypeSafe's JEV model to "talk" by typing one keyboard key per API request. Single server (`server.js`) serves static HTML (`public/index.html`) and proxies JEV API calls via SSE.

## Running it
- `docker compose -f docker-compose.base44.yml up -d --build`
- App listens on port 3000 (mapped from `JEV_CHAT_PORT`).
- `node --watch server.js` gives live reload on source changes.
- Health check: `GET /api/config` returns 200 JSON.

## Key dependency
- `TYPESAFE_API_KEY` (external, from typesafe.ai dashboard) — required for chat to work. The app boots without it (page shows a config error). Delivered via `/run/base44/app.env`.

## Notable quirks
- `server.js` originally bound to `127.0.0.1`; `JEV_CHAT_HOST` env var (default `127.0.0.1`) overrides the bind address so the container can expose the port.
- Node 21.7+ required (uses `fetch`, `process.loadEnvFile`).
- Only runtime dependency: `predictionary` (installed via `npm install`).
- Logs go to `logs/` (git-ignored); the API key is never logged.
- No frontend bundler — `public/index.html` is plain HTML served directly.
- Tests: `npm test` (mocked, no paid calls). `npm run eval` is LIVE and PAID.
