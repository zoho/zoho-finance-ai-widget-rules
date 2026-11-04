## Project Overview
This repository contains sample Zoho Finance widgets for **Books**, **Inventory**, **Billing**, and **Invoice** modules.
Widgets are implemented using different frameworks — **Vue**, **React**, **Preact**, and **Vanilla JS**.

AI assistants must follow the rules defined in this file and in the additional rule files located under:
`.github/instructions/sdk.instructions.md` and `.github/instructions/location.instructions.md`.

---

## Global Rules
- Follow Zoho’s widget development and manifest configuration standards.
- When editing `plugin.manifest.json`, always include the `"location"` field (refer to `.github/instructions/location.instructions.md`).
- Validate API calls using Zoho SDK guidelines (refer to `.github/instructions/sdk.instructions.md`).
- If SDK methods are unavailable for a module, provide a descriptive error message with a reference link.
- Maintain proper JSON structure and indentation when modifying manifest files.
- Always use professional, consistent comments in generated code.
- Prefer `async/await` syntax for API handling in JS frameworks.

---

## Documentation References
- **Zoho Developer Portal:** [https://www.zoho.com/developer/](https://www.zoho.com/books/developer/widgets/)
- **Zoho Extension SDK Docs:** [https://www.zoho.com/books/extensions/sdk/](https://www.zoho.com/books/developer/widgets/sdk-method.html)

---

## Behavior Expectations
- When generating code examples, always comment which framework they belong to (e.g., `// React widget example`).
- When writing step-by-step instructions, mention which file the change should occur in.
- Always assume the user is using an AI coding assistant in a **local extension development** context.

---

## Related Rule Files
- [SDK Rules](instructions/sdk.instructions.md)
- [Location Rules](instructions/location.instructions.md)
- [Vue.js Widget Rules](instructions/vue.instructions.md) - **Use this for all Vue.js widget development**
