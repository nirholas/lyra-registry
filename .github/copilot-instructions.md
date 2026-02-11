# lyra-registry

> Lyra Registry is a standalone API + MCP service that catalogs, scores, and serves metadata for all tools in the Lyra ecosystem. It enables discovery, evaluation, and integration of 800+ crypto, blockchain, DeFi, memecoin, NFT, metaverse, trading tools, MCP tools. The list will continue to grow.

### Terminal Management

- **Always use background terminals** (`isBackground: true`) for every command so a terminal ID is returned
- **Always kill the terminal** after the command completes, whether it succeeds or fails — never leave terminals open
- Do not reuse foreground shell sessions — stale sessions block future terminal operations in Codespaces
- In GitHub Codespaces, agent-spawned terminals may be hidden — they still work. Do not assume a terminal is broken if you cannot see it
- If a terminal appears unresponsive, kill it and create a new one rather than retrying in the same terminal
