# Budibase Build Notes — Ultron Audit

## Environment
- Container: Node 22 (matches engine `>=22.0.0 <23.0.0`)
- Package manager: Yarn 1.22.22 workspaces
- Monorepo: Lerna 9 + Nx

## Install Status
- `yarn install` succeeded (154 seconds)
- `lerna` binary must be invoked via `yarn lerna` (not in PATH)

## Build Results

### ✅ Tier 1 — Foundational Packages
Built successfully in 24 seconds:
- `@budibase/types` (esbuild + tsc declarations)
- `@budibase/shared-core` (esbuild + tsc declarations, depends on types)
- `@budibase/string-templates` (tsc declarations + rollup CJS/ESM bundles)

### ⏳ Tier 2 — Frontend/Backend Packages
Not yet attempted:
- `@budibase/frontend-core`
- `@budibase/bbui`
- `@budibase/client`
- `@budibase/builder`
- `@budibase/backend-core`
- `@budibase/pro`
- `@budibase/cli`
- `@budibase/server`
- `@budibase/worker`

### Honest Blockers (predicted)
- `@budibase/server` requires `isolated-vm`, `bcrypt`, and Docker for CouchDB/Redis
- `@budibase/builder` and `@budibase/client` are Svelte SPAs with no static-export config; they expect a live API backend
- Native modules may fail compilation without `build-essential` headers (bcrypt, isolated-vm, oracledb)
