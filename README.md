# opencode-models (tmiland-lab self-hosting API, static surface)

Public static API for the `tmiland-lab/opencode` self-hosting fork,
published via GitHub Pages at `https://tmiland-lab.github.io/opencode-models`.

## Endpoints

| Path | Source | Use |
| --- | --- | --- |
| `/api.json` | hourly sync from `https://models.opencode.ai/api.json` (`.github/workflows/sync.yml`) | `OPENCODE_MODELS_URL=https://tmiland-lab.github.io/opencode-models` |
| `/api.json.pinned` | manual pin of a known-good catalog | disaster fallback: point the env var here if upstream ships a bad catalog |

## Own limits

- Refresh cadence is ours (hourly cron, dispatchable).
- Fork-specific entries (local Ollama models, gateway models with our
  caps) get overlaid here later — upstream can never throttle or
  reshape the catalog under us.
- Stateful parts (LLM gateway with per-key quotas) live on the vps1
  **host** as a systemd service, not here — GitHub hosts only the
  static surface.

## Setup (one-time, needs org repo creation approval)

1. Create public repo `tmiland-lab/opencode-models` with this layout.
2. Settings → Pages → Deploy from branch → `main` → `/docs`.
3. Run `sync` workflow once via dispatch, verify
   `https://tmiland-lab.github.io/opencode-models/api.json` returns 200.
4. Keep `https://vps1.tmiland.com/opencode-models/api.json` as fallback
   mirror (already live, hourly cron on vps1).
