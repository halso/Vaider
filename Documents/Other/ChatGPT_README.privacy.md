# Privacy and Data Use in Vaider

Vaider is a local-first developer tool that enhances AI coding agents by interpreting GUI test videos. While your project and config files stay on your machine, Vaider does send GUI test videos to Google Gemini 1.5 Pro for analysis. Here’s what that means for your data:

---

## 🔒 What Happens With Your Data?

- **Your Code and Files Stay Local**: Vaider does not access or send your source code, test scripts, or configuration files anywhere.
- **Videos Are Sent to Gemini**: The test video files (e.g. Playwright recordings) are uploaded to Google Gemini 1.5 Pro for interpretation whenever Vaider runs.
- **No Vaider Servers**: Vaider does not operate any backend or cloud services. All API communication is direct between your machine and Google.

---

## 🧠 What Does Vaider Store?

- Vaider saves a log of each interaction with Gemini (request + response) in a folder next to each video: `.VaiderInteractions/`
- These logs remain on your machine. You can delete them at any time.

---

## 🧰 API Keys & Access

- You bring your own API key for Google Gemini 1.5 Pro.
- Your key is stored locally in your `.env` file or exported as an environment variable.
- Vaider never collects or sends this key anywhere else.

---

## 🛡️ Security and Best Practices

- MCP server only listens on `localhost`.
- All uploads to Gemini happen over HTTPS.
- Please don’t check `.env` or `.VaiderInteractions/` into version control.

---

## ❓ Got Questions?

Raise an issue on GitHub or contact the Vaider team. We’re committed to protecting your privacy and staying transparent.

---

