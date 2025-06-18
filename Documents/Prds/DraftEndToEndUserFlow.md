# Vaider – Draft End-to-End User Flow (v1)

> This draft captures the complete flow for a coder and the Agent when using Vaider's Version 1 Video Interpretation Service (Google Gemini). Review and annotate as needed.

## 1. Install & Enable
1. Coder adds the Vaider extension/CLI to the project.
2. Coder provides a paid Google API key for Gemini 1.5 Pro:
   - Place `GOOGLE_API_KEY=ai_********` in a local `.env` file.
   - Ensure `.env` is listed in `.gitignore` so the key is never committed.
   - Alternatively, export the variable in the shell for ephemeral use.
3. Configures Cursor so Agent can invoke Vaider APIs.
4. Adds and configures VaiderRules file (supplied in Vaider documentation) that instructs Vaider how and when to use the Vaider API (for example whenever Playwright UI tests are run)

## 2. Test Creation & Execution
1. When Agent writes Playwright GUI tests it makes sure to record a video of the whole test, for example in test-output/test-abc/test-abc.mp4

## 3. Video Interpretation
1. Immediately after the test finishes, the Agent calls Vaider, passing the video filename.
2. Vaider forwards the file to Google Gemini and receives a detailed, step-by-step textual description of on-screen activity which is returned to the Agent.

## 4. Validation Loop
1. Agent compares the description with its expected scenario.
2. If mismatches exist, VaiderRules instructs Agent to revise code or tests and re-run until expectations match.

## 5. Coder Feedback Surface
1. On success, coder simply sees green tests; development proceeds faster.
2. On failure (e.g. Agent stuck in a loop), coder opens `test-output/test-abc/test-abc.mp4.VaiderInteractions/` folder which contains a file for each request to Vaider and a file for each returned description for troubleshooting.

## 6. Improvement In Vibe Coder's Productivity
1. As the Agent is able to catch and rectify mistakes, errors and problems in the code using this GUI Vision API without the Vibe Coder's input this should mean it can be more autonomous and complete tasks with less feedback and involvement from the Vibe Coder who is overseeing the building of the code.