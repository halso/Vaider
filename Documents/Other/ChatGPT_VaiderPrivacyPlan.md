# Privacy Plan for Vaider

This Privacy Plan outlines how Vaider handles user data and interfaces with third-party services, and is intended for inclusion in the `README.md` to inform users about privacy practices.

---

## 1. Overview

Vaider is a local-first developer tool that enhances AI coding agents by interpreting GUI test videos. It is designed to operate primarily on the developer's machine, with no Vaider-run servers or cloud components involved in the processing of user data. However, test videos are always sent to the Google Gemini 1.5 Pro API for interpretation.

---

## 2. Data Handling Principles

Vaider is designed with the following data privacy principles:

- **Local Processing by Default**: All operations involving user projects and configuration files are handled locally. However, test videos are always uploaded to Google Gemini for analysis.
- **Developer-Supplied API Keys**: Vaider v1 requires users to supply their own Google Gemini 1.5 Pro API key. Vaider does not collect, store, or transmit any API keys or usage data.
- **No Telemetry**: Vaider does not include telemetry or tracking by default. All metrics or logs are stored locally and are accessible only to the developer.

---

## 3. Use of Third-Party APIs

In v1, Vaider sends test video files to Google Gemini 1.5 Pro for interpretation. This means:

- The video file is always uploaded to Google Gemini when Vaider runs its interpretation step.
- All API requests are routed directly from the local Vaider MCP server to Google Gemini.
- Vaider does not log, inspect, or persist API responses beyond saving local copies in the `.VaiderInteractions/` folder.

---

## 4. Optional Storage and Logs

- Descriptions returned from Gemini, and the associated request payloads, are saved locally in a `.VaiderInteractions/` folder next to the video file.
- Developers may delete these interaction logs at any time.
- Vaider does not send this data to any external system or developer team.

---

## 5. Security

- The Vaider MCP server runs locally on `localhost` by default and is not exposed to remote systems unless the developer changes the configuration.
- All interactions with Gemini occur over HTTPS using the official API.
- Developers are advised **not to commit their ********`.env`******** files** or any configuration containing API keys to public version control systems.

---

## 6. Recommendations

We strongly recommend:

- Keeping `.env` or key configuration files out of version control.
- Regularly reviewing `.VaiderInteractions/` contents to avoid persisting sensitive visual test data unintentionally.
- Ensuring any API provider (e.g., Google Gemini) used with Vaider meets your organisational privacy and security requirements.

---

## 7. Future Plans

In future versions, we may optionally offer a hosted proxy gateway for ease of use. If this is introduced:

- It will be **opt-in only**.
- It will include a dedicated privacy section and clear terms outlining data flow.
- The local-only mode will remain fully supported.

---

## 8. Questions or Issues

If you have any concerns about privacy while using Vaider, please open an issue on our GitHub repository or contact the maintainers directly. We welcome suggestions for improvement and are committed to maintaining developer trust.

---

