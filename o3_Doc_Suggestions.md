# Documentation Gap Suggestions (o3)

## PRD (VaiderPrd.md)

1. **Quantitative Success Metrics**
   - Add measurable targets (e.g., "manual UI-bug reports reduced by ≥ 50 %", "first-pass green-test time cut from _X_ min to _Y_ min").

2. **Competitive / Landscape Section**
   - Briefly compare to tools such as Applitools, Percy, TestCafe video comparison, etc.
   - Highlight Vaider's differentiators (local workflow, AI-first design).

3. **Non-Functional Requirements**
   - Performance budget (max added test-run time).
   - Reliability (uptime, retry behaviour).
   - Privacy / licensing constraints.

4. **Release Criteria / Roll-out Plan**
   - Define what qualifies as "v1 ready" (platform support, model coverage, demo repo passes, docs complete).

5. **Stakeholders & Risk Table**
   - Expand existing risk bullets into a table with "Owner / Mitigation / Status".

---

## Tech Spec (VaiderTechSpecDRAFTV002.md)

1. **HTTP API Contract**
   - Concrete request/response JSON, headers, error codes.
   - Clarify whether server streams bytes or accepts a path.

2. **MCP Server Implementation Plan**
   - Language choice, module layout, dependency versions.
   - Script/command to start the server; performance targets (file-size limit, latency).

3. **Security & Privacy Details**
   - CSRF/CORS stance, log-redaction options, disabling logs.

4. **VaiderRules Schema**
   - Full JSON schema (retry-count, timeout, enable flag, etc.).
   - Provide a commented example file.

5. **CLI / Developer UX**
   - Installation methods (npm, pip, binary).
   - Quick-start commands and troubleshooting guide.

6. **Testing & Validation Plan**
   - Unit tests for MCP server, integration test against Gemini sandbox.
   - Example CI workflow.

7. **Future Extensibility Hooks**
   - Interface/adapter pattern for plugging in additional video models (Claude, OpenAI, open-source). 