# capturly-live-signaling

Capturly Live's WebSocket signaling server: a fork of Steve Seguin's
websocket server variants for VDO.Ninja-style apps, deployed as a container
(`Dockerfile`) behind `live.capturly.app`.

<!-- SYNCED FROM framework/agents/base.md @ c966170 — do not edit here; fix in framework and re-sync. -->


# Corey Alan Consulting — org rules

This repo belongs to Corey Alan Consulting and runs on the company platform (GKE prod + K3s staging, GitOps via ArgoCD). The authoritative engineering standards live in the `framework` repo (`Corey-Alan-Consulting/framework`, locally usually `../framework`); where this file and framework disagree, framework wins. Its `llms.txt` indexes every page.

## Stack (org default)

TypeScript (strict) · Next.js App Router · pnpm 10 + Turborepo · Prisma + PostgreSQL · Redis via ioredis (Sentinel, fail-open) · NextAuth v5 through `@corey-alan-consulting/auth-core` · Tailwind + Radix · Vitest + Playwright · Changesets for releases · Sentry. Check this repo's `package.json` before assuming versions — repos migrate at different speeds.

## Non-negotiables

1. **No secrets in code, config, or output.** Secrets come from Bitwarden Secrets Manager via `.envrc` (local) or the platform pipeline (deployed). Never commit a value, never print one, never invent an `.env` workaround. `framework/standards/security/secrets.md`.
2. **Conventional Commits** for all commit messages and PR titles (`feat(scope): …`, `fix: …`); CI enforces PR titles. User-visible changes need a Changeset.
3. **Never weaken CI gates**: Trivy severity gates, CodeQL upload, action SHA-pinning, zizmor findings, `id-token` permissions. If a gate blocks you, fix the finding.
4. **Deployment is GitOps.** No `kubectl` against clusters, no manual image tags, no editing `platform-infra` rendered YAML by hand. Images are digest-pinned and Cosign-signed by the shared workflows. `framework/platform/app-contract.md`.
5. **Don't hand-roll what the platform provides**: auth (`auth-core`), rate limiting (shared presets), Redis client (Sentinel fail-open singleton), Dockerfiles (`.prebuilt`/`.migrate` pattern), CI (callers of `deploy-workflows`).
6. **Database changes go through Prisma migrations** and ship in the separate migration image; never mutate schema at app startup.
7. **Errors are handled loudly**: report to Sentry, no silently swallowed catches, rate limiters and caches fail open by design — but say so in code.

## When unsure

Consult framework first: `standards/` for requirements, `platform/` for infra contracts, `guides/` for procedures, `decisions/` for why. If framework lacks the answer, say so rather than inventing a convention, and flag it for `framework/meta/BACKLOG.md`.

<!-- END SYNC: framework/agents/base.md -->

## Layout

- `server.js` — basic fan-out server (generic, personal use only).
- `vdoninja.js` — the VDO.Ninja-optimized router (`npm start`).
- `vdoninja_advanced.js` — multi-room, stream-ownership aware router (AGPLv3).
- npm, no lint or test scripts; `npm start`, `npm run start:basic`, `npm run start:advanced`.

## Rules of this repo

- Upstream fork: keep changes to deployment and configuration; routing changes go
  upstream first. Licence terms (AGPLv3 for the advanced server) stay intact.
- TLS comes from `SERVER_CERT`/`SERVER_KEY` (or `CERT_PATH`/`KEY_PATH`); never
  commit certificate material.
