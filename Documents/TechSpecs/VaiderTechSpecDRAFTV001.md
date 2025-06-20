# Vaider Technical Specification - DRAFT - V001

## 1. System Architecture

High-level overview of the technical design.

## 2. Dependencies

Any external services or libraries.

## 3. MCP Server Technical Details

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

## 4. VaiderRules Configuration File

* **Purpose**: Tells the Agent when and how to use Vaider.
* **Minimal Viable Config**:

```json
{
  "video_trigger_pattern": "test-output/**/*.mp4",
  "analysis_timeout_seconds": 30,
  "on_mismatch": "retry"
}
```

* **Agent Introduction**:

  > "You have access to a new tool called Vaider which can interpret videos of GUI tests. Whenever a Playwright test produces a video, use Vaider to interpret what happened on screen and validate it against expectations."

* **When to Use**:

  * If a GUI-related test is run and a `.mp4` is generated.
  * Always analyze test videos.
  * Retry up to 5 times if mismatches exist.
  * After 5 failed attempts, stop and notify coder with `.VaiderInteractions` path.

* **Handling Vaider Results**:

  * The Agent uses the description from Vaider to validate against expectations.
  * If mismatches are found, the Agent re-attempts the test up to five times.
  * After reaching the retry limit, the Agent stops and alerts the developer.

* **Handling Errors or Timeouts**:

  * If Vaider fails to return results due to error or timeout, retry up to three times.
  * After that, log the failure and notify the developer.

## 5. Cursor Integration

* Via `.cursor/mcp.json`
* Tool name: `vaider`
* URL: `http://localhost:3456`

## 6. Video Storage

* Folder: `test-output/test-name.mp4.VaiderInteractions/`
* Contains: request/response logs, optional status/debug logs
