# Product Requirements Document: Vaider

## 1. Introduction

* **Problem Statement**: For the Vibe Coder the problem only occurs when they are coding an app with a GUI (e.g., a Phone App or a Web App). The Agent can write automated tests but cannot actually "see" or interact with the live UI, so UI issues go unnoticed unless the human points them out.
* **Vision**: What is the long-term vision for Vaider?

## 2. Goals and Objectives

* **Primary Goals**: Enable the Agent to understand the UI state during test execution.
* **Success Metrics**:

  * Speed up the coding process.
  * Reduce the need for Vibe Coders to verify or correct UI problems.

## 3. Target Audience

* **User Personas**: Primary Persona – Vibe Coder

  * Skill level: reasonably high.
  * Work environment: any (solo projects, startup teams, enterprise prototyping, etc.).
  * Current tools: focused on Cursor; first release will target Cursor users exclusively.

## 4. Features

* **Core Features**:

  * Video Interpretation Service (Agent-facing). Process:

    1. Agent provides a video file (or file name) generated during a GUI test.
    2. Vaider returns a detailed textual description of what happens in the video.
    3. The Agent compares this description with expected behavior to identify mismatches.

  * Coder Experience: No explicit new UI. Once the coder has provided the Vaider tool to the Agent, they should simply observe that the Agent becomes faster and better at writing GUI systems. In case of issues, for example if the Agent gets stuck in a loop due to negative responses from Vaider, then the coder can inspect a directory created next to the original video file named `<video_file_name>.VaiderInteractions`, which will contain the requests sent to Vaider and the corresponding responses.

 Limititations:-
 – Version 1 will only support Google Gemini 1.5 Pro as the underlying video-processing AI, which requires a paid API Key (since non-Pro Gemini models produce low quality video descriptions)
 
## 5. Design and UX

* **Mockups/Wireframes**: No dedicated UI in v1; mock-ups deferred until a visual surface is introduced.
* **User Flow**:

  1. **Install & Enable**

     1. Coder adds the Vaider extension/CLI to the project (details to be added in Technical Spec)
     2. Coder provides a paid Google API key for Gemini 1.5 Pro (e.g., in a local `.env` file ignored by Git, or exported as `GOOGLE_API_KEY`).
     3. Coder configures Cursor so the Agent can invoke Vaider APIs.
     4. Coder adds and configures the `VaiderRules` file (supplied in Vaider docs) that instructs the Agent when to use the Vaider API — for example, whenever Playwright UI tests are run.
  2. **Test Creation & Execution**

     1. When the Agent writes Playwright GUI tests it records a video of the whole test (e.g., `test-output/test-abc/test-abc.mp4`).
  3. **Video Interpretation**

     1. Immediately after the test finishes, the Agent calls Vaider, passing the video filename.
     2. Vaider forwards the file to Google Gemini and returns a detailed, step-by-step textual description of on-screen activity to the Agent.
  4. **Validation Loop**

     1. Agent compares the description with its expected scenario.
     2. If mismatches exist, `VaiderRules` instructs the Agent to revise code or tests and re-run until expectations match.
  5. **Coder Feedback Surface**

     1. On success, the coder simply sees green tests; development proceeds faster.
     2. On failure (e.g., Agent stuck in a loop), the coder can troubleshoot by checking the `test-output/test-abc/test-abc.mp4.VaiderInteractions/` folder, which contains files for every request to Vaider and every returned description.
  6. **Improvement in Vibe Coder Productivity**

     1. As the Agent can autonomously catch and rectify GUI issues via Vaider, it requires less feedback from the Vibe Coder, enabling faster, more independent task completion.

## 6. Technical Specifications

* **System Architecture**: High-level overview of the technical design.
* **Dependencies**: Any external services or libraries.

### 6.1 MCP Server Technical Details

* **Purpose**: The MCP (Message Communication Protocol) server allows the AI agent to communicate with the Vaider tool for video analysis.

* **Architecture**:

  * The MCP server runs locally on the developer's machine, listening on a specified port. By default, it listens only on localhost to avoid any unintended remote exposure.
  * It is configured to use HTTP/SSE (Server-Sent Events) transport for communication in version 1.
  * The server acts as a bridge between the Agent and the video analysis service (Google Gemini 1.5 Pro).

* **Configuration**:

  * Developers should configure HTTP transport by setting the appropriate configuration in the `.cursor/mcp.json` file.
  * Example HTTP configuration:

    ```json
    {
      "tools": {
        "vaider": {
          "transport": "http",
          "url": "http://localhost:3456"
        }
      }
    }
    ```

* **Workflow**:

  * When the Agent completes a GUI test, it sends the video file path to the MCP server.
  * The MCP server forwards the video to the configured video analysis service and returns the textual description to the Agent.
  * The communication protocol ensures that the Agent can seamlessly request and receive video analysis results in real-time.

* **Benefits Of HTTP/SSE Over Stdio Option**:

  * Easier testing and debugging due to the ability to interact with the server via standard web tools when using HTTP.
  * Flexibility in transport options allows developers to choose the method that best fits their development environment.

* **Error Handling**:

  * If an error occurs, the MCP server returns an HTTP 500 response with a JSON-formatted body describing the issue. This allows the Agent to gracefully detect and log failures.


## 7. Future Roadmap

*   **V2 Features**:
    * Support additional video-processing AI services beyond Google Gemini (e.g., OpenAI video models, Anthropic Claude Vision, open-source models).

## 8. Other Notes 

*   **Experimental Programs**: During development of the project a number of experimental programs will be used to try out various technologies employed in the system before working on the production code. These will include:
    - One that creates and runs a simple Hello World MCP server that can be run and tested by an Agent that just says "Hello Agent" and tells it the current time.
    - One that takes a test-input video file and sends it to Gemini to get a description of it.


## TO DO LATER

    3.1 Competitive Analysis
Comparison to Other Tools: Provide a brief analysis of similar tools in the market, highlighting what makes Vaider unique and why developers would choose it.

    Risk Management

- Model Dependency: Over-reliance on Gemini 1.5 Pro may limit accessibility. Mitigation: add support for open models and alternatives in V2.

- Looping Agent Behavior: Agent retry loops on malformed video descriptions could delay output. Mitigation: future fail-safe timeout + coder notification mechanism planned.

  - V2 Features:

    - Support additional video-processing AI services beyond Google Gemini (e.g., OpenAI video models, Anthropic Claude Vision, open-source models).