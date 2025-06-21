# Plan for README.md Privacy Section

This document outlines the content to be added to the main `README.md` file under a new "Privacy and Security" section.

---

## Privacy and Security

Vaider is designed with privacy and security in mind, especially since it processes video content that could potentially be sensitive. Here's what you need to know about how Vaider handles your data.

### **1. Local-First Operation**

*   **MCP Server:** The core Vaider server (MCP Server) runs entirely on your local machine and is bound to `localhost` by default. This means it is not accessible from the internet, preventing unauthorized remote access.
*   **Test Artifacts:** All files generated during testing, including the `.mp4` video files and the `.VaiderInteractions` log folders, are stored on your local disk. Vaider does not upload or store this data anywhere else.

### **2. Third-Party Service: Google Gemini**

*   **Video Analysis:** To provide a description of the GUI, Vaider sends the test video files to the Google Gemini 1.5 Pro API. This is the only time your video data leaves your local machine.
*   **Google's Privacy Policy:** Usage of the video analysis feature is subject to Google's API terms of service and privacy policy. We strongly recommend you review them to understand how Google handles the data sent to their services.
*   **No Persistent Storage by Google:** Based on Google's documentation for their generative models, they do not use customer data from their APIs to train their models. However, policies can change, and users should stay informed.

### **3. API Key Security**

*   **Your Responsibility:** You must provide your own Google API key to use Vaider. It is your responsibility to keep this key secure.
*   **Best Practices:** We strongly recommend storing your API key in an environment variable (`GOOGLE_API_KEY`) or a local `.env` file that is included in your project's `.gitignore`. Never hard-code your API key in your source code or commit it to a repository.

### **4. Data Sensitivity**

*   **You Are in Control:** The content of the videos sent for analysis is determined by the GUI tests you write. If your application displays sensitive information (e.g., personal user data, financial details, proprietary information), it will be present in the test videos.
*   **User Discretion:** Be mindful of the data displayed in your application's UI when creating tests that will be analyzed by Vaider. You are responsible for the data you process. For highly sensitive environments, consider using mock data in your tests.

### **Summary: Our Commitment**

Vaider itself is stateless and does not collect or store any of your personal data or test artifacts on any remote server. Your privacy and the security of your data are paramount. The tool is designed to give you a high degree of control over your data by keeping it local wherever possible and being transparent about when it needs to interact with external services. 