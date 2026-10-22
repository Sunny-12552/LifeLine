# LIFELINE

**Intelligent Emergency Vehicle Traffic Clearance System**

LIFELINE is a web application prototype for coordinating emergency response. It brings ambulance crews, traffic operations, patrol officers, hospitals, civilians, and system administrators into role-specific consoles, with shared views for active incidents, routes, traffic signals, and fleet status.

> **Prototype notice:** This project uses in-memory sample data and is intended for demonstration and development. It is not connected to verified emergency-service systems and must not be used to coordinate real emergencies.

## Contents

- [Features](#features)
- [Technology](#technology)
- [Requirements](#requirements)
- [Run locally](#run-locally)
- [Environment variables](#environment-variables)
- [Available scripts](#available-scripts)
- [Project structure](#project-structure)
- [Screenshots](#screenshots)
- [Prototype limitations](#prototype-limitations)

## Features

- Role-specific consoles for central administrators, ambulance drivers, traffic controllers, patrol officers, hospital staff, and civilians.
- Emergency and ambulance workflows, including trip management, vehicle telemetry, hospital coordination, and incident reporting.
- Tactical maps built with Leaflet and OpenStreetMap, with Bengaluru-focused sample locations and address search.
- Traffic signal and green-wave controls, route views, and congestion prediction.
- Administrative tools for user and organization management, reports, audit history, and emergency records.
- Demonstration authentication and sample data for exploring the different roles.

Some workflows are simulations backed by the local server; they do not dispatch real vehicles, contact real hospitals, or send messages through WhatsApp.

## Technology

- **Frontend:** React 19, TypeScript, Vite 6, Tailwind CSS 4
- **Server:** Node.js, Express 4, and `tsx` for running the TypeScript server during development
- **Maps:** Leaflet, React Leaflet, OpenStreetMap, and Nominatim address search
- **Charts and UI:** Recharts, Motion, and Lucide React
- **Optional AI:** Google GenAI SDK for Gemini-backed congestion predictions
- **Package manager lockfile:** Bun (`bun.lock`)

Dependencies are declared in [`package.json`](./package.json). The project does not require a database server.

## Requirements

- Node.js 18 or newer
- Bun (recommended, because the repository includes `bun.lock`) or npm
- An internet connection for loading OpenStreetMap tiles and using online address search

A Gemini API key is optional. Without one, congestion prediction uses the server's built-in heuristic fallback.

## Run locally

From the project root, install dependencies and start the application:

```bash
bun install
bun run dev
```

Or use npm:

```bash
npm install
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). The server uses port `3000` by default; set `PORT` in your environment if you need a different port.

On the login screen, choose a role and use the demonstration login option to explore its console. The app includes sample/demo accounts and data; do not enter real personal or operational information.

## Environment variables

No environment file is needed for a basic local run. The following variables are supported by the server:

| Variable | Required? | Purpose |
| --- | --- | --- |
| `GEMINI_API_KEY` | No | Enables Gemini-backed congestion prediction. If it is missing or the request fails, the server returns its heuristic prediction instead. |
| `PORT` | No | Changes the server port. Defaults to `3000`. |
| `JWT_SECRET` | No for local demo | Overrides the development token-signing secret. The built-in default is not suitable for deployment. |

To enable Gemini, copy `.env.example` to `.env` and replace the example value with your own key. Keep `.env` private; it is ignored by Git. Maps use OpenStreetMap and Leaflet and do not need a maps API key.

## Available scripts

| Command | Description |
| --- | --- |
| `bun run dev` or `npm run dev` | Starts the Express server with Vite development middleware and hot reload. |
| `bun run lint` or `npm run lint` | Runs the TypeScript check (`tsc --noEmit`). |
| `bun run build` or `npm run build` | Builds the frontend and bundles the server into `dist/server.cjs`. |
| `bun run start` or `npm start` | Starts the bundled server. Set `NODE_ENV=production` in the deployment environment to serve the built frontend from `dist`. |
| `bun run preview` or `npm run preview` | Runs Vite's frontend preview server. |

## Project structure

```text
.
├── assets/                         # Project/tooling assets
├── public/
│   └── images/
│       ├── backgrounds/            # Login and interface background artwork
│       └── roles/                  # Role-specific artwork and fallbacks
├── src/
│   ├── components/                 # Shared UI, maps, modals, and admin records
│   │   ├── AdminRecords/
│   │   └── modals/
│   ├── context/                    # Shared emergency and application state
│   ├── utils/                      # Audio, location data, and UI utilities
│   ├── views/                      # Role consoles and feature pages
│   ├── App.tsx                     # Main application and role-based navigation
│   ├── index.css                   # Global styles
│   ├── main.tsx                    # React entry point
│   └── types.ts                    # Shared TypeScript types
├── .env.example                    # Example optional environment configuration
├── index.html                      # Vite HTML entry point
├── package.json                    # Dependencies and scripts
├── bun.lock                        # Bun dependency lockfile
├── server.ts                       # Express API and development/production server
├── tsconfig.json                   # TypeScript configuration
└── vite.config.ts                  # Vite configuration
```

## Screenshots

The repository currently includes background and role artwork, but not screenshots of the running application. You can add your own screenshots under `docs/screenshots/` and embed them here. For example, save a screenshot as `docs/screenshots/dashboard.png`, then add:

```markdown
![LIFELINE dashboard](docs/screenshots/dashboard.png)
```

Useful screenshots to include are the role-selection/login screen, the live emergency map, and one or two role dashboards. Avoid including real names, phone numbers, locations, credentials, or other sensitive data in screenshots committed to a public repository.

## Prototype limitations

- Server-side records and application state are stored in memory; they are not persisted to a database and reset when the server restarts.
- Demo users, organizations, locations, and operational metrics are sample data.
- Gemini predictions are optional and should be treated as demonstrations, not validated operational guidance.
- Map tiles and address search depend on external OpenStreetMap services and their availability and usage policies.
- Authentication secrets and demo access are configured for local development, not production security.
- This project is not a certified emergency dispatch system. Do not rely on it for real emergency response.
