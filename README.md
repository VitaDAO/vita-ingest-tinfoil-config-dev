# Vita Ingest Tinfoil development config

Public, development-only Tinfoil configuration for testing Vita Ingest changes without adding releases to staging or production.

## Current candidate

- Vita App PR: [VitaDAO/vita-app#277](https://github.com/VitaDAO/vita-app/pull/277)
- Source: `7193ba9805d9c561f244819901be32670bb977e1`
- Image: `ghcr.io/vitadao/vita-ingest@sha256:1ae10c0c38bce7f93ab4c2f4e7ca0bbde61f3b0edbb02a8cc7251f5ffe57b29d`
- Container name: `vita-ingest-dev`
- URL: `https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev`

## Safety boundary

- Debug and synthetic/test data only. Do not use production users, health data, OAuth applications, Supabase credentials, or connection tokens.
- Secret values belong in Tinfoil's encrypted secret store, never in Git, issues, pull requests, screenshots, or logs.
- This repository uses only the `STAGING_*` Supabase and connection-token secret names.
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

The wearable credentials must belong to dedicated test OAuth applications or provider sandboxes. Register these callbacks:

```text
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/oura/callback
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/whoop/callback
https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev/api/wearable/withings/callback
```

## Release

After review and merge, run **Actions → Tinfoil Release** with an unused development version such as `v0.0.1`. Publishing a release does not deploy it. An authorized human then creates or updates only the development container through the Tinfoil dashboard.

## Local browser

```bash
VITE_VITA_INGEST_URL=https://vita-ingest-dev.debug.vitality-now.containers.tinfoil.dev \
VITE_VITA_ALLOW_DEBUG_INGEST=1 \
npm run dev -- --host 127.0.0.1 --port 8080
```

Keep production-target and attestation-required flags unset. Debug transport is for synthetic/test data only.
