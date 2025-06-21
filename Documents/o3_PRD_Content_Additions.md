# Proposed PRD Content Additions (o3)

> These sections are drafted for insertion into *VaiderPrd.md*.  Replace the angle-bracket placeholders as real data becomes available.

---

## 2.3 Quantitative Success Metrics  
(Add under section **2. Goals and Objectives**)

| Metric | Baseline (today) | Target (v1) | Measurement Method |
|--------|------------------|-------------|--------------------|
| Manual UI–bug reports per story | ≈ <baseline">2 / story</baseline> | ≤ 1 / story (≥ 50 % reduction) | GitHub issue labels `ui-bug` over sprint window |
| First-pass green test time | 15 min median | 7 min (≥ 2× faster) | CI timing for `npm test` on reference repo |
| Agent self-correction loops per GUI feature | 4.1 avg | ≤ 2 | Count retries logged in `.VaiderInteractions/` |

---

## 4.3 Competitive / Landscape Analysis  
(Add under section **4. Features** or a new **Competitive Analysis** heading)

| Tool | Focus | Differentiator(s) | Pricing / Licence |
|------|-------|-------------------|-------------------|
| [Applitools Eyes](https://applitools.com/) | Visual AI testing (cloud) | Mature Visual-AI diffing, parallel x-browser grid | Commercial, seat-based |
| [Percy by BrowserStack](https://percy.io/) | Visual regression screenshots | Deep GitHub integration, review UI | Freemium tiers |
| [Chromatic](https://www.chromatic.com/) | Storybook component snapshots | Component-level diffs, UI review workflow | SaaS, free for OSS |
| [Replay.io](https://www.replay.io/) | Record+replay debugging | Time-travel logs, team debugging | Early-stage SaaS |
| **Vaider (this project)** | Video-based semantic descriptions for AI agents | Works offline/local, text descriptions consumable by LLM agents | OSS (MIT) |

> Reference URLs collected 2025-06-20 (Applitools homepage, Percy homepage, etc.).

---

## 6.2 Non-Functional Requirements  
(Add under section **6. Technical Specifications** — only items accepted in review)

| Category | Requirement |
|----------|-------------|
| Privacy / Licensing | Test videos remain local; Gemini usage subject to Google ToS; Vaider distributed under MIT licence. |

---

## 7. Release Criteria / Roll-out Plan  
(Insert before **Future Roadmap**)

1. **Functional:** Agent executes sample Playwright suite with Vaider loop and passes (0 human interventions).
2. **Docs:** `README`, PRD, Tech Spec v1 finalised and published.
3. **Platforms:** macOS (Apple Silicon & Intel) + Ubuntu verified.
4. **Security:** Static scan shows 0 high-severity issues; API key never written to disk.
5. **Package:** Homebrew / PyPI / npm install scripts tested.

---

## 8. Risk Register (expanded)  
(Replace current bullet list)

| ID | Risk | Owner | Likelihood | Impact | Mitigation |
|----|------|-------|-----------|--------|------------|
| R-1 | Gemini API price spike | Steve | Medium | Medium | Cache shorter clips, add open-source model fallback in v2 |
| R-2 | Infinite Agent retry loop | Steve | Low | High | Hard cap 5 retries + timeout + user alert |
| R-3 | Large video files slow analysis | Community | Medium | Medium | Enforce video size limit & down-scale before upload |
| R-4 | Privacy concerns for proprietary UIs | User org | Medium | Medium | Recommend local Gemini proxy or on-prem model |

---

## 9. Appendix – Reference Links  

1. Applitools homepage – <https://applitools.com/>  
2. Percy by BrowserStack – <https://percy.io/>  
3. Chromatic visual testing – <https://www.chromatic.com/>  
4. Replay.io debugging – <https://www.replay.io/>  

*(Links captured via public web search on 2025-06-20)* 