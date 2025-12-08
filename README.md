# AI Rules for Zoho Finance Widgets

This directory contains AI assistant rules for developing Zoho Finance widgets. These rules ensure consistent code generation, proper SDK usage, and adherence to Zoho's widget development standards.

**Compatible with:** Cursor, GitHub Copilot, ChatGPT, Gemini, and other AI coding assistants that support rule files.

---

## 🚀 Quick Setup

### Step 1: Clone or Copy the Rules

If you're cloning this repository:

```bash
git clone https://github.com/zoho/zoho-finance-ai-widget-rules.git
cd zoho-finance-ai-widget-rules
```

### Step 2: Here's how to add our Zoho Finance widget rules to Cursor AI:

#### **Example: Adding Rules in Cursor IDE**

1. **Open Cursor Settings**
   - Press `Cmd + ,` (macOS) or `Ctrl + ,` (Windows/Linux)
   - Or navigate: `Cursor > Settings` → Search for **"Rules and Commands"**

2. **Go to Project Rules**
   - Click on **"Rules and Commands"** in the left sidebar
   - Select the **"Project Rules"** tab

3. **Copy Rules to .cursor/rules Folder**
   - In your project root, create: `.cursor/rules/` folder
   - Copy all `.mdc` files from this repo's `ai-rules/` folder
   - Paste them into `.cursor/rules/` folder

4. **Add Rules in Settings UI**
   - In `Settings → Rules and Commands → Project Rules`
   - Click **"+"** or **"Add Rule"** button
   - **Example for SDK Rules:**
     - Open `ai-rules/rules/sdk.mdc` file
     - Copy all content
     - Paste into the new rule editor
     - Name it: "Zoho Finance SDK Rules"
     - Click **Save**
   
   - **Example for Location Rules:**
     - Click **"Add Rule"** again
     - Open `ai-rules/rules/location.mdc` file
     - Copy all content
     - Paste into the new rule editor
     - Name it: "Zoho Finance Widget Locations"
     - Click **Save**
   
   - **Example for Additional Rules:**
     - Click **"Add Rule"** again
     - Open `ai-rules/ai-rule-.mdc` file
     - Copy all content
     - Paste into the new rule editor
     - Name it: "Zoho Finance Additional Rules"
     - Click **Save**

5. **Verify Rules Are Active**
   - You should see all three rules in the Project Rules list
   - Ensure toggle switches are **ON** (enabled)
   - Rules are now active! Start coding and Cursor AI will follow these rules.

---

## 📁 What's Included

The folder structure is:

```
ai-rules/
  ├── ai-rule-.mdc
  └── rules/
      ├── location.mdc
      └── sdk.mdc
```

- **`ai-rules/rules/sdk.mdc`** - Zoho Finance SDK rules: Proper initialization, API calls, event handling
- **`ai-rules/rules/location.mdc`** - Widget Locations: All available widget placement locations and module data structures
- **`ai-rules/ai-rule-.mdc`** - Additional AI rules


---

