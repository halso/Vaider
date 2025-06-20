# Proposed Tech Spec Additions (o3)

> The following blocks are intended for insertion into *VaiderTechSpecDRAFTV002.md* (or a later draft).  Each subsection is self-contained so you can copy-paste directly.  Reference links were captured on 2025-06-20.

---

## 3.x HTTP API Contract (MCP ⇄ Agent)

### Endpoint Summary

| Method | Path | Purpose |
|--------|------|---------|
| POST   | `/v1/analyse` | Submit a GUI-test video for analysis |
| GET    | `/v1/health`  | Liveness probe for CI / Agent |

### 3.x.1 `POST /v1/analyse`

```http
POST /v1/analyse HTTP/1.1
Host: localhost:3456
Content-Type: application/json
Authorization: Bearer <CURSOR_TOOL_TOKEN>

{
  "videoPath": "test-output/test-abc/test-abc.mp4",
  "timeoutSec": 30
}
```

**Response – success (200):**
```jsonc
{
  "description": "1. The app launches… 2. User taps Login … 3. Dashboard loads …",
  "framesAnalysed": 1872,
  "model": "Gemini-1.5-Pro",
  "durationMs": 12894
}
```

**Errors:**
| Code | Meaning | Typical Cause |
|------|---------|---------------|
| 400  | `INVALID_PATH` | Video file not found / unreadable |
| 408  | `TIMEOUT` | Gemini call exceeded `timeoutSec` |
| 500  | `UPSTREAM_ERROR` | Gemini API returned 5xx |

### cURL Quick-test
```bash
curl -X POST http://localhost:3456/v1/analyse \
  -H "Content-Type: application/json" \
  -d '{"videoPath":"sample.mp4","timeoutSec":30}' | jq
```

---

## 3.x MCP Server Implementation Plan

| Topic | Decision |
|-------|----------|
| **Language** | *Python 3.11 + FastAPI* – simple async I/O, mature SSE support, 1-file prototype in <200 LOC.|
| **Video Streaming** | Read file as bytes → stream via `requests` with `files={}` multipart.  `ffmpeg-python` can down-scale if size > 20 MB. |
| **Concurrency** | `uvicorn --workers 2`  (sufficient for local dev). |
| **Start Command** | `python -m vaider_mcp.server --port 3456` or `poetry run vaider-mcp` |
| **Deps** | `fastapi`, `uvicorn[standard]`, `python-dotenv`, `requests`, `ffmpeg-python` (optional). |
| **Performance Goal** | ≤ 30 s wall-clock for 20 MB input on M1 Mac (measured via `time curl …`). |

---

## 4.x VaiderRules Full Schema

```jsonc
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "VaiderRules",
  "type": "object",
  "properties": {
    "video_trigger_pattern": {
      "type": "string",
      "description": "Glob for test videos"
    },
    "analysis_timeout_seconds": {
      "type": "integer",
      "minimum": 5,
      "default": 30
    },
    "on_mismatch": {
      "enum": ["retry", "fail"],
      "default": "retry"
    },
    "max_retries": {
      "type": "integer",
      "minimum": 0,
      "default": 5
    },
    "enabled": {
      "type": "boolean",
      "default": true
    }
  },
  "required": ["video_trigger_pattern"]
}
```

> Store as `vaider.rules.json` in project root; Agent loads and validates against the schema using [`jsonschema`](https://pypi.org/project/jsonschema/).

---

## 5.x CLI / Developer Quick-Start

1. **Install**
```bash
brew install ffmpeg           # video tools
pipx install vaider-mcp       # installs `vaider-mcp` CLI
export GOOGLE_API_KEY=sk-...  # paid Gemini 1.5 Pro key
```
2. **Run MCP server**
```bash
vaider-mcp start --port 3456 &
```
3. **Run sample tests**
```bash
npm run test:e2e      # Playwright suite produces .mp4
```
4. **Inspect logs**
```bash
cat test-output/login.mp4.VaiderInteractions/0001_response.json | jq
```

---

## 6.x Security & Privacy Enhancements

* **CORS**: disabled (same-origin only).  Enable via `--allow-origins` flag for advanced use.
* **CSRF**: Not required for localhost; documented as out-of-scope.
* **Log Redaction**: `vaider-mcp --redact-tokens` hides API keys in `.VaiderInteractions/*`.
* **Disable Logging**: `vaider-mcp --no-log` flag for sensitive projects.

Reference: OWASP API Top 10 (2023) – <https://owasp.org/API-Security/>.

---

## 7.x Testing & CI

* **Unit tests**: `pytest tests/unit` – 90 % coverage goal.
* **Integration**: Docker-compose service runs MCP + mock Gemini stub (`wiremock`).
* **CI**: GitHub Actions file `ci.yml`:
  ```yaml
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-python@v5
          with: { python-version: '3.11' }
        - run: pip install -e .[dev]
        - run: pytest -q
  ```
* **Load test**: `locust` script targets `/v1/analyse` with 10 parallel clients, 20 MB videos.

---

## 8.x Extensibility Hooks

Create an adapter interface:
```python
class VideoModelAdapter(Protocol):
    name: str
    def analyse(self, video_bytes: bytes) -> str: ...
```
Implementations: `GeminiAdapter`, `OpenAIAdapter`, `ClaudeAdapter`.  Register via entry-points so new models are plug-and-play.

---

### Reference Links

* FastAPI docs – <https://fastapi.tiangolo.com/>  
* Streaming multipart with `requests` – <https://requests.readthedocs.io/en/latest/user/advanced/#streaming-uploads>  
* ffmpeg-python – <https://github.com/kkroening/ffmpeg-python>  
* JSON Schema Draft 2020-12 – <https://json-schema.org/>  
* OWASP API Security Top 10 – <https://owasp.org/API-Security/>  

</rewritten_file> 