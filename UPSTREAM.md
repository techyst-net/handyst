# Upstream source

| Field | Value |
|---|---|
| Upstream project | OpenHands (Agent Canvas UI) |
| Source | https://github.com/OpenHands/OpenHands |
| Version adapted | 1.16.0 |
| Licence | MIT |
| Retained notices | `LICENSE` |

## Licence obligations

MIT: rebranding and redistribution are permitted provided the copyright notice
and permission notice are retained. `LICENSE` is kept intact. There is no
network-use or source-disclosure obligation.

This is the most permissive licence of any product in this workspace.

## What this repository is not

It is the **UI only**. The agent runtime it connects to is a separate
deployment, and the ACP CLI agents it can drive (Claude Code, Codex, Gemini) are
third-party tools with their own licences and terms. Rebranding this UI does not
rebrand any of those.

## Pulling upstream fixes

```sh
git remote add upstream https://github.com/OpenHands/OpenHands.git
git fetch upstream --depth=50
```

Expect conflicts in `tailwind.config.js`, `src/assets/branding/`, `public/`,
and `electron-builder.config.mjs`.

Keep the wordmark's 3:2 viewBox when resolving — `agent-brand-icon.tsx` depends
on it — and do not replace the third-party agent marks in
`src/constants/acp-brand-marks.ts`.
