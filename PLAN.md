# Budibase Clone + Audit + Deploy Plan

## What it is
Budibase is an open-source low-code/no-code operations platform (fullstack monorepo).
Forked from: https://github.com/NexityNetwork/budibase
Architecture: Yarn workspaces + Lerna + Nx monorepo with 16 packages.
Key packages: server (Node/Koa), worker (Node), builder (Svelte/Vite frontend), client (runtime).
Infra deps: CouchDB, Redis, MinIO, PostgreSQL (for LiteLLM).

## Goal
1. Install deps and build what we can in the container.
2. Fix any build blockers.
3. Deploy whatever is feasible to WFP.
4. Report honestly on what works / what's stubbed / what's blocked.

## Plan
- [x] Fork + clone repo, create feat/ultron-audit branch.
- [x] Run `yarn install` (~2.5 min).
- [x] Write STRUCTURE_NOTES.md and BUILD_NOTES.md.
- [x] Build Tier 1 packages: types, shared-core, string-templates (all pass).
- [x] Build Tier 2 / builder frontend (builds successfully, output in packages/server/builder, 19MB).
- [x] Fix asset paths in builder static output (`/builder/` prefix removed from html/css/js).
- [ ] Deploy builder static output to WFP (BLOCKED: deploy_wfp returns success but serves stale cached HTML with old etag, modified files not reflected at https://budibase-builder.apps.51ultron.com).
- [ ] If resolved, verify live UI renders correctly.
- [ ] Document final status and push all commits.

## Expected blockers
- Full backend requires CouchDB + Redis + MinIO (not available in WFP).
- Builder is a client-side SPA that expects API endpoints at runtime (will show login or errors without backend).
- WFP deploy caching issue prevents updated index.html from being served.
- Some native deps (isolated-vm, bcrypt) may fail to compile (not yet tested).

## Work completed
- Git-durable: branch `feat/ultron-audit` pushed to origin.
- Dependencies installed and core packages built.
- Builder successfully built via `yarn lerna run build --scope @budibase/builder`.
- Builder output paths fixed for root-level deployment (removed `/builder/` prefix from `<link>` and `<script>` tags, plus CSS font URLs).
- Live URL deployed but serving stale content (etag `4474cca48c465d31ac9449a81ccbe424` unchanged after deploy).
