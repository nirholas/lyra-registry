---
name: query-lyra-registry
description: Run the Lyra Registry (a Next.js and Postgres API that catalogs and scores crypto and DeFi MCP tools) and search, filter and submit tools through its REST endpoints. Use when the user needs to discover, rank or register tools in a Lyra registry.
---

# Lyra Registry API

Lyra Registry is a Next.js 16 app (Drizzle ORM, Postgres on Neon or Supabase) that stores tool metadata with trust scores. It is not published as a runnable npm package: work from a clone of `https://github.com/nirholas/lyra-registry`. The README also lists a hosted MCP endpoint at `modelcontextprotocol.name`, which this plugin does not wire up.

## Run locally

```bash
npm install
cp .env.example .env        # set DATABASE_URL (PostgreSQL connection string)
npm run db:push             # create tables (drizzle-kit push)
npm run db:seed             # seed; LYRA_MCP_URL=http://localhost:3001/mcp npm run db:seed imports from a running Lyra MCP server
npm run dev                 # http://localhost:3000
```

Env in `.env.example`: `DATABASE_URL`, `LYRA_MCP_URL`, `API_SECRET_KEY`, `NEXT_PUBLIC_REGISTRY_URL`. Ask the user to supply `DATABASE_URL` in their own environment; do not paste connection strings into chat. Note the API routes shipped here do not check `API_SECRET_KEY`, so do not expose a writable instance publicly.

## Endpoints (`src/app/api/**/route.ts`)

All responses are `{ success, data, ... }` JSON.

- `GET /api/tools` and `GET /api/search`: query params `q`, `category` (slug), `chain`, `protocol`, `grade` (`a|b|f`), `requiresApiKey` (`true|false`), `tags` (comma separated), `page` (default 1), `limit` (1 to 100, default 20), `sortBy` (`name|createdAt|updatedAt|totalScore|downloadCount`, default `totalScore`), `sortOrder` (`asc|desc`).
- `GET /api/tools/:id`, plus `PATCH` and `DELETE` on it, and `POST /api/tools` to register a tool.
- `GET /api/trending?period=day|week|month&limit=&category=`.
- `GET /api/categories` (and `POST`).
- `GET /api/discovery?status=pending` and `POST /api/discovery` to submit a tool for review.
- `GET /api/health` returns status and stats.

## Example

```bash
curl -s 'http://localhost:3000/api/search?q=swap&chain=bsc&grade=a&limit=5'
curl -s 'http://localhost:3000/api/trending?period=week&limit=10'
```

Validate request bodies against `src/lib/validation.ts` (Zod) before POSTing; the API returns the validation error on bad input. Seed data is for development; do not present it as live ecosystem data.
