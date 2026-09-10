# Zeshan Agent Canvas — Operations

> Shared infrastructure (Postgres, Redis, S3, SMTP, LLM …) is wired in
> already — see [../INFRA.md](../INFRA.md). This app runs at http://localhost:3070.

## Generated configuration

A ready-to-run configuration has been generated in this directory:

- `.env`

Signing and encryption secrets in it are **real random values**, generated
per-file. Anything only you can supply — API keys, OAuth credentials — is
marked `CHANGE_ME`. Search for it:

```sh
grep -rn CHANGE_ME .
```

These files are gitignored and must not be committed.

## Ports

| Component | Port |
|---|---|
| Frontend (Vite) | `3001` |
| Agent backend (separate deployment) | `8000` |

## Required

| Variable | Purpose |
|---|---|
| `VITE_BACKEND_BASE_URL` | Absolute base URL the **browser** uses to reach the agent backend |
| `VITE_BACKEND_HOST` | `host:port` used by the Vite dev proxy |

Both point at the agent runtime, which is **not** part of this repository. With
no backend reachable, the UI loads but no conversation can start.

## Optional

| Variable | Default | Purpose |
|---|---|---|
| `VITE_FRONTEND_PORT` | `3001` | Frontend listen port |
| `VITE_BASE_PATH` | — | Serve under a sub-path |
| `VITE_USE_TLS` | `false` | HTTPS/WSS for proxied backend connections |
| `VITE_INSECURE_SKIP_VERIFY` | `false` | Skip TLS verification. **Development only.** |
| `VITE_AUTH_REQUIRED` | — | Require authentication |
| `VITE_SESSION_API_KEY` | — | Pre-shared key for backend sessions |
| `VITE_MOCK_API` | `false` | Serve mocked API responses via MSW |
| `VITE_WORKING_DIR` | — | Default workspace directory |
| `VITE_VSCODE_BASE_PATH` | — | Embedded editor mount path |
| `VITE_ENABLE_BROWSER_TOOLS` | — | Expose browser automation tools to the agent |
| `VITE_LOCK_TO_CLOUD` | — | Restrict to a hosted backend |
| `VITE_HOME_AUTOMATIONS_DEMO` | — | Demo content on the home screen |
| `NODE_ENV` | | `development` or `production` |

## Analytics (opt-in, off by default)

`VITE_POSTHOG_API_KEY`, `VITE_POSTHOG_HOST`, `VITE_POSTHOG_UI_HOST`.

There is **no default key** — `.env.sample` leaves it commented out, so nothing
is transmitted unless you deliberately configure it.

## A note on `VITE_` variables

Everything prefixed `VITE_` is **compiled into the browser bundle** and is
readable by anyone who loads the page. Never put a secret in one.
`VITE_SESSION_API_KEY` is therefore only appropriate for a trusted-network
deployment, not a public one.

## Deployment requirements

- **An agent backend must be running and reachable** at
  `VITE_BACKEND_BASE_URL`. This repository provides no runtime of its own.
- Serve the built static output behind a reverse proxy with TLS, and set
  `VITE_USE_TLS=true` so websocket connections upgrade correctly.
- Set `VITE_INSECURE_SKIP_VERIFY=false` and `VITE_MOCK_API=false` in production.
- Enable `VITE_AUTH_REQUIRED` for anything reachable beyond localhost — the
  canvas can execute code and drive a browser through the agent.
- Helm chart in `helm/agent-canvas/` for Kubernetes; `docker/` for containers.
