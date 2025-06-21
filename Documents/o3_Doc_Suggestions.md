# Documentation Gap Suggestions (o3)

## PRD (VaiderPrd.md)

1. **Quantitative Success Metrics**
   - Add measurable targets (e.g., "manual UI-bug reports reduced by ≥ 50 %", "first-pass green-test time cut from _X_ min to _Y_ min"). REJECTED BY STEVE.

2. **Competitive / Landscape Section**
   - Briefly compare to tools such as Applitools, Percy, TestCafe video comparison, etc. KEEP
   - Highlight Vaider's differentiators (local workflow, AI-first design). KEEP

3. **Non-Functional Requirements**
   - Performance budget (max added test-run time). REJECTED BY STEVE.
   - Reliability (uptime, retry behaviour). REJECTED BY STEVE.
   - Privacy / licensing constraints. KEEP

4. **Release Criteria / Roll-out Plan**
   - Define what qualifies as "v1 ready" (platform support, model coverage, demo repo passes, docs complete). KEEP

5. **Stakeholders & Risk Table**
   - Expand existing risk bullets into a table with "Owner / Mitigation / Status". KEEP

---

## Tech Spec (VaiderTechSpecDRAFTV002.md)

1. **HTTP API Contract**
   - Concrete request/response JSON, headers, error codes.  KEEP
   - Clarify whether server streams bytes or accepts a path.  KEEP

2. **MCP Server Implementation Plan**
   - Language choice, module layout, dependency versions.  KEEP
   - Script/command to start the server; performance targets (file-size limit, latency). KEEP

3. **Security & Privacy Details**
   - CSRF/CORS stance, log-redaction options, disabling logs. REJECTED

4. **VaiderRules Schema**
   - Full JSON schema (retry-count, timeout, enable flag, etc.). KEEP
   - Provide a commented example file. KEEP

5. **CLI / Developer UX**
   - Installation methods (npm, pip, binary). KEEP BUT PUT IN SEPARATE DEDICATED DOC
   - Quick-start commands and troubleshooting guide. KEEP BUT PUT IN SEPARATE DEDICATED DOC

6. **Testing & Validation Plan**  KEEP BUT PUT IN SEPARATE DEDICATED DOC
   - Unit tests for MCP server, integration test against Gemini sandbox.
   - Example CI workflow.

7. **Future Extensibility Hooks**
   - Interface/adapter pattern for plugging in additional video models (Claude, OpenAI, open-source). REJECTED.