# CLI & Developer UX Additions (o3)

> Content drafted for a separate *Developer Setup & CLI* document.

---

## Install & Setup

```bash
# 1. Prerequisites
brew install ffmpeg           # lightweight video tooling
pipx install vaider-mcp       # installs `vaider-mcp` CLI wrapper
export GOOGLE_API_KEY=sk-...  # paid Gemini 1.5 Pro key (shell or .env)
```

## Running the Local MCP Server

```bash
# Start on port 3456 (default) in background
vaider-mcp start --port 3456 &

# Health-check
curl http://localhost:3456/v1/health && echo "✓ running"
```

## Typical Dev Loop

1. `npm run test:e2e` – Playwright suite produces `.mp4` video.
2. Agent detects the video, calls Vaider; MCP returns description.
3. Inspect log for a single test:
   ```bash
   cat test-output/login.mp4.VaiderInteractions/0001_response.json | jq
   ```
4. Edit code/tests, repeat.

## Troubleshooting Guide

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| HTTP 500 `UPSTREAM_ERROR` | Gemini outage or wrong API key | Verify `GOOGLE_API_KEY`; retry later. |
| Timeout after 30 s | Large video file | Down-scale via ffmpeg `-vf scale=1280:-1`. |
| `.VaiderInteractions` missing | Agent not configured | Check `VaiderRules` paths & enable flag. |

---

*Last updated 2025-06-20* 