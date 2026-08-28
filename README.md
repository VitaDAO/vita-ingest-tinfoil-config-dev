# Vita Ingest Tinfoil development config

Public, development-only Tinfoil configuration for testing Vita Ingest changes without adding releases to staging or production.

## Current candidate

- Baseline config: `VitaDAO/vita-ingest-tinfoil-config-staging` release `v0.1.4`
- Vita App PR: [VitaDAO/vita-app#277](https://github.com/VitaDAO/vita-app/pull/277)
- Source: `131df139796f42a3364213f34b35d2c47f836320`
- Image: `ghcr.io/vitadao/vita-ingest@sha256:e11df4981502a1ec13927ab38beb70667d46c8d77d7365af24cbb82e75bef541`
- Container name: `vita-ingest-dev`
- URL: `https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev`

## Safety boundary

- Debug and synthetic/test data only. Do not use production users or real health data.
- Secret values belong in Tinfoil's encrypted secret store, never in Git, issues, pull requests, screenshots, or logs.
- This candidate intentionally uses the existing `STAGING_*` Supabase and connection-token secret names so it can exercise the migration already applied to staging. Its service-role access is therefore not data-plane isolated from staging; use only dedicated synthetic test users.
- Production and staging configuration repositories, releases, containers, domains, and tags remain separate.
- Tinfoil container lifecycle actions are performed by an authorized human in the dashboard.

## Required secret names

```text
VITA_INGEST_GHCR_TOKEN
STAGING_SUPABASE_URL
STAGING_SUPABASE_PUBLISHABLE_KEY
STAGING_SUPABASE_SECRET_KEY
STAGING_CONNECTION_TOKEN_KEK
OURA_CLIENT_ID
OURA_CLIENT_SECRET
WHOOP_CLIENT_ID
WHOOP_CLIENT_SECRET
WITHINGS_CLIENT_ID
WITHINGS_CLIENT_SECRET
```

Before deployment, confirm the wearable secret names resolve to staging/test OAuth applications that permit these callbacks. Do not deploy if they resolve to production-only OAuth applications.

```text
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/oura/callback
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/whoop/callback
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/withings/callback
```

## Release

After review and merge, run **Actions → Tinfoil Release** with an unused development version such as `v0.0.3`. Publishing a release does not deploy it. An authorized human then creates or updates only the development container through the Tinfoil dashboard.

## Local browser

```bash
VITE_VITA_INGEST_URL=https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev \
VITE_VITA_INGEST_CONFIG_REPO=VitaDAO/vita-ingest-tinfoil-config-dev \
VITE_VITA_ALLOW_DEBUG_INGEST=1 \
npm run dev -- --host 127.0.0.1 --port 8080
```

Keep production-target and attestation-required flags unset. Debug transport is for synthetic/test data only.
