# Vaider Licensing Plan

## 1. Context

Vaider is designed to be a developer-side tool that enhances the capabilities of AI coding agents (e.g., those in Cursor) by interpreting GUI test output videos. Version 1 integrates exclusively with Google Gemini 1.5 Pro for video analysis, requiring developers to bring their own paid API key.

This document outlines licensing options and a comparison of well-known Free and Open Source Software (FOSS) licenses.

---

## 2. Licensing Model Options

### 2.1 Developer Brings Their Own Key (BYOK)

**Policy**: Vaider requires developers to supply their own API key for Google Gemini 1.5 Pro.

**Justification**:

- Avoids billing and compliance overhead for the Vaider team.
- Keeps the tool lightweight and developer-managed.
- Developers retain full control over costs and model usage.

---

## 3. FOSS License Comparison

| License      | Allows Commercial Use | Requires Attribution | Copyleft (Must Share Derivatives) | Patent Grant | Suitability for Vaider             |
| ------------ | --------------------- | -------------------- | --------------------------------- | ------------ | ---------------------------------- |
| MIT          | ✅ Yes                 | ✅ Yes                | ❌ No                              | ❌ No         | ✅ Very suitable                    |
| Apache 2.0   | ✅ Yes                 | ✅ Yes                | ❌ No                              | ✅ Yes        | ✅ Suitable, more legal protection  |
| GPLv3        | ✅ Yes                 | ✅ Yes                | ✅ Yes                             | ✅ Yes        | ⚠️ Less suitable (strict copyleft) |
| BSD 3-Clause | ✅ Yes                 | ✅ Yes                | ❌ No                              | ❌ No         | ✅ Suitable                         |
| MPL 2.0      | ✅ Yes                 | ✅ Yes                | ✅ Yes (file-level)                | ✅ Yes        | ⚠️ OK, but adds complexity         |

---

## 4. Understanding Copyleft and Its Implications

**What is Copyleft?**\
Copyleft is a licensing principle requiring that any software derived from a copyleft-licensed project must also be released under the same license. It ensures that the freedoms granted by open source are preserved in all derivative works.

**Strict Copyleft** (as seen in GPLv3):

- Requires anyone modifying and distributing the software to also publish their source code under the same license.
- Applies to the whole combined work, not just the original files.

**Why Avoid It for Vaider?**

- Limits the ability of downstream users to integrate Vaider into proprietary systems.
- Could deter commercial users or contributors who want to maintain licensing flexibility.
- Adds legal and compliance overhead.

**Vaider’s Goal** is to maximise usability and adoption in a wide variety of developer environments, including closed-source or hybrid workflows — which is best supported by permissive licenses like MIT or Apache 2.0.

---

## 5. Understanding Patent Grant and Apache 2.0

**What is a Patent Grant?**\
Some FOSS licenses, including Apache 2.0, explicitly include a clause that grants users a license to any patents held by contributors that are necessary to use the software. This protects users from being sued by contributors over patent claims.

**Advantages of Apache 2.0:**

- Provides additional legal safety to end users, particularly organisations, by explicitly granting rights to any contributor-held patents.
- Clarifies patent-related rights, reducing ambiguity.

**Why Not Use Apache 2.0 for Vaider?**

- Vaider has no known patent risks or novel patented technology.
- The added legal wording and complexity are unnecessary for a lightweight developer tool.
- MIT is more concise and widely understood by individual developers.

**Conclusion:** For Vaider’s simple, self-hosted, developer-focused use case, the MIT License is legally sufficient and keeps onboarding friction low.

---

## 6. Recommendation

We recommend releasing Vaider under the **MIT License** for maximum adoption, simplicity, and compatibility with the developer ecosystem.

- Encourages community trust and contributions.
- Easily integrable with other open tools (e.g., Cursor).
- Developers remain responsible for third-party service use (e.g., Gemini).

Alternative: **Apache 2.0** offers additional patent protection but is slightly more verbose.

---

## 6a. How to Apply the MIT License (Step-by-Step)

To apply the MIT License to the Vaider project, follow these steps:

1. **Create a LICENSE File**

   - In the root of your project, create a file named `LICENSE` (no file extension).
   - Copy and paste the standard MIT License text from [opensource.org](https://opensource.org/licenses/MIT).
   - Replace `[year]` with the current year.
   - Replace `[fullname]` with your name or organisation name.

2. **Add License Header to Source Files (Optional but Recommended)**

   - At the top of each `.py`, `.js`, or `.ts` file (or any other source code file), add a comment block such as:
     ```
     MIT License

     Copyright (c) 2025 Your Name

     Permission is hereby granted, free of charge, to any person obtaining a copy
     of this software and associated documentation files (the "Software"), to deal
     in the Software without restriction...
     ```
     (Include the full license text or a short reference depending on your team's preference.)

3. **Declare the License in **``**, **``**, or other metadata**

   - For Python:
     ```toml
     [project]
     license = {text = "MIT"}
     ```
   - For Node.js:
     ```json
     "license": "MIT"
     ```

4. **Make It Clear in Your README**

   - Add a “License” section at the end of your `README.md`:
     ```markdown
     ## License

     This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
     ```

5. **(Optional) Add a LICENSE Badge**

   - Example Markdown badge for visibility:
     ```markdown
     ![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
     ```

These steps ensure that others can clearly understand the licensing terms and use the project confidently.

---

## 7. Licensing FAQ (Draft)

**Q: Can I use Vaider in commercial projects?**\
Yes, the core tool is open source under MIT or another permissive license.

**Q: Do I need to pay to use Vaider?**\
Not to us — but you must provide your own Google Gemini 1.5 Pro API key, which is a paid service.

**Q: Will you support other video AI models?**\
Possibly — but developers will always bring their own keys for any supported service.

**Q: Will you offer a hosted version?**\
No — Vaider is strictly a developer-side tool.

---

## 8. Next Steps

-

