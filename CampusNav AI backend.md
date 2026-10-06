# CampusNav AI backend

Express REST backend for the existing Vite + vanilla JavaScript CampusNav AI frontend. It uses the exact existing mock location IDs and edge arrays from `src/data/campus.mock.json`; no frontend redesign or database is required.

## Setup

```bash
cd backend
cp .env.example .env
npm install
npm run validate-data
npm test
npm run dev
```

The server runs at `http://localhost:4000`.

## API

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Health, dataset counts, LLM enabled flag |
| GET | `/api/campus` | Campus graph/data |
| GET | `/api/locations?type=Lab&building=Academic%20Block%20B` | Filter locations |
| GET | `/api/locations/:id` | Fetch an exact location |
| GET | `/api/search?q=library` | Ranked search |
| POST | `/api/routes` | Dijkstra route calculation |
| POST | `/api/assistant/query` | Rule-based natural-language resolution |

Example route request:

```bash
curl -X POST http://localhost:4000/api/routes \
  -H 'Content-Type: application/json' \
  -d '{"start":"main_gate","destination":"cse_lab_2","accessible":false}'
```

Example assistant request:

```bash
curl -X POST http://localhost:4000/api/assistant/query \
  -H 'Content-Type: application/json' \
  -d '{"query":"I need to pay my fees"}'
```

## Architecture

- `services/campus.service.js` normalizes the frontend JSON into nodes, places, buildings, and services.
- `algorithms/dijkstra.js` and `algorithms/minHeap.js` are pure routing logic.
- `algorithms/directions.js` converts a path into display-ready directions.
- `services/search.service.js` ranks exact, prefix, token, substring, and tag matches.
- `services/assistant.service.js` performs closed-catalog rule resolution. It never returns routes, coordinates, or directions.
- `services/llm.service.js` is disabled by default and currently safe/no-op; the rules layer always works without an API key.
- Middleware handles CORS, JSON size, rate limiting, 404s, and sanitized errors.

## Frontend integration

No existing frontend files were modified. The current frontend remains fully functional offline. To add the enhancement layer later, the intended changes are:

1. Add `src/services/api.js` using `VITE_API_BASE_URL`.
2. In search, try `GET /api/search` and fall back to current local filtering on failure.
3. In `navigation.js`, try `POST /api/routes` and fall back to current local Dijkstra.
4. In `assistant.js`, try `POST /api/assistant/query` and fall back to current resolver.
5. Keep the current mock data and UI shape as the fallback.

## Mock data disclaimer

The dataset is copied from the existing frontend and keeps `isMock: true`. It is prototype data, not a verified AITR campus map.
