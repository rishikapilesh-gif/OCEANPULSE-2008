# Ocean Pulse — Deployment Checklist

## Required repository layout

Upload the **contents** of this folder to the GitHub repository root. The root must contain:

- `Dockerfile`
- `docker-compose.yml` (optional; not required for hosted deployment)
- `render.yaml`
- `railway.json`
- `package.json` and `package-lock.json`
- `apps/` and `packages/`
- `scripts/start-production.sh`

Do not upload `node_modules`, `.env`, `apps/api/.env`, local SQLite databases, or build output.

## Render Blueprint

1. Push this folder's contents to the default branch of GitHub.
2. In Render, create a Blueprint and select that repository.
3. Accept the resources described in `render.yaml`.
4. Wait for the Docker build and startup to finish.
5. Open the generated public URL. `/ready` should return HTTP 200 after the database is connected.

The Blueprint creates one Docker web service and one PostgreSQL database. The container runs the production database schema push and repeat-safe demo seed before starting the API.

## Railway / Northflank

1. Create a service from the GitHub repository and use the root `Dockerfile`.
2. Add a PostgreSQL database.
3. Set `DATABASE_URL` on the web service to the database's connection URL.
4. Set `JWT_SECRET` to a unique random value of at least 32 characters.
5. Set `DEMO_MODE=true` and `AI_PROVIDER=fallback` for the hackathon demo.
6. Enable public HTTP networking and expose the service's assigned port.

Do not override the Dockerfile start command unless you also run `sh scripts/start-production.sh`.

## Demo flow

- Shore Safety Command Center: sign in as `operator@oceanpulse.org` / `ShorePass123!`.
- Fisherman HUD: switch to `fisher@oceanpulse.org` / `FisherPass123!`.
- From the Fisherman HUD, send a demo SOS and verify it appears in the Shore Console's Distress tab.
- Send an assistance request and verify it appears in the Logistics tab.
- Use the simulation toolbar to load a scenario and verify the shore fleet map updates.

These are demo credentials for the prototype only. Change them before any non-demo use.

## What the prototype does not claim

Fleet telemetry, weather, hazards, and scenario inputs are simulated unless a real data provider is separately configured. The software is a hackathon demonstration, not a certified maritime distress or navigation system.
