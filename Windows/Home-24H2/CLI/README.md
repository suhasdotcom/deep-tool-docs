# 💻 Windows Home 24H2 – CLI Experiments

This directory documents real-world command-line usage, quirks, and behavior patterns encountered in **Windows Home 24H2**, focused specifically on the **CLI environment**.

> These findings aim to go beyond help menus and document flags, edge-cases, or patterns developers might actually run into.

---

## 🧪 What You'll Find Here

* `cmd.exe` and PowerShell-specific behaviors
* Experiments with CLI tools on Windows — especially `cmd.exe`, Windows Terminal.
* Standard input/output/error formatting and return codes.
* Differences vs. other OS shells when applicable.
* Full, reproducible experiment logs using Markdown.

---

## 🗂️ Structure

Each experiment is documented in a separate Markdown file. Prefer grouping them by tool or topic.

```
CLI/
├── everyday-usage/
│   ├── explorer.md
│   └── ...
```

---

## 📌 Tips for Contributors

* Follow the CLI documentation guidelines in [CONTRIBUTING.md](../../../CONTRIBUTING.md#-cli-experimentation-guidelines)
* Always include:

    * The full command in a shell snippet block
    * Clear STDOUT/STDERR separation
    * Return/exit code
    * Any STDIN used (split or combined)
* Be verbose. Context matters.

---

Let’s make the Windows CLI less of a black box — one command at a time ⚡
