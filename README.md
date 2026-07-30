# 🌙 Lyra Registry 

> The NPM for Crypto AI Tools - Central registry for the Lyra ecosystem
 
Lyra Registry is a standalone API service that catalogs, scores, and serves metadata for all tools in the Lyra ecosystem. It enables discovery, evaluation, and integration of 800+ crypto & DeFi MCP tools.

Between builds I am working on cleaning up the main Lyra repo with over 800+ blockchain and cryptocurrency tools. I hope to release it soon.

## ✨ Features

- **📊 Trust Scoring** - Every tool is scored using the SperaxOS algorithm 
- **🔍 Search & Filter** - Full-text search with category, chain, and protocol filters
- **📈 Trending** - Track tool popularity and usage
- **🔌 MCP Integration** - Auto-seed from running Lyra MCP servers
- **🚀 Deploy Anywhere** - Deploy to any Node host in minutes

The public demo deployment is offline while hosting is being migrated. Run it locally with the Quick Start below, or read the source at [github.com/nirholas/lyra-registry](https://github.com/nirholas/lyra-registry).

## 🏗️ Tech Stack

- **Framework**: Next.js 14 (App Router)
- **Database**: PostgreSQL (Neon/Supabase)
- **ORM**: Drizzle
- **Language**: TypeScript
- **Validation**: Zod

## 🚀 Quick Start

### 1. Clone and Install

```bash
git clone https://github.com/nirholas/lyra-registry.git
cd lyra-registry
npm install
```

### 2. Set Up Database

Create a PostgreSQL database on [Neon](https://neon.tech) or [Supabase](https://supabase.com) (free tier works).

```bash
cp .env.example .env
# Edit .env with your DATABASE_URL
```

### 3. Run Migrations

```bash
npm run db:push
```

### 4. Seed Database

If you have a running Lyra MCP server:

```bash
LYRA_MCP_URL=http://localhost:3001/mcp npm run db:seed
```

Or seed with sample data:

```bash
npm run db:seed
```

### 5. Start Development Server

```bash
npm run dev
```

Visit http://localhost:3000 to see the landing page.

## 📡 API Reference

### Tools

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tools` | List all tools with pagination |
| POST | `/api/tools` | Register a new tool |
| GET | `/api/tools/:id` | Get tool by ID |
| PATCH | `/api/tools/:id` | Update a tool |
| DELETE | `/api/tools/:id` | Delete a tool |

### Search & Discovery

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/search?q=` | Full-text search |
| GET | `/api/trending` | Get trending tools |
| GET | `/api/categories` | List all categories |
| POST | `/api/discovery` | Submit tool for review |
| GET | `/api/health` | Health check & stats |

### Query Parameters

#### `/api/tools` and `/api/search`

| Param | Type | Description |
|-------|------|-------------|
| `q` | string | Search query |
| `category` | string | Filter by category slug |
| `chain` | string | Filter by blockchain (bsc, ethereum, etc.) |
| `protocol` | string | Filter by protocol (pancakeswap, aave, etc.) |
| `grade` | a\|b\|f | Filter by trust grade |
| `requiresApiKey` | true\|false | Filter by API key requirement |
| `tags` | string | Comma-separated tags |
| `page` | number | Page number (default: 1) |
| `limit` | number | Items per page (default: 20, max: 100) |
| `sortBy` | string | Sort field: name, createdAt, totalScore, downloadCount |
| `sortOrder` | asc\|desc | Sort direction (default: desc) |

#### `/api/trending`

| Param | Type | Description |
|-------|------|-------------|
| `period` | day\|week\|month | Time period (default: week) |
| `limit` | number | Number of results (default: 10, max: 50) |
| `category` | string | Filter by category |

### Example Requests

```bash
# Search for DeFi tools
curl "http://localhost:3000/api/search?q=defi&category=defi"

# Get top 10 trending tools this week
curl "http://localhost:3000/api/trending?period=week&limit=10"

# Get all security tools
curl "http://localhost:3000/api/tools?category=security&sortBy=totalScore"

# Register a new tool
curl -X POST "http://localhost:3000/api/tools" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "my_custom_tool",
    "description": "A custom DeFi tool",
    "category": "defi",
    "inputSchema": {
      "type": "object",
      "properties": {
        "address": { "type": "string" }
      },
      "required": ["address"]
    },
    "chains": ["ethereum", "bsc"],
    "tags": ["defi", "portfolio"]
  }'
```

## ⭐ Trust Score Algorithm

Every tool is scored using the SperaxOS trust algorithm. The score determines the tool's grade:

| Criteria | Weight | Required |
|----------|--------|----------|
| Validated | 20 | ✅ |
| Has Tools | 15 | ✅ |
| Deployment | 15 | ✅ |
| Documentation | 10 | ✅ |
| Auto Deploy | 12 | |
| License | 8 | |
| Prompts | 8 | |
| Resources | 8 | |
| Claimed | 4 | |

### Grades

| Grade | Score | Description |
|-------|-------|-------------|
| **A** | 80%+ | All required items met, excellent tool |
| **B** | 60-79% | All required items met, good tool |
| **F** | <60% | Missing required items or low score |

## 📁 Project Structure

```
lyra-registry/
├── src/
│   ├── app/
│   │   ├── api/
│   │   │   ├── tools/
│   │   │   │   ├── route.ts        # GET/POST /api/tools
│   │   │   │   └── [id]/
│   │   │   │       └── route.ts    # GET/PATCH/DELETE /api/tools/:id
│   │   │   ├── search/
│   │   │   │   └── route.ts        # GET /api/search
│   │   │   ├── trending/
│   │   │   │   └── route.ts        # GET /api/trending
│   │   │   ├── categories/
│   │   │   │   └── route.ts        # GET/POST /api/categories
│   │   │   ├── discovery/
│   │   │   │   └── route.ts        # GET/POST /api/discovery
│   │   │   └── health/
│   │   │       └── route.ts        # GET /api/health
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx                # Landing page
│   ├── db/
│   │   ├── index.ts                # Database connection
│   │   ├── schema.ts               # Drizzle schema
│   │   └── seed.ts                 # Seed script
│   ├── lib/
│   │   ├── calculateScore.ts       # Trust score algorithm (from SperaxOS)
│   │   └── validation.ts           # Zod schemas
│   └── types/
│       └── index.ts                # TypeScript types
├── drizzle.config.ts
├── next.config.ts
├── package.json
├── tsconfig.json
└── README.md
```

## 🚢 Deployment

### Vercel

1. Push to GitHub
2. Import to Vercel
3. Add `DATABASE_URL` environment variable
4. Deploy

### Railway

```bash
railway init
railway add
railway variables set DATABASE_URL=...
railway up
```

## 🤝 Integration with Lyra Ecosystem

### Auto-Seeding from Lyra MCP Server

The registry can automatically import tools from a running Lyra MCP server:

```bash
# Start Lyra server
cd ../Lyra && bun run build && bun dist/cli.mjs --transport=http --port=3001

# Seed registry
cd ../lyra-registry
LYRA_MCP_URL=http://localhost:3001/mcp npm run db:seed
```

### Discovery Pipeline (via Lyra-Intel)

The lyra-intel project can automatically discover and submit new tools:

```python
# In lyra-intel
from lyra_intel.discovery import RegistrySubmitter

submitter = RegistrySubmitter("http://localhost:3000/api/discovery")
await submitter.submit_tool(tool, security_result)
```

## 📜 License

All rights reserved. See [LICENSE](LICENSE).

## 🔗 Links

- [Lyra MCP Server](https://github.com/nirholas/Lyra) - The main MCP server with 280+ tools
- [Lyra Intel](https://github.com/nirholas/lyra-intel) - Auto-discovery and security scanning
- [SperaxOS](https://github.com/nirholas/SperaxOS) - The scoring algorithm source
- [Sperax](https://sperax.io) - Sperax ecosystem


---

## 🌐 Live HTTP Deployment

**Lyra Registry** is deployed and accessible over HTTP via [MCP Streamable HTTP](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) transport — no local installation required.

**Endpoint:**
```
https://modelcontextprotocol.name/mcp/lyra-registry
```

### Connect from any MCP Client

Add to your MCP client configuration (Claude Desktop, Cursor, SperaxOS, etc.):

```json
{
  "mcpServers": {
    "lyra-registry": {
      "type": "http",
      "url": "https://modelcontextprotocol.name/mcp/lyra-registry"
    }
  }
}
```

### Available Tools (3)

| Tool | Description |
|------|-------------|
| `search_mcp_ecosystem` | Search MCP ecosystem |
| `get_popular_mcp_servers` | Popular MCP servers |
| `get_sperax_mcp_servers` | All Sperax MCP servers |

### Example Requests

**Search MCP ecosystem:**
```bash
curl -X POST https://modelcontextprotocol.name/mcp/lyra-registry \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"search_mcp_ecosystem","arguments":{"query":"crypto defi"}}}'
```

**Popular MCP servers:**
```bash
curl -X POST https://modelcontextprotocol.name/mcp/lyra-registry \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_popular_mcp_servers","arguments":{"category":"crypto"}}}'
```

**All Sperax MCP servers:**
```bash
curl -X POST https://modelcontextprotocol.name/mcp/lyra-registry \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_sperax_mcp_servers","arguments":{}}}'
```

### List All Tools

```bash
curl -X POST https://modelcontextprotocol.name/mcp/lyra-registry \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

### Also Available On

- **[SperaxOS](https://github.com/nirholas/SperaxOS)** - source for the MCP marketplace. The hosted marketplace is offline while hosting is being migrated.
- **All 27 MCP servers** — See the full catalog at [modelcontextprotocol.name](https://modelcontextprotocol.name)

> Powered by [modelcontextprotocol.name](https://modelcontextprotocol.name) — the open MCP HTTP gateway

## Documentation

Full documentation site: **https://nirholas.github.io/lyra-registry/**

- [Getting started](docs/getting-started.md) covers install and first run.
- [Examples](docs/examples.md) has copy-paste snippets.
