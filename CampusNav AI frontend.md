# CampusNav AI frontend

A polished Vite + vanilla JavaScript prototype for the HACTOBERFEST '26 campus navigation concept.

## What is included

- Map-first responsive application shell
- Search with autocomplete over local JSON data
- Clickable SVG campus map with zoom controls
- Deterministic Dijkstra routing with accessible-route toggle
- Animated route drawing and destination markers
- Step-by-step simulated navigation mode
- Local assistant fallback for fees, exams, certificates, library, CSE and labs
- Action cards that open a route directly
- I'm Lost flow with selectable starting point
- About modal and visible Prototype Data warning

## Run locally

```bash
npm install
npm run dev
```

For a production build:

```bash
npm run build
npm run preview
```

## Demo flows

1. Search `CSE Lab 2`, select it, then use **Get Directions → Start Navigation → Next**.
2. Open **Assistant**, ask `I need to pay my fees`, then choose **Show Route**.
3. Choose **I'm Lost**, select Main Gate and Library.
4. Turn on **Accessible route** in the route panel to recalculate.

## Replaceable later

All locations and edges live in `src/data/campus.mock.json`. The file is explicitly marked with `isMock: true`; it should be replaced with verified campus data when available. The UI does not claim that the prototype locations are official AITR facts.

Routing is isolated in `src/services/navigation.js`, while natural-language fallback resolution is isolated in `src/services/assistant.js`, so an API-backed resolver or richer map data can be added without rewriting the shell.
