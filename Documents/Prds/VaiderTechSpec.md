# Vaider Technical Specification

## 1. System Architecture

High-level overview of the technical design.

## 2. Dependencies

List of external services, SDKs, or libraries required by Vaider.

## 3. MCP Server Technical Details

### 3.1 Purpose

The MCP (Message Communication Protocol) server allows the AI Agent to communicate with the Vaider tool for video analysis.

### 3.2 Architecture

* The MCP server runs locally on the developer's machine, listening on a specified port. By default, it listens only on localhost to avoid any unintended remote exposure.
* It is configured to use HTTP/SSE (Server-Sent Events) transport for communication in version 1.
* The server acts as a bridge between the Agent and the video analysis service (Google Gemini 1.5 Pro).

### 3.3 Configuration

Developers configure HTTP transport via `.cursor/mcp.json`:

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

### 3.4 Workflow

1. When a GUI test finishes, the Agent sends the video file path to the MCP server.
2. The MCP server forwards the video to the configured analysis service and returns a textual description to the Agent.
3. The Agent validates the description against expectations.

### 3.5 Benefits of HTTP/SSE over stdio

* Easier testing and debugging with standard web tools.
* Flexible transport choice for different development environments.

### 3.6 Error Handling

If an error occurs, the MCP server returns an HTTP 500 response with a JSON body describing the issue so the Agent can log the failure.

## 4. VaiderRules Configuration File

### 4.1 Purpose

Instructs the Agent when and how to invoke Vaider.

### 4.2 Minimal viable config

```json
{
  "video_trigger_pattern": "test-output/**/*.mp4",
  "analysis_timeout_seconds": 30,
  "on_mismatch": "retry"
}
```

### 4.3 Agent introduction

> You have access to a new tool called **Vaider** that can interpret videos of GUI tests. Whenever a Playwright test produces a video, use Vaider to interpret what happened on screen and validate it against expectations.

### 4.4 When to use

* Whenever a GUI-related test generates a `.mp4` file.
* Always analyse test videos.
* Retry up to **5** times if mismatches exist.
* After 5 failed attempts, stop and notify the developer, pointing to the `.VaiderInteractions` folder.

### 4.5 Handling Vaider results

* Use the returned description to validate against expectations.
* If mismatches are found, re-attempt the test (max 5 attempts).
* After the retry limit, alert the developer.

### 4.6 Handling errors or timeouts

* Retry up to **3** times for errors or timeouts.
* After that, log the failure and notify the developer.

## 5. Cursor Integration

* Defined via `.cursor/mcp.json`.
* Tool name: `vaider`.
* URL: `http://localhost:3456`.

## 6. Video Storage

* Folder pattern: `test-output/<test-name>.mp4.VaiderInteractions/`.
* Contains request/response logs and optional status/debug logs.
