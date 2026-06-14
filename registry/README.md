# MyBI registry

Consolidated public registry the MyBI app reads (moved here from the old `plugin-registry`):

- `plugin/` — marketplace `catalog.json`, `revoked.json`, `verify-config.json` (+ CI allow/approve lists)
- `adapter/` — chart-engine `registry.json` + `benchmark.json`
- `mcp/` — MCP server `catalog.json`

All are public, read-only metadata. Trust is decided by each artifact's SIGNATURE at install time,
never by a registry claim.
