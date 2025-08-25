
# 🤝 Contributing to `deep-tool-docs`

Thank you for your interest in contributing! This is an open and collaborative space for sharing real-world, hands-on **experiments and deep insights** about developer tools.

Whether you've explored a tool’s obscure flag, debugged a tricky behavior, or just want to document something you wish you'd known earlier — your contribution is welcome here. 💡

---

## 🌱 What to Contribute

We welcome contributions related to:
- Real experiments, edge cases, or behaviors of tools.
- Hands-on notes or workflows.
- Visuals, command breakdowns, and annotated logs.
- External references, tutorials, blog links, or whitepapers.

This repo is for the **practical, the tested, and the curious.**

---

## 📁 Repo Structure

Please follow this simple directory structure:

```text
deep-tool-docs/
├── git/
│   ├── 2.42.0/
│   │   ├── commit/
│   │   │   ├── commit-signing.md
│   │   │   └── amend.md
│   │   ├── branch/
│   │   │   └── branch-cleanups.md
│   │   └── resources/
│   │       └── commit-signing-diagram.png
│   └── 2.34.0/
│       ├── stash/
│       │   └── interactive-mode.md
│       └── resources/
│           └── stash-flow.svg
├── gpg/
│   ├── 2.4.3/
│   │   ├── keys/
│   │   │   └── export-import.md
│   │   ├── homedir/
│   │   │   └── export-import.md
│   │   └── resources/
│   │       └── key-visuals.png
```

- Each **top-level folder must be named after a tool**, e.g., `git`, `gpg`, `pytest`.
- Include **version** below the tool directory, e.g. `git`->`2.34.0`.
- Use **Markdown (`.md`) files** to document findings.
- You may create a `resources/` subfolder inside any tool directory for images, links, configs, or anything else.
- Group related experiments under **descriptive headings** inside each `.md` file.
- Cross-reference other Markdown files with relative links and feel free to create tags or indexes for easier navigation.

---

## ✍️ Style Guidelines

- Be authentic: Share your own experiments and lessons.
- Be concise, but informative — screenshots, logs, commands, and outputs are welcome.
- Use proper Markdown formatting (headings, code blocks, lists).
- Prefer clarity over cleverness — this is for others to learn from.
- Link to external resources when relevant.

---

## 🌈 Community Behavior

We are an open, respectful, and inclusive community.

- Harassment, bullying, or exclusionary behavior **will not be tolerated**.
- Be kind, constructive, and helpful.
- Assume positive intent.
- If someone makes a mistake, help them improve — don’t shame them.

---

## 🚀 How to Contribute

1. **Fork** the repo **or** create a branch in your own fork named like this:  
   `<github-username>/<tool-name>/<descriptive-branch-name>`

2. Add your Markdown file or edit an existing one.
3. Add any helpful assets inside the appropriate `resources/` folder.
4. Submit a **pull request** with a clear title and description.

If in doubt, open an issue to start a discussion!

---

## 💻 CLI Experimentation Guidelines

When documenting CLI-based experiments, please:

- Be **as verbose, clear and extensive as possible**. The more detail, the better.
- Provide the **full command** including all flags/options in a `shell` block/` ```shell ``` ` snippet:

  ```shell
  ls -al
  ```

- Include the **exact output** separated by purpose in a `text` block/` ```text ``` ` snippet, with STDOUT labeled as `OUT: ` per line and STDERR is not labeled at all. Or better, simply use the [GSO](https://github.com/Vaastav-Technologies/gso) tool. It was created for this purpose:

  **stdin** (if used):
  ```text
  your input here
  ```

  **output:**
  ```text
  OUT: standard output here
  OUT: printing next on stdout

  error message here from stderr
  stderr may have no label.
  ```

- You may also separate multi-step stdin and outputs like this:

  CLI:
  ```shell
  ./your-script
  ```

  stdin:
  ```text
  your-name
  ```

  output:
  ```text
  OUT: Hello your-name
  ```

  stdin:
  ```text
  extra info
  ```

  output:
  ```text
  OUT: Added details: extra info
  ```

- You may also use a **combined stdin** block for full flow replication:

  CLI:
  ```shell
  ./user-entry-script
  ```

  stdin:
  ```text
  suhas
  curious
  engineer

  here
  ```

  output:
  ```text
  OUT: Name entered: suhas
  OUT: description added:
  OUT: curious
  OUT: engineer
  OUT: 
  OUT: here
  ```

- Always show the **exit/return code**:

  ```text
  0
  ```

- Prefer **split stdin blocks** for clarity. Combined blocks are allowed, but use only when it enhances readability or replicability.

---

### 🔎 Full CLI Example

#### With stdin:

Setup:
```shell
echo a-file >> a-file
git add a-file
```

CLI:
```shell
git commit -F -
```

output:
```text
STDERR: (reading log message from standard input)
```

stdin:
```text
new commit message
^Z
```

output:
```text
OUT: [master 2efeea0] new commit
OUT:  1 file changed, 1 insertion(+)
```

exit code: 0

#### Without stdin:

```shell
git commit -m "new commit"
```

output:
```text
OUT: [master 2efeea0] new commit
OUT:  1 file changed, 1 insertion(+)
```

exit code: 0

---

## 🙌 Thanks!

Every experiment shared helps someone else.  
Let’s build a **deep, practical, and wildly useful** collection of tool knowledge together.

Happy tinkering! 🔧✨
