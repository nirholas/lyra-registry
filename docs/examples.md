# lyra-registry examples

Central registry hub for Lyra ecosystem tools, agents, and plugins - the NPM for Crypto AI Tools

## Example 1

```bash
git clone https://github.com/nirholas/lyra-registry.git
cd lyra-registry
npm install
```

## Example 2

```bash
cp .env.example .env
# Edit .env with your DATABASE_URL
```

## Example 3

```bash
npm run db:push
```

## Example 4

```bash
LYRA_MCP_URL=http://localhost:3001/mcp npm run db:seed
```

## Example 5

```bash
npm run db:seed
```

## Example 6

```bash
npm run dev
```

## Example 7

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

## Example 8

```text
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


Every snippet above is taken from the [repository documentation](https://github.com/nirholas/lyra-registry#readme).
