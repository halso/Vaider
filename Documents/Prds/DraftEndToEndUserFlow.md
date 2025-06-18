# Vaider – Draft End-to-End User Flow (v1)

> This draft captures the complete flow for a coder and the Agent when using Vaider's Version 1 Video Interpretation Service (Google Gemini). Review and annotate as needed.

## 1. Install & Enable
1. Coder adds the Vaider extension/CLI to the project.
2. Grants permissions so the Agent can invoke Vaider APIs.
3. Vaider auto-detects Playwright GUI tests that generate video output; no further configuration required.

## 2. Test Creation & Execution
1. Agent writes or updates Playwright GUI tests.
2. Each test run records a short video (e.g. `test-XYZ.webm`) in the `videos/` directory.

## 3. Video Interpretation
1. Immediately after the test finishes, the Agent calls Vaider, passing the video filename.
2. Vaider forwards the file to Google Gemini and receives a detailed, step-by-step textual description of on-screen activity.

## 4. Validation Loop
1. Agent compares the description with its expected scenario.
2. If mismatches exist, Agent revises code or tests and re-runs until expectations match.

## 5. Coder Feedback Surface
1. On success, coder simply sees green tests; development proceeds faster.
2. On failure (e.g. Agent stuck in a loop), coder opens `videos/<video>.VaiderInteractions/` which logs each request to Vaider and the returned description for troubleshooting.

## 6. Continuous Improvement
1. Over time the Agent learns from repeated Vaider descriptions, reducing round-trips and accelerating GUI development. 