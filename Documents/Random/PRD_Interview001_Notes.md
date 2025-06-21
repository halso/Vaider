# Vaider PRD Interview Notes (Session 1)

## Meeting Attendees
- Customer Success Manager (AI)
- Client (User)


## Terminology

- AI Agent, e.g. Gemini 2.5, in Cursor will be referred to as the "Agent"
- The human Vibe Coder will be referred to as the "Coder"


## High-Level Brainstorming & Initial Q&A

- **What is "Vibe Coding"?** (Source: In-session web search)
  - An intuitive, AI-assisted approach to software development.
  - Focuses on expressing ideas in natural language and letting an AI handle the boilerplate code.
  - Enables rapid prototyping and makes development more accessible.
  - The user persona is a "Vibe Coder".
- **Problem Statement:**
  - "For the Vibe Coder the problem only occurs when they are coding an app with a GUI e.g. a Phone App or a Web App."
  - The agent cannot visually inspect or interact with the app like a human, limiting its ability to detect UI issues.
  - It can write automated tests (e.g., Python Playwright) but relies on the Vibe Coder to identify visual problems.

## Primary Goals for Vaider

- Enable the Agent to understand the UI state during test execution.

## Additional Notes

- Consider using visual recognition technologies or integrating with existing UI testing frameworks to support the primary goal.

## Success Metrics

- Speeds up the coding process.
- Reduces the need for Vibe Coders to verify or correct UI problems.

## Target Audience

- **Primary Persona – Vibe Coder**
  - Skill Level: Reasonably high.
  - Work Environment: Any (solo projects, startup teams, enterprise prototyping, etc.).
  - Current Tools: Focused on Cursor; first release will target Cursor users exclusively.

## Features (First Release)

- **Video Interpretation Service** (Agent-facing)
  1. The Agent provides a video file (or a file name that Vaider can read) that was produced during a GUI test (e.g. Python Playwright)
  2. Vaider returns a detailed description of what is happening in that video.
  3. The Agent compares this description with the expected behavior to detect mismatches and uses this feedback to fix any problems until the code works as expected.
- **Coder Experience**: No explicit new UI; once the coders has provided the Vaider tool to the AI they should simply observe that the Agent becomes faster and better at writing GUI systems.  In case of issues, e.g. the AI gets stuck in a loop due to problems with the responses from Vaider, the coder will be able to see the requests to Vaider and the responses in a directory that sits next to the original video file and is called <video_file_name>.VaiderInteractions

[Notes will be added here as the interview progresses]
