# Product Requirements Document: Vaider

## 1. Introduction

*   **Problem Statement**: For the Vibe Coder the problem only occurs when they are coding an app with a GUI (e.g., a Phone App or a Web App). The Agent can write automated tests but cannot actually "see" or interact with the live UI, so UI issues go unnoticed unless the human points them out.
*   **Vision**: What is the long-term vision for Vaider?

## 2. Goals and Objectives

*   **Primary Goals**: Enable the Agent to understand the UI state during test execution.
*   **Success Metrics**: 
    * Speed up the coding process.
    * Reduce the need for Vibe Coders to verify or correct UI problems.

## 3. Target Audience

*   **User Personas**: Primary Persona – Vibe Coder
    * Skill level: reasonably high.
    * Work environment: any (solo projects, startup teams, enterprise prototyping, etc.).
    * Current tools: focused on Cursor; first release will target Cursor users exclusively.

## 4. Features

*   **Core Features**: 
    * Video Interpretation Service (Agent-facing) – initial implementation integrates with Google Gemini for video analysis:
        1. Agent provides a video file (or file name) generated during a GUI test.
        2. Vaider returns a detailed textual description of what happens in the video.
        3. The Agent compares this description with expected behavior to identify mismatches.

        *Note*: Version 1 supports only Google Gemini as the underlying video-processing AI.
    * Coder Experience: No explicit new UI. Once the coder has provided the Vaider tool to the Agent, they should simply observe that the Agent becomes faster and better at writing GUI systems. In case of issues, for example, if the Agent gets stuck in a loop due to problems with Vaider's responses, the coder can inspect a directory created next to the original video file named `<video_file_name>.VaiderInteractions`, which contains the requests sent to Vaider and the corresponding responses.

## 5. Design and UX

*   **Mockups/Wireframes**: Links to any design assets.
*   **User Flow**: Describe the user journey.

## 6. Technical Specifications

*   **System Architecture**: High-level overview of the technical design.
*   **Dependencies**: Any external services or libraries.

## 7. Future Roadmap

*   **V2 Features**:
    * Support additional video-processing AI services beyond Google Gemini (e.g., OpenAI video models, Anthropic Claude Vision, open-source models).

## 7. Other Notes 

*   **Experimental Programs**: During development of the project a number of experimental program will be used to try out various parts of the system before working on the code for them.  These will include:
    - One that creates and runs a simple Hello World MCP server that can be run and tested by an Agent that just says Hello Agent and tells it the current time.
    - One that takes a test input video file and sends it to Gemini to get a description of it.