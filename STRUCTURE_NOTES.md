# Budibase Monorepo Structure — Ultron Audit

## High-level Architecture
Budibase is a **low-code platform** for building internal tools. It has:
- A **Builder** (Svelte SPA) — visual app designer
- A **Client** (Svelte SPA) — runtime that renders end-user apps
- A **Server** (Node/Koa) — REST API, auth, data sources, automations
- A **Worker** (Node) — background jobs (Bull queues)

## Monorepo Tooling
- **Lerna v9** + **Nx** (not Turbo)
- **Yarn workspaces** with `packages/*`
- **Node engine**: `>=22.0.0 <23.0.0`

## Packages (15 total)

| Package | Type | Notes |
|---------|------|-------|
| `@budibase/builder` | Frontend (Svelte/Vite) | Admin/designer UI. Routify router. Builds to `packages/server/builder` |
| `@budibase/client` | Frontend (Svelte/Vite) | App runtime. Built JS injected into served apps |
| `@budibase/server` | Backend (Node/Koa) | Main API. Heavy deps: CouchDB, Redis, Mongo, PostgreSQL, S3, etc. |
| `@budibase/worker` | Backend (Node) | Background job worker (Bull) |
| `@budibase/pro` | Backend | Enterprise/licensing bits |
| `@budibase/cli` | CLI | Deployment/management CLI |
| `@budibase/sdk` | Library | JS SDK for external use |
| `@budibase/backend-core` | Library | Shared server logic |
| `@budibase/frontend-core` | Library | Shared frontend logic |
| `@budibase/shared-core` | Library | Shared types/utils |
| `@budibase/string-templates` | Library | Handlebars template engine |
| `@budibase/bbui` | UI Library | Svelte component library |
| `@budibase/types` | Library | TypeScript definitions |
| `upgrade-tests` | Tests | DB migration tests |

## Build Chain
```
string-templates → client, builder
client, builder → server (copied into server/builder, server/client)
backend-core, shared-core, types → server, worker, pro
```

## Deployability Assessment

### Workers for Platforms (WFP) — Static Site
- **NOT feasible as-is**: The builder is a dynamic SPA that expects a live Budibase API for app definitions, data sources, and auth.
- **Stubbed alternative**: We could build a **demo landing page** or a **built builder snapshot** (empty OutDir) that renders the shell UI without API calls. That would require stubbing all API calls.

### Workers for Platforms — Full Stack
- **Not feasible**: Budibase server needs CouchDB, Redis, and file-system access for apps. Cloudflare Workers has no native CouchDB/Redis.
- **Alternative**: Docker-based deploy (DigitalOcean, Railway, etc.) per their `hosting/` scripts.

### What CAN be deployed
1. **Static landing page** — built from scratch in the same repo (Next.js/Vite static) and deployed to WFP.
2. **Builder UI shell** — build builder with all API calls stubbed to show the UI shell (lots of work).
3. **Docs / marketing site** — if one exists in the repo (not found yet).

## Honest Blockers for Full Deployment
1. Heavy backend: CouchDB, Redis, Bull queues, isolated-vm, native modules (bcrypt, oracledb, etc.)
2. No static-export configuration in builder or client (SPA with client-side routing)
3. Runtime dependency on server APIs for any meaningful UI
