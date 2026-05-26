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
- [ ] Build Tier 2 / builder frontend (attempt in progress).
- [ ] If builder produces static output, deploy to WFP.
- [ ] If builder needs a backend to be useful, create a static landing/demo page.
- [ ] Document final status and push all commits.

## Expected blockers
- Full backend requires CouchDB + Redis + MinIO (not available in WFP).
- Builder may expect API endpoints at runtime.
- Some native deps (isolated-vm, bcrypt) may fail to compile.
