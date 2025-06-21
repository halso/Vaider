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

### Reference Links

* FastAPI docs – <https://fastapi.tiangolo.com/>  
* Streaming multipart with `requests` – <https://requests.readthedocs.io/en/latest/user/advanced/#streaming-uploads>  
* ffmpeg-python – <https://github.com/kkroening/ffmpeg-python>  
* JSON Schema Draft 2020-12 – <https://json-schema.org/>  
* OWASP API Security Top 10 – <https://owasp.org/API-Security/>  

</rewritten_file> 