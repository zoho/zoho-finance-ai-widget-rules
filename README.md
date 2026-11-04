# AI Rules for Zoho Finance Widgets

Rules for developing Zoho Finance widgets. The same rules are provided in the format each assistant loads: Cursor, Claude Code, and GitHub Copilot.

---

## Quick Setup

Clone this repository:

```bash
git clone https://github.com/zoho/zoho-finance-ai-widget-rules.git
cd zoho-finance-ai-widget-rules
```

Copy the folder for the assistant you use into your widget project. Each set contains the same four rules: project overview, SDK, widget locations, and Vue.js.

### Cursor

Copy `ai-rules/.cursor` to the root of your widget project so the project contains `.cursor/rules/`.

```bash
cp -R ai-rules/.cursor /path/to/your-widget-project/
```

Cursor loads every file in `.cursor/rules/` with `alwaysApply: true`:

| File | Rule |
| --- | --- |
| `.cursor/rules/project.mdc` | Project overview and global rules |
| `.cursor/rules/sdk.mdc` | Zoho Finance SDK usage |
| `.cursor/rules/location.mdc` | Widget locations, module keys, and events |
| `.cursor/rules/vue.mdc` | Vue.js widget development |

In Cursor, open **Settings → Rules and Commands → Project Rules** and confirm all four rules are enabled.

### Claude Code

Copy `ai-rules/.claude` to the root of your widget project so the project contains `.claude/CLAUDE.md` and `.claude/rules/`.

```bash
cp -R ai-rules/.claude /path/to/your-widget-project/
```

Claude Code loads these files at the start of every session:

| File | Rule |
| --- | --- |
| `.claude/CLAUDE.md` | Project overview and global rules |
| `.claude/rules/sdk.md` | Zoho Finance SDK usage |
| `.claude/rules/location.md` | Widget locations, module keys, and events |
| `.claude/rules/vue.md` | Vue.js widget development |

### GitHub Copilot

Copy `ai-rules/.github` to the root of your widget project so the project contains `.github/copilot-instructions.md` and `.github/instructions/`.

```bash
cp -R ai-rules/.github /path/to/your-widget-project/
```

| File | Rule |
| --- | --- |
| `.github/copilot-instructions.md` | Project overview and global rules |
| `.github/instructions/sdk.instructions.md` | Zoho Finance SDK usage |
| `.github/instructions/location.instructions.md` | Widget locations, module keys, and events |
| `.github/instructions/vue.instructions.md` | Vue.js widget development |

`copilot-instructions.md` applies to every Copilot request. The files in `.github/instructions/` use `applyTo: "**"`, so they apply across the project as well.

---

## What's Included

```
ai-rules/
├── .cursor/
│   └── rules/
│       ├── project.mdc
│       ├── sdk.mdc
│       ├── location.mdc
│       └── vue.mdc
├── .claude/
│   ├── CLAUDE.md
│   └── rules/
│       ├── sdk.md
│       ├── location.md
│       └── vue.md
└── .github/
    ├── copilot-instructions.md
    └── instructions/
        ├── sdk.instructions.md
        ├── location.instructions.md
        └── vue.instructions.md
```

- **Project overview** — Global widget rules, manifest expectations, and links to the other rule files.
- **SDK** — Initialization, allowed SDK methods, API calls, notifications, events, and global fields.
- **Locations** — Widget placement locations and the module data keys and events for each location.
- **Vue.js** — Vue 2 widget structure, ZFAPPS initialization, services, mixins, and build configuration.
