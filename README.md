# Zeshan Agent Canvas

A visual interface for running AI coding agents: a canvas of conversations, each
driving an agent against a workspace, with a file browser, terminal, editor and
browser tools. Ships as a web app and an Electron desktop app.

**This repository is the UI.** It talks to an agent backend over HTTP/WebSocket
(`VITE_BACKEND_BASE_URL`) and can drive ACP-compatible CLI agents as
subprocesses.

## Architecture

| Component | Detail |
|---|---|
| `src/` | React Router 7 + Vite frontend |
| `electron/` | Desktop shell |
| `helm/agent-canvas/` | Kubernetes chart |
| `docker/` | Container definitions |
| Styling | Tailwind, dark-first |

## Local setup

```sh
cp .env.sample .env
npm install
npm run dev          # web, on :3001
npm run electron:dev # desktop shell
```

See [OPERATIONS.md](./OPERATIONS.md) for environment variables, ports and
deployment requirements.

## Branding

| Surface | Where |
|---|---|
| Accent colour | `tailwind.config.js` (`primary`) |
| Wordmark SVGs | `src/assets/branding/openhands-logo.svg`, `openhands-logo-white.svg` |
| Favicons, PWA and tile icons | `public/` |
| PWA manifest | `public/site.webmanifest` — upstream shipped `name`/`short_name` **empty** |
| Windows tile colour | `public/browserconfig.xml` |
| Electron app id and product name | `electron-builder.config.mjs` |
| Product name | swept across 119 files |

The upstream accent was `#F3CE49`, a gold, now the brand indigo. The rest of the
palette is a neutral near-black scale (`#050505`, `#0a0a0a`, `#171717`,
`#242424`) that carries no brand identity.

The `3:2` aspect ratio of the wordmark was preserved deliberately:
`agent-brand-icon.tsx` pins `OPENHANDS_LOGO_ASPECT_RATIO = 3 / 2` so the
conversation chip and the 24×16 onboarding tile stay visually identical.

### Third-party marks left alone

`src/constants/acp-brand-marks.ts` holds the **Claude Code, Codex and Gemini**
logo paths, and `src/assets/branding/azure-devops-logo.svg` the Azure DevOps
mark. These identify *which agent or provider* a conversation is using — they
are other companies' trademarks used nominatively, and replacing them with your
own mark would misidentify the tool actually running.

### Deliberately left unchanged

- **`OpenHands*` CamelCase identifiers** — `OpenHandsEvent` (198 uses),
  `OpenHandsLogo`, `OpenHandsLogoButton`, `OpenHandsAgentProfile`. Real code
  symbols; the word-boundary sweep does not touch them.
- **`@openhands/agent-canvas` package name** and the `openhands-logo.svg`
  filenames — resolution identifiers referenced by `?react` imports.
- **`LICENSE`** (MIT), verbatim.

### Privacy

Analytics is PostHog, entirely env-gated: `VITE_POSTHOG_API_KEY` is commented
out in `.env.sample` with no default key, so nothing is sent unless you
configure it. Nothing needed changing.

## Provenance and licence

MIT. See [UPSTREAM.md](./UPSTREAM.md) and [LICENSE](./LICENSE).
