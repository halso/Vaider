# Testing & Validation Additions (o3)

> Intended for a standalone *Testing & CI* guide accompanying the Tech Spec.

---

## Unit Testing

* **Framework**: `pytest` (`tests/unit/*`).
* **Coverage Goal**: 90 % lines.
* **Focus Areas**:
  - JSON schema validation for `vaider.rules.json`.
  - Error mapping in `mcp_server/api.py` (400 vs 500).
  - Timeout logic.

## Integration Testing

* **Tooling**: `docker-compose` spins up:
  1. Vaider MCP server (FastAPI) on port 3456.
  2. **WireMock** stub that simulates Gemini 1.5 Pro API.
* **Happy-path test**: Post small sample `.mp4`, expect 200 with description.
* **Failure test**: Stub returns 500 → MCP propagates `UPSTREAM_ERROR`.

## Load / Stress Testing

* **Locust** script (`locustfile.py`) hits `/v1/analyse` with 10 virtual users, each uploading a 20 MB video.
* Target: < 50 % requests > 10 s; no errors.

## Continuous Integration (GitHub Actions)

```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-22.04
    services:
      wiremock:
        image: wiremock/wiremock:3.3.1
        ports: ["8080:8080"]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: {python-version: '3.11'}
      - run: pip install -e .[dev]
      - run: pytest -q
```

## Manual QA Checklist

- [ ] Run Playwright demo repo; ensure Vaider retries up to 5 times then stops.
- [ ] Disconnect internet → MCP returns `UPSTREAM_ERROR` and Agent pauses.
- [ ] API key redaction verified in `.VaiderInteractions/*` logs.

---

*Last updated 2025-06-20* 