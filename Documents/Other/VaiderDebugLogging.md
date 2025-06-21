# Vaider Debugging & Logging Strategy

This document outlines the logging strategy for Vaider, balancing advanced, machine-readable logs with simple, human-friendly files for easy debugging.

## 1. Primary Debugging for the Coder: `.VaiderInteractions`

The primary method for a coder to debug a problematic test run will be to inspect the contents of the `.VaiderInteractions` folder, which is created alongside the test video.

For simplicity and clarity, this folder will contain sequentially numbered files corresponding to each interaction (retry) with the Vaider analysis service. This makes it easy to follow the conversation between the Agent and Vaider.

### File Naming Convention

Given a video file named `test-login-flow.mp4`, the corresponding log directory will be `test-login-flow.mp4.VaiderInteractions/`. The contents will be structured as follows:

*   **`01_request.json`**: The JSON body of the first request sent to `/v1/analyse`.
*   **`01_response.json`**: The full JSON response received from the server for the first request.
*   **`02_request.json`**: The request for the second attempt (if a retry occurs).
*   **`02_response.json`**: The response for the second attempt.
*   ...and so on for each retry.
*   **`server.log`**: A dedicated log file containing detailed, structured logs from the MCP server for that specific video analysis session (see below).

This approach ensures that the most common-case debugging (seeing what the agent asked for and what Vaider replied) is as straightforward as possible.

## 2. Advanced Logging: Structured Server Logs

For more advanced debugging and diagnostics, the MCP server itself will generate **structured (JSON) logs**.

Instead of plain text, the `server.log` file within the `.VaiderInteractions` folder will contain a stream of JSON objects. Each object represents a specific event within the server. To make these logs easy to filter and understand, the `event` field will follow a clear naming convention:
*   **`agent.*`**: For events related to direct interaction with the AI Agent (e.g., receiving a request, sending a final response).
*   **`remote.*`**: For events involving calls to external, third-party services (e.g., the Google Gemini API).
*   **`internal.*`**: For events that describe the server's own internal logic (e.g., validating data, setting up a logger).

### Example `server.log` content:
```json
{"timestamp": "2025-06-21T17:00:00Z", "level": "INFO", "event": "agent.request.received", "video_path": "test-login-flow.mp4", "request_id": "uuid-1234"}
{"timestamp": "2025-06-21T17:00:00Z", "level": "INFO", "event": "internal.validation.success", "request_id": "uuid-1234"}
{"timestamp": "2025-06-21T17:00:01Z", "level": "INFO", "event": "remote.request.start", "provider": "GoogleGemini", "request_id": "uuid-1234"}
{"timestamp": "2025-06-21T17:00:25Z", "level": "INFO", "event": "remote.request.success", "provider": "GoogleGemini", "duration_ms": 24000, "request_id": "uuid-1234"}
{"timestamp": "2025-06-21T17:00:25Z", "level": "INFO", "event": "agent.response.sent", "status_code": 200, "request_id": "uuid-1234"}
```

### Benefits of this Approach

*   **Human-Readable by Default**: The simple `_request.json` and `_response.json` files serve the 90% use case for developers.
*   **Machine-Readable for Power Users**: The structured `server.log` allows for powerful, automated analysis if needed. For example, one could easily write a script to calculate the average response time from the Gemini provider across many test runs.
*   **Encapsulation**: All artifacts related to a single video analysis are contained in one place.
*   **Minimal Cost**: Implementing JSON logging in Python is a low-effort task with libraries like `python-json-logger`, but provides significant long-term benefits for maintainability.

## 3. Example Code Implementation

Here is a simplified example of how this could be set up in the Python-based MCP server.

### Logger Configuration
This setup function would be called once at server startup. It configures a logger to write JSON-formatted logs to a specific file.

```python
import logging
import uuid
from pythonjsonlogger import jsonlogger

def setup_session_logger(log_path: str, request_id: str):
    """Configures a logger for a specific analysis session."""
    logger = logging.getLogger(f"vaider_session_{request_id}")
    logger.setLevel(logging.INFO)
    
    # Prevent logs from propagating to the root logger
    logger.propagate = False
    
    # Create a file handler to write to the session-specific log file
    file_handler = logging.FileHandler(log_path)
    
    # Create a JSON formatter and add it to the handler
    formatter = jsonlogger.JsonFormatter(
        '%(asctime)s %(name)s %(levelname)s %(message)s'
    )
    file_handler.setFormatter(formatter)
    
    # Add the handler to the logger
    logger.addHandler(file_handler)
    
    return logger

```

### Usage in an Endpoint
Within an API endpoint, the server would create a unique request ID and set up a dedicated logger for that request. This logger is then used to record structured information throughout the request's lifecycle.

```python
# In a FastAPI endpoint for /v1/analyse

# 1. Generate a unique ID for this request
request_id = str(uuid.uuid4())

# 2. Define the path for the server log within the .VaiderInteractions directory
interactions_dir = f"{video_path}.VaiderInteractions"
# ... create directory if it doesn't exist ...
server_log_path = f"{interactions_dir}/server.log"

# 3. Set up the logger for this session
logger = setup_session_logger(server_log_path, request_id)

try:
    logger.info(
        "Agent request received", 
        extra={'event': 'agent.request.received', 'video_path': video_path, 'request_id': request_id}
    )

    # ... validate request data ...
    logger.info(
        "Request data validated",
        extra={'event': 'internal.validation.success', 'request_id': request_id}
    )

    # ... call the video analysis service ...
    logger.info(
        "Calling remote provider",
        extra={'event': 'remote.request.start', 'provider': 'GoogleGemini', 'request_id': request_id}
    )
    
    # ... on success from provider ...
    logger.info(
        "Remote provider call successful",
        extra={'event': 'remote.request.success', 'provider': 'GoogleGemini', 'duration_ms': 25000, 'request_id': request_id}
    )

    # ... finally, before returning to agent ...
    logger.info(
        "Sending final response to agent",
        extra={'event': 'agent.response.sent', 'status_code': 200, 'request_id': request_id}
    )
    
except Exception as e:
    logger.error(
        "An error occurred during analysis",
        extra={'event': 'internal.error.unhandled', 'error_message': str(e), 'request_id': request_id}
    )

finally:
    # Important: Clean up the logger handlers to avoid memory leaks
    for handler in logger.handlers:
        handler.close()
        logger.removeHandler(handler)

```
This example demonstrates how each analysis request gets its own isolated, structured log file, making it easy to trace the execution of a single task without noise from concurrent requests.

## 4. Configurable Log Verbosity

To provide flexibility during development and debugging, the verbosity of the `server.log` file will be controllable via a configuration file (e.g., `vaider.conf`).

### Configuration
A `[logging]` section in the configuration file will allow the user to set a minimum log level.

**Example `vaider.conf` setting:**
```ini
[logging]
# Sets the minimum level of events to log.
# Options: DEBUG, INFO, WARNING, ERROR, CRITICAL
level = "INFO"
```

### Log Levels
The server will use standard Python logging levels. When a level is chosen, all events at that level and higher will be recorded.

*   **`ERROR`**: (Minimal Logging) Only records errors and critical failures where the server was unable to perform a function.
*   **`WARNING`**: Records errors plus warnings about potential issues (e.g., a deprecated setting is being used). The application is still functioning correctly.
*   **`INFO`**: (Default) Records warnings, errors, and informational events about the high-level progress of a request (e.g., `agent.request.received`, `remote.request.success`). This provides a good overview of operations without excessive detail.
*   **`DEBUG`**: (High Detail) Records all of the above, plus detailed diagnostic information that is useful for tracing low-level logic and diagnosing specific problems (e.g., `internal.validation.success`).

### Implementation
The logger setup function will be modified to read this configuration and set the logger's level accordingly.

```python
# In the logger setup function
import configparser
import logging

# Read configuration
config = configparser.ConfigParser()
config.read('vaider.conf')
log_level_str = config.get('logging', 'level', fallback='INFO').upper()
log_level = getattr(logging, log_level_str, logging.INFO)

# ... inside setup_session_logger ...
def setup_session_logger(log_path: str, request_id: str, level: int):
    logger = logging.getLogger(f"vaider_session_{request_id}")
    logger.setLevel(level) # Set the level from the config
    # ... rest of the function
```
This ensures users can easily adjust the log detail to suit their needs, from a quiet, error-only log to a highly verbose trace for debugging. 